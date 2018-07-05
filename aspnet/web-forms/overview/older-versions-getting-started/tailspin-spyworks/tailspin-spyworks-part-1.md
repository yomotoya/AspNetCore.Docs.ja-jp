---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 'パート 1: ファイルに新しいプロジェクト-> |Microsoft Docs'
author: JoeStagner
description: このチュートリアル シリーズでは、すべての Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 第 1 部では、概要とファイル]/[新しいプロジェクトについて説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: 8c346e88277b6cf43676f7c6b58f9679a591d634
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388212"
---
<a name="part-1-file--new-project"></a><span data-ttu-id="99ddc-104">パート 1: -> 新しいプロジェクトのファイル</span><span class="sxs-lookup"><span data-stu-id="99ddc-104">Part 1: File-> New Project</span></span>
====================
<span data-ttu-id="99ddc-105">によって[Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="99ddc-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="99ddc-106">Tailspin Spyworks では、.NET プラットフォーム用の強力でスケーラブルなアプリケーションを作成するはどの非常に単純なを示します。</span><span class="sxs-lookup"><span data-stu-id="99ddc-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="99ddc-107">ASP.NET 4 の優れた新機能を使用して、ショッピング、チェック アウト、および管理を含む、オンライン ストアを構築する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="99ddc-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="99ddc-108">このチュートリアル シリーズでは、すべての Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="99ddc-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="99ddc-109">第 1 部では、概要とファイル]/[新しいプロジェクトについて説明します。</span><span class="sxs-lookup"><span data-stu-id="99ddc-109">Part 1 covers Overview and File/New Project.</span></span>


## <a id="_Toc260221666"></a>  <span data-ttu-id="99ddc-110">概要</span><span class="sxs-lookup"><span data-stu-id="99ddc-110">Overview</span></span>

<span data-ttu-id="99ddc-111">このチュートリアルでは、ASP.NET WebForms の概要についてです。</span><span class="sxs-lookup"><span data-stu-id="99ddc-111">This tutorial is an introduction to ASP.NET WebForms.</span></span> <span data-ttu-id="99ddc-112">私たちはゆっくりと開始されます、初心者レベルの web 開発のエクスペリエンスは問題ありません。</span><span class="sxs-lookup"><span data-stu-id="99ddc-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="99ddc-113">ビルドするアプリケーションは、シンプルなオンライン ストアです。</span><span class="sxs-lookup"><span data-stu-id="99ddc-113">The application we'll be building is a simple on-line store.</span></span>

![](tailspin-spyworks-part-1/_static/image1.jpg)


<span data-ttu-id="99ddc-114">訪問者には、カテゴリ別の製品を参照できます。</span><span class="sxs-lookup"><span data-stu-id="99ddc-114">Visitors can browse Products by Category:</span></span>

![](tailspin-spyworks-part-1/_static/image2.jpg)

<span data-ttu-id="99ddc-115">1 つの製品を表示およびカートに追加できます。</span><span class="sxs-lookup"><span data-stu-id="99ddc-115">They can view a single product and add it to their cart:</span></span>

![](tailspin-spyworks-part-1/_static/image3.jpg)

<span data-ttu-id="99ddc-116">必要がなくなったすべての項目を削除する、カートを確認することができます。</span><span class="sxs-lookup"><span data-stu-id="99ddc-116">They can review their cart, removing any items they no longer want:</span></span>

![](tailspin-spyworks-part-1/_static/image4.jpg)

<span data-ttu-id="99ddc-117">チェック アウトに進むには、するように求められます</span><span class="sxs-lookup"><span data-stu-id="99ddc-117">Proceeding to Checkout will prompt them to</span></span>

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

<span data-ttu-id="99ddc-118">、順序付けの後に、単純な確認画面が参照してください。</span><span class="sxs-lookup"><span data-stu-id="99ddc-118">After ordering, they see a simple confirmation screen:</span></span>

![](tailspin-spyworks-part-1/_static/image7.jpg)


<span data-ttu-id="99ddc-119">まず、Visual Studio 2010 では、新しい ASP.NET WebForms プロジェクトを作成して、段階的に機能している完全なアプリケーションを作成する機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="99ddc-119">We'll begin by creating a new ASP.NET WebForms project in Visual Studio 2010, and we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="99ddc-120">その過程について説明しますデータベースへのアクセス、リストやグリッド ビュー、データ更新のページ、データの検証、マスター ページを使用して、一貫性のあるページ レイアウト、AJAX、検証、ユーザーのメンバーシップ。</span><span class="sxs-lookup"><span data-stu-id="99ddc-120">Along the way, we'll cover database access, list and grid views, data update pages, data validation, using master pages for consistent page layout, AJAX, validation, user membership, and more.</span></span>

<span data-ttu-id="99ddc-121">ステップ バイ ステップに従うことができます。 か、完成したアプリケーションをダウンロードすることができます。 [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span><span class="sxs-lookup"><span data-stu-id="99ddc-121">You can follow along step by step, or you can download the completed application from [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span></span>

<span data-ttu-id="99ddc-122">Visual Studio 2010 または、無料 Visual Web Developer 2010 からのいずれかを使用する[ https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/)します。</span><span class="sxs-lookup"><span data-stu-id="99ddc-122">You can use either Visual Studio 2010 or the free Visual Web Developer 2010 from [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="99ddc-123">アプリケーションをビルドするには、SQL Server または free SQL Server Express データベースをホストするのいずれかを使用できます。</span><span class="sxs-lookup"><span data-stu-id="99ddc-123">To build the application, you can use either SQL Server or the free SQL Server Express to host the database.</span></span>

## <a id="_Toc260221667"></a>  <span data-ttu-id="99ddc-124">ファイル/新しいプロジェクト</span><span class="sxs-lookup"><span data-stu-id="99ddc-124">File / New Project</span></span>

<span data-ttu-id="99ddc-125">まず、Visual Studio で [ファイル] メニューから新しいプロジェクトを選択します。</span><span class="sxs-lookup"><span data-stu-id="99ddc-125">We'll start by selecting the New Project from the File menu in Visual Studio.</span></span> <span data-ttu-id="99ddc-126">新しいプロジェクト ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="99ddc-126">This brings up the New Project dialog.</span></span>

![](tailspin-spyworks-part-1/_static/image8.jpg)

<span data-ttu-id="99ddc-127">選択、Visual c#]、[Web テンプレートは、左側のグループし、中央の列で、「ASP.NET Web アプリケーション」テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="99ddc-127">We'll select the Visual C# / Web Templates group on the left, and then choose the "ASP.NET Web Application" template in the center column.</span></span> <span data-ttu-id="99ddc-128">TailspinSpyworks をプロジェクトに名前を [ok] ボタンを押します。</span><span class="sxs-lookup"><span data-stu-id="99ddc-128">Name your project TailspinSpyworks and press the OK button.</span></span>

![](tailspin-spyworks-part-1/_static/image9.jpg)

<span data-ttu-id="99ddc-129">これにより、プロジェクトが作成されます。</span><span class="sxs-lookup"><span data-stu-id="99ddc-129">This will create our project.</span></span> <span data-ttu-id="99ddc-130">右側にある ソリューション エクスプ ローラーで、アプリケーションに含まれているフォルダーを見ていきましょう。</span><span class="sxs-lookup"><span data-stu-id="99ddc-130">Let's take a look at the folders that are included in our application in the Solution Explorer on the right side.</span></span>

![](tailspin-spyworks-part-1/_static/image10.jpg)

<span data-ttu-id="99ddc-131">空のソリューションが完全に空でない – 基本的なフォルダー構造を追加します。</span><span class="sxs-lookup"><span data-stu-id="99ddc-131">The Empty Solution isn't completely empty – it adds a basic folder structure:</span></span>

![](tailspin-spyworks-part-1/_static/image1.png)

<span data-ttu-id="99ddc-132">ASP.NET 4 の既定のプロジェクト テンプレートによって実装される規則に注意してください。</span><span class="sxs-lookup"><span data-stu-id="99ddc-132">Note the conventions implemented by the ASP.NET 4 default project template.</span></span>

- <span data-ttu-id="99ddc-133">「アカウント」フォルダーは、ASP の基本的なユーザー インターフェイスを実装します。NET のメンバーシップのサブシステムです。</span><span class="sxs-lookup"><span data-stu-id="99ddc-133">The "Account" folder implements a basic user interface for ASP.NET's membership subsystem.</span></span>
- <span data-ttu-id="99ddc-134">"Scripts"フォルダーがクライアント側の JavaScript ファイルのリポジトリとして機能し、コア jQuery の .js ファイルは、既定で利用可能な。</span><span class="sxs-lookup"><span data-stu-id="99ddc-134">The "Scripts" folder serves as the repository for client side JavaScript files and the core jQuery .js files are made available by default.</span></span>
- <span data-ttu-id="99ddc-135">[スタイル] フォルダーは、web サイトのビジュアル (CSS スタイル シート) を整理するために使用します。</span><span class="sxs-lookup"><span data-stu-id="99ddc-135">The "Styles" folder is used to organize our web site visuals (CSS Style Sheets)</span></span>

<span data-ttu-id="99ddc-136">F5 キーを押して、アプリケーションを実行し、default.aspx ページをレンダリングするキーを押すときに、次のわかります。</span><span class="sxs-lookup"><span data-stu-id="99ddc-136">When we press F5 to run our application and render the default.aspx page we see the following.</span></span>

![](tailspin-spyworks-part-1/_static/image11.jpg)

<span data-ttu-id="99ddc-137">この最初のアプリケーションの拡張機能は、CSS クラスと関連付けられているイメージ ファイル、Tailspin Spyworks アプリケーションに追加する visual asthetics をレンダリングする WebForms の既定のテンプレートから Style.css ファイルを置換するになります。</span><span class="sxs-lookup"><span data-stu-id="99ddc-137">Our first application enhancement will be to replace the Style.css file from the default WebForms template with the CSS classes and associated image files that will render the visual asthetics that we want for our Tailspin Spyworks application.</span></span>

<span data-ttu-id="99ddc-138">これを行った後、default.aspx ページは、次のように表示します。</span><span class="sxs-lookup"><span data-stu-id="99ddc-138">After doing so our default.aspx page renders like this.</span></span>

![](tailspin-spyworks-part-1/_static/image12.jpg)

<span data-ttu-id="99ddc-139">上部にあるイメージ リンクに注意してください、ページとマスター ページに追加されたメニュー項目の右。</span><span class="sxs-lookup"><span data-stu-id="99ddc-139">Notice the image links at the top right of the page and the menu items that have been added to the master page.</span></span> <span data-ttu-id="99ddc-140">「サインイン」と「アカウント」のリンクは、ページは、(既定のテンプレートによって生成された) に存在して、アプリケーションの構築を実装しますが、ページの残りの部分をポイントします。</span><span class="sxs-lookup"><span data-stu-id="99ddc-140">Only the "Sign In" and "Account" links point to pages that exist (generated by the default template) and the rest of the pages we will implement as we build our application.</span></span>

<span data-ttu-id="99ddc-141">マスター ページをスタイル ディレクトリに配置する場合にもなります。</span><span class="sxs-lookup"><span data-stu-id="99ddc-141">We're also going to relocate the Master Page to the Styles directory.</span></span> <span data-ttu-id="99ddc-142">これは、優先順位ことをおモ ノを少し簡単にアプリケーションを今後で"skinable"になった場合。</span><span class="sxs-lookup"><span data-stu-id="99ddc-142">Though this is only a preference it may make things a little easier if we decide to make our application "skinable" in the future.</span></span>

<span data-ttu-id="99ddc-143">マスター ページを変更する必要がありますこれを行った後は、すべての .aspx ファイル内の参照は、ASP.NET WebForms ページを既定で生成されます。</span><span class="sxs-lookup"><span data-stu-id="99ddc-143">After doing this we'll need to change the master page references in all the .aspx files generated by the default ASP.NET WebForms pages.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="99ddc-144">次へ</span><span class="sxs-lookup"><span data-stu-id="99ddc-144">Next</span></span>](tailspin-spyworks-part-2.md)
