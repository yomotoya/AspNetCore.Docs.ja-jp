---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: OData v4 クライアント アプリ (c#) の作成 | Microsoft ドキュメント
author: MikeWasson
description: ''
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
ms.locfileid: "28036701"
---
<a name="create-an-odata-v4-client-app-c"></a><span data-ttu-id="4f69c-102">OData v4 クライアント アプリ (c#) の作成</span><span class="sxs-lookup"><span data-stu-id="4f69c-102">Create an OData v4 Client App (C#)</span></span>
====================
<span data-ttu-id="4f69c-103">作成者 [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="4f69c-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="4f69c-104">前のチュートリアルでは、CRUD 操作をサポートする基本的な OData サービスを作成しました。</span><span class="sxs-lookup"><span data-stu-id="4f69c-104">In the previous tutorial, you created a basic OData service that supports CRUD operations.</span></span> <span data-ttu-id="4f69c-105">次はサービスのクライアントを作成しましょう。</span><span class="sxs-lookup"><span data-stu-id="4f69c-105">Now let's create a client for the service.</span></span>

<span data-ttu-id="4f69c-106">Visual Studio の新しいインスタンスを開始し、新しいコンソール アプリケーション プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="4f69c-106">Start a new instance of Visual Studio and create a new console application project.</span></span> <span data-ttu-id="4f69c-107">**[新しいプロジェクト]** ダイアログで **[インストール済み]** &gt; **[テンプレート]** &gt; **[Visual C#]** &gt; **[Windows デスクトップ]** を選択し、**[コンソール アプリケーション]** テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="4f69c-107">In the **New Project** dialog, select **Installed** &gt; **Templates** &gt; **Visual C#** &gt; **Windows Desktop**, and select the **Console Application** template.</span></span> <span data-ttu-id="4f69c-108">プロジェクトには &quot;ProductsApp&quot; と名前をつけます。</span><span class="sxs-lookup"><span data-stu-id="4f69c-108">Name the project &quot;ProductsApp&quot;.</span></span>

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="4f69c-109">OData サービスを含んでいる同じ Visual Studio ソリューションにコンソール アプリを追加することもできます。</span><span class="sxs-lookup"><span data-stu-id="4f69c-109">You can also add the console app to the same Visual Studio solution that contains the OData service.</span></span>


## <a name="install-the-odata-client-code-generator"></a><span data-ttu-id="4f69c-110">OData Client Code Generator をインストールする</span><span class="sxs-lookup"><span data-stu-id="4f69c-110">Install the OData Client Code Generator</span></span>

<span data-ttu-id="4f69c-111">**[ツール]** メニューから **[拡張機能と更新プログラム]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="4f69c-111">From the **Tools** menu, select **Extensions and Updates**.</span></span> <span data-ttu-id="4f69c-112">**[オンライン]** &gt; **[Visual Studio Gallery]** を選択して、</span><span class="sxs-lookup"><span data-stu-id="4f69c-112">Select **Online** &gt; **Visual Studio Gallery**.</span></span> <span data-ttu-id="4f69c-113">[検索] ボックスから &quot;OData Client Code Generator&quot; を検索してください。</span><span class="sxs-lookup"><span data-stu-id="4f69c-113">In the search box, search for &quot;OData Client Code Generator&quot;.</span></span> <span data-ttu-id="4f69c-114">**[ダウンロード]** をクリックして VSIX をインストールします。</span><span class="sxs-lookup"><span data-stu-id="4f69c-114">Click **Download** to install the VSIX.</span></span> <span data-ttu-id="4f69c-115">ここでは　Visual Studio の再起動を求められることがあります。</span><span class="sxs-lookup"><span data-stu-id="4f69c-115">You might be prompted to restart Visual Studio.</span></span>

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a><span data-ttu-id="4f69c-116">OData サービスをローカルで実行する</span><span class="sxs-lookup"><span data-stu-id="4f69c-116">Run the OData Service Locally</span></span>

<span data-ttu-id="4f69c-117">Visual Studio から ProductService プロジェクトを実行します。</span><span class="sxs-lookup"><span data-stu-id="4f69c-117">Run the ProductService project from Visual Studio.</span></span> <span data-ttu-id="4f69c-118">既定では、Visual Studio はアプリケーション ルートに対してブラウザーを起動します。</span><span class="sxs-lookup"><span data-stu-id="4f69c-118">By default, Visual Studio launches a browser to the application root.</span></span> <span data-ttu-id="4f69c-119">次の手順で必要になるので、ここでの URI を書き留めておいてください。</span><span class="sxs-lookup"><span data-stu-id="4f69c-119">Note the URI; you will need this in the next step.</span></span> <span data-ttu-id="4f69c-120">アプリケーションは実行されている状態のままにします。</span><span class="sxs-lookup"><span data-stu-id="4f69c-120">Leave the application running.</span></span>

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> <span data-ttu-id="4f69c-121">同じソリューションに両方のプロジェクトを配置する場合は ProductService プロジェクトがデバッグをせずに実行されていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="4f69c-121">If you put both projects in the same solution, make sure to run the ProductService project without debugging.</span></span> <span data-ttu-id="4f69c-122">次の手順では、コンソール アプリケーション プロジェクトを変更している間、サービスが実行され続けている必要があります。</span><span class="sxs-lookup"><span data-stu-id="4f69c-122">In the next step, you will need to keep the service running while you modify the console application project.</span></span>


## <a name="generate-the-service-proxy"></a><span data-ttu-id="4f69c-123">サービス プロキシを生成する</span><span class="sxs-lookup"><span data-stu-id="4f69c-123">Generate the Service Proxy</span></span>

<span data-ttu-id="4f69c-124">サービス プロキシは、OData サービスにアクセスするためのメソッドを定義する .NET クラスです。</span><span class="sxs-lookup"><span data-stu-id="4f69c-124">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="4f69c-125">プロキシは、HTTP 要求にメソッド呼び出しを変換します。</span><span class="sxs-lookup"><span data-stu-id="4f69c-125">The proxy translates method calls into HTTP requests.</span></span> <span data-ttu-id="4f69c-126">実行して、プロキシ クラスを作成するが、 [T4 テンプレート](https://msdn.microsoft.com/library/bb126445.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="4f69c-126">You will create the proxy class by running a [T4 template](https://msdn.microsoft.com/library/bb126445.aspx).</span></span>

<span data-ttu-id="4f69c-127">プロジェクトで右クリックします。</span><span class="sxs-lookup"><span data-stu-id="4f69c-127">Right-click the project.</span></span> <span data-ttu-id="4f69c-128">**[追加]** &gt; **[新しい項目]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="4f69c-128">Select **Add** &gt; **New Item**.</span></span>

![](create-an-odata-v4-client-app/_static/image5.png)

<span data-ttu-id="4f69c-129">**[新しい項目の追加]** ダイアログで、**[Visual C# アイテム]** &gt; **[コード]** &gt; **[OData クライアント]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="4f69c-129">In the **Add New Item** dialog, select **Visual C# Items** &gt; **Code** &gt; **OData Client**.</span></span> <span data-ttu-id="4f69c-130">テンプレートには &quot;ProductClient.tt&quot; と名前を付けてください。</span><span class="sxs-lookup"><span data-stu-id="4f69c-130">Name the template &quot;ProductClient.tt&quot;.</span></span> <span data-ttu-id="4f69c-131">**[追加]** をクリックして、セキュリティ警告の画面を進めます。</span><span class="sxs-lookup"><span data-stu-id="4f69c-131">Click **Add** and click through the security warning.</span></span>

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

<span data-ttu-id="4f69c-132">この時点でエラーが表示されますが、無視してもよいものです。</span><span class="sxs-lookup"><span data-stu-id="4f69c-132">At this point, you'll get an error, which you can ignore.</span></span> <span data-ttu-id="4f69c-133">Visual Studio は、自動的にテンプレートを実行させますが、このテンプレートは最初にいくつかの構成設定を必要とします。</span><span class="sxs-lookup"><span data-stu-id="4f69c-133">Visual Studio automatically runs the template, but the template needs some configuration settings first.</span></span>

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

<span data-ttu-id="4f69c-134">ProductClient.odata.config ファイルを開きます。`Parameter` 要素の中へ、ProductService プロジェクト (前の手順) からコピーしてきた URI を貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="4f69c-134">Open the file ProductClient.odata.config. In the `Parameter` element, paste in the URI from the ProductService project (previous step).</span></span> <span data-ttu-id="4f69c-135">例えば:</span><span class="sxs-lookup"><span data-stu-id="4f69c-135">For example:</span></span>

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

<span data-ttu-id="4f69c-136">テンプレートをもう一度実行してください。</span><span class="sxs-lookup"><span data-stu-id="4f69c-136">Run the template again.</span></span> <span data-ttu-id="4f69c-137">ソリューション エクスプローラーで、ProductClient.tt ファイルを右クリックし **[カスタム ツールの実行]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="4f69c-137">In Solution Explorer, right click the ProductClient.tt file and select **Run Custom Tool**.</span></span>

<span data-ttu-id="4f69c-138">テンプレートは、プロキシを定義する ProductClient.cs という名前のコード ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="4f69c-138">The template creates a code file named ProductClient.cs that defines the proxy.</span></span> <span data-ttu-id="4f69c-139">アプリを開発するときに、OData エンドポイントを変更した場合は、プロキシを更新するためにテンプレートを再実行してください。</span><span class="sxs-lookup"><span data-stu-id="4f69c-139">As you develop your app, if you change the OData endpoint, run the template again to update the proxy.</span></span>

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a><span data-ttu-id="4f69c-140">OData サービスの呼び出すためにサービス プロキシを使用する</span><span class="sxs-lookup"><span data-stu-id="4f69c-140">Use the Service Proxy to Call the OData Service</span></span>

<span data-ttu-id="4f69c-141">Program.cs ファイルを開き、次のように定型コードを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="4f69c-141">Open the file Program.cs and replace the boilerplate code with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

<span data-ttu-id="4f69c-142">*serviceUri* の値を前のサービス URI へ置き換えます。</span><span class="sxs-lookup"><span data-stu-id="4f69c-142">Replace the value of *serviceUri* with the service URI from earlier.</span></span>

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

<span data-ttu-id="4f69c-143">アプリを実行すると、次のような出力を得られるはずです。</span><span class="sxs-lookup"><span data-stu-id="4f69c-143">When you run the app, it should output the following:</span></span>

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
