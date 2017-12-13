---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: "使用して OWIN セルフ ホスト ASP.NET Web API 2 |Microsoft ドキュメント"
author: rick-anderson
description: "このチュートリアルでは、OWIN を使用して Web API フレームワークを自己ホストするコンソール アプリケーションで ASP.NET Web API をホストする方法を示します。 .NET (OWIN) d 用 Web インターフェイスを開く."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2013
ms.topic: article
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: fda0db8155c3303907331a690af35f619b589154
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="use-owin-to-self-host-aspnet-web-api-2"></a><span data-ttu-id="cdbec-104">OWIN を使用して ASP.NET Web API 2 を自己ホスト</span><span class="sxs-lookup"><span data-stu-id="cdbec-104">Use OWIN to Self-Host ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="cdbec-105">によって[Kanchan Mehrotra](https://twitter.com/kanchanmeh)</span><span class="sxs-lookup"><span data-stu-id="cdbec-105">by [Kanchan Mehrotra](https://twitter.com/kanchanmeh)</span></span>

> <span data-ttu-id="cdbec-106">このチュートリアルでは、OWIN を使用して Web API フレームワークを自己ホストするコンソール アプリケーションで ASP.NET Web API をホストする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="cdbec-106">This tutorial shows how to host ASP.NET Web API in a console application, using OWIN to self-host the Web API framework.</span></span>
> 
> <span data-ttu-id="cdbec-107">[.NET 用 Web インターフェイスを開く](http://owin.org)(OWIN) .NET web サーバーと web アプリケーション間で抽象型を定義します。</span><span class="sxs-lookup"><span data-stu-id="cdbec-107">[Open Web Interface for .NET](http://owin.org) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="cdbec-108">OWIN には、web アプリケーション、サーバーは、OWIN のため、IIS の外部で独自のプロセス内の web アプリケーションを自己ホストするために最適ですから切り離します。</span><span class="sxs-lookup"><span data-stu-id="cdbec-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="cdbec-109">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="cdbec-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="cdbec-110">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (Visual Studio 2012 でも動作します)</span><span class="sxs-lookup"><span data-stu-id="cdbec-110">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (also works with Visual Studio 2012)</span></span>
> - <span data-ttu-id="cdbec-111">Web API 2</span><span class="sxs-lookup"><span data-stu-id="cdbec-111">Web API 2</span></span>


> [!NOTE]
> <span data-ttu-id="cdbec-112">このチュートリアルでの完全なソース コードを検索する[aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt)です。</span><span class="sxs-lookup"><span data-stu-id="cdbec-112">You can find the complete source code for this tutorial at [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span></span>


## <a name="create-a-console-application"></a><span data-ttu-id="cdbec-113">コンソール アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="cdbec-113">Create a Console Application</span></span>

<span data-ttu-id="cdbec-114">**ファイル** メニューのをクリックして**新規**をクリックし、**プロジェクト**です。</span><span class="sxs-lookup"><span data-stu-id="cdbec-114">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="cdbec-115">**インストールされたテンプレート**、Visual c# をクリックして**Windows**  をクリックし、**コンソール アプリケーション**です。</span><span class="sxs-lookup"><span data-stu-id="cdbec-115">From **Installed Templates**, under Visual C#, click **Windows** and then click **Console Application**.</span></span> <span data-ttu-id="cdbec-116">プロジェクト"OwinSelfhostSample"の名前し、をクリックして**OK**です。</span><span class="sxs-lookup"><span data-stu-id="cdbec-116">Name the project "OwinSelfhostSample" and click **OK**.</span></span>

[![](use-owin-to-self-host-web-api/_static/image2.png)](use-owin-to-self-host-web-api/_static/image1.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="cdbec-117">Web API と OWIN パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="cdbec-117">Add the Web API and OWIN Packages</span></span>

<span data-ttu-id="cdbec-118">**ツール** メニューのをクリックして**ライブラリ パッケージ マネージャー**をクリックし、 **Package Manager Console**です。</span><span class="sxs-lookup"><span data-stu-id="cdbec-118">From the **Tools** menu, click **Library Package Manager**, then click **Package Manager Console**.</span></span> <span data-ttu-id="cdbec-119">パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="cdbec-119">In the Package Manager Console window, enter the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

<span data-ttu-id="cdbec-120">WebAPI OWIN selfhost パッケージと必要なすべての OWIN パッケージがインストールされます。</span><span class="sxs-lookup"><span data-stu-id="cdbec-120">This will install the WebAPI OWIN selfhost package and all the required OWIN packages.</span></span>

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="cdbec-121">Web API を構成する自己ホスト</span><span class="sxs-lookup"><span data-stu-id="cdbec-121">Configure Web API for Self-Host</span></span>

<span data-ttu-id="cdbec-122">ソリューション エクスプ ローラーでプロジェクトを右クリックし、**追加** / **クラス**新しいクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="cdbec-122">In Solution Explorer, right click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="cdbec-123">クラスに `Startup` という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="cdbec-123">Name the class `Startup`.</span></span>

![](use-owin-to-self-host-web-api/_static/image5.png)

<span data-ttu-id="cdbec-124">次のようにすべてのこのファイルに定型コードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="cdbec-124">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="cdbec-125">Web API コント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="cdbec-125">Add a Web API Controller</span></span>

<span data-ttu-id="cdbec-126">次に、Web API コント ローラー クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="cdbec-126">Next, add a Web API controller class.</span></span> <span data-ttu-id="cdbec-127">ソリューション エクスプ ローラーでプロジェクトを右クリックし、**追加** / **クラス**新しいクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="cdbec-127">In Solution Explorer, right click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="cdbec-128">クラスに `ValuesController` という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="cdbec-128">Name the class `ValuesController`.</span></span>

<span data-ttu-id="cdbec-129">次のようにすべてのこのファイルに定型コードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="cdbec-129">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-using-httpclient"></a><span data-ttu-id="cdbec-130">OWIN ホストを起動して、HttpClient を使用して、要求</span><span class="sxs-lookup"><span data-stu-id="cdbec-130">Start the OWIN Host and Make a Request Using HttpClient</span></span>

<span data-ttu-id="cdbec-131">次のようにすべての Program.cs ファイルに定型コードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="cdbec-131">Replace all of the boilerplate code in the Program.cs file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="running-the-application"></a><span data-ttu-id="cdbec-132">アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="cdbec-132">Running the Application</span></span>

<span data-ttu-id="cdbec-133">アプリケーションを実行するには、Visual Studio で f5 キーを押します。</span><span class="sxs-lookup"><span data-stu-id="cdbec-133">To run the application, press F5 in Visual Studio.</span></span> <span data-ttu-id="cdbec-134">出力の内容は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="cdbec-134">The output should look like the following:</span></span>

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a><span data-ttu-id="cdbec-135">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="cdbec-135">Additional Resources</span></span>

[<span data-ttu-id="cdbec-136">プロジェクトの Katana の概要</span><span class="sxs-lookup"><span data-stu-id="cdbec-136">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[<span data-ttu-id="cdbec-137">Azure のワーカー ロールで ASP.NET Web API をホストします。</span><span class="sxs-lookup"><span data-stu-id="cdbec-137">Host ASP.NET Web API in an Azure Worker Role</span></span>](host-aspnet-web-api-in-an-azure-worker-role.md)
