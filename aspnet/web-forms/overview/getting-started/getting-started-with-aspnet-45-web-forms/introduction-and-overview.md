---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: ASP.NET 4.5 Web フォームと Visual Studio 2013 の概要 |Microsoft ドキュメント
author: Erikre
description: このステップ バイ ステップ チュートリアル シリーズでは、ASP.NET 4.5 と Microsoft Visual Studio の有効期限を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: 6ae398f94c0ac3872601bdc8fd935f6d285793db
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a><span data-ttu-id="e5ec2-103">ASP.NET 4.5 Web フォームと Visual Studio 2013 の概要</span><span class="sxs-lookup"><span data-stu-id="e5ec2-103">Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013</span></span>
====================
<span data-ttu-id="e5ec2-104">によって[Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="e5ec2-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="e5ec2-105">[Wingtip Toys のサンプル プロジェクト (c#) をダウンロード](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)または[電子書籍 (PDF) のダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="e5ec2-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="e5ec2-106">このステップ バイ ステップ チュートリアル シリーズでは、Web 用 ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明します。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-106">This step-by-step tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> [<span data-ttu-id="e5ec2-107">ASP.NET Web フォームのクイズ</span><span class="sxs-lookup"><span data-stu-id="e5ec2-107">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)  
> <span data-ttu-id="e5ec2-108">知識をテストし、ASP.NET Web フォーム Quiz 実行することで重要な概念を補足します。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-108">Test your knowledge and reinforce key concepts by taking the ASP.NET Web Forms Quiz.</span></span> <span data-ttu-id="e5ec2-109">このチュートリアルのシリーズに含まれるコンテンツからこのクイズが用意されました。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-109">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="e5ec2-110">クイズの各質問では、追加のガイダンスへのリンクと共に説明します。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-110">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>


## <a name="introduction"></a><span data-ttu-id="e5ec2-111">はじめに</span><span class="sxs-lookup"><span data-stu-id="e5ec2-111">Introduction</span></span>

<span data-ttu-id="e5ec2-112">この一連のチュートリアルでは、Web と ASP.NET 4.5 の Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションを作成するための手順を説明します。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-112">This series of tutorials guides you through the steps required to create an ASP.NET Web Forms application using Visual Studio Express 2013 for Web and ASP.NET 4.5.</span></span>

<span data-ttu-id="e5ec2-113">作成するアプリケーションの名前が**Wingtip Toys**です。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-113">The application you'll create is named **Wingtip Toys**.</span></span> <span data-ttu-id="e5ec2-114">オンラインの項目を販売しているフロント ストアの web サイトの簡単な例です。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-114">It's a simplified example of a store front web site that sells items online.</span></span> <span data-ttu-id="e5ec2-115">このチュートリアルのシリーズには、ASP.NET 4.5 の新機能が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-115">This tutorial series highlights new features available in ASP.NET 4.5.</span></span>

<span data-ttu-id="e5ec2-116">コメントへようこそ は、に関するご意見に基づいてこのチュートリアルのシリーズを更新するには、あらゆる努力がしましょう。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-116">Comments are welcome, and we'll make every effort to update this tutorial series based on your suggestions.</span></span>

### <a name="download-completed-project"></a><span data-ttu-id="e5ec2-117">ダウンロードが完了したプロジェクト</span><span class="sxs-lookup"><span data-stu-id="e5ec2-117">Download completed project</span></span>

<span data-ttu-id="e5ec2-118">完了したチュートリアルを含む c# プロジェクトをダウンロードすることができます。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-118">You can download a C# project that contains the completed tutorial.</span></span>

- <span data-ttu-id="e5ec2-119">[ASP.NET 4.5 Web フォームと Visual Studio 2013 - Wingtip Toys の概要](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)(c#)</span><span class="sxs-lookup"><span data-stu-id="e5ec2-119">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span>

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a><span data-ttu-id="e5ec2-120">関連する ASP.NET Web フォーム クイズを実行してコンテンツを確認します。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-120">Review the content by taking the related ASP.NET Web Forms quiz</span></span>

<span data-ttu-id="e5ec2-121">このチュートリアルを完了した後、ナレッジをテストし、実行することで重要な概念を補足、 [ASP.NET Web フォーム Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001)です。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-121">After you complete this tutorial, test your knowledge and reinforce key concepts by taking the [ASP.NET Web Forms Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span></span> <span data-ttu-id="e5ec2-122">このチュートリアルのシリーズに含まれるコンテンツからこのクイズが用意されました。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-122">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="e5ec2-123">クイズの各質問では、追加のガイダンスへのリンクと共に説明します。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-123">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>

- [<span data-ttu-id="e5ec2-124">ASP.NET Web フォームのクイズ</span><span class="sxs-lookup"><span data-stu-id="e5ec2-124">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a><span data-ttu-id="e5ec2-125">対象ユーザー</span><span class="sxs-lookup"><span data-stu-id="e5ec2-125">Audience</span></span>

<span data-ttu-id="e5ec2-126">このチュートリアルの一連の対象読者は、ASP.NET Web フォームに追加された新しい経験を積んだ開発者です。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-126">The intended audience of this tutorial series is experienced developers who are new to ASP.NET Web Forms.</span></span> <span data-ttu-id="e5ec2-127">開発者はこのチュートリアル シリーズの目的は、次のスキルを持つ必要があります。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-127">A developer interested in this tutorial series should have the following skills:</span></span>

- <span data-ttu-id="e5ec2-128">経験のあるオブジェクト指向プログラミング (OOP) 言語</span><span class="sxs-lookup"><span data-stu-id="e5ec2-128">Familiar with an object oriented programming (OOP) language</span></span>
- <span data-ttu-id="e5ec2-129">経験のある Web 開発の概念 (HTML、CSS、JavaScript)</span><span class="sxs-lookup"><span data-stu-id="e5ec2-129">Familiar with Web development concepts (HTML, CSS, JavaScript)</span></span>
- <span data-ttu-id="e5ec2-130">リレーショナル データベースの概念を理解しています。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-130">Familiar with relational database concepts</span></span>
- <span data-ttu-id="e5ec2-131">N 層アーキテクチャの概念を理解しています。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-131">Familiar with n-tier architecture concepts</span></span>

<span data-ttu-id="e5ec2-132">上記の領域を確認する場合は、次の内容を確認することを検討してください。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-132">If you are interested in reviewing the areas listed above, consider reviewing the following content:</span></span>

- [<span data-ttu-id="e5ec2-133">Visual c# の概要</span><span class="sxs-lookup"><span data-stu-id="e5ec2-133">Getting Started with Visual C#</span></span>](https://msdn.microsoft.com/library/a72418yk.aspx)
- <span data-ttu-id="e5ec2-134">[Web 開発](https://msdn.microsoft.com/beginner/bb308760.aspx)、 [HTML、CSS、JavaScript、SQL、PHP、JQuery](http://w3schools.com/)</span><span class="sxs-lookup"><span data-stu-id="e5ec2-134">[Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span></span>
- [<span data-ttu-id="e5ec2-135">リレーショナル データベース</span><span class="sxs-lookup"><span data-stu-id="e5ec2-135">Relational database</span></span>](http://en.wikipedia.org/wiki/Relational_database)
- [<span data-ttu-id="e5ec2-136">多層アーキテクチャ</span><span class="sxs-lookup"><span data-stu-id="e5ec2-136">Multitier architecture</span></span>](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a><span data-ttu-id="e5ec2-137">アプリケーションの機能</span><span class="sxs-lookup"><span data-stu-id="e5ec2-137">Application Features</span></span>

<span data-ttu-id="e5ec2-138">この系列に表示される ASP.NET Web フォームの機能は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-138">The ASP.NET Web Form features presented in this series include:</span></span>

- <span data-ttu-id="e5ec2-139">Web アプリケーション プロジェクト (Web サイト プロジェクトではなく)</span><span class="sxs-lookup"><span data-stu-id="e5ec2-139">The Web Application Project (not Web Site Project)</span></span>
- <span data-ttu-id="e5ec2-140">Web フォーム</span><span class="sxs-lookup"><span data-stu-id="e5ec2-140">Web Forms</span></span>
- <span data-ttu-id="e5ec2-141">マスター ページの構成</span><span class="sxs-lookup"><span data-stu-id="e5ec2-141">Master Pages, Configuration</span></span>
- <span data-ttu-id="e5ec2-142">ブートス トラップ</span><span class="sxs-lookup"><span data-stu-id="e5ec2-142">Bootstrap</span></span>
- <span data-ttu-id="e5ec2-143">Entity Framework コードの最初に、LocalDB</span><span class="sxs-lookup"><span data-stu-id="e5ec2-143">Entity Framework Code First, LocalDB</span></span>
- <span data-ttu-id="e5ec2-144">要求の検証</span><span class="sxs-lookup"><span data-stu-id="e5ec2-144">Request Validation</span></span>
- <span data-ttu-id="e5ec2-145">バインディング、データ注釈をモデルし、値のプロバイダー データ コントロールを厳密に型指定</span><span class="sxs-lookup"><span data-stu-id="e5ec2-145">Strongly Typed Data Controls, Model Binding, Data Annotations, and Value Providers</span></span>
- <span data-ttu-id="e5ec2-146">SSL と OAuth</span><span class="sxs-lookup"><span data-stu-id="e5ec2-146">SSL and OAuth</span></span>
- <span data-ttu-id="e5ec2-147">ASP.NET の Id、構成、および承認</span><span class="sxs-lookup"><span data-stu-id="e5ec2-147">ASP.NET Identity, Configuration, and Authorization</span></span>
- <span data-ttu-id="e5ec2-148">控えめな検証</span><span class="sxs-lookup"><span data-stu-id="e5ec2-148">Unobtrusive Validation</span></span>
- <span data-ttu-id="e5ec2-149">ルーティング</span><span class="sxs-lookup"><span data-stu-id="e5ec2-149">Routing</span></span>
- <span data-ttu-id="e5ec2-150">ASP.NET エラー処理</span><span class="sxs-lookup"><span data-stu-id="e5ec2-150">ASP.NET Error Handling</span></span>

### <a name="application-scenarios-and-tasks"></a><span data-ttu-id="e5ec2-151">アプリケーションのシナリオとタスク</span><span class="sxs-lookup"><span data-stu-id="e5ec2-151">Application Scenarios and Tasks</span></span>

<span data-ttu-id="e5ec2-152">この系列に示されているタスクは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-152">Tasks demonstrated in this series include:</span></span>

- <span data-ttu-id="e5ec2-153">新しいプロジェクトを実行して作成し、レビュー</span><span class="sxs-lookup"><span data-stu-id="e5ec2-153">Creating, reviewing and running the new project</span></span>
- <span data-ttu-id="e5ec2-154">データベース構造を作成します。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-154">Creating the database structure</span></span>
- <span data-ttu-id="e5ec2-155">初期化し、データベースのシード</span><span class="sxs-lookup"><span data-stu-id="e5ec2-155">Initializing and seeding the database</span></span>
- <span data-ttu-id="e5ec2-156">スタイル、グラフィックス、およびマスター ページを使用して UI をカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-156">Customizing the UI using styles, graphics and a master page</span></span>
- <span data-ttu-id="e5ec2-157">ページとナビゲーションを追加します。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-157">Adding pages and navigation</span></span>
- <span data-ttu-id="e5ec2-158">メニューの詳細と製品データを表示します。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-158">Displaying menu details and product data</span></span>
- <span data-ttu-id="e5ec2-159">ショッピング カートを作成します。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-159">Creating a shopping cart</span></span>
- <span data-ttu-id="e5ec2-160">追加の SSL と OAuth のサポート</span><span class="sxs-lookup"><span data-stu-id="e5ec2-160">Adding SSL and OAuth support</span></span>
- <span data-ttu-id="e5ec2-161">支払方法を追加します。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-161">Adding a payment method</span></span>
- <span data-ttu-id="e5ec2-162">管理者ロールおよびアプリケーションのユーザーを含む</span><span class="sxs-lookup"><span data-stu-id="e5ec2-162">Including an administrator role and a user to the application</span></span>
- <span data-ttu-id="e5ec2-163">特定のページとフォルダーへのアクセスを制限します。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-163">Restricting access to specific pages and folder</span></span>
- <span data-ttu-id="e5ec2-164">Web アプリケーションへのファイルのアップロード</span><span class="sxs-lookup"><span data-stu-id="e5ec2-164">Uploading a file to the web application</span></span>
- <span data-ttu-id="e5ec2-165">入力の検証を実装します。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-165">Implementing input validation</span></span>
- <span data-ttu-id="e5ec2-166">Web アプリケーションのルートを登録します。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-166">Registering routes for the web application</span></span>
- <span data-ttu-id="e5ec2-167">エラー処理およびエラーのログ記録を実装します。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-167">Implementing error handling and error logging</span></span>

## <a name="overview"></a><span data-ttu-id="e5ec2-168">概要</span><span class="sxs-lookup"><span data-stu-id="e5ec2-168">Overview</span></span>

<span data-ttu-id="e5ec2-169">ASP.NET Web フォームに慣れていない場合、プログラミングの概念に関する知識が右のチュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-169">If you are new to ASP.NET Web Forms but have familiarity with programming concepts, you have the right tutorial.</span></span> <span data-ttu-id="e5ec2-170">ASP.NET Web フォームを使い慣れている場合は、ASP.NET 4.5 の新機能によって、このチュートリアルのシリーズを活用できます。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-170">If you are already familiar with ASP.NET Web Forms, you can benefit from this tutorial series by the new features available in ASP.NET 4.5.</span></span> <span data-ttu-id="e5ec2-171">プログラミングに関する概念および ASP.NET Web フォームに慣れていない場合は、Web フォームで提供される追加のチュートリアルを参照してください[作業の開始](../../../index.md)セクションの ASP.NET Web サイトです。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-171">If you are unfamiliar with programming concepts and ASP.NET Web Forms, see the additional tutorials provided in the Web Forms [Getting Started](../../../index.md) section on the ASP.NET Web site.</span></span>

<span data-ttu-id="e5ec2-172">特定**最新**ASP.NET 4.5 機能がこの Web フォームで一連のチュートリアルについて、次のとおり指定します。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-172">The specific **latest** ASP.NET 4.5 features provided in this Web Forms tutorial series include the following:</span></span>

- <span data-ttu-id="e5ec2-173">作成するための単純な UI がその提供をプロジェクト[の複数の ASP.NET フレームワークをサポートして](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add)(Web フォーム、MVC、および Web API)。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-173">A simple UI for creating projects that offer [support for multiple ASP.NET frameworks](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC, and Web API).</span></span>
- <span data-ttu-id="e5ec2-174">[ブートス トラップ](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap)レイアウトとテーマのフレームワークで、応答性の高いデザインとテーマの機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-174">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), a layout and theming framework that provides responsive design and theming capabilities.</span></span>
- <span data-ttu-id="e5ec2-175">[ASP.NET Identity](../../../../identity/index.md)、新しい ASP.NET メンバーシップ システム web ホスティング IIS 以外のソフトウェアと連携するすべての ASP.NET フレームワークと動作で同じです。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-175">[ASP.NET Identity](../../../../identity/index.md), a new ASP.NET membership system that works the same in all ASP.NET frameworks and works with web hosting software other than IIS.</span></span>
- <span data-ttu-id="e5ec2-176">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx)型指定されたオブジェクトを取得して、厳密にデータを操作するにより、Entity Framework の更新、データへのアクセスは、非同期的に一時的な接続エラーを処理し、SQL ステートメントを記録します。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-176">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), an update to the Entity Framework which allows you retrieve and manipulate data as strongly typed objects, access data asynchronously, handle transient connection faults, and log SQL statements.</span></span>

<span data-ttu-id="e5ec2-177">ASP.NET 4.5 機能の一覧については、次を参照してください。 [ASP.NET および Web Tools for Visual Studio 2013 のリリース ノート](../../../../visual-studio/overview/2013/release-notes.md)です。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-177">For a complete list of ASP.NET 4.5 features, see [ASP.NET and Web Tools for Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span></span>

### <a name="the-wingtip-toys-sample-application"></a><span data-ttu-id="e5ec2-178">Wingtip Toys のサンプル アプリケーション</span><span class="sxs-lookup"><span data-stu-id="e5ec2-178">The Wingtip Toys Sample Application</span></span>

<span data-ttu-id="e5ec2-179">次のスクリーン ショットは、このチュートリアルの系列で作成する ASP.NET Web フォーム アプリケーションの簡易ビューを提供します。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-179">The following screen shots provide a quick view of the ASP.NET Web forms application that you will create in this tutorial series.</span></span> <span data-ttu-id="e5ec2-180">Visual Studio Express 2013 for Web からアプリケーションを実行するときに、次の web のホーム ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-180">When you run the application from Visual Studio Express 2013 for Web, you will see the following web Home page.</span></span>

![Wingtip Toys の既定のページ](introduction-and-overview/_static/image1.png)

<span data-ttu-id="e5ec2-182">新しいユーザーとして登録したり、既存のユーザーとしてログインできます。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-182">You can register as a new user, or log in as an existing user.</span></span> <span data-ttu-id="e5ec2-183">ナビゲーションは、データベースから使用可能な製品を取得することによって各製品カテゴリの最上部で提供されます。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-183">Navigation is provided at the top for each product category by retrieving the available products from the database.</span></span>

<span data-ttu-id="e5ec2-184">製品のリンクを選択すると、使用可能なすべての製品の一覧を表示することができます。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-184">By selecting the Products link, you will be able to see a list of all available products.</span></span>

![Wingtip Toys - 製品](introduction-and-overview/_static/image2.png)

<span data-ttu-id="e5ec2-186">表示されている製品のいずれかを選択して個々 の製品の詳細を表示できます。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-186">You can also see individual product details by selecting any of the listed products.</span></span>

![Wingtip Toys の製品の詳細](introduction-and-overview/_static/image3.png)

<span data-ttu-id="e5ec2-188">ユーザーは、登録し、Web フォーム テンプレートの既定の機能を使用してログインできます。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-188">As a user, you can register and log in using the default functionality of the Web Forms template.</span></span> <span data-ttu-id="e5ec2-189">このチュートリアルでは、gmail などの既存のアカウントを使用してログインする方法についても説明します。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-189">This tutorial also explains how to login using an existing Gmail account.</span></span> <span data-ttu-id="e5ec2-190">さらに、追加し、データベースから製品を削除するには、管理者としてログインすることができます。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-190">Additionally, you can login as the administrator to add and remove products from the database.</span></span>

![Wingtip Toys をログインします。](introduction-and-overview/_static/image4.png)

<span data-ttu-id="e5ec2-192">いったんユーザーとしてログインが、PayPal のチェック アウト、ショッピング カートに製品を追加できます。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-192">Once you have logged in as a user, you can add products to the shopping cart and checkout with PayPal.</span></span> <span data-ttu-id="e5ec2-193">このサンプル アプリケーションが PayPal の開発者のサンド ボックスで動作するように設計することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-193">Note that this sample application is designed to function with PayPal's developer sandbox.</span></span> <span data-ttu-id="e5ec2-194">実際の money トランザクションは行われません。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-194">No actual money transaction will take place.</span></span>

![Wingtip Toys のショッピング カート](introduction-and-overview/_static/image5.png)

<span data-ttu-id="e5ec2-196">PayPal は、アカウント、順序、お支払い情報を確認します。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-196">PayPal will confirm your account, order, and payment information.</span></span>

![Wingtip Toys の PayPal](introduction-and-overview/_static/image6.png)

<span data-ttu-id="e5ec2-198">PayPal から返された後、は、確認し、注文を完了できます。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-198">After returning from PayPal, you can review and complete your order.</span></span>

![Wingtip Toys の注文の確認](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a><span data-ttu-id="e5ec2-200">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="e5ec2-200">Prerequisites</span></span>

<span data-ttu-id="e5ec2-201">開始する前に、コンピューターにインストールされている次のソフトウェアがあることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-201">Before you start, make sure that you have the following software installed on your computer:</span></span>

- <span data-ttu-id="e5ec2-202">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs)または[Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)です。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-202">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) or [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span> <span data-ttu-id="e5ec2-203">.NET Framework は、自動的にインストールされます。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-203">The .NET Framework is installed automatically.</span></span>

<span data-ttu-id="e5ec2-204">この一連のチュートリアルについては、Web の Microsoft Visual Studio Express 2013 を使用します。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-204">This tutorial series uses Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="e5ec2-205">このチュートリアルのシリーズを完了するのに、Microsoft Visual Studio Express 2013 for Web か、Microsoft Visual Studio 2013 を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-205">You can use either Microsoft Visual Studio Express 2013 for Web or Microsoft Visual Studio 2013 to complete this tutorial series.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="e5ec2-206">Microsoft Visual Studio 2013 および Microsoft Visual Studio Express 2013 for Web が多くの場合、参照されます Visual Studio としてこのチュートリアルの系列全体です。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-206">Microsoft Visual Studio 2013 and Microsoft Visual Studio Express 2013 for Web will often be referred to as Visual Studio throughout this tutorial series.</span></span>


<span data-ttu-id="e5ec2-207">インストールされている Visual Studio のバージョンが既にあるを場合は、インストール プロセスが、既存のバージョンの横にあるに Visual Studio 2013 または Microsoft Visual Studio Express 2013 for Web をインストールします。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-207">If you already have a Visual Studio version installed, the installation process will install Visual Studio 2013 or Microsoft Visual Studio Express 2013 for Web next to the existing version.</span></span> <span data-ttu-id="e5ec2-208">以前のバージョンで作成したサイトは、Visual Studio 2013 で開くことができ、以前のバージョンを開くに進みます。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-208">Sites that you created in earlier versions can be opened in Visual Studio 2013 and continue to open in previous versions.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="e5ec2-209">このチュートリアルでは、選択されている、 *Web 開発*設定のコレクションを初めて Visual Studio を起動します。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-209">This walkthrough assumes that you selected the *Web Development* collection of settings the first time that you started Visual Studio.</span></span> <span data-ttu-id="e5ec2-210">詳細については、次を参照してください。[する方法: Web 開発環境設定の選択](https://msdn.microsoft.com/library/ff521558.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-210">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>


## <a name="download-the-sample-application"></a><span data-ttu-id="e5ec2-211">サンプル アプリケーションをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-211">Download the Sample Application</span></span>

<span data-ttu-id="e5ec2-212">前提条件をインストールすると、このチュートリアルの系列には、表示される新しい Web プロジェクトの作成を開始する準備ができたらです。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-212">After installing the prerequisites, you are ready to begin creating the new Web project that is presented in this tutorial series.</span></span> <span data-ttu-id="e5ec2-213">たい場合**必要に応じて**このチュートリアルのシリーズを作成するサンプル アプリケーションを実行する、サイトからダウンロードできる、MSDN Samples です。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-213">If you would like to **optionally** run the sample application that this tutorial series creates, you can download it from the MSDN Samples site.</span></span> <span data-ttu-id="e5ec2-214">このダウンロードには、次のものが含まれています。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-214">This download contains the following:</span></span>

- <span data-ttu-id="e5ec2-215">サンプル アプリケーション、 *WingtipToys*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-215">The sample application in the *WingtipToys* folder.</span></span>
- <span data-ttu-id="e5ec2-216">サンプル アプリケーションの作成に使用されるリソース、 *WingtipToys 資産*内のフォルダー、 *WingtipToys*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-216">The resources used to create the sample application in the *WingtipToys-Assets* folder in the *WingtipToys* folder.</span></span>

#### <a name="download-the-file-from-msdn-samples-site"></a><span data-ttu-id="e5ec2-217">MSDN サンプルのサイトからファイルをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-217">Download the file from MSDN Samples site:</span></span>

<span data-ttu-id="e5ec2-218">[ASP.NET 4.5 Web フォームと Visual Studio 2013 - Wingtip Toys の概要](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)(c#)</span><span class="sxs-lookup"><span data-stu-id="e5ec2-218">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span> 

<span data-ttu-id="e5ec2-219">ダウンロードが、 *.zip*ファイル。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-219">The download is a *.zip* file.</span></span> <span data-ttu-id="e5ec2-220">このチュートリアルのシリーズを作成する、完成したプロジェクトで検索と選択を表示する、 *c#*内のフォルダー、 *.zip*ファイル。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-220">To see the completed project that this tutorial series creates, find and select the *C#*folder in the *.zip* file.</span></span> <span data-ttu-id="e5ec2-221">保存、 *c#*フォルダーに Visual Studio 2013 プロジェクトの作業を使用するフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-221">Save the *C#* folderto the folder you use to work with Visual Studio 2013 projects.</span></span> <span data-ttu-id="e5ec2-222">既定では、Visual Studio 2013 プロジェクト フォルダーは次のよう</span><span class="sxs-lookup"><span data-stu-id="e5ec2-222">By default, the Visual Studio 2013 projects folder is the following:</span></span>

<span data-ttu-id="e5ec2-223">**C:\Users\*****&lt;username&gt;*****\Documents\Visual Studio 2013\Projects**</span><span class="sxs-lookup"><span data-stu-id="e5ec2-223">**C:\Users\*****&lt;username&gt;*****\Documents\Visual Studio 2013\Projects**</span></span>

<span data-ttu-id="e5ec2-224">名前を変更、 ***c#***フォルダー ***WingtipToys***です。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-224">Rename the ***C#*** folder to ***WingtipToys***.</span></span>

> [!NOTE]
> <span data-ttu-id="e5ec2-225">という名前のフォルダーが既にある場合*WingtipToys* Projects フォルダーに名前を一時的にその既存のフォルダーの名前変更する前に、 *c#*フォルダー *WingtipToys*です。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-225">If you already have a folder named *WingtipToys* in your Projects folder, temporarily rename that existing folder before renaming the *C#* folder to *WingtipToys*.</span></span>


<span data-ttu-id="e5ec2-226">実行するには、完成したプロジェクトを開く、 *WingtipToys*フォルダーをダブルクリック、 *WingtipToys.sln*ファイル。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-226">To run the completed project, open the *WingtipToys* folder and double-click the *WingtipToys.sln* file.</span></span> <span data-ttu-id="e5ec2-227">Visual Studio 2013 には、プロジェクトが開きます。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-227">Visual Studio 2013 will open the project.</span></span> <span data-ttu-id="e5ec2-228">次を右クリックし、 *Default.aspx*ソリューション エクスプ ローラー ウィンドウでファイルを右クリック メニューからブラウザーで表示をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-228">Next, right-click the *Default.aspx* file in the Solution Explorer window and click View In Browser from the right-click menu.</span></span>

### <a name="tutorial-support-and-comments"></a><span data-ttu-id="e5ec2-229">チュートリアルのサポートとコメント</span><span class="sxs-lookup"><span data-stu-id="e5ec2-229">Tutorial Support and Comments</span></span>

<span data-ttu-id="e5ec2-230">含まれている Q &AMP; A セクションを使用して、 [ASP.NET 4.5 Web フォームと Visual Studio 2013 - Wingtip Toys の概要](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)質問またはコメントのサンプル (C# の場合)。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-230">Use the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample for any questions or comments.</span></span>

<span data-ttu-id="e5ec2-231">このチュートリアル シリーズのコメントへようこそ は、このチュートリアルのシリーズが更新されたときにすべての作業量になりますチュートリアルのコメントに用意された機能強化に関するアカウントの修正または提案を考慮します。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-231">Comments on this tutorial series are welcome, and when this tutorial series is updated every effort will be made to take into account corrections or suggestions for improvements that are provided in the tutorial comments.</span></span>

<span data-ttu-id="e5ec2-232">開発中は、エラーの発生時、または Web サイトが正しく実行されない場合は、エラー メッセージは、問題の原因に複雑な手掛かりを与える可能性があります。 または、その修正方法については説明があります。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-232">When an error happens during development, or if the Web site does not run correctly, the error messages may give complex clues to the source of the problem or might not explain how to fix it.</span></span> <span data-ttu-id="e5ec2-233">問題の一般的なシナリオで役立つ、使用することも、 [ASP.NET フォーラム](https://forums.asp.net/)または Q &AMP; A セクションに含まれている、 [ASP.NET 4.5 Web フォームと Visual Studio 2013 - Wingtip Toys の概要](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)(C#) サンプルです。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-233">To help you with some common problem scenarios, you can also use the [ASP.NET forums](https://forums.asp.net/) or the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample.</span></span> <span data-ttu-id="e5ec2-234">エラー メッセージを取得する、または、チュートリアルを通過するとおりに機能しない、上記の場所を確認することを確認します。</span><span class="sxs-lookup"><span data-stu-id="e5ec2-234">If you get an error message or something doesn't work as you go through the tutorials, be sure to check the above locations.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="e5ec2-235">次へ</span><span class="sxs-lookup"><span data-stu-id="e5ec2-235">Next</span></span>](create-the-project.md)
