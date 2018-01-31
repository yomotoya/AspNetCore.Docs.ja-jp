---
uid: aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
title: "ハンズオン ラボ: Azure の web サイトの保守が容易な: 変更とスケールを管理する |Microsoft ドキュメント"
author: rick-anderson
description: "Microsoft Azure 簡単をビルドし、web サイトを実稼働環境にデプロイできます。 アプリケーションが完了していません、だけを開始します。 あなたが。。。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: ecfd0eb4-c4ad-44e6-9db9-a2a66611ff6a
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
msc.type: authoredcontent
ms.openlocfilehash: 3d24c633368abc14efcd9fcf200a4d05c5b182c9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="hands-on-lab-maintainable-azure-websites-managing-change-and-scale"></a><span data-ttu-id="22e83-105">ハンズオン ラボ: Azure の web サイトの保守が容易な: 変更とスケールを管理します。</span><span class="sxs-lookup"><span data-stu-id="22e83-105">Hands on Lab: Maintainable Azure Websites: Managing Change and Scale</span></span>
====================
<span data-ttu-id="22e83-106">によって[Web キャンプ チーム](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="22e83-106">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="22e83-107">Web キャンプ トレーニング キットをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="22e83-107">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="22e83-108">Microsoft Azure 簡単をビルドし、web サイトを実稼働環境にデプロイできます。</span><span class="sxs-lookup"><span data-stu-id="22e83-108">Microsoft Azure makes it easy to build and deploy websites to production.</span></span> <span data-ttu-id="22e83-109">アプリケーションが完了していません、だけを開始します。</span><span class="sxs-lookup"><span data-stu-id="22e83-109">But you're not done when your application is live, you're just getting started!</span></span> <span data-ttu-id="22e83-110">変化する要件、データベースの更新、スケール、および詳細を処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="22e83-110">You'll need to handle changing requirements, database updates, scale, and more.</span></span> <span data-ttu-id="22e83-111">さいわい、Azure App Service には、サイトの円滑な動作を確保するための機能の多くでは、カバーします。</span><span class="sxs-lookup"><span data-stu-id="22e83-111">Fortunately, Azure App Service has you covered, with plenty of features to help you keep your sites running smoothly.</span></span>
> 
> <span data-ttu-id="22e83-112">Azure では、セキュリティで保護された柔軟な開発、展開、およびスケーリングの任意のサイズの web アプリケーションのオプションを提供します。</span><span class="sxs-lookup"><span data-stu-id="22e83-112">Azure offers secure and flexible development, deployment and scaling options for any size web application.</span></span> <span data-ttu-id="22e83-113">インフラストラクチャを管理する煩わしさのないアプリケーションの展開を作成し、既存のツールを活用します。</span><span class="sxs-lookup"><span data-stu-id="22e83-113">Leverage your existing tools to create and deploy applications without the hassle of managing infrastructure.</span></span>
> 
> <span data-ttu-id="22e83-114">実稼働 web アプリケーションをプロビジョニング自分で分単位で、使い慣れた開発ツールを使用して作成されたコンテンツを簡単に展開しています。</span><span class="sxs-lookup"><span data-stu-id="22e83-114">Provision a production web application yourself in minutes by easily deploying content created using your favorite development tool.</span></span> <span data-ttu-id="22e83-115">ソース管理のサポートから直接既存のサイトを展開する**Git**、 **GitHub**、 **Bitbucket**、 **TFS**、およびでも**DropBox**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-115">You can deploy an existing site directly from source control with support for **Git**, **GitHub**, **Bitbucket**, **TFS**, and even **DropBox**.</span></span> <span data-ttu-id="22e83-116">好きな IDE から直接、またはを使用してスクリプトから展開**PowerShell** windows または**CLI**すべての OS で実行されるツールです。</span><span class="sxs-lookup"><span data-stu-id="22e83-116">Deploy directly from your favorite IDE or from scripts using **PowerShell** in Windows or **CLI** tools running on any OS.</span></span> <span data-ttu-id="22e83-117">展開した後は、サイトを常に最新の状態の継続的な展開のサポートのままにします。</span><span class="sxs-lookup"><span data-stu-id="22e83-117">Once deployed, keep your sites constantly up-to-date with support for continuous deployment.</span></span>
> 
> <span data-ttu-id="22e83-118">Azure では、拡張性、持続性のあるクラウドの記憶域、バックアップ、および復旧のソリューション、データを小規模または大規模なです。</span><span class="sxs-lookup"><span data-stu-id="22e83-118">Azure provides scalable, durable cloud storage, backup, and recovery solutions for any data, big or small.</span></span> <span data-ttu-id="22e83-119">実稼働環境、テーブル、Blob、SQL データベースなどの記憶域サービスのアプリケーションを展開するときに、クラウドでアプリケーションを拡張できます。</span><span class="sxs-lookup"><span data-stu-id="22e83-119">When deploying applications to a production environment, storage services such as Tables, Blobs and SQL Databases, help you scale your application in the cloud.</span></span>
> 
> <span data-ttu-id="22e83-120">SQL データベースを使用するには、新しいバージョンのアプリケーションを展開するときに生産性の高いデータベースを最新に保つ必要があります。</span><span class="sxs-lookup"><span data-stu-id="22e83-120">With SQL Databases, it is important to keep your productive database up-to-date when deploying new versions of your application.</span></span> <span data-ttu-id="22e83-121">感謝**Entity Framework Code First Migrations**、分単位で、環境を更新する、開発と、データ モデルの配置が簡素化されています。</span><span class="sxs-lookup"><span data-stu-id="22e83-121">Thanks to **Entity Framework Code First Migrations**, the development and deployment of your data model has been simplified to update your environments in minutes.</span></span> <span data-ttu-id="22e83-122">このハンズオン ラボでは、Microsoft Azure で実稼働環境に web アプリを配置するときに発生する可能性がありますのさまざまなトピックを示します。</span><span class="sxs-lookup"><span data-stu-id="22e83-122">This hands-on lab will show you the different topics you could encounter when deploying your web app to production environments in Microsoft Azure.</span></span>
> 
> <span data-ttu-id="22e83-123">すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit)です。</span><span class="sxs-lookup"><span data-stu-id="22e83-123">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>
> 
> <span data-ttu-id="22e83-124">このトピックの詳しい内容について詳細を参照してください、 [Azure 電子書籍の実際のクラウド アプリのビルド](building-real-world-cloud-apps-with-windows-azure/introduction.md)です。</span><span class="sxs-lookup"><span data-stu-id="22e83-124">For more in-depth coverage of this topic, see the [Building Real-World Cloud Apps with Azure e-book](building-real-world-cloud-apps-with-windows-azure/introduction.md).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="22e83-125">概要</span><span class="sxs-lookup"><span data-stu-id="22e83-125">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="22e83-126">目的</span><span class="sxs-lookup"><span data-stu-id="22e83-126">Objectives</span></span>

<span data-ttu-id="22e83-127">このハンズオン ラボでは学習する方法。</span><span class="sxs-lookup"><span data-stu-id="22e83-127">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="22e83-128">既存のモデルとエンティティ フレームワークの移行を有効にします。</span><span class="sxs-lookup"><span data-stu-id="22e83-128">Enable Entity Framework Migrations with an existing model</span></span>
- <span data-ttu-id="22e83-129">オブジェクト モデルとエンティティ フレームワークの移行を適宜使用してデータベースを更新します。</span><span class="sxs-lookup"><span data-stu-id="22e83-129">Update the object model and database accordingly using Entity Framework Migrations</span></span>
- <span data-ttu-id="22e83-130">Git を使用して Azure App Service に展開します。</span><span class="sxs-lookup"><span data-stu-id="22e83-130">Deploy to Azure App Service using Git</span></span>
- <span data-ttu-id="22e83-131">Azure 管理ポータルを使用して以前の展開にロールバック</span><span class="sxs-lookup"><span data-stu-id="22e83-131">Rollback to a previous deployment using the Azure Management portal</span></span>
- <span data-ttu-id="22e83-132">Azure ストレージを使用して web アプリをスケールするには</span><span class="sxs-lookup"><span data-stu-id="22e83-132">Use Azure Storage to scale a web app</span></span>
- <span data-ttu-id="22e83-133">Azure 管理ポータルを使用して、web アプリの自動スケーリングを構成します。</span><span class="sxs-lookup"><span data-stu-id="22e83-133">Configure auto-scaling for a web app using the Azure Management Portal</span></span>
- <span data-ttu-id="22e83-134">作成し、Visual Studio でのロード テストのプロジェクトを構成します。</span><span class="sxs-lookup"><span data-stu-id="22e83-134">Create and configure a load test project in Visual Studio</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="22e83-135">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="22e83-135">Prerequisites</span></span>

<span data-ttu-id="22e83-136">このハンズオン ラボを完了する、次が必要。</span><span class="sxs-lookup"><span data-stu-id="22e83-136">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="22e83-137">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/)以上</span><span class="sxs-lookup"><span data-stu-id="22e83-137">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="22e83-138">Azure SDK for .NET 2.2</span><span class="sxs-lookup"><span data-stu-id="22e83-138">Azure SDK for .NET 2.2</span></span>](https://www.microsoft.com/windowsazure/sdk/)
- [<span data-ttu-id="22e83-139">GIT バージョン管理システム</span><span class="sxs-lookup"><span data-stu-id="22e83-139">GIT Version Control System</span></span>](http://git-scm.com/download)
- <span data-ttu-id="22e83-140">Microsoft Azure サブスクリプション</span><span class="sxs-lookup"><span data-stu-id="22e83-140">A Microsoft Azure subscription</span></span> 

    - <span data-ttu-id="22e83-141">サインアップ、[無料試用版](http://aka.ms/watk-freetrial)</span><span class="sxs-lookup"><span data-stu-id="22e83-141">Sign up for a [Free Trial](http://aka.ms/watk-freetrial)</span></span>
    - <span data-ttu-id="22e83-142">場合は、Visual Studio Professional、Test Professional、Premium または Ultimate の MSDN または MSDN のプラットフォームのサブスクライバーと、アクティブ化、 [MSDN 特典付き](http://aka.ms/watk-msdn)の開発と Azure でテストを開始するようになりました</span><span class="sxs-lookup"><span data-stu-id="22e83-142">If you are a Visual Studio Professional, Test Professional, Premium or Ultimate with MSDN or MSDN Platforms subscriber, activate your [MSDN benefit](http://aka.ms/watk-msdn) now to start developing and testing on Azure</span></span>
    - <span data-ttu-id="22e83-143">[BizSpark](http://aka.ms/watk-bizspark)メンバーは、Azure を自動的に受信を通じて、Visual Studio Ultimate with MSDN サブスクリプション特典</span><span class="sxs-lookup"><span data-stu-id="22e83-143">[BizSpark](http://aka.ms/watk-bizspark) members automatically receive the Azure benefit through their Visual Studio Ultimate with MSDN subscriptions</span></span>
    - <span data-ttu-id="22e83-144">メンバー、 [Microsoft Partner Network](http://aka.ms/watk-mpn) Cloud Essentials プログラム毎月 Azure クレジットが無料の受信</span><span class="sxs-lookup"><span data-stu-id="22e83-144">Members of the [Microsoft Partner Network](http://aka.ms/watk-mpn) Cloud Essentials program receive monthly Azure credits at no charge</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="22e83-145">セットアップ</span><span class="sxs-lookup"><span data-stu-id="22e83-145">Setup</span></span>

<span data-ttu-id="22e83-146">このハンズオン ラボでは、演習を実行するのには、最初に、環境を設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="22e83-146">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="22e83-147">開いている Windows エクスプ ローラーおよびテスト環境の参照**ソース**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="22e83-147">Open Windows Explorer and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="22e83-148">右クリックして**Setup.cmd**選択と**管理者として実行**を環境を構成して、このラボ用の Visual Studio のコード スニペットをインストールするセットアップ プロセスを起動します。</span><span class="sxs-lookup"><span data-stu-id="22e83-148">Right-click on **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="22e83-149">ユーザー アカウント制御 ダイアログが表示されている場合は、アクションを続行を確認します。</span><span class="sxs-lookup"><span data-stu-id="22e83-149">If the User Account Control dialog is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="22e83-150">セットアップを実行する前に、このラボのすべての依存関係をチェックしたことを確認してください。</span><span class="sxs-lookup"><span data-stu-id="22e83-150">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="22e83-151">コード スニペットを使用します。</span><span class="sxs-lookup"><span data-stu-id="22e83-151">Using the Code Snippets</span></span>

<span data-ttu-id="22e83-152">ラボ ドキュメント全体にわたって、コード ブロックを挿入するように指示されます。</span><span class="sxs-lookup"><span data-stu-id="22e83-152">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="22e83-153">便宜上、このコードのほとんどは、Visual Studio コード スニペットを手動で追加することを回避する Visual Studio 2013 内からアクセスできるとして提供されます。</span><span class="sxs-lookup"><span data-stu-id="22e83-153">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="22e83-154">各演習にある開始ソリューションが組み込まれた、**開始**個別に各手順をたどることによってこの作業のフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="22e83-154">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="22e83-155">実習では、追加のコード スニペットがこれらのソリューションの開始から不足しており、演習を完了するまで動作しない可能性がありますに注意してください。</span><span class="sxs-lookup"><span data-stu-id="22e83-155">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="22e83-156">演習では、ソース コード、内部も表示されます、**終了**を対応する手順の手順を実行した結果のコードの Visual Studio ソリューションを含むフォルダー。</span><span class="sxs-lookup"><span data-stu-id="22e83-156">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="22e83-157">このハンズオン ラボを使用すると、追加のヘルプを必要がある場合は、これらのソリューションをガイドとして使用できます。</span><span class="sxs-lookup"><span data-stu-id="22e83-157">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="22e83-158">演習</span><span class="sxs-lookup"><span data-stu-id="22e83-158">Exercises</span></span>

<span data-ttu-id="22e83-159">このハンズオン ラボには、次の実習が含まれます。</span><span class="sxs-lookup"><span data-stu-id="22e83-159">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="22e83-160">Entity Framework Migrations を使用してください。</span><span class="sxs-lookup"><span data-stu-id="22e83-160">Using Entity Framework Migrations</span></span>](#Exercise1)
2. [<span data-ttu-id="22e83-161">ステージングに Web アプリを展開します。</span><span class="sxs-lookup"><span data-stu-id="22e83-161">Deploying a Web App to Staging</span></span>](#Exercise2)
3. [<span data-ttu-id="22e83-162">実稼働環境で展開のロールバックを実行します。</span><span class="sxs-lookup"><span data-stu-id="22e83-162">Performing Deployment Rollback in Production</span></span>](#Exercise3)
4. [<span data-ttu-id="22e83-163">Azure ストレージを使用して scaling</span><span class="sxs-lookup"><span data-stu-id="22e83-163">Scaling Using Azure Storage</span></span>](#Exercise4)
5. <span data-ttu-id="22e83-164">[Web アプリの自動スケールを使用して](#Exercise5)(Visual Studio 2013 Ultimate edition の省略可能)</span><span class="sxs-lookup"><span data-stu-id="22e83-164">[Using Autoscale for Web Apps](#Exercise5) (Optional for Visual Studio 2013 Ultimate edition)</span></span>

<span data-ttu-id="22e83-165">この演習を完了する時間を推定: **75 分**</span><span class="sxs-lookup"><span data-stu-id="22e83-165">Estimated time to complete this lab: **75 minutes**</span></span>

> [!NOTE]
> <span data-ttu-id="22e83-166">Visual Studio を初めて起動するときに定義済みの設定のコレクションの 1 つを選択する必要があります。</span><span class="sxs-lookup"><span data-stu-id="22e83-166">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="22e83-167">定義済みの各コレクションは、特定の開発スタイルと一致するものではまた、ウィンドウのレイアウト、エディターの動作、IntelliSense コード スニペットでは、およびダイアログ ボックスのオプションを判断します。</span><span class="sxs-lookup"><span data-stu-id="22e83-167">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="22e83-168">このラボの手順を使用する場合は、Visual Studio での特定のタスクを実行に必要な操作を記述する、**全般的な開発設定**コレクション。</span><span class="sxs-lookup"><span data-stu-id="22e83-168">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="22e83-169">開発環境に合わせてさまざまな設定のコレクションを選択する場合、考慮する必要がある手順に違いがあります。</span><span class="sxs-lookup"><span data-stu-id="22e83-169">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-using-entity-framework-migrations"></a><span data-ttu-id="22e83-170">手順 1: Entity Framework の移行を使用します。</span><span class="sxs-lookup"><span data-stu-id="22e83-170">Exercise 1: Using Entity Framework Migrations</span></span>

<span data-ttu-id="22e83-171">アプリケーションを開発している際に、データ モデルが時間の経過と共に変更します。</span><span class="sxs-lookup"><span data-stu-id="22e83-171">When you are developing an application, your data model might change over time.</span></span> <span data-ttu-id="22e83-172">これらの変更に影響する可能性、データベース内の既存のモデル (作成する場合は、新しいバージョン) と、データベースのエラーを防ぐために最新の状態を保持することが重要です。</span><span class="sxs-lookup"><span data-stu-id="22e83-172">These changes could affect the existing model in your database (if you are creating a new version) and it is important to keep your database up-to-date to prevent errors.</span></span>

<span data-ttu-id="22e83-173">モデル内のこれらの変更の追跡を簡略化する**Entity Framework Code First Migrations**自動的にデータベース スキーマと、モデルを比較する変更を検出し、データベースを更新する特定のコードが生成されます新規に作成する*バージョン*データベースのです。</span><span class="sxs-lookup"><span data-stu-id="22e83-173">To simplify the tracking of these changes in your model, **Entity Framework Code First Migrations** automatically detect changes comparing your model with the database schema and generates specific code to update your database, creating new *versions* of your database.</span></span>

<span data-ttu-id="22e83-174">この演習は、有効にする方法を示します**移行**アプリケーションと簡単に検出しする方法、データベースを更新する変更を生成します。</span><span class="sxs-lookup"><span data-stu-id="22e83-174">This exercise shows you how to enable **Migrations** for your application and how you can easily detect and generate changes to update your databases.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1--enabling-migrations"></a><span data-ttu-id="22e83-175">作業 1: 移行を有効にします。</span><span class="sxs-lookup"><span data-stu-id="22e83-175">Task 1 – Enabling Migrations</span></span>

<span data-ttu-id="22e83-176">このタスクで有効にする手順を参照してください**Entity Framework Code First Migrations**を**マニア Quiz**データベース、モデルを変更してにそれらの変更を反映する方法を理解すること、データベースです。</span><span class="sxs-lookup"><span data-stu-id="22e83-176">In this task, you will go through the steps of enabling **Entity Framework Code First Migrations** to the **Geek Quiz** database, changing the model and understanding how those changes are reflected in the database.</span></span>

1. <span data-ttu-id="22e83-177">Visual Studio を開き、開き、 **GeekQuiz.sln**からソリューション ファイル**Source\Ex1 UsingEntityFrameworkMigrations\Begin**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-177">Open Visual Studio and open the **GeekQuiz.sln** solution file from **Source\Ex1-UsingEntityFrameworkMigrations\Begin**.</span></span>
2. <span data-ttu-id="22e83-178">ダウンロードしてインストールするためにソリューションをビルド、 **NuGet**の依存関係をパッケージ化します。</span><span class="sxs-lookup"><span data-stu-id="22e83-178">Build the solution in order to download and install the **NuGet** package dependencies.</span></span> <span data-ttu-id="22e83-179">これを行うには、ソリューションを右クリックし、をクリックして**ソリューションのビルド**またはキーを押して**Ctrl + Shift + B**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-179">To do this, right-click the solution and click **Build Solution** or press **Ctrl + Shift + B**.</span></span>
3. <span data-ttu-id="22e83-180">**ツール**選択、Visual Studio のメニュー**ライブラリ パッケージ マネージャー**、クリックして**パッケージ マネージャー コンソール**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-180">From the **Tools** menu in Visual Studio, select **Library Package Manager**, and then click **Package Manager Console**.</span></span>
4. <span data-ttu-id="22e83-181">**Package Manager Console**、次のコマンドを入力し、キーを押します**Enter**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-181">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span> <span data-ttu-id="22e83-182">既存のモデルに基づいて最初の移行が作成されます。</span><span class="sxs-lookup"><span data-stu-id="22e83-182">An initial migration based on the existing model will be created.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample1.ps1)]

    <span data-ttu-id="22e83-183">![移行を有効にする](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "移行を有効にします。")</span><span class="sxs-lookup"><span data-stu-id="22e83-183">![Enabling Migrations](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "Enabling Migrations")</span></span>

    <span data-ttu-id="22e83-184">*移行を有効にします。*</span><span class="sxs-lookup"><span data-stu-id="22e83-184">*Enabling Migrations*</span></span>

    > [!NOTE]
    > <span data-ttu-id="22e83-185">このコマンドを追加、**移行**マニア クイズを含むプロジェクト ファイルをフォルダーと呼ばれる**される Configuration.cs**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-185">This command adds a **Migrations** folder to Geek Quiz project that contains a file called **Configuration.cs**.</span></span> <span data-ttu-id="22e83-186">**構成**クラスでは、コンテキストの移行の動作を構成することができます。</span><span class="sxs-lookup"><span data-stu-id="22e83-186">The **Configuration** class allows you to configure how Migrations behaves for your context.</span></span>
5. <span data-ttu-id="22e83-187">移行が有効になっているで更新する必要があります、**構成**初期データと共にデータベースを設定するクラスを**マニア Quiz**が必要です。</span><span class="sxs-lookup"><span data-stu-id="22e83-187">With Migrations enabled, you need to update the **Configuration** class to populate the database with the initial data that **Geek Quiz** requires.</span></span> <span data-ttu-id="22e83-188">**移行**、置換、**される Configuration.cs**で 1 つ持つファイルが格納されて、 **Source\Assets**このラボのフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="22e83-188">Under **Migrations**, replace the **Configuration.cs** file with the one located in the **Source\Assets** folder of this lab.</span></span>

    > [!NOTE]
    > <span data-ttu-id="22e83-189">**移行**が呼び出す、**シード**メソッドすべてのデータベース更新プログラムをデータベース内のレコードが重複していないことを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="22e83-189">Since **Migrations** will call the **Seed** method with every database update, you need to be sure that records are not duplicated in the database.</span></span> <span data-ttu-id="22e83-190">**AddOrUpdate**方法は、データの重複を防ぐために役立ちます。</span><span class="sxs-lookup"><span data-stu-id="22e83-190">The **AddOrUpdate** method will help to prevent duplicate data.</span></span>
6. <span data-ttu-id="22e83-191">追加する最初の移行、次のコマンドを入力し、キーを押します**Enter**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-191">To add an initial migration, enter the following command and then press **Enter**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="22e83-192">という名前のデータベースがないことを確認してください&quot;GeekQuizProd&quot; LocalDB インスタンスにします。</span><span class="sxs-lookup"><span data-stu-id="22e83-192">Make sure that there is no database named &quot;GeekQuizProd&quot; in your LocalDB instance.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample2.ps1)]

    <span data-ttu-id="22e83-193">![ベース スキーマの移行を追加する](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "追加ベース スキーマの移行")</span><span class="sxs-lookup"><span data-stu-id="22e83-193">![Adding base schema migration](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "Adding base schema migration")</span></span>

    <span data-ttu-id="22e83-194">*ベース スキーマの移行を追加します。*</span><span class="sxs-lookup"><span data-stu-id="22e83-194">*Adding base schema migration*</span></span>

    > [!NOTE]
    > <span data-ttu-id="22e83-195">**追加の移行**前回の移行が作成された後に、モデルに行われた変更に基づき、次の移行をスキャフォールディングできます。</span><span class="sxs-lookup"><span data-stu-id="22e83-195">**Add-Migration** will scaffold the next migration based on changes you have made to your model since the last migration was created.</span></span> <span data-ttu-id="22e83-196">この場合、プロジェクトの最初の移行が追加で定義されているすべてのテーブルを作成するスクリプト、 **TriviaContext**クラスです。</span><span class="sxs-lookup"><span data-stu-id="22e83-196">In this case, as it is the first migration of the project, it will add the scripts to create all the tables defined in the **TriviaContext** class.</span></span>
7. <span data-ttu-id="22e83-197">次のコマンドを実行して、データベースを更新する移行を実行します。</span><span class="sxs-lookup"><span data-stu-id="22e83-197">Execute the migration to update the database by running the following command.</span></span> <span data-ttu-id="22e83-198">このコマンドを指定します、 **Verbose**先データベースに適用されている SQL ステートメントを表示するフラグ。</span><span class="sxs-lookup"><span data-stu-id="22e83-198">For this command you will specify the **Verbose** flag to view the SQL statements being applied to the target database.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample3.ps1)]

    <span data-ttu-id="22e83-199">![最初にデータベースの作成](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "最初にデータベースの作成")</span><span class="sxs-lookup"><span data-stu-id="22e83-199">![Creating initial database](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "Creating initial database")</span></span>

    <span data-ttu-id="22e83-200">*初期データベースを作成します。*</span><span class="sxs-lookup"><span data-stu-id="22e83-200">*Creating initial database*</span></span>

    > [!NOTE]
    > <span data-ttu-id="22e83-201">**データベースを更新**保留中の移行をデータベースに適用されます。</span><span class="sxs-lookup"><span data-stu-id="22e83-201">**Update-Database** will apply any pending migrations to the database.</span></span> <span data-ttu-id="22e83-202">ここでは、これは、web.config ファイルで定義されている接続文字列を使用してデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="22e83-202">In this case, it will create the database using the connection string defined in your web.config file.</span></span>
8. <span data-ttu-id="22e83-203">移動して**ビュー**メニューと開いている**SQL Server オブジェクト エクスプ ローラー**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-203">Go to **View** menu and open **SQL Server Object Explorer**.</span></span>

    <span data-ttu-id="22e83-204">![SQL Server オブジェクト エクスプ ローラーで開く](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "SQL Server オブジェクト エクスプ ローラーで開く")</span><span class="sxs-lookup"><span data-stu-id="22e83-204">![Open in SQL Server Object Explorer](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "Open in SQL Server Object Explorer")</span></span>

    <span data-ttu-id="22e83-205">*SQL Server オブジェクト エクスプ ローラーで開く*</span><span class="sxs-lookup"><span data-stu-id="22e83-205">*Open in SQL Server Object Explorer*</span></span>
9. <span data-ttu-id="22e83-206">**SQL Server オブジェクト エクスプ ローラー**  ウィンドウを右クリックして、LocalDB インスタンスに接続、 **SQL Server**ノードを選択して**SQL サーバーを追加しています.**オプション。</span><span class="sxs-lookup"><span data-stu-id="22e83-206">In the **SQL Server Object Explorer** window, connect to your LocalDB instance by right-clicking the **SQL Server** node and selecting **Add SQL Server...** option.</span></span>

    <span data-ttu-id="22e83-207">![SQL Server インスタンスを追加する](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "SQL Server インスタンスを追加します。")</span><span class="sxs-lookup"><span data-stu-id="22e83-207">![Adding a SQL Server Instance](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "Adding a SQL Server Instance")</span></span>

    <span data-ttu-id="22e83-208">*SQL Server オブジェクト エクスプ ローラーに SQL Server インスタンスを追加します。*</span><span class="sxs-lookup"><span data-stu-id="22e83-208">*Adding a SQL Server instance to SQL Server Object Explorer*</span></span>
10. <span data-ttu-id="22e83-209">設定、**サーバー名**に*(localdb) \v11.0*のままにして**Windows 認証**認証モードとして。</span><span class="sxs-lookup"><span data-stu-id="22e83-209">Set the **server name** to *(localdb)\v11.0* and leave **Windows Authentication** as your authentication mode.</span></span> <span data-ttu-id="22e83-210">をクリックして**接続**を続行します。</span><span class="sxs-lookup"><span data-stu-id="22e83-210">Click **Connect** to continue.</span></span>

    <span data-ttu-id="22e83-211">![LocalDB への接続](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "LocalDB への接続")</span><span class="sxs-lookup"><span data-stu-id="22e83-211">![Connecting to LocalDB](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "Connecting to LocalDB")</span></span>

    <span data-ttu-id="22e83-212">*LocalDB への接続*</span><span class="sxs-lookup"><span data-stu-id="22e83-212">*Connecting to LocalDB*</span></span>
11. <span data-ttu-id="22e83-213">開く、 **GeekQuizProd**データベースを展開し、**テーブル**ノード。</span><span class="sxs-lookup"><span data-stu-id="22e83-213">Open the **GeekQuizProd** database and expand the **Tables** node.</span></span> <span data-ttu-id="22e83-214">わかります、 **Update-database**コマンドで定義されているすべてのテーブルを生成する、 **TriviaContext**クラスです。</span><span class="sxs-lookup"><span data-stu-id="22e83-214">As you can see, the **Update-Database** command generated all the tables defined in the **TriviaContext** class.</span></span> <span data-ttu-id="22e83-215">検索、 **dbo します。TriviaQuestions**テーブルし、列のノードを開きます。</span><span class="sxs-lookup"><span data-stu-id="22e83-215">Locate the **dbo.TriviaQuestions** table and open the columns node.</span></span> <span data-ttu-id="22e83-216">次のタスクでは、このテーブルに新しい列を追加し、データベースを使用して、更新**移行**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-216">In the next task, you will add a new column to this table and update the database using **Migrations**.</span></span>

    <span data-ttu-id="22e83-217">![列の質問にトリビア](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "トリビア質問列")</span><span class="sxs-lookup"><span data-stu-id="22e83-217">![Trivia Questions Columns](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "Trivia Questions Columns")</span></span>

    <span data-ttu-id="22e83-218">*トリビア質問列*</span><span class="sxs-lookup"><span data-stu-id="22e83-218">*Trivia Questions Columns*</span></span>

<a id="Ex1Task2"></a>
#### <a name="task-2--updating-database-schema-using-migrations"></a><span data-ttu-id="22e83-219">タスク 2: 移行を使用してデータベース スキーマを更新</span><span class="sxs-lookup"><span data-stu-id="22e83-219">Task 2 – Updating Database Schema Using Migrations</span></span>

<span data-ttu-id="22e83-220">このタスクでは、使用**Entity Framework Code First Migrations**モデルの変更を検出し、データベースを更新するために必要なコードを生成します。</span><span class="sxs-lookup"><span data-stu-id="22e83-220">In this task, you will use **Entity Framework Code First Migrations** to detect a change in your model and generate the necessary code to update the database.</span></span> <span data-ttu-id="22e83-221">更新が表示される、 **TriviaQuestions**エンティティを新しいプロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="22e83-221">You will update the **TriviaQuestions** entity by adding a new property to it.</span></span> <span data-ttu-id="22e83-222">テーブルに新しい列を含める新しい移行を作成するコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="22e83-222">Then you will run commands to create a new Migration to include the new column in the table.</span></span>

1. <span data-ttu-id="22e83-223">**ソリューション エクスプ ローラー**をダブルクリックして、 **TriviaQuestion.cs**ファイル内にある、**モデル**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="22e83-223">In **Solution Explorer**, double-click the **TriviaQuestion.cs** file located inside the **Models** folder.</span></span>
2. <span data-ttu-id="22e83-224">という名前の新しいプロパティを追加**ヒント**の次のコード スニペットに示すようにします。</span><span class="sxs-lookup"><span data-stu-id="22e83-224">Add a new property named **Hint**, as shown in the following code snippet.</span></span>

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample4.cs)]
3. <span data-ttu-id="22e83-225">**Package Manager Console**、次のコマンドを入力し、キーを押します**Enter**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-225">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span> <span data-ttu-id="22e83-226">新しい移行は、モデルの変更を反映して作成されます。</span><span class="sxs-lookup"><span data-stu-id="22e83-226">A new migration will be created reflecting the change in our model.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample5.ps1)]

    <span data-ttu-id="22e83-227">![Add-Migration](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "Add-Migration")</span><span class="sxs-lookup"><span data-stu-id="22e83-227">![Add-Migration](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "Add-Migration")</span></span>

    <span data-ttu-id="22e83-228">*Add-Migration*</span><span class="sxs-lookup"><span data-stu-id="22e83-228">*Add-Migration*</span></span>

    > [!NOTE]
    > <span data-ttu-id="22e83-229">移行ファイルが 2 つの方法で構成される**を**と**ダウン**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-229">A Migration file is composed of two methods, **Up** and **Down**.</span></span>
    > 
    > - <span data-ttu-id="22e83-230">**を**メソッドは、どのような変更をデータベースに適用する、アプリケーションのニーズの現在のバージョンを指定する使用されます。</span><span class="sxs-lookup"><span data-stu-id="22e83-230">The **Up** method will be used to specify what changes the current version of our application need to apply to the database.</span></span>
    > - <span data-ttu-id="22e83-231">**ダウン**に追加した変更を取り消すために使用、**を**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="22e83-231">The **Down** is used to reverse the changes we have added to the **Up** method.</span></span>
    > 
    > <span data-ttu-id="22e83-232">更新すると、データベースの移行、データベース、すべての移行、タイムスタンプ順、および前回の更新以降使用されていないもののみで実行されます (、 \_MigrationHistory テーブルは、移行の種類が適用されているを追跡し続けます)。</span><span class="sxs-lookup"><span data-stu-id="22e83-232">When the Database Migration updates the database, it will run all migrations in the timestamp order, and only those that have not been used since the last update (The \_MigrationHistory table keeps track of which migrations have been applied).</span></span> <span data-ttu-id="22e83-233">**を**すべての移行のメソッドが呼び出され、データベースへ指定した変更を加えます。</span><span class="sxs-lookup"><span data-stu-id="22e83-233">The **Up** method of all migrations will be called and will make the changes we have specified to the database.</span></span> <span data-ttu-id="22e83-234">前の移行に戻る場合、**ダウン**逆の順序の変更を再実行するメソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="22e83-234">If we decide to go back to a previous migration, the **Down** method will be called to redo the changes in a reverse order.</span></span>
4. <span data-ttu-id="22e83-235">**Package Manager Console**、次のコマンドを入力し、キーを押します**Enter**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-235">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample6.ps1)]
5. <span data-ttu-id="22e83-236">出力、 **Update-database**生成されたコマンド、 **Alter Table**に新しい列を追加する SQL ステートメント、 **TriviaQuestions**表に、次の図に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="22e83-236">The output of the **Update-Database** command generated an **Alter Table** SQL statement to add a new column to the **TriviaQuestions** table, as shown in the image below.</span></span>

    <span data-ttu-id="22e83-237">![生成される SQL ステートメントの列を追加](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "生成される SQL ステートメントの列を追加")</span><span class="sxs-lookup"><span data-stu-id="22e83-237">![Add column SQL statement generated](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "Add column SQL statement generated")</span></span>

    <span data-ttu-id="22e83-238">*生成される SQL ステートメントの列を追加します。*</span><span class="sxs-lookup"><span data-stu-id="22e83-238">*Add column SQL statement generated*</span></span>
6. <span data-ttu-id="22e83-239">**SQL Server オブジェクト エクスプ ローラー**、更新、 **dbo します。TriviaQuestions**テーブルが表示されことを確認、新しい**ヒント**列が表示されます。</span><span class="sxs-lookup"><span data-stu-id="22e83-239">In **SQL Server Object Explorer**, refresh the **dbo.TriviaQuestions** table and check that the new **Hint** column is displayed.</span></span>

    <span data-ttu-id="22e83-240">![新しい [ヒント] 列を示す](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "新しいヒント列を表示")</span><span class="sxs-lookup"><span data-stu-id="22e83-240">![Showing the new Hint Column](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "Showing the new Hint Column")</span></span>

    <span data-ttu-id="22e83-241">*新しい [ヒント] 列の表示*</span><span class="sxs-lookup"><span data-stu-id="22e83-241">*Showing the new Hint Column*</span></span>
7. <span data-ttu-id="22e83-242">戻り、 **TriviaQuestion.cs**エディターを追加、 **StringLength**制約を*ヒント*プロパティ、次のコード スニペットで示すようにします。</span><span class="sxs-lookup"><span data-stu-id="22e83-242">Back in the **TriviaQuestion.cs** editor, add a **StringLength** constraint to the *Hint* property, as shown in the following code snippet.</span></span>

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample7.cs)]
8. <span data-ttu-id="22e83-243">**Package Manager Console**、次のコマンドを入力し、キーを押します**Enter**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-243">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample8.ps1)]
9. <span data-ttu-id="22e83-244">**Package Manager Console**、次のコマンドを入力し、キーを押します**Enter**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-244">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample9.ps1)]
10. <span data-ttu-id="22e83-245">出力、 **Update-database**生成されたコマンド、 **Alter Table**を更新する SQL ステートメント、*ヒント*の列の型、 **TriviaQuestions**表に、次の図に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="22e83-245">The output of the **Update-Database** command generated an **Alter Table** SQL statement to update the *hint* column type of the **TriviaQuestions** table, as shown in the image below.</span></span>

    <span data-ttu-id="22e83-246">![生成される SQL ステートメントの列を alter](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "Alter column SQL ステートメントの生成")</span><span class="sxs-lookup"><span data-stu-id="22e83-246">![Alter column SQL statement generated](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "Alter column SQL statement generated")</span></span>

    <span data-ttu-id="22e83-247">*生成された列の SQL ステートメントを変更します。*</span><span class="sxs-lookup"><span data-stu-id="22e83-247">*Alter column SQL statement generated*</span></span>
11. <span data-ttu-id="22e83-248">**SQL Server オブジェクト エクスプ ローラー**、更新、 **dbo します。TriviaQuestions**テーブルが表示されことを確認、**ヒント**列の型が**nvarchar(150)**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-248">In **SQL Server Object Explorer**, refresh the **dbo.TriviaQuestions** table and check that the **Hint** column type is **nvarchar(150)**.</span></span>

    <span data-ttu-id="22e83-249">![新しい制約を示す](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "新しい制約を示す")</span><span class="sxs-lookup"><span data-stu-id="22e83-249">![Showing the new constraint](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "Showing the new constraint")</span></span>

    <span data-ttu-id="22e83-250">*新しい制約を示す*</span><span class="sxs-lookup"><span data-stu-id="22e83-250">*Showing the new constraint*</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-deploying-a-web-app-to-staging"></a><span data-ttu-id="22e83-251">手順 2: ステージングに Web アプリを展開します。</span><span class="sxs-lookup"><span data-stu-id="22e83-251">Exercise 2: Deploying a Web App to Staging</span></span>

<span data-ttu-id="22e83-252">**Web アプリを Azure App service の**ステージングされた発行を実行することができます。</span><span class="sxs-lookup"><span data-stu-id="22e83-252">**Web Apps in Azure App Service** enables you to perform staged publishing.</span></span> <span data-ttu-id="22e83-253">ステージングされた発行では、既定の運用サイトごとのステージング サイト スロットを作成し、ダウンタイムなしでこれらのスロットをスワップすることができます。</span><span class="sxs-lookup"><span data-stu-id="22e83-253">Staged publishing creates a staging site slot for each default production site and enables you to swap these slots with no down time.</span></span> <span data-ttu-id="22e83-254">これは一般に解放する前に変更を検証、増分を統合するサイトのコンテンツ変更が期待どおりに動作していない場合をロールバック本当に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="22e83-254">This is really useful to validate changes before releasing to the public, incrementally integrate site content, and roll back if changes are not working as expected.</span></span>

<span data-ttu-id="22e83-255">この演習では展開、**マニア Quiz** Git ソース管理を使用して、web アプリのステージング環境へのアプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="22e83-255">In this exercise, you will deploy the **Geek Quiz** application to the staging environment of your web app using Git source control.</span></span> <span data-ttu-id="22e83-256">これを行うが web アプリを作成し、管理ポータルで必要なコンポーネントのプロビジョニング、構成、 **Git**リポジトリとプッシュ アプリケーション ソース コードをローカル コンピューターから、ステージング スロットにします。</span><span class="sxs-lookup"><span data-stu-id="22e83-256">To do this, you will create the web app and provision the required components at the management portal, configure a **Git** repository and push the application source code from your local computer to the staging slot.</span></span> <span data-ttu-id="22e83-257">使用して実稼働データベースを更新することも、 **Code First Migrations**前述の手順で作成します。</span><span class="sxs-lookup"><span data-stu-id="22e83-257">You will also update your production database with the **Code First Migrations** you created in the previous exercise.</span></span> <span data-ttu-id="22e83-258">その操作を確認するには、このテスト環境でアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="22e83-258">You will then execute the application in this test environment to verify its operation.</span></span> <span data-ttu-id="22e83-259">入力が完了したらことが期待どおりに動作して、実稼働環境にアプリケーションが昇格されます。</span><span class="sxs-lookup"><span data-stu-id="22e83-259">Once you are satisfied that it is working according to your expectations, you will promote the application to production.</span></span>

> [!NOTE]
> <span data-ttu-id="22e83-260">ステージングされた発行を有効にするには、web アプリにあります**標準モード**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-260">To enable staged publishing, the web app must be in **Standard mode**.</span></span> <span data-ttu-id="22e83-261">Web アプリを標準モードに変更する場合に追加料金が発生することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="22e83-261">Note that additional charges will be incurred if you change your web app to Standard mode.</span></span> <span data-ttu-id="22e83-262">料金の詳細については、次を参照してください。 [App Service の料金](https://azure.microsoft.com/pricing/details/app-service/)です。</span><span class="sxs-lookup"><span data-stu-id="22e83-262">For more information about pricing, see [App Service Pricing](https://azure.microsoft.com/pricing/details/app-service/).</span></span>


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-web-app-in-azure-app-service"></a><span data-ttu-id="22e83-263">作業 1: Azure App Service で Web アプリの作成</span><span class="sxs-lookup"><span data-stu-id="22e83-263">Task 1 – Creating a Web App in Azure App Service</span></span>

<span data-ttu-id="22e83-264">このタスクでは、web アプリを作成します**Azure App Service**管理ポータルからです。</span><span class="sxs-lookup"><span data-stu-id="22e83-264">In this task, you will create a web app in **Azure App Service** from the management portal.</span></span> <span data-ttu-id="22e83-265">また、構成、 **SQL データベース**アプリケーション データを保持して、ローカル Git リポジトリをソース管理を構成します。</span><span class="sxs-lookup"><span data-stu-id="22e83-265">You will also configure a **SQL Database** to persist the application data, and configure a local Git repository for source control.</span></span>

1. <span data-ttu-id="22e83-266">移動して、 [Azure 管理ポータル](https://manage.windowsazure.com)サブスクリプションに関連付けられている Microsoft アカウントを使用してサインインします。</span><span class="sxs-lookup"><span data-stu-id="22e83-266">Go to the [Azure management portal](https://manage.windowsazure.com) and sign in using the Microsoft account associated with your subscription.</span></span>

    ![Azure 管理ポータルにサインインするには](maintainable-azure-websites-managing-change-and-scale/_static/image13.png)

    <span data-ttu-id="22e83-268">*Azure 管理ポータルにサインインするには*</span><span class="sxs-lookup"><span data-stu-id="22e83-268">*Sign in to the Azure management portal*</span></span>
2. <span data-ttu-id="22e83-269">をクリックして**新規**ページの下部にあるコマンド バーにします。</span><span class="sxs-lookup"><span data-stu-id="22e83-269">Click **New** in the command bar at the bottom of the page.</span></span>

    <span data-ttu-id="22e83-270">![新しい web アプリを作成する](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "新しい web アプリを作成します。")</span><span class="sxs-lookup"><span data-stu-id="22e83-270">![Creating a new web app](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "Creating a new web app")</span></span>

    <span data-ttu-id="22e83-271">*新しい web アプリを作成します。*</span><span class="sxs-lookup"><span data-stu-id="22e83-271">*Creating a new web app*</span></span>
3. <span data-ttu-id="22e83-272">をクリックして**コンピューティング**、 **web サイト**し**カスタム作成**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-272">Click **Compute**, **Website** and then **Custom Create**.</span></span>

    <span data-ttu-id="22e83-273">![カスタム作成を使用して新しい web アプリを作成する](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "カスタム作成を使用して新しい web アプリを作成します。")</span><span class="sxs-lookup"><span data-stu-id="22e83-273">![Creating a new web app using Custom Create](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "Creating a new web app using Custom Create")</span></span>

    <span data-ttu-id="22e83-274">*カスタム作成を使用して新しい web アプリを作成します。*</span><span class="sxs-lookup"><span data-stu-id="22e83-274">*Creating a new web app using Custom Create*</span></span>
4. <span data-ttu-id="22e83-275">**新しい web サイト - カスタム作成** ダイアログ ボックスで、使用可能な指定**URL** (例:*マニア クイズ*)、場所を選択、**地域**ドロップダウン リスト、および選択**新しい SQL データベースの作成**で、**データベース**ドロップダウン リスト。</span><span class="sxs-lookup"><span data-stu-id="22e83-275">In the **New Website - Custom Create** dialog box, provide an available **URL** (e.g. *geek-quiz*), select a location in the **Region** drop-down list, and select **Create a new SQL database** in the **Database** drop-down list.</span></span> <span data-ttu-id="22e83-276">最後に、選択、**ソース管理から発行** チェック ボックスをクリック**次**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-276">Finally, select the **Publish from source control** check box and click **Next**.</span></span>

    ![新しい web アプリのカスタマイズ](maintainable-azure-websites-managing-change-and-scale/_static/image16.png)

    <span data-ttu-id="22e83-278">*新しい web アプリのカスタマイズ*</span><span class="sxs-lookup"><span data-stu-id="22e83-278">*Customizing the new web app*</span></span>
5. <span data-ttu-id="22e83-279">データベースの設定の次の情報を指定します。</span><span class="sxs-lookup"><span data-stu-id="22e83-279">Specify the following information for the database settings:</span></span>

    - <span data-ttu-id="22e83-280">**名前**テキスト ボックスに、データベースの名前を入力 (例: *geekquiz\_db*)</span><span class="sxs-lookup"><span data-stu-id="22e83-280">In the **Name** text box, enter a database name (e.g. *geekquiz\_db*)</span></span>
    - <span data-ttu-id="22e83-281">サーバーで**ドロップダウン**一覧で、**新しい SQL データベース サーバー**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-281">In the Server **drop-down** list, select **New SQL database server**.</span></span> <span data-ttu-id="22e83-282">または、既存のサーバーを選択することができます。</span><span class="sxs-lookup"><span data-stu-id="22e83-282">Alternatively, you can select an existing server.</span></span>
    - <span data-ttu-id="22e83-283">**データベース ユーザー名**と**データベース パスワード**ボックスに、SQL データベース サーバーの管理者のユーザー名とパスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="22e83-283">In the **Database username** and **Database password** boxes, enter the administrator username and password for the SQL database server.</span></span> <span data-ttu-id="22e83-284">サーバーを選択する場合は、既に作成して、パスワードを求められます。</span><span class="sxs-lookup"><span data-stu-id="22e83-284">If you select a server you have already created, you will be prompted for the password.</span></span>

    ![データベースの設定を指定します。](maintainable-azure-websites-managing-change-and-scale/_static/image17.png)

    <span data-ttu-id="22e83-286">*データベースの設定を指定します。*</span><span class="sxs-lookup"><span data-stu-id="22e83-286">*Specifying the database settings*</span></span>
6. <span data-ttu-id="22e83-287">**[次へ]** をクリックして、続行します。</span><span class="sxs-lookup"><span data-stu-id="22e83-287">Click **Next** to continue.</span></span>
7. <span data-ttu-id="22e83-288">選択**ローカル Git リポジトリ**してをクリックして、ソース コントロールの**次**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-288">Select **Local Git repository** for the source control to use and click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="22e83-289">デプロイ資格情報 (ユーザー名とパスワード) の要求があります。</span><span class="sxs-lookup"><span data-stu-id="22e83-289">You may be prompted for the deployment credentials (a username and password).</span></span>

    ![Git レポジトリの作成](maintainable-azure-websites-managing-change-and-scale/_static/image18.png)

    <span data-ttu-id="22e83-291">*Git レポジトリの作成*</span><span class="sxs-lookup"><span data-stu-id="22e83-291">*Creating the Git repository*</span></span>
8. <span data-ttu-id="22e83-292">新しい web アプリが作成されるまで待機します。</span><span class="sxs-lookup"><span data-stu-id="22e83-292">Wait until the new web app is created.</span></span>

    > [!NOTE]
    > <span data-ttu-id="22e83-293">既定では、Azure 提供のドメイン*azurewebsites.net* Azure 管理ポータルを使用してカスタム ドメインを設定することも付与します。</span><span class="sxs-lookup"><span data-stu-id="22e83-293">By default, Azure provides domains at *azurewebsites.net* but also gives you the possibility to set custom domains using the Azure management portal.</span></span> <span data-ttu-id="22e83-294">ただし、特定の Azure App Service モードを使用している場合、のみカスタム ドメインを管理することができます。</span><span class="sxs-lookup"><span data-stu-id="22e83-294">However, you can only manage custom domains if you are using certain Azure App Service modes.</span></span>
    > 
    > <span data-ttu-id="22e83-295">Azure App Service は、空き、Shared、Basic、Standard、および Premium エディションで使用できます。</span><span class="sxs-lookup"><span data-stu-id="22e83-295">Azure App Service is available in Free, Shared, Basic, Standard, and Premium editions.</span></span> <span data-ttu-id="22e83-296">無料モードおよび共有モードでは、すべての web アプリは、マルチ テナント環境で実行し、CPU、メモリ、およびネットワークの使用量クォータがあります。</span><span class="sxs-lookup"><span data-stu-id="22e83-296">In Free and Shared mode, all web apps run in a multi-tenant environment and have quotas for CPU, Memory, and Network usage.</span></span> <span data-ttu-id="22e83-297">無料のアプリの最大数は、プランで異なる場合があります。</span><span class="sxs-lookup"><span data-stu-id="22e83-297">The maximum number of free apps may vary with your plan.</span></span> <span data-ttu-id="22e83-298">標準モードでは、標準の Azure に対応する専用の仮想マシンで実行するアプリのコンピューティング リソースを選択します。</span><span class="sxs-lookup"><span data-stu-id="22e83-298">In Standard mode, you choose which apps run on dedicated virtual machines that correspond to the standard Azure compute resources.</span></span> <span data-ttu-id="22e83-299">Web アプリ モードの構成を見つけることができます、**スケール**web アプリのメニュー。</span><span class="sxs-lookup"><span data-stu-id="22e83-299">You can find the web app mode configuration in the **Scale** menu of your web app.</span></span>
    > 
    > <span data-ttu-id="22e83-300">![Azure App Service モード](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "Azure App Service のモード")</span><span class="sxs-lookup"><span data-stu-id="22e83-300">![Azure App Service Modes](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "Azure App Service Modes")</span></span>
    > 
    > <span data-ttu-id="22e83-301">使用している場合**Shared**または**標準**モードでは、ことができます、アプリに移動して、web アプリのカスタム ドメインを管理する**構成**をクリックしてメニュー**ドメインの管理***ドメイン名*です。</span><span class="sxs-lookup"><span data-stu-id="22e83-301">If you are using **Shared** or **Standard** mode, you will be able to manage custom domains for your web app by going to your app's **Configure** menu and clicking **Manage Domains** under *domain names*.</span></span>
    > 
    > <span data-ttu-id="22e83-302">![ドメインを管理する](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "ドメインを管理します。")</span><span class="sxs-lookup"><span data-stu-id="22e83-302">![Manage Domains](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "Manage Domains")</span></span>
    > 
    > <span data-ttu-id="22e83-303">![カスタム ドメインを管理する](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "カスタム ドメインを管理します。")</span><span class="sxs-lookup"><span data-stu-id="22e83-303">![Manage Custom Domains](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "Manage Custom Domains")</span></span>
9. <span data-ttu-id="22e83-304">Web アプリを作成した後は、下のリンクをクリックして、 **URL**新しい web アプリが実行されていることを確認する列。</span><span class="sxs-lookup"><span data-stu-id="22e83-304">Once the web app is created, click the link under the **URL** column to check that the new web app is running.</span></span>

    ![新しい web アプリへの参照](maintainable-azure-websites-managing-change-and-scale/_static/image22.png)

    <span data-ttu-id="22e83-306">*新しい web アプリへの参照*</span><span class="sxs-lookup"><span data-stu-id="22e83-306">*Browsing to the new web app*</span></span>

    ![web アプリが実行されています。](maintainable-azure-websites-managing-change-and-scale/_static/image23.png)

    <span data-ttu-id="22e83-308">*web アプリが実行されています。*</span><span class="sxs-lookup"><span data-stu-id="22e83-308">*web app running*</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-production-sql-database"></a><span data-ttu-id="22e83-309">タスク 2: 実稼働 SQL データベースの作成</span><span class="sxs-lookup"><span data-stu-id="22e83-309">Task 2 – Creating the Production SQL Database</span></span>

<span data-ttu-id="22e83-310">このタスクでは使用して、 **Entity Framework Code First Migrations**を対象とするデータベースを作成する、 **Azure SQL Database**前のタスクで作成したインスタンス。</span><span class="sxs-lookup"><span data-stu-id="22e83-310">In this task, you will use the **Entity Framework Code First Migrations** to create the database targeting the **Azure SQL Database** instance you created in the previous task.</span></span>

1. <span data-ttu-id="22e83-311">管理ポータルで、前のタスクで作成した web アプリに移動しに移動その**ダッシュ ボード**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-311">In the Management Portal, navigate to the web app you created in the previous task and go to its **Dashboard**.</span></span>
2. <span data-ttu-id="22e83-312">**ダッシュ ボード**] ページで [**接続文字列を表示**下にあるリンク、**クイック グランス**セクションです。</span><span class="sxs-lookup"><span data-stu-id="22e83-312">In the **Dashboard** page, click **View connection strings** link under the **quick glance** section.</span></span>

    <span data-ttu-id="22e83-313">![接続文字列を表示](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "接続文字列を表示")</span><span class="sxs-lookup"><span data-stu-id="22e83-313">![View connection strings](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "View connection strings")</span></span>

    <span data-ttu-id="22e83-314">*接続文字列を表示します。*</span><span class="sxs-lookup"><span data-stu-id="22e83-314">*View connection strings*</span></span>
3. <span data-ttu-id="22e83-315">コピー、**接続文字列**値し、ダイアログ ボックスを閉じます。</span><span class="sxs-lookup"><span data-stu-id="22e83-315">Copy the **connection string** value and close the dialog box.</span></span>

    <span data-ttu-id="22e83-316">![Azure 管理ポータルでの接続文字列](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "Azure 管理ポータルでの接続文字列")</span><span class="sxs-lookup"><span data-stu-id="22e83-316">![Connection String in Azure Management Portal](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "Connection String in Azure Management Portal")</span></span>

    <span data-ttu-id="22e83-317">*Azure 管理ポータルでの接続文字列*</span><span class="sxs-lookup"><span data-stu-id="22e83-317">*Connection String in Azure Management Portal*</span></span>
4. <span data-ttu-id="22e83-318">をクリックして**SQL データベース**azure SQL データベースの一覧を表示するには</span><span class="sxs-lookup"><span data-stu-id="22e83-318">Click **SQL Databases** to see the list of SQL databases in Azure</span></span>

    <span data-ttu-id="22e83-319">![SQL データベース] メニューの [](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "SQL データベース メニュー")</span><span class="sxs-lookup"><span data-stu-id="22e83-319">![SQL Database menu](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "SQL Database menu")</span></span>

    <span data-ttu-id="22e83-320">*SQL データベース メニュー*</span><span class="sxs-lookup"><span data-stu-id="22e83-320">*SQL Database menu*</span></span>
5. <span data-ttu-id="22e83-321">前のタスクで作成したデータベースを見つけて、[サーバー] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="22e83-321">Locate the database you created in the previous task and click on the Server.</span></span>

    <span data-ttu-id="22e83-322">![SQL データベース サーバー](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "SQL データベース サーバー")</span><span class="sxs-lookup"><span data-stu-id="22e83-322">![SQL Database server](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "SQL Database server")</span></span>

    <span data-ttu-id="22e83-323">*SQL データベース サーバー*</span><span class="sxs-lookup"><span data-stu-id="22e83-323">*SQL Database server*</span></span>
6. <span data-ttu-id="22e83-324">**クイック スタート**ページ上をクリックして、サーバーの**構成**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-324">In the **Quick Start** page of the server, click on **Configure**.</span></span>

    <span data-ttu-id="22e83-325">![構成 メニュー](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "構成 メニュー")</span><span class="sxs-lookup"><span data-stu-id="22e83-325">![Configure menu](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "Configure menu")</span></span>

    <span data-ttu-id="22e83-326">*メニューを構成します。*</span><span class="sxs-lookup"><span data-stu-id="22e83-326">*Configure menu*</span></span>
7. <span data-ttu-id="22e83-327">**許可されている IP アドレス**セクションで、をクリックして**許可される IP アドレスを追加**リンクされた SQL データベース サーバーに接続する、IP を有効にします。</span><span class="sxs-lookup"><span data-stu-id="22e83-327">In the **Allowed IP addresses** section, click on **Add to the allowed IP addresses** link to enable your IP to connect to the SQL Database server.</span></span>

    <span data-ttu-id="22e83-328">![使用できる IP アドレス](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "許可されている IP アドレス")</span><span class="sxs-lookup"><span data-stu-id="22e83-328">![Allowed IP addresses](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "Allowed IP addresses")</span></span>

    <span data-ttu-id="22e83-329">*許可される IP アドレス*</span><span class="sxs-lookup"><span data-stu-id="22e83-329">*Allowed IP addresses*</span></span>
8. <span data-ttu-id="22e83-330">をクリックして**保存**手順を実行するページの下部にあります。</span><span class="sxs-lookup"><span data-stu-id="22e83-330">Click **Save** at the bottom of the page to complete the step.</span></span>
9. <span data-ttu-id="22e83-331">Visual Studio に切り替えます。</span><span class="sxs-lookup"><span data-stu-id="22e83-331">Switch back to Visual Studio.</span></span>
10. <span data-ttu-id="22e83-332">**Package Manager Console**、次のコマンドの置換を実行*[、接続文字列]*プレース ホルダーを Azure からコピーした接続文字列</span><span class="sxs-lookup"><span data-stu-id="22e83-332">In the **Package Manager Console**, execute the following command replacing *[YOUR-CONNECTION-STRING]* placeholder with the connection string you copied from Azure</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample10.ps1)]

    <span data-ttu-id="22e83-333">![Windows Azure SQL データベースを対象とするデータベースの更新](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "Windows Azure SQL データベースを対象とするデータベースの更新")</span><span class="sxs-lookup"><span data-stu-id="22e83-333">![Update database targeting Windows Azure SQL Database](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "Update database targeting Windows Azure SQL Database")</span></span>

    <span data-ttu-id="22e83-334">*Azure SQL データベースを対象とするデータベースの更新*</span><span class="sxs-lookup"><span data-stu-id="22e83-334">*Update database targeting Azure SQL Database*</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3--deploying-geek-quiz-to-staging-using-git"></a><span data-ttu-id="22e83-335">タスク 3: Git を使用してステージング展開マニア クイズ</span><span class="sxs-lookup"><span data-stu-id="22e83-335">Task 3 – Deploying Geek Quiz to Staging Using Git</span></span>

<span data-ttu-id="22e83-336">このタスクでは、web アプリのステージングされた発行を有効にします。</span><span class="sxs-lookup"><span data-stu-id="22e83-336">In this task, you will enable staged publishing in your web app.</span></span> <span data-ttu-id="22e83-337">次に、web アプリのステージング環境に、ローカル コンピューターから直接マニア Quiz アプリケーションを発行するのに Git を使用します。</span><span class="sxs-lookup"><span data-stu-id="22e83-337">Then, you will use Git to publish the Geek Quiz application directly from your local computer to the staging environment of your web app.</span></span>

1. <span data-ttu-id="22e83-338">ポータルに戻るしの下で web アプリの名前をクリックして、**名前**管理ページを表示する列。</span><span class="sxs-lookup"><span data-stu-id="22e83-338">Go back to the portal and click the name of the web app under the **Name** column to display the management pages.</span></span>

    ![Web アプリの管理ページを開く](maintainable-azure-websites-managing-change-and-scale/_static/image31.png)

    <span data-ttu-id="22e83-340">*Web アプリの管理ページを開く*</span><span class="sxs-lookup"><span data-stu-id="22e83-340">*Opening the web app management pages*</span></span>
2. <span data-ttu-id="22e83-341">移動し、**スケール**ページ。</span><span class="sxs-lookup"><span data-stu-id="22e83-341">Navigate to the **Scale** page.</span></span> <span data-ttu-id="22e83-342">下にある、**全般**セクションで、**標準**の構成と をクリック**保存**コマンド バーでします。</span><span class="sxs-lookup"><span data-stu-id="22e83-342">Under the **general** section, select **Standard** for the configuration and click **Save** in the command bar.</span></span>

    > [!NOTE]
    > <span data-ttu-id="22e83-343">現在の地域とサブスクリプションのすべての web アプリを実行する**標準**モードのままにして、**すべて選択** チェック ボックスをオンに、**サイトの選択**構成します。</span><span class="sxs-lookup"><span data-stu-id="22e83-343">To run all web apps in the current region and subscription in **Standard** mode, leave the **Select All** check box selected in the **Choose Sites** configuration.</span></span> <span data-ttu-id="22e83-344">それ以外の場合、オフ、**すべて選択**チェック ボックスをオンします。</span><span class="sxs-lookup"><span data-stu-id="22e83-344">Otherwise, clear the **Select All** check box.</span></span>

    <span data-ttu-id="22e83-345">![Web アプリを標準モードにアップグレードする](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "web アプリを標準モードにアップグレードします。")</span><span class="sxs-lookup"><span data-stu-id="22e83-345">![Upgrading the web app to Standard mode](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "Upgrading the web app to Standard mode")</span></span>

    <span data-ttu-id="22e83-346">*Web アプリを標準モードにアップグレードします。*</span><span class="sxs-lookup"><span data-stu-id="22e83-346">*Upgrading the Web App to Standard mode*</span></span>
3. <span data-ttu-id="22e83-347">をクリックして**はい**変更を確認します。</span><span class="sxs-lookup"><span data-stu-id="22e83-347">Click **Yes** to confirm the changes.</span></span>

    <span data-ttu-id="22e83-348">![標準モードに変更を確認する](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "web アプリのモードの変更を続行")</span><span class="sxs-lookup"><span data-stu-id="22e83-348">![Confirming the change to Standard mode](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "Continuing with the changing of the web app mode")</span></span>

    <span data-ttu-id="22e83-349">*標準モードに変更を確認します。*</span><span class="sxs-lookup"><span data-stu-id="22e83-349">*Confirming the change to Standard mode*</span></span>
4. <span data-ttu-id="22e83-350">移動して、**ダッシュ ボード**ページし、をクリックして**ステージングされた発行を有効にする**下にある、**クイック グランス**セクションです。</span><span class="sxs-lookup"><span data-stu-id="22e83-350">Go to the **Dashboard** page and click **Enable staged publishing** under the **quick glance** section.</span></span>

    <span data-ttu-id="22e83-351">![ステージングされた発行を有効にする](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "ステージングされた発行を有効にします。")</span><span class="sxs-lookup"><span data-stu-id="22e83-351">![Enabling staged publishing](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "Enabling staged publishing")</span></span>

    <span data-ttu-id="22e83-352">*ステージングされた発行を有効にします。*</span><span class="sxs-lookup"><span data-stu-id="22e83-352">*Enabling staged publishing*</span></span>
5. <span data-ttu-id="22e83-353">をクリックして**はい**ステージングされた発行を有効にします。</span><span class="sxs-lookup"><span data-stu-id="22e83-353">Click **Yes** to enable staged publishing.</span></span>

    <span data-ttu-id="22e83-354">![ステージングされた発行を確認する](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "ステージングされた発行を有効にするには、[はい] をクリックすると")</span><span class="sxs-lookup"><span data-stu-id="22e83-354">![Confirming staged publishing](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "Clicking Yes to enable staged publishing")</span></span>

    <span data-ttu-id="22e83-355">*ステージングされた発行を確認します。*</span><span class="sxs-lookup"><span data-stu-id="22e83-355">*Confirming staged publishing*</span></span>
6. <span data-ttu-id="22e83-356">Web アプリの一覧で、ステージング サイト スロットを表示する web アプリ名の左側に、マークを展開します。</span><span class="sxs-lookup"><span data-stu-id="22e83-356">In the list of web apps, expand the mark to the left of your web app name to display the staging site slot.</span></span> <span data-ttu-id="22e83-357">続けて、web アプリの名前を持つ***(ステージング)***です。</span><span class="sxs-lookup"><span data-stu-id="22e83-357">It has the name of your web app followed by ***(staging)***.</span></span> <span data-ttu-id="22e83-358">管理ページに移動するステージング サイトをクリックします。</span><span class="sxs-lookup"><span data-stu-id="22e83-358">Click the staging site to go to the management page.</span></span>

    <span data-ttu-id="22e83-359">![ステージングの web アプリに移動する](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "ステージングの web アプリに移動します。")</span><span class="sxs-lookup"><span data-stu-id="22e83-359">![Navigating to the staging web app](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "Navigating to the staging web app")</span></span>

    <span data-ttu-id="22e83-360">*ステージングのアプリに移動します。*</span><span class="sxs-lookup"><span data-stu-id="22e83-360">*Navigating to the staging app*</span></span>
7. <span data-ttu-id="22e83-361">彼管理 ページの他の web アプリの管理 ページのように注意してください。</span><span class="sxs-lookup"><span data-stu-id="22e83-361">Notice that he management page looks like any other web app's management page.</span></span> <span data-ttu-id="22e83-362">移動し、**展開**ページのコピーと、 **Git URL**値。</span><span class="sxs-lookup"><span data-stu-id="22e83-362">Navigate to the **Deployments** page and copy the **Git URL** value.</span></span> <span data-ttu-id="22e83-363">この演習で後で使用されます。</span><span class="sxs-lookup"><span data-stu-id="22e83-363">You will use it later in this exercise.</span></span>

    ![Git の URL の値をコピー](maintainable-azure-websites-managing-change-and-scale/_static/image37.png)

    <span data-ttu-id="22e83-365">*Git の URL の値をコピー*</span><span class="sxs-lookup"><span data-stu-id="22e83-365">*Copying the Git URL value*</span></span>
8. <span data-ttu-id="22e83-366">新しく開きます**Git Bash**コンソールし、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="22e83-366">Open a new **Git Bash** console and execute the following commands.</span></span> <span data-ttu-id="22e83-367">更新プログラム、 *[、アプリケーション パス]*へのパスのプレース ホルダー、 **GeekQuiz**ソリューションにある、 **Source\Ex1 DeployingWebSiteToStaging\Begin**のフォルダーこの演習です。</span><span class="sxs-lookup"><span data-stu-id="22e83-367">Update the *[YOUR-APPLICATION-PATH]* placeholder with the path to the **GeekQuiz** solution, located in the **Source\Ex1-DeployingWebSiteToStaging\Begin** folder of this lab.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample11.cmd)]

    ![Git の初期化と最初のコミット](maintainable-azure-websites-managing-change-and-scale/_static/image38.png)

    <span data-ttu-id="22e83-369">*Git の初期化と最初のコミット*</span><span class="sxs-lookup"><span data-stu-id="22e83-369">*Git initialization and first commit*</span></span>
9. <span data-ttu-id="22e83-370">Web アプリをリモートにプッシュする次のコマンド実行**Git**リポジトリです。</span><span class="sxs-lookup"><span data-stu-id="22e83-370">Run the following command to push your web app to the remote **Git** repository.</span></span> <span data-ttu-id="22e83-371">プレース ホルダーを管理ポータルから取得した URL に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="22e83-371">Replace the placeholder with the URL you obtained from the management portal.</span></span> <span data-ttu-id="22e83-372">展開パスワードを求められます。</span><span class="sxs-lookup"><span data-stu-id="22e83-372">You will be prompted for your deployment password.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample12.cmd)]

    ![Windows Azure へのプッシュ](maintainable-azure-websites-managing-change-and-scale/_static/image39.png)

    <span data-ttu-id="22e83-374">*Azure へのプッシュ*</span><span class="sxs-lookup"><span data-stu-id="22e83-374">*Pushing to Azure*</span></span>

    > [!NOTE]
    > <span data-ttu-id="22e83-375">コンテンツを FTP ホストや GIT リポジトリ web アプリを展開するときに使用して認証する必要があります、**デプロイ資格情報**から web アプリの作成した**クイック スタート**または**ダッシュ ボード**管理ページ。</span><span class="sxs-lookup"><span data-stu-id="22e83-375">When you deploy content to the FTP host or GIT repository of a web app, you must authenticate using the **deployment credentials** that you created from the web app's **Quick Start** or **Dashboard** management pages.</span></span> <span data-ttu-id="22e83-376">デプロイ資格情報がわからない場合、管理ポータルを使用して簡単にリセットできます。</span><span class="sxs-lookup"><span data-stu-id="22e83-376">If you do not know your deployment credentials you can easily reset them using the management portal.</span></span> <span data-ttu-id="22e83-377">Web アプリを開き、**ダッシュ ボード**ページ、をクリックし、**デプロイ資格情報をリセット**リンクします。</span><span class="sxs-lookup"><span data-stu-id="22e83-377">Open the web app **Dashboard** page and click the **Reset your deployment credentials** link.</span></span> <span data-ttu-id="22e83-378">新しいパスワードを指定して、をクリックして**OK**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-378">Provide a new password and click **OK**.</span></span> <span data-ttu-id="22e83-379">デプロイ資格情報は、お客様のサブスクリプションに関連付けられているすべての web アプリを使用して使用に対して有効です。</span><span class="sxs-lookup"><span data-stu-id="22e83-379">Deployment credentials are valid for use with all web apps associated with your subscription.</span></span>
10. <span data-ttu-id="22e83-380">Azure に web アプリを正常にプッシュすることを確認するために、管理ポータルに戻り、をクリックして**Websites**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-380">In order to verify the web app was successfully pushed to Azure, go back to the management portal and click **Websites**.</span></span>
11. <span data-ttu-id="22e83-381">Web アプリを見つけて、ステージング サイト スロットを表示するエントリを展開します。</span><span class="sxs-lookup"><span data-stu-id="22e83-381">Locate your web app and expand the entry to display the staging site slot.</span></span> <span data-ttu-id="22e83-382">クリックしてその**名前**管理ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="22e83-382">Click its **Name** to go to the management page.</span></span>
12. <span data-ttu-id="22e83-383">をクリックして**展開**を表示する、**デプロイ履歴**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-383">Click **Deployments** to see the **deployment history**.</span></span> <span data-ttu-id="22e83-384">あることを確認してください、**アクティブなデプロイ**で、 *&quot;初期コミット&quot;*です。</span><span class="sxs-lookup"><span data-stu-id="22e83-384">Verify that there is an **Active Deployment** with your *&quot;Initial Commit&quot;*.</span></span>

    ![アクティブな展開](maintainable-azure-websites-managing-change-and-scale/_static/image40.png)

    <span data-ttu-id="22e83-386">*アクティブな展開*</span><span class="sxs-lookup"><span data-stu-id="22e83-386">*Active deployment*</span></span>
13. <span data-ttu-id="22e83-387">最後に、をクリックして**参照**web アプリに移動して、コマンド バーにします。</span><span class="sxs-lookup"><span data-stu-id="22e83-387">Finally, click **Browse** in the command bar to go to the web app.</span></span>

    ![Web アプリを参照します。](maintainable-azure-websites-managing-change-and-scale/_static/image41.png)

    <span data-ttu-id="22e83-389">*Web アプリを参照します。*</span><span class="sxs-lookup"><span data-stu-id="22e83-389">*Browse web app*</span></span>
14. <span data-ttu-id="22e83-390">アプリケーションが正常に展開されている場合は、マニア Quiz ログイン ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="22e83-390">If the application is successfully deployed, you will see the Geek Quiz login page.</span></span>

    > [!NOTE]
    > <span data-ttu-id="22e83-391">展開済みアプリケーションのアドレスの URL には、続けて、web アプリの名前が含まれています。 *-ステージング*です。</span><span class="sxs-lookup"><span data-stu-id="22e83-391">The address URL of the deployed application contains the name of your web app followed by *-staging*.</span></span>

    ![ステージング環境で実行されるアプリケーション](maintainable-azure-websites-managing-change-and-scale/_static/image42.png)

    <span data-ttu-id="22e83-393">*ステージング環境で実行されるアプリケーション*</span><span class="sxs-lookup"><span data-stu-id="22e83-393">*Application running in the staging environment*</span></span>
15. <span data-ttu-id="22e83-394">アプリケーションを調査する場合は、クリックして**登録**新しいユーザーを登録します。</span><span class="sxs-lookup"><span data-stu-id="22e83-394">If you wish to explore the application, click **Register** to register a new user.</span></span> <span data-ttu-id="22e83-395">ユーザー名、電子メール アドレスとパスワードを入力して、アカウントの詳細を完了します。</span><span class="sxs-lookup"><span data-stu-id="22e83-395">Complete the account details by entering a user name, email address and password.</span></span> <span data-ttu-id="22e83-396">次に、アプリケーションは、クイズの最初の質問を示します。</span><span class="sxs-lookup"><span data-stu-id="22e83-396">Next, the application shows the first question of the quiz.</span></span> <span data-ttu-id="22e83-397">期待どおりに機能を確認するいくつかの質問に回答します。</span><span class="sxs-lookup"><span data-stu-id="22e83-397">Answer a few questions to make sure it is working as expected.</span></span>

    ![使用できるようにアプリケーション](maintainable-azure-websites-managing-change-and-scale/_static/image43.png)

    <span data-ttu-id="22e83-399">*使用できるようにアプリケーション*</span><span class="sxs-lookup"><span data-stu-id="22e83-399">*Application ready to be used*</span></span>

<a id="Ex2Task4"></a>
#### <a name="task-4--promoting-the-web-app-to-production"></a><span data-ttu-id="22e83-400">タスク 4-Web アプリを実稼働に昇格します。</span><span class="sxs-lookup"><span data-stu-id="22e83-400">Task 4 – Promoting the Web App to Production</span></span>

<span data-ttu-id="22e83-401">これで、ステージング環境で web アプリが正しく動作していることを確認している実稼働環境に昇格する準備ができます。</span><span class="sxs-lookup"><span data-stu-id="22e83-401">Now that you have verified that the web app is working correctly in the staging environment, you are ready to promote it to production.</span></span> <span data-ttu-id="22e83-402">このタスクでは、運用サイト スロットとステージング サイト スロットをスワップします。</span><span class="sxs-lookup"><span data-stu-id="22e83-402">In this task, you will swap the staging site slot with the production site slot.</span></span>

1. <span data-ttu-id="22e83-403">管理ポータルに戻り、ステージング サイト スロットを選択します。</span><span class="sxs-lookup"><span data-stu-id="22e83-403">Go back to the management portal and select the staging site slot.</span></span> <span data-ttu-id="22e83-404">をクリックして**スワップ**コマンド バーでします。</span><span class="sxs-lookup"><span data-stu-id="22e83-404">Click **Swap** in the command bar.</span></span>

    ![実稼働環境にスワップします。](maintainable-azure-websites-managing-change-and-scale/_static/image44.png)

    <span data-ttu-id="22e83-406">*実稼働環境にスワップします。*</span><span class="sxs-lookup"><span data-stu-id="22e83-406">*Swap to production*</span></span>
2. <span data-ttu-id="22e83-407">をクリックして**はい**確認ダイアログ ボックスでのスワップ操作を続行します。</span><span class="sxs-lookup"><span data-stu-id="22e83-407">Click **Yes** in the confirmation dialog box to proceed with the swap operation.</span></span> <span data-ttu-id="22e83-408">Azure は、ステージング サイトのコンテンツを持つ運用サイトのコンテンツをすぐに交換されます。</span><span class="sxs-lookup"><span data-stu-id="22e83-408">Azure will immediately swap the content of the production site with the content of the staging site.</span></span>

    > [!NOTE]
    > <span data-ttu-id="22e83-409">(例: 接続文字列の上書き、ハンドラー マッピングなど) の実稼働バージョンには、ステージングされたバージョンからのいくつかの設定が自動的にコピーされますが、(例: DNS エンドポイント、SSL のバインドなど)、その他の設定は変更されません。</span><span class="sxs-lookup"><span data-stu-id="22e83-409">Some settings from the staged version will automatically be copied to the production version (e.g. connection string overrides, handler mappings, etc.) but other settings will not change (e.g. DNS endpoints, SSL bindings, etc.).</span></span>

    ![スワップ操作を確認します。](maintainable-azure-websites-managing-change-and-scale/_static/image45.png)

    <span data-ttu-id="22e83-411">*スワップ操作を確認します。*</span><span class="sxs-lookup"><span data-stu-id="22e83-411">*Confirming swap operation*</span></span>
3. <span data-ttu-id="22e83-412">スワップの完了後、運用スロットを選択し、クリックして**参照**コマンド バーを開くには、実稼働サイトでします。</span><span class="sxs-lookup"><span data-stu-id="22e83-412">Once the swap is complete, select the production slot and click **Browse** in the command bar to open the production site.</span></span> <span data-ttu-id="22e83-413">アドレス バーの URL に注意してください。</span><span class="sxs-lookup"><span data-stu-id="22e83-413">Notice the URL in the address bar.</span></span>

    > [!NOTE]
    > <span data-ttu-id="22e83-414">キャッシュをクリアするブラウザーを更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="22e83-414">You might need to refresh your browser to clear cache.</span></span> <span data-ttu-id="22e83-415">Internet Explorer で、これを行うキーを押して**CTRL + R**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-415">In Internet Explorer, you can do this by pressing **CTRL+R**.</span></span>

    ![実稼働環境で実行されている web アプリ](maintainable-azure-websites-managing-change-and-scale/_static/image46.png)
4. <span data-ttu-id="22e83-417">**GitBash**コンソールで、運用スロットを対象とするローカルの Git リポジトリ用のリモート URL を更新します。</span><span class="sxs-lookup"><span data-stu-id="22e83-417">In the **GitBash** console, update the remote URL for the local Git repository to target the production slot.</span></span> <span data-ttu-id="22e83-418">これを行うには、展開ユーザー名と web アプリの名前のプレース ホルダーに置き換えて、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="22e83-418">To do this, run the following command replacing the placeholders with your deployment username and the name of your web app.</span></span>

    > [!NOTE]
    > <span data-ttu-id="22e83-419">次の演習では、ステージング、ラボのわかりやすくするためだけではなく運用サイトに変更をプッシュします。</span><span class="sxs-lookup"><span data-stu-id="22e83-419">In the following exercises, you will push changes to the production site instead of staging just for the simplicity of the lab.</span></span> <span data-ttu-id="22e83-420">実際のシナリオで、実稼働環境に昇格する前にステージング環境の変更を確認することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="22e83-420">In a real-world scenario, it is recommended to verify the changes in the staging environment before promoting to production.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample13.cmd)]

<a id="Exercise3"></a>
### <a name="exercise-3-performing-deployment-rollback-in-production"></a><span data-ttu-id="22e83-421">手順 3: 実稼働環境で展開のロールバックを実行します。</span><span class="sxs-lookup"><span data-stu-id="22e83-421">Exercise 3: Performing Deployment Rollback in Production</span></span>

<span data-ttu-id="22e83-422">場所がないステージングと運用などの間でホット スワップを実行するステージング スロットで作業している場合のシナリオがある**空き**または**Shared**モード。</span><span class="sxs-lookup"><span data-stu-id="22e83-422">There are scenarios where you do not have a staging slot to perform hot swap between staging and production, for example, if you are working with **Free** or **Shared** mode.</span></span> <span data-ttu-id="22e83-423">そのようなシナリオではアプリケーションをテストするテスト環境で – ローカルまたはリモート サイトに – 実稼働環境に展開する前にします。</span><span class="sxs-lookup"><span data-stu-id="22e83-423">In those scenarios, you should test your application in a testing environment –either locally or in a remote site– before deploying to production.</span></span> <span data-ttu-id="22e83-424">ただし、可能であればテスト フェーズ中に検出されない問題が実稼働サイトで発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="22e83-424">However, it is possible that an issue not detected during the testing phase may arise in the production site.</span></span> <span data-ttu-id="22e83-425">この場合、可能な限り早く前およびより安定したバージョンのアプリケーションに切り替える簡単にするためのメカニズムを理解しておくことができます。</span><span class="sxs-lookup"><span data-stu-id="22e83-425">In this case, it is important to have a mechanism to easily switch to a previous and more stable version of the application as quickly as possible.</span></span>

<span data-ttu-id="22e83-426">**Azure App Service**、ソース管理からの継続的なデプロイが可能なこのおかげ、**再展開**管理ポータルで使用できるアクション。</span><span class="sxs-lookup"><span data-stu-id="22e83-426">In **Azure App Service**, continuous deployment from source control makes this possible thanks to the **redeploy** action available in the management portal.</span></span> <span data-ttu-id="22e83-427">Azure では、リポジトリにプッシュされたコミットに関連付けられている展開を追跡し、いつでも、以前の展開のいずれかを使用してアプリケーションを再展開するためのオプションを提供します。</span><span class="sxs-lookup"><span data-stu-id="22e83-427">Azure keeps track of the deployments associated with the commits pushed to the repository and provides an option to redeploy your application using any of your previous deployments, at any time.</span></span>

<span data-ttu-id="22e83-428">この演習では、コードに変更を実行します、**マニア Quiz**意図的に挿入するアプリケーション、*バグ*です。</span><span class="sxs-lookup"><span data-stu-id="22e83-428">In this exercise you will perform a change to the code in the **Geek Quiz** application that intentionally injects a *bug*.</span></span> <span data-ttu-id="22e83-429">実稼働環境にアプリケーションが、エラーを展開して、し、前の状態に戻るに再展開機能を利用するためです。</span><span class="sxs-lookup"><span data-stu-id="22e83-429">You will deploy the application to production to see the error, and then you will take advantage of the redeploy feature to go back to the previous state.</span></span>

<a id="Ex3Task1"></a>
#### <a name="task-1--updating-the-geek-quiz-application"></a><span data-ttu-id="22e83-430">作業 1: マニア クイズ アプリケーションの更新</span><span class="sxs-lookup"><span data-stu-id="22e83-430">Task 1 – Updating the Geek Quiz Application</span></span>

<span data-ttu-id="22e83-431">このタスクで小規模のコードをリファクタリングが、 **TriviaController**データベースから新しいメソッドに、選択したクイズ オプションを取得するロジックの一部を抽出するクラス。</span><span class="sxs-lookup"><span data-stu-id="22e83-431">In this task, you will refactor a small piece of code of the **TriviaController** class to extract part of the logic that retrieves the selected quiz option from the database into a new method.</span></span>

1. <span data-ttu-id="22e83-432">Visual Studio のインスタンスに切り替え、 **GeekQuiz**前の手順からソリューションです。</span><span class="sxs-lookup"><span data-stu-id="22e83-432">Switch to the Visual Studio instance with the **GeekQuiz** solution from the previous exercise.</span></span>
2. <span data-ttu-id="22e83-433">**ソリューション エクスプ ローラー**を開き、 **TriviaController.cs**内のファイル、**コント ローラー**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="22e83-433">In **Solution Explorer**, open the **TriviaController.cs** file inside the **Controllers** folder.</span></span>
3. <span data-ttu-id="22e83-434">検索、 **StoreAsync**メソッドと、コードが次の図で強調表示を選択します。</span><span class="sxs-lookup"><span data-stu-id="22e83-434">Locate the **StoreAsync** method and select the code highlighted in the following figure.</span></span>

    ![コードを選択します。](maintainable-azure-websites-managing-change-and-scale/_static/image47.png)

    <span data-ttu-id="22e83-436">*コードを選択します。*</span><span class="sxs-lookup"><span data-stu-id="22e83-436">*Selecting the code*</span></span>
4. <span data-ttu-id="22e83-437">選択したコードを右クリックし、展開、**リファクター**メニュー**メソッドの抽出しています.**.</span><span class="sxs-lookup"><span data-stu-id="22e83-437">Right-click the selected code, expand the **Refactor** menu and select **Extract Method...**.</span></span>

    ![新しいメソッドとして、コードを抽出します。](maintainable-azure-websites-managing-change-and-scale/_static/image48.png)

    <span data-ttu-id="22e83-439">*Extract メソッドを選択します。*</span><span class="sxs-lookup"><span data-stu-id="22e83-439">*Selecting Extract Method*</span></span>
5. <span data-ttu-id="22e83-440">**メソッドの抽出** ダイアログ ボックスに、名前は新しいメソッド*MatchesOption*  をクリック**OK**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-440">In the **Extract Method** dialog box, name the new method *MatchesOption* and click **OK**.</span></span>

    ![メソッド名を指定します。](maintainable-azure-websites-managing-change-and-scale/_static/image49.png)

    <span data-ttu-id="22e83-442">*抽出されたメソッドの名前を指定します。*</span><span class="sxs-lookup"><span data-stu-id="22e83-442">*Specifying the name for the extracted method*</span></span>
6. <span data-ttu-id="22e83-443">選択したコードに抽出し、 **MatchesOption**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="22e83-443">The selected code is then extracted into the **MatchesOption** method.</span></span> <span data-ttu-id="22e83-444">結果のコードは、次のスニペットに表示されます。</span><span class="sxs-lookup"><span data-stu-id="22e83-444">The resulting code is shown in the following snippet.</span></span>

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample14.cs)]
7. <span data-ttu-id="22e83-445">キーを押して**ctrl キーを押しながら S**変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="22e83-445">Press **CTRL + S** to save the changes.</span></span>

<a id="Ex3Task2"></a>
#### <a name="task-2--redeploying-the-geek-quiz-application"></a><span data-ttu-id="22e83-446">タスク 2-マニア クイズ アプリケーションを再配置</span><span class="sxs-lookup"><span data-stu-id="22e83-446">Task 2 – Redeploying the Geek Quiz Application</span></span>

<span data-ttu-id="22e83-447">運用環境に新しい展開をトリガーする、リポジトリに前のタスクで加えた変更をプッシュするようになりました。</span><span class="sxs-lookup"><span data-stu-id="22e83-447">You will now push the changes you made in the previous task to the repository, which will trigger a new deployment to the production environment.</span></span> <span data-ttu-id="22e83-448">次を使用して、問題のトラブルシューティングは、 **F12 開発ツール**Internet Explorer によって提供され、Azure 管理ポータルから、以前のデプロイをロールバックを実行します。</span><span class="sxs-lookup"><span data-stu-id="22e83-448">Then, you will troubleshot an issue using the **F12 development tools** provided by Internet Explorer, and then perform a rollback to the previous deployment from the Azure management portal.</span></span>

1. <span data-ttu-id="22e83-449">新しく開きます**Git Bash** Azure App Service に更新されたアプリケーションを配置するコンソール。</span><span class="sxs-lookup"><span data-stu-id="22e83-449">Open a new **Git Bash** console to deploy the updated application to Azure App Service.</span></span>
2. <span data-ttu-id="22e83-450">変更を Azure にプッシュするには、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="22e83-450">Execute the following commands to push the changes to Azure.</span></span> <span data-ttu-id="22e83-451">更新プログラム、 *[、アプリケーション パス]*へのパスのプレース ホルダー、 **GeekQuiz**ソリューションです。</span><span class="sxs-lookup"><span data-stu-id="22e83-451">Update the *[YOUR-APPLICATION-PATH]* placeholder with the path to the **GeekQuiz** solution.</span></span> <span data-ttu-id="22e83-452">展開パスワードを求められます。</span><span class="sxs-lookup"><span data-stu-id="22e83-452">You will be prompted for your deployment password.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample15.cmd)]

    ![リファクタリングされたコードを Azure にプッシュします。](maintainable-azure-websites-managing-change-and-scale/_static/image50.png)

    <span data-ttu-id="22e83-454">*リファクタリングされたコードを Azure にプッシュします。*</span><span class="sxs-lookup"><span data-stu-id="22e83-454">*Pushing refactored code to Azure*</span></span>
3. <span data-ttu-id="22e83-455">Internet Explorer を開き、web アプリに移動します (例: `http://<your-web-site>.azurewebsites.net`)。</span><span class="sxs-lookup"><span data-stu-id="22e83-455">Open Internet Explorer and navigate to your web app (e.g. `http://<your-web-site>.azurewebsites.net`).</span></span> <span data-ttu-id="22e83-456">以前に作成した資格情報を使用してをログインします。</span><span class="sxs-lookup"><span data-stu-id="22e83-456">Log in using the previously created credentials.</span></span>
4. <span data-ttu-id="22e83-457">キーを押して**F12**開発ツールを起動するには、次のように選択します。、**ネットワーク** タブで  をクリックし、**再生**記録を開始するボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="22e83-457">Press **F12** to launch the development tools, select the **Network** tab and click the **Play** button to start recording.</span></span>

    <span data-ttu-id="22e83-458">![ネットワークの記録を開始](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "ネットワークの記録を開始")</span><span class="sxs-lookup"><span data-stu-id="22e83-458">![Starting network recording](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "Starting network recording")</span></span>

    <span data-ttu-id="22e83-459">*ネットワークの記録を開始*</span><span class="sxs-lookup"><span data-stu-id="22e83-459">*Starting network recording*</span></span>
5. <span data-ttu-id="22e83-460">クイズの任意のオプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="22e83-460">Select any option of the quiz.</span></span> <span data-ttu-id="22e83-461">何も起こらないことが表示されます。</span><span class="sxs-lookup"><span data-stu-id="22e83-461">You will see that nothing happens.</span></span>
6. <span data-ttu-id="22e83-462">**F12**ウィンドウで、POST HTTP 要求に対応するエントリを示しています、HTTP **500**結果。</span><span class="sxs-lookup"><span data-stu-id="22e83-462">In the **F12** window, the entry corresponding to the POST HTTP request shows an HTTP **500** result.</span></span>

    ![HTTP 500 エラー](maintainable-azure-websites-managing-change-and-scale/_static/image52.png)

    <span data-ttu-id="22e83-464">*HTTP 500 エラー*</span><span class="sxs-lookup"><span data-stu-id="22e83-464">*HTTP 500 error*</span></span>
7. <span data-ttu-id="22e83-465">選択、**コンソール**タブです。エラーが原因の詳細を記録します。</span><span class="sxs-lookup"><span data-stu-id="22e83-465">Select the **Console** tab. An error is logged with the details of the cause.</span></span>

    ![ログに記録されたエラー](maintainable-azure-websites-managing-change-and-scale/_static/image53.png)

    <span data-ttu-id="22e83-467">*ログに記録されたエラー*</span><span class="sxs-lookup"><span data-stu-id="22e83-467">*Logged error*</span></span>
8. <span data-ttu-id="22e83-468">エラーの詳細部分を探します。</span><span class="sxs-lookup"><span data-stu-id="22e83-468">Locate the details part of the error.</span></span> <span data-ttu-id="22e83-469">明確に、このエラーはリファクタリング前の手順でコミットするコードによって発生します。</span><span class="sxs-lookup"><span data-stu-id="22e83-469">Clearly, this error is caused by the code refactoring you committed in the previous steps.</span></span>

    <span data-ttu-id="22e83-470">`Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`。</span><span class="sxs-lookup"><span data-stu-id="22e83-470">`Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`.</span></span>
9. <span data-ttu-id="22e83-471">ブラウザーを閉じないでください。</span><span class="sxs-lookup"><span data-stu-id="22e83-471">Do not close the browser.</span></span>
10. <span data-ttu-id="22e83-472">新しいブラウザー インスタンスでは、に移動、 [Azure 管理ポータル](https://manage.windowsazure.com)サブスクリプションに関連付けられている Microsoft アカウントを使用してサインインします。</span><span class="sxs-lookup"><span data-stu-id="22e83-472">In a new browser instance, navigate to the [Azure management portal](https://manage.windowsazure.com) and sign in using the Microsoft account associated with your subscription.</span></span>
11. <span data-ttu-id="22e83-473">選択**web サイト**手順 2 で作成した web アプリケーション をクリックします。</span><span class="sxs-lookup"><span data-stu-id="22e83-473">Select **Websites** and click the web app you created in Exercise 2.</span></span>
12. <span data-ttu-id="22e83-474">移動し、**展開**ページ。</span><span class="sxs-lookup"><span data-stu-id="22e83-474">Navigate to the **Deployments** page.</span></span> <span data-ttu-id="22e83-475">実行されるすべてのコミットが、デプロイ履歴に表示されていることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="22e83-475">Notice that all the commits performed are listed in the deployment history.</span></span>

    ![既存の展開の一覧](maintainable-azure-websites-managing-change-and-scale/_static/image54.png)

    <span data-ttu-id="22e83-477">*既存の展開の一覧*</span><span class="sxs-lookup"><span data-stu-id="22e83-477">*List of existing deployments*</span></span>
13. <span data-ttu-id="22e83-478">前回のコミットを選択し、クリックして**再展開**コマンド バーでします。</span><span class="sxs-lookup"><span data-stu-id="22e83-478">Select the previous commit and click **Redeploy** on the command bar.</span></span>

    ![前回のコミットを再配置](maintainable-azure-websites-managing-change-and-scale/_static/image55.png)

    <span data-ttu-id="22e83-480">*前回のコミットを再配置*</span><span class="sxs-lookup"><span data-stu-id="22e83-480">*Redeploying the previous commit*</span></span>
14. <span data-ttu-id="22e83-481">確認するメッセージが表示されたら、クリックして**はい**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-481">When prompted to confirm, click **Yes**.</span></span>

    ![再展開を確認します。](maintainable-azure-websites-managing-change-and-scale/_static/image56.png)
15. <span data-ttu-id="22e83-483">展開が完了したら、web アプリとキーを押してブラウザー インスタンスに切り替える**ctrl キーを押しながら f5 キーを押して**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-483">When the deployment completes, switch back to the browser instance with your web app and press **CTRL + F5**.</span></span>
16. <span data-ttu-id="22e83-484">任意のオプションをクリックします。</span><span class="sxs-lookup"><span data-stu-id="22e83-484">Click any of the options.</span></span> <span data-ttu-id="22e83-485">フリップ アニメーション処理を実行し、結果 (*解決と不適切な*) が表示されます。</span><span class="sxs-lookup"><span data-stu-id="22e83-485">The flip animation will now take place and the result (*correct/incorrect*) will be displayed.</span></span>
17. <span data-ttu-id="22e83-486">(省略可能)切り替えて、 **Git Bash**コンソールし、前回のコミットに戻すには、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="22e83-486">(Optional) Switch to the **Git Bash** console and execute the following commands to revert to the previous commit.</span></span>

    > [!NOTE]
    > <span data-ttu-id="22e83-487">これらのコマンドは、Git リポジトリに不適切なコミットで行われたすべての変更を元に戻す新しい、コミットを作成します。</span><span class="sxs-lookup"><span data-stu-id="22e83-487">These commands create a new commit that undoes all changes in the Git repository that were made in the bad commit.</span></span> <span data-ttu-id="22e83-488">Azure は、新しい、コミットを使用して、アプリケーションとし、再展開します。</span><span class="sxs-lookup"><span data-stu-id="22e83-488">Azure will then redeploy the application using the new commit.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample16.cmd)]

<a id="Exercise4"></a>
### <a name="exercise-4-scaling-using-azure-storage"></a><span data-ttu-id="22e83-489">手順 4: スケールが Azure ストレージを使用します。</span><span class="sxs-lookup"><span data-stu-id="22e83-489">Exercise 4: Scaling Using Azure Storage</span></span>

<span data-ttu-id="22e83-490">**Blob**は、大量の非構造化テキストまたはビデオ、オーディオ、画像などのバイナリ データを格納する最も簡単な方法です。</span><span class="sxs-lookup"><span data-stu-id="22e83-490">**Blobs** are the simplest way to store large amounts of unstructured text or binary data such as video, audio and images.</span></span> <span data-ttu-id="22e83-491">記憶域にアプリの静的コンテンツを移動するにはのイメージまたはブラウザーに直接ドキュメントを使用して、アプリケーションの規模に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="22e83-491">Moving the static content of your application to Storage, helps to scale your application by serving images or documents directly to the browser.</span></span>

<span data-ttu-id="22e83-492">この練習では、Blob コンテナーに、アプリケーションの静的なコンテンツを移動します。</span><span class="sxs-lookup"><span data-stu-id="22e83-492">In this exercise, you will move the static content of your application to a Blob container.</span></span> <span data-ttu-id="22e83-493">追加するアプリケーションを構成するは、 **ASP.NET URL 書き換えルール**で、 **Web.config** Blob コンテナーにコンテンツをリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="22e83-493">Then you will configure your application to add an **ASP.NET URL rewrite rule** in the **Web.config** to redirect your content to the Blob container.</span></span>

<a id="Ex4Task1"></a>
#### <a name="task-1--creating-an-azure-storage-account"></a><span data-ttu-id="22e83-494">作業 1: Azure ストレージ アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="22e83-494">Task 1 – Creating an Azure Storage Account</span></span>

<span data-ttu-id="22e83-495">このタスクでは、管理ポータルを使用して新しいストレージ アカウントを作成する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="22e83-495">In this task you will learn how to create a new storage account using the management portal.</span></span>

1. <span data-ttu-id="22e83-496">移動し、 [Azure 管理ポータル](https://manage.windowsazure.com)サブスクリプションに関連付けられている Microsoft アカウントを使用してサインインします。</span><span class="sxs-lookup"><span data-stu-id="22e83-496">Navigate to the [Azure management portal](https://manage.windowsazure.com) and sign in using the Microsoft account associated with your subscription.</span></span>
2. <span data-ttu-id="22e83-497">選択**新しい |データ サービス |記憶域 |簡易作成**新しいストレージ アカウントの作成を開始します。</span><span class="sxs-lookup"><span data-stu-id="22e83-497">Select **New | Data Services | Storage | Quick Create** to start creating a new storage account.</span></span> <span data-ttu-id="22e83-498">クリックし、アカウントの一意の名前を入力、**地域**一覧からです。</span><span class="sxs-lookup"><span data-stu-id="22e83-498">Enter a unique name for the account and select a **Region** from the list.</span></span> <span data-ttu-id="22e83-499">をクリックして**ストレージ アカウントの作成**を続行します。</span><span class="sxs-lookup"><span data-stu-id="22e83-499">Click **Create Storage Account** to continue.</span></span>

    <span data-ttu-id="22e83-500">![新しいストレージ アカウントを作成する](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "新しいストレージ アカウントを作成します。")</span><span class="sxs-lookup"><span data-stu-id="22e83-500">![Creating a new Storage Account](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "Creating a new Storage Account")</span></span>

    <span data-ttu-id="22e83-501">*新しいストレージ アカウントを作成します。*</span><span class="sxs-lookup"><span data-stu-id="22e83-501">*Creating a new storage account*</span></span>
3. <span data-ttu-id="22e83-502">**ストレージ**セクションで、新しいストレージ アカウントの状態に変更されるまで待機*オンライン*次の手順を続行するためにします。</span><span class="sxs-lookup"><span data-stu-id="22e83-502">In the **Storage** section, wait until the status of the new storage account changes to *Online* in order to continue with the following step.</span></span>

    <span data-ttu-id="22e83-503">![作成されたストレージ アカウント](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "ストレージ アカウントの作成")</span><span class="sxs-lookup"><span data-stu-id="22e83-503">![Storage Account created](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "Storage Account created")</span></span>

    <span data-ttu-id="22e83-504">*ストレージ アカウントの作成*</span><span class="sxs-lookup"><span data-stu-id="22e83-504">*Storage Account created*</span></span>
4. <span data-ttu-id="22e83-505">ストレージ アカウント名をクリックし、をクリックして、**ダッシュ ボード**ページの上部にリンクします。</span><span class="sxs-lookup"><span data-stu-id="22e83-505">Click on the storage account name and then click the **Dashboard** link at the top of the page.</span></span> <span data-ttu-id="22e83-506">**ダッシュ ボード** ページでは、アカウントと、アプリケーション内で使用できるサービスのエンドポイントの状態に関する情報。</span><span class="sxs-lookup"><span data-stu-id="22e83-506">The **Dashboard** page provides you with information about the status of the account and the service endpoints that can be used within your applications.</span></span>

    <span data-ttu-id="22e83-507">![ストレージ アカウント ダッシュ ボードを表示する](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "ストレージ アカウント ダッシュ ボードを表示します。")</span><span class="sxs-lookup"><span data-stu-id="22e83-507">![Displaying the Storage Account Dashboard](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "Displaying the Storage Account Dashboard")</span></span>

    <span data-ttu-id="22e83-508">*ストレージ アカウント ダッシュ ボードを表示します。*</span><span class="sxs-lookup"><span data-stu-id="22e83-508">*Displaying the Storage Account Dashboard*</span></span>
5. <span data-ttu-id="22e83-509">クリックして、**アクセス キーの管理**ナビゲーション バーで、ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="22e83-509">Click the **Manage Access Keys** button in the navigation bar.</span></span>

    <span data-ttu-id="22e83-510">![管理アクセス キー ボタン](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "アクセス キーの管理 ボタン")</span><span class="sxs-lookup"><span data-stu-id="22e83-510">![Manage Access Keys button](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "Manage Access Keys button")</span></span>

    <span data-ttu-id="22e83-511">*管理アクセス キー ボタン*</span><span class="sxs-lookup"><span data-stu-id="22e83-511">*Manage Access Keys button*</span></span>
6. <span data-ttu-id="22e83-512">**アクセス キーの管理**ダイアログ ボックスで、コピー、**ストレージ アカウント名**と**プライマリ アクセス キー**ように次の手順で必要となります。</span><span class="sxs-lookup"><span data-stu-id="22e83-512">In the **Manage Access Keys** dialog box, copy the **Storage Account Name** and **Primary Access Key** as you will need them in the following exercise.</span></span> <span data-ttu-id="22e83-513">次に、ダイアログ ボックスを閉じます。</span><span class="sxs-lookup"><span data-stu-id="22e83-513">Then, close the dialog box.</span></span>

    <span data-ttu-id="22e83-514">![アクセス キーの管理 ダイアログ ボックス](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "アクセス キーの管理 ダイアログ ボックス")</span><span class="sxs-lookup"><span data-stu-id="22e83-514">![Manage Access Key dialog box](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "Manage Access Key dialog box")</span></span>

    <span data-ttu-id="22e83-515">*管理アクセス キー ダイアログ ボックス*</span><span class="sxs-lookup"><span data-stu-id="22e83-515">*Manage Access Key dialog box*</span></span>

<a id="Ex4Task2"></a>
#### <a name="task-2--uploading-an-asset-to-azure-blob-storage"></a><span data-ttu-id="22e83-516">タスク 2: Azure Blob ストレージに資産をアップロードします。</span><span class="sxs-lookup"><span data-stu-id="22e83-516">Task 2 – Uploading an Asset to Azure Blob Storage</span></span>

<span data-ttu-id="22e83-517">このタスクでは、Visual Studio からサーバー エクスプ ローラー ウィンドウを使用して、ストレージ アカウントへの接続にします。</span><span class="sxs-lookup"><span data-stu-id="22e83-517">In this task, you will use the Server Explorer window from Visual Studio to connect to your storage account.</span></span> <span data-ttu-id="22e83-518">Blob コンテナーを作成し、マニア Quiz ロゴ ファイルをコンテナーにアップロードされます。</span><span class="sxs-lookup"><span data-stu-id="22e83-518">You will then create a blob container and upload a file with the Geek Quiz logo to the container.</span></span>

1. <span data-ttu-id="22e83-519">Visual Studio のインスタンスに切り替え、 **GeekQuiz**前の手順からソリューションです。</span><span class="sxs-lookup"><span data-stu-id="22e83-519">Switch to the Visual Studio instance with the **GeekQuiz** solution from the previous exercise.</span></span>
2. <span data-ttu-id="22e83-520">メニュー バーから、選択**ビュー**  をクリックし、**サーバー エクスプ ローラー**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-520">From the menu bar, select **View** and then click **Server Explorer**.</span></span>
3. <span data-ttu-id="22e83-521">**サーバー エクスプ ローラー**を右クリックし、 **Azure**ノード**を Azure に接続しています.**.お客様のサブスクリプションに関連付けられている Microsoft アカウントを使用してサインインします。</span><span class="sxs-lookup"><span data-stu-id="22e83-521">In **Server Explorer**, right-click the **Azure** node and select **Connect to Azure...**. Sign in using the Microsoft account associated with your subscription.</span></span>

    ![Windows Azure への接続します。](maintainable-azure-websites-managing-change-and-scale/_static/image62.png)

    <span data-ttu-id="22e83-523">*Azure への接続します。*</span><span class="sxs-lookup"><span data-stu-id="22e83-523">*Connect to Azure*</span></span>
4. <span data-ttu-id="22e83-524">展開、 **Azure**  ノードを右クリックして**ストレージ**を選択し、**外部の記憶域をアタッチしています.**.</span><span class="sxs-lookup"><span data-stu-id="22e83-524">Expand the **Azure** node, right-click **Storage** and select **Attach External Storage...**.</span></span>
5. <span data-ttu-id="22e83-525">**新しいストレージ アカウントの追加** ダイアログ ボックスで、入力、**アカウント名**と**アカウント キー**をクリックして、前のタスクで取得した**OK**.</span><span class="sxs-lookup"><span data-stu-id="22e83-525">In the **Add New Storage Account** dialog box, enter the **Account name** and **Account key** you obtained in the previous task and click **OK**.</span></span>

    ![新しいストレージ アカウント ダイアログ ボックスを追加します。](maintainable-azure-websites-managing-change-and-scale/_static/image63.png)

    <span data-ttu-id="22e83-527">*新しいストレージ アカウント ダイアログ ボックスを追加します。*</span><span class="sxs-lookup"><span data-stu-id="22e83-527">*Add New Storage Account dialog box*</span></span>
6. <span data-ttu-id="22e83-528">ストレージ アカウントが表示されるはずの**ストレージ**ノード。</span><span class="sxs-lookup"><span data-stu-id="22e83-528">Your storage account should appear under the **Storage** node.</span></span> <span data-ttu-id="22e83-529">ストレージ アカウントを展開しを右クリックして**Blob**選択**Blob コンテナーの作成しています.**.</span><span class="sxs-lookup"><span data-stu-id="22e83-529">Expand your storage account, right-click **Blobs** and select **Create Blob Container...**.</span></span>

    <span data-ttu-id="22e83-530">![Blob コンテナーを作成する](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "Blob コンテナーの作成")</span><span class="sxs-lookup"><span data-stu-id="22e83-530">![Create Blob Container](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "Create Blob Container")</span></span>

    <span data-ttu-id="22e83-531">*Blob コンテナーを作成します。*</span><span class="sxs-lookup"><span data-stu-id="22e83-531">*Create Blob Container*</span></span>
7. <span data-ttu-id="22e83-532">**Blob コンテナーの作成** ダイアログ ボックスで、blob コンテナーの名前を入力し、をクリックして**OK**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-532">In the **Create Blob Container** dialog box, enter a name for the blob container and click **OK**.</span></span>

    <span data-ttu-id="22e83-533">![作成する Blob コンテナー ダイアログ ボックス](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "Blob コンテナーの作成 ダイアログ ボックス")</span><span class="sxs-lookup"><span data-stu-id="22e83-533">![Create Blob Container dialog box](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "Create Blob Container dialog box")</span></span>

    <span data-ttu-id="22e83-534">*Blob コンテナー ダイアログ ボックスを作成します。*</span><span class="sxs-lookup"><span data-stu-id="22e83-534">*Create Blob Container dialog box*</span></span>
8. <span data-ttu-id="22e83-535">新しい blob コンテナーを追加する必要があります、 **Blob**ノード。</span><span class="sxs-lookup"><span data-stu-id="22e83-535">The new blob container should be added to the **Blobs** node.</span></span> <span data-ttu-id="22e83-536">コンテナーを公開して、コンテナーのアクセス許可を変更します。</span><span class="sxs-lookup"><span data-stu-id="22e83-536">Change the access permissions in the container to make the container public.</span></span> <span data-ttu-id="22e83-537">これを行うを右クリックし、**イメージ**コンテナーと選択**プロパティ**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-537">To do this, right-click the **images** container and select **Properties**.</span></span>

    <span data-ttu-id="22e83-538">![コンテナーのプロパティをイメージ](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "イメージのコンテナーのプロパティ")</span><span class="sxs-lookup"><span data-stu-id="22e83-538">![images container properties](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "images container properties")</span></span>

    <span data-ttu-id="22e83-539">*イメージ コンテナーのプロパティ*</span><span class="sxs-lookup"><span data-stu-id="22e83-539">*Images container properties*</span></span>
9. <span data-ttu-id="22e83-540">**プロパティ**ウィンドウで、設定、**パブリック読み取りアクセス**に**コンテナー**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-540">In the **Properties** window, set the **Public Read Access** to **Container**.</span></span>

    <span data-ttu-id="22e83-541">![パブリック読み取りアクセスのプロパティを変更する](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "読み取りパブリック アクセス プロパティを変更します。")</span><span class="sxs-lookup"><span data-stu-id="22e83-541">![Changing public read access property](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "Changing public read access property")</span></span>

    <span data-ttu-id="22e83-542">*パブリック読み取りアクセスのプロパティを変更します。*</span><span class="sxs-lookup"><span data-stu-id="22e83-542">*Changing public read access property*</span></span>
10. <span data-ttu-id="22e83-543">パブリック アクセス プロパティを変更するには、をクリックすることを確認する場合は入力を求められたら**はい**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-543">When prompted if you are sure you want to change the public access property, click **Yes**.</span></span>

    <span data-ttu-id="22e83-544">![Microsoft Visual Studio 警告](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "Microsoft Visual Studio の警告")</span><span class="sxs-lookup"><span data-stu-id="22e83-544">![Microsoft Visual Studio warning](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "Microsoft Visual Studio warning")</span></span>

    <span data-ttu-id="22e83-545">*Microsoft Visual Studio の警告*</span><span class="sxs-lookup"><span data-stu-id="22e83-545">*Microsoft Visual Studio warning*</span></span>
11. <span data-ttu-id="22e83-546">**サーバー エクスプ ローラー**、右クリックし、**イメージ**blob コンテナーを選択**Blob コンテナーの**します。</span><span class="sxs-lookup"><span data-stu-id="22e83-546">In **Server Explorer**, right-click in the **images** blob container and select **View Blob Container**.</span></span>

    <span data-ttu-id="22e83-547">![Blob コンテナーを表示](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "Blob コンテナーを表示")</span><span class="sxs-lookup"><span data-stu-id="22e83-547">![View Blob Container](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "View Blob Container")</span></span>

    <span data-ttu-id="22e83-548">*Blob コンテナーの表示*</span><span class="sxs-lookup"><span data-stu-id="22e83-548">*View Blob Container*</span></span>
12. <span data-ttu-id="22e83-549">イメージ コンテナーが新しいウィンドウで開く必要があり、エントリがない凡例を表示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="22e83-549">The images container should open in a new window and a legend with no entries should be shown.</span></span> <span data-ttu-id="22e83-550">クリックして、**アップロード**blob コンテナーにファイルをアップロードするにはアイコン。</span><span class="sxs-lookup"><span data-stu-id="22e83-550">Click the **upload** icon to upload a file to the blob container.</span></span>

    <span data-ttu-id="22e83-551">![エントリがないイメージ コンテナー](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "エントリがないイメージ コンテナー")</span><span class="sxs-lookup"><span data-stu-id="22e83-551">![Images container with no entries](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "Images container with no entries")</span></span>

    <span data-ttu-id="22e83-552">*エントリがないイメージ コンテナー*</span><span class="sxs-lookup"><span data-stu-id="22e83-552">*Images container with no entries*</span></span>
13. <span data-ttu-id="22e83-553">**Blob のアップロード** ダイアログ ボックスに移動、**資産**ラボのフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="22e83-553">In the **Upload Blob** dialog box, navigate to the **Assets** folder of the lab.</span></span> <span data-ttu-id="22e83-554">選択、**ロゴ big.png**ファイルし、をクリックして**開く**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-554">Select the **logo-big.png** file and click **Open**.</span></span>
14. <span data-ttu-id="22e83-555">ファイルがアップロードされるまで待ちます。</span><span class="sxs-lookup"><span data-stu-id="22e83-555">Wait until the file is uploaded.</span></span> <span data-ttu-id="22e83-556">アップロードが完了したら、ファイルをイメージのコンテナーに表示されるはずです。</span><span class="sxs-lookup"><span data-stu-id="22e83-556">When the upload completes, the file should be listed in the images container.</span></span> <span data-ttu-id="22e83-557">ファイルのエントリを右クリックし  **URL のコピー**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-557">Right-click the file entry and select **Copy URL**.</span></span>

    <span data-ttu-id="22e83-558">![Blob の URL をコピー](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "blob ファイルの URL をコピー")</span><span class="sxs-lookup"><span data-stu-id="22e83-558">![Copy blob URL](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "Copy blob file URL")</span></span>

    <span data-ttu-id="22e83-559">*Blob の URL をコピーします。*</span><span class="sxs-lookup"><span data-stu-id="22e83-559">*Copy blob URL*</span></span>
15. <span data-ttu-id="22e83-560">Internet Explorer を開き、URL を貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="22e83-560">Open Internet Explorer and paste the URL.</span></span> <span data-ttu-id="22e83-561">次の図は、ブラウザーに表示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="22e83-561">The following image should be shown in the browser.</span></span>

    <span data-ttu-id="22e83-562">![Windows の Blob ストレージからイメージをロゴ big.png](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "ストレージからロゴ big.png イメージ")</span><span class="sxs-lookup"><span data-stu-id="22e83-562">![logo-big.png image from Windows Blob Storage](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "logo-big.png image from storage")</span></span>

    <span data-ttu-id="22e83-563">*Azure Blob ストレージからロゴ big.png イメージ*</span><span class="sxs-lookup"><span data-stu-id="22e83-563">*logo-big.png image from Azure Blob Storage*</span></span>

<a id="Ex4Task3"></a>
#### <a name="task-3--updating-the-solution-to-consume-static-content-from-azure-blob-storage"></a><span data-ttu-id="22e83-564">タスク 3: Azure Blob ストレージからの静的なコンテンツを利用するソリューションの更新</span><span class="sxs-lookup"><span data-stu-id="22e83-564">Task 3 – Updating the Solution to Consume Static Content from Azure Blob Storage</span></span>

<span data-ttu-id="22e83-565">このタスクでは、構成、 **GeekQuiz**イメージを使用するソリューションにアップロードされた Azure Blob ストレージではなく web アプリにあるイメージ) で ASP.NET URL 書き換えルールを追加することによって、 **web.config**ファイル。</span><span class="sxs-lookup"><span data-stu-id="22e83-565">In this task, you will configure the **GeekQuiz** solution to consume the image uploaded to Azure Blob Storage (instead of the image located in the web app) by adding an ASP.NET URL rewrite rule in the **web.config** file.</span></span>

1. <span data-ttu-id="22e83-566">Visual Studio で開く、 **Web.config**内のファイル、 **GeekQuiz**プロジェクトし、検索、  **&lt;system.webServer&gt;** 要素。</span><span class="sxs-lookup"><span data-stu-id="22e83-566">In Visual Studio, open the **Web.config** file inside the **GeekQuiz** project and locate the **&lt;system.webServer&gt;** element.</span></span>
2. <span data-ttu-id="22e83-567">URL 書き換えルールをストレージ アカウント名のプレース ホルダーの更新を追加するには、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="22e83-567">Add the following code to add an URL rewrite rule, updating the placeholder with your storage account name.</span></span>

    <span data-ttu-id="22e83-568">(コード スニペットの*WebSitesInProduction - Ex4 - UrlRewriteRule*)</span><span class="sxs-lookup"><span data-stu-id="22e83-568">(Code Snippet - *WebSitesInProduction - Ex4 - UrlRewriteRule*)</span></span>

    [!code-xml[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample17.xml)]

    > [!NOTE]
    > <span data-ttu-id="22e83-569">URL の書き換えは、受信 Web 要求を受け取り、別のリソースへの要求のリダイレクトのプロセスです。</span><span class="sxs-lookup"><span data-stu-id="22e83-569">URL rewriting is the process of intercepting an incoming Web request and redirecting the request to a different resource.</span></span> <span data-ttu-id="22e83-570">URL 書き換えルールは、要求をリダイレクトする必要がある場合と、リダイレクト先にする、書き換えエンジンを指示します。</span><span class="sxs-lookup"><span data-stu-id="22e83-570">The URL rewriting rules tells the rewriting engine when a request needs to be redirected, and where should they be redirected.</span></span> <span data-ttu-id="22e83-571">書き換え規則が 2 つの文字列で構成されます: 要求された URL 内で検索するパターン (通常は、正規表現を使用して) 場合は、そのパターンを置換する文字列が検出されたとします。</span><span class="sxs-lookup"><span data-stu-id="22e83-571">A rewriting rule is composed of two strings: the pattern to look for in the requested URL (usually, using regular expressions), and the string to replace the pattern with, if found.</span></span> <span data-ttu-id="22e83-572">詳細については、次を参照してください。 [ASP.NET の URL の書き換え](https://msdn.microsoft.com/library/ms972974.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="22e83-572">For more information, see [URL Rewriting in ASP.NET](https://msdn.microsoft.com/library/ms972974.aspx).</span></span>
3. <span data-ttu-id="22e83-573">キーを押して**ctrl キーを押しながら S**変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="22e83-573">Press **CTRL + S** to save the changes.</span></span>
4. <span data-ttu-id="22e83-574">新しく開きます**Git Bash** Azure App Service に更新されたアプリケーションを配置するコンソール。</span><span class="sxs-lookup"><span data-stu-id="22e83-574">Open a new **Git Bash** console to deploy the updated application to Azure App Service.</span></span>
5. <span data-ttu-id="22e83-575">変更を Azure にプッシュするには、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="22e83-575">Execute the following commands to push the changes to Azure.</span></span> <span data-ttu-id="22e83-576">更新プログラム、 *[、アプリケーション パス]*へのパスのプレース ホルダー、 **GeekQuiz**ソリューションです。</span><span class="sxs-lookup"><span data-stu-id="22e83-576">Update the *[YOUR-APPLICATION-PATH]* placeholder with the path to the **GeekQuiz** solution.</span></span> <span data-ttu-id="22e83-577">展開パスワードを求められます。</span><span class="sxs-lookup"><span data-stu-id="22e83-577">You will be prompted for your deployment password.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample18.cmd)]

    ![Azure への更新の展開](maintainable-azure-websites-managing-change-and-scale/_static/image73.png)

    <span data-ttu-id="22e83-579">*Azure への更新の展開*</span><span class="sxs-lookup"><span data-stu-id="22e83-579">*Deploying update to Azure*</span></span>

<a id="Ex4Task4"></a>
#### <a name="task-4--verification"></a><span data-ttu-id="22e83-580">タスク 4: 検証</span><span class="sxs-lookup"><span data-stu-id="22e83-580">Task 4 – Verification</span></span>

<span data-ttu-id="22e83-581">このタスクでは、使用**Internet Explorer**を参照する、**マニア Quiz**アプリケーションと URL がイメージの動作とするルールを書き直すことのチェックはでホストされているイメージにリダイレクト**Azure Blob記憶域**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-581">In this task you will use **Internet Explorer** to browse the **Geek Quiz** application and check that the URL rewrite rule for images works and you are redirected to the image hosted on **Azure Blob Storage**.</span></span>

1. <span data-ttu-id="22e83-582">Internet Explorer を開き、web アプリに移動します (例: `http://<your-web-site>.azurewebsites.net`)。</span><span class="sxs-lookup"><span data-stu-id="22e83-582">Open Internet Explorer and navigate to your web app (e.g. `http://<your-web-site>.azurewebsites.net`).</span></span> <span data-ttu-id="22e83-583">以前に作成した資格情報を使用してをログインします。</span><span class="sxs-lookup"><span data-stu-id="22e83-583">Log in using the previously created credentials.</span></span>

    <span data-ttu-id="22e83-584">![イメージとマニア Quiz web アプリケーションを表示](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "マニア Quiz で web アプリ イメージを表示")</span><span class="sxs-lookup"><span data-stu-id="22e83-584">![Showing the Geek Quiz web app with the image](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "Showing the Geek Quiz web app with the image")</span></span>

    <span data-ttu-id="22e83-585">*イメージとマニア Quiz web アプリケーションを表示*</span><span class="sxs-lookup"><span data-stu-id="22e83-585">*Showing the Geek Quiz web app with the image*</span></span>
2. <span data-ttu-id="22e83-586">キーを押して**F12**開発ツールを起動するには、選択、**ネットワーク**タブし、記録を開始します。</span><span class="sxs-lookup"><span data-stu-id="22e83-586">Press **F12** to launch the development tools, select the **Network** tab and start recording.</span></span>

    <span data-ttu-id="22e83-587">![ネットワークの記録を開始](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "ネットワークの記録を開始")</span><span class="sxs-lookup"><span data-stu-id="22e83-587">![Starting network recording](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "Starting network recording")</span></span>

    <span data-ttu-id="22e83-588">*ネットワークの記録を開始*</span><span class="sxs-lookup"><span data-stu-id="22e83-588">*Starting network recording*</span></span>
3. <span data-ttu-id="22e83-589">キーを押して**ctrl キーを押しながら f5 キーを押して**web ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="22e83-589">Press **CTRL + F5** to refresh the web page.</span></span>
4. <span data-ttu-id="22e83-590">に対する HTTP 要求が表示されます、ページが読み込みを完了した後、 **/img/logo-big.png** 、http URL **301**結果 (リダイレクト) と別の要求を`http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png`URL と HTTP **200**結果。</span><span class="sxs-lookup"><span data-stu-id="22e83-590">Once the page has finished loading, you should see an HTTP request for the **/img/logo-big.png** URL with an HTTP **301** result (redirect) and another request for `http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png` URL with a HTTP **200** result.</span></span>

    <span data-ttu-id="22e83-591">![URL リダイレクトを確認する](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "開発ツールでのリダイレクトを示す")</span><span class="sxs-lookup"><span data-stu-id="22e83-591">![Verifying the URL redirect](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "Showing the redirect in Dev Tools")</span></span>

    <span data-ttu-id="22e83-592">*URL リダイレクトを確認しています*</span><span class="sxs-lookup"><span data-stu-id="22e83-592">*Verifying the URL redirect*</span></span>

<a id="Exercise5"></a>
### <a name="exercise-5-using-autoscale-for-web-apps"></a><span data-ttu-id="22e83-593">手順 5: Web アプリの自動スケールを使用します。</span><span class="sxs-lookup"><span data-stu-id="22e83-593">Exercise 5: Using Autoscale for Web Apps</span></span>

> [!NOTE]
> <span data-ttu-id="22e83-594">Web の負荷のサポートを必要となるために、この手順は省略可能で&amp;パフォーマンス テストで使用可能なのみ**Visual Studio 2013 Ultimate Edition**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-594">This exercise is optional, since it requires support for Web Load &amp; Performance Testing which is only available for **Visual Studio 2013 Ultimate Edition**.</span></span> <span data-ttu-id="22e83-595">Visual Studio 2013 の特定の機能の詳細については、バージョンを比較[ここ](https://www.microsoft.com/visualstudio/eng/products/compare)です。</span><span class="sxs-lookup"><span data-stu-id="22e83-595">For more information on specific Visual Studio 2013 features, compare versions [here](https://www.microsoft.com/visualstudio/eng/products/compare).</span></span>


<span data-ttu-id="22e83-596">**Azure App Service Web Apps**で実行されている web アプリの自動スケール機能を提供して**標準モード**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-596">**Azure App Service Web Apps** provides the Autoscale feature for web apps running in **Standard Mode**.</span></span> <span data-ttu-id="22e83-597">自動スケールには、Azure の負荷に応じて、web アプリのインスタンス数を自動的にスケール設定ことができます。</span><span class="sxs-lookup"><span data-stu-id="22e83-597">Autoscale lets Azure automatically scale the instance count of your web app depending on the load.</span></span> <span data-ttu-id="22e83-598">自動スケールを有効にすると、Azure web アプリの CPU が 5 分ごとに 1 回をチェックされ、その時点で必要に応じてインスタンスが追加されます。</span><span class="sxs-lookup"><span data-stu-id="22e83-598">When Autoscale is enabled, Azure checks the CPU of your web app once every five minutes and adds instances as needed at that point in time.</span></span> <span data-ttu-id="22e83-599">CPU 使用率が低い場合は、インスタンスが削除されます 2 時間ごとに web アプリのパフォーマンスが低下していないことを確認してください。</span><span class="sxs-lookup"><span data-stu-id="22e83-599">If the CPU usage is low, Azure will remove instances once every two hours to ensure that the performance of your web app is not degraded.</span></span>

<span data-ttu-id="22e83-600">この手順で構成に必要な手順を参照してください、**自動スケール**の機能に関する、**マニア Quiz** web アプリです。</span><span class="sxs-lookup"><span data-stu-id="22e83-600">In this exercise you will go through the steps required to configure the **Autoscale** feature for the **Geek Quiz** web app.</span></span> <span data-ttu-id="22e83-601">インスタンスのアップグレードをトリガーするアプリケーションで十分な CPU の負荷を生成する Visual Studio ロード テストを実行して、この機能を確認します。</span><span class="sxs-lookup"><span data-stu-id="22e83-601">You will verify this feature by running a Visual Studio load test to generate enough CPU load on the application to trigger an instance upgrade.</span></span>

<a id="Ex5Task1"></a>
#### <a name="task-1--configuring-autoscale-based-on-the-cpu-metric"></a><span data-ttu-id="22e83-602">作業 1: CPU メトリックに基づいて自動スケールを構成します。</span><span class="sxs-lookup"><span data-stu-id="22e83-602">Task 1 – Configuring Autoscale Based on the CPU Metric</span></span>

<span data-ttu-id="22e83-603">このタスクでは、Azure 管理ポータルを使用して手順 2. で作成した web アプリケーションの自動スケール機能を有効にします。</span><span class="sxs-lookup"><span data-stu-id="22e83-603">In this task you will use the Azure management portal to enable the Autoscale feature for the web app you created in Exercise 2.</span></span>

1. <span data-ttu-id="22e83-604">[Azure 管理ポータル](https://manage.windowsazure.com/)[ **web サイト**手順 2 で作成した web アプリケーション] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="22e83-604">In the [Azure management portal](https://manage.windowsazure.com/), select **Websites** and click the web app you created in Exercise 2.</span></span>
2. <span data-ttu-id="22e83-605">移動し、**スケール**ページ。</span><span class="sxs-lookup"><span data-stu-id="22e83-605">Navigate to the **Scale** page.</span></span> <span data-ttu-id="22e83-606">下にある、**容量**セクションで、 **CPU**の**メトリックによるスケール**構成します。</span><span class="sxs-lookup"><span data-stu-id="22e83-606">Under the **capacity** section, select **CPU** for the **Scale by Metric** configuration.</span></span>

    > [!NOTE]
    > <span data-ttu-id="22e83-607">CPU によるスケール、Azure は、CPU 使用率が変更された場合、アプリが使用するインスタンスの数を動的に調整されます。</span><span class="sxs-lookup"><span data-stu-id="22e83-607">When scaling by CPU, Azure dynamically adjusts the number of instances that the app uses if the CPU usage changes.</span></span>

    <span data-ttu-id="22e83-608">![CPU によるスケールを選択する](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "自動スケーリングの CPU のメトリックを選択します。")</span><span class="sxs-lookup"><span data-stu-id="22e83-608">![Selecting to scale by CPU](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "Selecting the CPU metric for auto scaling")</span></span>

    <span data-ttu-id="22e83-609">*CPU によるスケールを選択します。*</span><span class="sxs-lookup"><span data-stu-id="22e83-609">*Selecting to scale by CPU*</span></span>
3. <span data-ttu-id="22e83-610">変更、**ターゲット CPU**構成**20**-**40** % です。</span><span class="sxs-lookup"><span data-stu-id="22e83-610">Change the **Target CPU** configuration to **20**-**40** percent.</span></span>

    > [!NOTE]
    > <span data-ttu-id="22e83-611">この範囲は、web アプリの平均 CPU 使用率を表します。</span><span class="sxs-lookup"><span data-stu-id="22e83-611">This range represents the average CPU usage for your web app.</span></span> <span data-ttu-id="22e83-612">Azure では、追加または web アプリをこの範囲内で保持するインスタンスを削除します。</span><span class="sxs-lookup"><span data-stu-id="22e83-612">Azure will add or remove instances to keep your web app in this range.</span></span> <span data-ttu-id="22e83-613">スケーリングに使用されたインスタンスの最小値と最大数が指定された、**インスタンス カウント**構成します。</span><span class="sxs-lookup"><span data-stu-id="22e83-613">The minimum and maximum number of instances used for scaling is specified in the **Instance Count** configuration.</span></span> <span data-ttu-id="22e83-614">Azure は、上またはその制限を超えるは接続されません。</span><span class="sxs-lookup"><span data-stu-id="22e83-614">Azure will never go above or beyond that limit.</span></span>
    > 
    > <span data-ttu-id="22e83-615">既定値**ターゲット CPU**だけのため、このラボの値が変更されます。</span><span class="sxs-lookup"><span data-stu-id="22e83-615">The default **Target CPU** values are modified just for the purposes of this lab.</span></span> <span data-ttu-id="22e83-616">CPU の範囲を小さい値に構成するは、中程度の負荷がアプリケーションに配置された自動スケールを発生させる可能性が大きくしています。</span><span class="sxs-lookup"><span data-stu-id="22e83-616">By configuring the CPU range with small values, you are increasing the chances to trigger Autoscale when a moderate load is placed on the application.</span></span>

    <span data-ttu-id="22e83-617">![20 ~ 40% を指定する CPU ターゲットを変更する](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "20 ~ 40% を指定する CPU ターゲットを変更します。")</span><span class="sxs-lookup"><span data-stu-id="22e83-617">![Changing the target CPU to be between 20 and 40 percent](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "Changing the target CPU to be between 20 and 40 percent")</span></span>

    <span data-ttu-id="22e83-618">*20 ~ 40% を指定するターゲット CPU を変更します。*</span><span class="sxs-lookup"><span data-stu-id="22e83-618">*Changing the Target CPU to be between 20 and 40 percent*</span></span>
4. <span data-ttu-id="22e83-619">をクリックして**保存**変更を保存するコマンド バーにします。</span><span class="sxs-lookup"><span data-stu-id="22e83-619">Click **Save** in the command bar to save the changes.</span></span>

<a id="Ex5Task2"></a>
#### <a name="task-2--load-testing-with-visual-studio"></a><span data-ttu-id="22e83-620">タスク 2-Visual Studio でロード テスト</span><span class="sxs-lookup"><span data-stu-id="22e83-620">Task 2 – Load Testing with Visual Studio</span></span>

<span data-ttu-id="22e83-621">これで**自動スケール**された構成を作成、 **Web パフォーマンスとロード テストのプロジェクト**web アプリである程度の CPU 負荷を生成する Visual Studio でします。</span><span class="sxs-lookup"><span data-stu-id="22e83-621">Now that **Autoscale** has been configured, you will create a **Web Performance and Load Test Project** in Visual Studio to generate some CPU load on your web app.</span></span>

1. <span data-ttu-id="22e83-622">開いている**Visual Studio Ultimate 2013**選択**ファイル |新しい |プロジェクトの追加.**を新しいソリューションを開始します。</span><span class="sxs-lookup"><span data-stu-id="22e83-622">Open **Visual Studio Ultimate 2013** and select **File | New | Project...** to start a new solution.</span></span>

    <span data-ttu-id="22e83-623">![新しいプロジェクトを作成する](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "新規プロジェクトの作成")</span><span class="sxs-lookup"><span data-stu-id="22e83-623">![Creating a new project](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "Creating a new project")</span></span>

    <span data-ttu-id="22e83-624">*新しいプロジェクトを作成します。*</span><span class="sxs-lookup"><span data-stu-id="22e83-624">*Creating a new project*</span></span>
2. <span data-ttu-id="22e83-625">**新しいプロジェクト**ダイアログ ボックスで、 **Web パフォーマンスとロード テストのプロジェクト**下にある、 **Visual c# |テスト**タブです。確認してください**.NET Framework 4.5**がプロジェクトに名前を選択すると、 *WebAndLoadTestProject*を選択、**場所** をクリック**OK**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-625">In the **New Project** dialog box, select **Web Performance and Load Test Project** under the **Visual C# | Test** tab. Make sure **.NET Framework 4.5** is selected, name the project *WebAndLoadTestProject*, choose a **Location** and click **OK**.</span></span>

    <span data-ttu-id="22e83-626">![新しい Web およびロード テスト プロジェクトを作成する](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "Web とロード テスト プロジェクトを新規作成")</span><span class="sxs-lookup"><span data-stu-id="22e83-626">![Creating a new Web and Load Test project](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "Creating a new Web and Load Test project")</span></span>

    <span data-ttu-id="22e83-627">*新しい Web およびロード テスト プロジェクトを作成します。*</span><span class="sxs-lookup"><span data-stu-id="22e83-627">*Creating a new Web and Load Test project*</span></span>
3. <span data-ttu-id="22e83-628">**WebTest1.webtest**を右クリックし、 **WebTest1**ノードとクリック**要求の追加**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-628">In the **WebTest1.webtest** Right-click the **WebTest1** node and click **Add Request**.</span></span>

    <span data-ttu-id="22e83-629">![WebTest1 への要求の追加](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "WebTest1 への要求の追加")</span><span class="sxs-lookup"><span data-stu-id="22e83-629">![Adding a request to WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "Adding a request to WebTest1")</span></span>

    <span data-ttu-id="22e83-630">*WebTest1 への要求の追加*</span><span class="sxs-lookup"><span data-stu-id="22e83-630">*Adding a request to WebTest1*</span></span>
4. <span data-ttu-id="22e83-631">**プロパティ**ウィンドウ、新しい要求のノードの更新、 **Url** web アプリの URL を参照するプロパティ (例:  *[http://geek-quiz.azurewebsites.net/](http://geek-quiz.azurewebsites.net/)*).</span><span class="sxs-lookup"><span data-stu-id="22e83-631">In the **Properties** window of the new request node, update the **Url** property to point to the URL of your web app (e.g. *[http://geek-quiz.azurewebsites.net/](http://geek-quiz.azurewebsites.net/)*).</span></span>

    <span data-ttu-id="22e83-632">![Url プロパティを変更する](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "Url プロパティを変更します。")</span><span class="sxs-lookup"><span data-stu-id="22e83-632">![Changing the Url property](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "Changing the Url property")</span></span>

    <span data-ttu-id="22e83-633">*Url プロパティを変更します。*</span><span class="sxs-lookup"><span data-stu-id="22e83-633">*Changing the Url property*</span></span>
5. <span data-ttu-id="22e83-634">**WebTest1.webtest**  ウィンドウを右クリックして**WebTest1**  をクリック**ループを追加しています.**.</span><span class="sxs-lookup"><span data-stu-id="22e83-634">In the **WebTest1.webtest** window, right-click **WebTest1** and click **Add Loop...**.</span></span>

    <span data-ttu-id="22e83-635">![WebTest1 にループを追加する](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "WebTest1 にループを追加します。")</span><span class="sxs-lookup"><span data-stu-id="22e83-635">![Adding a loop to WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "Adding a loop to WebTest1")</span></span>

    <span data-ttu-id="22e83-636">*WebTest1 にループを追加します。*</span><span class="sxs-lookup"><span data-stu-id="22e83-636">*Adding a loop to WebTest1*</span></span>
6. <span data-ttu-id="22e83-637">**条件付き規則の追加と項目をループ**ダイアログ ボックスで、 **For ループ**ルールし、次のプロパティを変更します。</span><span class="sxs-lookup"><span data-stu-id="22e83-637">In the **Add Conditional Rule and Items to Loop** dialog box, select the **For Loop** rule and modify the following properties.</span></span>

    1. <span data-ttu-id="22e83-638">**終了値:** 1000</span><span class="sxs-lookup"><span data-stu-id="22e83-638">**Terminating value:** 1000</span></span>
    2. <span data-ttu-id="22e83-639">**コンテキスト パラメーター名:**反復子</span><span class="sxs-lookup"><span data-stu-id="22e83-639">**Context Parameter Name:** Iterator</span></span>
    3. <span data-ttu-id="22e83-640">**増分値:** 1</span><span class="sxs-lookup"><span data-stu-id="22e83-640">**Increment Value:** 1</span></span>

    <span data-ttu-id="22e83-641">![For ループ ルールを選択し、プロパティを更新](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "For ループ ルールを選択し、プロパティの更新")</span><span class="sxs-lookup"><span data-stu-id="22e83-641">![Selecting the For Loop rule and updating the properties](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "Selecting the For Loop rule and updating the properties")</span></span>

    <span data-ttu-id="22e83-642">*For ループ ルールを選択し、プロパティの更新*</span><span class="sxs-lookup"><span data-stu-id="22e83-642">*Selecting the For Loop rule and updating the properties*</span></span>
7. <span data-ttu-id="22e83-643">下にある、**ループ内の項目**セクションで、ループの最初と最後の項目にする、以前に作成する要求を選択します。</span><span class="sxs-lookup"><span data-stu-id="22e83-643">Under the **Items in loop** section, select the request you created previously to be the first and last item for the loop.</span></span> <span data-ttu-id="22e83-644">**[OK]** をクリックして続行します。</span><span class="sxs-lookup"><span data-stu-id="22e83-644">Click **OK** to continue.</span></span>

    <span data-ttu-id="22e83-645">![ループの最初と最後の項目を選択する](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "ループの最初と最後の項目を選択します。")</span><span class="sxs-lookup"><span data-stu-id="22e83-645">![Selecting the first and last items for the loop](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "Selecting the first and last items for the loop")</span></span>

    <span data-ttu-id="22e83-646">*ループの最初と最後の項目を選択します。*</span><span class="sxs-lookup"><span data-stu-id="22e83-646">*Selecting the first and last items for the loop*</span></span>
8. <span data-ttu-id="22e83-647">**ソリューション エクスプ ローラー**を右クリックし、 **WebAndLoadTestProject**プロジェクトで、展開、**追加**メニュー**ロード テストしています.**.</span><span class="sxs-lookup"><span data-stu-id="22e83-647">In **Solution Explorer**, right-click the **WebAndLoadTestProject** project, expand the **Add** menu and select **Load Test...**.</span></span>

    <span data-ttu-id="22e83-648">![WebAndLoadTestProject プロジェクトへのロード テストの追加](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "WebAndLoadTestProject プロジェクトへのロード テストの追加")</span><span class="sxs-lookup"><span data-stu-id="22e83-648">![Adding a Load Test to the WebAndLoadTestProject project](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "Adding a Load Test to the WebAndLoadTestProject project")</span></span>

    <span data-ttu-id="22e83-649">*WebAndLoadTestProject プロジェクトへのロード テストの追加*</span><span class="sxs-lookup"><span data-stu-id="22e83-649">*Adding a Load Test to the WebAndLoadTestProject project*</span></span>
9. <span data-ttu-id="22e83-650">**新しいロード テスト ウィザード**ダイアログ ボックスで、をクリックして**次**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-650">In the **New Load Test Wizard** dialog box, click **Next**.</span></span>

    <span data-ttu-id="22e83-651">![新しいロード テスト ウィザード](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "新しいロード テスト ウィザード")</span><span class="sxs-lookup"><span data-stu-id="22e83-651">![New Load Test Wizard](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "New Load Test Wizard")</span></span>

    <span data-ttu-id="22e83-652">*新しいロード テスト ウィザード*</span><span class="sxs-lookup"><span data-stu-id="22e83-652">*New Load Test Wizard*</span></span>
10. <span data-ttu-id="22e83-653">**シナリオ** ページで、**待ち時間を使用しないでください** をクリック**次**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-653">In the **Scenario** page, select **Do not use think times** and click **Next**.</span></span>

    <span data-ttu-id="22e83-654">![待ち時間を使用しないことを選択すると](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "待ち時間を使用しないように選択します。")</span><span class="sxs-lookup"><span data-stu-id="22e83-654">![Selecting not to use think times](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "Selecting not to use think times")</span></span>

    <span data-ttu-id="22e83-655">*待ち時間を使用しないことを選択します。*</span><span class="sxs-lookup"><span data-stu-id="22e83-655">*Selecting not to use think times*</span></span>
11. <span data-ttu-id="22e83-656">**ロード パターン** ページで、ことを確認して、**持続ロード**オプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="22e83-656">In the **Load Pattern** page, make sure that the **Constant Load** option is selected.</span></span> <span data-ttu-id="22e83-657">変更、**ユーザー カウント**設定を**250**ユーザーとクリック**[次へ]**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-657">Change the **User Count** setting to **250** users and click **Next**.</span></span>

    <span data-ttu-id="22e83-658">![ユーザー カウントを 250 に変更する](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "250 に、ユーザーの数を変更します。")</span><span class="sxs-lookup"><span data-stu-id="22e83-658">![Changing the user count to 250](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "Changing the user count to 250")</span></span>

    <span data-ttu-id="22e83-659">*ユーザー カウントを 250 に変更します。*</span><span class="sxs-lookup"><span data-stu-id="22e83-659">*Changing the user count to 250*</span></span>
12. <span data-ttu-id="22e83-660">**テスト ミックス モデル** ページで、**テストの時系列順に基づいて** をクリック**次**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-660">In the **Test Mix Model** page, select **Based on sequential test order** and click **Next**.</span></span>

    <span data-ttu-id="22e83-661">![テスト ミックス モデルを選択すると](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "テスト ミックス モデルを選択します。")</span><span class="sxs-lookup"><span data-stu-id="22e83-661">![Selecting the test mix model](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "Selecting the test mix model")</span></span>

    <span data-ttu-id="22e83-662">*テスト ミックス モデルを選択します。*</span><span class="sxs-lookup"><span data-stu-id="22e83-662">*Selecting the test mix model*</span></span>
13. <span data-ttu-id="22e83-663">**テスト ミックス モデル**] ページで [**追加しています.**テストをミックスに追加します。</span><span class="sxs-lookup"><span data-stu-id="22e83-663">In the **Test Mix Model** page, click **Add...** to add a test to the mix.</span></span>

    <span data-ttu-id="22e83-664">![テストをテスト ミックスに追加する](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "テストをテスト ミックスに追加します。")</span><span class="sxs-lookup"><span data-stu-id="22e83-664">![Adding a test to the test mix](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "Adding a test to the test mix")</span></span>

    <span data-ttu-id="22e83-665">*テストをテスト ミックスに追加します。*</span><span class="sxs-lookup"><span data-stu-id="22e83-665">*Adding a test to the test mix*</span></span>
14. <span data-ttu-id="22e83-666">**テストの追加**ダイアログ ボックスをダブルクリックして**WebTest1**にテストを追加する、**選択したテストの** ボックスの一覧です。</span><span class="sxs-lookup"><span data-stu-id="22e83-666">In the **Add Tests** dialog box, double-click **WebTest1** to add the test to the **Selected tests** list.</span></span> <span data-ttu-id="22e83-667">**[OK]** をクリックして続行します。</span><span class="sxs-lookup"><span data-stu-id="22e83-667">Click **OK** to continue.</span></span>

    <span data-ttu-id="22e83-668">![WebTest1 テストを追加する](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "WebTest1 テストを追加します。")</span><span class="sxs-lookup"><span data-stu-id="22e83-668">![Adding the WebTest1 test](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "Adding the WebTest1 test")</span></span>

    <span data-ttu-id="22e83-669">*WebTest1 テストを追加します。*</span><span class="sxs-lookup"><span data-stu-id="22e83-669">*Adding the WebTest1 test*</span></span>
15. <span data-ttu-id="22e83-670">戻り、**テスト ミックス** ページで、をクリックして**次**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-670">Back in the **Test Mix** page, click **Next**.</span></span>

    <span data-ttu-id="22e83-671">![テスト ミックスのページを完了](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "テスト ミックスのページを完了します。")</span><span class="sxs-lookup"><span data-stu-id="22e83-671">![Completing the Test Mix page](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "Completing the Test Mix page")</span></span>

    <span data-ttu-id="22e83-672">*テスト ミックスのページを完了します。*</span><span class="sxs-lookup"><span data-stu-id="22e83-672">*Completing the Test Mix page*</span></span>
16. <span data-ttu-id="22e83-673">**ネットワーク ミックス**] ページで [**次**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-673">In the **Network Mix** page, click **Next**.</span></span>

    <span data-ttu-id="22e83-674">![クリックすると、ネットワーク ミックス ページで次に](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "ネットワーク ミックスのページで次をクリックします。")</span><span class="sxs-lookup"><span data-stu-id="22e83-674">![Clicking next in the Network Mix page](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "Clicking next in the Network Mix page")</span></span>

    <span data-ttu-id="22e83-675">*ネットワーク ミックスのページの [次へ] をクリックして*</span><span class="sxs-lookup"><span data-stu-id="22e83-675">*Clicking next in the Network Mix page*</span></span>
17. <span data-ttu-id="22e83-676">**ブラウザー ミックス**] ページで、[**インターネット エクスプ ローラー 10.0**をクリックして、ブラウザーの種類として**次**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-676">In the **Browser Mix** page, select **Internet Explorer 10.0** as the browser type and click **Next**.</span></span>

    <span data-ttu-id="22e83-677">![ブラウザーの種類を選択すると](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "ブラウザーの種類を選択します。")</span><span class="sxs-lookup"><span data-stu-id="22e83-677">![Selecting the browser type](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "Selecting the browser type")</span></span>

    <span data-ttu-id="22e83-678">*ブラウザーの種類を選択します。*</span><span class="sxs-lookup"><span data-stu-id="22e83-678">*Selecting the browser type*</span></span>
18. <span data-ttu-id="22e83-679">**カウンター セット** ページで **次**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-679">In the **Counter Sets** page, click **Next**.</span></span>

    <span data-ttu-id="22e83-680">![カウンター セット ページで [次へ] をクリックすると](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "カウンター セット] ページでは、[次をクリックすると")</span><span class="sxs-lookup"><span data-stu-id="22e83-680">![Clicking Next in the Counter Sets page](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "Clicking Next in the Counter Sets page")</span></span>

    <span data-ttu-id="22e83-681">*カウンター セット ページで 次へ をクリックして*</span><span class="sxs-lookup"><span data-stu-id="22e83-681">*Clicking Next in the Counter Sets page*</span></span>
19. <span data-ttu-id="22e83-682">**実行設定** ページで、設定、**ロード テストの継続**に**5 分** をクリック**完了**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-682">In the **Run Settings** page, set the **Load test duration** to **5 minutes** and click **Finish**.</span></span>

    <span data-ttu-id="22e83-683">![ロード テストの継続を 5 分に設定](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "ロード テストの継続を 5 分に設定します。")</span><span class="sxs-lookup"><span data-stu-id="22e83-683">![Setting the load test duration to 5 minutes](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "Setting the load test duration to 5 minutes")</span></span>

    <span data-ttu-id="22e83-684">*ロード テストの継続を 5 分に設定します。*</span><span class="sxs-lookup"><span data-stu-id="22e83-684">*Setting the load test duration to 5 minutes*</span></span>
20. <span data-ttu-id="22e83-685">**ソリューション エクスプ ローラー**をダブルクリックして、 **Local.settings**テストの設定を表示するファイル。</span><span class="sxs-lookup"><span data-stu-id="22e83-685">In **Solution Explorer**, double-click the **Local.settings** file to explore the test settings.</span></span> <span data-ttu-id="22e83-686">既定では、Visual Studio は、ローカル コンピューターを使用してテストを実行します。</span><span class="sxs-lookup"><span data-stu-id="22e83-686">By default, Visual Studio uses your local computer to run the tests.</span></span>

    > [!NOTE]
    > <span data-ttu-id="22e83-687">使用して、クラウドでロード テストを実行するテスト プロジェクトを構成する代わりに、 **Visual Studio Online (VSO)**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-687">Alternatively, you can configure your test project to run the load tests in the cloud using **Visual Studio Online (VSO)**.</span></span> <span data-ttu-id="22e83-688">VSO より現実的なロードをシミュレートするサービスのテスト、CPU の使用率、使用可能なメモリ、ネットワーク帯域幅などのローカル環境の制約を回避するクラウド ベースのロードを提供します。</span><span class="sxs-lookup"><span data-stu-id="22e83-688">VSO provides a cloud-based load testing service that simulates a more realistic load, avoiding local environment constraints like CPU capacity, available memory and network bandwidth.</span></span> <span data-ttu-id="22e83-689">VSO を使用してロード テストを実行する方法の詳細については、次を参照してください。[この資料](https://www.visualstudio.com/get-started/load-test-your-app-vs)です。</span><span class="sxs-lookup"><span data-stu-id="22e83-689">For more information about using VSO to run load tests, see [this article](https://www.visualstudio.com/get-started/load-test-your-app-vs).</span></span>

    ![テストの設定](maintainable-azure-websites-managing-change-and-scale/_static/image98.png)

<a id="Ex5Task3"></a>
#### <a name="task-3--autoscale-verification"></a><span data-ttu-id="22e83-691">タスク 3-自動スケールの検証</span><span class="sxs-lookup"><span data-stu-id="22e83-691">Task 3 – Autoscale Verification</span></span>

<span data-ttu-id="22e83-692">今すぐ、前のタスクで作成したロード テストを実行し、負荷の下で web アプリの動作を確認します。</span><span class="sxs-lookup"><span data-stu-id="22e83-692">You will now execute the load test you created in the previous task and see how your web app behaves under load.</span></span>

1. <span data-ttu-id="22e83-693">**ソリューション エクスプ ローラー**をダブルクリックして**LoadTest1.loadtest**をロード テストを開きます。</span><span class="sxs-lookup"><span data-stu-id="22e83-693">In **Solution Explorer**, double-click **LoadTest1.loadtest** to open the load test.</span></span>

    <span data-ttu-id="22e83-694">![LoadTest1.loadtest を開く](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "LoadTest1.loadtest を開く")</span><span class="sxs-lookup"><span data-stu-id="22e83-694">![Opening LoadTest1.loadtest](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "Opening LoadTest1.loadtest")</span></span>

    <span data-ttu-id="22e83-695">*開く LoadTest1.loadtest*</span><span class="sxs-lookup"><span data-stu-id="22e83-695">*Opening LoadTest1.loadtest*</span></span>
2. <span data-ttu-id="22e83-696">**LoadTest1.loadtest**ウィンドウで、ロード テストを実行するツールボックス内の最初のボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="22e83-696">In the **LoadTest1.loadtest** window, click the first button in the toolbox to run the load test.</span></span>

    <span data-ttu-id="22e83-697">![ロード テストを実行している](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "ロード テストの実行")</span><span class="sxs-lookup"><span data-stu-id="22e83-697">![Running the load test](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "Running the load test")</span></span>

    <span data-ttu-id="22e83-698">*ロード テストの実行*</span><span class="sxs-lookup"><span data-stu-id="22e83-698">*Running the load test*</span></span>
3. <span data-ttu-id="22e83-699">ロード テストが完了するまで待機します。</span><span class="sxs-lookup"><span data-stu-id="22e83-699">Wait until the load test completes.</span></span>

    > [!NOTE]
    > <span data-ttu-id="22e83-700">ロード テストでは、web アプリに同時に要求を送信する複数のユーザーをシミュレートします。</span><span class="sxs-lookup"><span data-stu-id="22e83-700">The load test simulates multiple users that send requests to the web app simultaneously.</span></span> <span data-ttu-id="22e83-701">テストが実行されている場合は、すべてのエラー、警告、またはロード テストの実行に関連するその他の情報を検出するために使用可能なカウンターを監視できます。</span><span class="sxs-lookup"><span data-stu-id="22e83-701">When the test is running, you can monitor the available counters to detect any errors, warnings or other information related to your load test run.</span></span>

    <span data-ttu-id="22e83-702">![ロード テストを実行している](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "ロード テストが完了するまで待機しています")</span><span class="sxs-lookup"><span data-stu-id="22e83-702">![Load test running](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "Waiting until the load test completes")</span></span>

    <span data-ttu-id="22e83-703">*ロード テストの実行*</span><span class="sxs-lookup"><span data-stu-id="22e83-703">*Load test running*</span></span>
4. <span data-ttu-id="22e83-704">テストが完了すると、管理ポータルに戻りに移動、**スケール**web アプリのページです。</span><span class="sxs-lookup"><span data-stu-id="22e83-704">Once the test completes, go back to the management portal and navigate to the **Scale** page of your web app.</span></span> <span data-ttu-id="22e83-705">下にある、**容量** セクションで、表示されます、グラフの新しいインスタンスが自動的に展開されたことです。</span><span class="sxs-lookup"><span data-stu-id="22e83-705">Under the **capacity** section, you should see in the graph that a new instance was automatically deployed.</span></span>

    ![新しいインスタンスを自動的に展開](maintainable-azure-websites-managing-change-and-scale/_static/image102.png)

    <span data-ttu-id="22e83-707">*新しいインスタンスを自動的に展開*</span><span class="sxs-lookup"><span data-stu-id="22e83-707">*New instance automatically deployed*</span></span>

    > [!NOTE]
    > <span data-ttu-id="22e83-708">グラフに表示を変更するには数分かかる場合があります (キーを押して**ctrl キーを押しながら f5 キーを押して**ページを更新するには、定期的に)。</span><span class="sxs-lookup"><span data-stu-id="22e83-708">It may take several minutes for the changes to appear in the graph (press **CTRL + F5** periodically to refresh the page).</span></span> <span data-ttu-id="22e83-709">すべての変更が表示されない場合、次を実行できます。</span><span class="sxs-lookup"><span data-stu-id="22e83-709">If you do not see any changes, you can try the following:</span></span>
    > 
    > - <span data-ttu-id="22e83-710">ロード テストの期間を延長 (たとえば**10 分**)</span><span class="sxs-lookup"><span data-stu-id="22e83-710">Increase the duration of the load test (e.g. to **10 minutes**)</span></span>
    > - <span data-ttu-id="22e83-711">最大値と最小値を減らして、**ターゲット CPU** web アプリの自動スケールの構成内の範囲</span><span class="sxs-lookup"><span data-stu-id="22e83-711">Reduce the maximum and minimum values of the **Target CPU** range in the Autoscale configuration of your web app</span></span>
    > - <span data-ttu-id="22e83-712">クラウド内のロード テストの実行**Visual Studio Online**です。</span><span class="sxs-lookup"><span data-stu-id="22e83-712">Run the load test in the cloud with **Visual Studio Online**.</span></span> <span data-ttu-id="22e83-713">詳細については[ここ](https://www.visualstudio.com/get-started/load-test-your-app-vs.aspx)</span><span class="sxs-lookup"><span data-stu-id="22e83-713">More information [here](https://www.visualstudio.com/get-started/load-test-your-app-vs.aspx)</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="22e83-714">まとめ</span><span class="sxs-lookup"><span data-stu-id="22e83-714">Summary</span></span>

<span data-ttu-id="22e83-715">このハンズオン ラボを設定し、Azure で web アプリを実稼働環境にアプリケーションを配置する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="22e83-715">In this hands-on lab, you learned how to set up and deploy your application to production web apps in Azure.</span></span> <span data-ttu-id="22e83-716">使用して、データベースの更新を検出して開始した**Entity Framework Code First Migrations**、しを使用して、サイトの新しいバージョンを展開することで続き**Git**へのロールバックを実行して、サイトの最新の安定バージョン。</span><span class="sxs-lookup"><span data-stu-id="22e83-716">You started by detecting and updating your databases using **Entity Framework Code First Migrations**, then continued by deploying new versions of your site using **Git** and performing rollbacks to the latest stable version of your site.</span></span> <span data-ttu-id="22e83-717">さらに、記憶域を使用して、静的なコンテンツを Blob コンテナーに移動するアプリをスケールする方法について学習しました。</span><span class="sxs-lookup"><span data-stu-id="22e83-717">Additionally, you learned how to scale your app using Storage to move your static content to a Blob container.</span></span>
