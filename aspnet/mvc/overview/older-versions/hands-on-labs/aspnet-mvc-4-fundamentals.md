---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: ASP.NET MVC 4 基礎 |Microsoft ドキュメント
author: rick-anderson
description: このハンズオン ラボは、MVC (モデル ビュー コント ローラー) Music Store、チュートリアルのアプリケーションを紹介し、ASP.NET MV を使用する方法の詳細な手順について説明しますに基づきます。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: 225dff4663e0e556cfb8966f1078848b4c2b47a5
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/18/2018
---
# <a name="aspnet-mvc-4-fundamentals"></a><span data-ttu-id="204f5-103">ASP.NET MVC 4 の基礎</span><span class="sxs-lookup"><span data-stu-id="204f5-103">ASP.NET MVC 4 Fundamentals</span></span>

<span data-ttu-id="204f5-104">によって[Web キャンプ チーム](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="204f5-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="204f5-105">Web キャンプ トレーニング キットをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="204f5-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="204f5-106">このハンズオン ラボは、MVC (モデル ビュー コント ローラー) Music Store、紹介し、ASP.NET MVC と Visual Studio を使用する方法の詳細な手順について説明しますチュートリアル アプリケーションに基づいています。</span><span class="sxs-lookup"><span data-stu-id="204f5-106">This Hands-On Lab is based on MVC (Model View Controller) Music Store, a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio.</span></span> <span data-ttu-id="204f5-107">ラボでは、わかりやすくするためを学習しますをこれらのテクノロジを同時に使用する、まだ power です。</span><span class="sxs-lookup"><span data-stu-id="204f5-107">Throughout the lab you will learn the simplicity, yet power of using these technologies together.</span></span> <span data-ttu-id="204f5-108">単純なアプリケーションを開始し、完全に機能 ASP.NET MVC 4 Web アプリケーションになるまで、ビルドします。</span><span class="sxs-lookup"><span data-stu-id="204f5-108">You will start with a simple application and will build it until you have a fully functional ASP.NET MVC 4 Web Application.</span></span>

<span data-ttu-id="204f5-109">このラボは、ASP.NET MVC 4 で動作します。</span><span class="sxs-lookup"><span data-stu-id="204f5-109">This Lab works with ASP.NET MVC 4.</span></span>

<span data-ttu-id="204f5-110">ASP.NET MVC 3 のバージョンのチュートリアル アプリケーションを調査したい場合でを検索[MVC Music Store](https://github.com/evilDave/MVC-Music-Store)です。</span><span class="sxs-lookup"><span data-stu-id="204f5-110">If you wish to explore the ASP.NET MVC 3 version of the tutorial application, you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="204f5-111">このハンズオン ラボでは、HTML および JavaScript などの Web 開発テクノロジで、開発者の経験があることを前提としています。</span><span class="sxs-lookup"><span data-stu-id="204f5-111">This Hands-On Lab assumes that the developer has experience in Web development technologies, such as HTML and JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="204f5-112">すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[Microsoft の Web/WebCampTrainingKit リリース](https://aka.ms/webcamps-training-kit)です。</span><span class="sxs-lookup"><span data-stu-id="204f5-112">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="204f5-113">このラボに固有のプロジェクトは[ASP.NET MVC 4 基礎](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals)です。</span><span class="sxs-lookup"><span data-stu-id="204f5-113">The project specific to this lab is available at [ASP.NET MVC 4 Fundamentals](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span></span>

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a><span data-ttu-id="204f5-114">音楽ストア アプリケーション</span><span class="sxs-lookup"><span data-stu-id="204f5-114">The Music Store application</span></span>

<span data-ttu-id="204f5-115">この実習でビルドされる音楽ストアの web アプリケーションに 3 つの主要な部分は構成されています: ショッピング、チェック アウト、および管理します。</span><span class="sxs-lookup"><span data-stu-id="204f5-115">The Music Store web application that will be built throughout this Lab comprises three main parts: shopping, checkout, and administration.</span></span> <span data-ttu-id="204f5-116">閲覧者は、ジャンル アルバムを参照、カートにアルバムを追加、選択内容を確認および最後にログインし、注文を完了にチェック アウトを続行することができます。</span><span class="sxs-lookup"><span data-stu-id="204f5-116">Visitors will be able to browse albums by genre, add albums to their cart, review their selection and finally proceed to checkout to login and complete the order.</span></span> <span data-ttu-id="204f5-117">さらに、ストア管理者は、使用可能なアルバムとその主要なプロパティを管理することができます。</span><span class="sxs-lookup"><span data-stu-id="204f5-117">Additionally, store administrators will be able to manage the available albums as well as their main properties.</span></span>

<span data-ttu-id="204f5-118">![Music Store 画面](aspnet-mvc-4-fundamentals/_static/image1.png "Music Store 画面")</span><span class="sxs-lookup"><span data-stu-id="204f5-118">![Music Store screens](aspnet-mvc-4-fundamentals/_static/image1.png "Music Store screens")</span></span>

<span data-ttu-id="204f5-119">*Music Store 画面*</span><span class="sxs-lookup"><span data-stu-id="204f5-119">*Music Store screens*</span></span>

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a><span data-ttu-id="204f5-120">ASP.NET MVC 4 Essentials</span><span class="sxs-lookup"><span data-stu-id="204f5-120">ASP.NET MVC 4 Essentials</span></span>

<span data-ttu-id="204f5-121">使用して音楽ストア アプリケーションがビルドされます**モデル ビュー コント ローラー (MVC)** アプリケーションを次の 3 つの主なコンポーネントを分離するアーキテクチャ パターン。</span><span class="sxs-lookup"><span data-stu-id="204f5-121">Music Store application will be built using **Model View Controller (MVC)**, an architectural pattern that separates an application into three main components:</span></span>

- <span data-ttu-id="204f5-122">**モデル**: モデル オブジェクトは、ドメイン ロジックを実装するアプリケーションの部分です。</span><span class="sxs-lookup"><span data-stu-id="204f5-122">**Models**: Model objects are the parts of the application that implement the domain logic.</span></span> <span data-ttu-id="204f5-123">多くの場合、モデル オブジェクトも取得し、モデルの状態をデータベースに格納します。</span><span class="sxs-lookup"><span data-stu-id="204f5-123">Often, model objects also retrieve and store model state in a database.</span></span>
- <span data-ttu-id="204f5-124">**ビュー:** ビューは、アプリケーションのユーザー インターフェイス (UI) を表示するコンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="204f5-124">**Views:** Views are the components that display the application's user interface (UI).</span></span> <span data-ttu-id="204f5-125">通常、この UI は、モデル データから作成されます。</span><span class="sxs-lookup"><span data-stu-id="204f5-125">Typically, this UI is created from the model data.</span></span> <span data-ttu-id="204f5-126">例には、テキスト ボックスやアルバム オブジェクトの現在の状態に基づいて、ボックスの一覧を表示するアルバムの編集ビューがあります。</span><span class="sxs-lookup"><span data-stu-id="204f5-126">An example would be the edit view of Albums that displays text boxes and a drop-down list based on the current state of an Album object.</span></span>
- <span data-ttu-id="204f5-127">**コント ローラー:** コント ローラーは、コンポーネントのユーザーとの対話を処理して、モデルを操作し、最終的には、UI をレンダリングするビューを選択します。</span><span class="sxs-lookup"><span data-stu-id="204f5-127">**Controllers:** Controllers are the components that handle user interaction, manipulate the model, and ultimately select a view to render the UI.</span></span> <span data-ttu-id="204f5-128">MVC アプリケーションでは、ビューは情報のみを表示し、コントローラーがユーザーの入力と操作を処理して応答します。</span><span class="sxs-lookup"><span data-stu-id="204f5-128">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="204f5-129">MVC パターンでは、これらの要素間の疎結合を提供しつつ、アプリケーション (入力ロジック、ビジネス ロジック、および UI ロジック) のさまざまな側面を分離するアプリケーションを作成するのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="204f5-129">The MVC pattern helps you to create applications that separate the different aspects of the application (input logic, business logic, and UI logic), while providing a loose coupling between these elements.</span></span> <span data-ttu-id="204f5-130">この分離では、一度に実装の 1 つの側面に注目することができます、アプリケーションをビルドするときに、複雑さを管理できます。</span><span class="sxs-lookup"><span data-stu-id="204f5-130">This separation helps you manage complexity when you build an application, as it allows you to focus on one aspect of the implementation at a time.</span></span> <span data-ttu-id="204f5-131">さらに、MVC パターンしやすいアプリケーションを作成するためのテスト駆動開発 (TDD) の使用を促進もアプリケーションをテストします。</span><span class="sxs-lookup"><span data-stu-id="204f5-131">In addition, the MVC pattern makes it easy to test applications, also encouraging the use of test-driven development (TDD) for creating applications.</span></span>

<span data-ttu-id="204f5-132">**ASP.NET MVC** framework では、ASP.NET MVC ベースの Web アプリケーションを作成するための代わりに、ASP.NET Web フォーム パターンが用意されています。</span><span class="sxs-lookup"><span data-stu-id="204f5-132">The **ASP.NET MVC** framework provides an alternative to the ASP.NET Web Forms pattern for creating ASP.NET MVC-based Web applications.</span></span> <span data-ttu-id="204f5-133">**ASP.NET MVC**フレームワークは、軽量で高テスト可能なプレゼンテーション フレームワークを (Web フォーム ベースのアプリケーションと同様) と統合されたマスター ページなど、メンバーシップに基づく既存の ASP.NET 機能認証できるため、中核となる .NET framework のすべての機能を取得します。</span><span class="sxs-lookup"><span data-stu-id="204f5-133">The **ASP.NET MVC** framework is a lightweight, highly testable presentation framework that (as with Web-forms-based applications) is integrated with existing ASP.NET features, such as master pages and membership-based authentication so you get all the power of the core .NET framework.</span></span> <span data-ttu-id="204f5-134">これは、既に使用しているすべてのライブラリがも ASP.NET MVC 4 で使用できるため ASP.NET Web フォームに慣れている既に場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="204f5-134">This is useful if you are already familiar with ASP.NET Web Forms because all the libraries that you already use are available in ASP.NET MVC 4 as well.</span></span>

<span data-ttu-id="204f5-135">さらに、MVC アプリケーションは、次の 3 つの主要なコンポーネント間の疎結合では、並行開発も促進されます。</span><span class="sxs-lookup"><span data-stu-id="204f5-135">In addition, the loose coupling between the three main components of an MVC application also promotes parallel development.</span></span> <span data-ttu-id="204f5-136">たとえば、1 人の開発者は、ビューで作業できる、2 番目の開発者、コント ローラー ロジックで作業でき、3 番目の開発者は、モデル内のビジネス ロジックに集中できます。</span><span class="sxs-lookup"><span data-stu-id="204f5-136">For instance, one developer can work on the view, a second developer can work on the controller logic, and a third developer can focus on the business logic in the model.</span></span>

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="204f5-137">目的</span><span class="sxs-lookup"><span data-stu-id="204f5-137">Objectives</span></span>

<span data-ttu-id="204f5-138">このハンズオン ラボでは学習する方法。</span><span class="sxs-lookup"><span data-stu-id="204f5-138">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="204f5-139">音楽ストア アプリケーションのチュートリアルに基づいて最初から ASP.NET MVC アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="204f5-139">Create an ASP.NET MVC application from scratch based on the Music Store Application tutorial</span></span>
- <span data-ttu-id="204f5-140">その主な機能を参照するため、サイトのホーム ページに Url を処理するコント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="204f5-140">Add Controllers to handle URLs to the Home page of the site and for browsing its main functionality</span></span>
- <span data-ttu-id="204f5-141">スタイルと共に表示される内容をカスタマイズするビューを追加します。</span><span class="sxs-lookup"><span data-stu-id="204f5-141">Add a View to customize the content displayed along with its style</span></span>
- <span data-ttu-id="204f5-142">含めるし、データとドメインのロジックを管理するモデル クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="204f5-142">Add Model classes to contain and manage data and domain logic</span></span>
- <span data-ttu-id="204f5-143">ビュー モデルのパターンを使用して、コント ローラーのアクションからテンプレートを表示する情報を渡す</span><span class="sxs-lookup"><span data-stu-id="204f5-143">Use View Model pattern to pass information from controller actions to the view templates</span></span>
- <span data-ttu-id="204f5-144">インターネット アプリケーションの ASP.NET MVC 4 の新しいテンプレートを調査します。</span><span class="sxs-lookup"><span data-stu-id="204f5-144">Explore the ASP.NET MVC 4 new template for internet applications</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="204f5-145">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="204f5-145">Prerequisites</span></span>

<span data-ttu-id="204f5-146">このラボを完成させるのには、次の項目が必要です。</span><span class="sxs-lookup"><span data-stu-id="204f5-146">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="204f5-147">[Visual Studio 2012 Express for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (読み取り[付録 A](#AppendixA)をインストールする方法について)</span><span class="sxs-lookup"><span data-stu-id="204f5-147">[Visual Studio 2012 Express for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (read [Appendix A](#AppendixA) for instructions on how to install it)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="204f5-148">セットアップ</span><span class="sxs-lookup"><span data-stu-id="204f5-148">Setup</span></span>

<span data-ttu-id="204f5-149">**コード スニペットをインストールします。**</span><span class="sxs-lookup"><span data-stu-id="204f5-149">**Installing Code Snippets**</span></span>

<span data-ttu-id="204f5-150">便宜上、このラボに沿ったを管理するコードの多くは、Visual Studio のコード スニペットとして利用できます。</span><span class="sxs-lookup"><span data-stu-id="204f5-150">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="204f5-151">実行のコード スニペットをインストールする **.\Source\Setup\CodeSnippets.vsi**ファイル。</span><span class="sxs-lookup"><span data-stu-id="204f5-151">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="204f5-152">このドキュメントの付録を参照することができます、Visual Studio のコード スニペットとその使用方法を学習するに慣れていない場合&quot;[付録 c: を使用してコード スニペット](#AppendixC)&quot;です。</span><span class="sxs-lookup"><span data-stu-id="204f5-152">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="204f5-153">演習</span><span class="sxs-lookup"><span data-stu-id="204f5-153">Exercises</span></span>

<span data-ttu-id="204f5-154">次の演習では、このハンズオン ラボから成ります。</span><span class="sxs-lookup"><span data-stu-id="204f5-154">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="204f5-155">手順 1: MusicStore ASP.NET MVC Web アプリケーション プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="204f5-155">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>](#Exercise1)
2. [<span data-ttu-id="204f5-156">手順 2: コント ローラーの作成</span><span class="sxs-lookup"><span data-stu-id="204f5-156">Exercise 2: Creating a Controller</span></span>](#Exercise2)
3. [<span data-ttu-id="204f5-157">手順 3: コント ローラーにパラメーターを渡す</span><span class="sxs-lookup"><span data-stu-id="204f5-157">Exercise 3: Passing parameters to a Controller</span></span>](#Exercise3)
4. [<span data-ttu-id="204f5-158">手順 4: ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="204f5-158">Exercise 4: Creating a View</span></span>](#Exercise4)
5. [<span data-ttu-id="204f5-159">手順 5: ビュー モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="204f5-159">Exercise 5: Creating a View Model</span></span>](#Exercise5)
6. [<span data-ttu-id="204f5-160">手順 6: ビュー内のパラメーターの使用</span><span class="sxs-lookup"><span data-stu-id="204f5-160">Exercise 6: Using parameters in View</span></span>](#Exercise6)
7. [<span data-ttu-id="204f5-161">手順 7: ASP.NET MVC 4 の新しいテンプレートの周囲膝</span><span class="sxs-lookup"><span data-stu-id="204f5-161">Exercise 7: A lap around ASP.NET MVC 4 New Template</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="204f5-162">各演習が組み込まれた、**終了**演習を完了した後に取得する必要があります、結果として得られるソリューションを含むフォルダー。</span><span class="sxs-lookup"><span data-stu-id="204f5-162">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="204f5-163">演習では操作のヘルプを参照する必要がある場合は、このソリューションをガイドとして使用できます。</span><span class="sxs-lookup"><span data-stu-id="204f5-163">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="204f5-164">この演習を完了する時間を推定: **60 分**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-164">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a><span data-ttu-id="204f5-165">手順 1: MusicStore ASP.NET MVC Web アプリケーション プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="204f5-165">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>

<span data-ttu-id="204f5-166">この演習では、Visual Studio 2012 Express でのメイン フォルダーの組織と、Web、ASP.NET MVC アプリケーションを作成する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="204f5-166">In this exercise, you will learn how to create an ASP.NET MVC application in Visual Studio 2012 Express for Web as well as its main folder organization.</span></span> <span data-ttu-id="204f5-167">さらに、新しいコント ローラーを追加し、アプリケーションのホーム ページで、単純な文字列を表示する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="204f5-167">Additionally, you will learn how to add a new Controller and make it display a simple string in the application's home page.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a><span data-ttu-id="204f5-168">タスク 1 - ASP.NET MVC Web アプリケーション プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="204f5-168">Task 1 - Creating the ASP.NET MVC Web Application Project</span></span>

1. <span data-ttu-id="204f5-169">このタスクでは、Visual Studio の MVC テンプレートを使用して、空の ASP.NET MVC アプリケーション プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="204f5-169">In this task, you will create an empty ASP.NET MVC application project using the MVC Visual Studio template.</span></span> <span data-ttu-id="204f5-170">開始**VS Express for Web**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-170">Start **VS Express for Web**.</span></span>
2. <span data-ttu-id="204f5-171">**[ファイル]** メニューの **[新しいプロジェクト]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="204f5-171">On the **File** menu, click **New Project**.</span></span>
3. <span data-ttu-id="204f5-172">**新しいプロジェクト**ダイアログ ボックスの 、 **ASP.NET MVC 4 Web Application** 、プロジェクトの種類の下にある**Visual C# の場合、** **Web**テンプレート一覧です。</span><span class="sxs-lookup"><span data-stu-id="204f5-172">In the **New Project** dialog box select the **ASP.NET MVC 4 Web Application** project type, located under **Visual C#,** **Web** template list.</span></span>
4. <span data-ttu-id="204f5-173">変更、**名前**に*MvcMusicStore*です。</span><span class="sxs-lookup"><span data-stu-id="204f5-173">Change the **Name** to *MvcMusicStore*.</span></span>
5. <span data-ttu-id="204f5-174">新しいソリューションの場所を設定**開始**例については、この手順のソース フォルダー内のフォルダー **[、HOL PATH] \Source\Ex01-CreatingMusicStoreProject\Begin**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-174">Set the location of the solution inside a new **Begin** folder in this Exercise's Source folder, for example **[YOUR-HOL-PATH]\Source\Ex01-CreatingMusicStoreProject\Begin**.</span></span> <span data-ttu-id="204f5-175">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="204f5-175">Click **OK**.</span></span>

    <span data-ttu-id="204f5-176">![[新しいプロジェクト] ダイアログ ボックスを作成する](aspnet-mvc-4-fundamentals/_static/image2.png "新しいプロジェクト ダイアログ ボックスを作成します。")</span><span class="sxs-lookup"><span data-stu-id="204f5-176">![Create New Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image2.png "Create New Project Dialog Box")</span></span>

    <span data-ttu-id="204f5-177">*[新しいプロジェクト] ダイアログ ボックスを作成します。*</span><span class="sxs-lookup"><span data-stu-id="204f5-177">*Create New Project Dialog Box*</span></span>
6. <span data-ttu-id="204f5-178">**新しい ASP.NET MVC 4 プロジェクト**ダイアログ ボックスの 、**基本**テンプレートことを確認し、**ビュー エンジン**が選択されている**Razor**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-178">In the **New ASP.NET MVC 4 Project** dialog box select the **Basic** template and make sure that the **View engine** selected is **Razor**.</span></span> <span data-ttu-id="204f5-179">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="204f5-179">Click **OK**.</span></span>

    <span data-ttu-id="204f5-180">![新しい ASP.NET MVC 4 プロジェクト ダイアログ ボックス](aspnet-mvc-4-fundamentals/_static/image3.png "新しい ASP.NET MVC 4 プロジェクト ダイアログ ボックス")</span><span class="sxs-lookup"><span data-stu-id="204f5-180">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image3.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="204f5-181">*新しい ASP.NET MVC 4 プロジェクト ダイアログ ボックス*</span><span class="sxs-lookup"><span data-stu-id="204f5-181">*New ASP.NET MVC 4 Project Dialog Box*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a><span data-ttu-id="204f5-182">タスク 2 - ソリューションの構造を調べる</span><span class="sxs-lookup"><span data-stu-id="204f5-182">Task 2 - Exploring the Solution Structure</span></span>

<span data-ttu-id="204f5-183">ASP.NET MVC フレームワークには、MVC パターンをサポートする Web アプリケーションを作成するための Visual Studio プロジェクト テンプレートが含まれています。</span><span class="sxs-lookup"><span data-stu-id="204f5-183">The ASP.NET MVC framework includes a Visual Studio project template that helps you create Web applications supporting the MVC pattern.</span></span> <span data-ttu-id="204f5-184">このテンプレートは、必要なフォルダー、項目テンプレート、および構成ファイル エントリを新しい ASP.NET MVC Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="204f5-184">This template creates a new ASP.NET MVC Web application with the required folders, item templates, and configuration-file entries.</span></span>

<span data-ttu-id="204f5-185">このタスクでは、関連する要素を理解するソリューションの構造とそのリレーションシップを調べます。</span><span class="sxs-lookup"><span data-stu-id="204f5-185">In this task, you will examine the solution structure to understand the elements that are involved and their relationships.</span></span> <span data-ttu-id="204f5-186">既定では、ASP.NET MVC フレームワークを使用するため、すべての ASP.NET MVC アプリケーションで、次のフォルダーが含まれて、&quot;設定よりも規約&quot;アプローチでは、し、いくつかの既定の条件がフォルダーの名前に基づく規則。</span><span class="sxs-lookup"><span data-stu-id="204f5-186">The following folders are included in all the ASP.NET MVC application because the ASP.NET MVC framework by default uses a &quot;convention over configuration&quot; approach, and makes some default assumptions based on folder naming conventions.</span></span>

1. <span data-ttu-id="204f5-187">プロジェクトを作成した後は、右側にあるソリューション エクスプ ローラーで作成されているフォルダー構造を確認します。</span><span class="sxs-lookup"><span data-stu-id="204f5-187">Once the project is created, review the folder structure that has been created in the Solution Explorer on the right side:</span></span>

    <span data-ttu-id="204f5-188">![ソリューション エクスプ ローラーでの ASP.NET MVC フォルダー構造](aspnet-mvc-4-fundamentals/_static/image4.png "ソリューション エクスプ ローラーでの ASP.NET MVC フォルダー構造")</span><span class="sxs-lookup"><span data-stu-id="204f5-188">![ASP.NET MVC Folder structure in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image4.png "ASP.NET MVC Folder structure in Solution Explorer")</span></span>

    <span data-ttu-id="204f5-189">*ソリューション エクスプ ローラーでの ASP.NET MVC フォルダー構造*</span><span class="sxs-lookup"><span data-stu-id="204f5-189">*ASP.NET MVC Folder structure in Solution Explorer*</span></span>

   1. <span data-ttu-id="204f5-190">**コント ローラー**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-190">**Controllers**.</span></span> <span data-ttu-id="204f5-191">このフォルダーは、コント ローラー クラスに格納されます。</span><span class="sxs-lookup"><span data-stu-id="204f5-191">This folder will contain the controller classes.</span></span> <span data-ttu-id="204f5-192">MVC ベース アプリケーションでコント ローラーは、エンド ユーザーとの対話を処理、モデルを操作および最終的には、UI をレンダリングするビューを選択するためです。</span><span class="sxs-lookup"><span data-stu-id="204f5-192">In an MVC based application, controllers are responsible for handling end user interaction, manipulating the model, and ultimately choosing a view to render the UI.</span></span>

       > [!NOTE]
       > <span data-ttu-id="204f5-193">MVC フレームワークには、末尾にはすべてのコント ローラーの名前が必要です。&quot;コント ローラー&quot;-たとえば、の HomeController、LoginController、productcontroller です。</span><span class="sxs-lookup"><span data-stu-id="204f5-193">The MVC framework requires the names of all controllers to end with &quot;Controller&quot;-for example, HomeController, LoginController, or ProductController.</span></span>
   2. <span data-ttu-id="204f5-194">**モデル**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-194">**Models**.</span></span> <span data-ttu-id="204f5-195">MVC Web アプリケーションのアプリケーション モデルを表すクラスのこのフォルダーが提供されます。</span><span class="sxs-lookup"><span data-stu-id="204f5-195">This folder is provided for classes that represent the application model for the MVC Web application.</span></span> <span data-ttu-id="204f5-196">これにより、オブジェクトとデータ ストアと対話するためのロジックを定義するコードが通常含まれます。</span><span class="sxs-lookup"><span data-stu-id="204f5-196">This usually includes code that defines objects and the logic for interacting with the data store.</span></span> <span data-ttu-id="204f5-197">通常、実際のモデル オブジェクトは、別のクラス ライブラリになります。</span><span class="sxs-lookup"><span data-stu-id="204f5-197">Typically, the actual model objects will be in separate class libraries.</span></span> <span data-ttu-id="204f5-198">新しいアプリケーションを作成するときにクラスを含めるし、開発サイクルの後で別のクラス ライブラリに移動可能性があります。</span><span class="sxs-lookup"><span data-stu-id="204f5-198">However, when you create a new application, you might include classes and then move them into separate class libraries at a later point in the development cycle.</span></span>
   3. <span data-ttu-id="204f5-199">**ビュー**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-199">**Views**.</span></span> <span data-ttu-id="204f5-200">このフォルダーは、ビュー、アプリケーションのユーザー インターフェイスを表示するために担当のコンポーネントの推奨される場所です。</span><span class="sxs-lookup"><span data-stu-id="204f5-200">This folder is the recommended location for views, the components responsible for displaying the application's user interface.</span></span> <span data-ttu-id="204f5-201">ビューは、ビューのレンダリングに関連するその他のファイルだけでなく、.aspx、.ascx、.cshtml および .master ファイルを使用します。</span><span class="sxs-lookup"><span data-stu-id="204f5-201">Views use .aspx, .ascx, .cshtml and .master files, in addition to any other files that are related to rendering views.</span></span> <span data-ttu-id="204f5-202">Views フォルダーには、各コント ローラー用のフォルダーが含まれています。フォルダーには、コント ローラー名のプレフィックスが付いたです。</span><span class="sxs-lookup"><span data-stu-id="204f5-202">Views folder contains a folder for each controller; the folder is named with the controller-name prefix.</span></span> <span data-ttu-id="204f5-203">たとえば、という名前のコント ローラーがある場合**HomeController**、Views フォルダーには、Home という名前のフォルダーにが含まれます。</span><span class="sxs-lookup"><span data-stu-id="204f5-203">For example, if you have a controller named **HomeController**, the Views folder will contain a folder named Home.</span></span> <span data-ttu-id="204f5-204">既定では、ASP.NET MVC フレームワークは、ビューを読み込むときに、.aspx ファイルが検索 Views\controllerName フォルダーに要求されたビューの名前を持つ (**ビュー [ControllerName] [アクション] .aspx**) または (**ビュー [ControllerName][アクション] .cshtml**) Razor ビュー。</span><span class="sxs-lookup"><span data-stu-id="204f5-204">By default, when the ASP.NET MVC framework loads a view, it looks for an .aspx file with the requested view name in the Views\controllerName folder (**Views[ControllerName][Action].aspx**) or (**Views[ControllerName][Action].cshtml**) for Razor Views.</span></span>

      > [!NOTE]
      > <span data-ttu-id="204f5-205">フォルダーに加え、前の表に、MVC Web アプリケーションを使用して、 **Global.asax**グローバル URL ルーティングを設定するファイルの既定値 を使用して、 **Web.config**アプリケーションを構成するファイル。</span><span class="sxs-lookup"><span data-stu-id="204f5-205">In addition to the folders listed previously, an MVC Web application uses the **Global.asax** file to set global URL routing defaults, and it uses the **Web.config** file to configure the application.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a><span data-ttu-id="204f5-206">タスク 3 -、HomeController の追加</span><span class="sxs-lookup"><span data-stu-id="204f5-206">Task 3 - Adding a HomeController</span></span>

<span data-ttu-id="204f5-207">MVC フレームワークを使用して ASP.NET アプリケーションでは、ユーザーとの対話はページとを発生させると、それらのページからのイベント処理の周囲に編成されます。</span><span class="sxs-lookup"><span data-stu-id="204f5-207">In ASP.NET applications that do not use the MVC framework, user interaction is organized around pages, and around raising and handling events from those pages.</span></span> <span data-ttu-id="204f5-208">これに対し、ASP.NET MVC アプリケーションでのユーザー操作は、コント ローラーとそのアクション メソッドの周囲に編成されます。</span><span class="sxs-lookup"><span data-stu-id="204f5-208">In contrast, user interaction with ASP.NET MVC applications is organized around controllers and their action methods.</span></span>

<span data-ttu-id="204f5-209">その一方で、ASP.NET MVC フレームワークは、Url をコント ローラーと呼ばれるクラスにマップします。</span><span class="sxs-lookup"><span data-stu-id="204f5-209">On the other hand, ASP.NET MVC framework maps URLs to classes that are referred to as controllers.</span></span> <span data-ttu-id="204f5-210">コント ローラーが入力方向の要求を処理、ユーザー入力との相互作用を処理、適切なアプリケーション ロジックを実行およびクライアントに返送する応答を決定 (HTML を表示、ファイルをダウンロードするには、別の URL をリダイレクトします。)。</span><span class="sxs-lookup"><span data-stu-id="204f5-210">Controllers process incoming requests, handle user input and interactions, execute appropriate application logic and determine the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span> <span data-ttu-id="204f5-211">HTML を表示するには、場合、コント ローラー クラスは、通常、要求の HTML マークアップを生成する別個のビュー コンポーネントを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="204f5-211">In the case of displaying HTML, a controller class typically calls a separate view component to generate the HTML markup for the request.</span></span> <span data-ttu-id="204f5-212">MVC アプリケーションでは、ビューは情報のみを表示し、コントローラーがユーザーの入力と操作を処理して応答します。</span><span class="sxs-lookup"><span data-stu-id="204f5-212">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="204f5-213">このタスクでは、音楽ストア サイトのホーム ページの Url を処理するコント ローラー クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="204f5-213">In this task, you will add a Controller class that will handle URLs to the Home page of the Music Store site.</span></span>

1. <span data-ttu-id="204f5-214">右クリック**コント ローラー** [ソリューション エクスプ ローラー] を選択内のフォルダー**追加**し**コント ローラー**コマンド。</span><span class="sxs-lookup"><span data-stu-id="204f5-214">Right-click **Controllers** folder within the Solution Explorer, select **Add** and then **Controller** command:</span></span>

    <span data-ttu-id="204f5-215">![コント ローラー コマンドを追加する](aspnet-mvc-4-fundamentals/_static/image5.png "コント ローラー コマンドを追加します。")</span><span class="sxs-lookup"><span data-stu-id="204f5-215">![Add a Controller Command](aspnet-mvc-4-fundamentals/_static/image5.png "Add a Controller Command")</span></span>

    <span data-ttu-id="204f5-216">*コント ローラーのコマンドを追加します。*</span><span class="sxs-lookup"><span data-stu-id="204f5-216">*Add Controller Command*</span></span>
2. <span data-ttu-id="204f5-217">**コント ローラーの追加**ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="204f5-217">The **Add Controller** dialog appears.</span></span> <span data-ttu-id="204f5-218">コント ローラーの名前を付けます*HomeController*とキーを押します**追加**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-218">Name the controller *HomeController* and press **Add**.</span></span>

    <span data-ttu-id="204f5-219">![[追加] ダイアログのコント ローラー](aspnet-mvc-4-fundamentals/_static/image6.png "コント ローラー ダイアログの追加")</span><span class="sxs-lookup"><span data-stu-id="204f5-219">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image6.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="204f5-220">*[追加] ダイアログのコント ローラー*</span><span class="sxs-lookup"><span data-stu-id="204f5-220">*Add Controller Dialog*</span></span>
3. <span data-ttu-id="204f5-221">ファイル**HomeController.cs**で作成された、**コント ローラー**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="204f5-221">The file **HomeController.cs** is created in the **Controllers** folder.</span></span> <span data-ttu-id="204f5-222">ために、 **HomeController**そのインデックスの操作で文字列を返す、置換、**インデックス**メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="204f5-222">In order to have the **HomeController** return a string on its Index action, replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="204f5-223">(コード スニペットの*ASP.NET MVC 4 の基礎: Ex1 HomeController インデックス*)</span><span class="sxs-lookup"><span data-stu-id="204f5-223">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex1 HomeController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="204f5-224">タスク 4 - アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="204f5-224">Task 4 - Running the Application</span></span>

<span data-ttu-id="204f5-225">このタスクでは、web ブラウザーでアプリケーションを試みます。</span><span class="sxs-lookup"><span data-stu-id="204f5-225">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="204f5-226">キーを押して**f5 キーを押して**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="204f5-226">Press **F5** to run the Application.</span></span> <span data-ttu-id="204f5-227">プロジェクトがコンパイルされ、ローカル IIS Web サーバーを起動します。</span><span class="sxs-lookup"><span data-stu-id="204f5-227">The project is compiled and the local IIS Web Server starts.</span></span> <span data-ttu-id="204f5-228">ローカル IIS Web サーバーが自動的に開きます web ブラウザーが Web サーバーの URL をポイントします。</span><span class="sxs-lookup"><span data-stu-id="204f5-228">The local IIS Web Server will automatically open a web browser pointing to the URL of the Web server.</span></span>

    <span data-ttu-id="204f5-229">![Web ブラウザーで実行されているアプリケーション](aspnet-mvc-4-fundamentals/_static/image7.png "web ブラウザーで実行されているアプリケーション")</span><span class="sxs-lookup"><span data-stu-id="204f5-229">![Application running in a web browser](aspnet-mvc-4-fundamentals/_static/image7.png "Application running in a web browser")</span></span>

    <span data-ttu-id="204f5-230">*Web ブラウザーで実行されているアプリケーション*</span><span class="sxs-lookup"><span data-stu-id="204f5-230">*Application running in a web browser*</span></span>

    > [!NOTE]
    > <span data-ttu-id="204f5-231">ローカル IIS Web サーバーは、ランダムな空いているポート番号で、web サイトが実行されます。</span><span class="sxs-lookup"><span data-stu-id="204f5-231">The local IIS Web Server will run the website on a random free port number.</span></span> <span data-ttu-id="204f5-232">上記の図に、サイトが実行されている`http://localhost:50103/`ポート 50103 使用されているため、します。</span><span class="sxs-lookup"><span data-stu-id="204f5-232">In the figure above, the site is running at `http://localhost:50103/`, so it's using port 50103.</span></span> <span data-ttu-id="204f5-233">使用するポート番号が異なる場合があります。</span><span class="sxs-lookup"><span data-stu-id="204f5-233">Your port number may vary.</span></span>
2. <span data-ttu-id="204f5-234">ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="204f5-234">Close the browser.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a><span data-ttu-id="204f5-235">手順 2: コント ローラーの作成</span><span class="sxs-lookup"><span data-stu-id="204f5-235">Exercise 2: Creating a Controller</span></span>

<span data-ttu-id="204f5-236">この演習では、音楽ストア アプリケーションの簡単な機能を実装するコント ローラーを更新する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="204f5-236">In this exercise, you will learn how to update the controller to implement simple functionality of the Music Store application.</span></span> <span data-ttu-id="204f5-237">そのコント ローラーには、それぞれ次の特定の要求を処理するアクション メソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="204f5-237">That controller will define action methods to handle each of the following specific requests:</span></span>

- <span data-ttu-id="204f5-238">ミュージック ストアで音楽のジャンルの一覧ページ</span><span class="sxs-lookup"><span data-stu-id="204f5-238">A listing page of the music genres in the Music Store</span></span>
- <span data-ttu-id="204f5-239">すべての特定のジャンルの音楽のアルバムを一覧表示する [参照] ページ</span><span class="sxs-lookup"><span data-stu-id="204f5-239">A browse page that lists all of the music albums for a particular genre</span></span>
- <span data-ttu-id="204f5-240">特定の音楽のアルバムに関する情報が表示される詳細ページ</span><span class="sxs-lookup"><span data-stu-id="204f5-240">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="204f5-241">この演習では、スコープにそれらのアクションが返されるだけ文字列ここまででします。</span><span class="sxs-lookup"><span data-stu-id="204f5-241">For the scope of this exercise, those actions will simply return a string by now.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a><span data-ttu-id="204f5-242">タスク 1 - 新しい StoreController クラスの追加</span><span class="sxs-lookup"><span data-stu-id="204f5-242">Task 1 - Adding a New StoreController Class</span></span>

<span data-ttu-id="204f5-243">このタスクでは、新しいコント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="204f5-243">In this task, you will add a new Controller.</span></span>

1. <span data-ttu-id="204f5-244">まだ開いていない場合は開始**VS が Express for Web 2012**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-244">If not already open, start **VS Express for Web 2012**.</span></span>
2. <span data-ttu-id="204f5-245">**ファイル**] メニューの [選択**プロジェクトを開く**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-245">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="204f5-246">プロジェクトを開くダイアログ ボックスを参照**Source\Ex02 CreatingAController\Begin****[Begin.sln]** をクリック**開く**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-246">In the Open Project dialog, browse to **Source\Ex02-CreatingAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="204f5-247">代わりに、前の手順の完了後に取得したソリューションは、引き続き行えます。</span><span class="sxs-lookup"><span data-stu-id="204f5-247">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="204f5-248">指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。</span><span class="sxs-lookup"><span data-stu-id="204f5-248">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="204f5-249">これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-249">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="204f5-250">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。</span><span class="sxs-lookup"><span data-stu-id="204f5-250">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="204f5-251">最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-251">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="204f5-252">NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="204f5-252">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="204f5-253">NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="204f5-253">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="204f5-254">これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。</span><span class="sxs-lookup"><span data-stu-id="204f5-254">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="204f5-255">新しいコント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="204f5-255">Add the new controller.</span></span> <span data-ttu-id="204f5-256">これを行うを右クリックし、**コント ローラー** [ソリューション エクスプ ローラー] を選択内のフォルダー**追加**し、**コント ローラー**コマンド。</span><span class="sxs-lookup"><span data-stu-id="204f5-256">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="204f5-257">変更、**コント ローラー名**に*StoreController*、 をクリック**追加**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-257">Change the **Controller Name** to *StoreController*, and click **Add**.</span></span>

    <span data-ttu-id="204f5-258">![[追加] ダイアログのコント ローラー](aspnet-mvc-4-fundamentals/_static/image8.png "コント ローラー ダイアログの追加")</span><span class="sxs-lookup"><span data-stu-id="204f5-258">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image8.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="204f5-259">*[追加] ダイアログのコント ローラー*</span><span class="sxs-lookup"><span data-stu-id="204f5-259">*Add Controller Dialog*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a><span data-ttu-id="204f5-260">タスク 2 - StoreController のアクションを変更します。</span><span class="sxs-lookup"><span data-stu-id="204f5-260">Task 2 - Modifying the StoreController's Actions</span></span>

<span data-ttu-id="204f5-261">このタスクでは、呼び出されるコント ローラーのメソッドを変更して**アクション**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-261">In this task, you will modify the Controller methods that are called **actions**.</span></span> <span data-ttu-id="204f5-262">アクションは、URL 要求を処理して、ブラウザーや URL を呼び出したユーザーに送信されるコンテンツを判断します。</span><span class="sxs-lookup"><span data-stu-id="204f5-262">Actions are responsible for handling URL requests and determining the content that should be sent back to the browser or user that invoked the URL.</span></span>

1. <span data-ttu-id="204f5-263">**StoreController**クラスには既に、**インデックス**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="204f5-263">The **StoreController** class already has an **Index** method.</span></span> <span data-ttu-id="204f5-264">このラボに後で、音楽ストアのすべてのジャンルを一覧するページを実装するのに使用するされます。</span><span class="sxs-lookup"><span data-stu-id="204f5-264">You will use it later in this Lab to implement the page that lists all genres of the music store.</span></span> <span data-ttu-id="204f5-265">ここでは、置き換えるだけです、**インデックス**メソッドを次のコード文字列を返す&quot;Store.Index() から Hello&quot;:。</span><span class="sxs-lookup"><span data-stu-id="204f5-265">For now, just replace the **Index** method with the following code that returns a string &quot;Hello from Store.Index()&quot;:</span></span>

    <span data-ttu-id="204f5-266">(コード スニペットの*ASP.NET MVC 4 の基礎: Ex2 StoreController インデックス*)</span><span class="sxs-lookup"><span data-stu-id="204f5-266">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. <span data-ttu-id="204f5-267">追加**参照**と**詳細**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="204f5-267">Add **Browse** and **Details** methods.</span></span> <span data-ttu-id="204f5-268">これを行うには、次のコードを追加、 **StoreController**:</span><span class="sxs-lookup"><span data-stu-id="204f5-268">To do this, add the following code to the **StoreController**:</span></span>

    <span data-ttu-id="204f5-269">(コード スニペットの*ASP.NET MVC 4 の基礎: Ex2 StoreController BrowseAndDetails*)</span><span class="sxs-lookup"><span data-stu-id="204f5-269">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController BrowseAndDetails*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="204f5-270">タスク 3 - アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="204f5-270">Task 3 - Running the Application</span></span>

<span data-ttu-id="204f5-271">このタスクでは、web ブラウザーでアプリケーションを試みます。</span><span class="sxs-lookup"><span data-stu-id="204f5-271">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="204f5-272">キーを押して**f5 キーを押して**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="204f5-272">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="204f5-273">プロジェクトを起動、**ホーム**ページ。</span><span class="sxs-lookup"><span data-stu-id="204f5-273">The project starts in the **Home** page.</span></span> <span data-ttu-id="204f5-274">各アクションの実装を確認する URL を変更します。</span><span class="sxs-lookup"><span data-stu-id="204f5-274">Change the URL to verify each action's implementation.</span></span>

    1. <span data-ttu-id="204f5-275">**/Store**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-275">**/Store**.</span></span> <span data-ttu-id="204f5-276">表示されます **&quot;Store.Index() からこんにちは&quot;** です。</span><span class="sxs-lookup"><span data-stu-id="204f5-276">You will see **&quot;Hello from Store.Index()&quot;**.</span></span>
    2. <span data-ttu-id="204f5-277">**/ストア/参照**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-277">**/Store/Browse**.</span></span> <span data-ttu-id="204f5-278">表示されます **&quot;Store.Browse() からこんにちは&quot;** です。</span><span class="sxs-lookup"><span data-stu-id="204f5-278">You will see **&quot;Hello from Store.Browse()&quot;**.</span></span>
    3. <span data-ttu-id="204f5-279">**/ストア/詳細**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-279">**/Store/Details**.</span></span> <span data-ttu-id="204f5-280">表示されます **&quot;Store.Details() からこんにちは&quot;** です。</span><span class="sxs-lookup"><span data-stu-id="204f5-280">You will see **&quot;Hello from Store.Details()&quot;**.</span></span>

        <span data-ttu-id="204f5-281">![参照 StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "StoreBrowse をブラウズ")</span><span class="sxs-lookup"><span data-stu-id="204f5-281">![Browsing StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "Browsing StoreBrowse")</span></span>

        <span data-ttu-id="204f5-282">*/Store/Browse をブラウズ*</span><span class="sxs-lookup"><span data-stu-id="204f5-282">*Browsing /Store/Browse*</span></span>
3. <span data-ttu-id="204f5-283">ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="204f5-283">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a><span data-ttu-id="204f5-284">手順 3: コント ローラーにパラメーターを渡す</span><span class="sxs-lookup"><span data-stu-id="204f5-284">Exercise 3: Passing parameters to a Controller</span></span>

<span data-ttu-id="204f5-285">これまでにはコント ローラーから定数文字列が返すことされています。</span><span class="sxs-lookup"><span data-stu-id="204f5-285">Until now, you have been returning constant strings from the Controllers.</span></span> <span data-ttu-id="204f5-286">この演習では、URL と、クエリ文字列を使用し、ブラウザーにテキストを含む応答メソッドのアクション、コント ローラーにパラメーターを渡す方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="204f5-286">In this exercise you will learn how to pass parameters to a Controller using the URL and querystring, and then making the method actions respond with text to the browser.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a><span data-ttu-id="204f5-287">タスク 1 - StoreController をジャンル パラメーターを追加します。</span><span class="sxs-lookup"><span data-stu-id="204f5-287">Task 1 - Adding Genre Parameter to StoreController</span></span>

<span data-ttu-id="204f5-288">このタスクでは使用して、 **querystring**へのパラメーターを送信する、**参照**でアクション メソッド、 **StoreController**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-288">In this task, you will use the **querystring** to send parameters to the **Browse** action method in the **StoreController**.</span></span>

1. <span data-ttu-id="204f5-289">まだ開いていない場合は開始**VS Express for Web**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-289">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="204f5-290">**ファイル**] メニューの [選択**プロジェクトを開く**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-290">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="204f5-291">プロジェクトを開くダイアログ ボックスを参照**Source\Ex03 PassingParametersToAController\Begin****[Begin.sln]** をクリック**開く**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-291">In the Open Project dialog, browse to **Source\Ex03-PassingParametersToAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="204f5-292">代わりに、前の手順の完了後に取得したソリューションは、引き続き行えます。</span><span class="sxs-lookup"><span data-stu-id="204f5-292">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="204f5-293">指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。</span><span class="sxs-lookup"><span data-stu-id="204f5-293">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="204f5-294">これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-294">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="204f5-295">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。</span><span class="sxs-lookup"><span data-stu-id="204f5-295">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="204f5-296">最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-296">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="204f5-297">NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="204f5-297">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="204f5-298">NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="204f5-298">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="204f5-299">これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。</span><span class="sxs-lookup"><span data-stu-id="204f5-299">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="204f5-300">開いている**StoreController**クラスです。</span><span class="sxs-lookup"><span data-stu-id="204f5-300">Open **StoreController** class.</span></span> <span data-ttu-id="204f5-301">これを行うには**ソリューション エクスプ ローラー**、展開、**コント ローラー**フォルダーをダブルクリック**StoreController.cs**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-301">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
4. <span data-ttu-id="204f5-302">変更、**参照**特定ジャンルを要求する文字列パラメーターを追加するメソッド。</span><span class="sxs-lookup"><span data-stu-id="204f5-302">Change the **Browse** method, adding a string parameter to request for a specific genre.</span></span> <span data-ttu-id="204f5-303">ASP.NET MVC の任意のクエリ文字列を渡すまたはフォーム ポスト パラメーターの名前が自動的に**ジャンル**このアクション メソッドが呼び出されたときにします。</span><span class="sxs-lookup"><span data-stu-id="204f5-303">ASP.NET MVC will automatically pass any querystring or form post parameters named **genre** to this action method when invoked.</span></span> <span data-ttu-id="204f5-304">これを行うには、置換、**参照**メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="204f5-304">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="204f5-305">(コード スニペットの*ASP.NET MVC 4 の基礎: Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="204f5-305">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="204f5-306">使用している、 **HttpUtility.HtmlEncode**するユーティリティ メソッドがのようなリンクを使用して、ビューに Javascript を挿入できないように**ストア/参照しますか?ジャンル =&lt;スクリプト&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;** です。</span><span class="sxs-lookup"><span data-stu-id="204f5-306">You are using the **HttpUtility.HtmlEncode** utility method to prevents users from injecting Javascript into the View with a link like **/Store/Browse?Genre=&lt;script&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.</span></span>
> 
> <span data-ttu-id="204f5-307">詳細についてを参照してください[この msdn 記事](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="204f5-307">For further explanation, please visit [this msdn article](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="204f5-308">タスク 2 - アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="204f5-308">Task 2 - Running the Application</span></span>

<span data-ttu-id="204f5-309">このタスクでは、web ブラウザーでアプリケーションを試してみるしを使用して、**ジャンル**パラメーター。</span><span class="sxs-lookup"><span data-stu-id="204f5-309">In this task, you will try out the Application in a web browser and use the **genre** parameter.</span></span>

1. <span data-ttu-id="204f5-310">キーを押して**f5 キーを押して**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="204f5-310">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="204f5-311">プロジェクトを起動、**ホーム**ページ。</span><span class="sxs-lookup"><span data-stu-id="204f5-311">The project starts in the **Home** page.</span></span> <span data-ttu-id="204f5-312">URL を変更して  */格納/参照しますか?ジャンル Disco を =* アクションがジャンル パラメーターを受け取ることを確認します。</span><span class="sxs-lookup"><span data-stu-id="204f5-312">Change the URL to */Store/Browse?Genre=Disco* to verify that the action receives the genre parameter.</span></span>

    <span data-ttu-id="204f5-313">![参照 StoreBrowseGenre Disco を =](aspnet-mvc-4-fundamentals/_static/image10.png "StoreBrowseGenre をブラウズ Disco を =")</span><span class="sxs-lookup"><span data-stu-id="204f5-313">![Browsing StoreBrowseGenre=Disco](aspnet-mvc-4-fundamentals/_static/image10.png "Browsing StoreBrowseGenre=Disco")</span></span>

    <span data-ttu-id="204f5-314">*/Store/Browse をブラウズしますか。ジャンル Disco を =*</span><span class="sxs-lookup"><span data-stu-id="204f5-314">*Browsing /Store/Browse?Genre=Disco*</span></span>
3. <span data-ttu-id="204f5-315">ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="204f5-315">Close the browser.</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a><span data-ttu-id="204f5-316">タスク 3 - URL に埋め込まれた Id パラメーターを追加します。</span><span class="sxs-lookup"><span data-stu-id="204f5-316">Task 3 - Adding an Id Parameter Embedded in the URL</span></span>

<span data-ttu-id="204f5-317">このタスクでは使用して、 **URL**に渡す、 **Id**パラメーターを**詳細**のアクション メソッド、 **StoreController**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-317">In this task, you will use the **URL** to pass an **Id** parameter to the **Details** action method of the **StoreController**.</span></span> <span data-ttu-id="204f5-318">ASP.NET MVC の既定のルーティング規則がという名前のパラメーターとしてアクション メソッド名の後に、URL のセグメントを処理するには**Id**です。アクション メソッドに Id をという名前のパラメーターがある場合は、し、ASP.NET MVC は自動的に URL セグメントをパラメーターとして渡します。</span><span class="sxs-lookup"><span data-stu-id="204f5-318">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named **Id**. If your action method has parameter named Id, then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span> <span data-ttu-id="204f5-319">URL で**ストア/詳細/5**、 **Id**として解釈されます**5**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-319">In the URL **Store/Details/5**, **Id** will be interpreted as **5**.</span></span>

1. <span data-ttu-id="204f5-320">変更、**詳細**のメソッド、 **StoreController**を追加する、 **int**という名前のパラメーター **id**です。これを行うには、置換、**詳細**メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="204f5-320">Change the **Details** method of the **StoreController**, adding an **int** parameter called **id**. To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="204f5-321">(コード スニペットの*ASP.NET MVC 4 の基礎: Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="204f5-321">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="204f5-322">タスク 4 - アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="204f5-322">Task 4 - Running the Application</span></span>

<span data-ttu-id="204f5-323">このタスクでは、web ブラウザーでアプリケーションを試してみるしを使用して、 **Id**パラメーター。</span><span class="sxs-lookup"><span data-stu-id="204f5-323">In this task, you will try out the Application in a web browser and use the **Id** parameter.</span></span>

1. <span data-ttu-id="204f5-324">キーを押して**f5 キーを押して**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="204f5-324">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="204f5-325">プロジェクトを起動、**ホーム**ページ。</span><span class="sxs-lookup"><span data-stu-id="204f5-325">The project starts in the **Home** page.</span></span> <span data-ttu-id="204f5-326">URL を変更して */Store/Details/5*アクションが、id パラメーターを受け取っていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="204f5-326">Change the URL to */Store/Details/5* to verify that the action receives the id parameter.</span></span>

    <span data-ttu-id="204f5-327">![参照 StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "StoreDetails5 をブラウズ")</span><span class="sxs-lookup"><span data-stu-id="204f5-327">![Browsing StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "Browsing StoreDetails5")</span></span>

    <span data-ttu-id="204f5-328">*/Store/Details/5 をブラウズ*</span><span class="sxs-lookup"><span data-stu-id="204f5-328">*Browsing /Store/Details/5*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a><span data-ttu-id="204f5-329">手順 4: ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="204f5-329">Exercise 4: Creating a View</span></span>

<span data-ttu-id="204f5-330">これまでにするがされた文字列から返すコント ローラーのアクション。</span><span class="sxs-lookup"><span data-stu-id="204f5-330">So far you have been returning strings from controller actions.</span></span> <span data-ttu-id="204f5-331">コント ローラーが機能するしくみを理解するための便利な方法はどのように、実際の Web アプリケーションは構築されません。</span><span class="sxs-lookup"><span data-stu-id="204f5-331">Although that is a useful way of understanding how controllers work, it is not how your real Web applications are built.</span></span> <span data-ttu-id="204f5-332">ビューは、テンプレート ファイルの使用にブラウザーに HTML を生成するためのより適切な方法を提供するコンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="204f5-332">Views are components that provide a better approach for generating HTML back to the browser with the use of template files.</span></span>

<span data-ttu-id="204f5-333">この演習では、一般的な HTML コンテンツをサイトと、最後に HTML を返す HomeController を有効にするビュー テンプレートのルック アンド フィールを強化するために、スタイル シートのテンプレートをセットアップするレイアウトのマスター ページを追加する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="204f5-333">In this exercise you will learn how to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel of the site and finally a View template to enable HomeController to return HTML.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-layoutcshtml"></a><span data-ttu-id="204f5-334">タスク 1 - ファイルを変更する\_layout.cshtml</span><span class="sxs-lookup"><span data-stu-id="204f5-334">Task 1 - Modifying the file \_layout.cshtml</span></span>

<span data-ttu-id="204f5-335">ファイル **~/Views/Shared/\_layout.cshtml**一般的な HTML web サイト全体にわたって使用するためのテンプレートをセットアップすることができます。</span><span class="sxs-lookup"><span data-stu-id="204f5-335">The file **~/Views/Shared/\_layout.cshtml** allows you to setup a template for common HTML to use across the entire website.</span></span> <span data-ttu-id="204f5-336">このタスクでは、ホーム ページおよびストアの領域にリンクに共通のヘッダーでレイアウトのマスター ページを追加します。</span><span class="sxs-lookup"><span data-stu-id="204f5-336">In this task you will add a layout master page with a common header with links to the Home page and Store area.</span></span>

1. <span data-ttu-id="204f5-337">まだ開いていない場合は開始**VS Express for Web**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-337">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="204f5-338">**ファイル**] メニューの [選択**プロジェクトを開く**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-338">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="204f5-339">プロジェクトを開くダイアログ ボックスを参照**Source\Ex04 CreatingAView\Begin****[Begin.sln]** をクリック**開く**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-339">In the Open Project dialog, browse to **Source\Ex04-CreatingAView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="204f5-340">代わりに、前の手順の完了後に取得したソリューションは、引き続き行えます。</span><span class="sxs-lookup"><span data-stu-id="204f5-340">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="204f5-341">指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。</span><span class="sxs-lookup"><span data-stu-id="204f5-341">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="204f5-342">これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-342">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="204f5-343">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。</span><span class="sxs-lookup"><span data-stu-id="204f5-343">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="204f5-344">最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-344">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="204f5-345">NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="204f5-345">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="204f5-346">NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="204f5-346">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="204f5-347">これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。</span><span class="sxs-lookup"><span data-stu-id="204f5-347">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="204f5-348">ファイル <strong>\_layout.cshtml</strong>コンテナーの HTML レイアウト、サイトのすべてのページが含まれています。</span><span class="sxs-lookup"><span data-stu-id="204f5-348">The file <strong>\_layout.cshtml</strong> contains the HTML container layout for all pages on the site.</span></span> <span data-ttu-id="204f5-349">含まれている、 <strong>&lt;html&gt;</strong> HTML 応答の要素だけでなく<strong>&lt;ヘッド&gt;</strong>と<strong>&lt;本文&gt;</strong>要素。</span><span class="sxs-lookup"><span data-stu-id="204f5-349">It includes the <strong>&lt;html&gt;</strong> element for the HTML response, as well as the <strong>&lt;head&gt;</strong> and <strong>&lt;body&gt;</strong> elements.</span></span> <span data-ttu-id="204f5-350"><strong>@RenderBody()</strong> HTML 内の本文を識別地域そのビューのテンプレートは、動的なコンテンツをコピーすることができます。</span><span class="sxs-lookup"><span data-stu-id="204f5-350"><strong>@RenderBody()</strong> within the HTML body identify regions that view templates will be able to fill in with dynamic content.</span></span>
   <span data-ttu-id="204f5-351">(C#)</span><span class="sxs-lookup"><span data-stu-id="204f5-351">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. <span data-ttu-id="204f5-352">サイト内のすべてのページで、ホーム ページおよびストアの領域へのリンクに共通のヘッダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="204f5-352">Add a common header with links to the Home page and Store area on all pages in the site.</span></span> <span data-ttu-id="204f5-353">そのためには、下の次のコードを追加&lt;本文&gt;ステートメントです。</span><span class="sxs-lookup"><span data-stu-id="204f5-353">In order to do that, add the following code below &lt;body&gt; statement.</span></span>
   <span data-ttu-id="204f5-354">(C#)</span><span class="sxs-lookup"><span data-stu-id="204f5-354">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. <span data-ttu-id="204f5-355">各ページの body セクションに表示するために div が含まれます。</span><span class="sxs-lookup"><span data-stu-id="204f5-355">Include a div to render the body section of each page.</span></span> <span data-ttu-id="204f5-356">置き換える <strong>@RenderBody()</strong>を次の強調表示されているコード: (c#)</span><span class="sxs-lookup"><span data-stu-id="204f5-356">Replace <strong>@RenderBody()</strong> with the following higlighted code: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="204f5-357">ご存知でしたか。</span><span class="sxs-lookup"><span data-stu-id="204f5-357">Did you know?</span></span> <span data-ttu-id="204f5-358">Visual Studio 2012 では、HTML やコード ファイルで一般的に使用されるコードを追加しやすいスニペットが。</span><span class="sxs-lookup"><span data-stu-id="204f5-358">Visual Studio 2012 has snippets that make it easy to add commonly used code in HTML, code files and more!</span></span> <span data-ttu-id="204f5-359">」と入力して試して**&lt;div&gt;** キーを押して**タブ**完全な挿入を 2 回**div**タグ。</span><span class="sxs-lookup"><span data-stu-id="204f5-359">Try it out by typing **&lt;div&gt;** and pressing **TAB** twice to insert a complete **div** tag.</span></span>

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a><span data-ttu-id="204f5-360">作業 2: 追加の CSS スタイル シート</span><span class="sxs-lookup"><span data-stu-id="204f5-360">Task 2 - Adding CSS Stylesheet</span></span>

<span data-ttu-id="204f5-361">空のプロジェクト テンプレートには、基本的なフォームや検証メッセージを表示するために使用するスタイルだけを含む非常に簡素化された CSS ファイルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="204f5-361">The empty project template includes a very streamlined CSS file which just includes styles used to display basic forms and validation messages.</span></span> <span data-ttu-id="204f5-362">追加の CSS および (デザイナーによって提供される可能性があります) イメージは、サイトのルック アンド フィールを強化するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="204f5-362">You will use additional CSS and images (potentially provided by a designer) in order to enhance the look and feel of the site.</span></span>

<span data-ttu-id="204f5-363">このタスクでは、サイトのスタイルを定義する CSS スタイル シートを追加します。</span><span class="sxs-lookup"><span data-stu-id="204f5-363">In this task, you will add a CSS stylesheet to define the styles of the site.</span></span>

1. <span data-ttu-id="204f5-364">CSS ファイルとイメージに含まれる、 **Source\Assets\Content**このラボのフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="204f5-364">The CSS file and images are included in the **Source\Assets\Content** folder of this Lab.</span></span> <span data-ttu-id="204f5-365">それらをアプリケーションに追加するためにからコンテンツをドラッグ、 **Windows エクスプ ローラー**ウィンドウに、**ソリューション エクスプ ローラー** Visual Studio Express for Web、次に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="204f5-365">In order to add them to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Studio Express for Web, as shown below:</span></span>

    <span data-ttu-id="204f5-366">![スタイルの内容をドラッグする](aspnet-mvc-4-fundamentals/_static/image12.png "スタイルの内容をドラッグします。")</span><span class="sxs-lookup"><span data-stu-id="204f5-366">![Dragging style contents](aspnet-mvc-4-fundamentals/_static/image12.png "Dragging style contents")</span></span>

    <span data-ttu-id="204f5-367">*スタイルの内容をドラッグします。*</span><span class="sxs-lookup"><span data-stu-id="204f5-367">*Dragging style contents*</span></span>
2. <span data-ttu-id="204f5-368">警告ダイアログが表示されます、置換するのには確認を求める**Site.css**ファイルといくつかの既存のイメージです。</span><span class="sxs-lookup"><span data-stu-id="204f5-368">A warning dialog will appear, asking for confirmation to replace **Site.css** file and some existing images.</span></span> <span data-ttu-id="204f5-369">確認**すべての項目に適用する** をクリック**はい**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-369">Check **Apply to all items** and click **Yes**.</span></span>

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a><span data-ttu-id="204f5-370">タスク 3 - ビュー テンプレートを追加します。</span><span class="sxs-lookup"><span data-stu-id="204f5-370">Task 3 - Adding a View Template</span></span>

<span data-ttu-id="204f5-371">このタスクでは、レイアウトのマスター ページを使用して HTML 応答を生成するテンプレートの表示を追加し、CSS は、この手順で追加します。</span><span class="sxs-lookup"><span data-stu-id="204f5-371">In this task, you will add a View template to generate the HTML response that will use the layout master page and CSS added in this exercise.</span></span>

1. <span data-ttu-id="204f5-372">サイトのホーム ページを参照するときに、ビュー テンプレートを使用して、まず必要があることを示す文字列を返す代わりに、 **HomeController インデックス**メソッドは、**ビュー**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-372">To use a View template when browsing the site's home page, you will first need to indicate that instead of returning a string, the **HomeController Index** method will return a **View**.</span></span> <span data-ttu-id="204f5-373">開いている**HomeController**クラスし、変更、**インデックス**を返すメソッドを**ActionResult**、してそれを返す**View()** です。</span><span class="sxs-lookup"><span data-stu-id="204f5-373">Open **HomeController** class and change its **Index** method to return an **ActionResult**, and have it return **View()**.</span></span>

    <span data-ttu-id="204f5-374">(コード スニペットの*ASP.NET MVC 4 の基礎: Ex4 HomeController インデックス*)</span><span class="sxs-lookup"><span data-stu-id="204f5-374">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex4 HomeController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. <span data-ttu-id="204f5-375">ここで、適切なビュー テンプレートを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="204f5-375">Now, you need to add an appropriate View template.</span></span> <span data-ttu-id="204f5-376">これを行う**を右クリックして**内、**インデックス**アクション メソッドを選択**ビューの追加**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-376">To do this, **right-click** inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="204f5-377">これが表示されます、**ビューの追加**ダイアログ。</span><span class="sxs-lookup"><span data-stu-id="204f5-377">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="204f5-378">![インデックス メソッド内からビューを追加する](aspnet-mvc-4-fundamentals/_static/image13.png "インデックス メソッド内からビューを追加します。")</span><span class="sxs-lookup"><span data-stu-id="204f5-378">![Adding a View from within the Index method](aspnet-mvc-4-fundamentals/_static/image13.png "Adding a View from within the Index method")</span></span>

    <span data-ttu-id="204f5-379">*インデックス メソッド内からビューを追加します。*</span><span class="sxs-lookup"><span data-stu-id="204f5-379">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="204f5-380">**ビューの追加**ビュー テンプレート ファイルを生成するダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="204f5-380">The **Add View** Dialog will appear to generate a View template file.</span></span> <span data-ttu-id="204f5-381">既定では、このダイアログ ボックスでは、それを使用するアクション メソッドに一致するビュー テンプレートの名前が事前設定します。</span><span class="sxs-lookup"><span data-stu-id="204f5-381">By default, this dialog pre-populates the name of the View template so that it matches the action method that will use it.</span></span> <span data-ttu-id="204f5-382">使用したため、**ビューの追加**内でコンテキスト メニュー、**インデックス**HomeController、内のアクション メソッド、**ビューの追加**ダイアログでは、既定の表示名としてのインデックス。</span><span class="sxs-lookup"><span data-stu-id="204f5-382">Because you used the **Add View** context menu within the **Index** action method within the HomeController, the **Add View** dialog has Index as the default view name.</span></span> <span data-ttu-id="204f5-383">**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="204f5-383">Click **Add**.</span></span>

    <span data-ttu-id="204f5-384">![[追加] ダイアログの表示](aspnet-mvc-4-fundamentals/_static/image14.png "追加ビュー ダイアログ ボックス")</span><span class="sxs-lookup"><span data-stu-id="204f5-384">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image14.png "Add View Dialog")</span></span>

    <span data-ttu-id="204f5-385">*[追加] ダイアログの表示*</span><span class="sxs-lookup"><span data-stu-id="204f5-385">*Add View Dialog*</span></span>
4. <span data-ttu-id="204f5-386">Visual Studio によって生成、 **Index.cshtml**内部テンプレートの表示、 **views \home**フォルダーし、それを開きます。</span><span class="sxs-lookup"><span data-stu-id="204f5-386">Visual Studio generates an **Index.cshtml** view template inside the **Views\Home** folder and then opens it.</span></span>

    <span data-ttu-id="204f5-387">![作成されたインデックス ビューをホーム](aspnet-mvc-4-fundamentals/_static/image15.png "ホーム インデックス ビューの作成")</span><span class="sxs-lookup"><span data-stu-id="204f5-387">![Home Index view created](aspnet-mvc-4-fundamentals/_static/image15.png "Home Index view created")</span></span>

    <span data-ttu-id="204f5-388">*作成されたホームのインデックス ビュー*</span><span class="sxs-lookup"><span data-stu-id="204f5-388">*Home Index view created*</span></span>

    > [!NOTE]
    > <span data-ttu-id="204f5-389">名前と場所、 **Index.cshtml**ファイルは関連し、既定の ASP.NET MVC の名前付け規則に従います。</span><span class="sxs-lookup"><span data-stu-id="204f5-389">name and location of the **Index.cshtml** file is relevant and follows the default ASP.NET MVC naming conventions.</span></span>
    > 
    > <span data-ttu-id="204f5-390">フォルダー \Views\**ホーム** コント ローラー名と一致する (**ホーム**コント ローラー)。</span><span class="sxs-lookup"><span data-stu-id="204f5-390">The folder \Views\**Home** matches the controller name (**Home** Controller).</span></span> <span data-ttu-id="204f5-391">ビューのテンプレート名 (**インデックス**)、ビューを表示するコント ローラー アクション メソッドに一致します。</span><span class="sxs-lookup"><span data-stu-id="204f5-391">The View template name (**Index**), matches the controller action method which will be displaying the View.</span></span>
    > 
    > <span data-ttu-id="204f5-392">これにより、ASP.NET MVC は、この名前付け規則を使用して、ビューを取得するときに、名前またはビュー テンプレートの場所を明示的に指定することで回避できます。</span><span class="sxs-lookup"><span data-stu-id="204f5-392">This way, ASP.NET MVC avoids having to explicitly specify the name or location of a View template when using this naming convention to return a View.</span></span>
5. <span data-ttu-id="204f5-393">生成されたビュー テンプレートがに基づいて、  **\_layout.cshtml**以前に定義されているテンプレートです。</span><span class="sxs-lookup"><span data-stu-id="204f5-393">The generated View template is based on the **\_layout.cshtml** template earlier defined.</span></span> <span data-ttu-id="204f5-394">ViewBag.Title するプロパティを更新**ホーム**、主な内容の変更と**これは、ホーム ページ**次のコードに示すように。</span><span class="sxs-lookup"><span data-stu-id="204f5-394">Update the ViewBag.Title property to **Home**, and change the main content to **This is the Home Page**, as shown in the code below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. <span data-ttu-id="204f5-395">選択**MvcMusicStore**キーを押して、ソリューション エクスプ ローラーでプロジェクト**f5 キーを押して**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="204f5-395">Select **MvcMusicStore** project in the Solution Explorer and Press **F5** to run the Application.</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a><span data-ttu-id="204f5-396">タスク 4: 検証</span><span class="sxs-lookup"><span data-stu-id="204f5-396">Task 4: Verification</span></span>

<span data-ttu-id="204f5-397">前述の手順で、すべての手順を正しく実行したことを確認するために次の手順します。</span><span class="sxs-lookup"><span data-stu-id="204f5-397">In order to verify that you have correctly performed all the steps in the previous exercise, proceed as follows:</span></span>

<span data-ttu-id="204f5-398">ブラウザーで開かれているアプリケーションを注意してください。</span><span class="sxs-lookup"><span data-stu-id="204f5-398">With the application opened in a browser, you should note that:</span></span>

1. <span data-ttu-id="204f5-399">HomeController の Index アクション メソッドが検出され、表示、 **\Views\Home\Index.cshtml**コードが呼び出されていなくても、テンプレートを表示**View() を返す**でビュー テンプレートの後にあるため、標準の名前付け規則です。</span><span class="sxs-lookup"><span data-stu-id="204f5-399">The HomeController's Index action method found and displayed the **\Views\Home\Index.cshtml** View template, even though the code called **return View()**, because the View template followed the standard naming convention.</span></span>
2. <span data-ttu-id="204f5-400">ホーム ページ内で定義されたへようこそ メッセージを表示、 **\Views\Home\Index.cshtml**ビュー テンプレート。</span><span class="sxs-lookup"><span data-stu-id="204f5-400">The Home Page displays the welcome message defined within the **\Views\Home\Index.cshtml** view template.</span></span>
3. <span data-ttu-id="204f5-401">ホーム ページを使用して、  **\_layout.cshtml**テンプレート、およびウェルカム メッセージが標準サイト HTML レイアウト内に含まれるようにします。</span><span class="sxs-lookup"><span data-stu-id="204f5-401">The Home Page is using the **\_layout.cshtml** template, and so the welcome message is contained within the standard site HTML layout.</span></span>

    <span data-ttu-id="204f5-402">![定義済みの LayoutPage とスタイルを使用して、インデックス ビューをホーム](aspnet-mvc-4-fundamentals/_static/image16.png "定義済みの LayoutPage とスタイルを使用して、インデックス ビュー ホーム")</span><span class="sxs-lookup"><span data-stu-id="204f5-402">![Home Index View using the defined LayoutPage and style](aspnet-mvc-4-fundamentals/_static/image16.png "Home Index View using the defined LayoutPage and style")</span></span>

    <span data-ttu-id="204f5-403">*定義済みの LayoutPage とスタイルを使用してホームのインデックス ビュー*</span><span class="sxs-lookup"><span data-stu-id="204f5-403">*Home Index View using the defined LayoutPage and style*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a><span data-ttu-id="204f5-404">手順 5: ビュー モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="204f5-404">Exercise 5: Creating a View Model</span></span>

<span data-ttu-id="204f5-405">これまでにハードコードされた、HTML を表示してビューを作成したが、動的な web アプリケーションを作成するために、ビュー テンプレートは、コント ローラーから情報を受信する必要があります。</span><span class="sxs-lookup"><span data-stu-id="204f5-405">So far, you made your Views display hardcoded HTML, but, in order to create dynamic web applications, the View template should receive information from the Controller.</span></span> <span data-ttu-id="204f5-406">そのような目的に使用する 1 つの一般的な手法は、 **ViewModel**パターンでは、これにより、コント ローラーを適切な HTML 応答を生成するために必要なすべての情報をパッケージ化します。</span><span class="sxs-lookup"><span data-stu-id="204f5-406">One common technique to be used for that purpose is the **ViewModel** pattern, which allows a Controller to package up all the information needed to generate the appropriate HTML response.</span></span>

<span data-ttu-id="204f5-407">この演習では、ViewModel クラスを作成し、必要なプロパティを追加する方法を学習します。 ストアに格納され、ジャンルの一覧のジャンルの数。</span><span class="sxs-lookup"><span data-stu-id="204f5-407">In this exercise, you will learn how to create a ViewModel class and add the required properties: the number of genres in the store and a list of those genres.</span></span> <span data-ttu-id="204f5-408">作成された ViewModel を使用する StoreController を更新することも、および最後に、ページで説明したようにプロパティを表示する新しいビュー テンプレートを作成します。</span><span class="sxs-lookup"><span data-stu-id="204f5-408">You will also update the StoreController to use the created ViewModel, and finally, you will create a new View template that will display the mentioned properties in the page.</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a><span data-ttu-id="204f5-409">タスク 1 - ViewModel クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="204f5-409">Task 1 - Creating a ViewModel Class</span></span>

<span data-ttu-id="204f5-410">このタスクでは、ストア ジャンルの一覧のシナリオを実装する ViewModel クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="204f5-410">In this task, you will create a ViewModel class that will implement the Store genre listing scenario.</span></span>

1. <span data-ttu-id="204f5-411">まだ開いていない場合は開始**VS Express for Web**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-411">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="204f5-412">**ファイル**] メニューの [選択**プロジェクトを開く**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-412">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="204f5-413">プロジェクトを開くダイアログ ボックスを参照**Source\Ex05 CreatingAViewModel\Begin****[Begin.sln]** をクリック**開く**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-413">In the Open Project dialog, browse to **Source\Ex05-CreatingAViewModel\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="204f5-414">代わりに、前の手順の完了後に取得したソリューションは、引き続き行えます。</span><span class="sxs-lookup"><span data-stu-id="204f5-414">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="204f5-415">指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。</span><span class="sxs-lookup"><span data-stu-id="204f5-415">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="204f5-416">これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-416">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="204f5-417">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。</span><span class="sxs-lookup"><span data-stu-id="204f5-417">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="204f5-418">最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-418">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="204f5-419">NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="204f5-419">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="204f5-420">NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="204f5-420">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="204f5-421">これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。</span><span class="sxs-lookup"><span data-stu-id="204f5-421">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="204f5-422">作成、 **ViewModels** ViewModel を保持するフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="204f5-422">Create a **ViewModels** folder to hold the ViewModel.</span></span> <span data-ttu-id="204f5-423">これを行うには、最上位レベルを右クリックし**MvcMusicStore**プロジェクトで、**追加**し**新しいフォルダー**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-423">To do this, right-click the top-level **MvcMusicStore** project, select **Add** and then **New Folder**.</span></span>

    <span data-ttu-id="204f5-424">![新しいフォルダーを追加する](aspnet-mvc-4-fundamentals/_static/image17.png "新しいフォルダーを追加します。")</span><span class="sxs-lookup"><span data-stu-id="204f5-424">![Adding a new folder](aspnet-mvc-4-fundamentals/_static/image17.png "Adding a new folder")</span></span>

    <span data-ttu-id="204f5-425">*新しいフォルダーを追加します。*</span><span class="sxs-lookup"><span data-stu-id="204f5-425">*Adding a new folder*</span></span>
4. <span data-ttu-id="204f5-426">名前のフォルダー *ViewModels*です。</span><span class="sxs-lookup"><span data-stu-id="204f5-426">Name the folder *ViewModels*.</span></span>

    <span data-ttu-id="204f5-427">![ソリューション エクスプ ローラーでフォルダーを ViewModels](aspnet-mvc-4-fundamentals/_static/image18.png "ソリューション エクスプ ローラー ViewModels フォルダー")</span><span class="sxs-lookup"><span data-stu-id="204f5-427">![ViewModels folder in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels folder in Solution Explorer")</span></span>

    <span data-ttu-id="204f5-428">*ソリューション エクスプ ローラー ViewModels フォルダー*</span><span class="sxs-lookup"><span data-stu-id="204f5-428">*ViewModels folder in Solution Explorer*</span></span>
5. <span data-ttu-id="204f5-429">作成、 **ViewModel**クラスです。</span><span class="sxs-lookup"><span data-stu-id="204f5-429">Create a **ViewModel** class.</span></span> <span data-ttu-id="204f5-430">これを行うを右クリックし、 **ViewModels**最近作成、フォルダーを選択**追加**し**新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="204f5-430">To do this, right-click on the **ViewModels** folder recently created, select **Add** and then **New Item**.</span></span> <span data-ttu-id="204f5-431">**コード**、選択、**クラス**項目し、ファイルの名前*StoreIndexViewModel.cs*、順にクリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-431">Under **Code**, choose the **Class** item and name the file *StoreIndexViewModel.cs*, then click **Add**.</span></span>

    <span data-ttu-id="204f5-432">![新しいクラスの追加](aspnet-mvc-4-fundamentals/_static/image19.png "新しいクラスの追加")</span><span class="sxs-lookup"><span data-stu-id="204f5-432">![Adding a new Class](aspnet-mvc-4-fundamentals/_static/image19.png "Adding a new Class")</span></span>

    <span data-ttu-id="204f5-433">*新しいクラスの追加*</span><span class="sxs-lookup"><span data-stu-id="204f5-433">*Adding a new Class*</span></span>

    <span data-ttu-id="204f5-434">![StoreIndexViewModel クラスを作成する](aspnet-mvc-4-fundamentals/_static/image20.png "作成 StoreIndexViewModel クラス")</span><span class="sxs-lookup"><span data-stu-id="204f5-434">![Creating StoreIndexViewModel class](aspnet-mvc-4-fundamentals/_static/image20.png "Creating StoreIndexViewModel class")</span></span>

    <span data-ttu-id="204f5-435">*StoreIndexViewModel クラスを作成します。*</span><span class="sxs-lookup"><span data-stu-id="204f5-435">*Creating StoreIndexViewModel class*</span></span>

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a><span data-ttu-id="204f5-436">タスク 2 - ViewModel クラスにプロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="204f5-436">Task 2 - Adding Properties to the ViewModel class</span></span>

<span data-ttu-id="204f5-437">予期される HTML 応答を生成するために、StoreController からビュー テンプレートに渡される 2 つのパラメーターがある: ストアに格納され、ジャンルの一覧のジャンルの数。</span><span class="sxs-lookup"><span data-stu-id="204f5-437">There are two parameters to be passed from the StoreController to the View template in order to generate the expected HTML response: the number of genres in the store and a list of those genres.</span></span>

<span data-ttu-id="204f5-438">このタスクでは、これら 2 つのプロパティを追加する、 **StoreIndexViewModel**クラス: **NumberOfGenres** (整数) と**ジャンル**(文字列の一覧)。</span><span class="sxs-lookup"><span data-stu-id="204f5-438">In this task, you will add those 2 properties to the **StoreIndexViewModel** class: **NumberOfGenres** (an integer) and **Genres** (a list of strings).</span></span>

1. <span data-ttu-id="204f5-439">追加**NumberOfGenres**と**ジャンル**プロパティを**StoreIndexViewModel**クラスです。</span><span class="sxs-lookup"><span data-stu-id="204f5-439">Add **NumberOfGenres** and **Genres** properties to the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="204f5-440">これを行うには、クラス定義に次の 2 行を追加します。</span><span class="sxs-lookup"><span data-stu-id="204f5-440">To do this, add the following 2 lines to the class definition:</span></span>

    <span data-ttu-id="204f5-441">(コード スニペットの*ASP.NET MVC 4 の基礎: Ex5 StoreIndexViewModel プロパティ*)</span><span class="sxs-lookup"><span data-stu-id="204f5-441">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel properties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

> [!NOTE]
> <span data-ttu-id="204f5-442">**{Get; に設定}** 表記法では、# の使用により、自動実装プロパティの機能です。</span><span class="sxs-lookup"><span data-stu-id="204f5-442">The **{ get; set; }** notation makes use of C#'s auto-implemented properties feature.</span></span> <span data-ttu-id="204f5-443">バッキング フィールドを宣言することを必要とせず、プロパティの利点を提供します。</span><span class="sxs-lookup"><span data-stu-id="204f5-443">It provides the benefits of a property without requiring us to declare a backing field.</span></span>

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a><span data-ttu-id="204f5-444">タスク 3 - 更新 StoreController、StoreIndexViewModel を使用するには</span><span class="sxs-lookup"><span data-stu-id="204f5-444">Task 3 - Updating StoreController to use the StoreIndexViewModel</span></span>

<span data-ttu-id="204f5-445">**StoreIndexViewModel**クラスからを渡すために必要な情報をカプセル化**StoreController**の**インデックス**メソッドを応答を生成するためにテンプレートの表示.</span><span class="sxs-lookup"><span data-stu-id="204f5-445">The **StoreIndexViewModel** class encapsulates the information needed to pass from **StoreController**'s **Index** method to a View template in order to generate a response.</span></span>

<span data-ttu-id="204f5-446">このタスクを更新、 **StoreController**を使用する、 **StoreIndexViewModel**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-446">In this task, you will update the **StoreController** to use the **StoreIndexViewModel**.</span></span>

1. <span data-ttu-id="204f5-447">開いている**StoreController**クラスです。</span><span class="sxs-lookup"><span data-stu-id="204f5-447">Open **StoreController** class.</span></span>

    <span data-ttu-id="204f5-448">![StoreController クラスを開いて](aspnet-mvc-4-fundamentals/_static/image21.png "開く StoreController クラス")</span><span class="sxs-lookup"><span data-stu-id="204f5-448">![Opening StoreController class](aspnet-mvc-4-fundamentals/_static/image21.png "Opening StoreController class")</span></span>

    <span data-ttu-id="204f5-449">*開く StoreController クラス*</span><span class="sxs-lookup"><span data-stu-id="204f5-449">*Opening StoreController class*</span></span>
2. <span data-ttu-id="204f5-450">使用するために、 **StoreIndexViewModel**クラス、 **StoreController**の上部にある次の名前空間を追加、 **StoreController**コード。</span><span class="sxs-lookup"><span data-stu-id="204f5-450">In order to use the **StoreIndexViewModel** class from the **StoreController**, add the following namespace at the top of the **StoreController** code:</span></span>

    <span data-ttu-id="204f5-451">(コード スニペットの*ASP.NET MVC 4 の基礎: ViewModels を使用して Ex5 StoreIndexViewModel*)</span><span class="sxs-lookup"><span data-stu-id="204f5-451">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel using ViewModels*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. <span data-ttu-id="204f5-452">変更、 **StoreController**の**インデックス**アクション メソッドを作成して設定ように、 **StoreIndexViewModel**オブジェクトし、をビュー テンプレートに渡します関連付けを HTML 応答を生成します。</span><span class="sxs-lookup"><span data-stu-id="204f5-452">Change the **StoreController**'s **Index** action method so that it creates and populates a **StoreIndexViewModel** object and then passes it off to a View template to generate an HTML response with it.</span></span>

    > [!NOTE]
    > <span data-ttu-id="204f5-453">ラボで&quot;ASP.NET MVC モデルおよびデータ アクセス&quot;は、データベースからストア ジャンルの一覧を取得するコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="204f5-453">In Lab &quot;ASP.NET MVC Models and Data Access&quot; you will write code that retrieves the list of store genres from a database.</span></span> <span data-ttu-id="204f5-454">次のコードでは、作成、**リスト**設定はダミー データ ジャンルの**StoreIndexViewModel**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-454">In the following code, you will create a **List** of dummy data genres that will populate the **StoreIndexViewModel**.</span></span>
    > 
    > <span data-ttu-id="204f5-455">作成し、設定した後、 **StoreIndexViewModel**オブジェクトを引数として渡されます、**ビュー**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="204f5-455">After creating and setting up the **StoreIndexViewModel** object, it will be passed as an argument to the **View** method.</span></span> <span data-ttu-id="204f5-456">これは、ビュー テンプレートがそのオブジェクトを使用して関連付けを HTML 応答を生成することを示します。</span><span class="sxs-lookup"><span data-stu-id="204f5-456">This indicates that the View template will use that object to generate an HTML response with it.</span></span>
4. <span data-ttu-id="204f5-457">置換、**インデックス**メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="204f5-457">Replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="204f5-458">(コード スニペットの*ASP.NET MVC 4 の基礎: Ex5 StoreController インデックス メソッド*)</span><span class="sxs-lookup"><span data-stu-id="204f5-458">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreController Index method*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

> [!NOTE]
> <span data-ttu-id="204f5-459">使用するを想定する可能性があります (C#) を使い慣れていない場合は、 **var**ことを意味、 **viewModel**変数は遅延バインディング。</span><span class="sxs-lookup"><span data-stu-id="204f5-459">If you're unfamiliar with C#, you may assume that using **var** means that the **viewModel** variable is late-bound.</span></span> <span data-ttu-id="204f5-460">正しくありません - c# コンパイラに使用されている型推論、変数に代入する新機能に基づくことがわかった**viewModel**の種類は**StoreIndexViewModel**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-460">That's not correct - the C# compiler is using type-inference based on what you assign to the variable to determine that **viewModel** is of type **StoreIndexViewModel**.</span></span> <span data-ttu-id="204f5-461">ローカルをコンパイルしても、 **viewModel**変数として、 **StoreIndexViewModel** get コンパイル時のチェックと Visual Studio コード エディターのサポートを入力します。</span><span class="sxs-lookup"><span data-stu-id="204f5-461">Also, by compiling the local **viewModel** variable as a **StoreIndexViewModel** type you get compile-time checking and Visual Studio code-editor support.</span></span>

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a><span data-ttu-id="204f5-462">タスク 4 - StoreIndexViewModel を使用する表示テンプレートを作成します。</span><span class="sxs-lookup"><span data-stu-id="204f5-462">Task 4 - Creating a View Template that Uses StoreIndexViewModel</span></span>

<span data-ttu-id="204f5-463">このタスクでは、コント ローラーから渡された StoreIndexViewModel オブジェクトを使用してジャンルの一覧を表示するビュー テンプレートを作成します。</span><span class="sxs-lookup"><span data-stu-id="204f5-463">In this task, you will create a View template that will use a StoreIndexViewModel object passed from the Controller to display a list of genres.</span></span>

1. <span data-ttu-id="204f5-464">新しいテンプレートの表示を作成する前に、プロジェクトを構築してみましょうできるように、**ビューの追加 ダイアログ**知って、 **StoreIndexViewModel**クラスです。</span><span class="sxs-lookup"><span data-stu-id="204f5-464">Before creating the new View template, let's build the project so that the **Add View Dialog** knows about the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="204f5-465">選択して、プロジェクトをビルド、**ビルド**メニュー項目し**ビルド MvcMusicStore**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-465">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="204f5-466">![プロジェクトのビルド](aspnet-mvc-4-fundamentals/_static/image22.png "プロジェクトのビルド")</span><span class="sxs-lookup"><span data-stu-id="204f5-466">![Building the project](aspnet-mvc-4-fundamentals/_static/image22.png "Building the project")</span></span>

    <span data-ttu-id="204f5-467">*プロジェクトのビルド*</span><span class="sxs-lookup"><span data-stu-id="204f5-467">*Building the project*</span></span>
2. <span data-ttu-id="204f5-468">新しいテンプレートの表示を作成します。</span><span class="sxs-lookup"><span data-stu-id="204f5-468">Create a new View template.</span></span> <span data-ttu-id="204f5-469">実行するには、内部を右クリックして、**インデックス**メソッドと選択**ビューの追加**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-469">To do that, right-click inside the **Index** method and select **Add View**.</span></span>

    <span data-ttu-id="204f5-470">![ビューを追加する](aspnet-mvc-4-fundamentals/_static/image23.png "ビューを追加します。")</span><span class="sxs-lookup"><span data-stu-id="204f5-470">![Adding a View](aspnet-mvc-4-fundamentals/_static/image23.png "Adding a View")</span></span>

    <span data-ttu-id="204f5-471">*ビューの追加*</span><span class="sxs-lookup"><span data-stu-id="204f5-471">*Adding a View*</span></span>
3. <span data-ttu-id="204f5-472">**ビューの追加 ダイアログ**から呼び出された、 **StoreController**、既定では、ビュー テンプレートが追加、 **\Views\Store\Index.cshtml**ファイル。</span><span class="sxs-lookup"><span data-stu-id="204f5-472">Because the **Add View Dialog** was invoked from the **StoreController**, it will add the View template by default in a **\Views\Store\Index.cshtml** file.</span></span> <span data-ttu-id="204f5-473">チェック、**ビューを作成する厳密に型指定された -** チェック ボックスをオンし、 **StoreIndexViewModel**として、**モデル クラス**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-473">Check the **Create a strongly-typed-view** checkbox and then select **StoreIndexViewModel** as the **Model class**.</span></span> <span data-ttu-id="204f5-474">また、必ず選択されているビュー エンジンが**Razor**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-474">Also, make sure that the View engine selected is **Razor**.</span></span> <span data-ttu-id="204f5-475">**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="204f5-475">Click **Add**.</span></span>

    <span data-ttu-id="204f5-476">![[追加] ダイアログの表示](aspnet-mvc-4-fundamentals/_static/image24.png "追加ビュー ダイアログ ボックス")</span><span class="sxs-lookup"><span data-stu-id="204f5-476">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image24.png "Add View Dialog")</span></span>

    <span data-ttu-id="204f5-477">*[追加] ダイアログの表示*</span><span class="sxs-lookup"><span data-stu-id="204f5-477">*Add View Dialog*</span></span>

    <span data-ttu-id="204f5-478">**\Views\Store\Index.cshtml**ビュー テンプレート ファイルが作成され、表示します。</span><span class="sxs-lookup"><span data-stu-id="204f5-478">The **\Views\Store\Index.cshtml** View template file is created and opened.</span></span> <span data-ttu-id="204f5-479">提供された情報に基づいて、**ビューの追加**最後の手順では、テンプレートを必要とするビューでダイアログ、 **StoreIndexViewModel**を HTML 応答の生成に使用するデータとしてのインスタンス。</span><span class="sxs-lookup"><span data-stu-id="204f5-479">Based on the information provided to the **Add View** dialog in the last step, the View template will expect a **StoreIndexViewModel** instance as the data to use to generate an HTML response.</span></span> <span data-ttu-id="204f5-480">テンプレートを継承することがわかります、 `ViewPage<musicstore.viewmodels.storeindexviewmodel>` C# の場合。</span><span class="sxs-lookup"><span data-stu-id="204f5-480">You will notice that the template inherits a `ViewPage<musicstore.viewmodels.storeindexviewmodel>` in C#.</span></span>

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a><span data-ttu-id="204f5-481">タスク 5 - ビュー テンプレートの更新</span><span class="sxs-lookup"><span data-stu-id="204f5-481">Task 5 - Updating the View Template</span></span>

<span data-ttu-id="204f5-482">このタスクでは、ジャンルおよびページ内でその名前の数を取得する最後のタスクで作成したビュー テンプレートが更新されます。</span><span class="sxs-lookup"><span data-stu-id="204f5-482">In this task, you will update the View template created in the last task to retrieve the number of genres and their names within the page.</span></span>

> [!NOTE]
> <span data-ttu-id="204f5-483">@ 構文を使用する (と呼ばれます&quot;コード ナゲット&quot;) ビュー テンプレート内でコードを実行します。</span><span class="sxs-lookup"><span data-stu-id="204f5-483">You will use @ syntax (often referred to as &quot;code nuggets&quot;) to execute code within the View template.</span></span>

1. <span data-ttu-id="204f5-484">**Index.cshtml**ファイル内で、**ストア**フォルダー、そのコードを次に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="204f5-484">In the **Index.cshtml** file, within the **Store** folder, replace its code with the following:</span></span>

[!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > As soon as you finish typing the period after the word **Model**, Visual Studio's Intellisense will show a list of possible properties and methods to choose from.
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > *Getting Model properties and methods with Visual Studio's IntelliSense*
    > 
    > The **Model** property references the **StoreIndexViewModel** object that the Controller passed to the View template. This means that you can access all of the data passed from the Controller to the View template via the **Model** property, and format it into an appropriate HTML response within the View template.
    > 
    > You can just select the **NumberOfGenres** property from the Intellisense list rather than typing it in and then it will auto-complete it by pressing the **tab key**.
2. <span data-ttu-id="204f5-485">ジャンルの一覧をループ**StoreIndexViewModel** HTML を作成および**&lt;ul&gt;** を使用して、 **foreach**ループします。</span><span class="sxs-lookup"><span data-stu-id="204f5-485">Loop over the genre list in **StoreIndexViewModel** and create an HTML **&lt;ul&gt;** list using a **foreach** loop.</span></span>
   <span data-ttu-id="204f5-486">(C#)</span><span class="sxs-lookup"><span data-stu-id="204f5-486">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. <span data-ttu-id="204f5-487">キーを押して**f5 キーを押して**アプリケーションを実行し、[参照] を **/格納**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-487">Press **F5** to run the Application and browse **/Store**.</span></span> <span data-ttu-id="204f5-488">渡されたジャンルの一覧が表示されます、 **StoreIndexViewModel**オブジェクトから、 **StoreController**ビュー テンプレートにします。</span><span class="sxs-lookup"><span data-stu-id="204f5-488">You will see the list of genres passed in the **StoreIndexViewModel** object from the **StoreController** to the View template.</span></span>

    <span data-ttu-id="204f5-489">![ジャンルの一覧を表示するビュー](aspnet-mvc-4-fundamentals/_static/image26.png "ジャンルの一覧を表示するビュー")</span><span class="sxs-lookup"><span data-stu-id="204f5-489">![View displaying a list of genres](aspnet-mvc-4-fundamentals/_static/image26.png "View displaying a list of genres")</span></span>

    <span data-ttu-id="204f5-490">*ジャンルの一覧を表示するビュー*</span><span class="sxs-lookup"><span data-stu-id="204f5-490">*View displaying a list of genres*</span></span>
4. <span data-ttu-id="204f5-491">ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="204f5-491">Close the browser.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a><span data-ttu-id="204f5-492">手順 6: ビュー内のパラメーターの使用</span><span class="sxs-lookup"><span data-stu-id="204f5-492">Exercise 6: Using Parameters in View</span></span>

<span data-ttu-id="204f5-493">手順 3 では、コント ローラーにパラメーターを渡す方法について学習しました。</span><span class="sxs-lookup"><span data-stu-id="204f5-493">In Exercise 3 you learned how to pass parameters to the Controller.</span></span> <span data-ttu-id="204f5-494">この演習では、テンプレートの表示でそれらのパラメーターを使用する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="204f5-494">In this exercise, you will learn how to use those parameters in the View template.</span></span> <span data-ttu-id="204f5-495">ため、するは最初に導入モデル クラスをデータとドメインのロジックを管理できるようにします。</span><span class="sxs-lookup"><span data-stu-id="204f5-495">For that purpose, you will be introduced first to Model classes that will help you to manage your data and domain logic.</span></span> <span data-ttu-id="204f5-496">さらに、心配するエンコードの URL パスなどの ASP.NET MVC アプリケーション内のページへのリンクを作成する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="204f5-496">Additionally, you will learn how to create links to pages inside the ASP.NET MVC application without worrying of things like URL paths encoding.</span></span>

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a><span data-ttu-id="204f5-497">タスク 1 - モデル クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="204f5-497">Task 1 - Adding Model Classes</span></span>

<span data-ttu-id="204f5-498">ViewModels、単に情報を渡すコント ローラーからビューに作成されるとは異なり、モデル クラスは、包含してデータとドメイン ロジックを管理する構築されます。</span><span class="sxs-lookup"><span data-stu-id="204f5-498">Unlike ViewModels, which are created just to pass information from the Controller to the View, Model classes are built to contain and manage data and domain logic.</span></span> <span data-ttu-id="204f5-499">このタスクでは、これらの概念を表す 2 つのモデル クラスを追加します:**ジャンル**と**アルバム**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-499">In this task you will add two model classes to represent these concepts: **Genre** and **Album**.</span></span>

1. <span data-ttu-id="204f5-500">まだ開いていない場合は開始**VS Express for Web**</span><span class="sxs-lookup"><span data-stu-id="204f5-500">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="204f5-501">**ファイル**] メニューの [選択**プロジェクトを開く**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-501">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="204f5-502">プロジェクトを開くダイアログ ボックスを参照**Source\Ex06 UsingParametersInView\Begin****[Begin.sln]** をクリック**開く**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-502">In the Open Project dialog, browse to **Source\Ex06-UsingParametersInView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="204f5-503">代わりに、前の手順の完了後に取得したソリューションは、引き続き行えます。</span><span class="sxs-lookup"><span data-stu-id="204f5-503">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="204f5-504">指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。</span><span class="sxs-lookup"><span data-stu-id="204f5-504">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="204f5-505">これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-505">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="204f5-506">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。</span><span class="sxs-lookup"><span data-stu-id="204f5-506">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="204f5-507">最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-507">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="204f5-508">NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="204f5-508">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="204f5-509">NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="204f5-509">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="204f5-510">これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。</span><span class="sxs-lookup"><span data-stu-id="204f5-510">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="204f5-511">追加、**ジャンル**モデル クラス。</span><span class="sxs-lookup"><span data-stu-id="204f5-511">Add a **Genre** Model class.</span></span> <span data-ttu-id="204f5-512">これを行うを右クリックし、**モデル**フォルダーに、**ソリューション エクスプ ローラー****追加**し、**新しい項目の**オプション。</span><span class="sxs-lookup"><span data-stu-id="204f5-512">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="204f5-513">**コード**、選択、**クラス**項目し、ファイルの名前*Genre.cs*、順にクリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-513">Under **Code**, choose the **Class** item and name the file *Genre.cs*, then click **Add**.</span></span>

    <span data-ttu-id="204f5-514">![クラスの追加](aspnet-mvc-4-fundamentals/_static/image27.png "クラスの追加")</span><span class="sxs-lookup"><span data-stu-id="204f5-514">![Adding a class](aspnet-mvc-4-fundamentals/_static/image27.png "Adding a class")</span></span>

    <span data-ttu-id="204f5-515">*新しい項目の追加*</span><span class="sxs-lookup"><span data-stu-id="204f5-515">*Adding a new item*</span></span>

    <span data-ttu-id="204f5-516">![ジャンル モデル クラスを追加](aspnet-mvc-4-fundamentals/_static/image28.png "ジャンル モデル クラスを追加")</span><span class="sxs-lookup"><span data-stu-id="204f5-516">![Add Genre Model Class](aspnet-mvc-4-fundamentals/_static/image28.png "Add Genre Model Class")</span></span>

    <span data-ttu-id="204f5-517">*ジャンル モデル クラスを追加します。*</span><span class="sxs-lookup"><span data-stu-id="204f5-517">*Add Genre Model Class*</span></span>
4. <span data-ttu-id="204f5-518">追加、**名前**ジャンル クラスにプロパティです。</span><span class="sxs-lookup"><span data-stu-id="204f5-518">Add a **Name** property to the Genre class.</span></span> <span data-ttu-id="204f5-519">これを行うには、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="204f5-519">To do this, add the following code:</span></span>

    <span data-ttu-id="204f5-520">(コード スニペットの*ASP.NET MVC 4 の基礎: Ex6 ジャンル*)</span><span class="sxs-lookup"><span data-stu-id="204f5-520">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Genre*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. <span data-ttu-id="204f5-521">次の前と同じ手順を追加、**アルバム**クラスです。</span><span class="sxs-lookup"><span data-stu-id="204f5-521">Following the same procedure as before, add an **Album** class.</span></span> <span data-ttu-id="204f5-522">これを行うを右クリックし、**モデル**フォルダーに、**ソリューション エクスプ ローラー****追加**し、**新しい項目の**オプション。</span><span class="sxs-lookup"><span data-stu-id="204f5-522">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="204f5-523">**コード**、選択、**クラス**項目し、ファイルの名前*Album.cs*、順にクリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-523">Under **Code**, choose the **Class** item and name the file *Album.cs*, then click **Add**.</span></span>
6. <span data-ttu-id="204f5-524">アルバム クラスに次の 2 つのプロパティを追加:**ジャンル**と**タイトル**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-524">Add two properties to the Album class: **Genre** and **Title**.</span></span> <span data-ttu-id="204f5-525">これを行うには、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="204f5-525">To do this, add the following code:</span></span>

    <span data-ttu-id="204f5-526">(コード スニペットの*ASP.NET MVC 4 の基礎: Ex6 アルバム*)</span><span class="sxs-lookup"><span data-stu-id="204f5-526">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Album*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a><span data-ttu-id="204f5-527">タスク 2 -、StoreBrowseViewModel を追加します。</span><span class="sxs-lookup"><span data-stu-id="204f5-527">Task 2 - Adding a StoreBrowseViewModel</span></span>

<span data-ttu-id="204f5-528">A **StoreBrowseViewModel**は選択したジャンルに一致するアルバムを表示するこのタスクで使用します。</span><span class="sxs-lookup"><span data-stu-id="204f5-528">A **StoreBrowseViewModel** will be used in this task to show the Albums that match a selected Genre.</span></span> <span data-ttu-id="204f5-529">このタスクでは、このクラスを作成し、処理する 2 つのプロパティを追加、**ジャンル**とその**アルバム**のリストします。</span><span class="sxs-lookup"><span data-stu-id="204f5-529">In this task, you will create this class and then add two properties to handle the **Genre** and its **Album**'s List.</span></span>

1. <span data-ttu-id="204f5-530">追加、 **StoreBrowseViewModel**クラスです。</span><span class="sxs-lookup"><span data-stu-id="204f5-530">Add a **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="204f5-531">これを行うを右クリックし、 **ViewModels**フォルダーに、**ソリューション エクスプ ローラー****追加**し、**新しい項目の**オプション。</span><span class="sxs-lookup"><span data-stu-id="204f5-531">To do this, right-click the **ViewModels** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="204f5-532">**コード**、選択、**クラス**項目し、ファイルの名前*StoreBrowseViewModel.cs*、順にクリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-532">Under **Code**, choose the **Class** item and name the file *StoreBrowseViewModel.cs*, then click **Add**.</span></span>
2. <span data-ttu-id="204f5-533">モデルへの参照を追加**StoreBrowseViewModel**クラスです。</span><span class="sxs-lookup"><span data-stu-id="204f5-533">Add a reference to the Models in **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="204f5-534">これを行うには、次の追加名前空間を使用します。</span><span class="sxs-lookup"><span data-stu-id="204f5-534">To do this, add the following using namespace:</span></span>

    <span data-ttu-id="204f5-535">(コード スニペットの*ASP.NET MVC 4 の基礎: Ex6 UsingModel*)</span><span class="sxs-lookup"><span data-stu-id="204f5-535">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModel*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. <span data-ttu-id="204f5-536">2 つのプロパティを追加**StoreBrowseViewModel**クラス:**ジャンル**と**アルバム**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-536">Add two properties to **StoreBrowseViewModel** class: **Genre** and **Albums**.</span></span> <span data-ttu-id="204f5-537">これを行うには、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="204f5-537">To do this, add the following code:</span></span>

    <span data-ttu-id="204f5-538">(コード スニペットの*ASP.NET MVC 4 の基礎: Ex6 ModelProperties*)</span><span class="sxs-lookup"><span data-stu-id="204f5-538">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 ModelProperties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

> [!NOTE]
> <span data-ttu-id="204f5-539">新機能**リスト&lt;アルバム&gt;** ?: この定義を使用して、**リスト&lt;T&gt;** 型、場所**T**制約、この要素を型**リスト**ここに属している**アルバム**(またはその子孫のいずれか)。</span><span class="sxs-lookup"><span data-stu-id="204f5-539">What is **List&lt;Album&gt;** ?: This definition is using the **List&lt;T&gt;** type, where **T** constrains the type to which elements of this **List** belong to, in this case **Album** (or any of its descendants).</span></span>
> 
> <span data-ttu-id="204f5-540">このクラスまたはメソッドが宣言されているし、c# 言語の機能は、クライアント コードでインスタンス化されるまでに 1 つまたは複数の種類の仕様を延期するクラスとメソッドをデザインする機能と呼ばれる**ジェネリック**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-540">This ability to design classes and methods that defer the specification of one or more types until the class or method is declared and instantiated by client code is a feature of the C# language called **Generics**.</span></span>
> 
> <span data-ttu-id="204f5-541">**リスト&lt;T&gt;** ジェネリックと同等は、 **ArrayList**を入力しで使用できるは、 **System.Collections.Generic**名前空間。</span><span class="sxs-lookup"><span data-stu-id="204f5-541">**List&lt;T&gt;** is the generic equivalent of the **ArrayList** type and is available in the **System.Collections.Generic** namespace.</span></span> <span data-ttu-id="204f5-542">使用する利点の 1 つ**ジェネリック**いるため、型を指定すると、する必要がない型に要素をキャストなどの操作のチェックを処理する**アルバム**で行うよう**ArrayList**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-542">One of the benefits of using **generics** is that since the type is specified, you do not need to take care of type checking operations such as casting the elements into **Album** as you would do with an **ArrayList**.</span></span>

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a><span data-ttu-id="204f5-543">タスク 3 -、StoreController で新しい ViewModel の使用</span><span class="sxs-lookup"><span data-stu-id="204f5-543">Task 3 - Using the New ViewModel in the StoreController</span></span>

<span data-ttu-id="204f5-544">このタスクでは、変更、 **StoreController**の**参照**と**詳細**のアクション メソッドを使用して、新しい**StoreBrowseViewModel**.</span><span class="sxs-lookup"><span data-stu-id="204f5-544">In this task, you will modify the **StoreController**'s **Browse** and **Details** action methods to use the new **StoreBrowseViewModel**.</span></span>

1. <span data-ttu-id="204f5-545">モデル フォルダーへの参照を追加**StoreController**クラスです。</span><span class="sxs-lookup"><span data-stu-id="204f5-545">Add a reference to the Models folder in **StoreController** class.</span></span> <span data-ttu-id="204f5-546">これを行うには、展開、**コント ローラー**内のフォルダー、**ソリューション エクスプ ローラー**を開くと、 **StoreController**クラスです。</span><span class="sxs-lookup"><span data-stu-id="204f5-546">To do this, expand the **Controllers** folder in the **Solution Explorer** and open the **StoreController** class.</span></span> <span data-ttu-id="204f5-547">次のコードを追加し、します。</span><span class="sxs-lookup"><span data-stu-id="204f5-547">Then add the following code:</span></span>

    <span data-ttu-id="204f5-548">(コード スニペットの*ASP.NET MVC 4 の基礎: Ex6 UsingModelInController*)</span><span class="sxs-lookup"><span data-stu-id="204f5-548">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModelInController*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. <span data-ttu-id="204f5-549">置換、**参照**アクション メソッドを使用する、 **StoreViewBrowseController**クラスです。</span><span class="sxs-lookup"><span data-stu-id="204f5-549">Replace the **Browse** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="204f5-550">ダミーのデータをジャンル、および新しいアルバムの 2 つのオブジェクトを作成します (次のハンズオン ラボでデータベースの実際のデータで使用します)。</span><span class="sxs-lookup"><span data-stu-id="204f5-550">You will create a Genre and two new Albums objects with dummy data (in the next Hands-on Lab you will consume real data from a database).</span></span> <span data-ttu-id="204f5-551">これを行うには、置換、**参照**メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="204f5-551">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="204f5-552">(コード スニペットの*ASP.NET MVC 4 の基礎: Ex6 BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="204f5-552">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. <span data-ttu-id="204f5-553">置換、**詳細**アクション メソッドを使用する、 **StoreViewBrowseController**クラスです。</span><span class="sxs-lookup"><span data-stu-id="204f5-553">Replace the **Details** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="204f5-554">新規に作成する、**アルバム**に返されるオブジェクト、**ビュー**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-554">You will create a new **Album** object to be returned to the **View**.</span></span> <span data-ttu-id="204f5-555">これを行うには、置換、**詳細**メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="204f5-555">To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="204f5-556">(コード スニペットの*ASP.NET MVC 4 の基礎: Ex6 DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="204f5-556">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a><span data-ttu-id="204f5-557">タスク 4 - 参照ビュー テンプレートを追加します。</span><span class="sxs-lookup"><span data-stu-id="204f5-557">Task 4 - Adding a Browse View Template</span></span>

<span data-ttu-id="204f5-558">このタスクでは、追加、**参照**特定ジャンルのアルバムを表示するビュー。</span><span class="sxs-lookup"><span data-stu-id="204f5-558">In this task, you will add a **Browse** View to show the Albums found for a specific Genre.</span></span>

1. <span data-ttu-id="204f5-559">新しいテンプレートの表示を作成する前に、プロジェクトをビルドする必要がありますできるように、**ビューの追加**ダイアログを知って、 **ViewModel**クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="204f5-559">Before creating the new View template, you should build the project so that the **Add View** Dialog knows about the **ViewModel** class to use.</span></span> <span data-ttu-id="204f5-560">選択して、プロジェクトをビルド、**ビルド**メニュー項目し**ビルド MvcMusicStore**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-560">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>
2. <span data-ttu-id="204f5-561">追加、**参照**ビュー。</span><span class="sxs-lookup"><span data-stu-id="204f5-561">Add a **Browse** View.</span></span> <span data-ttu-id="204f5-562">これを行うには、右クリックし、**参照**のアクション メソッド、 **StoreController**  をクリック**ビューの追加**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-562">To do this, right-click in the **Browse** action method of the **StoreController** and click **Add View**.</span></span>
3. <span data-ttu-id="204f5-563">**ビューの追加** ダイアログ ボックスで、ビュー名があることを確認**参照**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-563">In the **Add View** dialog box, verify that the View Name is **Browse**.</span></span> <span data-ttu-id="204f5-564">チェック、**厳密に型指定されたビューを作成する** チェック ボックスを選択し、 **StoreBrowseViewModel**から、**モデル クラス**ドロップダウンします。</span><span class="sxs-lookup"><span data-stu-id="204f5-564">Check the **Create a strongly-typed view** checkbox and select **StoreBrowseViewModel** from the **Model class** dropdown.</span></span> <span data-ttu-id="204f5-565">既定値は、他のフィールドのままにします。</span><span class="sxs-lookup"><span data-stu-id="204f5-565">Leave the other fields with their default value.</span></span> <span data-ttu-id="204f5-566">次に、 **[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="204f5-566">Then click **Add**.</span></span>

    <span data-ttu-id="204f5-567">![[参照] ビューを追加する](aspnet-mvc-4-fundamentals/_static/image29.png "参照ビューの追加")</span><span class="sxs-lookup"><span data-stu-id="204f5-567">![Adding a Browse View](aspnet-mvc-4-fundamentals/_static/image29.png "Adding a Browse View")</span></span>

    <span data-ttu-id="204f5-568">*[参照] ビューの追加*</span><span class="sxs-lookup"><span data-stu-id="204f5-568">*Adding a Browse View*</span></span>
4. <span data-ttu-id="204f5-569">変更、 **Browse.cshtml**ジャンルの情報を表示するへのアクセス、 **StoreBrowseViewModel**ビュー テンプレートに渡されるオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="204f5-569">Modify the **Browse.cshtml** to display the Genre's information, accessing the **StoreBrowseViewModel** object that is passed to the view template.</span></span> <span data-ttu-id="204f5-570">これを行うには、コンテンツを次に置き換えます: (c#)</span><span class="sxs-lookup"><span data-stu-id="204f5-570">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="204f5-571">タスク 5 - アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="204f5-571">Task 5 - Running the Application</span></span>

<span data-ttu-id="204f5-572">このタスクは、テストを**参照**メソッドからアルバムの取得、**参照**メソッドのアクション。</span><span class="sxs-lookup"><span data-stu-id="204f5-572">In this task, you will test that the **Browse** method retrieves Albums from the **Browse** method action.</span></span>

1. <span data-ttu-id="204f5-573">キーを押して**f5 キーを押して**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="204f5-573">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="204f5-574">ホーム ページで、プロジェクトを開始します。</span><span class="sxs-lookup"><span data-stu-id="204f5-574">The project starts in the Home page.</span></span> <span data-ttu-id="204f5-575">URL を変更して  **/格納/参照しますか?ジャンル Disco を =** アクションが 2 つのアルバムを返すことを確認します。</span><span class="sxs-lookup"><span data-stu-id="204f5-575">Change the URL to **/Store/Browse?Genre=Disco** to verify that the action returns two Albums.</span></span>

    <span data-ttu-id="204f5-576">![ストア Disco アルバムを閲覧](aspnet-mvc-4-fundamentals/_static/image30.png "ストア Disco アルバムを参照")</span><span class="sxs-lookup"><span data-stu-id="204f5-576">![Browsing Store Disco Albums](aspnet-mvc-4-fundamentals/_static/image30.png "Browsing Store Disco Albums")</span></span>

    <span data-ttu-id="204f5-577">*ストア Disco アルバムを参照*</span><span class="sxs-lookup"><span data-stu-id="204f5-577">*Browsing Store Disco Albums*</span></span>

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a><span data-ttu-id="204f5-578">タスク 6: を表示する情報は、特定のアルバム</span><span class="sxs-lookup"><span data-stu-id="204f5-578">Task 6 - Displaying information About a Specific Album</span></span>

<span data-ttu-id="204f5-579">このタスクでは、実装、**ストア/詳細**特定のアルバムに関する情報を表示するビュー。</span><span class="sxs-lookup"><span data-stu-id="204f5-579">In this task, you will implement the **Store/Details** view to display information about a specific album.</span></span> <span data-ttu-id="204f5-580">このハンズオン ラボでは、そのアルバムに関する表示すべてに既に含まれて、**ビュー**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="204f5-580">In this Hands-on Lab, everything you will display about the album is already contained in the **View** template.</span></span> <span data-ttu-id="204f5-581">作成する代わりに、 **StoreDetailsViewModel**クラスは現在使用して**StoreBrowseViewModel**アルバムを渡すテンプレート。</span><span class="sxs-lookup"><span data-stu-id="204f5-581">So, instead of creating a **StoreDetailsViewModel** class, you will use the current **StoreBrowseViewModel** template passing the Album to it.</span></span>

1. <span data-ttu-id="204f5-582">Visual Studio のウィンドウに戻るには、必要な場合は、ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="204f5-582">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="204f5-583">新しい**詳細**の表示、 **StoreController**の**詳細**アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="204f5-583">Add a new **Details** view for the **StoreController**'s **Details** action method.</span></span> <span data-ttu-id="204f5-584">これを行うを右クリックし、**詳細**メソッドで、 **StoreController**クラスし、をクリックして**ビューの追加**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-584">To do this, right-click the **Details** method in the **StoreController** class and click **Add View**.</span></span>
2. <span data-ttu-id="204f5-585">**ビューの追加**ダイアログ ボックスで、いることを確認、**ビュー名**は**詳細**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-585">In the **Add View** dialog, verify that the **View Name** is **Details**.</span></span> <span data-ttu-id="204f5-586">チェック、**厳密に型指定されたビューを作成する** チェック ボックスを選択し、**アルバム**から、**モデル クラス**ドロップダウンします。</span><span class="sxs-lookup"><span data-stu-id="204f5-586">Check the **Create a strongly-typed view** checkbox and select **Album** from the **Model class** drop-down.</span></span> <span data-ttu-id="204f5-587">既定値は、他のフィールドのままにします。</span><span class="sxs-lookup"><span data-stu-id="204f5-587">Leave the other fields with their default value.</span></span> <span data-ttu-id="204f5-588">次に、 **[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="204f5-588">Then click **Add**.</span></span> <span data-ttu-id="204f5-589">これが作成され、 **\Views\Store\Details.cshtml**ファイル。</span><span class="sxs-lookup"><span data-stu-id="204f5-589">This will create and open a **\Views\Store\Details.cshtml** file.</span></span>

    <span data-ttu-id="204f5-590">![詳細ビューを追加する](aspnet-mvc-4-fundamentals/_static/image31.png "詳細ビューを追加します。")</span><span class="sxs-lookup"><span data-stu-id="204f5-590">![Adding a Details View](aspnet-mvc-4-fundamentals/_static/image31.png "Adding a Details View")</span></span>

    <span data-ttu-id="204f5-591">*詳細ビューを追加します。*</span><span class="sxs-lookup"><span data-stu-id="204f5-591">*Adding a Details View*</span></span>
3. <span data-ttu-id="204f5-592">変更、 **Details.cshtml**アルバムの情報を表示するファイルへのアクセス、**アルバム**ビュー テンプレートに渡されるオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="204f5-592">Modify the **Details.cshtml** file to display the Album's information, accessing the **Album** object that is passed to the view template.</span></span> <span data-ttu-id="204f5-593">これを行うには、コンテンツを次に置き換えます: (c#)</span><span class="sxs-lookup"><span data-stu-id="204f5-593">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="204f5-594">タスク 7 - アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="204f5-594">Task 7 - Running the Application</span></span>

<span data-ttu-id="204f5-595">このタスクは、テストを**詳細**ビューからアルバムの情報を取得する、**アクションの詳細を**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="204f5-595">In this task, you will test that the **Details** View retrieves Album's information from the **Details action** method.</span></span>

1. <span data-ttu-id="204f5-596">キーを押して**f5 キーを押して**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="204f5-596">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="204f5-597">プロジェクトを起動、**ホーム**ページ。</span><span class="sxs-lookup"><span data-stu-id="204f5-597">The project starts in the **Home** page.</span></span> <span data-ttu-id="204f5-598">URL を変更して **/Store/Details/5**アルバムの情報を確認します。</span><span class="sxs-lookup"><span data-stu-id="204f5-598">Change the URL to **/Store/Details/5** to verify the album's information.</span></span>

    <span data-ttu-id="204f5-599">![アルバムの詳細を参照](aspnet-mvc-4-fundamentals/_static/image32.png "アルバムの詳細を参照")</span><span class="sxs-lookup"><span data-stu-id="204f5-599">![Browsing Albums Detail](aspnet-mvc-4-fundamentals/_static/image32.png "Browsing Albums Detail")</span></span>

    <span data-ttu-id="204f5-600">*アルバムの詳細を参照*</span><span class="sxs-lookup"><span data-stu-id="204f5-600">*Browsing Album's Detail*</span></span>

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a><span data-ttu-id="204f5-601">タスク 8 - ページ間のリンクの追加</span><span class="sxs-lookup"><span data-stu-id="204f5-601">Task 8 - Adding Links Between Pages</span></span>

<span data-ttu-id="204f5-602">このタスクでは、適切な各ジャンル名にリンクがあるストア ビューでリンクを追加します**ストア/参照**URL。</span><span class="sxs-lookup"><span data-stu-id="204f5-602">In this task, you will add a link in the Store View to have a link in every Genre name to the appropriate **/Store/Browse** URL.</span></span> <span data-ttu-id="204f5-603">これにより、たとえば、ジャンルをクリックすると**Disco**に移動**ストア/参照? ジャンル Disco を =** URL。</span><span class="sxs-lookup"><span data-stu-id="204f5-603">This way, when you click on a Genre, for instance **Disco**, it will navigate to **/Store/Browse?genre=Disco** URL.</span></span>

1. <span data-ttu-id="204f5-604">Visual Studio のウィンドウに戻るには、必要な場合は、ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="204f5-604">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="204f5-605">更新プログラム、**インデックス**へのリンクを追加するページ、**参照**ページ。</span><span class="sxs-lookup"><span data-stu-id="204f5-605">Update the **Index** page to add a link to the **Browse** page.</span></span> <span data-ttu-id="204f5-606">これを行うで、**ソリューション エクスプ ローラー**展開、**ビュー**フォルダー、**ストア**フォルダーをダブルクリック、 **Index.cshtml**ページ。</span><span class="sxs-lookup"><span data-stu-id="204f5-606">To do this, in the **Solution Explorer** expand the **Views** folder, then the **Store** folder and double-click the **Index.cshtml** page.</span></span>
2. <span data-ttu-id="204f5-607">選択したジャンルを示す参照ビューへのリンクを追加します。</span><span class="sxs-lookup"><span data-stu-id="204f5-607">Add a link to the Browse view indicating the genre selected.</span></span> <span data-ttu-id="204f5-608">これを行うには、次の強調表示されたコード内を置き換える、 **&lt;li&gt;** タグ: (c#)</span><span class="sxs-lookup"><span data-stu-id="204f5-608">To do this, replace the following highlighted code within the **&lt;li&gt;** tags: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

   > [!NOTE]
   > <span data-ttu-id="204f5-609">別の方法としては ページで、次のようなコードに直接リンクは。</span><span class="sxs-lookup"><span data-stu-id="204f5-609">another approach would be linking directly to the page, with a code like the following:</span></span>
   > 
   > <span data-ttu-id="204f5-610">&lt;href =&quot;ストア/参照? ジャンル =@genreName&quot;&gt;@genreName&lt;/a&gt;</span><span class="sxs-lookup"><span data-stu-id="204f5-610">&lt;a href=&quot;/Store/Browse?genre=@genreName&quot;&gt;@genreName&lt;/a&gt;</span></span>
   > 
   > <span data-ttu-id="204f5-611">このアプローチでも動作しますが、ハードコーディングされた文字列に依存します。</span><span class="sxs-lookup"><span data-stu-id="204f5-611">Although this approach works, it depends on a hardcoded string.</span></span> <span data-ttu-id="204f5-612">コント ローラーを後で変更した場合は、この命令を手動で変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="204f5-612">If you later rename the Controller, you will have to change this instruction manually.</span></span> <span data-ttu-id="204f5-613">優れた代替手段は、使用する、 **HTML ヘルパー**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="204f5-613">A better alternative is to use an **HTML Helper** method.</span></span> <span data-ttu-id="204f5-614">ASP.NET MVC には、このようなタスクを使用できる HTML ヘルパー メソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="204f5-614">ASP.NET MVC includes an HTML Helper method which is available for such tasks.</span></span> <span data-ttu-id="204f5-615">**Html.ActionLink()** ヘルパー メソッドでは、容易に HTML を構築する**&lt;、&gt;** リンク、URL パスは、正しく URL エンコードされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="204f5-615">The **Html.ActionLink()** helper method makes it easy to build HTML **&lt;a&gt;** links, making sure URL paths are properly URL encoded.</span></span>
   > 
   > <span data-ttu-id="204f5-616">Htlm.ActionLink には、いくつかのオーバー ロードがあります。</span><span class="sxs-lookup"><span data-stu-id="204f5-616">Htlm.ActionLink has several overloads.</span></span> <span data-ttu-id="204f5-617">この演習では、次の 3 つのパラメーターを受け取るいずれかを使用します。</span><span class="sxs-lookup"><span data-stu-id="204f5-617">In this exercise you will use one that takes three parameters:</span></span>
   > 
   > 1. <span data-ttu-id="204f5-618">リンク テキストは、ジャンル名前が表示されます。</span><span class="sxs-lookup"><span data-stu-id="204f5-618">Link text, which will display the Genre name</span></span>
   > 2. <span data-ttu-id="204f5-619">コント ローラー アクションの名前 (**参照**)</span><span class="sxs-lookup"><span data-stu-id="204f5-619">Controller action name (**Browse**)</span></span>
   > 3. <span data-ttu-id="204f5-620">ルート名の両方を指定する、パラメーター値 (**ジャンル**) と値 (**ジャンル名**)</span><span class="sxs-lookup"><span data-stu-id="204f5-620">Route parameter values, specifying both the name (**Genre**) and the value (**Genre name**)</span></span>

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a><span data-ttu-id="204f5-621">タスク 9 - アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="204f5-621">Task 9 - Running the Application</span></span>

<span data-ttu-id="204f5-622">このタスクで各ジャンルに適切なへのリンクが表示されることをテストは**ストア/参照**URL。</span><span class="sxs-lookup"><span data-stu-id="204f5-622">In this task, you will test that each Genre is displayed with a link to the appropriate **/Store/Browse** URL.</span></span>

1. <span data-ttu-id="204f5-623">キーを押して**f5 キーを押して**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="204f5-623">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="204f5-624">ホーム ページで、プロジェクトを開始します。</span><span class="sxs-lookup"><span data-stu-id="204f5-624">The project starts in the Home page.</span></span> <span data-ttu-id="204f5-625">URL を変更して **/格納**を適切な各ジャンルにリンクしていることを確認する**ストア/参照**URL。</span><span class="sxs-lookup"><span data-stu-id="204f5-625">Change the URL to **/Store** to verify that each Genre links to the appropriate **/Store/Browse** URL.</span></span>

    <span data-ttu-id="204f5-626">![[参照] ページへのリンクをジャンルをブラウズ](aspnet-mvc-4-fundamentals/_static/image33.png "と参照ページへのリンクのジャンルの参照")</span><span class="sxs-lookup"><span data-stu-id="204f5-626">![Browsing Genres with links to Browse page](aspnet-mvc-4-fundamentals/_static/image33.png "Browsing Genres with links to Browse page")</span></span>

    <span data-ttu-id="204f5-627">*[参照] ページへのリンクをジャンルをブラウズ*</span><span class="sxs-lookup"><span data-stu-id="204f5-627">*Browsing Genres with links to Browse page*</span></span>

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a><span data-ttu-id="204f5-628">タスク 10 - の値を渡すを使用して動的 ViewModel コレクション</span><span class="sxs-lookup"><span data-stu-id="204f5-628">Task 10 - Using Dynamic ViewModel Collection to Pass Values</span></span>

<span data-ttu-id="204f5-629">このタスクでは、モデルの変更を加えずに、コント ローラーとビューの間の値を渡すためのシンプルかつ強力なメソッドを学習します。</span><span class="sxs-lookup"><span data-stu-id="204f5-629">In this task, you will learn a simple and powerful method to pass values between the Controller and the View without making any changes in the Model.</span></span> <span data-ttu-id="204f5-630">ASP.NET MVC 4 は、コレクションを提供します。 &quot;ViewModel&quot;、動的な値に割り当てられコント ローラーとビューも体内でアクセスするには、をします。</span><span class="sxs-lookup"><span data-stu-id="204f5-630">ASP.NET MVC 4 provides the collection &quot;ViewModel&quot;, which can be assigned to any dynamic value and accessed inside controllers and views as well.</span></span>

<span data-ttu-id="204f5-631">一覧を渡す ViewBag 動的コレクションを使用する、今すぐ&quot;**ジャンルの星型**&quot;ビューに、コント ローラーからです。</span><span class="sxs-lookup"><span data-stu-id="204f5-631">You will now use the ViewBag dynamic collection to pass a list of &quot;**Starred genres**&quot; from the controller to the view.</span></span> <span data-ttu-id="204f5-632">アクセスは、ストアのインデックス ビュー **ViewModel**し、情報を表示します。</span><span class="sxs-lookup"><span data-stu-id="204f5-632">The Store Index view will access to **ViewModel** and display the information.</span></span>

1. <span data-ttu-id="204f5-633">Visual Studio のウィンドウに戻るには、必要な場合は、ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="204f5-633">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="204f5-634">開いている**StoreController.cs**および変更**インデックス**の一覧を作成するメソッドでは、ジャンルを放映 ViewModel コレクションに。</span><span class="sxs-lookup"><span data-stu-id="204f5-634">Open **StoreController.cs** and modify **Index** method to create a list of starred genres into ViewModel collection :</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > <span data-ttu-id="204f5-635">構文を使用することも**ViewBag [&quot;Starred&quot;]** プロパティにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="204f5-635">You could also use the syntax **ViewBag[&quot;Starred&quot;]** to access the properties.</span></span>
2. <span data-ttu-id="204f5-636">星アイコン**&quot;starred.png&quot;** に含まれる、 **Source\Assets\Images**このラボのフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="204f5-636">The star icon **&quot;starred.png&quot;** is included in the **Source\Assets\Images** folder of this lab.</span></span> <span data-ttu-id="204f5-637">これをアプリケーションに追加するためにからコンテンツをドラッグして、 **Windows エクスプ ローラー**ウィンドウに、**ソリューション エクスプ ローラー** Visual Web Developer Express、次に示すように。</span><span class="sxs-lookup"><span data-stu-id="204f5-637">In order to add it to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Web Developer Express, as shown below:</span></span>

    <span data-ttu-id="204f5-638">![ソリューションに追加するスター イメージ](aspnet-mvc-4-fundamentals/_static/image34.png "をソリューションに追加するスター イメージ")</span><span class="sxs-lookup"><span data-stu-id="204f5-638">![Adding star image to the solution](aspnet-mvc-4-fundamentals/_static/image34.png "Adding star image to the solution")</span></span>

    <span data-ttu-id="204f5-639">*星の画像をソリューションに追加します。*</span><span class="sxs-lookup"><span data-stu-id="204f5-639">*Adding star image to the solution*</span></span>
3. <span data-ttu-id="204f5-640">ビューを開く**Store/Index.cshtml**およびコンテンツを変更します。</span><span class="sxs-lookup"><span data-stu-id="204f5-640">Open the view **Store/Index.cshtml** and modify the content.</span></span> <span data-ttu-id="204f5-641">読み取りが、&quot;放映&quot;プロパティに、 **ViewBag**コレクション、ジャンルの現在の名前が一覧にかどうかを依頼してください。</span><span class="sxs-lookup"><span data-stu-id="204f5-641">You will read the &quot;starred&quot; property in the **ViewBag** collection, and ask if the current genre name is in the list.</span></span> <span data-ttu-id="204f5-642">その場合は、ジャンル リンクを右星形のアイコンが表示されます。</span><span class="sxs-lookup"><span data-stu-id="204f5-642">In that case you will show a star icon right to the genre link.</span></span>
   <span data-ttu-id="204f5-643">(C#)</span><span class="sxs-lookup"><span data-stu-id="204f5-643">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a><span data-ttu-id="204f5-644">タスク 11 - アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="204f5-644">Task 11 - Running the Application</span></span>

<span data-ttu-id="204f5-645">このタスクでは、星印が付いてジャンルが星形のアイコンを表示することをテストします。</span><span class="sxs-lookup"><span data-stu-id="204f5-645">In this task, you will test that the starred genres display a star icon.</span></span>

1. <span data-ttu-id="204f5-646">キーを押して**f5 キーを押して**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="204f5-646">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="204f5-647">プロジェクトを起動、**ホーム**ページ。</span><span class="sxs-lookup"><span data-stu-id="204f5-647">The project starts in the **Home** page.</span></span> <span data-ttu-id="204f5-648">URL を変更して **/格納**おすすめ各ジャンルに重視のラベルがあることを確認します。</span><span class="sxs-lookup"><span data-stu-id="204f5-648">Change the URL to **/Store** to verify that each featured genre has the respecting label:</span></span>

    <span data-ttu-id="204f5-649">![星印が付いて要素を持つジャンルをブラウズ](aspnet-mvc-4-fundamentals/_static/image35.png "星印が付いて要素を持つジャンルの参照")</span><span class="sxs-lookup"><span data-stu-id="204f5-649">![Browsing Genres with starred elements](aspnet-mvc-4-fundamentals/_static/image35.png "Browsing Genres with starred elements")</span></span>

    <span data-ttu-id="204f5-650">*星印が付いて要素を持つジャンルをブラウズ*</span><span class="sxs-lookup"><span data-stu-id="204f5-650">*Browsing Genres with starred elements*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a><span data-ttu-id="204f5-651">手順 7: ASP.NET MVC 4 の新しいテンプレートの周囲膝</span><span class="sxs-lookup"><span data-stu-id="204f5-651">Exercise 7: A lap around ASP.NET MVC 4 new template</span></span>

<span data-ttu-id="204f5-652">この演習では調査、ASP.NET MVC 4 プロジェクト テンプレートの拡張機能を参照して、新しいテンプレートに最も関連する機能を取得します。</span><span class="sxs-lookup"><span data-stu-id="204f5-652">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 project templates, taking a look at the most relevant features of the new template.</span></span>

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a><span data-ttu-id="204f5-653">タスク 1: ASP.NET MVC 4 のインターネット アプリケーション テンプレートの表示</span><span class="sxs-lookup"><span data-stu-id="204f5-653">Task 1: Exploring the ASP.NET MVC 4 Internet Application Template</span></span>

1. <span data-ttu-id="204f5-654">まだ開いていない場合は開始**VS Express for Web**</span><span class="sxs-lookup"><span data-stu-id="204f5-654">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="204f5-655">選択、**ファイル |新しい |プロジェクト**メニュー コマンド。</span><span class="sxs-lookup"><span data-stu-id="204f5-655">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="204f5-656">**新しいプロジェクト**ダイアログで、選択、 **Visual c# |Web**左側のウィンドウでテンプレート ツリー、および選択、 **ASP.NET MVC 4 Web Application**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-656">In the **New Project** dialog, select the **Visual C#|Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="204f5-657">**名前**プロジェクト*MusicStore*し、更新、**ソリューション名**に*開始*、し、場所を選択 (または既定値) をクリック**ok**.</span><span class="sxs-lookup"><span data-stu-id="204f5-657">**Name** the project *MusicStore* and update the **solution name** to *Begin*, then select a location (or leave the default) and click **OK**.</span></span>

    <span data-ttu-id="204f5-658">![新しい ASP.NET MVC 4 プロジェクトを作成する](aspnet-mvc-4-fundamentals/_static/image36.png "新しい ASP.NET MVC 4 プロジェクトを作成します。")</span><span class="sxs-lookup"><span data-stu-id="204f5-658">![Creating a new ASP.NET MVC 4 Project](aspnet-mvc-4-fundamentals/_static/image36.png "Creating a new ASP.NET MVC 4 Project")</span></span>

    <span data-ttu-id="204f5-659">*新しい ASP.NET MVC 4 プロジェクトを作成します。*</span><span class="sxs-lookup"><span data-stu-id="204f5-659">*Creating a new ASP.NET MVC 4 Project*</span></span>
3. <span data-ttu-id="204f5-660">**新しい ASP.NET MVC 4 プロジェクト**ダイアログで、選択、**インターネット アプリケーション**プロジェクト テンプレートをクリック**OK**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-660">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="204f5-661">ビュー エンジンとして Razor または ASPX のいずれかを選択できますに注意してください。</span><span class="sxs-lookup"><span data-stu-id="204f5-661">Notice you can select either Razor or ASPX as the view engine.</span></span>

    <span data-ttu-id="204f5-662">![新しい ASP.NET MVC 4 インターネット アプリケーションを作成する](aspnet-mvc-4-fundamentals/_static/image37.png "新しい ASP.NET MVC 4 インターネット アプリケーションの作成")</span><span class="sxs-lookup"><span data-stu-id="204f5-662">![Creating a new ASP.NET MVC 4 Internet Application](aspnet-mvc-4-fundamentals/_static/image37.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="204f5-663">*新しい ASP.NET MVC 4 インターネット アプリケーションの作成*</span><span class="sxs-lookup"><span data-stu-id="204f5-663">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="204f5-664">ASP.NET MVC 3 razor 構文が導入されました。</span><span class="sxs-lookup"><span data-stu-id="204f5-664">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="204f5-665">その目的は、文字の数および fast およびワークフローをコーディングする流体を有効にすると、ファイルに必要なキーストロークを最小限に抑えるです。</span><span class="sxs-lookup"><span data-stu-id="204f5-665">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="204f5-666">Razor 既存 C #/vb (またはその他) を活用して言語スキルと優れた HTML 構築ワークフローを使用できるテンプレートのマークアップ構文を配信します。</span><span class="sxs-lookup"><span data-stu-id="204f5-666">Razor leverages existing C#/VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="204f5-667">キーを押して**f5 キーを押して**にソリューションを実行し、更新されたテンプレートを表示します。</span><span class="sxs-lookup"><span data-stu-id="204f5-667">Press **F5** to run the solution and see the renewed template.</span></span> <span data-ttu-id="204f5-668">次の機能を確認することができます。</span><span class="sxs-lookup"><span data-stu-id="204f5-668">You can check out the following features:</span></span>

    1. <span data-ttu-id="204f5-669">**モダン スタイル テンプレート**</span><span class="sxs-lookup"><span data-stu-id="204f5-669">**Modern-style templates**</span></span>

        <span data-ttu-id="204f5-670">テンプレートが更新されました、モダン外見の他のスタイルを提供することです。</span><span class="sxs-lookup"><span data-stu-id="204f5-670">The templates have been renewed, providing more modern-looking styles.</span></span>

        <span data-ttu-id="204f5-671">![ASP.NET MVC 4 のスタイルを変更テンプレート](aspnet-mvc-4-fundamentals/_static/image38.png "ASP.NET MVC 4 のテンプレートのスタイルを変更")</span><span class="sxs-lookup"><span data-stu-id="204f5-671">![ASP.NET MVC 4 restyled templates](aspnet-mvc-4-fundamentals/_static/image38.png "ASP.NET MVC 4 restyled templates")</span></span>

        <span data-ttu-id="204f5-672">*ASP.NET MVC 4 のスタイルを変更テンプレート*</span><span class="sxs-lookup"><span data-stu-id="204f5-672">*ASP.NET MVC 4 restyled templates*</span></span>
    2. <span data-ttu-id="204f5-673">**アダプティブ レンダリング**</span><span class="sxs-lookup"><span data-stu-id="204f5-673">**Adaptive Rendering**</span></span>

        <span data-ttu-id="204f5-674">チェック アウト、ブラウザー ウィンドウのサイズを変更し、どのページ レイアウトに動的に適応新しいウィンドウのサイズに注意してください。</span><span class="sxs-lookup"><span data-stu-id="204f5-674">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="204f5-675">これらのテンプレートは、アダプティブ レンダリング手法をカスタマイズすることがなく、デスクトップおよびモバイルの両方のプラットフォームで正しく表示するために使用します。</span><span class="sxs-lookup"><span data-stu-id="204f5-675">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

        <span data-ttu-id="204f5-676">![ASP.NET MVC 4 プロジェクト テンプレートを別のブラウザーのサイズを](aspnet-mvc-4-fundamentals/_static/image39.png "別のブラウザーのサイズで ASP.NET MVC 4 プロジェクト テンプレート")</span><span class="sxs-lookup"><span data-stu-id="204f5-676">![ASP.NET MVC 4 project template in different browser sizes](aspnet-mvc-4-fundamentals/_static/image39.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

        <span data-ttu-id="204f5-677">*別のブラウザーのサイズで ASP.NET MVC 4 プロジェクト テンプレート*</span><span class="sxs-lookup"><span data-stu-id="204f5-677">*ASP.NET MVC 4 project template in different browser sizes*</span></span>
5. <span data-ttu-id="204f5-678">デバッガーを停止し、Visual Studio に戻り、ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="204f5-678">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="204f5-679">これで、ソリューションを探索し、いくつかのプロジェクト テンプレートの ASP.NET MVC 4 で導入された新しい機能を確認することができます。</span><span class="sxs-lookup"><span data-stu-id="204f5-679">Now you are able to explore the solution and check out some of the new features introduced by ASP.NET MVC 4 in the project template.</span></span>

    <span data-ttu-id="204f5-680">![ASP.NET MVC4 のインターネット-アプリケーション-プロジェクトのテンプレート](aspnet-mvc-4-fundamentals/_static/image40.png "ASP.NET MVC 4 インターネット アプリケーションのプロジェクト テンプレート")</span><span class="sxs-lookup"><span data-stu-id="204f5-680">![ASP.NET MVC4-internet-application-project-template](aspnet-mvc-4-fundamentals/_static/image40.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

    <span data-ttu-id="204f5-681">*ASP.NET MVC 4 インターネット アプリケーション プロジェクト テンプレート*</span><span class="sxs-lookup"><span data-stu-id="204f5-681">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

   1. <span data-ttu-id="204f5-682">**HTML5 マークアップ**</span><span class="sxs-lookup"><span data-stu-id="204f5-682">**HTML5 markup**</span></span>

       <span data-ttu-id="204f5-683">テンプレート ビューを開くなど、新しいテーマ マークアップを調べるには参照**About.cshtml**内で表示**ホーム**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="204f5-683">Browse template views to find out the new theme markup, for example open **About.cshtml** view within **Home** folder.</span></span>

       <span data-ttu-id="204f5-684">![Razor、HTML5 のマークアップを使用して、新しいテンプレート](aspnet-mvc-4-fundamentals/_static/image41.png "Razor、HTML5 のマークアップを使用して、新しいテンプレート")</span><span class="sxs-lookup"><span data-stu-id="204f5-684">![New template, using Razor and HTML5 markup](aspnet-mvc-4-fundamentals/_static/image41.png "New template, using Razor and HTML5 markup")</span></span>

       <span data-ttu-id="204f5-685">*Razor、HTML5 のマークアップを使用して、新しいテンプレート*</span><span class="sxs-lookup"><span data-stu-id="204f5-685">*New template, using Razor and HTML5 markup*</span></span>
   2. <span data-ttu-id="204f5-686">**JavaScript ライブラリが含まれています。**</span><span class="sxs-lookup"><span data-stu-id="204f5-686">**JavaScript libraries included**</span></span>

      1. <span data-ttu-id="204f5-687">**jQuery**: jQuery HTML ドキュメントを通過する、イベント処理、アニメーション、および Ajax の相互作用を簡略化します。</span><span class="sxs-lookup"><span data-stu-id="204f5-687">**jQuery**: jQuery simplifies HTML document traversing, event handling, animating, and Ajax interactions.</span></span>
      2. <span data-ttu-id="204f5-688">**jQuery UI**: このライブラリには、低レベルの相互作用、アニメーション、影響の詳細およびテーマを指定ウィジェット、jQuery JavaScript ライブラリ上に構築される抽象クラスが用意されています。</span><span class="sxs-lookup"><span data-stu-id="204f5-688">**jQuery UI**: This library provides abstractions for low-level interaction and animation, advanced effects and themeable widgets, built on top of the jQuery JavaScript Library.</span></span>

         > [!NOTE]
         > <span data-ttu-id="204f5-689">JQuery と jQuery UI について学習できますで[ [ http://docs.jquery.com/ ](http://docs.jquery.com/)](http://docs.jquery.com/)です。</span><span class="sxs-lookup"><span data-stu-id="204f5-689">You can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>
      3. <span data-ttu-id="204f5-690">**KnockoutJS**:: ASP.NET MVC 4 の既定のテンプレートが含まれています**KnockoutJS**JavaScript と HTML を使用してリッチと応答性の高い web アプリケーションを作成できる JavaScript MVVM フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="204f5-690">**KnockoutJS**: The ASP.NET MVC 4 default template now includes **KnockoutJS**, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="204f5-691">ように ASP.NET MVC 3、jQuery、jQuery UI ライブラリも含まれます ASP.NET MVC 4。</span><span class="sxs-lookup"><span data-stu-id="204f5-691">Like in ASP.NET MVC 3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

          > [!NOTE]
          > <span data-ttu-id="204f5-692">このリンクに KnockOutJS ライブラリに関する詳しい情報を入手できます: [ http://learn.knockoutjs.com/](http://learn.knockoutjs.com/)です。</span><span class="sxs-lookup"><span data-stu-id="204f5-692">You can get more information about KnockOutJS library in this link: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).</span></span>
      4. <span data-ttu-id="204f5-693">**Modernizr**: HTML5 および css3 用のテクノロジを使用するときに、サイトの古いブラウザーとの互換性を行うこのライブラリが自動的に実行されます。</span><span class="sxs-lookup"><span data-stu-id="204f5-693">**Modernizr**: This library runs automatically, making your site compatible with older browsers when using HTML5 and CSS3 technologies.</span></span>

          > [!NOTE]
          > <span data-ttu-id="204f5-694">このリンクに Modernizr ライブラリに関する詳しい情報を入手できます: [ http://www.modernizr.com/](http://www.modernizr.com/)です。</span><span class="sxs-lookup"><span data-stu-id="204f5-694">You can get more information about Modernizr library in this link: [http://www.modernizr.com/](http://www.modernizr.com/).</span></span>
   3. <span data-ttu-id="204f5-695">**ソリューションに含まれる SimpleMembership**</span><span class="sxs-lookup"><span data-stu-id="204f5-695">**SimpleMembership included in the solution**</span></span>

       <span data-ttu-id="204f5-696">SimpleMembership は、以前の ASP.NET ロールおよびメンバーシップ プロバイダーのシステムの代わりとして設計されています。</span><span class="sxs-lookup"><span data-stu-id="204f5-696">SimpleMembership has been designed as a replacement for the previous ASP.NET Role and Membership provider system.</span></span> <span data-ttu-id="204f5-697">容易にするセキュリティで保護された web ページに、開発者の柔軟な方法で多くの新機能があります。</span><span class="sxs-lookup"><span data-stu-id="204f5-697">It has many new features that make it easier for the developer to secure web pages in a more flexible way.</span></span>

       <span data-ttu-id="204f5-698">など、AccountController する準備ができた OAuthWebSecurity (OAuth アカウントの登録、ログイン、管理など) 用と Web セキュリティを使用して、インターネットのテンプレートは、SimpleMembership を統合する点を設定が既に。</span><span class="sxs-lookup"><span data-stu-id="204f5-698">The Internet template already has set up a few things to integrate SimpleMembership, for example, the AccountController is prepared to use OAuthWebSecurity (for OAuth account registration, login, management, etc.) and Web Security.</span></span>

       <span data-ttu-id="204f5-699">![ソリューションに含まれる SimpleMembership](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership がソリューションに含まれる")</span><span class="sxs-lookup"><span data-stu-id="204f5-699">![SimpleMembership Included in the solution](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership Included in the solution")</span></span>

       <span data-ttu-id="204f5-700">*ソリューションに含まれる SimpleMembership*</span><span class="sxs-lookup"><span data-stu-id="204f5-700">*SimpleMembership Included in the solution*</span></span>

       > [!NOTE]
       > <span data-ttu-id="204f5-701">に関する詳細を検索[OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) msdn に掲載されています。</span><span class="sxs-lookup"><span data-stu-id="204f5-701">Find more information about [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) in MSDN.</span></span>

> [!NOTE]
> <span data-ttu-id="204f5-702">Windows Azure Web サイトの次にこのアプリケーションを展開するさらに、[付録 b: 公開 Web Deploy を使用して ASP.NET MVC 4 アプリケーション](#AppendixB)です。</span><span class="sxs-lookup"><span data-stu-id="204f5-702">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="204f5-703">まとめ</span><span class="sxs-lookup"><span data-stu-id="204f5-703">Summary</span></span>

<span data-ttu-id="204f5-704">このハンズオン ラボを完了して、ASP.NET MVC の基本を学習しました。</span><span class="sxs-lookup"><span data-stu-id="204f5-704">By completing this Hands-On Lab you have learned the fundamentals of ASP.NET MVC:</span></span>

- <span data-ttu-id="204f5-705">MVC アプリケーションとやり取りする方法の主要な要素</span><span class="sxs-lookup"><span data-stu-id="204f5-705">The core elements of an MVC application and how they interact</span></span>
- <span data-ttu-id="204f5-706">ASP.NET MVC アプリケーションを作成する方法</span><span class="sxs-lookup"><span data-stu-id="204f5-706">How to create an ASP.NET MVC Application</span></span>
- <span data-ttu-id="204f5-707">URL とクエリ文字列を通じて渡される追加し、パラメーターを処理するコント ローラーを構成する方法</span><span class="sxs-lookup"><span data-stu-id="204f5-707">How to add and configure Controllers to handle parameters passed through the URL and querystring</span></span>
- <span data-ttu-id="204f5-708">一般的な HTML コンテンツ、外観、および HTML コンテンツを表示するビュー テンプレートを拡張するスタイル シートのテンプレートをセットアップするレイアウトのマスター ページを追加する方法</span><span class="sxs-lookup"><span data-stu-id="204f5-708">How to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel and a View template to display HTML content</span></span>
- <span data-ttu-id="204f5-709">ビュー テンプレートのプロパティに渡すため ViewModel パターンを使用して、動的な情報を表示する方法</span><span class="sxs-lookup"><span data-stu-id="204f5-709">How to use the ViewModel pattern for passing properties to the View template to display dynamic information</span></span>
- <span data-ttu-id="204f5-710">ビューのテンプレートでコント ローラーに渡されるパラメーターを使用する方法</span><span class="sxs-lookup"><span data-stu-id="204f5-710">How to use parameters passed to Controllers in the View template</span></span>
- <span data-ttu-id="204f5-711">ASP.NET MVC アプリケーション内のページへのリンクを追加する方法</span><span class="sxs-lookup"><span data-stu-id="204f5-711">How to add links to pages inside the ASP.NET MVC application</span></span>
- <span data-ttu-id="204f5-712">追加し、ビューで動的なプロパティを使用する方法</span><span class="sxs-lookup"><span data-stu-id="204f5-712">How to add and use dynamic properties in a View</span></span>
- <span data-ttu-id="204f5-713">ASP.NET MVC 4 プロジェクト テンプレートでの機能強化</span><span class="sxs-lookup"><span data-stu-id="204f5-713">The enhancements in the ASP.NET MVC 4 project templates</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="204f5-714">付録 a: をインストールする Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="204f5-714">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="204f5-715">インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、 **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="204f5-715">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="204f5-716">次の手順を案内するインストールに必要な手順*Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*です。</span><span class="sxs-lookup"><span data-stu-id="204f5-716">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="204f5-717">移動して[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)です。</span><span class="sxs-lookup"><span data-stu-id="204f5-717">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="204f5-718">または、既に Web Platform Installer をインストールした場合、開くことができ、製品を検索&quot; <em>Visual Studio Express 2012 for Web と Windows Azure SDK</em>&quot;です。</span><span class="sxs-lookup"><span data-stu-id="204f5-718">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="204f5-719">をクリックして**を今すぐインストール**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-719">Click on **Install Now**.</span></span> <span data-ttu-id="204f5-720">ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="204f5-720">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="204f5-721">1 回**Web Platform Installer**が開いて、をクリックして**インストール**セットアップを開始します。</span><span class="sxs-lookup"><span data-stu-id="204f5-721">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="204f5-722">![Visual Studio Express インストール](aspnet-mvc-4-fundamentals/_static/image43.png "Visual Studio Express のインストール")</span><span class="sxs-lookup"><span data-stu-id="204f5-722">![Install Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="204f5-723">*Visual Studio Express をインストールします。*</span><span class="sxs-lookup"><span data-stu-id="204f5-723">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="204f5-724">すべての製品のライセンスと条項を読み、クリックして**同意**を続行します。</span><span class="sxs-lookup"><span data-stu-id="204f5-724">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![ライセンス条項に同意](aspnet-mvc-4-fundamentals/_static/image44.png)

    <span data-ttu-id="204f5-726">*ライセンス条項に同意*</span><span class="sxs-lookup"><span data-stu-id="204f5-726">*Accepting the license terms*</span></span>
5. <span data-ttu-id="204f5-727">ダウンロードとインストール プロセスが完了するまで待機します。</span><span class="sxs-lookup"><span data-stu-id="204f5-727">Wait until the downloading and installation process completes.</span></span>

    ![インストールの進行状況](aspnet-mvc-4-fundamentals/_static/image45.png)

    <span data-ttu-id="204f5-729">*インストールの進行状況*</span><span class="sxs-lookup"><span data-stu-id="204f5-729">*Installation progress*</span></span>
6. <span data-ttu-id="204f5-730">インストールが完了したらをクリックして**完了**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-730">When the installation completes, click **Finish**.</span></span>

    ![インストールが完了しました](aspnet-mvc-4-fundamentals/_static/image46.png)

    <span data-ttu-id="204f5-732">*インストールが完了しました*</span><span class="sxs-lookup"><span data-stu-id="204f5-732">*Installation completed*</span></span>
7. <span data-ttu-id="204f5-733">をクリックして**終了**Web Platform Installer を閉じます。</span><span class="sxs-lookup"><span data-stu-id="204f5-733">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="204f5-734">Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、順にクリックして、 **VS Express for Web**並べて表示します。</span><span class="sxs-lookup"><span data-stu-id="204f5-734">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express Web タイルを](aspnet-mvc-4-fundamentals/_static/image47.png)

    <span data-ttu-id="204f5-736">*VS Express Web タイルを*</span><span class="sxs-lookup"><span data-stu-id="204f5-736">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="204f5-737">付録 b: Web Deploy を使用して ASP.NET MVC 4 アプリケーションを発行します。</span><span class="sxs-lookup"><span data-stu-id="204f5-737">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="204f5-738">この付録では、Windows Azure 管理ポータルから新しい web サイトを作成し、Windows Azure によって提供される Web 配置発行機能を活用して、次の演習では、取得したアプリケーションを公開する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="204f5-738">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="204f5-739">タスク 1 - Windows から新しい Web サイトを作成する Azure ポータル</span><span class="sxs-lookup"><span data-stu-id="204f5-739">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="204f5-740">移動して、 [Windows Azure 管理ポータル](https://manage.windowsazure.com/)サブスクリプションに関連付けられている Microsoft の資格情報を使用してサインインします。</span><span class="sxs-lookup"><span data-stu-id="204f5-740">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="204f5-741">Windows Azure 無料で 10 個の ASP.NET Web サイトをホストでき、トラフィックの増大に応じて拡大縮小できます。</span><span class="sxs-lookup"><span data-stu-id="204f5-741">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="204f5-742">サインアップする[ここ](http://aka.ms/aspnet-hol-azure)です。</span><span class="sxs-lookup"><span data-stu-id="204f5-742">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="204f5-743">![Windows Azure ポータルにログオンする](aspnet-mvc-4-fundamentals/_static/image48.png "Windows Azure ポータルにログオンします。")</span><span class="sxs-lookup"><span data-stu-id="204f5-743">![Log on to Windows Azure portal](aspnet-mvc-4-fundamentals/_static/image48.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="204f5-744">*Windows Azure 管理ポータルにログオンします。*</span><span class="sxs-lookup"><span data-stu-id="204f5-744">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="204f5-745">をクリックして**新規**コマンド バーでします。</span><span class="sxs-lookup"><span data-stu-id="204f5-745">Click **New** on the command bar.</span></span>

    <span data-ttu-id="204f5-746">![新しい Web サイトを作成する](aspnet-mvc-4-fundamentals/_static/image49.png "新しい Web サイトを作成します。")</span><span class="sxs-lookup"><span data-stu-id="204f5-746">![Creating a new Web Site](aspnet-mvc-4-fundamentals/_static/image49.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="204f5-747">*新しい Web サイトを作成します。*</span><span class="sxs-lookup"><span data-stu-id="204f5-747">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="204f5-748">をクリックして**コンピューティング** | **Web サイト**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-748">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="204f5-749">選択し、**簡易作成**オプション。</span><span class="sxs-lookup"><span data-stu-id="204f5-749">Then select **Quick Create** option.</span></span> <span data-ttu-id="204f5-750">新しい web サイトの利用可能な URL を指定して、をクリックして**Web サイトの作成**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-750">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="204f5-751">Windows Azure Web サイトは、制御および管理できる、クラウドで実行されている web アプリケーションのホストです。</span><span class="sxs-lookup"><span data-stu-id="204f5-751">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="204f5-752">簡易作成 オプションでは、完成した web アプリケーションを Windows Azure Web サイトからポータル外を展開することができます。</span><span class="sxs-lookup"><span data-stu-id="204f5-752">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="204f5-753">データベースを設定するための手順は含まれません。</span><span class="sxs-lookup"><span data-stu-id="204f5-753">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="204f5-754">![簡易作成を使用して新しい Web サイトを作成する](aspnet-mvc-4-fundamentals/_static/image50.png "簡易作成を使用して新しい Web サイトを作成します。")</span><span class="sxs-lookup"><span data-stu-id="204f5-754">![Creating a new Web Site using Quick Create](aspnet-mvc-4-fundamentals/_static/image50.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="204f5-755">*簡易作成を使用して新しい Web サイトを作成します。*</span><span class="sxs-lookup"><span data-stu-id="204f5-755">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="204f5-756">新しいまで待つ**Web サイト**を作成します。</span><span class="sxs-lookup"><span data-stu-id="204f5-756">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="204f5-757">Web サイトを作成した後は、下のリンクをクリックして、 **URL**列です。</span><span class="sxs-lookup"><span data-stu-id="204f5-757">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="204f5-758">新しい Web サイトが動作していることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="204f5-758">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="204f5-759">![新しい web サイトを参照して](aspnet-mvc-4-fundamentals/_static/image51.png "新しい web サイトを参照")</span><span class="sxs-lookup"><span data-stu-id="204f5-759">![Browsing to the new web site](aspnet-mvc-4-fundamentals/_static/image51.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="204f5-760">*新しい web サイトを参照*</span><span class="sxs-lookup"><span data-stu-id="204f5-760">*Browsing to the new web site*</span></span>

    <span data-ttu-id="204f5-761">![Web サイトを実行している](aspnet-mvc-4-fundamentals/_static/image52.png "を実行している Web サイト")</span><span class="sxs-lookup"><span data-stu-id="204f5-761">![Web site running](aspnet-mvc-4-fundamentals/_static/image52.png "Web site running")</span></span>

    <span data-ttu-id="204f5-762">*実行している web サイト*</span><span class="sxs-lookup"><span data-stu-id="204f5-762">*Web site running*</span></span>
6. <span data-ttu-id="204f5-763">ポータルに戻るし、下にある web サイトの名前をクリックして、**名前**管理ページを表示する列。</span><span class="sxs-lookup"><span data-stu-id="204f5-763">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="204f5-764">![Web サイトの管理ページを開く](aspnet-mvc-4-fundamentals/_static/image53.png "web サイトの管理ページを開く")</span><span class="sxs-lookup"><span data-stu-id="204f5-764">![Opening the web site management pages](aspnet-mvc-4-fundamentals/_static/image53.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="204f5-765">*Web サイトの管理ページを開く*</span><span class="sxs-lookup"><span data-stu-id="204f5-765">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="204f5-766">**ダッシュ ボード**] ページの [、**クイック グランス**セクションで、をクリックして、**発行プロファイルのダウンロード**リンクします。</span><span class="sxs-lookup"><span data-stu-id="204f5-766">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="204f5-767">*発行プロファイル*すべてのパブリケーションを有効になっているメソッドの各 Windows Azure web サイトに web アプリケーションを発行するために必要な情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="204f5-767">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="204f5-768">発行プロファイルには、Url、ユーザーの資格情報およびに接続し、各発行メソッドが有効になっているエンドポイントに対して認証するために必要なデータベース文字列が含まれています。</span><span class="sxs-lookup"><span data-stu-id="204f5-768">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="204f5-769">**Microsoft WebMatrix 2**、 **Microsoft Visual Studio Express for Web**と**Microsoft Visual Studio 2012**読み取りのサポートをこれらのプログラムの構成を自動化するプロファイルの発行Windows Azure の web サイトに web アプリケーションを公開します。</span><span class="sxs-lookup"><span data-stu-id="204f5-769">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="204f5-770">![発行プロファイルを web サイトをダウンロードする](aspnet-mvc-4-fundamentals/_static/image54.png "発行プロファイルを web サイトをダウンロードします。")</span><span class="sxs-lookup"><span data-stu-id="204f5-770">![Downloading the web site publish profile](aspnet-mvc-4-fundamentals/_static/image54.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="204f5-771">*発行プロファイルを Web サイトをダウンロードします。*</span><span class="sxs-lookup"><span data-stu-id="204f5-771">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="204f5-772">既知の場所に発行プロファイル ファイルをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="204f5-772">Download the publish profile file to a known location.</span></span> <span data-ttu-id="204f5-773">さらにこの練習では、このファイルを使用して、Visual Studio から web アプリケーション、Windows Azure Web サイトを発行する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="204f5-773">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="204f5-774">![発行プロファイル ファイルを保存](aspnet-mvc-4-fundamentals/_static/image55.png "発行プロファイルの保存")</span><span class="sxs-lookup"><span data-stu-id="204f5-774">![Saving the publish profile file](aspnet-mvc-4-fundamentals/_static/image55.png "Saving the publish profile")</span></span>

    <span data-ttu-id="204f5-775">*発行プロファイル ファイルを保存します。*</span><span class="sxs-lookup"><span data-stu-id="204f5-775">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="204f5-776">タスク 2 - データベース サーバーの構成</span><span class="sxs-lookup"><span data-stu-id="204f5-776">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="204f5-777">アプリケーションが SQL Server を使用する場合は、データベースの SQL データベース サーバーを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="204f5-777">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="204f5-778">SQL Server を使用しない簡単なアプリケーションを展開する場合は、このタスクをスキップする場合があります。</span><span class="sxs-lookup"><span data-stu-id="204f5-778">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="204f5-779">SQL データベース サーバーは、アプリケーション データベースを格納する必要があります。</span><span class="sxs-lookup"><span data-stu-id="204f5-779">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="204f5-780">SQL データベース サーバーを表示するには、Windows Azure 管理ポータルで自分のサブスクリプションから**Sql データベース** | **サーバー** | **サーバーのダッシュ ボード**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-780">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="204f5-781">使用して 1 つを作成するには作成されたサーバーがない、**追加**コマンド バーのボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="204f5-781">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="204f5-782">注意してください、**サーバー名および URL、管理者のログイン名とパスワード**、次のタスクで使用することができます。</span><span class="sxs-lookup"><span data-stu-id="204f5-782">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="204f5-783">データベースを作成しない、まだと後の段階で作成されます。</span><span class="sxs-lookup"><span data-stu-id="204f5-783">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="204f5-784">![SQL データベース サーバーのダッシュ ボード](aspnet-mvc-4-fundamentals/_static/image56.png "SQL データベース サーバーのダッシュ ボード")</span><span class="sxs-lookup"><span data-stu-id="204f5-784">![SQL Database Server Dashboard](aspnet-mvc-4-fundamentals/_static/image56.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="204f5-785">*SQL データベース サーバーのダッシュ ボード*</span><span class="sxs-lookup"><span data-stu-id="204f5-785">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="204f5-786">次のタスクでは、サーバーの一覧に、ローカル IP アドレスを含める必要があるため、Visual Studio からデータベース接続をテストします**使用できる IP アドレス**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-786">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="204f5-787">実行するには、クリックして**構成**から IP アドレスを選択する**現在のクライアント IP アドレス**に貼り付けます、**開始 IP アドレス**と**終了IPアドレス**のテキスト ボックスをクリックして、 ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png)ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="204f5-787">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) button.</span></span>

    ![クライアントの IP アドレスを追加します。](aspnet-mvc-4-fundamentals/_static/image58.png)

    <span data-ttu-id="204f5-789">*クライアントの IP アドレスを追加します。*</span><span class="sxs-lookup"><span data-stu-id="204f5-789">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="204f5-790">1 回、**クライアント IP アドレス**が許可される IP アドレスに追加一覧で、をクリックして**保存**変更を確認します。</span><span class="sxs-lookup"><span data-stu-id="204f5-790">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![変更を確認します。](aspnet-mvc-4-fundamentals/_static/image59.png)

    <span data-ttu-id="204f5-792">*変更を確認します。*</span><span class="sxs-lookup"><span data-stu-id="204f5-792">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="204f5-793">タスク 3 - Web Deploy を使用して ASP.NET MVC 4 アプリケーションの発行</span><span class="sxs-lookup"><span data-stu-id="204f5-793">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="204f5-794">ASP.NET MVC 4 ソリューションに戻ります。</span><span class="sxs-lookup"><span data-stu-id="204f5-794">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="204f5-795">**ソリューション エクスプ ローラー**を web サイト プロジェクトを右クリックし、**発行**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-795">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="204f5-796">![アプリケーションの発行](aspnet-mvc-4-fundamentals/_static/image60.png "アプリケーションの発行")</span><span class="sxs-lookup"><span data-stu-id="204f5-796">![Publishing the Application](aspnet-mvc-4-fundamentals/_static/image60.png "Publishing the Application")</span></span>

    <span data-ttu-id="204f5-797">*Web サイトの発行*</span><span class="sxs-lookup"><span data-stu-id="204f5-797">*Publishing the web site*</span></span>
2. <span data-ttu-id="204f5-798">最初の作業を保存した発行プロファイルをインポートします。</span><span class="sxs-lookup"><span data-stu-id="204f5-798">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="204f5-799">![発行プロファイルをインポートする](aspnet-mvc-4-fundamentals/_static/image61.png "発行プロファイルをインポートします。")</span><span class="sxs-lookup"><span data-stu-id="204f5-799">![Importing the publish profile](aspnet-mvc-4-fundamentals/_static/image61.png "Importing the publish profile")</span></span>

    <span data-ttu-id="204f5-800">*発行プロファイルのインポート*</span><span class="sxs-lookup"><span data-stu-id="204f5-800">*Importing publish profile*</span></span>
3. <span data-ttu-id="204f5-801">をクリックして**への接続検証**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-801">Click **Validate Connection**.</span></span> <span data-ttu-id="204f5-802">検証が完了したらクリックして**次**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-802">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="204f5-803">検証が完了すると、接続の検証 ボタンの横に表示される緑色のチェック マークを参照してください。</span><span class="sxs-lookup"><span data-stu-id="204f5-803">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="204f5-804">![接続の検証](aspnet-mvc-4-fundamentals/_static/image62.png "接続の検証")</span><span class="sxs-lookup"><span data-stu-id="204f5-804">![Validating connection](aspnet-mvc-4-fundamentals/_static/image62.png "Validating connection")</span></span>

    <span data-ttu-id="204f5-805">*接続の検証*</span><span class="sxs-lookup"><span data-stu-id="204f5-805">*Validating connection*</span></span>
4. <span data-ttu-id="204f5-806">**設定**] ページの [、**データベース**セクションで、データベース接続のテキスト ボックスの横にあるボタンをクリックして (つまり**DefaultConnection**)。</span><span class="sxs-lookup"><span data-stu-id="204f5-806">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="204f5-807">![Web 配置の構成](aspnet-mvc-4-fundamentals/_static/image63.png "Web 配置の構成")</span><span class="sxs-lookup"><span data-stu-id="204f5-807">![Web deploy configuration](aspnet-mvc-4-fundamentals/_static/image63.png "Web deploy configuration")</span></span>

    <span data-ttu-id="204f5-808">*Web 配置の構成*</span><span class="sxs-lookup"><span data-stu-id="204f5-808">*Web deploy configuration*</span></span>
5. <span data-ttu-id="204f5-809">次のように、データベースの接続を構成します。</span><span class="sxs-lookup"><span data-stu-id="204f5-809">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="204f5-810">**サーバー名**SQL データベース サーバーの URL を使用して、入力、 *tcp:* プレフィックス。</span><span class="sxs-lookup"><span data-stu-id="204f5-810">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="204f5-811">**ユーザー名**サーバー管理者のログイン名を入力します。</span><span class="sxs-lookup"><span data-stu-id="204f5-811">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="204f5-812">**パスワード**サーバー管理者のログイン パスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="204f5-812">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="204f5-813">たとえば、新しいデータベース名を入力: *MVC4SampleDB*です。</span><span class="sxs-lookup"><span data-stu-id="204f5-813">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="204f5-814">![対象の接続文字列を構成する](aspnet-mvc-4-fundamentals/_static/image64.png "対象の接続文字列を構成します。")</span><span class="sxs-lookup"><span data-stu-id="204f5-814">![Configuring destination connection string](aspnet-mvc-4-fundamentals/_static/image64.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="204f5-815">*対象の接続文字列を構成します。*</span><span class="sxs-lookup"><span data-stu-id="204f5-815">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="204f5-816">次に、 **[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="204f5-816">Then click **OK**.</span></span> <span data-ttu-id="204f5-817">データベースをクリックを作成するように求められたら**はい**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-817">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="204f5-818">![データベースを作成する](aspnet-mvc-4-fundamentals/_static/image65.png "データベース文字列を作成します。")</span><span class="sxs-lookup"><span data-stu-id="204f5-818">![Creating the database](aspnet-mvc-4-fundamentals/_static/image65.png "Creating the database string")</span></span>

    <span data-ttu-id="204f5-819">*データベースの作成*</span><span class="sxs-lookup"><span data-stu-id="204f5-819">*Creating the database*</span></span>
7. <span data-ttu-id="204f5-820">接続の既定のテキスト ボックス内では、Windows azure SQL データベースへの接続に使用する接続文字列が表示されます。</span><span class="sxs-lookup"><span data-stu-id="204f5-820">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="204f5-821">その後、 **[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="204f5-821">Then click **Next**.</span></span>

    <span data-ttu-id="204f5-822">![SQL データベースを指す接続文字列](aspnet-mvc-4-fundamentals/_static/image66.png "SQL データベースを指す接続文字列")</span><span class="sxs-lookup"><span data-stu-id="204f5-822">![Connection string pointing to SQL Database](aspnet-mvc-4-fundamentals/_static/image66.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="204f5-823">*SQL データベースを指す接続文字列*</span><span class="sxs-lookup"><span data-stu-id="204f5-823">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="204f5-824">**プレビュー** ] ページで [**発行**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-824">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="204f5-825">![Web アプリケーションの発行](aspnet-mvc-4-fundamentals/_static/image67.png "web アプリケーションの発行")</span><span class="sxs-lookup"><span data-stu-id="204f5-825">![Publishing the web application](aspnet-mvc-4-fundamentals/_static/image67.png "Publishing the web application")</span></span>

    <span data-ttu-id="204f5-826">*Web アプリケーションの発行*</span><span class="sxs-lookup"><span data-stu-id="204f5-826">*Publishing the web application*</span></span>
9. <span data-ttu-id="204f5-827">発行プロセスが終了すると、既定のブラウザーが公開された web サイトを開きます。</span><span class="sxs-lookup"><span data-stu-id="204f5-827">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="204f5-828">![アプリケーションが Windows Azure に発行](aspnet-mvc-4-fundamentals/_static/image68.png "アプリケーションが Windows Azure に発行")</span><span class="sxs-lookup"><span data-stu-id="204f5-828">![Application published to Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="204f5-829">*Windows Azure に発行されるアプリケーション*</span><span class="sxs-lookup"><span data-stu-id="204f5-829">*Application published to Windows Azure*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="204f5-830">付録 c: コード スニペットの使用</span><span class="sxs-lookup"><span data-stu-id="204f5-830">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="204f5-831">コード スニペットでは、すぐに利用できる必要があるすべてのコードがあります。</span><span class="sxs-lookup"><span data-stu-id="204f5-831">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="204f5-832">ラボ ドキュメントがわかります正確に使用する場合が、次の図に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="204f5-832">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="204f5-833">![Visual Studio のコード スニペットを使用して、プロジェクトにコードを挿入する](aspnet-mvc-4-fundamentals/_static/image69.png "プロジェクトにコードを挿入する Visual Studio を使ってコード スニペット")</span><span class="sxs-lookup"><span data-stu-id="204f5-833">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-fundamentals/_static/image69.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="204f5-834">*Visual Studio のコード スニペットを使用して、プロジェクトにコードを挿入するには*</span><span class="sxs-lookup"><span data-stu-id="204f5-834">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="204f5-835">***キーボード (C# の場合のみ) を使用してコード スニペットを追加するには***</span><span class="sxs-lookup"><span data-stu-id="204f5-835">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="204f5-836">コードを挿入するには、カーソルを置きます。</span><span class="sxs-lookup"><span data-stu-id="204f5-836">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="204f5-837">(なし、スペースやハイフン) スニペット名を入力してを起動します。</span><span class="sxs-lookup"><span data-stu-id="204f5-837">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="204f5-838">スニペットの名前に一致する IntelliSense 表示を確認します。</span><span class="sxs-lookup"><span data-stu-id="204f5-838">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="204f5-839">正しいスニペットを選択 (または、全体のスニペットの名前を選択するまで」と入力してください)。</span><span class="sxs-lookup"><span data-stu-id="204f5-839">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="204f5-840">カーソル位置にスニペットを挿入するには、2 回、Tab キーを押します。</span><span class="sxs-lookup"><span data-stu-id="204f5-840">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="204f5-841">![スニペット名を入力する開始](aspnet-mvc-4-fundamentals/_static/image70.png "スニペット名の入力を開始")</span><span class="sxs-lookup"><span data-stu-id="204f5-841">![Start typing the snippet name](aspnet-mvc-4-fundamentals/_static/image70.png "Start typing the snippet name")</span></span>

<span data-ttu-id="204f5-842">*スニペット名の入力を開始します。*</span><span class="sxs-lookup"><span data-stu-id="204f5-842">*Start typing the snippet name*</span></span>

<span data-ttu-id="204f5-843">![Tab キーを押して、強調表示されているスニペットを選択する](aspnet-mvc-4-fundamentals/_static/image71.png "強調表示されているスニペットを選択するキーを押してタブ")</span><span class="sxs-lookup"><span data-stu-id="204f5-843">![Press Tab to select the highlighted snippet](aspnet-mvc-4-fundamentals/_static/image71.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="204f5-844">*Tab キーを押して、強調表示されているスニペットを選択するには*</span><span class="sxs-lookup"><span data-stu-id="204f5-844">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="204f5-845">![キーを押して タブで再度と、スニペットが展開](aspnet-mvc-4-fundamentals/_static/image72.png "キーを押して タブで再度と、スニペットが展開されます")</span><span class="sxs-lookup"><span data-stu-id="204f5-845">![Press Tab again and the snippet will expand](aspnet-mvc-4-fundamentals/_static/image72.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="204f5-846">*キーを押して タブで再度と、スニペットが展開されます。*</span><span class="sxs-lookup"><span data-stu-id="204f5-846">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="204f5-847">***(C#、Visual Basic および XML) にマウスを使用してコード スニペットを追加する***1 です。</span><span class="sxs-lookup"><span data-stu-id="204f5-847">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="204f5-848">コード スニペットを挿入する場所を右クリックします。</span><span class="sxs-lookup"><span data-stu-id="204f5-848">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="204f5-849">選択**スニペットの挿入**続く**マイ コード スニペット**です。</span><span class="sxs-lookup"><span data-stu-id="204f5-849">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="204f5-850">クリックして一覧から、関連するスニペットを選択します。</span><span class="sxs-lookup"><span data-stu-id="204f5-850">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="204f5-851">![コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリックして](aspnet-mvc-4-fundamentals/_static/image73.png "コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリック")</span><span class="sxs-lookup"><span data-stu-id="204f5-851">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-fundamentals/_static/image73.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="204f5-852">*コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリックします。*</span><span class="sxs-lookup"><span data-stu-id="204f5-852">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="204f5-853">![クリックして一覧から、関連するスニペットを選択](aspnet-mvc-4-fundamentals/_static/image74.png "クリックして一覧から、関連するスニペットを選択")</span><span class="sxs-lookup"><span data-stu-id="204f5-853">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-fundamentals/_static/image74.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="204f5-854">*クリックして一覧から、関連するスニペットを選択します。*</span><span class="sxs-lookup"><span data-stu-id="204f5-854">*Pick the relevant snippet from the list, by clicking on it*</span></span>
