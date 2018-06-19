---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 'パート 1: ファイルに新しいプロジェクト-> |Microsoft ドキュメント'
author: JoeStagner
description: このチュートリアルの系列では、すべて Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 第 1 部では、概要とファイル]/[新しいプロジェクトについて説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: a1b9681516e626b6a0eec420b168a74e05d88afb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892466"
---
<a name="part-1-file--new-project"></a><span data-ttu-id="4727f-104">パート 1: ファイルを新しいプロジェクト</span><span class="sxs-lookup"><span data-stu-id="4727f-104">Part 1: File-> New Project</span></span>
====================
<span data-ttu-id="4727f-105">によって[行える](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="4727f-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="4727f-106">Tailspin Spyworks では、.NET プラットフォーム用の強力な拡張性の高いアプリケーションを作成するはどのように非常に単純なを示しています。</span><span class="sxs-lookup"><span data-stu-id="4727f-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="4727f-107">ASP.NET 4 の優れた新機能を使用して、ショッピング、チェック アウト、および管理を含む、オンライン ストアをビルドする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="4727f-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="4727f-108">このチュートリアルの系列では、すべて Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="4727f-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="4727f-109">第 1 部では、概要とファイル]/[新しいプロジェクトについて説明します。</span><span class="sxs-lookup"><span data-stu-id="4727f-109">Part 1 covers Overview and File/New Project.</span></span>


## <a id="_Toc260221666"></a>  <span data-ttu-id="4727f-110">概要</span><span class="sxs-lookup"><span data-stu-id="4727f-110">Overview</span></span>

<span data-ttu-id="4727f-111">このチュートリアルは、ASP.NET WebForms の概要です。</span><span class="sxs-lookup"><span data-stu-id="4727f-111">This tutorial is an introduction to ASP.NET WebForms.</span></span> <span data-ttu-id="4727f-112">お緩やかに変化を開始することを初級レベルの web 開発が発生するように問題ありません。</span><span class="sxs-lookup"><span data-stu-id="4727f-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="4727f-113">構築するアプリケーションは、単純なオンライン ストアです。</span><span class="sxs-lookup"><span data-stu-id="4727f-113">The application we'll be building is a simple on-line store.</span></span>

![](tailspin-spyworks-part-1/_static/image1.jpg)


<span data-ttu-id="4727f-114">閲覧者は、カテゴリによって製品を参照できます。</span><span class="sxs-lookup"><span data-stu-id="4727f-114">Visitors can browse Products by Category:</span></span>

![](tailspin-spyworks-part-1/_static/image2.jpg)

<span data-ttu-id="4727f-115">1 つの製品を表示し、カートに追加することができます。</span><span class="sxs-lookup"><span data-stu-id="4727f-115">They can view a single product and add it to their cart:</span></span>

![](tailspin-spyworks-part-1/_static/image3.jpg)

<span data-ttu-id="4727f-116">必要がなくなったすべての項目を削除する、カートを確認することができます。</span><span class="sxs-lookup"><span data-stu-id="4727f-116">They can review their cart, removing any items they no longer want:</span></span>

![](tailspin-spyworks-part-1/_static/image4.jpg)

<span data-ttu-id="4727f-117">チェック アウトを続行するプロンプトされます。</span><span class="sxs-lookup"><span data-stu-id="4727f-117">Proceeding to Checkout will prompt them to</span></span>

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

<span data-ttu-id="4727f-118">、順序付けした後に、単純な確認画面が参照してください。</span><span class="sxs-lookup"><span data-stu-id="4727f-118">After ordering, they see a simple confirmation screen:</span></span>

![](tailspin-spyworks-part-1/_static/image7.jpg)


<span data-ttu-id="4727f-119">まず、Visual Studio 2010 で ASP.NET WebForms の新しいプロジェクトを作成して、増分を完全に機能しているアプリケーションを作成する機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="4727f-119">We'll begin by creating a new ASP.NET WebForms project in Visual Studio 2010, and we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="4727f-120">その過程では、ここデータベースへのアクセス、リストやグリッド ビュー、データ更新のページ、データの検証、一貫したページ レイアウト、AJAX、検証、ユーザーのメンバーシップ、および複数のマスター ページを使用します。</span><span class="sxs-lookup"><span data-stu-id="4727f-120">Along the way, we'll cover database access, list and grid views, data update pages, data validation, using master pages for consistent page layout, AJAX, validation, user membership, and more.</span></span>

<span data-ttu-id="4727f-121">ステップ バイ ステップいくことができます。 をまたはから完成したアプリケーションをダウンロードすることができます。 [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span><span class="sxs-lookup"><span data-stu-id="4727f-121">You can follow along step by step, or you can download the completed application from [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span></span>

<span data-ttu-id="4727f-122">Visual Studio 2010 または、空き Visual Web Developer 2010 からのいずれかを使用することができます[ https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/)です。</span><span class="sxs-lookup"><span data-stu-id="4727f-122">You can use either Visual Studio 2010 or the free Visual Web Developer 2010 from [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="4727f-123">アプリケーションをビルドするには、SQL Server または空き SQL Server Express データベースをホストするのいずれかを使用できます。</span><span class="sxs-lookup"><span data-stu-id="4727f-123">To build the application, you can use either SQL Server or the free SQL Server Express to host the database.</span></span>

## <a id="_Toc260221667"></a>  <span data-ttu-id="4727f-124">ファイル/新しいプロジェクト</span><span class="sxs-lookup"><span data-stu-id="4727f-124">File / New Project</span></span>

<span data-ttu-id="4727f-125">まず、Visual Studio で、[ファイル] メニューから新しいプロジェクトを選択します。</span><span class="sxs-lookup"><span data-stu-id="4727f-125">We'll start by selecting the New Project from the File menu in Visual Studio.</span></span> <span data-ttu-id="4727f-126">新しいプロジェクト ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="4727f-126">This brings up the New Project dialog.</span></span>

![](tailspin-spyworks-part-1/_static/image8.jpg)

<span data-ttu-id="4727f-127">ここを選択すると、Visual c#/Web テンプレートは、左側のグループ化され、中央の列で、「ASP.NET Web アプリケーション」テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="4727f-127">We'll select the Visual C# / Web Templates group on the left, and then choose the "ASP.NET Web Application" template in the center column.</span></span> <span data-ttu-id="4727f-128">TailspinSpyworks、プロジェクトの名前を [ok] ボタンを押します。</span><span class="sxs-lookup"><span data-stu-id="4727f-128">Name your project TailspinSpyworks and press the OK button.</span></span>

![](tailspin-spyworks-part-1/_static/image9.jpg)

<span data-ttu-id="4727f-129">これにより、プロジェクトが作成されます。</span><span class="sxs-lookup"><span data-stu-id="4727f-129">This will create our project.</span></span> <span data-ttu-id="4727f-130">ソリューション エクスプ ローラーの右側にあるアプリケーションに含まれているフォルダーを見てをみましょう。</span><span class="sxs-lookup"><span data-stu-id="4727f-130">Let's take a look at the folders that are included in our application in the Solution Explorer on the right side.</span></span>

![](tailspin-spyworks-part-1/_static/image10.jpg)

<span data-ttu-id="4727f-131">空のソリューションが完全に空でない – 基本的なフォルダー構造を追加します。</span><span class="sxs-lookup"><span data-stu-id="4727f-131">The Empty Solution isn't completely empty – it adds a basic folder structure:</span></span>

![](tailspin-spyworks-part-1/_static/image1.png)

<span data-ttu-id="4727f-132">ASP.NET 4 の既定のプロジェクト テンプレートによって実装される規則に注意してください。</span><span class="sxs-lookup"><span data-stu-id="4727f-132">Note the conventions implemented by the ASP.NET 4 default project template.</span></span>

- <span data-ttu-id="4727f-133">"Account"フォルダーは、ASP の基本的なユーザー インターフェイスを実装します。NET のメンバーシップのサブシステムです。</span><span class="sxs-lookup"><span data-stu-id="4727f-133">The "Account" folder implements a basic user interface for ASP.NET's membership subsystem.</span></span>
- <span data-ttu-id="4727f-134">"Scripts"フォルダーがクライアント側の JavaScript ファイルのリポジトリとして機能し、コア jQuery の .js ファイルは、既定で利用可能な。</span><span class="sxs-lookup"><span data-stu-id="4727f-134">The "Scripts" folder serves as the repository for client side JavaScript files and the core jQuery .js files are made available by default.</span></span>
- <span data-ttu-id="4727f-135">「スタイル」フォルダーは、web サイトの視覚化 (CSS スタイル シート) を整理するために使用します。</span><span class="sxs-lookup"><span data-stu-id="4727f-135">The "Styles" folder is used to organize our web site visuals (CSS Style Sheets)</span></span>

<span data-ttu-id="4727f-136">F5 キーを押して、アプリケーションを実行し、default.aspx ページを表示するキーを押すとお次をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="4727f-136">When we press F5 to run our application and render the default.aspx page we see the following.</span></span>

![](tailspin-spyworks-part-1/_static/image11.jpg)

<span data-ttu-id="4727f-137">WebForms の既定のテンプレートから Style.css ファイルを置き換える、CSS クラスと Tailspin Spyworks アプリケーション用の対象が visual asthetics を表示する関連付けられているイメージ ファイルに、最初のアプリケーションの機能強化がされます。</span><span class="sxs-lookup"><span data-stu-id="4727f-137">Our first application enhancement will be to replace the Style.css file from the default WebForms template with the CSS classes and associated image files that will render the visual asthetics that we want for our Tailspin Spyworks application.</span></span>

<span data-ttu-id="4727f-138">これを行った後、default.aspx ページは、次のように表示します。</span><span class="sxs-lookup"><span data-stu-id="4727f-138">After doing so our default.aspx page renders like this.</span></span>

![](tailspin-spyworks-part-1/_static/image12.jpg)

<span data-ttu-id="4727f-139">上部にあるイメージへのリンクに注意してください、ページおよびマスター ページに追加されているメニュー項目の右。</span><span class="sxs-lookup"><span data-stu-id="4727f-139">Notice the image links at the top right of the page and the menu items that have been added to the master page.</span></span> <span data-ttu-id="4727f-140">「サインイン」と「アカウント」のリンクは、(既定のテンプレートによって生成される) に存在するページとアプリケーションを構築するには実装のページの残りの部分をポイントします。</span><span class="sxs-lookup"><span data-stu-id="4727f-140">Only the "Sign In" and "Account" links point to pages that exist (generated by the default template) and the rest of the pages we will implement as we build our application.</span></span>

<span data-ttu-id="4727f-141">またしようとして、マスター ページをスタイル ディレクトリに移動します。</span><span class="sxs-lookup"><span data-stu-id="4727f-141">We're also going to relocate the Master Page to the Styles directory.</span></span> <span data-ttu-id="4727f-142">これは、優先順位ことをお点少し簡単に場合は、将来"skinable"にするには、アプリケーションにします。</span><span class="sxs-lookup"><span data-stu-id="4727f-142">Though this is only a preference it may make things a little easier if we decide to make our application "skinable" in the future.</span></span>

<span data-ttu-id="4727f-143">マスター ページを変更する必要がありますこれを行った後は、すべての .aspx ファイル内の参照は、ASP.NET WebForms ページを既定で生成されます。</span><span class="sxs-lookup"><span data-stu-id="4727f-143">After doing this we'll need to change the master page references in all the .aspx files generated by the default ASP.NET WebForms pages.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="4727f-144">次へ</span><span class="sxs-lookup"><span data-stu-id="4727f-144">Next</span></span>](tailspin-spyworks-part-2.md)
