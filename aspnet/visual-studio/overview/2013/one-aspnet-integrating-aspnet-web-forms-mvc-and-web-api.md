---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: 'ハンズ オン ラボ: 1 つの ASP.NET: ASP.NET Web フォーム、MVC、Web API の統合 |Microsoft ドキュメント'
author: rick-anderson
description: ASP.NET は、Web サイト、アプリケーション、MVC、Web API、およびその他のユーザーなどの特殊なテクノロジを使用してサービスを構築するためのフレームワークです。 で ASP.NET h を拡張しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: 55109723e566a9f7c66c1a59414377b05dbec760
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "26507121"
---
<a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a><span data-ttu-id="3c312-104">ハンズ オン ラボ: 1 つの ASP.NET: ASP.NET Web フォーム、MVC、Web API の統合</span><span class="sxs-lookup"><span data-stu-id="3c312-104">Hands On Lab: One ASP.NET: Integrating ASP.NET Web Forms, MVC and Web API</span></span>
====================
<span data-ttu-id="3c312-105">によって[Web キャンプ チーム](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="3c312-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="3c312-106">Web キャンプ トレーニング キットをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="3c312-106">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="3c312-107">ASP.NET は、Web サイト、アプリケーション、MVC、Web API、およびその他のユーザーなどの特殊なテクノロジを使用してサービスを構築するためのフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="3c312-107">ASP.NET is a framework for building Web sites, apps and services using specialized technologies such as MVC, Web API and others.</span></span> <span data-ttu-id="3c312-108">展開と ASP.NET が作成されてから表示、明示する必要がありますこれらのテクノロジと統合された最近の取り組みに向けて作業**One ASP.NET**です。</span><span class="sxs-lookup"><span data-stu-id="3c312-108">With the expansion ASP.NET has seen since its creation and the expressed need to have these technologies integrated, there have been recent efforts in working towards **One ASP.NET**.</span></span>
> 
> <span data-ttu-id="3c312-109">Visual Studio 2013 には、アプリケーションを作成したり、すべての ASP.NET テクノロジを 1 つのプロジェクトで使用する新しい統合プロジェクト システムが導入されています。</span><span class="sxs-lookup"><span data-stu-id="3c312-109">Visual Studio 2013 introduces a new unified project system which lets you build an application and use all the ASP.NET technologies in one project.</span></span> <span data-ttu-id="3c312-110">この機能は、プロジェクトと関連付け、スティックの開始時に 1 つのテクノロジを選択する必要がある、代わりに 1 つのプロジェクト内の複数の ASP.NET フレームワークの使用が推奨します。</span><span class="sxs-lookup"><span data-stu-id="3c312-110">This feature eliminates the need to pick one technology at the start of a project and stick with it, and instead encourages the use of multiple ASP.NET frameworks within one project.</span></span>
> 
> <span data-ttu-id="3c312-111">すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[ http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit)です。</span><span class="sxs-lookup"><span data-stu-id="3c312-111">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="3c312-112">概要</span><span class="sxs-lookup"><span data-stu-id="3c312-112">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="3c312-113">目的</span><span class="sxs-lookup"><span data-stu-id="3c312-113">Objectives</span></span>

<span data-ttu-id="3c312-114">このハンズオン ラボでは学習する方法。</span><span class="sxs-lookup"><span data-stu-id="3c312-114">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="3c312-115">ベースの Web サイトを作成、 **One ASP.NET**プロジェクトの種類</span><span class="sxs-lookup"><span data-stu-id="3c312-115">Create a Web site based on the **One ASP.NET** project type</span></span>
- <span data-ttu-id="3c312-116">使用して異なる**ASP.NET**のようなフレームワーク**MVC**と**Web API**同じプロジェクト内</span><span class="sxs-lookup"><span data-stu-id="3c312-116">Use different **ASP.NET** frameworks like **MVC** and **Web API** in the same project</span></span>
- <span data-ttu-id="3c312-117">主要なコンポーネントを識別、 **ASP.NET**アプリケーション</span><span class="sxs-lookup"><span data-stu-id="3c312-117">Identify the main components of an **ASP.NET** application</span></span>
- <span data-ttu-id="3c312-118">利用、 **ASP.NET スキャフォールディング**CRUD 操作を実行するには、コント ローラーとビューを自動的に作成するためにフレームワークが、モデルのクラスに基づく</span><span class="sxs-lookup"><span data-stu-id="3c312-118">Take advantage of the **ASP.NET Scaffolding** framework to automatically create Controllers and Views to perform CRUD operations based on your model classes</span></span>
- <span data-ttu-id="3c312-119">同じジョブごとに適切なツールを使用してコンピューターの人間が判読できる形式で情報のセットを公開します。</span><span class="sxs-lookup"><span data-stu-id="3c312-119">Expose the same set of information in machine- and human-readable formats using the right tool for each job</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="3c312-120">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="3c312-120">Prerequisites</span></span>

<span data-ttu-id="3c312-121">このハンズオン ラボを完了する、次が必要。</span><span class="sxs-lookup"><span data-stu-id="3c312-121">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="3c312-122">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/)以上</span><span class="sxs-lookup"><span data-stu-id="3c312-122">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="3c312-123">Visual Studio 2013 Update 1</span><span class="sxs-lookup"><span data-stu-id="3c312-123">Visual Studio 2013 Update 1</span></span>](https://go.microsoft.com/fwlink/?LinkId=301714)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="3c312-124">セットアップ</span><span class="sxs-lookup"><span data-stu-id="3c312-124">Setup</span></span>

<span data-ttu-id="3c312-125">このハンズオン ラボでは、演習を実行するのには、最初に、環境を設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3c312-125">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="3c312-126">開いている Windows エクスプ ローラーおよびテスト環境の参照**ソース**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="3c312-126">Open Windows Explorer and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="3c312-127">右クリックして**Setup.cmd**選択と**管理者として実行**を環境を構成して、このラボ用の Visual Studio のコード スニペットをインストールするセットアップ プロセスを起動します。</span><span class="sxs-lookup"><span data-stu-id="3c312-127">Right-click on **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="3c312-128">ユーザー アカウント制御ダイアログ ボックスが表示されている場合は、続行するアクションを確認します。</span><span class="sxs-lookup"><span data-stu-id="3c312-128">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="3c312-129">セットアップを実行する前に、このラボのすべての依存関係をチェックしたことを確認してください。</span><span class="sxs-lookup"><span data-stu-id="3c312-129">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="3c312-130">コード スニペットを使用します。</span><span class="sxs-lookup"><span data-stu-id="3c312-130">Using the Code Snippets</span></span>

<span data-ttu-id="3c312-131">ラボ ドキュメント全体にわたって、コード ブロックを挿入するように指示されます。</span><span class="sxs-lookup"><span data-stu-id="3c312-131">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="3c312-132">便宜上、このコードのほとんどは、Visual Studio コード スニペットを手動で追加することを回避する Visual Studio 2013 内からアクセスできるとして提供されます。</span><span class="sxs-lookup"><span data-stu-id="3c312-132">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="3c312-133">各演習にある開始ソリューションが組み込まれた、**開始**個別に各手順をたどることによってこの作業のフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="3c312-133">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="3c312-134">実習では、追加のコード スニペットがこれらのソリューションの開始から不足しており、演習を完了するまで動作しない可能性がありますに注意してください。</span><span class="sxs-lookup"><span data-stu-id="3c312-134">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="3c312-135">演習では、ソース コード、内部も表示されます、**終了**を対応する手順の手順を実行した結果のコードの Visual Studio ソリューションを含むフォルダー。</span><span class="sxs-lookup"><span data-stu-id="3c312-135">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="3c312-136">このハンズオン ラボを使用すると、追加のヘルプを必要がある場合は、これらのソリューションをガイドとして使用できます。</span><span class="sxs-lookup"><span data-stu-id="3c312-136">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="3c312-137">演習</span><span class="sxs-lookup"><span data-stu-id="3c312-137">Exercises</span></span>

<span data-ttu-id="3c312-138">このハンズオン ラボには、次の実習が含まれます。</span><span class="sxs-lookup"><span data-stu-id="3c312-138">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="3c312-139">新しい Web フォーム プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="3c312-139">Creating a New Web Forms Project</span></span>](#Exercise1)
2. [<span data-ttu-id="3c312-140">スキャフォールディングを使用して、MVC コント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="3c312-140">Creating an MVC Controller Using Scaffolding</span></span>](#Exercise2)
3. [<span data-ttu-id="3c312-141">スキャフォールディングを使用して Web API コント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="3c312-141">Creating a Web API Controller Using Scaffolding</span></span>](#Exercise3)

<span data-ttu-id="3c312-142">この演習を完了する時間を推定: **60 分**</span><span class="sxs-lookup"><span data-stu-id="3c312-142">Estimated time to complete this lab: **60 minutes**</span></span>

> [!NOTE]
> <span data-ttu-id="3c312-143">Visual Studio を初めて起動するときに定義済みの設定のコレクションの 1 つを選択する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3c312-143">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="3c312-144">定義済みの各コレクションは、特定の開発スタイルと一致するものではまた、ウィンドウのレイアウト、エディターの動作、IntelliSense コード スニペットでは、およびダイアログ ボックスのオプションを判断します。</span><span class="sxs-lookup"><span data-stu-id="3c312-144">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="3c312-145">このラボの手順を使用する場合は、Visual Studio での特定のタスクを実行に必要な操作を記述する、**全般的な開発設定**コレクション。</span><span class="sxs-lookup"><span data-stu-id="3c312-145">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="3c312-146">開発環境に合わせてさまざまな設定のコレクションを選択する場合、考慮する必要がある手順に違いがあります。</span><span class="sxs-lookup"><span data-stu-id="3c312-146">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a><span data-ttu-id="3c312-147">手順 1: 新しい Web フォーム プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="3c312-147">Exercise 1: Creating a New Web Forms Project</span></span>

<span data-ttu-id="3c312-148">この演習では、Visual Studio 2013 を使用して新しい Web フォーム サイトを作成します、 **One ASP.NET**統合プロジェクトの環境は、同じアプリケーションで Web フォーム、MVC、Web API のコンポーネントを簡単に統合することができます。</span><span class="sxs-lookup"><span data-stu-id="3c312-148">In this exercise you will create a new Web Forms site in Visual Studio 2013 using the **One ASP.NET** unified project experience, which will allow you to easily integrate Web Forms, MVC and Web API components in the same application.</span></span> <span data-ttu-id="3c312-149">生成されたソリューションを調査し、その部分を特定および操作で、Web サイトを最後に表示されます。</span><span class="sxs-lookup"><span data-stu-id="3c312-149">You will then explore the generated solution and identify its parts, and finally you will see the Web site in action.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a><span data-ttu-id="3c312-150">作業 1: 1 つの ASP.NET エクスペリエンスを使用して新しいサイトを作成します。</span><span class="sxs-lookup"><span data-stu-id="3c312-150">Task 1 – Creating a New Site Using the One ASP.NET Experience</span></span>

<span data-ttu-id="3c312-151">このタスクの開始は Visual Studio で新しい Web サイトを作成するに基づいて、 **One ASP.NET**プロジェクトの種類。</span><span class="sxs-lookup"><span data-stu-id="3c312-151">In this task you will start creating a new Web site in Visual Studio based on the **One ASP.NET** project type.</span></span> <span data-ttu-id="3c312-152">**1 つの ASP.NET**にすべての ASP.NET テクノロジをまとめを混在させるし、必要に応じて、それらが一致するオプションを選択できます。</span><span class="sxs-lookup"><span data-stu-id="3c312-152">**One ASP.NET** unifies all ASP.NET technologies and gives you the option to mix and match them as desired.</span></span> <span data-ttu-id="3c312-153">アプリケーション内でサイド バイ サイドで、ライブ Web フォーム、MVC、Web API のさまざまなコンポーネントを識別します。</span><span class="sxs-lookup"><span data-stu-id="3c312-153">You will then recognize the different components of Web Forms, MVC and Web API that live side by side within your application.</span></span>

1. <span data-ttu-id="3c312-154">開いている**Visual Studio Express 2013 for Web**選択**ファイル |新しいプロジェクト.** を新しいソリューションを開始します。</span><span class="sxs-lookup"><span data-stu-id="3c312-154">Open **Visual Studio Express 2013 for Web** and select **File | New Project...** to start a new solution.</span></span>

    ![新規プロジェクトの作成](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    <span data-ttu-id="3c312-156">*新しいプロジェクトを作成します。*</span><span class="sxs-lookup"><span data-stu-id="3c312-156">*Creating a New Project*</span></span>
2. <span data-ttu-id="3c312-157">**新しいプロジェクト**ダイアログ ボックスで、 **ASP.NET Web アプリケーション**下にある、 **Visual c# |Web** ] タブの [し、確認 **.NET Framework 4.5**が選択されています。</span><span class="sxs-lookup"><span data-stu-id="3c312-157">In the **New Project** dialog box, select **ASP.NET Web Application** under the **Visual C# | Web** tab, and make sure **.NET Framework 4.5** is selected.</span></span> <span data-ttu-id="3c312-158">プロジェクトに名前を*MyHybridSite*、選択、**場所** をクリック**OK**です。</span><span class="sxs-lookup"><span data-stu-id="3c312-158">Name the project *MyHybridSite*, choose a **Location** and click **OK**.</span></span>

    ![新しい ASP.NET Web アプリケーション プロジェクト](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    <span data-ttu-id="3c312-160">*新しい ASP.NET Web アプリケーション プロジェクトを作成します。*</span><span class="sxs-lookup"><span data-stu-id="3c312-160">*Creating a new ASP.NET Web Application project*</span></span>
3. <span data-ttu-id="3c312-161">**新しい ASP.NET プロジェクト**ダイアログ ボックスで、 **Web フォーム**テンプレートを選択、 **MVC**と**Web API**オプション。</span><span class="sxs-lookup"><span data-stu-id="3c312-161">In the **New ASP.NET Project** dialog box, select the **Web Forms** template and select the **MVC** and **Web API** options.</span></span> <span data-ttu-id="3c312-162">また、ことを確認、**認証**にオプションが設定されている**個々 のユーザー アカウント**です。</span><span class="sxs-lookup"><span data-stu-id="3c312-162">Also, make sure that the **Authentication** option is set to **Individual User Accounts**.</span></span> <span data-ttu-id="3c312-163">**[OK]** をクリックして続行します。</span><span class="sxs-lookup"><span data-stu-id="3c312-163">Click **OK** to continue.</span></span>

    ![Web API と MVC コンポーネントを含め、Web フォーム テンプレートに新しいプロジェクトの作成](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    <span data-ttu-id="3c312-165">*Web API と MVC コンポーネントを含め、Web フォーム テンプレートに新しいプロジェクトの作成*</span><span class="sxs-lookup"><span data-stu-id="3c312-165">*Creating a new project with the Web Forms template, including Web API and MVC components*</span></span>
4. <span data-ttu-id="3c312-166">ここで生成されたソリューションの構造を調べることができます。</span><span class="sxs-lookup"><span data-stu-id="3c312-166">You can now explore the structure of the generated solution.</span></span>

    ![生成されたソリューションを調べる](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    <span data-ttu-id="3c312-168">*生成されたソリューションを調べる*</span><span class="sxs-lookup"><span data-stu-id="3c312-168">*Exploring the generated solution*</span></span>

    1. <span data-ttu-id="3c312-169">**アカウント:** このフォルダーには、登録にログインし、アプリケーションのユーザー アカウントを管理するには、Web フォーム ページが含まれています。</span><span class="sxs-lookup"><span data-stu-id="3c312-169">**Account:** This folder contains the Web Form pages to register, log in to and manage the application's user accounts.</span></span> <span data-ttu-id="3c312-170">このフォルダーが追加されたときに、**個々 のユーザー アカウント**認証オプションは、Web フォーム プロジェクト テンプレートの構成中に選択します。</span><span class="sxs-lookup"><span data-stu-id="3c312-170">This folder is added when the **Individual User Accounts** authentication option is selected during the configuration of the Web Forms project template.</span></span>
    2. <span data-ttu-id="3c312-171">**モデル:** このフォルダーは、アプリケーション データを表すクラスに格納されます。</span><span class="sxs-lookup"><span data-stu-id="3c312-171">**Models:** This folder will contain the classes that represent your application data.</span></span>
    3. <span data-ttu-id="3c312-172">**コント ローラー**と**ビュー**: これらのフォルダーに必要な**ASP.NET MVC**と**ASP.NET Web API**コンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="3c312-172">**Controllers** and **Views**: These folders are required for the **ASP.NET MVC** and **ASP.NET Web API** components.</span></span> <span data-ttu-id="3c312-173">次の演習で MVC および Web API のテクノロジについて学びます。</span><span class="sxs-lookup"><span data-stu-id="3c312-173">You will explore the MVC and Web API technologies in the next exercises.</span></span>
    4. <span data-ttu-id="3c312-174">**Default.aspx**、 **Contact.aspx**と**About.aspx**ファイルに固有のページを構築する開始点として使用できる定義済みの Web フォーム ページは、アプリケーション。</span><span class="sxs-lookup"><span data-stu-id="3c312-174">The **Default.aspx**, **Contact.aspx** and **About.aspx** files are pre-defined Web Form pages that you can use as starting points to build the pages specific to your application.</span></span> <span data-ttu-id="3c312-175">これらのファイルのプログラミング ロジックと呼ばれる別のファイルに含まれて、&quot;分離コード&quot;を持つファイルを&quot;..aspx.vb ファイル&quot;または&quot;. aspx.cs&quot;拡張機能 (によって、使用される言語)。</span><span class="sxs-lookup"><span data-stu-id="3c312-175">The programming logic of those files resides in a separate file referred to as the &quot;code-behind&quot; file, which has an &quot;.aspx.vb&quot; or &quot;.aspx.cs&quot; extension (depending on the language used).</span></span> <span data-ttu-id="3c312-176">分離コード ロジックは、サーバー上で実行し、動的に、ページの HTML 出力を生成します。</span><span class="sxs-lookup"><span data-stu-id="3c312-176">The code-behind logic runs on the server and dynamically produces the HTML output for your page.</span></span>
    5. <span data-ttu-id="3c312-177">**Site.Master**と**Site.Mobile.Master**ページ アプリケーションのルック アンド フィールとすべてのページの標準の動作を定義します。</span><span class="sxs-lookup"><span data-stu-id="3c312-177">The **Site.Master** and **Site.Mobile.Master** pages define the look and feel and the standard behavior of all the pages in the application.</span></span>
5. <span data-ttu-id="3c312-178">ダブルクリックして、 **Default.aspx**ページのコンテンツを参照するファイル。</span><span class="sxs-lookup"><span data-stu-id="3c312-178">Double-click the **Default.aspx** file to explore the content of the page.</span></span>

    ![Default.aspx ページを調べる](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    <span data-ttu-id="3c312-180">*Default.aspx ページを調べる*</span><span class="sxs-lookup"><span data-stu-id="3c312-180">*Exploring the Default.aspx page*</span></span>

    > [!NOTE]
    > <span data-ttu-id="3c312-181">**ページ**ファイルの上部にあるディレクティブが、Web フォーム ページの属性を定義します。</span><span class="sxs-lookup"><span data-stu-id="3c312-181">The **Page** directive at the top of the file defines the attributes of the Web Forms page.</span></span> <span data-ttu-id="3c312-182">たとえば、 **MasterPageFile**属性は、マスターへのパスを指定 ページでこのケースでは、 *Site.Master*ページ - と**Inherits**属性を定義、ページを継承する分離コード クラスです。</span><span class="sxs-lookup"><span data-stu-id="3c312-182">For example, the **MasterPageFile** attribute specifies the path to the master page -in this case, the *Site.Master* page- and the **Inherits** attribute defines the code-behind class for the page to inherit.</span></span> <span data-ttu-id="3c312-183">このクラスは、によって決まりますファイルにある、**分離コード**属性。</span><span class="sxs-lookup"><span data-stu-id="3c312-183">This class is located in the file determined by the **CodeBehind** attribute.</span></span>
    > 
    > <span data-ttu-id="3c312-184">**Asp:content**コントロールがページ (テキスト、マークアップとコントロール) の実際のコンテンツを保持しにマップされて、 **asp: ContentPlaceHolder**マスター ページ上のコントロールです。</span><span class="sxs-lookup"><span data-stu-id="3c312-184">The **asp:Content** control holds the actual content of the page (text, markup and controls) and is mapped to a **asp:ContentPlaceHolder** control on the master page.</span></span> <span data-ttu-id="3c312-185">内のページ コンテンツにレンダリングされるここで、*メイン*で定義されているコントロール、 *Site.Master*ページ。</span><span class="sxs-lookup"><span data-stu-id="3c312-185">In this case, the page content will be rendered inside the *MainContent* control defined in the *Site.Master* page.</span></span>
6. <span data-ttu-id="3c312-186">展開して、**アプリ\_開始**フォルダーとに注意してください、 **WebApiConfig.cs**ファイル。</span><span class="sxs-lookup"><span data-stu-id="3c312-186">Expand the **App\_Start** folder and notice the **WebApiConfig.cs** file.</span></span> <span data-ttu-id="3c312-187">Visual Studio には、テンプレートを 1 つの ASP.NET プロジェクトを構成するときに、Web API が含まれるために、そのファイルが生成されたソリューションに含まれます。</span><span class="sxs-lookup"><span data-stu-id="3c312-187">Visual Studio included that file in the generated solution because you included Web API when configuring your project with the One ASP.NET template.</span></span>
7. <span data-ttu-id="3c312-188">開く、 **WebApiConfig.cs**ファイル。</span><span class="sxs-lookup"><span data-stu-id="3c312-188">Open the **WebApiConfig.cs** file.</span></span> <span data-ttu-id="3c312-189">*WebApiConfig*を HTTP にマップする Web API に関連付けられている構成を検索がクラスにルーティング**Web API コント ローラー**です。</span><span class="sxs-lookup"><span data-stu-id="3c312-189">In the *WebApiConfig* class you will find the configuration associated with Web API, which maps HTTP routes to **Web API controllers**.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. <span data-ttu-id="3c312-190">開く、 **RouteConfig.cs**ファイル。</span><span class="sxs-lookup"><span data-stu-id="3c312-190">Open the **RouteConfig.cs** file.</span></span> <span data-ttu-id="3c312-191">内部、 *RegisterRoutes*メソッドへの HTTP ルートをマップする MVC に関連付けられている構成を検索、 **MVC コント ローラー**です。</span><span class="sxs-lookup"><span data-stu-id="3c312-191">Inside the *RegisterRoutes* method you will find the configuration associated with MVC, which maps HTTP routes to **MVC controllers**.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="3c312-192">タスク 2 – ソリューションの実行</span><span class="sxs-lookup"><span data-stu-id="3c312-192">Task 2 – Running the Solution</span></span>

<span data-ttu-id="3c312-193">生成されたソリューションを実行がこのタスクでされたら、アプリといくつかの URL の書き換えと組み込みの認証と同様に、その機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="3c312-193">In this task you will run the generated solution, explore the app and some of its features, like URL rewriting and built-in authentication.</span></span>

1. <span data-ttu-id="3c312-194">キーを押して、ソリューションを実行する**f5 キーを押して** をクリックして、**開始**ツールバーにボタンがあります。</span><span class="sxs-lookup"><span data-stu-id="3c312-194">To run the solution, press **F5** or click the **Start** button located on the toolbar.</span></span> <span data-ttu-id="3c312-195">アプリケーション ホーム ページは、ブラウザーで開く必要があります。</span><span class="sxs-lookup"><span data-stu-id="3c312-195">The application home page should open in the browser.</span></span>

    ![ソリューションの実行](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. <span data-ttu-id="3c312-197">Web フォーム ページが呼び出されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="3c312-197">Verify that the Web Forms pages are being invoked.</span></span> <span data-ttu-id="3c312-198">これを行うには、次のように追加します。 **/contact.aspx**キーを押して、アドレス バーの URL に**Enter**です。</span><span class="sxs-lookup"><span data-stu-id="3c312-198">To do this, append **/contact.aspx** to the URL in the address bar and press **Enter**.</span></span>

    ![フレンドリな Url](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    <span data-ttu-id="3c312-200">*フレンドリな Url*</span><span class="sxs-lookup"><span data-stu-id="3c312-200">*Friendly URLs*</span></span>

    > [!NOTE]
    > <span data-ttu-id="3c312-201">URL が変更されたことがわかります**にお問い合わせください/** です。</span><span class="sxs-lookup"><span data-stu-id="3c312-201">As you can see, the URL changes to **/contact**.</span></span> <span data-ttu-id="3c312-202">始まる**ASP.NET 4**URL ルーティング機能は、Web フォームに追加された、Url などのため、記述することができます、 *[ http://www.mysite.com/products/software ](http://www.mysite.com/products/software)* の代わりに *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*.</span><span class="sxs-lookup"><span data-stu-id="3c312-202">Starting from **ASP.NET 4**, URL routing capabilities were added to Web Forms, so you can write URLs like *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)* instead of *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*.</span></span> <span data-ttu-id="3c312-203">詳細についてを参照してください[URL ルーティング](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md)です。</span><span class="sxs-lookup"><span data-stu-id="3c312-203">For more information refer to [URL Routing](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).</span></span>
3. <span data-ttu-id="3c312-204">アプリケーションに統合認証の流れを調査します。</span><span class="sxs-lookup"><span data-stu-id="3c312-204">You will now explore the authentication flow integrated into the application.</span></span> <span data-ttu-id="3c312-205">これを行うには、をクリックして**登録**ページの右上隅にします。</span><span class="sxs-lookup"><span data-stu-id="3c312-205">To do this, click **Register** in the upper-right corner of the page.</span></span>

    ![新しいユーザーを登録します。](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    <span data-ttu-id="3c312-207">*新しいユーザーを登録します。*</span><span class="sxs-lookup"><span data-stu-id="3c312-207">*Registering a new user*</span></span>
4. <span data-ttu-id="3c312-208">**登録** ページで、入力、**ユーザー名**と**パスワード**、クリックして**登録**です。</span><span class="sxs-lookup"><span data-stu-id="3c312-208">In the **Register** page, enter a **User name** and **Password**, and then click **Register**.</span></span>

    ![登録 ページ](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    <span data-ttu-id="3c312-210">*登録 ページ*</span><span class="sxs-lookup"><span data-stu-id="3c312-210">*Register page*</span></span>
5. <span data-ttu-id="3c312-211">アプリケーションが、新しいアカウントを登録し、ユーザーを認証します。</span><span class="sxs-lookup"><span data-stu-id="3c312-211">The application registers the new account, and the user is authenticated.</span></span>

    ![認証されたユーザー](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    <span data-ttu-id="3c312-213">*認証されたユーザー*</span><span class="sxs-lookup"><span data-stu-id="3c312-213">*User authenticated*</span></span>
6. <span data-ttu-id="3c312-214">Visual Studio とキーを押してに戻って**shift キーを押しながら f5 キーを押して**デバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="3c312-214">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a><span data-ttu-id="3c312-215">手順 2: スキャフォールディングを使用して、MVC コント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="3c312-215">Exercise 2: Creating an MVC Controller Using Scaffolding</span></span>

<span data-ttu-id="3c312-216">この演習では、アクションと Razor ビューを 1 行のコードを記述することがなく、CRUD 操作を実行すると、ASP.NET MVC 5 コント ローラーを作成する Visual Studio によって提供される ASP.NET スキャフォールディング フレームワークの利点がかかります。</span><span class="sxs-lookup"><span data-stu-id="3c312-216">In this exercise you will take advantage of the ASP.NET Scaffolding framework provided by Visual Studio to create an ASP.NET MVC 5 controller with actions and Razor views to perform CRUD operations, without writing a single line of code.</span></span> <span data-ttu-id="3c312-217">スキャフォールディング プロセスは、SQL データベースのデータ コンテキストおよびデータベース スキーマを生成するのに Entity Framework Code First で使用されます。</span><span class="sxs-lookup"><span data-stu-id="3c312-217">The scaffolding process will use Entity Framework Code First to generate the data context and the database schema in the SQL database.</span></span>

<span data-ttu-id="3c312-218">**Entity Framework の Code First**</span><span class="sxs-lookup"><span data-stu-id="3c312-218">**About Entity Framework Code First**</span></span>

<span data-ttu-id="3c312-219">Entity Framework (EF) は、直接リレーショナル ストレージ スキーマを使用したプログラミングではなく、概念アプリケーション モデルを使用したプログラミング、データ アクセス アプリケーションを作成することができますオブジェクト リレーショナル マッパー (ORM) です。</span><span class="sxs-lookup"><span data-stu-id="3c312-219">Entity Framework (EF) is an object-relational mapper (ORM) that enables you to create data access applications by programming with a conceptual application model instead of programming directly using a relational storage schema.</span></span>

<span data-ttu-id="3c312-220">Entity Framework Code First のモデリング ワークフロー使用する EF は、このクエリを実行するを実行するときに依存しているモデルを表す独自のドメイン クラスを使用して変更の追跡および更新機能できます。</span><span class="sxs-lookup"><span data-stu-id="3c312-220">The Entity Framework Code First modeling workflow allows you to use your own domain classes to represent the model that EF relies on when performing querying, change-tracking and updating functions.</span></span> <span data-ttu-id="3c312-221">Code First の開発のワークフローを使用する必要はありません、データベースを作成するか、スキーマを指定して、アプリケーションを開始します。</span><span class="sxs-lookup"><span data-stu-id="3c312-221">Using the Code First development workflow, you do not need to begin your application by creating a database or specifying a schema.</span></span> <span data-ttu-id="3c312-222">オブジェクトを定義する、最も適切なドメイン モデル、アプリケーションの標準の .NET クラスを作成して、Entity Framework をデータベースに作成されます。</span><span class="sxs-lookup"><span data-stu-id="3c312-222">Instead, you can write standard .NET classes that define the most appropriate domain model objects for your application, and Entity Framework will create the database for you.</span></span>

> [!NOTE]
> <span data-ttu-id="3c312-223">Entity Framework に関する詳細については、[ここ](../../../entity-framework.md)です。</span><span class="sxs-lookup"><span data-stu-id="3c312-223">You can learn more about Entity Framework [here](../../../entity-framework.md).</span></span>


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a><span data-ttu-id="3c312-224">作業 1: 新しいモデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="3c312-224">Task 1 – Creating a New Model</span></span>

<span data-ttu-id="3c312-225">ここで定義する、**人**MVC コント ローラーとビューを作成する、スキャフォールディング プロセスによって使用されるモデルとなるクラスです。</span><span class="sxs-lookup"><span data-stu-id="3c312-225">You will now define a **Person** class, which will be the model used by the scaffolding process to create the MVC controller and the views.</span></span> <span data-ttu-id="3c312-226">作成することで開始されます、 **Person**モデル クラス、およびコント ローラーでの CRUD 操作が自動的に作成スキャフォールディング機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="3c312-226">You will start by creating a **Person** model class, and the CRUD operations in the controller will be automatically created using scaffolding features.</span></span>

1. <span data-ttu-id="3c312-227">開いている**Visual Studio Express 2013 for Web**と**MyHybridSite.sln**にソリューションがある、**ソース/Ex2-MvcScaffolding/開始**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="3c312-227">Open **Visual Studio Express 2013 for Web** and the **MyHybridSite.sln** solution located in the **Source/Ex2-MvcScaffolding/Begin** folder.</span></span> <span data-ttu-id="3c312-228">代わりに、前の手順で取得した、ソリューションを続行できます。</span><span class="sxs-lookup"><span data-stu-id="3c312-228">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="3c312-229">**ソリューション エクスプ ローラー**を右クリックし、**モデル**のフォルダー、 **MyHybridSite**プロジェクトし、選択**追加 |クラス.**.</span><span class="sxs-lookup"><span data-stu-id="3c312-229">In **Solution Explorer**, right-click the **Models** folder of the **MyHybridSite** project and select **Add | Class...**.</span></span>

    ![ユーザー モデル クラスを追加します。](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    <span data-ttu-id="3c312-231">*ユーザー モデル クラスを追加します。*</span><span class="sxs-lookup"><span data-stu-id="3c312-231">*Adding the Person model class*</span></span>
3. <span data-ttu-id="3c312-232">**新しい項目の追加**ダイアログ ボックスで、[ファイル名*Person.cs* ] をクリック**追加**です。</span><span class="sxs-lookup"><span data-stu-id="3c312-232">In the **Add New Item** dialog box, name the file *Person.cs* and click **Add**.</span></span>

    ![ユーザー モデル クラスを作成します。](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    <span data-ttu-id="3c312-234">*ユーザー モデル クラスを作成します。*</span><span class="sxs-lookup"><span data-stu-id="3c312-234">*Creating the Person model class*</span></span>
4. <span data-ttu-id="3c312-235">コンテンツを置き換える、 **Person.cs**を次のコード ファイル。</span><span class="sxs-lookup"><span data-stu-id="3c312-235">Replace the content of the **Person.cs** file with the following code.</span></span> <span data-ttu-id="3c312-236">キーを押して**ctrl キーを押しながら S**変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="3c312-236">Press **CTRL + S** to save the changes.</span></span>

    <span data-ttu-id="3c312-237">(コード スニペットの*BringingTogetherOneAspNet - Ex2 - PersonClass*)</span><span class="sxs-lookup"><span data-stu-id="3c312-237">(Code Snippet - *BringingTogetherOneAspNet - Ex2 - PersonClass*)</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. <span data-ttu-id="3c312-238">**ソリューション エクスプ ローラー**を右クリックし、 **MyHybridSite**プロジェクトを選択**ビルド**、またはキーを押して**ctrl キーと shift キーを押しながら B**プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="3c312-238">In **Solution Explorer**, right-click the **MyHybridSite** project and select **Build**, or press **CTRL + SHIFT + B** to build the project.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a><span data-ttu-id="3c312-239">タスク 2-ある MVC コント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="3c312-239">Task 2 – Creating an MVC Controller</span></span>

<span data-ttu-id="3c312-240">これで、 **Person**モデルが作成されると、Entity Framework と ASP.NET MVC のスキャフォールディングを使用すると、CRUD コント ローラーのアクションおよびのビューを作成する**Person**です。</span><span class="sxs-lookup"><span data-stu-id="3c312-240">Now that the **Person** model is created, you will use ASP.NET MVC scaffolding with Entity Framework to create the CRUD controller actions and views for **Person**.</span></span>

1. <span data-ttu-id="3c312-241">**ソリューション エクスプ ローラー**を右クリックし、**コント ローラー**のフォルダー、 **MyHybridSite**プロジェクトし、選択**追加 |新しい項目のスキャフォールディングしています.**.</span><span class="sxs-lookup"><span data-stu-id="3c312-241">In **Solution Explorer**, right-click the **Controllers** folder of the **MyHybridSite** project and select **Add | New Scaffolded Item...**.</span></span>

    ![新しいスキャフォールディング コント ローラーの作成](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    <span data-ttu-id="3c312-243">*スキャフォールディングされた新しいコント ローラーの作成*</span><span class="sxs-lookup"><span data-stu-id="3c312-243">*Creating a new Scaffolded Controller*</span></span>
2. <span data-ttu-id="3c312-244">**追加 Scaffold**ダイアログ ボックスで、 **MVC 5 コント ローラー、ビューがある Entity Framework を使用して** をクリックし、**追加します。**</span><span class="sxs-lookup"><span data-stu-id="3c312-244">In the **Add Scaffold** dialog box, select **MVC 5 Controller with views, using Entity Framework** and then click **Add.**</span></span>

    ![ビューおよび Entity Framework を使用した MVC 5 コント ローラーを選択](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    <span data-ttu-id="3c312-246">*ビューおよび Entity Framework を使用した MVC 5 コント ローラーを選択*</span><span class="sxs-lookup"><span data-stu-id="3c312-246">*Selecting MVC 5 Controller with views and Entity Framework*</span></span>
3. <span data-ttu-id="3c312-247">設定*MvcPersonController*として、**コント ローラー名**、select、**非同期コント ローラー アクションを使用して**をクリックして**人 (MyHybridSite.Models)** として、**モデル クラス**です。</span><span class="sxs-lookup"><span data-stu-id="3c312-247">Set *MvcPersonController* as the **Controller name**, select the **Use async controller actions** option and select **Person (MyHybridSite.Models)** as the **Model class**.</span></span>

    ![スキャフォールディングがある MVC コント ローラーを追加します。](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    <span data-ttu-id="3c312-249">*スキャフォールディングがある MVC コント ローラーを追加します。*</span><span class="sxs-lookup"><span data-stu-id="3c312-249">*Adding an MVC controller with scaffolding*</span></span>
4. <span data-ttu-id="3c312-250">**データ コンテキスト クラス**をクリックして**新しいデータ コンテキストしています.**.</span><span class="sxs-lookup"><span data-stu-id="3c312-250">Under **Data context class**, click **New data context...**.</span></span>

    ![新しいデータ コンテキストを作成します。](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    <span data-ttu-id="3c312-252">*新しいデータ コンテキストを作成します。*</span><span class="sxs-lookup"><span data-stu-id="3c312-252">*Creating a new data context*</span></span>
5. <span data-ttu-id="3c312-253">**新しいデータ コンテキスト** ダイアログ ボックスに、名前の新しいデータ コンテキスト*PersonContext*  をクリック**追加**です。</span><span class="sxs-lookup"><span data-stu-id="3c312-253">In the **New Data Context** dialog box, name the new data context *PersonContext* and click **Add**.</span></span>

    ![新しい PersonContext を作成します。](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    <span data-ttu-id="3c312-255">*新しい PersonContext 型を作成します。*</span><span class="sxs-lookup"><span data-stu-id="3c312-255">*Creating the new PersonContext type*</span></span>
6. <span data-ttu-id="3c312-256">をクリックして**追加**を新しいコント ローラーを作成する**Person**スキャフォールディングとします。</span><span class="sxs-lookup"><span data-stu-id="3c312-256">Click **Add** to create the new controller for **Person** with scaffolding.</span></span> <span data-ttu-id="3c312-257">Visual Studio は、コント ローラーのアクション、Person データ コンテキストおよび Razor ビュー生成し、されます。</span><span class="sxs-lookup"><span data-stu-id="3c312-257">Visual Studio will then generate the controller actions, the Person data context and the Razor views.</span></span>

    ![MVC コント ローラーをスキャフォールディングで作成した後](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    <span data-ttu-id="3c312-259">*MVC コント ローラーをスキャフォールディングで作成した後*</span><span class="sxs-lookup"><span data-stu-id="3c312-259">*After creating the MVC controller with scaffolding*</span></span>
7. <span data-ttu-id="3c312-260">開く、 **MvcPersonController.cs**ファイルで、**コント ローラー**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="3c312-260">Open the **MvcPersonController.cs** file in the **Controllers** folder.</span></span> <span data-ttu-id="3c312-261">CRUD アクション メソッドが自動的に生成されたことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="3c312-261">Notice that the CRUD action methods have been generated automatically.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > <span data-ttu-id="3c312-262">選択して、**非同期コント ローラー アクションを使用して**前の手順でオプションのチェック ボックスをスキャフォールディングから、Visual Studio は、Person データ コンテキストへのアクセスに関連するすべてのアクションの非同期アクション メソッドを生成します。</span><span class="sxs-lookup"><span data-stu-id="3c312-262">By selecting the **Use async controller actions** check box from the scaffolding options in the previous steps, Visual Studio generates asynchronous action methods for all actions that involve access to the Person data context.</span></span> <span data-ttu-id="3c312-263">実行時間の長いの非同期アクション メソッドを使用すると、CPU バインド以外の要求、要求の処理中に作業を実行してから、Web サーバーがブロックされないようにすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="3c312-263">It is recommended that you use asynchronous action methods for long-running, non-CPU bound requests to avoid blocking the Web server from performing work while the request is being processed.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a><span data-ttu-id="3c312-264">タスク 3: ソリューションの実行</span><span class="sxs-lookup"><span data-stu-id="3c312-264">Task 3 – Running the Solution</span></span>

<span data-ttu-id="3c312-265">このタスクではソリューションを実行することを確認するには、もう一度ビュー**人**が想定どおりに機能します。</span><span class="sxs-lookup"><span data-stu-id="3c312-265">In this task, you will run the solution again to verify that the views for **Person** are working as expected.</span></span> <span data-ttu-id="3c312-266">新しいデータベースに正常に保存することを確認する担当者を追加します。</span><span class="sxs-lookup"><span data-stu-id="3c312-266">You will add a new person to verify that it is successfully saved to the database.</span></span>

1. <span data-ttu-id="3c312-267">キーを押して**f5 キーを押して**ソリューションを実行します。</span><span class="sxs-lookup"><span data-stu-id="3c312-267">Press **F5** to run the solution.</span></span>
2. <span data-ttu-id="3c312-268">移動 **/MvcPerson**です。</span><span class="sxs-lookup"><span data-stu-id="3c312-268">Navigate to **/MvcPerson**.</span></span> <span data-ttu-id="3c312-269">ユーザーの一覧を表示するスキャフォールディング ビューが表示されます。</span><span class="sxs-lookup"><span data-stu-id="3c312-269">The scaffolded view that shows the list of people should appear.</span></span>
3. <span data-ttu-id="3c312-270">をクリックして**新規作成**を新しいユーザーを追加します。</span><span class="sxs-lookup"><span data-stu-id="3c312-270">Click **Create New** to add a new person.</span></span>

    ![スキャフォールディングの MVC ビューへの移動](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    <span data-ttu-id="3c312-272">*スキャフォールディングの MVC ビューへの移動*</span><span class="sxs-lookup"><span data-stu-id="3c312-272">*Navigating to the scaffolded MVC views*</span></span>
4. <span data-ttu-id="3c312-273">**作成**ビューで、提供、**名**と**Age**人、およびクリック**作成**。</span><span class="sxs-lookup"><span data-stu-id="3c312-273">In the **Create** view, provide a **Name** and an **Age** for the person, and click **Create**.</span></span>

    ![新しいユーザーを追加します。](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    <span data-ttu-id="3c312-275">*新しいユーザーを追加します。*</span><span class="sxs-lookup"><span data-stu-id="3c312-275">*Adding a new person*</span></span>
5. <span data-ttu-id="3c312-276">新しいユーザーは、一覧に追加されます。</span><span class="sxs-lookup"><span data-stu-id="3c312-276">The new person is added to the list.</span></span> <span data-ttu-id="3c312-277">要素の一覧でクリックして**詳細**人の詳細ビューを表示します。</span><span class="sxs-lookup"><span data-stu-id="3c312-277">In the element list, click **Details** to display the person's details view.</span></span> <span data-ttu-id="3c312-278">その後、、**詳細**ビューで、をクリックして**一覧に戻る**リスト ビューに戻ります。</span><span class="sxs-lookup"><span data-stu-id="3c312-278">Then, in the **Details** view, click **Back to List** to go back to the list view.</span></span>

    ![ユーザーの詳細ビュー](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    <span data-ttu-id="3c312-280">*ユーザーの詳細ビュー*</span><span class="sxs-lookup"><span data-stu-id="3c312-280">*Person's details view*</span></span>
6. <span data-ttu-id="3c312-281">クリックして、**削除**ユーザーを削除するリンクです。</span><span class="sxs-lookup"><span data-stu-id="3c312-281">Click the **Delete** link to delete the person.</span></span> <span data-ttu-id="3c312-282">**削除**ビューで、をクリックして**削除**操作を確認します。</span><span class="sxs-lookup"><span data-stu-id="3c312-282">In the **Delete** view, click **Delete** to confirm the operation.</span></span>

    ![ユーザーを削除します。](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    <span data-ttu-id="3c312-284">*ユーザーを削除します。*</span><span class="sxs-lookup"><span data-stu-id="3c312-284">*Deleting a person*</span></span>
7. <span data-ttu-id="3c312-285">Visual Studio とキーを押してに戻って**shift キーを押しながら f5 キーを押して**デバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="3c312-285">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a><span data-ttu-id="3c312-286">手順 3: スキャフォールディングを使用して Web API コント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="3c312-286">Exercise 3: Creating a Web API Controller Using Scaffolding</span></span>

<span data-ttu-id="3c312-287">Web API フレームワークでは、ASP.NET スタックの一部であるし、実装して HTTP サービスを簡単にするため、通常、RESTful API を通じて JSON または XML 形式のデータの送受信に設計されています。</span><span class="sxs-lookup"><span data-stu-id="3c312-287">The Web API framework is part of the ASP.NET Stack and designed to make implementing HTTP services easier, generally sending and receiving JSON- or XML-formatted data through a RESTful API.</span></span>

<span data-ttu-id="3c312-288">この練習では、Web API コント ローラーを生成するのに ASP.NET スキャフォールディングをもう一度使用されます。</span><span class="sxs-lookup"><span data-stu-id="3c312-288">In this exercise, you will use ASP.NET Scaffolding again to generate a Web API controller.</span></span> <span data-ttu-id="3c312-289">同じ使用する**Person**と**PersonContext** JSON 形式で同じ個人データを提供する前の手順からのクラスです。</span><span class="sxs-lookup"><span data-stu-id="3c312-289">You will use the same **Person** and **PersonContext** classes from the previous exercise to provide the same person data in JSON format.</span></span> <span data-ttu-id="3c312-290">同じ ASP.NET アプリケーション内でさまざまな方法で、同じリソースを公開する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="3c312-290">You will see how you can expose the same resources in different ways within the same ASP.NET application.</span></span>

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a><span data-ttu-id="3c312-291">作業 1: Web API コント ローラーの作成</span><span class="sxs-lookup"><span data-stu-id="3c312-291">Task 1 – Creating a Web API Controller</span></span>

<span data-ttu-id="3c312-292">このタスクでは新規に作成する**Web API コント ローラー**するには、JSON のようにコンピューターが使用できる形式でユーザー データを公開します。</span><span class="sxs-lookup"><span data-stu-id="3c312-292">In this task you will create a new **Web API Controller** that will expose the person data in a machine-consumable format like JSON.</span></span>

1. <span data-ttu-id="3c312-293">開いていない場合は開きます**Visual Studio Express 2013 for Web**を開くと、 **MyHybridSite.sln**にソリューションがある、**ソース/Ex3-WebAPI/開始**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="3c312-293">If not already opened, open **Visual Studio Express 2013 for Web** and open the **MyHybridSite.sln** solution located in the **Source/Ex3-WebAPI/Begin** folder.</span></span> <span data-ttu-id="3c312-294">代わりに、前の手順で取得した、ソリューションを続行できます。</span><span class="sxs-lookup"><span data-stu-id="3c312-294">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3c312-295">手順 3 から開始ソリューションを起動した場合、以下のキーを押して**ctrl キーと shift キーを押しながら B**ソリューションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="3c312-295">If you start with the Begin solution from Exercise 3, press **CTRL + SHIFT + B** to build the solution.</span></span>
2. <span data-ttu-id="3c312-296">**ソリューション エクスプ ローラー**を右クリックし、**コント ローラー**のフォルダー、 **MyHybridSite**プロジェクトし、選択**追加 |新しい項目のスキャフォールディングしています.**.</span><span class="sxs-lookup"><span data-stu-id="3c312-296">In **Solution Explorer**, right-click the **Controllers** folder of the **MyHybridSite** project and select **Add | New Scaffolded Item...**.</span></span>

    ![新しいスキャフォールディング コント ローラーの作成](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    <span data-ttu-id="3c312-298">*新しいスキャフォールディング コント ローラーの作成*</span><span class="sxs-lookup"><span data-stu-id="3c312-298">*Creating a new scaffolded Controller*</span></span>
3. <span data-ttu-id="3c312-299">**追加 Scaffold**ダイアログ ボックスで、 **Web API**し、左ペインで**Web API 2 コント ローラーのアクション、Entity Framework を使用して**中央のペインでをクリックして**追加します。**</span><span class="sxs-lookup"><span data-stu-id="3c312-299">In the **Add Scaffold** dialog box, select **Web API** in the left pane, then **Web API 2 Controller with actions, using Entity Framework** in the middle pane and then click **Add.**</span></span>

    <span data-ttu-id="3c312-300">![アクションおよび Entity Framework と Web API 2 コント ローラーを選択すると](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "を選択すると Web API 2 コント ローラーおよびアクションと Entity Framework")</span><span class="sxs-lookup"><span data-stu-id="3c312-300">![Selecting Web API 2 Controller with actions and Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "Selecting Web API 2 Controller with actions and Entity Framework")</span></span>

    <span data-ttu-id="3c312-301">*アクションおよび Entity Framework と Web API 2 コント ローラーを選択します。*</span><span class="sxs-lookup"><span data-stu-id="3c312-301">*Selecting Web API 2 Controller with actions and Entity Framework*</span></span>
4. <span data-ttu-id="3c312-302">設定*ApiPersonController*として、**コント ローラー名**、select、**非同期コント ローラー アクションを使用して**をクリックして**人 (MyHybridSite.Models)** と**PersonContext (MyHybridSite.Models)** として、**モデル**と**データ コンテキスト**それぞれクラスです。</span><span class="sxs-lookup"><span data-stu-id="3c312-302">Set *ApiPersonController* as the **Controller name**, select the **Use async controller actions** option and select **Person (MyHybridSite.Models)** and **PersonContext (MyHybridSite.Models)** as the **Model** and **Data context** classes respectively.</span></span> <span data-ttu-id="3c312-303">次に、 **[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3c312-303">Then click **Add**.</span></span>

    <span data-ttu-id="3c312-304">![スキャフォールディングが Web API コント ローラーを追加する](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "スキャフォールディングで Web API コント ローラーの追加")</span><span class="sxs-lookup"><span data-stu-id="3c312-304">![Adding a Web API Controller with scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "Adding a Web API controller with scaffolding")</span></span>

    <span data-ttu-id="3c312-305">*スキャフォールディングがある Web API コント ローラーを追加します。*</span><span class="sxs-lookup"><span data-stu-id="3c312-305">*Adding a Web API controller with scaffolding*</span></span>
5. <span data-ttu-id="3c312-306">Visual Studio は生成し、 **ApiPersonController**データを操作する 4 つの CRUD 操作を持つクラス。</span><span class="sxs-lookup"><span data-stu-id="3c312-306">Visual Studio will then generate the **ApiPersonController** class with the four CRUD actions to work with your data.</span></span>

    <span data-ttu-id="3c312-307">![スキャフォールディングで Web API コント ローラーを作成した後](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "スキャフォールディングで Web API コント ローラーを作成した後")</span><span class="sxs-lookup"><span data-stu-id="3c312-307">![After creating the Web API controller with scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "After creating the Web API controller with scaffolding")</span></span>

    <span data-ttu-id="3c312-308">*スキャフォールディングで Web API コント ローラーを作成した後*</span><span class="sxs-lookup"><span data-stu-id="3c312-308">*After creating the Web API controller with scaffolding*</span></span>
6. <span data-ttu-id="3c312-309">開く、 **ApiPersonController.cs**ファイルし、検査、 *GetPeople*アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="3c312-309">Open the **ApiPersonController.cs** file and inspect the *GetPeople* action method.</span></span> <span data-ttu-id="3c312-310">このメソッドはクエリの db フィールド**PersonContext**型、ユーザー データを取得するためにします。</span><span class="sxs-lookup"><span data-stu-id="3c312-310">This method queries the db field of **PersonContext** type in order to get the people data.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. <span data-ttu-id="3c312-311">メソッドの定義の上にあるコメントに注目してください。</span><span class="sxs-lookup"><span data-stu-id="3c312-311">Now notice the comment above the method definition.</span></span> <span data-ttu-id="3c312-312">次のタスクで使用するこのアクションを公開する URI を提供します。</span><span class="sxs-lookup"><span data-stu-id="3c312-312">It provides the URI that exposes this action which you will use in the next task.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > <span data-ttu-id="3c312-313">既定では、Web API が構成されている、クエリをキャッチする、 */api* MVC コント ローラーとの衝突を避けるためへのパス。</span><span class="sxs-lookup"><span data-stu-id="3c312-313">By default, Web API is configured to catch the queries to the */api* path to avoid collisions with MVC controllers.</span></span> <span data-ttu-id="3c312-314">この設定を変更する必要がある場合を参照してください[ASP.NET Web API でルーティング](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md)です。</span><span class="sxs-lookup"><span data-stu-id="3c312-314">If you need to change this setting, refer to [Routing in ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="3c312-315">タスク 2 – ソリューションの実行</span><span class="sxs-lookup"><span data-stu-id="3c312-315">Task 2 – Running the Solution</span></span>

<span data-ttu-id="3c312-316">このタスクでは、Internet Explorer を使用して**F12 開発者ツール**を Web API コント ローラーから応答の全体を検査します。</span><span class="sxs-lookup"><span data-stu-id="3c312-316">In this task you will use the Internet Explorer **F12 developer tools** to inspect the full response from the Web API controller.</span></span> <span data-ttu-id="3c312-317">アプリケーション データをより深く把握へのネットワーク トラフィックをキャプチャする方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="3c312-317">You will see how you can capture network traffic to get more insight into your application data.</span></span>

> [!NOTE]
> <span data-ttu-id="3c312-318">確認して**Internet Explorer**でが選択されている、**開始**Visual Studio ツールバー上にあるボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="3c312-318">Make sure that **Internet Explorer** is selected in the **Start** button located on the Visual Studio toolbar.</span></span>
> 
> ![Internet Explorer オプション](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> <span data-ttu-id="3c312-320">**F12 開発者ツール**幅広いこのハンズオンのラボで説明されていない機能があります。</span><span class="sxs-lookup"><span data-stu-id="3c312-320">The **F12 developer tools** have a wide set of functionality that is not covered in this hands-on-lab.</span></span> <span data-ttu-id="3c312-321">詳細情報を入手する場合を参照してください。 [F12 開発者ツールを使用して](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85))です。</span><span class="sxs-lookup"><span data-stu-id="3c312-321">If you want to learn more about it, refer to [Using the F12 developer tools](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).</span></span>


1. <span data-ttu-id="3c312-322">キーを押して**f5 キーを押して**ソリューションを実行します。</span><span class="sxs-lookup"><span data-stu-id="3c312-322">Press **F5** to run the solution.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3c312-323">このタスクを正常に従うために、アプリケーションをデータである必要があります。</span><span class="sxs-lookup"><span data-stu-id="3c312-323">In order to follow this task correctly, your application needs to have data.</span></span> <span data-ttu-id="3c312-324">データベースが空の場合は、手順 2 での作業 3 に戻るし、MVC ビューを使用して新しいユーザーを作成する方法の手順に従ってです。</span><span class="sxs-lookup"><span data-stu-id="3c312-324">If your database is empty, you can go back to Task 3 in Exercise 2 and follow the steps on how to create a new person using the MVC views.</span></span>
2. <span data-ttu-id="3c312-325">ブラウザーで、キーを押して**F12**を開くには、 **Developer Tools**パネルです。</span><span class="sxs-lookup"><span data-stu-id="3c312-325">In the browser, press **F12** to open the **Developer Tools** panel.</span></span> <span data-ttu-id="3c312-326">キーを押して**CTRL** + **4**  をクリックして、**ネットワーク**アイコンをクリックしてネットワーク トラフィックのキャプチャを開始する緑色の矢印ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="3c312-326">Press **CTRL** + **4** or click the **Network** icon, and then click the green arrow button to begin capturing network traffic.</span></span>

    <span data-ttu-id="3c312-327">![Web API のネットワーク キャプチャを開始する](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "ネットワーク キャプチャを開始する Web API")</span><span class="sxs-lookup"><span data-stu-id="3c312-327">![Initiating Web API network capture](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "Initiating Web API network capture")</span></span>

    <span data-ttu-id="3c312-328">*Web API のネットワーク キャプチャを開始しています*</span><span class="sxs-lookup"><span data-stu-id="3c312-328">*Initiating Web API network capture*</span></span>
3. <span data-ttu-id="3c312-329">追加**api/ApiPerson**ブラウザーのアドレス バーの URL にします。</span><span class="sxs-lookup"><span data-stu-id="3c312-329">Append **api/ApiPerson** to the URL in the browser's address bar.</span></span> <span data-ttu-id="3c312-330">応答の詳細を調査して、ここでは、 **ApiPersonController**です。</span><span class="sxs-lookup"><span data-stu-id="3c312-330">You will now inspect the details of the response from the **ApiPersonController**.</span></span>

    <span data-ttu-id="3c312-331">![Web API を使用してユーザー データを取得する](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "Web API を使用してユーザー データを取得します。")</span><span class="sxs-lookup"><span data-stu-id="3c312-331">![Retrieving person data through Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "Retrieving person data through Web API")</span></span>

    <span data-ttu-id="3c312-332">*Web API を使用してユーザー データを取得します。*</span><span class="sxs-lookup"><span data-stu-id="3c312-332">*Retrieving person data through Web API*</span></span>

    > [!NOTE]
    > <span data-ttu-id="3c312-333">ダウンロードが終了した、ダウンロードしたファイルの操作を行うが求められます。</span><span class="sxs-lookup"><span data-stu-id="3c312-333">Once the download finishes, you will be prompted to make an action with the downloaded file.</span></span> <span data-ttu-id="3c312-334">ダイアログ ボックスは、開発者ツール ウィンドウから応答のコンテンツを監視するために開いたままにしておきます。</span><span class="sxs-lookup"><span data-stu-id="3c312-334">Leave the dialog box open in order to be able to watch the response content through the Developers Tool window.</span></span>
4. <span data-ttu-id="3c312-335">ようになりましたが、応答の本文を検査します。</span><span class="sxs-lookup"><span data-stu-id="3c312-335">Now you will inspect the body of the response.</span></span> <span data-ttu-id="3c312-336">これを行うをクリックして、**詳細** タブをクリックして**応答本文**です。</span><span class="sxs-lookup"><span data-stu-id="3c312-336">To do this, click the **Details** tab and then click **Response body**.</span></span> <span data-ttu-id="3c312-337">ダウンロードされたデータにプロパティを持つオブジェクトの一覧があることを確認できます**Id**、**名前**と**Age**に対応する、 **Person**クラス。</span><span class="sxs-lookup"><span data-stu-id="3c312-337">You can check that the downloaded data is a list of objects with the properties **Id**, **Name** and **Age** that correspond to the **Person** class.</span></span>

    <span data-ttu-id="3c312-338">![表示する Web API の応答本文](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "表示 Web API の応答本文")</span><span class="sxs-lookup"><span data-stu-id="3c312-338">![Viewing Web API Response Body](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "Viewing Web API Response Body")</span></span>

    <span data-ttu-id="3c312-339">*表示する Web API の応答本文*</span><span class="sxs-lookup"><span data-stu-id="3c312-339">*Viewing Web API Response Body*</span></span>

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a><span data-ttu-id="3c312-340">タスク 3: Web API のヘルプ ページの追加</span><span class="sxs-lookup"><span data-stu-id="3c312-340">Task 3 – Adding Web API Help Pages</span></span>

<span data-ttu-id="3c312-341">Web API を作成するときは、他の開発者が API を呼び出す方法を知ることは、ヘルプ ページを作成すると便利です。</span><span class="sxs-lookup"><span data-stu-id="3c312-341">When you create a Web API, it is useful to create a help page so that other developers will know how to call your API.</span></span> <span data-ttu-id="3c312-342">作成し、ドキュメントのページを手動で更新しますが、自動生成すると、メンテナンス作業を行う必要があるようにすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="3c312-342">You could create and update the documentation pages manually, but it is better to auto-generate them to avoid having to do maintenance work.</span></span> <span data-ttu-id="3c312-343">ここでは、Nuget パッケージを使用して、ソリューションに Web API のヘルプ ページを自動的に生成されます。</span><span class="sxs-lookup"><span data-stu-id="3c312-343">In this task you will use a Nuget package to automatically generate Web API help pages to the solution.</span></span>

1. <span data-ttu-id="3c312-344">**ツール**選択、Visual Studio のメニュー**ライブラリ パッケージ マネージャー**、クリックして**パッケージ マネージャー コンソール**です。</span><span class="sxs-lookup"><span data-stu-id="3c312-344">From the **Tools** menu in Visual Studio, select **Library Package Manager**, and then click **Package Manager Console**.</span></span>
2. <span data-ttu-id="3c312-345">**パッケージ マネージャー コンソール**ウィンドウで、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="3c312-345">In the **Package Manager Console** window, execute the following command:</span></span>

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > <span data-ttu-id="3c312-346">**Microsoft.AspNet.WebApi.HelpPage**パッケージが必要なアセンブリをインストールし、下のヘルプ ページの MVC ビューを追加、**領域/HelpPage**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="3c312-346">The **Microsoft.AspNet.WebApi.HelpPage** package installs the necessary assemblies and adds MVC Views for the help pages under the **Areas/HelpPage** folder.</span></span>

    <span data-ttu-id="3c312-347">![HelpPage 領域](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage 領域")</span><span class="sxs-lookup"><span data-stu-id="3c312-347">![HelpPage Area](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage Area")</span></span>

    <span data-ttu-id="3c312-348">*HelpPage 領域*</span><span class="sxs-lookup"><span data-stu-id="3c312-348">*HelpPage Area*</span></span>
3. <span data-ttu-id="3c312-349">既定では、ヘルプ ページがあるドキュメントのプレース ホルダー文字列。</span><span class="sxs-lookup"><span data-stu-id="3c312-349">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="3c312-350">XML ドキュメント コメントを使用するには、ドキュメントを作成します。</span><span class="sxs-lookup"><span data-stu-id="3c312-350">You can use XML documentation comments to create the documentation.</span></span> <span data-ttu-id="3c312-351">この機能を有効にするを開き、 **HelpPageConfig.cs**にあるファイル、**領域/HelpPage/アプリケーション\_開始**フォルダーと、次の行のコメントを解除。</span><span class="sxs-lookup"><span data-stu-id="3c312-351">To enable this feature, open the **HelpPageConfig.cs** file located in the **Areas/HelpPage/App\_Start** folder and uncomment the following line:</span></span>

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. <span data-ttu-id="3c312-352">**ソリューション エクスプ ローラー**、プロジェクトを右クリックして**MyHybridSite****プロパティ** をクリックし、**ビルド** タブ。</span><span class="sxs-lookup"><span data-stu-id="3c312-352">In **Solution Explorer**, right-click the project **MyHybridSite**, select **Properties** and click the **Build** tab.</span></span>

    <span data-ttu-id="3c312-353">![[ビルド] タブ](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "セクションのビルド")</span><span class="sxs-lookup"><span data-stu-id="3c312-353">![Build tab](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "Build section")</span></span>

    <span data-ttu-id="3c312-354">*[ビルド] タブ*</span><span class="sxs-lookup"><span data-stu-id="3c312-354">*Build tab*</span></span>
5. <span data-ttu-id="3c312-355">**出力** **XML ドキュメント ファイル**です。</span><span class="sxs-lookup"><span data-stu-id="3c312-355">Under **Output**, select **XML documentation file**.</span></span> <span data-ttu-id="3c312-356">編集ボックスで、次のように入力します。**アプリ\_Data/XmlDocument.xml**です。</span><span class="sxs-lookup"><span data-stu-id="3c312-356">In the edit box, type **App\_Data/XmlDocument.xml**.</span></span>

    <span data-ttu-id="3c312-357">![[ビルド] タブのセクションを出力](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "[ビルド] タブのセクションの出力")</span><span class="sxs-lookup"><span data-stu-id="3c312-357">![Output section in Build tab](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "Output section in build tab")</span></span>

    <span data-ttu-id="3c312-358">*[ビルド] タブで、出力セクション*</span><span class="sxs-lookup"><span data-stu-id="3c312-358">*Output section in Build tab*</span></span>
6. <span data-ttu-id="3c312-359">キーを押して**CTRL** + **S**変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="3c312-359">Press **CTRL** + **S** to save the changes.</span></span>
7. <span data-ttu-id="3c312-360">開く、 **ApiPersonController.cs**ファイルから、**コント ローラー**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="3c312-360">Open the **ApiPersonController.cs** file from the **Controllers** folder.</span></span>
8. <span data-ttu-id="3c312-361">間に新しい行を入力、 *GetPeople*メソッドのシグネチャと */api/ApiPerson を取得/* コメントの追加、および 3 つのスラッシュを入力します。</span><span class="sxs-lookup"><span data-stu-id="3c312-361">Enter a new line between the *GetPeople* method signature and the *// GET api/ApiPerson* comment, and then type three forward slashes.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3c312-362">Visual Studio は、メソッドのドキュメントを定義する XML 要素を自動的に挿入します。</span><span class="sxs-lookup"><span data-stu-id="3c312-362">Visual Studio automatically inserts the XML elements which define the method documentation.</span></span>
9. <span data-ttu-id="3c312-363">概要テキストと、戻り値の追加、 *GetPeople*メソッドです。</span><span class="sxs-lookup"><span data-stu-id="3c312-363">Add a summary text and the return value for the *GetPeople* method.</span></span> <span data-ttu-id="3c312-364">次のようになります。</span><span class="sxs-lookup"><span data-stu-id="3c312-364">It should look like the following.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. <span data-ttu-id="3c312-365">キーを押して**f5 キーを押して**ソリューションを実行します。</span><span class="sxs-lookup"><span data-stu-id="3c312-365">Press **F5** to run the solution.</span></span>
11. <span data-ttu-id="3c312-366">追加 **/help**ヘルプ ページに移動する、アドレス バーに URL にします。</span><span class="sxs-lookup"><span data-stu-id="3c312-366">Append **/help** to the URL in the address bar to browse to the help page.</span></span>

    <span data-ttu-id="3c312-367">![ASP.NET Web API のヘルプ ページ](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "ヘルプ ページを ASP.NET Web API")</span><span class="sxs-lookup"><span data-stu-id="3c312-367">![ASP.NET Web API Help Page](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "ASP.NET Web API Help Page")</span></span>

    <span data-ttu-id="3c312-368">*ASP.NET Web API Help Page*</span><span class="sxs-lookup"><span data-stu-id="3c312-368">*ASP.NET Web API Help Page*</span></span>

    > [!NOTE]
    > <span data-ttu-id="3c312-369">ページのメイン コンテンツは、コント ローラーでグループ化された Api のテーブルです。</span><span class="sxs-lookup"><span data-stu-id="3c312-369">The main content of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="3c312-370">テーブルのエントリも動的に生成されたを使用して、 **IApiExplorer**インターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="3c312-370">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="3c312-371">追加またはある API コント ローラーを更新する場合、テーブルが自動更新する次に、アプリケーションをビルドするとき。</span><span class="sxs-lookup"><span data-stu-id="3c312-371">If you add or update an API controller, the table will be automatically updated the next time you build the application.</span></span>
    > 
    > <span data-ttu-id="3c312-372">**API** HTTP メソッドと相対 URI に列が一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="3c312-372">The **API** column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="3c312-373">**説明**列には、メソッドのドキュメントから抽出された情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="3c312-373">The **Description** column contains information that has been extracted from the method's documentation.</span></span>
12. <span data-ttu-id="3c312-374">説明 列に、メソッド定義の上に追加する説明が表示されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="3c312-374">Note that the description you added above the method definition is displayed in the description column.</span></span>

    <span data-ttu-id="3c312-375">![API メソッドの説明](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "API メソッドの説明")</span><span class="sxs-lookup"><span data-stu-id="3c312-375">![API method description](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "API method description")</span></span>

    <span data-ttu-id="3c312-376">*API メソッドの説明*</span><span class="sxs-lookup"><span data-stu-id="3c312-376">*API method description*</span></span>
13. <span data-ttu-id="3c312-377">応答例の本文を含む、詳細な情報をページに移動する、API のメソッドのいずれかをクリックします。</span><span class="sxs-lookup"><span data-stu-id="3c312-377">Click one of the API methods to navigate to a page with more detailed information, including sample response bodies.</span></span>

    <span data-ttu-id="3c312-378">![詳細情報ページ](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "詳細情報 ページ")</span><span class="sxs-lookup"><span data-stu-id="3c312-378">![Detail Information page](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "Detail Information page")</span></span>

    <span data-ttu-id="3c312-379">*詳細情報 ページ*</span><span class="sxs-lookup"><span data-stu-id="3c312-379">*Detailed information page*</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="3c312-380">まとめ</span><span class="sxs-lookup"><span data-stu-id="3c312-380">Summary</span></span>

<span data-ttu-id="3c312-381">このハンズオン ラボを完了して学習した方法。</span><span class="sxs-lookup"><span data-stu-id="3c312-381">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="3c312-382">Visual Studio 2013 で、1 つの ASP.NET エクスペリエンスを使用して新しい Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="3c312-382">Create a new Web application using the One ASP.NET Experience in Visual Studio 2013</span></span>
- <span data-ttu-id="3c312-383">1 つの 1 つのプロジェクトに複数の ASP.NET テクノロジを統合します。</span><span class="sxs-lookup"><span data-stu-id="3c312-383">Integrate multiple ASP.NET technologies into one single project</span></span>
- <span data-ttu-id="3c312-384">ASP.NET のスキャフォールディングを使用して、モデル クラスからの MVC コント ローラーとビューを生成します。</span><span class="sxs-lookup"><span data-stu-id="3c312-384">Generate MVC controllers and views from your model classes using ASP.NET Scaffolding</span></span>
- <span data-ttu-id="3c312-385">非同期プログラミングおよび Entity Framework を介してデータ アクセスなどの機能を使用して Web API コント ローラーを生成します。</span><span class="sxs-lookup"><span data-stu-id="3c312-385">Generate Web API controllers, which use features such as Async Programming and data access through Entity Framework</span></span>
- <span data-ttu-id="3c312-386">コント ローラーの Web API のヘルプ ページを自動的に生成します。</span><span class="sxs-lookup"><span data-stu-id="3c312-386">Automatically generate Web API Help Pages for your controllers</span></span>
