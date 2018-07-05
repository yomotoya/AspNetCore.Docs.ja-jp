---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: NerdDinner チュートリアルの紹介 |Microsoft Docs
author: shanselman
description: 新しいフレームワークを学習する最善の方法では、何らかの処理を構築します。 このチュートリアル ASP.NE を使用して、サイズは小さいが完了すると、アプリケーションを構築する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 735a775752beda3fc24852422b1ff9d60a7c46b5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371629"
---
<a name="introducing-the-nerddinner-tutorial"></a><span data-ttu-id="f949c-104">NerdDinner チュートリアルの紹介</span><span class="sxs-lookup"><span data-stu-id="f949c-104">Introducing the NerdDinner Tutorial</span></span>
====================
<span data-ttu-id="f949c-105">によって[Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="f949c-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

[<span data-ttu-id="f949c-106">PDF のダウンロード</span><span class="sxs-lookup"><span data-stu-id="f949c-106">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="f949c-107">新しいフレームワークを学習する最善の方法では、何らかの処理を構築します。</span><span class="sxs-lookup"><span data-stu-id="f949c-107">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="f949c-108">このチュートリアルでは、小さなをビルドしても、ASP.NET MVC 1 を使用してアプリケーションを実行する方法について説明し、その背後にある主要な概念の一部が導入されています。</span><span class="sxs-lookup"><span data-stu-id="f949c-108">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC 1, and introduces some of the core concepts behind it.</span></span>
> 
> <span data-ttu-id="f949c-109">次のことをお勧め ASP.NET MVC 3 を使用している場合、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアル。</span><span class="sxs-lookup"><span data-stu-id="f949c-109">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-tutorial"></a><span data-ttu-id="f949c-110">NerdDinner チュートリアル</span><span class="sxs-lookup"><span data-stu-id="f949c-110">NerdDinner Tutorial</span></span>

<span data-ttu-id="f949c-111">新しいフレームワークを学習する最善の方法では、何らかの処理を構築します。</span><span class="sxs-lookup"><span data-stu-id="f949c-111">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="f949c-112">このチュートリアルでは、小さなをビルドしても、ASP.NET MVC を使用してアプリケーションを実行する方法について説明し、その背後にある主要な概念の一部が導入されています。</span><span class="sxs-lookup"><span data-stu-id="f949c-112">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC, and introduces some of the core concepts behind it.</span></span>

<span data-ttu-id="f949c-113">アプリケーションをビルドするには、"NerdDinner"が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="f949c-113">The application we are going to build is called "NerdDinner".</span></span> <span data-ttu-id="f949c-114">NerdDinner では、ユーザーの検索し、整理の dinners をオンラインに簡単な方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="f949c-114">NerdDinner provides an easy way for people to find and organize dinners online:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image1.png)

<span data-ttu-id="f949c-115">NerdDinner では、登録済みユーザーを作成、編集、および dinners を削除できるようにします。</span><span class="sxs-lookup"><span data-stu-id="f949c-115">NerdDinner enables registered users to create, edit and delete dinners.</span></span> <span data-ttu-id="f949c-116">アプリケーション全体で一貫性のある一連の検証とビジネス ルールを適用します。</span><span class="sxs-lookup"><span data-stu-id="f949c-116">It enforces a consistent set of validation and business rules across the application:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image2.png)

<span data-ttu-id="f949c-117">訪問者は、AJAX ベースのマップを使用して、近くに保持されている次回の dinners を検索できます。</span><span class="sxs-lookup"><span data-stu-id="f949c-117">Visitors can use an AJAX-based map to search for upcoming dinners being held near them:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image3.png)

<span data-ttu-id="f949c-118">クリックすると、dinner は持ち運ぶ詳細ページに学ぶことができる詳細については。</span><span class="sxs-lookup"><span data-stu-id="f949c-118">Clicking a dinner will take them to a details page where they can learn more about it:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image4.png)

<span data-ttu-id="f949c-119">Dinner への参加に興味を持つ場合は、ログインまたはサイトに登録できます。</span><span class="sxs-lookup"><span data-stu-id="f949c-119">If they are interested in attending the dinner they can login or register on the site:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image5.png)

<span data-ttu-id="f949c-120">イベントに出席する RSVP の AJAX ベースのリンクをクリックしています。</span><span class="sxs-lookup"><span data-stu-id="f949c-120">They can then click an AJAX-based RSVP link to attend the event:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a><span data-ttu-id="f949c-121">NerdDinner を実装します。</span><span class="sxs-lookup"><span data-stu-id="f949c-121">Implementing NerdDinner</span></span>

<span data-ttu-id="f949c-122">ファイル-を使用して、NerdDinner アプリケーションを開始しようとして&gt;Visual Studio 内で新しい ASP.NET MVC プロジェクトを作成する新しいプロジェクト コマンド。</span><span class="sxs-lookup"><span data-stu-id="f949c-122">We are going to begin our NerdDinner application by using the File-&gt;New Project command within Visual Studio to create a brand new ASP.NET MVC project.</span></span> <span data-ttu-id="f949c-123">私たちの機能と機能増分追加します。</span><span class="sxs-lookup"><span data-stu-id="f949c-123">We will then incrementally add functionality and features.</span></span> <span data-ttu-id="f949c-124">その過程について説明します。</span><span class="sxs-lookup"><span data-stu-id="f949c-124">Along the way we'll cover:</span></span>

1. [<span data-ttu-id="f949c-125">新しい ASP.NET MVC プロジェクトを作成する方法</span><span class="sxs-lookup"><span data-stu-id="f949c-125">How to create a new ASP.NET MVC Project</span></span>](# "新しい ASP.NET MVC プロジェクトの作成")
2. [<span data-ttu-id="f949c-126">データベースを作成する方法</span><span class="sxs-lookup"><span data-stu-id="f949c-126">How to create a database</span></span>](# "データベースを作成します。")
3. [<span data-ttu-id="f949c-127">ビジネス ルール検証でモデルを構築する方法</span><span class="sxs-lookup"><span data-stu-id="f949c-127">How to build a model with business rule validations</span></span>](# "ビジネス ルール検証とモデルの構築")
4. [<span data-ttu-id="f949c-128">コント ローラーとビューを使用して、リスティング/詳細 UI を実装する方法</span><span class="sxs-lookup"><span data-stu-id="f949c-128">How to use controllers and views to implement a listing/details UI</span></span>](# "リスティング/詳細 UI を実装するを使用して、コント ローラーとビュー")
5. <span data-ttu-id="f949c-129">[CRUD を提供する方法 (作成、読み取り、更新、削除) データ フォーム エントリ サポート](# "提供の CRUD (作成、読み取り、更新、削除) データ フォーム エントリ サポート")</span><span class="sxs-lookup"><span data-stu-id="f949c-129">[How to provide CRUD (create, read, update, delete) data form entry support](# "Provide CRUD (Create, Read, Update, Delete) Data Form Entry Support")</span></span>
6. [<span data-ttu-id="f949c-130">ViewData を使用し、ViewModel クラスを実装する方法</span><span class="sxs-lookup"><span data-stu-id="f949c-130">How to use ViewData and implement ViewModel classes</span></span>](# "ViewData を使用し、ViewModel クラスの実装")
7. [<span data-ttu-id="f949c-131">マスター ページおよびパーシャルを使用して UI を再利用する方法</span><span class="sxs-lookup"><span data-stu-id="f949c-131">How to re-use UI using master pages and partials</span></span>](# "UI を使用してマスター ページの再利用およびパーシャル")
8. [<span data-ttu-id="f949c-132">効率的なデータ ページングを実装する方法</span><span class="sxs-lookup"><span data-stu-id="f949c-132">How to implement efficient data paging</span></span>](# "実装効率的なデータ ページング")
9. [<span data-ttu-id="f949c-133">認証と承認を使用してアプリケーションをセキュリティで保護する方法</span><span class="sxs-lookup"><span data-stu-id="f949c-133">How to secure applications using authentication and authorization</span></span>](# "セキュリティで保護されたアプリケーションを使用して認証と承認")
10. [<span data-ttu-id="f949c-134">AJAX を使用して、動的な更新プログラムを配信する方法</span><span class="sxs-lookup"><span data-stu-id="f949c-134">How to use AJAX to deliver dynamic updates</span></span>](# "動的な更新プログラムを配信する AJAX を使用して、")
11. [<span data-ttu-id="f949c-135">AJAX を使用して、マッピングのシナリオを実装する方法</span><span class="sxs-lookup"><span data-stu-id="f949c-135">How to use AJAX to implement mapping scenarios</span></span>](# "マッピング シナリオの実装を使用して AJAX")
12. [<span data-ttu-id="f949c-136">自動化された単体テストを有効にする方法</span><span class="sxs-lookup"><span data-stu-id="f949c-136">How to enable automated unit testing</span></span>](# "自動単体テストを有効にします。")

<span data-ttu-id="f949c-137">NerdDinner の独自のコピーを構築する各を実行して最初から手順チュートリアルの「します。</span><span class="sxs-lookup"><span data-stu-id="f949c-137">You can build your own copy of NerdDinner from scratch by completing each step we walkthrough in this chapter.</span></span> <span data-ttu-id="f949c-138">または、ここで、ソース コードの完成版をダウンロードできます: [GitHub で NerdDinner](https://github.com/AspNetMVPSamples/NerdDinner)します。</span><span class="sxs-lookup"><span data-stu-id="f949c-138">Alternatively, you can download a completed version of the source code here: [NerdDinner on GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span></span> <span data-ttu-id="f949c-139">また、必要に応じて[このチュートリアルの無料 PDF 版をダウンロード](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)オフライン チュートリアルを読みたい場合。</span><span class="sxs-lookup"><span data-stu-id="f949c-139">You can also optionally also [download a free PDF version of this tutorial](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) if you want to read the tutorial offline.</span></span>

<span data-ttu-id="f949c-140">Visual Studio 2008 または、無料の Visual Web Developer 2008 Express のいずれかを使用するには、アプリケーションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="f949c-140">You can use either Visual Studio 2008 or the free Visual Web Developer 2008 Express to build the application.</span></span> <span data-ttu-id="f949c-141">データベースの SQL Server または無料の SQL Server Express のいずれかを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="f949c-141">You can use either SQL Server or the free SQL Server Express for the database.</span></span>

<span data-ttu-id="f949c-142">ASP.NET MVC、Visual Web Developer 2008 Express、および SQL Server Express (すべて無料) の V2 を使用してをインストールすることができます、 [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)</span><span class="sxs-lookup"><span data-stu-id="f949c-142">You can install ASP.NET MVC, Visual Web Developer 2008 Express, and SQL Server Express (all free) using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)</span></span>

### <a name="now-lets-get-started"></a><span data-ttu-id="f949c-143">今すぐ開始しましょう.</span><span class="sxs-lookup"><span data-stu-id="f949c-143">Now let's get started....</span></span>

<span data-ttu-id="f949c-144">NerdDinner が、ここで説明した、これで、とりかかりましょうをいくつかのコードを記述しましょう。</span><span class="sxs-lookup"><span data-stu-id="f949c-144">Now that we've covered what NerdDinner is, let's roll up our sleeves and write some code.</span></span>

<span data-ttu-id="f949c-145">まず、ファイルを使用して&gt;NerdDinner アプリケーションを作成する Visual Studio 内で新しいプロジェクト。</span><span class="sxs-lookup"><span data-stu-id="f949c-145">We'll begin by using File-&gt;New Project within Visual Studio to create the NerdDinner application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f949c-146">次へ</span><span class="sxs-lookup"><span data-stu-id="f949c-146">Next</span></span>](create-a-new-aspnet-mvc-project.md)
