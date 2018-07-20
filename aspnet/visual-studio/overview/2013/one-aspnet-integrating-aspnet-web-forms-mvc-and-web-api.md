---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: 'ハンズ オン ラボ: One ASP.NET: ASP.NET Web フォーム、MVC、Web API の統合 |Microsoft Docs'
author: rick-anderson
description: ASP.NET は、Web サイト、アプリ、および MVC、Web API、およびその他のユーザーなどの特殊なテクノロジを使用してサービスを構築するためのフレームワークです。 で ASP.NET h を拡張しています.
ms.author: aspnetcontent
ms.date: 07/16/2014
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: 276207a6e7d2388ce53778928665c35de9327318
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837306"
---
<a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a><span data-ttu-id="efb7e-104">ハンズ オン ラボ: One ASP.NET: ASP.NET Web フォーム、MVC、Web API の統合</span><span class="sxs-lookup"><span data-stu-id="efb7e-104">Hands On Lab: One ASP.NET: Integrating ASP.NET Web Forms, MVC and Web API</span></span>
====================
<span data-ttu-id="efb7e-105">によって[Web キャンプ チーム](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="efb7e-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="efb7e-106">Web のキャンプ トレーニング キットをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="efb7e-106">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="efb7e-107">ASP.NET は、Web サイト、アプリ、および MVC、Web API、およびその他のユーザーなどの特殊なテクノロジを使用してサービスを構築するためのフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="efb7e-107">ASP.NET is a framework for building Web sites, apps and services using specialized technologies such as MVC, Web API and others.</span></span> <span data-ttu-id="efb7e-108">拡張とは、ASP.NET が作成されてから確認した、明示する必要がありますこれらのテクノロジが統合された最近の取り組みに作業**One ASP.NET**します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-108">With the expansion ASP.NET has seen since its creation and the expressed need to have these technologies integrated, there have been recent efforts in working towards **One ASP.NET**.</span></span>
> 
> <span data-ttu-id="efb7e-109">Visual Studio 2013 には、アプリケーションのビルドし、すべての ASP.NET テクノロジを 1 つのプロジェクトで使用できる新しい、統一されたプロジェクト システムが導入されています。</span><span class="sxs-lookup"><span data-stu-id="efb7e-109">Visual Studio 2013 introduces a new unified project system which lets you build an application and use all the ASP.NET technologies in one project.</span></span> <span data-ttu-id="efb7e-110">この機能は、プロジェクトとそれに伴うスティックの開始時に 1 つのテクノロジを選択する必要はありませんし、代わりに 1 つのプロジェクト内の複数の ASP.NET フレームワークの使用が推奨します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-110">This feature eliminates the need to pick one technology at the start of a project and stick with it, and instead encourages the use of multiple ASP.NET frameworks within one project.</span></span>
> 
> <span data-ttu-id="efb7e-111">すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[ http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit)します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-111">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="efb7e-112">概要</span><span class="sxs-lookup"><span data-stu-id="efb7e-112">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="efb7e-113">目的</span><span class="sxs-lookup"><span data-stu-id="efb7e-113">Objectives</span></span>

<span data-ttu-id="efb7e-114">このハンズオン ラボでは、学習する方法。</span><span class="sxs-lookup"><span data-stu-id="efb7e-114">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="efb7e-115">ベースの Web サイトを作成、 **One ASP.NET**プロジェクトの種類</span><span class="sxs-lookup"><span data-stu-id="efb7e-115">Create a Web site based on the **One ASP.NET** project type</span></span>
- <span data-ttu-id="efb7e-116">使用して異なる**ASP.NET**のようなフレームワーク**MVC**と**Web API**同じプロジェクト内で</span><span class="sxs-lookup"><span data-stu-id="efb7e-116">Use different **ASP.NET** frameworks like **MVC** and **Web API** in the same project</span></span>
- <span data-ttu-id="efb7e-117">主要なコンポーネントを識別、 **ASP.NET**アプリケーション</span><span class="sxs-lookup"><span data-stu-id="efb7e-117">Identify the main components of an **ASP.NET** application</span></span>
- <span data-ttu-id="efb7e-118">利用、 **ASP.NET スキャフォールディング**CRUD 操作を実行するには、コント ローラーとビューを自動的に作成するためにフレームワークが、モデル クラスに基づく</span><span class="sxs-lookup"><span data-stu-id="efb7e-118">Take advantage of the **ASP.NET Scaffolding** framework to automatically create Controllers and Views to perform CRUD operations based on your model classes</span></span>
- <span data-ttu-id="efb7e-119">ジョブごとに適切なツールを使用してマシンに、人間が判読できる形式で情報の同じセットを公開します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-119">Expose the same set of information in machine- and human-readable formats using the right tool for each job</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="efb7e-120">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="efb7e-120">Prerequisites</span></span>

<span data-ttu-id="efb7e-121">このハンズオン ラボを完了する、次が必要。</span><span class="sxs-lookup"><span data-stu-id="efb7e-121">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="efb7e-122">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/)以上</span><span class="sxs-lookup"><span data-stu-id="efb7e-122">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="efb7e-123">Visual Studio 2013 Update 1</span><span class="sxs-lookup"><span data-stu-id="efb7e-123">Visual Studio 2013 Update 1</span></span>](https://go.microsoft.com/fwlink/?LinkId=301714)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="efb7e-124">セットアップ</span><span class="sxs-lookup"><span data-stu-id="efb7e-124">Setup</span></span>

<span data-ttu-id="efb7e-125">このハンズオン ラボの演習を実行するためには、まず環境を設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="efb7e-125">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="efb7e-126">Windows Explorer をラボの [参照] を開いて**ソース**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="efb7e-126">Open Windows Explorer and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="efb7e-127">右クリックして**Setup.cmd**選択と**管理者として実行**環境を構成すると、このラボの Visual Studio のコード スニペットがインストールのセットアップ プロセスを起動します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-127">Right-click on **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="efb7e-128">ユーザー アカウント制御ダイアログ ボックスが表示されている場合は、続行するアクションを確認します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-128">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="efb7e-129">セットアップを実行する前に、このラボのすべての依存関係をチェックしたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-129">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="efb7e-130">コード スニペットの使用</span><span class="sxs-lookup"><span data-stu-id="efb7e-130">Using the Code Snippets</span></span>

<span data-ttu-id="efb7e-131">ラボ ドキュメント全体を通じて、コード ブロックを挿入するよう指示されます。</span><span class="sxs-lookup"><span data-stu-id="efb7e-131">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="efb7e-132">便宜上、このコードのほとんどは、Visual Studio コード スニペットを手動で追加することを避けるために Visual Studio 2013 内からアクセスできるとして提供されます。</span><span class="sxs-lookup"><span data-stu-id="efb7e-132">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="efb7e-133">ソリューションでは個々 の演習を伴います、**開始**を使用すると、各演習を他のユーザーとは無関係に練習のフォルダー。</span><span class="sxs-lookup"><span data-stu-id="efb7e-133">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="efb7e-134">演習の中に追加されるコード スニペットはこれらのスターティング ソリューションが表示されないし、演習を完了するまで動作しない可能性がありますに注意してください。</span><span class="sxs-lookup"><span data-stu-id="efb7e-134">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="efb7e-135">演習では、ソース コード内でも表示されます、**エンド**結果から、対応する演習の手順を実行するコードと Visual Studio ソリューションを含むフォルダー。</span><span class="sxs-lookup"><span data-stu-id="efb7e-135">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="efb7e-136">このハンズオン ラボを使用すると、追加のヘルプが必要な場合は、これらのソリューションをガイドとして使用できます。</span><span class="sxs-lookup"><span data-stu-id="efb7e-136">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="efb7e-137">演習</span><span class="sxs-lookup"><span data-stu-id="efb7e-137">Exercises</span></span>

<span data-ttu-id="efb7e-138">このハンズオン ラボには、次の演習が含まれます。</span><span class="sxs-lookup"><span data-stu-id="efb7e-138">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="efb7e-139">新しい Web フォーム プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-139">Creating a New Web Forms Project</span></span>](#Exercise1)
2. [<span data-ttu-id="efb7e-140">スキャフォールディングを使用した MVC コント ローラーを作成</span><span class="sxs-lookup"><span data-stu-id="efb7e-140">Creating an MVC Controller Using Scaffolding</span></span>](#Exercise2)
3. [<span data-ttu-id="efb7e-141">スキャフォールディングを使用して Web API コント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-141">Creating a Web API Controller Using Scaffolding</span></span>](#Exercise3)

<span data-ttu-id="efb7e-142">この演習の所要時間を推定: **60 分**</span><span class="sxs-lookup"><span data-stu-id="efb7e-142">Estimated time to complete this lab: **60 minutes**</span></span>

> [!NOTE]
> <span data-ttu-id="efb7e-143">Visual Studio を初めて起動すると、定義済みの設定のコレクションの 1 つを選択する必要があります。</span><span class="sxs-lookup"><span data-stu-id="efb7e-143">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="efb7e-144">定義済みの各コレクションは、特定の開発スタイルに一致するように設計されていて、ウィンドウのレイアウト、エディターの動作、IntelliSense コード スニペット、およびダイアログ ボックスのオプションを決定します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-144">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="efb7e-145">このラボの手順を使用する場合は、Visual Studio で特定のタスクを実行するために必要な操作を記述する、**汎用開発設定**コレクション。</span><span class="sxs-lookup"><span data-stu-id="efb7e-145">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="efb7e-146">開発環境のさまざまな設定のコレクションを選択する場合、考慮する必要がある手順に違いがあります。</span><span class="sxs-lookup"><span data-stu-id="efb7e-146">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a><span data-ttu-id="efb7e-147">手順 1: 新しい Web フォーム プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-147">Exercise 1: Creating a New Web Forms Project</span></span>

<span data-ttu-id="efb7e-148">この演習では、Visual Studio 2013 を使用して新しい Web フォーム サイトを作成する、 **One ASP.NET**同じアプリケーションで Web フォーム、MVC、Web API のコンポーネントを簡単に統合することができるプロジェクト エクスペリエンスの統合します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-148">In this exercise you will create a new Web Forms site in Visual Studio 2013 using the **One ASP.NET** unified project experience, which will allow you to easily integrate Web Forms, MVC and Web API components in the same application.</span></span> <span data-ttu-id="efb7e-149">生成されたソリューションを調査して、その部分を特定し、最後に、Web サイトのアクションが表示されます。</span><span class="sxs-lookup"><span data-stu-id="efb7e-149">You will then explore the generated solution and identify its parts, and finally you will see the Web site in action.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a><span data-ttu-id="efb7e-150">タスク 1 – 1 つの ASP.NET 機能を使用して新しいサイトを作成します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-150">Task 1 – Creating a New Site Using the One ASP.NET Experience</span></span>

<span data-ttu-id="efb7e-151">最初にこのタスクでは Visual Studio で新しい Web サイトの作成に基づく、 **One ASP.NET**プロジェクトの種類。</span><span class="sxs-lookup"><span data-stu-id="efb7e-151">In this task you will start creating a new Web site in Visual Studio based on the **One ASP.NET** project type.</span></span> <span data-ttu-id="efb7e-152">**1 つの ASP.NET**すべての ASP.NET のテクノロジの統合し、を混在させるし、必要に応じてとに一致させることができます。</span><span class="sxs-lookup"><span data-stu-id="efb7e-152">**One ASP.NET** unifies all ASP.NET technologies and gives you the option to mix and match them as desired.</span></span> <span data-ttu-id="efb7e-153">アプリケーション内で並行して、ライブ Web フォーム、MVC、Web API の別のコンポーネントを識別します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-153">You will then recognize the different components of Web Forms, MVC and Web API that live side by side within your application.</span></span>

1. <span data-ttu-id="efb7e-154">開いている**Visual Studio Express 2013 for Web**選択**ファイル |新しいプロジェクト.** 新しいソリューションを開始します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-154">Open **Visual Studio Express 2013 for Web** and select **File | New Project...** to start a new solution.</span></span>

    ![新規プロジェクトの作成](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    <span data-ttu-id="efb7e-156">*新しいプロジェクトを作成します。*</span><span class="sxs-lookup"><span data-stu-id="efb7e-156">*Creating a New Project*</span></span>
2. <span data-ttu-id="efb7e-157">**新しいプロジェクト**ダイアログ ボックスで、 **ASP.NET Web アプリケーション**下、 **(Visual C#) |Web**タブをクリックし、確認 **.NET Framework 4.5**が選択されています。</span><span class="sxs-lookup"><span data-stu-id="efb7e-157">In the **New Project** dialog box, select **ASP.NET Web Application** under the **Visual C# | Web** tab, and make sure **.NET Framework 4.5** is selected.</span></span> <span data-ttu-id="efb7e-158">プロジェクトの名前*MyHybridSite*、選択、**場所** をクリック**OK**します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-158">Name the project *MyHybridSite*, choose a **Location** and click **OK**.</span></span>

    ![新しい ASP.NET Web アプリケーション プロジェクト](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    <span data-ttu-id="efb7e-160">*新しい ASP.NET Web アプリケーション プロジェクトを作成します。*</span><span class="sxs-lookup"><span data-stu-id="efb7e-160">*Creating a new ASP.NET Web Application project*</span></span>
3. <span data-ttu-id="efb7e-161">**新しい ASP.NET プロジェクト**ダイアログ ボックスで、 **Web フォーム**テンプレートと選択、 **MVC**と**Web API**オプション。</span><span class="sxs-lookup"><span data-stu-id="efb7e-161">In the **New ASP.NET Project** dialog box, select the **Web Forms** template and select the **MVC** and **Web API** options.</span></span> <span data-ttu-id="efb7e-162">確認、**認証**にオプションが設定されている**個々 のユーザー アカウント**します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-162">Also, make sure that the **Authentication** option is set to **Individual User Accounts**.</span></span> <span data-ttu-id="efb7e-163">**[OK]** をクリックして続行します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-163">Click **OK** to continue.</span></span>

    ![Web API と MVC のコンポーネントを含め、Web フォーム テンプレートに新しいプロジェクトを作成します。](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    <span data-ttu-id="efb7e-165">*Web API と MVC のコンポーネントを含め、Web フォーム テンプレートに新しいプロジェクトを作成します。*</span><span class="sxs-lookup"><span data-stu-id="efb7e-165">*Creating a new project with the Web Forms template, including Web API and MVC components*</span></span>
4. <span data-ttu-id="efb7e-166">生成されたソリューションの構造を利用できるようになりました。</span><span class="sxs-lookup"><span data-stu-id="efb7e-166">You can now explore the structure of the generated solution.</span></span>

    ![生成されたソリューションの調査](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    <span data-ttu-id="efb7e-168">*生成されたソリューションの調査*</span><span class="sxs-lookup"><span data-stu-id="efb7e-168">*Exploring the generated solution*</span></span>

    1. <span data-ttu-id="efb7e-169">**アカウント:** このフォルダーには、登録にログインし、アプリケーションのユーザー アカウントを管理する Web フォーム ページが含まれています。</span><span class="sxs-lookup"><span data-stu-id="efb7e-169">**Account:** This folder contains the Web Form pages to register, log in to and manage the application's user accounts.</span></span> <span data-ttu-id="efb7e-170">このフォルダーが追加されたときに、**個々 のユーザー アカウント**Web フォーム プロジェクト テンプレートの構成時に認証オプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-170">This folder is added when the **Individual User Accounts** authentication option is selected during the configuration of the Web Forms project template.</span></span>
    2. <span data-ttu-id="efb7e-171">**モデルの**このフォルダーは、アプリケーション データを表すクラスが格納されます。</span><span class="sxs-lookup"><span data-stu-id="efb7e-171">**Models:** This folder will contain the classes that represent your application data.</span></span>
    3. <span data-ttu-id="efb7e-172">**コント ローラー**と**ビュー**: これらのフォルダーに必要な**ASP.NET MVC**と**ASP.NET Web API**コンポーネント。</span><span class="sxs-lookup"><span data-stu-id="efb7e-172">**Controllers** and **Views**: These folders are required for the **ASP.NET MVC** and **ASP.NET Web API** components.</span></span> <span data-ttu-id="efb7e-173">次の演習で MVC と Web API のテクノロジについて学びます。</span><span class="sxs-lookup"><span data-stu-id="efb7e-173">You will explore the MVC and Web API technologies in the next exercises.</span></span>
    4. <span data-ttu-id="efb7e-174">**Default.aspx**、 **Contact.aspx**と**About.aspx**ファイルに固有のページを構築する開始点として使用できる定義済みの Web フォーム ページは、アプリケーション。</span><span class="sxs-lookup"><span data-stu-id="efb7e-174">The **Default.aspx**, **Contact.aspx** and **About.aspx** files are pre-defined Web Form pages that you can use as starting points to build the pages specific to your application.</span></span> <span data-ttu-id="efb7e-175">呼ばれる別のファイルが存在するそれらのファイルのプログラミング ロジック、&quot;コード ビハインド&quot;ファイルは、 &quot;..aspx.vb ファイル&quot;または&quot;. aspx.cs&quot;拡張機能 (に応じて、使用される言語)。</span><span class="sxs-lookup"><span data-stu-id="efb7e-175">The programming logic of those files resides in a separate file referred to as the &quot;code-behind&quot; file, which has an &quot;.aspx.vb&quot; or &quot;.aspx.cs&quot; extension (depending on the language used).</span></span> <span data-ttu-id="efb7e-176">分離コード ロジックは、サーバー上で実行し、動的に、ページの HTML 出力を生成します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-176">The code-behind logic runs on the server and dynamically produces the HTML output for your page.</span></span>
    5. <span data-ttu-id="efb7e-177">**Site.Master**と**Site.Mobile.Master**ページは、アプリケーションで、外観とすべてのページの標準の動作を定義します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-177">The **Site.Master** and **Site.Mobile.Master** pages define the look and feel and the standard behavior of all the pages in the application.</span></span>
5. <span data-ttu-id="efb7e-178">ダブルクリックして、 **Default.aspx**ファイル ページの内容を調査します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-178">Double-click the **Default.aspx** file to explore the content of the page.</span></span>

    ![Default.aspx ページの調査](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    <span data-ttu-id="efb7e-180">*Default.aspx ページの調査*</span><span class="sxs-lookup"><span data-stu-id="efb7e-180">*Exploring the Default.aspx page*</span></span>

    > [!NOTE]
    > <span data-ttu-id="efb7e-181">**ページ**ファイルの上部にあるディレクティブは、Web フォーム ページの属性を定義します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-181">The **Page** directive at the top of the file defines the attributes of the Web Forms page.</span></span> <span data-ttu-id="efb7e-182">など、 **MasterPageFile**属性をマスターへのパスを指定します - この場合、ページ、 *Site.Master*ページ - と**Inherits**属性を定義します、継承するページの分離コード クラスです。</span><span class="sxs-lookup"><span data-stu-id="efb7e-182">For example, the **MasterPageFile** attribute specifies the path to the master page -in this case, the *Site.Master* page- and the **Inherits** attribute defines the code-behind class for the page to inherit.</span></span> <span data-ttu-id="efb7e-183">このクラスは、によって決まりますファイルにある、**分離コード**属性。</span><span class="sxs-lookup"><span data-stu-id="efb7e-183">This class is located in the file determined by the **CodeBehind** attribute.</span></span>
    > 
    > <span data-ttu-id="efb7e-184">**Asp:content**コントロール (テキスト、マークアップとコントロール) のページの実際の内容を保持およびにマップされて、 **asp: ContentPlaceHolder**マスター ページのコントロール。</span><span class="sxs-lookup"><span data-stu-id="efb7e-184">The **asp:Content** control holds the actual content of the page (text, markup and controls) and is mapped to a **asp:ContentPlaceHolder** control on the master page.</span></span> <span data-ttu-id="efb7e-185">ページのコンテンツ内にレンダリングされるここで、 *MainContent*で定義されているコントロール、 *Site.Master*ページ。</span><span class="sxs-lookup"><span data-stu-id="efb7e-185">In this case, the page content will be rendered inside the *MainContent* control defined in the *Site.Master* page.</span></span>
6. <span data-ttu-id="efb7e-186">展開、**アプリ\_開始**フォルダーに注目してください、 **WebApiConfig.cs**ファイル。</span><span class="sxs-lookup"><span data-stu-id="efb7e-186">Expand the **App\_Start** folder and notice the **WebApiConfig.cs** file.</span></span> <span data-ttu-id="efb7e-187">Visual Studio には、1 つの ASP.NET テンプレートを使用して、プロジェクトを構成するときに、Web API が含まれるために、生成されたソリューションにそのファイルが含まれます。</span><span class="sxs-lookup"><span data-stu-id="efb7e-187">Visual Studio included that file in the generated solution because you included Web API when configuring your project with the One ASP.NET template.</span></span>
7. <span data-ttu-id="efb7e-188">開く、 **WebApiConfig.cs**ファイル。</span><span class="sxs-lookup"><span data-stu-id="efb7e-188">Open the **WebApiConfig.cs** file.</span></span> <span data-ttu-id="efb7e-189">*WebApiConfig*マップ HTTP Web API に関連付けられている構成が表示されますクラスにルーティング**Web API コント ローラー**します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-189">In the *WebApiConfig* class you will find the configuration associated with Web API, which maps HTTP routes to **Web API controllers**.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. <span data-ttu-id="efb7e-190">開く、 **RouteConfig.cs**ファイル。</span><span class="sxs-lookup"><span data-stu-id="efb7e-190">Open the **RouteConfig.cs** file.</span></span> <span data-ttu-id="efb7e-191">内で、 *RegisterRoutes*メソッドへの HTTP ルートをマップする MVC と関連付けられている構成が表示されます**MVC コント ローラー**します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-191">Inside the *RegisterRoutes* method you will find the configuration associated with MVC, which maps HTTP routes to **MVC controllers**.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="efb7e-192">タスク 2 – ソリューションの実行</span><span class="sxs-lookup"><span data-stu-id="efb7e-192">Task 2 – Running the Solution</span></span>

<span data-ttu-id="efb7e-193">生成されたソリューションを実行がこのタスクでされたら、アプリといくつかの URL の書き換えや組み込みの認証など、その機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-193">In this task you will run the generated solution, explore the app and some of its features, like URL rewriting and built-in authentication.</span></span>

1. <span data-ttu-id="efb7e-194">キーを押して、ソリューションを実行する**F5**  をクリックしてまたは、**開始**ボタンがツールバーにあります。</span><span class="sxs-lookup"><span data-stu-id="efb7e-194">To run the solution, press **F5** or click the **Start** button located on the toolbar.</span></span> <span data-ttu-id="efb7e-195">アプリケーションのホーム ページは、ブラウザーで開く必要があります。</span><span class="sxs-lookup"><span data-stu-id="efb7e-195">The application home page should open in the browser.</span></span>

    ![ソリューションの実行](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. <span data-ttu-id="efb7e-197">Web フォーム ページが呼び出されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-197">Verify that the Web Forms pages are being invoked.</span></span> <span data-ttu-id="efb7e-198">これを行うには、次のように追加します。 **/contact.aspx**キーを押して、アドレス バーに URL を**Enter**します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-198">To do this, append **/contact.aspx** to the URL in the address bar and press **Enter**.</span></span>

    ![フレンドリな Url](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    <span data-ttu-id="efb7e-200">*フレンドリな Url*</span><span class="sxs-lookup"><span data-stu-id="efb7e-200">*Friendly URLs*</span></span>

    > [!NOTE]
    > <span data-ttu-id="efb7e-201">ご覧のように、URL 変更**にお問い合わせください/** します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-201">As you can see, the URL changes to **/contact**.</span></span> <span data-ttu-id="efb7e-202">始まる**ASP.NET 4**、URL ルーティング機能は、Web フォームに追加された、Url などの記述できるように*[ http://www.mysite.com/products/software ](http://www.mysite.com/products/software)* の代わりに *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*.</span><span class="sxs-lookup"><span data-stu-id="efb7e-202">Starting from **ASP.NET 4**, URL routing capabilities were added to Web Forms, so you can write URLs like *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)* instead of *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*.</span></span> <span data-ttu-id="efb7e-203">詳細についてを参照してください[URL ルーティング](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md)します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-203">For more information refer to [URL Routing](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).</span></span>
3. <span data-ttu-id="efb7e-204">これで、アプリケーションに統合認証フローについて学びます。</span><span class="sxs-lookup"><span data-stu-id="efb7e-204">You will now explore the authentication flow integrated into the application.</span></span> <span data-ttu-id="efb7e-205">これを行うには、次のようにクリックします。**登録**ページの右上隅にします。</span><span class="sxs-lookup"><span data-stu-id="efb7e-205">To do this, click **Register** in the upper-right corner of the page.</span></span>

    ![新しいユーザーを登録します。](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    <span data-ttu-id="efb7e-207">*新しいユーザーを登録します。*</span><span class="sxs-lookup"><span data-stu-id="efb7e-207">*Registering a new user*</span></span>
4. <span data-ttu-id="efb7e-208">**登録**ページで、入力、**ユーザー名**と**パスワード**、順にクリックします**登録**します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-208">In the **Register** page, enter a **User name** and **Password**, and then click **Register**.</span></span>

    ![登録 ページ](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    <span data-ttu-id="efb7e-210">*登録 ページ*</span><span class="sxs-lookup"><span data-stu-id="efb7e-210">*Register page*</span></span>
5. <span data-ttu-id="efb7e-211">アプリケーションが、新しいアカウントを登録し、ユーザーを認証します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-211">The application registers the new account, and the user is authenticated.</span></span>

    ![認証されたユーザー](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    <span data-ttu-id="efb7e-213">*認証されたユーザー*</span><span class="sxs-lookup"><span data-stu-id="efb7e-213">*User authenticated*</span></span>
6. <span data-ttu-id="efb7e-214">Visual Studio とキーを押してに戻って**shift キーを押しながら f5 キーを押して**デバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-214">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a><span data-ttu-id="efb7e-215">手順 2: スキャフォールディングを使用した MVC コント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-215">Exercise 2: Creating an MVC Controller Using Scaffolding</span></span>

<span data-ttu-id="efb7e-216">この演習では、アクションと Razor ビューを 1 行のコードを記述することがなく、CRUD 操作を実行すると、ASP.NET MVC 5 コント ローラーを作成する Visual Studio で提供される ASP.NET スキャフォールディング フレームワークの利点がかかります。</span><span class="sxs-lookup"><span data-stu-id="efb7e-216">In this exercise you will take advantage of the ASP.NET Scaffolding framework provided by Visual Studio to create an ASP.NET MVC 5 controller with actions and Razor views to perform CRUD operations, without writing a single line of code.</span></span> <span data-ttu-id="efb7e-217">スキャフォールディングのプロセスは、SQL database にデータ コンテキストおよびデータベース スキーマを生成するのに Entity Framework Code First では使用します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-217">The scaffolding process will use Entity Framework Code First to generate the data context and the database schema in the SQL database.</span></span>

<span data-ttu-id="efb7e-218">**Entity Framework の Code First**</span><span class="sxs-lookup"><span data-stu-id="efb7e-218">**About Entity Framework Code First**</span></span>

<span data-ttu-id="efb7e-219">Entity Framework (EF) は、リレーショナル ストレージ スキーマを使用して直接プログラミングではなく、概念アプリケーション モデルを使用したプログラミングによってデータ アクセス アプリケーションを作成できるようにするオブジェクト リレーショナル マッパー (ORM) です。</span><span class="sxs-lookup"><span data-stu-id="efb7e-219">Entity Framework (EF) is an object-relational mapper (ORM) that enables you to create data access applications by programming with a conceptual application model instead of programming directly using a relational storage schema.</span></span>

<span data-ttu-id="efb7e-220">Entity Framework Code First のモデリング ワークフローできますこのクエリを実行するには、実行するときに、EF が依存するモデルを表す独自のドメイン クラスを使用する関数の変更の追跡と更新。</span><span class="sxs-lookup"><span data-stu-id="efb7e-220">The Entity Framework Code First modeling workflow allows you to use your own domain classes to represent the model that EF relies on when performing querying, change-tracking and updating functions.</span></span> <span data-ttu-id="efb7e-221">Code First の開発ワークフローを使用する必要はありませんデータベースを作成するか、スキーマを指定することによって、アプリケーションを開始します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-221">Using the Code First development workflow, you do not need to begin your application by creating a database or specifying a schema.</span></span> <span data-ttu-id="efb7e-222">代わりに、アプリケーションの最も適切なドメイン モデル オブジェクトを定義する標準の .NET クラスを記述することができ、Entity Framework のデータベースが作成されます。</span><span class="sxs-lookup"><span data-stu-id="efb7e-222">Instead, you can write standard .NET classes that define the most appropriate domain model objects for your application, and Entity Framework will create the database for you.</span></span>

> [!NOTE]
> <span data-ttu-id="efb7e-223">Entity Framework に関する詳細については、[ここ](../../../entity-framework.md)します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-223">You can learn more about Entity Framework [here](../../../entity-framework.md).</span></span>


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a><span data-ttu-id="efb7e-224">タスク 1 – 新しいモデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-224">Task 1 – Creating a New Model</span></span>

<span data-ttu-id="efb7e-225">ここで定義する、**人**クラスは、MVC コント ローラーとビューを作成するスキャフォールディングのプロセスによって使用されるモデルになります。</span><span class="sxs-lookup"><span data-stu-id="efb7e-225">You will now define a **Person** class, which will be the model used by the scaffolding process to create the MVC controller and the views.</span></span> <span data-ttu-id="efb7e-226">作成から始めます、 **Person**モデル クラス、およびコント ローラーでの CRUD 操作が自動的に作成スキャフォールディング機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-226">You will start by creating a **Person** model class, and the CRUD operations in the controller will be automatically created using scaffolding features.</span></span>

1. <span data-ttu-id="efb7e-227">開いている**Visual Studio Express 2013 for Web**と**MyHybridSite.sln**ソリューション、**ソース/Ex2-MvcScaffolding/開始**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="efb7e-227">Open **Visual Studio Express 2013 for Web** and the **MyHybridSite.sln** solution located in the **Source/Ex2-MvcScaffolding/Begin** folder.</span></span> <span data-ttu-id="efb7e-228">または、前の手順で取得したソリューションを続行できます。</span><span class="sxs-lookup"><span data-stu-id="efb7e-228">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="efb7e-229">**ソリューション エクスプ ローラー**を右クリックし、**モデル**のフォルダー、 **MyHybridSite**順に選択して**追加 |クラス.**.</span><span class="sxs-lookup"><span data-stu-id="efb7e-229">In **Solution Explorer**, right-click the **Models** folder of the **MyHybridSite** project and select **Add | Class...**.</span></span>

    ![Person モデル クラスを追加します。](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    <span data-ttu-id="efb7e-231">*Person モデル クラスを追加します。*</span><span class="sxs-lookup"><span data-stu-id="efb7e-231">*Adding the Person model class*</span></span>
3. <span data-ttu-id="efb7e-232">**新しい項目の追加** ダイアログ ボックスで、ファイル名で*Person.cs*  をクリック**追加**します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-232">In the **Add New Item** dialog box, name the file *Person.cs* and click **Add**.</span></span>

    ![Person モデル クラスを作成します。](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    <span data-ttu-id="efb7e-234">*Person モデル クラスを作成します。*</span><span class="sxs-lookup"><span data-stu-id="efb7e-234">*Creating the Person model class*</span></span>
4. <span data-ttu-id="efb7e-235">内容が置換、 **Person.cs**を次のコード ファイル。</span><span class="sxs-lookup"><span data-stu-id="efb7e-235">Replace the content of the **Person.cs** file with the following code.</span></span> <span data-ttu-id="efb7e-236">キーを押して**CTRL + S**変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-236">Press **CTRL + S** to save the changes.</span></span>

    <span data-ttu-id="efb7e-237">(コード スニペット - *BringingTogetherOneAspNet - Ex2 - PersonClass*)</span><span class="sxs-lookup"><span data-stu-id="efb7e-237">(Code Snippet - *BringingTogetherOneAspNet - Ex2 - PersonClass*)</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. <span data-ttu-id="efb7e-238">**ソリューション エクスプ ローラー**を右クリックし、 **MyHybridSite**順に選択して**ビルド**、またはキーを押します**CTRL + SHIFT + B**プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="efb7e-238">In **Solution Explorer**, right-click the **MyHybridSite** project and select **Build**, or press **CTRL + SHIFT + B** to build the project.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a><span data-ttu-id="efb7e-239">タスク 2 – MVC コント ローラーを作成</span><span class="sxs-lookup"><span data-stu-id="efb7e-239">Task 2 – Creating an MVC Controller</span></span>

<span data-ttu-id="efb7e-240">これで、 **Person**モデルを作成、CRUD コント ローラー アクションとビューを作成する Entity Framework と ASP.NET MVC のスキャフォールディングを使用する**人**します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-240">Now that the **Person** model is created, you will use ASP.NET MVC scaffolding with Entity Framework to create the CRUD controller actions and views for **Person**.</span></span>

1. <span data-ttu-id="efb7e-241">**ソリューション エクスプ ローラー**を右クリックし、**コント ローラー**のフォルダー、 **MyHybridSite**順に選択して**追加 |新規スキャフォールディング アイテム.**.</span><span class="sxs-lookup"><span data-stu-id="efb7e-241">In **Solution Explorer**, right-click the **Controllers** folder of the **MyHybridSite** project and select **Add | New Scaffolded Item...**.</span></span>

    ![新しいスキャフォールディングされたコント ローラーの作成](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    <span data-ttu-id="efb7e-243">*新しいスキャフォールディングされたコント ローラーの作成*</span><span class="sxs-lookup"><span data-stu-id="efb7e-243">*Creating a new Scaffolded Controller*</span></span>
2. <span data-ttu-id="efb7e-244">**スキャフォールディングの追加**ダイアログ ボックスで、 **MVC 5 コント ローラーとビュー、Entity Framework を使用して** をクリックし、**追加します。**</span><span class="sxs-lookup"><span data-stu-id="efb7e-244">In the **Add Scaffold** dialog box, select **MVC 5 Controller with views, using Entity Framework** and then click **Add.**</span></span>

    ![ビューと Entity Framework を使用した MVC 5 コント ローラーを選択](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    <span data-ttu-id="efb7e-246">*ビューと Entity Framework を使用した MVC 5 コント ローラーを選択*</span><span class="sxs-lookup"><span data-stu-id="efb7e-246">*Selecting MVC 5 Controller with views and Entity Framework*</span></span>
3. <span data-ttu-id="efb7e-247">設定*MvcPersonController*として、**コント ローラー名**を選択、**非同期コント ローラー アクションを使用して、** オプションし、選択**人 (MyHybridSite.Models)** として、**モデル クラス**します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-247">Set *MvcPersonController* as the **Controller name**, select the **Use async controller actions** option and select **Person (MyHybridSite.Models)** as the **Model class**.</span></span>

    ![スキャフォールディングある MVC コント ローラーを追加します。](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    <span data-ttu-id="efb7e-249">*スキャフォールディングある MVC コント ローラーを追加します。*</span><span class="sxs-lookup"><span data-stu-id="efb7e-249">*Adding an MVC controller with scaffolding*</span></span>
4. <span data-ttu-id="efb7e-250">[**データ コンテキスト クラス**、] をクリックして**新しいデータ コンテキスト.**.</span><span class="sxs-lookup"><span data-stu-id="efb7e-250">Under **Data context class**, click **New data context...**.</span></span>

    ![新しいデータ コンテキストを作成します。](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    <span data-ttu-id="efb7e-252">*新しいデータ コンテキストを作成します。*</span><span class="sxs-lookup"><span data-stu-id="efb7e-252">*Creating a new data context*</span></span>
5. <span data-ttu-id="efb7e-253">**新しいデータ コンテキスト** ダイアログ ボックスで新しいデータ コンテキスト、名前*PersonContext*クリック**追加**します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-253">In the **New Data Context** dialog box, name the new data context *PersonContext* and click **Add**.</span></span>

    ![新しい PersonContext を作成します。](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    <span data-ttu-id="efb7e-255">*新しい PersonContext 型を作成します。*</span><span class="sxs-lookup"><span data-stu-id="efb7e-255">*Creating the new PersonContext type*</span></span>
6. <span data-ttu-id="efb7e-256">をクリックして**追加**の新しいコント ローラーを作成する**Person**スキャフォールディングとします。</span><span class="sxs-lookup"><span data-stu-id="efb7e-256">Click **Add** to create the new controller for **Person** with scaffolding.</span></span> <span data-ttu-id="efb7e-257">Visual Studio が生成されます、コント ローラー アクション、個人のデータ コンテキストおよび Razor ビュー。</span><span class="sxs-lookup"><span data-stu-id="efb7e-257">Visual Studio will then generate the controller actions, the Person data context and the Razor views.</span></span>

    ![MVC コント ローラーをスキャフォールディングを作成した後](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    <span data-ttu-id="efb7e-259">*MVC コント ローラーをスキャフォールディングを作成した後*</span><span class="sxs-lookup"><span data-stu-id="efb7e-259">*After creating the MVC controller with scaffolding*</span></span>
7. <span data-ttu-id="efb7e-260">開く、 **MvcPersonController.cs**ファイル、**コント ローラー**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="efb7e-260">Open the **MvcPersonController.cs** file in the **Controllers** folder.</span></span> <span data-ttu-id="efb7e-261">CRUD アクション メソッドが自動的に生成されたことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="efb7e-261">Notice that the CRUD action methods have been generated automatically.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > <span data-ttu-id="efb7e-262">選択して、**非同期コント ローラー アクションを使用して、** スキャフォールディングからのチェック ボックスは、前の手順でオプション、Visual Studio には、ユーザーのデータ コンテキストへのアクセスに関連するすべてのアクションの非同期アクション メソッドが生成されます。</span><span class="sxs-lookup"><span data-stu-id="efb7e-262">By selecting the **Use async controller actions** check box from the scaffolding options in the previous steps, Visual Studio generates asynchronous action methods for all actions that involve access to the Person data context.</span></span> <span data-ttu-id="efb7e-263">実行時間の長い非同期アクション メソッドを使用する、CPU 以外に、要求の処理中に作業を実行してから、Web サーバーがブロックされないようにする要求がバインドされていることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="efb7e-263">It is recommended that you use asynchronous action methods for long-running, non-CPU bound requests to avoid blocking the Web server from performing work while the request is being processed.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a><span data-ttu-id="efb7e-264">タスク 3 – ソリューションの実行</span><span class="sxs-lookup"><span data-stu-id="efb7e-264">Task 3 – Running the Solution</span></span>

<span data-ttu-id="efb7e-265">このタスクで実行することを確認するには、もう一度ソリューションのビュー **Person**想定どおりに動作します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-265">In this task, you will run the solution again to verify that the views for **Person** are working as expected.</span></span> <span data-ttu-id="efb7e-266">データベースに正常に保存することを確認する新しいユーザーを追加します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-266">You will add a new person to verify that it is successfully saved to the database.</span></span>

1. <span data-ttu-id="efb7e-267">キーを押して**F5**ソリューションを実行します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-267">Press **F5** to run the solution.</span></span>
2. <span data-ttu-id="efb7e-268">移動します **/MvcPerson**します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-268">Navigate to **/MvcPerson**.</span></span> <span data-ttu-id="efb7e-269">ユーザーの一覧を表示するスキャフォールディングされたビューが表示されます。</span><span class="sxs-lookup"><span data-stu-id="efb7e-269">The scaffolded view that shows the list of people should appear.</span></span>
3. <span data-ttu-id="efb7e-270">クリックして**新規作成**新しいメンバーを追加します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-270">Click **Create New** to add a new person.</span></span>

    ![スキャフォールディングの MVC ビューへの移動](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    <span data-ttu-id="efb7e-272">*スキャフォールディングの MVC ビューへの移動*</span><span class="sxs-lookup"><span data-stu-id="efb7e-272">*Navigating to the scaffolded MVC views*</span></span>
4. <span data-ttu-id="efb7e-273">**作成**ビューで、提供、**名前**と**年齢**人をクリックします**作成**です。</span><span class="sxs-lookup"><span data-stu-id="efb7e-273">In the **Create** view, provide a **Name** and an **Age** for the person, and click **Create**.</span></span>

    ![新しいユーザーを追加します。](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    <span data-ttu-id="efb7e-275">*新しいユーザーを追加します。*</span><span class="sxs-lookup"><span data-stu-id="efb7e-275">*Adding a new person*</span></span>
5. <span data-ttu-id="efb7e-276">新しいユーザーは、一覧に追加されます。</span><span class="sxs-lookup"><span data-stu-id="efb7e-276">The new person is added to the list.</span></span> <span data-ttu-id="efb7e-277">要素の一覧でクリックして**詳細**人の詳細ビューを表示します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-277">In the element list, click **Details** to display the person's details view.</span></span> <span data-ttu-id="efb7e-278">次に、**詳細**ビューで、をクリックして**リストに戻る**リスト ビューに戻ります。</span><span class="sxs-lookup"><span data-stu-id="efb7e-278">Then, in the **Details** view, click **Back to List** to go back to the list view.</span></span>

    ![ユーザーの詳細ビュー](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    <span data-ttu-id="efb7e-280">*ユーザーの詳細ビュー*</span><span class="sxs-lookup"><span data-stu-id="efb7e-280">*Person's details view*</span></span>
6. <span data-ttu-id="efb7e-281">をクリックして、**削除**ユーザーを削除するリンク。</span><span class="sxs-lookup"><span data-stu-id="efb7e-281">Click the **Delete** link to delete the person.</span></span> <span data-ttu-id="efb7e-282">**削除**表示で、**削除**操作を確定します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-282">In the **Delete** view, click **Delete** to confirm the operation.</span></span>

    ![ユーザーを削除します。](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    <span data-ttu-id="efb7e-284">*ユーザーを削除します。*</span><span class="sxs-lookup"><span data-stu-id="efb7e-284">*Deleting a person*</span></span>
7. <span data-ttu-id="efb7e-285">Visual Studio とキーを押してに戻って**shift キーを押しながら f5 キーを押して**デバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-285">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a><span data-ttu-id="efb7e-286">手順 3: スキャフォールディングを使用して Web API コント ローラーの作成</span><span class="sxs-lookup"><span data-stu-id="efb7e-286">Exercise 3: Creating a Web API Controller Using Scaffolding</span></span>

<span data-ttu-id="efb7e-287">Web API フレームワークは、ASP.NET スタックの一部であり、容易に実装する HTTP services 一般に RESTful API を使用して JSON または XML 形式のデータを送受信するように設計します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-287">The Web API framework is part of the ASP.NET Stack and designed to make implementing HTTP services easier, generally sending and receiving JSON- or XML-formatted data through a RESTful API.</span></span>

<span data-ttu-id="efb7e-288">この演習では、Web API コント ローラーを生成するのに ASP.NET スキャフォールディングをもう一度使用されます。</span><span class="sxs-lookup"><span data-stu-id="efb7e-288">In this exercise, you will use ASP.NET Scaffolding again to generate a Web API controller.</span></span> <span data-ttu-id="efb7e-289">同じ使用する**人**と**PersonContext**クラスを前の演習を JSON 形式で同じ個人データを提供します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-289">You will use the same **Person** and **PersonContext** classes from the previous exercise to provide the same person data in JSON format.</span></span> <span data-ttu-id="efb7e-290">同じ ASP.NET アプリケーション内でさまざまな方法で、同じリソースを公開する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="efb7e-290">You will see how you can expose the same resources in different ways within the same ASP.NET application.</span></span>

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a><span data-ttu-id="efb7e-291">タスク 1 – Web API コント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-291">Task 1 – Creating a Web API Controller</span></span>

<span data-ttu-id="efb7e-292">このタスクでは、新規に作成する**Web API コント ローラー** JSON のようにコンピューターが使用できる形式で個人データを公開します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-292">In this task you will create a new **Web API Controller** that will expose the person data in a machine-consumable format like JSON.</span></span>

1. <span data-ttu-id="efb7e-293">これを開いていない場合は開きます**Visual Studio Express 2013 for Web**を開くと、 **MyHybridSite.sln**ソリューション、**ソース/Ex3-WebAPI/開始**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="efb7e-293">If not already opened, open **Visual Studio Express 2013 for Web** and open the **MyHybridSite.sln** solution located in the **Source/Ex3-WebAPI/Begin** folder.</span></span> <span data-ttu-id="efb7e-294">または、前の手順で取得したソリューションを続行できます。</span><span class="sxs-lookup"><span data-stu-id="efb7e-294">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>

    > [!NOTE]
    > <span data-ttu-id="efb7e-295">手順 3 から Begin ソリューションを起動した場合、以下のキーを押して**CTRL + SHIFT + B**ソリューションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="efb7e-295">If you start with the Begin solution from Exercise 3, press **CTRL + SHIFT + B** to build the solution.</span></span>
2. <span data-ttu-id="efb7e-296">**ソリューション エクスプ ローラー**を右クリックし、**コント ローラー**のフォルダー、 **MyHybridSite**順に選択して**追加 |新規スキャフォールディング アイテム.**.</span><span class="sxs-lookup"><span data-stu-id="efb7e-296">In **Solution Explorer**, right-click the **Controllers** folder of the **MyHybridSite** project and select **Add | New Scaffolded Item...**.</span></span>

    ![新しいスキャフォールディングされたコント ローラーの作成](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    <span data-ttu-id="efb7e-298">*新しいスキャフォールディングされたコント ローラーの作成*</span><span class="sxs-lookup"><span data-stu-id="efb7e-298">*Creating a new scaffolded Controller*</span></span>
3. <span data-ttu-id="efb7e-299">**スキャフォールディングの追加**ダイアログ ボックスで、 **Web API**し、左側のウィンドウで**Web API 2 コント ローラー アクション、Entity Framework を使用して**中央のペインで順にクリックします**追加します。**</span><span class="sxs-lookup"><span data-stu-id="efb7e-299">In the **Add Scaffold** dialog box, select **Web API** in the left pane, then **Web API 2 Controller with actions, using Entity Framework** in the middle pane and then click **Add.**</span></span>

    <span data-ttu-id="efb7e-300">![アクションと Entity Framework を使用した Web API 2 コント ローラーを選択](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "を選択すると Web API 2 コント ローラー アクションと Entity Framework")</span><span class="sxs-lookup"><span data-stu-id="efb7e-300">![Selecting Web API 2 Controller with actions and Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "Selecting Web API 2 Controller with actions and Entity Framework")</span></span>

    <span data-ttu-id="efb7e-301">*アクションと Entity Framework を使用した Web API 2 コント ローラーを選択*</span><span class="sxs-lookup"><span data-stu-id="efb7e-301">*Selecting Web API 2 Controller with actions and Entity Framework*</span></span>
4. <span data-ttu-id="efb7e-302">設定*ApiPersonController*として、**コント ローラー名**を選択、**非同期コント ローラー アクションを使用して、** オプションし、選択**人 (MyHybridSite.Models)** と**PersonContext (MyHybridSite.Models)** として、**モデル**と**データ コンテキスト**それぞれクラスします。</span><span class="sxs-lookup"><span data-stu-id="efb7e-302">Set *ApiPersonController* as the **Controller name**, select the **Use async controller actions** option and select **Person (MyHybridSite.Models)** and **PersonContext (MyHybridSite.Models)** as the **Model** and **Data context** classes respectively.</span></span> <span data-ttu-id="efb7e-303">次に、 **[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="efb7e-303">Then click **Add**.</span></span>

    <span data-ttu-id="efb7e-304">![スキャフォールディングで Web API コント ローラーの追加](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "スキャフォールディングと Web API コント ローラーの追加")</span><span class="sxs-lookup"><span data-stu-id="efb7e-304">![Adding a Web API Controller with scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "Adding a Web API controller with scaffolding")</span></span>

    <span data-ttu-id="efb7e-305">*スキャフォールディングで Web API コント ローラーの追加*</span><span class="sxs-lookup"><span data-stu-id="efb7e-305">*Adding a Web API controller with scaffolding*</span></span>
5. <span data-ttu-id="efb7e-306">Visual Studio が生成し、 **ApiPersonController**データを操作する 4 つの CRUD 操作を持つクラス。</span><span class="sxs-lookup"><span data-stu-id="efb7e-306">Visual Studio will then generate the **ApiPersonController** class with the four CRUD actions to work with your data.</span></span>

    <span data-ttu-id="efb7e-307">![Web API コント ローラーをスキャフォールディングを作成したら](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "スキャフォールディングと Web API コント ローラーを作成した後")</span><span class="sxs-lookup"><span data-stu-id="efb7e-307">![After creating the Web API controller with scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "After creating the Web API controller with scaffolding")</span></span>

    <span data-ttu-id="efb7e-308">*Web API コント ローラーをスキャフォールディングを作成した後*</span><span class="sxs-lookup"><span data-stu-id="efb7e-308">*After creating the Web API controller with scaffolding*</span></span>
6. <span data-ttu-id="efb7e-309">開く、 **ApiPersonController.cs**ファイルし、検査、 *GetPeople*アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="efb7e-309">Open the **ApiPersonController.cs** file and inspect the *GetPeople* action method.</span></span> <span data-ttu-id="efb7e-310">このメソッドはクエリの db フィールド**PersonContext**ユーザー データを取得するには型です。</span><span class="sxs-lookup"><span data-stu-id="efb7e-310">This method queries the db field of **PersonContext** type in order to get the people data.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. <span data-ttu-id="efb7e-311">今すぐメソッド定義の前のコメントに注意してください。</span><span class="sxs-lookup"><span data-stu-id="efb7e-311">Now notice the comment above the method definition.</span></span> <span data-ttu-id="efb7e-312">次のタスクで使用するこのアクションを公開する URI を提供します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-312">It provides the URI that exposes this action which you will use in the next task.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > <span data-ttu-id="efb7e-313">既定では、Web API は、クエリをキャッチする構成、 */api* MVC コント ローラーとの競合を避けるためのパス。</span><span class="sxs-lookup"><span data-stu-id="efb7e-313">By default, Web API is configured to catch the queries to the */api* path to avoid collisions with MVC controllers.</span></span> <span data-ttu-id="efb7e-314">この設定を変更する必要がある場合を参照してください[ASP.NET Web API におけるルーティング](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md)します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-314">If you need to change this setting, refer to [Routing in ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="efb7e-315">タスク 2 – ソリューションの実行</span><span class="sxs-lookup"><span data-stu-id="efb7e-315">Task 2 – Running the Solution</span></span>

<span data-ttu-id="efb7e-316">このタスクでは、Internet Explorer を使用して**F12 開発者ツール**を Web API コント ローラーから完全な応答を検査します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-316">In this task you will use the Internet Explorer **F12 developer tools** to inspect the full response from the Web API controller.</span></span> <span data-ttu-id="efb7e-317">アプリケーション データの多くの洞察を取得するネットワーク トラフィックをキャプチャする方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="efb7e-317">You will see how you can capture network traffic to get more insight into your application data.</span></span>

> [!NOTE]
> <span data-ttu-id="efb7e-318">確認します**Internet Explorer**でが選択されている、**開始**Visual Studio ツールバーにあるボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="efb7e-318">Make sure that **Internet Explorer** is selected in the **Start** button located on the Visual Studio toolbar.</span></span>
> 
> ![Internet Explorer オプション](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> <span data-ttu-id="efb7e-320">**F12 開発者ツール**このハンズオン ラボでカバーされていない機能の幅広いセットがあります。</span><span class="sxs-lookup"><span data-stu-id="efb7e-320">The **F12 developer tools** have a wide set of functionality that is not covered in this hands-on-lab.</span></span> <span data-ttu-id="efb7e-321">詳細については、する場合を参照してください。 [F12 開発者ツールを使用して](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85))します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-321">If you want to learn more about it, refer to [Using the F12 developer tools](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).</span></span>


1. <span data-ttu-id="efb7e-322">キーを押して**F5**ソリューションを実行します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-322">Press **F5** to run the solution.</span></span>

    > [!NOTE]
    > <span data-ttu-id="efb7e-323">このタスクを正常に従うために、アプリケーションがデータである必要があります。</span><span class="sxs-lookup"><span data-stu-id="efb7e-323">In order to follow this task correctly, your application needs to have data.</span></span> <span data-ttu-id="efb7e-324">データベースが空の場合は、手順 2 での作業 3 に戻るし、MVC ビューを使用して新しいユーザーを作成する方法の手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="efb7e-324">If your database is empty, you can go back to Task 3 in Exercise 2 and follow the steps on how to create a new person using the MVC views.</span></span>
2. <span data-ttu-id="efb7e-325">キーを押して、ブラウザーで**F12**を開く、 **Developer Tools**パネル。</span><span class="sxs-lookup"><span data-stu-id="efb7e-325">In the browser, press **F12** to open the **Developer Tools** panel.</span></span> <span data-ttu-id="efb7e-326">キーを押して**CTRL** + **4**  をクリックしてまたは、**ネットワーク**緑色の矢印は、ネットワーク トラフィックのキャプチャを開始するボタン アイコンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="efb7e-326">Press **CTRL** + **4** or click the **Network** icon, and then click the green arrow button to begin capturing network traffic.</span></span>

    <span data-ttu-id="efb7e-327">![Web API のネットワーク キャプチャを開始する](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "ネットワーク キャプチャを開始する Web API")</span><span class="sxs-lookup"><span data-stu-id="efb7e-327">![Initiating Web API network capture](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "Initiating Web API network capture")</span></span>

    <span data-ttu-id="efb7e-328">*Web API のネットワーク キャプチャを開始します。*</span><span class="sxs-lookup"><span data-stu-id="efb7e-328">*Initiating Web API network capture*</span></span>
3. <span data-ttu-id="efb7e-329">追加**api/ApiPerson**ブラウザーのアドレス バーで URL にします。</span><span class="sxs-lookup"><span data-stu-id="efb7e-329">Append **api/ApiPerson** to the URL in the browser's address bar.</span></span> <span data-ttu-id="efb7e-330">応答の詳細を調べるようになりましたが、 **ApiPersonController**します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-330">You will now inspect the details of the response from the **ApiPersonController**.</span></span>

    <span data-ttu-id="efb7e-331">![Web API を介してユーザー データを取得する](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "Web API を介してユーザー データを取得します。")</span><span class="sxs-lookup"><span data-stu-id="efb7e-331">![Retrieving person data through Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "Retrieving person data through Web API")</span></span>

    <span data-ttu-id="efb7e-332">*Web API を介してユーザー データを取得します。*</span><span class="sxs-lookup"><span data-stu-id="efb7e-332">*Retrieving person data through Web API*</span></span>

    > [!NOTE]
    > <span data-ttu-id="efb7e-333">ダウンロードが完了すると、ダウンロードしたファイルを使用して行う促されます。</span><span class="sxs-lookup"><span data-stu-id="efb7e-333">Once the download finishes, you will be prompted to make an action with the downloaded file.</span></span> <span data-ttu-id="efb7e-334">ダイアログ ボックスは、開発者ツール ウィンドウからの応答のコンテンツを視聴できるように開いたままにしておきます。</span><span class="sxs-lookup"><span data-stu-id="efb7e-334">Leave the dialog box open in order to be able to watch the response content through the Developers Tool window.</span></span>
4. <span data-ttu-id="efb7e-335">ここでは、応答の本文を検査します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-335">Now you will inspect the body of the response.</span></span> <span data-ttu-id="efb7e-336">これを行うには、をクリックして、**詳細** タブをクリックして**応答本文**します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-336">To do this, click the **Details** tab and then click **Response body**.</span></span> <span data-ttu-id="efb7e-337">ダウンロードされたデータがオブジェクトとプロパティの一覧を確認する**Id**、**名前**と**年齢**に対応する、 **Person**クラス。</span><span class="sxs-lookup"><span data-stu-id="efb7e-337">You can check that the downloaded data is a list of objects with the properties **Id**, **Name** and **Age** that correspond to the **Person** class.</span></span>

    <span data-ttu-id="efb7e-338">![表示する Web API の応答本文](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "表示する Web API の応答本文")</span><span class="sxs-lookup"><span data-stu-id="efb7e-338">![Viewing Web API Response Body](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "Viewing Web API Response Body")</span></span>

    <span data-ttu-id="efb7e-339">*表示する Web API の応答本文*</span><span class="sxs-lookup"><span data-stu-id="efb7e-339">*Viewing Web API Response Body*</span></span>

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a><span data-ttu-id="efb7e-340">タスク 3 – Web API ヘルプ ページの追加</span><span class="sxs-lookup"><span data-stu-id="efb7e-340">Task 3 – Adding Web API Help Pages</span></span>

<span data-ttu-id="efb7e-341">Web API を作成するときに、他の開発者は、API を呼び出す方法を認識できるように、ヘルプ ページを作成すると便利です。</span><span class="sxs-lookup"><span data-stu-id="efb7e-341">When you create a Web API, it is useful to create a help page so that other developers will know how to call your API.</span></span> <span data-ttu-id="efb7e-342">作成し、ドキュメント ページを手動で更新しますが、自動生成すると、メンテナンス作業を実行しなくてもすむようにすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="efb7e-342">You could create and update the documentation pages manually, but it is better to auto-generate them to avoid having to do maintenance work.</span></span> <span data-ttu-id="efb7e-343">このタスクでは、ソリューションに Web API のヘルプ ページを自動的に生成するのに Nuget パッケージを使用します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-343">In this task you will use a Nuget package to automatically generate Web API help pages to the solution.</span></span>

1. <span data-ttu-id="efb7e-344">**ツール** メニューの選択 Visual Studio で**ライブラリ パッケージ マネージャー**、 をクリックし、**パッケージ マネージャー コンソール**します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-344">From the **Tools** menu in Visual Studio, select **Library Package Manager**, and then click **Package Manager Console**.</span></span>
2. <span data-ttu-id="efb7e-345">**パッケージ マネージャー コンソール**ウィンドウで、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-345">In the **Package Manager Console** window, execute the following command:</span></span>

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > <span data-ttu-id="efb7e-346">**Microsoft.AspNet.WebApi.HelpPage**パッケージが必要なアセンブリをインストールし、ヘルプ ページにあるを MVC ビューを追加します、**領域/HelpPage**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="efb7e-346">The **Microsoft.AspNet.WebApi.HelpPage** package installs the necessary assemblies and adds MVC Views for the help pages under the **Areas/HelpPage** folder.</span></span>

    <span data-ttu-id="efb7e-347">![HelpPage 領域](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage 領域")</span><span class="sxs-lookup"><span data-stu-id="efb7e-347">![HelpPage Area](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage Area")</span></span>

    <span data-ttu-id="efb7e-348">*HelpPage 領域*</span><span class="sxs-lookup"><span data-stu-id="efb7e-348">*HelpPage Area*</span></span>
3. <span data-ttu-id="efb7e-349">既定では、ヘルプ ページがあるドキュメントのプレース ホルダー文字列。</span><span class="sxs-lookup"><span data-stu-id="efb7e-349">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="efb7e-350">XML ドキュメントのコメントを使用するには、ドキュメントを作成します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-350">You can use XML documentation comments to create the documentation.</span></span> <span data-ttu-id="efb7e-351">この機能を有効にする、開く、 **HelpPageConfig.cs**にあるファイル、**領域/HelpPage/アプリ\_開始**フォルダー、次の行をコメント解除します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-351">To enable this feature, open the **HelpPageConfig.cs** file located in the **Areas/HelpPage/App\_Start** folder and uncomment the following line:</span></span>

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. <span data-ttu-id="efb7e-352">**ソリューション エクスプ ローラー**、プロジェクトを右クリックして**MyHybridSite**を選択します**プロパティ** をクリックし、**ビルド**タブ。</span><span class="sxs-lookup"><span data-stu-id="efb7e-352">In **Solution Explorer**, right-click the project **MyHybridSite**, select **Properties** and click the **Build** tab.</span></span>

    <span data-ttu-id="efb7e-353">![[ビルド] タブ](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "[ビルド] セクション")</span><span class="sxs-lookup"><span data-stu-id="efb7e-353">![Build tab](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "Build section")</span></span>

    <span data-ttu-id="efb7e-354">*[ビルド] タブ*</span><span class="sxs-lookup"><span data-stu-id="efb7e-354">*Build tab*</span></span>
5. <span data-ttu-id="efb7e-355">**出力**、 **XML ドキュメント ファイル**します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-355">Under **Output**, select **XML documentation file**.</span></span> <span data-ttu-id="efb7e-356">編集ボックスに「**アプリ\_Data/XmlDocument.xml**します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-356">In the edit box, type **App\_Data/XmlDocument.xml**.</span></span>

    <span data-ttu-id="efb7e-357">![[ビルド] タブでセクションを出力](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "ビルド タブでセクションの出力")</span><span class="sxs-lookup"><span data-stu-id="efb7e-357">![Output section in Build tab](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "Output section in build tab")</span></span>

    <span data-ttu-id="efb7e-358">*[ビルド] タブで、出力セクション*</span><span class="sxs-lookup"><span data-stu-id="efb7e-358">*Output section in Build tab*</span></span>
6. <span data-ttu-id="efb7e-359">キーを押して**CTRL** + **S**変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-359">Press **CTRL** + **S** to save the changes.</span></span>
7. <span data-ttu-id="efb7e-360">開く、 **ApiPersonController.cs**ファイルから、**コント ローラー**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="efb7e-360">Open the **ApiPersonController.cs** file from the **Controllers** folder.</span></span>
8. <span data-ttu-id="efb7e-361">間に新しい行を入力、 *GetPeople*メソッド シグネチャと *//get api/ApiPerson*コメントの追加、し、次の 3 つのスラッシュを入力します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-361">Enter a new line between the *GetPeople* method signature and the *// GET api/ApiPerson* comment, and then type three forward slashes.</span></span>

    > [!NOTE]
    > <span data-ttu-id="efb7e-362">Visual Studio は、メソッドのドキュメントを定義する XML 要素を自動的に挿入します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-362">Visual Studio automatically inserts the XML elements which define the method documentation.</span></span>
9. <span data-ttu-id="efb7e-363">概要のテキストと、戻り値の追加、 *GetPeople*メソッド。</span><span class="sxs-lookup"><span data-stu-id="efb7e-363">Add a summary text and the return value for the *GetPeople* method.</span></span> <span data-ttu-id="efb7e-364">次のようになります。</span><span class="sxs-lookup"><span data-stu-id="efb7e-364">It should look like the following.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. <span data-ttu-id="efb7e-365">キーを押して**F5**ソリューションを実行します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-365">Press **F5** to run the solution.</span></span>
11. <span data-ttu-id="efb7e-366">追加 **/help**ヘルプ ページを参照する、アドレス バーで URL にします。</span><span class="sxs-lookup"><span data-stu-id="efb7e-366">Append **/help** to the URL in the address bar to browse to the help page.</span></span>

    <span data-ttu-id="efb7e-367">![ASP.NET Web API ヘルプ ページ](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "ASP.NET Web API ヘルプ ページ")</span><span class="sxs-lookup"><span data-stu-id="efb7e-367">![ASP.NET Web API Help Page](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "ASP.NET Web API Help Page")</span></span>

    <span data-ttu-id="efb7e-368">*ASP.NET Web API ヘルプ ページ*</span><span class="sxs-lookup"><span data-stu-id="efb7e-368">*ASP.NET Web API Help Page*</span></span>

    > [!NOTE]
    > <span data-ttu-id="efb7e-369">ページのメインのコンテンツは、コント ローラーでグループ化された Api のテーブルです。</span><span class="sxs-lookup"><span data-stu-id="efb7e-369">The main content of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="efb7e-370">テーブルのエントリも動的に生成されたを使用して、 **IApiExplorer**インターフェイス。</span><span class="sxs-lookup"><span data-stu-id="efb7e-370">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="efb7e-371">追加 API コント ローラーを更新するか、テーブルは自動的に更新、次回アプリケーションを構築します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-371">If you add or update an API controller, the table will be automatically updated the next time you build the application.</span></span>
    > 
    > <span data-ttu-id="efb7e-372">**API**列には、HTTP メソッドと相対 URI が表示されます。</span><span class="sxs-lookup"><span data-stu-id="efb7e-372">The **API** column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="efb7e-373">**説明**列には、メソッドのドキュメントから抽出された情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="efb7e-373">The **Description** column contains information that has been extracted from the method's documentation.</span></span>
12. <span data-ttu-id="efb7e-374">[説明] 列、メソッド定義の上に追加した説明が表示されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="efb7e-374">Note that the description you added above the method definition is displayed in the description column.</span></span>

    <span data-ttu-id="efb7e-375">![API メソッドの説明](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "API メソッドの説明")</span><span class="sxs-lookup"><span data-stu-id="efb7e-375">![API method description](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "API method description")</span></span>

    <span data-ttu-id="efb7e-376">*API メソッドの説明*</span><span class="sxs-lookup"><span data-stu-id="efb7e-376">*API method description*</span></span>
13. <span data-ttu-id="efb7e-377">詳細については、サンプルの応答本文を含むページに移動する API のメソッドのいずれかをクリックします。</span><span class="sxs-lookup"><span data-stu-id="efb7e-377">Click one of the API methods to navigate to a page with more detailed information, including sample response bodies.</span></span>

    <span data-ttu-id="efb7e-378">![詳細情報ページ](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "詳細情報 ページ")</span><span class="sxs-lookup"><span data-stu-id="efb7e-378">![Detail Information page](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "Detail Information page")</span></span>

    <span data-ttu-id="efb7e-379">*詳細情報 ページ*</span><span class="sxs-lookup"><span data-stu-id="efb7e-379">*Detailed information page*</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="efb7e-380">まとめ</span><span class="sxs-lookup"><span data-stu-id="efb7e-380">Summary</span></span>

<span data-ttu-id="efb7e-381">このハンズオン ラボについて説明した方法。</span><span class="sxs-lookup"><span data-stu-id="efb7e-381">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="efb7e-382">Visual Studio 2013 で ASP.NET の 1 つのエクスペリエンスを使用して新しい Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-382">Create a new Web application using the One ASP.NET Experience in Visual Studio 2013</span></span>
- <span data-ttu-id="efb7e-383">1 つ 1 つのプロジェクトに複数の ASP.NET テクノロジを統合します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-383">Integrate multiple ASP.NET technologies into one single project</span></span>
- <span data-ttu-id="efb7e-384">ASP.NET のスキャフォールディングを使用して、モデル クラスからの MVC コント ローラーとビューを生成します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-384">Generate MVC controllers and views from your model classes using ASP.NET Scaffolding</span></span>
- <span data-ttu-id="efb7e-385">非同期プログラミングと Entity Framework を介してデータ アクセスなどの機能を使用して Web API コント ローラーを生成します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-385">Generate Web API controllers, which use features such as Async Programming and data access through Entity Framework</span></span>
- <span data-ttu-id="efb7e-386">コント ローラーの Web API のヘルプ ページを自動的に生成します。</span><span class="sxs-lookup"><span data-stu-id="efb7e-386">Automatically generate Web API Help Pages for your controllers</span></span>
