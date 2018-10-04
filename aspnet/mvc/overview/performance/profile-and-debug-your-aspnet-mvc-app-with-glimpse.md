---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: プロファイルし、Glimpse による ASP.NET MVC アプリのデバッグ |Microsoft Docs
author: Rick-Anderson
description: Glimpse に関する情報は盛況と詳細なパフォーマンスを提供するオープン ソースの NuGet パッケージのファミリを増加して、デバッグおよび ASP.NET 用の診断情報をしています.
ms.author: riande
ms.date: 03/26/2015
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: 94a72f22cbcd7fa84528dde502cceaa1e26dcaa1
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577289"
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a><span data-ttu-id="8c812-103">プロファイルし、Glimpse による ASP.NET MVC アプリのデバッグ</span><span class="sxs-lookup"><span data-stu-id="8c812-103">Profile and debug your ASP.NET MVC app with Glimpse</span></span>
====================
<span data-ttu-id="8c812-104">によって[Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="8c812-104">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

> <span data-ttu-id="8c812-105">Glimpse に関する情報は盛況し詳細なパフォーマンスを提供するオープン ソースの NuGet パッケージのファミリを拡大し、デバッグ、および ASP.NET アプリの診断情報。</span><span class="sxs-lookup"><span data-stu-id="8c812-105">Glimpse is a thriving and growing family of open source NuGet packages that provides detailed performance, debugging and diagnostic information for ASP.NET apps.</span></span> <span data-ttu-id="8c812-106">インストールする単純な軽量で、超高速には、すべてのページの下部にある主要なパフォーマンス メトリックを表示します。</span><span class="sxs-lookup"><span data-stu-id="8c812-106">It's trivial to install, lightweight, ultra-fast, and displays key performance metrics at the bottom of every page.</span></span> <span data-ttu-id="8c812-107">サーバーで起こっていることを確認する必要がある場合、アプリにドリルダウンすることができます。</span><span class="sxs-lookup"><span data-stu-id="8c812-107">It allows you to drill down into your app when you need to find out what's going on at the server.</span></span> <span data-ttu-id="8c812-108">Glimpse に関する情報など、Azure テスト環境、開発サイクル全体で使用することをお勧めします。 非常に貴重な情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="8c812-108">Glimpse provides so much valuable information we recommend you use it throughout your development cycle, including your Azure test environment.</span></span> <span data-ttu-id="8c812-109">中に[Fiddler](http://www.telerik.com/fiddler)と[F-12 開発ツール](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx)クライアント側の提供ビュー、glimpse に関する情報がサーバーから詳細ビューを提供します。</span><span class="sxs-lookup"><span data-stu-id="8c812-109">While [Fiddler](http://www.telerik.com/fiddler) and the [F-12 development tools](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) provide a client side view, Glimpse provides a detailed view from the server.</span></span> <span data-ttu-id="8c812-110">Glimpse ASP.NET MVC と EF のパッケージを使用してこのチュートリアルを取り上げますが、その他の多くのパッケージを使用できます。</span><span class="sxs-lookup"><span data-stu-id="8c812-110">This tutorial will focus on using the Glimpse ASP.NET MVC and EF packages, but many other packages are available.</span></span> <span data-ttu-id="8c812-111">適切なリンクが可能な限り[Glimpse の docs](http://getglimpse.com/Docs/)を維持するためです。</span><span class="sxs-lookup"><span data-stu-id="8c812-111">Where possible I will link to the appropriate [Glimpse docs](http://getglimpse.com/Docs/) which I help maintain.</span></span> <span data-ttu-id="8c812-112">Glimpse に関する情報は、オープン ソース プロジェクト、ソース コードとドキュメントにも寄与できます。</span><span class="sxs-lookup"><span data-stu-id="8c812-112">Glimpse is an open source project, you too can contribute to the source code and the docs.</span></span>


- [<span data-ttu-id="8c812-113">Glimpse に関する情報をインストールします。</span><span class="sxs-lookup"><span data-stu-id="8c812-113">Installing Glimpse</span></span>](#ig)
- [<span data-ttu-id="8c812-114">Localhost の glimpse に関する情報を有効にします。</span><span class="sxs-lookup"><span data-stu-id="8c812-114">Enable Glimpse for localhost</span></span>](#eg)
- <span data-ttu-id="8c812-115">[[タイムライン] タブ](#Time)</span><span class="sxs-lookup"><span data-stu-id="8c812-115">[The Timeline tab](#Time)</span></span>
- [<span data-ttu-id="8c812-116">モデル バインド</span><span class="sxs-lookup"><span data-stu-id="8c812-116">Model Binding</span></span>](#mb)
- [<span data-ttu-id="8c812-117">Routes</span><span class="sxs-lookup"><span data-stu-id="8c812-117">Routes</span></span>](#route)
- [<span data-ttu-id="8c812-118">Glimpse に関する情報を使用して Azure に</span><span class="sxs-lookup"><span data-stu-id="8c812-118">Using Glimpse on Azure</span></span>](#da)
- [<span data-ttu-id="8c812-119">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="8c812-119">Additional Resources</span></span>](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a><span data-ttu-id="8c812-120">Glimpse に関する情報をインストールします。</span><span class="sxs-lookup"><span data-stu-id="8c812-120">Installing Glimpse</span></span>

<span data-ttu-id="8c812-121">Glimpse に関する情報をインストールするには、NuGet パッケージ マネージャー コンソールから、またはから、 **NuGet パッケージの管理**コンソール。</span><span class="sxs-lookup"><span data-stu-id="8c812-121">You can install Glimpse from the NuGet package manager console or from the **Manage NuGet Packages** console.</span></span> <span data-ttu-id="8c812-122">このデモでは、Mvc5 と EF6 のパッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="8c812-122">For this demo, I'll install the Mvc5 and EF6 packages:</span></span>

![NuGet Dlg から glimpse に関する情報をインストールします。](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

<span data-ttu-id="8c812-124">検索*Glimpse.EF*</span><span class="sxs-lookup"><span data-stu-id="8c812-124">Search for *Glimpse.EF*</span></span>

![NuGet のインストール ダイアログから Glimpse.EF](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

<span data-ttu-id="8c812-126">選択して**パッケージがインストールされている**、インストールされている Glimpse 依存モジュールを確認できます。</span><span class="sxs-lookup"><span data-stu-id="8c812-126">By selecting **Installed packages**, you can see the Glimpse dependent modules installed:</span></span>

![メニューから Glimpse パッケージをインストールします。](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

<span data-ttu-id="8c812-128">次のコマンドは、パッケージ マネージャー コンソールから Glimpse MVC5 と EF6 のモジュールをインストールします。</span><span class="sxs-lookup"><span data-stu-id="8c812-128">The following commands install Glimpse MVC5 and EF6 modules from the package manager console:</span></span>

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a><span data-ttu-id="8c812-129">Localhost の glimpse に関する情報を有効にします。</span><span class="sxs-lookup"><span data-stu-id="8c812-129">Enable Glimpse for localhost</span></span>

<span data-ttu-id="8c812-130">移動します http://localhost:&lt; ポート番号&gt;/glimpse.axd し、 <strong>glimpse に関する情報をオンにする</strong>ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="8c812-130">Navigate to http://localhost:&lt;port #&gt;/glimpse.axd and click the <strong>Turn Glimpse On</strong> button.</span></span>

![Glimpse axd ページ](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

<span data-ttu-id="8c812-132">お気に入りバーの表示があれば、ドラッグ Glimpse ボタンをドロップし bookmarklets として追加およびできます。</span><span class="sxs-lookup"><span data-stu-id="8c812-132">If you have your favorites bar displayed, you can drag and drop the Glimpse buttons and add them as bookmarklets:</span></span>

![Glimpse boookmarklets で IE](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

<span data-ttu-id="8c812-134">アプリを移動できるようになりました、**ヘッドを表示**(HUD) は、ページの下部に表示されます。</span><span class="sxs-lookup"><span data-stu-id="8c812-134">You can now navigate your app, and the **Heads Up Display** (HUD) is shown at the bottom of the page.</span></span>

![HUD を連絡先マネージャー ページ](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

<span data-ttu-id="8c812-136">[Glimpse HUD ページ](http://getglimpse.com/Docs/Heads-up-Display)詳細の上に示したタイミング情報。</span><span class="sxs-lookup"><span data-stu-id="8c812-136">The [Glimpse HUD page](http://getglimpse.com/Docs/Heads-up-Display) details the timing information shown above.</span></span> <span data-ttu-id="8c812-137">控えめなパフォーマンスの HUD のデータが表示されますの通知も問題にすぐにテスト サイクルに到達する前にします。</span><span class="sxs-lookup"><span data-stu-id="8c812-137">The unobtrusive performance data the HUD displays can notify you of a problem immediately - before you get to the test cycle.</span></span> <span data-ttu-id="8c812-138">クリックすると、 &quot;g&quot;右下隅で Glimpse パネルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="8c812-138">Clicking on the &quot;g&quot; in the lower right corner brings up the Glimpse panel:</span></span>

![Glimpse パネル](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

<span data-ttu-id="8c812-140">上の図で、[実行タブ](http://getglimpse.com/Docs/Execution-Tab)が選択されているパイプラインは、フィルター、アクションのタイミングの詳細に表示します。</span><span class="sxs-lookup"><span data-stu-id="8c812-140">In the image above, the [Execution tab](http://getglimpse.com/Docs/Execution-Tab) is selected, which shows timing details of the actions and filters in the pipeline.</span></span> <span data-ttu-id="8c812-141">確認できます、[停止ウォッチ フィルター タイマー](http://www.nuget.org/packages/StopWatch/)パイプラインのステージ 6 から開始します。</span><span class="sxs-lookup"><span data-stu-id="8c812-141">You can see my [Stop Watch filter timer](http://www.nuget.org/packages/StopWatch/) start at stage 6 of the pipeline.</span></span> <span data-ttu-id="8c812-142">私の軽量のタイマーを確保できますで承認やビューの表示のすべての時間がないデータのプロファイル/タイミングに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="8c812-142">While my light weight timer can provide useful profile/timing data, it misses all the time spent in authorization and rendering the view.</span></span> <span data-ttu-id="8c812-143">については、タイマーを[プロファイルし、Azure に ASP.NET MVC アプリの時刻](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="8c812-143">You can read about my timer at [Profile and Time your ASP.NET MVC app all the way to Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span></span> <span data-ttu-id="8c812-144">[タブ](http://getglimpse.com/Docs/Tabs)ページは、各タブで詳細な情報へのリンクを提供します。</span><span class="sxs-lookup"><span data-stu-id="8c812-144">The [Tabs](http://getglimpse.com/Docs/Tabs) page provides links to detailed information on each tab.</span></span>

<a id="Time"></a>
## <a name="the-timeline-tab"></a><span data-ttu-id="8c812-145">[タイムライン] タブ</span><span class="sxs-lookup"><span data-stu-id="8c812-145">The Timeline tab</span></span>

<span data-ttu-id="8c812-146">Tom Dykstra の変更しました未処理[EF 6 と MVC 5 のチュートリアル](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)instructors コント ローラーを次のコードに変更します。</span><span class="sxs-lookup"><span data-stu-id="8c812-146">I've modified Tom Dykstra's outstanding [EF 6/MVC 5 tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) with the following code change to the instructors controller:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

<span data-ttu-id="8c812-147">上記のコードを渡すクエリ文字列にできます (`eager`) eager または明示的なコントロールにデータの読み込み。</span><span class="sxs-lookup"><span data-stu-id="8c812-147">The code above allows me to pass in query string (`eager`) to control eager or explicit loading of data.</span></span> <span data-ttu-id="8c812-148">明示的読み込みを使用する次の図でとタイミングのページに読み込まれた各登録が表示されます、`Index`アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="8c812-148">In the image below, explicit loading is used and the timing page shows each enrollment loaded in the `Index` action method:</span></span>

![明示的な読み込み](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

<span data-ttu-id="8c812-150">次のコードで eager が指定され、各登録のフェッチ後に、`Index`ビューが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="8c812-150">In the following code, eager is specified, and each enrollment is fetched after the `Index` view is called:</span></span>

![eager を指定します。](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

<span data-ttu-id="8c812-152">詳細なタイミング情報を取得する時間のセグメントを合わせることができます。</span><span class="sxs-lookup"><span data-stu-id="8c812-152">You can hover over a time segment to get detailed timing information:</span></span>

![自動的に詳細なタイミングを参照してください。](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a><span data-ttu-id="8c812-154">モデル バインド</span><span class="sxs-lookup"><span data-stu-id="8c812-154">Model Binding</span></span>

<span data-ttu-id="8c812-155">[モデル バインド タブ](http://getglimpse.com/Docs/Model-Binding-Tab)豊富なフォーム変数をバインドする方法と理由一部がバインドされていないの予想どおりの理解に役立つ情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="8c812-155">The [model binding tab](http://getglimpse.com/Docs/Model-Binding-Tab) provides a wealth of information to help you understand how your form variables are bound and why some are not bound as would expect.</span></span> <span data-ttu-id="8c812-156">次のイメージ、**でしょうか。**</span><span class="sxs-lookup"><span data-stu-id="8c812-156">The image below shows the **?**</span></span> <span data-ttu-id="8c812-157">アイコンをクリックすると glimpse に関する情報のヘルプ ページ機能を構成します。</span><span class="sxs-lookup"><span data-stu-id="8c812-157">icon, which you can click on to bring up the glimpse help page for that feature.</span></span>

![glimpse のモデル バインド ビュー](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a><span data-ttu-id="8c812-159">ルート</span><span class="sxs-lookup"><span data-stu-id="8c812-159">Routes</span></span>

 <span data-ttu-id="8c812-160">デバッグおよびルーティングを理解する Glimpse ルート タブが役立つことができます。</span><span class="sxs-lookup"><span data-stu-id="8c812-160">The Glimpse Routes tab will can help you debug and understand routing.</span></span> <span data-ttu-id="8c812-161">次の図で製品のルートが選択されている (および緑、Glimpse 規則に表示されます)。</span><span class="sxs-lookup"><span data-stu-id="8c812-161">In the image below, the product route is selected (and it shows in green, a Glimpse convention).</span></span> <span data-ttu-id="8c812-162">![選択した製品名](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png)ルート制約、領域およびデータのトークンも表示されます。</span><span class="sxs-lookup"><span data-stu-id="8c812-162">![product name selected](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) Route constraints, Areas and data tokens are also displayed.</span></span> <span data-ttu-id="8c812-163">参照してください[Glimpse ルート](http://getglimpse.com/Docs/Routes-Tab)と[属性は、ASP.NET MVC 5 でルーティング](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="8c812-163">See [Glimpse Routes](http://getglimpse.com/Docs/Routes-Tab) and [Attribute Routing in ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) for more information.</span></span> 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a><span data-ttu-id="8c812-164">Glimpse に関する情報を使用して Azure に</span><span class="sxs-lookup"><span data-stu-id="8c812-164">Using Glimpse on Azure</span></span>

<span data-ttu-id="8c812-165">Glimpse の既定のセキュリティ ポリシーは、ローカル ホストから表示される概要データのみを許可します。</span><span class="sxs-lookup"><span data-stu-id="8c812-165">The Glimpse default security policy only allows Glimpse data to be displayed from local host.</span></span> <span data-ttu-id="8c812-166">(Azure の web アプリ) などのリモート サーバーでこのデータを表示できるように、このセキュリティ ポリシーを変更できます。</span><span class="sxs-lookup"><span data-stu-id="8c812-166">You can change this security policy so you can view this data on a remote server (such as a web app on Azure).</span></span> <span data-ttu-id="8c812-167">Azure でのテスト環境、最大で一番下の強調表示されているマークの追加、 *web.confg* glimpse に関する情報を有効にするファイル。</span><span class="sxs-lookup"><span data-stu-id="8c812-167">For test environments on Azure, add the highlighted mark up to the bottom of the *web.confg* file to enable Glimpse:</span></span>

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

<span data-ttu-id="8c812-168">だけでこの変更により、任意のユーザーはリモート サイトに Glimpse データを表示できます。</span><span class="sxs-lookup"><span data-stu-id="8c812-168">With this change alone, any user can see your Glimpse data on a remote site.</span></span> <span data-ttu-id="8c812-169">のみ展開を適用するときにその発行プロファイル (たとえば、Azure テスト proifle。)、発行プロファイルに上記のマークアップを追加することを検討してください。Glimpse のデータを制限するのには追加、`canViewGlimpseData`ロールと glimpse に関する情報のデータを表示するには、このロールのユーザーのみを許可します。</span><span class="sxs-lookup"><span data-stu-id="8c812-169">Consider adding the markup above to a publish profile so it's only deployed an applyed when you use that publish profile (for example, your Azure test proifle.) To restrict Glimpse data, we will add the `canViewGlimpseData` role and only allow users in this role to view Glimpse data.</span></span>

<span data-ttu-id="8c812-170">コメントを削除、 *GlimpseSecurityPolicy.cs*ファイルし、変更、 [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx)から呼び出す`Administrator`を`canViewGlimpseData`ロール。</span><span class="sxs-lookup"><span data-stu-id="8c812-170">Remove the comments from the *GlimpseSecurityPolicy.cs* file and change the [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) call from `Administrator` to the `canViewGlimpseData` role:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> <span data-ttu-id="8c812-171">セキュリティ - glimpse に関する情報が提供する豊富なデータは、アプリのセキュリティを公開する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8c812-171">Security - The rich data provided by Glimpse could expose the security of your app.</span></span> <span data-ttu-id="8c812-172">Microsoft では、運用アプリで使用するため glimpse に関する情報のセキュリティ監査が実行されません。</span><span class="sxs-lookup"><span data-stu-id="8c812-172">Microsoft has not performed a security audit of Glimpse for use on productions apps.</span></span>


<span data-ttu-id="8c812-173">役割を追加する方法の詳細については、次を参照してください。 マイ[メンバーシップ、OAuth、SQL Database を使用した ASP.NET MVC 5 のセキュリティで保護された web アプリを Azure にデプロイ](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)チュートリアル。</span><span class="sxs-lookup"><span data-stu-id="8c812-173">For information on adding roles, see my [Deploy a Secure ASP.NET MVC 5 web app with Membership, OAuth, and SQL Database to Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) tutorial.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="8c812-174">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="8c812-174">Additional Resources</span></span>

- [<span data-ttu-id="8c812-175">メンバーシップ、OAuth、SQL Database を使用した安全な ASP.NET MVC 5 アプリを Azure にデプロイします。</span><span class="sxs-lookup"><span data-stu-id="8c812-175">Deploy a Secure ASP.NET MVC 5 app with Membership, OAuth, and SQL Database to Azure</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- <span data-ttu-id="8c812-176">[Glimpse の構成](http://getglimpse.com/Docs/Configuration)-ドキュメント ページのタブ、実行時ポリシー、ログ記録を構成します。</span><span class="sxs-lookup"><span data-stu-id="8c812-176">[Glimpse Configuration](http://getglimpse.com/Docs/Configuration) - Doc page on configuring tabs, runtime policy, logging and more.</span></span>
