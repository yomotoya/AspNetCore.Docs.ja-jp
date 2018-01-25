---
uid: web-api/overview/older-versions/self-host-a-web-api
title: "ASP.NET Web API 1 (c#) を自己ホスト |Microsoft ドキュメント"
author: MikeWasson
description: "ASP.NET Web API には、IIS は不要です。 自己ホスト プロセスを独自の web API をホストできます。 このチュートリアルでは、applic コンソール内の web API をホストする方法を説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2012
ms.topic: article
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: 564f859e73a88ac9c5f27e9b8f7409ec126642f8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="self-host-aspnet-web-api-1-c"></a><span data-ttu-id="e1d87-105">ASP.NET Web API 1 (c#) を自己ホストします。</span><span class="sxs-lookup"><span data-stu-id="e1d87-105">Self-Host ASP.NET Web API 1 (C#)</span></span>
====================
<span data-ttu-id="e1d87-106">によって[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e1d87-106">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="e1d87-107">ASP.NET Web API には、IIS は不要です。</span><span class="sxs-lookup"><span data-stu-id="e1d87-107">ASP.NET Web API does not require IIS.</span></span> <span data-ttu-id="e1d87-108">自己ホスト プロセスを独自の web API をホストできます。</span><span class="sxs-lookup"><span data-stu-id="e1d87-108">You can self-host a web API in your own host process.</span></span> <span data-ttu-id="e1d87-109">このチュートリアルでは、コンソール アプリケーション内部の web API をホストする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="e1d87-109">This tutorial shows how to host a web API inside a console application.</span></span>
> 
> <span data-ttu-id="e1d87-110">**新しいアプリケーションでは、Web API の自己ホストするのに OWIN を使用する必要があります。**</span><span class="sxs-lookup"><span data-stu-id="e1d87-110">**New applications should use OWIN to self-host Web API.**</span></span> <span data-ttu-id="e1d87-111">参照してください[OWIN を使用して ASP.NET Web API 2 を自己ホスト](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)です。</span><span class="sxs-lookup"><span data-stu-id="e1d87-111">See [Use OWIN to Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="e1d87-112">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="e1d87-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="e1d87-113">Web API 1</span><span class="sxs-lookup"><span data-stu-id="e1d87-113">Web API 1</span></span>
> - <span data-ttu-id="e1d87-114">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="e1d87-114">Visual Studio 2012</span></span>


## <a name="create-the-console-application-project"></a><span data-ttu-id="e1d87-115">コンソール アプリケーション プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="e1d87-115">Create the Console Application Project</span></span>

<span data-ttu-id="e1d87-116">Visual Studio を起動し、選択**新しいプロジェクト**から、**開始**ページ。</span><span class="sxs-lookup"><span data-stu-id="e1d87-116">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="e1d87-117">またはから、**ファイル**メニューの **新規**し**プロジェクト**です。</span><span class="sxs-lookup"><span data-stu-id="e1d87-117">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="e1d87-118">**テンプレート**ペインで、**インストールされたテンプレート**を展開し、 **Visual c#**ノード。</span><span class="sxs-lookup"><span data-stu-id="e1d87-118">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="e1d87-119">**Visual c#** **Windows**です。</span><span class="sxs-lookup"><span data-stu-id="e1d87-119">Under **Visual C#**, select **Windows**.</span></span> <span data-ttu-id="e1d87-120">プロジェクト テンプレートの一覧で選択**コンソール アプリケーション**です。</span><span class="sxs-lookup"><span data-stu-id="e1d87-120">In the list of project templates, select **Console Application**.</span></span> <span data-ttu-id="e1d87-121">プロジェクトに名前を&quot;SelfHost&quot;  をクリック**OK**です。</span><span class="sxs-lookup"><span data-stu-id="e1d87-121">Name the project &quot;SelfHost&quot; and click **OK**.</span></span>

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a><span data-ttu-id="e1d87-122">ターゲット フレームワーク (Visual Studio 2010) の設定します。</span><span class="sxs-lookup"><span data-stu-id="e1d87-122">Set the Target Framework (Visual Studio 2010)</span></span>

<span data-ttu-id="e1d87-123">Visual Studio 2010 を使用している場合は、.NET Framework 4.0 をターゲット フレームワークを変更します。</span><span class="sxs-lookup"><span data-stu-id="e1d87-123">If you are using Visual Studio 2010, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="e1d87-124">(既定では、プロジェクト テンプレートの対象、 [.Net Framework クライアント プロファイル](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile))。</span><span class="sxs-lookup"><span data-stu-id="e1d87-124">(By default, the project template targets the [.Net Framework Client Profile](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span></span>

<span data-ttu-id="e1d87-125">ソリューション エクスプ ローラーでプロジェクトを右クリックし、**プロパティ**です。</span><span class="sxs-lookup"><span data-stu-id="e1d87-125">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="e1d87-126">**ターゲット フレームワーク** ドロップダウン リストで、ターゲット フレームワークを .NET Framework 4.0 に変更します。</span><span class="sxs-lookup"><span data-stu-id="e1d87-126">In the **Target framework** dropdown list, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="e1d87-127">変更を適用するメッセージが表示されたら、クリックして**はい**です。</span><span class="sxs-lookup"><span data-stu-id="e1d87-127">When prompted to apply the change, click **Yes**.</span></span>

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a><span data-ttu-id="e1d87-128">NuGet Package Manager をインストールします。</span><span class="sxs-lookup"><span data-stu-id="e1d87-128">Install NuGet Package Manager</span></span>

<span data-ttu-id="e1d87-129">NuGet パッケージ マネージャーは、Web API アセンブリを ASP.NET 以外のプロジェクトに追加する最も簡単な方法です。</span><span class="sxs-lookup"><span data-stu-id="e1d87-129">The NuGet Package Manager is the easiest way to add the Web API assemblies to a non-ASP.NET project.</span></span>

<span data-ttu-id="e1d87-130">NuGet Package Manager がインストールされているかどうかをチェックするをクリックして、**ツール**Visual Studio のメニュー。</span><span class="sxs-lookup"><span data-stu-id="e1d87-130">To check if NuGet Package Manager is installed, click the **Tools** menu in Visual Studio.</span></span> <span data-ttu-id="e1d87-131">メニュー項目を表示する場合は**ライブラリ パッケージ マネージャー**、NuGet Package Manager 必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1d87-131">If you see a menu item called **Library Package Manager**, then you have NuGet Package Manager.</span></span>

<span data-ttu-id="e1d87-132">NuGet Package Manager をインストールします。</span><span class="sxs-lookup"><span data-stu-id="e1d87-132">To install NuGet Package Manager:</span></span>

1. <span data-ttu-id="e1d87-133">Visual Studio を起動します。</span><span class="sxs-lookup"><span data-stu-id="e1d87-133">Start Visual Studio.</span></span>
2. <span data-ttu-id="e1d87-134">**ツール**メニューの **拡張機能と更新プログラム**です。</span><span class="sxs-lookup"><span data-stu-id="e1d87-134">From the **Tools** menu, select **Extensions and Updates**.</span></span>
3. <span data-ttu-id="e1d87-135">**拡張機能と更新**ダイアログで、**オンライン**です。</span><span class="sxs-lookup"><span data-stu-id="e1d87-135">In the **Extensions and Updates** dialog, select **Online**.</span></span>
4. <span data-ttu-id="e1d87-136">"NuGet Package Manager"が表示されない場合は、検索ボックスに"nuget package manager"を入力します。</span><span class="sxs-lookup"><span data-stu-id="e1d87-136">If you don't see "NuGet Package Manager", type "nuget package manager" in the search box.</span></span>
5. <span data-ttu-id="e1d87-137">NuGet パッケージ マネージャーを選択し、クリックして**ダウンロード**です。</span><span class="sxs-lookup"><span data-stu-id="e1d87-137">Select the NuGet Package Manager and click **Download**.</span></span>
6. <span data-ttu-id="e1d87-138">ダウンロードが完了したら、インストールが求められます。</span><span class="sxs-lookup"><span data-stu-id="e1d87-138">After the download completes, you will be prompted to install.</span></span>
7. <span data-ttu-id="e1d87-139">インストールが完了したら、Visual Studio を再起動するように求められます可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e1d87-139">After the installation completes, you might be prompted to restart Visual Studio.</span></span>

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a><span data-ttu-id="e1d87-140">Web API NuGet パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="e1d87-140">Add the Web API NuGet Package</span></span>

<span data-ttu-id="e1d87-141">NuGet Package Manager をインストールした後は、Web API Self-Host パッケージをプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="e1d87-141">After NuGet Package Manager is installed, add the Web API Self-Host package to your project.</span></span>

1. <span data-ttu-id="e1d87-142">**ツール**メニューの **ライブラリ パッケージ マネージャー**です。</span><span class="sxs-lookup"><span data-stu-id="e1d87-142">From the **Tools** menu, select **Library Package Manager**.</span></span> <span data-ttu-id="e1d87-143">*注*: は表示されませんこのメニュー項目、NuGet Package Manager は正しくインストールされていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="e1d87-143">*Note*: If do you not see this menu item, make sure that NuGet Package Manager installed correctly.</span></span>
2. <span data-ttu-id="e1d87-144">選択**ソリューションの NuGet パッケージを管理しています.**</span><span class="sxs-lookup"><span data-stu-id="e1d87-144">Select **Manage NuGet Packages for Solution...**</span></span>
3. <span data-ttu-id="e1d87-145">**注目パッケージの管理**ダイアログで、**オンライン**です。</span><span class="sxs-lookup"><span data-stu-id="e1d87-145">In the **Manage NugGet Packages** dialog, select **Online**.</span></span>
4. <span data-ttu-id="e1d87-146">[検索] ボックスに入力&quot;Microsoft.AspNet.WebApi.SelfHost&quot;です。</span><span class="sxs-lookup"><span data-stu-id="e1d87-146">In the search box, type &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span></span>
5. <span data-ttu-id="e1d87-147">ASP.NET Web API Self Host パッケージを選択し、をクリックして**インストール**です。</span><span class="sxs-lookup"><span data-stu-id="e1d87-147">Select the ASP.NET Web API Self Host package and click **Install**.</span></span>
6. <span data-ttu-id="e1d87-148">パッケージをインストールした後にをクリックして**閉じる**ダイアログ ボックスを閉じます。</span><span class="sxs-lookup"><span data-stu-id="e1d87-148">After the package installs, click **Close** to close the dialog.</span></span>

> [!NOTE]
> <span data-ttu-id="e1d87-149">Microsoft.AspNet.WebApi.SelfHost、いない AspNetWebApi.SelfHost をという名前のパッケージをインストールすることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="e1d87-149">Make sure to install the package named Microsoft.AspNet.WebApi.SelfHost, not AspNetWebApi.SelfHost.</span></span>


![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="e1d87-150">モデルとコント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="e1d87-150">Create the Model and Controller</span></span>

<span data-ttu-id="e1d87-151">このチュートリアルは、同じモデルとコント ローラーとしてのクラスを使用して、[作業の開始](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)チュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="e1d87-151">This tutorial uses the same model and controller classes as the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="e1d87-152">という名前のパブリック クラスを追加`Product`です。</span><span class="sxs-lookup"><span data-stu-id="e1d87-152">Add a public class named `Product`.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

<span data-ttu-id="e1d87-153">という名前のパブリック クラスを追加`ProductsController`です。</span><span class="sxs-lookup"><span data-stu-id="e1d87-153">Add a public class named `ProductsController`.</span></span> <span data-ttu-id="e1d87-154">このクラスから派生**System.Web.Http.ApiController**です。</span><span class="sxs-lookup"><span data-stu-id="e1d87-154">Derive this class from **System.Web.Http.ApiController**.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

<span data-ttu-id="e1d87-155">このコント ローラーでコードの詳細については、次を参照してください。、[作業の開始](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)チュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="e1d87-155">For more information about the code in this controller, see the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span> <span data-ttu-id="e1d87-156">このコント ローラーには、次の 3 つの GET 操作を定義します。</span><span class="sxs-lookup"><span data-stu-id="e1d87-156">This controller defines three GET actions:</span></span>

| <span data-ttu-id="e1d87-157">URI</span><span class="sxs-lookup"><span data-stu-id="e1d87-157">URI</span></span> | <span data-ttu-id="e1d87-158">説明</span><span class="sxs-lookup"><span data-stu-id="e1d87-158">Description</span></span> |
| --- | --- |
| <span data-ttu-id="e1d87-159">/api/products</span><span class="sxs-lookup"><span data-stu-id="e1d87-159">/api/products</span></span> | <span data-ttu-id="e1d87-160">すべての製品の一覧を取得します。</span><span class="sxs-lookup"><span data-stu-id="e1d87-160">Get a list of all products.</span></span> |
| <span data-ttu-id="e1d87-161">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="e1d87-161">/api/products/*id*</span></span> | <span data-ttu-id="e1d87-162">ID によって製品を取得します。</span><span class="sxs-lookup"><span data-stu-id="e1d87-162">Get a product by ID.</span></span> |
| <span data-ttu-id="e1d87-163">/api/products/?category=*category*</span><span class="sxs-lookup"><span data-stu-id="e1d87-163">/api/products/?category=*category*</span></span> | <span data-ttu-id="e1d87-164">カテゴリによって製品の一覧を取得します。</span><span class="sxs-lookup"><span data-stu-id="e1d87-164">Get a list of products by category.</span></span> |

## <a name="host-the-web-api"></a><span data-ttu-id="e1d87-165">Web API をホストします。</span><span class="sxs-lookup"><span data-stu-id="e1d87-165">Host the Web API</span></span>

<span data-ttu-id="e1d87-166">Program.cs ファイルを開き、次の追加のステートメントを使用します。</span><span class="sxs-lookup"><span data-stu-id="e1d87-166">Open the file Program.cs and add the following using statements:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

<span data-ttu-id="e1d87-167">次のコードを追加、**プログラム**クラスです。</span><span class="sxs-lookup"><span data-stu-id="e1d87-167">Add the following code to the **Program** class.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a><span data-ttu-id="e1d87-168">(省略可能)HTTP URL Namespace 予約を追加します。</span><span class="sxs-lookup"><span data-stu-id="e1d87-168">(Optional) Add an HTTP URL Namespace Reservation</span></span>

<span data-ttu-id="e1d87-169">このアプリケーションがリッスンする`http://localhost:8080/`です。</span><span class="sxs-lookup"><span data-stu-id="e1d87-169">This application listens to `http://localhost:8080/`.</span></span> <span data-ttu-id="e1d87-170">既定では、特定の HTTP アドレスでリッスンするいると、管理者特権が必要です。</span><span class="sxs-lookup"><span data-stu-id="e1d87-170">By default, listening at a particular HTTP address requires administrator privileges.</span></span> <span data-ttu-id="e1d87-171">このチュートリアルを実行するときにそのため、エラーが発生この:「HTTP が URL http://+:8080/ を登録できません」はこのエラーを回避する 2 つの方法があります。</span><span class="sxs-lookup"><span data-stu-id="e1d87-171">When you run the tutorial, therefore, you may get this error: "HTTP could not register URL http://+:8080/" There are two ways to avoid this error:</span></span>

- <span data-ttu-id="e1d87-172">管理者特権での管理者のアクセス許可で Visual Studio を実行または</span><span class="sxs-lookup"><span data-stu-id="e1d87-172">Run Visual Studio with elevated administrator permissions, or</span></span>
- <span data-ttu-id="e1d87-173">Netsh.exe を使用して、URL を予約するアカウントのアクセス許可を与えます。</span><span class="sxs-lookup"><span data-stu-id="e1d87-173">Use Netsh.exe to give your account permissions to reserve the URL.</span></span>

<span data-ttu-id="e1d87-174">Netsh.exe を使用して、管理者特権でコマンド プロンプトを開きし、次のコマンド: 次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="e1d87-174">To use Netsh.exe, open a command prompt with administrator privileges and enter the following command:following command:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

<span data-ttu-id="e1d87-175">ここで*machine \username*は自分のユーザー アカウントです。</span><span class="sxs-lookup"><span data-stu-id="e1d87-175">where *machine\username* is your user account.</span></span>

<span data-ttu-id="e1d87-176">完了したら、自己ホスト型を必ず、予約を削除します。</span><span class="sxs-lookup"><span data-stu-id="e1d87-176">When you are finished self-hosting, be sure to delete the reservation:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a><span data-ttu-id="e1d87-177">Web API、クライアント アプリケーションから呼び出す (c#)</span><span class="sxs-lookup"><span data-stu-id="e1d87-177">Call the Web API from a Client Application (C#)</span></span>

<span data-ttu-id="e1d87-178">Web API を呼び出す単純なコンソール アプリケーションを作成してみましょう。</span><span class="sxs-lookup"><span data-stu-id="e1d87-178">Let's write a simple console application that calls the web API.</span></span>

<span data-ttu-id="e1d87-179">新しいコンソール アプリケーション プロジェクトをソリューションに追加します。</span><span class="sxs-lookup"><span data-stu-id="e1d87-179">Add a new console application project to the solution:</span></span>

- <span data-ttu-id="e1d87-180">ソリューション エクスプ ローラーでソリューションを右クリックし、**新しいプロジェクトの追加**です。</span><span class="sxs-lookup"><span data-stu-id="e1d87-180">In Solution Explorer, right-click the solution and select **Add New Project**.</span></span>
- <span data-ttu-id="e1d87-181">という名前の新しいコンソール アプリケーションを作成する&quot;ClientApp&quot;です。</span><span class="sxs-lookup"><span data-stu-id="e1d87-181">Create a new console application named &quot;ClientApp&quot;.</span></span>

![](self-host-a-web-api/_static/image5.png)

<span data-ttu-id="e1d87-182">NuGet パッケージ マネージャーを使用して、ASP.NET Web API Core Libraries パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="e1d87-182">Use NuGet Package Manager to add the ASP.NET Web API Core Libraries package:</span></span>

- <span data-ttu-id="e1d87-183">[ツール] メニューから選択**ライブラリ パッケージ マネージャー**です。</span><span class="sxs-lookup"><span data-stu-id="e1d87-183">From the Tools menu, select **Library Package Manager**.</span></span>
- <span data-ttu-id="e1d87-184">選択**ソリューションの NuGet パッケージを管理しています.**</span><span class="sxs-lookup"><span data-stu-id="e1d87-184">Select **Manage NuGet Packages for Solution...**</span></span>
- <span data-ttu-id="e1d87-185">**NuGet パッケージの管理**ダイアログで、**オンライン**です。</span><span class="sxs-lookup"><span data-stu-id="e1d87-185">In the **Manage NuGet Packages** dialog, select **Online**.</span></span>
- <span data-ttu-id="e1d87-186">[検索] ボックスに入力&quot;Microsoft.AspNet.WebApi.Client&quot;です。</span><span class="sxs-lookup"><span data-stu-id="e1d87-186">In the search box, type &quot;Microsoft.AspNet.WebApi.Client&quot;.</span></span>
- <span data-ttu-id="e1d87-187">Microsoft ASP.NET Web API Client Libraries パッケージを選択し、をクリックして**インストール**です。</span><span class="sxs-lookup"><span data-stu-id="e1d87-187">Select the Microsoft ASP.NET Web API Client Libraries package and click **Install**.</span></span>

<span data-ttu-id="e1d87-188">ClientApp で自己ホスト型プロジェクトへの参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="e1d87-188">Add a reference in ClientApp to the SelfHost project:</span></span>

- <span data-ttu-id="e1d87-189">ソリューション エクスプ ローラーで、ClientApp プロジェクトを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="e1d87-189">In Solution Explorer, right-click the ClientApp project.</span></span>
- <span data-ttu-id="e1d87-190">選択**参照の追加**です。</span><span class="sxs-lookup"><span data-stu-id="e1d87-190">Select **Add Reference**.</span></span>
- <span data-ttu-id="e1d87-191">**参照マネージャー** ] ダイアログで、**ソリューション**[**プロジェクト**です。</span><span class="sxs-lookup"><span data-stu-id="e1d87-191">In the **Reference Manager** dialog, under **Solution**, select **Projects**.</span></span>
- <span data-ttu-id="e1d87-192">自己ホスト型プロジェクトを選択します。</span><span class="sxs-lookup"><span data-stu-id="e1d87-192">Select the SelfHost project.</span></span>
- <span data-ttu-id="e1d87-193">**[OK]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e1d87-193">Click **OK**.</span></span>

![](self-host-a-web-api/_static/image6.png)

<span data-ttu-id="e1d87-194">Client/Program.cs ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="e1d87-194">Open the Client/Program.cs file.</span></span> <span data-ttu-id="e1d87-195">次の追加**を使用して**ステートメント。</span><span class="sxs-lookup"><span data-stu-id="e1d87-195">Add the following **using** statement:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

<span data-ttu-id="e1d87-196">静的なを追加**HttpClient**インスタンス。</span><span class="sxs-lookup"><span data-stu-id="e1d87-196">Add a static **HttpClient** instance:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

<span data-ttu-id="e1d87-197">リスト ID を使用して製品のすべての製品と製品の一覧をカテゴリ別一覧を次のメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="e1d87-197">Add the following methods to list all products, list a product by ID, and list products by category.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

<span data-ttu-id="e1d87-198">これらの各メソッドには、同様のパターンが次に示します。</span><span class="sxs-lookup"><span data-stu-id="e1d87-198">Each of these methods follows the same pattern:</span></span>

1. <span data-ttu-id="e1d87-199">呼び出す**HttpClient.GetAsync**適切な URI に GET 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="e1d87-199">Call **HttpClient.GetAsync** to send a GET request to the appropriate URI.</span></span>
2. <span data-ttu-id="e1d87-200">呼び出す**HttpResponseMessage.EnsureSuccessStatusCode**です。</span><span class="sxs-lookup"><span data-stu-id="e1d87-200">Call **HttpResponseMessage.EnsureSuccessStatusCode**.</span></span> <span data-ttu-id="e1d87-201">このメソッドは、HTTP 応答状態がエラー コードの場合、例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="e1d87-201">This method throws an exception if the HTTP response status is an error code.</span></span>
3. <span data-ttu-id="e1d87-202">呼び出す**ReadAsAsync&lt;T&gt;** を HTTP 応答から CLR 型を逆シリアル化します。</span><span class="sxs-lookup"><span data-stu-id="e1d87-202">Call **ReadAsAsync&lt;T&gt;** to deserialize a CLR type from the HTTP response.</span></span> <span data-ttu-id="e1d87-203">このメソッドで定義されている、拡張メソッドは、 **System.Net.Http.HttpContentExtensions**です。</span><span class="sxs-lookup"><span data-stu-id="e1d87-203">This method is an extension method, defined in **System.Net.Http.HttpContentExtensions**.</span></span>

<span data-ttu-id="e1d87-204">**されます**と**ReadAsAsync**メソッドは、非同期の両方です。</span><span class="sxs-lookup"><span data-stu-id="e1d87-204">The **GetAsync** and **ReadAsAsync** methods are both asynchronous.</span></span> <span data-ttu-id="e1d87-205">返される**タスク**を非同期操作を表すオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="e1d87-205">They return **Task** objects that represent the asynchronous operation.</span></span> <span data-ttu-id="e1d87-206">取得する、**結果**プロパティは、操作が完了するまでに、スレッドをブロックします。</span><span class="sxs-lookup"><span data-stu-id="e1d87-206">Getting the **Result** property blocks the thread until the operation completes.</span></span>

<span data-ttu-id="e1d87-207">詳細については、非ブロッキング呼び出しを作成する方法を含む、HttpClient を使用して次を参照してください。 [Web API から、.NET クライアントを呼び出す](../advanced/calling-a-web-api-from-a-net-client.md)です。</span><span class="sxs-lookup"><span data-stu-id="e1d87-207">For more information about using HttpClient, including how to make non-blocking calls, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="e1d87-208">これらのメソッドを呼び出す前に BaseAddress プロパティを設定する HttpClient インスタンス"`http://localhost:8080`"です。</span><span class="sxs-lookup"><span data-stu-id="e1d87-208">Before calling these methods, set the BaseAddress property on the HttpClient instance to "`http://localhost:8080`".</span></span> <span data-ttu-id="e1d87-209">例:</span><span class="sxs-lookup"><span data-stu-id="e1d87-209">For example:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

<span data-ttu-id="e1d87-210">これは、次を出力します。</span><span class="sxs-lookup"><span data-stu-id="e1d87-210">This should output the following.</span></span> <span data-ttu-id="e1d87-211">(最初に、自己ホスト型アプリケーションを実行してください。)</span><span class="sxs-lookup"><span data-stu-id="e1d87-211">(Remember to run the SelfHost application first.)</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
