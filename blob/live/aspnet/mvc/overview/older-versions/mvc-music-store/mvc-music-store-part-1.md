---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: "パート 1: 概要と新しいプロジェクト]-> [ファイル |Microsoft ドキュメント"
author: jongalloway
description: "このチュートリアルの系列では、すべての ASP.NET MVC Music Store サンプル アプリケーションをビルドする手順について説明します。 パート 1 カバーの概要とファイルには、新しいプロジェクト]-> [です。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: 1e3373a21c7d1766cfad390a7ba68b1363d8d895
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="part-1-overview-and-file-new-project"></a><span data-ttu-id="a251b-104">パート 1: 概要と新しいプロジェクト]-> [ファイル</span><span class="sxs-lookup"><span data-stu-id="a251b-104">Part 1: Overview and File->New Project</span></span>
====================
<span data-ttu-id="a251b-105">によって[Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="a251b-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="a251b-106">MVC Music Store は、チュートリアル アプリケーションを紹介し、ASP.NET MVC と Visual Studio web 開発を使用する方法の詳細な手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="a251b-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="a251b-107">MVC 音楽ストアは、オンラインにして、音楽のアルバムを販売し、基本的なサイトの管理、ユーザー サインインおよび買い物カゴの機能を実装する軽量なサンプル ストアの実装です。</span><span class="sxs-lookup"><span data-stu-id="a251b-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="a251b-108">このチュートリアルの系列では、すべての ASP.NET MVC Music Store サンプル アプリケーションをビルドする手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="a251b-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="a251b-109">第 1 部は、概要ファイル-&gt;新しいプロジェクト。</span><span class="sxs-lookup"><span data-stu-id="a251b-109">Part 1 covers Overview and File-&gt;New Project.</span></span>


## <a name="overview"></a><span data-ttu-id="a251b-110">概要</span><span class="sxs-lookup"><span data-stu-id="a251b-110">Overview</span></span>

<span data-ttu-id="a251b-111">MVC Music Store は、チュートリアル アプリケーションを紹介し、ASP.NET MVC と Visual Web Developer web 開発を使用する方法の詳細な手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="a251b-111">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Web Developer for web development.</span></span> <span data-ttu-id="a251b-112">お緩やかに変化を開始することを初級レベルの web 開発が発生するように問題ありません。</span><span class="sxs-lookup"><span data-stu-id="a251b-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="a251b-113">構築するアプリケーションは、単純な音楽ストアです。</span><span class="sxs-lookup"><span data-stu-id="a251b-113">The application we'll be building is a simple music store.</span></span> <span data-ttu-id="a251b-114">アプリケーションに 3 つの主要な部分がある: ショッピング、チェック アウト、および管理します。</span><span class="sxs-lookup"><span data-stu-id="a251b-114">There are three main parts to the application: shopping, checkout, and administration.</span></span>

![](mvc-music-store-part-1/_static/image1.jpg)

<span data-ttu-id="a251b-115">閲覧者は、ジャンルのアルバムを参照できます。</span><span class="sxs-lookup"><span data-stu-id="a251b-115">Visitors can browse Albums by Genre:</span></span>

![](mvc-music-store-part-1/_static/image2.jpg)

<span data-ttu-id="a251b-116">1 枚のアルバムを表示し、カートに追加することができます。</span><span class="sxs-lookup"><span data-stu-id="a251b-116">They can view a single album and add it to their cart:</span></span>

![](mvc-music-store-part-1/_static/image3.jpg)

<span data-ttu-id="a251b-117">必要がなくなったすべての項目を削除する、カートを確認することができます。</span><span class="sxs-lookup"><span data-stu-id="a251b-117">They can review their cart, removing any items they no longer want:</span></span>

![](mvc-music-store-part-1/_static/image4.jpg)

<span data-ttu-id="a251b-118">チェック アウトに進むには、ログインまたはユーザー アカウントの登録にそれらを求められます。</span><span class="sxs-lookup"><span data-stu-id="a251b-118">Proceeding to Checkout will prompt them to login or register for a user account.</span></span>

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

<span data-ttu-id="a251b-119">アカウントを作成すると、配布と支払い情報を入力して、注文を完了することができます。</span><span class="sxs-lookup"><span data-stu-id="a251b-119">After creating an account, they can complete the order by filling out shipping and payment information.</span></span> <span data-ttu-id="a251b-120">煩雑にならないように、驚くほどの昇格を実行している: すべての設定が「無料」のプロモーション コードを入力している場合は無料です。</span><span class="sxs-lookup"><span data-stu-id="a251b-120">To keep things simple, we're running an amazing promotion: everything's free if they enter promotion code "FREE"!</span></span>

![](mvc-music-store-part-1/_static/image5.jpg)

<span data-ttu-id="a251b-121">、順序付けした後に、単純な確認画面が参照してください。</span><span class="sxs-lookup"><span data-stu-id="a251b-121">After ordering, they see a simple confirmation screen:</span></span>

![](mvc-music-store-part-1/_static/image6.jpg)

<span data-ttu-id="a251b-122">顧客 faceing ページだけでなくおをもアルバムの元の管理者を作成、編集の一覧を表示する管理者」セクションをビルドし、アルバムを削除します。</span><span class="sxs-lookup"><span data-stu-id="a251b-122">In addition to customer-faceing pages, we'll also build an administrator section that shows a list of albums from which Administrators can Create, Edit, and Delete albums:</span></span>

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a><span data-ttu-id="a251b-123">1.ファイル -&gt;新しいプロジェクト</span><span class="sxs-lookup"><span data-stu-id="a251b-123">1. File -&gt; New Project</span></span>

### <a name="installing-the-software"></a><span data-ttu-id="a251b-124">ソフトウェアのインストール</span><span class="sxs-lookup"><span data-stu-id="a251b-124">Installing the software</span></span>

<span data-ttu-id="a251b-125">このチュートリアルは、空き Visual Web Developer 2010 Express (これは無料) を使用して新しい ASP.NET MVC 3 プロジェクトを作成することで開始され、おを増分追加機能を完全に機能しているアプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="a251b-125">This tutorial will begin by creating a new ASP.NET MVC 3 project using the free Visual Web Developer 2010 Express (which is free), and then we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="a251b-126">その過程では、ここデータベースへのアクセス、フォーム ポスト シナリオ、データの検証、ページ更新し、検証、ユーザーのログイン、および AJAX を使用して一貫したページ レイアウトのマスター ページを使用します。</span><span class="sxs-lookup"><span data-stu-id="a251b-126">Along the way, we'll cover database access, form posting scenarios, data validation, using master pages for consistent page layout, using AJAX for page updates and validation, user login, and more.</span></span>

<span data-ttu-id="a251b-127">順を追っていくことができますをまたはから完成したアプリケーションをダウンロードする[MVC Music Store](https://github.com/evilDave/MVC-Music-Store)です。</span><span class="sxs-lookup"><span data-stu-id="a251b-127">You can follow along step by step, or you can download the completed application from [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="a251b-128">Visual Studio 2010 SP1 または Visual Web Developer 2010 Express SP1 (無料版の Visual Studio 2010) のいずれかを使用するには、アプリケーションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="a251b-128">You can use either Visual Studio 2010 SP1 or Visual Web Developer 2010 Express SP1 (a free version of Visual Studio 2010) to build the application.</span></span> <span data-ttu-id="a251b-129">使用する、SQL Server Compact (も無料) データベースをホストします。</span><span class="sxs-lookup"><span data-stu-id="a251b-129">We'll be using the SQL Server Compact (also free) to host the database.</span></span> <span data-ttu-id="a251b-130">開始する前に、以下に示す前提条件がインストールされていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="a251b-130">Before you start, make sure you've installed the prerequisites listed below.</span></span>


- <span data-ttu-id="a251b-131">[Visual Studio Web Developer Express SP1 の前提条件]</span><span class="sxs-lookup"><span data-stu-id="a251b-131">[Visual Studio Web Developer Express SP1 prerequisites]</span></span>
- <span data-ttu-id="a251b-132">[ASP.NET MVC 3 Tools Update]</span><span class="sxs-lookup"><span data-stu-id="a251b-132">[ASP.NET MVC 3 Tools Update]</span></span>
- <span data-ttu-id="a251b-133">[SQL Server Compact 4.0] - ランタイムとツールの両方のサポートを含む</span><span class="sxs-lookup"><span data-stu-id="a251b-133">[SQL Server Compact 4.0] - including both runtime and tools support</span></span>


### <a name="creating-a-new-aspnet-mvc-3-project"></a><span data-ttu-id="a251b-134">新しい ASP.NET MVC 3 プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="a251b-134">Creating a new ASP.NET MVC 3 project</span></span>

<span data-ttu-id="a251b-135">まず、Visual Web Developer で [ファイル] メニューから新しいプロジェクトの""を選択します。</span><span class="sxs-lookup"><span data-stu-id="a251b-135">We'll start by selecting "New Project" from the File menu in Visual Web Developer.</span></span> <span data-ttu-id="a251b-136">新しいプロジェクト ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a251b-136">This brings up the New Project dialog.</span></span>

![](mvc-music-store-part-1/_static/image5.png)

<span data-ttu-id="a251b-137">ここを選択すると、Visual c# で&gt;Web テンプレートは、左側のグループ化し、中央の列で、「ASP.NET MVC 3 Web アプリケーション」テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="a251b-137">We'll select the Visual C# -&gt; Web Templates group on the left, then choose the "ASP.NET MVC 3 Web Application" template in the center column.</span></span> <span data-ttu-id="a251b-138">MvcMusicStore、プロジェクトの名前を [ok] ボタンを押します。</span><span class="sxs-lookup"><span data-stu-id="a251b-138">Name your project MvcMusicStore and press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image8.jpg)

<span data-ttu-id="a251b-139">これにより、特定の MVC プロジェクトの設定を作成することが可能セカンダリ ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a251b-139">This will display a secondary dialog which allows us to make some MVC specific settings for our project.</span></span> <span data-ttu-id="a251b-140">次を選択します。</span><span class="sxs-lookup"><span data-stu-id="a251b-140">Select the following:</span></span>

<span data-ttu-id="a251b-141">プロジェクト テンプレート-空の選択</span><span class="sxs-lookup"><span data-stu-id="a251b-141">Project Template - select Empty</span></span>

<span data-ttu-id="a251b-142">エンジンの表示 - Razor を選択</span><span class="sxs-lookup"><span data-stu-id="a251b-142">View Engine - select Razor</span></span>

<span data-ttu-id="a251b-143">HTML5 セマンティック マークアップのチェックを使用します。</span><span class="sxs-lookup"><span data-stu-id="a251b-143">Use HTML5 semantic markup - checked</span></span>

<span data-ttu-id="a251b-144">設定が、次のようには、し、[ok] ボタンを押すことを確認します。</span><span class="sxs-lookup"><span data-stu-id="a251b-144">Verify that your settings are as shown below, then press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image9.jpg)

<span data-ttu-id="a251b-145">これにより、プロジェクトが作成されます。</span><span class="sxs-lookup"><span data-stu-id="a251b-145">This will create our project.</span></span> <span data-ttu-id="a251b-146">ソリューション エクスプ ローラーの右側にあるアプリケーションに追加されているフォルダーを見てをみましょう。</span><span class="sxs-lookup"><span data-stu-id="a251b-146">Let's take a look at the folders that have been added to our application in the Solution Explorer on the right side.</span></span>

![](mvc-music-store-part-1/_static/image10.jpg)

<span data-ttu-id="a251b-147">空の MVC 3 のテンプレートが完全に空でない – 基本的なフォルダー構造を追加します。</span><span class="sxs-lookup"><span data-stu-id="a251b-147">The Empty MVC 3 template isn't completely empty – it adds a basic folder structure:</span></span>

![](mvc-music-store-part-1/_static/image6.png)

<span data-ttu-id="a251b-148">ASP.NET MVC フォルダー名をいくつかの基本的な名前付け規則を利用します。</span><span class="sxs-lookup"><span data-stu-id="a251b-148">ASP.NET MVC makes use of some basic naming conventions for folder names:</span></span>

| <span data-ttu-id="a251b-149">**フォルダー**</span><span class="sxs-lookup"><span data-stu-id="a251b-149">**Folder**</span></span> | <span data-ttu-id="a251b-150">**目的**</span><span class="sxs-lookup"><span data-stu-id="a251b-150">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="a251b-151">**/コント ローラー**</span><span class="sxs-lookup"><span data-stu-id="a251b-151">**/Controllers**</span></span> | <span data-ttu-id="a251b-152">コント ローラーは、ブラウザーからの入力を操作を実行、およびユーザーへの応答を返すを決めてに応答します。</span><span class="sxs-lookup"><span data-stu-id="a251b-152">Controllers respond to input from the browser, decide what to do with it, and return response to the user.</span></span> |
| <span data-ttu-id="a251b-153">**/ビュー**</span><span class="sxs-lookup"><span data-stu-id="a251b-153">**/Views**</span></span> | <span data-ttu-id="a251b-154">ビューは、UI のテンプレートを保持します。</span><span class="sxs-lookup"><span data-stu-id="a251b-154">Views hold our UI templates</span></span> |
| <span data-ttu-id="a251b-155">**/モデル**</span><span class="sxs-lookup"><span data-stu-id="a251b-155">**/Models**</span></span> | <span data-ttu-id="a251b-156">モデルを保持およびデータの操作</span><span class="sxs-lookup"><span data-stu-id="a251b-156">Models hold and manipulate data</span></span> |
| <span data-ttu-id="a251b-157">**/コンテンツ**</span><span class="sxs-lookup"><span data-stu-id="a251b-157">**/Content**</span></span> | <span data-ttu-id="a251b-158">このフォルダーは、イメージ、CSS、およびその他の静的なコンテンツを保持します。</span><span class="sxs-lookup"><span data-stu-id="a251b-158">This folder holds our images, CSS, and any other static content</span></span> |
| <span data-ttu-id="a251b-159">**/スクリプト**</span><span class="sxs-lookup"><span data-stu-id="a251b-159">**/Scripts**</span></span> | <span data-ttu-id="a251b-160">このフォルダーの JavaScript ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="a251b-160">This folder holds our JavaScript files</span></span> |

<span data-ttu-id="a251b-161">既定では、ASP.NET MVC フレームワークは、「設定よりも規約」アプローチが使用され、いくつかのフォルダーの名前付け規則に基づく既定の前提条件ために、これらのフォルダーは空の ASP.NET MVC アプリケーションであっても含まれています。</span><span class="sxs-lookup"><span data-stu-id="a251b-161">These folders are included even in an Empty ASP.NET MVC application because the ASP.NET MVC framework by default uses a "convention over configuration" approach and makes some default assumptions based on folder naming conventions.</span></span> <span data-ttu-id="a251b-162">たとえば、コント ローラーを検索ビュー フォルダー内のビュー既定では、コードでこれを明示的に指定する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="a251b-162">For instance, controllers look for views in the Views folder by default without you having to explicitly specify this in your code.</span></span> <span data-ttu-id="a251b-163">書き込むには、必要なコードの量を削減する既定の規則に移れなくなることができますも容易にできるように、プロジェクトを理解するには、他の開発者とします。</span><span class="sxs-lookup"><span data-stu-id="a251b-163">Sticking with the default conventions reduces the amount of code you need to write, and can also make it easier for other developers to understand your project.</span></span> <span data-ttu-id="a251b-164">これらのアプリケーションを構築するには、複数の規則を用いて説明します。</span><span class="sxs-lookup"><span data-stu-id="a251b-164">We'll explain these conventions more as we build our application.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="a251b-165">次へ</span><span class="sxs-lookup"><span data-stu-id="a251b-165">Next</span></span>](mvc-music-store-part-2.md)
