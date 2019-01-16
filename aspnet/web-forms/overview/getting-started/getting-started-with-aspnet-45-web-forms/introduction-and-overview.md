---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: ASP.NET 4.7 Web フォームと Visual Studio 2017 の概要 |Microsoft Docs
author: Erikre
description: このステップ バイ ステップ チュートリアル シリーズが ASP.NET 4.7 および Microsoft Visual Studio を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を講義します。
ms.author: riande
ms.date: 01/09/2019
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: fb41ce72e9454d8d670a0b95234d2bc3f909f0ee
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341554"
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2017"></a><span data-ttu-id="fd9a9-103">ASP.NET 4.5 Web フォームと Visual Studio 2017 の概要</span><span class="sxs-lookup"><span data-stu-id="fd9a9-103">Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2017</span></span>
====================

<span data-ttu-id="fd9a9-104">[Wingtip Toys のサンプル プロジェクト (C#) をダウンロード](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)または[電子書籍 (PDF) をダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="fd9a9-104">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

<span data-ttu-id="fd9a9-105">このチュートリアル シリーズでは、ASP.NET 4.5 と Microsoft Visual Studio 2017 で ASP.NET Web フォーム アプリケーションを構築する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-105">This tutorial series shows you how to build an ASP.NET Web Forms application with ASP.NET 4.5 and Microsoft Visual Studio 2017.</span></span> 

## <a name="introduction"></a><span data-ttu-id="fd9a9-106">はじめに</span><span class="sxs-lookup"><span data-stu-id="fd9a9-106">Introduction</span></span>

<span data-ttu-id="fd9a9-107">このチュートリアル シリーズでは、Visual Studio 2017 と ASP.NET 4.5 を使用して ASP.NET Web フォーム アプリケーションを作成する手順に従ってできます。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-107">This tutorial series guides you through creating an ASP.NET Web Forms application using Visual Studio 2017 and ASP.NET 4.5.</span></span> <span data-ttu-id="fd9a9-108">という名前のアプリケーションを作成します**Wingtip Toys** - 販売品目をオンラインの簡略化された店舗の web サイト。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-108">You'll create an application named **Wingtip Toys** - a simplified storefront web site selling items online.</span></span> <span data-ttu-id="fd9a9-109">このシリーズでは、中には、ASP.NET 4.5 の新機能が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-109">During the series, new ASP.NET 4.5 features are highlighted.</span></span>

### <a name="target-audience"></a><span data-ttu-id="fd9a9-110">対象ユーザー</span><span class="sxs-lookup"><span data-stu-id="fd9a9-110">Target audience</span></span>

<span data-ttu-id="fd9a9-111">ASP.NET Web フォームに慣れていない開発者は、このチュートリアル シリーズの対象者です。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-111">Developers new to ASP.NET Web Forms are the target audience for this tutorial series.</span></span>

<span data-ttu-id="fd9a9-112">ある程度の知識は、次の領域が必要です。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-112">You should have some knowledge in the following areas:</span></span>

- <span data-ttu-id="fd9a9-113">オブジェクト指向プログラミング (OOP) および言語</span><span class="sxs-lookup"><span data-stu-id="fd9a9-113">Object-oriented programming (OOP) and languages</span></span>
- <span data-ttu-id="fd9a9-114">Web 開発 (HTML、CSS、JavaScript)</span><span class="sxs-lookup"><span data-stu-id="fd9a9-114">Web development (HTML, CSS, JavaScript)</span></span>
- <span data-ttu-id="fd9a9-115">リレーショナル データベース</span><span class="sxs-lookup"><span data-stu-id="fd9a9-115">Relational databases</span></span>
- <span data-ttu-id="fd9a9-116">N 層アーキテクチャ</span><span class="sxs-lookup"><span data-stu-id="fd9a9-116">N-tier architecture</span></span>

<span data-ttu-id="fd9a9-117">これらの領域を確認するには、次の内容を調査を検討してください。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-117">To review these areas, consider studying the following content:</span></span>

- [<span data-ttu-id="fd9a9-118">Visual C# の概要</span><span class="sxs-lookup"><span data-stu-id="fd9a9-118">Getting Started with Visual C#</span></span>](https://msdn.microsoft.com/library/a72418yk.aspx)
- <span data-ttu-id="fd9a9-119">[Web 開発](https://msdn.microsoft.com/beginner/bb308760.aspx)、 [HTML、CSS、JavaScript、SQL、PHP、JQuery](http://w3schools.com/)</span><span class="sxs-lookup"><span data-stu-id="fd9a9-119">[Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span></span>
- [<span data-ttu-id="fd9a9-120">リレーショナル データベース</span><span class="sxs-lookup"><span data-stu-id="fd9a9-120">Relational database</span></span>](http://en.wikipedia.org/wiki/Relational_database)
- [<span data-ttu-id="fd9a9-121">複数層アーキテクチャ</span><span class="sxs-lookup"><span data-stu-id="fd9a9-121">Multitier architecture</span></span>](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a><span data-ttu-id="fd9a9-122">アプリケーションの機能</span><span class="sxs-lookup"><span data-stu-id="fd9a9-122">Application features</span></span>

<span data-ttu-id="fd9a9-123">このシリーズに表示される ASP.NET Web フォームの機能は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-123">The ASP.NET Web Form features presented in this series include:</span></span>

- <span data-ttu-id="fd9a9-124">Web アプリケーション プロジェクト (Web サイト プロジェクトではない)</span><span class="sxs-lookup"><span data-stu-id="fd9a9-124">The Web Application Project (not Web Site Project)</span></span>
- <span data-ttu-id="fd9a9-125">Web フォーム</span><span class="sxs-lookup"><span data-stu-id="fd9a9-125">Web Forms</span></span>
- <span data-ttu-id="fd9a9-126">マスター ページの構成</span><span class="sxs-lookup"><span data-stu-id="fd9a9-126">Master Pages, Configuration</span></span>
- <span data-ttu-id="fd9a9-127">ブートス トラップ</span><span class="sxs-lookup"><span data-stu-id="fd9a9-127">Bootstrap</span></span>
- <span data-ttu-id="fd9a9-128">Entity Framework コードの最初に、LocalDB</span><span class="sxs-lookup"><span data-stu-id="fd9a9-128">Entity Framework Code First, LocalDB</span></span>
- <span data-ttu-id="fd9a9-129">要求の検証</span><span class="sxs-lookup"><span data-stu-id="fd9a9-129">Request Validation</span></span>
- <span data-ttu-id="fd9a9-130">厳密に型指定されたデータ コントロール</span><span class="sxs-lookup"><span data-stu-id="fd9a9-130">Strongly-typed Data Controls</span></span>
- <span data-ttu-id="fd9a9-131">モデル バインド</span><span class="sxs-lookup"><span data-stu-id="fd9a9-131">Model Binding</span></span>
- <span data-ttu-id="fd9a9-132">データの注釈</span><span class="sxs-lookup"><span data-stu-id="fd9a9-132">Data Annotations</span></span>
- <span data-ttu-id="fd9a9-133">値プロバイダー</span><span class="sxs-lookup"><span data-stu-id="fd9a9-133">Value Providers</span></span>
- <span data-ttu-id="fd9a9-134">SSL と OAuth</span><span class="sxs-lookup"><span data-stu-id="fd9a9-134">SSL and OAuth</span></span>
- <span data-ttu-id="fd9a9-135">ASP.NET Identity、構成、および承認</span><span class="sxs-lookup"><span data-stu-id="fd9a9-135">ASP.NET Identity, Configuration, and Authorization</span></span>
- <span data-ttu-id="fd9a9-136">控え目な検証</span><span class="sxs-lookup"><span data-stu-id="fd9a9-136">Unobtrusive Validation</span></span>
- <span data-ttu-id="fd9a9-137">ルーティング</span><span class="sxs-lookup"><span data-stu-id="fd9a9-137">Routing</span></span>
- <span data-ttu-id="fd9a9-138">ASP.NET エラー処理</span><span class="sxs-lookup"><span data-stu-id="fd9a9-138">ASP.NET Error Handling</span></span>

### <a name="application-scenarios-and-tasks"></a><span data-ttu-id="fd9a9-139">アプリケーションのシナリオとタスク</span><span class="sxs-lookup"><span data-stu-id="fd9a9-139">Application scenarios and tasks</span></span>

<span data-ttu-id="fd9a9-140">チュートリアル シリーズのタスクは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-140">Tutorial series tasks include:</span></span>

- <span data-ttu-id="fd9a9-141">新しいプロジェクトの作成、レビュー、および</span><span class="sxs-lookup"><span data-stu-id="fd9a9-141">Creating, reviewing, and running a new project</span></span>
- <span data-ttu-id="fd9a9-142">データベース構造を作成します。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-142">Creating a database structure</span></span>
- <span data-ttu-id="fd9a9-143">初期化とデータベースをシード処理</span><span class="sxs-lookup"><span data-stu-id="fd9a9-143">Initializing and seeding a database</span></span>
- <span data-ttu-id="fd9a9-144">スタイル、グラフィック、およびマスター ページの UI のカスタマイズ</span><span class="sxs-lookup"><span data-stu-id="fd9a9-144">Customizing the UI with styles, graphics, and a master page</span></span>
- <span data-ttu-id="fd9a9-145">ページおよびナビゲーションの追加</span><span class="sxs-lookup"><span data-stu-id="fd9a9-145">Adding pages and navigation</span></span>
- <span data-ttu-id="fd9a9-146">メニューの詳細と製品データを表示します。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-146">Displaying menu details and product data</span></span>
- <span data-ttu-id="fd9a9-147">ショッピング カートの作成</span><span class="sxs-lookup"><span data-stu-id="fd9a9-147">Creating a shopping cart</span></span>
- <span data-ttu-id="fd9a9-148">追加の SSL と OAuth サポート</span><span class="sxs-lookup"><span data-stu-id="fd9a9-148">Adding SSL and OAuth support</span></span>
- <span data-ttu-id="fd9a9-149">支払い方法を追加します。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-149">Adding a payment method</span></span>
- <span data-ttu-id="fd9a9-150">管理者のロールとアプリケーションのユーザーを含む</span><span class="sxs-lookup"><span data-stu-id="fd9a9-150">Including an administrator role and a user to the application</span></span>
- <span data-ttu-id="fd9a9-151">特定のページとフォルダーへのアクセスを制限します。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-151">Restricting access to specific pages and folder</span></span>
- <span data-ttu-id="fd9a9-152">Web アプリケーションにファイルをアップロードします。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-152">Uploading a file to the web application</span></span>
- <span data-ttu-id="fd9a9-153">入力の検証を実装します。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-153">Implementing input validation</span></span>
- <span data-ttu-id="fd9a9-154">Web アプリケーションのルートを登録します。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-154">Registering routes for the web application</span></span>
- <span data-ttu-id="fd9a9-155">エラー処理とエラーのログ記録を実装します。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-155">Implementing error handling and error logging</span></span>

## <a name="overview"></a><span data-ttu-id="fd9a9-156">概要</span><span class="sxs-lookup"><span data-stu-id="fd9a9-156">Overview</span></span>

<span data-ttu-id="fd9a9-157">このチュートリアル シリーズは、プログラミングの概念を理解して他のユーザーは新しい ASP.NET Web フォームには。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-157">This tutorial series is intended for someone familiar with programming concepts, but new to ASP.NET Web Forms.</span></span> <span data-ttu-id="fd9a9-158">ASP.NET Web フォームを使い慣れている場合は、このシリーズまだできます ASP.NET 4.5 の新機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-158">If you're already familiar with ASP.NET Web Forms, this series can still help you learn about new ASP.NET 4.5 features.</span></span> <span data-ttu-id="fd9a9-159">プログラミングの概念と、ASP.NET Web フォームに不慣れで提供される追加の Web フォームのチュートリアルを参照してください、 [Getting Started](../../../index.md) ASP.NET Web サイトに関するセクション。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-159">For readers unfamiliar with programming concepts and ASP.NET Web Forms, see the additional Web Forms tutorials provided in the [Getting Started](../../../index.md) section on the ASP.NET Web site.</span></span>

<span data-ttu-id="fd9a9-160">このチュートリアル シリーズで提供される ASP.NET 4.5 には、次の機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-160">The ASP.NET 4.5 provided in this tutorial series includes the following features:</span></span>

- <span data-ttu-id="fd9a9-161">提供するプロジェクトを作成するための単純な UI[多くの ASP.NET フレームワークのサポート](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add)(Web フォーム、MVC、および Web API)。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-161">A simple UI for creating projects that offers [support for many ASP.NET frameworks](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC, and Web API).</span></span>
- <span data-ttu-id="fd9a9-162">[ブートス トラップ](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap)レイアウト、テーマ、および応答性の高い設計のフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-162">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), a layout, theming, and responsive design framework.</span></span>
- <span data-ttu-id="fd9a9-163">[ASP.NET Identity](../../../../identity/index.md)、IIS 以外のソフトウェアをホストしている web を使用したすべての ASP.NET フレームワークと動作で同様に動作する新しい ASP.NET メンバーシップ システムです。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-163">[ASP.NET Identity](../../../../identity/index.md), a new ASP.NET membership system that works the same in all ASP.NET frameworks and works with web hosting software other than IIS.</span></span>
- [<span data-ttu-id="fd9a9-164">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="fd9a9-164">Entity Framework 6</span></span>](https://msdn.microsoft.com/data/ef.aspx)

  <span data-ttu-id="fd9a9-165">有効にすると、Entity Framework に更新します。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-165">An update to the Entity Framework enabling you to:</span></span>
  - <span data-ttu-id="fd9a9-166">取得し、厳密に型指定されたオブジェクトとしてデータを操作します。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-166">Retrieve and manipulate data as strongly-typed objects</span></span>
  - <span data-ttu-id="fd9a9-167">データを非同期的にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-167">Access data asynchronously</span></span>
  - <span data-ttu-id="fd9a9-168">一時的な接続エラーを処理します。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-168">Handle transient connection faults</span></span>
  - <span data-ttu-id="fd9a9-169">SQL ステートメントのログ</span><span class="sxs-lookup"><span data-stu-id="fd9a9-169">Log SQL statements</span></span>

<span data-ttu-id="fd9a9-170">完全な ASP.NET 4.5 機能の一覧を参照してください。 [ASP.NET および Web Tools for Visual Studio 2013 リリース ノート](../../../../visual-studio/overview/2013/release-notes.md)します。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-170">For a complete ASP.NET 4.5 feature list, see [ASP.NET and Web Tools for Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span></span>

### <a name="the-wingtip-toys-sample-application"></a><span data-ttu-id="fd9a9-171">Wingtip Toys のサンプル アプリケーション</span><span class="sxs-lookup"><span data-stu-id="fd9a9-171">The Wingtip Toys sample application</span></span>

<span data-ttu-id="fd9a9-172">次のスクリーン ショットは、このチュートリアル シリーズで作成した ASP.NET Web フォーム アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-172">The following screenshots are from the ASP.NET Web Forms application that you create in this tutorial series.</span></span> <span data-ttu-id="fd9a9-173">Visual Studio でアプリケーションを実行すると、次の web のホーム ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-173">When you run the application in Visual Studio, the following web Home page appears.</span></span>

![Wingtip Toys の既定のページ](introduction-and-overview/_static/image1.png)

<span data-ttu-id="fd9a9-175">新しいユーザーとして登録したり、既存のユーザーとしてサインインできます。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-175">You can register as a new user, or sign in as an existing user.</span></span> <span data-ttu-id="fd9a9-176">上部のナビゲーションには、データベースから製品カテゴリとその製品へのリンクがあります。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-176">The top navigation has links to product categories and their products from the database.</span></span>

<span data-ttu-id="fd9a9-177">選択した場合**製品**、利用可能なすべての製品が表示されます。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-177">If you select **Products**, all available products are displayed.</span></span> 

![Wingtip Toys の製品](introduction-and-overview/_static/image2.png)

<span data-ttu-id="fd9a9-179">特定の製品を選択した場合は、製品の詳細が表示されます。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-179">If you select a specific product, product details are displayed.</span></span>


![Wingtip Toys の製品の詳細](introduction-and-overview/_static/image3.png)

<span data-ttu-id="fd9a9-181">ユーザーは、登録し、Web フォーム テンプレートの既定の機能を使用してサインインできます。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-181">As a user, you can register and sign in with Web Forms template default functionality.</span></span> <span data-ttu-id="fd9a9-182">このチュートリアルでは、既存の Gmail アカウントを使用してサインインする方法も説明します。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-182">This tutorial also explains how to sign in using an existing Gmail account.</span></span> <span data-ttu-id="fd9a9-183">さらに、追加し、データベースから製品を削除するには、管理者としてサインインすることができます。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-183">Additionally, you can sign in as the administrator to add and remove products from the database.</span></span>

![Wingtip Toys のサインイン](introduction-and-overview/_static/image4.png)

<span data-ttu-id="fd9a9-185">ユーザーとしてサインインした後は、ショッピング カートと精算と PayPal に製品を追加できます。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-185">Once you've signed in as a user, you can add products to the shopping cart and checkout with PayPal.</span></span> <span data-ttu-id="fd9a9-186">サンプル アプリケーションは、PayPal の開発者のサンド ボックスで作業する設計されています。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-186">The sample application is designed to work in PayPal's developer sandbox.</span></span> <span data-ttu-id="fd9a9-187">実際の金額のトランザクションは実行されません。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-187">No actual money transaction takes place.</span></span>

![Wingtip Toys のショッピング カート](introduction-and-overview/_static/image5.png)

<span data-ttu-id="fd9a9-189">PayPal は、アカウント、注文、支払情報を確認します。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-189">PayPal confirms your account, order, and payment information.</span></span>

![Wingtip Toys の PayPal](introduction-and-overview/_static/image6.png)

<span data-ttu-id="fd9a9-191">PayPal から返された後、は、確認し、注文を完了できます。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-191">After returning from PayPal, you can review and complete your order.</span></span>

![Wingtip Toys の注文の確認](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a><span data-ttu-id="fd9a9-193">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="fd9a9-193">Prerequisites</span></span>

<span data-ttu-id="fd9a9-194">開始する前に、次のソフトウェアがコンピューターにインストールされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-194">Before you start, make sure the following software is installed on your computer:</span></span>

- <span data-ttu-id="fd9a9-195">[Microsoft Visual Studio 2017 または Microsoft Visual Studio Community 2017](https://visualstudio.microsoft.com/downloads/)します。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-195">[Microsoft Visual Studio 2017 or Microsoft Visual Studio Community 2017](https://visualstudio.microsoft.com/downloads/).</span></span>

<span data-ttu-id="fd9a9-196">.NET Framework は、自動的にインストールされます。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-196">The .NET Framework is installed automatically.</span></span>

<span data-ttu-id="fd9a9-197">このチュートリアル シリーズでは、Microsoft Visual Studio Community 2017 を使用します。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-197">This tutorial series uses Microsoft Visual Studio Community 2017.</span></span> <span data-ttu-id="fd9a9-198">ボリュームを使用しているか、またはこのチュートリアル シリーズを完了する、Microsoft Visual Studio 2017。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-198">You can use either that or Microsoft Visual Studio 2017 to complete this tutorial series.</span></span>

<span data-ttu-id="fd9a9-199">Visual Studio については、次に注意してください。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-199">Note the following about Visual Studio:</span></span>

* <span data-ttu-id="fd9a9-200">Microsoft Visual Studio 2017 および Microsoft Visual Studio Community 2017 と呼びます*Visual Studio*このチュートリアル シリーズ全体でします。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-200">Microsoft Visual Studio 2017 and Microsoft Visual Studio Community 2017 are referred to as *Visual Studio* throughout this tutorial series.</span></span>

* <span data-ttu-id="fd9a9-201">Visual Studio 2017 が既にインストールされている古いバージョンの横にあるインストールされます。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-201">Visual Studio 2017 is installed next to any older versions already installed.</span></span> <span data-ttu-id="fd9a9-202">以前のバージョンで作成されたサイトでは、Visual Studio 2017 で開くことができ、以前のバージョンを開くに進みます。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-202">Sites created in earlier versions can be opened in Visual Studio 2017 and continue to open in previous versions.</span></span>

* <span data-ttu-id="fd9a9-203">初めて Visual Studio を開始した、選択したと見なされます、 *Web 開発*設定します。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-203">The first time you started Visual Studio, it is assumed you selected the *Web Development* settings.</span></span> <span data-ttu-id="fd9a9-204">詳細については、「[方法 :Web 開発環境の設定を選択して](https://msdn.microsoft.com/library/ff521558.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-204">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>

<span data-ttu-id="fd9a9-205">前提条件をインストールすると、このチュートリアル シリーズで表示される Web プロジェクトの作成を開始する準備ができました。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-205">After installing the prerequisites, you're ready to begin creating the Web project presented in this tutorial series.</span></span>

## <a name="download-the-sample-application"></a><span data-ttu-id="fd9a9-206">サンプル アプリケーションをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-206">Download the sample application</span></span>

 <span data-ttu-id="fd9a9-207">MSDN のサンプル サイトからの完全なサンプル applicatiion をいつでもダウンロードできます。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-207">You can download the completed sample applicatiion at anytime from the MSDN Samples site:</span></span>

<span data-ttu-id="fd9a9-208">[ASP.NET 4.5 Web フォームと Visual Studio 2013 - Wingtip Toys 概要](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)(C#)</span><span class="sxs-lookup"><span data-stu-id="fd9a9-208">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span> 

 <span data-ttu-id="fd9a9-209">このダウンロードには、次のものがあります。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-209">This download has the following items:</span></span>

- <span data-ttu-id="fd9a9-210">サンプル アプリケーションは、 *WingtipToys*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-210">The sample application in the *WingtipToys* folder.</span></span>
- <span data-ttu-id="fd9a9-211">サンプル アプリケーションの作成に使用されるリソース、 *WingtipToys 資産*フォルダーで、 *WingtipToys*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-211">The resources used to create the sample application in the *WingtipToys-Assets* folder in the *WingtipToys* folder.</span></span>

<span data-ttu-id="fd9a9-212">ダウンロードが、 *.zip*ファイル。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-212">The download is a *.zip* file.</span></span> <span data-ttu-id="fd9a9-213">このチュートリアル シリーズを作成する完成したプロジェクト、検索と選択して、 *C#* .zip ファイル内のフォルダー。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-213">To see the completed project that this tutorial series creates, find and select the *C#* folder in the .zip file.</span></span> <span data-ttu-id="fd9a9-214">保存、C#を使用する Visual Studio プロジェクトと作業フォルダーのフォルダー。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-214">Save the C# folder to the folder you use to work with Visual Studio projects.</span></span> <span data-ttu-id="fd9a9-215">既定では、Visual Studio 2017 のプロジェクト フォルダーは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-215">By default, the Visual Studio 2017 projects folder is:</span></span>

<span data-ttu-id="fd9a9-216"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\source\repos</strong></span><span class="sxs-lookup"><span data-stu-id="fd9a9-216"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\source\repos</strong></span></span>

<span data-ttu-id="fd9a9-217">名前を変更、 ***C#*** フォルダー ***WingtipToys***です。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-217">Rename the ***C#*** folder to ***WingtipToys***.</span></span>

> [!NOTE]
> <span data-ttu-id="fd9a9-218">という名前のフォルダーが既にある場合*WingtipToys* Projects フォルダーに名前を一時的にその既存のフォルダーの名前変更する前に、 *C#* フォルダー *WingtipToys*です。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-218">If you already have a folder named *WingtipToys* in your Projects folder, temporarily rename that existing folder before renaming the *C#* folder to *WingtipToys*.</span></span>

<span data-ttu-id="fd9a9-219">完成したプロジェクトを実行するには、開く、 *WingtipToys*フォルダーをダブルクリックします、 *WingtipToys.sln*ファイル。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-219">To run the completed project, open the *WingtipToys* folder and double-click the *WingtipToys.sln* file.</span></span> <span data-ttu-id="fd9a9-220">Visual Studio 2017 では、プロジェクトを開きます。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-220">Visual Studio 2017 opens the project.</span></span> <span data-ttu-id="fd9a9-221">次に、右クリックし、 *Default.aspx*ファイル**ソリューション エクスプ ローラー**選択**ブラウザーで表示**します。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-221">Next, right-click the *Default.aspx* file in **Solution Explorer** and select **View In Browser**.</span></span>

## <a name="take-a-aspnet-web-forms-quiz-to-review-content"></a><span data-ttu-id="fd9a9-222">ASP.NET Web フォーム クイズにコンテンツを確認するには</span><span class="sxs-lookup"><span data-stu-id="fd9a9-222">Take a ASP.NET Web Forms quiz to review content</span></span>

<span data-ttu-id="fd9a9-223">チュートリアルのシリーズを完了すると、知識をテストし、主要な概念を強調するクイズに挑戦します。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-223">After completing the tutorial series, take a quiz to test your knowledge and reinforce key concepts.</span></span> <span data-ttu-id="fd9a9-224">各質問は、説明とその他のガイダンスへのリンクを提供します。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-224">Each question provides an explanation and links to additional guidance.</span></span>

 * [<span data-ttu-id="fd9a9-225">ASP.NET Web フォームのクイズ</span><span class="sxs-lookup"><span data-stu-id="fd9a9-225">ASP.NET Web Forms Quiz</span></span>](https://blogs.msdn.microsoft.com/erikreitan/2016/01/08/asp-net-web-forms-quiz/) 

## <a name="tutorial-support-and-comments"></a><span data-ttu-id="fd9a9-226">チュートリアルのサポートとコメント</span><span class="sxs-lookup"><span data-stu-id="fd9a9-226">Tutorial support and comments</span></span>

<span data-ttu-id="fd9a9-227">質問やコメントは、使用、Q & A のセクションに含まれています、 [ASP.NET 4.5 Web フォームと Visual Studio 2013 - Wingtip Toys 概要](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)(C#) サンプル ページ。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-227">For questions and comments, use the Q and A section included on the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample page.</span></span>

<span data-ttu-id="fd9a9-228">このチュートリアル シリーズのコメントは、ようこそ。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-228">Comments on this tutorial series are welcome.</span></span> <span data-ttu-id="fd9a9-229">このチュートリアル シリーズを更新すると、機能強化に関する修正または提案を検討する努力が行われます。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-229">When this tutorial series is updated, every effort is made to consider corrections or suggestions for improvements.</span></span>

<span data-ttu-id="fd9a9-230">かどうか、エラーが発生した、対応するエラー メッセージが、混乱を招く可能性がありますでないその修正方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-230">If an error occurs, the corresponding error messages could be confusing, with no good explanation on how to fix it.</span></span> <span data-ttu-id="fd9a9-231">ヘルプについては、確認することができます、 [ASP.NET フォーラム](https://forums.asp.net/)します。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-231">For help, you can check the [ASP.NET forums](https://forums.asp.net/).</span></span> <span data-ttu-id="fd9a9-232">別の適切なソースは、Q & A のセクションで、 [ASP.NET 4.5 Web フォームと Visual Studio 2013 - Wingtip Toys 概要](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)(C#) サンプル ページ。</span><span class="sxs-lookup"><span data-stu-id="fd9a9-232">Another good source is the Q and A section in the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample page.</span></span> 

> [!div class="step-by-step"]
> [<span data-ttu-id="fd9a9-233">次へ</span><span class="sxs-lookup"><span data-stu-id="fd9a9-233">Next</span></span>](create-the-project.md)
