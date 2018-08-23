---
uid: web-pages/overview/ui-layouts-and-themes/10-working-with-video
title: ページ (Razor) サイトを ASP.NET web ビデオの表示 |Microsoft Docs
author: tfitzmac
description: この章では、ビデオ、ASP.NET Web Pages での Razor 構文のページを表示する方法について説明します。
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 332fb3da-e2a5-460d-bb90-dd911e1e2c95
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/10-working-with-video
msc.type: authoredcontent
ms.openlocfilehash: 28884d8c4998ea5b00a60bf094f41b915b565bd8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828059"
---
<a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="fae10-103">ASP.NET Web Pages (Razor) サイトにビデオを表示します。</span><span class="sxs-lookup"><span data-stu-id="fae10-103">Displaying Video in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="fae10-104">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="fae10-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="fae10-105">この記事では、ASP.NET Web Pages (Razor) の web サイトに (メディア) のビデオ プレーヤーを使用して、サイトに保存されているビデオを表示できるようにする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="fae10-105">This article explains how to use a video (media) player in an ASP.NET Web Pages (Razor) website to let users view videos that are stored on the site.</span></span> <span data-ttu-id="fae10-106">Razor 構文を使用して ASP.NET Web ページでは、Flash を再生することができます (*.swf*)、Media Player (*.wmv*)、および Silverlight (*.xap*) ビデオ。</span><span class="sxs-lookup"><span data-stu-id="fae10-106">ASP.NET Web Pages with Razor syntax lets you play Flash (*.swf*), Media Player (*.wmv*), and Silverlight (*.xap*) videos.</span></span>
> 
> <span data-ttu-id="fae10-107">学習内容。</span><span class="sxs-lookup"><span data-stu-id="fae10-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="fae10-108">ビデオ プレーヤーを選択する方法。</span><span class="sxs-lookup"><span data-stu-id="fae10-108">How to choose a video player.</span></span>
> - <span data-ttu-id="fae10-109">Web ページにビデオを追加する方法。</span><span class="sxs-lookup"><span data-stu-id="fae10-109">How to add video to a web page.</span></span>
> - <span data-ttu-id="fae10-110">ビデオ プレーヤー属性を設定する方法。</span><span class="sxs-lookup"><span data-stu-id="fae10-110">How to set video player attributes.</span></span>
> 
> <span data-ttu-id="fae10-111">これらは、ASP.NET Razor ページの記事で導入された機能。</span><span class="sxs-lookup"><span data-stu-id="fae10-111">These are the ASP.NET Razor pages features introduced in the article:</span></span>
> 
> - <span data-ttu-id="fae10-112">`Video`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="fae10-112">The `Video` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="fae10-113">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="fae10-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="fae10-114">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="fae10-114">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="fae10-115">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="fae10-115">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="fae10-116">このチュートリアルは、WebMatrix 3 でも機能します。</span><span class="sxs-lookup"><span data-stu-id="fae10-116">This tutorial also works with WebMatrix 3.</span></span>


## <a name="introduction"></a><span data-ttu-id="fae10-117">はじめに</span><span class="sxs-lookup"><span data-stu-id="fae10-117">Introduction</span></span>

<span data-ttu-id="fae10-118">サイトのビデオを表示する場合があります。</span><span class="sxs-lookup"><span data-stu-id="fae10-118">You might want to display a video on your site.</span></span> <span data-ttu-id="fae10-119">1 つの簡単な方法では、YouTube のように、ビデオが既にサイトにリンクします。</span><span class="sxs-lookup"><span data-stu-id="fae10-119">One way to do that is to link to a site that already has the video, like YouTube.</span></span> <span data-ttu-id="fae10-120">独自のページに直接これらのサイトからビデオを埋め込む場合は、通常は、サイトからの HTML マークアップを取得し、ページにコピーします。</span><span class="sxs-lookup"><span data-stu-id="fae10-120">If you want to embed a video from these sites directly in your own pages, you can usually get HTML markup from the site and then copy it into your page.</span></span> <span data-ttu-id="fae10-121">たとえば、次の例では、ビデオを示しています、YouTube を埋め込む方法。</span><span class="sxs-lookup"><span data-stu-id="fae10-121">For example, the following example shows how to embed a YouTube video:</span></span>

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

<span data-ttu-id="fae10-122">(パブリック ビデオ共有サイト) ではなく、独自の web サイト上にあるビデオを再生する場合は、直接を次のように埋め込みのマークアップを使用してリンクすることはできません。</span><span class="sxs-lookup"><span data-stu-id="fae10-122">If you want to play a video that's on your own website (not on a public video-sharing site), you can't directly link to it using embedded markup like this.</span></span> <span data-ttu-id="fae10-123">使用して、サイトからビデオを再生するただし、`Video`ヘルパーで、ページに直接メディア プレーヤーを表示します。</span><span class="sxs-lookup"><span data-stu-id="fae10-123">However, you can play videos from your site by using the `Video` helper, which renders a media player directly in a page.</span></span>

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a><span data-ttu-id="fae10-124">ビデオ プレーヤーを選択します。</span><span class="sxs-lookup"><span data-stu-id="fae10-124">Choosing a Video Player</span></span>

<span data-ttu-id="fae10-125">大量のビデオのファイルの形式があり、さまざまなプレーヤー、プレーヤーを構成するさまざまな方法に通常各形式が必要です。</span><span class="sxs-lookup"><span data-stu-id="fae10-125">There are lots of formats for video files, and each format typically requires a different player and a different way to configure the player.</span></span> <span data-ttu-id="fae10-126">ASP.NET Razor ページでは、web ページを使用してビデオを再生できます、`Video`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="fae10-126">In ASP.NET Razor pages, you can play a video in a web page using the `Video` helper.</span></span> <span data-ttu-id="fae10-127">`Video`ヘルパーが自動的に生成されるため、web ページで動画の埋め込みのプロセスを簡略化、`object`と`embed`ページにビデオを追加することに通常使用される HTML 要素。</span><span class="sxs-lookup"><span data-stu-id="fae10-127">The `Video` helper simplifies the process of embedding videos in a web page because it automatically generates the `object` and `embed` HTML elements that are normally used to add video to the page.</span></span>

<span data-ttu-id="fae10-128">`Video`ヘルパーは、次のメディア プレーヤーをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="fae10-128">The `Video` helper supports the following media players:</span></span>

- <span data-ttu-id="fae10-129">Adobe Flash</span><span class="sxs-lookup"><span data-stu-id="fae10-129">Adobe Flash</span></span>
- <span data-ttu-id="fae10-130">Windows MediaPlayer</span><span class="sxs-lookup"><span data-stu-id="fae10-130">Windows MediaPlayer</span></span>
- <span data-ttu-id="fae10-131">Microsoft Silverlight</span><span class="sxs-lookup"><span data-stu-id="fae10-131">Microsoft Silverlight</span></span>

### <a name="the-flash-player"></a><span data-ttu-id="fae10-132">Flash Player</span><span class="sxs-lookup"><span data-stu-id="fae10-132">The Flash Player</span></span>

<span data-ttu-id="fae10-133">`Flash`のプレーヤー、`Video`ヘルパーを使用して、フラッシュのビデオを再生できます (*.swf*ファイル) で web ページ。</span><span class="sxs-lookup"><span data-stu-id="fae10-133">The `Flash` player of the `Video` helper let you play Flash videos (*.swf* files) in a web page.</span></span> <span data-ttu-id="fae10-134">少なくともは、ビデオ ファイルへのパスを指定する必要あります。</span><span class="sxs-lookup"><span data-stu-id="fae10-134">At a minimum, you have to provide a path to the video file.</span></span> <span data-ttu-id="fae10-135">何が、パスを指定する場合、player は Flash の現在のバージョンで設定されている既定値を使用します。</span><span class="sxs-lookup"><span data-stu-id="fae10-135">If you specify nothing but the path, the player uses default values that are set by the current version of Flash.</span></span> <span data-ttu-id="fae10-136">一般的な既定の設定は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="fae10-136">Typical default settings are:</span></span>

- <span data-ttu-id="fae10-137">ビデオは、既定の幅と高さを使用して表示されると、背景色を使用しない場合。</span><span class="sxs-lookup"><span data-stu-id="fae10-137">The video is displayed using its default width and height and without a background color.</span></span>
- <span data-ttu-id="fae10-138">ビデオは、ページの読み込み時に自動的に再生されます。</span><span class="sxs-lookup"><span data-stu-id="fae10-138">The video plays automatically when the page loads.</span></span>
- <span data-ttu-id="fae10-139">ビデオは、明示的に停止されるまでに継続的にループ処理します。</span><span class="sxs-lookup"><span data-stu-id="fae10-139">The video loops continuously until it's explicitly stopped.</span></span>
- <span data-ttu-id="fae10-140">ビデオをスケーリングして、特定のサイズに合わせてビデオをトリミングするのではなく、ビデオをすべて表示します。</span><span class="sxs-lookup"><span data-stu-id="fae10-140">The video is scaled to show all of the video, rather than cropping the video to fit a specific size.</span></span>
- <span data-ttu-id="fae10-141">ウィンドウで、ビデオを再生します。</span><span class="sxs-lookup"><span data-stu-id="fae10-141">The video plays in a window.</span></span>

### <a name="the-mediaplayer-player"></a><span data-ttu-id="fae10-142">MediaPlayer プレーヤー</span><span class="sxs-lookup"><span data-stu-id="fae10-142">The MediaPlayer Player</span></span>

<span data-ttu-id="fae10-143">`MediaPlayer`のプレーヤー、`Video`ヘルパーでは、Windows Media ビデオを再生することができます (*.wmv*ファイル)、Windows Media オーディオ (*.wma*ファイル)、および MP3 (*.mp3*web ページ内のファイル)。</span><span class="sxs-lookup"><span data-stu-id="fae10-143">The `MediaPlayer` player of the `Video` helper lets you play Windows Media videos (*.wmv* files), Windows Media audio (*.wma* files), and MP3 (*.mp3* files) in a web page.</span></span> <span data-ttu-id="fae10-144">を再生するメディア ファイルのパスを含める必要がありますその他のすべてのパラメーターは省略可能です。</span><span class="sxs-lookup"><span data-stu-id="fae10-144">You must include path of the media file to play; all other parameters are optional.</span></span> <span data-ttu-id="fae10-145">パスだけを指定する場合、player は既定の設定など、media Player の現在のバージョンで設定を使用します。</span><span class="sxs-lookup"><span data-stu-id="fae10-145">If you specify only a path, the player uses default settings set by the current version of MediaPlayer, such as:</span></span>

- <span data-ttu-id="fae10-146">既定の幅と高さを使用して、ビデオが表示されます。</span><span class="sxs-lookup"><span data-stu-id="fae10-146">The video is displayed using its default width and height.</span></span>
- <span data-ttu-id="fae10-147">ビデオは、ページの読み込み時に自動的に再生されます。</span><span class="sxs-lookup"><span data-stu-id="fae10-147">The video plays automatically when the page loads.</span></span>
- <span data-ttu-id="fae10-148">ビデオが 1 回再生 (にループしません)。</span><span class="sxs-lookup"><span data-stu-id="fae10-148">The video plays once (it doesn't loop).</span></span>
- <span data-ttu-id="fae10-149">ユーザー インターフェイスのコントロールの完全なセットが表示されます。</span><span class="sxs-lookup"><span data-stu-id="fae10-149">The player displays the full set of controls in the user interface.</span></span>
- <span data-ttu-id="fae10-150">ウィンドウで、ビデオを再生します。</span><span class="sxs-lookup"><span data-stu-id="fae10-150">The video plays in a window.</span></span>

### <a name="the-silverlight-player"></a><span data-ttu-id="fae10-151">Silverlight プレーヤー</span><span class="sxs-lookup"><span data-stu-id="fae10-151">The Silverlight Player</span></span>

<span data-ttu-id="fae10-152">`Silverlight`のプレーヤー、`Video`ヘルパーでは、Windows Media ビデオを再生することができます (*.wmv*ファイル)、Windows Media オーディオ (*.wma*ファイル)、および MP3 (*.mp3*ファイルの場合)。</span><span class="sxs-lookup"><span data-stu-id="fae10-152">The `Silverlight` player of the `Video` helper lets you play Windows Media Video (*.wmv* files), Windows Media Audio (*.wma* files), and MP3 (*.mp3* files).</span></span> <span data-ttu-id="fae10-153">Silverlight ベースのアプリケーション パッケージを指すパス パラメーターを設定する必要があります (*.xap*ファイル)。</span><span class="sxs-lookup"><span data-stu-id="fae10-153">You must set the path parameter to point to a Silverlight-based application package (*.xap* file).</span></span> <span data-ttu-id="fae10-154">また、幅と高さのパラメーターを設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fae10-154">You also must set the width and height parameters.</span></span> <span data-ttu-id="fae10-155">その他のすべてのパラメーターは省略できます。</span><span class="sxs-lookup"><span data-stu-id="fae10-155">All other parameters are optional.</span></span> <span data-ttu-id="fae10-156">必要なパラメーターのみを設定した場合にビデオについては、Silverlight プレーヤーを使用する Silverlight プレーヤーの背景色のないビデオが表示されます。</span><span class="sxs-lookup"><span data-stu-id="fae10-156">When you use the Silverlight player for video, if you set only the required parameters, the Silverlight player displays the video without a background color.</span></span>

> [!NOTE]
> <span data-ttu-id="fae10-157">Silverlight がまだわからない場合: *.xap*ファイルは、圧縮されたファイルのレイアウトの指示を含む、 *.xaml*ファイル、アセンブリ、および省略可能なリソースのマネージ コード。</span><span class="sxs-lookup"><span data-stu-id="fae10-157">In case you don't already know Silverlight: the *.xap* file is a compressed file that contains layout instructions in a *.xaml* file, managed code in assemblies, and optional resources.</span></span> <span data-ttu-id="fae10-158">作成することができます、 *.xap* Silverlight アプリケーション プロジェクトとして Visual Studio でのファイル。</span><span class="sxs-lookup"><span data-stu-id="fae10-158">You can create a *.xap* file in Visual Studio as a Silverlight application project.</span></span>


<span data-ttu-id="fae10-159">`Silverlight`ビデオ プレーヤーをプレーヤーに対して指定した設定とに用意されている設定の両方を使用して、 *.xap*ファイル。</span><span class="sxs-lookup"><span data-stu-id="fae10-159">The `Silverlight` video player uses both the settings that you provide for the player and the settings that are provided in the *.xap* file.</span></span>

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a><span data-ttu-id="fae10-160">MIME の種類</span><span class="sxs-lookup"><span data-stu-id="fae10-160">MIME Types</span></span>
> 
> <span data-ttu-id="fae10-161">ブラウザー ファイルをダウンロードするとき、ブラウザーは、ファイルの種類が表示されるドキュメントが指定されている MIME の種類と一致していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="fae10-161">When a browser downloads a file, the browser makes sure that the file type matches the MIME type that's specified for the document that's being rendered.</span></span> <span data-ttu-id="fae10-162">MIME の種類は、ファイルのコンテンツの種類またはメディアの種類です。</span><span class="sxs-lookup"><span data-stu-id="fae10-162">The MIME type is the content type or media type of a file.</span></span> <span data-ttu-id="fae10-163">`Video`ヘルパーは、次の MIME の種類を使用します。</span><span class="sxs-lookup"><span data-stu-id="fae10-163">The `Video` helper uses the following MIME types:</span></span>
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`


<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a><span data-ttu-id="fae10-164">フラッシュ (.swf) のビデオの再生</span><span class="sxs-lookup"><span data-stu-id="fae10-164">Playing Flash (.swf) Videos</span></span>

<span data-ttu-id="fae10-165">この手順は、という名前のフラッシュ ビデオを再生する方法を示します*sample.swf*します。</span><span class="sxs-lookup"><span data-stu-id="fae10-165">This procedure shows you how to play a Flash video named *sample.swf*.</span></span> <span data-ttu-id="fae10-166">という名前のフォルダーが得られたら、手順の前提条件*メディア*自分のサイトにある、 *.swf*ファイルがそのフォルダーにします。</span><span class="sxs-lookup"><span data-stu-id="fae10-166">The procedure assumes that you've got a folder named *Media* on your site and that the *.swf* file is in that folder.</span></span>

1. <span data-ttu-id="fae10-167">」の説明に従って、web サイトに、ASP.NET Web Helpers Library を追加[ASP.NET Web ページ サイトでインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)、既に追加していない場合。</span><span class="sxs-lookup"><span data-stu-id="fae10-167">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="fae10-168">Web サイトでページを追加し、名前*FlashVideo.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="fae10-168">In the website, add a page and name it *FlashVideo.cshtml*.</span></span>
3. <span data-ttu-id="fae10-169">次のマークアップをページに追加します。</span><span class="sxs-lookup"><span data-stu-id="fae10-169">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. <span data-ttu-id="fae10-170">ブラウザーでページを実行します。</span><span class="sxs-lookup"><span data-stu-id="fae10-170">Run the page in a browser.</span></span> <span data-ttu-id="fae10-171">(内でページが選択されていることを確認、**ファイル**ワークスペースを実行する前にします)。ページが表示され、ビデオを自動的に再生します。</span><span class="sxs-lookup"><span data-stu-id="fae10-171">(Make sure the page is selected in the **Files** workspace before you run it.) The page is displayed and the video plays automatically.</span></span> 

    <span data-ttu-id="fae10-172">![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")</span><span class="sxs-lookup"><span data-stu-id="fae10-172">![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")</span></span>

<span data-ttu-id="fae10-173">設定することができます、`quality`フラッシュ ビデオのパラメーター `low`、 `autolow`、 `autohigh`、 `medium`、 `high`、および`best`:</span><span class="sxs-lookup"><span data-stu-id="fae10-173">You can set the `quality` parameter for a Flash video to `low`, `autolow`, `autohigh`, `medium`, `high`, and `best`:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

<span data-ttu-id="fae10-174">フラッシュ ビデオを再生を使用して特定のサイズを変更する、`scale`パラメーターで、次を設定することができます。</span><span class="sxs-lookup"><span data-stu-id="fae10-174">You can change the Flash video to play at a specific size using the `scale` parameter, which you can set to the following:</span></span>

- <span data-ttu-id="fae10-175">`showall`。</span><span class="sxs-lookup"><span data-stu-id="fae10-175">`showall`.</span></span> <span data-ttu-id="fae10-176">これにより、元の縦横比を維持しながらビデオ全体を表示にします。</span><span class="sxs-lookup"><span data-stu-id="fae10-176">This makes the entire video visible while maintaining the original aspect ratio.</span></span> <span data-ttu-id="fae10-177">ただし、それぞれの側に境界線で入ることがあります。</span><span class="sxs-lookup"><span data-stu-id="fae10-177">However, you might end up with borders on each side.</span></span>
- <span data-ttu-id="fae10-178">`noorder`。</span><span class="sxs-lookup"><span data-stu-id="fae10-178">`noorder`.</span></span> <span data-ttu-id="fae10-179">元の縦横比を維持しながら、ビデオが拡大または縮小しますが、それがトリミングされる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="fae10-179">This scales the video while maintaining the original aspect ratio, but it might be cropped.</span></span>
- <span data-ttu-id="fae10-180">`exactfit`。</span><span class="sxs-lookup"><span data-stu-id="fae10-180">`exactfit`.</span></span> <span data-ttu-id="fae10-181">元の縦横比を維持せずビデオ全体を表示によりこれが、ゆがみが発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="fae10-181">This makes the entire video visible without preserving the original aspect ratio, but distortion may occur.</span></span>

<span data-ttu-id="fae10-182">指定しない場合、`scale`パラメーター、ビデオ全体が表示されます、トリミングせずに元の縦横比は維持されます。</span><span class="sxs-lookup"><span data-stu-id="fae10-182">If you don't specify a `scale` parameter, the entire video will be visible and the original aspect ratio will be maintained without any cropping.</span></span> <span data-ttu-id="fae10-183">次の例は、使用する方法を示します、`scale`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="fae10-183">The following example shows how to use the `scale` parameter:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

<span data-ttu-id="fae10-184">Flash player がビデオ モードという名前の設定をサポートしている`windowMode`します。</span><span class="sxs-lookup"><span data-stu-id="fae10-184">The Flash player supports a video mode setting named `windowMode`.</span></span> <span data-ttu-id="fae10-185">これを設定して`window`、 `opaque`、および`transparent`します。</span><span class="sxs-lookup"><span data-stu-id="fae10-185">You can set this to `window`, `opaque`, and `transparent`.</span></span> <span data-ttu-id="fae10-186">既定で、`windowMode`に設定されている`window`、web ページ上の別のウィンドウで、ビデオが表示されます。</span><span class="sxs-lookup"><span data-stu-id="fae10-186">By default, the `windowMode` is set to `window`, which displays the video in a separate window on the web page.</span></span> <span data-ttu-id="fae10-187">`opaque`設定では、web ページ上のビデオの背後にあるすべて非表示にします。</span><span class="sxs-lookup"><span data-stu-id="fae10-187">The `opaque` setting hides everything behind the video on the web page.</span></span> <span data-ttu-id="fae10-188">`transparent`設定により、ビデオを表示する web ページのバック グラウンド ビデオの任意の部分が透明であると仮定します。</span><span class="sxs-lookup"><span data-stu-id="fae10-188">The `transparent` setting lets the background of the web page show through the video, assuming any part of the video is transparent.</span></span>

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a><span data-ttu-id="fae10-189">Media Player の再生 (*.wmv*) のビデオ</span><span class="sxs-lookup"><span data-stu-id="fae10-189">Playing MediaPlayer (*.wmv*) Videos</span></span>

<span data-ttu-id="fae10-190">次の手順は、名前付きウィンドウ Media ビデオを再生する方法を示します*sample.wmv*内にある、*メディア*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="fae10-190">The following procedure shows you how to play a Window Media video named *sample.wmv* that's in the *Media* folder.</span></span>

1. <span data-ttu-id="fae10-191">」の説明に従って、web サイトに、ASP.NET Web Helpers Library を追加[ASP.NET Web ページ サイトでインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)まだ行っていない場合は、します。</span><span class="sxs-lookup"><span data-stu-id="fae10-191">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="fae10-192">という名前の新しいページを作成する*MediaPlayerVideo.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="fae10-192">Create a new page named *MediaPlayerVideo.cshtml*.</span></span>
3. <span data-ttu-id="fae10-193">次のマークアップをページに追加します。</span><span class="sxs-lookup"><span data-stu-id="fae10-193">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. <span data-ttu-id="fae10-194">ブラウザーでページを実行します。</span><span class="sxs-lookup"><span data-stu-id="fae10-194">Run the page in a browser.</span></span> <span data-ttu-id="fae10-195">ビデオでは、読み込みを自動的に再生します。</span><span class="sxs-lookup"><span data-stu-id="fae10-195">The video loads and plays automatically.</span></span> 

    <span data-ttu-id="fae10-196">![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")</span><span class="sxs-lookup"><span data-stu-id="fae10-196">![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")</span></span>

<span data-ttu-id="fae10-197">設定することができます`playCount`に自動的にビデオを再生する回数を示す整数です。</span><span class="sxs-lookup"><span data-stu-id="fae10-197">You can set `playCount` to an integer that indicates how many times to play the video automatically:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

<span data-ttu-id="fae10-198">`uiMode`パラメーターを使用して、ユーザー インターフェイスに表示するコントロールを指定できます。</span><span class="sxs-lookup"><span data-stu-id="fae10-198">The `uiMode` parameter lets you specify which controls show up in the user interface.</span></span> <span data-ttu-id="fae10-199">設定できる`uiMode`に`invisible`、 `none`、 `mini`、または`full`します。</span><span class="sxs-lookup"><span data-stu-id="fae10-199">You can set `uiMode` to `invisible`, `none`, `mini`, or `full`.</span></span> <span data-ttu-id="fae10-200">指定しない場合、`uiMode`パラメーター、ビデオになりますステータス ウィンドウに表示される、シーク バー ボタン、およびビデオのウィンドウのほかに、ボリューム コントロールを制御します。</span><span class="sxs-lookup"><span data-stu-id="fae10-200">If you don't specify a `uiMode` parameter, the video will be displayed with the status window, seek bar, control buttons, and volume controls in addition to the video window.</span></span> <span data-ttu-id="fae10-201">これらのコントロールが、オーディオ ファイルを再生するプレーヤーを使用する場合にも表示されます。</span><span class="sxs-lookup"><span data-stu-id="fae10-201">These controls will also be displayed if you use the player to play an audio file.</span></span> <span data-ttu-id="fae10-202">使用する方法の例を次に示します、`uiMode`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="fae10-202">Here's an example of how to use the `uiMode` parameter:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

<span data-ttu-id="fae10-203">既定では、オーディオは、ビデオの再生時に。</span><span class="sxs-lookup"><span data-stu-id="fae10-203">By default, audio is on when the video plays.</span></span> <span data-ttu-id="fae10-204">設定して、オーディオをミュートすることができます、`mute`パラメーターを true にします。</span><span class="sxs-lookup"><span data-stu-id="fae10-204">You can mute the audio by setting the `mute` parameter to true:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

<span data-ttu-id="fae10-205">MediaPlayer のビデオのオーディオ レベルを設定して制御することができます、`volume`パラメーターが 0 から 100 までの値。</span><span class="sxs-lookup"><span data-stu-id="fae10-205">You can control the audio level of the MediaPlayer video by setting the `volume` parameter to a value between 0 and 100.</span></span> <span data-ttu-id="fae10-206">既定値は 50 です。</span><span class="sxs-lookup"><span data-stu-id="fae10-206">The default value is 50.</span></span> <span data-ttu-id="fae10-207">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="fae10-207">Here's an example:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a><span data-ttu-id="fae10-208">Silverlight のビデオの再生</span><span class="sxs-lookup"><span data-stu-id="fae10-208">Playing Silverlight Videos</span></span>

<span data-ttu-id="fae10-209">この手順は、Silverlight に含まれているビデオを再生する方法を示します *.xap*という名前のフォルダーには、ページ*メディア*します。</span><span class="sxs-lookup"><span data-stu-id="fae10-209">This procedure shows you how to play video contained in a Silverlight *.xap* page that's in a folder named *Media*.</span></span>

1. <span data-ttu-id="fae10-210">」の説明に従って、web サイトに、ASP.NET Web Helpers Library を追加[ASP.NET Web ページ サイトでインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)まだ行っていない場合は、します。</span><span class="sxs-lookup"><span data-stu-id="fae10-210">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already .</span></span>
2. <span data-ttu-id="fae10-211">という名前の新しいページを作成する*SilverlightVideo.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="fae10-211">Create a new page named *SilverlightVideo.cshtml*.</span></span>
3. <span data-ttu-id="fae10-212">次のマークアップをページに追加します。</span><span class="sxs-lookup"><span data-stu-id="fae10-212">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. <span data-ttu-id="fae10-213">ブラウザーでページを実行します。</span><span class="sxs-lookup"><span data-stu-id="fae10-213">Run the page in a browser.</span></span> 

    <span data-ttu-id="fae10-214">![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")</span><span class="sxs-lookup"><span data-stu-id="fae10-214">![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="fae10-215">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="fae10-215">Additional Resources</span></span>


<span data-ttu-id="fae10-216">[Silverlight の概要](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)</span><span class="sxs-lookup"><span data-stu-id="fae10-216">[Silverlight Overview](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)</span></span>

[<span data-ttu-id="fae10-217">Flash のオブジェクトおよび埋め込みのタグの属性</span><span class="sxs-lookup"><span data-stu-id="fae10-217">Flash OBJECT and EMBED tag attributes</span></span>](http://kb2.adobe.com/cps/127/tn_12701.html)

<span data-ttu-id="fae10-218">[Windows Media Player 11 SDK PARAM タグします。](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)</span><span class="sxs-lookup"><span data-stu-id="fae10-218">[Windows Media Player 11 SDK PARAM Tags](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)</span></span>
