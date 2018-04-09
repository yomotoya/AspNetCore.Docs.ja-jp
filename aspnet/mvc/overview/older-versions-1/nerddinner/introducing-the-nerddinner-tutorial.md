---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: NerdDinner チュートリアルの概要 |Microsoft ドキュメント
author: shanselman
description: 新しいフレームワークを学習する最善の方法では、何らかの処理をビルドします。 このチュートリアルで ASP.NE を使用するサイズは小さいが完了すると、アプリケーションを構築する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 3d925a7dc89fc0c742468653c5c138a0f1d71231
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="introducing-the-nerddinner-tutorial"></a><span data-ttu-id="e225f-104">NerdDinner チュートリアルの概要</span><span class="sxs-lookup"><span data-stu-id="e225f-104">Introducing the NerdDinner Tutorial</span></span>
====================
<span data-ttu-id="e225f-105">によって[Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="e225f-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

[<span data-ttu-id="e225f-106">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="e225f-106">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="e225f-107">新しいフレームワークを学習する最善の方法では、何らかの処理をビルドします。</span><span class="sxs-lookup"><span data-stu-id="e225f-107">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="e225f-108">このチュートリアルでは、小さなの構築が完了すると、ASP.NET MVC 1 を使用してアプリケーションにする方法を説明し、一部の背後にある主要な概念について説明します。</span><span class="sxs-lookup"><span data-stu-id="e225f-108">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC 1, and introduces some of the core concepts behind it.</span></span>
> 
> <span data-ttu-id="e225f-109">ASP.NET MVC 3 を使用している場合ことをお勧めする、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="e225f-109">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-tutorial"></a><span data-ttu-id="e225f-110">NerdDinner チュートリアル</span><span class="sxs-lookup"><span data-stu-id="e225f-110">NerdDinner Tutorial</span></span>

<span data-ttu-id="e225f-111">新しいフレームワークを学習する最善の方法では、何らかの処理をビルドします。</span><span class="sxs-lookup"><span data-stu-id="e225f-111">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="e225f-112">このチュートリアルでは、小さなの構築が完了すると、ASP.NET MVC を使用してアプリケーションにする方法を説明し、一部の背後にある主要な概念について説明します。</span><span class="sxs-lookup"><span data-stu-id="e225f-112">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC, and introduces some of the core concepts behind it.</span></span>

<span data-ttu-id="e225f-113">ビルドしようとして、アプリケーションは、"NerdDinner"と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="e225f-113">The application we are going to build is called "NerdDinner".</span></span> <span data-ttu-id="e225f-114">NerdDinner は、検索や整理ディナー オンライン方のための簡単な方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="e225f-114">NerdDinner provides an easy way for people to find and organize dinners online:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image1.png)

<span data-ttu-id="e225f-115">NerdDinner により、登録されたユーザーが作成、編集、およびディナーを削除します。</span><span class="sxs-lookup"><span data-stu-id="e225f-115">NerdDinner enables registered users to create, edit and delete dinners.</span></span> <span data-ttu-id="e225f-116">アプリケーション全体で一貫性のある一連の検証とビジネス ルールを適用します。</span><span class="sxs-lookup"><span data-stu-id="e225f-116">It enforces a consistent set of validation and business rules across the application:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image2.png)

<span data-ttu-id="e225f-117">訪問者は、AJAX ベースのマップを使用して、それらの近くに保持される今後ディナーを検索できます。</span><span class="sxs-lookup"><span data-stu-id="e225f-117">Visitors can use an AJAX-based map to search for upcoming dinners being held near them:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image3.png)

<span data-ttu-id="e225f-118">クリックすると、dinner は移動に詳細 ページに学べる場所は、詳細については。</span><span class="sxs-lookup"><span data-stu-id="e225f-118">Clicking a dinner will take them to a details page where they can learn more about it:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image4.png)

<span data-ttu-id="e225f-119">ログインするか、サイト上のレジスタ dinner への参加に関心のある: 場合</span><span class="sxs-lookup"><span data-stu-id="e225f-119">If they are interested in attending the dinner they can login or register on the site:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image5.png)

<span data-ttu-id="e225f-120">イベントへの参加を AJAX ベース RSVP リンクをクリックすることができます。</span><span class="sxs-lookup"><span data-stu-id="e225f-120">They can then click an AJAX-based RSVP link to attend the event:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a><span data-ttu-id="e225f-121">NerdDinner を実装します。</span><span class="sxs-lookup"><span data-stu-id="e225f-121">Implementing NerdDinner</span></span>

<span data-ttu-id="e225f-122">使用して、ファイルによって NerdDinner アプリケーションを開始しようとして&gt;新しい ASP.NET MVC プロジェクトを作成する Visual Studio 内で新しいプロジェクト コマンド。</span><span class="sxs-lookup"><span data-stu-id="e225f-122">We are going to begin our NerdDinner application by using the File-&gt;New Project command within Visual Studio to create a brand new ASP.NET MVC project.</span></span> <span data-ttu-id="e225f-123">お機能と特徴増分追加します。</span><span class="sxs-lookup"><span data-stu-id="e225f-123">We will then incrementally add functionality and features.</span></span> <span data-ttu-id="e225f-124">その過程でおについて説明します。</span><span class="sxs-lookup"><span data-stu-id="e225f-124">Along the way we'll cover:</span></span>

1. [<span data-ttu-id="e225f-125">新しい ASP.NET MVC プロジェクトを作成する方法</span><span class="sxs-lookup"><span data-stu-id="e225f-125">How to create a new ASP.NET MVC Project</span></span>](# "新しい ASP.NET MVC プロジェクトを作成します。")
2. [<span data-ttu-id="e225f-126">データベースを作成する方法</span><span class="sxs-lookup"><span data-stu-id="e225f-126">How to create a database</span></span>](# "データベースを作成します。")
3. [<span data-ttu-id="e225f-127">ビジネス ルールの検証を使用してモデルを構築する方法</span><span class="sxs-lookup"><span data-stu-id="e225f-127">How to build a model with business rule validations</span></span>](# "ビジネス ルールの検証とモデルの構築")
4. [<span data-ttu-id="e225f-128">コント ローラーとビューを使用して、一覧/詳細 UI を実装する方法</span><span class="sxs-lookup"><span data-stu-id="e225f-128">How to use controllers and views to implement a listing/details UI</span></span>](# "一覧と詳細の UI を実装するを使用してコント ローラーとビュー")
5. <span data-ttu-id="e225f-129">[CRUD を提供する方法 (作成、読み取り、更新、削除) データ エントリのサポートを形成する](# "提供 CRUD (Create、Read、Update、Delete) データ形式のエントリをサポート")</span><span class="sxs-lookup"><span data-stu-id="e225f-129">[How to provide CRUD (create, read, update, delete) data form entry support](# "Provide CRUD (Create, Read, Update, Delete) Data Form Entry Support")</span></span>
6. [<span data-ttu-id="e225f-130">ViewData ViewModel クラスを実装する方法</span><span class="sxs-lookup"><span data-stu-id="e225f-130">How to use ViewData and implement ViewModel classes</span></span>](# "ViewData の使用と ViewModel クラスの実装")
7. [<span data-ttu-id="e225f-131">マスター ページとパーシャルを使用して UI を再利用する方法</span><span class="sxs-lookup"><span data-stu-id="e225f-131">How to re-use UI using master pages and partials</span></span>](# "マスター ページを使用して UI を再利用とパーシャル")
8. [<span data-ttu-id="e225f-132">効率的なデータのページングを実装する方法</span><span class="sxs-lookup"><span data-stu-id="e225f-132">How to implement efficient data paging</span></span>](# "実装効率的なデータ ページング")
9. [<span data-ttu-id="e225f-133">認証と承認を使用してアプリケーションをセキュリティで保護する方法</span><span class="sxs-lookup"><span data-stu-id="e225f-133">How to secure applications using authentication and authorization</span></span>](# "セキュリティで保護されたアプリケーションを使用して認証と承認")
10. [<span data-ttu-id="e225f-134">AJAX を使用して、動的な更新プログラムを配信する方法</span><span class="sxs-lookup"><span data-stu-id="e225f-134">How to use AJAX to deliver dynamic updates</span></span>](# "動的な更新プログラムを配信する AJAX を使用します。")
11. [<span data-ttu-id="e225f-135">AJAX を使用して、マッピングのシナリオを実装する方法</span><span class="sxs-lookup"><span data-stu-id="e225f-135">How to use AJAX to implement mapping scenarios</span></span>](# "マッピング シナリオの実装を使用して AJAX")
12. [<span data-ttu-id="e225f-136">自動化された単体テストを有効にする方法</span><span class="sxs-lookup"><span data-stu-id="e225f-136">How to enable automated unit testing</span></span>](# "自動化された単体テストを有効にします。")

<span data-ttu-id="e225f-137">NerdDinner の独自のコピーをビルドすることができますをそれぞれを完了して最初からステップはこの章で、チュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="e225f-137">You can build your own copy of NerdDinner from scratch by completing each step we walkthrough in this chapter.</span></span> <span data-ttu-id="e225f-138">代わりに、ソース コードをここでの完成版をダウンロードできます: [github NerdDinner](https://github.com/AspNetMVPSamples/NerdDinner)です。</span><span class="sxs-lookup"><span data-stu-id="e225f-138">Alternatively, you can download a completed version of the source code here: [NerdDinner on GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span></span> <span data-ttu-id="e225f-139">また、必要に応じても行えます[このチュートリアルの無料の PDF バージョンをダウンロード](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)オフライン チュートリアルを参照する場合。</span><span class="sxs-lookup"><span data-stu-id="e225f-139">You can also optionally also [download a free PDF version of this tutorial](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) if you want to read the tutorial offline.</span></span>

<span data-ttu-id="e225f-140">Visual Studio 2008 または、空き Visual Web Developer 2008 Express のいずれかを使用するには、アプリケーションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="e225f-140">You can use either Visual Studio 2008 or the free Visual Web Developer 2008 Express to build the application.</span></span> <span data-ttu-id="e225f-141">データベースの SQL Server または無料の SQL Server Express のいずれかを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="e225f-141">You can use either SQL Server or the free SQL Server Express for the database.</span></span>

<span data-ttu-id="e225f-142">ASP.NET MVC、Visual Web Developer 2008 Express、および SQL Server Express (すべて無償) の V2 を使用してをインストールすることができます、 [Microsoft Web プラットフォーム インストーラー](https://www.microsoft.com/web/downloads/platform.aspx)</span><span class="sxs-lookup"><span data-stu-id="e225f-142">You can install ASP.NET MVC, Visual Web Developer 2008 Express, and SQL Server Express (all free) using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)</span></span>

### <a name="now-lets-get-started"></a><span data-ttu-id="e225f-143">今すぐ始めましょう.</span><span class="sxs-lookup"><span data-stu-id="e225f-143">Now let's get started....</span></span>

<span data-ttu-id="e225f-144">後に取り上げる NerdDinner は、これで、袖をロールアップし、コードを記述してみましょう。</span><span class="sxs-lookup"><span data-stu-id="e225f-144">Now that we've covered what NerdDinner is, let's roll up our sleeves and write some code.</span></span>

<span data-ttu-id="e225f-145">まずファイルを使用して、&gt;NerdDinner アプリケーションを作成する Visual Studio 内で新しいプロジェクト。</span><span class="sxs-lookup"><span data-stu-id="e225f-145">We'll begin by using File-&gt;New Project within Visual Studio to create the NerdDinner application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e225f-146">次へ</span><span class="sxs-lookup"><span data-stu-id="e225f-146">Next</span></span>](create-a-new-aspnet-mvc-project.md)
