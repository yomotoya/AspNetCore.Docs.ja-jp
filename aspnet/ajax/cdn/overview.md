---
uid: ajax/cdn/overview
title: "Microsoft Ajax コンテンツ配信ネットワーク |Microsoft ドキュメント"
author: rick-anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/14/2017
ms.topic: article
ms.assetid: 8935bf14-ca6d-4a4e-9dbe-b96ce74cef49
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /ajax/cdn
msc.type: content
ms.openlocfilehash: f69f707ba64d13fc372b7bc44718c9dcf8cec6e2
ms.sourcegitcommit: 3f491f887074310fc0f145cd01a670aa63b969e3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/22/2018
---
<a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="630a7-102">Microsoft Ajax コンテンツ配信ネットワーク</span><span class="sxs-lookup"><span data-stu-id="630a7-102">Microsoft Ajax Content Delivery Network</span></span>
====================
<span data-ttu-id="630a7-103">注: Microsoft Ajax CDN を持たない SLA の Azure CDN を使用する他にします。</span><span class="sxs-lookup"><span data-stu-id="630a7-103">Note: The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="630a7-104">目次</span><span class="sxs-lookup"><span data-stu-id="630a7-104">Table of Contents</span></span>

<span data-ttu-id="630a7-105">**[ajax.microsoft.com ajax.aspnetcdn.com に変更されました](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="630a7-105">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="630a7-106">**[Visual Studio .vsdoc サポート](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="630a7-106">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="630a7-107">**[CDN から ASP.NET Ajax を使用します。](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="630a7-107">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="630a7-108">**[CDN から jQuery の使用](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="630a7-108">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="630a7-109">**[JQuery UI、CDN からの使用](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="630a7-109">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="630a7-110">**[CDN でサード パーティ製ファイル](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="630a7-110">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="630a7-111">CDN の jQuery リリース</span><span class="sxs-lookup"><span data-stu-id="630a7-111">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="630a7-112">CDN の jQuery 移行リリース</span><span class="sxs-lookup"><span data-stu-id="630a7-112">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="630a7-113">jQuery UI、CDN のリリース</span><span class="sxs-lookup"><span data-stu-id="630a7-113">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="630a7-114">jQuery 検証 CDN のリリース</span><span class="sxs-lookup"><span data-stu-id="630a7-114">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="630a7-115">jQuery Mobile、CDN のリリース</span><span class="sxs-lookup"><span data-stu-id="630a7-115">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="630a7-116">jQuery CDN でテンプレートのリリース</span><span class="sxs-lookup"><span data-stu-id="630a7-116">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="630a7-117">jQuery CDN のサイクル リリース</span><span class="sxs-lookup"><span data-stu-id="630a7-117">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="630a7-118">jQuery CDN の Datatable リリース</span><span class="sxs-lookup"><span data-stu-id="630a7-118">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="630a7-119">CDN の Modernizr リリース</span><span class="sxs-lookup"><span data-stu-id="630a7-119">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="630a7-120">CDN の JSHint リリース</span><span class="sxs-lookup"><span data-stu-id="630a7-120">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="630a7-121">CDN の knockout リリース</span><span class="sxs-lookup"><span data-stu-id="630a7-121">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="630a7-122">CDN のリリースをグローバル化します。</span><span class="sxs-lookup"><span data-stu-id="630a7-122">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="630a7-123">CDN のリリースを応答します。</span><span class="sxs-lookup"><span data-stu-id="630a7-123">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="630a7-124">CDN でブートス トラップのリリース</span><span class="sxs-lookup"><span data-stu-id="630a7-124">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="630a7-125">CDN のブートス トラップ TouchCarousel リリース</span><span class="sxs-lookup"><span data-stu-id="630a7-125">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="630a7-126">CDN の Hammer.js リリース</span><span class="sxs-lookup"><span data-stu-id="630a7-126">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="630a7-127">ASP.NET Web フォームと Ajax CDN のリリース</span><span class="sxs-lookup"><span data-stu-id="630a7-127">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="630a7-128">ASP.NET MVC が CDN でリリースします。</span><span class="sxs-lookup"><span data-stu-id="630a7-128">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="630a7-129">ASP.NET SignalR を CDN で解放します。</span><span class="sxs-lookup"><span data-stu-id="630a7-129">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="630a7-130">Microsoft Ajax コンテンツ配信ネットワーク (CDN) では、jQuery などのサード パーティ製の一般的な JavaScript ライブラリをホストし、それらを Web アプリケーションに簡単に追加することができます。</span><span class="sxs-lookup"><span data-stu-id="630a7-130">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="630a7-131">この CDN に追加するだけでホストされている jQuery の使用を開始するなど、&lt;スクリプト&gt;ajax.aspnetcdn.com を指すページへのタグ。</span><span class="sxs-lookup"><span data-stu-id="630a7-131">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="630a7-132">CDN を利用して、Ajax アプリケーションのパフォーマンスを大幅に向上させることができます。</span><span class="sxs-lookup"><span data-stu-id="630a7-132">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="630a7-133">CDN の内容は、世界各地に配置されたサーバーでキャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="630a7-133">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="630a7-134">さらに、CDN は、別のドメインに配置されている web サイトのキャッシュされたサード パーティ製の JavaScript ファイルを再利用するブラウザーを使用できます。</span><span class="sxs-lookup"><span data-stu-id="630a7-134">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="630a7-135">CDN は、Secure Sockets Layer を使用し、web ページを使用する必要がある場合に、SSL (HTTPS) をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="630a7-135">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="630a7-136">CDN では、アップロードされていると、これらのライブラリの所有者が、お客様にライセンス供与は、次のサード パーティ スクリプト ライブラリをホストします。</span><span class="sxs-lookup"><span data-stu-id="630a7-136">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="630a7-137">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="630a7-137">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="630a7-138">jQuery UI (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="630a7-138">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="630a7-139">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="630a7-139">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="630a7-140">jQuery 検証 (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="630a7-140">jQuery Validation (www.jquery.com)</span></span>
- <span data-ttu-id="630a7-141">jQuery サイクル (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="630a7-141">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="630a7-142">jQuery Datatable (http://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="630a7-142">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="630a7-143">Microsoft Ajax CDN には、Microsoft によってアップロードされた次のライブラリが含まれています。</span><span class="sxs-lookup"><span data-stu-id="630a7-143">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="630a7-144">ASP.NET Ajax</span><span class="sxs-lookup"><span data-stu-id="630a7-144">ASP.NET Ajax</span></span>
- <span data-ttu-id="630a7-145">ASP.NET MVC の JavaScript ファイル</span><span class="sxs-lookup"><span data-stu-id="630a7-145">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="630a7-146">ASP.NET SignalR JavaScript ファイルします。</span><span class="sxs-lookup"><span data-stu-id="630a7-146">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="630a7-147">Microsoft では、この CDN でホストされているすべてのサード パーティ ライブラリの所有権を主張しません。</span><span class="sxs-lookup"><span data-stu-id="630a7-147">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="630a7-148">ライブラリの著作権の所有者は、これらのライブラリをライセンスされています。</span><span class="sxs-lookup"><span data-stu-id="630a7-148">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="630a7-149">それぞれの著作権の所有者によってのみをダウンロードしてこのようなライブラリを使用する必要のある任意の権限が付与されます。</span><span class="sxs-lookup"><span data-stu-id="630a7-149">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="630a7-150">これらは Microsoft ライブラリではありません、ため Microsoft でない保証または (暗黙の特許権を含むなし) 知的財産権ライセンスをこの CDN でホストされているサードパーティ製ライブラリ。</span><span class="sxs-lookup"><span data-stu-id="630a7-150">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="630a7-151">かどうか、JavaScript ライブラリを送信して、ライブラリは最上位の JavaScript ライブラリ (に従って http://trends.builtwith.com) または拡張機能/プラグインは、これらのライブラリへの 1 つ (a) 人気のあります。または (b) する場合に便利で ASP.NET を使用しにお問い合わせくださいAjaxCDNSubmission@Microsoft.comです。</span><span class="sxs-lookup"><span data-stu-id="630a7-151">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="630a7-152">ajax.microsoft.com ajax.aspnetcdn.com に変更されました</span><span class="sxs-lookup"><span data-stu-id="630a7-152">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="630a7-153">CDN では、microsoft.com ドメイン名を使用するために使用し、aspnetcdn.com ドメイン名の使用に変更されました。</span><span class="sxs-lookup"><span data-stu-id="630a7-153">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="630a7-154">ブラウザーには、microsoft.com ドメインが参照されている場合に送信可能なため、クッキーそのドメインから要求ごとにネットワーク経由でパフォーマンスを向上させる、この変更が加えられました。</span><span class="sxs-lookup"><span data-stu-id="630a7-154">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="630a7-155">Microsoft.com 以外のドメイン名を変更するとパフォーマンスを向上できますで 25% ほどです。</span><span class="sxs-lookup"><span data-stu-id="630a7-155">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="630a7-156">注 ajax.microsoft.com は引き続き機能しますが、ajax.aspnetcdn.com をお勧めします。</span><span class="sxs-lookup"><span data-stu-id="630a7-156">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="630a7-157">古い形式: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="630a7-157">Old Format: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="630a7-158">新しい形式: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="630a7-158">New Format: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="630a7-159">Visual Studio .vsdoc サポート</span><span class="sxs-lookup"><span data-stu-id="630a7-159">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="630a7-160">VS 2008 SP1 があることを確認する必要があります。 Visual Studio 2008 に .vsdoc ファイルを適切に使用するインストールし、vsdoc ファイル用の修正プログラムがインストールされています。</span><span class="sxs-lookup"><span data-stu-id="630a7-160">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="630a7-161">ここではこれらを取得できます。</span><span class="sxs-lookup"><span data-stu-id="630a7-161">You can get these from here:</span></span>

- [<span data-ttu-id="630a7-162">Visual Studio 2008 SP1 をダウンロード</span><span class="sxs-lookup"><span data-stu-id="630a7-162">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "Visual Studio 2008 SP1 をダウンロードします。")
- [<span data-ttu-id="630a7-163">Visual Studio 2008 SP1 の .vsdoc 修正プログラムをダウンロード</span><span class="sxs-lookup"><span data-stu-id="630a7-163">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 ".vsdoc 修正プログラムを Visual Studio 2008 SP1 のダウンロード")

<span data-ttu-id="630a7-164">Visual Studio 2010 では、追加、更新プログラムなし .vsdoc ファイルをサポートします。</span><span class="sxs-lookup"><span data-stu-id="630a7-164">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="630a7-165">CDN から ASP.NET Ajax を使用します。</span><span class="sxs-lookup"><span data-stu-id="630a7-165">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="630a7-166">ASP.NET 4 を使用する場合は、ASP.NET framework スクリプトに対するすべての要求を CDN にリダイレクトできます。</span><span class="sxs-lookup"><span data-stu-id="630a7-166">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="630a7-167">ローカル web サーバーではなく、CDN からスクリプトを取得すると、パブリックの ASP.NET web サイトのパフォーマンスが大幅に向上します。</span><span class="sxs-lookup"><span data-stu-id="630a7-167">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="630a7-168">Microsoft Ajax CDN を ASP.NET framework スクリプトのすべての要求をリダイレクトするためには、ScriptManager EnableCDN プロパティを使用します。</span><span class="sxs-lookup"><span data-stu-id="630a7-168">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="630a7-169">CDN から jQuery の使用</span><span class="sxs-lookup"><span data-stu-id="630a7-169">Using jQuery from the CDN</span></span>

<span data-ttu-id="630a7-170">ページに次のスクリプト要素を追加することで、Web アプリケーション CDN でホストされている jQuery スクリプトを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="630a7-170">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="630a7-171">CDN は、入手できる jQuery スクリプトの縮小されたバージョンも含まれています。 次の要素を使用します。</span><span class="sxs-lookup"><span data-stu-id="630a7-171">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="630a7-172">フォールバック CDN が使用できない場合、独自の web サイト上のローカル パスからの jQuery の読み込みを使用してページを許可するのには、CDN を参照する要素の直後後次の要素を追加します。</span><span class="sxs-lookup"><span data-stu-id="630a7-172">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="630a7-173">次のサンプルのページは、ボタンがクリックされたときに、div 要素の内容を表示する (ローカル コピーへのフォールバック) とともに jQuery ライブラリの CDN バージョンを使用します。</span><span class="sxs-lookup"><span data-stu-id="630a7-173">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="630a7-174">詳細については、jQuery およびにアクセスして jQuery のローカル コピーをダウンロードすることができます、 [jQuery](http://jquery.com/) Web サイトです。</span><span class="sxs-lookup"><span data-stu-id="630a7-174">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="630a7-175">JQuery UI、CDN からの使用</span><span class="sxs-lookup"><span data-stu-id="630a7-175">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="630a7-176">CDN では、jQuery UI ライブラリもホストします。</span><span class="sxs-lookup"><span data-stu-id="630a7-176">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="630a7-177">JQuery UI ライブラリには、ウィジェットと、ASP.NET アプリケーションで使用できる特殊効果の豊富なセットが含まれています。</span><span class="sxs-lookup"><span data-stu-id="630a7-177">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="630a7-178">たとえば、次のページは、ポップアップ カレンダーを表示する ASP.NET Web フォーム アプリケーションのコンテキストで jQuery UI Datepicker を使用する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="630a7-178">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="630a7-179">キーボードを使用してテキスト ボックスにフォーカスを移動すると、カレンダーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="630a7-179">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![作成 Datepicker ポップアップ カレンダー](overview/_static/image1.png)

<span data-ttu-id="630a7-181">上記のコードで、CDN から 3 つのファイルを含める必要があることを確認します。</span><span class="sxs-lookup"><span data-stu-id="630a7-181">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="630a7-182">JQuery ライブラリ&mdash;jQuery UI ライブラリは、jQuery ライブラリに依存します。</span><span class="sxs-lookup"><span data-stu-id="630a7-182">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="630a7-183">JQuery UI ライブラリを追加する前に、ページに jQuery ライブラリを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="630a7-183">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="630a7-184">JQuery UI ライブラリ&mdash;jQuery UI ライブラリには、すべてのページの上部で使用される Datepicker ウィジェットなどのウィジェットと jQuery UI 効果が含まれています。</span><span class="sxs-lookup"><span data-stu-id="630a7-184">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="630a7-185">JQuery UI のテーマ&mdash;jQuery UI には、さまざまなテーマがサポートしています。</span><span class="sxs-lookup"><span data-stu-id="630a7-185">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="630a7-186">ページ上部にはには、Redmond のテーマをインポートする CSS ファイルへのリンクが含まれています。</span><span class="sxs-lookup"><span data-stu-id="630a7-186">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="630a7-187">すべての標準的な jQuery UI のテーマは、CDN でホストされます。</span><span class="sxs-lookup"><span data-stu-id="630a7-187">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="630a7-188">[このページを参照してください](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 Microsoft Ajax CDN の")各テーマのサムネイルを表示します。</span><span class="sxs-lookup"><span data-stu-id="630a7-188">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="630a7-189">JQuery UI ライブラリに関する詳細については、公式にアクセスして[jQuery UI の web サイト](http://jQueryUI.com "jQuery UI の web サイト")です。</span><span class="sxs-lookup"><span data-stu-id="630a7-189">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="630a7-190">CDN でサード パーティ製ファイル</span><span class="sxs-lookup"><span data-stu-id="630a7-190">Third-Party Files on the CDN</span></span>

<span data-ttu-id="630a7-191">CDN では、最も一般的なサード パーティ製の JavaScript ライブラリの一部をホストします。</span><span class="sxs-lookup"><span data-stu-id="630a7-191">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="630a7-192">Microsoft では、この CDN でホストされているすべてのサード パーティ ライブラリの所有権を主張しません。</span><span class="sxs-lookup"><span data-stu-id="630a7-192">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="630a7-193">ライブラリの著作権の所有者は、これらのライブラリをライセンスされています。</span><span class="sxs-lookup"><span data-stu-id="630a7-193">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="630a7-194">それぞれの著作権の所有者によってのみをダウンロードしてこのようなライブラリを使用する必要のある任意の権限が付与されます。</span><span class="sxs-lookup"><span data-stu-id="630a7-194">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="630a7-195">これらは Microsoft ライブラリではありません、ため Microsoft でない保証または (暗黙の特許権を含むなし) 知的財産権ライセンスをこの CDN でホストされているサードパーティ製ライブラリ。</span><span class="sxs-lookup"><span data-stu-id="630a7-195">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="630a7-196">CDN の jQuery リリース</span><span class="sxs-lookup"><span data-stu-id="630a7-196">jQuery Releases on the CDN</span></span>

<span data-ttu-id="630a7-197">CDN では、jQuery の次のリリースがホストされています。</span><span class="sxs-lookup"><span data-stu-id="630a7-197">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-331"></a><span data-ttu-id="630a7-198">jQuery バージョン 3.3.1</span><span class="sxs-lookup"><span data-stu-id="630a7-198">jQuery version 3.3.1</span></span>
- <span data-ttu-id="630a7-199">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.js</span><span class="sxs-lookup"><span data-stu-id="630a7-199">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.js</span></span>
- <span data-ttu-id="630a7-200">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-200">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js</span></span>
- <span data-ttu-id="630a7-201">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.map</span><span class="sxs-lookup"><span data-stu-id="630a7-201">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.map</span></span>
- <span data-ttu-id="630a7-202">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.js</span><span class="sxs-lookup"><span data-stu-id="630a7-202">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.js</span></span>
- <span data-ttu-id="630a7-203">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-203">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.js</span></span>
- <span data-ttu-id="630a7-204">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.map</span><span class="sxs-lookup"><span data-stu-id="630a7-204">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.map</span></span>

#### <a name="jquery-version-321"></a><span data-ttu-id="630a7-205">jQuery バージョン 3.2.1</span><span class="sxs-lookup"><span data-stu-id="630a7-205">jQuery version 3.2.1</span></span>
- <span data-ttu-id="630a7-206">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js</span><span class="sxs-lookup"><span data-stu-id="630a7-206">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js</span></span>
- <span data-ttu-id="630a7-207">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-207">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js</span></span>
- <span data-ttu-id="630a7-208">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map</span><span class="sxs-lookup"><span data-stu-id="630a7-208">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map</span></span>
- <span data-ttu-id="630a7-209">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js</span><span class="sxs-lookup"><span data-stu-id="630a7-209">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js</span></span>
- <span data-ttu-id="630a7-210">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-210">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js</span></span>
- <span data-ttu-id="630a7-211">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map</span><span class="sxs-lookup"><span data-stu-id="630a7-211">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map</span></span>

#### <a name="jquery-version-320"></a><span data-ttu-id="630a7-212">jQuery バージョン 3.2.0</span><span class="sxs-lookup"><span data-stu-id="630a7-212">jQuery version 3.2.0</span></span>

- <span data-ttu-id="630a7-213">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js</span><span class="sxs-lookup"><span data-stu-id="630a7-213">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js</span></span>
- <span data-ttu-id="630a7-214">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-214">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js</span></span>
- <span data-ttu-id="630a7-215">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map</span><span class="sxs-lookup"><span data-stu-id="630a7-215">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map</span></span>
- <span data-ttu-id="630a7-216">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js</span><span class="sxs-lookup"><span data-stu-id="630a7-216">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js</span></span>
- <span data-ttu-id="630a7-217">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-217">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js</span></span>
- <span data-ttu-id="630a7-218">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map</span><span class="sxs-lookup"><span data-stu-id="630a7-218">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map</span></span>

#### <a name="jquery-version-311"></a><span data-ttu-id="630a7-219">jQuery バージョン 3.1.1</span><span class="sxs-lookup"><span data-stu-id="630a7-219">jQuery version 3.1.1</span></span>

- <span data-ttu-id="630a7-220">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js</span><span class="sxs-lookup"><span data-stu-id="630a7-220">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js</span></span>
- <span data-ttu-id="630a7-221">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-221">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js</span></span>
- <span data-ttu-id="630a7-222">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map</span><span class="sxs-lookup"><span data-stu-id="630a7-222">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map</span></span>
- <span data-ttu-id="630a7-223">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js</span><span class="sxs-lookup"><span data-stu-id="630a7-223">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js</span></span>
- <span data-ttu-id="630a7-224">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-224">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js</span></span>
- <span data-ttu-id="630a7-225">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map</span><span class="sxs-lookup"><span data-stu-id="630a7-225">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map</span></span>

#### <a name="jquery-version-310"></a><span data-ttu-id="630a7-226">jQuery バージョン 3.1.0</span><span class="sxs-lookup"><span data-stu-id="630a7-226">jQuery version 3.1.0</span></span>

- <span data-ttu-id="630a7-227">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js</span><span class="sxs-lookup"><span data-stu-id="630a7-227">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js</span></span>
- <span data-ttu-id="630a7-228">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-228">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js</span></span>
- <span data-ttu-id="630a7-229">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map</span><span class="sxs-lookup"><span data-stu-id="630a7-229">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map</span></span>
- <span data-ttu-id="630a7-230">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js</span><span class="sxs-lookup"><span data-stu-id="630a7-230">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js</span></span>
- <span data-ttu-id="630a7-231">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-231">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js</span></span>
- <span data-ttu-id="630a7-232">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map</span><span class="sxs-lookup"><span data-stu-id="630a7-232">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map</span></span>

#### <a name="jquery-version-300"></a><span data-ttu-id="630a7-233">jQuery 3.0.0</span><span class="sxs-lookup"><span data-stu-id="630a7-233">jQuery version 3.0.0</span></span>

- <span data-ttu-id="630a7-234">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="630a7-234">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js</span></span>
- <span data-ttu-id="630a7-235">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-235">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js</span></span>
- <span data-ttu-id="630a7-236">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map</span><span class="sxs-lookup"><span data-stu-id="630a7-236">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map</span></span>
- <span data-ttu-id="630a7-237">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js</span><span class="sxs-lookup"><span data-stu-id="630a7-237">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js</span></span>
- <span data-ttu-id="630a7-238">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-238">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js</span></span>
- <span data-ttu-id="630a7-239">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map</span><span class="sxs-lookup"><span data-stu-id="630a7-239">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map</span></span>

#### <a name="jquery-version-224"></a><span data-ttu-id="630a7-240">jQuery バージョン 2.2.4</span><span class="sxs-lookup"><span data-stu-id="630a7-240">jQuery version 2.2.4</span></span>

- <span data-ttu-id="630a7-241">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js</span><span class="sxs-lookup"><span data-stu-id="630a7-241">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js</span></span>
- <span data-ttu-id="630a7-242">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-242">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js</span></span>
- <span data-ttu-id="630a7-243">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map</span><span class="sxs-lookup"><span data-stu-id="630a7-243">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map</span></span>

#### <a name="jquery-version-223"></a><span data-ttu-id="630a7-244">jQuery バージョン 2.2.3</span><span class="sxs-lookup"><span data-stu-id="630a7-244">jQuery version 2.2.3</span></span>

- <span data-ttu-id="630a7-245">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js</span><span class="sxs-lookup"><span data-stu-id="630a7-245">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js</span></span>
- <span data-ttu-id="630a7-246">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-246">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js</span></span>
- <span data-ttu-id="630a7-247">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map</span><span class="sxs-lookup"><span data-stu-id="630a7-247">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map</span></span>

#### <a name="jquery-version-222"></a><span data-ttu-id="630a7-248">jQuery バージョン 2.2.2</span><span class="sxs-lookup"><span data-stu-id="630a7-248">jQuery version 2.2.2</span></span>

- <span data-ttu-id="630a7-249">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js</span><span class="sxs-lookup"><span data-stu-id="630a7-249">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js</span></span>
- <span data-ttu-id="630a7-250">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-250">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js</span></span>
- <span data-ttu-id="630a7-251">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map</span><span class="sxs-lookup"><span data-stu-id="630a7-251">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map</span></span>

#### <a name="jquery-version-221"></a><span data-ttu-id="630a7-252">jQuery バージョン 2.2.1</span><span class="sxs-lookup"><span data-stu-id="630a7-252">jQuery version 2.2.1</span></span>

- <span data-ttu-id="630a7-253">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="630a7-253">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js</span></span>
- <span data-ttu-id="630a7-254">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-254">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js</span></span>
- <span data-ttu-id="630a7-255">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map</span><span class="sxs-lookup"><span data-stu-id="630a7-255">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map</span></span>

#### <a name="jquery-version-220"></a><span data-ttu-id="630a7-256">jQuery 2.2.0</span><span class="sxs-lookup"><span data-stu-id="630a7-256">jQuery version 2.2.0</span></span>

- <span data-ttu-id="630a7-257">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="630a7-257">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js</span></span>
- <span data-ttu-id="630a7-258">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-258">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js</span></span>
- <span data-ttu-id="630a7-259">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map</span><span class="sxs-lookup"><span data-stu-id="630a7-259">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map</span></span>

#### <a name="jquery-version-214"></a><span data-ttu-id="630a7-260">jQuery バージョン 2.1.4</span><span class="sxs-lookup"><span data-stu-id="630a7-260">jQuery version 2.1.4</span></span>

- <span data-ttu-id="630a7-261">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js</span><span class="sxs-lookup"><span data-stu-id="630a7-261">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js</span></span>
- <span data-ttu-id="630a7-262">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-262">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js</span></span>
- <span data-ttu-id="630a7-263">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map</span><span class="sxs-lookup"><span data-stu-id="630a7-263">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map</span></span>

#### <a name="jquery-version-213"></a><span data-ttu-id="630a7-264">jQuery バージョン 2.1.3</span><span class="sxs-lookup"><span data-stu-id="630a7-264">jQuery version 2.1.3</span></span>

- <span data-ttu-id="630a7-265">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js</span><span class="sxs-lookup"><span data-stu-id="630a7-265">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js</span></span>
- <span data-ttu-id="630a7-266">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-266">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js</span></span>
- <span data-ttu-id="630a7-267">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map</span><span class="sxs-lookup"><span data-stu-id="630a7-267">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map</span></span>

#### <a name="jquery-version-212"></a><span data-ttu-id="630a7-268">jQuery バージョン 2.1.2</span><span class="sxs-lookup"><span data-stu-id="630a7-268">jQuery version 2.1.2</span></span>

- <span data-ttu-id="630a7-269">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js</span><span class="sxs-lookup"><span data-stu-id="630a7-269">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js</span></span>
- <span data-ttu-id="630a7-270">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-270">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js</span></span>

#### <a name="jquery-version-211"></a><span data-ttu-id="630a7-271">jQuery バージョン 2.1.1</span><span class="sxs-lookup"><span data-stu-id="630a7-271">jQuery version 2.1.1</span></span>

- <span data-ttu-id="630a7-272">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js</span><span class="sxs-lookup"><span data-stu-id="630a7-272">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js</span></span>
- <span data-ttu-id="630a7-273">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-273">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js</span></span>
- <span data-ttu-id="630a7-274">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map</span><span class="sxs-lookup"><span data-stu-id="630a7-274">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map</span></span>

#### <a name="jquery-version-210"></a><span data-ttu-id="630a7-275">jQuery バージョン 2.1.0</span><span class="sxs-lookup"><span data-stu-id="630a7-275">jQuery version 2.1.0</span></span>

- <span data-ttu-id="630a7-276">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="630a7-276">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js</span></span>
- <span data-ttu-id="630a7-277">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-277">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js</span></span>
- <span data-ttu-id="630a7-278">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="630a7-278">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js</span></span>
- <span data-ttu-id="630a7-279">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map</span><span class="sxs-lookup"><span data-stu-id="630a7-279">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map</span></span>

#### <a name="jquery-version-203"></a><span data-ttu-id="630a7-280">jQuery バージョン 2.0.3</span><span class="sxs-lookup"><span data-stu-id="630a7-280">jQuery version 2.0.3</span></span>

- <span data-ttu-id="630a7-281">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js</span><span class="sxs-lookup"><span data-stu-id="630a7-281">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js</span></span>
- <span data-ttu-id="630a7-282">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-282">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js</span></span>
- <span data-ttu-id="630a7-283">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="630a7-283">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js</span></span>
- <span data-ttu-id="630a7-284">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map</span><span class="sxs-lookup"><span data-stu-id="630a7-284">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map</span></span>

#### <a name="jquery-version-202"></a><span data-ttu-id="630a7-285">jQuery バージョン 2.0.2</span><span class="sxs-lookup"><span data-stu-id="630a7-285">jQuery version 2.0.2</span></span>

- <span data-ttu-id="630a7-286">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js</span><span class="sxs-lookup"><span data-stu-id="630a7-286">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js</span></span>
- <span data-ttu-id="630a7-287">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-287">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js</span></span>
- <span data-ttu-id="630a7-288">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="630a7-288">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js</span></span>
- <span data-ttu-id="630a7-289">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map</span><span class="sxs-lookup"><span data-stu-id="630a7-289">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map</span></span>

#### <a name="jquery-version-201"></a><span data-ttu-id="630a7-290">jQuery 2.0.1</span><span class="sxs-lookup"><span data-stu-id="630a7-290">jQuery version 2.0.1</span></span>

- <span data-ttu-id="630a7-291">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js</span><span class="sxs-lookup"><span data-stu-id="630a7-291">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js</span></span>
- <span data-ttu-id="630a7-292">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-292">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js</span></span>
- <span data-ttu-id="630a7-293">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="630a7-293">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js</span></span>
- <span data-ttu-id="630a7-294">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map</span><span class="sxs-lookup"><span data-stu-id="630a7-294">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map</span></span>

#### <a name="jquery-version-200"></a><span data-ttu-id="630a7-295">jQuery version 2.0.0</span><span class="sxs-lookup"><span data-stu-id="630a7-295">jQuery version 2.0.0</span></span>

- <span data-ttu-id="630a7-296">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js</span><span class="sxs-lookup"><span data-stu-id="630a7-296">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js</span></span>
- <span data-ttu-id="630a7-297">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-297">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js</span></span>
- <span data-ttu-id="630a7-298">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="630a7-298">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js</span></span>
- <span data-ttu-id="630a7-299">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map</span><span class="sxs-lookup"><span data-stu-id="630a7-299">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map</span></span>

#### <a name="jquery-version-1124"></a><span data-ttu-id="630a7-300">jQuery バージョン 1.12.4</span><span class="sxs-lookup"><span data-stu-id="630a7-300">jQuery version 1.12.4</span></span>

- <span data-ttu-id="630a7-301">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js</span><span class="sxs-lookup"><span data-stu-id="630a7-301">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js</span></span>
- <span data-ttu-id="630a7-302">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-302">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js</span></span>
- <span data-ttu-id="630a7-303">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map</span><span class="sxs-lookup"><span data-stu-id="630a7-303">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map</span></span>

#### <a name="jquery-version-1123"></a><span data-ttu-id="630a7-304">jQuery バージョン 1.12.3</span><span class="sxs-lookup"><span data-stu-id="630a7-304">jQuery version 1.12.3</span></span>

- <span data-ttu-id="630a7-305">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js</span><span class="sxs-lookup"><span data-stu-id="630a7-305">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js</span></span>
- <span data-ttu-id="630a7-306">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-306">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js</span></span>
- <span data-ttu-id="630a7-307">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map</span><span class="sxs-lookup"><span data-stu-id="630a7-307">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map</span></span>

#### <a name="jquery-version-1122"></a><span data-ttu-id="630a7-308">jQuery バージョン 1.12.2</span><span class="sxs-lookup"><span data-stu-id="630a7-308">jQuery version 1.12.2</span></span>

- <span data-ttu-id="630a7-309">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js</span><span class="sxs-lookup"><span data-stu-id="630a7-309">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js</span></span>
- <span data-ttu-id="630a7-310">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-310">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js</span></span>
- <span data-ttu-id="630a7-311">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map</span><span class="sxs-lookup"><span data-stu-id="630a7-311">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map</span></span>

#### <a name="jquery-version-1121"></a><span data-ttu-id="630a7-312">jQuery バージョン 1.12.1</span><span class="sxs-lookup"><span data-stu-id="630a7-312">jQuery version 1.12.1</span></span>

- <span data-ttu-id="630a7-313">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js</span><span class="sxs-lookup"><span data-stu-id="630a7-313">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js</span></span>
- <span data-ttu-id="630a7-314">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-314">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js</span></span>
- <span data-ttu-id="630a7-315">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map</span><span class="sxs-lookup"><span data-stu-id="630a7-315">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map</span></span>

#### <a name="jquery-version-1120"></a><span data-ttu-id="630a7-316">jQuery バージョン 1.12.0</span><span class="sxs-lookup"><span data-stu-id="630a7-316">jQuery version 1.12.0</span></span>

- <span data-ttu-id="630a7-317">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js</span><span class="sxs-lookup"><span data-stu-id="630a7-317">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js</span></span>
- <span data-ttu-id="630a7-318">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-318">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js</span></span>
- <span data-ttu-id="630a7-319">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map</span><span class="sxs-lookup"><span data-stu-id="630a7-319">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map</span></span>

#### <a name="jquery-version-1113"></a><span data-ttu-id="630a7-320">jQuery バージョン 1.11.3</span><span class="sxs-lookup"><span data-stu-id="630a7-320">jQuery version 1.11.3</span></span>

- <span data-ttu-id="630a7-321">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js</span><span class="sxs-lookup"><span data-stu-id="630a7-321">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js</span></span>
- <span data-ttu-id="630a7-322">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-322">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js</span></span>
- <span data-ttu-id="630a7-323">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map</span><span class="sxs-lookup"><span data-stu-id="630a7-323">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map</span></span>

#### <a name="jquery-version-1112"></a><span data-ttu-id="630a7-324">jQuery バージョン 1.11.2</span><span class="sxs-lookup"><span data-stu-id="630a7-324">jQuery version 1.11.2</span></span>

- <span data-ttu-id="630a7-325">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js</span><span class="sxs-lookup"><span data-stu-id="630a7-325">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js</span></span>
- <span data-ttu-id="630a7-326">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-326">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js</span></span>
- <span data-ttu-id="630a7-327">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map</span><span class="sxs-lookup"><span data-stu-id="630a7-327">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map</span></span>

#### <a name="jquery-version-1111"></a><span data-ttu-id="630a7-328">jQuery バージョン 1.11.1</span><span class="sxs-lookup"><span data-stu-id="630a7-328">jQuery version 1.11.1</span></span>

- <span data-ttu-id="630a7-329">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js</span><span class="sxs-lookup"><span data-stu-id="630a7-329">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js</span></span>
- <span data-ttu-id="630a7-330">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-330">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js</span></span>
- <span data-ttu-id="630a7-331">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map</span><span class="sxs-lookup"><span data-stu-id="630a7-331">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map</span></span>

#### <a name="jquery-version-1110"></a><span data-ttu-id="630a7-332">jQuery バージョン 1.11.0</span><span class="sxs-lookup"><span data-stu-id="630a7-332">jQuery version 1.11.0</span></span>

- <span data-ttu-id="630a7-333">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js</span><span class="sxs-lookup"><span data-stu-id="630a7-333">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js</span></span>
- <span data-ttu-id="630a7-334">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-334">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js</span></span>
- <span data-ttu-id="630a7-335">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="630a7-335">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js</span></span>
- <span data-ttu-id="630a7-336">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map</span><span class="sxs-lookup"><span data-stu-id="630a7-336">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map</span></span>

#### <a name="jquery-version-1102"></a><span data-ttu-id="630a7-337">jQuery バージョン 1.10.2</span><span class="sxs-lookup"><span data-stu-id="630a7-337">jQuery version 1.10.2</span></span>

- <span data-ttu-id="630a7-338">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js</span><span class="sxs-lookup"><span data-stu-id="630a7-338">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js</span></span>
- <span data-ttu-id="630a7-339">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-339">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js</span></span>
- <span data-ttu-id="630a7-340">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="630a7-340">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js</span></span>
- <span data-ttu-id="630a7-341">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map</span><span class="sxs-lookup"><span data-stu-id="630a7-341">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map</span></span>

#### <a name="jquery-version-1101"></a><span data-ttu-id="630a7-342">jQuery バージョン 1.10.1</span><span class="sxs-lookup"><span data-stu-id="630a7-342">jQuery version 1.10.1</span></span>

- <span data-ttu-id="630a7-343">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js</span><span class="sxs-lookup"><span data-stu-id="630a7-343">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js</span></span>
- <span data-ttu-id="630a7-344">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-344">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js</span></span>
- <span data-ttu-id="630a7-345">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="630a7-345">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js</span></span>
- <span data-ttu-id="630a7-346">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map</span><span class="sxs-lookup"><span data-stu-id="630a7-346">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map</span></span>

#### <a name="jquery-version-1100"></a><span data-ttu-id="630a7-347">jQuery バージョン 1.10.0</span><span class="sxs-lookup"><span data-stu-id="630a7-347">jQuery version 1.10.0</span></span>

- <span data-ttu-id="630a7-348">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js</span><span class="sxs-lookup"><span data-stu-id="630a7-348">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js</span></span>
- <span data-ttu-id="630a7-349">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-349">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js</span></span>
- <span data-ttu-id="630a7-350">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="630a7-350">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js</span></span>
- <span data-ttu-id="630a7-351">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map</span><span class="sxs-lookup"><span data-stu-id="630a7-351">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map</span></span>

#### <a name="jquery-version-191"></a><span data-ttu-id="630a7-352">jQuery バージョン 1.9.1</span><span class="sxs-lookup"><span data-stu-id="630a7-352">jQuery version 1.9.1</span></span>

- <span data-ttu-id="630a7-353">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js</span><span class="sxs-lookup"><span data-stu-id="630a7-353">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js</span></span>
- <span data-ttu-id="630a7-354">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-354">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js</span></span>
- <span data-ttu-id="630a7-355">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="630a7-355">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js</span></span>
- <span data-ttu-id="630a7-356">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map</span><span class="sxs-lookup"><span data-stu-id="630a7-356">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map</span></span>

#### <a name="jquery-version-190"></a><span data-ttu-id="630a7-357">jQuery バージョン 1.9.0</span><span class="sxs-lookup"><span data-stu-id="630a7-357">jQuery version 1.9.0</span></span>

- <span data-ttu-id="630a7-358">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js</span><span class="sxs-lookup"><span data-stu-id="630a7-358">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js</span></span>
- <span data-ttu-id="630a7-359">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-359">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js</span></span>
- <span data-ttu-id="630a7-360">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="630a7-360">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js</span></span>
- <span data-ttu-id="630a7-361">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map</span><span class="sxs-lookup"><span data-stu-id="630a7-361">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map</span></span>

#### <a name="jquery-version-183"></a><span data-ttu-id="630a7-362">jQuery バージョン 1.8.3</span><span class="sxs-lookup"><span data-stu-id="630a7-362">jQuery version 1.8.3</span></span>

- <span data-ttu-id="630a7-363">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js</span><span class="sxs-lookup"><span data-stu-id="630a7-363">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js</span></span>
- <span data-ttu-id="630a7-364">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-364">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js</span></span>
- <span data-ttu-id="630a7-365">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="630a7-365">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js</span></span>

#### <a name="jquery-version-182"></a><span data-ttu-id="630a7-366">jQuery バージョン 1.8.2</span><span class="sxs-lookup"><span data-stu-id="630a7-366">jQuery version 1.8.2</span></span>

- <span data-ttu-id="630a7-367">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js</span><span class="sxs-lookup"><span data-stu-id="630a7-367">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js</span></span>
- <span data-ttu-id="630a7-368">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-368">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js</span></span>
- <span data-ttu-id="630a7-369">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="630a7-369">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js</span></span>

#### <a name="jquery-version-181"></a><span data-ttu-id="630a7-370">jQuery バージョン 1.8.1</span><span class="sxs-lookup"><span data-stu-id="630a7-370">jQuery version 1.8.1</span></span>

- <span data-ttu-id="630a7-371">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js</span><span class="sxs-lookup"><span data-stu-id="630a7-371">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js</span></span>
- <span data-ttu-id="630a7-372">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-372">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js</span></span>
- <span data-ttu-id="630a7-373">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="630a7-373">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js</span></span>

#### <a name="jquery-version-180"></a><span data-ttu-id="630a7-374">jQuery バージョン 1.8.0</span><span class="sxs-lookup"><span data-stu-id="630a7-374">jQuery version 1.8.0</span></span>

- <span data-ttu-id="630a7-375">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="630a7-375">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="630a7-376">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-376">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js</span></span>
- <span data-ttu-id="630a7-377">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="630a7-377">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js</span></span>

#### <a name="jquery-version-172"></a><span data-ttu-id="630a7-378">jQuery バージョン 1.7.2</span><span class="sxs-lookup"><span data-stu-id="630a7-378">jQuery version 1.7.2</span></span>

- <span data-ttu-id="630a7-379">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js</span><span class="sxs-lookup"><span data-stu-id="630a7-379">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js</span></span>
- <span data-ttu-id="630a7-380">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-380">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js</span></span>

#### <a name="jquery-version-171"></a><span data-ttu-id="630a7-381">jQuery バージョン 1.7.1</span><span class="sxs-lookup"><span data-stu-id="630a7-381">jQuery version 1.7.1</span></span>

- <span data-ttu-id="630a7-382">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js</span><span class="sxs-lookup"><span data-stu-id="630a7-382">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js</span></span>
- <span data-ttu-id="630a7-383">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-383">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js</span></span>
- <span data-ttu-id="630a7-384">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="630a7-384">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js</span></span>

#### <a name="jquery-version-17"></a><span data-ttu-id="630a7-385">jQuery バージョン 1.7</span><span class="sxs-lookup"><span data-stu-id="630a7-385">jQuery version 1.7</span></span>

- <span data-ttu-id="630a7-386">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js</span><span class="sxs-lookup"><span data-stu-id="630a7-386">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js</span></span>
- <span data-ttu-id="630a7-387">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-387">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js</span></span>
- <span data-ttu-id="630a7-388">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="630a7-388">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js</span></span>

#### <a name="jquery-version-164"></a><span data-ttu-id="630a7-389">jQuery バージョン 1.6.4</span><span class="sxs-lookup"><span data-stu-id="630a7-389">jQuery version 1.6.4</span></span>

- <span data-ttu-id="630a7-390">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js</span><span class="sxs-lookup"><span data-stu-id="630a7-390">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js</span></span>
- <span data-ttu-id="630a7-391">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-391">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js</span></span>
- <span data-ttu-id="630a7-392">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="630a7-392">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js</span></span>

#### <a name="jquery-version-163"></a><span data-ttu-id="630a7-393">jQuery バージョン 1.6.3</span><span class="sxs-lookup"><span data-stu-id="630a7-393">jQuery version 1.6.3</span></span>

- <span data-ttu-id="630a7-394">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js</span><span class="sxs-lookup"><span data-stu-id="630a7-394">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js</span></span>
- <span data-ttu-id="630a7-395">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-395">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js</span></span>
- <span data-ttu-id="630a7-396">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="630a7-396">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js</span></span>

#### <a name="jquery-version-162"></a><span data-ttu-id="630a7-397">jQuery バージョン 1.6.2 でした。</span><span class="sxs-lookup"><span data-stu-id="630a7-397">jQuery version 1.6.2</span></span>

- <span data-ttu-id="630a7-398">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js</span><span class="sxs-lookup"><span data-stu-id="630a7-398">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js</span></span>
- <span data-ttu-id="630a7-399">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-399">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js</span></span>
- <span data-ttu-id="630a7-400">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="630a7-400">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js</span></span>

#### <a name="jquery-version-161"></a><span data-ttu-id="630a7-401">jQuery バージョン 1.6.1</span><span class="sxs-lookup"><span data-stu-id="630a7-401">jQuery version 1.6.1</span></span>

- <span data-ttu-id="630a7-402">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js</span><span class="sxs-lookup"><span data-stu-id="630a7-402">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js</span></span>
- <span data-ttu-id="630a7-403">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-403">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js</span></span>
- <span data-ttu-id="630a7-404">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="630a7-404">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js</span></span>

#### <a name="jquery-version-16"></a><span data-ttu-id="630a7-405">jQuery バージョン 1.6</span><span class="sxs-lookup"><span data-stu-id="630a7-405">jQuery version 1.6</span></span>

- <span data-ttu-id="630a7-406">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js</span><span class="sxs-lookup"><span data-stu-id="630a7-406">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js</span></span>
- <span data-ttu-id="630a7-407">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-407">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js</span></span>
- <span data-ttu-id="630a7-408">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="630a7-408">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js</span></span>

#### <a name="jquery-version-152"></a><span data-ttu-id="630a7-409">jQuery バージョン 1.5.2</span><span class="sxs-lookup"><span data-stu-id="630a7-409">jQuery version 1.5.2</span></span>

- <span data-ttu-id="630a7-410">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js</span><span class="sxs-lookup"><span data-stu-id="630a7-410">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js</span></span>
- <span data-ttu-id="630a7-411">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-411">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js</span></span>
- <span data-ttu-id="630a7-412">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="630a7-412">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js</span></span>

#### <a name="jquery-version-151"></a><span data-ttu-id="630a7-413">jQuery バージョン 1.5.1</span><span class="sxs-lookup"><span data-stu-id="630a7-413">jQuery version 1.5.1</span></span>

- <span data-ttu-id="630a7-414">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js</span><span class="sxs-lookup"><span data-stu-id="630a7-414">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js</span></span>
- <span data-ttu-id="630a7-415">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-415">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js</span></span>
- <span data-ttu-id="630a7-416">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="630a7-416">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js</span></span>

#### <a name="jquery-version-15"></a><span data-ttu-id="630a7-417">jQuery 1.5</span><span class="sxs-lookup"><span data-stu-id="630a7-417">jQuery version 1.5</span></span>

- <span data-ttu-id="630a7-418">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js</span><span class="sxs-lookup"><span data-stu-id="630a7-418">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js</span></span>
- <span data-ttu-id="630a7-419">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-419">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js</span></span>
- <span data-ttu-id="630a7-420">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="630a7-420">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js</span></span>

#### <a name="jquery-version-144"></a><span data-ttu-id="630a7-421">jQuery バージョン 1.4.4</span><span class="sxs-lookup"><span data-stu-id="630a7-421">jQuery version 1.4.4</span></span>

- <span data-ttu-id="630a7-422">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js</span><span class="sxs-lookup"><span data-stu-id="630a7-422">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js</span></span>
- <span data-ttu-id="630a7-423">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-423">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js</span></span>
- <span data-ttu-id="630a7-424">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="630a7-424">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js</span></span>

#### <a name="jquery-version-143"></a><span data-ttu-id="630a7-425">jQuery バージョン 1.4.3</span><span class="sxs-lookup"><span data-stu-id="630a7-425">jQuery version 1.4.3</span></span>

- <span data-ttu-id="630a7-426">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js</span><span class="sxs-lookup"><span data-stu-id="630a7-426">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js</span></span>
- <span data-ttu-id="630a7-427">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-427">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js</span></span>
- <span data-ttu-id="630a7-428">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="630a7-428">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js</span></span>

#### <a name="jquery-version-142"></a><span data-ttu-id="630a7-429">jQuery バージョン 1.4.2</span><span class="sxs-lookup"><span data-stu-id="630a7-429">jQuery version 1.4.2</span></span>

- <span data-ttu-id="630a7-430">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js</span><span class="sxs-lookup"><span data-stu-id="630a7-430">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js</span></span>
- <span data-ttu-id="630a7-431">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-431">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js</span></span>
- <span data-ttu-id="630a7-432">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="630a7-432">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js</span></span>

#### <a name="jquery-version-141"></a><span data-ttu-id="630a7-433">jQuery バージョン 1.4.1</span><span class="sxs-lookup"><span data-stu-id="630a7-433">jQuery version 1.4.1</span></span>

- <span data-ttu-id="630a7-434">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js</span><span class="sxs-lookup"><span data-stu-id="630a7-434">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js</span></span>
- <span data-ttu-id="630a7-435">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-435">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js</span></span>
- <span data-ttu-id="630a7-436">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="630a7-436">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js</span></span>

#### <a name="jquery-version-14"></a><span data-ttu-id="630a7-437">jQuery バージョン 1.4</span><span class="sxs-lookup"><span data-stu-id="630a7-437">jQuery version 1.4</span></span>

- <span data-ttu-id="630a7-438">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js</span><span class="sxs-lookup"><span data-stu-id="630a7-438">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js</span></span>
- <span data-ttu-id="630a7-439">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-439">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js</span></span>

#### <a name="jquery-version-132"></a><span data-ttu-id="630a7-440">jQuery バージョン 1.3.2</span><span class="sxs-lookup"><span data-stu-id="630a7-440">jQuery version 1.3.2</span></span>

- <span data-ttu-id="630a7-441">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js</span><span class="sxs-lookup"><span data-stu-id="630a7-441">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js</span></span>
- <span data-ttu-id="630a7-442">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-442">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js</span></span>
- <span data-ttu-id="630a7-443">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="630a7-443">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js</span></span>
- <span data-ttu-id="630a7-444">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="630a7-444">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js</span></span>

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="630a7-445">CDN の jQuery 移行リリース</span><span class="sxs-lookup"><span data-stu-id="630a7-445">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="630a7-446">CDN では、jQuery の移行の次のリリースがホストされています。</span><span class="sxs-lookup"><span data-stu-id="630a7-446">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="630a7-447">jQuery 3.0.0 の移行</span><span class="sxs-lookup"><span data-stu-id="630a7-447">jQuery Migrate version 3.0.0</span></span>

- <span data-ttu-id="630a7-448">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="630a7-448">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js</span></span>
- <span data-ttu-id="630a7-449">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-449">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js</span></span>

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="630a7-450">jQuery 1.2.1 の移行</span><span class="sxs-lookup"><span data-stu-id="630a7-450">jQuery Migrate version 1.2.1</span></span>

- <span data-ttu-id="630a7-451">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js</span><span class="sxs-lookup"><span data-stu-id="630a7-451">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js</span></span>
- <span data-ttu-id="630a7-452">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-452">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js</span></span>

<span data-ttu-id="630a7-453">jQuery バージョン 1.2.0 の移行</span><span class="sxs-lookup"><span data-stu-id="630a7-453">jQuery Migrate version 1.2.0</span></span>

- <span data-ttu-id="630a7-454">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js</span><span class="sxs-lookup"><span data-stu-id="630a7-454">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js</span></span>
- <span data-ttu-id="630a7-455">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-455">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js</span></span>

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="630a7-456">jQuery バージョン 1.1.1 の移行</span><span class="sxs-lookup"><span data-stu-id="630a7-456">jQuery Migrate version 1.1.1</span></span>

- <span data-ttu-id="630a7-457">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js</span><span class="sxs-lookup"><span data-stu-id="630a7-457">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js</span></span>
- <span data-ttu-id="630a7-458">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-458">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js</span></span>

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="630a7-459">jQuery version 1.1.0 の移行</span><span class="sxs-lookup"><span data-stu-id="630a7-459">jQuery Migrate version 1.1.0</span></span>

- <span data-ttu-id="630a7-460">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js</span><span class="sxs-lookup"><span data-stu-id="630a7-460">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js</span></span>
- <span data-ttu-id="630a7-461">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-461">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js</span></span>

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="630a7-462">jQuery バージョン 1.0.0 の移行</span><span class="sxs-lookup"><span data-stu-id="630a7-462">jQuery Migrate version 1.0.0</span></span>

- <span data-ttu-id="630a7-463">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js</span><span class="sxs-lookup"><span data-stu-id="630a7-463">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js</span></span>
- <span data-ttu-id="630a7-464">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-464">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js</span></span>

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="630a7-465">jQuery UI、CDN のリリース</span><span class="sxs-lookup"><span data-stu-id="630a7-465">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="630a7-466">JQuery UI ライブラリの次のリリースは、この CDN でホストされています。</span><span class="sxs-lookup"><span data-stu-id="630a7-466">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="630a7-467">実際のファイルの一覧を表示するには、各リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="630a7-467">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="630a7-468">jQuery UI 1.12.1</span><span class="sxs-lookup"><span data-stu-id="630a7-468">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "jQuery UI 1.12.1 Microsoft Ajax CDN の")
- [<span data-ttu-id="630a7-469">jQuery UI 1.12.0</span><span class="sxs-lookup"><span data-stu-id="630a7-469">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "jQuery UI 1.12.0 Microsoft Ajax CDN の")
- [<span data-ttu-id="630a7-470">jQuery UI 1.11.4</span><span class="sxs-lookup"><span data-stu-id="630a7-470">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "jQuery UI 1.11.4 Microsoft Ajax CDN の")
- [<span data-ttu-id="630a7-471">jQuery UI 1.11.3</span><span class="sxs-lookup"><span data-stu-id="630a7-471">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "jQuery UI 1.11.3 Microsoft Ajax CDN の")
- [<span data-ttu-id="630a7-472">jQuery UI 1.11.2</span><span class="sxs-lookup"><span data-stu-id="630a7-472">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "jQuery UI 1.11.2 Microsoft Ajax CDN の")
- [<span data-ttu-id="630a7-473">jQuery UI 1.11.1</span><span class="sxs-lookup"><span data-stu-id="630a7-473">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "jQuery UI 1.11.1 Microsoft Ajax CDN の")
- [<span data-ttu-id="630a7-474">jQuery UI 1.11.0</span><span class="sxs-lookup"><span data-stu-id="630a7-474">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "jQuery UI 1.11.0 Microsoft Ajax CDN の")
- [<span data-ttu-id="630a7-475">jQuery UI 1.10.4</span><span class="sxs-lookup"><span data-stu-id="630a7-475">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "jQuery UI 1.10.4 Microsoft Ajax CDN の")
- [<span data-ttu-id="630a7-476">jQuery UI 1.10.3</span><span class="sxs-lookup"><span data-stu-id="630a7-476">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "jQuery UI 1.10.3 Microsoft Ajax CDN の")
- [<span data-ttu-id="630a7-477">jQuery UI 1.10.2</span><span class="sxs-lookup"><span data-stu-id="630a7-477">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "jQuery UI 1.10.2 Microsoft Ajax CDN の")
- [<span data-ttu-id="630a7-478">jQuery UI 1.10.1</span><span class="sxs-lookup"><span data-stu-id="630a7-478">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "jQuery UI 1.10.1 Microsoft Ajax CDN の")
- [<span data-ttu-id="630a7-479">jQuery UI 1.10.0</span><span class="sxs-lookup"><span data-stu-id="630a7-479">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "jQuery UI 1.10.0、Microsoft Ajax CDN の")
- [<span data-ttu-id="630a7-480">jQuery UI 1.9.2</span><span class="sxs-lookup"><span data-stu-id="630a7-480">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "jQuery UI 1.9.2 Microsoft Ajax CDN の")
- [<span data-ttu-id="630a7-481">jQuery UI 1.9.1</span><span class="sxs-lookup"><span data-stu-id="630a7-481">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "jQuery UI 1.9.1 Microsoft Ajax CDN の")
- [<span data-ttu-id="630a7-482">jQuery UI 1.9.0</span><span class="sxs-lookup"><span data-stu-id="630a7-482">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "jQuery UI 1.9.0 Microsoft Ajax CDN の")
- [<span data-ttu-id="630a7-483">jQuery UI 1.8.24</span><span class="sxs-lookup"><span data-stu-id="630a7-483">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "jQuery UI 1.8.24 Microsoft Ajax CDN の")
- [<span data-ttu-id="630a7-484">jQuery UI 1.8.23</span><span class="sxs-lookup"><span data-stu-id="630a7-484">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "jQuery UI 1.8.23 Microsoft Ajax CDN の")
- [<span data-ttu-id="630a7-485">jQuery UI 1.8.22</span><span class="sxs-lookup"><span data-stu-id="630a7-485">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "jQuery UI 1.8.22 Microsoft Ajax CDN の")
- [<span data-ttu-id="630a7-486">jQuery UI 1.8.21</span><span class="sxs-lookup"><span data-stu-id="630a7-486">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "jQuery UI 1.8.21 Microsoft Ajax CDN の")
- [<span data-ttu-id="630a7-487">jQuery UI 1.8.20</span><span class="sxs-lookup"><span data-stu-id="630a7-487">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "jQuery UI 1.8.20 Microsoft Ajax CDN の")
- [<span data-ttu-id="630a7-488">jQuery UI 1.8.19</span><span class="sxs-lookup"><span data-stu-id="630a7-488">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "jQuery UI 1.8.19 Microsoft Ajax CDN の")
- [<span data-ttu-id="630a7-489">jQuery UI 1.8.18</span><span class="sxs-lookup"><span data-stu-id="630a7-489">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "jQuery UI 1.8.18 Microsoft Ajax CDN の")
- [<span data-ttu-id="630a7-490">jQuery UI 1.8.17</span><span class="sxs-lookup"><span data-stu-id="630a7-490">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "jQuery UI 1.8.17 Microsoft Ajax CDN の")
- [<span data-ttu-id="630a7-491">jQuery UI 1.8.16</span><span class="sxs-lookup"><span data-stu-id="630a7-491">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "jQuery UI 1.8.16 Microsoft Ajax CDN の")
- [<span data-ttu-id="630a7-492">jQuery UI 1.8.15</span><span class="sxs-lookup"><span data-stu-id="630a7-492">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "jQuery UI 1.8.15 Microsoft Ajax CDN の")
- [<span data-ttu-id="630a7-493">jQuery UI 1.8.14</span><span class="sxs-lookup"><span data-stu-id="630a7-493">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "jQuery UI 1.8.14 Microsoft Ajax CDN の")
- [<span data-ttu-id="630a7-494">jQuery UI 1.8.13</span><span class="sxs-lookup"><span data-stu-id="630a7-494">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "jQuery UI 1.8.13 Microsoft Ajax CDN の")
- [<span data-ttu-id="630a7-495">jQuery UI 1.8.12</span><span class="sxs-lookup"><span data-stu-id="630a7-495">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "jQuery UI 1.8.12 Microsoft Ajax CDN の")
- [<span data-ttu-id="630a7-496">jQuery UI 1.8.11</span><span class="sxs-lookup"><span data-stu-id="630a7-496">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "jQuery UI 1.8.11 Microsoft Ajax CDN の")
- [<span data-ttu-id="630a7-497">jQuery UI 1.8.10</span><span class="sxs-lookup"><span data-stu-id="630a7-497">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 Microsoft Ajax CDN の")
- [<span data-ttu-id="630a7-498">jQuery UI 1.8.9</span><span class="sxs-lookup"><span data-stu-id="630a7-498">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "jQuery UI 1.8.9 Microsoft Ajax CDN の")
- [<span data-ttu-id="630a7-499">jQuery UI 1.8.8</span><span class="sxs-lookup"><span data-stu-id="630a7-499">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "jQuery UI 1.8.8 Microsoft Ajax CDN の")
- [<span data-ttu-id="630a7-500">jQuery UI 1.8.7</span><span class="sxs-lookup"><span data-stu-id="630a7-500">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "jQuery UI 1.8.7 Microsoft Ajax CDN の")
- [<span data-ttu-id="630a7-501">jQuery UI 1.8.6</span><span class="sxs-lookup"><span data-stu-id="630a7-501">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "jQuery UI 1.8.6 Microsoft Ajax CDN の")
- [<span data-ttu-id="630a7-502">jQuery UI 1.8.5</span><span class="sxs-lookup"><span data-stu-id="630a7-502">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="630a7-503">jQuery 検証 CDN のリリース</span><span class="sxs-lookup"><span data-stu-id="630a7-503">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="630a7-504">JQuery 検証ライブラリの次のリリースは、この CDN でホストされています。</span><span class="sxs-lookup"><span data-stu-id="630a7-504">The following releases of the jQuery Validation library are hosted on this CDN.</span></span> <span data-ttu-id="630a7-505">実際のファイルの一覧を表示するには、各リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="630a7-505">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="630a7-506">jQuery 検証 1.17.0</span><span class="sxs-lookup"><span data-stu-id="630a7-506">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "jQuery 検証 1.17.0")
- [<span data-ttu-id="630a7-507">jQuery 検証 1.16.0</span><span class="sxs-lookup"><span data-stu-id="630a7-507">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "jQuery 検証 1.16.0")
- [<span data-ttu-id="630a7-508">jQuery 検証 1.15.1</span><span class="sxs-lookup"><span data-stu-id="630a7-508">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "jQuery 検証 1.15.1")
- [<span data-ttu-id="630a7-509">jQuery 検証 1.15.0</span><span class="sxs-lookup"><span data-stu-id="630a7-509">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "jQuery 検証 1.15.0")
- [<span data-ttu-id="630a7-510">jQuery 検証 1.14.0</span><span class="sxs-lookup"><span data-stu-id="630a7-510">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "jQuery 検証 1.14.0")
- [<span data-ttu-id="630a7-511">jQuery 検証 1.13.1</span><span class="sxs-lookup"><span data-stu-id="630a7-511">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "jQuery 検証 1.13.1")
- [<span data-ttu-id="630a7-512">jQuery 検証 1.13.0</span><span class="sxs-lookup"><span data-stu-id="630a7-512">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "jQuery 検証 1.13.0")
- [<span data-ttu-id="630a7-513">jQuery 検証 1.12.0</span><span class="sxs-lookup"><span data-stu-id="630a7-513">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "jQuery 検証 1.12.0")
- [<span data-ttu-id="630a7-514">jQuery 検証 1.11.1</span><span class="sxs-lookup"><span data-stu-id="630a7-514">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "jQuery 検証 1.11.1")
- [<span data-ttu-id="630a7-515">jQuery 検証 1.11.0</span><span class="sxs-lookup"><span data-stu-id="630a7-515">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "jQuery 検証 1.11.0")
- [<span data-ttu-id="630a7-516">jQuery 検証 1.10.0</span><span class="sxs-lookup"><span data-stu-id="630a7-516">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "jQuery 検証 1.10.0")
- [<span data-ttu-id="630a7-517">jQuery 検証 1.9</span><span class="sxs-lookup"><span data-stu-id="630a7-517">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jquery.validate バージョン 1.9")
- [<span data-ttu-id="630a7-518">jQuery 検証 1.8.1</span><span class="sxs-lookup"><span data-stu-id="630a7-518">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "jquery.validate バージョン 1.8.1")
- [<span data-ttu-id="630a7-519">jQuery 検証 1.8</span><span class="sxs-lookup"><span data-stu-id="630a7-519">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jquery.validate バージョン 1.8")
- [<span data-ttu-id="630a7-520">jQuery 検証 1.7</span><span class="sxs-lookup"><span data-stu-id="630a7-520">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jquery.validate バージョン 1.7")
- [<span data-ttu-id="630a7-521">jQuery 検証 1.6</span><span class="sxs-lookup"><span data-stu-id="630a7-521">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "jQuery 検証 1.6")
- [<span data-ttu-id="630a7-522">jQuery 検証 1.5.5</span><span class="sxs-lookup"><span data-stu-id="630a7-522">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "jQuery 検証 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="630a7-523">jQuery Mobile、CDN のリリース</span><span class="sxs-lookup"><span data-stu-id="630a7-523">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="630a7-524">JQuery モバイル ライブラリの次のリリースは、この CDN でホストされています。</span><span class="sxs-lookup"><span data-stu-id="630a7-524">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="630a7-525">実際のファイルの一覧を表示するには、各リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="630a7-525">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="630a7-526">jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="630a7-526">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 Microsoft Ajax CDN を")
- [<span data-ttu-id="630a7-527">jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="630a7-527">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 Microsoft Ajax CDN を")
- [<span data-ttu-id="630a7-528">jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="630a7-528">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 Microsoft Ajax CDN を")
- [<span data-ttu-id="630a7-529">jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="630a7-529">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 Microsoft Ajax CDN を")
- [<span data-ttu-id="630a7-530">jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="630a7-530">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 Microsoft Ajax CDN を")
- [<span data-ttu-id="630a7-531">jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="630a7-531">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 Microsoft Ajax CDN を")
- [<span data-ttu-id="630a7-532">jQuery Mobile 1.3.0</span><span class="sxs-lookup"><span data-stu-id="630a7-532">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 Microsoft Ajax CDN を")
- [<span data-ttu-id="630a7-533">jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="630a7-533">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 Microsoft Ajax CDN を")
- [<span data-ttu-id="630a7-534">jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="630a7-534">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 Microsoft Ajax CDN を")
- [<span data-ttu-id="630a7-535">jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="630a7-535">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 Microsoft Ajax CDN を")
- [<span data-ttu-id="630a7-536">jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="630a7-536">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 Microsoft Ajax CDN を")
- [<span data-ttu-id="630a7-537">jQuery Mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="630a7-537">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 Microsoft Ajax CDN を")
- [<span data-ttu-id="630a7-538">jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="630a7-538">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 Microsoft Ajax CDN を")
- [<span data-ttu-id="630a7-539">jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="630a7-539">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 Microsoft Ajax CDN を")
- [<span data-ttu-id="630a7-540">jQuery Mobile 1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="630a7-540">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "Microsoft Ajax CDN の jQuery Mobile 1.0 RC2")
- [<span data-ttu-id="630a7-541">jQuery Mobile 1.0 RC 1</span><span class="sxs-lookup"><span data-stu-id="630a7-541">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "Microsoft Ajax CDN の jQuery Mobile 1.0 RC1")
- [<span data-ttu-id="630a7-542">jQuery Mobile 1.0 beta 3</span><span class="sxs-lookup"><span data-stu-id="630a7-542">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "Microsoft Ajax CDN の jQuery Mobile 1.0 Beta 3")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="630a7-543">jQuery CDN でテンプレートのリリース</span><span class="sxs-lookup"><span data-stu-id="630a7-543">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="630a7-544">JQuery テンプレートのプラグインの次のリリースは、この CDN でホストされています。</span><span class="sxs-lookup"><span data-stu-id="630a7-544">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="630a7-545">実際のファイルの一覧を表示するには、各リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="630a7-545">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="630a7-546">jQuery テンプレート Beta 1</span><span class="sxs-lookup"><span data-stu-id="630a7-546">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery テンプレート Beta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="630a7-547">jQuery CDN のサイクル リリース</span><span class="sxs-lookup"><span data-stu-id="630a7-547">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="630a7-548">JQuery サイクル プラグインの次のリリースは、この CDN でホストされています。</span><span class="sxs-lookup"><span data-stu-id="630a7-548">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="630a7-549">実際のファイルの一覧を表示するには、各リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="630a7-549">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="630a7-550">jQuery サイクル 2.99</span><span class="sxs-lookup"><span data-stu-id="630a7-550">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery サイクル 2.99")
- [<span data-ttu-id="630a7-551">jQuery サイクル 2.94</span><span class="sxs-lookup"><span data-stu-id="630a7-551">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery サイクル 2.94")
- [<span data-ttu-id="630a7-552">jQuery サイクル 2.88</span><span class="sxs-lookup"><span data-stu-id="630a7-552">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery サイクル 2.88")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="630a7-553">jQuery CDN の Datatable リリース</span><span class="sxs-lookup"><span data-stu-id="630a7-553">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="630a7-554">JQuery Datatable プラグインの次のリリースは、この CDN でホストされています。</span><span class="sxs-lookup"><span data-stu-id="630a7-554">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="630a7-555">実際のファイルの一覧を表示するには、各リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="630a7-555">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="630a7-556">jQuery Datatable 1.10.5</span><span class="sxs-lookup"><span data-stu-id="630a7-556">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery Datatable 1.10.5")
- [<span data-ttu-id="630a7-557">jQuery Datatable 1.10.4</span><span class="sxs-lookup"><span data-stu-id="630a7-557">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery Datatable 1.10.4")
- [<span data-ttu-id="630a7-558">jQuery Datatable 1.9.4</span><span class="sxs-lookup"><span data-stu-id="630a7-558">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery Datatable 1.9.4")
- [<span data-ttu-id="630a7-559">jQuery Datatable 1.9.3</span><span class="sxs-lookup"><span data-stu-id="630a7-559">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery Datatable 1.9.3")
- [<span data-ttu-id="630a7-560">jQuery Datatable 1.9.2</span><span class="sxs-lookup"><span data-stu-id="630a7-560">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery Datatable 1.9.2")
- [<span data-ttu-id="630a7-561">jQuery Datatable 1.9.1</span><span class="sxs-lookup"><span data-stu-id="630a7-561">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery Datatable 1.9.1")
- [<span data-ttu-id="630a7-562">jQuery Datatable 1.9.0</span><span class="sxs-lookup"><span data-stu-id="630a7-562">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery Datatable 1.9.0")
- [<span data-ttu-id="630a7-563">jQuery Datatable 1.8.2</span><span class="sxs-lookup"><span data-stu-id="630a7-563">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery Datatable 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="630a7-564">CDN の Modernizr リリース</span><span class="sxs-lookup"><span data-stu-id="630a7-564">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="630a7-565">次のリリースの[Modernizr](http://www.modernizr.com "Modernizr") CDN でホストされます。</span><span class="sxs-lookup"><span data-stu-id="630a7-565">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- <span data-ttu-id="630a7-566">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js</span><span class="sxs-lookup"><span data-stu-id="630a7-566">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js</span></span>
- <span data-ttu-id="630a7-567">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js</span><span class="sxs-lookup"><span data-stu-id="630a7-567">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js</span></span>
- <span data-ttu-id="630a7-568">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js</span><span class="sxs-lookup"><span data-stu-id="630a7-568">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js</span></span>
- <span data-ttu-id="630a7-569">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js</span><span class="sxs-lookup"><span data-stu-id="630a7-569">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js</span></span>
- <span data-ttu-id="630a7-570">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js</span><span class="sxs-lookup"><span data-stu-id="630a7-570">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js</span></span>
- <span data-ttu-id="630a7-571">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js</span><span class="sxs-lookup"><span data-stu-id="630a7-571">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js</span></span>

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="630a7-572">CDN の JSHint リリース</span><span class="sxs-lookup"><span data-stu-id="630a7-572">JSHint Releases on the CDN</span></span>

<span data-ttu-id="630a7-573">次のリリースの[JSHint](http://www.jshint.com "JSHint") CDN でホストされます。</span><span class="sxs-lookup"><span data-stu-id="630a7-573">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- <span data-ttu-id="630a7-574">http://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js</span><span class="sxs-lookup"><span data-stu-id="630a7-574">http://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js</span></span>

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="630a7-575">CDN の knockout リリース</span><span class="sxs-lookup"><span data-stu-id="630a7-575">Knockout Releases on the CDN</span></span>

<span data-ttu-id="630a7-576">次のリリースの[Knockout](http://www.knockoutjs.com "Knockout") CDN でホストされます。</span><span class="sxs-lookup"><span data-stu-id="630a7-576">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

- <span data-ttu-id="630a7-577">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="630a7-577">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js</span></span>
- <span data-ttu-id="630a7-578">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js</span><span class="sxs-lookup"><span data-stu-id="630a7-578">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js</span></span>
- <span data-ttu-id="630a7-579">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="630a7-579">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js</span></span>
- <span data-ttu-id="630a7-580">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js</span><span class="sxs-lookup"><span data-stu-id="630a7-580">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js</span></span>
- <span data-ttu-id="630a7-581">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="630a7-581">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js</span></span>
- <span data-ttu-id="630a7-582">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js</span><span class="sxs-lookup"><span data-stu-id="630a7-582">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js</span></span>
- <span data-ttu-id="630a7-583">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="630a7-583">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js</span></span>
- <span data-ttu-id="630a7-584">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js</span><span class="sxs-lookup"><span data-stu-id="630a7-584">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js</span></span>
- <span data-ttu-id="630a7-585">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js</span><span class="sxs-lookup"><span data-stu-id="630a7-585">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js</span></span>
- <span data-ttu-id="630a7-586">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js</span><span class="sxs-lookup"><span data-stu-id="630a7-586">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js</span></span>
- <span data-ttu-id="630a7-587">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js</span><span class="sxs-lookup"><span data-stu-id="630a7-587">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js</span></span>
- <span data-ttu-id="630a7-588">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js</span><span class="sxs-lookup"><span data-stu-id="630a7-588">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js</span></span>
- <span data-ttu-id="630a7-589">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js</span><span class="sxs-lookup"><span data-stu-id="630a7-589">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js</span></span>
- <span data-ttu-id="630a7-590">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js</span><span class="sxs-lookup"><span data-stu-id="630a7-590">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js</span></span>
- <span data-ttu-id="630a7-591">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js</span><span class="sxs-lookup"><span data-stu-id="630a7-591">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js</span></span>
- <span data-ttu-id="630a7-592">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js</span><span class="sxs-lookup"><span data-stu-id="630a7-592">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js</span></span>
- <span data-ttu-id="630a7-593">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js</span><span class="sxs-lookup"><span data-stu-id="630a7-593">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js</span></span>
- <span data-ttu-id="630a7-594">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js</span><span class="sxs-lookup"><span data-stu-id="630a7-594">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js</span></span>
- <span data-ttu-id="630a7-595">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js</span><span class="sxs-lookup"><span data-stu-id="630a7-595">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js</span></span>
- <span data-ttu-id="630a7-596">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js</span><span class="sxs-lookup"><span data-stu-id="630a7-596">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js</span></span>

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="630a7-597">CDN のリリースをグローバル化します。</span><span class="sxs-lookup"><span data-stu-id="630a7-597">Globalize Releases on the CDN</span></span>

<span data-ttu-id="630a7-598">次のリリースの[Globalize](https://github.com/jquery/globalize "Globalize") CDN でホストされます。</span><span class="sxs-lookup"><span data-stu-id="630a7-598">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="630a7-599">バージョン 1.0.0 をグローバル化します。</span><span class="sxs-lookup"><span data-stu-id="630a7-599">Globalize version 1.0.0</span></span>

- <span data-ttu-id="630a7-600">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js</span><span class="sxs-lookup"><span data-stu-id="630a7-600">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js</span></span>
- <span data-ttu-id="630a7-601">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js</span><span class="sxs-lookup"><span data-stu-id="630a7-601">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js</span></span>
- <span data-ttu-id="630a7-602">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js</span><span class="sxs-lookup"><span data-stu-id="630a7-602">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js</span></span>
- <span data-ttu-id="630a7-603">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js</span><span class="sxs-lookup"><span data-stu-id="630a7-603">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js</span></span>
- <span data-ttu-id="630a7-604">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js</span><span class="sxs-lookup"><span data-stu-id="630a7-604">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js</span></span>
- <span data-ttu-id="630a7-605">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js</span><span class="sxs-lookup"><span data-stu-id="630a7-605">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js</span></span>
- <span data-ttu-id="630a7-606">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js</span><span class="sxs-lookup"><span data-stu-id="630a7-606">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js</span></span>
- <span data-ttu-id="630a7-607">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js</span><span class="sxs-lookup"><span data-stu-id="630a7-607">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js</span></span>

#### <a name="globalize-version-011"></a><span data-ttu-id="630a7-608">バージョン 0.1.1 をグローバル化します。</span><span class="sxs-lookup"><span data-stu-id="630a7-608">Globalize version 0.1.1</span></span>

- <span data-ttu-id="630a7-609">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-609">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js</span></span>
- <span data-ttu-id="630a7-610">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js</span><span class="sxs-lookup"><span data-stu-id="630a7-610">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js</span></span>
- <span data-ttu-id="630a7-611">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js</span><span class="sxs-lookup"><span data-stu-id="630a7-611">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js</span></span>

    - <span data-ttu-id="630a7-612">すべてのカルチャ</span><span class="sxs-lookup"><span data-stu-id="630a7-612">all cultures</span></span>
- <span data-ttu-id="630a7-613">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture です。{カルチャ コード} .js</span><span class="sxs-lookup"><span data-stu-id="630a7-613">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js</span></span>

    - <span data-ttu-id="630a7-614">「{カルチャ コード}」を目的のカルチャ コードに置き換えます、CDN でファイルを globalize.culture.en GB.js== Microsoft 例: = = これらのライブラリは、Microsoft によってアップロードされました。</span><span class="sxs-lookup"><span data-stu-id="630a7-614">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="630a7-615">CDN のリリースを応答します。</span><span class="sxs-lookup"><span data-stu-id="630a7-615">Respond Releases on the CDN</span></span>

<span data-ttu-id="630a7-616">次のリリースの[https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") CDN では、応答がホストされています。</span><span class="sxs-lookup"><span data-stu-id="630a7-616">The following releases of [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") Respond are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="630a7-617">応答バージョン 1.4.2</span><span class="sxs-lookup"><span data-stu-id="630a7-617">Respond version 1.4.2</span></span>

- <span data-ttu-id="630a7-618">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js</span><span class="sxs-lookup"><span data-stu-id="630a7-618">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js</span></span>
- <span data-ttu-id="630a7-619">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-619">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js</span></span>
- <span data-ttu-id="630a7-620">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="630a7-620">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="630a7-621">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-621">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-141"></a><span data-ttu-id="630a7-622">応答バージョン 1.4.1</span><span class="sxs-lookup"><span data-stu-id="630a7-622">Respond version 1.4.1</span></span>

- <span data-ttu-id="630a7-623">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js</span><span class="sxs-lookup"><span data-stu-id="630a7-623">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js</span></span>
- <span data-ttu-id="630a7-624">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-624">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js</span></span>
- <span data-ttu-id="630a7-625">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="630a7-625">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="630a7-626">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-626">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-140"></a><span data-ttu-id="630a7-627">応答バージョン 1.4.0</span><span class="sxs-lookup"><span data-stu-id="630a7-627">Respond version 1.4.0</span></span>

- <span data-ttu-id="630a7-628">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js</span><span class="sxs-lookup"><span data-stu-id="630a7-628">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js</span></span>
- <span data-ttu-id="630a7-629">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-629">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js</span></span>
- <span data-ttu-id="630a7-630">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="630a7-630">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="630a7-631">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-631">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-130"></a><span data-ttu-id="630a7-632">応答バージョン 1.3.0</span><span class="sxs-lookup"><span data-stu-id="630a7-632">Respond version 1.3.0</span></span>

- <span data-ttu-id="630a7-633">http://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js</span><span class="sxs-lookup"><span data-stu-id="630a7-633">http://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js</span></span>

#### <a name="respond-version-120"></a><span data-ttu-id="630a7-634">応答バージョン 1.2.0</span><span class="sxs-lookup"><span data-stu-id="630a7-634">Respond version 1.2.0</span></span>

- <span data-ttu-id="630a7-635">http://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js</span><span class="sxs-lookup"><span data-stu-id="630a7-635">http://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js</span></span>

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="630a7-636">CDN でブートス トラップのリリース</span><span class="sxs-lookup"><span data-stu-id="630a7-636">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="630a7-637">次のリリースの[getbootstrap.com](http://getbootstrap.com "getbootstrap.com")ブートス トラップが CDN でホストされています。</span><span class="sxs-lookup"><span data-stu-id="630a7-637">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-337"></a><span data-ttu-id="630a7-638">ブートス トラップ バージョン 3.3.7</span><span class="sxs-lookup"><span data-stu-id="630a7-638">Bootstrap version 3.3.7</span></span>

- <span data-ttu-id="630a7-639">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="630a7-639">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js</span></span>
- <span data-ttu-id="630a7-640">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-640">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js</span></span>
- <span data-ttu-id="630a7-641">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="630a7-641">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css</span></span>
- <span data-ttu-id="630a7-642">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="630a7-642">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map</span></span>
- <span data-ttu-id="630a7-643">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="630a7-643">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css</span></span>
- <span data-ttu-id="630a7-644">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="630a7-644">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="630a7-645">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="630a7-645">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="630a7-646">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="630a7-646">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="630a7-647">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="630a7-647">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="630a7-648">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg</span><span class="sxs-lookup"><span data-stu-id="630a7-648">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="630a7-649">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="630a7-649">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="630a7-650">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="630a7-650">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="630a7-651">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2</span><span class="sxs-lookup"><span data-stu-id="630a7-651">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-336"></a><span data-ttu-id="630a7-652">ブートス トラップ バージョン 3.3.6</span><span class="sxs-lookup"><span data-stu-id="630a7-652">Bootstrap version 3.3.6</span></span>

- <span data-ttu-id="630a7-653">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="630a7-653">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js</span></span>
- <span data-ttu-id="630a7-654">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-654">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js</span></span>
- <span data-ttu-id="630a7-655">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="630a7-655">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css</span></span>
- <span data-ttu-id="630a7-656">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="630a7-656">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map</span></span>
- <span data-ttu-id="630a7-657">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="630a7-657">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css</span></span>
- <span data-ttu-id="630a7-658">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="630a7-658">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="630a7-659">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="630a7-659">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="630a7-660">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="630a7-660">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="630a7-661">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="630a7-661">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="630a7-662">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg</span><span class="sxs-lookup"><span data-stu-id="630a7-662">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="630a7-663">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="630a7-663">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="630a7-664">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="630a7-664">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="630a7-665">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2</span><span class="sxs-lookup"><span data-stu-id="630a7-665">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-335"></a><span data-ttu-id="630a7-666">ブートス トラップ バージョン 3.3.5</span><span class="sxs-lookup"><span data-stu-id="630a7-666">Bootstrap version 3.3.5</span></span>

- <span data-ttu-id="630a7-667">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="630a7-667">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js</span></span>
- <span data-ttu-id="630a7-668">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-668">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js</span></span>
- <span data-ttu-id="630a7-669">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="630a7-669">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css</span></span>
- <span data-ttu-id="630a7-670">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="630a7-670">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map</span></span>
- <span data-ttu-id="630a7-671">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="630a7-671">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css</span></span>
- <span data-ttu-id="630a7-672">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="630a7-672">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="630a7-673">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="630a7-673">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="630a7-674">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="630a7-674">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="630a7-675">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="630a7-675">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="630a7-676">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg</span><span class="sxs-lookup"><span data-stu-id="630a7-676">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="630a7-677">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="630a7-677">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="630a7-678">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="630a7-678">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="630a7-679">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2</span><span class="sxs-lookup"><span data-stu-id="630a7-679">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-334"></a><span data-ttu-id="630a7-680">ブートス トラップ バージョン 3.3.4</span><span class="sxs-lookup"><span data-stu-id="630a7-680">Bootstrap version 3.3.4</span></span>

- <span data-ttu-id="630a7-681">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="630a7-681">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js</span></span>
- <span data-ttu-id="630a7-682">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-682">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js</span></span>
- <span data-ttu-id="630a7-683">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="630a7-683">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css</span></span>
- <span data-ttu-id="630a7-684">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="630a7-684">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map</span></span>
- <span data-ttu-id="630a7-685">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="630a7-685">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css</span></span>
- <span data-ttu-id="630a7-686">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="630a7-686">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="630a7-687">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="630a7-687">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="630a7-688">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="630a7-688">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="630a7-689">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="630a7-689">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="630a7-690">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg</span><span class="sxs-lookup"><span data-stu-id="630a7-690">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="630a7-691">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="630a7-691">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="630a7-692">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="630a7-692">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="630a7-693">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2</span><span class="sxs-lookup"><span data-stu-id="630a7-693">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-332"></a><span data-ttu-id="630a7-694">ブートス トラップ バージョン 3.3.2</span><span class="sxs-lookup"><span data-stu-id="630a7-694">Bootstrap version 3.3.2</span></span>

- <span data-ttu-id="630a7-695">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="630a7-695">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js</span></span>
- <span data-ttu-id="630a7-696">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-696">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js</span></span>
- <span data-ttu-id="630a7-697">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="630a7-697">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css</span></span>
- <span data-ttu-id="630a7-698">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="630a7-698">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map</span></span>
- <span data-ttu-id="630a7-699">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="630a7-699">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="630a7-700">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="630a7-700">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="630a7-701">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="630a7-701">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="630a7-702">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="630a7-702">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="630a7-703">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="630a7-703">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="630a7-704">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg</span><span class="sxs-lookup"><span data-stu-id="630a7-704">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="630a7-705">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="630a7-705">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="630a7-706">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="630a7-706">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="630a7-707">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2</span><span class="sxs-lookup"><span data-stu-id="630a7-707">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-331"></a><span data-ttu-id="630a7-708">ブートス トラップ バージョン 3.3.1</span><span class="sxs-lookup"><span data-stu-id="630a7-708">Bootstrap version 3.3.1</span></span>

- <span data-ttu-id="630a7-709">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="630a7-709">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js</span></span>
- <span data-ttu-id="630a7-710">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-710">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js</span></span>
- <span data-ttu-id="630a7-711">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="630a7-711">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css</span></span>
- <span data-ttu-id="630a7-712">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="630a7-712">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map</span></span>
- <span data-ttu-id="630a7-713">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="630a7-713">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="630a7-714">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="630a7-714">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="630a7-715">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="630a7-715">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="630a7-716">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="630a7-716">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="630a7-717">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="630a7-717">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="630a7-718">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg</span><span class="sxs-lookup"><span data-stu-id="630a7-718">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="630a7-719">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="630a7-719">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="630a7-720">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="630a7-720">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-330"></a><span data-ttu-id="630a7-721">ブートス トラップ バージョン 3.3.0</span><span class="sxs-lookup"><span data-stu-id="630a7-721">Bootstrap version 3.3.0</span></span>

- <span data-ttu-id="630a7-722">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="630a7-722">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js</span></span>
- <span data-ttu-id="630a7-723">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-723">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js</span></span>
- <span data-ttu-id="630a7-724">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="630a7-724">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css</span></span>
- <span data-ttu-id="630a7-725">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="630a7-725">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="630a7-726">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="630a7-726">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="630a7-727">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="630a7-727">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="630a7-728">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="630a7-728">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="630a7-729">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="630a7-729">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="630a7-730">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="630a7-730">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="630a7-731">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg</span><span class="sxs-lookup"><span data-stu-id="630a7-731">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="630a7-732">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="630a7-732">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="630a7-733">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="630a7-733">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-320"></a><span data-ttu-id="630a7-734">ブートス トラップ バージョン 3.2.0</span><span class="sxs-lookup"><span data-stu-id="630a7-734">Bootstrap version 3.2.0</span></span>

- <span data-ttu-id="630a7-735">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="630a7-735">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js</span></span>
- <span data-ttu-id="630a7-736">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-736">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js</span></span>
- <span data-ttu-id="630a7-737">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="630a7-737">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css</span></span>
- <span data-ttu-id="630a7-738">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="630a7-738">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="630a7-739">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="630a7-739">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="630a7-740">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="630a7-740">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="630a7-741">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="630a7-741">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="630a7-742">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="630a7-742">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="630a7-743">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="630a7-743">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="630a7-744">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg</span><span class="sxs-lookup"><span data-stu-id="630a7-744">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="630a7-745">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="630a7-745">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="630a7-746">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="630a7-746">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-311"></a><span data-ttu-id="630a7-747">ブートス トラップ バージョン 3.1.1</span><span class="sxs-lookup"><span data-stu-id="630a7-747">Bootstrap version 3.1.1</span></span>

- <span data-ttu-id="630a7-748">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="630a7-748">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js</span></span>
- <span data-ttu-id="630a7-749">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-749">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js</span></span>
- <span data-ttu-id="630a7-750">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="630a7-750">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css</span></span>
- <span data-ttu-id="630a7-751">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="630a7-751">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map</span></span>
- <span data-ttu-id="630a7-752">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="630a7-752">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="630a7-753">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="630a7-753">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="630a7-754">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="630a7-754">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="630a7-755">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="630a7-755">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="630a7-756">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="630a7-756">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="630a7-757">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg</span><span class="sxs-lookup"><span data-stu-id="630a7-757">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="630a7-758">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="630a7-758">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="630a7-759">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="630a7-759">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-310"></a><span data-ttu-id="630a7-760">ブートス トラップ バージョン 3.1.0</span><span class="sxs-lookup"><span data-stu-id="630a7-760">Bootstrap version 3.1.0</span></span>

- <span data-ttu-id="630a7-761">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="630a7-761">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js</span></span>
- <span data-ttu-id="630a7-762">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-762">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js</span></span>
- <span data-ttu-id="630a7-763">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="630a7-763">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css</span></span>
- <span data-ttu-id="630a7-764">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="630a7-764">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="630a7-765">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="630a7-765">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="630a7-766">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="630a7-766">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="630a7-767">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="630a7-767">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="630a7-768">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="630a7-768">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="630a7-769">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="630a7-769">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="630a7-770">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg</span><span class="sxs-lookup"><span data-stu-id="630a7-770">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="630a7-771">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="630a7-771">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="630a7-772">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="630a7-772">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-303"></a><span data-ttu-id="630a7-773">ブートス トラップ バージョン 3.0.3</span><span class="sxs-lookup"><span data-stu-id="630a7-773">Bootstrap version 3.0.3</span></span>

- <span data-ttu-id="630a7-774">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="630a7-774">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js</span></span>
- <span data-ttu-id="630a7-775">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-775">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js</span></span>
- <span data-ttu-id="630a7-776">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="630a7-776">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css</span></span>
- <span data-ttu-id="630a7-777">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="630a7-777">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css</span></span>
- <span data-ttu-id="630a7-778">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="630a7-778">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="630a7-779">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="630a7-779">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="630a7-780">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="630a7-780">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="630a7-781">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg</span><span class="sxs-lookup"><span data-stu-id="630a7-781">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="630a7-782">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="630a7-782">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="630a7-783">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="630a7-783">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-302"></a><span data-ttu-id="630a7-784">ブートス トラップ バージョン 3.0.2</span><span class="sxs-lookup"><span data-stu-id="630a7-784">Bootstrap version 3.0.2</span></span>

- <span data-ttu-id="630a7-785">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="630a7-785">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js</span></span>
- <span data-ttu-id="630a7-786">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-786">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js</span></span>
- <span data-ttu-id="630a7-787">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="630a7-787">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css</span></span>
- <span data-ttu-id="630a7-788">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="630a7-788">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="630a7-789">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="630a7-789">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="630a7-790">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="630a7-790">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="630a7-791">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="630a7-791">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="630a7-792">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg</span><span class="sxs-lookup"><span data-stu-id="630a7-792">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="630a7-793">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="630a7-793">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="630a7-794">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="630a7-794">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-301"></a><span data-ttu-id="630a7-795">ブートス トラップ バージョン 3.0.1</span><span class="sxs-lookup"><span data-stu-id="630a7-795">Bootstrap version 3.0.1</span></span>

- <span data-ttu-id="630a7-796">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="630a7-796">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js</span></span>
- <span data-ttu-id="630a7-797">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-797">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js</span></span>
- <span data-ttu-id="630a7-798">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="630a7-798">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css</span></span>
- <span data-ttu-id="630a7-799">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="630a7-799">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="630a7-800">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="630a7-800">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="630a7-801">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="630a7-801">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="630a7-802">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="630a7-802">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="630a7-803">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg</span><span class="sxs-lookup"><span data-stu-id="630a7-803">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="630a7-804">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="630a7-804">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="630a7-805">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="630a7-805">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-300"></a><span data-ttu-id="630a7-806">ブートス トラップ 3.0.0</span><span class="sxs-lookup"><span data-stu-id="630a7-806">Bootstrap version 3.0.0</span></span>

- <span data-ttu-id="630a7-807">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="630a7-807">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js</span></span>
- <span data-ttu-id="630a7-808">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-808">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js</span></span>
- <span data-ttu-id="630a7-809">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="630a7-809">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css</span></span>
- <span data-ttu-id="630a7-810">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="630a7-810">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="630a7-811">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="630a7-811">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="630a7-812">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="630a7-812">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="630a7-813">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="630a7-813">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="630a7-814">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg</span><span class="sxs-lookup"><span data-stu-id="630a7-814">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="630a7-815">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="630a7-815">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="630a7-816">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="630a7-816">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-232"></a><span data-ttu-id="630a7-817">ブートス トラップ バージョン 2.3.2</span><span class="sxs-lookup"><span data-stu-id="630a7-817">Bootstrap version 2.3.2</span></span>

- <span data-ttu-id="630a7-818">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="630a7-818">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js</span></span>
- <span data-ttu-id="630a7-819">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-819">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js</span></span>
- <span data-ttu-id="630a7-820">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="630a7-820">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css</span></span>
- <span data-ttu-id="630a7-821">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="630a7-821">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="630a7-822">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css</span><span class="sxs-lookup"><span data-stu-id="630a7-822">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css</span></span>
- <span data-ttu-id="630a7-823">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css</span><span class="sxs-lookup"><span data-stu-id="630a7-823">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css</span></span>
- <span data-ttu-id="630a7-824">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png</span><span class="sxs-lookup"><span data-stu-id="630a7-824">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png</span></span>
- <span data-ttu-id="630a7-825">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png</span><span class="sxs-lookup"><span data-stu-id="630a7-825">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png</span></span>

#### <a name="bootstrap-version-231"></a><span data-ttu-id="630a7-826">ブートス トラップ version 2.3.1</span><span class="sxs-lookup"><span data-stu-id="630a7-826">Bootstrap version 2.3.1</span></span>

- <span data-ttu-id="630a7-827">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="630a7-827">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js</span></span>
- <span data-ttu-id="630a7-828">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-828">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js</span></span>
- <span data-ttu-id="630a7-829">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="630a7-829">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css</span></span>
- <span data-ttu-id="630a7-830">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="630a7-830">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="630a7-831">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css</span><span class="sxs-lookup"><span data-stu-id="630a7-831">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css</span></span>
- <span data-ttu-id="630a7-832">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css</span><span class="sxs-lookup"><span data-stu-id="630a7-832">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css</span></span>
- <span data-ttu-id="630a7-833">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png</span><span class="sxs-lookup"><span data-stu-id="630a7-833">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png</span></span>
- <span data-ttu-id="630a7-834">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png</span><span class="sxs-lookup"><span data-stu-id="630a7-834">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png</span></span>

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="630a7-835">CDN のブートス トラップ TouchCarousel リリース</span><span class="sxs-lookup"><span data-stu-id="630a7-835">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="630a7-836">次のリリースの[https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel")ブートス トラップ TouchCarousel リリースが CDN でホストされています。:</span><span class="sxs-lookup"><span data-stu-id="630a7-836">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="630a7-837">ブートス トラップ TouchCarousel バージョン 0.8.0 から</span><span class="sxs-lookup"><span data-stu-id="630a7-837">Bootstrap TouchCarousel version 0.8.0</span></span>

- <span data-ttu-id="630a7-838">http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css</span><span class="sxs-lookup"><span data-stu-id="630a7-838">http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css</span></span>
- <span data-ttu-id="630a7-839">http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js</span><span class="sxs-lookup"><span data-stu-id="630a7-839">http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js</span></span>

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="630a7-840">CDN の Hammer.js リリース</span><span class="sxs-lookup"><span data-stu-id="630a7-840">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="630a7-841">次のリリースの[http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js リリースが CDN でホストされています。</span><span class="sxs-lookup"><span data-stu-id="630a7-841">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="630a7-842">Hammer.js バージョン 2.0.4</span><span class="sxs-lookup"><span data-stu-id="630a7-842">Hammer.js version 2.0.4</span></span>

- <span data-ttu-id="630a7-843">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js</span><span class="sxs-lookup"><span data-stu-id="630a7-843">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js</span></span>
- <span data-ttu-id="630a7-844">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-844">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js</span></span>
- <span data-ttu-id="630a7-845">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map</span><span class="sxs-lookup"><span data-stu-id="630a7-845">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map</span></span>

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="630a7-846">ASP.NET Web フォームと Ajax CDN のリリース</span><span class="sxs-lookup"><span data-stu-id="630a7-846">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="630a7-847">CDN では、ASP.NET Ajax ライブラリの次のリリースがホストされています。</span><span class="sxs-lookup"><span data-stu-id="630a7-847">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="630a7-848">実際のファイルの一覧を表示するには、各リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="630a7-848">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="630a7-849">ASP.NET Web フォームと Ajax にバージョン 4.5.2</span><span class="sxs-lookup"><span data-stu-id="630a7-849">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "Ajax 4.5.2 and ASP.NET Web フォーム")
- [<span data-ttu-id="630a7-850">ASP.NET Web フォームと Ajax バージョン 4</span><span class="sxs-lookup"><span data-stu-id="630a7-850">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "Ajax 4 and ASP.NET Web フォーム")
- [<span data-ttu-id="630a7-851">ASP.NET Ajax version 3.5</span><span class="sxs-lookup"><span data-stu-id="630a7-851">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "ASP.NET Ajax 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="630a7-852">ASP.NET MVC が CDN でリリースします。</span><span class="sxs-lookup"><span data-stu-id="630a7-852">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="630a7-853">この CDN では、次の ASP.NET MVC の JavaScript ファイルがホストされています。</span><span class="sxs-lookup"><span data-stu-id="630a7-853">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="630a7-854">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="630a7-854">ASP.NET MVC 5.2.3</span></span>

- <span data-ttu-id="630a7-855">http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="630a7-855">http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="630a7-856">http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-856">http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="630a7-857">ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="630a7-857">ASP.NET MVC 5.1</span></span>

- <span data-ttu-id="630a7-858">http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="630a7-858">http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="630a7-859">http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-859">http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="630a7-860">ASP.NET MVC 5.0</span><span class="sxs-lookup"><span data-stu-id="630a7-860">ASP.NET MVC 5.0</span></span>

- <span data-ttu-id="630a7-861">http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="630a7-861">http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="630a7-862">http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-862">http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="630a7-863">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="630a7-863">ASP.NET MVC 4.0</span></span>

- <span data-ttu-id="630a7-864">http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="630a7-864">http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="630a7-865">http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-865">http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="630a7-866">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="630a7-866">ASP.NET MVC 3.0</span></span>

- <span data-ttu-id="630a7-867">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js</span><span class="sxs-lookup"><span data-stu-id="630a7-867">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js</span></span>
- <span data-ttu-id="630a7-868">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-868">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js</span></span>
- <span data-ttu-id="630a7-869">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="630a7-869">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="630a7-870">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-870">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js</span></span>
- <span data-ttu-id="630a7-871">http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="630a7-871">http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="630a7-872">http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js</span><span class="sxs-lookup"><span data-stu-id="630a7-872">http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js</span></span>

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="630a7-873">ASP.NET MVC 2.0</span><span class="sxs-lookup"><span data-stu-id="630a7-873">ASP.NET MVC 2.0</span></span>

- <span data-ttu-id="630a7-874">http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="630a7-874">http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="630a7-875">http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js</span><span class="sxs-lookup"><span data-stu-id="630a7-875">http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js</span></span>

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="630a7-876">ASP.NET MVC 1.0</span><span class="sxs-lookup"><span data-stu-id="630a7-876">ASP.NET MVC 1.0</span></span>

- <span data-ttu-id="630a7-877">http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="630a7-877">http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="630a7-878">http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js</span><span class="sxs-lookup"><span data-stu-id="630a7-878">http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js</span></span>

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="630a7-879">ASP.NET SignalR を CDN で解放します。</span><span class="sxs-lookup"><span data-stu-id="630a7-879">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="630a7-880">この CDN では、次の ASP.NET SignalR JavaScript ファイルがホストされています。</span><span class="sxs-lookup"><span data-stu-id="630a7-880">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="630a7-881">ASP.NET SignalR 2.2.2</span><span class="sxs-lookup"><span data-stu-id="630a7-881">ASP.NET SignalR 2.2.2</span></span>

- <span data-ttu-id="630a7-882">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-882">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js</span></span>
- <span data-ttu-id="630a7-883">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js</span><span class="sxs-lookup"><span data-stu-id="630a7-883">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js</span></span>

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="630a7-884">ASP.NET SignalR 2.2.1</span><span class="sxs-lookup"><span data-stu-id="630a7-884">ASP.NET SignalR 2.2.1</span></span>

- <span data-ttu-id="630a7-885">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-885">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js</span></span>
- <span data-ttu-id="630a7-886">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="630a7-886">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js</span></span>

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="630a7-887">ASP.NET SignalR 2.2.0</span><span class="sxs-lookup"><span data-stu-id="630a7-887">ASP.NET SignalR 2.2.0</span></span>

- <span data-ttu-id="630a7-888">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-888">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js</span></span>
- <span data-ttu-id="630a7-889">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="630a7-889">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js</span></span>

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="630a7-890">ASP.NET SignalR 2.1.0</span><span class="sxs-lookup"><span data-stu-id="630a7-890">ASP.NET SignalR 2.1.0</span></span>

- <span data-ttu-id="630a7-891">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-891">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js</span></span>
- <span data-ttu-id="630a7-892">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="630a7-892">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js</span></span>

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="630a7-893">ASP.NET SignalR 2.0.3</span><span class="sxs-lookup"><span data-stu-id="630a7-893">ASP.NET SignalR 2.0.3</span></span>

- <span data-ttu-id="630a7-894">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-894">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js</span></span>
- <span data-ttu-id="630a7-895">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js</span><span class="sxs-lookup"><span data-stu-id="630a7-895">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js</span></span>

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="630a7-896">ASP.NET SignalR 2.0.2</span><span class="sxs-lookup"><span data-stu-id="630a7-896">ASP.NET SignalR 2.0.2</span></span>

- <span data-ttu-id="630a7-897">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-897">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js</span></span>
- <span data-ttu-id="630a7-898">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js</span><span class="sxs-lookup"><span data-stu-id="630a7-898">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js</span></span>

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="630a7-899">ASP.NET SignalR 2.0.1</span><span class="sxs-lookup"><span data-stu-id="630a7-899">ASP.NET SignalR 2.0.1</span></span>

- <span data-ttu-id="630a7-900">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-900">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js</span></span>
- <span data-ttu-id="630a7-901">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js</span><span class="sxs-lookup"><span data-stu-id="630a7-901">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js</span></span>

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="630a7-902">ASP.NET SignalR 2.0.0</span><span class="sxs-lookup"><span data-stu-id="630a7-902">ASP.NET SignalR 2.0.0</span></span>

- <span data-ttu-id="630a7-903">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-903">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js</span></span>
- <span data-ttu-id="630a7-904">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js</span><span class="sxs-lookup"><span data-stu-id="630a7-904">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js</span></span>

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="630a7-905">ASP.NET SignalR 1.1.3</span><span class="sxs-lookup"><span data-stu-id="630a7-905">ASP.NET SignalR 1.1.3</span></span>

- <span data-ttu-id="630a7-906">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-906">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js</span></span>
- <span data-ttu-id="630a7-907">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js</span><span class="sxs-lookup"><span data-stu-id="630a7-907">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js</span></span>

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="630a7-908">ASP.NET SignalR 1.1.2</span><span class="sxs-lookup"><span data-stu-id="630a7-908">ASP.NET SignalR 1.1.2</span></span>

- <span data-ttu-id="630a7-909">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-909">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js</span></span>
- <span data-ttu-id="630a7-910">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js</span><span class="sxs-lookup"><span data-stu-id="630a7-910">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js</span></span>

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="630a7-911">ASP.NET SignalR 1.1.1</span><span class="sxs-lookup"><span data-stu-id="630a7-911">ASP.NET SignalR 1.1.1</span></span>

- <span data-ttu-id="630a7-912">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-912">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js</span></span>
- <span data-ttu-id="630a7-913">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js</span><span class="sxs-lookup"><span data-stu-id="630a7-913">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js</span></span>

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="630a7-914">ASP.NET SignalR 1.1.0</span><span class="sxs-lookup"><span data-stu-id="630a7-914">ASP.NET SignalR 1.1.0</span></span>

- <span data-ttu-id="630a7-915">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-915">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js</span></span>
- <span data-ttu-id="630a7-916">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js</span><span class="sxs-lookup"><span data-stu-id="630a7-916">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js</span></span>

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="630a7-917">ASP.NET SignalR 1.0.1</span><span class="sxs-lookup"><span data-stu-id="630a7-917">ASP.NET SignalR 1.0.1</span></span>

- <span data-ttu-id="630a7-918">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="630a7-918">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js</span></span>
- <span data-ttu-id="630a7-919">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js</span><span class="sxs-lookup"><span data-stu-id="630a7-919">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js</span></span>

<span data-ttu-id="630a7-920">CDN の使用条件については、次を参照してください。 [Microsoft Ajax CDN の使用条件](https://www.asp.net/terms-of-use "Microsoft Ajax CDN の使用条件")です。</span><span class="sxs-lookup"><span data-stu-id="630a7-920">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>
