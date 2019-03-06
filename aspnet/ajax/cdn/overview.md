---
uid: ajax/cdn/overview
title: Microsoft Ajax Content Delivery Network |Microsoft Docs
author: rick-anderson
description: ''
ms.author: riande
ms.date: 10/14/2017
ms.assetid: 8935bf14-ca6d-4a4e-9dbe-b96ce74cef49
msc.legacyurl: /ajax/cdn
msc.type: content
ms.openlocfilehash: ea42a33fbca09787a6643cb14e87f9a7b2fd32b0
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/05/2019
ms.locfileid: "57346555"
---
<a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="26921-102">Microsoft Ajax Content Delivery Network</span><span class="sxs-lookup"><span data-stu-id="26921-102">Microsoft Ajax Content Delivery Network</span></span>
====================
> [!WARNING]
> <span data-ttu-id="26921-103">実稼働アプリケーションでは、CDN 資産のハードの依存関係をなりません。</span><span class="sxs-lookup"><span data-stu-id="26921-103">Production applications should not take a hard dependency on CDN assets.</span></span> <span data-ttu-id="26921-104">アプリケーションは、参照されている場合、CDN 資産のテストし、CDN が利用できない場合は、フォールバックの資産を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="26921-104">Applications should test for the CDN asset referenced, and use a fallback asset when the CDN is not available.</span></span> 
>
> <span data-ttu-id="26921-105">Microsoft Ajax CDN では、Azure CDN の使用を上回ると SLA がありません。</span><span class="sxs-lookup"><span data-stu-id="26921-105">The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>
>
> <span data-ttu-id="26921-106">使用[この GitHub の問題](https://github.com/aspnet/Docs/issues/5832)Microsoft Ajax CDN の問題を報告します。</span><span class="sxs-lookup"><span data-stu-id="26921-106">Use [this GitHub issue](https://github.com/aspnet/Docs/issues/5832) to report problems with the Microsoft Ajax CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="26921-107">目次</span><span class="sxs-lookup"><span data-stu-id="26921-107">Table of Contents</span></span>

<span data-ttu-id="26921-108">**[ajax.microsoft.com ajax.aspnetcdn.com に変更されました](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="26921-108">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="26921-109">**[Visual Studio .vsdoc サポート](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="26921-109">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="26921-110">**[CDN から ASP.NET Ajax を使用します。](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="26921-110">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="26921-111">**[CDN から jQuery の使用](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="26921-111">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="26921-112">**[UI、CDN から jQuery の使用](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="26921-112">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="26921-113">**[Cdn のサード パーティ製ファイル](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="26921-113">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="26921-114">cdn の jQuery リリース</span><span class="sxs-lookup"><span data-stu-id="26921-114">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="26921-115">cdn の jQuery 移行リリース</span><span class="sxs-lookup"><span data-stu-id="26921-115">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="26921-116">jQuery UI、CDN のリリース</span><span class="sxs-lookup"><span data-stu-id="26921-116">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="26921-117">jQuery の検証、CDN のリリース</span><span class="sxs-lookup"><span data-stu-id="26921-117">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="26921-118">jQuery Mobile、CDN のリリース</span><span class="sxs-lookup"><span data-stu-id="26921-118">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="26921-119">jQuery cdn テンプレート リリース</span><span class="sxs-lookup"><span data-stu-id="26921-119">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="26921-120">jQuery、CDN のリリース サイクル</span><span class="sxs-lookup"><span data-stu-id="26921-120">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="26921-121">jQuery、CDN での Datatable のリリース</span><span class="sxs-lookup"><span data-stu-id="26921-121">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="26921-122">Cdn Modernizr リリース</span><span class="sxs-lookup"><span data-stu-id="26921-122">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="26921-123">CDN で JSHint リリース</span><span class="sxs-lookup"><span data-stu-id="26921-123">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="26921-124">Cdn knockout リリース</span><span class="sxs-lookup"><span data-stu-id="26921-124">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="26921-125">CDN でリリースをグローバル化します。</span><span class="sxs-lookup"><span data-stu-id="26921-125">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="26921-126">Cdn のリリースを応答します。</span><span class="sxs-lookup"><span data-stu-id="26921-126">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="26921-127">Cdn のブートス トラップのリリース</span><span class="sxs-lookup"><span data-stu-id="26921-127">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="26921-128">Cdn のブートス トラップ TouchCarousel リリース</span><span class="sxs-lookup"><span data-stu-id="26921-128">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="26921-129">Cdn Hammer.js リリース</span><span class="sxs-lookup"><span data-stu-id="26921-129">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="26921-130">ASP.NET Web フォームと Ajax CDN のリリース</span><span class="sxs-lookup"><span data-stu-id="26921-130">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="26921-131">ASP.NET MVC は、CDN にリリースします。</span><span class="sxs-lookup"><span data-stu-id="26921-131">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="26921-132">ASP.NET SignalR が CDN にリリースします。</span><span class="sxs-lookup"><span data-stu-id="26921-132">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="26921-133">Microsoft Ajax Content Delivery Network (CDN) は、jQuery などの人気のサード パーティ製の JavaScript ライブラリをホストし、Web アプリケーションに簡単に追加することができます。</span><span class="sxs-lookup"><span data-stu-id="26921-133">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="26921-134">追加するだけでこの CDN にホストされている jQuery の使用を開始するなど、&lt;スクリプト&gt;ajax.aspnetcdn.com を指すページにタグ。</span><span class="sxs-lookup"><span data-stu-id="26921-134">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="26921-135">CDN を利用して、Ajax アプリケーションのパフォーマンスを大幅に向上させることができます。</span><span class="sxs-lookup"><span data-stu-id="26921-135">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="26921-136">CDN のコンテンツは、世界各地に配置されたサーバーにキャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="26921-136">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="26921-137">さらに、CDN は、別のドメインに配置されている web サイトのキャッシュされたサード パーティ製の JavaScript ファイルを再利用のブラウザーを使用できます。</span><span class="sxs-lookup"><span data-stu-id="26921-137">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="26921-138">CDN は、Secure Sockets Layer を使用して web ページを使用する必要がある場合に、SSL (HTTPS) をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="26921-138">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="26921-139">CDN では、アップロードされているし、ライセンス供与はこれらのライブラリの所有者で次のサード パーティ製のスクリプト ライブラリをホストします。</span><span class="sxs-lookup"><span data-stu-id="26921-139">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="26921-140">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="26921-140">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="26921-141">jQuery UI (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="26921-141">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="26921-142">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="26921-142">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="26921-143">jQuery Validation (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="26921-143">jQuery Validation (www.jquery.com)</span></span>
- <span data-ttu-id="26921-144">jQuery Cycle (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="26921-144">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="26921-145">jQuery Datatable (http://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="26921-145">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="26921-146">Microsoft Ajax CDN には、Microsoft によってアップロードされた次のライブラリも含まれています。</span><span class="sxs-lookup"><span data-stu-id="26921-146">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="26921-147">ASP.NET Ajax</span><span class="sxs-lookup"><span data-stu-id="26921-147">ASP.NET Ajax</span></span>
- <span data-ttu-id="26921-148">ASP.NET MVC の JavaScript ファイル</span><span class="sxs-lookup"><span data-stu-id="26921-148">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="26921-149">ASP.NET SignalR JavaScript ファイルします。</span><span class="sxs-lookup"><span data-stu-id="26921-149">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="26921-150">Microsoft では、この CDN でホストされているすべてのサード パーティ製ライブラリの所有権を主張しません。</span><span class="sxs-lookup"><span data-stu-id="26921-150">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="26921-151">ライブラリの著作権所有者には、これらのライブラリにはライセンス付与します。</span><span class="sxs-lookup"><span data-stu-id="26921-151">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="26921-152">権限をダウンロードしてこのようなライブラリを使用する必要がありますが、それぞれの著作権所有者によってのみ付与されます。</span><span class="sxs-lookup"><span data-stu-id="26921-152">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="26921-153">これらは Microsoft ライブラリではありません、ため、Microsoft は用意されていません保証または (暗黙の特許権を含むなし) 知的財産権ライセンスこの CDN でホストされているサード パーティのライブラリです。</span><span class="sxs-lookup"><span data-stu-id="26921-153">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="26921-154">場合は、JavaScript ライブラリを提出して、ライブラリは、最上位の JavaScript ライブラリのいずれか (に示された http://trends.builtwith.com) または 拡張機能/プラグインには、人気のある (a)。 または (b) ASP.NET で使用するために役立ちますし、にお問い合わせくださいこれらのライブラリに AjaxCDNSubmission@Microsoft.com します。</span><span class="sxs-lookup"><span data-stu-id="26921-154">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="26921-155">ajax.microsoft.com ajax.aspnetcdn.com に変更されました</span><span class="sxs-lookup"><span data-stu-id="26921-155">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="26921-156">CDN では、microsoft.com ドメイン名を使用するために使用し、aspnetcdn.com のドメイン名の使用に変更されました。</span><span class="sxs-lookup"><span data-stu-id="26921-156">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="26921-157">この変更は、送信ので、ブラウザーには、microsoft.com ドメインが参照されている場合に、クッキー ドメインから要求ごとにネットワーク経由でパフォーマンスが向上しました。</span><span class="sxs-lookup"><span data-stu-id="26921-157">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="26921-158">Microsoft.com 以外のドメイン名に名前を変更するパフォーマンスは、25% に多くの部分を増やすことができます。</span><span class="sxs-lookup"><span data-stu-id="26921-158">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="26921-159">注 ajax.microsoft.com は引き続き機能しますが、ajax.aspnetcdn.com をお勧めします。</span><span class="sxs-lookup"><span data-stu-id="26921-159">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="26921-160">古い形式: https://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="26921-160">Old Format: https://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="26921-161">新しい形式: https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="26921-161">New Format: https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="26921-162">Visual Studio .vsdoc サポート</span><span class="sxs-lookup"><span data-stu-id="26921-162">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="26921-163">VS 2008 SP1 であることを確認する必要がある、Visual Studio 2008 .vsdoc ファイルを適切に使用するにはインストールし、vsdoc ファイルの修正プログラムをインストールします。</span><span class="sxs-lookup"><span data-stu-id="26921-163">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="26921-164">ここからこれらを取得できます。</span><span class="sxs-lookup"><span data-stu-id="26921-164">You can get these from here:</span></span>

- [<span data-ttu-id="26921-165">Visual Studio 2008 SP1 をダウンロード</span><span class="sxs-lookup"><span data-stu-id="26921-165">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "Visual Studio 2008 SP1 をダウンロードします。")
- [<span data-ttu-id="26921-166">Visual Studio 2008 SP1 の .vsdoc 修正プログラムをダウンロード</span><span class="sxs-lookup"><span data-stu-id="26921-166">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 ".vsdoc 修正プログラムを Visual Studio 2008 SP1 のダウンロード")

<span data-ttu-id="26921-167">Visual Studio 2010 では、追加、更新プログラムなし .vsdoc ファイルをサポートします。</span><span class="sxs-lookup"><span data-stu-id="26921-167">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="26921-168">CDN から ASP.NET Ajax を使用します。</span><span class="sxs-lookup"><span data-stu-id="26921-168">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="26921-169">ASP.NET 4 を使用する場合は、CDN に ASP.NET framework スクリプトのすべての要求をリダイレクトできます。</span><span class="sxs-lookup"><span data-stu-id="26921-169">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="26921-170">ローカル web サーバーではなく、CDN からスクリプトを取得すると、パブリックの ASP.NET web サイトのパフォーマンスを大幅向上できます。</span><span class="sxs-lookup"><span data-stu-id="26921-170">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="26921-171">Microsoft Ajax CDN の ASP.NET フレームワーク スクリプトのすべての要求をリダイレクトするのにには、ScriptManager EnableCDN プロパティを使用します。</span><span class="sxs-lookup"><span data-stu-id="26921-171">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="26921-172">CDN から jQuery の使用</span><span class="sxs-lookup"><span data-stu-id="26921-172">Using jQuery from the CDN</span></span>

<span data-ttu-id="26921-173">次のスクリプト要素をページに追加することで、Web アプリケーション CDN でホストされている jQuery スクリプトを使用できます。</span><span class="sxs-lookup"><span data-stu-id="26921-173">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="26921-174">CDN は、入手できる jQuery スクリプトの縮小されたバージョンも含まれています。 次の要素を使用します。</span><span class="sxs-lookup"><span data-stu-id="26921-174">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="26921-175">ページを使用できない場合は、CDN、独自の web サイト上のローカル パスから jQuery の読み込みにフォールバックを許可するには、CDN を参照する要素の直後に次の要素を追加します。</span><span class="sxs-lookup"><span data-stu-id="26921-175">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="26921-176">次のサンプル ページには、ボタンがクリックされたときに、div 要素の内容を表示する (ローカル コピーへのフォールバック) とともに、jQuery ライブラリの CDN のバージョンが使用されます。</span><span class="sxs-lookup"><span data-stu-id="26921-176">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="26921-177">詳細については、jQuery はでき、jQuery のローカル コピーをダウンロードにアクセスして、 [jQuery](http://jquery.com/) Web サイト。</span><span class="sxs-lookup"><span data-stu-id="26921-177">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="26921-178">UI、CDN から jQuery の使用</span><span class="sxs-lookup"><span data-stu-id="26921-178">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="26921-179">CDN では、jQuery UI ライブラリもホストします。</span><span class="sxs-lookup"><span data-stu-id="26921-179">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="26921-180">JQuery UI ライブラリには、ウィジェットと、ASP.NET アプリケーションで使用できる効果の豊富なセットが含まれています。</span><span class="sxs-lookup"><span data-stu-id="26921-180">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="26921-181">たとえば、次のページでは、ポップアップ カレンダーを表示する ASP.NET Web フォーム アプリケーションのコンテキストでの jQuery UI Datepicker を使用する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="26921-181">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="26921-182">キーボードを使用して、テキスト ボックスにフォーカスを移動すると、カレンダーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="26921-182">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![Datepicker で作成されたポップアップ カレンダー](overview/_static/image1.png)

<span data-ttu-id="26921-184">上記のコードで、CDN から 3 つのファイルを含める必要があることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="26921-184">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="26921-185">JQuery ライブラリ&mdash;jQuery UI ライブラリは jQuery ライブラリに依存します。</span><span class="sxs-lookup"><span data-stu-id="26921-185">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="26921-186">JQuery UI ライブラリを追加する前に、ページに jQuery ライブラリを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="26921-186">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="26921-187">JQuery UI ライブラリ&mdash;jQuery UI ライブラリには、上記のページで使用される、Datepicker ウィジェットなどのウィジェットと jQuery UI 効果がすべて含まれます。</span><span class="sxs-lookup"><span data-stu-id="26921-187">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="26921-188">JQuery UI テーマ&mdash;jQuery UI がさまざまなテーマをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="26921-188">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="26921-189">上記のページには、Redmond のテーマをインポートする CSS ファイルへのリンクが含まれています。</span><span class="sxs-lookup"><span data-stu-id="26921-189">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="26921-190">すべての標準的な jQuery UI のテーマは、CDN でホストされます。</span><span class="sxs-lookup"><span data-stu-id="26921-190">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="26921-191">[このページを参照してください。](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 Microsoft Ajax CDN")テーマごとにサムネイルを表示します。</span><span class="sxs-lookup"><span data-stu-id="26921-191">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="26921-192">詳細については、jQuery UI ライブラリ、公式を参照してください。 [jQuery UI の web サイト](http://jQueryUI.com "jQuery UI の web サイト")します。</span><span class="sxs-lookup"><span data-stu-id="26921-192">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="26921-193">Cdn のサード パーティ製ファイル</span><span class="sxs-lookup"><span data-stu-id="26921-193">Third-Party Files on the CDN</span></span>

<span data-ttu-id="26921-194">CDN は、最も一般的なサード パーティ製の JavaScript ライブラリの一部をホストします。</span><span class="sxs-lookup"><span data-stu-id="26921-194">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="26921-195">Microsoft では、この CDN でホストされているすべてのサード パーティ製ライブラリの所有権を主張しません。</span><span class="sxs-lookup"><span data-stu-id="26921-195">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="26921-196">ライブラリの著作権所有者には、これらのライブラリにはライセンス付与します。</span><span class="sxs-lookup"><span data-stu-id="26921-196">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="26921-197">権限をダウンロードしてこのようなライブラリを使用する必要がありますが、それぞれの著作権所有者によってのみ付与されます。</span><span class="sxs-lookup"><span data-stu-id="26921-197">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="26921-198">これらは Microsoft ライブラリではありません、ため、Microsoft は用意されていません保証または (暗黙の特許権を含むなし) 知的財産権ライセンスこの CDN でホストされているサード パーティのライブラリです。</span><span class="sxs-lookup"><span data-stu-id="26921-198">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="26921-199">cdn の jQuery リリース</span><span class="sxs-lookup"><span data-stu-id="26921-199">jQuery Releases on the CDN</span></span>

<span data-ttu-id="26921-200">CDN では、jQuery の次のリリースがホストされています。</span><span class="sxs-lookup"><span data-stu-id="26921-200">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-331"></a><span data-ttu-id="26921-201">jQuery バージョン 3.3.1</span><span class="sxs-lookup"><span data-stu-id="26921-201">jQuery version 3.3.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.map

#### <a name="jquery-version-321"></a><span data-ttu-id="26921-202">jQuery バージョン 3.2.1</span><span class="sxs-lookup"><span data-stu-id="26921-202">jQuery version 3.2.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map

#### <a name="jquery-version-320"></a><span data-ttu-id="26921-203">jQuery バージョン 3.2.0</span><span class="sxs-lookup"><span data-stu-id="26921-203">jQuery version 3.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map

#### <a name="jquery-version-311"></a><span data-ttu-id="26921-204">jQuery バージョン 3.1.1</span><span class="sxs-lookup"><span data-stu-id="26921-204">jQuery version 3.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map

#### <a name="jquery-version-310"></a><span data-ttu-id="26921-205">jQuery バージョン 3.1.0</span><span class="sxs-lookup"><span data-stu-id="26921-205">jQuery version 3.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map

#### <a name="jquery-version-300"></a><span data-ttu-id="26921-206">jQuery バージョン 3.0.0</span><span class="sxs-lookup"><span data-stu-id="26921-206">jQuery version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map

#### <a name="jquery-version-224"></a><span data-ttu-id="26921-207">jQuery バージョン 2.2.4</span><span class="sxs-lookup"><span data-stu-id="26921-207">jQuery version 2.2.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map

#### <a name="jquery-version-223"></a><span data-ttu-id="26921-208">jQuery バージョン 2.2.3</span><span class="sxs-lookup"><span data-stu-id="26921-208">jQuery version 2.2.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map

#### <a name="jquery-version-222"></a><span data-ttu-id="26921-209">jQuery バージョン 2.2.2</span><span class="sxs-lookup"><span data-stu-id="26921-209">jQuery version 2.2.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map

#### <a name="jquery-version-221"></a><span data-ttu-id="26921-210">jQuery バージョン 2.2.1</span><span class="sxs-lookup"><span data-stu-id="26921-210">jQuery version 2.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map

#### <a name="jquery-version-220"></a><span data-ttu-id="26921-211">jQuery バージョン 2.2.0</span><span class="sxs-lookup"><span data-stu-id="26921-211">jQuery version 2.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map

#### <a name="jquery-version-214"></a><span data-ttu-id="26921-212">jQuery バージョン 2.1.4</span><span class="sxs-lookup"><span data-stu-id="26921-212">jQuery version 2.1.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map

#### <a name="jquery-version-213"></a><span data-ttu-id="26921-213">jQuery バージョン 2.1.3</span><span class="sxs-lookup"><span data-stu-id="26921-213">jQuery version 2.1.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map

#### <a name="jquery-version-212"></a><span data-ttu-id="26921-214">jQuery バージョン 2.1.2</span><span class="sxs-lookup"><span data-stu-id="26921-214">jQuery version 2.1.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js

#### <a name="jquery-version-211"></a><span data-ttu-id="26921-215">jQuery バージョン 2.1.1</span><span class="sxs-lookup"><span data-stu-id="26921-215">jQuery version 2.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map

#### <a name="jquery-version-210"></a><span data-ttu-id="26921-216">jQuery バージョン 2.1.0</span><span class="sxs-lookup"><span data-stu-id="26921-216">jQuery version 2.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map

#### <a name="jquery-version-203"></a><span data-ttu-id="26921-217">jQuery バージョン 2.0.3</span><span class="sxs-lookup"><span data-stu-id="26921-217">jQuery version 2.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map

#### <a name="jquery-version-202"></a><span data-ttu-id="26921-218">jQuery バージョン 2.0.2</span><span class="sxs-lookup"><span data-stu-id="26921-218">jQuery version 2.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map

#### <a name="jquery-version-201"></a><span data-ttu-id="26921-219">jQuery バージョン 2.0.1</span><span class="sxs-lookup"><span data-stu-id="26921-219">jQuery version 2.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map

#### <a name="jquery-version-200"></a><span data-ttu-id="26921-220">jQuery バージョン 2.0.0</span><span class="sxs-lookup"><span data-stu-id="26921-220">jQuery version 2.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map

#### <a name="jquery-version-1124"></a><span data-ttu-id="26921-221">jQuery バージョン 1.12.4</span><span class="sxs-lookup"><span data-stu-id="26921-221">jQuery version 1.12.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map

#### <a name="jquery-version-1123"></a><span data-ttu-id="26921-222">jQuery バージョン 1.12.3</span><span class="sxs-lookup"><span data-stu-id="26921-222">jQuery version 1.12.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map

#### <a name="jquery-version-1122"></a><span data-ttu-id="26921-223">jQuery バージョン 1.12.2</span><span class="sxs-lookup"><span data-stu-id="26921-223">jQuery version 1.12.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map

#### <a name="jquery-version-1121"></a><span data-ttu-id="26921-224">jQuery バージョン 1.12.1</span><span class="sxs-lookup"><span data-stu-id="26921-224">jQuery version 1.12.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map

#### <a name="jquery-version-1120"></a><span data-ttu-id="26921-225">jQuery バージョン 1.12.0 以降</span><span class="sxs-lookup"><span data-stu-id="26921-225">jQuery version 1.12.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map

#### <a name="jquery-version-1113"></a><span data-ttu-id="26921-226">jQuery バージョン 1.11.3</span><span class="sxs-lookup"><span data-stu-id="26921-226">jQuery version 1.11.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map

#### <a name="jquery-version-1112"></a><span data-ttu-id="26921-227">jQuery バージョン 1.11.2</span><span class="sxs-lookup"><span data-stu-id="26921-227">jQuery version 1.11.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map

#### <a name="jquery-version-1111"></a><span data-ttu-id="26921-228">jQuery バージョン 1.11.1</span><span class="sxs-lookup"><span data-stu-id="26921-228">jQuery version 1.11.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map

#### <a name="jquery-version-1110"></a><span data-ttu-id="26921-229">jQuery バージョン 1.11.0</span><span class="sxs-lookup"><span data-stu-id="26921-229">jQuery version 1.11.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map

#### <a name="jquery-version-1102"></a><span data-ttu-id="26921-230">jQuery バージョン 1.10.2</span><span class="sxs-lookup"><span data-stu-id="26921-230">jQuery version 1.10.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map

#### <a name="jquery-version-1101"></a><span data-ttu-id="26921-231">jQuery バージョン 1.10.1</span><span class="sxs-lookup"><span data-stu-id="26921-231">jQuery version 1.10.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map

#### <a name="jquery-version-1100"></a><span data-ttu-id="26921-232">jQuery バージョン 1.10.0</span><span class="sxs-lookup"><span data-stu-id="26921-232">jQuery version 1.10.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map

#### <a name="jquery-version-191"></a><span data-ttu-id="26921-233">jQuery バージョン 1.9.1</span><span class="sxs-lookup"><span data-stu-id="26921-233">jQuery version 1.9.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map

#### <a name="jquery-version-190"></a><span data-ttu-id="26921-234">jQuery バージョン 1.9.0</span><span class="sxs-lookup"><span data-stu-id="26921-234">jQuery version 1.9.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map

#### <a name="jquery-version-183"></a><span data-ttu-id="26921-235">jQuery バージョン 1.8.3</span><span class="sxs-lookup"><span data-stu-id="26921-235">jQuery version 1.8.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js

#### <a name="jquery-version-182"></a><span data-ttu-id="26921-236">jQuery バージョン 1.8.2</span><span class="sxs-lookup"><span data-stu-id="26921-236">jQuery version 1.8.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js

#### <a name="jquery-version-181"></a><span data-ttu-id="26921-237">jQuery バージョン 1.8.1</span><span class="sxs-lookup"><span data-stu-id="26921-237">jQuery version 1.8.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js

#### <a name="jquery-version-180"></a><span data-ttu-id="26921-238">jQuery バージョン 1.8.0</span><span class="sxs-lookup"><span data-stu-id="26921-238">jQuery version 1.8.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js

#### <a name="jquery-version-172"></a><span data-ttu-id="26921-239">jQuery バージョンを 1.7.2</span><span class="sxs-lookup"><span data-stu-id="26921-239">jQuery version 1.7.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js

#### <a name="jquery-version-171"></a><span data-ttu-id="26921-240">jQuery バージョン 1.7.1</span><span class="sxs-lookup"><span data-stu-id="26921-240">jQuery version 1.7.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js

#### <a name="jquery-version-17"></a><span data-ttu-id="26921-241">jQuery バージョン 1.7</span><span class="sxs-lookup"><span data-stu-id="26921-241">jQuery version 1.7</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js

#### <a name="jquery-version-164"></a><span data-ttu-id="26921-242">jQuery バージョン 1.6.4</span><span class="sxs-lookup"><span data-stu-id="26921-242">jQuery version 1.6.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js

#### <a name="jquery-version-163"></a><span data-ttu-id="26921-243">jQuery バージョン 1.6.3</span><span class="sxs-lookup"><span data-stu-id="26921-243">jQuery version 1.6.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js

#### <a name="jquery-version-162"></a><span data-ttu-id="26921-244">jQuery バージョン 1.6.2</span><span class="sxs-lookup"><span data-stu-id="26921-244">jQuery version 1.6.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js

#### <a name="jquery-version-161"></a><span data-ttu-id="26921-245">jQuery バージョン 1.6.1</span><span class="sxs-lookup"><span data-stu-id="26921-245">jQuery version 1.6.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js

#### <a name="jquery-version-16"></a><span data-ttu-id="26921-246">jQuery バージョン 1.6</span><span class="sxs-lookup"><span data-stu-id="26921-246">jQuery version 1.6</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js

#### <a name="jquery-version-152"></a><span data-ttu-id="26921-247">jQuery 1.5.2 のバージョン</span><span class="sxs-lookup"><span data-stu-id="26921-247">jQuery version 1.5.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js

#### <a name="jquery-version-151"></a><span data-ttu-id="26921-248">jQuery バージョン 1.5.1</span><span class="sxs-lookup"><span data-stu-id="26921-248">jQuery version 1.5.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js

#### <a name="jquery-version-15"></a><span data-ttu-id="26921-249">jQuery バージョン 1.5</span><span class="sxs-lookup"><span data-stu-id="26921-249">jQuery version 1.5</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js

#### <a name="jquery-version-144"></a><span data-ttu-id="26921-250">jQuery バージョン 1.4.4</span><span class="sxs-lookup"><span data-stu-id="26921-250">jQuery version 1.4.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js

#### <a name="jquery-version-143"></a><span data-ttu-id="26921-251">jQuery バージョン 1.4.3</span><span class="sxs-lookup"><span data-stu-id="26921-251">jQuery version 1.4.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js

#### <a name="jquery-version-142"></a><span data-ttu-id="26921-252">jQuery バージョン 1.4.2</span><span class="sxs-lookup"><span data-stu-id="26921-252">jQuery version 1.4.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js

#### <a name="jquery-version-141"></a><span data-ttu-id="26921-253">jQuery バージョン 1.4.1</span><span class="sxs-lookup"><span data-stu-id="26921-253">jQuery version 1.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js

#### <a name="jquery-version-14"></a><span data-ttu-id="26921-254">jQuery バージョン 1.4</span><span class="sxs-lookup"><span data-stu-id="26921-254">jQuery version 1.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js

#### <a name="jquery-version-132"></a><span data-ttu-id="26921-255">jQuery バージョン 1.3.2</span><span class="sxs-lookup"><span data-stu-id="26921-255">jQuery version 1.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="26921-256">cdn の jQuery 移行リリース</span><span class="sxs-lookup"><span data-stu-id="26921-256">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="26921-257">CDN では、jQuery の移行の次のリリースがホストされています。</span><span class="sxs-lookup"><span data-stu-id="26921-257">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="26921-258">jQuery バージョン 3.0.0 の移行</span><span class="sxs-lookup"><span data-stu-id="26921-258">jQuery Migrate version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="26921-259">jQuery バージョン 1.2.1 の移行</span><span class="sxs-lookup"><span data-stu-id="26921-259">jQuery Migrate version 1.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js

<span data-ttu-id="26921-260">jQuery バージョン 1.2.0 に移行</span><span class="sxs-lookup"><span data-stu-id="26921-260">jQuery Migrate version 1.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="26921-261">jQuery バージョン 1.1.1 の移行</span><span class="sxs-lookup"><span data-stu-id="26921-261">jQuery Migrate version 1.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="26921-262">jQuery バージョン 1.1.0 の移行</span><span class="sxs-lookup"><span data-stu-id="26921-262">jQuery Migrate version 1.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="26921-263">jQuery バージョン 1.0.0 の移行</span><span class="sxs-lookup"><span data-stu-id="26921-263">jQuery Migrate version 1.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="26921-264">jQuery UI、CDN のリリース</span><span class="sxs-lookup"><span data-stu-id="26921-264">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="26921-265">この cdn の jQuery UI ライブラリは、次のリリースがホストされます。</span><span class="sxs-lookup"><span data-stu-id="26921-265">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="26921-266">ファイルの実際の一覧を表示するには、各リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="26921-266">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="26921-267">jQuery UI 1.12.1</span><span class="sxs-lookup"><span data-stu-id="26921-267">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "jQuery UI 1.12.1 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-268">jQuery UI 1.12.0</span><span class="sxs-lookup"><span data-stu-id="26921-268">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "の jQuery UI 1.12.0 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-269">jQuery UI 1.11.4</span><span class="sxs-lookup"><span data-stu-id="26921-269">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "の jQuery UI 1.11.4 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-270">jQuery UI 1.11.3</span><span class="sxs-lookup"><span data-stu-id="26921-270">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "jQuery UI 1.11.3 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-271">jQuery UI 1.11.2</span><span class="sxs-lookup"><span data-stu-id="26921-271">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "jQuery UI 1.11.2 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-272">jQuery UI 1.11.1</span><span class="sxs-lookup"><span data-stu-id="26921-272">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "の jQuery UI 1.11.1 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-273">jQuery UI 1.11.0</span><span class="sxs-lookup"><span data-stu-id="26921-273">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "の jQuery UI 1.11.0 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-274">jQuery UI 1.10.4</span><span class="sxs-lookup"><span data-stu-id="26921-274">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "jQuery UI 1.10.4 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-275">jQuery UI 1.10.3</span><span class="sxs-lookup"><span data-stu-id="26921-275">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "jQuery UI 1.10.3 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-276">jQuery UI 1.10.2</span><span class="sxs-lookup"><span data-stu-id="26921-276">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "jQuery UI 1.10.2 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-277">jQuery UI 1.10.1</span><span class="sxs-lookup"><span data-stu-id="26921-277">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "jQuery UI 1.10.1 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-278">jQuery UI 1.10.0</span><span class="sxs-lookup"><span data-stu-id="26921-278">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "jQuery UI 1.10.0 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-279">jQuery UI 1.9.2</span><span class="sxs-lookup"><span data-stu-id="26921-279">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "の jQuery UI 1.9.2 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-280">jQuery UI 1.9.1</span><span class="sxs-lookup"><span data-stu-id="26921-280">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "の jQuery UI 1.9.1 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-281">jQuery UI 1.9.0</span><span class="sxs-lookup"><span data-stu-id="26921-281">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "の jQuery UI 1.9.0 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-282">jQuery UI 1.8.24</span><span class="sxs-lookup"><span data-stu-id="26921-282">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "jQuery UI 1.8.24 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-283">jQuery UI 1.8.23</span><span class="sxs-lookup"><span data-stu-id="26921-283">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "jQuery UI 1.8.23 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-284">jQuery UI 1.8.22</span><span class="sxs-lookup"><span data-stu-id="26921-284">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "jQuery UI 1.8.22 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-285">jQuery UI 1.8.21</span><span class="sxs-lookup"><span data-stu-id="26921-285">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "jQuery UI 1.8.21 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-286">jQuery UI 1.8.20</span><span class="sxs-lookup"><span data-stu-id="26921-286">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "jQuery UI 1.8.20 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-287">jQuery UI 1.8.19</span><span class="sxs-lookup"><span data-stu-id="26921-287">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "jQuery UI 1.8.19 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-288">jQuery UI 1.8.18</span><span class="sxs-lookup"><span data-stu-id="26921-288">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "jQuery UI 1.8.18 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-289">jQuery UI 1.8.17</span><span class="sxs-lookup"><span data-stu-id="26921-289">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "jQuery UI 1.8.17 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-290">jQuery UI 1.8.16</span><span class="sxs-lookup"><span data-stu-id="26921-290">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "jQuery UI 1.8.16 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-291">jQuery UI 1.8.15</span><span class="sxs-lookup"><span data-stu-id="26921-291">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "jQuery UI 1.8.15 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-292">jQuery UI 1.8.14</span><span class="sxs-lookup"><span data-stu-id="26921-292">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "jQuery UI 1.8.14 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-293">jQuery UI 1.8.13</span><span class="sxs-lookup"><span data-stu-id="26921-293">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "jQuery UI 1.8.13 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-294">jQuery UI 1.8.12</span><span class="sxs-lookup"><span data-stu-id="26921-294">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "jQuery UI 1.8.12 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-295">jQuery UI 1.8.11</span><span class="sxs-lookup"><span data-stu-id="26921-295">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "jQuery UI 1.8.11 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-296">jQuery UI 1.8.10</span><span class="sxs-lookup"><span data-stu-id="26921-296">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-297">jQuery UI 1.8.9</span><span class="sxs-lookup"><span data-stu-id="26921-297">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "の jQuery UI 1.8.9 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-298">jQuery UI 1.8.8</span><span class="sxs-lookup"><span data-stu-id="26921-298">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "の jQuery UI 1.8.8 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-299">jQuery UI 1.8.7</span><span class="sxs-lookup"><span data-stu-id="26921-299">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "jQuery UI 1.8.7 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-300">jQuery UI 1.8.6</span><span class="sxs-lookup"><span data-stu-id="26921-300">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "jQuery UI 1.8.6 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-301">jQuery UI 1.8.5</span><span class="sxs-lookup"><span data-stu-id="26921-301">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="26921-302">jQuery の検証、CDN のリリース</span><span class="sxs-lookup"><span data-stu-id="26921-302">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="26921-303">JQuery の検証ライブラリの次のリリースは、この CDN でホストされます。</span><span class="sxs-lookup"><span data-stu-id="26921-303">The following releases of the jQuery Validation library are hosted on this CDN.</span></span> <span data-ttu-id="26921-304">ファイルの実際の一覧を表示するには、各リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="26921-304">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="26921-305">jQuery Validate 1.19.0</span><span class="sxs-lookup"><span data-stu-id="26921-305">jQuery Validate 1.19.0</span></span>](jquery-validate/cdnjqueryvalidate1190.md "jQuery Validation 1.19.0")
- [<span data-ttu-id="26921-306">jQuery Validate 1.17.0 以降</span><span class="sxs-lookup"><span data-stu-id="26921-306">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "jQuery Validation 1.17.0 以降")
- [<span data-ttu-id="26921-307">jQuery Validate 1.16.0</span><span class="sxs-lookup"><span data-stu-id="26921-307">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "jQuery Validation 1.16.0")
- [<span data-ttu-id="26921-308">jQuery Validate 1.15.1</span><span class="sxs-lookup"><span data-stu-id="26921-308">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "jQuery Validation 1.15.1")
- [<span data-ttu-id="26921-309">jQuery Validate 1.15.0</span><span class="sxs-lookup"><span data-stu-id="26921-309">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "jQuery Validation 1.15.0")
- [<span data-ttu-id="26921-310">jQuery Validate 1.14.0</span><span class="sxs-lookup"><span data-stu-id="26921-310">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "jQuery Validation 1.14.0")
- [<span data-ttu-id="26921-311">jQuery Validate 1.13.1</span><span class="sxs-lookup"><span data-stu-id="26921-311">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "jQuery Validation 1.13.1")
- [<span data-ttu-id="26921-312">jQuery Validate 1.13.0</span><span class="sxs-lookup"><span data-stu-id="26921-312">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "jQuery Validation 1.13.0")
- [<span data-ttu-id="26921-313">jQuery Validate 1.12.0</span><span class="sxs-lookup"><span data-stu-id="26921-313">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "jQuery Validation 1.12.0")
- [<span data-ttu-id="26921-314">jQuery Validate 1.11.1</span><span class="sxs-lookup"><span data-stu-id="26921-314">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "jQuery Validation 1.11.1")
- [<span data-ttu-id="26921-315">jQuery Validate 1.11.0</span><span class="sxs-lookup"><span data-stu-id="26921-315">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "jQuery Validation 1.11.0")
- [<span data-ttu-id="26921-316">jQuery Validate 1.10.0</span><span class="sxs-lookup"><span data-stu-id="26921-316">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "jQuery Validation 1.10.0")
- [<span data-ttu-id="26921-317">jQuery Validate 1.9</span><span class="sxs-lookup"><span data-stu-id="26921-317">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jquery.validate バージョン 1.9")
- [<span data-ttu-id="26921-318">jQuery Validate 1.8.1</span><span class="sxs-lookup"><span data-stu-id="26921-318">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "jquery.validate バージョン 1.8.1")
- [<span data-ttu-id="26921-319">jQuery Validate 1.8</span><span class="sxs-lookup"><span data-stu-id="26921-319">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jquery.validate バージョン 1.8")
- [<span data-ttu-id="26921-320">jQuery Validate 1.7</span><span class="sxs-lookup"><span data-stu-id="26921-320">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jquery.validate バージョン 1.7")
- [<span data-ttu-id="26921-321">jQuery Validate 1.6</span><span class="sxs-lookup"><span data-stu-id="26921-321">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "jQuery Validate 1.6")
- [<span data-ttu-id="26921-322">jQuery Validate 1.5.5</span><span class="sxs-lookup"><span data-stu-id="26921-322">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "jQuery Validate 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="26921-323">jQuery Mobile、CDN のリリース</span><span class="sxs-lookup"><span data-stu-id="26921-323">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="26921-324">この CDN の jQuery Mobile ライブラリの次のリリースがホストされます。</span><span class="sxs-lookup"><span data-stu-id="26921-324">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="26921-325">ファイルの実際の一覧を表示するには、各リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="26921-325">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="26921-326">jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="26921-326">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "の jQuery Mobile 1.4.5 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-327">jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="26921-327">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "の jQuery Mobile 1.4.2 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-328">jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="26921-328">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "の jQuery Mobile 1.4.1 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-329">jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="26921-329">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "の jQuery Mobile 1.4.0 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-330">jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="26921-330">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "の jQuery Mobile 1.3.2 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-331">jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="26921-331">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "の jQuery Mobile 1.3.1 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-332">jQuery Mobile 1.3.0</span><span class="sxs-lookup"><span data-stu-id="26921-332">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "の jQuery Mobile 1.3.0 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-333">jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="26921-333">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "の jQuery Mobile 1.2.0 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-334">jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="26921-334">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "の jQuery Mobile 1.1.2 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-335">jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="26921-335">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "の jQuery Mobile 1.1.1、Microsoft Ajax CDN")
- [<span data-ttu-id="26921-336">jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="26921-336">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "の jQuery Mobile 1.1.0 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-337">jQuery Mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="26921-337">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "の jQuery Mobile 1.1.0 RC2 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-338">jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="26921-338">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "の jQuery Mobile 1.0.1 Microsoft Ajax CDN")
- [<span data-ttu-id="26921-339">jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="26921-339">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "Microsoft Ajax CDN の jQuery Mobile 1.0")
- [<span data-ttu-id="26921-340">jQuery Mobile 1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="26921-340">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "Microsoft Ajax CDN の jQuery Mobile 1.0 RC2")
- [<span data-ttu-id="26921-341">jQuery Mobile 1.0 RC 1</span><span class="sxs-lookup"><span data-stu-id="26921-341">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "Microsoft Ajax CDN の jQuery Mobile 1.0 RC1")
- [<span data-ttu-id="26921-342">jQuery Mobile 1.0 beta 3</span><span class="sxs-lookup"><span data-stu-id="26921-342">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "Microsoft Ajax CDN の jQuery Mobile 1.0 Beta 3")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="26921-343">jQuery cdn テンプレート リリース</span><span class="sxs-lookup"><span data-stu-id="26921-343">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="26921-344">この cdn の jQuery テンプレートのプラグインは、次のリリースがホストされます。</span><span class="sxs-lookup"><span data-stu-id="26921-344">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="26921-345">ファイルの実際の一覧を表示するには、各リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="26921-345">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="26921-346">jQuery テンプレート Beta 1</span><span class="sxs-lookup"><span data-stu-id="26921-346">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery テンプレート Beta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="26921-347">jQuery、CDN のリリース サイクル</span><span class="sxs-lookup"><span data-stu-id="26921-347">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="26921-348">次のリリース サイクルの jQuery プラグインは、この CDN でホストされます。</span><span class="sxs-lookup"><span data-stu-id="26921-348">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="26921-349">ファイルの実際の一覧を表示するには、各リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="26921-349">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="26921-350">jQuery Cycle 2.99</span><span class="sxs-lookup"><span data-stu-id="26921-350">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery Cycle 2.99")
- [<span data-ttu-id="26921-351">jQuery Cycle 2.94</span><span class="sxs-lookup"><span data-stu-id="26921-351">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery Cycle 2.94")
- [<span data-ttu-id="26921-352">jQuery Cycle 2.88</span><span class="sxs-lookup"><span data-stu-id="26921-352">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery Cycle 2.88")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="26921-353">jQuery、CDN での Datatable のリリース</span><span class="sxs-lookup"><span data-stu-id="26921-353">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="26921-354">Datatable の jQuery プラグインの次のリリースは、この CDN でホストされます。</span><span class="sxs-lookup"><span data-stu-id="26921-354">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="26921-355">ファイルの実際の一覧を表示するには、各リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="26921-355">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="26921-356">jQuery Datatable 1.10.5</span><span class="sxs-lookup"><span data-stu-id="26921-356">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery Datatable 1.10.5")
- [<span data-ttu-id="26921-357">jQuery Datatable 1.10.4</span><span class="sxs-lookup"><span data-stu-id="26921-357">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery Datatable 1.10.4")
- [<span data-ttu-id="26921-358">jQuery Datatable 1.9.4</span><span class="sxs-lookup"><span data-stu-id="26921-358">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery Datatable 1.9.4")
- [<span data-ttu-id="26921-359">jQuery Datatable 1.9.3</span><span class="sxs-lookup"><span data-stu-id="26921-359">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery Datatable 1.9.3")
- [<span data-ttu-id="26921-360">jQuery Datatable 1.9.2</span><span class="sxs-lookup"><span data-stu-id="26921-360">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery Datatable 1.9.2")
- [<span data-ttu-id="26921-361">jQuery 1.9.1 Datatable</span><span class="sxs-lookup"><span data-stu-id="26921-361">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery 1.9.1 Datatable")
- [<span data-ttu-id="26921-362">jQuery Datatable 1.9.0</span><span class="sxs-lookup"><span data-stu-id="26921-362">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery Datatable 1.9.0")
- [<span data-ttu-id="26921-363">jQuery Datatable 1.8.2</span><span class="sxs-lookup"><span data-stu-id="26921-363">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery Datatable 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="26921-364">Cdn Modernizr リリース</span><span class="sxs-lookup"><span data-stu-id="26921-364">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="26921-365">次のリリースの[Modernizr](http://www.modernizr.com "Modernizr") CDN でホストされています。</span><span class="sxs-lookup"><span data-stu-id="26921-365">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-3.5.0.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="26921-366">CDN で JSHint リリース</span><span class="sxs-lookup"><span data-stu-id="26921-366">JSHint Releases on the CDN</span></span>

<span data-ttu-id="26921-367">次のリリースの[JSHint](http://www.jshint.com "JSHint") CDN にホストされます。</span><span class="sxs-lookup"><span data-stu-id="26921-367">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="26921-368">Cdn knockout リリース</span><span class="sxs-lookup"><span data-stu-id="26921-368">Knockout Releases on the CDN</span></span>

<span data-ttu-id="26921-369">次のリリースの[Knockout](http://www.knockoutjs.com "Knockout") CDN でホストされています。</span><span class="sxs-lookup"><span data-stu-id="26921-369">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.5.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.5.0.debug.js

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="26921-370">CDN でリリースをグローバル化します。</span><span class="sxs-lookup"><span data-stu-id="26921-370">Globalize Releases on the CDN</span></span>

<span data-ttu-id="26921-371">次のリリースの[Globalize](https://github.com/jquery/globalize "Globalize") CDN にホストされます。</span><span class="sxs-lookup"><span data-stu-id="26921-371">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="26921-372">バージョン 1.0.0 のグローバル化します。</span><span class="sxs-lookup"><span data-stu-id="26921-372">Globalize version 1.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js

#### <a name="globalize-version-011"></a><span data-ttu-id="26921-373">バージョンは 0.1.1 をグローバル化します。</span><span class="sxs-lookup"><span data-stu-id="26921-373">Globalize version 0.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js

    - <span data-ttu-id="26921-374">すべてのカルチャ</span><span class="sxs-lookup"><span data-stu-id="26921-374">all cultures</span></span>
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js

    - <span data-ttu-id="26921-375">「{カルチャ コード}」を目的のカルチャ コードに置き換えます、cdn の globalize.culture.en GB.js== Microsoft のファイルなど、これらを = = ライブラリは、Microsoft によってアップロードされました。</span><span class="sxs-lookup"><span data-stu-id="26921-375">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="26921-376">Cdn のリリースを応答します。</span><span class="sxs-lookup"><span data-stu-id="26921-376">Respond Releases on the CDN</span></span>

<span data-ttu-id="26921-377">次のリリースの [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") 応答が CDN にホストされています。</span><span class="sxs-lookup"><span data-stu-id="26921-377">The following releases of [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") Respond are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="26921-378">応答バージョン 1.4.2</span><span class="sxs-lookup"><span data-stu-id="26921-378">Respond version 1.4.2</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js

#### <a name="respond-version-141"></a><span data-ttu-id="26921-379">応答バージョン 1.4.1</span><span class="sxs-lookup"><span data-stu-id="26921-379">Respond version 1.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js

#### <a name="respond-version-140"></a><span data-ttu-id="26921-380">応答バージョン 1.4.0</span><span class="sxs-lookup"><span data-stu-id="26921-380">Respond version 1.4.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js

#### <a name="respond-version-130"></a><span data-ttu-id="26921-381">応答バージョン 1.3.0</span><span class="sxs-lookup"><span data-stu-id="26921-381">Respond version 1.3.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js

#### <a name="respond-version-120"></a><span data-ttu-id="26921-382">応答バージョン 1.2.0</span><span class="sxs-lookup"><span data-stu-id="26921-382">Respond version 1.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="26921-383">Cdn のブートス トラップのリリース</span><span class="sxs-lookup"><span data-stu-id="26921-383">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="26921-384">次のリリースの[getbootstrap.com](http://getbootstrap.com "getbootstrap.com")ブートス トラップが CDN にホストされています。</span><span class="sxs-lookup"><span data-stu-id="26921-384">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-421"></a><span data-ttu-id="26921-385">ブートス トラップ バージョン 4.2.1</span><span class="sxs-lookup"><span data-stu-id="26921-385">Bootstrap version 4.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-411"></a><span data-ttu-id="26921-386">ブートス トラップのバージョン 4.1.1</span><span class="sxs-lookup"><span data-stu-id="26921-386">Bootstrap version 4.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-400"></a><span data-ttu-id="26921-387">ブートス トラップ バージョン 4.0.0</span><span class="sxs-lookup"><span data-stu-id="26921-387">Bootstrap version 4.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-340"></a><span data-ttu-id="26921-388">ブートス トラップ バージョン 3.4.0</span><span class="sxs-lookup"><span data-stu-id="26921-388">Bootstrap version 3.4.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-337"></a><span data-ttu-id="26921-389">ブートス トラップ バージョン 3.3.7</span><span class="sxs-lookup"><span data-stu-id="26921-389">Bootstrap version 3.3.7</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-336"></a><span data-ttu-id="26921-390">ブートス トラップ バージョン 3.3.6</span><span class="sxs-lookup"><span data-stu-id="26921-390">Bootstrap version 3.3.6</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-335"></a><span data-ttu-id="26921-391">バージョン 3.3.5 をブートス トラップします。</span><span class="sxs-lookup"><span data-stu-id="26921-391">Bootstrap version 3.3.5</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-334"></a><span data-ttu-id="26921-392">ブートス トラップ バージョン 3.3.4</span><span class="sxs-lookup"><span data-stu-id="26921-392">Bootstrap version 3.3.4</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-332"></a><span data-ttu-id="26921-393">ブートス トラップ バージョン 3.3.2</span><span class="sxs-lookup"><span data-stu-id="26921-393">Bootstrap version 3.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-331"></a><span data-ttu-id="26921-394">ブートス トラップ バージョン 3.3.1</span><span class="sxs-lookup"><span data-stu-id="26921-394">Bootstrap version 3.3.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-330"></a><span data-ttu-id="26921-395">ブートス トラップ バージョン 3.3.0</span><span class="sxs-lookup"><span data-stu-id="26921-395">Bootstrap version 3.3.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-320"></a><span data-ttu-id="26921-396">ブートス トラップ バージョン 3.2.0</span><span class="sxs-lookup"><span data-stu-id="26921-396">Bootstrap version 3.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-311"></a><span data-ttu-id="26921-397">3.1.1 ブートス トラップのバージョン</span><span class="sxs-lookup"><span data-stu-id="26921-397">Bootstrap version 3.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-310"></a><span data-ttu-id="26921-398">バージョン 3.1.0 のブートス トラップします。</span><span class="sxs-lookup"><span data-stu-id="26921-398">Bootstrap version 3.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-303"></a><span data-ttu-id="26921-399">ブートス トラップ バージョン 3.0.3</span><span class="sxs-lookup"><span data-stu-id="26921-399">Bootstrap version 3.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-302"></a><span data-ttu-id="26921-400">ブートス トラップ バージョン 3.0.2</span><span class="sxs-lookup"><span data-stu-id="26921-400">Bootstrap version 3.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-301"></a><span data-ttu-id="26921-401">バージョン 3.0.1 をブートス トラップします。</span><span class="sxs-lookup"><span data-stu-id="26921-401">Bootstrap version 3.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-300"></a><span data-ttu-id="26921-402">バージョン 3.0.0 のブートス トラップします。</span><span class="sxs-lookup"><span data-stu-id="26921-402">Bootstrap version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-232"></a><span data-ttu-id="26921-403">バージョン 2.3.2 のブートス トラップします。</span><span class="sxs-lookup"><span data-stu-id="26921-403">Bootstrap version 2.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png

#### <a name="bootstrap-version-231"></a><span data-ttu-id="26921-404">Version 2.3.1 のブートス トラップします。</span><span class="sxs-lookup"><span data-stu-id="26921-404">Bootstrap version 2.3.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="26921-405">Cdn のブートス トラップ TouchCarousel リリース</span><span class="sxs-lookup"><span data-stu-id="26921-405">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="26921-406">次のリリースの[https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel")ブートス トラップ TouchCarousel リリースは、CDN でホストされています。</span><span class="sxs-lookup"><span data-stu-id="26921-406">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="26921-407">ブートス トラップ TouchCarousel バージョン 0.8.0 以降</span><span class="sxs-lookup"><span data-stu-id="26921-407">Bootstrap TouchCarousel version 0.8.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css
- https://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="26921-408">Cdn Hammer.js リリース</span><span class="sxs-lookup"><span data-stu-id="26921-408">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="26921-409">次のリリースの[http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js リリースは、CDN でホストされています。</span><span class="sxs-lookup"><span data-stu-id="26921-409">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="26921-410">Hammer.js バージョン 2.0.4</span><span class="sxs-lookup"><span data-stu-id="26921-410">Hammer.js version 2.0.4</span></span>

- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js
- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js
- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="26921-411">ASP.NET Web フォームと Ajax CDN のリリース</span><span class="sxs-lookup"><span data-stu-id="26921-411">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="26921-412">ASP.NET Ajax ライブラリの次のリリースは、CDN でホストされます。</span><span class="sxs-lookup"><span data-stu-id="26921-412">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="26921-413">ファイルの実際の一覧を表示するには、各リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="26921-413">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="26921-414">ASP.NET Web フォームと Ajax バージョン 4.5.2</span><span class="sxs-lookup"><span data-stu-id="26921-414">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "ASP.NET Web フォームと Ajax 4.5.2")
- [<span data-ttu-id="26921-415">ASP.NET Web フォームと Ajax バージョン 4</span><span class="sxs-lookup"><span data-stu-id="26921-415">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "ASP.NET Web フォームと Ajax 4")
- [<span data-ttu-id="26921-416">ASP.NET Ajax 3.5</span><span class="sxs-lookup"><span data-stu-id="26921-416">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "ASP.NET Ajax 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="26921-417">ASP.NET MVC は、CDN にリリースします。</span><span class="sxs-lookup"><span data-stu-id="26921-417">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="26921-418">次の ASP.NET MVC の JavaScript ファイルは、この CDN にホストされます。</span><span class="sxs-lookup"><span data-stu-id="26921-418">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="26921-419">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="26921-419">ASP.NET MVC 5.2.3</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="26921-420">ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="26921-420">ASP.NET MVC 5.1</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="26921-421">ASP.NET MVC 5.0</span><span class="sxs-lookup"><span data-stu-id="26921-421">ASP.NET MVC 5.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="26921-422">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="26921-422">ASP.NET MVC 4.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="26921-423">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="26921-423">ASP.NET MVC 3.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js
- https://ajax.aspnetcdn.com/ajax/jquery.unobtrusive-ajax/3.2.5/jquery.unobtrusive-ajax.js
- https://ajax.aspnetcdn.com/ajax/jquery.unobtrusive-ajax/3.2.5/jquery.unobtrusive-ajax.min.js 
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js
- https://ajax.aspnetcdn.com/ajax/jquery.validation.unobtrusive/3.2.10/jquery.validate.unobtrusive.js 
- https://ajax.aspnetcdn.com/ajax/jquery.validation.unobtrusive/3.2.10/jquery.validate.unobtrusive.min.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="26921-424">ASP.NET MVC 2.0</span><span class="sxs-lookup"><span data-stu-id="26921-424">ASP.NET MVC 2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="26921-425">ASP.NET MVC 1.0</span><span class="sxs-lookup"><span data-stu-id="26921-425">ASP.NET MVC 1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="26921-426">ASP.NET SignalR が CDN にリリースします。</span><span class="sxs-lookup"><span data-stu-id="26921-426">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="26921-427">次の ASP.NET SignalR JavaScript ファイルは、この CDN にホストされます。</span><span class="sxs-lookup"><span data-stu-id="26921-427">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="26921-428">ASP.NET SignalR 2.2.2</span><span class="sxs-lookup"><span data-stu-id="26921-428">ASP.NET SignalR 2.2.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="26921-429">ASP.NET SignalR 2.2.1</span><span class="sxs-lookup"><span data-stu-id="26921-429">ASP.NET SignalR 2.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="26921-430">ASP.NET SignalR 2.2.0</span><span class="sxs-lookup"><span data-stu-id="26921-430">ASP.NET SignalR 2.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="26921-431">ASP.NET SignalR 2.1.0</span><span class="sxs-lookup"><span data-stu-id="26921-431">ASP.NET SignalR 2.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="26921-432">ASP.NET SignalR 2.0.3</span><span class="sxs-lookup"><span data-stu-id="26921-432">ASP.NET SignalR 2.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="26921-433">ASP.NET SignalR 2.0.2</span><span class="sxs-lookup"><span data-stu-id="26921-433">ASP.NET SignalR 2.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="26921-434">ASP.NET SignalR 2.0.1</span><span class="sxs-lookup"><span data-stu-id="26921-434">ASP.NET SignalR 2.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="26921-435">ASP.NET SignalR 2.0.0</span><span class="sxs-lookup"><span data-stu-id="26921-435">ASP.NET SignalR 2.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="26921-436">ASP.NET SignalR 1.1.3</span><span class="sxs-lookup"><span data-stu-id="26921-436">ASP.NET SignalR 1.1.3</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="26921-437">ASP.NET SignalR 1.1.2</span><span class="sxs-lookup"><span data-stu-id="26921-437">ASP.NET SignalR 1.1.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="26921-438">ASP.NET SignalR 1.1.1</span><span class="sxs-lookup"><span data-stu-id="26921-438">ASP.NET SignalR 1.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="26921-439">ASP.NET SignalR 1.1.0</span><span class="sxs-lookup"><span data-stu-id="26921-439">ASP.NET SignalR 1.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="26921-440">ASP.NET SignalR 1.0.1</span><span class="sxs-lookup"><span data-stu-id="26921-440">ASP.NET SignalR 1.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js

<span data-ttu-id="26921-441">CDN の使用条件については、次を参照してください。 [Microsoft Ajax CDN の使用条件](https://www.asp.net/terms-of-use "Microsoft Ajax CDN の使用条件")します。</span><span class="sxs-lookup"><span data-stu-id="26921-441">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>
