---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: "OData v4 クライアント アプリ (c#) を作成 |Microsoft ドキュメント"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2014
ms.topic: article
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: 51a3c7b9c5b6525d6d82b9a45910f58b71268b7f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="create-an-odata-v4-client-app-c"></a><span data-ttu-id="68fdc-102">OData v4 クライアント アプリでは (c#) を作成します。</span><span class="sxs-lookup"><span data-stu-id="68fdc-102">Create an OData v4 Client App (C#)</span></span>
====================
<span data-ttu-id="68fdc-103">によって[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="68fdc-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="68fdc-104">前のチュートリアルでは、CRUD 操作をサポートする基本的な OData サービスを作成します。</span><span class="sxs-lookup"><span data-stu-id="68fdc-104">In the previous tutorial, you created a basic OData service that supports CRUD operations.</span></span> <span data-ttu-id="68fdc-105">これで、サービスのクライアントを作成しましょう。</span><span class="sxs-lookup"><span data-stu-id="68fdc-105">Now let's create a client for the service.</span></span>

<span data-ttu-id="68fdc-106">Visual Studio の新しいインスタンスを起動し、新しいコンソール アプリケーション プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="68fdc-106">Start a new instance of Visual Studio and create a new console application project.</span></span> <span data-ttu-id="68fdc-107">**新しいプロジェクト**ダイアログで、**インストール** &gt; **テンプレート** &gt; **Visual c#** &gt; **Windows デスクトップ**を選択し、**コンソール アプリケーション**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="68fdc-107">In the **New Project** dialog, select **Installed** &gt; **Templates** &gt; **Visual C#** &gt; **Windows Desktop**, and select the **Console Application** template.</span></span> <span data-ttu-id="68fdc-108">プロジェクトに名前を&quot;ProductsApp&quot;です。</span><span class="sxs-lookup"><span data-stu-id="68fdc-108">Name the project &quot;ProductsApp&quot;.</span></span>

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="68fdc-109">コンソール アプリは、OData サービスを含む Visual Studio ソリューションに追加することもできます。</span><span class="sxs-lookup"><span data-stu-id="68fdc-109">You can also add the console app to the same Visual Studio solution that contains the OData service.</span></span>


## <a name="install-the-odata-client-code-generator"></a><span data-ttu-id="68fdc-110">OData クライアント コード ジェネレーターをインストールします。</span><span class="sxs-lookup"><span data-stu-id="68fdc-110">Install the OData Client Code Generator</span></span>

<span data-ttu-id="68fdc-111">**ツール**メニューの **拡張機能と更新プログラム**です。</span><span class="sxs-lookup"><span data-stu-id="68fdc-111">From the **Tools** menu, select **Extensions and Updates**.</span></span> <span data-ttu-id="68fdc-112">選択**オンライン** &gt; **Visual Studio ギャラリー**です。</span><span class="sxs-lookup"><span data-stu-id="68fdc-112">Select **Online** &gt; **Visual Studio Gallery**.</span></span> <span data-ttu-id="68fdc-113">[検索] ボックスで検索&quot;OData クライアントのコード ジェネレーター&quot;です。</span><span class="sxs-lookup"><span data-stu-id="68fdc-113">In the search box, search for &quot;OData Client Code Generator&quot;.</span></span> <span data-ttu-id="68fdc-114">をクリックして**ダウンロード**VSIX をインストールします。</span><span class="sxs-lookup"><span data-stu-id="68fdc-114">Click **Download** to install the VSIX.</span></span> <span data-ttu-id="68fdc-115">Visual Studio を再起動するように求められます可能性があります。</span><span class="sxs-lookup"><span data-stu-id="68fdc-115">You might be prompted to restart Visual Studio.</span></span>

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a><span data-ttu-id="68fdc-116">OData サービスをローカルで実行します。</span><span class="sxs-lookup"><span data-stu-id="68fdc-116">Run the OData Service Locally</span></span>

<span data-ttu-id="68fdc-117">Visual Studio から ProductService プロジェクトを実行します。</span><span class="sxs-lookup"><span data-stu-id="68fdc-117">Run the ProductService project from Visual Studio.</span></span> <span data-ttu-id="68fdc-118">既定では、Visual Studio は、アプリケーション ルートにブラウザーを起動します。</span><span class="sxs-lookup"><span data-stu-id="68fdc-118">By default, Visual Studio launches a browser to the application root.</span></span> <span data-ttu-id="68fdc-119">URI に注意してくださいこれは、次の手順で必要があります。</span><span class="sxs-lookup"><span data-stu-id="68fdc-119">Note the URI; you will need this in the next step.</span></span> <span data-ttu-id="68fdc-120">アプリケーションが実行されている状態のままにします。</span><span class="sxs-lookup"><span data-stu-id="68fdc-120">Leave the application running.</span></span>

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> <span data-ttu-id="68fdc-121">同じソリューション内の両方のプロジェクトを配置する場合は、デバッグを行わず、ProductService プロジェクトを実行することを確認してください。</span><span class="sxs-lookup"><span data-stu-id="68fdc-121">If you put both projects in the same solution, make sure to run the ProductService project without debugging.</span></span> <span data-ttu-id="68fdc-122">次の手順では、コンソール アプリケーション プロジェクトを変更するときに実行されているサービスを保持する必要があります。</span><span class="sxs-lookup"><span data-stu-id="68fdc-122">In the next step, you will need to keep the service running while you modify the console application project.</span></span>


## <a name="generate-the-service-proxy"></a><span data-ttu-id="68fdc-123">サービス プロキシを生成します。</span><span class="sxs-lookup"><span data-stu-id="68fdc-123">Generate the Service Proxy</span></span>

<span data-ttu-id="68fdc-124">サービス プロキシは、OData サービスにアクセスするためのメソッドを定義する .NET クラスです。</span><span class="sxs-lookup"><span data-stu-id="68fdc-124">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="68fdc-125">プロキシは、HTTP 要求にメソッド呼び出しを変換します。</span><span class="sxs-lookup"><span data-stu-id="68fdc-125">The proxy translates method calls into HTTP requests.</span></span> <span data-ttu-id="68fdc-126">実行して、プロキシ クラスを作成するが、 [T4 テンプレート](https://msdn.microsoft.com/library/bb126445.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="68fdc-126">You will create the proxy class by running a [T4 template](https://msdn.microsoft.com/library/bb126445.aspx).</span></span>

<span data-ttu-id="68fdc-127">プロジェクトを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="68fdc-127">Right-click the project.</span></span> <span data-ttu-id="68fdc-128">選択**追加** &gt; **新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="68fdc-128">Select **Add** &gt; **New Item**.</span></span>

![](create-an-odata-v4-client-app/_static/image5.png)

<span data-ttu-id="68fdc-129">**新しい項目の追加**ダイアログで、 **Visual c# アイテム** &gt; **コード** &gt; **OData クライアント**です。</span><span class="sxs-lookup"><span data-stu-id="68fdc-129">In the **Add New Item** dialog, select **Visual C# Items** &gt; **Code** &gt; **OData Client**.</span></span> <span data-ttu-id="68fdc-130">テンプレートに名前を&quot;ProductClient.tt&quot;です。</span><span class="sxs-lookup"><span data-stu-id="68fdc-130">Name the template &quot;ProductClient.tt&quot;.</span></span> <span data-ttu-id="68fdc-131">をクリックして**追加**でセキュリティの警告 をクリックします。</span><span class="sxs-lookup"><span data-stu-id="68fdc-131">Click **Add** and click through the security warning.</span></span>

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

<span data-ttu-id="68fdc-132">この時点では、無視してかまいませんが、エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="68fdc-132">At this point, you'll get an error, which you can ignore.</span></span> <span data-ttu-id="68fdc-133">Visual Studio は、テンプレートを自動的に実行されますが、テンプレートには、一部の構成設定が必要があります最初。</span><span class="sxs-lookup"><span data-stu-id="68fdc-133">Visual Studio automatically runs the template, but the template needs some configuration settings first.</span></span>

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

<span data-ttu-id="68fdc-134">ProductClient.odata.config ファイルを開きます。`Parameter` ProductService プロジェクト (前の手順) 元の URI での貼り付けの要素。</span><span class="sxs-lookup"><span data-stu-id="68fdc-134">Open the file ProductClient.odata.config. In the `Parameter` element, paste in the URI from the ProductService project (previous step).</span></span> <span data-ttu-id="68fdc-135">例:</span><span class="sxs-lookup"><span data-stu-id="68fdc-135">For example:</span></span>

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

<span data-ttu-id="68fdc-136">テンプレートをもう一度実行します。</span><span class="sxs-lookup"><span data-stu-id="68fdc-136">Run the template again.</span></span> <span data-ttu-id="68fdc-137">ソリューション エクスプ ローラーで、ProductClient.tt ファイルを右クリックし、選択**カスタム ツールの実行**です。</span><span class="sxs-lookup"><span data-stu-id="68fdc-137">In Solution Explorer, right click the ProductClient.tt file and select **Run Custom Tool**.</span></span>

<span data-ttu-id="68fdc-138">テンプレートでは、プロキシを定義する ProductClient.cs をという名前のコード ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="68fdc-138">The template creates a code file named ProductClient.cs that defines the proxy.</span></span> <span data-ttu-id="68fdc-139">OData エンドポイントを変更する場合に、アプリを開発するときは、プロキシを更新するには、再度テンプレートを実行します。</span><span class="sxs-lookup"><span data-stu-id="68fdc-139">As you develop your app, if you change the OData endpoint, run the template again to update the proxy.</span></span>

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a><span data-ttu-id="68fdc-140">サービス プロキシを使用して、OData サービスの呼び出し</span><span class="sxs-lookup"><span data-stu-id="68fdc-140">Use the Service Proxy to Call the OData Service</span></span>

<span data-ttu-id="68fdc-141">Program.cs ファイルを開き、次のように定型コードを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="68fdc-141">Open the file Program.cs and replace the boilerplate code with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

<span data-ttu-id="68fdc-142">値を置き換える*serviceUri*以前からサービス URI とします。</span><span class="sxs-lookup"><span data-stu-id="68fdc-142">Replace the value of *serviceUri* with the service URI from earlier.</span></span>

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

<span data-ttu-id="68fdc-143">アプリを実行するときに、次と出力されます。</span><span class="sxs-lookup"><span data-stu-id="68fdc-143">When you run the app, it should output the following:</span></span>

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
