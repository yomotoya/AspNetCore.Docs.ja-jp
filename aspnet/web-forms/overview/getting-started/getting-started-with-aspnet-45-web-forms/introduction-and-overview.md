---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: ASP.NET 4.5 Web フォームと Visual Studio 2013 の概要 |Microsoft Docs
author: Erikre
description: このステップ バイ ステップ チュートリアル シリーズでは、ASP.NET 4.5 と Microsoft Visual Studio の表現を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明しています.
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: ad5e97cd596e146f742c4c5e882d3938005070d1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830333"
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a><span data-ttu-id="a7050-103">ASP.NET 4.5 Web フォームと Visual Studio 2013 の概要</span><span class="sxs-lookup"><span data-stu-id="a7050-103">Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013</span></span>
====================
<span data-ttu-id="a7050-104">によって[Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="a7050-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="a7050-105">[Wingtip Toys のサンプル プロジェクト (c#) をダウンロード](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)または[電子書籍 (PDF) をダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="a7050-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="a7050-106">このステップ バイ ステップ チュートリアル シリーズでは、Web 用 ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明します。</span><span class="sxs-lookup"><span data-stu-id="a7050-106">This step-by-step tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> [<span data-ttu-id="a7050-107">ASP.NET Web フォームのクイズ</span><span class="sxs-lookup"><span data-stu-id="a7050-107">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)  
> <span data-ttu-id="a7050-108">知識をテストし、ASP.NET Web フォームのクイズを主要な概念を補足します。</span><span class="sxs-lookup"><span data-stu-id="a7050-108">Test your knowledge and reinforce key concepts by taking the ASP.NET Web Forms Quiz.</span></span> <span data-ttu-id="a7050-109">このクイズは、このチュートリアル シリーズに含まれるコンテンツからに設計されています。</span><span class="sxs-lookup"><span data-stu-id="a7050-109">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="a7050-110">クイズの各項目について追加のガイダンスへのリンクと共に説明します。</span><span class="sxs-lookup"><span data-stu-id="a7050-110">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>


## <a name="introduction"></a><span data-ttu-id="a7050-111">はじめに</span><span class="sxs-lookup"><span data-stu-id="a7050-111">Introduction</span></span>

<span data-ttu-id="a7050-112">この一連のチュートリアルに従って、Web アプリケーションおよび ASP.NET 4.5 用の Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションを作成するために必要な手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="a7050-112">This series of tutorials guides you through the steps required to create an ASP.NET Web Forms application using Visual Studio Express 2013 for Web and ASP.NET 4.5.</span></span>

<span data-ttu-id="a7050-113">作成するアプリケーションの名前は**Wingtip Toys**します。</span><span class="sxs-lookup"><span data-stu-id="a7050-113">The application you'll create is named **Wingtip Toys**.</span></span> <span data-ttu-id="a7050-114">オンラインのアイテムを販売するストア フロントの web サイトの簡略化された例です。</span><span class="sxs-lookup"><span data-stu-id="a7050-114">It's a simplified example of a store front web site that sells items online.</span></span> <span data-ttu-id="a7050-115">このチュートリアル シリーズには、ASP.NET 4.5 の新機能が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="a7050-115">This tutorial series highlights new features available in ASP.NET 4.5.</span></span>

<span data-ttu-id="a7050-116">コメントは、[ようこそ] と、このチュートリアル シリーズのご提案に基づいてを更新するには、あらゆる努力をしましょう。</span><span class="sxs-lookup"><span data-stu-id="a7050-116">Comments are welcome, and we'll make every effort to update this tutorial series based on your suggestions.</span></span>

### <a name="download-completed-project"></a><span data-ttu-id="a7050-117">プロジェクトのダウンロードが完了しました</span><span class="sxs-lookup"><span data-stu-id="a7050-117">Download completed project</span></span>

<span data-ttu-id="a7050-118">完了したチュートリアルを含む c# プロジェクトをダウンロードすることができます。</span><span class="sxs-lookup"><span data-stu-id="a7050-118">You can download a C# project that contains the completed tutorial.</span></span>

- <span data-ttu-id="a7050-119">[ASP.NET 4.5 Web フォームと Visual Studio 2013 - Wingtip Toys 概要](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)(c#)</span><span class="sxs-lookup"><span data-stu-id="a7050-119">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span>

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a><span data-ttu-id="a7050-120">関連する ASP.NET Web フォームのクイズを実行してコンテンツを確認します。</span><span class="sxs-lookup"><span data-stu-id="a7050-120">Review the content by taking the related ASP.NET Web Forms quiz</span></span>

<span data-ttu-id="a7050-121">このチュートリアルを完了すると、知識をテストしを実行して重要な概念を補足、 [ASP.NET Web フォームのクイズ](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001)します。</span><span class="sxs-lookup"><span data-stu-id="a7050-121">After you complete this tutorial, test your knowledge and reinforce key concepts by taking the [ASP.NET Web Forms Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span></span> <span data-ttu-id="a7050-122">このクイズは、このチュートリアル シリーズに含まれるコンテンツからに設計されています。</span><span class="sxs-lookup"><span data-stu-id="a7050-122">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="a7050-123">クイズの各項目について追加のガイダンスへのリンクと共に説明します。</span><span class="sxs-lookup"><span data-stu-id="a7050-123">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>

- [<span data-ttu-id="a7050-124">ASP.NET Web フォームのクイズ</span><span class="sxs-lookup"><span data-stu-id="a7050-124">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a><span data-ttu-id="a7050-125">対象ユーザー</span><span class="sxs-lookup"><span data-stu-id="a7050-125">Audience</span></span>

<span data-ttu-id="a7050-126">初心者に ASP.NET Web フォームは経験豊富な開発者はこのチュートリアル シリーズの対象とします。</span><span class="sxs-lookup"><span data-stu-id="a7050-126">The intended audience of this tutorial series is experienced developers who are new to ASP.NET Web Forms.</span></span> <span data-ttu-id="a7050-127">開発者はこのチュートリアル シリーズでは、次のスキルを持つ必要があります。</span><span class="sxs-lookup"><span data-stu-id="a7050-127">A developer interested in this tutorial series should have the following skills:</span></span>

- <span data-ttu-id="a7050-128">使い慣れたオブジェクト指向プログラミング (OOP) 言語</span><span class="sxs-lookup"><span data-stu-id="a7050-128">Familiar with an object oriented programming (OOP) language</span></span>
- <span data-ttu-id="a7050-129">経験のある Web 開発の概念 (HTML、CSS、JavaScript)</span><span class="sxs-lookup"><span data-stu-id="a7050-129">Familiar with Web development concepts (HTML, CSS, JavaScript)</span></span>
- <span data-ttu-id="a7050-130">リレーショナル データベースの概念を理解しています。</span><span class="sxs-lookup"><span data-stu-id="a7050-130">Familiar with relational database concepts</span></span>
- <span data-ttu-id="a7050-131">N 層アーキテクチャの概念を理解</span><span class="sxs-lookup"><span data-stu-id="a7050-131">Familiar with n-tier architecture concepts</span></span>

<span data-ttu-id="a7050-132">上記の領域を確認したい場合は、次の内容の確認を検討してください。</span><span class="sxs-lookup"><span data-stu-id="a7050-132">If you are interested in reviewing the areas listed above, consider reviewing the following content:</span></span>

- [<span data-ttu-id="a7050-133">Visual c# の概要</span><span class="sxs-lookup"><span data-stu-id="a7050-133">Getting Started with Visual C#</span></span>](https://msdn.microsoft.com/library/a72418yk.aspx)
- <span data-ttu-id="a7050-134">[Web 開発](https://msdn.microsoft.com/beginner/bb308760.aspx)、 [HTML、CSS、JavaScript、SQL、PHP、JQuery](http://w3schools.com/)</span><span class="sxs-lookup"><span data-stu-id="a7050-134">[Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span></span>
- [<span data-ttu-id="a7050-135">リレーショナル データベース</span><span class="sxs-lookup"><span data-stu-id="a7050-135">Relational database</span></span>](http://en.wikipedia.org/wiki/Relational_database)
- [<span data-ttu-id="a7050-136">複数層アーキテクチャ</span><span class="sxs-lookup"><span data-stu-id="a7050-136">Multitier architecture</span></span>](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a><span data-ttu-id="a7050-137">アプリケーションの機能</span><span class="sxs-lookup"><span data-stu-id="a7050-137">Application Features</span></span>

<span data-ttu-id="a7050-138">このシリーズに表示される ASP.NET Web フォームの機能は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="a7050-138">The ASP.NET Web Form features presented in this series include:</span></span>

- <span data-ttu-id="a7050-139">Web アプリケーション プロジェクト (Web サイト プロジェクトではない)</span><span class="sxs-lookup"><span data-stu-id="a7050-139">The Web Application Project (not Web Site Project)</span></span>
- <span data-ttu-id="a7050-140">Web フォーム</span><span class="sxs-lookup"><span data-stu-id="a7050-140">Web Forms</span></span>
- <span data-ttu-id="a7050-141">マスター ページの構成</span><span class="sxs-lookup"><span data-stu-id="a7050-141">Master Pages, Configuration</span></span>
- <span data-ttu-id="a7050-142">ブートス トラップ</span><span class="sxs-lookup"><span data-stu-id="a7050-142">Bootstrap</span></span>
- <span data-ttu-id="a7050-143">Entity Framework コードの最初に、LocalDB</span><span class="sxs-lookup"><span data-stu-id="a7050-143">Entity Framework Code First, LocalDB</span></span>
- <span data-ttu-id="a7050-144">要求の検証</span><span class="sxs-lookup"><span data-stu-id="a7050-144">Request Validation</span></span>
- <span data-ttu-id="a7050-145">厳密に型指定されたデータ コントロールでは、モデルのバインディング、データの注釈と値のプロバイダー</span><span class="sxs-lookup"><span data-stu-id="a7050-145">Strongly Typed Data Controls, Model Binding, Data Annotations, and Value Providers</span></span>
- <span data-ttu-id="a7050-146">SSL と OAuth</span><span class="sxs-lookup"><span data-stu-id="a7050-146">SSL and OAuth</span></span>
- <span data-ttu-id="a7050-147">ASP.NET Identity、構成、および承認</span><span class="sxs-lookup"><span data-stu-id="a7050-147">ASP.NET Identity, Configuration, and Authorization</span></span>
- <span data-ttu-id="a7050-148">控え目な検証</span><span class="sxs-lookup"><span data-stu-id="a7050-148">Unobtrusive Validation</span></span>
- <span data-ttu-id="a7050-149">ルーティング</span><span class="sxs-lookup"><span data-stu-id="a7050-149">Routing</span></span>
- <span data-ttu-id="a7050-150">ASP.NET エラー処理</span><span class="sxs-lookup"><span data-stu-id="a7050-150">ASP.NET Error Handling</span></span>

### <a name="application-scenarios-and-tasks"></a><span data-ttu-id="a7050-151">アプリケーションのシナリオとタスク</span><span class="sxs-lookup"><span data-stu-id="a7050-151">Application Scenarios and Tasks</span></span>

<span data-ttu-id="a7050-152">このシリーズで説明するタスクは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="a7050-152">Tasks demonstrated in this series include:</span></span>

- <span data-ttu-id="a7050-153">新しいプロジェクトを実行して作成し、レビュー</span><span class="sxs-lookup"><span data-stu-id="a7050-153">Creating, reviewing and running the new project</span></span>
- <span data-ttu-id="a7050-154">データベース構造を作成します。</span><span class="sxs-lookup"><span data-stu-id="a7050-154">Creating the database structure</span></span>
- <span data-ttu-id="a7050-155">初期化とデータベースのシード処理</span><span class="sxs-lookup"><span data-stu-id="a7050-155">Initializing and seeding the database</span></span>
- <span data-ttu-id="a7050-156">スタイル、グラフィックス、およびマスター ページを使用して UI のカスタマイズ</span><span class="sxs-lookup"><span data-stu-id="a7050-156">Customizing the UI using styles, graphics and a master page</span></span>
- <span data-ttu-id="a7050-157">ページおよびナビゲーションの追加</span><span class="sxs-lookup"><span data-stu-id="a7050-157">Adding pages and navigation</span></span>
- <span data-ttu-id="a7050-158">メニューの詳細と製品データを表示します。</span><span class="sxs-lookup"><span data-stu-id="a7050-158">Displaying menu details and product data</span></span>
- <span data-ttu-id="a7050-159">ショッピング カートの作成</span><span class="sxs-lookup"><span data-stu-id="a7050-159">Creating a shopping cart</span></span>
- <span data-ttu-id="a7050-160">追加の SSL と OAuth サポート</span><span class="sxs-lookup"><span data-stu-id="a7050-160">Adding SSL and OAuth support</span></span>
- <span data-ttu-id="a7050-161">支払い方法を追加します。</span><span class="sxs-lookup"><span data-stu-id="a7050-161">Adding a payment method</span></span>
- <span data-ttu-id="a7050-162">管理者のロールとアプリケーションのユーザーを含む</span><span class="sxs-lookup"><span data-stu-id="a7050-162">Including an administrator role and a user to the application</span></span>
- <span data-ttu-id="a7050-163">特定のページとフォルダーへのアクセスを制限します。</span><span class="sxs-lookup"><span data-stu-id="a7050-163">Restricting access to specific pages and folder</span></span>
- <span data-ttu-id="a7050-164">Web アプリケーションにファイルをアップロードします。</span><span class="sxs-lookup"><span data-stu-id="a7050-164">Uploading a file to the web application</span></span>
- <span data-ttu-id="a7050-165">入力の検証を実装します。</span><span class="sxs-lookup"><span data-stu-id="a7050-165">Implementing input validation</span></span>
- <span data-ttu-id="a7050-166">Web アプリケーションのルートを登録します。</span><span class="sxs-lookup"><span data-stu-id="a7050-166">Registering routes for the web application</span></span>
- <span data-ttu-id="a7050-167">エラー処理とエラーのログ記録を実装します。</span><span class="sxs-lookup"><span data-stu-id="a7050-167">Implementing error handling and error logging</span></span>

## <a name="overview"></a><span data-ttu-id="a7050-168">概要</span><span class="sxs-lookup"><span data-stu-id="a7050-168">Overview</span></span>

<span data-ttu-id="a7050-169">ASP.NET Web フォームに慣れていない場合がプログラミングの概念に関する知識があるが、適切なチュートリアルがあります。</span><span class="sxs-lookup"><span data-stu-id="a7050-169">If you are new to ASP.NET Web Forms but have familiarity with programming concepts, you have the right tutorial.</span></span> <span data-ttu-id="a7050-170">ASP.NET Web フォーム理解している場合は、ASP.NET 4.5 の新機能によって、このチュートリアル シリーズを活用できます。</span><span class="sxs-lookup"><span data-stu-id="a7050-170">If you are already familiar with ASP.NET Web Forms, you can benefit from this tutorial series by the new features available in ASP.NET 4.5.</span></span> <span data-ttu-id="a7050-171">プログラミングに関する概念および ASP.NET Web フォームに慣れていない場合は、Web フォームで提供される追加のチュートリアルを参照してください[Getting Started](../../../index.md) ASP.NET Web サイトに関するセクション。</span><span class="sxs-lookup"><span data-stu-id="a7050-171">If you are unfamiliar with programming concepts and ASP.NET Web Forms, see the additional tutorials provided in the Web Forms [Getting Started](../../../index.md) section on the ASP.NET Web site.</span></span>

<span data-ttu-id="a7050-172">特定**最新**チュートリアル シリーズの次のとおりこの Web フォームで提供される ASP.NET 4.5 の機能します。</span><span class="sxs-lookup"><span data-stu-id="a7050-172">The specific **latest** ASP.NET 4.5 features provided in this Web Forms tutorial series include the following:</span></span>

- <span data-ttu-id="a7050-173">作成するための単純な UI がそのオファーをプロジェクト[複数の ASP.NET フレームワークのサポート](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add)(Web フォーム、MVC、および Web API)。</span><span class="sxs-lookup"><span data-stu-id="a7050-173">A simple UI for creating projects that offer [support for multiple ASP.NET frameworks](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC, and Web API).</span></span>
- <span data-ttu-id="a7050-174">[ブートス トラップ](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap)レイアウトとテーマのフレームワークで、応答性の高いデザインおよびテーマの適用の機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="a7050-174">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), a layout and theming framework that provides responsive design and theming capabilities.</span></span>
- <span data-ttu-id="a7050-175">[ASP.NET Identity](../../../../identity/index.md)、IIS 以外のソフトウェアをホストしている web を使用したすべての ASP.NET フレームワークと動作で同様に動作する新しい ASP.NET メンバーシップ システムです。</span><span class="sxs-lookup"><span data-stu-id="a7050-175">[ASP.NET Identity](../../../../identity/index.md), a new ASP.NET membership system that works the same in all ASP.NET frameworks and works with web hosting software other than IIS.</span></span>
- <span data-ttu-id="a7050-176">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx)型指定されたオブジェクトを取得し、厳密にデータを操作できる Entity Framework の更新プログラム、データへのアクセスは、非同期的に一時的な接続エラーのエラーを処理し、SQL ステートメントを記録します。</span><span class="sxs-lookup"><span data-stu-id="a7050-176">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), an update to the Entity Framework which allows you retrieve and manipulate data as strongly typed objects, access data asynchronously, handle transient connection faults, and log SQL statements.</span></span>

<span data-ttu-id="a7050-177">ASP.NET 4.5 機能の完全な一覧を参照してください。 [ASP.NET および Web Tools for Visual Studio 2013 リリース ノート](../../../../visual-studio/overview/2013/release-notes.md)します。</span><span class="sxs-lookup"><span data-stu-id="a7050-177">For a complete list of ASP.NET 4.5 features, see [ASP.NET and Web Tools for Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span></span>

### <a name="the-wingtip-toys-sample-application"></a><span data-ttu-id="a7050-178">Wingtip Toys のサンプル アプリケーション</span><span class="sxs-lookup"><span data-stu-id="a7050-178">The Wingtip Toys Sample Application</span></span>

<span data-ttu-id="a7050-179">次のスクリーン ショットでは、このチュートリアル シリーズで作成する ASP.NET Web フォーム アプリケーションの簡易ビューを提供します。</span><span class="sxs-lookup"><span data-stu-id="a7050-179">The following screen shots provide a quick view of the ASP.NET Web forms application that you will create in this tutorial series.</span></span> <span data-ttu-id="a7050-180">Visual Studio Express 2013 for Web からアプリケーションを実行する場合は、次の web のホーム ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a7050-180">When you run the application from Visual Studio Express 2013 for Web, you will see the following web Home page.</span></span>

![Wingtip Toys の既定のページ](introduction-and-overview/_static/image1.png)

<span data-ttu-id="a7050-182">新しいユーザーとして登録したり、既存のユーザーとしてログインできます。</span><span class="sxs-lookup"><span data-stu-id="a7050-182">You can register as a new user, or log in as an existing user.</span></span> <span data-ttu-id="a7050-183">ナビゲーションは、データベースから使用可能な製品を取得することによって各製品カテゴリの上部で提供されます。</span><span class="sxs-lookup"><span data-stu-id="a7050-183">Navigation is provided at the top for each product category by retrieving the available products from the database.</span></span>

<span data-ttu-id="a7050-184">製品リンクを選択すると、すべての利用可能な製品の一覧を表示することができます。</span><span class="sxs-lookup"><span data-stu-id="a7050-184">By selecting the Products link, you will be able to see a list of all available products.</span></span>

![Wingtip Toys の製品](introduction-and-overview/_static/image2.png)

<span data-ttu-id="a7050-186">以下の製品のいずれかを選択して個々 の製品の詳細を確認できます。</span><span class="sxs-lookup"><span data-stu-id="a7050-186">You can also see individual product details by selecting any of the listed products.</span></span>

![Wingtip Toys の製品の詳細](introduction-and-overview/_static/image3.png)

<span data-ttu-id="a7050-188">ユーザーは、登録し、Web フォーム テンプレートの既定の機能を使用してログインできます。</span><span class="sxs-lookup"><span data-stu-id="a7050-188">As a user, you can register and log in using the default functionality of the Web Forms template.</span></span> <span data-ttu-id="a7050-189">このチュートリアルでは、既存の Gmail アカウントを使用してログインする方法も説明します。</span><span class="sxs-lookup"><span data-stu-id="a7050-189">This tutorial also explains how to login using an existing Gmail account.</span></span> <span data-ttu-id="a7050-190">さらに、追加し、データベースから製品を削除するには、管理者としてログインすることができます。</span><span class="sxs-lookup"><span data-stu-id="a7050-190">Additionally, you can login as the administrator to add and remove products from the database.</span></span>

![Wingtip Toys をログインします。](introduction-and-overview/_static/image4.png)

<span data-ttu-id="a7050-192">ユーザー ログインすると、ショッピング カートと PayPal によるチェック アウトする製品を追加できます。</span><span class="sxs-lookup"><span data-stu-id="a7050-192">Once you have logged in as a user, you can add products to the shopping cart and checkout with PayPal.</span></span> <span data-ttu-id="a7050-193">このサンプル アプリケーションが PayPal の開発者のサンド ボックスで機能するように設計することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="a7050-193">Note that this sample application is designed to function with PayPal's developer sandbox.</span></span> <span data-ttu-id="a7050-194">実際の金額のトランザクションは行われません。</span><span class="sxs-lookup"><span data-stu-id="a7050-194">No actual money transaction will take place.</span></span>

![Wingtip Toys のショッピング カート](introduction-and-overview/_static/image5.png)

<span data-ttu-id="a7050-196">PayPal は、アカウント、順序、および支払い情報を確認します。</span><span class="sxs-lookup"><span data-stu-id="a7050-196">PayPal will confirm your account, order, and payment information.</span></span>

![Wingtip Toys の PayPal](introduction-and-overview/_static/image6.png)

<span data-ttu-id="a7050-198">PayPal から返された後、は、確認し、注文を完了できます。</span><span class="sxs-lookup"><span data-stu-id="a7050-198">After returning from PayPal, you can review and complete your order.</span></span>

![Wingtip Toys の注文の確認](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a><span data-ttu-id="a7050-200">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="a7050-200">Prerequisites</span></span>

<span data-ttu-id="a7050-201">開始する前に、コンピューターにインストールされている、次のソフトウェアがあることを確認します。</span><span class="sxs-lookup"><span data-stu-id="a7050-201">Before you start, make sure that you have the following software installed on your computer:</span></span>

- <span data-ttu-id="a7050-202">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs)または[Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)します。</span><span class="sxs-lookup"><span data-stu-id="a7050-202">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) or [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span> <span data-ttu-id="a7050-203">.NET Framework は、自動的にインストールされます。</span><span class="sxs-lookup"><span data-stu-id="a7050-203">The .NET Framework is installed automatically.</span></span>

<span data-ttu-id="a7050-204">このチュートリアル シリーズでは、Web の Microsoft Visual Studio Express 2013 を使用します。</span><span class="sxs-lookup"><span data-stu-id="a7050-204">This tutorial series uses Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="a7050-205">Microsoft Visual Studio Express 2013 for Web、または Microsoft Visual Studio 2013 のいずれかを使用して、このチュートリアル シリーズを完了することができます。</span><span class="sxs-lookup"><span data-stu-id="a7050-205">You can use either Microsoft Visual Studio Express 2013 for Web or Microsoft Visual Studio 2013 to complete this tutorial series.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="a7050-206">Microsoft Visual Studio 2013 および Microsoft Visual Studio Express 2013 for Web は多くの場合、呼びます Visual Studio とこのチュートリアル シリーズです。</span><span class="sxs-lookup"><span data-stu-id="a7050-206">Microsoft Visual Studio 2013 and Microsoft Visual Studio Express 2013 for Web will often be referred to as Visual Studio throughout this tutorial series.</span></span>


<span data-ttu-id="a7050-207">Visual Studio のバージョンがインストールされている、既にある場合は、インストール プロセスが、既存のバージョンの横にあるに Visual Studio 2013 または Microsoft Visual Studio Express 2013 for Web をインストールします。</span><span class="sxs-lookup"><span data-stu-id="a7050-207">If you already have a Visual Studio version installed, the installation process will install Visual Studio 2013 or Microsoft Visual Studio Express 2013 for Web next to the existing version.</span></span> <span data-ttu-id="a7050-208">以前のバージョンで作成したサイトでは、Visual Studio 2013 で開くことし、以前のバージョンで開きます。</span><span class="sxs-lookup"><span data-stu-id="a7050-208">Sites that you created in earlier versions can be opened in Visual Studio 2013 and continue to open in previous versions.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="a7050-209">このチュートリアルでは、選択した、 *Web 開発*設定のコレクションを初めて Visual Studio を起動します。</span><span class="sxs-lookup"><span data-stu-id="a7050-209">This walkthrough assumes that you selected the *Web Development* collection of settings the first time that you started Visual Studio.</span></span> <span data-ttu-id="a7050-210">詳細については、次を参照してください。[方法: Web 開発環境の設定の選択](https://msdn.microsoft.com/library/ff521558.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="a7050-210">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>


## <a name="download-the-sample-application"></a><span data-ttu-id="a7050-211">サンプル アプリケーションをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="a7050-211">Download the Sample Application</span></span>

<span data-ttu-id="a7050-212">前提条件をインストールすると、このチュートリアル シリーズで表示される、新しい Web プロジェクトの作成を開始する準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="a7050-212">After installing the prerequisites, you are ready to begin creating the new Web project that is presented in this tutorial series.</span></span> <span data-ttu-id="a7050-213">たい場合**必要に応じて**このチュートリアル シリーズを作成するサンプル アプリケーションを実行するからダウンロードできますが、MSDN のサンプル サイト。</span><span class="sxs-lookup"><span data-stu-id="a7050-213">If you would like to **optionally** run the sample application that this tutorial series creates, you can download it from the MSDN Samples site.</span></span> <span data-ttu-id="a7050-214">このダウンロードには、次のものが含まれています。</span><span class="sxs-lookup"><span data-stu-id="a7050-214">This download contains the following:</span></span>

- <span data-ttu-id="a7050-215">サンプル アプリケーションは、 *WingtipToys*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="a7050-215">The sample application in the *WingtipToys* folder.</span></span>
- <span data-ttu-id="a7050-216">サンプル アプリケーションの作成に使用されるリソース、 *WingtipToys 資産*フォルダーで、 *WingtipToys*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="a7050-216">The resources used to create the sample application in the *WingtipToys-Assets* folder in the *WingtipToys* folder.</span></span>

#### <a name="download-the-file-from-msdn-samples-site"></a><span data-ttu-id="a7050-217">MSDN のサンプル サイトからファイルをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="a7050-217">Download the file from MSDN Samples site:</span></span>

<span data-ttu-id="a7050-218">[ASP.NET 4.5 Web フォームと Visual Studio 2013 - Wingtip Toys 概要](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)(c#)</span><span class="sxs-lookup"><span data-stu-id="a7050-218">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span> 

<span data-ttu-id="a7050-219">ダウンロードが、 <em>.zip</em>ファイル。</span><span class="sxs-lookup"><span data-stu-id="a7050-219">The download is a <em>.zip</em> file.</span></span> <span data-ttu-id="a7050-220">このチュートリアル シリーズを作成する完成したプロジェクト、検索と選択して、 <em>c#</em>フォルダーで、 <em>.zip</em>ファイル。</span><span class="sxs-lookup"><span data-stu-id="a7050-220">To see the completed project that this tutorial series creates, find and select the <em>C#</em>folder in the <em>.zip</em> file.</span></span> <span data-ttu-id="a7050-221">保存、 <em>c#</em>フォルダー、フォルダーを使用して、Visual Studio 2013 プロジェクトを操作します。</span><span class="sxs-lookup"><span data-stu-id="a7050-221">Save the <em>C#</em> folderto the folder you use to work with Visual Studio 2013 projects.</span></span> <span data-ttu-id="a7050-222">既定では、次に、Visual Studio 2013 のプロジェクト フォルダー示します。</span><span class="sxs-lookup"><span data-stu-id="a7050-222">By default, the Visual Studio 2013 projects folder is the following:</span></span>

<span data-ttu-id="a7050-223"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\Documents\Visual Studio 2013 \projects</strong></span><span class="sxs-lookup"><span data-stu-id="a7050-223"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong></span></span>

<span data-ttu-id="a7050-224">名前を変更、 ***c#*** フォルダー ***WingtipToys***です。</span><span class="sxs-lookup"><span data-stu-id="a7050-224">Rename the ***C#*** folder to ***WingtipToys***.</span></span>

> [!NOTE]
> <span data-ttu-id="a7050-225">という名前のフォルダーが既にある場合*WingtipToys* Projects フォルダーに名前を一時的にその既存のフォルダーの名前変更する前に、 *c#* フォルダー *WingtipToys*です。</span><span class="sxs-lookup"><span data-stu-id="a7050-225">If you already have a folder named *WingtipToys* in your Projects folder, temporarily rename that existing folder before renaming the *C#* folder to *WingtipToys*.</span></span>


<span data-ttu-id="a7050-226">完成したプロジェクトを実行するには、開く、 *WingtipToys*フォルダーをダブルクリックします、 *WingtipToys.sln*ファイル。</span><span class="sxs-lookup"><span data-stu-id="a7050-226">To run the completed project, open the *WingtipToys* folder and double-click the *WingtipToys.sln* file.</span></span> <span data-ttu-id="a7050-227">Visual Studio 2013 には、プロジェクトが開きます。</span><span class="sxs-lookup"><span data-stu-id="a7050-227">Visual Studio 2013 will open the project.</span></span> <span data-ttu-id="a7050-228">次に、右クリックし、 *Default.aspx*ソリューション エクスプ ローラー ウィンドウでファイルを開き、右クリック メニューからブラウザーで表示をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a7050-228">Next, right-click the *Default.aspx* file in the Solution Explorer window and click View In Browser from the right-click menu.</span></span>

### <a name="tutorial-support-and-comments"></a><span data-ttu-id="a7050-229">チュートリアルのサポートとコメント</span><span class="sxs-lookup"><span data-stu-id="a7050-229">Tutorial Support and Comments</span></span>

<span data-ttu-id="a7050-230">付属の Q &AMP; A セクションを使用して、 [ASP.NET 4.5 Web フォームと Visual Studio 2013 - Wingtip Toys 概要](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)質問またはコメントのサンプル (c#)。</span><span class="sxs-lookup"><span data-stu-id="a7050-230">Use the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample for any questions or comments.</span></span>

<span data-ttu-id="a7050-231">このチュートリアル シリーズのコメントは、[ようこそ]、およびあらゆる努力をアカウントの修正または提案のチュートリアルのコメントに用意されている改善点を考慮できるようにこのチュートリアル シリーズが更新されたときにします。</span><span class="sxs-lookup"><span data-stu-id="a7050-231">Comments on this tutorial series are welcome, and when this tutorial series is updated every effort will be made to take into account corrections or suggestions for improvements that are provided in the tutorial comments.</span></span>

<span data-ttu-id="a7050-232">開発中にエラーが発生したとき、または Web サイトが正しく実行されない場合は、エラー メッセージは、問題の原因に複雑な手がかりを与える可能性がありますか、その修正方法については説明があります。</span><span class="sxs-lookup"><span data-stu-id="a7050-232">When an error happens during development, or if the Web site does not run correctly, the error messages may give complex clues to the source of the problem or might not explain how to fix it.</span></span> <span data-ttu-id="a7050-233">一般的な問題のシナリオで役立つするには、使用の[ASP.NET フォーラム](https://forums.asp.net/)またはに付属の Q &AMP; A セクション、 [ASP.NET 4.5 Web フォームと Visual Studio 2013 - Wingtip Toys 概要](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)(C#) サンプル。</span><span class="sxs-lookup"><span data-stu-id="a7050-233">To help you with some common problem scenarios, you can also use the [ASP.NET forums](https://forums.asp.net/) or the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample.</span></span> <span data-ttu-id="a7050-234">エラー メッセージを取得する、または、チュートリアルを進めるときに機能しない、必ず上記の場所を確認してください。</span><span class="sxs-lookup"><span data-stu-id="a7050-234">If you get an error message or something doesn't work as you go through the tutorials, be sure to check the above locations.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a7050-235">次へ</span><span class="sxs-lookup"><span data-stu-id="a7050-235">Next</span></span>](create-the-project.md)
