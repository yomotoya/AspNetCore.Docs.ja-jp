---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: ASP.NET MVC 4 ヘルパー、フォーム、および検証 |Microsoft ドキュメント
author: rick-anderson
description: ASP.NET MVC 4 モデルおよびデータ アクセスのハンズオン ラボでされて読み込みしてデータベースからデータを表示します。 このハンズオン ラボに追加します.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: 4cfa98144919c3f1bdb3608970af1a7952fe6ea7
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "30878179"
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a><span data-ttu-id="133de-104">ASP.NET MVC 4 ヘルパー、フォーム、および検証</span><span class="sxs-lookup"><span data-stu-id="133de-104">ASP.NET MVC 4 Helpers, Forms and Validation</span></span>

<span data-ttu-id="133de-105">によって[Web キャンプ チーム](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="133de-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="133de-106">Web キャンプ トレーニング キットをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="133de-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="133de-107">**ASP.NET MVC 4 モデルおよびデータ アクセス**ハンズオン ラボされている必要が読み込みと、データベースからデータを表示します。</span><span class="sxs-lookup"><span data-stu-id="133de-107">In **ASP.NET MVC 4 Models and Data Access** Hands-on Lab, you have been loading and displaying data from the database.</span></span> <span data-ttu-id="133de-108">このハンズオン ラボに追加、 **Music Store**アプリケーション データを編集する機能。</span><span class="sxs-lookup"><span data-stu-id="133de-108">In this Hands-on Lab, you will add to the **Music Store** application the ability to edit that data.</span></span>

<span data-ttu-id="133de-109">その目的を念頭にするとには、アルバムの作成、読み取り、更新、削除 (CRUD) 操作をサポートするコント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="133de-109">With that goal in mind, you will first create the controller that will support the Create, Read, Update and Delete (CRUD) actions of albums.</span></span> <span data-ttu-id="133de-110">HTML テーブルのアルバムのプロパティを表示する ASP.NET MVC のスキャフォールディング機能の活用インデックス ビュー テンプレートが生成されます。</span><span class="sxs-lookup"><span data-stu-id="133de-110">You will generate an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span> <span data-ttu-id="133de-111">そのビューを強化するためには、カスタム HTML ヘルパーの長い説明を切り捨てることを追加します。</span><span class="sxs-lookup"><span data-stu-id="133de-111">To enhance that view, you will add a custom HTML helper that will truncate long descriptions.</span></span>

<span data-ttu-id="133de-112">その後、編集しているビューを作成するために使用する alter のドロップダウン リストのようなフォーム要素を利用して、データベースのアルバムを追加します。</span><span class="sxs-lookup"><span data-stu-id="133de-112">Afterwards, you will add the Edit and Create Views that will let you alter the albums in the database, with the help of form elements like dropdowns.</span></span>

<span data-ttu-id="133de-113">最後に、するは、ユーザーがアルバムを削除してもするにその入力を検証することによって正しくないデータを入力します。</span><span class="sxs-lookup"><span data-stu-id="133de-113">Lastly, you will let users delete an album and also you will prevent them from entering wrong data by validating their input.</span></span>

<span data-ttu-id="133de-114">このハンズオン ラボは、の基本的な知識がある前提としています。 **ASP.NET MVC**です。</span><span class="sxs-lookup"><span data-stu-id="133de-114">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="133de-115">使用していない場合**ASP.NET MVC**経由で移動する前をお勧め**ASP.NET MVC の基本事項**ハンズオン ラボ。</span><span class="sxs-lookup"><span data-stu-id="133de-115">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="133de-116">このラボには、機能強化と軽微な変更を元のフォルダーで提供されるサンプル Web アプリケーションに適用することで前に説明した新しい機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="133de-116">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="133de-117">すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[Microsoft の Web/WebCampTrainingKit リリース](https://aka.ms/webcamps-training-kit)です。</span><span class="sxs-lookup"><span data-stu-id="133de-117">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="133de-118">このラボに固有のプロジェクトは[ASP.NET MVC 4 ヘルパー、フォーム、および検証](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation)です。</span><span class="sxs-lookup"><span data-stu-id="133de-118">The project specific to this lab is available at [ASP.NET MVC 4 Helpers, Forms and Validation](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="133de-119">目的</span><span class="sxs-lookup"><span data-stu-id="133de-119">Objectives</span></span>

<span data-ttu-id="133de-120">このハンズオン ラボでは学習する方法。</span><span class="sxs-lookup"><span data-stu-id="133de-120">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="133de-121">CRUD 操作をサポートするためにコント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="133de-121">Create a controller to support CRUD operations</span></span>
- <span data-ttu-id="133de-122">HTML テーブルにエンティティのプロパティを表示する、インデックス ビューを生成します。</span><span class="sxs-lookup"><span data-stu-id="133de-122">Generate an Index View to display entity properties in an HTML table</span></span>
- <span data-ttu-id="133de-123">カスタム HTML ヘルパーを追加します。</span><span class="sxs-lookup"><span data-stu-id="133de-123">Add a custom HTML helper</span></span>
- <span data-ttu-id="133de-124">作成し、編集ビューをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="133de-124">Create and customize an Edit View</span></span>
- <span data-ttu-id="133de-125">HTTP GET または HTTP POST 呼び出しのいずれかに対応するアクション メソッドの違い</span><span class="sxs-lookup"><span data-stu-id="133de-125">Differentiate between action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="133de-126">追加し、ビューの作成のカスタマイズ</span><span class="sxs-lookup"><span data-stu-id="133de-126">Add and customize a Create View</span></span>
- <span data-ttu-id="133de-127">エンティティの削除を処理します。</span><span class="sxs-lookup"><span data-stu-id="133de-127">Handle the deletion of an entity</span></span>
- <span data-ttu-id="133de-128">ユーザー入力を検証します。</span><span class="sxs-lookup"><span data-stu-id="133de-128">Validate user input</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="133de-129">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="133de-129">Prerequisites</span></span>

<span data-ttu-id="133de-130">このラボを完成させるのには、次の項目が必要です。</span><span class="sxs-lookup"><span data-stu-id="133de-130">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="133de-131">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)または上位 (読み取り[付録 A](#AppendixA)をインストールする方法について)。</span><span class="sxs-lookup"><span data-stu-id="133de-131">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="133de-132">セットアップ</span><span class="sxs-lookup"><span data-stu-id="133de-132">Setup</span></span>

<span data-ttu-id="133de-133">**コード スニペットをインストールします。**</span><span class="sxs-lookup"><span data-stu-id="133de-133">**Installing Code Snippets**</span></span>

<span data-ttu-id="133de-134">便宜上、このラボに沿ったを管理するコードの多くは、Visual Studio のコード スニペットとして利用できます。</span><span class="sxs-lookup"><span data-stu-id="133de-134">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="133de-135">実行のコード スニペットをインストールする **.\Source\Setup\CodeSnippets.vsi**ファイル。</span><span class="sxs-lookup"><span data-stu-id="133de-135">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="133de-136">このドキュメントの付録を参照することができます、Visual Studio のコード スニペットとその使用方法を学習するに慣れていない場合&quot;[付録 b: を使用してコード スニペット](#AppendixB)&quot;です。</span><span class="sxs-lookup"><span data-stu-id="133de-136">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="133de-137">演習</span><span class="sxs-lookup"><span data-stu-id="133de-137">Exercises</span></span>

<span data-ttu-id="133de-138">次の演習では、このハンズオン ラボを構成します。</span><span class="sxs-lookup"><span data-stu-id="133de-138">The following exercises make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="133de-139">ストア マネージャー コント ローラーとそのインデックス ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="133de-139">Creating the Store Manager controller and its Index view</span></span>](#Exercise1)
2. [<span data-ttu-id="133de-140">HTML ヘルパーを追加します。</span><span class="sxs-lookup"><span data-stu-id="133de-140">Adding an HTML Helper</span></span>](#Exercise2)
3. [<span data-ttu-id="133de-141">編集ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="133de-141">Creating the Edit View</span></span>](#Exercise3)
4. [<span data-ttu-id="133de-142">作成ビューを追加します。</span><span class="sxs-lookup"><span data-stu-id="133de-142">Adding a Create View</span></span>](#Exercise4)
5. [<span data-ttu-id="133de-143">削除の処理</span><span class="sxs-lookup"><span data-stu-id="133de-143">Handling Deletion</span></span>](#Exercise5)
6. [<span data-ttu-id="133de-144">検証の追加</span><span class="sxs-lookup"><span data-stu-id="133de-144">Adding Validation</span></span>](#Exercise6)
7. [<span data-ttu-id="133de-145">クライアント側での控えめな jQuery の使用</span><span class="sxs-lookup"><span data-stu-id="133de-145">Using Unobtrusive jQuery at Client Side</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="133de-146">各演習が組み込まれた、**終了**演習を完了した後に取得する必要があります、結果として得られるソリューションを含むフォルダー。</span><span class="sxs-lookup"><span data-stu-id="133de-146">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="133de-147">演習では操作のヘルプを参照する必要がある場合は、このソリューションをガイドとして使用できます。</span><span class="sxs-lookup"><span data-stu-id="133de-147">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="133de-148">この演習を完了する時間を推定: **60 分**</span><span class="sxs-lookup"><span data-stu-id="133de-148">Estimated time to complete this lab: **60 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a><span data-ttu-id="133de-149">手順 1: ストア マネージャー コント ローラーとそのインデックス ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="133de-149">Exercise 1: Creating the Store Manager controller and its Index view</span></span>

<span data-ttu-id="133de-150">この演習では、CRUD 操作をサポートするには、そのインデックスは、データベースと ASP.NET MVC のスキャフォールディングを活用インデックス ビュー テンプレートの最後に生成からアルバムの一覧を返すメソッドをアクションのカスタマイズを新しいコント ローラーを作成する方法を学習しますHTML テーブルのアルバムのプロパティを表示する機能です。</span><span class="sxs-lookup"><span data-stu-id="133de-150">In this exercise, you will learn how to create a new controller to support CRUD operations, customize its Index action method to return a list of albums from the database and finally generating an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a><span data-ttu-id="133de-151">タスク 1 -、StoreManagerController を作成します。</span><span class="sxs-lookup"><span data-stu-id="133de-151">Task 1 - Creating the StoreManagerController</span></span>

<span data-ttu-id="133de-152">このタスクと呼ばれる新しいコント ローラーを作成する**StoreManagerController** CRUD 操作をサポートします。</span><span class="sxs-lookup"><span data-stu-id="133de-152">In this task, you will create a new controller called **StoreManagerController** to support CRUD operations.</span></span>

1. <span data-ttu-id="133de-153">開く、**開始**ソリューションにある**ソース/Ex1-CreatingTheStoreManagerController/開始/** フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="133de-153">Open the **Begin** solution located at **Source/Ex1-CreatingTheStoreManagerController/Begin/** folder.</span></span>

   1. <span data-ttu-id="133de-154">いくつか不足している NuGet パッケージをダウンロードする必要がありますが続行前にします。</span><span class="sxs-lookup"><span data-stu-id="133de-154">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="133de-155">これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="133de-155">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="133de-156">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。</span><span class="sxs-lookup"><span data-stu-id="133de-156">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="133de-157">最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。</span><span class="sxs-lookup"><span data-stu-id="133de-157">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="133de-158">NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="133de-158">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="133de-159">NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="133de-159">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="133de-160">これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。</span><span class="sxs-lookup"><span data-stu-id="133de-160">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="133de-161">新しいコント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="133de-161">Add a new controller.</span></span> <span data-ttu-id="133de-162">これを行うを右クリックし、**コント ローラー** [ソリューション エクスプ ローラー] を選択内のフォルダー**追加**し、**コント ローラー**コマンド。</span><span class="sxs-lookup"><span data-stu-id="133de-162">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="133de-163">変更、**コント ローラー** **名前**に**StoreManagerController**オプションの確認と**の空の読み取り/書き込みアクションがあるMVCコントローラー**が選択されています。</span><span class="sxs-lookup"><span data-stu-id="133de-163">Change the **Controller** **Name** to **StoreManagerController** and make sure the option **MVC controller with empty read/write actions** is selected.</span></span> <span data-ttu-id="133de-164">**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="133de-164">Click **Add**.</span></span>

    <span data-ttu-id="133de-165">![コント ローラーの追加ダイアログ](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "コント ローラーの追加ダイアログ")</span><span class="sxs-lookup"><span data-stu-id="133de-165">![Add controller dialog](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Add controller dialog")</span></span>

    <span data-ttu-id="133de-166">*[追加] ダイアログのコント ローラー*</span><span class="sxs-lookup"><span data-stu-id="133de-166">*Add Controller Dialog*</span></span>

    <span data-ttu-id="133de-167">新しいコント ローラー クラスが生成されます。</span><span class="sxs-lookup"><span data-stu-id="133de-167">A new Controller class is generated.</span></span> <span data-ttu-id="133de-168">示されているアクションの読み取り/書き込み、スタブ メソッドを追加するので、一般的な CRUD ƒaƒnƒvƒ ‡ ƒ はアプリケーション固有のロジックを含むメッセージを表示、入力 TODO コメントで作成されます。</span><span class="sxs-lookup"><span data-stu-id="133de-168">Since you indicated to add actions for read/write, stub methods for those, common CRUD actions are created with TODO comments filled in, prompting to include the application specific logic.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a><span data-ttu-id="133de-169">タスク 2 - StoreManager インデックスをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="133de-169">Task 2 - Customizing the StoreManager Index</span></span>

<span data-ttu-id="133de-170">このタスクでは、データベースからアルバムのリストとビューを返す StoreManager Index アクション メソッドをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="133de-170">In this task, you will customize the StoreManager Index action method to return a View with the list of albums from the database.</span></span>

1. <span data-ttu-id="133de-171">StoreManagerController クラスで、次のコードを追加*を使用して*ディレクティブです。</span><span class="sxs-lookup"><span data-stu-id="133de-171">In the StoreManagerController class, add the following *using* directives.</span></span>

    <span data-ttu-id="133de-172">(コード スニペットの*ASP.NET MVC 4 ヘルパーおよびフォーム検証 - Ex1 MvcMusicStore を使用して*)</span><span class="sxs-lookup"><span data-stu-id="133de-172">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 using MvcMusicStore*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. <span data-ttu-id="133de-173">フィールドを追加、 **StoreManagerController**のインスタンスを保持するために**MusicStoreEntities です。**</span><span class="sxs-lookup"><span data-stu-id="133de-173">Add a field to the **StoreManagerController** to hold an instance of **MusicStoreEntities.**</span></span>

    <span data-ttu-id="133de-174">(コード スニペットの*ASP.NET MVC 4 ヘルパーおよびフォーム検証 - Ex1 MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="133de-174">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. <span data-ttu-id="133de-175">アルバムのリストとビューを返す StoreManagerController インデックスの操作を実装します。</span><span class="sxs-lookup"><span data-stu-id="133de-175">Implement the StoreManagerController Index action to return a View with the list of albums.</span></span>

    <span data-ttu-id="133de-176">コント ローラー アクション ロジックは、以前に書き込まれる StoreController のインデックスの操作によく似ていますされます。</span><span class="sxs-lookup"><span data-stu-id="133de-176">The Controller action logic will be very similar to the StoreController's Index action written earlier.</span></span> <span data-ttu-id="133de-177">LINQ を使用すると、すべてのアルバム、ジャンル、アーティストの情報を表示するためなどを取得できます。</span><span class="sxs-lookup"><span data-stu-id="133de-177">Use LINQ to retrieve all albums, including Genre and Artist information for display.</span></span>

    <span data-ttu-id="133de-178">(コード スニペットの*ASP.NET MVC 4 ヘルパーおよびフォーム検証 - Ex1 StoreManagerController インデックス*)</span><span class="sxs-lookup"><span data-stu-id="133de-178">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 StoreManagerController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a><span data-ttu-id="133de-179">タスク 3 - インデックス ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="133de-179">Task 3 - Creating the Index View</span></span>

<span data-ttu-id="133de-180">このタスクでは、によって返されるアルバムの一覧を表示するインデックス ビュー テンプレートを作成します、 **StoreManager**コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="133de-180">In this task, you will create the Index View template to display the list of albums returned by the **StoreManager** Controller.</span></span>

1. <span data-ttu-id="133de-181">新しいテンプレートの表示を作成する前に、プロジェクトをビルドする必要がありますできるように、**ビューの追加 ダイアログ**知って、**アルバム**を使用するクラス。</span><span class="sxs-lookup"><span data-stu-id="133de-181">Before creating the new View template, you should build the project so that the **Add View Dialog** knows about the **Album** class to use.</span></span> <span data-ttu-id="133de-182">選択**ビルド |ビルド MvcMusicStore**プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="133de-182">Select **Build | Build MvcMusicStore** to build the project.</span></span>
2. <span data-ttu-id="133de-183">内部を右クリックし、**インデックス**アクション メソッドと選択**ビューの追加**です。</span><span class="sxs-lookup"><span data-stu-id="133de-183">Right-click inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="133de-184">これが表示されます、**ビューの追加**ダイアログ。</span><span class="sxs-lookup"><span data-stu-id="133de-184">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="133de-185">![ビューを追加](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "ビューを追加します。")</span><span class="sxs-lookup"><span data-stu-id="133de-185">![Add view](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Add view")</span></span>

    <span data-ttu-id="133de-186">*インデックス メソッド内からビューを追加します。*</span><span class="sxs-lookup"><span data-stu-id="133de-186">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="133de-187">ビューの追加] ダイアログ ボックスで [ビュー名があることを確認します**インデックス**です。</span><span class="sxs-lookup"><span data-stu-id="133de-187">In the Add View dialog, verify that the View Name is **Index**.</span></span> <span data-ttu-id="133de-188">選択、**厳密に型指定されたビューを作成する**オプション、および選択**アルバム (MvcMusicStore.Models)** から、**モデル クラス**ドロップダウンします。</span><span class="sxs-lookup"><span data-stu-id="133de-188">Select the **Create a strongly-typed view** option, and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="133de-189">選択**リスト**から、 **Scaffold テンプレート**ドロップダウンします。</span><span class="sxs-lookup"><span data-stu-id="133de-189">Select **List** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="133de-190">ままにして、**ビュー エンジン**に**Razor**クリックし、その既定値はその他のフィールド値**追加**です。</span><span class="sxs-lookup"><span data-stu-id="133de-190">Leave the **View engine** to **Razor** and the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="133de-191">![インデックス ビューの追加](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "インデックス ビューの追加")</span><span class="sxs-lookup"><span data-stu-id="133de-191">![Adding an index view](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Adding an index view")</span></span>

    <span data-ttu-id="133de-192">*インデックス ビューの追加*</span><span class="sxs-lookup"><span data-stu-id="133de-192">*Adding an Index View*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a><span data-ttu-id="133de-193">タスク 4 - インデックス ビューのスキャフォールディングをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="133de-193">Task 4 - Customizing the scaffold of the Index View</span></span>

<span data-ttu-id="133de-194">このタスクでは、ASP.NET MVC のスキャフォールディング機能に必要なフィールドを表示することで作成された単純なビュー テンプレートを調整します。</span><span class="sxs-lookup"><span data-stu-id="133de-194">In this task, you will adjust the simple View template created with ASP.NET MVC scaffolding feature to have it display the fields you want.</span></span>

> [!NOTE]
> <span data-ttu-id="133de-195">**スキャフォールディング**ASP.NET MVC でのサポートには、アルバムのモデルのすべてのフィールドの一覧を表示する単純なビュー テンプレートが生成されます。</span><span class="sxs-lookup"><span data-stu-id="133de-195">The **scaffolding** support within ASP.NET MVC generates a simple View template which lists all fields in the Album model.</span></span> <span data-ttu-id="133de-196">**スキャフォールディング**厳密に型指定されたビューで作業を開始する簡単な方法を提供します。 ビュー テンプレートを手動で記述するのではなくすばやくスキャフォールディング テンプレートが生成される既定値とし、生成されたコードを変更することができます。</span><span class="sxs-lookup"><span data-stu-id="133de-196">**Scaffolding** provides a quick way to get started on a strongly typed view: rather than having to write the View template manually, scaffolding quickly generates a default template and then you can modify the generated code.</span></span>


1. <span data-ttu-id="133de-197">作成したコードを確認します。</span><span class="sxs-lookup"><span data-stu-id="133de-197">Review the code created.</span></span> <span data-ttu-id="133de-198">生成されたフィールドの一覧を次の一部となる HTML テーブルを**スキャフォールディング**表形式のデータを表示するために使用されています。</span><span class="sxs-lookup"><span data-stu-id="133de-198">The generated list of fields will be part of the following HTML table that **Scaffolding** is using for displaying tabular data.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. <span data-ttu-id="133de-199">置換、 **&lt;テーブル&gt;** コードのみを表示する次のコードと、**ジャンル**、**アーティスト**、**アルバム タイトル**、および**価格**フィールドです。</span><span class="sxs-lookup"><span data-stu-id="133de-199">Replace the **&lt;table&gt;** code with the following code to display only the **Genre**, **Artist**, **Album Title**, and **Price** fields.</span></span> <span data-ttu-id="133de-200">これにより、削除、 **AlbumId**と**アルバム アート URL**列です。</span><span class="sxs-lookup"><span data-stu-id="133de-200">This deletes the **AlbumId** and **Album Art URL** columns.</span></span> <span data-ttu-id="133de-201">GenreId と ArtistId を表示する列の場合は、そのリンクされたクラス プロパティを変更ことも、 **Artist.Name**と**Genre.Name**、し、削除、**詳細**リンクします。</span><span class="sxs-lookup"><span data-stu-id="133de-201">Also, it changes GenreId and ArtistId columns to display their linked class properties of **Artist.Name** and **Genre.Name**, and removes the **Details** link.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. <span data-ttu-id="133de-202">次の説明を変更します。</span><span class="sxs-lookup"><span data-stu-id="133de-202">Change the following descriptions.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="133de-203">タスク 5 - アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="133de-203">Task 5 - Running the Application</span></span>

<span data-ttu-id="133de-204">このタスクは、テストを**StoreManager** **インデックス**ビュー テンプレートは、前の手順のデザインに従ってアルバムの一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="133de-204">In this task, you will test that the **StoreManager** **Index** View template displays a list of albums according to the design of the previous steps.</span></span>

1. <span data-ttu-id="133de-205">キーを押して**f5 キーを押して**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="133de-205">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="133de-206">ホーム ページで、プロジェクトを開始します。</span><span class="sxs-lookup"><span data-stu-id="133de-206">The project starts in the Home page.</span></span> <span data-ttu-id="133de-207">URL を変更して **/StoreManager**アルバムの一覧が表示されることを示すことを確認する、**タイトル**、**アーティスト**と**ジャンル**です。</span><span class="sxs-lookup"><span data-stu-id="133de-207">Change the URL to **/StoreManager** to verify that a list of albums is displayed, showing their **Title**, **Artist** and **Genre**.</span></span>

    <span data-ttu-id="133de-208">![アルバムの一覧を参照](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "アルバムの一覧を参照")</span><span class="sxs-lookup"><span data-stu-id="133de-208">![Browsing the list of albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Browsing the list of albums")</span></span>

    <span data-ttu-id="133de-209">*アルバムの一覧を参照*</span><span class="sxs-lookup"><span data-stu-id="133de-209">*Browsing the list of albums*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a><span data-ttu-id="133de-210">手順 2: HTML ヘルパーを追加します。</span><span class="sxs-lookup"><span data-stu-id="133de-210">Exercise 2: Adding an HTML Helper</span></span>

<span data-ttu-id="133de-211">StoreManager インデックス ページが潜在的な問題の 1 つ。 タイトル、アーティスト名のプロパティは両方とも、テーブルの書式設定をスローするのに十分な長さです。</span><span class="sxs-lookup"><span data-stu-id="133de-211">The StoreManager Index page has one potential issue: Title and Artist Name properties can both be long enough to throw off the table formatting.</span></span> <span data-ttu-id="133de-212">この演習では、そのテキストの切り捨てを行うためのカスタム HTML ヘルパーを追加する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="133de-212">In this exercise you will learn how to add a custom HTML helper to truncate that text.</span></span>

<span data-ttu-id="133de-213">次の図では、サイズの小さなブラウザーを使用するときに、テキストの長さのために変更が、形式の方法がわかります。</span><span class="sxs-lookup"><span data-stu-id="133de-213">In the following figure, you can see how the format is modified because of the length of the text when you use a small browser size.</span></span>

<span data-ttu-id="133de-214">![テキストは切り捨てられませんされたアルバムの一覧を参照](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "テキストは切り捨てられませんされたアルバムの一覧を参照")</span><span class="sxs-lookup"><span data-stu-id="133de-214">![Browsing the list of Albums with not truncated text](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Browsing the list of Albums with not truncated text")</span></span>

<span data-ttu-id="133de-215">*テキストは切り捨てられませんされたアルバムの一覧を参照*</span><span class="sxs-lookup"><span data-stu-id="133de-215">*Browsing the list of Albums with not truncated text*</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a><span data-ttu-id="133de-216">タスク 1 - された HTML ヘルパーを拡張します。</span><span class="sxs-lookup"><span data-stu-id="133de-216">Task 1 - Extending the HTML Helper</span></span>

<span data-ttu-id="133de-217">このタスクでは、新しいメソッドを追加します**Truncate**を**HTML** ASP.NET MVC ビューの中で公開されたオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="133de-217">In this task, you will add a new method **Truncate** to the **HTML** object exposed within ASP.NET MVC Views.</span></span> <span data-ttu-id="133de-218">これを行うにを実装する、**拡張メソッド**組み込み**System.Web.Mvc.HtmlHelper** ASP.NET MVC によって提供されるクラスです。</span><span class="sxs-lookup"><span data-stu-id="133de-218">To do this, you will implement an **extension method** to the built-in **System.Web.Mvc.HtmlHelper** class provided by ASP.NET MVC.</span></span>

> [!NOTE]
> <span data-ttu-id="133de-219">詳細を確認する**拡張メソッド**、この msdn の記事を参照してください。</span><span class="sxs-lookup"><span data-stu-id="133de-219">To read more about **Extension Methods**, please visit this msdn article.</span></span> <span data-ttu-id="133de-220">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span><span class="sxs-lookup"><span data-stu-id="133de-220">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span></span>


1. <span data-ttu-id="133de-221">開く、**開始**ソリューションにある**ソース/Ex2-AddingAnHTMLHelper/開始/** フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="133de-221">Open the **Begin** solution located at **Source/Ex2-AddingAnHTMLHelper/Begin/** folder.</span></span> <span data-ttu-id="133de-222">それ以外の場合、作業を続行できますを使用して、**終了**ソリューションは、前の手順を完了して取得します。</span><span class="sxs-lookup"><span data-stu-id="133de-222">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="133de-223">指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。</span><span class="sxs-lookup"><span data-stu-id="133de-223">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="133de-224">これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="133de-224">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="133de-225">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。</span><span class="sxs-lookup"><span data-stu-id="133de-225">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="133de-226">最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。</span><span class="sxs-lookup"><span data-stu-id="133de-226">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="133de-227">NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="133de-227">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="133de-228">NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="133de-228">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="133de-229">これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。</span><span class="sxs-lookup"><span data-stu-id="133de-229">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="133de-230">StoreManager のインデックス ビューを開きます。</span><span class="sxs-lookup"><span data-stu-id="133de-230">Open StoreManager's Index View.</span></span> <span data-ttu-id="133de-231">これを行うには、ソリューション エクスプ ローラーで、展開、**ビュー** 、フォルダー、 **StoreManager**を開くと、 **Index.cshtml**ファイル。</span><span class="sxs-lookup"><span data-stu-id="133de-231">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
3. <span data-ttu-id="133de-232">下の次のコードを追加、 <strong>@model</strong>を定義するディレクティブ、 <strong>Truncate</strong>ヘルパー メソッドです。</span><span class="sxs-lookup"><span data-stu-id="133de-232">Add the following code below the <strong>@model</strong> directive to define the <strong>Truncate</strong> helper method.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a><span data-ttu-id="133de-233">作業 2: ページのテキストが切り捨て</span><span class="sxs-lookup"><span data-stu-id="133de-233">Task 2 - Truncating Text in the Page</span></span>

<span data-ttu-id="133de-234">このタスクでは使用して、 **Truncate**ビュー テンプレート内のテキストを短くします。</span><span class="sxs-lookup"><span data-stu-id="133de-234">In this task, you will use the **Truncate** method to truncate the text in the View template.</span></span>

1. <span data-ttu-id="133de-235">StoreManager のインデックス ビューを開きます。</span><span class="sxs-lookup"><span data-stu-id="133de-235">Open StoreManager's Index View.</span></span> <span data-ttu-id="133de-236">これを行うには、ソリューション エクスプ ローラーで、展開、**ビュー** 、フォルダー、 **StoreManager**を開くと、 **Index.cshtml**ファイル。</span><span class="sxs-lookup"><span data-stu-id="133de-236">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
2. <span data-ttu-id="133de-237">表示する行を置き換える、**アーティスト名**およびアルバムの**タイトル**です。</span><span class="sxs-lookup"><span data-stu-id="133de-237">Replace the lines that show the **Artist Name** and Album's **Title**.</span></span> <span data-ttu-id="133de-238">これを行うには、次の行を置き換えます。</span><span class="sxs-lookup"><span data-stu-id="133de-238">To do this, replace the following lines.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="133de-239">タスク 3 - アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="133de-239">Task 3 - Running the Application</span></span>

<span data-ttu-id="133de-240">このタスクは、テストを**StoreManager** **インデックス**アルバムのタイトル、アーティスト名、ビュー テンプレートが切り捨てられます。</span><span class="sxs-lookup"><span data-stu-id="133de-240">In this task, you will test that the **StoreManager** **Index** View template truncates the Album's Title and Artist Name.</span></span>

1. <span data-ttu-id="133de-241">キーを押して**f5 キーを押して**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="133de-241">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="133de-242">ホーム ページで、プロジェクトを開始します。</span><span class="sxs-lookup"><span data-stu-id="133de-242">The project starts in the Home page.</span></span> <span data-ttu-id="133de-243">URL を変更して **/StoreManager**その時間の長いことを確認するテキストで、**タイトル**と**アーティスト**列は切り捨てられます。</span><span class="sxs-lookup"><span data-stu-id="133de-243">Change the URL to **/StoreManager** to verify that long texts in the **Title** and **Artist** column are truncated.</span></span>

    <span data-ttu-id="133de-244">![切り捨てのタイトルやアーティスト名](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "切り捨てタイトルやアーティスト名")</span><span class="sxs-lookup"><span data-stu-id="133de-244">![Truncated titles and artists names](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Truncated titles and artists names")</span></span>

    <span data-ttu-id="133de-245">*切り捨てられたタイトル、アーティスト名*</span><span class="sxs-lookup"><span data-stu-id="133de-245">*Truncated Titles and Artist Names*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a><span data-ttu-id="133de-246">手順 3: 編集ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="133de-246">Exercise 3: Creating the Edit View</span></span>

<span data-ttu-id="133de-247">この演習では、ストア マネージャー アルバムを編集するためのフォームを作成する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="133de-247">In this exercise, you will learn how to create a form to allow store managers to edit an Album.</span></span> <span data-ttu-id="133de-248">参照する、 **/StoreManager/Edit/id** URL (**id**を編集するアルバムの一意の id をされている)、サーバーへの HTTP GET 呼び出しになります。</span><span class="sxs-lookup"><span data-stu-id="133de-248">They will browse the **/StoreManager/Edit/id** URL (**id** being the unique id of the album to edit), thus making an HTTP-GET call to the server.</span></span>

<span data-ttu-id="133de-249">アクション メソッドのコント ローラーの編集は、データベースから適切なアルバムを取得、作成、 **StoreManagerViewModel** (アーティスト、ジャンルの一覧)、およびカプセル化およびにビュー テンプレートに渡すオブジェクトユーザーに、HTML ページを表示します。</span><span class="sxs-lookup"><span data-stu-id="133de-249">The Controller Edit action method will retrieve the appropriate Album from the database, create a **StoreManagerViewModel** object to encapsulate it (along with a list of Artists and Genres), and then pass it off to a View template to render the HTML page back to the user.</span></span> <span data-ttu-id="133de-250">このページには、 **&lt;フォーム&gt;** テキスト ボックスとアルバムのプロパティを編集するためのドロップダウン リストを持つ要素。</span><span class="sxs-lookup"><span data-stu-id="133de-250">This page will contain a **&lt;form&gt;** element with textboxes and dropdowns for editing the Album properties.</span></span>

<span data-ttu-id="133de-251">ユーザーは、アルバムのフォーム値を更新しをクリックした後、**保存** ボタンを使用して、変更を送信する HTTP POST にコールバックする **/StoreManager/Edit/id**です。URL は、最後の呼び出しの場合と同様、ASP.NET MVC を識別するこの時間は、HTTP POST し、そのため、別の編集操作方法を実行 (で修飾された 1 つ **[HttpPost]**)。</span><span class="sxs-lookup"><span data-stu-id="133de-251">Once the user updates the Album form values and clicks the **Save** button, the changes are submitted via an HTTP-POST call back to **/StoreManager/Edit/id**. Although the URL remains the same as in the last call, ASP.NET MVC identifies that this time it is an HTTP-POST and therefore executes a different Edit action method (one decorated with **[HttpPost]**).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a><span data-ttu-id="133de-252">タスク 1 - HTTP GET 編集アクション メソッドの実装</span><span class="sxs-lookup"><span data-stu-id="133de-252">Task 1 - Implementing the HTTP-GET Edit Action Method</span></span>

<span data-ttu-id="133de-253">このタスクでは、データベースから適切なアルバムを取得する編集のアクション メソッドの HTTP GET バージョンだけでなくすべてのジャンル、アーティストの一覧を実装します。</span><span class="sxs-lookup"><span data-stu-id="133de-253">In this task, you will implement the HTTP-GET version of the Edit action method to retrieve the appropriate Album from the database, as well as a list of all Genres and Artists.</span></span> <span data-ttu-id="133de-254">これはパッケージ化このデータ**StoreManagerViewModel**で応答を表示するためにテンプレートの表示にし、渡される、最後の手順で定義されているオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="133de-254">It will package this data up into the **StoreManagerViewModel** object defined in the last step, which will then be passed to a View template to render the response with.</span></span>

1. <span data-ttu-id="133de-255">開く、**開始**ソリューションにある**ソース/Ex3-CreatingTheEditView/開始/** フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="133de-255">Open the **Begin** solution located at **Source/Ex3-CreatingTheEditView/Begin/** folder.</span></span> <span data-ttu-id="133de-256">それ以外の場合、作業を続行できますを使用して、**終了**ソリューションは、前の手順を完了して取得します。</span><span class="sxs-lookup"><span data-stu-id="133de-256">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="133de-257">指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。</span><span class="sxs-lookup"><span data-stu-id="133de-257">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="133de-258">これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="133de-258">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="133de-259">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。</span><span class="sxs-lookup"><span data-stu-id="133de-259">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="133de-260">最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。</span><span class="sxs-lookup"><span data-stu-id="133de-260">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="133de-261">NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="133de-261">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="133de-262">NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="133de-262">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="133de-263">これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。</span><span class="sxs-lookup"><span data-stu-id="133de-263">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="133de-264">開く、 **StoreManagerController**クラスです。</span><span class="sxs-lookup"><span data-stu-id="133de-264">Open the **StoreManagerController** class.</span></span> <span data-ttu-id="133de-265">これを行うには、展開、**コント ローラー**フォルダーおよびダブルクリック**StoreManagerController.cs**です。</span><span class="sxs-lookup"><span data-stu-id="133de-265">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="133de-266">置換、 **HTTP-GET 編集**アクション メソッドを適切な取得は、次のコードを**アルバム**だけでなく**ジャンル**と**アーティスト**を一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="133de-266">Replace the **HTTP-GET Edit** action method with the following code to retrieve the appropriate **Album** as well as the **Genres** and **Artists** lists.</span></span>

    <span data-ttu-id="133de-267">(コード スニペットの*ASP.NET MVC 4 ヘルパーおよびフォームと検証のアクションの Ex3 StoreManagerController HTTP-GET 編集*)</span><span class="sxs-lookup"><span data-stu-id="133de-267">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-GET Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="133de-268">使用している**System.Web.Mvc** **SelectList**アーティスト、ジャンルの代わりに、 **System.Collections.Generic**  ボックスの一覧です。</span><span class="sxs-lookup"><span data-stu-id="133de-268">You are using **System.Web.Mvc** **SelectList** for Artists and Genres instead of the **System.Collections.Generic** List.</span></span>
    > 
    > <span data-ttu-id="133de-269">**SelectList**クリーナーの方法を HTML ドロップダウン リストを作成し、現在の選択などを管理します。</span><span class="sxs-lookup"><span data-stu-id="133de-269">**SelectList** is a cleaner way to populate HTML dropdowns and manage things like current selection.</span></span> <span data-ttu-id="133de-270">インスタンス化して、コント ローラーのアクションでこれらの ViewModel オブジェクトの後で設定すると、編集フォーム シナリオ クリーナーです。</span><span class="sxs-lookup"><span data-stu-id="133de-270">Instantiating and later setting up these ViewModel objects in the controller action will make the Edit form scenario cleaner.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a><span data-ttu-id="133de-271">タスク 2 - 編集ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="133de-271">Task 2 - Creating the Edit View</span></span>

<span data-ttu-id="133de-272">このタスクでは、後で、アルバムのプロパティを表示するビューの編集 テンプレートを作成します。</span><span class="sxs-lookup"><span data-stu-id="133de-272">In this task, you will create an Edit View template that will later display the album properties.</span></span>

1. <span data-ttu-id="133de-273">編集ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="133de-273">Create the Edit View.</span></span> <span data-ttu-id="133de-274">これを行うには、内部を右クリックして、**編集**アクション メソッドと選択**ビューの追加**です。</span><span class="sxs-lookup"><span data-stu-id="133de-274">To do this, right-click inside the **Edit** action method and select **Add View**.</span></span>
2. <span data-ttu-id="133de-275">ビューの追加] ダイアログ ボックスで [ビュー名があることを確認します**編集**です。</span><span class="sxs-lookup"><span data-stu-id="133de-275">In the Add View dialog, verify that the View Name is **Edit**.</span></span> <span data-ttu-id="133de-276">チェック、**厳密に型指定されたビューを作成する** チェック ボックスを選択し、**アルバム (MvcMusicStore.Models)** から、**データ クラスを表示**ドロップダウンします。</span><span class="sxs-lookup"><span data-stu-id="133de-276">Check the **Create a strongly-typed view** checkbox and select **Album (MvcMusicStore.Models)** from the **View data class** drop-down.</span></span> <span data-ttu-id="133de-277">選択**編集**から、 **Scaffold テンプレート**ドロップダウンします。</span><span class="sxs-lookup"><span data-stu-id="133de-277">Select **Edit** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="133de-278">既定値は、他のフィールドのままにし、をクリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="133de-278">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="133de-279">![編集ビューを追加する](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "編集ビューを追加します。")</span><span class="sxs-lookup"><span data-stu-id="133de-279">![Adding an Edit view](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Adding an Edit view")</span></span>

    <span data-ttu-id="133de-280">*編集ビューを追加します。*</span><span class="sxs-lookup"><span data-stu-id="133de-280">*Adding an Edit view*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="133de-281">タスク 3 - アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="133de-281">Task 3 - Running the Application</span></span>

<span data-ttu-id="133de-282">このタスクは、テストを**StoreManager** **編集**ビュー ページには、パラメーターとして渡されるアルバムのプロパティの値が表示されます。</span><span class="sxs-lookup"><span data-stu-id="133de-282">In this task, you will test that the **StoreManager** **Edit** View page displays the properties' values for the album passed as parameter.</span></span>

1. <span data-ttu-id="133de-283">キーを押して**f5 キーを押して**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="133de-283">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="133de-284">ホーム ページで、プロジェクトを開始します。</span><span class="sxs-lookup"><span data-stu-id="133de-284">The project starts in the Home page.</span></span> <span data-ttu-id="133de-285">URL を変更して **/StoreManager/Edit/1**を渡されるアルバムのプロパティの値が表示されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="133de-285">Change the URL to **/StoreManager/Edit/1** to verify that the properties' values for the album passed are displayed.</span></span>

    <span data-ttu-id="133de-286">![アルバムの編集ビューを参照](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "アルバムの編集ビューを参照")</span><span class="sxs-lookup"><span data-stu-id="133de-286">![Browsing Album's Edit View](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Browsing Album's Edit View")</span></span>

    <span data-ttu-id="133de-287">*アルバムの編集ビューを参照*</span><span class="sxs-lookup"><span data-stu-id="133de-287">*Browsing Album's Edit view*</span></span>

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a><span data-ttu-id="133de-288">タスク 4 - アルバム エディター テンプレートをドロップダウン リストを実装します。</span><span class="sxs-lookup"><span data-stu-id="133de-288">Task 4 - Implementing drop-downs on the Album Editor Template</span></span>

<span data-ttu-id="133de-289">このタスクでは、ユーザーは、アーティスト、ジャンルの一覧から選択できるように、前回の作業で作成したビュー テンプレートをドロップダウン リストを追加します。</span><span class="sxs-lookup"><span data-stu-id="133de-289">In this task, you will add drop-downs to the View template created in the last task, so that the user can select from a list of Artists and Genres.</span></span>

1. <span data-ttu-id="133de-290">[すべて置換]、**アルバム**を次のフィールド セット コード。</span><span class="sxs-lookup"><span data-stu-id="133de-290">Replace all the **Album** fieldset code with the following:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="133de-291">**Html.DropDownList**アーティスト、ジャンルを選択するためのドロップダウン リストを表示するためにヘルパーが追加されました。</span><span class="sxs-lookup"><span data-stu-id="133de-291">An **Html.DropDownList** helper has been added to render drop-downs for choosing Artists and Genres.</span></span> <span data-ttu-id="133de-292">渡されるパラメーター **Html.DropDownList**は。</span><span class="sxs-lookup"><span data-stu-id="133de-292">The parameters passed to **Html.DropDownList** are:</span></span>
    > 
    > 1. <span data-ttu-id="133de-293">フォーム フィールドの名前 (**&quot;ArtistId&quot;**)。</span><span class="sxs-lookup"><span data-stu-id="133de-293">The name of the form field (**&quot;ArtistId&quot;**).</span></span>
    > 2. <span data-ttu-id="133de-294">**SelectList**ドロップダウン リストの値。</span><span class="sxs-lookup"><span data-stu-id="133de-294">The **SelectList** of values for the drop-down.</span></span>

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="133de-295">タスク 5 - アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="133de-295">Task 5 - Running the Application</span></span>

<span data-ttu-id="133de-296">このタスクは、テストを**StoreManager** **編集**ビュー ページには、アーティスト、ジャンル ID のテキスト フィールドではなくドロップダウン リストが表示されます。</span><span class="sxs-lookup"><span data-stu-id="133de-296">In this task, you will test that the **StoreManager** **Edit** View page displays drop-downs instead of Artist and Genre ID text fields.</span></span>

1. <span data-ttu-id="133de-297">キーを押して**f5 キーを押して**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="133de-297">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="133de-298">ホーム ページで、プロジェクトを開始します。</span><span class="sxs-lookup"><span data-stu-id="133de-298">The project starts in the Home page.</span></span> <span data-ttu-id="133de-299">URL を変更して **/StoreManager/Edit/1**アーティスト、ジャンル ID のテキスト フィールドではなくドロップダウン リストが表示されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="133de-299">Change the URL to **/StoreManager/Edit/1** to verify that it displays drop-downs instead of Artist and Genre ID text fields.</span></span>

    <span data-ttu-id="133de-300">![アルバムのドロップダウン リストでビューの編集をブラウズ](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "参照アルバムのドロップダウン リストでビューの編集")</span><span class="sxs-lookup"><span data-stu-id="133de-300">![Browsing Album's Edit View with drop downs](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Browsing Album's Edit View with drop downs")</span></span>

    <span data-ttu-id="133de-301">*アルバムの編集ビューのこの時点でのドロップダウン リストを参照*</span><span class="sxs-lookup"><span data-stu-id="133de-301">*Browsing Album's Edit view, this time with dropdowns*</span></span>

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a><span data-ttu-id="133de-302">タスク 6 - HTTP POST Edit アクション メソッドを実装します。</span><span class="sxs-lookup"><span data-stu-id="133de-302">Task 6 - Implementing the HTTP-POST Edit action method</span></span>

<span data-ttu-id="133de-303">ビューの編集は、期待どおりに表示されます、アルバムへの変更を保存するアクションの編集 HTTP POST メソッドを実装する必要があります。</span><span class="sxs-lookup"><span data-stu-id="133de-303">Now that the Edit View displays as expected, you need to implement the HTTP-POST Edit Action method to save the changes made to the Album.</span></span>

1. <span data-ttu-id="133de-304">Visual Studio のウィンドウに戻るには、必要な場合は、ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="133de-304">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="133de-305">開いている**StoreManagerController**から、**コント ローラー**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="133de-305">Open **StoreManagerController** from the **Controllers** folder.</span></span>
2. <span data-ttu-id="133de-306">置き換える**HTTP POST 編集**を次のアクション メソッドのコード (置き換える必要があるメソッドを 2 つのパラメーターを受け取るオーバー ロードされたバージョンであることに注意してください)。</span><span class="sxs-lookup"><span data-stu-id="133de-306">Replace **HTTP-POST Edit** action method code with the following (note that the method that must be replaced is overloaded version that receives two parameters):</span></span>

    <span data-ttu-id="133de-307">(コード スニペットの*ASP.NET MVC 4 ヘルパーおよびフォームと検証のアクションの Ex3 StoreManagerController HTTP POST の編集*)</span><span class="sxs-lookup"><span data-stu-id="133de-307">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-POST Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="133de-308">このメソッドは、ユーザーがクリックしたときに、実行は、**保存**ビューのボタンをクリックし、HTTP POST サーバーへのフォーム値のデータベース内に保持するを実行します。</span><span class="sxs-lookup"><span data-stu-id="133de-308">This method will be executed when the user clicks the **Save** button of the View and performs an HTTP-POST of the form values back to the server to persist them in the database.</span></span> <span data-ttu-id="133de-309">デコレータ **[HttpPost]** HTTP POST シナリオのメソッドを使用することを示します。</span><span class="sxs-lookup"><span data-stu-id="133de-309">The decorator **[HttpPost]** indicates that the method should be used for those HTTP-POST scenarios.</span></span> <span data-ttu-id="133de-310">メソッドは、**アルバム**オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="133de-310">The method takes an **Album** object.</span></span> <span data-ttu-id="133de-311">ASP.NET MVC が、ポストされたからアルバム オブジェクトを自動的に作成します。&lt;フォーム&gt;値。</span><span class="sxs-lookup"><span data-stu-id="133de-311">ASP.NET MVC will automatically create the Album object from the posted &lt;form&gt; values.</span></span>
    > 
    > <span data-ttu-id="133de-312">メソッドは、これらの手順が実行されます。</span><span class="sxs-lookup"><span data-stu-id="133de-312">The method will perform these steps:</span></span>
    > 
    > 1. <span data-ttu-id="133de-313">モデルが有効な場合。</span><span class="sxs-lookup"><span data-stu-id="133de-313">If model is valid:</span></span>
    > 
    >     1. <span data-ttu-id="133de-314">変更されたオブジェクトをマークするコンテキストでアルバム エントリを更新します。</span><span class="sxs-lookup"><span data-stu-id="133de-314">Update the album entry in the context to mark it as a modified object.</span></span>
    >     2. <span data-ttu-id="133de-315">変更を保存し、インデックス ビューにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="133de-315">Save the changes and redirect to the index view.</span></span>
    > 2. <span data-ttu-id="133de-316">ViewBag が表示されます、モデルが有効でない場合、 **GenreId**と**ArtistId**ユーザーを許可する受信したアルバム オブジェクトでビューが返されますし、任意の必要な更新プログラムを実行します。</span><span class="sxs-lookup"><span data-stu-id="133de-316">If the model is not valid, it will populate the ViewBag with the **GenreId** and **ArtistId**, then it will return the view with the received Album object to allow the user perform any required update.</span></span>

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="133de-317">タスク 7 - アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="133de-317">Task 7 - Running the Application</span></span>

<span data-ttu-id="133de-318">このタスクは、テストを**StoreManager 編集**ビュー ページ実際には、更新されたアルバム データ、データベースに保存します。</span><span class="sxs-lookup"><span data-stu-id="133de-318">In this task, you will test that the **StoreManager Edit** View page actually saves the updated Album data in the database.</span></span>

1. <span data-ttu-id="133de-319">キーを押して**f5 キーを押して**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="133de-319">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="133de-320">ホーム ページで、プロジェクトを開始します。</span><span class="sxs-lookup"><span data-stu-id="133de-320">The project starts in the Home page.</span></span> <span data-ttu-id="133de-321">URL を変更して **/StoreManager/Edit/1**です。</span><span class="sxs-lookup"><span data-stu-id="133de-321">Change the URL to **/StoreManager/Edit/1**.</span></span> <span data-ttu-id="133de-322">アルバムのタイトルを変更**ロード** をクリック**保存**です。</span><span class="sxs-lookup"><span data-stu-id="133de-322">Change the Album title to **Load** and click on **Save**.</span></span> <span data-ttu-id="133de-323">アルバムの一覧で、アルバムのタイトルが実際に変更されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="133de-323">Verify that album's title actually changed in the list of albums.</span></span>

    <span data-ttu-id="133de-324">![アルバムを更新](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "アルバムの更新")</span><span class="sxs-lookup"><span data-stu-id="133de-324">![Updating an album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "Updating an album")</span></span>

    <span data-ttu-id="133de-325">*アルバムの更新*</span><span class="sxs-lookup"><span data-stu-id="133de-325">*Updating an Album*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a><span data-ttu-id="133de-326">手順 4: 追加のビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="133de-326">Exercise 4: Adding a Create View</span></span>

<span data-ttu-id="133de-327">これで、 **StoreManagerController**をサポートしている、**編集**機能は、ここでは、格納できるようにするビューの作成テンプレートを追加する方法を学習するにマネージャーが、アプリケーションに新しいアルバムを追加します。</span><span class="sxs-lookup"><span data-stu-id="133de-327">Now that the **StoreManagerController** supports the **Edit** ability, in this exercise you will learn how to add a Create View template to let store managers add new Albums to the application.</span></span>

<span data-ttu-id="133de-328">内の 2 つの異なるメソッドを使用して作成するシナリオを実装のように編集機能を持つ、 **StoreManagerController**クラス。</span><span class="sxs-lookup"><span data-stu-id="133de-328">Like you did with the Edit functionality, you will implement the Create scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="133de-329">1 つのアクション メソッドは、ストア マネージャーを閲覧すると空のフォームに表示されます、 **StoreManager/作成**URL。</span><span class="sxs-lookup"><span data-stu-id="133de-329">One action method will display an empty form when store managers first visit the **/StoreManager/Create** URL.</span></span>
2. <span data-ttu-id="133de-330">2 番目のアクション メソッドは、シナリオを処理するストア マネージャーがクリックした、**保存**フォーム内でボタンをクリックし、値にバックアップを送信する、 **StoreManager/作成**URL、HTTP POST として。</span><span class="sxs-lookup"><span data-stu-id="133de-330">A second action method will handle the scenario where the store manager clicks the **Save** button within the form and submits the values back to the **/StoreManager/Create** URL as an HTTP-POST.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a><span data-ttu-id="133de-331">タスク 1 - HTTP-GET 作成アクション メソッドの実装</span><span class="sxs-lookup"><span data-stu-id="133de-331">Task 1 - Implementing the HTTP-GET Create action method</span></span>

<span data-ttu-id="133de-332">このタスクでは、HTTP GET 以外のバージョンにこのデータをパッケージ化、すべてのジャンル、アーティストの一覧を取得する作成アクション メソッドを実装します、 **StoreManagerViewModel**オブジェクトで、ビュー テンプレートに渡されます。</span><span class="sxs-lookup"><span data-stu-id="133de-332">In this task, you will implement the HTTP-GET version of the Create action method to retrieve a list of all Genres and Artists, package this data up into a **StoreManagerViewModel** object, which will then be passed to a View template.</span></span>

1. <span data-ttu-id="133de-333">開く、**開始**ソリューションにある**ソース/Ex4-AddingACreateView/開始/** フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="133de-333">Open the **Begin** solution located at **Source/Ex4-AddingACreateView/Begin/** folder.</span></span> <span data-ttu-id="133de-334">それ以外の場合、作業を続行できますを使用して、**終了**ソリューションは、前の手順を完了して取得します。</span><span class="sxs-lookup"><span data-stu-id="133de-334">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="133de-335">指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。</span><span class="sxs-lookup"><span data-stu-id="133de-335">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="133de-336">これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="133de-336">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="133de-337">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。</span><span class="sxs-lookup"><span data-stu-id="133de-337">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="133de-338">最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。</span><span class="sxs-lookup"><span data-stu-id="133de-338">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="133de-339">NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="133de-339">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="133de-340">NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="133de-340">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="133de-341">これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。</span><span class="sxs-lookup"><span data-stu-id="133de-341">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="133de-342">開いている**StoreManagerController**クラスです。</span><span class="sxs-lookup"><span data-stu-id="133de-342">Open **StoreManagerController** class.</span></span> <span data-ttu-id="133de-343">これを行うには、展開、**コント ローラー**フォルダーおよびダブルクリック**StoreManagerController.cs**です。</span><span class="sxs-lookup"><span data-stu-id="133de-343">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="133de-344">置換、**作成**を次のアクション メソッドのコード。</span><span class="sxs-lookup"><span data-stu-id="133de-344">Replace the **Create** action method code with the following:</span></span>

    <span data-ttu-id="133de-345">(コード スニペットの*ASP.NET MVC 4 ヘルパーおよびフォーム検証 - Ex4 StoreManagerController HTTP-GET 作成アクション*)</span><span class="sxs-lookup"><span data-stu-id="133de-345">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP-GET Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a><span data-ttu-id="133de-346">タスク 2 - 作成ビューの追加</span><span class="sxs-lookup"><span data-stu-id="133de-346">Task 2 - Adding the Create View</span></span>

<span data-ttu-id="133de-347">このタスクを新しい (空) のアルバム フォームを表示するビューの作成テンプレートを追加します。</span><span class="sxs-lookup"><span data-stu-id="133de-347">In this task, you will add the Create View template that will display a new (empty) Album form.</span></span>

1. <span data-ttu-id="133de-348">内部を右クリックし、**作成**アクション メソッドを選択**ビューの追加**です。</span><span class="sxs-lookup"><span data-stu-id="133de-348">Right-click inside the **Create** action method and select **Add View**.</span></span> <span data-ttu-id="133de-349">ビューの追加 ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="133de-349">This will bring up the Add View dialog.</span></span>
2. <span data-ttu-id="133de-350">ビューの追加] ダイアログ ボックスで [ビュー名があることを確認します**作成**です。</span><span class="sxs-lookup"><span data-stu-id="133de-350">In the Add View dialog, verify that the View Name is **Create**.</span></span> <span data-ttu-id="133de-351">選択、**厳密に型指定されたビューを作成する**をクリックして**アルバム (MvcMusicStore.Models)** から、**モデル クラス**ドロップダウンおよび**の作成**から、 **Scaffold テンプレート**ドロップダウンします。</span><span class="sxs-lookup"><span data-stu-id="133de-351">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down and **Create** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="133de-352">既定値は、他のフィールドのままにし、をクリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="133de-352">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="133de-353">![作成ビューを追加する](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "の追加-a の作成-view.png")</span><span class="sxs-lookup"><span data-stu-id="133de-353">![Adding a create view](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "adding-a-create-view.png")</span></span>

    <span data-ttu-id="133de-354">*作成ビューの追加*</span><span class="sxs-lookup"><span data-stu-id="133de-354">*Adding the Create View*</span></span>
3. <span data-ttu-id="133de-355">更新プログラム、 **GenreId**と**ArtistId**次に示すように、ドロップダウン リストを使用するフィールド。</span><span class="sxs-lookup"><span data-stu-id="133de-355">Update the **GenreId** and **ArtistId** fields to use a drop-down list as shown below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="133de-356">タスク 3 - アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="133de-356">Task 3 - Running the Application</span></span>

<span data-ttu-id="133de-357">このタスクは、テストを**StoreManager** **作成**ビュー ページには、アルバムの空のフォームが表示されます。</span><span class="sxs-lookup"><span data-stu-id="133de-357">In this task, you will test that the **StoreManager** **Create** View page displays an empty Album form.</span></span>

1. <span data-ttu-id="133de-358">キーを押して**f5 キーを押して**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="133de-358">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="133de-359">ホーム ページで、プロジェクトを開始します。</span><span class="sxs-lookup"><span data-stu-id="133de-359">The project starts in the Home page.</span></span> <span data-ttu-id="133de-360">URL を変更して**StoreManager/作成**です。</span><span class="sxs-lookup"><span data-stu-id="133de-360">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="133de-361">新しいアルバム プロパティを補正するための空のフォームが表示されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="133de-361">Verify that an empty form is displayed for filling the new Album properties.</span></span>

    <span data-ttu-id="133de-362">![空のフォームでビューを作成](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "の空のフォームでのビューの作成")</span><span class="sxs-lookup"><span data-stu-id="133de-362">![Create View with an empty form](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View with an empty form")</span></span>

    <span data-ttu-id="133de-363">*空のフォームでビューを作成します。*</span><span class="sxs-lookup"><span data-stu-id="133de-363">*Create View with an empty form*</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a><span data-ttu-id="133de-364">タスク 4 - HTTP POST を実装するアクション メソッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="133de-364">Task 4 - Implementing the HTTP-POST Create Action Method</span></span>

<span data-ttu-id="133de-365">このタスクでは、HTTP POST のバージョンのユーザーがクリックしたときに呼び出される作成アクション メソッドを実装します、**保存**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="133de-365">In this task, you will implement the HTTP-POST version of the Create action method that will be invoked when a user clicks the **Save** button.</span></span> <span data-ttu-id="133de-366">メソッドは、データベースに新しいアルバムを保存する必要があります。</span><span class="sxs-lookup"><span data-stu-id="133de-366">The method should save the new album in the database.</span></span>

1. <span data-ttu-id="133de-367">Visual Studio のウィンドウに戻るには、必要な場合は、ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="133de-367">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="133de-368">開いている**StoreManagerController**クラスです。</span><span class="sxs-lookup"><span data-stu-id="133de-368">Open **StoreManagerController** class.</span></span> <span data-ttu-id="133de-369">これを行うには、展開、**コント ローラー**フォルダーおよびダブルクリック**StoreManagerController.cs**です。</span><span class="sxs-lookup"><span data-stu-id="133de-369">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="133de-370">置き換える**HTTP POST 作成**を次のアクション メソッドのコード。</span><span class="sxs-lookup"><span data-stu-id="133de-370">Replace **HTTP-POST Create** action method code with the following:</span></span>

    <span data-ttu-id="133de-371">(コード スニペットの*ASP.NET MVC 4 ヘルパーおよびフォーム検証 - Ex4 StoreManagerController HTTP POST 作成アクション*)</span><span class="sxs-lookup"><span data-stu-id="133de-371">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP- POST Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="133de-372">作成アクションは前の編集操作メソッドに非常に似ていますが、オブジェクトを設定するには、変更済みとして代わりに追加されることをコンテキストにします。</span><span class="sxs-lookup"><span data-stu-id="133de-372">The Create action is pretty similar to the previous Edit action method but instead of setting the object as modified, it is being added to the context.</span></span>

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="133de-373">タスク 5 - アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="133de-373">Task 5 - Running the Application</span></span>

<span data-ttu-id="133de-374">このタスクは、テストを**StoreManager 作成**ビュー ページが新しいアルバムを作成することができ、StoreManager インデックス ビューをリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="133de-374">In this task, you will test that the **StoreManager Create** View page lets you create a new Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="133de-375">キーを押して**f5 キーを押して**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="133de-375">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="133de-376">ホーム ページで、プロジェクトを開始します。</span><span class="sxs-lookup"><span data-stu-id="133de-376">The project starts in the Home page.</span></span> <span data-ttu-id="133de-377">URL を変更して**StoreManager/作成**です。</span><span class="sxs-lookup"><span data-stu-id="133de-377">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="133de-378">次の図のように、新しいアルバムのデータにすべてのフォーム フィールドを入力します。</span><span class="sxs-lookup"><span data-stu-id="133de-378">Fill all the form fields with data for a new Album, like the one in the following figure:</span></span>

    <span data-ttu-id="133de-379">![アルバムを作成する](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "アルバムを作成します。")</span><span class="sxs-lookup"><span data-stu-id="133de-379">![Creating an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "Creating an Album")</span></span>

    <span data-ttu-id="133de-380">*アルバムを作成します。*</span><span class="sxs-lookup"><span data-stu-id="133de-380">*Creating an Album*</span></span>
3. <span data-ttu-id="133de-381">StoreManager インデックスを含むビューを作成した新しいアルバムにリダイレクトすることを確認します。</span><span class="sxs-lookup"><span data-stu-id="133de-381">Verify that you get redirected to the StoreManager Index View that includes the new Album just created.</span></span>

    <span data-ttu-id="133de-382">![作成された新しいアルバム](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "新しいアルバムの作成")</span><span class="sxs-lookup"><span data-stu-id="133de-382">![New Album Created](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "New Album Created")</span></span>

    <span data-ttu-id="133de-383">*新しいアルバムの作成*</span><span class="sxs-lookup"><span data-stu-id="133de-383">*New Album Created*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a><span data-ttu-id="133de-384">手順 5: 削除の処理</span><span class="sxs-lookup"><span data-stu-id="133de-384">Exercise 5: Handling Deletion</span></span>

<span data-ttu-id="133de-385">アルバムを削除する機能はまだ実装されていません。</span><span class="sxs-lookup"><span data-stu-id="133de-385">The ability to delete albums is not yet implemented.</span></span> <span data-ttu-id="133de-386">これは、この手順がどうなるのです。</span><span class="sxs-lookup"><span data-stu-id="133de-386">This is what this exercise will be about.</span></span> <span data-ttu-id="133de-387">内の 2 つの異なるメソッドを使用して、削除のシナリオを実装する前と同じように、 **StoreManagerController**クラス。</span><span class="sxs-lookup"><span data-stu-id="133de-387">Like before, you will implement the Delete scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="133de-388">1 つのアクション メソッドが確認フォームに表示されます。</span><span class="sxs-lookup"><span data-stu-id="133de-388">One action method will display a confirmation form</span></span>
2. <span data-ttu-id="133de-389">2 番目のアクション メソッドでは、フォームの送信を処理します。</span><span class="sxs-lookup"><span data-stu-id="133de-389">A second action method will handle the form submission</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a><span data-ttu-id="133de-390">タスク 1 - HTTP GET Delete アクション メソッドの実装</span><span class="sxs-lookup"><span data-stu-id="133de-390">Task 1 - Implementing the HTTP-GET Delete Action Method</span></span>

<span data-ttu-id="133de-391">このタスクでは、HTTP GET バージョンのアルバムの情報を取得する Delete アクション メソッドを実装します。</span><span class="sxs-lookup"><span data-stu-id="133de-391">In this task, you will implement the HTTP-GET version of the Delete action method to retrieve the album's information.</span></span>

1. <span data-ttu-id="133de-392">開く、**開始**ソリューションにある**ソース/Ex5-HandlingDeletion/開始/** フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="133de-392">Open the **Begin** solution located at **Source/Ex5-HandlingDeletion/Begin/** folder.</span></span> <span data-ttu-id="133de-393">それ以外の場合、作業を続行できますを使用して、**終了**ソリューションは、前の手順を完了して取得します。</span><span class="sxs-lookup"><span data-stu-id="133de-393">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="133de-394">指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。</span><span class="sxs-lookup"><span data-stu-id="133de-394">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="133de-395">これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="133de-395">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="133de-396">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。</span><span class="sxs-lookup"><span data-stu-id="133de-396">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="133de-397">最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。</span><span class="sxs-lookup"><span data-stu-id="133de-397">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="133de-398">NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="133de-398">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="133de-399">NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="133de-399">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="133de-400">これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。</span><span class="sxs-lookup"><span data-stu-id="133de-400">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="133de-401">開いている**StoreManagerController**クラスです。</span><span class="sxs-lookup"><span data-stu-id="133de-401">Open **StoreManagerController** class.</span></span> <span data-ttu-id="133de-402">これを行うには、展開、**コント ローラー**フォルダーおよびダブルクリック**StoreManagerController.cs**です。</span><span class="sxs-lookup"><span data-stu-id="133de-402">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="133de-403">Delete コント ローラーのアクションには正確には、前のストアの詳細コント ローラー アクションと同じ: クエリして、**アルバム**オブジェクトを使用して、データベースから、 **id** URL を返しますで提供される、適切な**ビュー**です。</span><span class="sxs-lookup"><span data-stu-id="133de-403">The Delete controller action is exactly the same as the previous Store Details controller action: it queries the **album** object from the database using the **id** provided in the URL and returns the appropriate **View**.</span></span> <span data-ttu-id="133de-404">これを行うには、HTTP GET を置き換える**削除**を次のアクション メソッドのコード。</span><span class="sxs-lookup"><span data-stu-id="133de-404">To do this, replace the HTTP-GET **Delete** action method code with the following:</span></span>

    <span data-ttu-id="133de-405">(コード スニペットの*ASP.NET MVC 4 ヘルパーおよびフォーム検証 - Ex5 処理削除 HTTP-GET 削除アクション*)</span><span class="sxs-lookup"><span data-stu-id="133de-405">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-GET Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. <span data-ttu-id="133de-406">内部を右クリックし、**削除**アクション メソッドを選択**ビューの追加**です。</span><span class="sxs-lookup"><span data-stu-id="133de-406">Right-click inside the **Delete** action method and select **Add View**.</span></span> <span data-ttu-id="133de-407">ビューの追加 ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="133de-407">This will bring up the Add View dialog.</span></span>
5. <span data-ttu-id="133de-408">ビューの追加] ダイアログ ボックスで [ビュー名があることを確認します**削除**です。</span><span class="sxs-lookup"><span data-stu-id="133de-408">In the Add View dialog, verify that the View name is **Delete**.</span></span> <span data-ttu-id="133de-409">選択、**厳密に型指定されたビューを作成する**をクリックして**アルバム (MvcMusicStore.Models)** から、**モデル クラス**ドロップダウンします。</span><span class="sxs-lookup"><span data-stu-id="133de-409">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="133de-410">選択**削除**から、 **Scaffold テンプレート**ドロップダウンします。</span><span class="sxs-lookup"><span data-stu-id="133de-410">Select **Delete** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="133de-411">既定値は、他のフィールドのままにし、をクリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="133de-411">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="133de-412">![削除ビューを追加する](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "削除ビューを追加します。")</span><span class="sxs-lookup"><span data-stu-id="133de-412">![Adding a Delete View](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Adding a Delete View")</span></span>

    <span data-ttu-id="133de-413">*削除ビューを追加します。*</span><span class="sxs-lookup"><span data-stu-id="133de-413">*Adding a Delete View*</span></span>
6. <span data-ttu-id="133de-414">Delete テンプレートは、モデルからのすべてのフィールドを示しています。</span><span class="sxs-lookup"><span data-stu-id="133de-414">The Delete template shows all the fields from the model.</span></span> <span data-ttu-id="133de-415">アルバムのタイトルのみが表示されます。</span><span class="sxs-lookup"><span data-stu-id="133de-415">You will show only the album's title.</span></span> <span data-ttu-id="133de-416">これを行うには、ビューの内容を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="133de-416">To do this, replace the content of the view with the following code:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="133de-417">タスク 2 - アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="133de-417">Task 2 - Running the Application</span></span>

<span data-ttu-id="133de-418">このタスクは、テストを**StoreManager** **削除**ビュー ページには、確認削除フォームが表示されます。</span><span class="sxs-lookup"><span data-stu-id="133de-418">In this task, you will test that the **StoreManager** **Delete** View page displays a confirmation deletion form.</span></span>

1. <span data-ttu-id="133de-419">キーを押して**f5 キーを押して**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="133de-419">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="133de-420">ホーム ページで、プロジェクトを開始します。</span><span class="sxs-lookup"><span data-stu-id="133de-420">The project starts in the Home page.</span></span> <span data-ttu-id="133de-421">URL を変更して **/StoreManager**です。</span><span class="sxs-lookup"><span data-stu-id="133de-421">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="133de-422">クリックして削除する 1 つのアルバムを選択して**削除**新しいビューがアップロードされたことを確認してください。</span><span class="sxs-lookup"><span data-stu-id="133de-422">Select one album to delete by clicking **Delete** and verify that the new view is uploaded.</span></span>

    <span data-ttu-id="133de-423">![アルバムを削除する](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "アルバムを削除します。")</span><span class="sxs-lookup"><span data-stu-id="133de-423">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Deleting an Album")</span></span>

    <span data-ttu-id="133de-424">*アルバムを削除します。*</span><span class="sxs-lookup"><span data-stu-id="133de-424">*Deleting an Album*</span></span>

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a><span data-ttu-id="133de-425">タスク 3-HTTP POST Delete アクション メソッドの実装</span><span class="sxs-lookup"><span data-stu-id="133de-425">Task 3- Implementing the HTTP-POST Delete Action Method</span></span>

<span data-ttu-id="133de-426">このタスクでは、HTTP POST のバージョンのユーザーがクリックしたときに呼び出される Delete アクション メソッドを実装します、**削除**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="133de-426">In this task, you will implement the HTTP-POST version of the Delete action method that will be invoked when a user clicks the **Delete** button.</span></span> <span data-ttu-id="133de-427">メソッドは、データベースのアルバムを削除する必要があります。</span><span class="sxs-lookup"><span data-stu-id="133de-427">The method should delete the album in the database.</span></span>

1. <span data-ttu-id="133de-428">Visual Studio のウィンドウに戻るには、必要な場合は、ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="133de-428">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="133de-429">開いている**StoreManagerController**クラスです。</span><span class="sxs-lookup"><span data-stu-id="133de-429">Open **StoreManagerController** class.</span></span> <span data-ttu-id="133de-430">これを行うには、展開、**コント ローラー**フォルダーおよびダブルクリック**StoreManagerController.cs**です。</span><span class="sxs-lookup"><span data-stu-id="133de-430">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="133de-431">置き換える**HTTP POST 削除**を次のアクション メソッドのコード。</span><span class="sxs-lookup"><span data-stu-id="133de-431">Replace **HTTP-POST Delete** action method code with the following:</span></span>

    <span data-ttu-id="133de-432">(コード スニペットの*ASP.NET MVC 4 ヘルパーおよびフォーム検証 - Ex5 処理削除 HTTP POST 削除アクション*)</span><span class="sxs-lookup"><span data-stu-id="133de-432">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-POST Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="133de-433">タスク 4 - アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="133de-433">Task 4 - Running the Application</span></span>

<span data-ttu-id="133de-434">このタスクは、テストを**StoreManager Delete**ビュー ページがアルバムを削除することができ、StoreManager インデックス ビューをリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="133de-434">In this task, you will test that the **StoreManager Delete** View page lets you delete an Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="133de-435">キーを押して**f5 キーを押して**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="133de-435">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="133de-436">ホーム ページで、プロジェクトを開始します。</span><span class="sxs-lookup"><span data-stu-id="133de-436">The project starts in the Home page.</span></span> <span data-ttu-id="133de-437">URL を変更して **/StoreManager**です。</span><span class="sxs-lookup"><span data-stu-id="133de-437">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="133de-438">クリックして削除する 1 つのアルバムを選択して**を削除します。**</span><span class="sxs-lookup"><span data-stu-id="133de-438">Select one album to delete by clicking **Delete.**</span></span> <span data-ttu-id="133de-439">クリックして、削除の確認**削除**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="133de-439">Confirm the deletion by clicking **Delete** button:</span></span>

    <span data-ttu-id="133de-440">![アルバムを削除する](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "アルバムを削除します。")</span><span class="sxs-lookup"><span data-stu-id="133de-440">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Deleting an Album")</span></span>

    <span data-ttu-id="133de-441">*アルバムを削除します。*</span><span class="sxs-lookup"><span data-stu-id="133de-441">*Deleting an Album*</span></span>
3. <span data-ttu-id="133de-442">表示されないので、アルバムが削除されたことを確認してください、**インデックス**ページ。</span><span class="sxs-lookup"><span data-stu-id="133de-442">Verify that the album was deleted since it does not appear in the **Index** page.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a><span data-ttu-id="133de-443">手順 6: 検証の追加</span><span class="sxs-lookup"><span data-stu-id="133de-443">Exercise 6: Adding Validation</span></span>

<span data-ttu-id="133de-444">現時点では、インプレースが作成および編集フォームには、任意の種類の検証は行いません。</span><span class="sxs-lookup"><span data-stu-id="133de-444">Currently, the Create and Edit forms you have in place do not perform any kind of validation.</span></span> <span data-ttu-id="133de-445">場合は、ユーザーは、[価格] フィールドに必要なフィールドを空白または型の文字が、最初のエラーが表示されますが、データベースからなります。</span><span class="sxs-lookup"><span data-stu-id="133de-445">If the user leaves a required field blank or type letters in the price field, the first error you will get will be from the database.</span></span>

<span data-ttu-id="133de-446">アプリケーションに検証を追加するには、モデル クラスへのデータの注釈を追加します。</span><span class="sxs-lookup"><span data-stu-id="133de-446">You can add validation to the application by adding Data Annotations to your model class.</span></span> <span data-ttu-id="133de-447">データ注釈が、モデルのプロパティに適用するルールを記述するを使用して、ASP.NET MVC くれますを適用し、ユーザーに適切なメッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="133de-447">Data Annotations allow describing the rules you want applied to your model properties, and ASP.NET MVC will take care of enforcing and displaying appropriate message to users.</span></span>

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a><span data-ttu-id="133de-448">タスク 1 - データ注釈を追加します。</span><span class="sxs-lookup"><span data-stu-id="133de-448">Task 1 - Adding Data Annotations</span></span>

<span data-ttu-id="133de-449">このタスクでは、データ注釈を作成および編集するページを構成するアルバム モデルに該当する場合、検証メッセージの表示を追加します。</span><span class="sxs-lookup"><span data-stu-id="133de-449">In this task, you will add Data Annotations to the Album Model that will make the Create and Edit page display validation messages when appropriate.</span></span>

<span data-ttu-id="133de-450">シンプルなモデル クラスのデータ注釈を追加することはだけ処理を追加して、**を使用して**ステートメント**System.ComponentModel.DataAnnotation**、配置、 **[必須]**、適切なプロパティの属性です。</span><span class="sxs-lookup"><span data-stu-id="133de-450">For a simple Model class, adding a Data Annotation is just handled by adding a **using** statement for **System.ComponentModel.DataAnnotation**, then placing a **[Required]** attribute on the appropriate properties.</span></span> <span data-ttu-id="133de-451">次の例のように、**名前**プロパティ ビューに必要なフィールドです。</span><span class="sxs-lookup"><span data-stu-id="133de-451">The following example would make the **Name** property a required field in the View.</span></span>

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

<span data-ttu-id="133de-452">これは、エンティティ データ モデルが生成されるこのアプリケーションのような場合、もう少し複雑です。</span><span class="sxs-lookup"><span data-stu-id="133de-452">This is a little more complex in cases like this application where the Entity Data Model is generated.</span></span> <span data-ttu-id="133de-453">モデルのクラスに直接データ注釈を追加した場合が上書きされますデータベースからモデルを更新する場合。</span><span class="sxs-lookup"><span data-stu-id="133de-453">If you added Data Annotations directly to the model classes, they would be overwritten if you update the model from the database.</span></span> <span data-ttu-id="133de-454">代わりに、行うことができます、注釈を保持するためには存在し、モデルに関連付けられたメタデータ部分クラスを使用してクラスを使用して、 **[MetadataType]** 属性。</span><span class="sxs-lookup"><span data-stu-id="133de-454">Instead, you can make use of metadata partial classes which will exist to hold the annotations and are associated with the model classes using the **[MetadataType]** attribute.</span></span>

1. <span data-ttu-id="133de-455">開く、**開始**ソリューションにある**ソース/Ex6-AddingValidation/開始/** フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="133de-455">Open the **Begin** solution located at **Source/Ex6-AddingValidation/Begin/** folder.</span></span> <span data-ttu-id="133de-456">それ以外の場合、作業を続行できますを使用して、**終了**ソリューションは、前の手順を完了して取得します。</span><span class="sxs-lookup"><span data-stu-id="133de-456">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="133de-457">指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。</span><span class="sxs-lookup"><span data-stu-id="133de-457">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="133de-458">これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="133de-458">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="133de-459">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。</span><span class="sxs-lookup"><span data-stu-id="133de-459">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="133de-460">最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。</span><span class="sxs-lookup"><span data-stu-id="133de-460">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="133de-461">NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="133de-461">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="133de-462">NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="133de-462">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="133de-463">これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。</span><span class="sxs-lookup"><span data-stu-id="133de-463">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="133de-464">開く、 **Album.cs**から、**モデル**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="133de-464">Open the **Album.cs** from the **Models** folder.</span></span>
3. <span data-ttu-id="133de-465">置き換える**Album.cs**コンテンツの強調表示されたコードでは、次のように見えるようにします。</span><span class="sxs-lookup"><span data-stu-id="133de-465">Replace **Album.cs** content with the highlighted code, so that it looks like the following:</span></span>

    > [!NOTE]
    > <span data-ttu-id="133de-466">行 **[DisplayFormat(ConvertEmptyStringToNull=false)]** データ ソースのデータ フィールドが更新されたときに、モデルから空の文字列を null に変換されないことを示します。</span><span class="sxs-lookup"><span data-stu-id="133de-466">The line **[DisplayFormat(ConvertEmptyStringToNull=false)]** indicates that empty strings from the model won't be converted to null when the data field is updated in the data source.</span></span> <span data-ttu-id="133de-467">この設定には、Entity Framework は、データ注釈は、フィールドを検証する前に、モデルに null 値を割り当てを行うときに例外を回避できます。</span><span class="sxs-lookup"><span data-stu-id="133de-467">This setting will avoid an exception when the Entity Framework assigns null values to the model before Data Annotation validates the fields.</span></span>

    <span data-ttu-id="133de-468">(コード スニペットの*ASP.NET MVC 4 ヘルパーおよびフォーム検証 - Ex6 アルバムのメタデータ部分クラス*)</span><span class="sxs-lookup"><span data-stu-id="133de-468">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex6 Album metadata partial class*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > <span data-ttu-id="133de-469">これは、**アルバム**部分クラスには、 **MetadataType**属性を指す、 **AlbumMetaData**データ注釈のクラスです。</span><span class="sxs-lookup"><span data-stu-id="133de-469">This **Album** partial class has a **MetadataType** attribute which points to the **AlbumMetaData** class for the Data Annotations.</span></span> <span data-ttu-id="133de-470">これらは、注釈を付ける、アルバムのモデルを使用しているデータの注釈属性の一部を示します。</span><span class="sxs-lookup"><span data-stu-id="133de-470">These are some of the Data Annotation attributes you are using to annotate the Album model:</span></span>
    > 
    > - <span data-ttu-id="133de-471">必須 - プロパティが必須フィールドであることを示します</span><span class="sxs-lookup"><span data-stu-id="133de-471">Required - Indicates that the property is a required field</span></span>
    > - <span data-ttu-id="133de-472">DisplayName - フォーム フィールドと検証メッセージを使用するテキストを定義します。</span><span class="sxs-lookup"><span data-stu-id="133de-472">DisplayName - Defines the text to be used on form fields and validation messages</span></span>
    > - <span data-ttu-id="133de-473">DisplayFormat - には、データ フィールドの表示方法および書式設定方法を指定します。</span><span class="sxs-lookup"><span data-stu-id="133de-473">DisplayFormat - Specifies how data fields are displayed and formatted.</span></span>
    > - <span data-ttu-id="133de-474">StringLength - 文字列フィールドの最大長を定義します。</span><span class="sxs-lookup"><span data-stu-id="133de-474">StringLength - Defines a maximum length for a string field</span></span>
    > - <span data-ttu-id="133de-475">範囲 - 数値フィールドの最大値と最小値を設定</span><span class="sxs-lookup"><span data-stu-id="133de-475">Range - Gives a maximum and minimum value for a numeric field</span></span>
    > - <span data-ttu-id="133de-476">ScaffoldColumn - により、エディターのフォームのフィールドを非表示にします。</span><span class="sxs-lookup"><span data-stu-id="133de-476">ScaffoldColumn - Allows hiding fields from editor forms</span></span>

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="133de-477">タスク 2 - アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="133de-477">Task 2 - Running the Application</span></span>

<span data-ttu-id="133de-478">このタスクではテストを作成および編集ページが、フィールドを検証する最後のタスクで選択された表示名を使用しています。</span><span class="sxs-lookup"><span data-stu-id="133de-478">In this task, you will test that the Create and Edit pages validate fields, using the display names chosen in the last task.</span></span>

1. <span data-ttu-id="133de-479">キーを押して**f5 キーを押して**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="133de-479">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="133de-480">ホーム ページで、プロジェクトを開始します。</span><span class="sxs-lookup"><span data-stu-id="133de-480">The project starts in the Home page.</span></span> <span data-ttu-id="133de-481">URL を変更して**StoreManager/作成**です。</span><span class="sxs-lookup"><span data-stu-id="133de-481">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="133de-482">表示名が、部分クラスにあるものと一致することを確認 (と同様に**アルバム アート URL**の代わりに**AlbumArtUrl**)</span><span class="sxs-lookup"><span data-stu-id="133de-482">Verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**)</span></span>
3. <span data-ttu-id="133de-483">をクリックして**作成**フォームを入力しなくてもします。</span><span class="sxs-lookup"><span data-stu-id="133de-483">Click **Create**, without filling the form.</span></span> <span data-ttu-id="133de-484">対応する検証メッセージを取得することを確認します。</span><span class="sxs-lookup"><span data-stu-id="133de-484">Verify that you get the corresponding validation messages.</span></span>

    <span data-ttu-id="133de-485">![[作成] ページでフィールドを検証](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "の作成 ページでフィールドの検証")</span><span class="sxs-lookup"><span data-stu-id="133de-485">![Validated fields in the Create page](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Validated fields in the Create page")</span></span>

    <span data-ttu-id="133de-486">*[作成] ページで検証済みのフィールド*</span><span class="sxs-lookup"><span data-stu-id="133de-486">*Validated fields in the Create page*</span></span>
4. <span data-ttu-id="133de-487">同じでも発生することを確認することができます、**編集**ページ。</span><span class="sxs-lookup"><span data-stu-id="133de-487">You can verify that the same occurs with the **Edit** page.</span></span> <span data-ttu-id="133de-488">URL を変更して **/StoreManager/Edit/1**表示名が、部分クラスにあるものと一致することを確認し、(など**アルバム アート URL**の代わりに**AlbumArtUrl**)。</span><span class="sxs-lookup"><span data-stu-id="133de-488">Change the URL to **/StoreManager/Edit/1** and verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**).</span></span> <span data-ttu-id="133de-489">空、**タイトル**と**価格**フィールドとクリック**保存**です。</span><span class="sxs-lookup"><span data-stu-id="133de-489">Empty the **Title** and **Price** fields and click **Save**.</span></span> <span data-ttu-id="133de-490">対応する検証メッセージを取得することを確認します。</span><span class="sxs-lookup"><span data-stu-id="133de-490">Verify that you get the corresponding validation messages.</span></span>

    ![検証済みのフィールドの編集 ページ](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    <span data-ttu-id="133de-492">*検証済みのフィールドの編集 ページ*</span><span class="sxs-lookup"><span data-stu-id="133de-492">*Validated fields in the Edit page*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a><span data-ttu-id="133de-493">手順 7: クライアント側での控えめな jQuery の使用</span><span class="sxs-lookup"><span data-stu-id="133de-493">Exercise 7: Using Unobtrusive jQuery at Client Side</span></span>

<span data-ttu-id="133de-494">この演習では、クライアント側で MVC 4 の控えめな jQuery 検証を有効にする方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="133de-494">In this exercise, you will learn how to enable MVC 4 Unobtrusive jQuery validation at client side.</span></span>

> [!NOTE]
> <span data-ttu-id="133de-495">控えめな jQuery では、データ ajax プレフィックス JavaScript を使用して、インライン クライアント スクリプトをそのままの状態の出力ではなく、サーバー上のアクション メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="133de-495">The Unobtrusive jQuery uses data-ajax prefix JavaScript to invoke action methods on the server rather than intrusively emitting inline client scripts.</span></span>


<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a><span data-ttu-id="133de-496">タスク 1 - 有効にすると控えめな jQuery 前に、アプリケーションを実行しています。</span><span class="sxs-lookup"><span data-stu-id="133de-496">Task 1 - Running the Application before Enabling Unobtrusive jQuery</span></span>

<span data-ttu-id="133de-497">このタスクでは、両方の検証モデルを比較するために jQuery をインクルードする前にアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="133de-497">In this task, you will run the application before including jQuery in order to compare both validation models.</span></span>

1. <span data-ttu-id="133de-498">開く、**開始**ソリューションにある**ソース/Ex7-UnobtrusivejQueryValidation/開始/** フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="133de-498">Open the **Begin** solution located at **Source/Ex7-UnobtrusivejQueryValidation/Begin/** folder.</span></span> <span data-ttu-id="133de-499">それ以外の場合、作業を続行できますを使用して、**終了**ソリューションは、前の手順を完了して取得します。</span><span class="sxs-lookup"><span data-stu-id="133de-499">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="133de-500">指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。</span><span class="sxs-lookup"><span data-stu-id="133de-500">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="133de-501">これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="133de-501">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="133de-502">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。</span><span class="sxs-lookup"><span data-stu-id="133de-502">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="133de-503">最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。</span><span class="sxs-lookup"><span data-stu-id="133de-503">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="133de-504">NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="133de-504">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="133de-505">NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="133de-505">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="133de-506">これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。</span><span class="sxs-lookup"><span data-stu-id="133de-506">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="133de-507">**F5** キーを押してアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="133de-507">Press **F5** to run the application.</span></span>
3. <span data-ttu-id="133de-508">ホーム ページで、プロジェクトを開始します。</span><span class="sxs-lookup"><span data-stu-id="133de-508">The project starts in the Home page.</span></span> <span data-ttu-id="133de-509">参照 **StoreManager/作成** をクリック**作成**検証メッセージを取得することを確認するフォームの入力なし。</span><span class="sxs-lookup"><span data-stu-id="133de-509">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="133de-510">![クライアント検証を無効に](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "クライアント検証を無効になっています")</span><span class="sxs-lookup"><span data-stu-id="133de-510">![Client validation disabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Client validation disabled")</span></span>

    <span data-ttu-id="133de-511">*クライアント検証を無効になっています*</span><span class="sxs-lookup"><span data-stu-id="133de-511">*Client validation disabled*</span></span>
4. <span data-ttu-id="133de-512">ブラウザーで HTML ソース コードを開きます。</span><span class="sxs-lookup"><span data-stu-id="133de-512">In the browser, open the HTML source code:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a><span data-ttu-id="133de-513">タスク 2 - 控えめなクライアント検証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="133de-513">Task 2 - Enabling Unobtrusive Client Validation</span></span>

<span data-ttu-id="133de-514">このタスクでは、jQuery を有効に**控えめなクライアント検証**から**Web.config**ファイル。 これは既定ですべての新しい ASP.NET MVC 4 プロジェクトで false に設定します。</span><span class="sxs-lookup"><span data-stu-id="133de-514">In this task, you will enable jQuery **unobtrusive client validation** from **Web.config** file, which is by default set to false in all new ASP.NET MVC 4 projects.</span></span> <span data-ttu-id="133de-515">さらに、必要なスクリプト jQuery 控えめなクライアント検証作業をするため、参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="133de-515">Additionally, you will add the necessary scripts references to make jQuery Unobtrusive Client Validation work.</span></span>

1. <span data-ttu-id="133de-516">開いている**Web.Config**プロジェクトのルートにファイルを確認して、 **ClientValidationEnabled**と**UnobtrusiveJavaScriptEnabled** にキー値が設定**true**です。</span><span class="sxs-lookup"><span data-stu-id="133de-516">Open **Web.Config** file at project root, and make sure that the **ClientValidationEnabled** and **UnobtrusiveJavaScriptEnabled** keys values are set to **true**.</span></span>

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > <span data-ttu-id="133de-517">また、同じ結果を得るための Global.asax.cs に対してコードでクライアント検証を有効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="133de-517">You can also enable client validation by code at Global.asax.cs to get the same results:</span></span>
    > 
    > <span data-ttu-id="133de-518">**HtmlHelper.ClientValidationEnabled = true**</span><span class="sxs-lookup"><span data-stu-id="133de-518">**HtmlHelper.ClientValidationEnabled = true;**</span></span>
    > 
    > <span data-ttu-id="133de-519">さらに、カスタム動作のコント ローラーに ClientValidationEnabled 属性を割り当てることができます。</span><span class="sxs-lookup"><span data-stu-id="133de-519">Additionally, you can assign ClientValidationEnabled attribute into any controller to have a custom behavior.</span></span>
2. <span data-ttu-id="133de-520">開いている**Create.cshtml**で**Views\StoreManager**です。</span><span class="sxs-lookup"><span data-stu-id="133de-520">Open **Create.cshtml** at **Views\StoreManager**.</span></span>
3. <span data-ttu-id="133de-521">次のスクリプト ファイルを確認してください**jquery.validate**と**jquery.validate.unobtrusive**、ビューを介してで参照される、 &quot; **~/bundles/jqueryval**&quot;バンドルします。</span><span class="sxs-lookup"><span data-stu-id="133de-521">Make sure the following script files, **jquery.validate** and **jquery.validate.unobtrusive**, are referenced in the view throught the &quot;**~/bundles/jqueryval**&quot; bundle.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="133de-522">MVC 4 の新しいプロジェクトでは、これらすべての jQuery ライブラリが含まれています。</span><span class="sxs-lookup"><span data-stu-id="133de-522">All these jQuery libraries are included in MVC 4 new projects.</span></span> <span data-ttu-id="133de-523">複数のライブラリを見つけることができます、 **スクリプト/** するプロジェクトのフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="133de-523">You can find more libraries in the **/Scripts** folder of you project.</span></span>
    > 
    > <span data-ttu-id="133de-524">この検証を行うためには、ライブラリの機能は、jQuery フレームワーク ライブラリへの参照を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="133de-524">In order to make this validation libraries work, you need to add a reference to the jQuery framework library.</span></span> <span data-ttu-id="133de-525">この参照が既に追加されているため、  **\_Layout.cshtml**ファイル、する必要はありませんをこの特定のビューに追加します。</span><span class="sxs-lookup"><span data-stu-id="133de-525">Since this reference is already added in the **\_Layout.cshtml** file, you do not need to add it in this particular view.</span></span>

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a><span data-ttu-id="133de-526">タスク 3 - アプリケーションを使用して控えめな jQuery 検証を実行しています。</span><span class="sxs-lookup"><span data-stu-id="133de-526">Task 3 - Running the Application Using Unobtrusive jQuery Validation</span></span>

<span data-ttu-id="133de-527">このタスクは、テストを**StoreManager**テンプレートは、ユーザーが新しいアルバムを作成するときに jQuery ライブラリを使用してクライアント側の検証を実行するビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="133de-527">In this task, you will test that the **StoreManager** create view template performs client side validation using jQuery libraries when the user creates a new album.</span></span>

1. <span data-ttu-id="133de-528">**F5** キーを押してアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="133de-528">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="133de-529">ホーム ページで、プロジェクトを開始します。</span><span class="sxs-lookup"><span data-stu-id="133de-529">The project starts in the Home page.</span></span> <span data-ttu-id="133de-530">参照 **StoreManager/作成** をクリック**作成**検証メッセージを取得することを確認するフォームの入力なし。</span><span class="sxs-lookup"><span data-stu-id="133de-530">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="133de-531">![クライアント検証を有効になっている jQuery](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "有効になっている jQuery を使用したクライアントの検証")</span><span class="sxs-lookup"><span data-stu-id="133de-531">![Client validation with jQuery enabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Client validation with jQuery enabled")</span></span>

    <span data-ttu-id="133de-532">*有効になっている jQuery を使用したクライアントの検証*</span><span class="sxs-lookup"><span data-stu-id="133de-532">*Client validation with jQuery enabled*</span></span>
3. <span data-ttu-id="133de-533">ブラウザーで、ビューを作成するためのソース コードを開きます。</span><span class="sxs-lookup"><span data-stu-id="133de-533">In the browser, open the source code for Create view:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

   > [!NOTE]
   > <span data-ttu-id="133de-534">控えめな jQuery がデータを使用して属性を追加する各クライアント検証規則の-val -*rulename*=&quot;*メッセージ*&quot;です。</span><span class="sxs-lookup"><span data-stu-id="133de-534">For each client validation rule, Unobtrusive jQuery adds an attribute with data-val-*rulename*=&quot;*message*&quot;.</span></span> <span data-ttu-id="133de-535">その Unobtrusive タグの一覧を以下は、jQuery クライアント検証を実行するには、html 入力フィールドに挿入します。</span><span class="sxs-lookup"><span data-stu-id="133de-535">Below is a list of tags that Unobtrusive jQuery inserts into the html input field to perform client validation:</span></span>
   > 
   > - <span data-ttu-id="133de-536">データ val</span><span class="sxs-lookup"><span data-stu-id="133de-536">Data-val</span></span>
   > - <span data-ttu-id="133de-537">データ val 番号</span><span class="sxs-lookup"><span data-stu-id="133de-537">Data-val-number</span></span>
   > - <span data-ttu-id="133de-538">データ val 範囲</span><span class="sxs-lookup"><span data-stu-id="133de-538">Data-val-range</span></span>
   > - <span data-ttu-id="133de-539">データ val 範囲 min/データ val range max</span><span class="sxs-lookup"><span data-stu-id="133de-539">Data-val-range-min / Data-val-range-max</span></span>
   > - <span data-ttu-id="133de-540">データ val 必要</span><span class="sxs-lookup"><span data-stu-id="133de-540">Data-val-required</span></span>
   > - <span data-ttu-id="133de-541">Val 長さがデータに</span><span class="sxs-lookup"><span data-stu-id="133de-541">Data-val-length</span></span>
   > - <span data-ttu-id="133de-542">データ val 長さ最大/データ val 長さ分</span><span class="sxs-lookup"><span data-stu-id="133de-542">Data-val-length-max / Data-val-length-min</span></span>
   > 
   > <span data-ttu-id="133de-543">モデルはすべてのデータ値が格納**データ注釈**です。</span><span class="sxs-lookup"><span data-stu-id="133de-543">All the data values are filled with model **Data Annotation**.</span></span> <span data-ttu-id="133de-544">次に、サーバー側で動作するすべてのロジックは、クライアント側で実行できます。</span><span class="sxs-lookup"><span data-stu-id="133de-544">Then, all the logic that works at server side can be run at client side.</span></span> <span data-ttu-id="133de-545">たとえば、Price 属性に次の注釈をデータ モデル。</span><span class="sxs-lookup"><span data-stu-id="133de-545">For example, Price attribute has the following data annotation in the model:</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
   > 
   > <span data-ttu-id="133de-546">控えめな jQuery を使用すると、生成されたコードです。</span><span class="sxs-lookup"><span data-stu-id="133de-546">After using Unobtrusive jQuery, the generated code is:</span></span>
   > 
   > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="133de-547">まとめ</span><span class="sxs-lookup"><span data-stu-id="133de-547">Summary</span></span>

<span data-ttu-id="133de-548">このハンズオン ラボを完了して、次のデータベースに格納されているデータを変更するユーザーを有効にする方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="133de-548">By completing this Hands-On Lab you have learned how to enable users to change the data stored in the database with the use of the following:</span></span>

- <span data-ttu-id="133de-549">コント ローラーのアクションなどのインデックスを作成、編集、削除</span><span class="sxs-lookup"><span data-stu-id="133de-549">Controller actions like Index, Create, Edit, Delete</span></span>
- <span data-ttu-id="133de-550">HTML テーブルのプロパティを表示するための ASP.NET MVC のスキャフォールディング機能</span><span class="sxs-lookup"><span data-stu-id="133de-550">ASP.NET MVC's scaffolding feature for displaying properties in an HTML table</span></span>
- <span data-ttu-id="133de-551">ユーザーを向上させるためにカスタムの HTML ヘルパーが発生します。</span><span class="sxs-lookup"><span data-stu-id="133de-551">Custom HTML helpers to improve user experience</span></span>
- <span data-ttu-id="133de-552">HTTP GET または HTTP POST 呼び出しのいずれかに対応するアクション メソッド</span><span class="sxs-lookup"><span data-stu-id="133de-552">Action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="133de-553">同様のビュー テンプレートの作成や編集などの共有エディター テンプレート</span><span class="sxs-lookup"><span data-stu-id="133de-553">A shared editor template for similar View templates like Create and Edit</span></span>
- <span data-ttu-id="133de-554">ドロップダウン リストなどの form 要素</span><span class="sxs-lookup"><span data-stu-id="133de-554">Form elements like drop-downs</span></span>
- <span data-ttu-id="133de-555">モデル検証のためデータ注釈</span><span class="sxs-lookup"><span data-stu-id="133de-555">Data annotations for Model validation</span></span>
- <span data-ttu-id="133de-556">JQuery 控えめなライブラリを使用してクライアント側の検証</span><span class="sxs-lookup"><span data-stu-id="133de-556">Client Side Validation using jQuery Unobtrusive library</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="133de-557">付録 a: をインストールする Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="133de-557">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="133de-558">インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、 **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="133de-558">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="133de-559">次の手順を案内するインストールに必要な手順*Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*です。</span><span class="sxs-lookup"><span data-stu-id="133de-559">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="133de-560">移動して[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)です。</span><span class="sxs-lookup"><span data-stu-id="133de-560">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="133de-561">または、既に Web Platform Installer をインストールした場合、開くことができ、製品を検索&quot; <em>Visual Studio Express 2012 for Web と Windows Azure SDK</em>&quot;です。</span><span class="sxs-lookup"><span data-stu-id="133de-561">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="133de-562">をクリックして**を今すぐインストール**です。</span><span class="sxs-lookup"><span data-stu-id="133de-562">Click on **Install Now**.</span></span> <span data-ttu-id="133de-563">ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="133de-563">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="133de-564">1 回**Web Platform Installer**が開いて、をクリックして**インストール**セットアップを開始します。</span><span class="sxs-lookup"><span data-stu-id="133de-564">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="133de-565">![Visual Studio Express インストール](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Visual Studio Express のインストール")</span><span class="sxs-lookup"><span data-stu-id="133de-565">![Install Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="133de-566">*Visual Studio Express をインストールします。*</span><span class="sxs-lookup"><span data-stu-id="133de-566">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="133de-567">すべての製品のライセンスと条項を読み、クリックして**同意**を続行します。</span><span class="sxs-lookup"><span data-stu-id="133de-567">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![ライセンス条項に同意](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    <span data-ttu-id="133de-569">*ライセンス条項に同意*</span><span class="sxs-lookup"><span data-stu-id="133de-569">*Accepting the license terms*</span></span>
5. <span data-ttu-id="133de-570">ダウンロードとインストール プロセスが完了するまで待機します。</span><span class="sxs-lookup"><span data-stu-id="133de-570">Wait until the downloading and installation process completes.</span></span>

    ![インストールの進行状況](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    <span data-ttu-id="133de-572">*インストールの進行状況*</span><span class="sxs-lookup"><span data-stu-id="133de-572">*Installation progress*</span></span>
6. <span data-ttu-id="133de-573">インストールが完了したらをクリックして**完了**です。</span><span class="sxs-lookup"><span data-stu-id="133de-573">When the installation completes, click **Finish**.</span></span>

    ![インストールが完了しました](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    <span data-ttu-id="133de-575">*インストールが完了しました*</span><span class="sxs-lookup"><span data-stu-id="133de-575">*Installation completed*</span></span>
7. <span data-ttu-id="133de-576">をクリックして**終了**Web Platform Installer を閉じます。</span><span class="sxs-lookup"><span data-stu-id="133de-576">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="133de-577">Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、順にクリックして、 **VS Express for Web**並べて表示します。</span><span class="sxs-lookup"><span data-stu-id="133de-577">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express Web タイルを](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    <span data-ttu-id="133de-579">*VS Express Web タイルを*</span><span class="sxs-lookup"><span data-stu-id="133de-579">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="133de-580">付録 b: コード スニペットの使用</span><span class="sxs-lookup"><span data-stu-id="133de-580">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="133de-581">コード スニペットでは、すぐに利用できる必要があるすべてのコードがあります。</span><span class="sxs-lookup"><span data-stu-id="133de-581">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="133de-582">ラボ ドキュメントがわかります正確に使用する場合が、次の図に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="133de-582">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="133de-583">![Visual Studio のコード スニペットを使用して、プロジェクトにコードを挿入する](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "プロジェクトにコードを挿入する Visual Studio を使ってコード スニペット")</span><span class="sxs-lookup"><span data-stu-id="133de-583">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="133de-584">*Visual Studio のコード スニペットを使用して、プロジェクトにコードを挿入するには*</span><span class="sxs-lookup"><span data-stu-id="133de-584">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="133de-585">***キーボード (C# の場合のみ) を使用してコード スニペットを追加するには***</span><span class="sxs-lookup"><span data-stu-id="133de-585">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="133de-586">コードを挿入するには、カーソルを置きます。</span><span class="sxs-lookup"><span data-stu-id="133de-586">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="133de-587">(なし、スペースやハイフン) スニペット名を入力してを起動します。</span><span class="sxs-lookup"><span data-stu-id="133de-587">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="133de-588">スニペットの名前に一致する IntelliSense 表示を確認します。</span><span class="sxs-lookup"><span data-stu-id="133de-588">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="133de-589">正しいスニペットを選択 (または、全体のスニペットの名前を選択するまで」と入力してください)。</span><span class="sxs-lookup"><span data-stu-id="133de-589">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="133de-590">カーソル位置にスニペットを挿入するには、2 回、Tab キーを押します。</span><span class="sxs-lookup"><span data-stu-id="133de-590">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="133de-591">![スニペット名を入力する開始](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "スニペット名の入力を開始")</span><span class="sxs-lookup"><span data-stu-id="133de-591">![Start typing the snippet name](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="133de-592">*スニペット名の入力を開始します。*</span><span class="sxs-lookup"><span data-stu-id="133de-592">*Start typing the snippet name*</span></span>

<span data-ttu-id="133de-593">![Tab キーを押して、強調表示されているスニペットを選択する](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "強調表示されているスニペットを選択するキーを押してタブ")</span><span class="sxs-lookup"><span data-stu-id="133de-593">![Press Tab to select the highlighted snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="133de-594">*Tab キーを押して、強調表示されているスニペットを選択するには*</span><span class="sxs-lookup"><span data-stu-id="133de-594">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="133de-595">![キーを押して タブで再度と、スニペットが展開](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "キーを押して タブで再度と、スニペットが展開されます")</span><span class="sxs-lookup"><span data-stu-id="133de-595">![Press Tab again and the snippet will expand](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="133de-596">*キーを押して タブで再度と、スニペットが展開されます。*</span><span class="sxs-lookup"><span data-stu-id="133de-596">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="133de-597">***(C#、Visual Basic および XML) にマウスを使用してコード スニペットを追加する***1 です。</span><span class="sxs-lookup"><span data-stu-id="133de-597">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="133de-598">コード スニペットを挿入する場所を右クリックします。</span><span class="sxs-lookup"><span data-stu-id="133de-598">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="133de-599">選択**スニペットの挿入**続く**マイ コード スニペット**です。</span><span class="sxs-lookup"><span data-stu-id="133de-599">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="133de-600">クリックして一覧から、関連するスニペットを選択します。</span><span class="sxs-lookup"><span data-stu-id="133de-600">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="133de-601">![コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリックして](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリック")</span><span class="sxs-lookup"><span data-stu-id="133de-601">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="133de-602">*コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリックします。*</span><span class="sxs-lookup"><span data-stu-id="133de-602">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="133de-603">![クリックして一覧から、関連するスニペットを選択](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "クリックして一覧から、関連するスニペットを選択")</span><span class="sxs-lookup"><span data-stu-id="133de-603">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="133de-604">*クリックして一覧から、関連するスニペットを選択します。*</span><span class="sxs-lookup"><span data-stu-id="133de-604">*Pick the relevant snippet from the list, by clicking on it*</span></span>
