---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: ページ (Razor) サイトを ASP.NET web ヘルパーをインストールする |Microsoft Docs
author: tfitzmac
description: この記事では、ASP.NET Web Pages (Razor) の web サイトでヘルパーをインストールする方法について説明します。 コードとごとにマークアップを含む再利用可能なコンポーネントをヘルパーには.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2014
ms.topic: article
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 38290fd47355e7893eddd1f867f47b113b54ca7e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37361807"
---
<a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="798e5-104">ASP.NET Web ページ (Razor) サイトでヘルパーをインストールします。</span><span class="sxs-lookup"><span data-stu-id="798e5-104">Installing a Helper in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="798e5-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="798e5-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="798e5-106">この記事では、ASP.NET Web Pages (Razor) の web サイトでヘルパーをインストールする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="798e5-106">This article describes how to install a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="798e5-107">A*ヘルパー*はコードと面倒か複雑になる可能性があるタスクを実行するためのマークアップを含む再利用可能なコンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="798e5-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="798e5-108">学習内容。</span><span class="sxs-lookup"><span data-stu-id="798e5-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="798e5-109">WebMatrix 3 を使用して作成された web サイトでヘルパーをインストールする方法。</span><span class="sxs-lookup"><span data-stu-id="798e5-109">How to install a helper in a website created using WebMatrix 3.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="798e5-110">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="798e5-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="798e5-111">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="798e5-111">WebMatrix 3</span></span>


## <a name="overview-of-helpers"></a><span data-ttu-id="798e5-112">ヘルパーの概要</span><span class="sxs-lookup"><span data-stu-id="798e5-112">Overview of Helpers</span></span>

<span data-ttu-id="798e5-113">多くの場合、ユーザーは web ページで実行するいくつかのタスクは、多くのコードが必要または余分な知識が必要です。</span><span class="sxs-lookup"><span data-stu-id="798e5-113">Some tasks that people often want to do on web pages require a lot of code or require extra knowledge.</span></span> <span data-ttu-id="798e5-114">例としては、データ用のグラフを表示します。ページで、Twitter の「フォロー」ボタンを配置します。web サイトからメールを送信トリミングまたはのイメージをサイズ変更サイトの PayPal を使用します。</span><span class="sxs-lookup"><span data-stu-id="798e5-114">Examples include displaying a chart for data; putting a Twitter "Follow" button on a page; sending email from your website; cropping or resizing images; using PayPal for your site.</span></span> <span data-ttu-id="798e5-115">簡単にこれらの種類の項目を実行する ASP.NET Web Pages を使用できます*ヘルパー*します。</span><span class="sxs-lookup"><span data-stu-id="798e5-115">To make it easy to do these kinds of things, ASP.NET Web Pages lets you use *helpers*.</span></span> <span data-ttu-id="798e5-116">ヘルパーは、コンポーネントをインストールすること、サイトとできるようにする、Razor コードの 2 行だけを使用して一般的なタスクを実行します。</span><span class="sxs-lookup"><span data-stu-id="798e5-116">Helpers are components that you install for a site and that let you perform typical tasks by using just a line or two of Razor code.</span></span>

<span data-ttu-id="798e5-117">ASP.NET Web ページが、いくつかのヘルパーが組み込まれています。</span><span class="sxs-lookup"><span data-stu-id="798e5-117">ASP.NET Web Pages has a few helpers built in.</span></span> <span data-ttu-id="798e5-118">ただし、多くのヘルパーは、NuGet パッケージ マネージャーを使用して提供されているパッケージ (アドインの場合) で利用できます。</span><span class="sxs-lookup"><span data-stu-id="798e5-118">However, many helpers are available in packages (add-ins) that are provided using the NuGet package manager.</span></span> <span data-ttu-id="798e5-119">インストールの詳細をすべて処理しと NuGet を使用してをインストールするパッケージを選択できます。</span><span class="sxs-lookup"><span data-stu-id="798e5-119">NuGet lets you select a package to install and then it takes care of all the details of the installation.</span></span>

## <a name="installing-a-helper-in-webmatrix-3"></a><span data-ttu-id="798e5-120">WebMatrix 3 でヘルパーをインストールします。</span><span class="sxs-lookup"><span data-stu-id="798e5-120">Installing a Helper in WebMatrix 3</span></span>

1. <span data-ttu-id="798e5-121">WebMatrix 3 をクリックして、 **NuGet**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="798e5-121">In WebMatrix 3, click the **NuGet** button.</span></span>

    ![WebMatrix での NuGet ギャラリー ダイアログ ボックス](installing-helpers/_static/image1.png)
2. <span data-ttu-id="798e5-123">これにより、NuGet パッケージ マネージャーを起動し、利用可能なパッケージを表示します。</span><span class="sxs-lookup"><span data-stu-id="798e5-123">This launches the NuGet package manager and displays available packages.</span></span> <span data-ttu-id="798e5-124">検索ボックスには、インストールするヘルパーのキーワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="798e5-124">In the search box, enter a keyword for the helper you want to install.</span></span>

    ![WebMatrix での NuGet ギャラリー ダイアログ ボックス](installing-helpers/_static/image2.png)
3. <span data-ttu-id="798e5-126">パッケージを選択し、をクリックし、**インストール**します。</span><span class="sxs-lookup"><span data-stu-id="798e5-126">Select the package and then click **Install**.</span></span> <span data-ttu-id="798e5-127">クリックして**はい**されたら、パッケージをインストールし、条項に同意することを指定するかどうか。</span><span class="sxs-lookup"><span data-stu-id="798e5-127">Click **Yes** when asked if you want to install the package and indicate that you accept the terms.</span></span>

     <span data-ttu-id="798e5-128">初めてのヘルパーをインストールした場合は、NuGet では、ヘルパーを構成するコードの web サイトのフォルダーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="798e5-128">If this is the first time you've installed a helper, NuGet creates folders in your website for the code that makes up the helper.</span></span>
4. <span data-ttu-id="798e5-129">ヘルパーをアンインストールするには、クリックして、**ギャラリー**ボタンをクリックし、**インストール済み**タブをクリックしをアンインストールするパッケージを選択します。</span><span class="sxs-lookup"><span data-stu-id="798e5-129">To uninstall a helper, click the **Gallery** button, click the **Installed** tab, and pick the package you want to uninstall.</span></span>

## <a name="installing-the-twitter-helper"></a><span data-ttu-id="798e5-130">Twitter ヘルパーをインストールします。</span><span class="sxs-lookup"><span data-stu-id="798e5-130">Installing the Twitter helper</span></span>

<span data-ttu-id="798e5-131">Twitter API の最新バージョンは、NuGet からインストールする Twitter ヘルパーと互換性がありません。</span><span class="sxs-lookup"><span data-stu-id="798e5-131">The latest version of the Twitter API is not compatible with the Twitter helper you install through NuGet.</span></span> <span data-ttu-id="798e5-132">代わりを参照してください、 [WebMatrix を使用した Twitter Helper](twitter-helper.md)プロジェクトでの Twitter ヘルパーを設定する方法の詳細については、トピック。</span><span class="sxs-lookup"><span data-stu-id="798e5-132">Instead, see the [Twitter Helper with WebMatrix](twitter-helper.md) topic for information about how to set up the Twitter helper in your project.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="798e5-133">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="798e5-133">Additional Resources</span></span>


[<span data-ttu-id="798e5-134">概要の ASP.NET Web ページ 2 のプログラミングの基礎</span><span class="sxs-lookup"><span data-stu-id="798e5-134">Introducing ASP.NET Web Pages 2 - Programming Basics</span></span>](../getting-started/introducing-razor-syntax-c.md)

[<span data-ttu-id="798e5-135">WebMatrix を使用した twitter ヘルパー</span><span class="sxs-lookup"><span data-stu-id="798e5-135">Twitter Helper with WebMatrix</span></span>](twitter-helper.md)
