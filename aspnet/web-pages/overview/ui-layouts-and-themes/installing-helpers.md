---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: "ページ (Razor) サイトの ASP.NET web ヘルパーをインストールする |Microsoft ドキュメント"
author: tfitzmac
description: "この記事では、ASP.NET Web Pages (Razor) の web サイトでヘルパーをインストールする方法について説明します。 コードおよびごとにマークアップを含む再使用可能なコンポーネントをヘルパーには."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2014
ms.topic: article
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 842c5a56d14314217c1e6ad6d48ded28d3cc5b4e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="9d1ad-104">ASP.NET Web Pages (Razor) サイトにヘルパーをインストールします。</span><span class="sxs-lookup"><span data-stu-id="9d1ad-104">Installing a Helper in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="9d1ad-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="9d1ad-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="9d1ad-106">この記事では、ASP.NET Web Pages (Razor) の web サイトでヘルパーをインストールする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="9d1ad-106">This article describes how to install a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="9d1ad-107">A*ヘルパー*コードと面倒か複雑になる可能性があるタスクを実行するマークアップを含む再使用可能なコンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="9d1ad-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="9d1ad-108">学習する内容。</span><span class="sxs-lookup"><span data-stu-id="9d1ad-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="9d1ad-109">WebMatrix 3 を使用して作成された web サイトでヘルパーをインストールする方法です。</span><span class="sxs-lookup"><span data-stu-id="9d1ad-109">How to install a helper in a website created using WebMatrix 3.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="9d1ad-110">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="9d1ad-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="9d1ad-111">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="9d1ad-111">WebMatrix 3</span></span>


## <a name="overview-of-helpers"></a><span data-ttu-id="9d1ad-112">ヘルパーの概要</span><span class="sxs-lookup"><span data-stu-id="9d1ad-112">Overview of Helpers</span></span>

<span data-ttu-id="9d1ad-113">多くの場合、ユーザーは web ページで実行するいくつかのタスクは、多くのコードを要求したり、余分な知識が必要があります。</span><span class="sxs-lookup"><span data-stu-id="9d1ad-113">Some tasks that people often want to do on web pages require a lot of code or require extra knowledge.</span></span> <span data-ttu-id="9d1ad-114">例としては、データのグラフが表示されます。ページで、Twitter [フォロー] ボタンを配置します。web サイト; からメールを送信トリミングまたはイメージのサイズを変更します。サイトの PayPal を使用します。</span><span class="sxs-lookup"><span data-stu-id="9d1ad-114">Examples include displaying a chart for data; putting a Twitter "Follow" button on a page; sending email from your website; cropping or resizing images; using PayPal for your site.</span></span> <span data-ttu-id="9d1ad-115">簡単にこれをこれらの処理を行うには、ASP.NET Web Pages を使用できます*ヘルパー*です。</span><span class="sxs-lookup"><span data-stu-id="9d1ad-115">To make it easy to do these kinds of things, ASP.NET Web Pages lets you use *helpers*.</span></span> <span data-ttu-id="9d1ad-116">ヘルパーは、コンポーネントをインストールすること、サイトとできるようにするが、2 の Razor コードの行だけを使用して一般的なタスクを実行します。</span><span class="sxs-lookup"><span data-stu-id="9d1ad-116">Helpers are components that you install for a site and that let you perform typical tasks by using just a line or two of Razor code.</span></span>

<span data-ttu-id="9d1ad-117">ASP.NET Web ページには、いくつかのヘルパーが組み込まれているがあります。</span><span class="sxs-lookup"><span data-stu-id="9d1ad-117">ASP.NET Web Pages has a few helpers built in.</span></span> <span data-ttu-id="9d1ad-118">ただし、多くヘルパーは、NuGet パッケージ マネージャーを使用して提供されているパッケージ (アドインの場合) で使用できます。</span><span class="sxs-lookup"><span data-stu-id="9d1ad-118">However, many helpers are available in packages (add-ins) that are provided using the NuGet package manager.</span></span> <span data-ttu-id="9d1ad-119">インストールの詳細をすべてを管理しと NuGet を使用してをインストールするパッケージを選択できます。</span><span class="sxs-lookup"><span data-stu-id="9d1ad-119">NuGet lets you select a package to install and then it takes care of all the details of the installation.</span></span>

## <a name="installing-a-helper-in-webmatrix-3"></a><span data-ttu-id="9d1ad-120">3 の webmatrix ヘルパーをインストールします。</span><span class="sxs-lookup"><span data-stu-id="9d1ad-120">Installing a Helper in WebMatrix 3</span></span>

1. <span data-ttu-id="9d1ad-121">WebMatrix 3 でをクリックして、 **NuGet**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="9d1ad-121">In WebMatrix 3, click the **NuGet** button.</span></span>

    ![WebMatrix で NuGet ギャラリー ダイアログ ボックス](installing-helpers/_static/image1.png)
2. <span data-ttu-id="9d1ad-123">これは、NuGet パッケージ マネージャーを起動し、利用可能なパッケージを表示します。</span><span class="sxs-lookup"><span data-stu-id="9d1ad-123">This launches the NuGet package manager and displays available packages.</span></span> <span data-ttu-id="9d1ad-124">[検索] ボックスをインストールするヘルパーのキーワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="9d1ad-124">In the search box, enter a keyword for the helper you want to install.</span></span>

    ![WebMatrix で NuGet ギャラリー ダイアログ ボックス](installing-helpers/_static/image2.png)
- <span data-ttu-id="9d1ad-126">パッケージを選択し、をクリックして**インストール**です。</span><span class="sxs-lookup"><span data-stu-id="9d1ad-126">Select the package and then click **Install**.</span></span> <span data-ttu-id="9d1ad-127">をクリックして**はい**されたら、パッケージをインストールし、条項に同意することを指定するかどうか。</span><span class="sxs-lookup"><span data-stu-id="9d1ad-127">Click **Yes** when asked if you want to install the package and indicate that you accept the terms.</span></span>

    <span data-ttu-id="9d1ad-128">初めてヘルパーをインストールした場合は、NuGet は、ヘルパーを構成するコードの web サイトにフォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="9d1ad-128">If this is the first time you've installed a helper, NuGet creates folders in your website for the code that makes up the helper.</span></span>
- <span data-ttu-id="9d1ad-129">ヘルパーをアンインストールするには、クリックして、**ギャラリー**ボタンをクリックし、**インストール**タブをクリックしをアンインストールするパッケージを選択します。</span><span class="sxs-lookup"><span data-stu-id="9d1ad-129">To uninstall a helper, click the **Gallery** button, click the **Installed** tab, and pick the package you want to uninstall.</span></span>

## <a name="installing-the-twitter-helper"></a><span data-ttu-id="9d1ad-130">Twitter helper をインストールします。</span><span class="sxs-lookup"><span data-stu-id="9d1ad-130">Installing the Twitter helper</span></span>

<span data-ttu-id="9d1ad-131">Twitter API の最新バージョンは、NuGet をインストールする Twitter ヘルパーと互換性がありません。</span><span class="sxs-lookup"><span data-stu-id="9d1ad-131">The latest version of the Twitter API is not compatible with the Twitter helper you install through NuGet.</span></span> <span data-ttu-id="9d1ad-132">代わりを参照してください、 [webmatrix Twitter Helper](twitter-helper.md)については、プロジェクトの Twitter helper をセットアップする方法に関するトピック。</span><span class="sxs-lookup"><span data-stu-id="9d1ad-132">Instead, see the [Twitter Helper with WebMatrix](twitter-helper.md) topic for information about how to set up the Twitter helper in your project.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="9d1ad-133">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="9d1ad-133">Additional Resources</span></span>


[<span data-ttu-id="9d1ad-134">紹介の ASP.NET Web Pages 2 のプログラミングの基礎</span><span class="sxs-lookup"><span data-stu-id="9d1ad-134">Introducing ASP.NET Web Pages 2 - Programming Basics</span></span>](../getting-started/introducing-razor-syntax-c.md)

[<span data-ttu-id="9d1ad-135">Twitter の webmatrix ヘルパー</span><span class="sxs-lookup"><span data-stu-id="9d1ad-135">Twitter Helper with WebMatrix</span></span>](twitter-helper.md)
