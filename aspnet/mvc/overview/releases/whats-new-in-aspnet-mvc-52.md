---
uid: mvc/overview/releases/whats-new-in-aspnet-mvc-52
title: ASP.NET MVC 5.2 の新機能新機能 |Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 12/25/2014
ms.assetid: 97972587-2720-48b4-b158-f35f2e855fbf
msc.legacyurl: /mvc/overview/releases/whats-new-in-aspnet-mvc-52
msc.type: authoredcontent
ms.openlocfilehash: 76e1c13453c45a1b8ca71dd6435dcbfbd8bec66d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37801542"
---
<a name="whats-new-in-aspnet-mvc-52"></a><span data-ttu-id="fba29-102">ASP.NET MVC 5.2 の新機能新機能</span><span class="sxs-lookup"><span data-stu-id="fba29-102">What’s New in ASP.NET MVC 5.2</span></span>
====================
<span data-ttu-id="fba29-103">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="fba29-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="fba29-104">このトピックでは、ASP.NET MVC 5.2 の新機能新機能について説明[Microsoft.AspNet.MVC 5.2.2](#52)と[ASP.NET MVC 5.2.3 Beta](#mvc523Beta)</span><span class="sxs-lookup"><span data-stu-id="fba29-104">This topic describes what's new for ASP.NET MVC 5.2, [Microsoft.AspNet.MVC 5.2.2](#52) and [ASP.NET MVC 5.2.3 Beta](#mvc523Beta)</span></span>

- [<span data-ttu-id="fba29-105">ソフトウェアの要件</span><span class="sxs-lookup"><span data-stu-id="fba29-105">Software Requirements</span></span>](#softRequire)
- [<span data-ttu-id="fba29-106">ダウンロード</span><span class="sxs-lookup"><span data-stu-id="fba29-106">Download</span></span>](#download)
- [<span data-ttu-id="fba29-107">ドキュメント</span><span class="sxs-lookup"><span data-stu-id="fba29-107">Documentation</span></span>](#documentation)
- [<span data-ttu-id="fba29-108">ASP.NET MVC 5.2 の新機能</span><span class="sxs-lookup"><span data-stu-id="fba29-108">New Features in ASP.NET MVC 5.2</span></span>](#new-features)

    - [<span data-ttu-id="fba29-109">属性のルーティングが強化されました</span><span class="sxs-lookup"><span data-stu-id="fba29-109">Attribute routing improvements</span></span>](#attributerouting)
- [<span data-ttu-id="fba29-110">既知の問題と重大な変更</span><span class="sxs-lookup"><span data-stu-id="fba29-110">Known Issues and Breaking Changes</span></span>](#knownbreakingchanges)
- [<span data-ttu-id="fba29-111">バグの修正</span><span class="sxs-lookup"><span data-stu-id="fba29-111">Bug Fixes</span></span>](#bug-fixes)
- [<span data-ttu-id="fba29-112">Microsoft.AspNet.MVC 5.2.2</span><span class="sxs-lookup"><span data-stu-id="fba29-112">Microsoft.AspNet.MVC 5.2.2</span></span>](#52)

<a id="softRequire"></a>
## <a name="software-requirements"></a><span data-ttu-id="fba29-113">ソフトウェア要件</span><span class="sxs-lookup"><span data-stu-id="fba29-113">Software Requirements</span></span>

- <span data-ttu-id="fba29-114">Visual Studio 2012: ダウンロード[ASP.NET および Web Tools 2013.1 for Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062)します。</span><span class="sxs-lookup"><span data-stu-id="fba29-114">Visual Studio 2012: Download [ASP.NET and Web Tools 2013.1 for Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).</span></span>
- <span data-ttu-id="fba29-115">Visual Studio 2013 の場合: ダウンロード[Visual Studio 2013 Update](https://go.microsoft.com/fwlink/?LinkId=390064)またはそれ以降。</span><span class="sxs-lookup"><span data-stu-id="fba29-115">Visual Studio 2013: Download [Visual Studio 2013 Update](https://go.microsoft.com/fwlink/?LinkId=390064) or higher.</span></span> <span data-ttu-id="fba29-116">ASP.NET MVC 5.2 の Razor ビューを編集するためには、この更新プログラムが必要です。</span><span class="sxs-lookup"><span data-stu-id="fba29-116">This update is needed for editing ASP.NET MVC 5.2 Razor Views.</span></span>

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="fba29-117">ダウンロード</span><span class="sxs-lookup"><span data-stu-id="fba29-117">Download</span></span>

<span data-ttu-id="fba29-118">ランタイムの機能は、NuGet ギャラリーでの NuGet パッケージとしてリリースされます。</span><span class="sxs-lookup"><span data-stu-id="fba29-118">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="fba29-119">次のすべてのランタイム パッケージ、[セマンティック バージョニング](http://semver.org/)仕様。</span><span class="sxs-lookup"><span data-stu-id="fba29-119">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="fba29-120">ASP.NET MVC 5.2 の最新のパッケージは、次のバージョン:「5.2.0」。</span><span class="sxs-lookup"><span data-stu-id="fba29-120">The latest ASP.NET MVC 5.2 package has the following version: "5.2.0".</span></span> <span data-ttu-id="fba29-121">インストールまたはを通じてこれらのパッケージを更新することができます[NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/)します。</span><span class="sxs-lookup"><span data-stu-id="fba29-121">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/).</span></span> <span data-ttu-id="fba29-122">リリースには、NuGet での対応するローカライズされたパッケージも含まれています。</span><span class="sxs-lookup"><span data-stu-id="fba29-122">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="fba29-123">インストールまたは NuGet パッケージ マネージャー コンソールを使用して、リリースされた NuGet パッケージを更新できます。</span><span class="sxs-lookup"><span data-stu-id="fba29-123">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

<span data-ttu-id="fba29-124">インストール パッケージ Microsoft.AspNet.Mvc-バージョン 5.2.0</span><span class="sxs-lookup"><span data-stu-id="fba29-124">Install-Package Microsoft.AspNet.Mvc -Version 5.2.0</span></span>

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="fba29-125">ドキュメント</span><span class="sxs-lookup"><span data-stu-id="fba29-125">Documentation</span></span>

<span data-ttu-id="fba29-126">チュートリアルと ASP.NET MVC 5.2 に関する他の情報は、ASP.NET web サイトから入手できます ([https://www.asp.net/mvc](../../index.md))。</span><span class="sxs-lookup"><span data-stu-id="fba29-126">Tutorials and other information about ASP.NET MVC 5.2 are available from the ASP.NET web site ([https://www.asp.net/mvc](../../index.md)).</span></span>

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-52"></a><span data-ttu-id="fba29-127">ASP.NET MVC 5.2 の新機能</span><span class="sxs-lookup"><span data-stu-id="fba29-127">New Features in ASP.NET MVC 5.2</span></span>

<a id="attributerouting"></a>
### <a name="attribute-routing-improvements"></a><span data-ttu-id="fba29-128">属性のルーティングが強化されました</span><span class="sxs-lookup"><span data-stu-id="fba29-128">Attribute routing improvements</span></span>

<span data-ttu-id="fba29-129">属性ルーティングを今すぐ IDirectRouteProvider で、属性ルートが検出され、構成する方法を完全に制御できますと呼ばれる機能拡張ポイントを提供します。</span><span class="sxs-lookup"><span data-stu-id="fba29-129">Attribute Routing now provides an extensibility point called IDirectRouteProvider, which allows full control over how attribute routes are discovered and configured.</span></span> <span data-ttu-id="fba29-130">IDirectRouteProvider はアクションとコント ローラーとアクションのどのようなルーティングの構成が必要なだけを指定する関連付けられたルート情報の一覧を提供します。</span><span class="sxs-lookup"><span data-stu-id="fba29-130">An IDirectRouteProvider is responsible for providing a list of actions and controllers along with associated route information to specify exactly what routing configuration is desired for those actions.</span></span> <span data-ttu-id="fba29-131">MapAttributes/MapHttpAttributeRoutes を呼び出すときに、IDirectRouteProvider 実装を指定することがあります。</span><span class="sxs-lookup"><span data-stu-id="fba29-131">An IDirectRouteProvider implementation may be specified when calling MapAttributes/MapHttpAttributeRoutes.</span></span>

<span data-ttu-id="fba29-132">IDirectRouteProvider のカスタマイズになります最も簡単な既定の実装、DefaultDirectRouteProvider を拡張することによって。</span><span class="sxs-lookup"><span data-stu-id="fba29-132">Customizing IDirectRouteProvider will be easiest by extending our default implementation, DefaultDirectRouteProvider.</span></span> <span data-ttu-id="fba29-133">このクラスは、属性の検出、ルートのエントリを作成するルート プレフィックス領域プレフィックスを検出するためのロジックを変更する別のオーバーライド可能な仮想メソッドを提供します。</span><span class="sxs-lookup"><span data-stu-id="fba29-133">This class provides separate overridable virtual methods to change the logic for discovering attributes, creating route entries, and discovering route prefix and area prefix.</span></span>

<span data-ttu-id="fba29-134">新しい属性ルーティングの拡張性と IDirectRouteProvider、ユーザーは、次の方法でした。</span><span class="sxs-lookup"><span data-stu-id="fba29-134">With the new attribute routing extensibility of IDirectRouteProvider, a user could do the following:</span></span>

1. <span data-ttu-id="fba29-135">属性ルートの継承をサポートします。</span><span class="sxs-lookup"><span data-stu-id="fba29-135">Support Inheritance of attribute routes.</span></span> <span data-ttu-id="fba29-136">たとえば、次のシナリオでブログとストアのコント ローラーを使用している、BaseController で定義されている属性ルート規則。</span><span class="sxs-lookup"><span data-stu-id="fba29-136">For example, in the following scenario Blog and Store controllers are using an attribute route convention that is defined by the BaseController.</span></span> 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample1.cs)]
2. <span data-ttu-id="fba29-137">属性ルートのルート名を自動的に生成します。</span><span class="sxs-lookup"><span data-stu-id="fba29-137">Automatically generate route names for attribute routes.</span></span> 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample2.cs)]
3. <span data-ttu-id="fba29-138">ルート、ルート テーブルに追加される前に、1 か所でルート プレフィックスを変更します。</span><span class="sxs-lookup"><span data-stu-id="fba29-138">Modify route prefixes in one central place before the routes get added to the route table.</span></span>
4. <span data-ttu-id="fba29-139">検索する属性のルーティング先のコント ローラーを除外します。</span><span class="sxs-lookup"><span data-stu-id="fba29-139">Filter out the controllers on which you want the attribute routing to look for.</span></span> <span data-ttu-id="fba29-140">お役に 3 および 4 のブログにすぐにします。</span><span class="sxs-lookup"><span data-stu-id="fba29-140">We hope to blog on 3 and 4 soon.</span></span>

### <a name="facebook-fixes-for-changed-api-surface"></a><span data-ttu-id="fba29-141">Facebook が変更された API サーフェイスでの修正します。</span><span class="sxs-lookup"><span data-stu-id="fba29-141">Facebook fixes for changed API surface</span></span>

<span data-ttu-id="fba29-142">MVC Facebook パッケージ[が壊れた](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=Facebook&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=All)Facebook によって行われたいくつかの API の変更が原因です。</span><span class="sxs-lookup"><span data-stu-id="fba29-142">The MVC Facebook package [was broken](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=Facebook&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=All) due to few API changes made by Facebook.</span></span> <span data-ttu-id="fba29-143">新しい Facebook パッケージ (Microsoft.AspNet.Facebook 1.0.0) これらの問題を修正することもリリースします。</span><span class="sxs-lookup"><span data-stu-id="fba29-143">We are also releasing a new Facebook package (Microsoft.AspNet.Facebook 1.0.0) to fix these issues.</span></span>

<a id="knownbreakingchanges"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="fba29-144">既知の問題と重大な変更</span><span class="sxs-lookup"><span data-stu-id="fba29-144">Known Issues and Breaking Changes</span></span>

### <a name="scaffolding-mvcweb-api-into-a-project-with-520-packages-results-in-512-packages-for-ones-that-dont-already-exist-in-the-project"></a><span data-ttu-id="fba29-145">プロジェクトに 5.2.0 にスキャフォールディング MVC または Web API プロジェクトに既に存在しないものの 5.1.2 で結果をパッケージにパッケージ化</span><span class="sxs-lookup"><span data-stu-id="fba29-145">Scaffolding MVC/Web API into a project with 5.2.0 packages results in 5.1.2 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="fba29-146">ASP.NET MVC 5.2.0 用 NuGet パッケージを更新しても、ASP.NET スキャフォールディングなどの Visual Studio のツールや、ASP.NET Web アプリケーション プロジェクト テンプレートには更新されません。</span><span class="sxs-lookup"><span data-stu-id="fba29-146">Updating NuGet packages for ASP.NET MVC 5.2.0 does not update the Visual Studio tools such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="fba29-147">ASP.NET ランタイム パッケージ (例: 更新プログラム 2 で 5.1.2) の以前のバージョンを使用します。</span><span class="sxs-lookup"><span data-stu-id="fba29-147">They use the previous version of the ASP.NET runtime packages (e.g. 5.1.2 in Update 2).</span></span> <span data-ttu-id="fba29-148">その結果、プロジェクトで使用されていない場合は、ASP.NET スキャフォールディングは、必要なパッケージの以前のバージョン (例: 更新プログラム 2 で 5.1.2) がインストールされます。</span><span class="sxs-lookup"><span data-stu-id="fba29-148">As a result, the ASP.NET scaffolding will install the previous version (e.g. 5.1.2 in Update 2) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="fba29-149">ただし、Visual Studio 2013 RTM や Update 1 での ASP.NET スキャフォールディングでは、プロジェクトで最新のパッケージは上書きされません。</span><span class="sxs-lookup"><span data-stu-id="fba29-149">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span> <span data-ttu-id="fba29-150">Web API 2.2 または ASP.NET MVC 5.2 に、プロジェクトのパッケージを更新した後、ASP.NET スキャフォールディングを使用する場合は、Web API と ASP.NET MVC のバージョンでは、一貫性を確認します。</span><span class="sxs-lookup"><span data-stu-id="fba29-150">If you use ASP.NET scaffolding after updating the packages of your projects to Web API 2.2 or ASP.NET MVC 5.2, make sure the versions of Web API and ASP.NET MVC are consistent.</span></span>

### <a name="microsoftjqueryunobtrusivevalidation-nuget-package-installation-fails-because-it-is-unable-to-find-a-version-of-microsoftjqueryunobtrusivevalidation-compatible-to-jquery-141"></a><span data-ttu-id="fba29-151">JQuery 1.4.1 に互換性のある Microsoft.jQuery.Unobtrusive.Validation のバージョンを検索できないために、Microsoft.jQuery.Unobtrusive.Validation NuGet パッケージのインストールが失敗します。</span><span class="sxs-lookup"><span data-stu-id="fba29-151">Microsoft.jQuery.Unobtrusive.Validation NuGet package installation fails because it is unable to find a version of Microsoft.jQuery.Unobtrusive.Validation compatible to jQuery 1.4.1</span></span>

<span data-ttu-id="fba29-152">Microsoft.jQuery.Unobtrusive.Validation jQuery を必要と&gt;= 1.8 および jQuery.Validation &gt;1.8 を = です。</span><span class="sxs-lookup"><span data-stu-id="fba29-152">Microsoft.jQuery.Unobtrusive.Validation requires jQuery &gt;=1.8 and jQuery.Validation &gt;=1.8.</span></span> <span data-ttu-id="fba29-153">But,jQuery.Validation (1.8) には、jQuery が必要があります (&#8805; 1.3.2 &amp; &amp; &#8804; 1.6)。</span><span class="sxs-lookup"><span data-stu-id="fba29-153">But,jQuery.Validation (1.8) needs jQuery (&#8805; 1.3.2 &amp;&amp; &#8804; 1.6).</span></span> <span data-ttu-id="fba29-154">このため、NuGet と同時に、JQuery 1.8 および jQuery.Validation 1.8 のインストール時に失敗します。</span><span class="sxs-lookup"><span data-stu-id="fba29-154">Because of this, when NuGet installs the JQuery 1.8 and jQuery.Validation 1.8 at the same time, it fails.</span></span> <span data-ttu-id="fba29-155">この問題を確認したら、単に jQuery.Validation のバージョンを更新できます&gt; =  [1.8.0.1](https://www.nuget.org/packages/jQuery.Validation/1.8.0.1)固定 jQuery キャップを持つ最初に、おく必要があることをインストールするにはMicrosoft.jQuery.Unobtrusive.Validation します。</span><span class="sxs-lookup"><span data-stu-id="fba29-155">When you see this issue, you can simply update the version of jQuery.Validation to &gt;= [1.8.0.1](https://www.nuget.org/packages/jQuery.Validation/1.8.0.1) which has the jQuery cap fixed first, you should be able to install Microsoft.jQuery.Unobtrusive.Validation.</span></span>

### <a name="the-jqueryvalidation-nuget-package-version-1130-does-not-recognize-some-international-email-addresses"></a><span data-ttu-id="fba29-156">Jquery です。検証 1.13.0 nuget パッケージのバージョンでは、いくつかの国際対応の電子メール アドレスは認識されません。</span><span class="sxs-lookup"><span data-stu-id="fba29-156">The jquery.Validation nuget package version 1.13.0 does not recognize some international email addresses</span></span>

<span data-ttu-id="fba29-157">jQuery.Validation nuget パッケージのバージョン 1.11.1 は、次の有効な電子メール アドレスを認識する既知の最新バージョンです。</span><span class="sxs-lookup"><span data-stu-id="fba29-157">jQuery.Validation nuget package version 1.11.1 is the last known version which recognizes following valid email addresses.</span></span> <span data-ttu-id="fba29-158">それ以降のバージョンはできないことを認識することがあります。</span><span class="sxs-lookup"><span data-stu-id="fba29-158">Any later versions might not be able to recognize them.</span></span> <span data-ttu-id="fba29-159">例えば:</span><span class="sxs-lookup"><span data-stu-id="fba29-159">For example:</span></span>

<span data-ttu-id="fba29-160">電子メール アドレスの国際化 (EAI) の標準 (例: [ &#29992; &#25143; @domain.com ](mailto:&#29992;&#25143;@domain.com))</span><span class="sxs-lookup"><span data-stu-id="fba29-160">E-Mail Address Internationalization (EAI) standard (e.g., [&#29992;&#25143;@domain.com](mailto:&#29992;&#25143;@domain.com))</span></span>   
 <span data-ttu-id="fba29-161">EAI + 国際化されたリソース識別子 (Iri) (例:、 [ &#29992; &#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;。&#1088; 。&#1092;](mailto:&#29992;&#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;.&#1088;&#1092;))</span><span class="sxs-lookup"><span data-stu-id="fba29-161">EAI + Internationalized Resource Identifiers (IRIs) (eg., [&#29992;&#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;.&#1088;&#1092;](mailto:&#29992;&#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;.&#1088;&#1092;))</span></span>

<span data-ttu-id="fba29-162">問題がレポートされます。 [https://github.com/jzaefferer/jquery-validation/issues/1222](https://github.com/jzaefferer/jquery-validation/issues/1222)</span><span class="sxs-lookup"><span data-stu-id="fba29-162">The issue is reported at [https://github.com/jzaefferer/jquery-validation/issues/1222](https://github.com/jzaefferer/jquery-validation/issues/1222)</span></span>

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a><span data-ttu-id="fba29-163">Visual Studio 2013 での Razor ビューの構文の強調表示</span><span class="sxs-lookup"><span data-stu-id="fba29-163">Syntax Highlighting for Razor Views in Visual Studio 2013</span></span>

<span data-ttu-id="fba29-164">Visual Studio 2013 を更新することがなく、ASP.NET MVC 5.2 に更新する場合は、Razor ビューの編集中に構文の強調表示の Visual Studio エディターのサポートを取得するはできません。</span><span class="sxs-lookup"><span data-stu-id="fba29-164">If you update to ASP.NET MVC 5.2 without updating Visual Studio 2013, you will not get Visual Studio editor support for syntax highlighting while editing the Razor views.</span></span> <span data-ttu-id="fba29-165">このサポートを受ける Visual Studio 2013 を更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fba29-165">You will need to update Visual Studio 2013 to get this support.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a><span data-ttu-id="fba29-166">バグの修正と軽微な機能の更新</span><span class="sxs-lookup"><span data-stu-id="fba29-166">Bug Fixes and Minor feature updates</span></span>

<span data-ttu-id="fba29-167">このリリースではいくつかのバグ修正と軽微な機能も更新します。</span><span class="sxs-lookup"><span data-stu-id="fba29-167">This release also includes several bug fixes and minor feature updates.</span></span> <span data-ttu-id="fba29-168">完全な一覧をここにあります。</span><span class="sxs-lookup"><span data-stu-id="fba29-168">You can find the complete list here:</span></span>

- [<span data-ttu-id="fba29-169">5.2 パッケージ</span><span class="sxs-lookup"><span data-stu-id="fba29-169">5.2 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

<a id="52"></a>
## <a name="microsoftaspnetmvc-522"></a><span data-ttu-id="fba29-170">Microsoft.AspNet.MVC 5.2.2</span><span class="sxs-lookup"><span data-stu-id="fba29-170">Microsoft.AspNet.MVC 5.2.2</span></span>

<span data-ttu-id="fba29-171">このリリースでは、その新しい機能やバグ修正を mvc がありません。</span><span class="sxs-lookup"><span data-stu-id="fba29-171">This release doesn't have any new features or bug fixes in MVC.</span></span> <span data-ttu-id="fba29-172">行いました、 [Web ページで変更](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx)大幅なパフォーマンス向上が Web ページのこの新しいバージョンに依存する私たちが所有の他のすべての従属パッケージを更新して、その後、します。</span><span class="sxs-lookup"><span data-stu-id="fba29-172">We made a [change in Web Pages](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx) for a significant performance improvement and have subsequently updated all other dependent packages we own to depend on this new version of Web Pages.</span></span>

<a id="mvc523Beta"></a>
## <a name="aspnet-mvc-523-beta"></a><span data-ttu-id="fba29-173">ASP.NET MVC 5.2.3 Beta</span><span class="sxs-lookup"><span data-stu-id="fba29-173">ASP.NET MVC 5.2.3 Beta</span></span>

<span data-ttu-id="fba29-174">リリースについて[ここ](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="fba29-174">You can read about the release [here](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx).</span></span> <span data-ttu-id="fba29-175">このリリースには、バグの修正プログラムのみが含まれています。</span><span class="sxs-lookup"><span data-stu-id="fba29-175">This release contains only bug fixes.</span></span> <span data-ttu-id="fba29-176">使用することができます[このクエリ](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=MVC&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed)をこのリリースで修正された問題の一覧を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fba29-176">You can use [this query](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=MVC&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) to see the list of issues fixed in this release.</span></span>
