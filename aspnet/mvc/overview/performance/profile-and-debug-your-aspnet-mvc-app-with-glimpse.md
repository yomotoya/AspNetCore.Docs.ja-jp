---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: "プロファイルし、Glimpse による ASP.NET MVC アプリのデバッグ |Microsoft ドキュメント"
author: Rick-Anderson
description: "Glimpse は、成功し、詳細なパフォーマンスを提供する NuGet パッケージのオープン ソースのファミリを拡大して、デバッグ、および ASP.NET の診断情報を."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: 9cfdced21251b482ca527dda9c3a698de77cc8ca
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a><span data-ttu-id="4339a-103">プロファイルし、Glimpse による ASP.NET MVC アプリのデバッグ</span><span class="sxs-lookup"><span data-stu-id="4339a-103">Profile and debug your ASP.NET MVC app with Glimpse</span></span>
====================
<span data-ttu-id="4339a-104">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="4339a-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="4339a-105">Glimpse は、成功し、詳細なパフォーマンスを提供する NuGet パッケージのオープン ソースのファミリを拡大して、デバッグ、および ASP.NET アプリ用の診断情報。</span><span class="sxs-lookup"><span data-stu-id="4339a-105">Glimpse is a thriving and growing family of open source NuGet packages that provides detailed performance, debugging and diagnostic information for ASP.NET apps.</span></span> <span data-ttu-id="4339a-106">インストールする自明の軽量の超高速と、すべてのページの下部にある主要なパフォーマンス メトリックを表示します。</span><span class="sxs-lookup"><span data-stu-id="4339a-106">It's trivial to install, lightweight, ultra-fast, and displays key performance metrics at the bottom of every page.</span></span> <span data-ttu-id="4339a-107">サーバーで何が起こってを確認する必要がある場合、アプリにドリル ダウンすることができます。</span><span class="sxs-lookup"><span data-stu-id="4339a-107">It allows you to drill down into your app when you need to find out what's going on at the server.</span></span> <span data-ttu-id="4339a-108">Glimpse は、Azure のテスト環境など、開発サイクル全体で使用することをお勧めほどの貴重な情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="4339a-108">Glimpse provides so much valuable information we recommend you use it throughout your development cycle, including your Azure test environment.</span></span> <span data-ttu-id="4339a-109">中に[Fiddler](http://www.telerik.com/fiddler)と[F-12 開発ツール](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx)クライアント側の提供ビュー、Glimpse がサーバーから詳細ビューを提供します。</span><span class="sxs-lookup"><span data-stu-id="4339a-109">While [Fiddler](http://www.telerik.com/fiddler) and the [F-12 development tools](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) provide a client side view, Glimpse provides a detailed view from the server.</span></span> <span data-ttu-id="4339a-110">Glimpse ASP.NET MVC と EF パッケージを使用してではこのチュートリアルに焦点が、その他の多くのパッケージを利用できます。</span><span class="sxs-lookup"><span data-stu-id="4339a-110">This tutorial will focus on using the Glimpse ASP.NET MVC and EF packages, but many other packages are available.</span></span> <span data-ttu-id="4339a-111">適切なリンクが可能な限り[皮下 docs](http://getglimpse.com/Docs/)のためを維持します。</span><span class="sxs-lookup"><span data-stu-id="4339a-111">Where possible I will link to the appropriate [Glimpse docs](http://getglimpse.com/Docs/) which I help maintain.</span></span> <span data-ttu-id="4339a-112">Glimpse はオープン ソース プロジェクトは、ソース コードとドキュメントにも寄与できます。</span><span class="sxs-lookup"><span data-stu-id="4339a-112">Glimpse is an open source project, you too can contribute to the source code and the docs.</span></span>


- [<span data-ttu-id="4339a-113">Glimpse のインストール</span><span class="sxs-lookup"><span data-stu-id="4339a-113">Installing Glimpse</span></span>](#ig)
- [<span data-ttu-id="4339a-114">Localhost の流れを有効にします。</span><span class="sxs-lookup"><span data-stu-id="4339a-114">Enable Glimpse for localhost</span></span>](#eg)
- <span data-ttu-id="4339a-115">[[タイムライン] タブ](#Time)</span><span class="sxs-lookup"><span data-stu-id="4339a-115">[The Timeline tab](#Time)</span></span>
- [<span data-ttu-id="4339a-116">モデル バインド</span><span class="sxs-lookup"><span data-stu-id="4339a-116">Model Binding</span></span>](#mb)
- [<span data-ttu-id="4339a-117">ルート</span><span class="sxs-lookup"><span data-stu-id="4339a-117">Routes</span></span>](#route)
- [<span data-ttu-id="4339a-118">Azure で Glimpse の使用</span><span class="sxs-lookup"><span data-stu-id="4339a-118">Using Glimpse on Azure</span></span>](#da)
- [<span data-ttu-id="4339a-119">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="4339a-119">Additional Resources</span></span>](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a><span data-ttu-id="4339a-120">Glimpse のインストール</span><span class="sxs-lookup"><span data-stu-id="4339a-120">Installing Glimpse</span></span>

<span data-ttu-id="4339a-121">Glimpse をインストールするには、NuGet パッケージ マネージャー コンソールから、またはから、 **NuGet パッケージの管理**コンソールです。</span><span class="sxs-lookup"><span data-stu-id="4339a-121">You can install Glimpse from the NuGet package manager console or from the **Manage NuGet Packages** console.</span></span> <span data-ttu-id="4339a-122">このデモでは、Mvc5 と EF6 パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="4339a-122">For this demo, I'll install the Mvc5 and EF6 packages:</span></span>

![NuGet の追加 ダイアログから Glimpse をインストールします。](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

<span data-ttu-id="4339a-124">検索*Glimpse.EF*</span><span class="sxs-lookup"><span data-stu-id="4339a-124">Search for *Glimpse.EF*</span></span>

![NuGet のインストールの追加 ダイアログから Glimpse.EF](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

<span data-ttu-id="4339a-126">選択して**インストールされているパッケージ**、インストールされている Glimpse 依存するモジュールを確認できます。</span><span class="sxs-lookup"><span data-stu-id="4339a-126">By selecting **Installed packages**, you can see the Glimpse dependent modules installed:</span></span>

![追加 ダイアログから Glimpse パッケージをインストール](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

<span data-ttu-id="4339a-128">次のコマンドは、パッケージ マネージャー コンソールから Glimpse MVC5 および EF6 モジュールをインストールします。</span><span class="sxs-lookup"><span data-stu-id="4339a-128">The following commands install Glimpse MVC5 and EF6 modules from the package manager console:</span></span>

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a><span data-ttu-id="4339a-129">Localhost の流れを有効にします。</span><span class="sxs-lookup"><span data-stu-id="4339a-129">Enable Glimpse for localhost</span></span>

<span data-ttu-id="4339a-130">Navigate to http://localhost:&lt;port #&gt;/glimpse.axd and click the **Turn Glimpse On** button.</span><span class="sxs-lookup"><span data-stu-id="4339a-130">Navigate to http://localhost:&lt;port #&gt;/glimpse.axd and click the **Turn Glimpse On** button.</span></span>

![Glimpse axd ページ](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

<span data-ttu-id="4339a-132">お気に入りバーの表示があれば、ドラッグ Glimpse ボタンのドロップし bookmarklets として追加およびできます。</span><span class="sxs-lookup"><span data-stu-id="4339a-132">If you have your favorites bar displayed, you can drag and drop the Glimpse buttons and add them as bookmarklets:</span></span>

![Glimpse boookmarklets で IE](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

<span data-ttu-id="4339a-134">今すぐアプリを移動することができます、**ヘッドを表示**(HUD) は、ページの下部に表示します。</span><span class="sxs-lookup"><span data-stu-id="4339a-134">You can now navigate your app, and the **Heads Up Display** (HUD) is shown at the bottom of the page.</span></span>

![HUD に連絡先のマネージャー ページ](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

<span data-ttu-id="4339a-136">[Glimpse HUD ページ](http://getglimpse.com/Docs/Heads-up-Display)上に示したタイミング情報を詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="4339a-136">The [Glimpse HUD page](http://getglimpse.com/Docs/Heads-up-Display) details the timing information shown above.</span></span> <span data-ttu-id="4339a-137">控えめなパフォーマンス データ HUD 表示に通知できます問題を直ちに - テスト サイクルを始める前に、。</span><span class="sxs-lookup"><span data-stu-id="4339a-137">The unobtrusive performance data the HUD displays can notify you of a problem immediately - before you get to the test cycle.</span></span> <span data-ttu-id="4339a-138">クリックすると、 &quot;g&quot;右下隅で Glimpse パネルを表示します。</span><span class="sxs-lookup"><span data-stu-id="4339a-138">Clicking on the &quot;g&quot; in the lower right corner brings up the Glimpse panel:</span></span>

![Glimpse パネル](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

<span data-ttu-id="4339a-140">、上記の図に、[実行 タブに](http://getglimpse.com/Docs/Execution-Tab)を選択すると、パイプライン内のアクションとフィルターのタイミングの詳細が示されます。</span><span class="sxs-lookup"><span data-stu-id="4339a-140">In the image above, the [Execution tab](http://getglimpse.com/Docs/Execution-Tab) is selected, which shows timing details of the actions and filters in the pipeline.</span></span> <span data-ttu-id="4339a-141">確認できます my[停止ウォッチ フィルター タイマー](http://www.nuget.org/packages/StopWatch/)パイプラインのステージ 6 から開始します。</span><span class="sxs-lookup"><span data-stu-id="4339a-141">You can see my [Stop Watch filter timer](http://www.nuget.org/packages/StopWatch/) start at stage 6 of the pipeline.</span></span> <span data-ttu-id="4339a-142">My 軽量タイマーを提供できるプロファイル/タイミング データを便利に、認証やビューの表示のすべての時間はすべてミスします。</span><span class="sxs-lookup"><span data-stu-id="4339a-142">While my light weight timer can provide useful profile/timing data, it misses all the time spent in authorization and rendering the view.</span></span> <span data-ttu-id="4339a-143">自分のタイマーを知っておくことができます[をプロファイルし、Azure に ASP.NET MVC アプリの時間](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="4339a-143">You can read about my timer at [Profile and Time your ASP.NET MVC app all the way to Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span></span> <span data-ttu-id="4339a-144">[タブ](http://getglimpse.com/Docs/Tabs)ページは、各タブで詳細情報へのリンクを提供します。</span><span class="sxs-lookup"><span data-stu-id="4339a-144">The [Tabs](http://getglimpse.com/Docs/Tabs) page provides links to detailed information on each tab.</span></span>

<a id="Time"></a>
## <a name="the-timeline-tab"></a><span data-ttu-id="4339a-145">[タイムライン] タブ</span><span class="sxs-lookup"><span data-stu-id="4339a-145">The Timeline tab</span></span>

<span data-ttu-id="4339a-146">Tom Dykstra を変更しました未処理[EF 6/MVC 5 のチュートリアル](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)講習においてインストラクター コント ローラーを次のコードに変更します。</span><span class="sxs-lookup"><span data-stu-id="4339a-146">I've modified Tom Dykstra's outstanding [EF 6/MVC 5 tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) with the following code change to the instructors controller:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

<span data-ttu-id="4339a-147">上記のコードを渡すクエリ文字列に使用する (`eager`) データの読み込み eager または明示的なを制御します。</span><span class="sxs-lookup"><span data-stu-id="4339a-147">The code above allows me to pass in query string (`eager`) to control eager or explicit loading of data.</span></span> <span data-ttu-id="4339a-148">次の図では明示的な読み込みを使用し、タイミング ページに読み込まれている各登録を示しています、`Index`アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="4339a-148">In the image below, explicit loading is used and the timing page shows each enrollment loaded in the `Index` action method:</span></span>

![明示的な読み込み](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

<span data-ttu-id="4339a-150">次のコードで eager が指定され、各登録が後にフェッチ、`Index`ビューが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="4339a-150">In the following code, eager is specified, and each enrollment is fetched after the `Index` view is called:</span></span>

![eager を指定します。](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

<span data-ttu-id="4339a-152">詳細なタイミング情報を取得する時間のセグメントを合わせることができます。</span><span class="sxs-lookup"><span data-stu-id="4339a-152">You can hover over a time segment to get detailed timing information:</span></span>

![ホバー時に詳細なタイミングを参照してください。](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a><span data-ttu-id="4339a-154">モデル バインディング</span><span class="sxs-lookup"><span data-stu-id="4339a-154">Model Binding</span></span>

<span data-ttu-id="4339a-155">[モデル バインド タブ](http://getglimpse.com/Docs/Model-Binding-Tab)豊富なフォーム変数のバインド方法と理由一部がバインドされていないはずの理解に役立つ情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="4339a-155">The [model binding tab](http://getglimpse.com/Docs/Model-Binding-Tab) provides a wealth of information to help you understand how your form variables are bound and why some are not bound as would expect.</span></span> <span data-ttu-id="4339a-156">次のイメージ、**しますか?**</span><span class="sxs-lookup"><span data-stu-id="4339a-156">The image below shows the **?**</span></span> <span data-ttu-id="4339a-157">アイコンをクリックすると、glimpse のヘルプ ページで、その機能を表示します。</span><span class="sxs-lookup"><span data-stu-id="4339a-157">icon, which you can click on to bring up the glimpse help page for that feature.</span></span>

![glimpse モデルのビューをバインド](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a><span data-ttu-id="4339a-159">ルート</span><span class="sxs-lookup"><span data-stu-id="4339a-159">Routes</span></span>

 <span data-ttu-id="4339a-160">デバッグおよびルーティングを理解する Glimpse ルート タブが役立つことができます。</span><span class="sxs-lookup"><span data-stu-id="4339a-160">The Glimpse Routes tab will can help you debug and understand routing.</span></span> <span data-ttu-id="4339a-161">次の図で製品のルートが選択されている (およびそれ緑、Glimpse 規約で表示)。</span><span class="sxs-lookup"><span data-stu-id="4339a-161">In the image below, the product route is selected (and it shows in green, a Glimpse convention).</span></span> <span data-ttu-id="4339a-162">![選択した製品名](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png)ルート制約、領域およびデータのトークンも表示されます。</span><span class="sxs-lookup"><span data-stu-id="4339a-162">![product name selected](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) Route constraints, Areas and data tokens are also displayed.</span></span> <span data-ttu-id="4339a-163">参照してください[Glimpse ルート](http://getglimpse.com/Docs/Routes-Tab)と[属性は、ASP.NET MVC 5 でルーティング](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="4339a-163">See [Glimpse Routes](http://getglimpse.com/Docs/Routes-Tab) and [Attribute Routing in ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) for more information.</span></span> 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a><span data-ttu-id="4339a-164">Azure で Glimpse の使用</span><span class="sxs-lookup"><span data-stu-id="4339a-164">Using Glimpse on Azure</span></span>

<span data-ttu-id="4339a-165">Glimpse の既定のセキュリティ ポリシーは、ローカル ホストから表示される Glimpse データだけを許可します。</span><span class="sxs-lookup"><span data-stu-id="4339a-165">The Glimpse default security policy only allows Glimpse data to be displayed from local host.</span></span> <span data-ttu-id="4339a-166">(Azure の web アプリ) などのリモート サーバーでこのデータを表示できるように、このセキュリティ ポリシーを変更できます。</span><span class="sxs-lookup"><span data-stu-id="4339a-166">You can change this security policy so you can view this data on a remote server (such as a web app on Azure).</span></span> <span data-ttu-id="4339a-167">Azure でのテスト環境、追加の下部にある最大の強調表示されている、マーク、 *web.confg* Glimpse を有効にするファイル。</span><span class="sxs-lookup"><span data-stu-id="4339a-167">For test environments on Azure, add the highlighted mark up to the bottom of the *web.confg* file to enable Glimpse:</span></span>

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

<span data-ttu-id="4339a-168">この変更によりだけで、すべてのユーザーは、リモート サイトに Glimpse データを表示できます。</span><span class="sxs-lookup"><span data-stu-id="4339a-168">With this change alone, any user can see your Glimpse data on a remote site.</span></span> <span data-ttu-id="4339a-169">のみ展開、適用されている発行プロファイル (など、Azure テスト proifle。) を使用する場合は、発行プロファイルを上マークアップを追加します。Glimpse データを制限するのには追加、`canViewGlimpseData`ロール Glimpse データを表示するには、このロールのユーザーのみを許可するとします。</span><span class="sxs-lookup"><span data-stu-id="4339a-169">Consider adding the markup above to a publish profile so it's only deployed an applyed when you use that publish profile (for example, your Azure test proifle.) To restrict Glimpse data, we will add the `canViewGlimpseData` role and only allow users in this role to view Glimpse data.</span></span>

<span data-ttu-id="4339a-170">コメントを削除、 *GlimpseSecurityPolicy.cs*ファイルし、変更、 [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx)から呼び出す`Administrator`を`canViewGlimpseData`ロール。</span><span class="sxs-lookup"><span data-stu-id="4339a-170">Remove the comments from the *GlimpseSecurityPolicy.cs* file and change the [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) call from `Administrator` to the `canViewGlimpseData` role:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> <span data-ttu-id="4339a-171">セキュリティ - Glimpse によって提供される豊富なデータには、アプリのセキュリティでした公開します。</span><span class="sxs-lookup"><span data-stu-id="4339a-171">Security - The rich data provided by Glimpse could expose the security of your app.</span></span> <span data-ttu-id="4339a-172">Microsoft では、運用アプリで使用する Glimpse のセキュリティ監査が実行されません。</span><span class="sxs-lookup"><span data-stu-id="4339a-172">Microsoft has not performed a security audit of Glimpse for use on productions apps.</span></span>


<span data-ttu-id="4339a-173">ロールを追加する方法については、次を参照してください。 [メンバーシップ、OAuth、SQL データベースで ASP.NET MVC 5 のセキュリティで保護された web アプリケーションを Azure にデプロイ](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)チュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="4339a-173">For information on adding roles, see my [Deploy a Secure ASP.NET MVC 5 web app with Membership, OAuth, and SQL Database to Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) tutorial.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="4339a-174">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="4339a-174">Additional Resources</span></span>

- [<span data-ttu-id="4339a-175">Azure へのメンバーシップ、OAuth、および SQL データベースでのセキュリティで保護された ASP.NET MVC 5 アプリを配置します。</span><span class="sxs-lookup"><span data-stu-id="4339a-175">Deploy a Secure ASP.NET MVC 5 app with Membership, OAuth, and SQL Database to Azure</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- <span data-ttu-id="4339a-176">[構成を皮下](http://getglimpse.com/Docs/Configuration)-ドキュメント ページ タブ、実行時ポリシー、ログ記録の構成にします。</span><span class="sxs-lookup"><span data-stu-id="4339a-176">[Glimpse Configuration](http://getglimpse.com/Docs/Configuration) - Doc page on configuring tabs, runtime policy, logging and more.</span></span>
