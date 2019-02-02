---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: OWIN を使用して、ASP.NET Web API を自己ホスト |Microsoft Docs
author: rick-anderson
description: このチュートリアルでは、OWIN を使用して Web API フレームワークを自己ホストするコンソール アプリケーションで ASP.NET Web API をホストする方法を示します。 Web Interface for .NET (OWIN) d を開く.
ms.author: riande
ms.date: 07/09/2013
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: 59ce24aa47ca590fbe9b617dbbe8bc6b3711849e
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667389"
---
<a name="use-owin-to-self-host-aspnet-web-api"></a><span data-ttu-id="1cc63-104">OWIN を使用して、ASP.NET Web API を自己ホスト</span><span class="sxs-lookup"><span data-stu-id="1cc63-104">Use OWIN to Self-Host ASP.NET Web API</span></span> 
====================

> <span data-ttu-id="1cc63-105">このチュートリアルでは、OWIN を使用して Web API フレームワークを自己ホストするコンソール アプリケーションで ASP.NET Web API をホストする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="1cc63-105">This tutorial shows how to host ASP.NET Web API in a console application, using OWIN to self-host the Web API framework.</span></span>
>
> <span data-ttu-id="1cc63-106">[.NET 用 Web インターフェイスを開き](http://owin.org)(OWIN) .NET web サーバーおよび web アプリケーション間の抽象化を定義します。</span><span class="sxs-lookup"><span data-stu-id="1cc63-106">[Open Web Interface for .NET](http://owin.org) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="1cc63-107">OWIN により、OWIN の IIS の外部の独自のプロセスで web アプリケーションを自己ホストするために最適ですが、サーバーから web アプリケーションを分離します。</span><span class="sxs-lookup"><span data-stu-id="1cc63-107">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="1cc63-108">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="1cc63-108">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="1cc63-109">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="1cc63-109">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/) 
> - <span data-ttu-id="1cc63-110">Web API 5.2.7</span><span class="sxs-lookup"><span data-stu-id="1cc63-110">Web API 5.2.7</span></span>


> [!NOTE]
> <span data-ttu-id="1cc63-111">完全なソース コードを検索するにはこのチュートリアルで[aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt)します。</span><span class="sxs-lookup"><span data-stu-id="1cc63-111">You can find the complete source code for this tutorial at [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span></span>


## <a name="create-a-console-application"></a><span data-ttu-id="1cc63-112">コンソール アプリケーションを作成する</span><span class="sxs-lookup"><span data-stu-id="1cc63-112">Create a console application</span></span>

<span data-ttu-id="1cc63-113">**ファイル**] メニューの [**新規**を選択し、**プロジェクト**します。</span><span class="sxs-lookup"><span data-stu-id="1cc63-113">On the **File** menu,  **New**, then select **Project**.</span></span> <span data-ttu-id="1cc63-114">**インストール済み** **Visual C#** を選択します**Windows デスクトップ**選び**コンソール アプリ (.Net Framework)**。</span><span class="sxs-lookup"><span data-stu-id="1cc63-114">From **Installed**, under **Visual C#**, select **Windows Desktop** and then select **Console App (.Net Framework)**.</span></span> <span data-ttu-id="1cc63-115">"OwinSelfhostSample"プロジェクトの名前し、選択**OK**します。</span><span class="sxs-lookup"><span data-stu-id="1cc63-115">Name the project "OwinSelfhostSample" and select **OK**.</span></span>

[![](use-owin-to-self-host-web-api/_static/image7.png)](use-owin-to-self-host-web-api/_static/image7.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="1cc63-116">Web API と OWIN パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="1cc63-116">Add the Web API and OWIN packages</span></span>

<span data-ttu-id="1cc63-117">**ツール**メニューの  **NuGet パッケージ マネージャー**を選択し、**パッケージ マネージャー コンソール**します。</span><span class="sxs-lookup"><span data-stu-id="1cc63-117">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="1cc63-118">パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="1cc63-118">In the Package Manager Console window, enter the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

<span data-ttu-id="1cc63-119">これは、WebAPI を OWIN 自己ホスト型のパッケージと必要なすべての OWIN パッケージでインストールされます。</span><span class="sxs-lookup"><span data-stu-id="1cc63-119">This will install the WebAPI OWIN selfhost package and all the required OWIN packages.</span></span>

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="1cc63-120">Web API の構成の自己ホスト</span><span class="sxs-lookup"><span data-stu-id="1cc63-120">Configure Web API for self-host</span></span>

<span data-ttu-id="1cc63-121">ソリューション エクスプ ローラーでプロジェクトを右クリックし、選択**追加** / **クラス**新しいクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="1cc63-121">In Solution Explorer, right-click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="1cc63-122">クラスに `Startup` という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="1cc63-122">Name the class `Startup`.</span></span>

![](use-owin-to-self-host-web-api/_static/image5.png)

<span data-ttu-id="1cc63-123">このファイルの定型コードのすべて、次に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="1cc63-123">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="1cc63-124">Web API コント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="1cc63-124">Add a Web API controller</span></span>

<span data-ttu-id="1cc63-125">次に、Web API コント ローラー クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="1cc63-125">Next, add a Web API controller class.</span></span> <span data-ttu-id="1cc63-126">ソリューション エクスプ ローラーでプロジェクトを右クリックし、選択**追加** / **クラス**新しいクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="1cc63-126">In Solution Explorer, right-click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="1cc63-127">クラスに `ValuesController` という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="1cc63-127">Name the class `ValuesController`.</span></span>

<span data-ttu-id="1cc63-128">このファイルの定型コードのすべて、次に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="1cc63-128">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-with-httpclient"></a><span data-ttu-id="1cc63-129">OWIN ホストを起動し、HttpClient で要求を行う</span><span class="sxs-lookup"><span data-stu-id="1cc63-129">Start the OWIN Host and make a request with HttpClient</span></span>

<span data-ttu-id="1cc63-130">次のようにすべての Program.cs ファイルの定型コードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="1cc63-130">Replace all of the boilerplate code in the Program.cs file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="run-the-application"></a><span data-ttu-id="1cc63-131">アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="1cc63-131">Run the application</span></span>

<span data-ttu-id="1cc63-132">アプリケーションを実行するには、Visual Studio で F5 キーを押します。</span><span class="sxs-lookup"><span data-stu-id="1cc63-132">To run the application, press F5 in Visual Studio.</span></span> <span data-ttu-id="1cc63-133">出力の内容は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="1cc63-133">The output should look like the following:</span></span>

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a><span data-ttu-id="1cc63-134">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="1cc63-134">Additional resources</span></span>

[<span data-ttu-id="1cc63-135">プロジェクト Katana の概要</span><span class="sxs-lookup"><span data-stu-id="1cc63-135">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[<span data-ttu-id="1cc63-136">Azure Worker ロールにおける ASP.NET Web API をホストします。</span><span class="sxs-lookup"><span data-stu-id="1cc63-136">Host ASP.NET Web API in an Azure Worker Role</span></span>](host-aspnet-web-api-in-an-azure-worker-role.md)
