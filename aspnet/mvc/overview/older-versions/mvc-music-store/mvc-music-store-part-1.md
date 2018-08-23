---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 'パート 1: 概要と新しいプロジェクト]-> [ファイル |Microsoft Docs'
author: jongalloway
description: このチュートリアル シリーズでは、すべての ASP.NET MVC のミュージック ストア サンプル アプリケーションをビルドする手順について説明します。 第 1 部では概要とファイルには、新しいプロジェクト]-> [です。
ms.author: riande
ms.date: 04/21/2011
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: d84a525221e40b79853be55069367134d17fb5ec
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838868"
---
<a name="part-1-overview-and-file-new-project"></a><span data-ttu-id="b8e55-104">パート 1: 概要と新しいプロジェクト]-> [ファイル</span><span class="sxs-lookup"><span data-stu-id="b8e55-104">Part 1: Overview and File->New Project</span></span>
====================
<span data-ttu-id="b8e55-105">によって[Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="b8e55-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="b8e55-106">MVC のミュージック ストアは、チュートリアル アプリケーションを紹介し、web 開発用の ASP.NET MVC と Visual Studio を使用する方法をステップ バイ ステップについて説明します。</span><span class="sxs-lookup"><span data-stu-id="b8e55-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="b8e55-107">MVC のミュージック ストアは、オンラインで音楽のアルバムを販売し、基本的なサイトの管理、ユーザー サインインし、買い物カゴの機能を実装する軽量サンプル ストア実装です。</span><span class="sxs-lookup"><span data-stu-id="b8e55-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="b8e55-108">このチュートリアル シリーズでは、すべての ASP.NET MVC のミュージック ストア サンプル アプリケーションをビルドする手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="b8e55-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="b8e55-109">パート 1 では、概要とファイルの&gt;新しいプロジェクト。</span><span class="sxs-lookup"><span data-stu-id="b8e55-109">Part 1 covers Overview and File-&gt;New Project.</span></span>


## <a name="overview"></a><span data-ttu-id="b8e55-110">概要</span><span class="sxs-lookup"><span data-stu-id="b8e55-110">Overview</span></span>

<span data-ttu-id="b8e55-111">MVC のミュージック ストアは、紹介し、web 開発用の ASP.NET MVC と Visual Web Developer を使用する方法を順を追って説明するチュートリアル アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="b8e55-111">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Web Developer for web development.</span></span> <span data-ttu-id="b8e55-112">私たちはゆっくりと開始されます、初心者レベルの web 開発のエクスペリエンスは問題ありません。</span><span class="sxs-lookup"><span data-stu-id="b8e55-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="b8e55-113">ビルドするアプリケーションは、単純な音楽ストアです。</span><span class="sxs-lookup"><span data-stu-id="b8e55-113">The application we'll be building is a simple music store.</span></span> <span data-ttu-id="b8e55-114">アプリケーションに 3 つの主要な部分がある: ショッピング、チェック アウト、および管理します。</span><span class="sxs-lookup"><span data-stu-id="b8e55-114">There are three main parts to the application: shopping, checkout, and administration.</span></span>

![](mvc-music-store-part-1/_static/image1.jpg)

<span data-ttu-id="b8e55-115">訪問者には、ジャンルのアルバムを参照できます。</span><span class="sxs-lookup"><span data-stu-id="b8e55-115">Visitors can browse Albums by Genre:</span></span>

![](mvc-music-store-part-1/_static/image2.jpg)

<span data-ttu-id="b8e55-116">1 つのアルバムを表示およびカートに追加できます。</span><span class="sxs-lookup"><span data-stu-id="b8e55-116">They can view a single album and add it to their cart:</span></span>

![](mvc-music-store-part-1/_static/image3.jpg)

<span data-ttu-id="b8e55-117">必要がなくなったすべての項目を削除する、カートを確認することができます。</span><span class="sxs-lookup"><span data-stu-id="b8e55-117">They can review their cart, removing any items they no longer want:</span></span>

![](mvc-music-store-part-1/_static/image4.jpg)

<span data-ttu-id="b8e55-118">チェック アウトに進むには、ログインまたはユーザー アカウントを登録することを求められます。</span><span class="sxs-lookup"><span data-stu-id="b8e55-118">Proceeding to Checkout will prompt them to login or register for a user account.</span></span>

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

<span data-ttu-id="b8e55-119">アカウントを作成すると、配布と支払い情報を入力して、注文を完了します。</span><span class="sxs-lookup"><span data-stu-id="b8e55-119">After creating an account, they can complete the order by filling out shipping and payment information.</span></span> <span data-ttu-id="b8e55-120">すばらしい昇格実行をことをシンプルにする: すべてがプロモーション コード"FREE"を入力している場合は、無料!</span><span class="sxs-lookup"><span data-stu-id="b8e55-120">To keep things simple, we're running an amazing promotion: everything's free if they enter promotion code "FREE"!</span></span>

![](mvc-music-store-part-1/_static/image5.jpg)

<span data-ttu-id="b8e55-121">、順序付けの後に、単純な確認画面が参照してください。</span><span class="sxs-lookup"><span data-stu-id="b8e55-121">After ordering, they see a simple confirmation screen:</span></span>

![](mvc-music-store-part-1/_static/image6.jpg)

<span data-ttu-id="b8e55-122">だけでなく、顧客 faceing ページは、アルバムをから管理者を作成できます、編集の一覧を表示する管理者セクションを構築し、アルバムを削除しましたがも。</span><span class="sxs-lookup"><span data-stu-id="b8e55-122">In addition to customer-faceing pages, we'll also build an administrator section that shows a list of albums from which Administrators can Create, Edit, and Delete albums:</span></span>

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a><span data-ttu-id="b8e55-123">1.ファイル -&gt;新しいプロジェクト</span><span class="sxs-lookup"><span data-stu-id="b8e55-123">1. File -&gt; New Project</span></span>

### <a name="installing-the-software"></a><span data-ttu-id="b8e55-124">ソフトウェアのインストール</span><span class="sxs-lookup"><span data-stu-id="b8e55-124">Installing the software</span></span>

<span data-ttu-id="b8e55-125">このチュートリアルでは、無料 Visual Web Developer 2010 Express (これは無料) を使用して新しい ASP.NET MVC 3 プロジェクトの作成、まず、差分を追加してみます完全に機能しているアプリケーションを作成する機能。</span><span class="sxs-lookup"><span data-stu-id="b8e55-125">This tutorial will begin by creating a new ASP.NET MVC 3 project using the free Visual Web Developer 2010 Express (which is free), and then we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="b8e55-126">その過程について説明しますデータベースへのアクセス、フォーム ポスト シナリオ、データの検証、ページ更新と検証、ユーザーのログイン、および AJAX を使用して、一貫性のあるページ レイアウトのマスター ページを使用します。</span><span class="sxs-lookup"><span data-stu-id="b8e55-126">Along the way, we'll cover database access, form posting scenarios, data validation, using master pages for consistent page layout, using AJAX for page updates and validation, user login, and more.</span></span>

<span data-ttu-id="b8e55-127">ステップ バイ ステップに従うことができますか、完成したアプリケーションをダウンロードできます[MVC-ミュージック ストア](https://github.com/evilDave/MVC-Music-Store)します。</span><span class="sxs-lookup"><span data-stu-id="b8e55-127">You can follow along step by step, or you can download the completed application from [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="b8e55-128">Visual Studio 2010 SP1 または Visual Web Developer 2010 Express SP1 (Visual Studio 2010 の無料版) のいずれかを使用するには、アプリケーションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="b8e55-128">You can use either Visual Studio 2010 SP1 or Visual Web Developer 2010 Express SP1 (a free version of Visual Studio 2010) to build the application.</span></span> <span data-ttu-id="b8e55-129">使用する、SQL Server Compact (も無料)、データベースをホストします。</span><span class="sxs-lookup"><span data-stu-id="b8e55-129">We'll be using the SQL Server Compact (also free) to host the database.</span></span> <span data-ttu-id="b8e55-130">始める前に、以下の前提条件がインストールされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="b8e55-130">Before you start, make sure you've installed the prerequisites listed below.</span></span>


- <span data-ttu-id="b8e55-131">[Visual Studio Web Developer Express SP1 の前提条件]</span><span class="sxs-lookup"><span data-stu-id="b8e55-131">[Visual Studio Web Developer Express SP1 prerequisites]</span></span>
- <span data-ttu-id="b8e55-132">[ASP.NET MVC 3 Tools Update]</span><span class="sxs-lookup"><span data-stu-id="b8e55-132">[ASP.NET MVC 3 Tools Update]</span></span>
- <span data-ttu-id="b8e55-133">[SQL Server Compact 4.0] - ランタイムとツールの両方のサポートを含む</span><span class="sxs-lookup"><span data-stu-id="b8e55-133">[SQL Server Compact 4.0] - including both runtime and tools support</span></span>


### <a name="creating-a-new-aspnet-mvc-3-project"></a><span data-ttu-id="b8e55-134">新しい ASP.NET MVC 3 プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="b8e55-134">Creating a new ASP.NET MVC 3 project</span></span>

<span data-ttu-id="b8e55-135">まず、Visual Web Developer で [ファイル] メニューから [新しいプロジェクト] を選択します。</span><span class="sxs-lookup"><span data-stu-id="b8e55-135">We'll start by selecting "New Project" from the File menu in Visual Web Developer.</span></span> <span data-ttu-id="b8e55-136">新しいプロジェクト ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b8e55-136">This brings up the New Project dialog.</span></span>

![](mvc-music-store-part-1/_static/image5.png)

<span data-ttu-id="b8e55-137">選択、Visual c# -&gt; Web テンプレートは、左側のグループ化し、中央の列で、「ASP.NET MVC 3 Web アプリケーション」テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="b8e55-137">We'll select the Visual C# -&gt; Web Templates group on the left, then choose the "ASP.NET MVC 3 Web Application" template in the center column.</span></span> <span data-ttu-id="b8e55-138">MvcMusicStore をプロジェクトに名前を [ok] ボタンを押します。</span><span class="sxs-lookup"><span data-stu-id="b8e55-138">Name your project MvcMusicStore and press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image8.jpg)

<span data-ttu-id="b8e55-139">これにより、特定の MVC プロジェクトの設定を作成することが可能セカンダリ ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b8e55-139">This will display a secondary dialog which allows us to make some MVC specific settings for our project.</span></span> <span data-ttu-id="b8e55-140">次を選択します。</span><span class="sxs-lookup"><span data-stu-id="b8e55-140">Select the following:</span></span>

<span data-ttu-id="b8e55-141">プロジェクト テンプレート - 空を選択します。</span><span class="sxs-lookup"><span data-stu-id="b8e55-141">Project Template - select Empty</span></span>

<span data-ttu-id="b8e55-142">エンジンの表示 - Razor を選択します。</span><span class="sxs-lookup"><span data-stu-id="b8e55-142">View Engine - select Razor</span></span>

<span data-ttu-id="b8e55-143">HTML5 セマンティック マークアップ - チェックの使用します。</span><span class="sxs-lookup"><span data-stu-id="b8e55-143">Use HTML5 semantic markup - checked</span></span>

<span data-ttu-id="b8e55-144">設定は、次に示すように、[ok] ボタンを押して確認します。</span><span class="sxs-lookup"><span data-stu-id="b8e55-144">Verify that your settings are as shown below, then press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image9.jpg)

<span data-ttu-id="b8e55-145">これにより、プロジェクトが作成されます。</span><span class="sxs-lookup"><span data-stu-id="b8e55-145">This will create our project.</span></span> <span data-ttu-id="b8e55-146">右側にある ソリューション エクスプ ローラーで、アプリケーションに追加されたフォルダーを見ていきましょう。</span><span class="sxs-lookup"><span data-stu-id="b8e55-146">Let's take a look at the folders that have been added to our application in the Solution Explorer on the right side.</span></span>

![](mvc-music-store-part-1/_static/image10.jpg)

<span data-ttu-id="b8e55-147">空の MVC 3 のテンプレートが完全に空でない – 基本的なフォルダー構造を追加します。</span><span class="sxs-lookup"><span data-stu-id="b8e55-147">The Empty MVC 3 template isn't completely empty – it adds a basic folder structure:</span></span>

![](mvc-music-store-part-1/_static/image6.png)

<span data-ttu-id="b8e55-148">ASP.NET MVC フォルダー名をいくつかの基本的な名前付け規則の使用します。</span><span class="sxs-lookup"><span data-stu-id="b8e55-148">ASP.NET MVC makes use of some basic naming conventions for folder names:</span></span>

| <span data-ttu-id="b8e55-149">**フォルダー**</span><span class="sxs-lookup"><span data-stu-id="b8e55-149">**Folder**</span></span> | <span data-ttu-id="b8e55-150">**目的**</span><span class="sxs-lookup"><span data-stu-id="b8e55-150">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="b8e55-151">**/コント ローラー**</span><span class="sxs-lookup"><span data-stu-id="b8e55-151">**/Controllers**</span></span> | <span data-ttu-id="b8e55-152">コント ローラーは、ブラウザーからの入力を操作、およびユーザーへの応答を返す方法を決めるに応答します。</span><span class="sxs-lookup"><span data-stu-id="b8e55-152">Controllers respond to input from the browser, decide what to do with it, and return response to the user.</span></span> |
| <span data-ttu-id="b8e55-153">**/Views**</span><span class="sxs-lookup"><span data-stu-id="b8e55-153">**/Views**</span></span> | <span data-ttu-id="b8e55-154">ビューは、UI テンプレートを保持します。</span><span class="sxs-lookup"><span data-stu-id="b8e55-154">Views hold our UI templates</span></span> |
| <span data-ttu-id="b8e55-155">**/モデル**</span><span class="sxs-lookup"><span data-stu-id="b8e55-155">**/Models**</span></span> | <span data-ttu-id="b8e55-156">モデルの保持し、データの操作</span><span class="sxs-lookup"><span data-stu-id="b8e55-156">Models hold and manipulate data</span></span> |
| <span data-ttu-id="b8e55-157">**/Content**</span><span class="sxs-lookup"><span data-stu-id="b8e55-157">**/Content**</span></span> | <span data-ttu-id="b8e55-158">このフォルダーは、イメージ、CSS、およびその他の静的コンテンツを保持します。</span><span class="sxs-lookup"><span data-stu-id="b8e55-158">This folder holds our images, CSS, and any other static content</span></span> |
| <span data-ttu-id="b8e55-159">**/Scripts**</span><span class="sxs-lookup"><span data-stu-id="b8e55-159">**/Scripts**</span></span> | <span data-ttu-id="b8e55-160">このフォルダーは、JavaScript ファイルを保持します。</span><span class="sxs-lookup"><span data-stu-id="b8e55-160">This folder holds our JavaScript files</span></span> |

<span data-ttu-id="b8e55-161">既定では、ASP.NET MVC フレームワークは、「設定より規約」の手法を使用し、フォルダーの名前付け規則に基づくいくつかの既定の解釈は、ために、これらのフォルダーが空の ASP.NET MVC アプリケーションであっても含まれます。</span><span class="sxs-lookup"><span data-stu-id="b8e55-161">These folders are included even in an Empty ASP.NET MVC application because the ASP.NET MVC framework by default uses a "convention over configuration" approach and makes some default assumptions based on folder naming conventions.</span></span> <span data-ttu-id="b8e55-162">たとえば、コント ローラーを検索、Views フォルダー内のビュー既定では、コードでこれを明示的に指定する必要がありません。</span><span class="sxs-lookup"><span data-stu-id="b8e55-162">For instance, controllers look for views in the Views folder by default without you having to explicitly specify this in your code.</span></span> <span data-ttu-id="b8e55-163">既定の規則を使用することでコードを記述する必要がある量を削減できますも容易にできるように、プロジェクトを理解するには、他の開発者とします。</span><span class="sxs-lookup"><span data-stu-id="b8e55-163">Sticking with the default conventions reduces the amount of code you need to write, and can also make it easier for other developers to understand your project.</span></span> <span data-ttu-id="b8e55-164">これらの規則は、アプリケーションの構築に詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="b8e55-164">We'll explain these conventions more as we build our application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="b8e55-165">次へ</span><span class="sxs-lookup"><span data-stu-id="b8e55-165">Next</span></span>](mvc-music-store-part-2.md)
