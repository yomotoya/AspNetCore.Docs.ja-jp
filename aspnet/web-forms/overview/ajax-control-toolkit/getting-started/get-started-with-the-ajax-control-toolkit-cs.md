---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
title: AJAX Control Toolkit (c#) の概要 |Microsoft Docs
author: microsoft
description: AJAX Control Toolkit の使用を開始するために必要なについて説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 16dc5c11-65be-4eae-a818-9fad7f8259c6
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
msc.type: authoredcontent
ms.openlocfilehash: 120213ce27858e2f0650b71c82f689ae4493d8a8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399598"
---
<a name="get-started-with-the-ajax-control-toolkit-c"></a><span data-ttu-id="ece2a-103">AJAX Control Toolkit (c#) の概要します。</span><span class="sxs-lookup"><span data-stu-id="ece2a-103">Get Started with the AJAX Control Toolkit (C#)</span></span>
====================
<span data-ttu-id="ece2a-104">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="ece2a-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="ece2a-105">AJAX Control Toolkit の使用を開始するために必要なについて説明します。</span><span class="sxs-lookup"><span data-stu-id="ece2a-105">Learn all you need to know to get started using the AJAX Control Toolkit.</span></span>


<span data-ttu-id="ece2a-106">AJAX Control Toolkit には、30 を超える無料コントロール、ASP.NET アプリケーションで使用できるが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ece2a-106">The AJAX Control Toolkit contains more than 30 free controls that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="ece2a-107">このチュートリアルでは、AJAX Control Toolkit をダウンロードして、toolkit のコントロールを Visual Studio または Visual Web Developer Express ツールボックスに追加する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="ece2a-107">In this tutorial, you learn how to download the AJAX Control Toolkit and add the toolkit controls to your Visual Studio/Visual Web Developer Express toolbox.</span></span>

## <a name="downloading-the-ajax-control-toolkit"></a><span data-ttu-id="ece2a-108">AJAX Control Toolkit のダウンロード</span><span class="sxs-lookup"><span data-stu-id="ece2a-108">Downloading the AJAX Control Toolkit</span></span>

<span data-ttu-id="ece2a-109">[AJAX Control Toolkit](http://devexpress.com/act)は、ASP.NET コミュニティと ASP.NET チームのメンバーによって開発されたオープン ソース プロジェクトです。</span><span class="sxs-lookup"><span data-stu-id="ece2a-109">The [AJAX Control Toolkit](http://devexpress.com/act) is an open source project developed by the members of the ASP.NET community and the ASP.NET team.</span></span> 


<span data-ttu-id="ece2a-110">[![AJAX Control Toolkit のダウンロード](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ece2a-110">[![Downloading the AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)</span></span>

<span data-ttu-id="ece2a-111">**図 01**: AJAX Control Toolkit のダウンロード ([フルサイズの画像を表示する をクリックします](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))。</span><span class="sxs-lookup"><span data-stu-id="ece2a-111">**Figure 01**: Downloading the AJAX Control Toolkit([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))</span></span>


<span data-ttu-id="ece2a-112">ファイルをダウンロードした後、ファイルのブロックを解除する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ece2a-112">After you download the file, you need to unblock the file.</span></span> <span data-ttu-id="ece2a-113">ファイルを右クリックし、プロパティを選択してをクリックして、**ブロック解除**(図 2 参照) ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="ece2a-113">Right-click the file, select Properties, and click the **Unblock** button (see Figure 2).</span></span>


<span data-ttu-id="ece2a-114">[![AJAX Control Toolkit の ZIP ファイルをブロック解除](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="ece2a-114">[![Unblocking the AJAX Control Toolkit ZIP file](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)</span></span>

<span data-ttu-id="ece2a-115">**図 02**: AJAX Control Toolkit の ZIP ファイルをブロック解除 ([フルサイズの画像を表示する をクリックします](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))。</span><span class="sxs-lookup"><span data-stu-id="ece2a-115">**Figure 02**: Unblocking the AJAX Control Toolkit ZIP file([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))</span></span>


<span data-ttu-id="ece2a-116">ファイルのブロックを解除すると後、は、ファイルを解凍できます: ファイルを右クリックして、**すべて展開**メニュー オプション。</span><span class="sxs-lookup"><span data-stu-id="ece2a-116">After you unblock the file, you can unzip the file: Right-click the file and select the **Extract All** menu option.</span></span> <span data-ttu-id="ece2a-117">ここで、Visual Studio または Visual Web Developer ツールボックスにこのツールキットを追加する準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="ece2a-117">Now, we are ready to add the toolkit to the Visual Studio/Visual Web Developer toolbox.</span></span>

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a><span data-ttu-id="ece2a-118">AJAX Control Toolkit をツールボックスに追加します。</span><span class="sxs-lookup"><span data-stu-id="ece2a-118">Adding the AJAX Control Toolkit to the Toolbox</span></span>

<span data-ttu-id="ece2a-119">AJAX Control Toolkit を使用する最も簡単な方法は、Visual Studio または Visual Web Developer ツールボックスにこのツールキットを追加する、(図 3 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="ece2a-119">The easiest way to use the AJAX Control Toolkit is to add the toolkit to your Visual Studio/Visual Web Developer toolbox (see Figure 3).</span></span> <span data-ttu-id="ece2a-120">そうすることだけドラッグできます toolkit のコントロールをページにそれを使用する場合。</span><span class="sxs-lookup"><span data-stu-id="ece2a-120">That way, you can simply drag a toolkit control onto a page when you want to use it.</span></span>


<span data-ttu-id="ece2a-121">[![AJAX Control Toolkit がツールボックスに表示されます。](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="ece2a-121">[![AJAX Control Toolkit appears in toolbox](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)</span></span>

<span data-ttu-id="ece2a-122">**図 03**: AJAX Control Toolkit がツールボックスに表示されます ([フルサイズの画像を表示する をクリックします](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))。</span><span class="sxs-lookup"><span data-stu-id="ece2a-122">**Figure 03**: AJAX Control Toolkit appears in toolbox([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))</span></span>


<span data-ttu-id="ece2a-123">最初に、AJAX Control Toolkit の タブをツールボックスに追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ece2a-123">First, you need to add an AJAX Control Toolkit tab to the toolbox.</span></span> <span data-ttu-id="ece2a-124">これらの手順に従います。</span><span class="sxs-lookup"><span data-stu-id="ece2a-124">Follow these steps.</span></span>

1. <span data-ttu-id="ece2a-125">メニュー オプション ファイルを新しい web サイトを選択して、新しい ASP.NET web サイトを作成します。</span><span class="sxs-lookup"><span data-stu-id="ece2a-125">Create a new ASP.NET Website by selecting the menu option File, New Website.</span></span> <span data-ttu-id="ece2a-126">ソリューション エクスプ ローラー ウィンドウで、エディターでファイルを開くの Default.aspx をダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="ece2a-126">Double-click the Default.aspx in the Solution Explorer window to open the file in the editor.</span></span>
2. <span data-ttu-id="ece2a-127">[全般] タブの下にあるツールボックスを右クリックし、メニュー オプションを選択**タブの追加**(図 4 参照)。</span><span class="sxs-lookup"><span data-stu-id="ece2a-127">Right-click the Toolbox beneath the General Tab and select the menu option **Add Tab** (see Figure 4).</span></span>
3. <span data-ttu-id="ece2a-128">AJAX Control Toolkit という名前の新しいタブを入力します。</span><span class="sxs-lookup"><span data-stu-id="ece2a-128">Enter a new tab named AJAX Control Toolkit.</span></span>


<span data-ttu-id="ece2a-129">[![新しいタブを追加します。](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="ece2a-129">[![Adding a new tab](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)</span></span>

<span data-ttu-id="ece2a-130">**図 04**: 新しいタブの追加 ([フルサイズの画像を表示する をクリックします](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))。</span><span class="sxs-lookup"><span data-stu-id="ece2a-130">**Figure 04**: Adding a new tab([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))</span></span>


<span data-ttu-id="ece2a-131">次に、新しいタブに、AJAX Control Toolkit コントロールを追加する必要があります。この場合は、以下の手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="ece2a-131">Next, you need to add the AJAX Control Toolkit controls to the new tab. Follow these steps:</span></span>

- <span data-ttu-id="ece2a-132">AJAX Control Toolkit] タブの下を右クリックし、メニュー オプションを選択 **[アイテムの選択 (図 5 参照)** します。</span><span class="sxs-lookup"><span data-stu-id="ece2a-132">Right-click beneath the AJAX Control Toolkit tab and select the menu option **Choose Items (see Figure 5)**.</span></span>
- <span data-ttu-id="ece2a-133">AJAX Control Toolkit を解凍するを AjaxControlToolkit.dll アセンブリを選択する場所に移動します。</span><span class="sxs-lookup"><span data-stu-id="ece2a-133">Browse to the location where you unzipped the AJAX Control Toolkit and select the AjaxControlToolkit.dll assembly.</span></span>


<span data-ttu-id="ece2a-134">[![ツールボックスに追加する項目を選択します。](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="ece2a-134">[![Choose items to add to the toolbox](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)</span></span>

<span data-ttu-id="ece2a-135">**図 05**: ツールボックスに追加する項目を選択 ([フルサイズの画像を表示する をクリックします](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))。</span><span class="sxs-lookup"><span data-stu-id="ece2a-135">**Figure 05**: Choose items to add to the toolbox([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))</span></span>


<span data-ttu-id="ece2a-136">次の手順を完了するは、ツールボックスに toolkit のコントロールのすべて表示されます。</span><span class="sxs-lookup"><span data-stu-id="ece2a-136">After you complete these steps, all of the toolkit controls will appear in your toolbox.</span></span>

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a><span data-ttu-id="ece2a-137">ツールキットの新しいバージョンにアップグレードします。</span><span class="sxs-lookup"><span data-stu-id="ece2a-137">Upgrading to a New Version of the Toolkit</span></span>

<span data-ttu-id="ece2a-138">移動する必要があり、旧バージョンのツールキットを使用していた場合、以降のバージョンとここでは推奨される手順を示します。</span><span class="sxs-lookup"><span data-stu-id="ece2a-138">If you were using an older release of the Toolkit and now need to move to a later version here are the recommended steps:</span></span>

- <span data-ttu-id="ece2a-139">バイナリには、web サイトの Bin フォルダーから AjaxControlToolkit.dll アセンブリの古いバージョンを削除します。</span><span class="sxs-lookup"><span data-stu-id="ece2a-139">Binaries - Delete the old version of the AjaxControlToolkit.dll assembly from your website Bin folder.</span></span>
- <span data-ttu-id="ece2a-140">ツールボックス項目には、AJAX Control Toolkit タブを削除し、AjaxControlToolkit.dll アセンブリの新しいバージョンで、タブの再作成に上記の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="ece2a-140">Toolbox Items - Delete the AJAX Control Toolkit tab and follow the steps above to re-create the tab with the new version of the AjaxControlToolkit.dll assembly.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ece2a-141">次へ</span><span class="sxs-lookup"><span data-stu-id="ece2a-141">Next</span></span>](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
