---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: OWIN を使用して、ASP.NET Web API 2 をセルフホスト |Microsoft Docs
author: rick-anderson
description: このチュートリアルでは、OWIN を使用して Web API フレームワークを自己ホストするコンソール アプリケーションで ASP.NET Web API をホストする方法を示します。 Web Interface for .NET (OWIN) d を開く.
ms.author: riande
ms.date: 07/09/2013
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: 06fd13fe9b12d172d615ae76a71d246a89f5386d
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910487"
---
<a name="use-owin-to-self-host-aspnet-web-api-2"></a><span data-ttu-id="5cd56-104">OWIN を使用して、ASP.NET Web API 2 を自己ホスト</span><span class="sxs-lookup"><span data-stu-id="5cd56-104">Use OWIN to Self-Host ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="5cd56-105">によって[Kanchan Mehrotra](https://twitter.com/kanchanmeh)</span><span class="sxs-lookup"><span data-stu-id="5cd56-105">by [Kanchan Mehrotra](https://twitter.com/kanchanmeh)</span></span>

> <span data-ttu-id="5cd56-106">このチュートリアルでは、OWIN を使用して Web API フレームワークを自己ホストするコンソール アプリケーションで ASP.NET Web API をホストする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="5cd56-106">This tutorial shows how to host ASP.NET Web API in a console application, using OWIN to self-host the Web API framework.</span></span>
>
> <span data-ttu-id="5cd56-107">[.NET 用 Web インターフェイスを開き](http://owin.org)(OWIN) .NET web サーバーおよび web アプリケーション間の抽象化を定義します。</span><span class="sxs-lookup"><span data-stu-id="5cd56-107">[Open Web Interface for .NET](http://owin.org) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="5cd56-108">OWIN により、OWIN の IIS の外部の独自のプロセスで web アプリケーションを自己ホストするために最適ですが、サーバーから web アプリケーションを分離します。</span><span class="sxs-lookup"><span data-stu-id="5cd56-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="5cd56-109">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="5cd56-109">Software versions used in the tutorial</span></span>
>
>
> - <span data-ttu-id="5cd56-110">[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) (また、Visual Studio 2012 で動作する)</span><span class="sxs-lookup"><span data-stu-id="5cd56-110">[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) (also works with Visual Studio 2012)</span></span>
> - <span data-ttu-id="5cd56-111">Web API 2</span><span class="sxs-lookup"><span data-stu-id="5cd56-111">Web API 2</span></span>


> [!NOTE]
> <span data-ttu-id="5cd56-112">完全なソース コードを検索するにはこのチュートリアルで[aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt)します。</span><span class="sxs-lookup"><span data-stu-id="5cd56-112">You can find the complete source code for this tutorial at [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span></span>


## <a name="create-a-console-application"></a><span data-ttu-id="5cd56-113">コンソール アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="5cd56-113">Create a Console Application</span></span>

<span data-ttu-id="5cd56-114">**ファイル** メニューのをクリックして**新規**、 をクリックし、**プロジェクト**します。</span><span class="sxs-lookup"><span data-stu-id="5cd56-114">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="5cd56-115">**インストールされたテンプレート**、Visual c# では、クリックして**Windows**  をクリックし、**コンソール アプリケーション**します。</span><span class="sxs-lookup"><span data-stu-id="5cd56-115">From **Installed Templates**, under Visual C#, click **Windows** and then click **Console Application**.</span></span> <span data-ttu-id="5cd56-116">プロジェクトに"OwinSelfhostSample"という名前にして**OK**します。</span><span class="sxs-lookup"><span data-stu-id="5cd56-116">Name the project "OwinSelfhostSample" and click **OK**.</span></span>

[![](use-owin-to-self-host-web-api/_static/image2.png)](use-owin-to-self-host-web-api/_static/image1.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="5cd56-117">Web API と OWIN パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="5cd56-117">Add the Web API and OWIN Packages</span></span>

<span data-ttu-id="5cd56-118">**ツール** メニューのをクリックして**NuGet パッケージ マネージャー**、 をクリックし、**パッケージ マネージャー コンソール**します。</span><span class="sxs-lookup"><span data-stu-id="5cd56-118">From the **Tools** menu, click **NuGet Package Manager**, then click **Package Manager Console**.</span></span> <span data-ttu-id="5cd56-119">パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="5cd56-119">In the Package Manager Console window, enter the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

<span data-ttu-id="5cd56-120">これは、WebAPI を OWIN 自己ホスト型のパッケージと必要なすべての OWIN パッケージでインストールされます。</span><span class="sxs-lookup"><span data-stu-id="5cd56-120">This will install the WebAPI OWIN selfhost package and all the required OWIN packages.</span></span>

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="5cd56-121">用の Web API を構成する自己ホスト</span><span class="sxs-lookup"><span data-stu-id="5cd56-121">Configure Web API for Self-Host</span></span>

<span data-ttu-id="5cd56-122">ソリューション エクスプ ローラーでプロジェクトを右クリックし、選択**追加** / **クラス**新しいクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="5cd56-122">In Solution Explorer, right click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="5cd56-123">クラスに `Startup` という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="5cd56-123">Name the class `Startup`.</span></span>

![](use-owin-to-self-host-web-api/_static/image5.png)

<span data-ttu-id="5cd56-124">このファイルの定型コードのすべて、次に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="5cd56-124">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="5cd56-125">Web API コント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="5cd56-125">Add a Web API Controller</span></span>

<span data-ttu-id="5cd56-126">次に、Web API コント ローラー クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="5cd56-126">Next, add a Web API controller class.</span></span> <span data-ttu-id="5cd56-127">ソリューション エクスプ ローラーでプロジェクトを右クリックし、選択**追加** / **クラス**新しいクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="5cd56-127">In Solution Explorer, right click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="5cd56-128">クラスに `ValuesController` という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="5cd56-128">Name the class `ValuesController`.</span></span>

<span data-ttu-id="5cd56-129">このファイルの定型コードのすべて、次に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="5cd56-129">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-using-httpclient"></a><span data-ttu-id="5cd56-130">OWIN ホストを起動し、HttpClient を使用して要求を行う</span><span class="sxs-lookup"><span data-stu-id="5cd56-130">Start the OWIN Host and Make a Request Using HttpClient</span></span>

<span data-ttu-id="5cd56-131">次のようにすべての Program.cs ファイルの定型コードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="5cd56-131">Replace all of the boilerplate code in the Program.cs file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="running-the-application"></a><span data-ttu-id="5cd56-132">アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="5cd56-132">Running the Application</span></span>

<span data-ttu-id="5cd56-133">アプリケーションを実行するには、Visual Studio で F5 キーを押します。</span><span class="sxs-lookup"><span data-stu-id="5cd56-133">To run the application, press F5 in Visual Studio.</span></span> <span data-ttu-id="5cd56-134">出力の内容は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="5cd56-134">The output should look like the following:</span></span>

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a><span data-ttu-id="5cd56-135">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="5cd56-135">Additional Resources</span></span>

[<span data-ttu-id="5cd56-136">プロジェクト Katana の概要</span><span class="sxs-lookup"><span data-stu-id="5cd56-136">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[<span data-ttu-id="5cd56-137">Azure Worker ロールにおける ASP.NET Web API をホストします。</span><span class="sxs-lookup"><span data-stu-id="5cd56-137">Host ASP.NET Web API in an Azure Worker Role</span></span>](host-aspnet-web-api-in-an-azure-worker-role.md)
