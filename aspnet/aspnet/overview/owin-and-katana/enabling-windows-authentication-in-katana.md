---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: Katana で Windows 認証を有効にする |Microsoft Docs
author: MikeWasson
description: この記事では、Katana で Windows 認証を有効にする方法を示しています。 2 つのシナリオについて説明します IIS ホストしていた Katana を使用して、セルフホスト Kat HttpListener を使用しています.。
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: 8afa2c9dfbe03a9874513f7d083adf7608f4218f
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910461"
---
<a name="enabling-windows-authentication-in-katana"></a><span data-ttu-id="09ba4-104">Katana で Windows 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="09ba4-104">Enabling Windows Authentication in Katana</span></span>
====================
<span data-ttu-id="09ba4-105">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="09ba4-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="09ba4-106">この記事では、Katana で Windows 認証を有効にする方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="09ba4-106">This article shows how to enable Windows Authentication in Katana.</span></span> <span data-ttu-id="09ba4-107">2 つのシナリオについて説明します。 IIS ホストしていた Katana を使用して、カスタム プロセスでセルフホスト Katana HttpListener を使用しています。</span><span class="sxs-lookup"><span data-stu-id="09ba4-107">It covers two scenarios: Using IIS to host Katana, and using HttpListener to self-host Katana in a custom process.</span></span> <span data-ttu-id="09ba4-108">この記事のレビュー Barry Dorrans、David Matson、および Chris Ross に感謝します。</span><span class="sxs-lookup"><span data-stu-id="09ba4-108">Thanks to Barry Dorrans, David Matson, and Chris Ross for reviewing this article.</span></span>


<span data-ttu-id="09ba4-109">Katana は Microsoft の実装の[OWIN](http://owin.org/)、Open Web Interface for .NET。</span><span class="sxs-lookup"><span data-stu-id="09ba4-109">Katana is Microsoft's implementation of [OWIN](http://owin.org/), the Open Web Interface for .NET.</span></span> <span data-ttu-id="09ba4-110">OWIN と Katana の概要については読み取ることができます[ここ](an-overview-of-project-katana.md)します。</span><span class="sxs-lookup"><span data-stu-id="09ba4-110">You can read an introduction to OWIN and Katana [here](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="09ba4-111">OWIN のアーキテクチャには、複数のレイヤーがあります。</span><span class="sxs-lookup"><span data-stu-id="09ba4-111">The OWIN architecture has several layers:</span></span>

- <span data-ttu-id="09ba4-112">ホスト: は、OWIN パイプラインを実行するプロセスを管理します。</span><span class="sxs-lookup"><span data-stu-id="09ba4-112">Host: Manages the process in which the OWIN pipeline runs.</span></span>
- <span data-ttu-id="09ba4-113">サーバー: は、ネットワーク ソケットを開き、要求をリッスンします。</span><span class="sxs-lookup"><span data-stu-id="09ba4-113">Server: Opens a network socket and listens for requests.</span></span>
- <span data-ttu-id="09ba4-114">ミドルウェア: は、HTTP 要求と応答を処理します。</span><span class="sxs-lookup"><span data-stu-id="09ba4-114">Middleware: Processes the HTTP request and response.</span></span>

<span data-ttu-id="09ba4-115">Katana には、Windows 統合認証をサポートする 2 つのサーバーが現在提供しています。</span><span class="sxs-lookup"><span data-stu-id="09ba4-115">Katana currently provides two servers, both of which support Windows Integrated Authentication:</span></span>

- <span data-ttu-id="09ba4-116">**Microsoft.Owin.Host.SystemWeb**します。</span><span class="sxs-lookup"><span data-stu-id="09ba4-116">**Microsoft.Owin.Host.SystemWeb**.</span></span> <span data-ttu-id="09ba4-117">ASP.NET パイプラインでは、IIS を使用します。</span><span class="sxs-lookup"><span data-stu-id="09ba4-117">Uses IIS with the ASP.NET pipeline.</span></span>
- <span data-ttu-id="09ba4-118">**Microsoft.Owin.Host.HttpListener**します。</span><span class="sxs-lookup"><span data-stu-id="09ba4-118">**Microsoft.Owin.Host.HttpListener**.</span></span> <span data-ttu-id="09ba4-119">使用して[System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="09ba4-119">Uses [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx).</span></span> <span data-ttu-id="09ba4-120">このサーバーは現在、既定のオプションと自己ホスト Katana します。</span><span class="sxs-lookup"><span data-stu-id="09ba4-120">This server is currently the default option when self-hosting Katana.</span></span>

> [!NOTE]
> <span data-ttu-id="09ba4-121">Katana は提供されていません OWIN ミドルウェアの Windows 認証では、この機能は、サーバーで使用可能な既にあるため。</span><span class="sxs-lookup"><span data-stu-id="09ba4-121">Katana does not currently provide OWIN middleware for Windows Authentication, because this functionality is already available in the servers.</span></span>

## <a name="windows-authentication-in-iis"></a><span data-ttu-id="09ba4-122">IIS で Windows 認証</span><span class="sxs-lookup"><span data-stu-id="09ba4-122">Windows Authentication in IIS</span></span>

<span data-ttu-id="09ba4-123">Microsoft.Owin.Host.SystemWeb を使用して、単に、IIS で Windows 認証を有効にできます。</span><span class="sxs-lookup"><span data-stu-id="09ba4-123">Using Microsoft.Owin.Host.SystemWeb, you can simply enable Windows Authentication in IIS.</span></span>

<span data-ttu-id="09ba4-124">「ASP.NET 空の Web アプリケーション」プロジェクト テンプレートを使用して、新しい ASP.NET アプリケーションを作成してみましょう。</span><span class="sxs-lookup"><span data-stu-id="09ba4-124">Let's start by creating a new ASP.NET application, using the "ASP.NET Empty Web Application" project template.</span></span>

![](enabling-windows-authentication-in-katana/_static/image1.png)

<span data-ttu-id="09ba4-125">次に、NuGet パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="09ba4-125">Next, add NuGet packages.</span></span> <span data-ttu-id="09ba4-126">**ツール**メニューの  **NuGet パッケージ マネージャー**を選択し、**パッケージ マネージャー コンソール**します。</span><span class="sxs-lookup"><span data-stu-id="09ba4-126">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="09ba4-127">パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="09ba4-127">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

<span data-ttu-id="09ba4-128">という名前のクラスを追加するようになりました`Startup`を次のコード。</span><span class="sxs-lookup"><span data-stu-id="09ba4-128">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

<span data-ttu-id="09ba4-129">Owin の IIS で実行されている"Hello world"アプリケーションを作成する必要があるだけです。</span><span class="sxs-lookup"><span data-stu-id="09ba4-129">That's all you need to create a "Hello world" application for OWIN, running on IIS.</span></span> <span data-ttu-id="09ba4-130">F5 キーを押してアプリケーションをデバッグします。</span><span class="sxs-lookup"><span data-stu-id="09ba4-130">Press F5 to debug the application.</span></span> <span data-ttu-id="09ba4-131">"Hello World!"を表示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="09ba4-131">You should see "Hello World!"</span></span> <span data-ttu-id="09ba4-132">ブラウザーのウィンドウです。</span><span class="sxs-lookup"><span data-stu-id="09ba4-132">in the browser window.</span></span>

![](enabling-windows-authentication-in-katana/_static/image2.png)

<span data-ttu-id="09ba4-133">次に、IIS Express で Windows 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="09ba4-133">Next, we'll enable Windows Authentication in IIS Express.</span></span> <span data-ttu-id="09ba4-134">**ビュー**メニューの **プロパティ**します。</span><span class="sxs-lookup"><span data-stu-id="09ba4-134">From the **View** menu, select **Properties**.</span></span> <span data-ttu-id="09ba4-135">プロジェクトのプロパティを表示するソリューション エクスプ ローラーでプロジェクト名をクリックします。</span><span class="sxs-lookup"><span data-stu-id="09ba4-135">Click on the project name in Solution Explorer to view the project properties.</span></span>

<span data-ttu-id="09ba4-136">**プロパティ**ウィンドウで、設定**匿名認証**に**無効**設定と**Windows 認証**に**有効になっている**します。</span><span class="sxs-lookup"><span data-stu-id="09ba4-136">In the **Properties** window, set **Anonymous Authentication** to **Disabled** and set **Windows Authentication** to **Enabled**.</span></span>

![](enabling-windows-authentication-in-katana/_static/image3.png)

<span data-ttu-id="09ba4-137">Visual Studio からアプリケーションを実行するときに IIS Express とユーザーの Windows 資格情報が必要です。</span><span class="sxs-lookup"><span data-stu-id="09ba4-137">When you run the application from Visual Studio, IIS Express will require the user's Windows credentials.</span></span> <span data-ttu-id="09ba4-138">これを使用して表示できる[Fiddler](http://fiddler2.com/home)または別の HTTP デバッグ ツール。</span><span class="sxs-lookup"><span data-stu-id="09ba4-138">You can see this by using [Fiddler](http://fiddler2.com/home) or another HTTP debugging tool.</span></span> <span data-ttu-id="09ba4-139">HTTP 応答例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="09ba4-139">Here is an example HTTP response:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

<span data-ttu-id="09ba4-140">この応答で Www-authenticate ヘッダーを示す、サーバーをサポートしている、[ネゴシエート](http://www.ietf.org/rfc/rfc4559.txt)、Kerberos または NTLM を使用するプロトコル。</span><span class="sxs-lookup"><span data-stu-id="09ba4-140">The WWW-Authenticate headers in this response indicate that the server supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protocol, which uses either Kerberos or NTLM.</span></span>

<span data-ttu-id="09ba4-141">後で、サーバーにアプリケーションを展開するときにに従って[手順](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication)そのサーバー上の IIS で Windows 認証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="09ba4-141">Later, when you deploy the application to a server, follow [these steps](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) to enable Windows Authentication in IIS on that server.</span></span>

## <a name="windows-authentication-in-httplistener"></a><span data-ttu-id="09ba4-142">HttpListener で Windows 認証</span><span class="sxs-lookup"><span data-stu-id="09ba4-142">Windows Authentication in HttpListener</span></span>

<span data-ttu-id="09ba4-143">直接は Windows 認証を有効にする Microsoft.Owin.Host.HttpListener Katana を自己ホストを使用している場合、 **HttpListener**インスタンス。</span><span class="sxs-lookup"><span data-stu-id="09ba4-143">If you are using Microsoft.Owin.Host.HttpListener to self-host Katana, you can enable Windows Authentication directly on the **HttpListener** instance.</span></span>

<span data-ttu-id="09ba4-144">最初に、新しいコンソール アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="09ba4-144">First, create a new console application.</span></span> <span data-ttu-id="09ba4-145">次に、NuGet パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="09ba4-145">Next, add NuGet packages.</span></span> <span data-ttu-id="09ba4-146">**ツール**メニューの  **NuGet パッケージ マネージャー**を選択し、**パッケージ マネージャー コンソール**します。</span><span class="sxs-lookup"><span data-stu-id="09ba4-146">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="09ba4-147">パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="09ba4-147">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

<span data-ttu-id="09ba4-148">という名前のクラスを追加するようになりました`Startup`を次のコード。</span><span class="sxs-lookup"><span data-stu-id="09ba4-148">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

<span data-ttu-id="09ba4-149">このクラスが実装する前に、同じ"Hello world"の例は、認証方法として Windows 認証を設定します。</span><span class="sxs-lookup"><span data-stu-id="09ba4-149">This class implements the same "Hello world" example from before, but it also sets Windows Authentication as the authentication scheme.</span></span>

<span data-ttu-id="09ba4-150">内で、`Main`関数を OWIN パイプラインを開始します。</span><span class="sxs-lookup"><span data-stu-id="09ba4-150">Inside the `Main` function, start the OWIN pipeline:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

<span data-ttu-id="09ba4-151">アプリケーションが Windows 認証を使用していることを確認するように Fiddler では、要求を送信できます。</span><span class="sxs-lookup"><span data-stu-id="09ba4-151">You can send a request in Fiddler to confirm that the application is using Windows Authentication:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a><span data-ttu-id="09ba4-152">関連トピック</span><span class="sxs-lookup"><span data-stu-id="09ba4-152">Related Topics</span></span>

[<span data-ttu-id="09ba4-153">プロジェクト Katana の概要</span><span class="sxs-lookup"><span data-stu-id="09ba4-153">An Overview of Project Katana</span></span>](an-overview-of-project-katana.md)

[<span data-ttu-id="09ba4-154">System.Net.HttpListener</span><span class="sxs-lookup"><span data-stu-id="09ba4-154">System.Net.HttpListener</span></span>](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[<span data-ttu-id="09ba4-155">MVC 5 で OWIN フォーム認証を理解します。</span><span class="sxs-lookup"><span data-stu-id="09ba4-155">Understanding OWIN Forms Authentication in MVC 5</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
