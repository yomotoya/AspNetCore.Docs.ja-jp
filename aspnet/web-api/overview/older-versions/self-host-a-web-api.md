---
uid: web-api/overview/older-versions/self-host-a-web-api
title: セルフホスト ASP.NET Web API 1 (c#) |Microsoft Docs
author: MikeWasson
description: ASP.NET Web API では、IIS は必要ありません。 Web API は、独自のホスト プロセスで自己ホストできます。 このチュートリアルでは、アプリのコンソール内で web API をホストする方法を使用しています.
ms.author: aspnetcontent
ms.date: 01/26/2012
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: 50681dcd89dfed480cf343f753371af384fd3e68
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811738"
---
<a name="self-host-aspnet-web-api-1-c"></a><span data-ttu-id="1ea99-105">ASP.NET Web API 1 (c#) を自己ホストします。</span><span class="sxs-lookup"><span data-stu-id="1ea99-105">Self-Host ASP.NET Web API 1 (C#)</span></span>
====================
<span data-ttu-id="1ea99-106">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1ea99-106">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="1ea99-107">ASP.NET Web API では、IIS は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="1ea99-107">ASP.NET Web API does not require IIS.</span></span> <span data-ttu-id="1ea99-108">Web API は、独自のホスト プロセスで自己ホストできます。</span><span class="sxs-lookup"><span data-stu-id="1ea99-108">You can self-host a web API in your own host process.</span></span> <span data-ttu-id="1ea99-109">このチュートリアルでは、コンソール アプリケーション内部の web API をホストする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-109">This tutorial shows how to host a web API inside a console application.</span></span>
> 
> <span data-ttu-id="1ea99-110">**新しいアプリケーションは、OWIN を使用して、Web API を自己ホストする必要があります。**</span><span class="sxs-lookup"><span data-stu-id="1ea99-110">**New applications should use OWIN to self-host Web API.**</span></span> <span data-ttu-id="1ea99-111">参照してください[OWIN を使用して、ASP.NET Web API 2 をセルフホスト](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-111">See [Use OWIN to Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="1ea99-112">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="1ea99-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="1ea99-113">Web API 1</span><span class="sxs-lookup"><span data-stu-id="1ea99-113">Web API 1</span></span>
> - <span data-ttu-id="1ea99-114">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="1ea99-114">Visual Studio 2012</span></span>


## <a name="create-the-console-application-project"></a><span data-ttu-id="1ea99-115">コンソール アプリケーション プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-115">Create the Console Application Project</span></span>

<span data-ttu-id="1ea99-116">Visual Studio を起動し、選択**新しいプロジェクト**から、**開始**ページ。</span><span class="sxs-lookup"><span data-stu-id="1ea99-116">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="1ea99-117">またはから、**ファイル**メニューの **新規**し**プロジェクト**します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-117">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="1ea99-118">**テンプレート**ペインで、**インストールされたテンプレート**を展開し、 **Visual c#** ノード。</span><span class="sxs-lookup"><span data-stu-id="1ea99-118">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="1ea99-119">**Visual c#**、 **Windows**します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-119">Under **Visual C#**, select **Windows**.</span></span> <span data-ttu-id="1ea99-120">プロジェクト テンプレートの一覧で選択**コンソール アプリケーション**します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-120">In the list of project templates, select **Console Application**.</span></span> <span data-ttu-id="1ea99-121">プロジェクトに名前を&quot;SelfHost&quot;  をクリック**OK**します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-121">Name the project &quot;SelfHost&quot; and click **OK**.</span></span>

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a><span data-ttu-id="1ea99-122">ターゲット フレームワーク (Visual Studio 2010) に設定します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-122">Set the Target Framework (Visual Studio 2010)</span></span>

<span data-ttu-id="1ea99-123">Visual Studio 2010 を使用している場合は、.NET Framework 4.0 をターゲット フレームワークを変更します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-123">If you are using Visual Studio 2010, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="1ea99-124">(既定では、プロジェクト テンプレートの対象として、 [.Net Framework クライアント プロファイル](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile))。</span><span class="sxs-lookup"><span data-stu-id="1ea99-124">(By default, the project template targets the [.Net Framework Client Profile](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span></span>

<span data-ttu-id="1ea99-125">ソリューション エクスプ ローラーでプロジェクトを右クリックし、選択**プロパティ**します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-125">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="1ea99-126">**ターゲット フレームワーク**ドロップダウン リストで、ターゲット フレームワークを .NET Framework 4.0 に変更します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-126">In the **Target framework** dropdown list, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="1ea99-127">変更を適用するメッセージが表示されたら、クリックして**はい**します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-127">When prompted to apply the change, click **Yes**.</span></span>

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a><span data-ttu-id="1ea99-128">NuGet パッケージ マネージャーをインストールします。</span><span class="sxs-lookup"><span data-stu-id="1ea99-128">Install NuGet Package Manager</span></span>

<span data-ttu-id="1ea99-129">NuGet パッケージ マネージャーは、Web API アセンブリを非 ASP.NET プロジェクトに追加する最も簡単な方法です。</span><span class="sxs-lookup"><span data-stu-id="1ea99-129">The NuGet Package Manager is the easiest way to add the Web API assemblies to a non-ASP.NET project.</span></span>

<span data-ttu-id="1ea99-130">NuGet パッケージ マネージャーがインストールされていることを確認するには、クリックして、**ツール**Visual Studio のメニュー。</span><span class="sxs-lookup"><span data-stu-id="1ea99-130">To check if NuGet Package Manager is installed, click the **Tools** menu in Visual Studio.</span></span> <span data-ttu-id="1ea99-131">メニュー項目を表示する場合は**ライブラリ パッケージ マネージャー**、NuGet パッケージ マネージャーがあります。</span><span class="sxs-lookup"><span data-stu-id="1ea99-131">If you see a menu item called **Library Package Manager**, then you have NuGet Package Manager.</span></span>

<span data-ttu-id="1ea99-132">NuGet パッケージ マネージャーをインストールするには</span><span class="sxs-lookup"><span data-stu-id="1ea99-132">To install NuGet Package Manager:</span></span>

1. <span data-ttu-id="1ea99-133">Visual Studio を起動します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-133">Start Visual Studio.</span></span>
2. <span data-ttu-id="1ea99-134">**[ツール]** メニューから **[拡張機能と更新プログラム]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-134">From the **Tools** menu, select **Extensions and Updates**.</span></span>
3. <span data-ttu-id="1ea99-135">**拡張機能と更新**ダイアログ ボックスで、**オンライン**します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-135">In the **Extensions and Updates** dialog, select **Online**.</span></span>
4. <span data-ttu-id="1ea99-136">「NuGet パッケージ マネージャー」が表示されない場合は、検索ボックスに「nuget パッケージ マネージャー」を入力します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-136">If you don't see "NuGet Package Manager", type "nuget package manager" in the search box.</span></span>
5. <span data-ttu-id="1ea99-137">NuGet パッケージ マネージャーを選択し、クリックして**ダウンロード**します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-137">Select the NuGet Package Manager and click **Download**.</span></span>
6. <span data-ttu-id="1ea99-138">ダウンロードの完了後は、インストールを促されます。</span><span class="sxs-lookup"><span data-stu-id="1ea99-138">After the download completes, you will be prompted to install.</span></span>
7. <span data-ttu-id="1ea99-139">インストールの完了後は、Visual Studio を再起動する求め可能性があります。</span><span class="sxs-lookup"><span data-stu-id="1ea99-139">After the installation completes, you might be prompted to restart Visual Studio.</span></span>

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a><span data-ttu-id="1ea99-140">Web API の NuGet パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-140">Add the Web API NuGet Package</span></span>

<span data-ttu-id="1ea99-141">NuGet パッケージ マネージャーをインストールした後は、プロジェクトに Web API Self-Host パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-141">After NuGet Package Manager is installed, add the Web API Self-Host package to your project.</span></span>

1. <span data-ttu-id="1ea99-142">**ツール**メニューの **ライブラリ パッケージ マネージャー**します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-142">From the **Tools** menu, select **Library Package Manager**.</span></span> <span data-ttu-id="1ea99-143">*注*: かどうかは表示されないこのメニュー項目をその NuGet パッケージ マネージャーが正しくインストールされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-143">*Note*: If do you not see this menu item, make sure that NuGet Package Manager installed correctly.</span></span>
2. <span data-ttu-id="1ea99-144">選択**ソリューションの NuGet パッケージを管理しています.**</span><span class="sxs-lookup"><span data-stu-id="1ea99-144">Select **Manage NuGet Packages for Solution...**</span></span>
3. <span data-ttu-id="1ea99-145">**NugGet パッケージの管理**ダイアログ ボックスで、**オンライン**します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-145">In the **Manage NugGet Packages** dialog, select **Online**.</span></span>
4. <span data-ttu-id="1ea99-146">検索ボックスに「 &quot;Microsoft.AspNet.WebApi.SelfHost&quot;します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-146">In the search box, type &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span></span>
5. <span data-ttu-id="1ea99-147">ASP.NET Web API の自己ホスト パッケージを選択し、クリックして**インストール**します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-147">Select the ASP.NET Web API Self Host package and click **Install**.</span></span>
6. <span data-ttu-id="1ea99-148">パッケージをインストールした後にをクリックして**閉じます**ダイアログ ボックスを閉じます。</span><span class="sxs-lookup"><span data-stu-id="1ea99-148">After the package installs, click **Close** to close the dialog.</span></span>

> [!NOTE]
> <span data-ttu-id="1ea99-149">Microsoft.AspNet.WebApi.SelfHost、AspNetWebApi.SelfHost しないという名前のパッケージをインストールしてください。</span><span class="sxs-lookup"><span data-stu-id="1ea99-149">Make sure to install the package named Microsoft.AspNet.WebApi.SelfHost, not AspNetWebApi.SelfHost.</span></span>


![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="1ea99-150">モデルとコント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-150">Create the Model and Controller</span></span>

<span data-ttu-id="1ea99-151">このチュートリアルと同じモデルとコント ローラー クラスを使用して、 [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)チュートリアル。</span><span class="sxs-lookup"><span data-stu-id="1ea99-151">This tutorial uses the same model and controller classes as the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="1ea99-152">という名前のパブリック クラスを追加`Product`します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-152">Add a public class named `Product`.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

<span data-ttu-id="1ea99-153">という名前のパブリック クラスを追加`ProductsController`します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-153">Add a public class named `ProductsController`.</span></span> <span data-ttu-id="1ea99-154">このクラスから派生**System.Web.Http.ApiController**します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-154">Derive this class from **System.Web.Http.ApiController**.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

<span data-ttu-id="1ea99-155">このコント ローラー コードの詳細については、次を参照してください。、 [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)チュートリアル。</span><span class="sxs-lookup"><span data-stu-id="1ea99-155">For more information about the code in this controller, see the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span> <span data-ttu-id="1ea99-156">このコント ローラーには、次の 3 つの GET 操作を定義します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-156">This controller defines three GET actions:</span></span>

| <span data-ttu-id="1ea99-157">URI</span><span class="sxs-lookup"><span data-stu-id="1ea99-157">URI</span></span> | <span data-ttu-id="1ea99-158">説明</span><span class="sxs-lookup"><span data-stu-id="1ea99-158">Description</span></span> |
| --- | --- |
| <span data-ttu-id="1ea99-159">/api/products</span><span class="sxs-lookup"><span data-stu-id="1ea99-159">/api/products</span></span> | <span data-ttu-id="1ea99-160">すべての製品の一覧を取得します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-160">Get a list of all products.</span></span> |
| <span data-ttu-id="1ea99-161">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="1ea99-161">/api/products/*id*</span></span> | <span data-ttu-id="1ea99-162">ID によって製品を取得します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-162">Get a product by ID.</span></span> |
| <span data-ttu-id="1ea99-163">/api/products/?category=*category*</span><span class="sxs-lookup"><span data-stu-id="1ea99-163">/api/products/?category=*category*</span></span> | <span data-ttu-id="1ea99-164">カテゴリ別、製品の一覧を取得します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-164">Get a list of products by category.</span></span> |

## <a name="host-the-web-api"></a><span data-ttu-id="1ea99-165">Web API をホストします。</span><span class="sxs-lookup"><span data-stu-id="1ea99-165">Host the Web API</span></span>

<span data-ttu-id="1ea99-166">Program.cs ファイルを開き、次を追加するステートメントを使用します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-166">Open the file Program.cs and add the following using statements:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

<span data-ttu-id="1ea99-167">次のコードを追加、**プログラム**クラス。</span><span class="sxs-lookup"><span data-stu-id="1ea99-167">Add the following code to the **Program** class.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a><span data-ttu-id="1ea99-168">(省略可能)HTTP URL Namespace 予約を追加します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-168">(Optional) Add an HTTP URL Namespace Reservation</span></span>

<span data-ttu-id="1ea99-169">このアプリケーションがリッスン`http://localhost:8080/`します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-169">This application listens to `http://localhost:8080/`.</span></span> <span data-ttu-id="1ea99-170">既定では、特定の HTTP アドレスでリッスンしている管理者特権が必要です。</span><span class="sxs-lookup"><span data-stu-id="1ea99-170">By default, listening at a particular HTTP address requires administrator privileges.</span></span> <span data-ttu-id="1ea99-171">このチュートリアルを実行するときにそのため、エラーが発生この:"HTTP URL を登録できませんでしたhttp://+:8080/"はこのエラーを回避するために 2 つの方法があります。</span><span class="sxs-lookup"><span data-stu-id="1ea99-171">When you run the tutorial, therefore, you may get this error: "HTTP could not register URL http://+:8080/" There are two ways to avoid this error:</span></span>

- <span data-ttu-id="1ea99-172">Visual Studio を管理者として昇格されたアクセス許可を持つ実行または</span><span class="sxs-lookup"><span data-stu-id="1ea99-172">Run Visual Studio with elevated administrator permissions, or</span></span>
- <span data-ttu-id="1ea99-173">Netsh.exe を使用して、URL を予約するアカウントのアクセス許可を与えます。</span><span class="sxs-lookup"><span data-stu-id="1ea99-173">Use Netsh.exe to give your account permissions to reserve the URL.</span></span>

<span data-ttu-id="1ea99-174">Netsh.exe を使用して、管理者特権でコマンド プロンプトを開きし、次のコマンド: 次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-174">To use Netsh.exe, open a command prompt with administrator privileges and enter the following command:following command:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

<span data-ttu-id="1ea99-175">場所*machine \username など*は、ユーザー アカウントです。</span><span class="sxs-lookup"><span data-stu-id="1ea99-175">where *machine\username* is your user account.</span></span>

<span data-ttu-id="1ea99-176">自己ホストを終了したら、必ず、予約を削除します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-176">When you are finished self-hosting, be sure to delete the reservation:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a><span data-ttu-id="1ea99-177">クライアント アプリケーション (c#) から Web API の呼び出し</span><span class="sxs-lookup"><span data-stu-id="1ea99-177">Call the Web API from a Client Application (C#)</span></span>

<span data-ttu-id="1ea99-178">Web API を呼び出す単純なコンソール アプリケーションを記述してみましょう。</span><span class="sxs-lookup"><span data-stu-id="1ea99-178">Let's write a simple console application that calls the web API.</span></span>

<span data-ttu-id="1ea99-179">新しいコンソール アプリケーション プロジェクトをソリューションに追加します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-179">Add a new console application project to the solution:</span></span>

- <span data-ttu-id="1ea99-180">ソリューション エクスプ ローラーでソリューションを右クリックし、選択**新しいプロジェクトの追加**します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-180">In Solution Explorer, right-click the solution and select **Add New Project**.</span></span>
- <span data-ttu-id="1ea99-181">という名前の新しいコンソール アプリケーションを作成&quot;ClientApp&quot;します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-181">Create a new console application named &quot;ClientApp&quot;.</span></span>

![](self-host-a-web-api/_static/image5.png)

<span data-ttu-id="1ea99-182">ASP.NET Web API のコア ライブラリのパッケージを追加するには、NuGet パッケージ マネージャーを使用します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-182">Use NuGet Package Manager to add the ASP.NET Web API Core Libraries package:</span></span>

- <span data-ttu-id="1ea99-183">[ツール] メニューで、次のように選択します。**ライブラリ パッケージ マネージャー**します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-183">From the Tools menu, select **Library Package Manager**.</span></span>
- <span data-ttu-id="1ea99-184">選択**ソリューションの NuGet パッケージを管理しています.**</span><span class="sxs-lookup"><span data-stu-id="1ea99-184">Select **Manage NuGet Packages for Solution...**</span></span>
- <span data-ttu-id="1ea99-185">**NuGet パッケージの管理**ダイアログ ボックスで、**オンライン**します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-185">In the **Manage NuGet Packages** dialog, select **Online**.</span></span>
- <span data-ttu-id="1ea99-186">検索ボックスに「 &quot;Microsoft.AspNet.WebApi.Client&quot;します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-186">In the search box, type &quot;Microsoft.AspNet.WebApi.Client&quot;.</span></span>
- <span data-ttu-id="1ea99-187">Microsoft ASP.NET Web API クライアント ライブラリのパッケージを選択し、クリックして**インストール**します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-187">Select the Microsoft ASP.NET Web API Client Libraries package and click **Install**.</span></span>

<span data-ttu-id="1ea99-188">ClientApp で自己ホスト型プロジェクトへの参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-188">Add a reference in ClientApp to the SelfHost project:</span></span>

- <span data-ttu-id="1ea99-189">ソリューション エクスプ ローラーで、ClientApp プロジェクトを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="1ea99-189">In Solution Explorer, right-click the ClientApp project.</span></span>
- <span data-ttu-id="1ea99-190">**[参照の追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="1ea99-190">Select **Add Reference**.</span></span>
- <span data-ttu-id="1ea99-191">**参照マネージャー**ダイアログで、**ソリューション**を選択します**プロジェクト**します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-191">In the **Reference Manager** dialog, under **Solution**, select **Projects**.</span></span>
- <span data-ttu-id="1ea99-192">自己ホスト型プロジェクトを選択します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-192">Select the SelfHost project.</span></span>
- <span data-ttu-id="1ea99-193">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="1ea99-193">Click **OK**.</span></span>

![](self-host-a-web-api/_static/image6.png)

<span data-ttu-id="1ea99-194">Client/Program.cs ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="1ea99-194">Open the Client/Program.cs file.</span></span> <span data-ttu-id="1ea99-195">次の追加**を使用して**ステートメント。</span><span class="sxs-lookup"><span data-stu-id="1ea99-195">Add the following **using** statement:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

<span data-ttu-id="1ea99-196">追加の静的な**HttpClient**インスタンス。</span><span class="sxs-lookup"><span data-stu-id="1ea99-196">Add a static **HttpClient** instance:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

<span data-ttu-id="1ea99-197">カテゴリ別一覧、製品 ID を使用して、すべての製品と製品の一覧を一覧表示する次のメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-197">Add the following methods to list all products, list a product by ID, and list products by category.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

<span data-ttu-id="1ea99-198">これらの各メソッドには、同じパターンがとおりです。</span><span class="sxs-lookup"><span data-stu-id="1ea99-198">Each of these methods follows the same pattern:</span></span>

1. <span data-ttu-id="1ea99-199">呼び出す**HttpClient.GetAsync**適切な URI に GET 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-199">Call **HttpClient.GetAsync** to send a GET request to the appropriate URI.</span></span>
2. <span data-ttu-id="1ea99-200">呼び出す**HttpResponseMessage.EnsureSuccessStatusCode**します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-200">Call **HttpResponseMessage.EnsureSuccessStatusCode**.</span></span> <span data-ttu-id="1ea99-201">このメソッドは、HTTP 応答の状態がエラー コードの場合、例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="1ea99-201">This method throws an exception if the HTTP response status is an error code.</span></span>
3. <span data-ttu-id="1ea99-202">呼び出す**ReadAsAsync&lt;T&gt;** を HTTP 応答から CLR 型を逆シリアル化します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-202">Call **ReadAsAsync&lt;T&gt;** to deserialize a CLR type from the HTTP response.</span></span> <span data-ttu-id="1ea99-203">このメソッドで定義されている、拡張メソッドは、 **System.Net.Http.HttpContentExtensions**します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-203">This method is an extension method, defined in **System.Net.Http.HttpContentExtensions**.</span></span>

<span data-ttu-id="1ea99-204">**GetAsync**と**ReadAsAsync**メソッドは、非同期のどちらもします。</span><span class="sxs-lookup"><span data-stu-id="1ea99-204">The **GetAsync** and **ReadAsAsync** methods are both asynchronous.</span></span> <span data-ttu-id="1ea99-205">返される**タスク**非同期操作を表すオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="1ea99-205">They return **Task** objects that represent the asynchronous operation.</span></span> <span data-ttu-id="1ea99-206">取得、**結果**プロパティは、操作が完了するまでスレッドをブロックします。</span><span class="sxs-lookup"><span data-stu-id="1ea99-206">Getting the **Result** property blocks the thread until the operation completes.</span></span>

<span data-ttu-id="1ea99-207">非ブロッキング呼び出しを実行する方法など、HttpClient を使用しての詳細については、次を参照してください。 [Web API から、.NET クライアントを呼び出す](../advanced/calling-a-web-api-from-a-net-client.md)します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-207">For more information about using HttpClient, including how to make non-blocking calls, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="1ea99-208">これらのメソッドを呼び出す前に BaseAddress プロパティを設定する HttpClient インスタンス"`http://localhost:8080`"。</span><span class="sxs-lookup"><span data-stu-id="1ea99-208">Before calling these methods, set the BaseAddress property on the HttpClient instance to "`http://localhost:8080`".</span></span> <span data-ttu-id="1ea99-209">例えば:</span><span class="sxs-lookup"><span data-stu-id="1ea99-209">For example:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

<span data-ttu-id="1ea99-210">これは、次を出力します。</span><span class="sxs-lookup"><span data-stu-id="1ea99-210">This should output the following.</span></span> <span data-ttu-id="1ea99-211">(自己ホスト型アプリケーションを最初に実行してください。)</span><span class="sxs-lookup"><span data-stu-id="1ea99-211">(Remember to run the SelfHost application first.)</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
