---
uid: aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
title: 'ハンズ オン ラボ: 保守性の高い Azure の web サイト: 変更とスケールの管理 |Microsoft Docs'
author: rick-anderson
description: このラボでは、どの Microsoft Azure 簡単にビルドして、web サイトを運用環境にデプロイについて説明します。
ms.author: aspnetcontent
ms.date: 07/16/2014
ms.assetid: ecfd0eb4-c4ad-44e6-9db9-a2a66611ff6a
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
msc.type: authoredcontent
ms.openlocfilehash: 3a118cdd7e3f3878976e4f8480ce2236b8d3ba88
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824797"
---
<a name="hands-on-lab-maintainable-azure-websites-managing-change-and-scale"></a><span data-ttu-id="53416-103">ハンズ オン ラボ: 保守性の高い Azure の web サイト: 変更とスケールの管理</span><span class="sxs-lookup"><span data-stu-id="53416-103">Hands on Lab: Maintainable Azure Websites: Managing Change and Scale</span></span>
====================
<span data-ttu-id="53416-104">によって[Web キャンプ チーム](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="53416-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="53416-105">Web のキャンプ トレーニング キットをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="53416-105">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="53416-106">Microsoft Azure では、簡単にビルドし、web サイトを運用環境にデプロイできます。</span><span class="sxs-lookup"><span data-stu-id="53416-106">Microsoft Azure makes it easy to build and deploy websites to production.</span></span> <span data-ttu-id="53416-107">アプリケーションのライブが完了して、始めたばかりという方します。</span><span class="sxs-lookup"><span data-stu-id="53416-107">But you're not done when your application is live, you're just getting started!</span></span> <span data-ttu-id="53416-108">変化する要件、データベースの更新、スケール、および詳細を処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="53416-108">You'll need to handle changing requirements, database updates, scale, and more.</span></span> <span data-ttu-id="53416-109">さいわい、Azure App Service では、多数の機能をスムーズに実行されている、サイトを保護するために、対応していますが。</span><span class="sxs-lookup"><span data-stu-id="53416-109">Fortunately, Azure App Service has you covered, with plenty of features to help you keep your sites running smoothly.</span></span>
> 
> <span data-ttu-id="53416-110">Azure では、セキュリティで保護された柔軟性の高い開発、展開、および拡張オプションをあらゆるサイズの web アプリケーションを提供します。</span><span class="sxs-lookup"><span data-stu-id="53416-110">Azure offers secure and flexible development, deployment and scaling options for any size web application.</span></span> <span data-ttu-id="53416-111">インフラストラクチャの管理の負担をかけずアプリケーションの展開を作成し、既存のツールを活用します。</span><span class="sxs-lookup"><span data-stu-id="53416-111">Leverage your existing tools to create and deploy applications without the hassle of managing infrastructure.</span></span>
> 
> <span data-ttu-id="53416-112">運用環境の web アプリケーションを自分で数分でプロビジョニング、好みの開発ツールを使用して作成されたコンテンツを簡単にデプロイすることで。</span><span class="sxs-lookup"><span data-stu-id="53416-112">Provision a production web application yourself in minutes by easily deploying content created using your favorite development tool.</span></span> <span data-ttu-id="53416-113">対応のソース管理から直接既存のサイトを展開する**Git**、 **GitHub**、 **Bitbucket**、 **TFS**とでも**DropBox**します。</span><span class="sxs-lookup"><span data-stu-id="53416-113">You can deploy an existing site directly from source control with support for **Git**, **GitHub**, **Bitbucket**, **TFS**, and even **DropBox**.</span></span> <span data-ttu-id="53416-114">お気に入りの IDE から直接、またはを使用してスクリプトから展開**PowerShell**で Windows または**CLI**あらゆる OS で実行されるツールです。</span><span class="sxs-lookup"><span data-stu-id="53416-114">Deploy directly from your favorite IDE or from scripts using **PowerShell** in Windows or **CLI** tools running on any OS.</span></span> <span data-ttu-id="53416-115">展開した後、サイトを継続的なデプロイのサポートを常に最新の状態のままにします。</span><span class="sxs-lookup"><span data-stu-id="53416-115">Once deployed, keep your sites constantly up-to-date with support for continuous deployment.</span></span>
> 
> <span data-ttu-id="53416-116">Azure にはスケーラブルで持続性のあるクラウド ストレージ、バックアップ、および回復ソリューションのすべてのデータでは、小規模または大規模な。</span><span class="sxs-lookup"><span data-stu-id="53416-116">Azure provides scalable, durable cloud storage, backup, and recovery solutions for any data, big or small.</span></span> <span data-ttu-id="53416-117">実稼働環境では、テーブル、Blob、SQL データベースなどのストレージ サービスにアプリケーションをデプロイするときに、クラウドでアプリケーションがスケール アップできます。</span><span class="sxs-lookup"><span data-stu-id="53416-117">When deploying applications to a production environment, storage services such as Tables, Blobs and SQL Databases, help you scale your application in the cloud.</span></span>
> 
> <span data-ttu-id="53416-118">SQL データベースが新しいバージョンのアプリケーションをデプロイするときに、生産性の高いデータベースを最新に重要です。</span><span class="sxs-lookup"><span data-stu-id="53416-118">With SQL Databases, it is important to keep your productive database up-to-date when deploying new versions of your application.</span></span> <span data-ttu-id="53416-119">方々 に感謝**Entity Framework Code First Migrations**、分単位でお使いの環境を更新する、開発と、データ モデルの展開が簡素化されています。</span><span class="sxs-lookup"><span data-stu-id="53416-119">Thanks to **Entity Framework Code First Migrations**, the development and deployment of your data model has been simplified to update your environments in minutes.</span></span> <span data-ttu-id="53416-120">このハンズオン ラボでは、web アプリを Microsoft Azure で運用環境にデプロイするときに発生する可能性があります、さまざまなトピックを示します。</span><span class="sxs-lookup"><span data-stu-id="53416-120">This hands-on lab will show you the different topics you could encounter when deploying your web app to production environments in Microsoft Azure.</span></span>
> 
> <span data-ttu-id="53416-121">すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[ http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit)します。</span><span class="sxs-lookup"><span data-stu-id="53416-121">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>
> 
> <span data-ttu-id="53416-122">このトピックの詳しい内容について詳細を参照してください、 [Azure 電子書籍の実際のクラウド アプリの構築](building-real-world-cloud-apps-with-windows-azure/introduction.md)します。</span><span class="sxs-lookup"><span data-stu-id="53416-122">For more in-depth coverage of this topic, see the [Building Real-World Cloud Apps with Azure e-book](building-real-world-cloud-apps-with-windows-azure/introduction.md).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="53416-123">概要</span><span class="sxs-lookup"><span data-stu-id="53416-123">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="53416-124">目的</span><span class="sxs-lookup"><span data-stu-id="53416-124">Objectives</span></span>

<span data-ttu-id="53416-125">このハンズオン ラボでは、学習する方法。</span><span class="sxs-lookup"><span data-stu-id="53416-125">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="53416-126">既存のモデルで Entity Framework の移行を有効にします。</span><span class="sxs-lookup"><span data-stu-id="53416-126">Enable Entity Framework Migrations with an existing model</span></span>
- <span data-ttu-id="53416-127">オブジェクト モデルと Entity Framework の移行を適宜使用してデータベースを更新します。</span><span class="sxs-lookup"><span data-stu-id="53416-127">Update the object model and database accordingly using Entity Framework Migrations</span></span>
- <span data-ttu-id="53416-128">Git を使用して Azure App Service にデプロイします。</span><span class="sxs-lookup"><span data-stu-id="53416-128">Deploy to Azure App Service using Git</span></span>
- <span data-ttu-id="53416-129">Azure Management portal を使用して以前の展開にロールバックします。</span><span class="sxs-lookup"><span data-stu-id="53416-129">Rollback to a previous deployment using the Azure Management portal</span></span>
- <span data-ttu-id="53416-130">Azure Storage を使用して web アプリをスケーリング</span><span class="sxs-lookup"><span data-stu-id="53416-130">Use Azure Storage to scale a web app</span></span>
- <span data-ttu-id="53416-131">Azure 管理ポータルを使用して web アプリの自動スケーリングの構成します。</span><span class="sxs-lookup"><span data-stu-id="53416-131">Configure auto-scaling for a web app using the Azure Management Portal</span></span>
- <span data-ttu-id="53416-132">作成し、Visual Studio でロード テストのプロジェクトを構成します。</span><span class="sxs-lookup"><span data-stu-id="53416-132">Create and configure a load test project in Visual Studio</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="53416-133">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="53416-133">Prerequisites</span></span>

<span data-ttu-id="53416-134">このハンズオン ラボを完了する、次が必要。</span><span class="sxs-lookup"><span data-stu-id="53416-134">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="53416-135">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/)以上</span><span class="sxs-lookup"><span data-stu-id="53416-135">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="53416-136">Azure SDK for .NET 2.2</span><span class="sxs-lookup"><span data-stu-id="53416-136">Azure SDK for .NET 2.2</span></span>](https://www.microsoft.com/windowsazure/sdk/)
- [<span data-ttu-id="53416-137">GIT バージョン管理システム</span><span class="sxs-lookup"><span data-stu-id="53416-137">GIT Version Control System</span></span>](http://git-scm.com/download)
- <span data-ttu-id="53416-138">Microsoft Azure サブスクリプション</span><span class="sxs-lookup"><span data-stu-id="53416-138">A Microsoft Azure subscription</span></span> 

    - <span data-ttu-id="53416-139">サインアップ、[無料試用版](http://aka.ms/watk-freetrial)</span><span class="sxs-lookup"><span data-stu-id="53416-139">Sign up for a [Free Trial](http://aka.ms/watk-freetrial)</span></span>
    - <span data-ttu-id="53416-140">アクティブ Visual Studio Professional、Test Professional、Premium または Ultimate with MSDN または MSDN Platforms のサブスクライバーに、 [MSDN の特典](http://aka.ms/watk-msdn)Azure で開発とテストを開始するようになりました</span><span class="sxs-lookup"><span data-stu-id="53416-140">If you are a Visual Studio Professional, Test Professional, Premium or Ultimate with MSDN or MSDN Platforms subscriber, activate your [MSDN benefit](http://aka.ms/watk-msdn) now to start developing and testing on Azure</span></span>
    - <span data-ttu-id="53416-141">[BizSpark](http://aka.ms/watk-bizspark)メンバーは、Azure に自動的に受信、Visual Studio Ultimate with MSDN サブスクリプション特典</span><span class="sxs-lookup"><span data-stu-id="53416-141">[BizSpark](http://aka.ms/watk-bizspark) members automatically receive the Azure benefit through their Visual Studio Ultimate with MSDN subscriptions</span></span>
    - <span data-ttu-id="53416-142">メンバー、 [Microsoft Partner Network](http://aka.ms/watk-mpn) Cloud Essentials プログラムが無料で毎月の Azure クレジットを受け取る</span><span class="sxs-lookup"><span data-stu-id="53416-142">Members of the [Microsoft Partner Network](http://aka.ms/watk-mpn) Cloud Essentials program receive monthly Azure credits at no charge</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="53416-143">セットアップ</span><span class="sxs-lookup"><span data-stu-id="53416-143">Setup</span></span>

<span data-ttu-id="53416-144">このハンズオン ラボの演習を実行するためには、まず環境を設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="53416-144">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="53416-145">Windows Explorer をラボの [参照] を開いて**ソース**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="53416-145">Open Windows Explorer and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="53416-146">右クリックして**Setup.cmd**選択と**管理者として実行**環境を構成すると、このラボの Visual Studio のコード スニペットがインストールのセットアップ プロセスを起動します。</span><span class="sxs-lookup"><span data-stu-id="53416-146">Right-click on **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="53416-147">ユーザー アカウント制御 ダイアログが表示されている場合は、操作を続行を確認します。</span><span class="sxs-lookup"><span data-stu-id="53416-147">If the User Account Control dialog is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="53416-148">セットアップを実行する前に、このラボのすべての依存関係をチェックしたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="53416-148">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="53416-149">コード スニペットの使用</span><span class="sxs-lookup"><span data-stu-id="53416-149">Using the Code Snippets</span></span>

<span data-ttu-id="53416-150">ラボ ドキュメント全体を通じて、コード ブロックを挿入するよう指示されます。</span><span class="sxs-lookup"><span data-stu-id="53416-150">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="53416-151">便宜上、このコードのほとんどは、Visual Studio コード スニペットを手動で追加することを避けるために Visual Studio 2013 内からアクセスできるとして提供されます。</span><span class="sxs-lookup"><span data-stu-id="53416-151">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="53416-152">ソリューションでは個々 の演習を伴います、**開始**を使用すると、各演習を他のユーザーとは無関係に練習のフォルダー。</span><span class="sxs-lookup"><span data-stu-id="53416-152">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="53416-153">演習の中に追加されるコード スニペットはこれらのスターティング ソリューションが表示されないし、演習を完了するまで動作しない可能性がありますに注意してください。</span><span class="sxs-lookup"><span data-stu-id="53416-153">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="53416-154">演習では、ソース コード内でも表示されます、**エンド**結果から、対応する演習の手順を実行するコードと Visual Studio ソリューションを含むフォルダー。</span><span class="sxs-lookup"><span data-stu-id="53416-154">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="53416-155">このハンズオン ラボを使用すると、追加のヘルプが必要な場合は、これらのソリューションをガイドとして使用できます。</span><span class="sxs-lookup"><span data-stu-id="53416-155">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="53416-156">演習</span><span class="sxs-lookup"><span data-stu-id="53416-156">Exercises</span></span>

<span data-ttu-id="53416-157">このハンズオン ラボには、次の演習が含まれます。</span><span class="sxs-lookup"><span data-stu-id="53416-157">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="53416-158">Entity Framework の移行を使用します。</span><span class="sxs-lookup"><span data-stu-id="53416-158">Using Entity Framework Migrations</span></span>](#Exercise1)
2. [<span data-ttu-id="53416-159">ステージング Web アプリを展開します。</span><span class="sxs-lookup"><span data-stu-id="53416-159">Deploying a Web App to Staging</span></span>](#Exercise2)
3. [<span data-ttu-id="53416-160">運用環境でデプロイのロールバックを実行します。</span><span class="sxs-lookup"><span data-stu-id="53416-160">Performing Deployment Rollback in Production</span></span>](#Exercise3)
4. [<span data-ttu-id="53416-161">Azure Storage を使用したスケーリング</span><span class="sxs-lookup"><span data-stu-id="53416-161">Scaling Using Azure Storage</span></span>](#Exercise4)
5. <span data-ttu-id="53416-162">[Web アプリの自動スケールを使用して](#Exercise5)(Visual Studio 2013 Ultimate エディションの省略可能)</span><span class="sxs-lookup"><span data-stu-id="53416-162">[Using Autoscale for Web Apps](#Exercise5) (Optional for Visual Studio 2013 Ultimate edition)</span></span>

<span data-ttu-id="53416-163">この演習の所要時間を推定: **75 分**</span><span class="sxs-lookup"><span data-stu-id="53416-163">Estimated time to complete this lab: **75 minutes**</span></span>

> [!NOTE]
> <span data-ttu-id="53416-164">Visual Studio を初めて起動すると、定義済みの設定のコレクションの 1 つを選択する必要があります。</span><span class="sxs-lookup"><span data-stu-id="53416-164">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="53416-165">定義済みの各コレクションは、特定の開発スタイルに一致するように設計されていて、ウィンドウのレイアウト、エディターの動作、IntelliSense コード スニペット、およびダイアログ ボックスのオプションを決定します。</span><span class="sxs-lookup"><span data-stu-id="53416-165">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="53416-166">このラボの手順を使用する場合は、Visual Studio で特定のタスクを実行するために必要な操作を記述する、**汎用開発設定**コレクション。</span><span class="sxs-lookup"><span data-stu-id="53416-166">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="53416-167">開発環境のさまざまな設定のコレクションを選択する場合、考慮する必要がある手順に違いがあります。</span><span class="sxs-lookup"><span data-stu-id="53416-167">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-using-entity-framework-migrations"></a><span data-ttu-id="53416-168">手順 1: Entity Framework の移行を使用します。</span><span class="sxs-lookup"><span data-stu-id="53416-168">Exercise 1: Using Entity Framework Migrations</span></span>

<span data-ttu-id="53416-169">アプリケーションを開発しているときに、データ モデルが時間の経過と共に変更可能性があります。</span><span class="sxs-lookup"><span data-stu-id="53416-169">When you are developing an application, your data model might change over time.</span></span> <span data-ttu-id="53416-170">これらの変更に影響する可能性、データベース内の既存のモデル (新しいバージョンを作成する) 場合と、データベースのエラーを防ぐために最新の状態を維持することが重要です。</span><span class="sxs-lookup"><span data-stu-id="53416-170">These changes could affect the existing model in your database (if you are creating a new version) and it is important to keep your database up-to-date to prevent errors.</span></span>

<span data-ttu-id="53416-171">モデルのこれらの変更の追跡を簡略化する**Entity Framework Code First Migrations**自動的にデータベース スキーマを使用してモデルを比較する変更を検出し、データベースを更新する特定のコードを生成します新規作成*バージョン*データベースの。</span><span class="sxs-lookup"><span data-stu-id="53416-171">To simplify the tracking of these changes in your model, **Entity Framework Code First Migrations** automatically detect changes comparing your model with the database schema and generates specific code to update your database, creating new *versions* of your database.</span></span>

<span data-ttu-id="53416-172">この演習は、有効にする方法を示します**移行**アプリケーションと簡単に検出しする方法、データベースを更新する変更を生成します。</span><span class="sxs-lookup"><span data-stu-id="53416-172">This exercise shows you how to enable **Migrations** for your application and how you can easily detect and generate changes to update your databases.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1--enabling-migrations"></a><span data-ttu-id="53416-173">タスク 1 – Migrations の有効化</span><span class="sxs-lookup"><span data-stu-id="53416-173">Task 1 – Enabling Migrations</span></span>

<span data-ttu-id="53416-174">このタスクで有効にすると次の手順を参照してください**Entity Framework Code First Migrations**を**ギーク Quiz**データベース、モデルを変更してでこれらの変更を反映する方法を理解すること、データベース。</span><span class="sxs-lookup"><span data-stu-id="53416-174">In this task, you will go through the steps of enabling **Entity Framework Code First Migrations** to the **Geek Quiz** database, changing the model and understanding how those changes are reflected in the database.</span></span>

1. <span data-ttu-id="53416-175">Visual Studio を開き、開く、 **GeekQuiz.sln**ソリューション ファイルから**Source\Ex1 UsingEntityFrameworkMigrations\Begin**します。</span><span class="sxs-lookup"><span data-stu-id="53416-175">Open Visual Studio and open the **GeekQuiz.sln** solution file from **Source\Ex1-UsingEntityFrameworkMigrations\Begin**.</span></span>
2. <span data-ttu-id="53416-176">ダウンロードしてインストールするためにソリューションをビルド、 **NuGet**依存関係をパッケージ化します。</span><span class="sxs-lookup"><span data-stu-id="53416-176">Build the solution in order to download and install the **NuGet** package dependencies.</span></span> <span data-ttu-id="53416-177">これを行うには、ソリューションを右クリックし、をクリックして**ソリューションのビルド**またはキーを押します**Ctrl + Shift + B**します。</span><span class="sxs-lookup"><span data-stu-id="53416-177">To do this, right-click the solution and click **Build Solution** or press **Ctrl + Shift + B**.</span></span>
3. <span data-ttu-id="53416-178">**ツール** メニューの選択 Visual Studio で**ライブラリ パッケージ マネージャー**、 をクリックし、**パッケージ マネージャー コンソール**します。</span><span class="sxs-lookup"><span data-stu-id="53416-178">From the **Tools** menu in Visual Studio, select **Library Package Manager**, and then click **Package Manager Console**.</span></span>
4. <span data-ttu-id="53416-179">**パッケージ マネージャー コンソール**、次のコマンドを入力し、キーを押します**Enter**します。</span><span class="sxs-lookup"><span data-stu-id="53416-179">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span> <span data-ttu-id="53416-180">既存のモデルに基づく最初の移行が作成されます。</span><span class="sxs-lookup"><span data-stu-id="53416-180">An initial migration based on the existing model will be created.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample1.ps1)]

    <span data-ttu-id="53416-181">![移行を有効にする](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "Migrations を有効にします。")</span><span class="sxs-lookup"><span data-stu-id="53416-181">![Enabling Migrations](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "Enabling Migrations")</span></span>

    <span data-ttu-id="53416-182">*移行を有効にします。*</span><span class="sxs-lookup"><span data-stu-id="53416-182">*Enabling Migrations*</span></span>

    > [!NOTE]
    > <span data-ttu-id="53416-183">このコマンドを追加、**移行**というフォルダーにファイルを含むギーク Quiz プロジェクト**Configuration.cs**します。</span><span class="sxs-lookup"><span data-stu-id="53416-183">This command adds a **Migrations** folder to Geek Quiz project that contains a file called **Configuration.cs**.</span></span> <span data-ttu-id="53416-184">**構成**クラスでは、コンテキストの移行の動作を構成できます。</span><span class="sxs-lookup"><span data-stu-id="53416-184">The **Configuration** class allows you to configure how Migrations behaves for your context.</span></span>
5. <span data-ttu-id="53416-185">移行が有効になっている、更新する必要があります、**構成**初期データと共にデータベースを設定するクラスを**ギーク Quiz**が必要です。</span><span class="sxs-lookup"><span data-stu-id="53416-185">With Migrations enabled, you need to update the **Configuration** class to populate the database with the initial data that **Geek Quiz** requires.</span></span> <span data-ttu-id="53416-186">**移行**、置換、 **Configuration.cs**に 1 つのファイルがある、 **Source\Assets**このラボのフォルダー。</span><span class="sxs-lookup"><span data-stu-id="53416-186">Under **Migrations**, replace the **Configuration.cs** file with the one located in the **Source\Assets** folder of this lab.</span></span>

    > [!NOTE]
    > <span data-ttu-id="53416-187">**移行**が呼び出す、**シード**メソッドすべてのデータベース更新をデータベース内のレコードが重複していないことを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="53416-187">Since **Migrations** will call the **Seed** method with every database update, you need to be sure that records are not duplicated in the database.</span></span> <span data-ttu-id="53416-188">**AddOrUpdate**方法は、データの重複を防ぐために役立ちます。</span><span class="sxs-lookup"><span data-stu-id="53416-188">The **AddOrUpdate** method will help to prevent duplicate data.</span></span>
6. <span data-ttu-id="53416-189">最初の移行を追加するに次のコマンドを入力し、キーを押します**Enter**します。</span><span class="sxs-lookup"><span data-stu-id="53416-189">To add an initial migration, enter the following command and then press **Enter**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="53416-190">という名前のデータベースがないことを確認&quot;GeekQuizProd&quot; LocalDB インスタンスにします。</span><span class="sxs-lookup"><span data-stu-id="53416-190">Make sure that there is no database named &quot;GeekQuizProd&quot; in your LocalDB instance.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample2.ps1)]

    <span data-ttu-id="53416-191">![ベース スキーマの移行を追加する](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "追加ベース スキーマの移行")</span><span class="sxs-lookup"><span data-stu-id="53416-191">![Adding base schema migration](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "Adding base schema migration")</span></span>

    <span data-ttu-id="53416-192">*ベース スキーマの移行を追加します。*</span><span class="sxs-lookup"><span data-stu-id="53416-192">*Adding base schema migration*</span></span>

    > [!NOTE]
    > <span data-ttu-id="53416-193">**Add-migration**最後の移行が作成されたため、モデルに加えられた変更内容に基づいて次の移行をスキャフォールディングします。</span><span class="sxs-lookup"><span data-stu-id="53416-193">**Add-Migration** will scaffold the next migration based on changes you have made to your model since the last migration was created.</span></span> <span data-ttu-id="53416-194">ここでは、プロジェクトの最初の移行は、これはスクリプトを追加で定義されているすべてのテーブルを作成する、 **TriviaContext**クラス。</span><span class="sxs-lookup"><span data-stu-id="53416-194">In this case, as it is the first migration of the project, it will add the scripts to create all the tables defined in the **TriviaContext** class.</span></span>
7. <span data-ttu-id="53416-195">次のコマンドを実行して、データベースの更新への移行を実行します。</span><span class="sxs-lookup"><span data-stu-id="53416-195">Execute the migration to update the database by running the following command.</span></span> <span data-ttu-id="53416-196">このコマンドを指定します、 **Verbose**先データベースに適用される SQL ステートメントを表示するフラグ。</span><span class="sxs-lookup"><span data-stu-id="53416-196">For this command you will specify the **Verbose** flag to view the SQL statements being applied to the target database.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample3.ps1)]

    <span data-ttu-id="53416-197">![最初のデータベースを作成する](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "初期データベースを作成します。")</span><span class="sxs-lookup"><span data-stu-id="53416-197">![Creating initial database](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "Creating initial database")</span></span>

    <span data-ttu-id="53416-198">*初期データベースを作成します。*</span><span class="sxs-lookup"><span data-stu-id="53416-198">*Creating initial database*</span></span>

    > [!NOTE]
    > <span data-ttu-id="53416-199">**Update Database**保留中の移行をデータベースに適用されます。</span><span class="sxs-lookup"><span data-stu-id="53416-199">**Update-Database** will apply any pending migrations to the database.</span></span> <span data-ttu-id="53416-200">この場合、web.config ファイルで定義されている接続文字列を使用してデータベースを作成、されます。</span><span class="sxs-lookup"><span data-stu-id="53416-200">In this case, it will create the database using the connection string defined in your web.config file.</span></span>
8. <span data-ttu-id="53416-201">移動して**ビュー**メニューを開く**SQL Server オブジェクト エクスプ ローラー**します。</span><span class="sxs-lookup"><span data-stu-id="53416-201">Go to **View** menu and open **SQL Server Object Explorer**.</span></span>

    <span data-ttu-id="53416-202">![SQL Server オブジェクト エクスプ ローラーで開く](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "SQL Server オブジェクト エクスプ ローラーで開く")</span><span class="sxs-lookup"><span data-stu-id="53416-202">![Open in SQL Server Object Explorer](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "Open in SQL Server Object Explorer")</span></span>

    <span data-ttu-id="53416-203">*SQL Server オブジェクト エクスプ ローラーで開く*</span><span class="sxs-lookup"><span data-stu-id="53416-203">*Open in SQL Server Object Explorer*</span></span>
9. <span data-ttu-id="53416-204">**SQL Server オブジェクト エクスプ ローラー**ウィンドウを右クリックして、LocalDB インスタンスに接続、 **SQL Server**ノードと選択**SQL Server の追加.** オプション。</span><span class="sxs-lookup"><span data-stu-id="53416-204">In the **SQL Server Object Explorer** window, connect to your LocalDB instance by right-clicking the **SQL Server** node and selecting **Add SQL Server...** option.</span></span>

    <span data-ttu-id="53416-205">![SQL Server のインスタンスを追加する](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "SQL Server のインスタンスを追加します。")</span><span class="sxs-lookup"><span data-stu-id="53416-205">![Adding a SQL Server Instance](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "Adding a SQL Server Instance")</span></span>

    <span data-ttu-id="53416-206">*SQL Server オブジェクト エクスプ ローラーへの SQL Server インスタンスの追加*</span><span class="sxs-lookup"><span data-stu-id="53416-206">*Adding a SQL Server instance to SQL Server Object Explorer*</span></span>
10. <span data-ttu-id="53416-207">設定、**サーバー名**に *(localdb) \v11.0*して**Windows 認証**認証モードとして。</span><span class="sxs-lookup"><span data-stu-id="53416-207">Set the **server name** to *(localdb)\v11.0* and leave **Windows Authentication** as your authentication mode.</span></span> <span data-ttu-id="53416-208">クリックして**Connect**を続行します。</span><span class="sxs-lookup"><span data-stu-id="53416-208">Click **Connect** to continue.</span></span>

    <span data-ttu-id="53416-209">![LocalDB への接続](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "LocalDB への接続")</span><span class="sxs-lookup"><span data-stu-id="53416-209">![Connecting to LocalDB](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "Connecting to LocalDB")</span></span>

    <span data-ttu-id="53416-210">*LocalDB への接続*</span><span class="sxs-lookup"><span data-stu-id="53416-210">*Connecting to LocalDB*</span></span>
11. <span data-ttu-id="53416-211">開く、 **GeekQuizProd**データベースし、展開、**テーブル**ノード。</span><span class="sxs-lookup"><span data-stu-id="53416-211">Open the **GeekQuizProd** database and expand the **Tables** node.</span></span> <span data-ttu-id="53416-212">ご覧のとおり、 **Update-database**コマンドで定義されているすべてのテーブルを生成する、 **TriviaContext**クラス。</span><span class="sxs-lookup"><span data-stu-id="53416-212">As you can see, the **Update-Database** command generated all the tables defined in the **TriviaContext** class.</span></span> <span data-ttu-id="53416-213">検索、 **dbo します。TriviaQuestions**テーブルし、列のノードを開きます。</span><span class="sxs-lookup"><span data-stu-id="53416-213">Locate the **dbo.TriviaQuestions** table and open the columns node.</span></span> <span data-ttu-id="53416-214">次のタスクでは、このテーブルに新しい列を追加し、データベースを使用して、更新**移行**します。</span><span class="sxs-lookup"><span data-stu-id="53416-214">In the next task, you will add a new column to this table and update the database using **Migrations**.</span></span>

    <span data-ttu-id="53416-215">![トリビアは、列を質問](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "トリビアの列の質問")</span><span class="sxs-lookup"><span data-stu-id="53416-215">![Trivia Questions Columns](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "Trivia Questions Columns")</span></span>

    <span data-ttu-id="53416-216">*トリビアの列を質問します。*</span><span class="sxs-lookup"><span data-stu-id="53416-216">*Trivia Questions Columns*</span></span>

<a id="Ex1Task2"></a>
#### <a name="task-2--updating-database-schema-using-migrations"></a><span data-ttu-id="53416-217">タスク 2 – Migrations を使用してデータベース スキーマの更新</span><span class="sxs-lookup"><span data-stu-id="53416-217">Task 2 – Updating Database Schema Using Migrations</span></span>

<span data-ttu-id="53416-218">このタスクで使用する**Entity Framework Code First Migrations**モデルの変更を検出し、データベースを更新するために必要なコードを生成します。</span><span class="sxs-lookup"><span data-stu-id="53416-218">In this task, you will use **Entity Framework Code First Migrations** to detect a change in your model and generate the necessary code to update the database.</span></span> <span data-ttu-id="53416-219">更新は、 **TriviaQuestions**エンティティに新しいプロパティを追加しています。</span><span class="sxs-lookup"><span data-stu-id="53416-219">You will update the **TriviaQuestions** entity by adding a new property to it.</span></span> <span data-ttu-id="53416-220">テーブルに新しい列を含めるには新しい移行を作成するためのコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="53416-220">Then you will run commands to create a new Migration to include the new column in the table.</span></span>

1. <span data-ttu-id="53416-221">**ソリューション エクスプ ローラー**、ダブルクリックして、 **TriviaQuestion.cs**ファイル内にある、**モデル**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="53416-221">In **Solution Explorer**, double-click the **TriviaQuestion.cs** file located inside the **Models** folder.</span></span>
2. <span data-ttu-id="53416-222">という名前の新しいプロパティを追加**ヒント**の次のコード スニペットに示すようにします。</span><span class="sxs-lookup"><span data-stu-id="53416-222">Add a new property named **Hint**, as shown in the following code snippet.</span></span>

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample4.cs)]
3. <span data-ttu-id="53416-223">**パッケージ マネージャー コンソール**、次のコマンドを入力し、キーを押します**Enter**します。</span><span class="sxs-lookup"><span data-stu-id="53416-223">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span> <span data-ttu-id="53416-224">私たちのモデルの変更を反映した新しい移行が作成されます。</span><span class="sxs-lookup"><span data-stu-id="53416-224">A new migration will be created reflecting the change in our model.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample5.ps1)]

    <span data-ttu-id="53416-225">![Add-migration](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "Add-migration")</span><span class="sxs-lookup"><span data-stu-id="53416-225">![Add-Migration](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "Add-Migration")</span></span>

    <span data-ttu-id="53416-226">*追加の移行*</span><span class="sxs-lookup"><span data-stu-id="53416-226">*Add-Migration*</span></span>

    > [!NOTE]
    > <span data-ttu-id="53416-227">移行ファイルが 2 つの方法で構成される**を**と**ダウン**します。</span><span class="sxs-lookup"><span data-stu-id="53416-227">A Migration file is composed of two methods, **Up** and **Down**.</span></span>
    > 
    > - <span data-ttu-id="53416-228">**を**どのような変更をデータベースに適用する、アプリケーションのニーズの現在のバージョンの指定に使用されるメソッド。</span><span class="sxs-lookup"><span data-stu-id="53416-228">The **Up** method will be used to specify what changes the current version of our application need to apply to the database.</span></span>
    > - <span data-ttu-id="53416-229">**ダウン**に追加した変更を取り消すために使用、**を**メソッド。</span><span class="sxs-lookup"><span data-stu-id="53416-229">The **Down** is used to reverse the changes we have added to the **Up** method.</span></span>
    > 
    > <span data-ttu-id="53416-230">データベースの移行では、データベースを更新と、は、タイムスタンプの順序と前回の更新以降に使用されていないものだけですべての移行が実行されます (、 \_MigrationHistory テーブルの追跡どの移行が適用されています)。</span><span class="sxs-lookup"><span data-stu-id="53416-230">When the Database Migration updates the database, it will run all migrations in the timestamp order, and only those that have not been used since the last update (The \_MigrationHistory table keeps track of which migrations have been applied).</span></span> <span data-ttu-id="53416-231">**を**すべての移行のメソッドが呼び出され、データベースを指定して、変更すると、します。</span><span class="sxs-lookup"><span data-stu-id="53416-231">The **Up** method of all migrations will be called and will make the changes we have specified to the database.</span></span> <span data-ttu-id="53416-232">以前の移行に戻るになった場合、**ダウン**逆の順序の変更を元に戻すメソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="53416-232">If we decide to go back to a previous migration, the **Down** method will be called to redo the changes in a reverse order.</span></span>
4. <span data-ttu-id="53416-233">**パッケージ マネージャー コンソール**、次のコマンドを入力し、キーを押します**Enter**します。</span><span class="sxs-lookup"><span data-stu-id="53416-233">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample6.ps1)]
5. <span data-ttu-id="53416-234">出力、 **Update-database**生成されたコマンド、 **Alter Table**に新しい列を追加する SQL ステートメント、 **TriviaQuestions**テーブル、次の図に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="53416-234">The output of the **Update-Database** command generated an **Alter Table** SQL statement to add a new column to the **TriviaQuestions** table, as shown in the image below.</span></span>

    <span data-ttu-id="53416-235">![生成される SQL ステートメントの列を追加](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "生成される SQL ステートメントの列の追加")</span><span class="sxs-lookup"><span data-stu-id="53416-235">![Add column SQL statement generated](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "Add column SQL statement generated")</span></span>

    <span data-ttu-id="53416-236">*生成される SQL ステートメントの列を追加します。*</span><span class="sxs-lookup"><span data-stu-id="53416-236">*Add column SQL statement generated*</span></span>
6. <span data-ttu-id="53416-237">**SQL Server オブジェクト エクスプ ローラー**、更新、 **dbo します。TriviaQuestions**テーブル、いることを確認し、新しい**ヒント**列が表示されます。</span><span class="sxs-lookup"><span data-stu-id="53416-237">In **SQL Server Object Explorer**, refresh the **dbo.TriviaQuestions** table and check that the new **Hint** column is displayed.</span></span>

    <span data-ttu-id="53416-238">![新しいヒント列を示す](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "新しいヒント列の表示")</span><span class="sxs-lookup"><span data-stu-id="53416-238">![Showing the new Hint Column](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "Showing the new Hint Column")</span></span>

    <span data-ttu-id="53416-239">*新しいヒント列の表示*</span><span class="sxs-lookup"><span data-stu-id="53416-239">*Showing the new Hint Column*</span></span>
7. <span data-ttu-id="53416-240">戻り、 **TriviaQuestion.cs**エディター、追加、 **StringLength**に制約、*ヒント*プロパティは、次のコード スニペットに示すようにします。</span><span class="sxs-lookup"><span data-stu-id="53416-240">Back in the **TriviaQuestion.cs** editor, add a **StringLength** constraint to the *Hint* property, as shown in the following code snippet.</span></span>

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample7.cs)]
8. <span data-ttu-id="53416-241">**パッケージ マネージャー コンソール**、次のコマンドを入力し、キーを押します**Enter**します。</span><span class="sxs-lookup"><span data-stu-id="53416-241">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample8.ps1)]
9. <span data-ttu-id="53416-242">**パッケージ マネージャー コンソール**、次のコマンドを入力し、キーを押します**Enter**します。</span><span class="sxs-lookup"><span data-stu-id="53416-242">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample9.ps1)]
10. <span data-ttu-id="53416-243">出力、 **Update-database**生成されたコマンド、 **Alter Table**を更新する SQL ステートメント、*ヒント*の列の型、 **TriviaQuestions**テーブル、次の図に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="53416-243">The output of the **Update-Database** command generated an **Alter Table** SQL statement to update the *hint* column type of the **TriviaQuestions** table, as shown in the image below.</span></span>

    <span data-ttu-id="53416-244">![生成される SQL ステートメントの列を alter](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "Alter column SQL ステートメントの生成")</span><span class="sxs-lookup"><span data-stu-id="53416-244">![Alter column SQL statement generated](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "Alter column SQL statement generated")</span></span>

    <span data-ttu-id="53416-245">*Alter column SQL ステートメントの生成*</span><span class="sxs-lookup"><span data-stu-id="53416-245">*Alter column SQL statement generated*</span></span>
11. <span data-ttu-id="53416-246">**SQL Server オブジェクト エクスプ ローラー**、更新、 **dbo します。TriviaQuestions**テーブルが表示されことを確認、**ヒント**列の型が**nvarchar(150)** します。</span><span class="sxs-lookup"><span data-stu-id="53416-246">In **SQL Server Object Explorer**, refresh the **dbo.TriviaQuestions** table and check that the **Hint** column type is **nvarchar(150)**.</span></span>

    <span data-ttu-id="53416-247">![新しい制約を示す](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "新しい制約を示す")</span><span class="sxs-lookup"><span data-stu-id="53416-247">![Showing the new constraint](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "Showing the new constraint")</span></span>

    <span data-ttu-id="53416-248">*新しい制約を示す*</span><span class="sxs-lookup"><span data-stu-id="53416-248">*Showing the new constraint*</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-deploying-a-web-app-to-staging"></a><span data-ttu-id="53416-249">手順 2: ステージング Web アプリを展開します。</span><span class="sxs-lookup"><span data-stu-id="53416-249">Exercise 2: Deploying a Web App to Staging</span></span>

<span data-ttu-id="53416-250">**Web アプリを Azure App Service で**ステージングされた発行を実行することができます。</span><span class="sxs-lookup"><span data-stu-id="53416-250">**Web Apps in Azure App Service** enables you to perform staged publishing.</span></span> <span data-ttu-id="53416-251">ステージングされた発行では、既定の運用サイトごとにステージング サイト スロットを作成し、ダウンタイムなしでこれらのスロットをスワップすることができます。</span><span class="sxs-lookup"><span data-stu-id="53416-251">Staged publishing creates a staging site slot for each default production site and enables you to swap these slots with no down time.</span></span> <span data-ttu-id="53416-252">これは、パブリックにリリースする前に変更を検証、段階的サイトのコンテンツを統合し、変更が期待どおりに動作していない場合はロールバックを本当に便利です。</span><span class="sxs-lookup"><span data-stu-id="53416-252">This is really useful to validate changes before releasing to the public, incrementally integrate site content, and roll back if changes are not working as expected.</span></span>

<span data-ttu-id="53416-253">この演習ではデプロイ、**ギーク Quiz** Git ソース管理を使用して web アプリのステージング環境へのアプリケーション。</span><span class="sxs-lookup"><span data-stu-id="53416-253">In this exercise, you will deploy the **Geek Quiz** application to the staging environment of your web app using Git source control.</span></span> <span data-ttu-id="53416-254">これを行うが web アプリを作成し、管理ポータルで必要なコンポーネントのプロビジョニング、構成、 **Git**リポジトリとプッシュ アプリケーション ソース コードをローカル コンピューターから、ステージング スロットにします。</span><span class="sxs-lookup"><span data-stu-id="53416-254">To do this, you will create the web app and provision the required components at the management portal, configure a **Git** repository and push the application source code from your local computer to the staging slot.</span></span> <span data-ttu-id="53416-255">実稼働データベースを更新することもが、 **Code First Migrations**前の演習で作成しました。</span><span class="sxs-lookup"><span data-stu-id="53416-255">You will also update your production database with the **Code First Migrations** you created in the previous exercise.</span></span> <span data-ttu-id="53416-256">その操作を確認するには、このテスト環境でアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="53416-256">You will then execute the application in this test environment to verify its operation.</span></span> <span data-ttu-id="53416-257">問題がなければ、期待どおりに動作しているそのこと、運用環境にアプリケーションが昇格されます。</span><span class="sxs-lookup"><span data-stu-id="53416-257">Once you are satisfied that it is working according to your expectations, you will promote the application to production.</span></span>

> [!NOTE]
> <span data-ttu-id="53416-258">ステージングされた発行を有効にするのには、web アプリである必要があります**標準モード**します。</span><span class="sxs-lookup"><span data-stu-id="53416-258">To enable staged publishing, the web app must be in **Standard mode**.</span></span> <span data-ttu-id="53416-259">Web アプリを標準モードに変更する場合は、追加料金が発生することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="53416-259">Note that additional charges will be incurred if you change your web app to Standard mode.</span></span> <span data-ttu-id="53416-260">価格の詳細については、次を参照してください。 [App Service の価格](https://azure.microsoft.com/pricing/details/app-service/)します。</span><span class="sxs-lookup"><span data-stu-id="53416-260">For more information about pricing, see [App Service Pricing](https://azure.microsoft.com/pricing/details/app-service/).</span></span>


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-web-app-in-azure-app-service"></a><span data-ttu-id="53416-261">タスク 1 – Azure App Service で Web アプリの作成</span><span class="sxs-lookup"><span data-stu-id="53416-261">Task 1 – Creating a Web App in Azure App Service</span></span>

<span data-ttu-id="53416-262">このタスクでの web アプリを作成します**Azure App Service**管理ポータルから。</span><span class="sxs-lookup"><span data-stu-id="53416-262">In this task, you will create a web app in **Azure App Service** from the management portal.</span></span> <span data-ttu-id="53416-263">構成することもが、 **SQL Database**アプリケーションのデータを保持し、ソース コントロールのローカル Git リポジトリを構成します。</span><span class="sxs-lookup"><span data-stu-id="53416-263">You will also configure a **SQL Database** to persist the application data, and configure a local Git repository for source control.</span></span>

1. <span data-ttu-id="53416-264">移動して、 [Azure 管理ポータル](https://manage.windowsazure.com)サブスクリプションに関連付けられた Microsoft アカウントを使用してサインインします。</span><span class="sxs-lookup"><span data-stu-id="53416-264">Go to the [Azure management portal](https://manage.windowsazure.com) and sign in using the Microsoft account associated with your subscription.</span></span>

    ![Azure 管理ポータルにサインインするには](maintainable-azure-websites-managing-change-and-scale/_static/image13.png)

    <span data-ttu-id="53416-266">*Azure 管理ポータルにサインインするには*</span><span class="sxs-lookup"><span data-stu-id="53416-266">*Sign in to the Azure management portal*</span></span>
2. <span data-ttu-id="53416-267">クリックして**新規**ページの下部にあるコマンド バーにします。</span><span class="sxs-lookup"><span data-stu-id="53416-267">Click **New** in the command bar at the bottom of the page.</span></span>

    <span data-ttu-id="53416-268">![新しい web アプリを作成する](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "新しい web アプリを作成します。")</span><span class="sxs-lookup"><span data-stu-id="53416-268">![Creating a new web app](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "Creating a new web app")</span></span>

    <span data-ttu-id="53416-269">*新しい web アプリを作成します。*</span><span class="sxs-lookup"><span data-stu-id="53416-269">*Creating a new web app*</span></span>
3. <span data-ttu-id="53416-270">クリックして**コンピューティング**、 **web サイト**し**カスタム作成**です。</span><span class="sxs-lookup"><span data-stu-id="53416-270">Click **Compute**, **Website** and then **Custom Create**.</span></span>

    <span data-ttu-id="53416-271">![カスタム作成を使用して新しい web アプリを作成する](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "カスタム作成を使用して新しい web アプリを作成します。")</span><span class="sxs-lookup"><span data-stu-id="53416-271">![Creating a new web app using Custom Create](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "Creating a new web app using Custom Create")</span></span>

    <span data-ttu-id="53416-272">*カスタム作成を使用して新しい web アプリを作成します。*</span><span class="sxs-lookup"><span data-stu-id="53416-272">*Creating a new web app using Custom Create*</span></span>
4. <span data-ttu-id="53416-273">**新規 web サイト - カスタム作成** ダイアログ ボックスで、使用可能な入力**URL** (例:*おたくクイズ*)、場所を選択、**リージョン**ドロップダウン リスト、および選択**新しい SQL データベースの作成**で、**データベース**ドロップダウン リスト。</span><span class="sxs-lookup"><span data-stu-id="53416-273">In the **New Website - Custom Create** dialog box, provide an available **URL** (e.g. *geek-quiz*), select a location in the **Region** drop-down list, and select **Create a new SQL database** in the **Database** drop-down list.</span></span> <span data-ttu-id="53416-274">最後に、選択、**ソース管理から発行** チェック ボックスをクリックします**次**します。</span><span class="sxs-lookup"><span data-stu-id="53416-274">Finally, select the **Publish from source control** check box and click **Next**.</span></span>

    ![新しい web アプリをカスタマイズします。](maintainable-azure-websites-managing-change-and-scale/_static/image16.png)

    <span data-ttu-id="53416-276">*新しい web アプリをカスタマイズします。*</span><span class="sxs-lookup"><span data-stu-id="53416-276">*Customizing the new web app*</span></span>
5. <span data-ttu-id="53416-277">データベースの設定の次の情報を指定します。</span><span class="sxs-lookup"><span data-stu-id="53416-277">Specify the following information for the database settings:</span></span>

   - <span data-ttu-id="53416-278">**名前**テキスト ボックスに、データベース名を入力します (例: *geekquiz\_db*)</span><span class="sxs-lookup"><span data-stu-id="53416-278">In the **Name** text box, enter a database name (e.g. *geekquiz\_db*)</span></span>
   - <span data-ttu-id="53416-279">サーバーで**ドロップダウン**一覧で、**新しい SQL database server**します。</span><span class="sxs-lookup"><span data-stu-id="53416-279">In the Server **drop-down** list, select **New SQL database server**.</span></span> <span data-ttu-id="53416-280">または、既存のサーバーを選択できます。</span><span class="sxs-lookup"><span data-stu-id="53416-280">Alternatively, you can select an existing server.</span></span>
   - <span data-ttu-id="53416-281">**データベース ユーザー名**と**データベース パスワード**ボックスに、SQL database のサーバー管理者のユーザー名とパスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="53416-281">In the **Database username** and **Database password** boxes, enter the administrator username and password for the SQL database server.</span></span> <span data-ttu-id="53416-282">サーバーを選択する場合は、既に作成して、パスワードを求められます。</span><span class="sxs-lookup"><span data-stu-id="53416-282">If you select a server you have already created, you will be prompted for the password.</span></span>

     ![データベースの設定を指定します。](maintainable-azure-websites-managing-change-and-scale/_static/image17.png)

     <span data-ttu-id="53416-284">*データベースの設定を指定します。*</span><span class="sxs-lookup"><span data-stu-id="53416-284">*Specifying the database settings*</span></span>
6. <span data-ttu-id="53416-285">**[次へ]** をクリックして、続行します。</span><span class="sxs-lookup"><span data-stu-id="53416-285">Click **Next** to continue.</span></span>
7. <span data-ttu-id="53416-286">選択**ローカル Git リポジトリ**してをクリックしてソース管理の**次**。</span><span class="sxs-lookup"><span data-stu-id="53416-286">Select **Local Git repository** for the source control to use and click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="53416-287">展開資格情報 (ユーザー名とパスワード) が求められるあります。</span><span class="sxs-lookup"><span data-stu-id="53416-287">You may be prompted for the deployment credentials (a username and password).</span></span>

    ![Git リポジトリを作成します。](maintainable-azure-websites-managing-change-and-scale/_static/image18.png)

    <span data-ttu-id="53416-289">*Git リポジトリを作成します。*</span><span class="sxs-lookup"><span data-stu-id="53416-289">*Creating the Git repository*</span></span>
8. <span data-ttu-id="53416-290">新しい web アプリが作成されるまで待機します。</span><span class="sxs-lookup"><span data-stu-id="53416-290">Wait until the new web app is created.</span></span>

    > [!NOTE]
    > <span data-ttu-id="53416-291">既定では、Azure には、ドメインに*azurewebsites.net*が、Azure 管理ポータルを使用してカスタム ドメインを設定することができます。</span><span class="sxs-lookup"><span data-stu-id="53416-291">By default, Azure provides domains at *azurewebsites.net* but also gives you the possibility to set custom domains using the Azure management portal.</span></span> <span data-ttu-id="53416-292">ただし、特定の Azure App Service のモードを使用している場合、のみカスタム ドメインを管理することができます。</span><span class="sxs-lookup"><span data-stu-id="53416-292">However, you can only manage custom domains if you are using certain Azure App Service modes.</span></span>
    > 
    > <span data-ttu-id="53416-293">Azure App Service は、Free、Shared、Basic、Standard、および Premium エディションで使用できます。</span><span class="sxs-lookup"><span data-stu-id="53416-293">Azure App Service is available in Free, Shared, Basic, Standard, and Premium editions.</span></span> <span data-ttu-id="53416-294">Free および Shared のモードでは、すべての web アプリはマルチ テナント環境で実行され、CPU、メモリ、およびネットワークの使用量クォータがあります。</span><span class="sxs-lookup"><span data-stu-id="53416-294">In Free and Shared mode, all web apps run in a multi-tenant environment and have quotas for CPU, Memory, and Network usage.</span></span> <span data-ttu-id="53416-295">プランの無料のアプリの最大数が異なる場合があります。</span><span class="sxs-lookup"><span data-stu-id="53416-295">The maximum number of free apps may vary with your plan.</span></span> <span data-ttu-id="53416-296">標準モードでは、標準の Azure への対応する専用の仮想マシンで実行されるアプリはコンピューティング リソースを選択します。</span><span class="sxs-lookup"><span data-stu-id="53416-296">In Standard mode, you choose which apps run on dedicated virtual machines that correspond to the standard Azure compute resources.</span></span> <span data-ttu-id="53416-297">Web アプリ モードの構成を見つけることができます、**スケール**web アプリのメニュー。</span><span class="sxs-lookup"><span data-stu-id="53416-297">You can find the web app mode configuration in the **Scale** menu of your web app.</span></span>
    > 
    > <span data-ttu-id="53416-298">![Azure App Service モード](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "Azure App Service のモード")</span><span class="sxs-lookup"><span data-stu-id="53416-298">![Azure App Service Modes](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "Azure App Service Modes")</span></span>
    > 
    > <span data-ttu-id="53416-299">使用する場合**Shared**または**標準**モードができます、アプリに移動して、web アプリ用のカスタム ドメインを管理する**構成**をクリックしてメニュー**ドメインの管理***ドメイン名*します。</span><span class="sxs-lookup"><span data-stu-id="53416-299">If you are using **Shared** or **Standard** mode, you will be able to manage custom domains for your web app by going to your app's **Configure** menu and clicking **Manage Domains** under *domain names*.</span></span>
    > 
    > <span data-ttu-id="53416-300">![ドメインの管理](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "ドメインの管理")</span><span class="sxs-lookup"><span data-stu-id="53416-300">![Manage Domains](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "Manage Domains")</span></span>
    > 
    > <span data-ttu-id="53416-301">![カスタム ドメインを管理](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "カスタム ドメインの管理")</span><span class="sxs-lookup"><span data-stu-id="53416-301">![Manage Custom Domains](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "Manage Custom Domains")</span></span>
9. <span data-ttu-id="53416-302">Web アプリを作成した後は、下のリンクをクリックして、 **URL**新しい web アプリが実行されていることを確認する列。</span><span class="sxs-lookup"><span data-stu-id="53416-302">Once the web app is created, click the link under the **URL** column to check that the new web app is running.</span></span>

    ![新しい web アプリを参照](maintainable-azure-websites-managing-change-and-scale/_static/image22.png)

    <span data-ttu-id="53416-304">*新しい web アプリを参照*</span><span class="sxs-lookup"><span data-stu-id="53416-304">*Browsing to the new web app*</span></span>

    ![実行されている web アプリ](maintainable-azure-websites-managing-change-and-scale/_static/image23.png)

    <span data-ttu-id="53416-306">*実行されている web アプリ*</span><span class="sxs-lookup"><span data-stu-id="53416-306">*web app running*</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-production-sql-database"></a><span data-ttu-id="53416-307">タスク 2 – 運用環境の SQL データベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="53416-307">Task 2 – Creating the Production SQL Database</span></span>

<span data-ttu-id="53416-308">このタスクでは、使用、 **Entity Framework Code First Migrations**対象とするデータベースを作成する、 **Azure SQL Database**前のタスクで作成したインスタンス。</span><span class="sxs-lookup"><span data-stu-id="53416-308">In this task, you will use the **Entity Framework Code First Migrations** to create the database targeting the **Azure SQL Database** instance you created in the previous task.</span></span>

1. <span data-ttu-id="53416-309">管理ポータルで、前のタスクで作成した web アプリに移動しに移動その**ダッシュ ボード**します。</span><span class="sxs-lookup"><span data-stu-id="53416-309">In the Management Portal, navigate to the web app you created in the previous task and go to its **Dashboard**.</span></span>
2. <span data-ttu-id="53416-310">**ダッシュ ボード**] ページで [**接続文字列を表示**下のリンク、**概要**セクション。</span><span class="sxs-lookup"><span data-stu-id="53416-310">In the **Dashboard** page, click **View connection strings** link under the **quick glance** section.</span></span>

    <span data-ttu-id="53416-311">![接続文字列を表示](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "接続文字列を表示")</span><span class="sxs-lookup"><span data-stu-id="53416-311">![View connection strings](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "View connection strings")</span></span>

    <span data-ttu-id="53416-312">*接続文字列の表示*</span><span class="sxs-lookup"><span data-stu-id="53416-312">*View connection strings*</span></span>
3. <span data-ttu-id="53416-313">コピー、**接続文字列**値し、ダイアログ ボックスを閉じます。</span><span class="sxs-lookup"><span data-stu-id="53416-313">Copy the **connection string** value and close the dialog box.</span></span>

    <span data-ttu-id="53416-314">![Azure 管理ポータルで接続文字列](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "Azure 管理ポータルで接続文字列")</span><span class="sxs-lookup"><span data-stu-id="53416-314">![Connection String in Azure Management Portal](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "Connection String in Azure Management Portal")</span></span>

    <span data-ttu-id="53416-315">*Azure 管理ポータルで接続文字列*</span><span class="sxs-lookup"><span data-stu-id="53416-315">*Connection String in Azure Management Portal*</span></span>
4. <span data-ttu-id="53416-316">クリックして**SQL データベース**で、Azure SQL データベースの一覧を表示するには</span><span class="sxs-lookup"><span data-stu-id="53416-316">Click **SQL Databases** to see the list of SQL databases in Azure</span></span>

    <span data-ttu-id="53416-317">![SQL Database メニューの ](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "SQL Database のメニュー")</span><span class="sxs-lookup"><span data-stu-id="53416-317">![SQL Database menu](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "SQL Database menu")</span></span>

    <span data-ttu-id="53416-318">*SQL Database のメニュー*</span><span class="sxs-lookup"><span data-stu-id="53416-318">*SQL Database menu*</span></span>
5. <span data-ttu-id="53416-319">前のタスクで作成したデータベースを検索し、[サーバー] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="53416-319">Locate the database you created in the previous task and click on the Server.</span></span>

    <span data-ttu-id="53416-320">![SQL Database サーバー](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "SQL Database サーバー")</span><span class="sxs-lookup"><span data-stu-id="53416-320">![SQL Database server](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "SQL Database server")</span></span>

    <span data-ttu-id="53416-321">*SQL Database サーバー*</span><span class="sxs-lookup"><span data-stu-id="53416-321">*SQL Database server*</span></span>
6. <span data-ttu-id="53416-322">**クイック スタート**ページ サーバーのクリックして**構成**します。</span><span class="sxs-lookup"><span data-stu-id="53416-322">In the **Quick Start** page of the server, click on **Configure**.</span></span>

    <span data-ttu-id="53416-323">![構成メニュー](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "構成メニュー")</span><span class="sxs-lookup"><span data-stu-id="53416-323">![Configure menu](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "Configure menu")</span></span>

    <span data-ttu-id="53416-324">*メニューを構成します。*</span><span class="sxs-lookup"><span data-stu-id="53416-324">*Configure menu*</span></span>
7. <span data-ttu-id="53416-325">**使用できる IP アドレス**セクションで、をクリックして**許可された IP アドレスを追加**ip アドレスを SQL Database サーバーへの接続を有効にするリンク。</span><span class="sxs-lookup"><span data-stu-id="53416-325">In the **Allowed IP addresses** section, click on **Add to the allowed IP addresses** link to enable your IP to connect to the SQL Database server.</span></span>

    <span data-ttu-id="53416-326">![使用できる IP アドレス](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "使用できる IP アドレス")</span><span class="sxs-lookup"><span data-stu-id="53416-326">![Allowed IP addresses](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "Allowed IP addresses")</span></span>

    <span data-ttu-id="53416-327">*許可された IP アドレス*</span><span class="sxs-lookup"><span data-stu-id="53416-327">*Allowed IP addresses*</span></span>
8. <span data-ttu-id="53416-328">クリックして**保存**手順を実行するページの下部にあります。</span><span class="sxs-lookup"><span data-stu-id="53416-328">Click **Save** at the bottom of the page to complete the step.</span></span>
9. <span data-ttu-id="53416-329">Visual Studio に切り替えます。</span><span class="sxs-lookup"><span data-stu-id="53416-329">Switch back to Visual Studio.</span></span>
10. <span data-ttu-id="53416-330">**パッケージ マネージャー コンソール**、次のコマンドの置換を実行する *[YOUR CONNECTION STRING]* プレース ホルダーを Azure からコピーした接続文字列</span><span class="sxs-lookup"><span data-stu-id="53416-330">In the **Package Manager Console**, execute the following command replacing *[YOUR-CONNECTION-STRING]* placeholder with the connection string you copied from Azure</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample10.ps1)]

    <span data-ttu-id="53416-331">![Windows Azure SQL データベースを対象とするデータベースの更新](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "Windows Azure SQL データベースを対象とするデータベースの更新")</span><span class="sxs-lookup"><span data-stu-id="53416-331">![Update database targeting Windows Azure SQL Database](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "Update database targeting Windows Azure SQL Database")</span></span>

    <span data-ttu-id="53416-332">*Azure SQL データベースを対象とするデータベースの更新*</span><span class="sxs-lookup"><span data-stu-id="53416-332">*Update database targeting Azure SQL Database*</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3--deploying-geek-quiz-to-staging-using-git"></a><span data-ttu-id="53416-333">タスク 3 – Git を使用してステージング環境に展開するコンピューターおたくクイズ</span><span class="sxs-lookup"><span data-stu-id="53416-333">Task 3 – Deploying Geek Quiz to Staging Using Git</span></span>

<span data-ttu-id="53416-334">このタスクでは、web アプリのステージングされた発行を有効にします。</span><span class="sxs-lookup"><span data-stu-id="53416-334">In this task, you will enable staged publishing in your web app.</span></span> <span data-ttu-id="53416-335">次に、ギーク Quiz アプリケーションをローカル コンピューターから直接 web アプリのステージング環境に発行を Git を使用します。</span><span class="sxs-lookup"><span data-stu-id="53416-335">Then, you will use Git to publish the Geek Quiz application directly from your local computer to the staging environment of your web app.</span></span>

1. <span data-ttu-id="53416-336">ポータルに戻り、下で web アプリの名前をクリックして、**名前**管理ページを表示する列。</span><span class="sxs-lookup"><span data-stu-id="53416-336">Go back to the portal and click the name of the web app under the **Name** column to display the management pages.</span></span>

    ![Web アプリの管理ページを開く](maintainable-azure-websites-managing-change-and-scale/_static/image31.png)

    <span data-ttu-id="53416-338">*Web アプリの管理ページを開く*</span><span class="sxs-lookup"><span data-stu-id="53416-338">*Opening the web app management pages*</span></span>
2. <span data-ttu-id="53416-339">移動し、**スケール**ページ。</span><span class="sxs-lookup"><span data-stu-id="53416-339">Navigate to the **Scale** page.</span></span> <span data-ttu-id="53416-340">下、**全般**セクションで、**標準**構成をクリックします**保存**コマンド バーでします。</span><span class="sxs-lookup"><span data-stu-id="53416-340">Under the **general** section, select **Standard** for the configuration and click **Save** in the command bar.</span></span>

    > [!NOTE]
    > <span data-ttu-id="53416-341">現在の地域とサブスクリプションのすべての web アプリを実行する**標準**モードのままにしてください、**すべて選択**チェック ボックスをオン、**サイトの選択**構成。</span><span class="sxs-lookup"><span data-stu-id="53416-341">To run all web apps in the current region and subscription in **Standard** mode, leave the **Select All** check box selected in the **Choose Sites** configuration.</span></span> <span data-ttu-id="53416-342">それ以外の場合、オフ、**すべて選択**チェック ボックスをオンします。</span><span class="sxs-lookup"><span data-stu-id="53416-342">Otherwise, clear the **Select All** check box.</span></span>

    <span data-ttu-id="53416-343">![Web アプリを標準モードにアップグレードする](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "標準モードに web アプリをアップグレードします。")</span><span class="sxs-lookup"><span data-stu-id="53416-343">![Upgrading the web app to Standard mode](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "Upgrading the web app to Standard mode")</span></span>

    <span data-ttu-id="53416-344">*Web アプリを標準モードにアップグレードします。*</span><span class="sxs-lookup"><span data-stu-id="53416-344">*Upgrading the Web App to Standard mode*</span></span>
3. <span data-ttu-id="53416-345">クリックして**はい**変更を確認します。</span><span class="sxs-lookup"><span data-stu-id="53416-345">Click **Yes** to confirm the changes.</span></span>

    <span data-ttu-id="53416-346">![標準モードに変更を確認する](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "web アプリのモードの変更を続行")</span><span class="sxs-lookup"><span data-stu-id="53416-346">![Confirming the change to Standard mode](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "Continuing with the changing of the web app mode")</span></span>

    <span data-ttu-id="53416-347">*標準モードに変更を確認します。*</span><span class="sxs-lookup"><span data-stu-id="53416-347">*Confirming the change to Standard mode*</span></span>
4. <span data-ttu-id="53416-348">移動して、**ダッシュ ボード**ページし、をクリックして**ステージングされた発行を有効にする**下、**概要**セクション。</span><span class="sxs-lookup"><span data-stu-id="53416-348">Go to the **Dashboard** page and click **Enable staged publishing** under the **quick glance** section.</span></span>

    <span data-ttu-id="53416-349">![ステージングされた発行を有効にする](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "ステージングされた発行を有効にします。")</span><span class="sxs-lookup"><span data-stu-id="53416-349">![Enabling staged publishing](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "Enabling staged publishing")</span></span>

    <span data-ttu-id="53416-350">*ステージングされた発行を有効にします。*</span><span class="sxs-lookup"><span data-stu-id="53416-350">*Enabling staged publishing*</span></span>
5. <span data-ttu-id="53416-351">クリックして**はい**ステージングされた発行を有効にします。</span><span class="sxs-lookup"><span data-stu-id="53416-351">Click **Yes** to enable staged publishing.</span></span>

    <span data-ttu-id="53416-352">![発行を段階的な確認](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "ステージングされた発行を有効にするには、[はい] をクリックして")</span><span class="sxs-lookup"><span data-stu-id="53416-352">![Confirming staged publishing](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "Clicking Yes to enable staged publishing")</span></span>

    <span data-ttu-id="53416-353">*ステージングされた発行を確認します。*</span><span class="sxs-lookup"><span data-stu-id="53416-353">*Confirming staged publishing*</span></span>
6. <span data-ttu-id="53416-354">Web アプリの一覧では、ステージング サイト スロットを表示する web アプリ名の左側に、マークを展開します。</span><span class="sxs-lookup"><span data-stu-id="53416-354">In the list of web apps, expand the mark to the left of your web app name to display the staging site slot.</span></span> <span data-ttu-id="53416-355">続けて、web アプリの名前を持つ ***(ステージング)*** します。</span><span class="sxs-lookup"><span data-stu-id="53416-355">It has the name of your web app followed by ***(staging)***.</span></span> <span data-ttu-id="53416-356">管理ページに移動するステージング サイトをクリックします。</span><span class="sxs-lookup"><span data-stu-id="53416-356">Click the staging site to go to the management page.</span></span>

    <span data-ttu-id="53416-357">![ステージング web アプリに移動する](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "ステージング web アプリに移動します。")</span><span class="sxs-lookup"><span data-stu-id="53416-357">![Navigating to the staging web app](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "Navigating to the staging web app")</span></span>

    <span data-ttu-id="53416-358">*ステージングのアプリに移動します。*</span><span class="sxs-lookup"><span data-stu-id="53416-358">*Navigating to the staging app*</span></span>
7. <span data-ttu-id="53416-359">その彼の管理 ページの管理ページの他の web アプリのように注意してください。</span><span class="sxs-lookup"><span data-stu-id="53416-359">Notice that he management page looks like any other web app's management page.</span></span> <span data-ttu-id="53416-360">移動し、**展開**ページとコピー、 **Git URL**値。</span><span class="sxs-lookup"><span data-stu-id="53416-360">Navigate to the **Deployments** page and copy the **Git URL** value.</span></span> <span data-ttu-id="53416-361">この演習の後半で使用します。</span><span class="sxs-lookup"><span data-stu-id="53416-361">You will use it later in this exercise.</span></span>

    ![Git URL 値をコピー](maintainable-azure-websites-managing-change-and-scale/_static/image37.png)

    <span data-ttu-id="53416-363">*Git URL 値をコピー*</span><span class="sxs-lookup"><span data-stu-id="53416-363">*Copying the Git URL value*</span></span>
8. <span data-ttu-id="53416-364">新しく開きます**Git Bash**コンソールし、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="53416-364">Open a new **Git Bash** console and execute the following commands.</span></span> <span data-ttu-id="53416-365">更新プログラム、 *[YOUR APPLICATION PATH]* へのパスのプレース ホルダー、 **GeekQuiz**内にあるソリューション、 **Source\Ex1 DeployingWebSiteToStaging\Begin**のフォルダーこのラボでは。</span><span class="sxs-lookup"><span data-stu-id="53416-365">Update the *[YOUR-APPLICATION-PATH]* placeholder with the path to the **GeekQuiz** solution, located in the **Source\Ex1-DeployingWebSiteToStaging\Begin** folder of this lab.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample11.cmd)]

    ![Git の初期化と最初のコミット](maintainable-azure-websites-managing-change-and-scale/_static/image38.png)

    <span data-ttu-id="53416-367">*Git の初期化と最初のコミット*</span><span class="sxs-lookup"><span data-stu-id="53416-367">*Git initialization and first commit*</span></span>
9. <span data-ttu-id="53416-368">Web アプリをリモートにプッシュするには、次のコマンドを実行**Git**リポジトリ。</span><span class="sxs-lookup"><span data-stu-id="53416-368">Run the following command to push your web app to the remote **Git** repository.</span></span> <span data-ttu-id="53416-369">管理ポータルから取得した URL では、プレース ホルダーを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="53416-369">Replace the placeholder with the URL you obtained from the management portal.</span></span> <span data-ttu-id="53416-370">デプロイ パスワードを求めるメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="53416-370">You will be prompted for your deployment password.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample12.cmd)]

    ![Windows Azure へのプッシュ](maintainable-azure-websites-managing-change-and-scale/_static/image39.png)

    <span data-ttu-id="53416-372">*Azure へのプッシュ*</span><span class="sxs-lookup"><span data-stu-id="53416-372">*Pushing to Azure*</span></span>

    > [!NOTE]
    > <span data-ttu-id="53416-373">FTP ホストや GIT リポジトリの web アプリにコンテンツを展開する場合を使用して認証する必要があります、**デプロイ資格情報**から web アプリの作成した**クイック スタート**または**ダッシュ ボード**管理ページ。</span><span class="sxs-lookup"><span data-stu-id="53416-373">When you deploy content to the FTP host or GIT repository of a web app, you must authenticate using the **deployment credentials** that you created from the web app's **Quick Start** or **Dashboard** management pages.</span></span> <span data-ttu-id="53416-374">デプロイ資格情報がわからない場合、管理ポータルを使用して簡単にリセットできます。</span><span class="sxs-lookup"><span data-stu-id="53416-374">If you do not know your deployment credentials you can easily reset them using the management portal.</span></span> <span data-ttu-id="53416-375">Web アプリを開いて**ダッシュ ボード**ページ、をクリックし、**デプロイ資格情報をリセット**リンク。</span><span class="sxs-lookup"><span data-stu-id="53416-375">Open the web app **Dashboard** page and click the **Reset your deployment credentials** link.</span></span> <span data-ttu-id="53416-376">新しいパスワードを入力し、クリックして**OK**します。</span><span class="sxs-lookup"><span data-stu-id="53416-376">Provide a new password and click **OK**.</span></span> <span data-ttu-id="53416-377">デプロイ資格情報は、サブスクリプションに関連付けられているすべての web apps の使用に対して有効です。</span><span class="sxs-lookup"><span data-stu-id="53416-377">Deployment credentials are valid for use with all web apps associated with your subscription.</span></span>
10. <span data-ttu-id="53416-378">Web アプリは、Azure に正常にプッシュされたことを確認するために、管理ポータルに戻り をクリックして**Websites**します。</span><span class="sxs-lookup"><span data-stu-id="53416-378">In order to verify the web app was successfully pushed to Azure, go back to the management portal and click **Websites**.</span></span>
11. <span data-ttu-id="53416-379">Web アプリを検索し、ステージング サイト スロットを表示するエントリを展開します。</span><span class="sxs-lookup"><span data-stu-id="53416-379">Locate your web app and expand the entry to display the staging site slot.</span></span> <span data-ttu-id="53416-380">をクリックして、**名前**管理ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="53416-380">Click its **Name** to go to the management page.</span></span>
12. <span data-ttu-id="53416-381">クリックして**展開**を参照してください、**デプロイ履歴**します。</span><span class="sxs-lookup"><span data-stu-id="53416-381">Click **Deployments** to see the **deployment history**.</span></span> <span data-ttu-id="53416-382">あることを確認、**アクティブなデプロイ**で、 *&quot;初期コミット&quot;* します。</span><span class="sxs-lookup"><span data-stu-id="53416-382">Verify that there is an **Active Deployment** with your *&quot;Initial Commit&quot;*.</span></span>

    ![アクティブなデプロイ](maintainable-azure-websites-managing-change-and-scale/_static/image40.png)

    <span data-ttu-id="53416-384">*アクティブなデプロイ*</span><span class="sxs-lookup"><span data-stu-id="53416-384">*Active deployment*</span></span>
13. <span data-ttu-id="53416-385">最後に、クリックして**参照**web アプリに移動して、コマンド バーにします。</span><span class="sxs-lookup"><span data-stu-id="53416-385">Finally, click **Browse** in the command bar to go to the web app.</span></span>

    ![Web アプリを参照します。](maintainable-azure-websites-managing-change-and-scale/_static/image41.png)

    <span data-ttu-id="53416-387">*Web アプリを参照します。*</span><span class="sxs-lookup"><span data-stu-id="53416-387">*Browse web app*</span></span>
14. <span data-ttu-id="53416-388">アプリケーションが正常にデプロイされている場合は、ギーク Quiz ログイン ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="53416-388">If the application is successfully deployed, you will see the Geek Quiz login page.</span></span>

    > [!NOTE]
    > <span data-ttu-id="53416-389">デプロイされたアプリケーションのアドレスの URL に続けて、web アプリの名前が含まれています *-ステージング*します。</span><span class="sxs-lookup"><span data-stu-id="53416-389">The address URL of the deployed application contains the name of your web app followed by *-staging*.</span></span>

    ![ステージング環境で実行されているアプリケーション](maintainable-azure-websites-managing-change-and-scale/_static/image42.png)

    <span data-ttu-id="53416-391">*ステージング環境で実行されているアプリケーション*</span><span class="sxs-lookup"><span data-stu-id="53416-391">*Application running in the staging environment*</span></span>
15. <span data-ttu-id="53416-392">アプリケーションを探索する場合は、クリックして**登録**新しいユーザーを登録します。</span><span class="sxs-lookup"><span data-stu-id="53416-392">If you wish to explore the application, click **Register** to register a new user.</span></span> <span data-ttu-id="53416-393">ユーザー名、電子メール アドレスとパスワードを入力して、アカウントの詳細を完了します。</span><span class="sxs-lookup"><span data-stu-id="53416-393">Complete the account details by entering a user name, email address and password.</span></span> <span data-ttu-id="53416-394">次に、アプリケーションは、クイズの最初の質問を示します。</span><span class="sxs-lookup"><span data-stu-id="53416-394">Next, the application shows the first question of the quiz.</span></span> <span data-ttu-id="53416-395">想定どおりに動作を確認するいくつかの質問に回答します。</span><span class="sxs-lookup"><span data-stu-id="53416-395">Answer a few questions to make sure it is working as expected.</span></span>

    ![アプリケーションをすぐに使用できます。](maintainable-azure-websites-managing-change-and-scale/_static/image43.png)

    <span data-ttu-id="53416-397">*アプリケーションをすぐに使用できます。*</span><span class="sxs-lookup"><span data-stu-id="53416-397">*Application ready to be used*</span></span>

<a id="Ex2Task4"></a>
#### <a name="task-4--promoting-the-web-app-to-production"></a><span data-ttu-id="53416-398">タスク 4 – Web アプリを運用環境に昇格します。</span><span class="sxs-lookup"><span data-stu-id="53416-398">Task 4 – Promoting the Web App to Production</span></span>

<span data-ttu-id="53416-399">Web アプリがステージング環境で正しく動作していることを確認できた運用環境に昇格させることする準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="53416-399">Now that you have verified that the web app is working correctly in the staging environment, you are ready to promote it to production.</span></span> <span data-ttu-id="53416-400">このタスクでは、運用サイト スロットとステージング サイト スロットをスワップします。</span><span class="sxs-lookup"><span data-stu-id="53416-400">In this task, you will swap the staging site slot with the production site slot.</span></span>

1. <span data-ttu-id="53416-401">管理ポータルに戻るし、ステージング サイト スロットを選択します。</span><span class="sxs-lookup"><span data-stu-id="53416-401">Go back to the management portal and select the staging site slot.</span></span> <span data-ttu-id="53416-402">クリックして**スワップ**コマンド バーにします。</span><span class="sxs-lookup"><span data-stu-id="53416-402">Click **Swap** in the command bar.</span></span>

    ![運用環境にスワップします。](maintainable-azure-websites-managing-change-and-scale/_static/image44.png)

    <span data-ttu-id="53416-404">*運用環境にスワップします。*</span><span class="sxs-lookup"><span data-stu-id="53416-404">*Swap to production*</span></span>
2. <span data-ttu-id="53416-405">クリックして**はい**スワップ操作を続行する確認のダイアログ ボックスでします。</span><span class="sxs-lookup"><span data-stu-id="53416-405">Click **Yes** in the confirmation dialog box to proceed with the swap operation.</span></span> <span data-ttu-id="53416-406">Azure は、ステージング サイトのコンテンツを含む実稼働サイトのコンテンツをすぐにスワップされます。</span><span class="sxs-lookup"><span data-stu-id="53416-406">Azure will immediately swap the content of the production site with the content of the staging site.</span></span>

    > [!NOTE]
    > <span data-ttu-id="53416-407">(例: 接続文字列の上書き、ハンドラー マッピングなど) の production バージョンには、ステージングされたバージョンからのいくつかの設定が自動的にコピーされますが、(例: DNS エンドポイント、SSL のバインドなど)、その他の設定は変更されません。</span><span class="sxs-lookup"><span data-stu-id="53416-407">Some settings from the staged version will automatically be copied to the production version (e.g. connection string overrides, handler mappings, etc.) but other settings will not change (e.g. DNS endpoints, SSL bindings, etc.).</span></span>

    ![スワップ操作を確認します。](maintainable-azure-websites-managing-change-and-scale/_static/image45.png)

    <span data-ttu-id="53416-409">*スワップ操作を確認します。*</span><span class="sxs-lookup"><span data-stu-id="53416-409">*Confirming swap operation*</span></span>
3. <span data-ttu-id="53416-410">スワップが完了すると、運用スロットを選択し、をクリックして**参照**コマンド バーを実稼働サイトを開きます。</span><span class="sxs-lookup"><span data-stu-id="53416-410">Once the swap is complete, select the production slot and click **Browse** in the command bar to open the production site.</span></span> <span data-ttu-id="53416-411">アドレス バーの URL に注意してください。</span><span class="sxs-lookup"><span data-stu-id="53416-411">Notice the URL in the address bar.</span></span>

    > [!NOTE]
    > <span data-ttu-id="53416-412">キャッシュをクリアするブラウザーを更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="53416-412">You might need to refresh your browser to clear cache.</span></span> <span data-ttu-id="53416-413">Internet Explorer で、これを行うキーを押して**CTRL + R**します。</span><span class="sxs-lookup"><span data-stu-id="53416-413">In Internet Explorer, you can do this by pressing **CTRL+R**.</span></span>

    ![運用環境で実行されている web アプリ](maintainable-azure-websites-managing-change-and-scale/_static/image46.png)
4. <span data-ttu-id="53416-415">**GitBash**コンソールで、運用スロットを対象に、ローカルの Git リポジトリのリモート URL を更新します。</span><span class="sxs-lookup"><span data-stu-id="53416-415">In the **GitBash** console, update the remote URL for the local Git repository to target the production slot.</span></span> <span data-ttu-id="53416-416">これを行うには、プレース ホルダーをデプロイ ユーザー名と、web アプリの名前に置き換えて、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="53416-416">To do this, run the following command replacing the placeholders with your deployment username and the name of your web app.</span></span>

    > [!NOTE]
    > <span data-ttu-id="53416-417">次の演習では、ステージング ラボの簡潔さのためだけではなく、運用サイトへ変更をプッシュします。</span><span class="sxs-lookup"><span data-stu-id="53416-417">In the following exercises, you will push changes to the production site instead of staging just for the simplicity of the lab.</span></span> <span data-ttu-id="53416-418">実際のシナリオで、運用環境に昇格する前にステージング環境の変更を確認することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="53416-418">In a real-world scenario, it is recommended to verify the changes in the staging environment before promoting to production.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample13.cmd)]

<a id="Exercise3"></a>
### <a name="exercise-3-performing-deployment-rollback-in-production"></a><span data-ttu-id="53416-419">手順 3: 運用環境でデプロイのロールバックを実行します。</span><span class="sxs-lookup"><span data-stu-id="53416-419">Exercise 3: Performing Deployment Rollback in Production</span></span>

<span data-ttu-id="53416-420">場所がないステージングや運用などの間でのホット スワップを実行するステージング スロットを使用する場合のシナリオがある**Free**または**Shared**モード。</span><span class="sxs-lookup"><span data-stu-id="53416-420">There are scenarios where you do not have a staging slot to perform hot swap between staging and production, for example, if you are working with **Free** or **Shared** mode.</span></span> <span data-ttu-id="53416-421">そのようなシナリオでする必要がありますアプリケーションをテストするテスト環境で – ローカルまたはリモート サイトで – 運用環境にデプロイする前にします。</span><span class="sxs-lookup"><span data-stu-id="53416-421">In those scenarios, you should test your application in a testing environment –either locally or in a remote site– before deploying to production.</span></span> <span data-ttu-id="53416-422">ただし、実稼働サイトに、テスト フェーズ中に検出されない問題が発生することが可能です。</span><span class="sxs-lookup"><span data-stu-id="53416-422">However, it is possible that an issue not detected during the testing phase may arise in the production site.</span></span> <span data-ttu-id="53416-423">この場合、可能な限り早く前とより安定したバージョンのアプリケーションに切り替える簡単にするためのメカニズムを理解しておくことができます。</span><span class="sxs-lookup"><span data-stu-id="53416-423">In this case, it is important to have a mechanism to easily switch to a previous and more stable version of the application as quickly as possible.</span></span>

<span data-ttu-id="53416-424">**Azure App Service**、ソース管理からの継続的なデプロイは、考えられるこのおかげ、**再デプロイ**管理ポータルで使用できるアクション。</span><span class="sxs-lookup"><span data-stu-id="53416-424">In **Azure App Service**, continuous deployment from source control makes this possible thanks to the **redeploy** action available in the management portal.</span></span> <span data-ttu-id="53416-425">Azure では、リポジトリにプッシュされたコミットに関連付けられている展開を追跡し、いつでも、以前の展開のいずれかを使用してアプリケーションを再デプロイするためのオプションを提供します。</span><span class="sxs-lookup"><span data-stu-id="53416-425">Azure keeps track of the deployments associated with the commits pushed to the repository and provides an option to redeploy your application using any of your previous deployments, at any time.</span></span>

<span data-ttu-id="53416-426">この演習では、コードに変更を実行して、**ギーク Quiz**意図的に挿入するアプリケーション、*バグ*します。</span><span class="sxs-lookup"><span data-stu-id="53416-426">In this exercise you will perform a change to the code in the **Geek Quiz** application that intentionally injects a *bug*.</span></span> <span data-ttu-id="53416-427">ときに、運用環境にアプリケーションを展開するし、以前の状態に戻るに再デプロイ機能がかかります。</span><span class="sxs-lookup"><span data-stu-id="53416-427">You will deploy the application to production to see the error, and then you will take advantage of the redeploy feature to go back to the previous state.</span></span>

<a id="Ex3Task1"></a>
#### <a name="task-1--updating-the-geek-quiz-application"></a><span data-ttu-id="53416-428">タスク 1 – おたくクイズ アプリケーションの更新</span><span class="sxs-lookup"><span data-stu-id="53416-428">Task 1 – Updating the Geek Quiz Application</span></span>

<span data-ttu-id="53416-429">このタスクでは、小型のコードのリファクタリングが、 **TriviaController**クラスの新しいメソッドに、データベースから選択したクイズ オプションを取得するロジックの一部を抽出します。</span><span class="sxs-lookup"><span data-stu-id="53416-429">In this task, you will refactor a small piece of code of the **TriviaController** class to extract part of the logic that retrieves the selected quiz option from the database into a new method.</span></span>

1. <span data-ttu-id="53416-430">Visual Studio のインスタンスに切り替え、 **GeekQuiz**前の演習からソリューション。</span><span class="sxs-lookup"><span data-stu-id="53416-430">Switch to the Visual Studio instance with the **GeekQuiz** solution from the previous exercise.</span></span>
2. <span data-ttu-id="53416-431">**ソリューション エクスプ ローラー**、オープン、 **TriviaController.cs**ファイル内で、**コント ローラー**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="53416-431">In **Solution Explorer**, open the **TriviaController.cs** file inside the **Controllers** folder.</span></span>
3. <span data-ttu-id="53416-432">検索、 **StoreAsync**メソッドと、コードは次の図で強調表示を選択します。</span><span class="sxs-lookup"><span data-stu-id="53416-432">Locate the **StoreAsync** method and select the code highlighted in the following figure.</span></span>

    ![コードを選択します。](maintainable-azure-websites-managing-change-and-scale/_static/image47.png)

    <span data-ttu-id="53416-434">*コードを選択します。*</span><span class="sxs-lookup"><span data-stu-id="53416-434">*Selecting the code*</span></span>
4. <span data-ttu-id="53416-435">選択したコードを右クリックし、展開、**リファクタリング**メニュー選択し、**メソッドの抽出しています.**.</span><span class="sxs-lookup"><span data-stu-id="53416-435">Right-click the selected code, expand the **Refactor** menu and select **Extract Method...**.</span></span>

    ![新しいメソッドとして、コードを抽出](maintainable-azure-websites-managing-change-and-scale/_static/image48.png)

    <span data-ttu-id="53416-437">*メソッドの抽出を選択します。*</span><span class="sxs-lookup"><span data-stu-id="53416-437">*Selecting Extract Method*</span></span>
5. <span data-ttu-id="53416-438">**メソッドの抽出**ダイアログ ボックスで、新しいメソッドの名前*MatchesOption*  をクリック**OK**します。</span><span class="sxs-lookup"><span data-stu-id="53416-438">In the **Extract Method** dialog box, name the new method *MatchesOption* and click **OK**.</span></span>

    ![メソッド名を指定します。](maintainable-azure-websites-managing-change-and-scale/_static/image49.png)

    <span data-ttu-id="53416-440">*抽出されたメソッドの名前を指定します。*</span><span class="sxs-lookup"><span data-stu-id="53416-440">*Specifying the name for the extracted method*</span></span>
6. <span data-ttu-id="53416-441">選択したコードに抽出し、 **MatchesOption**メソッド。</span><span class="sxs-lookup"><span data-stu-id="53416-441">The selected code is then extracted into the **MatchesOption** method.</span></span> <span data-ttu-id="53416-442">結果として得られるコードは、次のスニペットに示します。</span><span class="sxs-lookup"><span data-stu-id="53416-442">The resulting code is shown in the following snippet.</span></span>

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample14.cs)]
7. <span data-ttu-id="53416-443">キーを押して**CTRL + S**変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="53416-443">Press **CTRL + S** to save the changes.</span></span>

<a id="Ex3Task2"></a>
#### <a name="task-2--redeploying-the-geek-quiz-application"></a><span data-ttu-id="53416-444">タスク 2 – おたくクイズのアプリケーションを再デプロイします。</span><span class="sxs-lookup"><span data-stu-id="53416-444">Task 2 – Redeploying the Geek Quiz Application</span></span>

<span data-ttu-id="53416-445">運用環境への新しい配置をトリガーすると、リポジトリに、前のタスクで加えた変更をプッシュします。</span><span class="sxs-lookup"><span data-stu-id="53416-445">You will now push the changes you made in the previous task to the repository, which will trigger a new deployment to the production environment.</span></span> <span data-ttu-id="53416-446">次を使用して、問題のトラブルシューティングは、 **F12 開発ツール**、Internet Explorer によって提供され、Azure 管理ポータルから、以前のデプロイにロールバックを実行します。</span><span class="sxs-lookup"><span data-stu-id="53416-446">Then, you will troubleshot an issue using the **F12 development tools** provided by Internet Explorer, and then perform a rollback to the previous deployment from the Azure management portal.</span></span>

1. <span data-ttu-id="53416-447">新しく開きます**Git Bash**更新されたアプリケーションを Azure App Service をデプロイするコンソール。</span><span class="sxs-lookup"><span data-stu-id="53416-447">Open a new **Git Bash** console to deploy the updated application to Azure App Service.</span></span>
2. <span data-ttu-id="53416-448">Azure に変更をプッシュするには、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="53416-448">Execute the following commands to push the changes to Azure.</span></span> <span data-ttu-id="53416-449">更新プログラム、 *[YOUR APPLICATION PATH]* へのパスのプレース ホルダー、 **GeekQuiz**ソリューション。</span><span class="sxs-lookup"><span data-stu-id="53416-449">Update the *[YOUR-APPLICATION-PATH]* placeholder with the path to the **GeekQuiz** solution.</span></span> <span data-ttu-id="53416-450">デプロイ パスワードを求めるメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="53416-450">You will be prompted for your deployment password.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample15.cmd)]

    ![リファクタリング後のコードを Azure にプッシュ](maintainable-azure-websites-managing-change-and-scale/_static/image50.png)

    <span data-ttu-id="53416-452">*リファクタリング後のコードを Azure にプッシュ*</span><span class="sxs-lookup"><span data-stu-id="53416-452">*Pushing refactored code to Azure*</span></span>
3. <span data-ttu-id="53416-453">Internet Explorer を開き、web アプリに移動します (例: `http://<your-web-site>.azurewebsites.net`)。</span><span class="sxs-lookup"><span data-stu-id="53416-453">Open Internet Explorer and navigate to your web app (e.g. `http://<your-web-site>.azurewebsites.net`).</span></span> <span data-ttu-id="53416-454">以前に作成した資格情報を使用してをログインします。</span><span class="sxs-lookup"><span data-stu-id="53416-454">Log in using the previously created credentials.</span></span>
4. <span data-ttu-id="53416-455">キーを押して**F12**開発ツールを起動するには、次のように選択します。、**ネットワーク** タブで  をクリックし、**再生**記録を開始するボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="53416-455">Press **F12** to launch the development tools, select the **Network** tab and click the **Play** button to start recording.</span></span>

    <span data-ttu-id="53416-456">![ネットワークの記録を開始](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "ネットワークの記録を開始")</span><span class="sxs-lookup"><span data-stu-id="53416-456">![Starting network recording](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "Starting network recording")</span></span>

    <span data-ttu-id="53416-457">*ネットワークの記録を開始*</span><span class="sxs-lookup"><span data-stu-id="53416-457">*Starting network recording*</span></span>
5. <span data-ttu-id="53416-458">クイズの任意のオプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="53416-458">Select any option of the quiz.</span></span> <span data-ttu-id="53416-459">何も起こらないことが表示されます。</span><span class="sxs-lookup"><span data-stu-id="53416-459">You will see that nothing happens.</span></span>
6. <span data-ttu-id="53416-460">**F12**ウィンドウで、POST HTTP 要求に対応するエントリを示しています、HTTP **500**結果。</span><span class="sxs-lookup"><span data-stu-id="53416-460">In the **F12** window, the entry corresponding to the POST HTTP request shows an HTTP **500** result.</span></span>

    ![HTTP 500 エラー](maintainable-azure-websites-managing-change-and-scale/_static/image52.png)

    <span data-ttu-id="53416-462">*HTTP 500 エラー*</span><span class="sxs-lookup"><span data-stu-id="53416-462">*HTTP 500 error*</span></span>
7. <span data-ttu-id="53416-463">選択、**コンソール**タブ。エラーが原因の詳細を記録します。</span><span class="sxs-lookup"><span data-stu-id="53416-463">Select the **Console** tab. An error is logged with the details of the cause.</span></span>

    ![ログに記録されたエラー](maintainable-azure-websites-managing-change-and-scale/_static/image53.png)

    <span data-ttu-id="53416-465">*ログに記録されたエラー*</span><span class="sxs-lookup"><span data-stu-id="53416-465">*Logged error*</span></span>
8. <span data-ttu-id="53416-466">エラーの詳細部分を見つけます。</span><span class="sxs-lookup"><span data-stu-id="53416-466">Locate the details part of the error.</span></span> <span data-ttu-id="53416-467">明らかに、リファクタリング前の手順でコミットするコードによってこのエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="53416-467">Clearly, this error is caused by the code refactoring you committed in the previous steps.</span></span>

    <span data-ttu-id="53416-468">`Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`。</span><span class="sxs-lookup"><span data-stu-id="53416-468">`Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`.</span></span>
9. <span data-ttu-id="53416-469">ブラウザーを閉じないでください。</span><span class="sxs-lookup"><span data-stu-id="53416-469">Do not close the browser.</span></span>
10. <span data-ttu-id="53416-470">新しいブラウザー インスタンスでは、に移動、 [Azure 管理ポータル](https://manage.windowsazure.com)サブスクリプションに関連付けられた Microsoft アカウントを使用してサインインします。</span><span class="sxs-lookup"><span data-stu-id="53416-470">In a new browser instance, navigate to the [Azure management portal](https://manage.windowsazure.com) and sign in using the Microsoft account associated with your subscription.</span></span>
11. <span data-ttu-id="53416-471">選択**Websites**手順 2 で作成した web アプリをクリックします。</span><span class="sxs-lookup"><span data-stu-id="53416-471">Select **Websites** and click the web app you created in Exercise 2.</span></span>
12. <span data-ttu-id="53416-472">移動し、**展開**ページ。</span><span class="sxs-lookup"><span data-stu-id="53416-472">Navigate to the **Deployments** page.</span></span> <span data-ttu-id="53416-473">デプロイ履歴で、実行されるすべてのコミットが表示されていることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="53416-473">Notice that all the commits performed are listed in the deployment history.</span></span>

    ![既存の展開の一覧](maintainable-azure-websites-managing-change-and-scale/_static/image54.png)

    <span data-ttu-id="53416-475">*既存の展開の一覧*</span><span class="sxs-lookup"><span data-stu-id="53416-475">*List of existing deployments*</span></span>
13. <span data-ttu-id="53416-476">以前のコミットを選択し、クリックして**再デプロイ**コマンド バーにします。</span><span class="sxs-lookup"><span data-stu-id="53416-476">Select the previous commit and click **Redeploy** on the command bar.</span></span>

    ![以前のコミットを再デプロイします。](maintainable-azure-websites-managing-change-and-scale/_static/image55.png)

    <span data-ttu-id="53416-478">*以前のコミットを再デプロイします。*</span><span class="sxs-lookup"><span data-stu-id="53416-478">*Redeploying the previous commit*</span></span>
14. <span data-ttu-id="53416-479">確認するメッセージが表示されたら、クリックして**はい**します。</span><span class="sxs-lookup"><span data-stu-id="53416-479">When prompted to confirm, click **Yes**.</span></span>

    ![再展開を確認します。](maintainable-azure-websites-managing-change-and-scale/_static/image56.png)
15. <span data-ttu-id="53416-481">デプロイが完了したらは、web アプリとキーを押して、ブラウザー インスタンスに切り替える**ctrl キーを押しながら f5 キーを押して**します。</span><span class="sxs-lookup"><span data-stu-id="53416-481">When the deployment completes, switch back to the browser instance with your web app and press **CTRL + F5**.</span></span>
16. <span data-ttu-id="53416-482">任意のオプションをクリックします。</span><span class="sxs-lookup"><span data-stu-id="53416-482">Click any of the options.</span></span> <span data-ttu-id="53416-483">フリップ アニメーション処理を実行し、結果 (*修正または正しくない*) が表示されます。</span><span class="sxs-lookup"><span data-stu-id="53416-483">The flip animation will now take place and the result (*correct/incorrect*) will be displayed.</span></span>
17. <span data-ttu-id="53416-484">(省略可能)切り替えて、 **Git Bash**コンソールし、以前のコミットに戻すには、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="53416-484">(Optional) Switch to the **Git Bash** console and execute the following commands to revert to the previous commit.</span></span>

    > [!NOTE]
    > <span data-ttu-id="53416-485">これらのコマンドは、Git リポジトリに正しくないコミットに加えられたすべての変更を元に戻すための新しいコミットを作成します。</span><span class="sxs-lookup"><span data-stu-id="53416-485">These commands create a new commit that undoes all changes in the Git repository that were made in the bad commit.</span></span> <span data-ttu-id="53416-486">Azure は、新しいコミットを使用して、アプリケーションとし、再デプロイします。</span><span class="sxs-lookup"><span data-stu-id="53416-486">Azure will then redeploy the application using the new commit.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample16.cmd)]

<a id="Exercise4"></a>
### <a name="exercise-4-scaling-using-azure-storage"></a><span data-ttu-id="53416-487">手順 4: スケーリング、Azure Storage の使用</span><span class="sxs-lookup"><span data-stu-id="53416-487">Exercise 4: Scaling Using Azure Storage</span></span>

<span data-ttu-id="53416-488">**Blob**は大量の非構造化テキストまたはバイナリ データなど、ビデオ、オーディオ、イメージを格納する最も簡単な方法です。</span><span class="sxs-lookup"><span data-stu-id="53416-488">**Blobs** are the simplest way to store large amounts of unstructured text or binary data such as video, audio and images.</span></span> <span data-ttu-id="53416-489">記憶域をアプリケーションの静的コンテンツを移動するは、画像またはドキュメントをブラウザーに直接配信することによって、アプリケーションをスケールするのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="53416-489">Moving the static content of your application to Storage, helps to scale your application by serving images or documents directly to the browser.</span></span>

<span data-ttu-id="53416-490">この演習では、Blob コンテナーに、アプリケーションの静的コンテンツを移動します。</span><span class="sxs-lookup"><span data-stu-id="53416-490">In this exercise, you will move the static content of your application to a Blob container.</span></span> <span data-ttu-id="53416-491">アプリケーションを追加するを構成し、 **ASP.NET URL の書き換え規則**で、 **Web.config**コンテンツを Blob コンテナーにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="53416-491">Then you will configure your application to add an **ASP.NET URL rewrite rule** in the **Web.config** to redirect your content to the Blob container.</span></span>

<a id="Ex4Task1"></a>
#### <a name="task-1--creating-an-azure-storage-account"></a><span data-ttu-id="53416-492">タスク 1 – Azure ストレージ アカウントの作成</span><span class="sxs-lookup"><span data-stu-id="53416-492">Task 1 – Creating an Azure Storage Account</span></span>

<span data-ttu-id="53416-493">このタスクでは、管理ポータルを使用して、新しいストレージ アカウントを作成する方法を学びます。</span><span class="sxs-lookup"><span data-stu-id="53416-493">In this task you will learn how to create a new storage account using the management portal.</span></span>

1. <span data-ttu-id="53416-494">移動し、 [Azure 管理ポータル](https://manage.windowsazure.com)サブスクリプションに関連付けられた Microsoft アカウントを使用してサインインします。</span><span class="sxs-lookup"><span data-stu-id="53416-494">Navigate to the [Azure management portal](https://manage.windowsazure.com) and sign in using the Microsoft account associated with your subscription.</span></span>
2. <span data-ttu-id="53416-495">選択**新しい |Data Services |記憶域 |簡易作成**新しいストレージ アカウントの作成を開始します。</span><span class="sxs-lookup"><span data-stu-id="53416-495">Select **New | Data Services | Storage | Quick Create** to start creating a new storage account.</span></span> <span data-ttu-id="53416-496">クリックし、アカウントの一意の名前を入力、**リージョン**一覧から。</span><span class="sxs-lookup"><span data-stu-id="53416-496">Enter a unique name for the account and select a **Region** from the list.</span></span> <span data-ttu-id="53416-497">クリックして**ストレージ アカウントの作成**を続行します。</span><span class="sxs-lookup"><span data-stu-id="53416-497">Click **Create Storage Account** to continue.</span></span>

    <span data-ttu-id="53416-498">![新しいストレージ アカウントを作成する](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "新しいストレージ アカウントの作成")</span><span class="sxs-lookup"><span data-stu-id="53416-498">![Creating a new Storage Account](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "Creating a new Storage Account")</span></span>

    <span data-ttu-id="53416-499">*新しいストレージ アカウントの作成*</span><span class="sxs-lookup"><span data-stu-id="53416-499">*Creating a new storage account*</span></span>
3. <span data-ttu-id="53416-500">**ストレージ**セクションで、新しいストレージ アカウントの状態に変わるまで待機*オンライン*次の手順を続行するためです。</span><span class="sxs-lookup"><span data-stu-id="53416-500">In the **Storage** section, wait until the status of the new storage account changes to *Online* in order to continue with the following step.</span></span>

    <span data-ttu-id="53416-501">![作成したストレージ アカウント](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "ストレージ アカウントの作成")</span><span class="sxs-lookup"><span data-stu-id="53416-501">![Storage Account created](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "Storage Account created")</span></span>

    <span data-ttu-id="53416-502">*ストレージ アカウントの作成*</span><span class="sxs-lookup"><span data-stu-id="53416-502">*Storage Account created*</span></span>
4. <span data-ttu-id="53416-503">ストレージ アカウントの名前をクリックし、**ダッシュ ボード**ページの上部にあるリンクです。</span><span class="sxs-lookup"><span data-stu-id="53416-503">Click on the storage account name and then click the **Dashboard** link at the top of the page.</span></span> <span data-ttu-id="53416-504">**ダッシュ ボード**ページによって、アカウントと、アプリケーション内で使用できるサービス エンドポイントの状態に関する情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="53416-504">The **Dashboard** page provides you with information about the status of the account and the service endpoints that can be used within your applications.</span></span>

    <span data-ttu-id="53416-505">![ストレージ アカウント ダッシュ ボードを表示する](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "ストレージ アカウント ダッシュ ボードを表示します。")</span><span class="sxs-lookup"><span data-stu-id="53416-505">![Displaying the Storage Account Dashboard](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "Displaying the Storage Account Dashboard")</span></span>

    <span data-ttu-id="53416-506">*ストレージ アカウント ダッシュ ボードを表示します。*</span><span class="sxs-lookup"><span data-stu-id="53416-506">*Displaying the Storage Account Dashboard*</span></span>
5. <span data-ttu-id="53416-507">をクリックして、**アクセス キーの管理**ナビゲーション バーのボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="53416-507">Click the **Manage Access Keys** button in the navigation bar.</span></span>

    <span data-ttu-id="53416-508">![アクセス キーの管理 ボタン](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "アクセス キーの管理 ボタン")</span><span class="sxs-lookup"><span data-stu-id="53416-508">![Manage Access Keys button](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "Manage Access Keys button")</span></span>

    <span data-ttu-id="53416-509">*[アクセス キー] ボタンを管理します。*</span><span class="sxs-lookup"><span data-stu-id="53416-509">*Manage Access Keys button*</span></span>
6. <span data-ttu-id="53416-510">**アクセス キーの管理**ダイアログ ボックスで、コピー、**ストレージ アカウント名**と**プライマリ アクセス キー**に次の手順で必要になります。</span><span class="sxs-lookup"><span data-stu-id="53416-510">In the **Manage Access Keys** dialog box, copy the **Storage Account Name** and **Primary Access Key** as you will need them in the following exercise.</span></span> <span data-ttu-id="53416-511">次に、ダイアログ ボックスを閉じます。</span><span class="sxs-lookup"><span data-stu-id="53416-511">Then, close the dialog box.</span></span>

    <span data-ttu-id="53416-512">![アクセス キーの管理 ダイアログ ボックス](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "アクセス キーの管理 ダイアログ ボックス")</span><span class="sxs-lookup"><span data-stu-id="53416-512">![Manage Access Key dialog box](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "Manage Access Key dialog box")</span></span>

    <span data-ttu-id="53416-513">*管理アクセス キー ダイアログ ボックス*</span><span class="sxs-lookup"><span data-stu-id="53416-513">*Manage Access Key dialog box*</span></span>

<a id="Ex4Task2"></a>
#### <a name="task-2--uploading-an-asset-to-azure-blob-storage"></a><span data-ttu-id="53416-514">タスク 2 – Azure Blob Storage への資産のアップロード</span><span class="sxs-lookup"><span data-stu-id="53416-514">Task 2 – Uploading an Asset to Azure Blob Storage</span></span>

<span data-ttu-id="53416-515">このタスクでは、ストレージ アカウントに接続する Visual Studio から、サーバー エクスプ ローラー ウィンドウを使用します。</span><span class="sxs-lookup"><span data-stu-id="53416-515">In this task, you will use the Server Explorer window from Visual Studio to connect to your storage account.</span></span> <span data-ttu-id="53416-516">Blob コンテナーを作成し、ギーク Quiz ロゴのファイルをコンテナーにアップロードされます。</span><span class="sxs-lookup"><span data-stu-id="53416-516">You will then create a blob container and upload a file with the Geek Quiz logo to the container.</span></span>

1. <span data-ttu-id="53416-517">Visual Studio のインスタンスに切り替え、 **GeekQuiz**前の演習からソリューション。</span><span class="sxs-lookup"><span data-stu-id="53416-517">Switch to the Visual Studio instance with the **GeekQuiz** solution from the previous exercise.</span></span>
2. <span data-ttu-id="53416-518">メニュー バーから選択**ビュー**  をクリックし、**サーバー エクスプ ローラー**します。</span><span class="sxs-lookup"><span data-stu-id="53416-518">From the menu bar, select **View** and then click **Server Explorer**.</span></span>
3. <span data-ttu-id="53416-519">**サーバー エクスプ ローラー**を右クリックし、 **Azure**ノード**を Azure に接続しています.**.サブスクリプションに関連付けられている Microsoft アカウントを使用してサインインします。</span><span class="sxs-lookup"><span data-stu-id="53416-519">In **Server Explorer**, right-click the **Azure** node and select **Connect to Azure...**. Sign in using the Microsoft account associated with your subscription.</span></span>

    ![Windows Azure への接続します。](maintainable-azure-websites-managing-change-and-scale/_static/image62.png)

    <span data-ttu-id="53416-521">*Azure への接続します。*</span><span class="sxs-lookup"><span data-stu-id="53416-521">*Connect to Azure*</span></span>
4. <span data-ttu-id="53416-522">展開、 **Azure**ノードを右クリックし**ストレージ**選択と**外部ストレージのアタッチしています.**.</span><span class="sxs-lookup"><span data-stu-id="53416-522">Expand the **Azure** node, right-click **Storage** and select **Attach External Storage...**.</span></span>
5. <span data-ttu-id="53416-523">**新しいストレージ アカウントの追加** ダイアログ ボックスに、入力、**アカウント名**と**アカウント キー** 、前のタスクとクリックで取得した**OK**.</span><span class="sxs-lookup"><span data-stu-id="53416-523">In the **Add New Storage Account** dialog box, enter the **Account name** and **Account key** you obtained in the previous task and click **OK**.</span></span>

    ![新しいストレージ アカウント ダイアログ ボックスを追加します。](maintainable-azure-websites-managing-change-and-scale/_static/image63.png)

    <span data-ttu-id="53416-525">*新しいストレージ アカウント ダイアログ ボックスを追加します。*</span><span class="sxs-lookup"><span data-stu-id="53416-525">*Add New Storage Account dialog box*</span></span>
6. <span data-ttu-id="53416-526">ストレージ アカウントは、下に表示する必要があります、**ストレージ**ノード。</span><span class="sxs-lookup"><span data-stu-id="53416-526">Your storage account should appear under the **Storage** node.</span></span> <span data-ttu-id="53416-527">ストレージ アカウントを展開し、右クリックして**Blob**選択**Blob コンテナーの作成.**.</span><span class="sxs-lookup"><span data-stu-id="53416-527">Expand your storage account, right-click **Blobs** and select **Create Blob Container...**.</span></span>

    <span data-ttu-id="53416-528">![Blob コンテナーを作成する](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "Blob コンテナーを作成します。")</span><span class="sxs-lookup"><span data-stu-id="53416-528">![Create Blob Container](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "Create Blob Container")</span></span>

    <span data-ttu-id="53416-529">*Blob コンテナーを作成します。*</span><span class="sxs-lookup"><span data-stu-id="53416-529">*Create Blob Container*</span></span>
7. <span data-ttu-id="53416-530">**Blob コンテナーの作成** ダイアログ ボックスで、blob コンテナーの名前を入力し、をクリックして**OK**します。</span><span class="sxs-lookup"><span data-stu-id="53416-530">In the **Create Blob Container** dialog box, enter a name for the blob container and click **OK**.</span></span>

    <span data-ttu-id="53416-531">![Blob コンテナーの作成 ダイアログ ボックス](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "Blob コンテナーの作成 ダイアログ ボックス")</span><span class="sxs-lookup"><span data-stu-id="53416-531">![Create Blob Container dialog box](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "Create Blob Container dialog box")</span></span>

    <span data-ttu-id="53416-532">*Blob コンテナー ダイアログ ボックスを作成します。*</span><span class="sxs-lookup"><span data-stu-id="53416-532">*Create Blob Container dialog box*</span></span>
8. <span data-ttu-id="53416-533">新しい blob コンテナーに追加する必要があります、 **Blob**ノード。</span><span class="sxs-lookup"><span data-stu-id="53416-533">The new blob container should be added to the **Blobs** node.</span></span> <span data-ttu-id="53416-534">コンテナーを公開して、コンテナーのアクセス許可を変更します。</span><span class="sxs-lookup"><span data-stu-id="53416-534">Change the access permissions in the container to make the container public.</span></span> <span data-ttu-id="53416-535">これを行うを右クリックし、**イメージ**コンテナーと選択**プロパティ**します。</span><span class="sxs-lookup"><span data-stu-id="53416-535">To do this, right-click the **images** container and select **Properties**.</span></span>

    <span data-ttu-id="53416-536">![コンテナーのプロパティをイメージ](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "イメージのコンテナーのプロパティ")</span><span class="sxs-lookup"><span data-stu-id="53416-536">![images container properties](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "images container properties")</span></span>

    <span data-ttu-id="53416-537">*コンテナー イメージのプロパティ*</span><span class="sxs-lookup"><span data-stu-id="53416-537">*Images container properties*</span></span>
9. <span data-ttu-id="53416-538">**プロパティ**ウィンドウで、設定、**パブリック読み取りアクセス**に**コンテナー**します。</span><span class="sxs-lookup"><span data-stu-id="53416-538">In the **Properties** window, set the **Public Read Access** to **Container**.</span></span>

    <span data-ttu-id="53416-539">![パブリック読み取りアクセスのプロパティを変更する](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "パブリック読み取りアクセスのプロパティを変更します。")</span><span class="sxs-lookup"><span data-stu-id="53416-539">![Changing public read access property](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "Changing public read access property")</span></span>

    <span data-ttu-id="53416-540">*パブリック読み取りアクセスのプロパティを変更します。*</span><span class="sxs-lookup"><span data-stu-id="53416-540">*Changing public read access property*</span></span>
10. <span data-ttu-id="53416-541">パブリック アクセス プロパティを変更するには、をクリックする場合は、確認を求められたら**はい**します。</span><span class="sxs-lookup"><span data-stu-id="53416-541">When prompted if you are sure you want to change the public access property, click **Yes**.</span></span>

    <span data-ttu-id="53416-542">![Microsoft Visual Studio 警告](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "Microsoft Visual Studio 警告")</span><span class="sxs-lookup"><span data-stu-id="53416-542">![Microsoft Visual Studio warning](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "Microsoft Visual Studio warning")</span></span>

    <span data-ttu-id="53416-543">*Microsoft Visual Studio 警告*</span><span class="sxs-lookup"><span data-stu-id="53416-543">*Microsoft Visual Studio warning*</span></span>
11. <span data-ttu-id="53416-544">**サーバー エクスプ ローラー**でを右クリックし、**イメージ**blob コンテナーと選択**Blob コンテナーの表示**します。</span><span class="sxs-lookup"><span data-stu-id="53416-544">In **Server Explorer**, right-click in the **images** blob container and select **View Blob Container**.</span></span>

    <span data-ttu-id="53416-545">![Blob コンテナーを表示](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "Blob コンテナーの表示")</span><span class="sxs-lookup"><span data-stu-id="53416-545">![View Blob Container](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "View Blob Container")</span></span>

    <span data-ttu-id="53416-546">*Blob コンテナーの表示*</span><span class="sxs-lookup"><span data-stu-id="53416-546">*View Blob Container*</span></span>
12. <span data-ttu-id="53416-547">イメージ コンテナーが新しいウィンドウで開く必要があり、エントリがないの凡例を表示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="53416-547">The images container should open in a new window and a legend with no entries should be shown.</span></span> <span data-ttu-id="53416-548">をクリックして、**アップロード**アイコン ファイルを blob コンテナーにアップロードします。</span><span class="sxs-lookup"><span data-stu-id="53416-548">Click the **upload** icon to upload a file to the blob container.</span></span>

    <span data-ttu-id="53416-549">![エントリがないイメージ コンテナー](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "エントリがないイメージ コンテナー")</span><span class="sxs-lookup"><span data-stu-id="53416-549">![Images container with no entries](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "Images container with no entries")</span></span>

    <span data-ttu-id="53416-550">*エントリがないイメージ コンテナー*</span><span class="sxs-lookup"><span data-stu-id="53416-550">*Images container with no entries*</span></span>
13. <span data-ttu-id="53416-551">**Blob のアップロード** ダイアログ ボックスに移動、**資産**ラボのフォルダー。</span><span class="sxs-lookup"><span data-stu-id="53416-551">In the **Upload Blob** dialog box, navigate to the **Assets** folder of the lab.</span></span> <span data-ttu-id="53416-552">選択、**ロゴ big.png**ファイルし、クリックして**オープン**。</span><span class="sxs-lookup"><span data-stu-id="53416-552">Select the **logo-big.png** file and click **Open**.</span></span>
14. <span data-ttu-id="53416-553">ファイルがアップロードされるまで待ちます。</span><span class="sxs-lookup"><span data-stu-id="53416-553">Wait until the file is uploaded.</span></span> <span data-ttu-id="53416-554">アップロードが完了したら、ファイルがイメージのコンテナーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="53416-554">When the upload completes, the file should be listed in the images container.</span></span> <span data-ttu-id="53416-555">ファイルのエントリを右クリックして**URL のコピー**します。</span><span class="sxs-lookup"><span data-stu-id="53416-555">Right-click the file entry and select **Copy URL**.</span></span>

    <span data-ttu-id="53416-556">![Blob の URL をコピー](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "blob ファイルの URL をコピー")</span><span class="sxs-lookup"><span data-stu-id="53416-556">![Copy blob URL](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "Copy blob file URL")</span></span>

    <span data-ttu-id="53416-557">*Blob の URL をコピーします。*</span><span class="sxs-lookup"><span data-stu-id="53416-557">*Copy blob URL*</span></span>
15. <span data-ttu-id="53416-558">Internet Explorer を開き、URL を貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="53416-558">Open Internet Explorer and paste the URL.</span></span> <span data-ttu-id="53416-559">次の図は、ブラウザーに表示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="53416-559">The following image should be shown in the browser.</span></span>

    <span data-ttu-id="53416-560">![Windows の Blob ストレージからイメージをロゴ big.png](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "ストレージからロゴ big.png イメージ")</span><span class="sxs-lookup"><span data-stu-id="53416-560">![logo-big.png image from Windows Blob Storage](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "logo-big.png image from storage")</span></span>

    <span data-ttu-id="53416-561">*Azure Blob Storage からロゴ big.png イメージ*</span><span class="sxs-lookup"><span data-stu-id="53416-561">*logo-big.png image from Azure Blob Storage*</span></span>

<a id="Ex4Task3"></a>
#### <a name="task-3--updating-the-solution-to-consume-static-content-from-azure-blob-storage"></a><span data-ttu-id="53416-562">タスク 3 – Azure Blob Storage から静的コンテンツを使用するソリューションを更新しています</span><span class="sxs-lookup"><span data-stu-id="53416-562">Task 3 – Updating the Solution to Consume Static Content from Azure Blob Storage</span></span>

<span data-ttu-id="53416-563">このタスクでは、構成、 **GeekQuiz**ソリューション、イメージを使用する Azure Blob Storage にアップロード (web アプリにあるイメージ) ではなくで ASP.NET URL 書き換え規則を追加することで、 **web.config**ファイル。</span><span class="sxs-lookup"><span data-stu-id="53416-563">In this task, you will configure the **GeekQuiz** solution to consume the image uploaded to Azure Blob Storage (instead of the image located in the web app) by adding an ASP.NET URL rewrite rule in the **web.config** file.</span></span>

1. <span data-ttu-id="53416-564">Visual Studio で開く、 **Web.config**ファイル内で、 **GeekQuiz**プロジェクトし、検索、 **&lt;system.webServer&gt;** 要素。</span><span class="sxs-lookup"><span data-stu-id="53416-564">In Visual Studio, open the **Web.config** file inside the **GeekQuiz** project and locate the **&lt;system.webServer&gt;** element.</span></span>
2. <span data-ttu-id="53416-565">URL 書き換え規則が、プレース ホルダーをストレージ アカウント名で更新を追加するには、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="53416-565">Add the following code to add an URL rewrite rule, updating the placeholder with your storage account name.</span></span>

    <span data-ttu-id="53416-566">(コード スニペット - *WebSitesInProduction - Ex4 - UrlRewriteRule*)</span><span class="sxs-lookup"><span data-stu-id="53416-566">(Code Snippet - *WebSitesInProduction - Ex4 - UrlRewriteRule*)</span></span>

    [!code-xml[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample17.xml)]

    > [!NOTE]
    > <span data-ttu-id="53416-567">URL リライトは、受信 Web 要求をインターセプトし、別のリソースへの要求のリダイレクトのプロセスです。</span><span class="sxs-lookup"><span data-stu-id="53416-567">URL rewriting is the process of intercepting an incoming Web request and redirecting the request to a different resource.</span></span> <span data-ttu-id="53416-568">URL 書き換えルールでは、書き換えエンジンを要求をリダイレクトする必要がある場合と、リダイレクト先にするように指示します。</span><span class="sxs-lookup"><span data-stu-id="53416-568">The URL rewriting rules tells the rewriting engine when a request needs to be redirected, and where should they be redirected.</span></span> <span data-ttu-id="53416-569">書き換えルールは 2 つの文字列で構成されます: 要求された URL で検索するパターン (通常は、正規表現を使用して) 場合に使用すると、パターンを置換する文字列が見つかったとします。</span><span class="sxs-lookup"><span data-stu-id="53416-569">A rewriting rule is composed of two strings: the pattern to look for in the requested URL (usually, using regular expressions), and the string to replace the pattern with, if found.</span></span> <span data-ttu-id="53416-570">詳細については、次を参照してください。 [ASP.NET における URL リライト](https://msdn.microsoft.com/library/ms972974.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="53416-570">For more information, see [URL Rewriting in ASP.NET](https://msdn.microsoft.com/library/ms972974.aspx).</span></span>
3. <span data-ttu-id="53416-571">キーを押して**CTRL + S**変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="53416-571">Press **CTRL + S** to save the changes.</span></span>
4. <span data-ttu-id="53416-572">新しく開きます**Git Bash**更新されたアプリケーションを Azure App Service をデプロイするコンソール。</span><span class="sxs-lookup"><span data-stu-id="53416-572">Open a new **Git Bash** console to deploy the updated application to Azure App Service.</span></span>
5. <span data-ttu-id="53416-573">Azure に変更をプッシュするには、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="53416-573">Execute the following commands to push the changes to Azure.</span></span> <span data-ttu-id="53416-574">更新プログラム、 *[YOUR APPLICATION PATH]* へのパスのプレース ホルダー、 **GeekQuiz**ソリューション。</span><span class="sxs-lookup"><span data-stu-id="53416-574">Update the *[YOUR-APPLICATION-PATH]* placeholder with the path to the **GeekQuiz** solution.</span></span> <span data-ttu-id="53416-575">デプロイ パスワードを求めるメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="53416-575">You will be prompted for your deployment password.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample18.cmd)]

    ![Azure への更新プログラムの展開](maintainable-azure-websites-managing-change-and-scale/_static/image73.png)

    <span data-ttu-id="53416-577">*Azure への更新プログラムの展開*</span><span class="sxs-lookup"><span data-stu-id="53416-577">*Deploying update to Azure*</span></span>

<a id="Ex4Task4"></a>
#### <a name="task-4--verification"></a><span data-ttu-id="53416-578">タスク 4 – 検証</span><span class="sxs-lookup"><span data-stu-id="53416-578">Task 4 – Verification</span></span>

<span data-ttu-id="53416-579">このタスクでは、使用**Internet Explorer**を参照する、**ギーク Quiz**でホストされているイメージにアプリケーションと、URL の書き換え規則がのイメージが機能し、チェックをリダイレクト**Azure Blob記憶域**します。</span><span class="sxs-lookup"><span data-stu-id="53416-579">In this task you will use **Internet Explorer** to browse the **Geek Quiz** application and check that the URL rewrite rule for images works and you are redirected to the image hosted on **Azure Blob Storage**.</span></span>

1. <span data-ttu-id="53416-580">Internet Explorer を開き、web アプリに移動します (例: `http://<your-web-site>.azurewebsites.net`)。</span><span class="sxs-lookup"><span data-stu-id="53416-580">Open Internet Explorer and navigate to your web app (e.g. `http://<your-web-site>.azurewebsites.net`).</span></span> <span data-ttu-id="53416-581">以前に作成した資格情報を使用してをログインします。</span><span class="sxs-lookup"><span data-stu-id="53416-581">Log in using the previously created credentials.</span></span>

    <span data-ttu-id="53416-582">![イメージとギーク Quiz web アプリを表示](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "イメージ ギーク Quiz web アプリを表示")</span><span class="sxs-lookup"><span data-stu-id="53416-582">![Showing the Geek Quiz web app with the image](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "Showing the Geek Quiz web app with the image")</span></span>

    <span data-ttu-id="53416-583">*イメージとギーク Quiz web アプリを表示*</span><span class="sxs-lookup"><span data-stu-id="53416-583">*Showing the Geek Quiz web app with the image*</span></span>
2. <span data-ttu-id="53416-584">キーを押して**F12**開発ツールを起動するには、選択、**ネットワーク**タブし、記録を開始します。</span><span class="sxs-lookup"><span data-stu-id="53416-584">Press **F12** to launch the development tools, select the **Network** tab and start recording.</span></span>

    <span data-ttu-id="53416-585">![ネットワークの記録を開始](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "ネットワークの記録を開始")</span><span class="sxs-lookup"><span data-stu-id="53416-585">![Starting network recording](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "Starting network recording")</span></span>

    <span data-ttu-id="53416-586">*ネットワークの記録を開始*</span><span class="sxs-lookup"><span data-stu-id="53416-586">*Starting network recording*</span></span>
3. <span data-ttu-id="53416-587">キーを押して**ctrl キーを押しながら f5 キーを押して**web ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="53416-587">Press **CTRL + F5** to refresh the web page.</span></span>
4. <span data-ttu-id="53416-588">HTTP 要求が表示、ページの読み込みが完了すると、 **/img/logo-big.png** http URL **301**結果 (リダイレクト) と別の要求を`http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png`URL と HTTP **200**結果。</span><span class="sxs-lookup"><span data-stu-id="53416-588">Once the page has finished loading, you should see an HTTP request for the **/img/logo-big.png** URL with an HTTP **301** result (redirect) and another request for `http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png` URL with a HTTP **200** result.</span></span>

    <span data-ttu-id="53416-589">![URL リダイレクトの確認](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "開発ツールでリダイレクトを示す")</span><span class="sxs-lookup"><span data-stu-id="53416-589">![Verifying the URL redirect](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "Showing the redirect in Dev Tools")</span></span>

    <span data-ttu-id="53416-590">*URL リダイレクトの確認*</span><span class="sxs-lookup"><span data-stu-id="53416-590">*Verifying the URL redirect*</span></span>

<a id="Exercise5"></a>
### <a name="exercise-5-using-autoscale-for-web-apps"></a><span data-ttu-id="53416-591">手順 5: Web アプリの自動スケールの使用</span><span class="sxs-lookup"><span data-stu-id="53416-591">Exercise 5: Using Autoscale for Web Apps</span></span>

> [!NOTE]
> <span data-ttu-id="53416-592">Web ロードのサポートを必要があるために、この手順は省略可能で&amp;パフォーマンス テストで使用可能なだけ**Visual Studio 2013 Ultimate Edition**します。</span><span class="sxs-lookup"><span data-stu-id="53416-592">This exercise is optional, since it requires support for Web Load &amp; Performance Testing which is only available for **Visual Studio 2013 Ultimate Edition**.</span></span> <span data-ttu-id="53416-593">Visual Studio 2013 の特定の機能の詳細については、のバージョンを比較[ここ](https://www.microsoft.com/visualstudio/eng/products/compare)します。</span><span class="sxs-lookup"><span data-stu-id="53416-593">For more information on specific Visual Studio 2013 features, compare versions [here](https://www.microsoft.com/visualstudio/eng/products/compare).</span></span>


<span data-ttu-id="53416-594">**Azure App Service Web Apps**実行されている web アプリの自動スケール機能を提供して**標準モード**します。</span><span class="sxs-lookup"><span data-stu-id="53416-594">**Azure App Service Web Apps** provides the Autoscale feature for web apps running in **Standard Mode**.</span></span> <span data-ttu-id="53416-595">自動スケールは、Azure の負荷に応じて、web アプリのインスタンス数の自動スケールを使用できます。</span><span class="sxs-lookup"><span data-stu-id="53416-595">Autoscale lets Azure automatically scale the instance count of your web app depending on the load.</span></span> <span data-ttu-id="53416-596">自動スケールを有効にすると、Azure は、web アプリの CPU が 5 分ごとに 1 回チェックし、その時点で必要に応じてインスタンスが追加されます。</span><span class="sxs-lookup"><span data-stu-id="53416-596">When Autoscale is enabled, Azure checks the CPU of your web app once every five minutes and adds instances as needed at that point in time.</span></span> <span data-ttu-id="53416-597">CPU 使用率が低い場合インスタンスが削除されます 2 時間おきに web アプリのパフォーマンスが低下しないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="53416-597">If the CPU usage is low, Azure will remove instances once every two hours to ensure that the performance of your web app is not degraded.</span></span>

<span data-ttu-id="53416-598">この演習では構成に必要な手順を参照してください、**自動スケール**の特徴として、**ギーク Quiz** web アプリ。</span><span class="sxs-lookup"><span data-stu-id="53416-598">In this exercise you will go through the steps required to configure the **Autoscale** feature for the **Geek Quiz** web app.</span></span> <span data-ttu-id="53416-599">インスタンスのアップグレードをトリガーするアプリケーションで十分な CPU の負荷を生成する Visual Studio ロード テストを実行して、この機能を確認します。</span><span class="sxs-lookup"><span data-stu-id="53416-599">You will verify this feature by running a Visual Studio load test to generate enough CPU load on the application to trigger an instance upgrade.</span></span>

<a id="Ex5Task1"></a>
#### <a name="task-1--configuring-autoscale-based-on-the-cpu-metric"></a><span data-ttu-id="53416-600">タスク 1 –、CPU のメトリックに基づく自動スケールを構成します。</span><span class="sxs-lookup"><span data-stu-id="53416-600">Task 1 – Configuring Autoscale Based on the CPU Metric</span></span>

<span data-ttu-id="53416-601">このタスクでは、手順 2 で作成した web アプリの自動スケール機能を有効にするのに、Azure 管理ポータルを使用します。</span><span class="sxs-lookup"><span data-stu-id="53416-601">In this task you will use the Azure management portal to enable the Autoscale feature for the web app you created in Exercise 2.</span></span>

1. <span data-ttu-id="53416-602">[Azure 管理ポータル](https://manage.windowsazure.com/)を選択します**Websites**手順 2 で作成した web アプリをクリックします。</span><span class="sxs-lookup"><span data-stu-id="53416-602">In the [Azure management portal](https://manage.windowsazure.com/), select **Websites** and click the web app you created in Exercise 2.</span></span>
2. <span data-ttu-id="53416-603">移動し、**スケール**ページ。</span><span class="sxs-lookup"><span data-stu-id="53416-603">Navigate to the **Scale** page.</span></span> <span data-ttu-id="53416-604">で、**容量**セクションで、 **CPU**の**メトリックによるスケール**構成します。</span><span class="sxs-lookup"><span data-stu-id="53416-604">Under the **capacity** section, select **CPU** for the **Scale by Metric** configuration.</span></span>

    > [!NOTE]
    > <span data-ttu-id="53416-605">CPU によってスケーリング、Azure は、CPU 使用率が変更された場合に、アプリが使用するインスタンスの数を動的に調整されます。</span><span class="sxs-lookup"><span data-stu-id="53416-605">When scaling by CPU, Azure dynamically adjusts the number of instances that the app uses if the CPU usage changes.</span></span>

    <span data-ttu-id="53416-606">![CPU によるスケールを選択する](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "自動スケーリングの CPU のメトリックを選択します。")</span><span class="sxs-lookup"><span data-stu-id="53416-606">![Selecting to scale by CPU](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "Selecting the CPU metric for auto scaling")</span></span>

    <span data-ttu-id="53416-607">*CPU によるスケールを選択します。*</span><span class="sxs-lookup"><span data-stu-id="53416-607">*Selecting to scale by CPU*</span></span>
3. <span data-ttu-id="53416-608">変更、**ターゲット CPU**構成**20**-**40** %。</span><span class="sxs-lookup"><span data-stu-id="53416-608">Change the **Target CPU** configuration to **20**-**40** percent.</span></span>

    > [!NOTE]
    > <span data-ttu-id="53416-609">この範囲は、web アプリの平均 CPU 使用率を表します。</span><span class="sxs-lookup"><span data-stu-id="53416-609">This range represents the average CPU usage for your web app.</span></span> <span data-ttu-id="53416-610">Azure では、追加または、この範囲内で web アプリを保持するインスタンスを削除します。</span><span class="sxs-lookup"><span data-stu-id="53416-610">Azure will add or remove instances to keep your web app in this range.</span></span> <span data-ttu-id="53416-611">拡張するために使用されるインスタンスの最小値と最大数がで指定された、**インスタンス数**構成します。</span><span class="sxs-lookup"><span data-stu-id="53416-611">The minimum and maximum number of instances used for scaling is specified in the **Instance Count** configuration.</span></span> <span data-ttu-id="53416-612">上またはその制限を超える azure になることはありません。</span><span class="sxs-lookup"><span data-stu-id="53416-612">Azure will never go above or beyond that limit.</span></span>
    > 
    > <span data-ttu-id="53416-613">既定の**ターゲット CPU**値は、このラボの目的でのみ変更されます。</span><span class="sxs-lookup"><span data-stu-id="53416-613">The default **Target CPU** values are modified just for the purposes of this lab.</span></span> <span data-ttu-id="53416-614">中程度の負荷をアプリケーションに配置すると、CPU の範囲を小さい値に構成すると、自動スケールをトリガーする機会は増加します。</span><span class="sxs-lookup"><span data-stu-id="53416-614">By configuring the CPU range with small values, you are increasing the chances to trigger Autoscale when a moderate load is placed on the application.</span></span>

    <span data-ttu-id="53416-615">![CPU の 20 ~ 40% がターゲットの変更](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "に 20 ~ 40% に CPU ターゲットの変更")</span><span class="sxs-lookup"><span data-stu-id="53416-615">![Changing the target CPU to be between 20 and 40 percent](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "Changing the target CPU to be between 20 and 40 percent")</span></span>

    <span data-ttu-id="53416-616">*ターゲット CPU の 20 ~ 40% に変更します。*</span><span class="sxs-lookup"><span data-stu-id="53416-616">*Changing the Target CPU to be between 20 and 40 percent*</span></span>
4. <span data-ttu-id="53416-617">クリックして**保存**変更を保存するコマンド バーにします。</span><span class="sxs-lookup"><span data-stu-id="53416-617">Click **Save** in the command bar to save the changes.</span></span>

<a id="Ex5Task2"></a>
#### <a name="task-2--load-testing-with-visual-studio"></a><span data-ttu-id="53416-618">タスク 2 – Visual Studio によるロード テスト</span><span class="sxs-lookup"><span data-stu-id="53416-618">Task 2 – Load Testing with Visual Studio</span></span>

<span data-ttu-id="53416-619">できた**自動スケール**された作成は、構成されている、 **Web パフォーマンスとロード テストのプロジェクト**で Visual Studio で web アプリに対する CPU 負荷を生成します。</span><span class="sxs-lookup"><span data-stu-id="53416-619">Now that **Autoscale** has been configured, you will create a **Web Performance and Load Test Project** in Visual Studio to generate some CPU load on your web app.</span></span>

1. <span data-ttu-id="53416-620">開いている**Visual Studio Ultimate 2013**選択**ファイル |新しい |プロジェクトの追加.** 新しいソリューションを開始します。</span><span class="sxs-lookup"><span data-stu-id="53416-620">Open **Visual Studio Ultimate 2013** and select **File | New | Project...** to start a new solution.</span></span>

    <span data-ttu-id="53416-621">![新しいプロジェクトを作成する](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "新しいプロジェクトを作成します。")</span><span class="sxs-lookup"><span data-stu-id="53416-621">![Creating a new project](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "Creating a new project")</span></span>

    <span data-ttu-id="53416-622">*新しいプロジェクトを作成します。*</span><span class="sxs-lookup"><span data-stu-id="53416-622">*Creating a new project*</span></span>
2. <span data-ttu-id="53416-623">**新しいプロジェクト**ダイアログ ボックスで、 **Web パフォーマンスとロード テストのプロジェクト**下、 **(Visual C#) |テスト**タブ。確認します **.NET Framework 4.5**がプロジェクトに名前を選択すると、 *WebAndLoadTestProject*、選択、**場所** をクリック**OK**。</span><span class="sxs-lookup"><span data-stu-id="53416-623">In the **New Project** dialog box, select **Web Performance and Load Test Project** under the **Visual C# | Test** tab. Make sure **.NET Framework 4.5** is selected, name the project *WebAndLoadTestProject*, choose a **Location** and click **OK**.</span></span>

    <span data-ttu-id="53416-624">![新しい Web およびロード テスト プロジェクトを作成する](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "Web およびロード テスト プロジェクトを新規作成")</span><span class="sxs-lookup"><span data-stu-id="53416-624">![Creating a new Web and Load Test project](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "Creating a new Web and Load Test project")</span></span>

    <span data-ttu-id="53416-625">*新しい Web およびロード テスト プロジェクトを作成します。*</span><span class="sxs-lookup"><span data-stu-id="53416-625">*Creating a new Web and Load Test project*</span></span>
3. <span data-ttu-id="53416-626">**WebTest1.webtest**を右クリックし、 **WebTest1**ノードをクリックします**要求の追加**します。</span><span class="sxs-lookup"><span data-stu-id="53416-626">In the **WebTest1.webtest** Right-click the **WebTest1** node and click **Add Request**.</span></span>

    <span data-ttu-id="53416-627">![[Webtest1] への要求の追加](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "[webtest1] への要求の追加")</span><span class="sxs-lookup"><span data-stu-id="53416-627">![Adding a request to WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "Adding a request to WebTest1")</span></span>

    <span data-ttu-id="53416-628">*[Webtest1] への要求の追加*</span><span class="sxs-lookup"><span data-stu-id="53416-628">*Adding a request to WebTest1*</span></span>
4. <span data-ttu-id="53416-629">**プロパティ**ウィンドウ、新しい要求のノードの更新、 **Url** web アプリの URL を指すプロパティ (例: *[ http://geek-quiz.azurewebsites.net/ ](http://geek-quiz.azurewebsites.net/)*).</span><span class="sxs-lookup"><span data-stu-id="53416-629">In the **Properties** window of the new request node, update the **Url** property to point to the URL of your web app (e.g. *[http://geek-quiz.azurewebsites.net/](http://geek-quiz.azurewebsites.net/)*).</span></span>

    <span data-ttu-id="53416-630">![Url プロパティを変更する](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "Url プロパティを変更します。")</span><span class="sxs-lookup"><span data-stu-id="53416-630">![Changing the Url property](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "Changing the Url property")</span></span>

    <span data-ttu-id="53416-631">*Url プロパティを変更します。*</span><span class="sxs-lookup"><span data-stu-id="53416-631">*Changing the Url property*</span></span>
5. <span data-ttu-id="53416-632">**WebTest1.webtest**ウィンドウで、右クリック**WebTest1**  をクリック**ループの追加.**.</span><span class="sxs-lookup"><span data-stu-id="53416-632">In the **WebTest1.webtest** window, right-click **WebTest1** and click **Add Loop...**.</span></span>

    <span data-ttu-id="53416-633">![[Webtest1] へのループの追加](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "WebTest1 にループの追加")</span><span class="sxs-lookup"><span data-stu-id="53416-633">![Adding a loop to WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "Adding a loop to WebTest1")</span></span>

    <span data-ttu-id="53416-634">*[Webtest1] へのループの追加*</span><span class="sxs-lookup"><span data-stu-id="53416-634">*Adding a loop to WebTest1*</span></span>
6. <span data-ttu-id="53416-635">**条件付き規則の追加と項目をループ**ダイアログ ボックスで、 **For ループ**ルールし、次のプロパティを変更します。</span><span class="sxs-lookup"><span data-stu-id="53416-635">In the **Add Conditional Rule and Items to Loop** dialog box, select the **For Loop** rule and modify the following properties.</span></span>

   1. <span data-ttu-id="53416-636">**終了値:** 1000</span><span class="sxs-lookup"><span data-stu-id="53416-636">**Terminating value:** 1000</span></span>
   2. <span data-ttu-id="53416-637">**コンテキスト パラメーター名:** 反復子</span><span class="sxs-lookup"><span data-stu-id="53416-637">**Context Parameter Name:** Iterator</span></span>
   3. <span data-ttu-id="53416-638">**増分の値:** 1</span><span class="sxs-lookup"><span data-stu-id="53416-638">**Increment Value:** 1</span></span>

      <span data-ttu-id="53416-639">![For ループのルールを選択し、プロパティを更新](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "For ループのルールを選択し、プロパティの更新")</span><span class="sxs-lookup"><span data-stu-id="53416-639">![Selecting the For Loop rule and updating the properties](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "Selecting the For Loop rule and updating the properties")</span></span>

      <span data-ttu-id="53416-640">*For ループのルールを選択し、プロパティの更新*</span><span class="sxs-lookup"><span data-stu-id="53416-640">*Selecting the For Loop rule and updating the properties*</span></span>
7. <span data-ttu-id="53416-641">で、**ループ内の項目**セクションで、ループの最初と最後の項目にする前に作成した要求を選択します。</span><span class="sxs-lookup"><span data-stu-id="53416-641">Under the **Items in loop** section, select the request you created previously to be the first and last item for the loop.</span></span> <span data-ttu-id="53416-642">**[OK]** をクリックして続行します。</span><span class="sxs-lookup"><span data-stu-id="53416-642">Click **OK** to continue.</span></span>

    <span data-ttu-id="53416-643">![ループの最初と最後の項目を選択する](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "ループの最初と最後の項目を選択します。")</span><span class="sxs-lookup"><span data-stu-id="53416-643">![Selecting the first and last items for the loop](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "Selecting the first and last items for the loop")</span></span>

    <span data-ttu-id="53416-644">*ループの最初と最後の項目を選択します。*</span><span class="sxs-lookup"><span data-stu-id="53416-644">*Selecting the first and last items for the loop*</span></span>
8. <span data-ttu-id="53416-645">**ソリューション エクスプ ローラー**を右クリックし、 **WebAndLoadTestProject**プロジェクトで、展開、**追加**メニュー選択し、**ロード テストしています.**.</span><span class="sxs-lookup"><span data-stu-id="53416-645">In **Solution Explorer**, right-click the **WebAndLoadTestProject** project, expand the **Add** menu and select **Load Test...**.</span></span>

    <span data-ttu-id="53416-646">![WebAndLoadTestProject プロジェクトへのロード テストの追加](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "WebAndLoadTestProject プロジェクトへのロード テストの追加")</span><span class="sxs-lookup"><span data-stu-id="53416-646">![Adding a Load Test to the WebAndLoadTestProject project](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "Adding a Load Test to the WebAndLoadTestProject project")</span></span>

    <span data-ttu-id="53416-647">*WebAndLoadTestProject プロジェクトへのロード テストの追加*</span><span class="sxs-lookup"><span data-stu-id="53416-647">*Adding a Load Test to the WebAndLoadTestProject project*</span></span>
9. <span data-ttu-id="53416-648">**新しいロード テスト ウィザード**ダイアログ ボックスで、をクリックして**次**します。</span><span class="sxs-lookup"><span data-stu-id="53416-648">In the **New Load Test Wizard** dialog box, click **Next**.</span></span>

    <span data-ttu-id="53416-649">![新しいロード テスト ウィザード](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "新しいロード テスト ウィザード")</span><span class="sxs-lookup"><span data-stu-id="53416-649">![New Load Test Wizard](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "New Load Test Wizard")</span></span>

    <span data-ttu-id="53416-650">*新しいロード テスト ウィザード*</span><span class="sxs-lookup"><span data-stu-id="53416-650">*New Load Test Wizard*</span></span>
10. <span data-ttu-id="53416-651">**シナリオ** ページで、**待ち時間を使用しないでください** をクリック**次**します。</span><span class="sxs-lookup"><span data-stu-id="53416-651">In the **Scenario** page, select **Do not use think times** and click **Next**.</span></span>

    <span data-ttu-id="53416-652">![待ち時間を使用しないように選択](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "待ち時間を使用しないように選択します。")</span><span class="sxs-lookup"><span data-stu-id="53416-652">![Selecting not to use think times](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "Selecting not to use think times")</span></span>

    <span data-ttu-id="53416-653">*待ち時間を使用しないように選択します。*</span><span class="sxs-lookup"><span data-stu-id="53416-653">*Selecting not to use think times*</span></span>
11. <span data-ttu-id="53416-654">**ロード パターン**ことを確認します ページで、**持続ロード**オプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="53416-654">In the **Load Pattern** page, make sure that the **Constant Load** option is selected.</span></span> <span data-ttu-id="53416-655">変更、**ユーザー カウント**に設定**250**ユーザーとクリック **[次へ]** します。</span><span class="sxs-lookup"><span data-stu-id="53416-655">Change the **User Count** setting to **250** users and click **Next**.</span></span>

    <span data-ttu-id="53416-656">![ユーザー カウントを 250 に変更する](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "を 250 ユーザー数を変更します。")</span><span class="sxs-lookup"><span data-stu-id="53416-656">![Changing the user count to 250](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "Changing the user count to 250")</span></span>

    <span data-ttu-id="53416-657">*ユーザー カウントを 250 に変更します。*</span><span class="sxs-lookup"><span data-stu-id="53416-657">*Changing the user count to 250*</span></span>
12. <span data-ttu-id="53416-658">**テスト ミックス モデル** ページで、**時系列順に基づいて** をクリック**次**します。</span><span class="sxs-lookup"><span data-stu-id="53416-658">In the **Test Mix Model** page, select **Based on sequential test order** and click **Next**.</span></span>

    <span data-ttu-id="53416-659">![テスト ミックス モデルを選択する](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "テスト ミックス モデルの選択")</span><span class="sxs-lookup"><span data-stu-id="53416-659">![Selecting the test mix model](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "Selecting the test mix model")</span></span>

    <span data-ttu-id="53416-660">*テスト ミックス モデルの選択*</span><span class="sxs-lookup"><span data-stu-id="53416-660">*Selecting the test mix model*</span></span>
13. <span data-ttu-id="53416-661">**テスト ミックス モデル**] ページで [**追加しています.** テストをミックスに追加します。</span><span class="sxs-lookup"><span data-stu-id="53416-661">In the **Test Mix Model** page, click **Add...** to add a test to the mix.</span></span>

    <span data-ttu-id="53416-662">![テスト ミックスにテストを追加する](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "テスト ミックスにテストを追加します。")</span><span class="sxs-lookup"><span data-stu-id="53416-662">![Adding a test to the test mix](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "Adding a test to the test mix")</span></span>

    <span data-ttu-id="53416-663">*テスト ミックスにテストを追加します。*</span><span class="sxs-lookup"><span data-stu-id="53416-663">*Adding a test to the test mix*</span></span>
14. <span data-ttu-id="53416-664">**テストの追加**ダイアログ ボックスをダブルクリック**WebTest1**にテストを追加する、**選択されたテスト**一覧。</span><span class="sxs-lookup"><span data-stu-id="53416-664">In the **Add Tests** dialog box, double-click **WebTest1** to add the test to the **Selected tests** list.</span></span> <span data-ttu-id="53416-665">**[OK]** をクリックして続行します。</span><span class="sxs-lookup"><span data-stu-id="53416-665">Click **OK** to continue.</span></span>

    <span data-ttu-id="53416-666">![[Webtest1] テストを追加する](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "[webtest1] テストを追加します。")</span><span class="sxs-lookup"><span data-stu-id="53416-666">![Adding the WebTest1 test](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "Adding the WebTest1 test")</span></span>

    <span data-ttu-id="53416-667">*[Webtest1] テストを追加します。*</span><span class="sxs-lookup"><span data-stu-id="53416-667">*Adding the WebTest1 test*</span></span>
15. <span data-ttu-id="53416-668">戻り、**テスト ミックス**] ページで [**次**します。</span><span class="sxs-lookup"><span data-stu-id="53416-668">Back in the **Test Mix** page, click **Next**.</span></span>

    <span data-ttu-id="53416-669">![テスト ミックスのページを完了](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "テスト ミックスのページの完了")</span><span class="sxs-lookup"><span data-stu-id="53416-669">![Completing the Test Mix page](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "Completing the Test Mix page")</span></span>

    <span data-ttu-id="53416-670">*テスト ミックスのページの完了*</span><span class="sxs-lookup"><span data-stu-id="53416-670">*Completing the Test Mix page*</span></span>
16. <span data-ttu-id="53416-671">**ネットワーク ミックス**] ページで [**次**します。</span><span class="sxs-lookup"><span data-stu-id="53416-671">In the **Network Mix** page, click **Next**.</span></span>

    <span data-ttu-id="53416-672">![クリックして、[ネットワーク ミックス] ページで次に](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "ネットワーク ミックス ページで次をクリックすると")</span><span class="sxs-lookup"><span data-stu-id="53416-672">![Clicking next in the Network Mix page](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "Clicking next in the Network Mix page")</span></span>

    <span data-ttu-id="53416-673">*ネットワーク ミックス ページで 次へ をクリックします。*</span><span class="sxs-lookup"><span data-stu-id="53416-673">*Clicking next in the Network Mix page*</span></span>
17. <span data-ttu-id="53416-674">**ブラウザー ミックス**] ページで、[ **Internet Explorer 10.0**ブラウザーの種類をクリックします**次**します。</span><span class="sxs-lookup"><span data-stu-id="53416-674">In the **Browser Mix** page, select **Internet Explorer 10.0** as the browser type and click **Next**.</span></span>

    <span data-ttu-id="53416-675">![ブラウザーの種類を選択する](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "ブラウザーの種類を選択します。")</span><span class="sxs-lookup"><span data-stu-id="53416-675">![Selecting the browser type](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "Selecting the browser type")</span></span>

    <span data-ttu-id="53416-676">*ブラウザーの種類を選択します。*</span><span class="sxs-lookup"><span data-stu-id="53416-676">*Selecting the browser type*</span></span>
18. <span data-ttu-id="53416-677">**カウンター セット** ページで **次**します。</span><span class="sxs-lookup"><span data-stu-id="53416-677">In the **Counter Sets** page, click **Next**.</span></span>

    <span data-ttu-id="53416-678">![カウンター セット ページで 次へ をクリックすると](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "カウンター セット ページでは、次をクリックすると")</span><span class="sxs-lookup"><span data-stu-id="53416-678">![Clicking Next in the Counter Sets page](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "Clicking Next in the Counter Sets page")</span></span>

    <span data-ttu-id="53416-679">*カウンター セット ページで 次へ をクリックして*</span><span class="sxs-lookup"><span data-stu-id="53416-679">*Clicking Next in the Counter Sets page*</span></span>
19. <span data-ttu-id="53416-680">**実行設定** ページで、設定、**ロード テストの継続時間**に**5 分** をクリック**完了**します。</span><span class="sxs-lookup"><span data-stu-id="53416-680">In the **Run Settings** page, set the **Load test duration** to **5 minutes** and click **Finish**.</span></span>

    <span data-ttu-id="53416-681">![ロード テストの継続時間を 5 分に設定](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "ロード テストの継続時間を 5 分に設定します。")</span><span class="sxs-lookup"><span data-stu-id="53416-681">![Setting the load test duration to 5 minutes](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "Setting the load test duration to 5 minutes")</span></span>

    <span data-ttu-id="53416-682">*ロード テストの継続時間を 5 分に設定します。*</span><span class="sxs-lookup"><span data-stu-id="53416-682">*Setting the load test duration to 5 minutes*</span></span>
20. <span data-ttu-id="53416-683">**ソリューション エクスプ ローラー**、ダブルクリックして、 **Local.settings**テストの設定を表示するファイル。</span><span class="sxs-lookup"><span data-stu-id="53416-683">In **Solution Explorer**, double-click the **Local.settings** file to explore the test settings.</span></span> <span data-ttu-id="53416-684">既定では、Visual Studio は、テストを実行するのにローカル コンピューターを使用します。</span><span class="sxs-lookup"><span data-stu-id="53416-684">By default, Visual Studio uses your local computer to run the tests.</span></span>

    > [!NOTE]
    > <span data-ttu-id="53416-685">使用して、クラウドでロード テストを実行するテスト プロジェクトを構成する代わりに、 **Visual Studio Online (VSO)** します。</span><span class="sxs-lookup"><span data-stu-id="53416-685">Alternatively, you can configure your test project to run the load tests in the cloud using **Visual Studio Online (VSO)**.</span></span> <span data-ttu-id="53416-686">VSO では、クラウド ベースのロード テスト サービスをより現実的な負荷をシミュレートする、CPU 容量、使用可能なメモリ、ネットワーク帯域幅などのローカル環境の制約を回避を提供します。</span><span class="sxs-lookup"><span data-stu-id="53416-686">VSO provides a cloud-based load testing service that simulates a more realistic load, avoiding local environment constraints like CPU capacity, available memory and network bandwidth.</span></span> <span data-ttu-id="53416-687">VSO を使用してロード テストを実行する方法の詳細については、次を参照してください。[今回](https://www.visualstudio.com/get-started/load-test-your-app-vs)します。</span><span class="sxs-lookup"><span data-stu-id="53416-687">For more information about using VSO to run load tests, see [this article](https://www.visualstudio.com/get-started/load-test-your-app-vs).</span></span>

    ![テストの設定](maintainable-azure-websites-managing-change-and-scale/_static/image98.png)

<a id="Ex5Task3"></a>
#### <a name="task-3--autoscale-verification"></a><span data-ttu-id="53416-689">タスク 3 – 自動スケール検証</span><span class="sxs-lookup"><span data-stu-id="53416-689">Task 3 – Autoscale Verification</span></span>

<span data-ttu-id="53416-690">前のタスクで作成したロード テストを実行するようになりましたし負荷の下で web アプリの動作を参照してください。</span><span class="sxs-lookup"><span data-stu-id="53416-690">You will now execute the load test you created in the previous task and see how your web app behaves under load.</span></span>

1. <span data-ttu-id="53416-691">**ソリューション エクスプ ローラー**、ダブルクリックして**LoadTest1.loadtest**をロード テストを開きます。</span><span class="sxs-lookup"><span data-stu-id="53416-691">In **Solution Explorer**, double-click **LoadTest1.loadtest** to open the load test.</span></span>

    <span data-ttu-id="53416-692">![LoadTest1.loadtest を開く](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "LoadTest1.loadtest を開く")</span><span class="sxs-lookup"><span data-stu-id="53416-692">![Opening LoadTest1.loadtest](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "Opening LoadTest1.loadtest")</span></span>

    <span data-ttu-id="53416-693">*LoadTest1.loadtest を開く*</span><span class="sxs-lookup"><span data-stu-id="53416-693">*Opening LoadTest1.loadtest*</span></span>
2. <span data-ttu-id="53416-694">**LoadTest1.loadtest**ウィンドウで、ロード テストを実行するツールボックス内の最初のボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="53416-694">In the **LoadTest1.loadtest** window, click the first button in the toolbox to run the load test.</span></span>

    <span data-ttu-id="53416-695">![ロード テストを実行している](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "ロード テストの実行")</span><span class="sxs-lookup"><span data-stu-id="53416-695">![Running the load test](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "Running the load test")</span></span>

    <span data-ttu-id="53416-696">*ロード テストの実行*</span><span class="sxs-lookup"><span data-stu-id="53416-696">*Running the load test*</span></span>
3. <span data-ttu-id="53416-697">ロード テストが完了するまで待機します。</span><span class="sxs-lookup"><span data-stu-id="53416-697">Wait until the load test completes.</span></span>

    > [!NOTE]
    > <span data-ttu-id="53416-698">ロード テストでは、web アプリを同時に要求を送信する複数のユーザーをシミュレートします。</span><span class="sxs-lookup"><span data-stu-id="53416-698">The load test simulates multiple users that send requests to the web app simultaneously.</span></span> <span data-ttu-id="53416-699">テストが実行されている場合は、すべてのエラー、警告、またはロード テストの実行に関連するその他の情報を検出するために使用可能なカウンターを監視できます。</span><span class="sxs-lookup"><span data-stu-id="53416-699">When the test is running, you can monitor the available counters to detect any errors, warnings or other information related to your load test run.</span></span>

    <span data-ttu-id="53416-700">![ロード テストを実行している](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "ロード テストが完了するまで待機しています")</span><span class="sxs-lookup"><span data-stu-id="53416-700">![Load test running](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "Waiting until the load test completes")</span></span>

    <span data-ttu-id="53416-701">*ロード テストの実行*</span><span class="sxs-lookup"><span data-stu-id="53416-701">*Load test running*</span></span>
4. <span data-ttu-id="53416-702">テストが完了すると、管理ポータルに戻るしに移動します、**スケール**web アプリのページ。</span><span class="sxs-lookup"><span data-stu-id="53416-702">Once the test completes, go back to the management portal and navigate to the **Scale** page of your web app.</span></span> <span data-ttu-id="53416-703">で、**容量**セクションに表示されます、グラフの新しいインスタンスが自動的にデプロイされたことです。</span><span class="sxs-lookup"><span data-stu-id="53416-703">Under the **capacity** section, you should see in the graph that a new instance was automatically deployed.</span></span>

    ![新しいインスタンスを自動的にデプロイ](maintainable-azure-websites-managing-change-and-scale/_static/image102.png)

    <span data-ttu-id="53416-705">*新しいインスタンスを自動的にデプロイ*</span><span class="sxs-lookup"><span data-stu-id="53416-705">*New instance automatically deployed*</span></span>

    > [!NOTE]
    > <span data-ttu-id="53416-706">変更、グラフに表示されるまで数分かかる場合があります (キーを押して**ctrl キーを押しながら f5 キーを押して**ページを更新するには、定期的に)。</span><span class="sxs-lookup"><span data-stu-id="53416-706">It may take several minutes for the changes to appear in the graph (press **CTRL + F5** periodically to refresh the page).</span></span> <span data-ttu-id="53416-707">すべての変更が表示されない場合、次の操作を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="53416-707">If you do not see any changes, you can try the following:</span></span>
    > 
    > - <span data-ttu-id="53416-708">ロード テストの期間は長くなります (例: に**10 分**)</span><span class="sxs-lookup"><span data-stu-id="53416-708">Increase the duration of the load test (e.g. to **10 minutes**)</span></span>
    > - <span data-ttu-id="53416-709">最大値と最小値を減らして、**ターゲット CPU**範囲は、web アプリの自動スケールの構成</span><span class="sxs-lookup"><span data-stu-id="53416-709">Reduce the maximum and minimum values of the **Target CPU** range in the Autoscale configuration of your web app</span></span>
    > - <span data-ttu-id="53416-710">使用してクラウドでロード テストの実行**Visual Studio Online**します。</span><span class="sxs-lookup"><span data-stu-id="53416-710">Run the load test in the cloud with **Visual Studio Online**.</span></span> <span data-ttu-id="53416-711">詳細については[ここ](https://www.visualstudio.com/get-started/load-test-your-app-vs.aspx)</span><span class="sxs-lookup"><span data-stu-id="53416-711">More information [here](https://www.visualstudio.com/get-started/load-test-your-app-vs.aspx)</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="53416-712">まとめ</span><span class="sxs-lookup"><span data-stu-id="53416-712">Summary</span></span>

<span data-ttu-id="53416-713">このハンズオン ラボを設定して、Azure で web アプリを運用環境にアプリケーションを配置する方法について説明しました。</span><span class="sxs-lookup"><span data-stu-id="53416-713">In this hands-on lab, you learned how to set up and deploy your application to production web apps in Azure.</span></span> <span data-ttu-id="53416-714">使用してデータベースの更新を検出して開始した**Entity Framework Code First Migrations**、しを使用して、サイトの新しいバージョンを展開することで続き**Git**へのロールバックを実行して、サイトの最新の安定バージョン。</span><span class="sxs-lookup"><span data-stu-id="53416-714">You started by detecting and updating your databases using **Entity Framework Code First Migrations**, then continued by deploying new versions of your site using **Git** and performing rollbacks to the latest stable version of your site.</span></span> <span data-ttu-id="53416-715">さらに、記憶域を使用して静的コンテンツを Blob コンテナーに移動するアプリをスケールする方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="53416-715">Additionally, you learned how to scale your app using Storage to move your static content to a Blob container.</span></span>
