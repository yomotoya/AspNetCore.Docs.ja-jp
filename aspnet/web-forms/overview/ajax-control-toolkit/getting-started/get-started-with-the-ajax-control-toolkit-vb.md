---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-vb
title: AJAX コントロール ツールキット (VB) を使用して作業を開始 |Microsoft ドキュメント
author: microsoft
description: AJAX コントロール ツールキットを使用して作業を開始するために知っておく必要がありますすべてを説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 9f8fa166-49a2-402c-b236-20caef0c658f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-vb
msc.type: authoredcontent
ms.openlocfilehash: 30653a147bd3bf581af27220e11cdecc2f89fc4a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="get-started-with-the-ajax-control-toolkit-vb"></a><span data-ttu-id="53eb3-103">AJAX コントロール Toolkit (VB) の概要します。</span><span class="sxs-lookup"><span data-stu-id="53eb3-103">Get Started with the AJAX Control Toolkit (VB)</span></span>
====================
<span data-ttu-id="53eb3-104">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="53eb3-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="53eb3-105">AJAX コントロール ツールキットを使用して作業を開始するために知っておく必要がありますすべてを説明します。</span><span class="sxs-lookup"><span data-stu-id="53eb3-105">Learn all you need to know to get started using the AJAX Control Toolkit.</span></span>


<span data-ttu-id="53eb3-106">AJAX コントロール Toolkit には、ASP.NET アプリケーションで使用できる 30 以上の空きコントロールが含まれます。</span><span class="sxs-lookup"><span data-stu-id="53eb3-106">The AJAX Control Toolkit contains more than 30 free controls that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="53eb3-107">このチュートリアルでは、AJAX コントロール ツールキットをダウンロードして、toolkit のコントロールを Visual Studio または Visual Web Developer Express ツールボックスに追加する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="53eb3-107">In this tutorial, you learn how to download the AJAX Control Toolkit and add the toolkit controls to your Visual Studio/Visual Web Developer Express toolbox.</span></span>

## <a name="downloading-the-ajax-control-toolkit"></a><span data-ttu-id="53eb3-108">AJAX コントロール Toolkit のダウンロード</span><span class="sxs-lookup"><span data-stu-id="53eb3-108">Downloading the AJAX Control Toolkit</span></span>

<span data-ttu-id="53eb3-109">[AJAX コントロール Toolkit](http://devexpress.com/act)は、ASP.NET コミュニティや ASP.NET のチームのメンバーによって開発されたオープン ソース プロジェクトです。</span><span class="sxs-lookup"><span data-stu-id="53eb3-109">The [AJAX Control Toolkit](http://devexpress.com/act) is an open source project developed by the members of the ASP.NET community and the ASP.NET team.</span></span>


<span data-ttu-id="53eb3-110">[![AJAX コントロール Toolkit のダウンロード](get-started-with-the-ajax-control-toolkit-vb/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="53eb3-110">[![Downloading the AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-vb/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image1.png)</span></span>

<span data-ttu-id="53eb3-111">**図 01**: AJAX コントロール Toolkit のダウンロード ([フルサイズのイメージを表示するをクリックして](get-started-with-the-ajax-control-toolkit-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="53eb3-111">**Figure 01**: Downloading the AJAX Control Toolkit([Click to view full-size image](get-started-with-the-ajax-control-toolkit-vb/_static/image2.png))</span></span>


<span data-ttu-id="53eb3-112">ファイルをダウンロードした後は、ファイルのブロックを解除する必要があります。</span><span class="sxs-lookup"><span data-stu-id="53eb3-112">After you download the file, you need to unblock the file.</span></span> <span data-ttu-id="53eb3-113">ファイルを右クリックし、プロパティを選択し、クリックして、**ブロックを解除する**(図 2 を参照してください) ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="53eb3-113">Right-click the file, select Properties, and click the **Unblock** button (see Figure 2).</span></span>


<span data-ttu-id="53eb3-114">[![AJAX コントロール Toolkit の ZIP ファイルのブロックを解除](get-started-with-the-ajax-control-toolkit-vb/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="53eb3-114">[![Unblocking the AJAX Control Toolkit ZIP file](get-started-with-the-ajax-control-toolkit-vb/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image3.png)</span></span>

<span data-ttu-id="53eb3-115">**図 02**: AJAX コントロール Toolkit の ZIP ファイルのブロックを解除 ([フルサイズのイメージを表示するをクリックして](get-started-with-the-ajax-control-toolkit-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="53eb3-115">**Figure 02**: Unblocking the AJAX Control Toolkit ZIP file([Click to view full-size image](get-started-with-the-ajax-control-toolkit-vb/_static/image4.png))</span></span>


<span data-ttu-id="53eb3-116">ファイルのブロックを解除すると後、は、ファイルを解凍することができます: ファイルを右クリックして、**すべて展開**メニュー オプション。</span><span class="sxs-lookup"><span data-stu-id="53eb3-116">After you unblock the file, you can unzip the file: Right-click the file and select the **Extract All** menu option.</span></span> <span data-ttu-id="53eb3-117">ここで、このツールキットを Visual Studio または Visual Web Developer ツールボックスに追加する準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="53eb3-117">Now, we are ready to add the toolkit to the Visual Studio/Visual Web Developer toolbox.</span></span>

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a><span data-ttu-id="53eb3-118">ツールボックスへの AJAX コントロール Toolkit の追加</span><span class="sxs-lookup"><span data-stu-id="53eb3-118">Adding the AJAX Control Toolkit to the Toolbox</span></span>

<span data-ttu-id="53eb3-119">AJAX コントロール ツールキットを使用する最も簡単な方法は、Visual Studio または Visual Web Developer ツールボックスにツールキットを追加する (図 3 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="53eb3-119">The easiest way to use the AJAX Control Toolkit is to add the toolkit to your Visual Studio/Visual Web Developer toolbox (see Figure 3).</span></span> <span data-ttu-id="53eb3-120">こうすれば、単にドラッグできます toolkit コントロール ページにそれを使用する場合。</span><span class="sxs-lookup"><span data-stu-id="53eb3-120">That way, you can simply drag a toolkit control onto a page when you want to use it.</span></span>


<span data-ttu-id="53eb3-121">[![AJAX コントロール Toolkit ツールボックスに表示されます。](get-started-with-the-ajax-control-toolkit-vb/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="53eb3-121">[![AJAX Control Toolkit appears in toolbox](get-started-with-the-ajax-control-toolkit-vb/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image5.png)</span></span>

<span data-ttu-id="53eb3-122">**図 03**: AJAX コントロール Toolkit ツールボックスに表示されます ([フルサイズのイメージを表示するをクリックして](get-started-with-the-ajax-control-toolkit-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="53eb3-122">**Figure 03**: AJAX Control Toolkit appears in toolbox([Click to view full-size image](get-started-with-the-ajax-control-toolkit-vb/_static/image6.png))</span></span>


<span data-ttu-id="53eb3-123">最初に、AJAX コントロール ツールキット タブがツールボックスに追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="53eb3-123">First, you need to add an AJAX Control Toolkit tab to the toolbox.</span></span> <span data-ttu-id="53eb3-124">これらの手順に従います。</span><span class="sxs-lookup"><span data-stu-id="53eb3-124">Follow these steps.</span></span>

1. <span data-ttu-id="53eb3-125">新しい web サイト メニュー オプション、ファイルを選択して、新しい ASP.NET web サイトを作成します。</span><span class="sxs-lookup"><span data-stu-id="53eb3-125">Create a new ASP.NET Website by selecting the menu option File, New Website.</span></span> <span data-ttu-id="53eb3-126">エディターでファイルを開くソリューション エクスプ ローラー ウィンドウで、Default.aspx をダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="53eb3-126">Double-click the Default.aspx in the Solution Explorer window to open the file in the editor.</span></span>
2. <span data-ttu-id="53eb3-127">[全般] タブの下にあるツールボックスを右クリックし、メニュー オプションを選択**タブの追加**(図 4 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="53eb3-127">Right-click the Toolbox beneath the General Tab and select the menu option **Add Tab** (see Figure 4).</span></span>
3. <span data-ttu-id="53eb3-128">AJAX コントロール ツールキットをという名前の新しいタブを入力します。</span><span class="sxs-lookup"><span data-stu-id="53eb3-128">Enter a new tab named AJAX Control Toolkit.</span></span>


<span data-ttu-id="53eb3-129">[![新しいタブを追加します。](get-started-with-the-ajax-control-toolkit-vb/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="53eb3-129">[![Adding a new tab](get-started-with-the-ajax-control-toolkit-vb/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image7.png)</span></span>

<span data-ttu-id="53eb3-130">**図 04**: 新しいタブの追加 ([フルサイズのイメージを表示するをクリックして](get-started-with-the-ajax-control-toolkit-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="53eb3-130">**Figure 04**: Adding a new tab([Click to view full-size image](get-started-with-the-ajax-control-toolkit-vb/_static/image8.png))</span></span>


<span data-ttu-id="53eb3-131">次に、新しいタブに AJAX コントロール Toolkit コントロールを追加する必要があります。この場合は、以下の手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="53eb3-131">Next, you need to add the AJAX Control Toolkit controls to the new tab. Follow these steps:</span></span>

- <span data-ttu-id="53eb3-132">AJAX コントロール ツールキット] タブの下を右クリックし、メニュー オプションを選択**[アイテムの選択 (図 5 を参照してください)**です。</span><span class="sxs-lookup"><span data-stu-id="53eb3-132">Right-click beneath the AJAX Control Toolkit tab and select the menu option **Choose Items (see Figure 5)**.</span></span>
- <span data-ttu-id="53eb3-133">AJAX コントロール ツールキットを解凍し、選択 AjaxControlToolkit.dll アセンブリの場所を参照します。</span><span class="sxs-lookup"><span data-stu-id="53eb3-133">Browse to the location where you unzipped the AJAX Control Toolkit and select the AjaxControlToolkit.dll assembly.</span></span>


<span data-ttu-id="53eb3-134">[![ツールボックスに追加する項目を選択します。](get-started-with-the-ajax-control-toolkit-vb/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="53eb3-134">[![Choose items to add to the toolbox](get-started-with-the-ajax-control-toolkit-vb/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image9.png)</span></span>

<span data-ttu-id="53eb3-135">**図 05**: ツールボックスに追加する項目の選択 ([フルサイズのイメージを表示するをクリックして](get-started-with-the-ajax-control-toolkit-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="53eb3-135">**Figure 05**: Choose items to add to the toolbox([Click to view full-size image](get-started-with-the-ajax-control-toolkit-vb/_static/image10.png))</span></span>


<span data-ttu-id="53eb3-136">これらの手順を実行するツールキット コントロールのすべて、ツールボックスに表示されます。</span><span class="sxs-lookup"><span data-stu-id="53eb3-136">After you complete these steps, all of the toolkit controls will appear in your toolbox.</span></span>

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a><span data-ttu-id="53eb3-137">ツールキットの新しいバージョンにアップグレードします。</span><span class="sxs-lookup"><span data-stu-id="53eb3-137">Upgrading to a New Version of the Toolkit</span></span>

<span data-ttu-id="53eb3-138">ツールキットの古いリリースを使用していたしてに移動する必要があります、以降のバージョンは、ここは推奨される手順を示します。</span><span class="sxs-lookup"><span data-stu-id="53eb3-138">If you were using an older release of the Toolkit and now need to move to a later version here are the recommended steps:</span></span>

- <span data-ttu-id="53eb3-139">バイナリには、web サイトの Bin フォルダーから AjaxControlToolkit.dll アセンブリの古いバージョンを削除します。</span><span class="sxs-lookup"><span data-stu-id="53eb3-139">Binaries - Delete the old version of the AjaxControlToolkit.dll assembly from your website Bin folder.</span></span>
- <span data-ttu-id="53eb3-140">ツールボックス項目では、AJAX コントロール ツールキット タブを削除し、AjaxControlToolkit.dll アセンブリの新しいバージョンで、タブの再作成に上記の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="53eb3-140">Toolbox Items - Delete the AJAX Control Toolkit tab and follow the steps above to re-create the tab with the new version of the AjaxControlToolkit.dll assembly.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="53eb3-141">[前へ](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)
> [次へ](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)</span><span class="sxs-lookup"><span data-stu-id="53eb3-141">[Previous](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)
[Next](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)</span></span>
