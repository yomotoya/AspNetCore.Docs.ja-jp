---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: OWIN と Katana の概要 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 09/27/2013
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: 9920861da0e67d9304a944cacfb8ff8685267cd6
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/10/2018
ms.locfileid: "48913178"
---
<a name="getting-started-with-owin-and-katana"></a><span data-ttu-id="09531-102">OWIN と Katana の概要</span><span class="sxs-lookup"><span data-stu-id="09531-102">Getting Started with OWIN and Katana</span></span>
====================
<span data-ttu-id="09531-103">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="09531-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="09531-104">[Web Interface for .NET (OWIN) を開く](http://owin.org/).NET web サーバーおよび web アプリケーション間の抽象化を定義します。</span><span class="sxs-lookup"><span data-stu-id="09531-104">[Open Web Interface for .NET (OWIN)](http://owin.org/) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="09531-105">OWIN は、アプリケーションから web サーバーを分離することで、簡単に .NET web 開発のためのミドルウェアを作成します。</span><span class="sxs-lookup"><span data-stu-id="09531-105">By decoupling the web server from the application, OWIN makes it easier to create middleware for .NET web development.</span></span> <span data-ttu-id="09531-106">また、OWIN 簡単に他のホストに web アプリケーションを移植&#8212;などの Windows サービスまたはその他のプロセスで自己ホストします。</span><span class="sxs-lookup"><span data-stu-id="09531-106">Also, OWIN makes it easier to port web applications to other hosts&#8212;for example, self-hosting in a Windows service or other process.</span></span>

<span data-ttu-id="09531-107">OWIN は、実装ではなく、コミュニティが所有している仕様です。</span><span class="sxs-lookup"><span data-stu-id="09531-107">OWIN is a community-owned specification, not an implementation.</span></span> <span data-ttu-id="09531-108">Katana プロジェクトには、Microsoft によって開発されたオープン ソース OWIN コンポーネントのセットです。</span><span class="sxs-lookup"><span data-stu-id="09531-108">The Katana project is a set of open-source OWIN components developed by Microsoft.</span></span> <span data-ttu-id="09531-109">OWIN と Katana の両方の一般的な概要については、次を参照してください。[プロジェクト Katana の概要を、](an-overview-of-project-katana.md)します。</span><span class="sxs-lookup"><span data-stu-id="09531-109">For a general overview of both OWIN and Katana, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="09531-110">この記事でが、右をジャンプを開始するコードにします。</span><span class="sxs-lookup"><span data-stu-id="09531-110">In this article, I will jump right into code to get started.</span></span>

<span data-ttu-id="09531-111">このチュートリアルでは[Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566)、Visual Studio 2012 を使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="09531-111">This tutorial uses [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), but you can also use Visual Studio 2012.</span></span> <span data-ttu-id="09531-112">いくつかの手順は、Visual Studio 2012 では、下記のメモをここでは異なります。</span><span class="sxs-lookup"><span data-stu-id="09531-112">A few of the steps are different in Visual Studio 2012, which I note below.</span></span>

## <a name="host-owin-in-iis"></a><span data-ttu-id="09531-113">OWIN を IIS でホストします。</span><span class="sxs-lookup"><span data-stu-id="09531-113">Host OWIN in IIS</span></span>

<span data-ttu-id="09531-114">このセクションでは、IIS で OWIN をホストされます。</span><span class="sxs-lookup"><span data-stu-id="09531-114">In this section, we'll host OWIN in IIS.</span></span> <span data-ttu-id="09531-115">このオプションは、IIS の完成度の高い機能セットと共に OWIN パイプラインの構成可能性と柔軟性を使用できます。</span><span class="sxs-lookup"><span data-stu-id="09531-115">This option gives you the flexibility and composability of an OWIN pipeline together with the mature feature set of IIS.</span></span> <span data-ttu-id="09531-116">このオプションを使用して、OWIN アプリケーションは、ASP.NET の要求パイプラインで実行されます。</span><span class="sxs-lookup"><span data-stu-id="09531-116">Using this option, the OWIN application runs in the ASP.NET request pipeline.</span></span>

<span data-ttu-id="09531-117">最初に、新しい ASP.NET Web アプリケーション プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="09531-117">First, create a new ASP.NET Web Application project.</span></span> <span data-ttu-id="09531-118">(Visual Studio 2012 では、空の ASP.NET Web アプリケーション プロジェクトの種類を使用します)。</span><span class="sxs-lookup"><span data-stu-id="09531-118">(In Visual Studio 2012, use the ASP.NET Empty Web Application project type.)</span></span>

![](getting-started-with-owin-and-katana/_static/image1.png)

<span data-ttu-id="09531-119">**新しい ASP.NET プロジェクト**ダイアログ ボックスで、**空**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="09531-119">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span>

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a><span data-ttu-id="09531-120">NuGet パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="09531-120">Add NuGet Packages</span></span>

<span data-ttu-id="09531-121">次に、必要な NuGet パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="09531-121">Next, add the required NuGet packages.</span></span> <span data-ttu-id="09531-122">**ツール**メニューの  **NuGet パッケージ マネージャー**を選択し、**パッケージ マネージャー コンソール**します。</span><span class="sxs-lookup"><span data-stu-id="09531-122">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="09531-123">パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="09531-123">In the Package Manager Console window, type the following command:</span></span>

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a><span data-ttu-id="09531-124">Startup クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="09531-124">Add a Startup Class</span></span>

<span data-ttu-id="09531-125">次に、OWIN startup クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="09531-125">Next, add an OWIN startup class.</span></span> <span data-ttu-id="09531-126">ソリューション エクスプ ローラーでプロジェクトを右クリックし、選択**追加**を選択し、**新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="09531-126">In Solution Explorer, right-click the project and select **Add**, then select **New Item**.</span></span> <span data-ttu-id="09531-127">**新しい項目の追加**ダイアログ ボックスで、 **Owin Startup クラス**します。</span><span class="sxs-lookup"><span data-stu-id="09531-127">In the **Add New Item** dialog, select **Owin Startup class**.</span></span> <span data-ttu-id="09531-128">スタートアップ クラスの構成の詳細については、次を参照してください。 [OWIN スタートアップ クラス検出](owin-startup-class-detection.md)します。</span><span class="sxs-lookup"><span data-stu-id="09531-128">For more info on configuring the startup class, see [OWIN Startup Class Detection](owin-startup-class-detection.md).</span></span>

![](getting-started-with-owin-and-katana/_static/image4.png)

<span data-ttu-id="09531-129">`Startup1.Configuration` メソッドに次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="09531-129">Add the following code to the `Startup1.Configuration` method:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

<span data-ttu-id="09531-130">このコードを受け取る関数として実装されている、OWIN パイプラインにミドルウェアの簡単なピースを追加します、 **Microsoft.Owin.IOwinContext**インスタンス。</span><span class="sxs-lookup"><span data-stu-id="09531-130">This code adds a simple piece of middleware to the OWIN pipeline, implemented as a function that receives a **Microsoft.Owin.IOwinContext** instance.</span></span> <span data-ttu-id="09531-131">サーバーは、HTTP 要求を受信すると、OWIN パイプラインは、ミドルウェアを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="09531-131">When the server receives an HTTP request, the OWIN pipeline invokes the middleware.</span></span> <span data-ttu-id="09531-132">ミドルウェアは、応答のコンテンツの種類を設定し、応答本文を書き込みます。</span><span class="sxs-lookup"><span data-stu-id="09531-132">The middleware sets the content type for the response and writes the response body.</span></span>

> [!NOTE]
> <span data-ttu-id="09531-133">OWIN Startup クラス テンプレートは、Visual Studio 2013 で使用できます。</span><span class="sxs-lookup"><span data-stu-id="09531-133">The OWIN Startup class template is available in Visual Studio 2013.</span></span> <span data-ttu-id="09531-134">Visual Studio 2012 を使用している場合は、という名前の新しい空のクラスを追加だけ`Startup1`、し、次のコードを貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="09531-134">If you are using Visual Studio 2012, just add a new empty class named `Startup1`, and paste in the following code:</span></span>


[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a><span data-ttu-id="09531-135">アプリケーションを実行する</span><span class="sxs-lookup"><span data-stu-id="09531-135">Run the Application</span></span>

<span data-ttu-id="09531-136">F5 キーを押して、デバッグを開始します。</span><span class="sxs-lookup"><span data-stu-id="09531-136">Press F5 to begin debugging.</span></span> <span data-ttu-id="09531-137">Visual Studio はブラウザー ウィンドウを開いて`http://localhost:*port*/`します。</span><span class="sxs-lookup"><span data-stu-id="09531-137">Visual Studio will open a browser window to `http://localhost:*port*/`.</span></span> <span data-ttu-id="09531-138">ページは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="09531-138">The page should look like the following:</span></span>

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a><span data-ttu-id="09531-139">コンソール アプリケーションで OWIN を自己ホストします。</span><span class="sxs-lookup"><span data-stu-id="09531-139">Self-Host OWIN in a Console Application</span></span>

<span data-ttu-id="09531-140">カスタムのプロセスで自己ホストする IIS のホストからこのアプリケーションを変換する簡単です。</span><span class="sxs-lookup"><span data-stu-id="09531-140">It's easy to convert this application from IIS hosting to self-hosting in a custom process.</span></span> <span data-ttu-id="09531-141">IIS ホスティングでは、IIS は、HTTP サーバーとは、サービスをホストするプロセスとして機能します。</span><span class="sxs-lookup"><span data-stu-id="09531-141">With IIS hosting, IIS acts as both the HTTP server and as the process that hosts the service.</span></span> <span data-ttu-id="09531-142">、自己ホストによる、アプリケーションは、プロセスを作成し、使用、 **HttpListener** HTTP サーバーとしてのクラス。</span><span class="sxs-lookup"><span data-stu-id="09531-142">With self-hosting, your application creates the process and uses the **HttpListener** class as the HTTP server.</span></span>

<span data-ttu-id="09531-143">Visual Studio で、新しいコンソール アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="09531-143">In Visual Studio, create a new console application.</span></span> <span data-ttu-id="09531-144">パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="09531-144">In the Package Manager Console window, type the following command:</span></span>

`Install-Package Microsoft.Owin.SelfHost -Pre`

<span data-ttu-id="09531-145">追加、`Startup1`このチュートリアルのパート 1 からクラスをプロジェクト。</span><span class="sxs-lookup"><span data-stu-id="09531-145">Add a `Startup1` class from part 1 of this tutorial to the project.</span></span> <span data-ttu-id="09531-146">このクラスを変更する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="09531-146">You don't need to modify this class.</span></span>

<span data-ttu-id="09531-147">アプリケーションの実装`Main`メソッドとして、次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="09531-147">Implement the application's `Main` method as follows.</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

<span data-ttu-id="09531-148">サーバーでは、リッスンを開始、コンソール アプリケーションを実行すると`http://localhost:9000`します。</span><span class="sxs-lookup"><span data-stu-id="09531-148">When you run the console application, the server starts listening to `http://localhost:9000`.</span></span> <span data-ttu-id="09531-149">Web ブラウザーでこのアドレスに移動する場合は、"Hello world"ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="09531-149">If you navigate to this address in a web browser, you will see the "Hello world" page.</span></span>

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a><span data-ttu-id="09531-150">OWIN の診断を追加します。</span><span class="sxs-lookup"><span data-stu-id="09531-150">Add OWIN Diagnostics</span></span>

<span data-ttu-id="09531-151">Microsoft.Owin.Diagnostics パッケージには、未処理の例外をキャッチし、エラーの詳細に HTML ページを表示するミドルウェアが含まれています。</span><span class="sxs-lookup"><span data-stu-id="09531-151">The Microsoft.Owin.Diagnostics package contains middleware that catches unhandled exceptions and displays an HTML page with error details.</span></span> <span data-ttu-id="09531-152">このページの機能は、ASP.NET エラー ページとも呼ばれるよく似た、"[死の黄色い画面](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)"(YSOD)。</span><span class="sxs-lookup"><span data-stu-id="09531-152">This page functions much like the ASP.NET error page that is sometimes called the "[yellow screen of death](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD).</span></span> <span data-ttu-id="09531-153">YSOD と同様には、Katana のエラー ページは、開発中は、便利ですが、運用モードで無効にすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="09531-153">Like the YSOD, the Katana error page is useful during development, but it's a good practice to disable it in production mode.</span></span>

<span data-ttu-id="09531-154">プロジェクトで診断パッケージをインストールするには、パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="09531-154">To install the Diagnostics package in your project, type the following command in the Package Manager Console window:</span></span>

`install-package Microsoft.Owin.Diagnostics –Pre`

<span data-ttu-id="09531-155">コードを変更、`Startup1.Configuration`メソッドとして、次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="09531-155">Change the code in your `Startup1.Configuration` method as follows:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

<span data-ttu-id="09531-156">今すぐ Visual Studio は、例外で中断しないように、デバッグを行わず、アプリケーションの実行に ctrl キーを押しながら f5 キーを使用します。</span><span class="sxs-lookup"><span data-stu-id="09531-156">Now use CTRL+F5 to run the application without debugging, so that Visual Studio will not break on the exception.</span></span> <span data-ttu-id="09531-157">アプリケーションの動作と同じと同様に移動するまで`http://localhost/fail`、この時点では、アプリケーションは、例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="09531-157">The application behaves the same as before, until you navigate to `http://localhost/fail`, at which point the application throws the exception.</span></span> <span data-ttu-id="09531-158">エラー ページ ミドルウェアが例外をキャッチし、エラーに関する情報を HTML ページを表示します。</span><span class="sxs-lookup"><span data-stu-id="09531-158">The error page middleware will catch the exception and display an HTML page with information about the error.</span></span> <span data-ttu-id="09531-159">スタック、クエリ文字列、cookie、要求ヘッダー、および OWIN 環境変数を確認するには、タブをクリックすることができます。</span><span class="sxs-lookup"><span data-stu-id="09531-159">You can click the tabs to see the stack, query string, cookies, request header, and OWIN environment variables.</span></span>

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a><span data-ttu-id="09531-160">次の手順</span><span class="sxs-lookup"><span data-stu-id="09531-160">Next Steps</span></span>

- [<span data-ttu-id="09531-161">OWIN スタートアップ クラス検出</span><span class="sxs-lookup"><span data-stu-id="09531-161">OWIN Startup Class Detection</span></span>](owin-startup-class-detection.md)
- [<span data-ttu-id="09531-162">OWIN を使用して、ASP.NET Web API を自己ホスト</span><span class="sxs-lookup"><span data-stu-id="09531-162">Use OWIN to Self-Host ASP.NET Web API</span></span>](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [<span data-ttu-id="09531-163">OWIN を使用して、SignalR セルフホスト</span><span class="sxs-lookup"><span data-stu-id="09531-163">Use OWIN to Self-Host SignalR</span></span>](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
