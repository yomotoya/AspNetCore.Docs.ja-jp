---
uid: web-pages/overview/ui-layouts-and-themes/10-working-with-video
title: ページ (Razor) サイトの ASP.NET Web でのビデオの表示 |Microsoft ドキュメント
author: tfitzmac
description: この章では、ビデオ、ASP.NET Web Pages で Razor 構文のページを表示する方法について説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 332fb3da-e2a5-460d-bb90-dd911e1e2c95
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/10-working-with-video
msc.type: authoredcontent
ms.openlocfilehash: 4e7dc50fb60546d1e1f10a16ed863c0b812ec82b
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/12/2018
---
<a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="ddae3-103">ASP.NET Web Pages (Razor) サイトにビデオを表示します。</span><span class="sxs-lookup"><span data-stu-id="ddae3-103">Displaying Video in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="ddae3-104">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="ddae3-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="ddae3-105">この記事では、ASP.NET Web Pages (Razor) の web サイトにビデオ (メディア プレーヤーを使用して、サイトに保存されているビデオを表示できるようにする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="ddae3-105">This article explains how to use a video (media) player in an ASP.NET Web Pages (Razor) website to let users view videos that are stored on the site.</span></span> <span data-ttu-id="ddae3-106">ASP.NET Web ページ Razor 構文を使用すると、Flash を再生することができます (*.swf*)、Media Player (*.wmv*)、および Silverlight (*.xap*) ビデオです。</span><span class="sxs-lookup"><span data-stu-id="ddae3-106">ASP.NET Web Pages with Razor syntax lets you play Flash (*.swf*), Media Player (*.wmv*), and Silverlight (*.xap*) videos.</span></span>
> 
> <span data-ttu-id="ddae3-107">学習する内容。</span><span class="sxs-lookup"><span data-stu-id="ddae3-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="ddae3-108">ビデオ プレーヤーを選択する方法です。</span><span class="sxs-lookup"><span data-stu-id="ddae3-108">How to choose a video player.</span></span>
> - <span data-ttu-id="ddae3-109">Web ページにビデオを追加する方法です。</span><span class="sxs-lookup"><span data-stu-id="ddae3-109">How to add video to a web page.</span></span>
> - <span data-ttu-id="ddae3-110">ビデオ プレーヤーの属性を設定する方法。</span><span class="sxs-lookup"><span data-stu-id="ddae3-110">How to set video player attributes.</span></span>
> 
> <span data-ttu-id="ddae3-111">これらは、ASP.NET Razor ページのアーティクルで導入された機能。</span><span class="sxs-lookup"><span data-stu-id="ddae3-111">These are the ASP.NET Razor pages features introduced in the article:</span></span>
> 
> - <span data-ttu-id="ddae3-112">`Video`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="ddae3-112">The `Video` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ddae3-113">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="ddae3-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="ddae3-114">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="ddae3-114">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="ddae3-115">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="ddae3-115">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="ddae3-116">このチュートリアルは、WebMatrix 3 でも動作します。</span><span class="sxs-lookup"><span data-stu-id="ddae3-116">This tutorial also works with WebMatrix 3.</span></span>


## <a name="introduction"></a><span data-ttu-id="ddae3-117">はじめに</span><span class="sxs-lookup"><span data-stu-id="ddae3-117">Introduction</span></span>

<span data-ttu-id="ddae3-118">サイトのビデオを表示する場合があります。</span><span class="sxs-lookup"><span data-stu-id="ddae3-118">You might want to display a video on your site.</span></span> <span data-ttu-id="ddae3-119">1 つの方法では、YouTube と同様に、ビデオは既にサイトへのリンクです。</span><span class="sxs-lookup"><span data-stu-id="ddae3-119">One way to do that is to link to a site that already has the video, like YouTube.</span></span> <span data-ttu-id="ddae3-120">独自のページに直接これらのサイトからビデオを埋め込む場合は、通常、サイトからの HTML マークアップを取得でき、ページにコピーできます。</span><span class="sxs-lookup"><span data-stu-id="ddae3-120">If you want to embed a video from these sites directly in your own pages, you can usually get HTML markup from the site and then copy it into your page.</span></span> <span data-ttu-id="ddae3-121">たとえば、次の例を示しています、YouTube を埋め込む方法ビデオ。</span><span class="sxs-lookup"><span data-stu-id="ddae3-121">For example, the following example shows how to embed a YouTube video:</span></span>

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

<span data-ttu-id="ddae3-122">(パブリックのビデオ共有サイト) ではなく、独自の web サイト上にあるビデオを再生する場合は、直接、次のように埋め込みのマークアップを使用してリンクすることはできません。</span><span class="sxs-lookup"><span data-stu-id="ddae3-122">If you want to play a video that's on your own website (not on a public video-sharing site), you can't directly link to it using embedded markup like this.</span></span> <span data-ttu-id="ddae3-123">使用して、サイトからビデオを再生するただし、`Video`ヘルパーに渡し、ページに直接メディア プレーヤーをレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="ddae3-123">However, you can play videos from your site by using the `Video` helper, which renders a media player directly in a page.</span></span>

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a><span data-ttu-id="ddae3-124">ビデオ プレーヤーを選択します。</span><span class="sxs-lookup"><span data-stu-id="ddae3-124">Choosing a Video Player</span></span>

<span data-ttu-id="ddae3-125">ビデオ ファイルの形式のたくさん、それぞれの形式は、他のプレーヤーと、player を構成するさまざまな方法に通常必要とします。</span><span class="sxs-lookup"><span data-stu-id="ddae3-125">There are lots of formats for video files, and each format typically requires a different player and a different way to configure the player.</span></span> <span data-ttu-id="ddae3-126">ASP.NET Razor ページで、web ページを使用して、ビデオを再生することができます、`Video`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="ddae3-126">In ASP.NET Razor pages, you can play a video in a web page using the `Video` helper.</span></span> <span data-ttu-id="ddae3-127">`Video`ヘルパーが自動的に生成されるため、web ページでビデオを埋め込みのプロセスを簡略化、`object`と`embed`ビデオ ページを追加する、通常使用される HTML 要素です。</span><span class="sxs-lookup"><span data-stu-id="ddae3-127">The `Video` helper simplifies the process of embedding videos in a web page because it automatically generates the `object` and `embed` HTML elements that are normally used to add video to the page.</span></span>

<span data-ttu-id="ddae3-128">`Video`ヘルパーは、次のメディア プレーヤーをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="ddae3-128">The `Video` helper supports the following media players:</span></span>

- <span data-ttu-id="ddae3-129">Adobe Flash</span><span class="sxs-lookup"><span data-stu-id="ddae3-129">Adobe Flash</span></span>
- <span data-ttu-id="ddae3-130">Windows MediaPlayer</span><span class="sxs-lookup"><span data-stu-id="ddae3-130">Windows MediaPlayer</span></span>
- <span data-ttu-id="ddae3-131">Microsoft Silverlight</span><span class="sxs-lookup"><span data-stu-id="ddae3-131">Microsoft Silverlight</span></span>

### <a name="the-flash-player"></a><span data-ttu-id="ddae3-132">Flash Player</span><span class="sxs-lookup"><span data-stu-id="ddae3-132">The Flash Player</span></span>

<span data-ttu-id="ddae3-133">`Flash`のプレーヤー、`Video`ヘルパーを使用して、フラッシュのビデオを再生できます (*.swf*ファイル)、web ページにします。</span><span class="sxs-lookup"><span data-stu-id="ddae3-133">The `Flash` player of the `Video` helper let you play Flash videos (*.swf* files) in a web page.</span></span> <span data-ttu-id="ddae3-134">少なくとも、ビデオ ファイルへのパスを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ddae3-134">At a minimum, you have to provide a path to the video file.</span></span> <span data-ttu-id="ddae3-135">パスだけを指定する場合、player はフラッシュの現在のバージョンで設定されている既定値を使用します。</span><span class="sxs-lookup"><span data-stu-id="ddae3-135">If you specify nothing but the path, the player uses default values that are set by the current version of Flash.</span></span> <span data-ttu-id="ddae3-136">一般的な既定の設定は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="ddae3-136">Typical default settings are:</span></span>

- <span data-ttu-id="ddae3-137">既定の幅と高さを使用してビデオを表示および背景色がないです。</span><span class="sxs-lookup"><span data-stu-id="ddae3-137">The video is displayed using its default width and height and without a background color.</span></span>
- <span data-ttu-id="ddae3-138">ビデオは、ページが読み込まれるときに自動的に再生されます。</span><span class="sxs-lookup"><span data-stu-id="ddae3-138">The video plays automatically when the page loads.</span></span>
- <span data-ttu-id="ddae3-139">ビデオは、明示的に停止されるまでに継続的にループ処理します。</span><span class="sxs-lookup"><span data-stu-id="ddae3-139">The video loops continuously until it's explicitly stopped.</span></span>
- <span data-ttu-id="ddae3-140">ビデオは、特定のサイズに合わせてビデオがトリミングされる点ではなく、ビデオのすべてを表示するスケールします。</span><span class="sxs-lookup"><span data-stu-id="ddae3-140">The video is scaled to show all of the video, rather than cropping the video to fit a specific size.</span></span>
- <span data-ttu-id="ddae3-141">ウィンドウでビデオを再生します。</span><span class="sxs-lookup"><span data-stu-id="ddae3-141">The video plays in a window.</span></span>

### <a name="the-mediaplayer-player"></a><span data-ttu-id="ddae3-142">Media Player プレーヤー</span><span class="sxs-lookup"><span data-stu-id="ddae3-142">The MediaPlayer Player</span></span>

<span data-ttu-id="ddae3-143">`MediaPlayer`のプレーヤー、`Video`ヘルパーでは、Windows Media ビデオを再生することができます (*.wmv*ファイル)、Windows Media オーディオ (*.wma*ファイル)、および MP3 (*.mp3*web ページではファイル)。</span><span class="sxs-lookup"><span data-stu-id="ddae3-143">The `MediaPlayer` player of the `Video` helper lets you play Windows Media videos (*.wmv* files), Windows Media audio (*.wma* files), and MP3 (*.mp3* files) in a web page.</span></span> <span data-ttu-id="ddae3-144">を再生するメディア ファイルのパスを含める必要がありますその他のすべてのパラメーターは省略できます。</span><span class="sxs-lookup"><span data-stu-id="ddae3-144">You must include path of the media file to play; all other parameters are optional.</span></span> <span data-ttu-id="ddae3-145">パスのみを指定する場合、player はなどの設定、media Player の現在のバージョンで既定の設定を使用します。</span><span class="sxs-lookup"><span data-stu-id="ddae3-145">If you specify only a path, the player uses default settings set by the current version of MediaPlayer, such as:</span></span>

- <span data-ttu-id="ddae3-146">既定の幅と高さを使用して、ビデオが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ddae3-146">The video is displayed using its default width and height.</span></span>
- <span data-ttu-id="ddae3-147">ビデオは、ページが読み込まれるときに自動的に再生されます。</span><span class="sxs-lookup"><span data-stu-id="ddae3-147">The video plays automatically when the page loads.</span></span>
- <span data-ttu-id="ddae3-148">ビデオが 1 回再生 (そのループしません)。</span><span class="sxs-lookup"><span data-stu-id="ddae3-148">The video plays once (it doesn't loop).</span></span>
- <span data-ttu-id="ddae3-149">Player は、ユーザー インターフェイスのコントロールの完全なセットを表示します。</span><span class="sxs-lookup"><span data-stu-id="ddae3-149">The player displays the full set of controls in the user interface.</span></span>
- <span data-ttu-id="ddae3-150">ウィンドウでビデオを再生します。</span><span class="sxs-lookup"><span data-stu-id="ddae3-150">The video plays in a window.</span></span>

### <a name="the-silverlight-player"></a><span data-ttu-id="ddae3-151">Silverlight プレーヤー</span><span class="sxs-lookup"><span data-stu-id="ddae3-151">The Silverlight Player</span></span>

<span data-ttu-id="ddae3-152">`Silverlight`のプレーヤー、`Video`ヘルパーでは、Windows Media ビデオを再生することができます (*.wmv*ファイル)、Windows Media Audio (*.wma*ファイル)、および MP3 (*.mp3*ファイル)。</span><span class="sxs-lookup"><span data-stu-id="ddae3-152">The `Silverlight` player of the `Video` helper lets you play Windows Media Video (*.wmv* files), Windows Media Audio (*.wma* files), and MP3 (*.mp3* files).</span></span> <span data-ttu-id="ddae3-153">Silverlight ベースのアプリケーション パッケージを指すよう path パラメーターを設定する必要があります (*.xap*ファイル)。</span><span class="sxs-lookup"><span data-stu-id="ddae3-153">You must set the path parameter to point to a Silverlight-based application package (*.xap* file).</span></span> <span data-ttu-id="ddae3-154">また、幅と高さのパラメーターを設定することも必要があります。</span><span class="sxs-lookup"><span data-stu-id="ddae3-154">You also must set the width and height parameters.</span></span> <span data-ttu-id="ddae3-155">その他のすべてのパラメーターは省略できます。</span><span class="sxs-lookup"><span data-stu-id="ddae3-155">All other parameters are optional.</span></span> <span data-ttu-id="ddae3-156">必須パラメーターのみを設定した場合、ビデオ、Silverlight プレーヤーを使用するときに、Silverlight プレーヤーには、背景色がないビデオが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ddae3-156">When you use the Silverlight player for video, if you set only the required parameters, the Silverlight player displays the video without a background color.</span></span>

> [!NOTE]
> <span data-ttu-id="ddae3-157">Silverlight がわからない場合に備えて、: *.xap*ファイルは、レイアウトの指示を含む圧縮ファイル、 *.xaml*ファイル、アセンブリ、および省略可能なリソースのマネージ コード。</span><span class="sxs-lookup"><span data-stu-id="ddae3-157">In case you don't already know Silverlight: the *.xap* file is a compressed file that contains layout instructions in a *.xaml* file, managed code in assemblies, and optional resources.</span></span> <span data-ttu-id="ddae3-158">作成することができます、 *.xap* Silverlight アプリケーション プロジェクトとして Visual Studio でのファイルです。</span><span class="sxs-lookup"><span data-stu-id="ddae3-158">You can create a *.xap* file in Visual Studio as a Silverlight application project.</span></span>


<span data-ttu-id="ddae3-159">`Silverlight`ビデオ プレーヤー、プレーヤーに指定される設定とに用意されている設定の両方を使用して、 *.xap*ファイル。</span><span class="sxs-lookup"><span data-stu-id="ddae3-159">The `Silverlight` video player uses both the settings that you provide for the player and the settings that are provided in the *.xap* file.</span></span>

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a><span data-ttu-id="ddae3-160">MIME の種類</span><span class="sxs-lookup"><span data-stu-id="ddae3-160">MIME Types</span></span>
> 
> <span data-ttu-id="ddae3-161">ブラウザーがファイルをダウンロードするとき、ブラウザーは、ファイルの種類が表示されるドキュメントに対して指定されている MIME の種類と一致していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="ddae3-161">When a browser downloads a file, the browser makes sure that the file type matches the MIME type that's specified for the document that's being rendered.</span></span> <span data-ttu-id="ddae3-162">MIME の種類は、ファイルのコンテンツの種類またはメディアの種類です。</span><span class="sxs-lookup"><span data-stu-id="ddae3-162">The MIME type is the content type or media type of a file.</span></span> <span data-ttu-id="ddae3-163">`Video`ヘルパーは、次の MIME の種類を使用します。</span><span class="sxs-lookup"><span data-stu-id="ddae3-163">The `Video` helper uses the following MIME types:</span></span>
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`


<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a><span data-ttu-id="ddae3-164">フラッシュ (.swf) のビデオの再生</span><span class="sxs-lookup"><span data-stu-id="ddae3-164">Playing Flash (.swf) Videos</span></span>

<span data-ttu-id="ddae3-165">この手順は、名前、フラッシュ ビデオを再生する方法を示します*sample.swf*です。</span><span class="sxs-lookup"><span data-stu-id="ddae3-165">This procedure shows you how to play a Flash video named *sample.swf*.</span></span> <span data-ttu-id="ddae3-166">手順では、という名前のフォルダーをしたものと*メディア*ことと、サイトで、 *.swf*ファイルがそのフォルダーにします。</span><span class="sxs-lookup"><span data-stu-id="ddae3-166">The procedure assumes that you've got a folder named *Media* on your site and that the *.swf* file is in that folder.</span></span>

1. <span data-ttu-id="ddae3-167">説明に従って、ASP.NET Web Helpers Library を web サイトに追加[ASP.NET Web Pages サイトをインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)、既に追加していない場合。</span><span class="sxs-lookup"><span data-stu-id="ddae3-167">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="ddae3-168">Web サイトでページを追加し、名前を付けます*FlashVideo.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="ddae3-168">In the website, add a page and name it *FlashVideo.cshtml*.</span></span>
3. <span data-ttu-id="ddae3-169">ページに、次のマークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="ddae3-169">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. <span data-ttu-id="ddae3-170">ブラウザーでページを実行します。</span><span class="sxs-lookup"><span data-stu-id="ddae3-170">Run the page in a browser.</span></span> <span data-ttu-id="ddae3-171">(ページが選択されて、必ず、**ファイル**ワークスペースを実行する前にします)。ページが表示され、ビデオを自動的に再生します。</span><span class="sxs-lookup"><span data-stu-id="ddae3-171">(Make sure the page is selected in the **Files** workspace before you run it.) The page is displayed and the video plays automatically.</span></span> 

    <span data-ttu-id="ddae3-172">![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")</span><span class="sxs-lookup"><span data-stu-id="ddae3-172">![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")</span></span>

<span data-ttu-id="ddae3-173">設定することができます、`quality`フラッシュ ビデオへのパラメーター `low`、 `autolow`、 `autohigh`、 `medium`、 `high`、および`best`:</span><span class="sxs-lookup"><span data-stu-id="ddae3-173">You can set the `quality` parameter for a Flash video to `low`, `autolow`, `autohigh`, `medium`, `high`, and `best`:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

<span data-ttu-id="ddae3-174">フラッシュ ビデオを使用して特定のサイズで再生を変更することができます、`scale`パラメーターで、次を設定することができます。</span><span class="sxs-lookup"><span data-stu-id="ddae3-174">You can change the Flash video to play at a specific size using the `scale` parameter, which you can set to the following:</span></span>

- <span data-ttu-id="ddae3-175">`showall`。</span><span class="sxs-lookup"><span data-stu-id="ddae3-175">`showall`.</span></span> <span data-ttu-id="ddae3-176">これにより、元の縦横比を維持しながらビデオ全体を表示にします。</span><span class="sxs-lookup"><span data-stu-id="ddae3-176">This makes the entire video visible while maintaining the original aspect ratio.</span></span> <span data-ttu-id="ddae3-177">ただし、それぞれの側の枠で囲まれた終了する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="ddae3-177">However, you might end up with borders on each side.</span></span>
- <span data-ttu-id="ddae3-178">`noorder`。</span><span class="sxs-lookup"><span data-stu-id="ddae3-178">`noorder`.</span></span> <span data-ttu-id="ddae3-179">元の縦横比を維持しながら、ビデオ拡大または縮小がトリミングされる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="ddae3-179">This scales the video while maintaining the original aspect ratio, but it might be cropped.</span></span>
- <span data-ttu-id="ddae3-180">`exactfit`。</span><span class="sxs-lookup"><span data-stu-id="ddae3-180">`exactfit`.</span></span> <span data-ttu-id="ddae3-181">これにより、ビデオを全体表示されている元の縦横比を維持せずがゆがみが発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="ddae3-181">This makes the entire video visible without preserving the original aspect ratio, but distortion may occur.</span></span>

<span data-ttu-id="ddae3-182">指定しない場合は、`scale`パラメーター、ビデオ全体が表示されます、元の縦横比がトリミングことがなく継続されます。</span><span class="sxs-lookup"><span data-stu-id="ddae3-182">If you don't specify a `scale` parameter, the entire video will be visible and the original aspect ratio will be maintained without any cropping.</span></span> <span data-ttu-id="ddae3-183">次の例を使用する方法を示しています、`scale`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="ddae3-183">The following example shows how to use the `scale` parameter:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

<span data-ttu-id="ddae3-184">Flash player が名前付きの設定のビデオ モードをサポートしている`windowMode`です。</span><span class="sxs-lookup"><span data-stu-id="ddae3-184">The Flash player supports a video mode setting named `windowMode`.</span></span> <span data-ttu-id="ddae3-185">これを設定することができます`window`、 `opaque`、および`transparent`です。</span><span class="sxs-lookup"><span data-stu-id="ddae3-185">You can set this to `window`, `opaque`, and `transparent`.</span></span> <span data-ttu-id="ddae3-186">既定では、`windowMode`に設定されている`window`、web ページ上の別のウィンドウで、ビデオが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ddae3-186">By default, the `windowMode` is set to `window`, which displays the video in a separate window on the web page.</span></span> <span data-ttu-id="ddae3-187">`opaque`設定では、web ページ上のビデオの背後にすべて非表示にします。</span><span class="sxs-lookup"><span data-stu-id="ddae3-187">The `opaque` setting hides everything behind the video on the web page.</span></span> <span data-ttu-id="ddae3-188">`transparent`設定は、ビデオの任意の部分は透過的と仮定した場合、ビデオを表示する web ページの背景をことができます。</span><span class="sxs-lookup"><span data-stu-id="ddae3-188">The `transparent` setting lets the background of the web page show through the video, assuming any part of the video is transparent.</span></span>

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a><span data-ttu-id="ddae3-189">Media Player の再生 (*.wmv*) のビデオ</span><span class="sxs-lookup"><span data-stu-id="ddae3-189">Playing MediaPlayer (*.wmv*) Videos</span></span>

<span data-ttu-id="ddae3-190">次の手順は、という名前のウィンドウ Media ビデオを再生する方法を示します*sample.wmv*内にある、*メディア*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="ddae3-190">The following procedure shows you how to play a Window Media video named *sample.wmv* that's in the *Media* folder.</span></span>

1. <span data-ttu-id="ddae3-191">説明に従って、ASP.NET Web Helpers Library を web サイトに追加[ASP.NET Web Pages サイトをインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)まだ行っていない場合は、します。</span><span class="sxs-lookup"><span data-stu-id="ddae3-191">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="ddae3-192">という名前の新しいページを作成する*MediaPlayerVideo.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="ddae3-192">Create a new page named *MediaPlayerVideo.cshtml*.</span></span>
3. <span data-ttu-id="ddae3-193">ページに、次のマークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="ddae3-193">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. <span data-ttu-id="ddae3-194">ブラウザーでページを実行します。</span><span class="sxs-lookup"><span data-stu-id="ddae3-194">Run the page in a browser.</span></span> <span data-ttu-id="ddae3-195">ビデオを読み込んで、自動的に再生します。</span><span class="sxs-lookup"><span data-stu-id="ddae3-195">The video loads and plays automatically.</span></span> 

    <span data-ttu-id="ddae3-196">![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")</span><span class="sxs-lookup"><span data-stu-id="ddae3-196">![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")</span></span>

<span data-ttu-id="ddae3-197">設定することができます`playCount`を自動的にビデオを再生する回数を示す整数。</span><span class="sxs-lookup"><span data-stu-id="ddae3-197">You can set `playCount` to an integer that indicates how many times to play the video automatically:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

<span data-ttu-id="ddae3-198">`uiMode`パラメーターを使用して、ユーザー インターフェイスに表示するコントロールを指定できます。</span><span class="sxs-lookup"><span data-stu-id="ddae3-198">The `uiMode` parameter lets you specify which controls show up in the user interface.</span></span> <span data-ttu-id="ddae3-199">設定することができます`uiMode`に`invisible`、 `none`、 `mini`、または`full`です。</span><span class="sxs-lookup"><span data-stu-id="ddae3-199">You can set `uiMode` to `invisible`, `none`, `mini`, or `full`.</span></span> <span data-ttu-id="ddae3-200">指定しない場合、`uiMode`パラメーター、ビデオがでシーク、ステータス ウィンドウが表示されます、バー、ボタン、およびビデオのウィンドウに加え、ボリューム コントロールを制御します。</span><span class="sxs-lookup"><span data-stu-id="ddae3-200">If you don't specify a `uiMode` parameter, the video will be displayed with the status window, seek bar, control buttons, and volume controls in addition to the video window.</span></span> <span data-ttu-id="ddae3-201">これらのコントロールが、オーディオ ファイルを再生するプレーヤーを使用する場合にも表示されます。</span><span class="sxs-lookup"><span data-stu-id="ddae3-201">These controls will also be displayed if you use the player to play an audio file.</span></span> <span data-ttu-id="ddae3-202">使用する方法の例をここでは、`uiMode`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="ddae3-202">Here's an example of how to use the `uiMode` parameter:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

<span data-ttu-id="ddae3-203">既定では、オーディオは、ビデオの再生時にです。</span><span class="sxs-lookup"><span data-stu-id="ddae3-203">By default, audio is on when the video plays.</span></span> <span data-ttu-id="ddae3-204">設定して、オーディオをミュートすることができます、`mute`パラメーターを true にします。</span><span class="sxs-lookup"><span data-stu-id="ddae3-204">You can mute the audio by setting the `mute` parameter to true:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

<span data-ttu-id="ddae3-205">設定して、media Player ビデオのオーディオ レベルを制御することができます、`volume`パラメーターを 0 ~ 100 の範囲の値。</span><span class="sxs-lookup"><span data-stu-id="ddae3-205">You can control the audio level of the MediaPlayer video by setting the `volume` parameter to a value between 0 and 100.</span></span> <span data-ttu-id="ddae3-206">既定値は 50 です。</span><span class="sxs-lookup"><span data-stu-id="ddae3-206">The default value is 50.</span></span> <span data-ttu-id="ddae3-207">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="ddae3-207">Here's an example:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a><span data-ttu-id="ddae3-208">Silverlight のビデオの再生</span><span class="sxs-lookup"><span data-stu-id="ddae3-208">Playing Silverlight Videos</span></span>

<span data-ttu-id="ddae3-209">この手順は、Silverlight に含まれているビデオを再生する方法を示します *.xap*という名前のフォルダーには、ページ*メディア*です。</span><span class="sxs-lookup"><span data-stu-id="ddae3-209">This procedure shows you how to play video contained in a Silverlight *.xap* page that's in a folder named *Media*.</span></span>

1. <span data-ttu-id="ddae3-210">説明に従って、ASP.NET Web Helpers Library を web サイトに追加[ASP.NET Web Pages サイトをインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)まだ行っていない場合は、します。</span><span class="sxs-lookup"><span data-stu-id="ddae3-210">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already .</span></span>
2. <span data-ttu-id="ddae3-211">という名前の新しいページを作成する*SilverlightVideo.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="ddae3-211">Create a new page named *SilverlightVideo.cshtml*.</span></span>
3. <span data-ttu-id="ddae3-212">ページに、次のマークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="ddae3-212">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. <span data-ttu-id="ddae3-213">ブラウザーでページを実行します。</span><span class="sxs-lookup"><span data-stu-id="ddae3-213">Run the page in a browser.</span></span> 

    <span data-ttu-id="ddae3-214">![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")</span><span class="sxs-lookup"><span data-stu-id="ddae3-214">![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="ddae3-215">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="ddae3-215">Additional Resources</span></span>


<span data-ttu-id="ddae3-216">[Silverlight の概要](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)</span><span class="sxs-lookup"><span data-stu-id="ddae3-216">[Silverlight Overview](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)</span></span>

[<span data-ttu-id="ddae3-217">タグ属性をフラッシュのオブジェクトと埋め込み</span><span class="sxs-lookup"><span data-stu-id="ddae3-217">Flash OBJECT and EMBED tag attributes</span></span>](http://kb2.adobe.com/cps/127/tn_12701.html)

<span data-ttu-id="ddae3-218">[Windows Media Player 11 SDK PARAM タグします。](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)</span><span class="sxs-lookup"><span data-stu-id="ddae3-218">[Windows Media Player 11 SDK PARAM Tags](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)</span></span>
