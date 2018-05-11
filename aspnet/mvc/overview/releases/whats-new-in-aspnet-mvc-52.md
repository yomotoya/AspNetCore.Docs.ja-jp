---
uid: mvc/overview/releases/whats-new-in-aspnet-mvc-52
title: ASP.NET MVC 5.2 の新機能 |Microsoft ドキュメント
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/25/2014
ms.topic: article
ms.assetid: 97972587-2720-48b4-b158-f35f2e855fbf
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/releases/whats-new-in-aspnet-mvc-52
msc.type: authoredcontent
ms.openlocfilehash: 94f6131fdb86d1694c1f563c5f6806f119c71266
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-mvc-52"></a><span data-ttu-id="12609-102">ASP.NET MVC 5.2 の新機能</span><span class="sxs-lookup"><span data-stu-id="12609-102">What’s New in ASP.NET MVC 5.2</span></span>
====================
<span data-ttu-id="12609-103">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="12609-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="12609-104">このトピックでは、ASP.NET MVC 5.2 の新機能について説明[Microsoft.AspNet.MVC 5.2.2](#52)と[ASP.NET MVC 5.2.3 Beta](#mvc523Beta)</span><span class="sxs-lookup"><span data-stu-id="12609-104">This topic describes what's new for ASP.NET MVC 5.2, [Microsoft.AspNet.MVC 5.2.2](#52) and [ASP.NET MVC 5.2.3 Beta](#mvc523Beta)</span></span>

- [<span data-ttu-id="12609-105">ソフトウェアの要件</span><span class="sxs-lookup"><span data-stu-id="12609-105">Software Requirements</span></span>](#softRequire)
- [<span data-ttu-id="12609-106">ダウンロード</span><span class="sxs-lookup"><span data-stu-id="12609-106">Download</span></span>](#download)
- [<span data-ttu-id="12609-107">ドキュメント</span><span class="sxs-lookup"><span data-stu-id="12609-107">Documentation</span></span>](#documentation)
- [<span data-ttu-id="12609-108">ASP.NET MVC 5.2 の新機能</span><span class="sxs-lookup"><span data-stu-id="12609-108">New Features in ASP.NET MVC 5.2</span></span>](#new-features)

    - [<span data-ttu-id="12609-109">属性のルーティングの機能強化</span><span class="sxs-lookup"><span data-stu-id="12609-109">Attribute routing improvements</span></span>](#attributerouting)
- [<span data-ttu-id="12609-110">既知の問題と重大な変更</span><span class="sxs-lookup"><span data-stu-id="12609-110">Known Issues and Breaking Changes</span></span>](#knownbreakingchanges)
- [<span data-ttu-id="12609-111">バグの修正</span><span class="sxs-lookup"><span data-stu-id="12609-111">Bug Fixes</span></span>](#bug-fixes)
- [<span data-ttu-id="12609-112">Microsoft.AspNet.MVC 5.2.2</span><span class="sxs-lookup"><span data-stu-id="12609-112">Microsoft.AspNet.MVC 5.2.2</span></span>](#52)

<a id="softRequire"></a>
## <a name="software-requirements"></a><span data-ttu-id="12609-113">ソフトウェア要件</span><span class="sxs-lookup"><span data-stu-id="12609-113">Software Requirements</span></span>

- <span data-ttu-id="12609-114">Visual Studio 2012: ダウンロード[ASP.NET および Web Tools for Visual Studio 2012 2013.1](https://go.microsoft.com/fwlink/?LinkId=390062)です。</span><span class="sxs-lookup"><span data-stu-id="12609-114">Visual Studio 2012: Download [ASP.NET and Web Tools 2013.1 for Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).</span></span>
- <span data-ttu-id="12609-115">Visual Studio 2013 の場合: ダウンロード[Visual Studio 2013 Update](https://go.microsoft.com/fwlink/?LinkId=390064)またはそれ以降。</span><span class="sxs-lookup"><span data-stu-id="12609-115">Visual Studio 2013: Download [Visual Studio 2013 Update](https://go.microsoft.com/fwlink/?LinkId=390064) or higher.</span></span> <span data-ttu-id="12609-116">ASP.NET MVC 5.2 Razor ビューを編集するため、この更新プログラムが必要です。</span><span class="sxs-lookup"><span data-stu-id="12609-116">This update is needed for editing ASP.NET MVC 5.2 Razor Views.</span></span>

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="12609-117">ダウンロード</span><span class="sxs-lookup"><span data-stu-id="12609-117">Download</span></span>

<span data-ttu-id="12609-118">ランタイムの機能は、NuGet ギャラリー上の NuGet パッケージとしてリリースされています。</span><span class="sxs-lookup"><span data-stu-id="12609-118">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="12609-119">次のすべてのランタイム パッケージ、[セマンティック バージョニング](http://semver.org/)仕様です。</span><span class="sxs-lookup"><span data-stu-id="12609-119">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="12609-120">ASP.NET MVC 5.2 の最新のパッケージは、次のバージョン:「5.2.0」です。</span><span class="sxs-lookup"><span data-stu-id="12609-120">The latest ASP.NET MVC 5.2 package has the following version: "5.2.0".</span></span> <span data-ttu-id="12609-121">インストールまたはを介してこれらのパッケージを更新することができます[NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/)です。</span><span class="sxs-lookup"><span data-stu-id="12609-121">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/).</span></span> <span data-ttu-id="12609-122">リリースには、NuGet で対応するローカライズ版パッケージも含まれています。</span><span class="sxs-lookup"><span data-stu-id="12609-122">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="12609-123">インストールまたは NuGet パッケージ マネージャー コンソールを使用してリリースされた NuGet パッケージを更新できます。</span><span class="sxs-lookup"><span data-stu-id="12609-123">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

<span data-ttu-id="12609-124">インストール パッケージ Microsoft.AspNet.Mvc-バージョン 5.2.0</span><span class="sxs-lookup"><span data-stu-id="12609-124">Install-Package Microsoft.AspNet.Mvc -Version 5.2.0</span></span>

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="12609-125">ドキュメント</span><span class="sxs-lookup"><span data-stu-id="12609-125">Documentation</span></span>

<span data-ttu-id="12609-126">チュートリアルと ASP.NET MVC 5.2 に関する他の情報は、ASP.NET web サイトから使用可能な ([https://www.asp.net/mvc](../../index.md))。</span><span class="sxs-lookup"><span data-stu-id="12609-126">Tutorials and other information about ASP.NET MVC 5.2 are available from the ASP.NET web site ([https://www.asp.net/mvc](../../index.md)).</span></span>

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-52"></a><span data-ttu-id="12609-127">ASP.NET MVC 5.2 の新機能</span><span class="sxs-lookup"><span data-stu-id="12609-127">New Features in ASP.NET MVC 5.2</span></span>

<a id="attributerouting"></a>
### <a name="attribute-routing-improvements"></a><span data-ttu-id="12609-128">属性のルーティングの機能強化</span><span class="sxs-lookup"><span data-stu-id="12609-128">Attribute routing improvements</span></span>

<span data-ttu-id="12609-129">属性のようになりましたルーティング属性ルートを検出および構成する方法を完全に制御できる IDirectRouteProvider と呼ばれる機能拡張ポイントを提供します。</span><span class="sxs-lookup"><span data-stu-id="12609-129">Attribute Routing now provides an extensibility point called IDirectRouteProvider, which allows full control over how attribute routes are discovered and configured.</span></span> <span data-ttu-id="12609-130">アクションおよびアクションのどのようなルーティング構成が必要なだけ指定に関連するルート情報と共にコント ローラーの一覧を提供する場合は、IDirectRouteProvider します。</span><span class="sxs-lookup"><span data-stu-id="12609-130">An IDirectRouteProvider is responsible for providing a list of actions and controllers along with associated route information to specify exactly what routing configuration is desired for those actions.</span></span> <span data-ttu-id="12609-131">MapAttributes/MapHttpAttributeRoutes を呼び出すときに、IDirectRouteProvider 実装を指定することがあります。</span><span class="sxs-lookup"><span data-stu-id="12609-131">An IDirectRouteProvider implementation may be specified when calling MapAttributes/MapHttpAttributeRoutes.</span></span>

<span data-ttu-id="12609-132">IDirectRouteProvider のカスタマイズが簡単に、既定の実装、DefaultDirectRouteProvider を拡張することによってです。</span><span class="sxs-lookup"><span data-stu-id="12609-132">Customizing IDirectRouteProvider will be easiest by extending our default implementation, DefaultDirectRouteProvider.</span></span> <span data-ttu-id="12609-133">このクラスは、属性を検出し、ルート エントリを作成して、ルート プレフィックスおよび領域プレフィックスを検出するためのロジックを変更する別のオーバーライド可能な仮想メソッドを提供します。</span><span class="sxs-lookup"><span data-stu-id="12609-133">This class provides separate overridable virtual methods to change the logic for discovering attributes, creating route entries, and discovering route prefix and area prefix.</span></span>

<span data-ttu-id="12609-134">新しい属性ルーティングの拡張性と IDirectRouteProvider、ユーザーは、次の操作を行います可能性があります。</span><span class="sxs-lookup"><span data-stu-id="12609-134">With the new attribute routing extensibility of IDirectRouteProvider, a user could do the following:</span></span>

1. <span data-ttu-id="12609-135">属性ルートの継承をサポートします。</span><span class="sxs-lookup"><span data-stu-id="12609-135">Support Inheritance of attribute routes.</span></span> <span data-ttu-id="12609-136">たとえば、次のシナリオでブログおよびストアのコント ローラーを使用している、BaseController で定義されている属性のルーティング規約。</span><span class="sxs-lookup"><span data-stu-id="12609-136">For example, in the following scenario Blog and Store controllers are using an attribute route convention that is defined by the BaseController.</span></span> 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample1.cs)]
2. <span data-ttu-id="12609-137">属性のルートのルート名を自動的に生成します。</span><span class="sxs-lookup"><span data-stu-id="12609-137">Automatically generate route names for attribute routes.</span></span> 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample2.cs)]
3. <span data-ttu-id="12609-138">ルートは、ルート テーブルに追加される前に、1 か所でルート プレフィックスを変更します。</span><span class="sxs-lookup"><span data-stu-id="12609-138">Modify route prefixes in one central place before the routes get added to the route table.</span></span>
4. <span data-ttu-id="12609-139">検索する属性をルーティングするコント ローラーを除外します。</span><span class="sxs-lookup"><span data-stu-id="12609-139">Filter out the controllers on which you want the attribute routing to look for.</span></span> <span data-ttu-id="12609-140">3 と 4 でブログにすぐに予定です。</span><span class="sxs-lookup"><span data-stu-id="12609-140">We hope to blog on 3 and 4 soon.</span></span>

### <a name="facebook-fixes-for-changed-api-surface"></a><span data-ttu-id="12609-141">Facebook が変更された API サーフェイスでの修正します。</span><span class="sxs-lookup"><span data-stu-id="12609-141">Facebook fixes for changed API surface</span></span>

<span data-ttu-id="12609-142">MVC Facebook パッケージ[が解除された](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=Facebook&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=All)Facebook によって行われたいくつかの API の変更のためです。</span><span class="sxs-lookup"><span data-stu-id="12609-142">The MVC Facebook package [was broken](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=Facebook&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=All) due to few API changes made by Facebook.</span></span> <span data-ttu-id="12609-143">新しい Facebook パッケージ (Microsoft.AspNet.Facebook 1.0.0) これらの問題を修正するリリースもします。</span><span class="sxs-lookup"><span data-stu-id="12609-143">We are also releasing a new Facebook package (Microsoft.AspNet.Facebook 1.0.0) to fix these issues.</span></span>

<a id="knownbreakingchanges"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="12609-144">既知の問題と重大な変更</span><span class="sxs-lookup"><span data-stu-id="12609-144">Known Issues and Breaking Changes</span></span>

### <a name="scaffolding-mvcweb-api-into-a-project-with-520-packages-results-in-512-packages-for-ones-that-dont-already-exist-in-the-project"></a><span data-ttu-id="12609-145">5.2.0 をプロジェクトに MVC または Web API のスキャフォールディングをプロジェクトに既に存在しないもの 5.1.2 パッケージの結果をパッケージ化</span><span class="sxs-lookup"><span data-stu-id="12609-145">Scaffolding MVC/Web API into a project with 5.2.0 packages results in 5.1.2 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="12609-146">ASP.NET MVC 5.2.0 の NuGet パッケージを更新しても、ASP.NET スキャフォールディングなど Visual Studio のツールや ASP.NET Web アプリケーション プロジェクト テンプレートには更新されません。</span><span class="sxs-lookup"><span data-stu-id="12609-146">Updating NuGet packages for ASP.NET MVC 5.2.0 does not update the Visual Studio tools such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="12609-147">ASP.NET ランタイム パッケージ (Update 2 で 5.1.2 など) の以前のバージョンを使用します。</span><span class="sxs-lookup"><span data-stu-id="12609-147">They use the previous version of the ASP.NET runtime packages (e.g. 5.1.2 in Update 2).</span></span> <span data-ttu-id="12609-148">プロジェクトで使用されていない場合に、ASP.NET スキャフォールディングでその結果、必要なパッケージの以前のバージョン (Update 2 で 5.1.2 など) がインストールされますか。</span><span class="sxs-lookup"><span data-stu-id="12609-148">As a result, the ASP.NET scaffolding will install the previous version (e.g. 5.1.2 in Update 2) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="12609-149">ただし、Visual Studio 2013 RTM または更新プログラム 1 で ASP.NET スキャフォールディングでは、プロジェクト内の最新のパッケージは上書きされません。</span><span class="sxs-lookup"><span data-stu-id="12609-149">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span> <span data-ttu-id="12609-150">Web API 2.2 または ASP.NET MVC 5.2、プロジェクトのパッケージを更新した後に ASP.NET のスキャフォールディングを使用する場合は、Web API と ASP.NET MVC のバージョンでは、一貫性を確認します。</span><span class="sxs-lookup"><span data-stu-id="12609-150">If you use ASP.NET scaffolding after updating the packages of your projects to Web API 2.2 or ASP.NET MVC 5.2, make sure the versions of Web API and ASP.NET MVC are consistent.</span></span>

### <a name="microsoftjqueryunobtrusivevalidation-nuget-package-installation-fails-because-it-is-unable-to-find-a-version-of-microsoftjqueryunobtrusivevalidation-compatible-to-jquery-141"></a><span data-ttu-id="12609-151">JQuery 1.4.1 と互換性のある Microsoft.jQuery.Unobtrusive.Validation のバージョンを検索することはないために、Microsoft.jQuery.Unobtrusive.Validation NuGet パッケージのインストールが失敗しました。</span><span class="sxs-lookup"><span data-stu-id="12609-151">Microsoft.jQuery.Unobtrusive.Validation NuGet package installation fails because it is unable to find a version of Microsoft.jQuery.Unobtrusive.Validation compatible to jQuery 1.4.1</span></span>

<span data-ttu-id="12609-152">Microsoft.jQuery.Unobtrusive.Validation jQuery が必要です&gt;= 1.8 および jQuery.Validation &gt;1.8 を = です。</span><span class="sxs-lookup"><span data-stu-id="12609-152">Microsoft.jQuery.Unobtrusive.Validation requires jQuery &gt;=1.8 and jQuery.Validation &gt;=1.8.</span></span> <span data-ttu-id="12609-153">But,jQuery.Validation (1.8) には、jQuery が必要があります (&#8805; 1.3.2 &amp; &amp; &#8804;1.6;)。</span><span class="sxs-lookup"><span data-stu-id="12609-153">But,jQuery.Validation (1.8) needs jQuery (&#8805; 1.3.2 &amp;&amp; &#8804; 1.6).</span></span> <span data-ttu-id="12609-154">このため、NuGet は、同時に、JQuery 1.8 と jQuery.Validation 1.8 をインストールすると失敗します。</span><span class="sxs-lookup"><span data-stu-id="12609-154">Because of this, when NuGet installs the JQuery 1.8 and jQuery.Validation 1.8 at the same time, it fails.</span></span> <span data-ttu-id="12609-155">JQuery.Validation のバージョンを更新できるだけでこの問題を見つけたら、 &gt; =  [1.8.0.1](https://www.nuget.org/packages/jQuery.Validation/1.8.0.1)固定 jQuery キャップを持つ最初に、ことができますをインストールするにはMicrosoft.jQuery.Unobtrusive.Validation です。</span><span class="sxs-lookup"><span data-stu-id="12609-155">When you see this issue, you can simply update the version of jQuery.Validation to &gt;= [1.8.0.1](https://www.nuget.org/packages/jQuery.Validation/1.8.0.1) which has the jQuery cap fixed first, you should be able to install Microsoft.jQuery.Unobtrusive.Validation.</span></span>

### <a name="the-jqueryvalidation-nuget-package-version-1130-does-not-recognize-some-international-email-addresses"></a><span data-ttu-id="12609-156">Jquery です。検証 1.13.0 nuget パッケージのバージョンでは、いくつかの国際対応の電子メール アドレスは認識されません。</span><span class="sxs-lookup"><span data-stu-id="12609-156">The jquery.Validation nuget package version 1.13.0 does not recognize some international email addresses</span></span>

<span data-ttu-id="12609-157">jQuery.Validation 1.11.1 nuget パッケージのバージョンは、次の有効な電子メール アドレスが認識する既知の最新バージョンです。</span><span class="sxs-lookup"><span data-stu-id="12609-157">jQuery.Validation nuget package version 1.11.1 is the last known version which recognizes following valid email addresses.</span></span> <span data-ttu-id="12609-158">それ以降のバージョンは、認識されるようにすることがない場合があります。</span><span class="sxs-lookup"><span data-stu-id="12609-158">Any later versions might not be able to recognize them.</span></span> <span data-ttu-id="12609-159">例:</span><span class="sxs-lookup"><span data-stu-id="12609-159">For example:</span></span>

<span data-ttu-id="12609-160">電子メール アドレスの国際化 (EAI) の標準 (例: [&#29992; &#25143;@domain.com](mailto:&#29992;&#25143;@domain.com))</span><span class="sxs-lookup"><span data-stu-id="12609-160">E-Mail Address Internationalization (EAI) standard (e.g., [&#29992;&#25143;@domain.com](mailto:&#29992;&#25143;@domain.com))</span></span>   
 <span data-ttu-id="12609-161">EAI + 国際化 Resource Identifier (Iri) (例えば。、 [(& a) #29992; &#25143; @&#1076; (& a) #1086; (& a) #1084; (& a) #1077; (& a) #1085;。&#1088; &#1092;です。](mailto:&#29992;&#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;.&#1088;&#1092;))</span><span class="sxs-lookup"><span data-stu-id="12609-161">EAI + Internationalized Resource Identifiers (IRIs) (eg., [&#29992;&#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;.&#1088;&#1092;](mailto:&#29992;&#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;.&#1088;&#1092;))</span></span>

<span data-ttu-id="12609-162">問題が報告された[https://github.com/jzaefferer/jquery-validation/issues/1222](https://github.com/jzaefferer/jquery-validation/issues/1222)</span><span class="sxs-lookup"><span data-stu-id="12609-162">The issue is reported at [https://github.com/jzaefferer/jquery-validation/issues/1222](https://github.com/jzaefferer/jquery-validation/issues/1222)</span></span>

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a><span data-ttu-id="12609-163">Visual Studio 2013 での Razor ビューの構文の強調表示</span><span class="sxs-lookup"><span data-stu-id="12609-163">Syntax Highlighting for Razor Views in Visual Studio 2013</span></span>

<span data-ttu-id="12609-164">Visual Studio 2013 を更新せずに ASP.NET MVC 5.2 を更新する場合は、Razor ビューの編集中に構文の強調表示の Visual Studio エディターのサポートを取得するはできません。</span><span class="sxs-lookup"><span data-stu-id="12609-164">If you update to ASP.NET MVC 5.2 without updating Visual Studio 2013, you will not get Visual Studio editor support for syntax highlighting while editing the Razor views.</span></span> <span data-ttu-id="12609-165">このサポートを取得する Visual Studio 2013 を更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="12609-165">You will need to update Visual Studio 2013 to get this support.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a><span data-ttu-id="12609-166">バグの修正およびマイナー機能の更新</span><span class="sxs-lookup"><span data-stu-id="12609-166">Bug Fixes and Minor feature updates</span></span>

<span data-ttu-id="12609-167">このリリースではいくつかのバグ修正とマイナー機能も更新します。</span><span class="sxs-lookup"><span data-stu-id="12609-167">This release also includes several bug fixes and minor feature updates.</span></span> <span data-ttu-id="12609-168">完全な一覧は、ここをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="12609-168">You can find the complete list here:</span></span>

- [<span data-ttu-id="12609-169">5.2 パッケージ</span><span class="sxs-lookup"><span data-stu-id="12609-169">5.2 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

<a id="52"></a>
## <a name="microsoftaspnetmvc-522"></a><span data-ttu-id="12609-170">Microsoft.AspNet.MVC 5.2.2</span><span class="sxs-lookup"><span data-stu-id="12609-170">Microsoft.AspNet.MVC 5.2.2</span></span>

<span data-ttu-id="12609-171">このリリースでは、MVC での新機能、バグの修正がありません。</span><span class="sxs-lookup"><span data-stu-id="12609-171">This release doesn't have any new features or bug fixes in MVC.</span></span> <span data-ttu-id="12609-172">行いました、 [Web ページで変更](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx)大幅なパフォーマンス向上のためとおを所有して Web ページのこの新しいバージョンに依存するその他のすべての依存パッケージが、後で更新します。</span><span class="sxs-lookup"><span data-stu-id="12609-172">We made a [change in Web Pages](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx) for a significant performance improvement and have subsequently updated all other dependent packages we own to depend on this new version of Web Pages.</span></span>

<a id="mvc523Beta"></a>
## <a name="aspnet-mvc-523-beta"></a><span data-ttu-id="12609-173">ASP.NET MVC 5.2.3 Beta</span><span class="sxs-lookup"><span data-stu-id="12609-173">ASP.NET MVC 5.2.3 Beta</span></span>

<span data-ttu-id="12609-174">リリースについて[ここ](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="12609-174">You can read about the release [here](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx).</span></span> <span data-ttu-id="12609-175">このリリースには、バグの修正プログラムのみが含まれています。</span><span class="sxs-lookup"><span data-stu-id="12609-175">This release contains only bug fixes.</span></span> <span data-ttu-id="12609-176">使用することができます[このクエリ](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=MVC&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed)今回のリリースで修正された問題の一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="12609-176">You can use [this query](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=MVC&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) to see the list of issues fixed in this release.</span></span>
