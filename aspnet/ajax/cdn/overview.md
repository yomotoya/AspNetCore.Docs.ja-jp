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
ms.openlocfilehash: f1225f06e5218d893e3f49b2ccc67d56365b30e5
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
---
<a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="ae2f7-102">Microsoft Ajax コンテンツ配信ネットワーク</span><span class="sxs-lookup"><span data-stu-id="ae2f7-102">Microsoft Ajax Content Delivery Network</span></span>
====================
<span data-ttu-id="ae2f7-103">注: Microsoft Ajax CDN を持たない SLA の Azure CDN を使用する他にします。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-103">Note: The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="ae2f7-104">目次</span><span class="sxs-lookup"><span data-stu-id="ae2f7-104">Table of Contents</span></span>

<span data-ttu-id="ae2f7-105">**[ajax.microsoft.com ajax.aspnetcdn.com に変更されました](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="ae2f7-105">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="ae2f7-106">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="ae2f7-106">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="ae2f7-107">**[CDN から ASP.NET Ajax を使用します。](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="ae2f7-107">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="ae2f7-108">**[CDN から jQuery の使用](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="ae2f7-108">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="ae2f7-109">**[JQuery UI、CDN からの使用](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="ae2f7-109">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="ae2f7-110">**[CDN でサード パーティ製ファイル](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="ae2f7-110">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="ae2f7-111">CDN の jQuery リリース</span><span class="sxs-lookup"><span data-stu-id="ae2f7-111">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="ae2f7-112">CDN の jQuery 移行リリース</span><span class="sxs-lookup"><span data-stu-id="ae2f7-112">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="ae2f7-113">jQuery UI、CDN のリリース</span><span class="sxs-lookup"><span data-stu-id="ae2f7-113">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="ae2f7-114">jQuery 検証 CDN のリリース</span><span class="sxs-lookup"><span data-stu-id="ae2f7-114">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="ae2f7-115">jQuery Mobile、CDN のリリース</span><span class="sxs-lookup"><span data-stu-id="ae2f7-115">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="ae2f7-116">jQuery CDN でテンプレートのリリース</span><span class="sxs-lookup"><span data-stu-id="ae2f7-116">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="ae2f7-117">jQuery CDN のサイクル リリース</span><span class="sxs-lookup"><span data-stu-id="ae2f7-117">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="ae2f7-118">jQuery CDN の Datatable リリース</span><span class="sxs-lookup"><span data-stu-id="ae2f7-118">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="ae2f7-119">CDN の Modernizr リリース</span><span class="sxs-lookup"><span data-stu-id="ae2f7-119">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="ae2f7-120">CDN の JSHint リリース</span><span class="sxs-lookup"><span data-stu-id="ae2f7-120">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="ae2f7-121">CDN の knockout リリース</span><span class="sxs-lookup"><span data-stu-id="ae2f7-121">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="ae2f7-122">CDN のリリースをグローバル化します。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-122">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="ae2f7-123">CDN のリリースを応答します。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-123">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="ae2f7-124">CDN でブートス トラップのリリース</span><span class="sxs-lookup"><span data-stu-id="ae2f7-124">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="ae2f7-125">CDN のブートス トラップ TouchCarousel リリース</span><span class="sxs-lookup"><span data-stu-id="ae2f7-125">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="ae2f7-126">CDN の Hammer.js リリース</span><span class="sxs-lookup"><span data-stu-id="ae2f7-126">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="ae2f7-127">ASP.NET Web フォームと Ajax CDN のリリース</span><span class="sxs-lookup"><span data-stu-id="ae2f7-127">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="ae2f7-128">ASP.NET MVC が CDN でリリースします。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-128">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="ae2f7-129">ASP.NET SignalR を CDN で解放します。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-129">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="ae2f7-130">Microsoft Ajax コンテンツ配信ネットワーク (CDN) では、jQuery などのサード パーティ製の一般的な JavaScript ライブラリをホストし、それらを Web アプリケーションに簡単に追加することができます。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-130">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="ae2f7-131">この CDN に追加するだけでホストされている jQuery の使用を開始するなど、&lt;スクリプト&gt;ajax.aspnetcdn.com を指すページへのタグ。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-131">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="ae2f7-132">CDN を利用して、Ajax アプリケーションのパフォーマンスを大幅に向上させることができます。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-132">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="ae2f7-133">CDN の内容は、世界各地に配置されたサーバーでキャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-133">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="ae2f7-134">さらに、CDN は、別のドメインに配置されている web サイトのキャッシュされたサード パーティ製の JavaScript ファイルを再利用するブラウザーを使用できます。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-134">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="ae2f7-135">CDN は、Secure Sockets Layer を使用し、web ページを使用する必要がある場合に、SSL (HTTPS) をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-135">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="ae2f7-136">CDN では、アップロードされていると、これらのライブラリの所有者が、お客様にライセンス供与は、次のサード パーティ スクリプト ライブラリをホストします。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-136">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="ae2f7-137">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="ae2f7-137">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="ae2f7-138">jQuery UI (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="ae2f7-138">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="ae2f7-139">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="ae2f7-139">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="ae2f7-140">jQuery 検証 (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="ae2f7-140">jQuery Validation (www.jquery.com)</span></span>
- <span data-ttu-id="ae2f7-141">jQuery サイクル (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="ae2f7-141">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="ae2f7-142">jQuery Datatable (http://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="ae2f7-142">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="ae2f7-143">Microsoft Ajax CDN には、Microsoft によってアップロードされた次のライブラリが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-143">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="ae2f7-144">ASP.NET Ajax</span><span class="sxs-lookup"><span data-stu-id="ae2f7-144">ASP.NET Ajax</span></span>
- <span data-ttu-id="ae2f7-145">ASP.NET MVC の JavaScript ファイル</span><span class="sxs-lookup"><span data-stu-id="ae2f7-145">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="ae2f7-146">ASP.NET SignalR JavaScript ファイルします。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-146">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="ae2f7-147">Microsoft では、この CDN でホストされているすべてのサード パーティ ライブラリの所有権を主張しません。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-147">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="ae2f7-148">ライブラリの著作権の所有者は、これらのライブラリをライセンスされています。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-148">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="ae2f7-149">それぞれの著作権の所有者によってのみをダウンロードしてこのようなライブラリを使用する必要のある任意の権限が付与されます。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-149">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="ae2f7-150">これらは Microsoft ライブラリではありません、ため Microsoft でない保証または (暗黙の特許権を含むなし) 知的財産権ライセンスをこの CDN でホストされているサードパーティ製ライブラリ。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-150">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="ae2f7-151">場合は、JavaScript ライブラリを送信して、ライブラリは、1 つの最上位の JavaScript ライブラリ (に示されたhttp://trends.builtwith.com)や拡張機能/プラグインには、(a) 人気のある; または (b) ASP.NET で使用するために役立ちますしにお問い合わせくださいこれらのライブラリにAjaxCDNSubmission@Microsoft.comです。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-151">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="ae2f7-152">ajax.microsoft.com ajax.aspnetcdn.com に変更されました</span><span class="sxs-lookup"><span data-stu-id="ae2f7-152">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="ae2f7-153">CDN では、microsoft.com ドメイン名を使用するために使用し、aspnetcdn.com ドメイン名の使用に変更されました。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-153">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="ae2f7-154">ブラウザーには、microsoft.com ドメインが参照されている場合に送信可能なため、クッキーそのドメインから要求ごとにネットワーク経由でパフォーマンスを向上させる、この変更が加えられました。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-154">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="ae2f7-155">Microsoft.com 以外のドメイン名を変更するとパフォーマンスを向上できますで 25% ほどです。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-155">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="ae2f7-156">注 ajax.microsoft.com は引き続き機能しますが、ajax.aspnetcdn.com をお勧めします。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-156">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="ae2f7-157">古い形式: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="ae2f7-157">Old Format: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="ae2f7-158">新しい形式: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="ae2f7-158">New Format: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="ae2f7-159">Visual Studio .vsdoc サポート</span><span class="sxs-lookup"><span data-stu-id="ae2f7-159">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="ae2f7-160">VS 2008 SP1 があることを確認する必要があります。 Visual Studio 2008 に .vsdoc ファイルを適切に使用するインストールし、vsdoc ファイル用の修正プログラムがインストールされています。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-160">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="ae2f7-161">ここではこれらを取得できます。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-161">You can get these from here:</span></span>

- [<span data-ttu-id="ae2f7-162">Visual Studio 2008 SP1 をダウンロード</span><span class="sxs-lookup"><span data-stu-id="ae2f7-162">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "Visual Studio 2008 SP1 をダウンロードします。")
- [<span data-ttu-id="ae2f7-163">Visual Studio 2008 SP1 の .vsdoc 修正プログラムをダウンロード</span><span class="sxs-lookup"><span data-stu-id="ae2f7-163">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 ".vsdoc 修正プログラムを Visual Studio 2008 SP1 のダウンロード")

<span data-ttu-id="ae2f7-164">Visual Studio 2010 では、追加、更新プログラムなし .vsdoc ファイルをサポートします。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-164">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="ae2f7-165">CDN から ASP.NET Ajax を使用します。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-165">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="ae2f7-166">ASP.NET 4 を使用する場合は、ASP.NET framework スクリプトに対するすべての要求を CDN にリダイレクトできます。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-166">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="ae2f7-167">ローカル web サーバーではなく、CDN からスクリプトを取得すると、パブリックの ASP.NET web サイトのパフォーマンスが大幅に向上します。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-167">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="ae2f7-168">Microsoft Ajax CDN を ASP.NET framework スクリプトのすべての要求をリダイレクトするためには、ScriptManager EnableCDN プロパティを使用します。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-168">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="ae2f7-169">CDN から jQuery の使用</span><span class="sxs-lookup"><span data-stu-id="ae2f7-169">Using jQuery from the CDN</span></span>

<span data-ttu-id="ae2f7-170">ページに次のスクリプト要素を追加することで、Web アプリケーション CDN でホストされている jQuery スクリプトを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-170">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="ae2f7-171">CDN は、入手できる jQuery スクリプトの縮小されたバージョンも含まれています。 次の要素を使用します。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-171">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="ae2f7-172">フォールバック CDN が使用できない場合、独自の web サイト上のローカル パスからの jQuery の読み込みを使用してページを許可するのには、CDN を参照する要素の直後後次の要素を追加します。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-172">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="ae2f7-173">次のサンプルのページは、ボタンがクリックされたときに、div 要素の内容を表示する (ローカル コピーへのフォールバック) とともに jQuery ライブラリの CDN バージョンを使用します。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-173">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="ae2f7-174">詳細については、jQuery およびにアクセスして jQuery のローカル コピーをダウンロードすることができます、 [jQuery](http://jquery.com/) Web サイトです。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-174">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="ae2f7-175">JQuery UI、CDN からの使用</span><span class="sxs-lookup"><span data-stu-id="ae2f7-175">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="ae2f7-176">CDN では、jQuery UI ライブラリもホストします。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-176">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="ae2f7-177">JQuery UI ライブラリには、ウィジェットと、ASP.NET アプリケーションで使用できる特殊効果の豊富なセットが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-177">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="ae2f7-178">たとえば、次のページは、ポップアップ カレンダーを表示する ASP.NET Web フォーム アプリケーションのコンテキストで jQuery UI Datepicker を使用する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-178">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="ae2f7-179">キーボードを使用してテキスト ボックスにフォーカスを移動すると、カレンダーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-179">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![作成 Datepicker ポップアップ カレンダー](overview/_static/image1.png)

<span data-ttu-id="ae2f7-181">上記のコードで、CDN から 3 つのファイルを含める必要があることを確認します。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-181">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="ae2f7-182">JQuery ライブラリ&mdash;jQuery UI ライブラリは、jQuery ライブラリに依存します。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-182">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="ae2f7-183">JQuery UI ライブラリを追加する前に、ページに jQuery ライブラリを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-183">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="ae2f7-184">JQuery UI ライブラリ&mdash;jQuery UI ライブラリには、すべてのページの上部で使用される Datepicker ウィジェットなどのウィジェットと jQuery UI 効果が含まれています。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-184">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="ae2f7-185">JQuery UI のテーマ&mdash;jQuery UI には、さまざまなテーマがサポートしています。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-185">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="ae2f7-186">ページ上部にはには、Redmond のテーマをインポートする CSS ファイルへのリンクが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-186">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="ae2f7-187">すべての標準的な jQuery UI のテーマは、CDN でホストされます。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-187">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="ae2f7-188">[このページを参照してください](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 Microsoft Ajax CDN の")各テーマのサムネイルを表示します。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-188">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="ae2f7-189">JQuery UI ライブラリに関する詳細については、公式にアクセスして[jQuery UI の web サイト](http://jQueryUI.com "jQuery UI の web サイト")です。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-189">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="ae2f7-190">CDN でサード パーティ製ファイル</span><span class="sxs-lookup"><span data-stu-id="ae2f7-190">Third-Party Files on the CDN</span></span>

<span data-ttu-id="ae2f7-191">CDN では、最も一般的なサード パーティ製の JavaScript ライブラリの一部をホストします。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-191">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="ae2f7-192">Microsoft では、この CDN でホストされているすべてのサード パーティ ライブラリの所有権を主張しません。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-192">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="ae2f7-193">ライブラリの著作権の所有者は、これらのライブラリをライセンスされています。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-193">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="ae2f7-194">それぞれの著作権の所有者によってのみをダウンロードしてこのようなライブラリを使用する必要のある任意の権限が付与されます。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-194">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="ae2f7-195">これらは Microsoft ライブラリではありません、ため Microsoft でない保証または (暗黙の特許権を含むなし) 知的財産権ライセンスをこの CDN でホストされているサードパーティ製ライブラリ。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-195">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="ae2f7-196">CDN の jQuery リリース</span><span class="sxs-lookup"><span data-stu-id="ae2f7-196">jQuery Releases on the CDN</span></span>

<span data-ttu-id="ae2f7-197">CDN では、jQuery の次のリリースがホストされています。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-197">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-331"></a><span data-ttu-id="ae2f7-198">jQuery バージョン 3.3.1</span><span class="sxs-lookup"><span data-stu-id="ae2f7-198">jQuery version 3.3.1</span></span>
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.map

#### <a name="jquery-version-321"></a><span data-ttu-id="ae2f7-199">jQuery バージョン 3.2.1</span><span class="sxs-lookup"><span data-stu-id="ae2f7-199">jQuery version 3.2.1</span></span>
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map

#### <a name="jquery-version-320"></a><span data-ttu-id="ae2f7-200">jQuery バージョン 3.2.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-200">jQuery version 3.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map

#### <a name="jquery-version-311"></a><span data-ttu-id="ae2f7-201">jQuery バージョン 3.1.1</span><span class="sxs-lookup"><span data-stu-id="ae2f7-201">jQuery version 3.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map

#### <a name="jquery-version-310"></a><span data-ttu-id="ae2f7-202">jQuery バージョン 3.1.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-202">jQuery version 3.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map

#### <a name="jquery-version-300"></a><span data-ttu-id="ae2f7-203">jQuery 3.0.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-203">jQuery version 3.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map

#### <a name="jquery-version-224"></a><span data-ttu-id="ae2f7-204">jQuery バージョン 2.2.4</span><span class="sxs-lookup"><span data-stu-id="ae2f7-204">jQuery version 2.2.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map

#### <a name="jquery-version-223"></a><span data-ttu-id="ae2f7-205">jQuery バージョン 2.2.3</span><span class="sxs-lookup"><span data-stu-id="ae2f7-205">jQuery version 2.2.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map

#### <a name="jquery-version-222"></a><span data-ttu-id="ae2f7-206">jQuery バージョン 2.2.2</span><span class="sxs-lookup"><span data-stu-id="ae2f7-206">jQuery version 2.2.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map

#### <a name="jquery-version-221"></a><span data-ttu-id="ae2f7-207">jQuery バージョン 2.2.1</span><span class="sxs-lookup"><span data-stu-id="ae2f7-207">jQuery version 2.2.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map

#### <a name="jquery-version-220"></a><span data-ttu-id="ae2f7-208">jQuery 2.2.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-208">jQuery version 2.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map

#### <a name="jquery-version-214"></a><span data-ttu-id="ae2f7-209">jQuery バージョン 2.1.4</span><span class="sxs-lookup"><span data-stu-id="ae2f7-209">jQuery version 2.1.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map

#### <a name="jquery-version-213"></a><span data-ttu-id="ae2f7-210">jQuery バージョン 2.1.3</span><span class="sxs-lookup"><span data-stu-id="ae2f7-210">jQuery version 2.1.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map

#### <a name="jquery-version-212"></a><span data-ttu-id="ae2f7-211">jQuery バージョン 2.1.2</span><span class="sxs-lookup"><span data-stu-id="ae2f7-211">jQuery version 2.1.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js

#### <a name="jquery-version-211"></a><span data-ttu-id="ae2f7-212">jQuery バージョン 2.1.1</span><span class="sxs-lookup"><span data-stu-id="ae2f7-212">jQuery version 2.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map

#### <a name="jquery-version-210"></a><span data-ttu-id="ae2f7-213">jQuery バージョン 2.1.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-213">jQuery version 2.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map

#### <a name="jquery-version-203"></a><span data-ttu-id="ae2f7-214">jQuery バージョン 2.0.3</span><span class="sxs-lookup"><span data-stu-id="ae2f7-214">jQuery version 2.0.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map

#### <a name="jquery-version-202"></a><span data-ttu-id="ae2f7-215">jQuery バージョン 2.0.2</span><span class="sxs-lookup"><span data-stu-id="ae2f7-215">jQuery version 2.0.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map

#### <a name="jquery-version-201"></a><span data-ttu-id="ae2f7-216">jQuery 2.0.1</span><span class="sxs-lookup"><span data-stu-id="ae2f7-216">jQuery version 2.0.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map

#### <a name="jquery-version-200"></a><span data-ttu-id="ae2f7-217">jQuery version 2.0.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-217">jQuery version 2.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map

#### <a name="jquery-version-1124"></a><span data-ttu-id="ae2f7-218">jQuery バージョン 1.12.4</span><span class="sxs-lookup"><span data-stu-id="ae2f7-218">jQuery version 1.12.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map

#### <a name="jquery-version-1123"></a><span data-ttu-id="ae2f7-219">jQuery バージョン 1.12.3</span><span class="sxs-lookup"><span data-stu-id="ae2f7-219">jQuery version 1.12.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map

#### <a name="jquery-version-1122"></a><span data-ttu-id="ae2f7-220">jQuery バージョン 1.12.2</span><span class="sxs-lookup"><span data-stu-id="ae2f7-220">jQuery version 1.12.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map

#### <a name="jquery-version-1121"></a><span data-ttu-id="ae2f7-221">jQuery バージョン 1.12.1</span><span class="sxs-lookup"><span data-stu-id="ae2f7-221">jQuery version 1.12.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map

#### <a name="jquery-version-1120"></a><span data-ttu-id="ae2f7-222">jQuery バージョン 1.12.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-222">jQuery version 1.12.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map

#### <a name="jquery-version-1113"></a><span data-ttu-id="ae2f7-223">jQuery バージョン 1.11.3</span><span class="sxs-lookup"><span data-stu-id="ae2f7-223">jQuery version 1.11.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map

#### <a name="jquery-version-1112"></a><span data-ttu-id="ae2f7-224">jQuery バージョン 1.11.2</span><span class="sxs-lookup"><span data-stu-id="ae2f7-224">jQuery version 1.11.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map

#### <a name="jquery-version-1111"></a><span data-ttu-id="ae2f7-225">jQuery バージョン 1.11.1</span><span class="sxs-lookup"><span data-stu-id="ae2f7-225">jQuery version 1.11.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map

#### <a name="jquery-version-1110"></a><span data-ttu-id="ae2f7-226">jQuery バージョン 1.11.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-226">jQuery version 1.11.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map

#### <a name="jquery-version-1102"></a><span data-ttu-id="ae2f7-227">jQuery バージョン 1.10.2</span><span class="sxs-lookup"><span data-stu-id="ae2f7-227">jQuery version 1.10.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map

#### <a name="jquery-version-1101"></a><span data-ttu-id="ae2f7-228">jQuery バージョン 1.10.1</span><span class="sxs-lookup"><span data-stu-id="ae2f7-228">jQuery version 1.10.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map

#### <a name="jquery-version-1100"></a><span data-ttu-id="ae2f7-229">jQuery バージョン 1.10.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-229">jQuery version 1.10.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map

#### <a name="jquery-version-191"></a><span data-ttu-id="ae2f7-230">jQuery バージョン 1.9.1</span><span class="sxs-lookup"><span data-stu-id="ae2f7-230">jQuery version 1.9.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map

#### <a name="jquery-version-190"></a><span data-ttu-id="ae2f7-231">jQuery バージョン 1.9.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-231">jQuery version 1.9.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map

#### <a name="jquery-version-183"></a><span data-ttu-id="ae2f7-232">jQuery バージョン 1.8.3</span><span class="sxs-lookup"><span data-stu-id="ae2f7-232">jQuery version 1.8.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js

#### <a name="jquery-version-182"></a><span data-ttu-id="ae2f7-233">jQuery バージョン 1.8.2</span><span class="sxs-lookup"><span data-stu-id="ae2f7-233">jQuery version 1.8.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js

#### <a name="jquery-version-181"></a><span data-ttu-id="ae2f7-234">jQuery バージョン 1.8.1</span><span class="sxs-lookup"><span data-stu-id="ae2f7-234">jQuery version 1.8.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js

#### <a name="jquery-version-180"></a><span data-ttu-id="ae2f7-235">jQuery バージョン 1.8.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-235">jQuery version 1.8.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js

#### <a name="jquery-version-172"></a><span data-ttu-id="ae2f7-236">jQuery バージョン 1.7.2</span><span class="sxs-lookup"><span data-stu-id="ae2f7-236">jQuery version 1.7.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js

#### <a name="jquery-version-171"></a><span data-ttu-id="ae2f7-237">jQuery バージョン 1.7.1</span><span class="sxs-lookup"><span data-stu-id="ae2f7-237">jQuery version 1.7.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js

#### <a name="jquery-version-17"></a><span data-ttu-id="ae2f7-238">jQuery バージョン 1.7</span><span class="sxs-lookup"><span data-stu-id="ae2f7-238">jQuery version 1.7</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js

#### <a name="jquery-version-164"></a><span data-ttu-id="ae2f7-239">jQuery バージョン 1.6.4</span><span class="sxs-lookup"><span data-stu-id="ae2f7-239">jQuery version 1.6.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js

#### <a name="jquery-version-163"></a><span data-ttu-id="ae2f7-240">jQuery バージョン 1.6.3</span><span class="sxs-lookup"><span data-stu-id="ae2f7-240">jQuery version 1.6.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js

#### <a name="jquery-version-162"></a><span data-ttu-id="ae2f7-241">jQuery バージョン 1.6.2 でした。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-241">jQuery version 1.6.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js

#### <a name="jquery-version-161"></a><span data-ttu-id="ae2f7-242">jQuery バージョン 1.6.1</span><span class="sxs-lookup"><span data-stu-id="ae2f7-242">jQuery version 1.6.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js

#### <a name="jquery-version-16"></a><span data-ttu-id="ae2f7-243">jQuery バージョン 1.6</span><span class="sxs-lookup"><span data-stu-id="ae2f7-243">jQuery version 1.6</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js

#### <a name="jquery-version-152"></a><span data-ttu-id="ae2f7-244">jQuery バージョン 1.5.2</span><span class="sxs-lookup"><span data-stu-id="ae2f7-244">jQuery version 1.5.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js

#### <a name="jquery-version-151"></a><span data-ttu-id="ae2f7-245">jQuery バージョン 1.5.1</span><span class="sxs-lookup"><span data-stu-id="ae2f7-245">jQuery version 1.5.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js

#### <a name="jquery-version-15"></a><span data-ttu-id="ae2f7-246">jQuery 1.5</span><span class="sxs-lookup"><span data-stu-id="ae2f7-246">jQuery version 1.5</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js

#### <a name="jquery-version-144"></a><span data-ttu-id="ae2f7-247">jQuery バージョン 1.4.4</span><span class="sxs-lookup"><span data-stu-id="ae2f7-247">jQuery version 1.4.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js

#### <a name="jquery-version-143"></a><span data-ttu-id="ae2f7-248">jQuery バージョン 1.4.3</span><span class="sxs-lookup"><span data-stu-id="ae2f7-248">jQuery version 1.4.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js

#### <a name="jquery-version-142"></a><span data-ttu-id="ae2f7-249">jQuery バージョン 1.4.2</span><span class="sxs-lookup"><span data-stu-id="ae2f7-249">jQuery version 1.4.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js

#### <a name="jquery-version-141"></a><span data-ttu-id="ae2f7-250">jQuery バージョン 1.4.1</span><span class="sxs-lookup"><span data-stu-id="ae2f7-250">jQuery version 1.4.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js

#### <a name="jquery-version-14"></a><span data-ttu-id="ae2f7-251">jQuery バージョン 1.4</span><span class="sxs-lookup"><span data-stu-id="ae2f7-251">jQuery version 1.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js

#### <a name="jquery-version-132"></a><span data-ttu-id="ae2f7-252">jQuery バージョン 1.3.2</span><span class="sxs-lookup"><span data-stu-id="ae2f7-252">jQuery version 1.3.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="ae2f7-253">CDN の jQuery 移行リリース</span><span class="sxs-lookup"><span data-stu-id="ae2f7-253">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="ae2f7-254">CDN では、jQuery の移行の次のリリースがホストされています。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-254">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="ae2f7-255">jQuery 3.0.0 の移行</span><span class="sxs-lookup"><span data-stu-id="ae2f7-255">jQuery Migrate version 3.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="ae2f7-256">jQuery 1.2.1 の移行</span><span class="sxs-lookup"><span data-stu-id="ae2f7-256">jQuery Migrate version 1.2.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js

<span data-ttu-id="ae2f7-257">jQuery バージョン 1.2.0 の移行</span><span class="sxs-lookup"><span data-stu-id="ae2f7-257">jQuery Migrate version 1.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="ae2f7-258">jQuery バージョン 1.1.1 の移行</span><span class="sxs-lookup"><span data-stu-id="ae2f7-258">jQuery Migrate version 1.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="ae2f7-259">jQuery version 1.1.0 の移行</span><span class="sxs-lookup"><span data-stu-id="ae2f7-259">jQuery Migrate version 1.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="ae2f7-260">jQuery バージョン 1.0.0 の移行</span><span class="sxs-lookup"><span data-stu-id="ae2f7-260">jQuery Migrate version 1.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="ae2f7-261">jQuery UI、CDN のリリース</span><span class="sxs-lookup"><span data-stu-id="ae2f7-261">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="ae2f7-262">JQuery UI ライブラリの次のリリースは、この CDN でホストされています。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-262">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="ae2f7-263">実際のファイルの一覧を表示するには、各リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-263">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="ae2f7-264">jQuery UI 1.12.1</span><span class="sxs-lookup"><span data-stu-id="ae2f7-264">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "jQuery UI 1.12.1 Microsoft Ajax CDN の")
- [<span data-ttu-id="ae2f7-265">jQuery UI 1.12.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-265">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "jQuery UI 1.12.0 Microsoft Ajax CDN の")
- [<span data-ttu-id="ae2f7-266">jQuery UI 1.11.4</span><span class="sxs-lookup"><span data-stu-id="ae2f7-266">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "jQuery UI 1.11.4 Microsoft Ajax CDN の")
- [<span data-ttu-id="ae2f7-267">jQuery UI 1.11.3</span><span class="sxs-lookup"><span data-stu-id="ae2f7-267">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "jQuery UI 1.11.3 Microsoft Ajax CDN の")
- [<span data-ttu-id="ae2f7-268">jQuery UI 1.11.2</span><span class="sxs-lookup"><span data-stu-id="ae2f7-268">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "jQuery UI 1.11.2 Microsoft Ajax CDN の")
- [<span data-ttu-id="ae2f7-269">jQuery UI 1.11.1</span><span class="sxs-lookup"><span data-stu-id="ae2f7-269">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "jQuery UI 1.11.1 Microsoft Ajax CDN の")
- [<span data-ttu-id="ae2f7-270">jQuery UI 1.11.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-270">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "jQuery UI 1.11.0 Microsoft Ajax CDN の")
- [<span data-ttu-id="ae2f7-271">jQuery UI 1.10.4</span><span class="sxs-lookup"><span data-stu-id="ae2f7-271">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "jQuery UI 1.10.4 Microsoft Ajax CDN の")
- [<span data-ttu-id="ae2f7-272">jQuery UI 1.10.3</span><span class="sxs-lookup"><span data-stu-id="ae2f7-272">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "jQuery UI 1.10.3 Microsoft Ajax CDN の")
- [<span data-ttu-id="ae2f7-273">jQuery UI 1.10.2</span><span class="sxs-lookup"><span data-stu-id="ae2f7-273">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "jQuery UI 1.10.2 Microsoft Ajax CDN の")
- [<span data-ttu-id="ae2f7-274">jQuery UI 1.10.1</span><span class="sxs-lookup"><span data-stu-id="ae2f7-274">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "jQuery UI 1.10.1 Microsoft Ajax CDN の")
- [<span data-ttu-id="ae2f7-275">jQuery UI 1.10.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-275">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "jQuery UI 1.10.0、Microsoft Ajax CDN の")
- [<span data-ttu-id="ae2f7-276">jQuery UI 1.9.2</span><span class="sxs-lookup"><span data-stu-id="ae2f7-276">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "jQuery UI 1.9.2 Microsoft Ajax CDN の")
- [<span data-ttu-id="ae2f7-277">jQuery UI 1.9.1</span><span class="sxs-lookup"><span data-stu-id="ae2f7-277">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "jQuery UI 1.9.1 Microsoft Ajax CDN の")
- [<span data-ttu-id="ae2f7-278">jQuery UI 1.9.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-278">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "jQuery UI 1.9.0 Microsoft Ajax CDN の")
- [<span data-ttu-id="ae2f7-279">jQuery UI 1.8.24</span><span class="sxs-lookup"><span data-stu-id="ae2f7-279">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "jQuery UI 1.8.24 Microsoft Ajax CDN の")
- [<span data-ttu-id="ae2f7-280">jQuery UI 1.8.23</span><span class="sxs-lookup"><span data-stu-id="ae2f7-280">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "jQuery UI 1.8.23 Microsoft Ajax CDN の")
- [<span data-ttu-id="ae2f7-281">jQuery UI 1.8.22</span><span class="sxs-lookup"><span data-stu-id="ae2f7-281">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "jQuery UI 1.8.22 Microsoft Ajax CDN の")
- [<span data-ttu-id="ae2f7-282">jQuery UI 1.8.21</span><span class="sxs-lookup"><span data-stu-id="ae2f7-282">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "jQuery UI 1.8.21 Microsoft Ajax CDN の")
- [<span data-ttu-id="ae2f7-283">jQuery UI 1.8.20</span><span class="sxs-lookup"><span data-stu-id="ae2f7-283">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "jQuery UI 1.8.20 Microsoft Ajax CDN の")
- [<span data-ttu-id="ae2f7-284">jQuery UI 1.8.19</span><span class="sxs-lookup"><span data-stu-id="ae2f7-284">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "jQuery UI 1.8.19 Microsoft Ajax CDN の")
- [<span data-ttu-id="ae2f7-285">jQuery UI 1.8.18</span><span class="sxs-lookup"><span data-stu-id="ae2f7-285">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "jQuery UI 1.8.18 Microsoft Ajax CDN の")
- [<span data-ttu-id="ae2f7-286">jQuery UI 1.8.17</span><span class="sxs-lookup"><span data-stu-id="ae2f7-286">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "jQuery UI 1.8.17 Microsoft Ajax CDN の")
- [<span data-ttu-id="ae2f7-287">jQuery UI 1.8.16</span><span class="sxs-lookup"><span data-stu-id="ae2f7-287">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "jQuery UI 1.8.16 Microsoft Ajax CDN の")
- [<span data-ttu-id="ae2f7-288">jQuery UI 1.8.15</span><span class="sxs-lookup"><span data-stu-id="ae2f7-288">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "jQuery UI 1.8.15 Microsoft Ajax CDN の")
- [<span data-ttu-id="ae2f7-289">jQuery UI 1.8.14</span><span class="sxs-lookup"><span data-stu-id="ae2f7-289">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "jQuery UI 1.8.14 Microsoft Ajax CDN の")
- [<span data-ttu-id="ae2f7-290">jQuery UI 1.8.13</span><span class="sxs-lookup"><span data-stu-id="ae2f7-290">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "jQuery UI 1.8.13 Microsoft Ajax CDN の")
- [<span data-ttu-id="ae2f7-291">jQuery UI 1.8.12</span><span class="sxs-lookup"><span data-stu-id="ae2f7-291">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "jQuery UI 1.8.12 Microsoft Ajax CDN の")
- [<span data-ttu-id="ae2f7-292">jQuery UI 1.8.11</span><span class="sxs-lookup"><span data-stu-id="ae2f7-292">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "jQuery UI 1.8.11 Microsoft Ajax CDN の")
- [<span data-ttu-id="ae2f7-293">jQuery UI 1.8.10</span><span class="sxs-lookup"><span data-stu-id="ae2f7-293">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 Microsoft Ajax CDN の")
- [<span data-ttu-id="ae2f7-294">jQuery UI 1.8.9</span><span class="sxs-lookup"><span data-stu-id="ae2f7-294">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "jQuery UI 1.8.9 Microsoft Ajax CDN の")
- [<span data-ttu-id="ae2f7-295">jQuery UI 1.8.8</span><span class="sxs-lookup"><span data-stu-id="ae2f7-295">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "jQuery UI 1.8.8 Microsoft Ajax CDN の")
- [<span data-ttu-id="ae2f7-296">jQuery UI 1.8.7</span><span class="sxs-lookup"><span data-stu-id="ae2f7-296">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "jQuery UI 1.8.7 Microsoft Ajax CDN の")
- [<span data-ttu-id="ae2f7-297">jQuery UI 1.8.6</span><span class="sxs-lookup"><span data-stu-id="ae2f7-297">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "jQuery UI 1.8.6 Microsoft Ajax CDN の")
- [<span data-ttu-id="ae2f7-298">jQuery UI 1.8.5</span><span class="sxs-lookup"><span data-stu-id="ae2f7-298">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="ae2f7-299">jQuery 検証 CDN のリリース</span><span class="sxs-lookup"><span data-stu-id="ae2f7-299">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="ae2f7-300">JQuery 検証ライブラリの次のリリースは、この CDN でホストされています。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-300">The following releases of the jQuery Validation library are hosted on this CDN.</span></span> <span data-ttu-id="ae2f7-301">実際のファイルの一覧を表示するには、各リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-301">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="ae2f7-302">jQuery 検証 1.17.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-302">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "jQuery 検証 1.17.0")
- [<span data-ttu-id="ae2f7-303">jQuery 検証 1.16.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-303">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "jQuery 検証 1.16.0")
- [<span data-ttu-id="ae2f7-304">jQuery 検証 1.15.1</span><span class="sxs-lookup"><span data-stu-id="ae2f7-304">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "jQuery 検証 1.15.1")
- [<span data-ttu-id="ae2f7-305">jQuery 検証 1.15.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-305">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "jQuery 検証 1.15.0")
- [<span data-ttu-id="ae2f7-306">jQuery 検証 1.14.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-306">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "jQuery 検証 1.14.0")
- [<span data-ttu-id="ae2f7-307">jQuery 検証 1.13.1</span><span class="sxs-lookup"><span data-stu-id="ae2f7-307">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "jQuery 検証 1.13.1")
- [<span data-ttu-id="ae2f7-308">jQuery 検証 1.13.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-308">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "jQuery 検証 1.13.0")
- [<span data-ttu-id="ae2f7-309">jQuery 検証 1.12.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-309">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "jQuery 検証 1.12.0")
- [<span data-ttu-id="ae2f7-310">jQuery 検証 1.11.1</span><span class="sxs-lookup"><span data-stu-id="ae2f7-310">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "jQuery 検証 1.11.1")
- [<span data-ttu-id="ae2f7-311">jQuery 検証 1.11.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-311">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "jQuery 検証 1.11.0")
- [<span data-ttu-id="ae2f7-312">jQuery 検証 1.10.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-312">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "jQuery 検証 1.10.0")
- [<span data-ttu-id="ae2f7-313">jQuery 検証 1.9</span><span class="sxs-lookup"><span data-stu-id="ae2f7-313">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jquery.validate バージョン 1.9")
- [<span data-ttu-id="ae2f7-314">jQuery 検証 1.8.1</span><span class="sxs-lookup"><span data-stu-id="ae2f7-314">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "jquery.validate バージョン 1.8.1")
- [<span data-ttu-id="ae2f7-315">jQuery 検証 1.8</span><span class="sxs-lookup"><span data-stu-id="ae2f7-315">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jquery.validate バージョン 1.8")
- [<span data-ttu-id="ae2f7-316">jQuery Validate 1.7</span><span class="sxs-lookup"><span data-stu-id="ae2f7-316">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jquery.validate version 1.7")
- [<span data-ttu-id="ae2f7-317">jQuery 検証 1.6</span><span class="sxs-lookup"><span data-stu-id="ae2f7-317">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "jQuery 検証 1.6")
- [<span data-ttu-id="ae2f7-318">jQuery 検証 1.5.5</span><span class="sxs-lookup"><span data-stu-id="ae2f7-318">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "jQuery 検証 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="ae2f7-319">jQuery Mobile、CDN のリリース</span><span class="sxs-lookup"><span data-stu-id="ae2f7-319">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="ae2f7-320">JQuery モバイル ライブラリの次のリリースは、この CDN でホストされています。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-320">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="ae2f7-321">実際のファイルの一覧を表示するには、各リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-321">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="ae2f7-322">jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="ae2f7-322">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 Microsoft Ajax CDN を")
- [<span data-ttu-id="ae2f7-323">jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="ae2f7-323">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 Microsoft Ajax CDN を")
- [<span data-ttu-id="ae2f7-324">jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="ae2f7-324">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 Microsoft Ajax CDN を")
- [<span data-ttu-id="ae2f7-325">jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-325">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 Microsoft Ajax CDN を")
- [<span data-ttu-id="ae2f7-326">jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="ae2f7-326">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 Microsoft Ajax CDN を")
- [<span data-ttu-id="ae2f7-327">jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="ae2f7-327">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 Microsoft Ajax CDN を")
- [<span data-ttu-id="ae2f7-328">jQuery Mobile 1.3.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-328">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 Microsoft Ajax CDN を")
- [<span data-ttu-id="ae2f7-329">jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-329">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 Microsoft Ajax CDN を")
- [<span data-ttu-id="ae2f7-330">jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="ae2f7-330">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 Microsoft Ajax CDN を")
- [<span data-ttu-id="ae2f7-331">jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="ae2f7-331">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 Microsoft Ajax CDN を")
- [<span data-ttu-id="ae2f7-332">jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-332">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 Microsoft Ajax CDN を")
- [<span data-ttu-id="ae2f7-333">jQuery Mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="ae2f7-333">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 Microsoft Ajax CDN を")
- [<span data-ttu-id="ae2f7-334">jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="ae2f7-334">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 Microsoft Ajax CDN を")
- [<span data-ttu-id="ae2f7-335">jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-335">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 Microsoft Ajax CDN を")
- [<span data-ttu-id="ae2f7-336">jQuery Mobile 1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="ae2f7-336">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "Microsoft Ajax CDN の jQuery Mobile 1.0 RC2")
- [<span data-ttu-id="ae2f7-337">jQuery Mobile 1.0 RC 1</span><span class="sxs-lookup"><span data-stu-id="ae2f7-337">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "Microsoft Ajax CDN の jQuery Mobile 1.0 RC1")
- [<span data-ttu-id="ae2f7-338">jQuery Mobile 1.0 beta 3</span><span class="sxs-lookup"><span data-stu-id="ae2f7-338">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "Microsoft Ajax CDN の jQuery Mobile 1.0 Beta 3")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="ae2f7-339">jQuery CDN でテンプレートのリリース</span><span class="sxs-lookup"><span data-stu-id="ae2f7-339">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="ae2f7-340">JQuery テンプレートのプラグインの次のリリースは、この CDN でホストされています。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-340">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="ae2f7-341">実際のファイルの一覧を表示するには、各リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-341">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="ae2f7-342">jQuery テンプレート Beta 1</span><span class="sxs-lookup"><span data-stu-id="ae2f7-342">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery テンプレート Beta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="ae2f7-343">jQuery CDN のサイクル リリース</span><span class="sxs-lookup"><span data-stu-id="ae2f7-343">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="ae2f7-344">JQuery サイクル プラグインの次のリリースは、この CDN でホストされています。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-344">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="ae2f7-345">実際のファイルの一覧を表示するには、各リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-345">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="ae2f7-346">jQuery サイクル 2.99</span><span class="sxs-lookup"><span data-stu-id="ae2f7-346">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery サイクル 2.99")
- [<span data-ttu-id="ae2f7-347">jQuery サイクル 2.94</span><span class="sxs-lookup"><span data-stu-id="ae2f7-347">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery サイクル 2.94")
- [<span data-ttu-id="ae2f7-348">jQuery サイクル 2.88</span><span class="sxs-lookup"><span data-stu-id="ae2f7-348">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery サイクル 2.88")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="ae2f7-349">jQuery CDN の Datatable リリース</span><span class="sxs-lookup"><span data-stu-id="ae2f7-349">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="ae2f7-350">JQuery Datatable プラグインの次のリリースは、この CDN でホストされています。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-350">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="ae2f7-351">実際のファイルの一覧を表示するには、各リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-351">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="ae2f7-352">jQuery Datatable 1.10.5</span><span class="sxs-lookup"><span data-stu-id="ae2f7-352">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery Datatable 1.10.5")
- [<span data-ttu-id="ae2f7-353">jQuery Datatable 1.10.4</span><span class="sxs-lookup"><span data-stu-id="ae2f7-353">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery Datatable 1.10.4")
- [<span data-ttu-id="ae2f7-354">jQuery Datatable 1.9.4</span><span class="sxs-lookup"><span data-stu-id="ae2f7-354">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery Datatable 1.9.4")
- [<span data-ttu-id="ae2f7-355">jQuery Datatable 1.9.3</span><span class="sxs-lookup"><span data-stu-id="ae2f7-355">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery Datatable 1.9.3")
- [<span data-ttu-id="ae2f7-356">jQuery Datatable 1.9.2</span><span class="sxs-lookup"><span data-stu-id="ae2f7-356">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery Datatable 1.9.2")
- [<span data-ttu-id="ae2f7-357">jQuery Datatable 1.9.1</span><span class="sxs-lookup"><span data-stu-id="ae2f7-357">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery Datatable 1.9.1")
- [<span data-ttu-id="ae2f7-358">jQuery Datatable 1.9.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-358">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery Datatable 1.9.0")
- [<span data-ttu-id="ae2f7-359">jQuery Datatable 1.8.2</span><span class="sxs-lookup"><span data-stu-id="ae2f7-359">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery Datatable 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="ae2f7-360">CDN の Modernizr リリース</span><span class="sxs-lookup"><span data-stu-id="ae2f7-360">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="ae2f7-361">次のリリースの[Modernizr](http://www.modernizr.com "Modernizr") CDN でホストされます。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-361">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="ae2f7-362">CDN の JSHint リリース</span><span class="sxs-lookup"><span data-stu-id="ae2f7-362">JSHint Releases on the CDN</span></span>

<span data-ttu-id="ae2f7-363">次のリリースの[JSHint](http://www.jshint.com "JSHint") CDN でホストされます。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-363">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- http://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="ae2f7-364">CDN の knockout リリース</span><span class="sxs-lookup"><span data-stu-id="ae2f7-364">Knockout Releases on the CDN</span></span>

<span data-ttu-id="ae2f7-365">次のリリースの[Knockout](http://www.knockoutjs.com "Knockout") CDN でホストされます。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-365">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="ae2f7-366">CDN のリリースをグローバル化します。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-366">Globalize Releases on the CDN</span></span>

<span data-ttu-id="ae2f7-367">次のリリースの[Globalize](https://github.com/jquery/globalize "Globalize") CDN でホストされます。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-367">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="ae2f7-368">バージョン 1.0.0 をグローバル化します。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-368">Globalize version 1.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js

#### <a name="globalize-version-011"></a><span data-ttu-id="ae2f7-369">バージョン 0.1.1 をグローバル化します。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-369">Globalize version 0.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js
- http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js
- http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js

    - <span data-ttu-id="ae2f7-370">すべてのカルチャ</span><span class="sxs-lookup"><span data-stu-id="ae2f7-370">all cultures</span></span>
- http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js

    - <span data-ttu-id="ae2f7-371">「{カルチャ コード}」を目的のカルチャ コードに置き換えます、CDN でファイルを globalize.culture.en GB.js== Microsoft 例: = = これらのライブラリは、Microsoft によってアップロードされました。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-371">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="ae2f7-372">CDN のリリースを応答します。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-372">Respond Releases on the CDN</span></span>

<span data-ttu-id="ae2f7-373">次のリリースの[ https://github.com/scottjehl/Respond ] (https://github.com/scottjehl/Respond " https://github.com/scottjehl/Respond ") CDN では、応答がホストされています。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-373">The following releases of [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") Respond are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="ae2f7-374">応答バージョン 1.4.2</span><span class="sxs-lookup"><span data-stu-id="ae2f7-374">Respond version 1.4.2</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js

#### <a name="respond-version-141"></a><span data-ttu-id="ae2f7-375">応答バージョン 1.4.1</span><span class="sxs-lookup"><span data-stu-id="ae2f7-375">Respond version 1.4.1</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js

#### <a name="respond-version-140"></a><span data-ttu-id="ae2f7-376">応答バージョン 1.4.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-376">Respond version 1.4.0</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js

#### <a name="respond-version-130"></a><span data-ttu-id="ae2f7-377">応答バージョン 1.3.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-377">Respond version 1.3.0</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js

#### <a name="respond-version-120"></a><span data-ttu-id="ae2f7-378">応答バージョン 1.2.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-378">Respond version 1.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="ae2f7-379">CDN でブートス トラップのリリース</span><span class="sxs-lookup"><span data-stu-id="ae2f7-379">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="ae2f7-380">次のリリースの[getbootstrap.com](http://getbootstrap.com "getbootstrap.com")ブートス トラップが CDN でホストされています。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-380">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-400"></a><span data-ttu-id="ae2f7-381">ブートス トラップ バージョン 4.0.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-381">Bootstrap version 4.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-337"></a><span data-ttu-id="ae2f7-382">ブートス トラップ バージョン 3.3.7</span><span class="sxs-lookup"><span data-stu-id="ae2f7-382">Bootstrap version 3.3.7</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-336"></a><span data-ttu-id="ae2f7-383">ブートス トラップ バージョン 3.3.6</span><span class="sxs-lookup"><span data-stu-id="ae2f7-383">Bootstrap version 3.3.6</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-335"></a><span data-ttu-id="ae2f7-384">ブートス トラップ バージョン 3.3.5</span><span class="sxs-lookup"><span data-stu-id="ae2f7-384">Bootstrap version 3.3.5</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-334"></a><span data-ttu-id="ae2f7-385">ブートス トラップ バージョン 3.3.4</span><span class="sxs-lookup"><span data-stu-id="ae2f7-385">Bootstrap version 3.3.4</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-332"></a><span data-ttu-id="ae2f7-386">ブートス トラップ バージョン 3.3.2</span><span class="sxs-lookup"><span data-stu-id="ae2f7-386">Bootstrap version 3.3.2</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-331"></a><span data-ttu-id="ae2f7-387">ブートス トラップ バージョン 3.3.1</span><span class="sxs-lookup"><span data-stu-id="ae2f7-387">Bootstrap version 3.3.1</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-330"></a><span data-ttu-id="ae2f7-388">ブートス トラップ バージョン 3.3.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-388">Bootstrap version 3.3.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-320"></a><span data-ttu-id="ae2f7-389">ブートス トラップ バージョン 3.2.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-389">Bootstrap version 3.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-311"></a><span data-ttu-id="ae2f7-390">ブートス トラップ バージョン 3.1.1</span><span class="sxs-lookup"><span data-stu-id="ae2f7-390">Bootstrap version 3.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-310"></a><span data-ttu-id="ae2f7-391">ブートス トラップ バージョン 3.1.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-391">Bootstrap version 3.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-303"></a><span data-ttu-id="ae2f7-392">ブートス トラップ バージョン 3.0.3</span><span class="sxs-lookup"><span data-stu-id="ae2f7-392">Bootstrap version 3.0.3</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-302"></a><span data-ttu-id="ae2f7-393">ブートス トラップ バージョン 3.0.2</span><span class="sxs-lookup"><span data-stu-id="ae2f7-393">Bootstrap version 3.0.2</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-301"></a><span data-ttu-id="ae2f7-394">ブートス トラップ バージョン 3.0.1</span><span class="sxs-lookup"><span data-stu-id="ae2f7-394">Bootstrap version 3.0.1</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-300"></a><span data-ttu-id="ae2f7-395">ブートス トラップ 3.0.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-395">Bootstrap version 3.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-232"></a><span data-ttu-id="ae2f7-396">ブートス トラップ バージョン 2.3.2</span><span class="sxs-lookup"><span data-stu-id="ae2f7-396">Bootstrap version 2.3.2</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png

#### <a name="bootstrap-version-231"></a><span data-ttu-id="ae2f7-397">ブートス トラップ version 2.3.1</span><span class="sxs-lookup"><span data-stu-id="ae2f7-397">Bootstrap version 2.3.1</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="ae2f7-398">CDN のブートス トラップ TouchCarousel リリース</span><span class="sxs-lookup"><span data-stu-id="ae2f7-398">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="ae2f7-399">次のリリースの[ https://github.com/ixisio/bootstrap-touch-carousel ] (https://github.com/ixisio/bootstrap-touch-carousel " https://github.com/ixisio/bootstrap-touch-carousel ")ブートス トラップ TouchCarousel リリースが CDN でホストされています。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-399">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="ae2f7-400">ブートス トラップ TouchCarousel バージョン 0.8.0 から</span><span class="sxs-lookup"><span data-stu-id="ae2f7-400">Bootstrap TouchCarousel version 0.8.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css
- http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="ae2f7-401">CDN の Hammer.js リリース</span><span class="sxs-lookup"><span data-stu-id="ae2f7-401">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="ae2f7-402">次のリリースの[ http://hammerjs.github.io/ ] (http://hammerjs.github.io/ " http://hammerjs.github.io/ ") Hammer.js リリースが CDN でホストされています。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-402">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="ae2f7-403">Hammer.js バージョン 2.0.4</span><span class="sxs-lookup"><span data-stu-id="ae2f7-403">Hammer.js version 2.0.4</span></span>

- http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js
- http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js
- http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="ae2f7-404">ASP.NET Web フォームと Ajax CDN のリリース</span><span class="sxs-lookup"><span data-stu-id="ae2f7-404">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="ae2f7-405">CDN では、ASP.NET Ajax ライブラリの次のリリースがホストされています。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-405">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="ae2f7-406">実際のファイルの一覧を表示するには、各リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-406">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="ae2f7-407">ASP.NET Web フォームと Ajax にバージョン 4.5.2</span><span class="sxs-lookup"><span data-stu-id="ae2f7-407">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "Ajax 4.5.2 and ASP.NET Web フォーム")
- [<span data-ttu-id="ae2f7-408">ASP.NET Web フォームと Ajax バージョン 4</span><span class="sxs-lookup"><span data-stu-id="ae2f7-408">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "Ajax 4 and ASP.NET Web フォーム")
- [<span data-ttu-id="ae2f7-409">ASP.NET Ajax version 3.5</span><span class="sxs-lookup"><span data-stu-id="ae2f7-409">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "ASP.NET Ajax 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="ae2f7-410">ASP.NET MVC が CDN でリリースします。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-410">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="ae2f7-411">この CDN では、次の ASP.NET MVC の JavaScript ファイルがホストされています。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-411">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="ae2f7-412">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="ae2f7-412">ASP.NET MVC 5.2.3</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="ae2f7-413">ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="ae2f7-413">ASP.NET MVC 5.1</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="ae2f7-414">ASP.NET MVC 5.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-414">ASP.NET MVC 5.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="ae2f7-415">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-415">ASP.NET MVC 4.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="ae2f7-416">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-416">ASP.NET MVC 3.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="ae2f7-417">ASP.NET MVC 2.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-417">ASP.NET MVC 2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js
- http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="ae2f7-418">ASP.NET MVC 1.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-418">ASP.NET MVC 1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js
- http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="ae2f7-419">ASP.NET SignalR を CDN で解放します。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-419">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="ae2f7-420">この CDN では、次の ASP.NET SignalR JavaScript ファイルがホストされています。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-420">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="ae2f7-421">ASP.NET SignalR 2.2.2</span><span class="sxs-lookup"><span data-stu-id="ae2f7-421">ASP.NET SignalR 2.2.2</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="ae2f7-422">ASP.NET SignalR 2.2.1</span><span class="sxs-lookup"><span data-stu-id="ae2f7-422">ASP.NET SignalR 2.2.1</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="ae2f7-423">ASP.NET SignalR 2.2.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-423">ASP.NET SignalR 2.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="ae2f7-424">ASP.NET SignalR 2.1.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-424">ASP.NET SignalR 2.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="ae2f7-425">ASP.NET SignalR 2.0.3</span><span class="sxs-lookup"><span data-stu-id="ae2f7-425">ASP.NET SignalR 2.0.3</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="ae2f7-426">ASP.NET SignalR 2.0.2</span><span class="sxs-lookup"><span data-stu-id="ae2f7-426">ASP.NET SignalR 2.0.2</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="ae2f7-427">ASP.NET SignalR 2.0.1</span><span class="sxs-lookup"><span data-stu-id="ae2f7-427">ASP.NET SignalR 2.0.1</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="ae2f7-428">ASP.NET SignalR 2.0.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-428">ASP.NET SignalR 2.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="ae2f7-429">ASP.NET SignalR 1.1.3</span><span class="sxs-lookup"><span data-stu-id="ae2f7-429">ASP.NET SignalR 1.1.3</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="ae2f7-430">ASP.NET SignalR 1.1.2</span><span class="sxs-lookup"><span data-stu-id="ae2f7-430">ASP.NET SignalR 1.1.2</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="ae2f7-431">ASP.NET SignalR 1.1.1</span><span class="sxs-lookup"><span data-stu-id="ae2f7-431">ASP.NET SignalR 1.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="ae2f7-432">ASP.NET SignalR 1.1.0</span><span class="sxs-lookup"><span data-stu-id="ae2f7-432">ASP.NET SignalR 1.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="ae2f7-433">ASP.NET SignalR 1.0.1</span><span class="sxs-lookup"><span data-stu-id="ae2f7-433">ASP.NET SignalR 1.0.1</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js

<span data-ttu-id="ae2f7-434">CDN の使用条件については、次を参照してください。 [Microsoft Ajax CDN の使用条件](https://www.asp.net/terms-of-use "Microsoft Ajax CDN の使用条件")です。</span><span class="sxs-lookup"><span data-stu-id="ae2f7-434">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>
