---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: ASP.NET MVC 4 ヘルパー、フォーム、および検証 |Microsoft Docs
author: rick-anderson
description: ASP.NET MVC 4 モデルとデータ アクセスのハンズオン ラボでされて読み込みし、データベースからデータを表示します。 このハンズオン ラボに追加します.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: f2eb624e72d6f52d1694b5753ee2b1f8117c2851
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376738"
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a><span data-ttu-id="970a0-104">ASP.NET MVC 4 ヘルパー、フォーム、および検証</span><span class="sxs-lookup"><span data-stu-id="970a0-104">ASP.NET MVC 4 Helpers, Forms and Validation</span></span>

<span data-ttu-id="970a0-105">によって[Web キャンプ チーム](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="970a0-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="970a0-106">Web のキャンプ トレーニング キットをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="970a0-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="970a0-107">**ASP.NET MVC 4 モデルとデータ アクセス**ハンズオン ラボでは、読み込みと、データベースからデータを表示するをされています。</span><span class="sxs-lookup"><span data-stu-id="970a0-107">In **ASP.NET MVC 4 Models and Data Access** Hands-on Lab, you have been loading and displaying data from the database.</span></span> <span data-ttu-id="970a0-108">このハンズオン ラボを追加、**ミュージック ストア**アプリケーション、そのデータを編集する機能。</span><span class="sxs-lookup"><span data-stu-id="970a0-108">In this Hands-on Lab, you will add to the **Music Store** application the ability to edit that data.</span></span>

<span data-ttu-id="970a0-109">その目的を念頭には、使用には、アルバムの作成、読み取り、更新、削除 (CRUD) 操作をサポートするコント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="970a0-109">With that goal in mind, you will first create the controller that will support the Create, Read, Update and Delete (CRUD) actions of albums.</span></span> <span data-ttu-id="970a0-110">HTML テーブルのアルバムのプロパティを表示する ASP.NET MVC のスキャフォールディング機能の活用、インデックス ビュー テンプレートが生成されます。</span><span class="sxs-lookup"><span data-stu-id="970a0-110">You will generate an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span> <span data-ttu-id="970a0-111">そのビューを強化するためには、長い説明が切り捨てられますが、カスタム HTML ヘルパーを追加します。</span><span class="sxs-lookup"><span data-stu-id="970a0-111">To enhance that view, you will add a custom HTML helper that will truncate long descriptions.</span></span>

<span data-ttu-id="970a0-112">その後、フォームのドロップダウン リストの要素のヘルプで、データベースのアルバムを変更できるようにするビューの作成と編集を追加します。</span><span class="sxs-lookup"><span data-stu-id="970a0-112">Afterwards, you will add the Edit and Create Views that will let you alter the albums in the database, with the help of form elements like dropdowns.</span></span>

<span data-ttu-id="970a0-113">最後に、アルバムを削除するユーザーができるようにし、もすることができなくなりますが入力を検証することで無効なデータを入力します。</span><span class="sxs-lookup"><span data-stu-id="970a0-113">Lastly, you will let users delete an album and also you will prevent them from entering wrong data by validating their input.</span></span>

<span data-ttu-id="970a0-114">このハンズオン ラボは、の基本的な知識があることを前提**ASP.NET MVC**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-114">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="970a0-115">使用していない場合**ASP.NET MVC**前を経由することを勧めします。 **ASP.NET MVC の基礎**ハンズオン ラボ。</span><span class="sxs-lookup"><span data-stu-id="970a0-115">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="970a0-116">このラボでは、拡張機能とソース フォルダーにサンプル Web アプリケーションに軽微な変更を適用することで以前に説明する新機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="970a0-116">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="970a0-117">すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[Microsoft の Web/WebCampTrainingKit リリース](https://aka.ms/webcamps-training-kit)します。</span><span class="sxs-lookup"><span data-stu-id="970a0-117">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="970a0-118">このラボに固有のプロジェクトは、「 [ASP.NET MVC 4 ヘルパー、フォーム、検証](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation)です。</span><span class="sxs-lookup"><span data-stu-id="970a0-118">The project specific to this lab is available at [ASP.NET MVC 4 Helpers, Forms and Validation](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="970a0-119">目的</span><span class="sxs-lookup"><span data-stu-id="970a0-119">Objectives</span></span>

<span data-ttu-id="970a0-120">このハンズオン ラボでは、学習する方法。</span><span class="sxs-lookup"><span data-stu-id="970a0-120">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="970a0-121">CRUD 操作をサポートするためにコント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="970a0-121">Create a controller to support CRUD operations</span></span>
- <span data-ttu-id="970a0-122">HTML テーブルにエンティティのプロパティを表示する、インデックス ビューの生成します。</span><span class="sxs-lookup"><span data-stu-id="970a0-122">Generate an Index View to display entity properties in an HTML table</span></span>
- <span data-ttu-id="970a0-123">カスタム HTML ヘルパーを追加します。</span><span class="sxs-lookup"><span data-stu-id="970a0-123">Add a custom HTML helper</span></span>
- <span data-ttu-id="970a0-124">作成し、編集ビューをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="970a0-124">Create and customize an Edit View</span></span>
- <span data-ttu-id="970a0-125">HTTP GET または HTTP POST 呼び出しのいずれかに対応するアクション メソッドの区別します。</span><span class="sxs-lookup"><span data-stu-id="970a0-125">Differentiate between action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="970a0-126">追加し、ビューの作成のカスタマイズ</span><span class="sxs-lookup"><span data-stu-id="970a0-126">Add and customize a Create View</span></span>
- <span data-ttu-id="970a0-127">エンティティの削除を処理します。</span><span class="sxs-lookup"><span data-stu-id="970a0-127">Handle the deletion of an entity</span></span>
- <span data-ttu-id="970a0-128">ユーザー入力を検証します。</span><span class="sxs-lookup"><span data-stu-id="970a0-128">Validate user input</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="970a0-129">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="970a0-129">Prerequisites</span></span>

<span data-ttu-id="970a0-130">このラボを完成させるのには、次の項目が必要です。</span><span class="sxs-lookup"><span data-stu-id="970a0-130">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="970a0-131">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)または上位 (読み取り[付録 A](#AppendixA)をインストールする方法について)。</span><span class="sxs-lookup"><span data-stu-id="970a0-131">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="970a0-132">セットアップ</span><span class="sxs-lookup"><span data-stu-id="970a0-132">Setup</span></span>

<span data-ttu-id="970a0-133">**コード スニペットをインストールします。**</span><span class="sxs-lookup"><span data-stu-id="970a0-133">**Installing Code Snippets**</span></span>

<span data-ttu-id="970a0-134">便宜上、このラボに管理するコードの多くは、Visual Studio コード スニペットとして利用できます。</span><span class="sxs-lookup"><span data-stu-id="970a0-134">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="970a0-135">実行コード スニペットをインストールする **.\Source\Setup\CodeSnippets.vsi**ファイル。</span><span class="sxs-lookup"><span data-stu-id="970a0-135">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="970a0-136">このドキュメントの付録を参照することができます、Visual Studio のコード スニペットとその使用方法を学習するに慣れていない場合&quot;[付録 b: を使用してコード スニペット](#AppendixB)&quot;します。</span><span class="sxs-lookup"><span data-stu-id="970a0-136">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="970a0-137">演習</span><span class="sxs-lookup"><span data-stu-id="970a0-137">Exercises</span></span>

<span data-ttu-id="970a0-138">次の演習では、このハンズオン ラボを構成します。</span><span class="sxs-lookup"><span data-stu-id="970a0-138">The following exercises make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="970a0-139">ストア マネージャー コント ローラーとそのインデックス ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="970a0-139">Creating the Store Manager controller and its Index view</span></span>](#Exercise1)
2. [<span data-ttu-id="970a0-140">HTML ヘルパーを追加します。</span><span class="sxs-lookup"><span data-stu-id="970a0-140">Adding an HTML Helper</span></span>](#Exercise2)
3. [<span data-ttu-id="970a0-141">編集ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="970a0-141">Creating the Edit View</span></span>](#Exercise3)
4. [<span data-ttu-id="970a0-142">作成ビューの追加</span><span class="sxs-lookup"><span data-stu-id="970a0-142">Adding a Create View</span></span>](#Exercise4)
5. [<span data-ttu-id="970a0-143">削除の処理</span><span class="sxs-lookup"><span data-stu-id="970a0-143">Handling Deletion</span></span>](#Exercise5)
6. [<span data-ttu-id="970a0-144">検証の追加</span><span class="sxs-lookup"><span data-stu-id="970a0-144">Adding Validation</span></span>](#Exercise6)
7. [<span data-ttu-id="970a0-145">クライアント側での控えめな jQuery の使用</span><span class="sxs-lookup"><span data-stu-id="970a0-145">Using Unobtrusive jQuery at Client Side</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="970a0-146">各演習が用意されており、**エンド**演習を完了した後に取得する必要があります、結果として得られるソリューションに含まれているフォルダー。</span><span class="sxs-lookup"><span data-stu-id="970a0-146">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="970a0-147">作業、演習を通じて追加のヘルプが必要な場合は、このソリューションをガイドとして使用できます。</span><span class="sxs-lookup"><span data-stu-id="970a0-147">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="970a0-148">この演習の所要時間を推定: **60 分**</span><span class="sxs-lookup"><span data-stu-id="970a0-148">Estimated time to complete this lab: **60 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a><span data-ttu-id="970a0-149">手順 1: ストア マネージャー コント ローラーとそのインデックス ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="970a0-149">Exercise 1: Creating the Store Manager controller and its Index view</span></span>

<span data-ttu-id="970a0-150">この演習では、CRUD 操作をサポートするには、データベースおよび最後に、インデックス ビュー テンプレートを活用 ASP.NET MVC のスキャフォールディングを生成からアルバムのリストを返すため、インデックス アクション メソッドをカスタマイズする新しいコント ローラーを作成する方法について説明しますHTML テーブルのアルバムのプロパティを表示する機能。</span><span class="sxs-lookup"><span data-stu-id="970a0-150">In this exercise, you will learn how to create a new controller to support CRUD operations, customize its Index action method to return a list of albums from the database and finally generating an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a><span data-ttu-id="970a0-151">タスク 1 -、StoreManagerController の作成</span><span class="sxs-lookup"><span data-stu-id="970a0-151">Task 1 - Creating the StoreManagerController</span></span>

<span data-ttu-id="970a0-152">このタスクと呼ばれる新しいコント ローラーを作成する**StoreManagerController** CRUD 操作をサポートします。</span><span class="sxs-lookup"><span data-stu-id="970a0-152">In this task, you will create a new controller called **StoreManagerController** to support CRUD operations.</span></span>

1. <span data-ttu-id="970a0-153">開く、**開始**ソリューションがある**ソース/Ex1-CreatingTheStoreManagerController/開始/** フォルダー。</span><span class="sxs-lookup"><span data-stu-id="970a0-153">Open the **Begin** solution located at **Source/Ex1-CreatingTheStoreManagerController/Begin/** folder.</span></span>

   1. <span data-ttu-id="970a0-154">いくつか不足している NuGet パッケージをダウンロードする必要がありますが続行する前にします。</span><span class="sxs-lookup"><span data-stu-id="970a0-154">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="970a0-155">これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-155">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="970a0-156">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。</span><span class="sxs-lookup"><span data-stu-id="970a0-156">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="970a0-157">最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-157">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="970a0-158">NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="970a0-158">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="970a0-159">With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="970a0-159">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="970a0-160">これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="970a0-160">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="970a0-161">新しいコント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="970a0-161">Add a new controller.</span></span> <span data-ttu-id="970a0-162">これを行うを右クリックし、**コント ローラー**フォルダー内、ソリューション エクスプ ローラー、選択**追加**をクリックし、**コント ローラー**コマンド。</span><span class="sxs-lookup"><span data-stu-id="970a0-162">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="970a0-163">変更、**コント ローラー** **名前**に**StoreManagerController**オプションを確認して**の空の読み取り/書き込みアクションがあるMVCコントローラー**が選択されています。</span><span class="sxs-lookup"><span data-stu-id="970a0-163">Change the **Controller** **Name** to **StoreManagerController** and make sure the option **MVC controller with empty read/write actions** is selected.</span></span> <span data-ttu-id="970a0-164">**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="970a0-164">Click **Add**.</span></span>

    <span data-ttu-id="970a0-165">![コント ローラーの追加ダイアログ](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "コント ローラーの追加ダイアログ")</span><span class="sxs-lookup"><span data-stu-id="970a0-165">![Add controller dialog](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Add controller dialog")</span></span>

    <span data-ttu-id="970a0-166">*[追加] ダイアログのコント ローラー*</span><span class="sxs-lookup"><span data-stu-id="970a0-166">*Add Controller Dialog*</span></span>

    <span data-ttu-id="970a0-167">新しいコント ローラー クラスが生成されます。</span><span class="sxs-lookup"><span data-stu-id="970a0-167">A new Controller class is generated.</span></span> <span data-ttu-id="970a0-168">読み取り/書き込み、それらをスタブ メソッドのアクションを追加する指定したため、一般的な CRUD 操作は、アプリケーション固有のロジックを含めるに確認入力されている、TODO コメントで作成されます。</span><span class="sxs-lookup"><span data-stu-id="970a0-168">Since you indicated to add actions for read/write, stub methods for those, common CRUD actions are created with TODO comments filled in, prompting to include the application specific logic.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a><span data-ttu-id="970a0-169">タスク 2 - StoreManager インデックスをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="970a0-169">Task 2 - Customizing the StoreManager Index</span></span>

<span data-ttu-id="970a0-170">このタスクをデータベースからアルバムの一覧では、ビューを返す StoreManager インデックス アクション メソッドをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="970a0-170">In this task, you will customize the StoreManager Index action method to return a View with the list of albums from the database.</span></span>

1. <span data-ttu-id="970a0-171">次のコードを追加、StoreManagerController クラスで*を使用して*ディレクティブ。</span><span class="sxs-lookup"><span data-stu-id="970a0-171">In the StoreManagerController class, add the following *using* directives.</span></span>

    <span data-ttu-id="970a0-172">(コード スニペット -*と ASP.NET MVC 4 ヘルパー、フォーム、および検証 - Ex1 MvcMusicStore を使用して*)</span><span class="sxs-lookup"><span data-stu-id="970a0-172">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 using MvcMusicStore*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. <span data-ttu-id="970a0-173">フィールドを追加、 **StoreManagerController**のインスタンスを保持する**MusicStoreEntities します。**</span><span class="sxs-lookup"><span data-stu-id="970a0-173">Add a field to the **StoreManagerController** to hold an instance of **MusicStoreEntities.**</span></span>

    <span data-ttu-id="970a0-174">(コード スニペット -*と ASP.NET MVC 4 ヘルパー、フォーム、および検証 - Ex1 MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="970a0-174">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. <span data-ttu-id="970a0-175">アルバムのリストでビューを返す StoreManagerController Index アクションを実装します。</span><span class="sxs-lookup"><span data-stu-id="970a0-175">Implement the StoreManagerController Index action to return a View with the list of albums.</span></span>

    <span data-ttu-id="970a0-176">コント ローラーのアクション ロジックは、StoreController の Index アクションの前に記述された非常にようになります。</span><span class="sxs-lookup"><span data-stu-id="970a0-176">The Controller action logic will be very similar to the StoreController's Index action written earlier.</span></span> <span data-ttu-id="970a0-177">LINQ を使用すると、すべてのアルバム、ジャンル、アーティストの情報の表示などを取得できます。</span><span class="sxs-lookup"><span data-stu-id="970a0-177">Use LINQ to retrieve all albums, including Genre and Artist information for display.</span></span>

    <span data-ttu-id="970a0-178">(コード スニペット -*と ASP.NET MVC 4 ヘルパー、フォーム、および検証 - Ex1 StoreManagerController インデックス*)</span><span class="sxs-lookup"><span data-stu-id="970a0-178">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 StoreManagerController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a><span data-ttu-id="970a0-179">タスク 3 - インデックス ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="970a0-179">Task 3 - Creating the Index View</span></span>

<span data-ttu-id="970a0-180">によって返されるアルバムの一覧を表示する Index ビュー テンプレートを作成するこのタスクで、 **StoreManager**コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="970a0-180">In this task, you will create the Index View template to display the list of albums returned by the **StoreManager** Controller.</span></span>

1. <span data-ttu-id="970a0-181">新しいテンプレートの表示を作成する前にプロジェクトをビルドする必要がありますように、**ビューの追加 ダイアログ**認識、**アルバム**クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="970a0-181">Before creating the new View template, you should build the project so that the **Add View Dialog** knows about the **Album** class to use.</span></span> <span data-ttu-id="970a0-182">選択**ビルド |ビルド MvcMusicStore**プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="970a0-182">Select **Build | Build MvcMusicStore** to build the project.</span></span>
2. <span data-ttu-id="970a0-183">内側を右クリックし、**インデックス**アクション メソッドと select**ビューの追加**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-183">Right-click inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="970a0-184">これが表示されます、**ビューの追加**ダイアログ。</span><span class="sxs-lookup"><span data-stu-id="970a0-184">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="970a0-185">![ビューの追加](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "ビューの追加")</span><span class="sxs-lookup"><span data-stu-id="970a0-185">![Add view](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Add view")</span></span>

    <span data-ttu-id="970a0-186">*Index メソッド内からビューを追加します。*</span><span class="sxs-lookup"><span data-stu-id="970a0-186">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="970a0-187">ビューの追加 ダイアログ ボックスで、ビュー名があることを確認します**インデックス**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-187">In the Add View dialog, verify that the View Name is **Index**.</span></span> <span data-ttu-id="970a0-188">選択、**厳密に型指定されたビューを作成する**オプション、および選択**アルバム (MvcMusicStore.Models)** から、**モデル クラス**ドロップダウンします。</span><span class="sxs-lookup"><span data-stu-id="970a0-188">Select the **Create a strongly-typed view** option, and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="970a0-189">選択**一覧**から、**スキャフォールディング テンプレート**ドロップダウンします。</span><span class="sxs-lookup"><span data-stu-id="970a0-189">Select **List** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="970a0-190">ままに、**ビュー エンジン**に**Razor** 、既定値は、その他のフィールドの値をクリックして**追加**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-190">Leave the **View engine** to **Razor** and the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="970a0-191">![インデックス、ビューの追加](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "インデックス ビューの追加")</span><span class="sxs-lookup"><span data-stu-id="970a0-191">![Adding an index view](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Adding an index view")</span></span>

    <span data-ttu-id="970a0-192">*インデックス、ビューの追加*</span><span class="sxs-lookup"><span data-stu-id="970a0-192">*Adding an Index View*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a><span data-ttu-id="970a0-193">タスク 4 - Index ビューのスキャフォールディングをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="970a0-193">Task 4 - Customizing the scaffold of the Index View</span></span>

<span data-ttu-id="970a0-194">このタスクでは、ASP.NET MVC のスキャフォールディング機能を表示するフィールドで作成された単純なビュー テンプレートを調整します。</span><span class="sxs-lookup"><span data-stu-id="970a0-194">In this task, you will adjust the simple View template created with ASP.NET MVC scaffolding feature to have it display the fields you want.</span></span>

> [!NOTE]
> <span data-ttu-id="970a0-195">**スキャフォールディング**ASP.NET MVC でのサポートは、アルバムのモデルのすべてのフィールドの一覧を表示する単純なビュー テンプレートを生成します。</span><span class="sxs-lookup"><span data-stu-id="970a0-195">The **scaffolding** support within ASP.NET MVC generates a simple View template which lists all fields in the Album model.</span></span> <span data-ttu-id="970a0-196">**スキャフォールディング**厳密に型指定されたビューで開始する簡単な方法を提供します。 ビュー テンプレートを手動で記述するのではなくすばやくスキャフォールディング既定のテンプレートを生成し、し、生成されたコードを変更することができます。</span><span class="sxs-lookup"><span data-stu-id="970a0-196">**Scaffolding** provides a quick way to get started on a strongly typed view: rather than having to write the View template manually, scaffolding quickly generates a default template and then you can modify the generated code.</span></span>


1. <span data-ttu-id="970a0-197">作成したコードを確認します。</span><span class="sxs-lookup"><span data-stu-id="970a0-197">Review the code created.</span></span> <span data-ttu-id="970a0-198">生成されたフィールドの一覧を次の一部となる HTML テーブルを**スキャフォールディング**が表形式のデータを表示するために使用しています。</span><span class="sxs-lookup"><span data-stu-id="970a0-198">The generated list of fields will be part of the following HTML table that **Scaffolding** is using for displaying tabular data.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. <span data-ttu-id="970a0-199">置換、 **&lt;テーブル&gt;** のみを表示する次のコードを使用したコード、**ジャンル**、**アーティスト**、 **アルバムのタイトル**、および**価格**フィールド。</span><span class="sxs-lookup"><span data-stu-id="970a0-199">Replace the **&lt;table&gt;** code with the following code to display only the **Genre**, **Artist**, **Album Title**, and **Price** fields.</span></span> <span data-ttu-id="970a0-200">これにより、削除、 **AlbumId**と**アルバム アート URL**列。</span><span class="sxs-lookup"><span data-stu-id="970a0-200">This deletes the **AlbumId** and **Album Art URL** columns.</span></span> <span data-ttu-id="970a0-201">また、そのリンクされたクラス プロパティを表示する GenreId、ArtistId の列を変更**Artist.Name**と**Genre.Name**、し、削除、**詳細**リンク。</span><span class="sxs-lookup"><span data-stu-id="970a0-201">Also, it changes GenreId and ArtistId columns to display their linked class properties of **Artist.Name** and **Genre.Name**, and removes the **Details** link.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. <span data-ttu-id="970a0-202">次の説明を変更します。</span><span class="sxs-lookup"><span data-stu-id="970a0-202">Change the following descriptions.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="970a0-203">タスク 5 - アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="970a0-203">Task 5 - Running the Application</span></span>

<span data-ttu-id="970a0-204">このタスクではテストを**StoreManager** **インデックス**ビュー テンプレートには、前の手順のデザインに従ってアルバムの一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="970a0-204">In this task, you will test that the **StoreManager** **Index** View template displays a list of albums according to the design of the previous steps.</span></span>

1. <span data-ttu-id="970a0-205">キーを押して**F5**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="970a0-205">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="970a0-206">ホーム ページで、プロジェクトが開始されます。</span><span class="sxs-lookup"><span data-stu-id="970a0-206">The project starts in the Home page.</span></span> <span data-ttu-id="970a0-207">URL を変更して **/StoreManager**アルバムの一覧が表示されますかを示すことを確認する、**タイトル**、**アーティスト**と**ジャンル**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-207">Change the URL to **/StoreManager** to verify that a list of albums is displayed, showing their **Title**, **Artist** and **Genre**.</span></span>

    <span data-ttu-id="970a0-208">![アルバムの一覧を参照](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "アルバムの一覧を参照")</span><span class="sxs-lookup"><span data-stu-id="970a0-208">![Browsing the list of albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Browsing the list of albums")</span></span>

    <span data-ttu-id="970a0-209">*アルバムの一覧を参照*</span><span class="sxs-lookup"><span data-stu-id="970a0-209">*Browsing the list of albums*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a><span data-ttu-id="970a0-210">手順 2: HTML ヘルパーを追加します。</span><span class="sxs-lookup"><span data-stu-id="970a0-210">Exercise 2: Adding an HTML Helper</span></span>

<span data-ttu-id="970a0-211">StoreManager インデックス ページが 1 つの潜在的な問題: タイトル、アーティスト名のプロパティ両方できる、テーブルを書式設定をスローするのに十分な長さ。</span><span class="sxs-lookup"><span data-stu-id="970a0-211">The StoreManager Index page has one potential issue: Title and Artist Name properties can both be long enough to throw off the table formatting.</span></span> <span data-ttu-id="970a0-212">この演習では、そのテキストを切り捨てるカスタム HTML ヘルパーを追加する方法を学びます。</span><span class="sxs-lookup"><span data-stu-id="970a0-212">In this exercise you will learn how to add a custom HTML helper to truncate that text.</span></span>

<span data-ttu-id="970a0-213">次の図では、小さなブラウザーのサイズを使用するときに、テキストの長さのために変更が形式の方法がわかります。</span><span class="sxs-lookup"><span data-stu-id="970a0-213">In the following figure, you can see how the format is modified because of the length of the text when you use a small browser size.</span></span>

<span data-ttu-id="970a0-214">![切り捨てられたテキストとアルバムの一覧を参照しない](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "切り捨てられたテキストとアルバムの一覧を参照できません")</span><span class="sxs-lookup"><span data-stu-id="970a0-214">![Browsing the list of Albums with not truncated text](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Browsing the list of Albums with not truncated text")</span></span>

<span data-ttu-id="970a0-215">*切り捨てられたテキストとアルバムの一覧を参照できません。*</span><span class="sxs-lookup"><span data-stu-id="970a0-215">*Browsing the list of Albums with not truncated text*</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a><span data-ttu-id="970a0-216">タスク 1 - HTML ヘルパーを拡張します。</span><span class="sxs-lookup"><span data-stu-id="970a0-216">Task 1 - Extending the HTML Helper</span></span>

<span data-ttu-id="970a0-217">このタスクでは、新しいメソッドを追加する**Truncate**を**HTML** ASP.NET MVC ビュー内で公開されているオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="970a0-217">In this task, you will add a new method **Truncate** to the **HTML** object exposed within ASP.NET MVC Views.</span></span> <span data-ttu-id="970a0-218">これを行うには、実装、**拡張メソッド**組み込み**System.Web.Mvc.HtmlHelper** ASP.NET MVC によって提供されるクラス。</span><span class="sxs-lookup"><span data-stu-id="970a0-218">To do this, you will implement an **extension method** to the built-in **System.Web.Mvc.HtmlHelper** class provided by ASP.NET MVC.</span></span>

> [!NOTE]
> <span data-ttu-id="970a0-219">詳細について**拡張メソッド**、こちらの msdn 記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="970a0-219">To read more about **Extension Methods**, please visit this msdn article.</span></span> <span data-ttu-id="970a0-220">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span><span class="sxs-lookup"><span data-stu-id="970a0-220">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span></span>


1. <span data-ttu-id="970a0-221">開く、**開始**ソリューションがある**ソース/Ex2-AddingAnHTMLHelper/開始/** フォルダー。</span><span class="sxs-lookup"><span data-stu-id="970a0-221">Open the **Begin** solution located at **Source/Ex2-AddingAnHTMLHelper/Begin/** folder.</span></span> <span data-ttu-id="970a0-222">使用を続ける可能性がありますそれ以外の場合、**エンド**ソリューションは、前の演習を完了して取得します。</span><span class="sxs-lookup"><span data-stu-id="970a0-222">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="970a0-223">指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。</span><span class="sxs-lookup"><span data-stu-id="970a0-223">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="970a0-224">これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-224">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="970a0-225">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。</span><span class="sxs-lookup"><span data-stu-id="970a0-225">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="970a0-226">最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-226">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="970a0-227">NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="970a0-227">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="970a0-228">With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="970a0-228">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="970a0-229">これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="970a0-229">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="970a0-230">StoreManager のインデックス ビューを開きます。</span><span class="sxs-lookup"><span data-stu-id="970a0-230">Open StoreManager's Index View.</span></span> <span data-ttu-id="970a0-231">これを行うには、ソリューション エクスプ ローラーで展開、**ビュー**フォルダー、 **StoreManager**を開くと、 **Index.cshtml**ファイル。</span><span class="sxs-lookup"><span data-stu-id="970a0-231">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
3. <span data-ttu-id="970a0-232">下の次のコードを追加、 <strong>@model</strong>を定義するディレクティブ、 <strong>Truncate</strong>ヘルパー メソッド。</span><span class="sxs-lookup"><span data-stu-id="970a0-232">Add the following code below the <strong>@model</strong> directive to define the <strong>Truncate</strong> helper method.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a><span data-ttu-id="970a0-233">タスク 2 - ページのテキストの切り捨て</span><span class="sxs-lookup"><span data-stu-id="970a0-233">Task 2 - Truncating Text in the Page</span></span>

<span data-ttu-id="970a0-234">このタスクでは、使用、 **Truncate**メソッドをテンプレートの表示のテキストを切り捨てます。</span><span class="sxs-lookup"><span data-stu-id="970a0-234">In this task, you will use the **Truncate** method to truncate the text in the View template.</span></span>

1. <span data-ttu-id="970a0-235">StoreManager のインデックス ビューを開きます。</span><span class="sxs-lookup"><span data-stu-id="970a0-235">Open StoreManager's Index View.</span></span> <span data-ttu-id="970a0-236">これを行うには、ソリューション エクスプ ローラーで展開、**ビュー**フォルダー、 **StoreManager**を開くと、 **Index.cshtml**ファイル。</span><span class="sxs-lookup"><span data-stu-id="970a0-236">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
2. <span data-ttu-id="970a0-237">表示する行を置き換える、**アーティスト名**とアルバムの**タイトル**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-237">Replace the lines that show the **Artist Name** and Album's **Title**.</span></span> <span data-ttu-id="970a0-238">これを行うには、次の行を置き換えます。</span><span class="sxs-lookup"><span data-stu-id="970a0-238">To do this, replace the following lines.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="970a0-239">タスク 3 - アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="970a0-239">Task 3 - Running the Application</span></span>

<span data-ttu-id="970a0-240">このタスクではテストを**StoreManager** **インデックス**アルバムのタイトル、アーティスト名、テンプレートの表示が切り捨てられます。</span><span class="sxs-lookup"><span data-stu-id="970a0-240">In this task, you will test that the **StoreManager** **Index** View template truncates the Album's Title and Artist Name.</span></span>

1. <span data-ttu-id="970a0-241">キーを押して**F5**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="970a0-241">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="970a0-242">ホーム ページで、プロジェクトが開始されます。</span><span class="sxs-lookup"><span data-stu-id="970a0-242">The project starts in the Home page.</span></span> <span data-ttu-id="970a0-243">URL を変更して **/StoreManager**ことを確認するテキストで、**タイトル**と**アーティスト**列は切り捨てられます。</span><span class="sxs-lookup"><span data-stu-id="970a0-243">Change the URL to **/StoreManager** to verify that long texts in the **Title** and **Artist** column are truncated.</span></span>

    <span data-ttu-id="970a0-244">![切り捨てのタイトルやアーティスト名](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "切り捨てタイトルやアーティスト名")</span><span class="sxs-lookup"><span data-stu-id="970a0-244">![Truncated titles and artists names](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Truncated titles and artists names")</span></span>

    <span data-ttu-id="970a0-245">*切り捨てられたタイトル、アーティスト名*</span><span class="sxs-lookup"><span data-stu-id="970a0-245">*Truncated Titles and Artist Names*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a><span data-ttu-id="970a0-246">手順 3: 編集ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="970a0-246">Exercise 3: Creating the Edit View</span></span>

<span data-ttu-id="970a0-247">この演習では、ストア管理者は、アルバムの編集を許可するフォームを作成する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="970a0-247">In this exercise, you will learn how to create a form to allow store managers to edit an Album.</span></span> <span data-ttu-id="970a0-248">参照する、 **/StoreManager/Edit/id** URL (**id**を編集するアルバムの一意の id)、サーバーへの HTTP GET 呼び出しになります。</span><span class="sxs-lookup"><span data-stu-id="970a0-248">They will browse the **/StoreManager/Edit/id** URL (**id** being the unique id of the album to edit), thus making an HTTP-GET call to the server.</span></span>

<span data-ttu-id="970a0-249">コント ローラーの Edit アクション メソッドは、データベースから適切なアルバムを取得、作成、 **StoreManagerViewModel** (およびアーティスト、ジャンルのリスト) をカプセル化をにビュー テンプレートに渡すオブジェクトユーザーには、HTML ページをレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="970a0-249">The Controller Edit action method will retrieve the appropriate Album from the database, create a **StoreManagerViewModel** object to encapsulate it (along with a list of Artists and Genres), and then pass it off to a View template to render the HTML page back to the user.</span></span> <span data-ttu-id="970a0-250">このページが表示されます、 **&lt;フォーム&gt;** テキスト ボックスとアルバムのプロパティを編集するためのドロップダウン リストを持つ要素。</span><span class="sxs-lookup"><span data-stu-id="970a0-250">This page will contain a **&lt;form&gt;** element with textboxes and dropdowns for editing the Album properties.</span></span>

<span data-ttu-id="970a0-251">ユーザーは、アルバムのフォーム値を更新しをクリックすると、**保存** ボタンを使用して、変更を送信、HTTP POST にコールバックする **/StoreManager/Edit/id**します。URL は、最後の呼び出しの場合と同様、ASP.NET MVC を識別するそのため、別の Edit アクション メソッドを実行し、HTTP POST は、この時間 (で修飾された 1 つ **[HttpPost]**)。</span><span class="sxs-lookup"><span data-stu-id="970a0-251">Once the user updates the Album form values and clicks the **Save** button, the changes are submitted via an HTTP-POST call back to **/StoreManager/Edit/id**. Although the URL remains the same as in the last call, ASP.NET MVC identifies that this time it is an HTTP-POST and therefore executes a different Edit action method (one decorated with **[HttpPost]**).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a><span data-ttu-id="970a0-252">タスク 1 - HTTP GET の Edit アクション メソッドを実装します。</span><span class="sxs-lookup"><span data-stu-id="970a0-252">Task 1 - Implementing the HTTP-GET Edit Action Method</span></span>

<span data-ttu-id="970a0-253">このタスクでは、データベースから適切なアルバムを取得する Edit アクション メソッドの HTTP GET のバージョンとのすべてのジャンル、アーティスト一覧を実装します。</span><span class="sxs-lookup"><span data-stu-id="970a0-253">In this task, you will implement the HTTP-GET version of the Edit action method to retrieve the appropriate Album from the database, as well as a list of all Genres and Artists.</span></span> <span data-ttu-id="970a0-254">このデータをパッケージ化、 **StoreManagerViewModel**で応答をレンダリングするビュー テンプレートに渡されるし、最後の手順で定義されているオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="970a0-254">It will package this data up into the **StoreManagerViewModel** object defined in the last step, which will then be passed to a View template to render the response with.</span></span>

1. <span data-ttu-id="970a0-255">開く、**開始**ソリューションがある**ソース/Ex3-CreatingTheEditView/開始/** フォルダー。</span><span class="sxs-lookup"><span data-stu-id="970a0-255">Open the **Begin** solution located at **Source/Ex3-CreatingTheEditView/Begin/** folder.</span></span> <span data-ttu-id="970a0-256">使用を続ける可能性がありますそれ以外の場合、**エンド**ソリューションは、前の演習を完了して取得します。</span><span class="sxs-lookup"><span data-stu-id="970a0-256">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="970a0-257">指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。</span><span class="sxs-lookup"><span data-stu-id="970a0-257">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="970a0-258">これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-258">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="970a0-259">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。</span><span class="sxs-lookup"><span data-stu-id="970a0-259">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="970a0-260">最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-260">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="970a0-261">NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="970a0-261">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="970a0-262">With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="970a0-262">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="970a0-263">これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="970a0-263">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="970a0-264">開く、 **StoreManagerController**クラス。</span><span class="sxs-lookup"><span data-stu-id="970a0-264">Open the **StoreManagerController** class.</span></span> <span data-ttu-id="970a0-265">これを行うには、展開、**コント ローラー**フォルダーをダブルクリックします**StoreManagerController.cs**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-265">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="970a0-266">置換、 **HTTP-GET 編集**アクション メソッドを次のコードを適切な取得**アルバム**だけでなく**ジャンル**と**アーティスト**を一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="970a0-266">Replace the **HTTP-GET Edit** action method with the following code to retrieve the appropriate **Album** as well as the **Genres** and **Artists** lists.</span></span>

    <span data-ttu-id="970a0-267">(コード スニペット -*と ASP.NET MVC 4 ヘルパー、フォーム、および検証のアクションの Ex3 StoreManagerController HTTP-GET 編集*)</span><span class="sxs-lookup"><span data-stu-id="970a0-267">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-GET Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="970a0-268">使用している**System.Web.Mvc** **SelectList**のアーティスト、ジャンルの代わりに、 **System.Collections.Generic**一覧。</span><span class="sxs-lookup"><span data-stu-id="970a0-268">You are using **System.Web.Mvc** **SelectList** for Artists and Genres instead of the **System.Collections.Generic** List.</span></span>
    > 
    > <span data-ttu-id="970a0-269">**SelectList**クリーナー HTML ドロップダウン リストからの入力および現在の選択などを管理する方法です。</span><span class="sxs-lookup"><span data-stu-id="970a0-269">**SelectList** is a cleaner way to populate HTML dropdowns and manage things like current selection.</span></span> <span data-ttu-id="970a0-270">インスタンス化し、コント ローラー アクションでこれらの ViewModel オブジェクトを後で設定すると、編集フォーム シナリオ クリーナーします。</span><span class="sxs-lookup"><span data-stu-id="970a0-270">Instantiating and later setting up these ViewModel objects in the controller action will make the Edit form scenario cleaner.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a><span data-ttu-id="970a0-271">タスク 2 - 編集ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="970a0-271">Task 2 - Creating the Edit View</span></span>

<span data-ttu-id="970a0-272">このタスクでは、後で、アルバムのプロパティを表示する編集ビュー テンプレートを作成します。</span><span class="sxs-lookup"><span data-stu-id="970a0-272">In this task, you will create an Edit View template that will later display the album properties.</span></span>

1. <span data-ttu-id="970a0-273">編集ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="970a0-273">Create the Edit View.</span></span> <span data-ttu-id="970a0-274">これを行うには、内を右クリックして、**編集**アクション メソッドと select**ビューの追加**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-274">To do this, right-click inside the **Edit** action method and select **Add View**.</span></span>
2. <span data-ttu-id="970a0-275">ビューの追加 ダイアログ ボックスで、ビュー名があることを確認します**編集**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-275">In the Add View dialog, verify that the View Name is **Edit**.</span></span> <span data-ttu-id="970a0-276">チェック、**厳密に型指定されたビューを作成**チェック ボックスを選択し、**アルバム (MvcMusicStore.Models)** から、**データ クラスを表示**ドロップダウンします。</span><span class="sxs-lookup"><span data-stu-id="970a0-276">Check the **Create a strongly-typed view** checkbox and select **Album (MvcMusicStore.Models)** from the **View data class** drop-down.</span></span> <span data-ttu-id="970a0-277">選択**編集**から、**スキャフォールディング テンプレート**ドロップダウンします。</span><span class="sxs-lookup"><span data-stu-id="970a0-277">Select **Edit** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="970a0-278">その他のフィールド値が既定値のままにし、**追加**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-278">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="970a0-279">![編集ビューを追加する](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "編集ビューを追加します。")</span><span class="sxs-lookup"><span data-stu-id="970a0-279">![Adding an Edit view](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Adding an Edit view")</span></span>

    <span data-ttu-id="970a0-280">*編集ビューを追加します。*</span><span class="sxs-lookup"><span data-stu-id="970a0-280">*Adding an Edit view*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="970a0-281">タスク 3 - アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="970a0-281">Task 3 - Running the Application</span></span>

<span data-ttu-id="970a0-282">このタスクではテストを**StoreManager** **編集**ビュー ページには、パラメーターとして渡されるアルバムのプロパティの値が表示されます。</span><span class="sxs-lookup"><span data-stu-id="970a0-282">In this task, you will test that the **StoreManager** **Edit** View page displays the properties' values for the album passed as parameter.</span></span>

1. <span data-ttu-id="970a0-283">キーを押して**F5**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="970a0-283">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="970a0-284">ホーム ページで、プロジェクトが開始されます。</span><span class="sxs-lookup"><span data-stu-id="970a0-284">The project starts in the Home page.</span></span> <span data-ttu-id="970a0-285">URL を変更して **/StoreManager/Edit/1**を渡されたアルバムのプロパティの値が表示されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="970a0-285">Change the URL to **/StoreManager/Edit/1** to verify that the properties' values for the album passed are displayed.</span></span>

    <span data-ttu-id="970a0-286">![アルバムの編集ビューを閲覧](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "アルバムの編集ビューの参照")</span><span class="sxs-lookup"><span data-stu-id="970a0-286">![Browsing Album's Edit View](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Browsing Album's Edit View")</span></span>

    <span data-ttu-id="970a0-287">*アルバムの編集ビューの参照*</span><span class="sxs-lookup"><span data-stu-id="970a0-287">*Browsing Album's Edit view*</span></span>

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a><span data-ttu-id="970a0-288">タスク 4 - アルバム エディター テンプレートをドロップダウン リストを実装します。</span><span class="sxs-lookup"><span data-stu-id="970a0-288">Task 4 - Implementing drop-downs on the Album Editor Template</span></span>

<span data-ttu-id="970a0-289">このタスクで、ユーザーは、アーティスト、ジャンルの一覧から選択できるように、前回の作業で作成したビュー テンプレートをドロップダウン リストを追加します。</span><span class="sxs-lookup"><span data-stu-id="970a0-289">In this task, you will add drop-downs to the View template created in the last task, so that the user can select from a list of Artists and Genres.</span></span>

1. <span data-ttu-id="970a0-290">[すべて置換]、**アルバム**コードを次のフィールド セット。</span><span class="sxs-lookup"><span data-stu-id="970a0-290">Replace all the **Album** fieldset code with the following:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="970a0-291">**Html.DropDownList**アーティスト、ジャンルを選択するためのドロップダウン リストを表示するためにヘルパーが追加されました。</span><span class="sxs-lookup"><span data-stu-id="970a0-291">An **Html.DropDownList** helper has been added to render drop-downs for choosing Artists and Genres.</span></span> <span data-ttu-id="970a0-292">渡されるパラメーター **Html.DropDownList**は。</span><span class="sxs-lookup"><span data-stu-id="970a0-292">The parameters passed to **Html.DropDownList** are:</span></span>
    > 
    > 1. <span data-ttu-id="970a0-293">フォーム フィールドの名前 (**&quot;ArtistId&quot;**)。</span><span class="sxs-lookup"><span data-stu-id="970a0-293">The name of the form field (**&quot;ArtistId&quot;**).</span></span>
    > 2. <span data-ttu-id="970a0-294">**SelectList**のドロップダウン リストの値。</span><span class="sxs-lookup"><span data-stu-id="970a0-294">The **SelectList** of values for the drop-down.</span></span>

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="970a0-295">タスク 5 - アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="970a0-295">Task 5 - Running the Application</span></span>

<span data-ttu-id="970a0-296">このタスクではテストを**StoreManager** **編集**ビュー ページには、アーティスト、ジャンル ID のテキスト フィールドではなくドロップダウン リストが表示されます。</span><span class="sxs-lookup"><span data-stu-id="970a0-296">In this task, you will test that the **StoreManager** **Edit** View page displays drop-downs instead of Artist and Genre ID text fields.</span></span>

1. <span data-ttu-id="970a0-297">キーを押して**F5**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="970a0-297">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="970a0-298">ホーム ページで、プロジェクトが開始されます。</span><span class="sxs-lookup"><span data-stu-id="970a0-298">The project starts in the Home page.</span></span> <span data-ttu-id="970a0-299">URL を変更して **/StoreManager/Edit/1**アーティスト、ジャンル ID のテキスト フィールドではなくドロップダウン リストが表示されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="970a0-299">Change the URL to **/StoreManager/Edit/1** to verify that it displays drop-downs instead of Artist and Genre ID text fields.</span></span>

    <span data-ttu-id="970a0-300">![アルバムのドロップダウン リストでビューの編集をブラウズ](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "参照アルバムのドロップダウン リストでビューの編集")</span><span class="sxs-lookup"><span data-stu-id="970a0-300">![Browsing Album's Edit View with drop downs](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Browsing Album's Edit View with drop downs")</span></span>

    <span data-ttu-id="970a0-301">*アルバムの編集ビューのこの時点で、ドロップダウン リストからの参照*</span><span class="sxs-lookup"><span data-stu-id="970a0-301">*Browsing Album's Edit view, this time with dropdowns*</span></span>

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a><span data-ttu-id="970a0-302">タスク 6 - HTTP POST の Edit アクション メソッドを実装します。</span><span class="sxs-lookup"><span data-stu-id="970a0-302">Task 6 - Implementing the HTTP-POST Edit action method</span></span>

<span data-ttu-id="970a0-303">ビューの編集は、期待どおりに表示されます、アルバムに加えた変更を保存する HTTP POST アクションの編集メソッドを実装する必要があります。</span><span class="sxs-lookup"><span data-stu-id="970a0-303">Now that the Edit View displays as expected, you need to implement the HTTP-POST Edit Action method to save the changes made to the Album.</span></span>

1. <span data-ttu-id="970a0-304">Visual Studio ウィンドウに戻りますが、必要な場合は、ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="970a0-304">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="970a0-305">開いている**StoreManagerController**から、**コント ローラー**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="970a0-305">Open **StoreManagerController** from the **Controllers** folder.</span></span>
2. <span data-ttu-id="970a0-306">置換**HTTP POST 編集**を次のアクション メソッドのコード (2 つのパラメーターを受け取るオーバー ロードされたバージョンに置き換える必要があるメソッドに注意してください)。</span><span class="sxs-lookup"><span data-stu-id="970a0-306">Replace **HTTP-POST Edit** action method code with the following (note that the method that must be replaced is overloaded version that receives two parameters):</span></span>

    <span data-ttu-id="970a0-307">(コード スニペット -*と ASP.NET MVC 4 ヘルパー、フォームの検証 - アクションの Ex3 StoreManagerController HTTP POST の編集および*)</span><span class="sxs-lookup"><span data-stu-id="970a0-307">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-POST Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="970a0-308">ユーザーがクリックしたときに、このメソッドが実行される、**保存**ビューのボタンをクリックし、データベースに保持へのサーバーにフォームの値の HTTP の POST を実行します。</span><span class="sxs-lookup"><span data-stu-id="970a0-308">This method will be executed when the user clicks the **Save** button of the View and performs an HTTP-POST of the form values back to the server to persist them in the database.</span></span> <span data-ttu-id="970a0-309">デコレーター **[HttpPost]** メソッドが HTTP POST のこれらのシナリオに使用することを示します。</span><span class="sxs-lookup"><span data-stu-id="970a0-309">The decorator **[HttpPost]** indicates that the method should be used for those HTTP-POST scenarios.</span></span> <span data-ttu-id="970a0-310">メソッドには、**アルバム**オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="970a0-310">The method takes an **Album** object.</span></span> <span data-ttu-id="970a0-311">ASP.NET MVC が、ポストされたからアルバム オブジェクトを自動的に作成します。&lt;フォーム&gt;値。</span><span class="sxs-lookup"><span data-stu-id="970a0-311">ASP.NET MVC will automatically create the Album object from the posted &lt;form&gt; values.</span></span>
    > 
    > <span data-ttu-id="970a0-312">メソッドは、これらの手順が実行されます。</span><span class="sxs-lookup"><span data-stu-id="970a0-312">The method will perform these steps:</span></span>
    > 
    > 1. <span data-ttu-id="970a0-313">モデルが有効な場合。</span><span class="sxs-lookup"><span data-stu-id="970a0-313">If model is valid:</span></span>
    > 
    >     1. <span data-ttu-id="970a0-314">変更されたオブジェクトをマークするコンテキストでアルバム エントリを更新します。</span><span class="sxs-lookup"><span data-stu-id="970a0-314">Update the album entry in the context to mark it as a modified object.</span></span>
    >     2. <span data-ttu-id="970a0-315">変更を保存し、インデックス ビューにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="970a0-315">Save the changes and redirect to the index view.</span></span>
    > 2. <span data-ttu-id="970a0-316">ViewBag を設定は、モデルが有効でない場合、 **GenreId**と**ArtistId**、ユーザーが受信したアルバム オブジェクトにビューが返されますし、任意の必要な更新プログラムを実行します。</span><span class="sxs-lookup"><span data-stu-id="970a0-316">If the model is not valid, it will populate the ViewBag with the **GenreId** and **ArtistId**, then it will return the view with the received Album object to allow the user perform any required update.</span></span>

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="970a0-317">タスク 7 のアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="970a0-317">Task 7 - Running the Application</span></span>

<span data-ttu-id="970a0-318">このタスクではテストを**StoreManager 編集**ビュー ページでは、データベースで、アルバムの更新されたデータを実際に保存します。</span><span class="sxs-lookup"><span data-stu-id="970a0-318">In this task, you will test that the **StoreManager Edit** View page actually saves the updated Album data in the database.</span></span>

1. <span data-ttu-id="970a0-319">キーを押して**F5**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="970a0-319">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="970a0-320">ホーム ページで、プロジェクトが開始されます。</span><span class="sxs-lookup"><span data-stu-id="970a0-320">The project starts in the Home page.</span></span> <span data-ttu-id="970a0-321">URL を変更して **/StoreManager/Edit/1**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-321">Change the URL to **/StoreManager/Edit/1**.</span></span> <span data-ttu-id="970a0-322">アルバム タイトルに変更**ロード** をクリック**保存**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-322">Change the Album title to **Load** and click on **Save**.</span></span> <span data-ttu-id="970a0-323">アルバムのタイトルがアルバムの一覧で、実際に変更されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="970a0-323">Verify that album's title actually changed in the list of albums.</span></span>

    <span data-ttu-id="970a0-324">![アルバムを更新](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "アルバムを更新しています")</span><span class="sxs-lookup"><span data-stu-id="970a0-324">![Updating an album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "Updating an album")</span></span>

    <span data-ttu-id="970a0-325">*アルバムを更新しています*</span><span class="sxs-lookup"><span data-stu-id="970a0-325">*Updating an Album*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a><span data-ttu-id="970a0-326">手順 4: 追加のビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="970a0-326">Exercise 4: Adding a Create View</span></span>

<span data-ttu-id="970a0-327">これで、 **StoreManagerController**をサポートしています、**編集**格納できるようにするビューの作成テンプレートを追加する方法について説明しますが、この演習では、機能マネージャーは、アプリケーションに新しいアルバムを追加します。</span><span class="sxs-lookup"><span data-stu-id="970a0-327">Now that the **StoreManagerController** supports the **Edit** ability, in this exercise you will learn how to add a Create View template to let store managers add new Albums to the application.</span></span>

<span data-ttu-id="970a0-328">内の 2 つの個別のメソッドを使用して作成シナリオを実装は編集機能と同様、 **StoreManagerController**クラス。</span><span class="sxs-lookup"><span data-stu-id="970a0-328">Like you did with the Edit functionality, you will implement the Create scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="970a0-329">1 つのアクション メソッドは、ストア マネージャーは初回アクセス時に空のフォームに表示されます、 **/StoreManager/作成**URL。</span><span class="sxs-lookup"><span data-stu-id="970a0-329">One action method will display an empty form when store managers first visit the **/StoreManager/Create** URL.</span></span>
2. <span data-ttu-id="970a0-330">2 つ目のアクション メソッドは、シナリオを処理するストア マネージャーがクリックした、**保存**フォーム内でボタンをクリックし、値を送信する、 **/StoreManager/作成**として、HTTP POST の URL。</span><span class="sxs-lookup"><span data-stu-id="970a0-330">A second action method will handle the scenario where the store manager clicks the **Save** button within the form and submits the values back to the **/StoreManager/Create** URL as an HTTP-POST.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a><span data-ttu-id="970a0-331">タスク 1 - アクションの作成の HTTP GET メソッドを実装します。</span><span class="sxs-lookup"><span data-stu-id="970a0-331">Task 1 - Implementing the HTTP-GET Create action method</span></span>

<span data-ttu-id="970a0-332">すべてのジャンル、アーティストの一覧を取得するにこのデータをパッケージ化、Create アクション メソッドの HTTP GET のバージョンを実装するこのタスクで、 **StoreManagerViewModel**テンプレートの表示にし、渡されるオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="970a0-332">In this task, you will implement the HTTP-GET version of the Create action method to retrieve a list of all Genres and Artists, package this data up into a **StoreManagerViewModel** object, which will then be passed to a View template.</span></span>

1. <span data-ttu-id="970a0-333">開く、**開始**ソリューションがある**ソース/Ex4-AddingACreateView/開始/** フォルダー。</span><span class="sxs-lookup"><span data-stu-id="970a0-333">Open the **Begin** solution located at **Source/Ex4-AddingACreateView/Begin/** folder.</span></span> <span data-ttu-id="970a0-334">使用を続ける可能性がありますそれ以外の場合、**エンド**ソリューションは、前の演習を完了して取得します。</span><span class="sxs-lookup"><span data-stu-id="970a0-334">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="970a0-335">指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。</span><span class="sxs-lookup"><span data-stu-id="970a0-335">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="970a0-336">これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-336">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="970a0-337">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。</span><span class="sxs-lookup"><span data-stu-id="970a0-337">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="970a0-338">最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-338">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="970a0-339">NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="970a0-339">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="970a0-340">With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="970a0-340">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="970a0-341">これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="970a0-341">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="970a0-342">開いている**StoreManagerController**クラス。</span><span class="sxs-lookup"><span data-stu-id="970a0-342">Open **StoreManagerController** class.</span></span> <span data-ttu-id="970a0-343">これを行うには、展開、**コント ローラー**フォルダーをダブルクリックします**StoreManagerController.cs**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-343">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="970a0-344">置換、**作成**を次のアクション メソッドのコード。</span><span class="sxs-lookup"><span data-stu-id="970a0-344">Replace the **Create** action method code with the following:</span></span>

    <span data-ttu-id="970a0-345">(コード スニペット -*と ASP.NET MVC 4 ヘルパー、フォーム、および Ex4 StoreManagerController HTTP GET の作成操作の検証 -*)</span><span class="sxs-lookup"><span data-stu-id="970a0-345">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP-GET Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a><span data-ttu-id="970a0-346">タスク 2 - 作成ビューの追加</span><span class="sxs-lookup"><span data-stu-id="970a0-346">Task 2 - Adding the Create View</span></span>

<span data-ttu-id="970a0-347">このタスクでは、新しい (空) のアルバムのフォームを表示するビューの作成テンプレートを追加します。</span><span class="sxs-lookup"><span data-stu-id="970a0-347">In this task, you will add the Create View template that will display a new (empty) Album form.</span></span>

1. <span data-ttu-id="970a0-348">内側を右クリックし、**作成**アクション メソッドと select**ビューの追加**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-348">Right-click inside the **Create** action method and select **Add View**.</span></span> <span data-ttu-id="970a0-349">ビューの追加 ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="970a0-349">This will bring up the Add View dialog.</span></span>
2. <span data-ttu-id="970a0-350">ビューの追加 ダイアログ ボックスで、ビュー名があることを確認します**作成**です。</span><span class="sxs-lookup"><span data-stu-id="970a0-350">In the Add View dialog, verify that the View Name is **Create**.</span></span> <span data-ttu-id="970a0-351">選択、**厳密に型指定されたビューを作成する**オプションし、選択**アルバム (MvcMusicStore.Models)** から、**モデル クラス**ドロップダウンと**を作成します。** から、**スキャフォールディング テンプレート**ドロップダウンします。</span><span class="sxs-lookup"><span data-stu-id="970a0-351">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down and **Create** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="970a0-352">その他のフィールド値が既定値のままにし、**追加**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-352">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="970a0-353">![作成ビューを追加する](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "の追加-、-作成-view.png")</span><span class="sxs-lookup"><span data-stu-id="970a0-353">![Adding a create view](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "adding-a-create-view.png")</span></span>

    <span data-ttu-id="970a0-354">*作成ビューの追加*</span><span class="sxs-lookup"><span data-stu-id="970a0-354">*Adding the Create View*</span></span>
3. <span data-ttu-id="970a0-355">更新プログラム、 **GenreId**と**ArtistId**次に示すように、ドロップダウン リストを使用するフィールド。</span><span class="sxs-lookup"><span data-stu-id="970a0-355">Update the **GenreId** and **ArtistId** fields to use a drop-down list as shown below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="970a0-356">タスク 3 - アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="970a0-356">Task 3 - Running the Application</span></span>

<span data-ttu-id="970a0-357">このタスクではテストを**StoreManager** **作成**ビュー ページには、アルバムの空のフォームが表示されます。</span><span class="sxs-lookup"><span data-stu-id="970a0-357">In this task, you will test that the **StoreManager** **Create** View page displays an empty Album form.</span></span>

1. <span data-ttu-id="970a0-358">キーを押して**F5**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="970a0-358">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="970a0-359">ホーム ページで、プロジェクトが開始されます。</span><span class="sxs-lookup"><span data-stu-id="970a0-359">The project starts in the Home page.</span></span> <span data-ttu-id="970a0-360">URL を変更して**StoreManager/作成**です。</span><span class="sxs-lookup"><span data-stu-id="970a0-360">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="970a0-361">新しいアルバム プロパティを入力するための空のフォームが表示されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="970a0-361">Verify that an empty form is displayed for filling the new Album properties.</span></span>

    <span data-ttu-id="970a0-362">![空のフォームでビュー作成](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "の空のフォームとビューの作成")</span><span class="sxs-lookup"><span data-stu-id="970a0-362">![Create View with an empty form](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View with an empty form")</span></span>

    <span data-ttu-id="970a0-363">*空のフォームとビューを作成します。*</span><span class="sxs-lookup"><span data-stu-id="970a0-363">*Create View with an empty form*</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a><span data-ttu-id="970a0-364">タスク 4 - HTTP POST を実装するアクション メソッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="970a0-364">Task 4 - Implementing the HTTP-POST Create Action Method</span></span>

<span data-ttu-id="970a0-365">このタスクでは、ユーザーがクリックしたときに呼び出される Create アクション メソッドの HTTP POST のバージョンを実装します、**保存**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="970a0-365">In this task, you will implement the HTTP-POST version of the Create action method that will be invoked when a user clicks the **Save** button.</span></span> <span data-ttu-id="970a0-366">メソッドは、データベースに新しいアルバムを保存する必要があります。</span><span class="sxs-lookup"><span data-stu-id="970a0-366">The method should save the new album in the database.</span></span>

1. <span data-ttu-id="970a0-367">Visual Studio ウィンドウに戻りますが、必要な場合は、ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="970a0-367">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="970a0-368">開いている**StoreManagerController**クラス。</span><span class="sxs-lookup"><span data-stu-id="970a0-368">Open **StoreManagerController** class.</span></span> <span data-ttu-id="970a0-369">これを行うには、展開、**コント ローラー**フォルダーをダブルクリックします**StoreManagerController.cs**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-369">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="970a0-370">置換**HTTP POST 作成**を次のアクション メソッドのコード。</span><span class="sxs-lookup"><span data-stu-id="970a0-370">Replace **HTTP-POST Create** action method code with the following:</span></span>

    <span data-ttu-id="970a0-371">(コード スニペット -*と ASP.NET MVC 4 ヘルパー、フォーム、および Ex4 StoreManagerController HTTP-投稿を作成するアクションの検証 -*)</span><span class="sxs-lookup"><span data-stu-id="970a0-371">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP- POST Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="970a0-372">作成アクションは、前の Edit アクション メソッドとよく似ていますが、変更されたオブジェクトを設定する代わりに追加されているコンテキストにします。</span><span class="sxs-lookup"><span data-stu-id="970a0-372">The Create action is pretty similar to the previous Edit action method but instead of setting the object as modified, it is being added to the context.</span></span>

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="970a0-373">タスク 5 - アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="970a0-373">Task 5 - Running the Application</span></span>

<span data-ttu-id="970a0-374">このタスクでは、テストを**StoreManager 作成**ビュー ページが新しいアルバムを作成することができ、StoreManager インデックス ビューにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="970a0-374">In this task, you will test that the **StoreManager Create** View page lets you create a new Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="970a0-375">キーを押して**F5**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="970a0-375">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="970a0-376">ホーム ページで、プロジェクトが開始されます。</span><span class="sxs-lookup"><span data-stu-id="970a0-376">The project starts in the Home page.</span></span> <span data-ttu-id="970a0-377">URL を変更して**StoreManager/作成**です。</span><span class="sxs-lookup"><span data-stu-id="970a0-377">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="970a0-378">次の図のように、新しいアルバムのデータをすべてのフォーム フィールドを入力します。</span><span class="sxs-lookup"><span data-stu-id="970a0-378">Fill all the form fields with data for a new Album, like the one in the following figure:</span></span>

    <span data-ttu-id="970a0-379">![アルバムを作成する](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "アルバムを作成します。")</span><span class="sxs-lookup"><span data-stu-id="970a0-379">![Creating an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "Creating an Album")</span></span>

    <span data-ttu-id="970a0-380">*アルバムを作成します。*</span><span class="sxs-lookup"><span data-stu-id="970a0-380">*Creating an Album*</span></span>
3. <span data-ttu-id="970a0-381">先ほど作成した新しいアルバムを含む StoreManager インデックス ビューにリダイレクトすることを確認します。</span><span class="sxs-lookup"><span data-stu-id="970a0-381">Verify that you get redirected to the StoreManager Index View that includes the new Album just created.</span></span>

    <span data-ttu-id="970a0-382">![作成された新しいアルバム](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "新しいアルバムの作成")</span><span class="sxs-lookup"><span data-stu-id="970a0-382">![New Album Created](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "New Album Created")</span></span>

    <span data-ttu-id="970a0-383">*新しいアルバムの作成*</span><span class="sxs-lookup"><span data-stu-id="970a0-383">*New Album Created*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a><span data-ttu-id="970a0-384">手順 5: 削除を処理します。</span><span class="sxs-lookup"><span data-stu-id="970a0-384">Exercise 5: Handling Deletion</span></span>

<span data-ttu-id="970a0-385">アルバムを削除する機能はまだ実装されていません。</span><span class="sxs-lookup"><span data-stu-id="970a0-385">The ability to delete albums is not yet implemented.</span></span> <span data-ttu-id="970a0-386">これは、この演習の詳細になります。</span><span class="sxs-lookup"><span data-stu-id="970a0-386">This is what this exercise will be about.</span></span> <span data-ttu-id="970a0-387">内の 2 つの個別のメソッドを使用して、削除のシナリオを実装する前に、ように、 **StoreManagerController**クラス。</span><span class="sxs-lookup"><span data-stu-id="970a0-387">Like before, you will implement the Delete scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="970a0-388">1 つのアクション メソッドは確認フォームを表示します。</span><span class="sxs-lookup"><span data-stu-id="970a0-388">One action method will display a confirmation form</span></span>
2. <span data-ttu-id="970a0-389">2 つ目のアクション メソッドは、フォームの送信を処理します。</span><span class="sxs-lookup"><span data-stu-id="970a0-389">A second action method will handle the form submission</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a><span data-ttu-id="970a0-390">タスク 1 - Delete アクション メソッドを HTTP GET を実装します。</span><span class="sxs-lookup"><span data-stu-id="970a0-390">Task 1 - Implementing the HTTP-GET Delete Action Method</span></span>

<span data-ttu-id="970a0-391">このタスクでは、HTTP GET のバージョンのアルバムの情報を取得する Delete アクション メソッドを実装します。</span><span class="sxs-lookup"><span data-stu-id="970a0-391">In this task, you will implement the HTTP-GET version of the Delete action method to retrieve the album's information.</span></span>

1. <span data-ttu-id="970a0-392">開く、**開始**ソリューションがある**ソース/Ex5-HandlingDeletion/開始/** フォルダー。</span><span class="sxs-lookup"><span data-stu-id="970a0-392">Open the **Begin** solution located at **Source/Ex5-HandlingDeletion/Begin/** folder.</span></span> <span data-ttu-id="970a0-393">使用を続ける可能性がありますそれ以外の場合、**エンド**ソリューションは、前の演習を完了して取得します。</span><span class="sxs-lookup"><span data-stu-id="970a0-393">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="970a0-394">指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。</span><span class="sxs-lookup"><span data-stu-id="970a0-394">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="970a0-395">これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-395">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="970a0-396">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。</span><span class="sxs-lookup"><span data-stu-id="970a0-396">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="970a0-397">最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-397">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="970a0-398">NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="970a0-398">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="970a0-399">With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="970a0-399">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="970a0-400">これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="970a0-400">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="970a0-401">開いている**StoreManagerController**クラス。</span><span class="sxs-lookup"><span data-stu-id="970a0-401">Open **StoreManagerController** class.</span></span> <span data-ttu-id="970a0-402">これを行うには、展開、**コント ローラー**フォルダーをダブルクリックします**StoreManagerController.cs**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-402">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="970a0-403">削除のコント ローラー アクションは、前のストアの詳細コント ローラー アクションと同じで、まさに: クエリ、**アルバム**オブジェクトを使用して、データベースから、 **id** URL を返しますで提供される、適切な**ビュー**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-403">The Delete controller action is exactly the same as the previous Store Details controller action: it queries the **album** object from the database using the **id** provided in the URL and returns the appropriate **View**.</span></span> <span data-ttu-id="970a0-404">これを行うには、HTTP GET を置き換える**削除**を次のアクション メソッドのコード。</span><span class="sxs-lookup"><span data-stu-id="970a0-404">To do this, replace the HTTP-GET **Delete** action method code with the following:</span></span>

    <span data-ttu-id="970a0-405">(コード スニペット -*と ASP.NET MVC 4 ヘルパー、フォーム、および Ex5 処理削除 HTTP-GET 削除アクションの検証 -*)</span><span class="sxs-lookup"><span data-stu-id="970a0-405">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-GET Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. <span data-ttu-id="970a0-406">内側を右クリックし、**削除**アクション メソッドと select**ビューの追加**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-406">Right-click inside the **Delete** action method and select **Add View**.</span></span> <span data-ttu-id="970a0-407">ビューの追加 ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="970a0-407">This will bring up the Add View dialog.</span></span>
5. <span data-ttu-id="970a0-408">ビューの追加 ダイアログ ボックスで、ビュー名があることを確認します**削除**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-408">In the Add View dialog, verify that the View name is **Delete**.</span></span> <span data-ttu-id="970a0-409">選択、**厳密に型指定されたビューを作成する**オプションし、選択**アルバム (MvcMusicStore.Models)** から、**モデル クラス**ドロップダウンします。</span><span class="sxs-lookup"><span data-stu-id="970a0-409">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="970a0-410">選択**削除**から、**スキャフォールディング テンプレート**ドロップダウンします。</span><span class="sxs-lookup"><span data-stu-id="970a0-410">Select **Delete** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="970a0-411">その他のフィールド値が既定値のままにし、**追加**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-411">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="970a0-412">![削除ビューを追加する](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Delete ビューの追加")</span><span class="sxs-lookup"><span data-stu-id="970a0-412">![Adding a Delete View](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Adding a Delete View")</span></span>

    <span data-ttu-id="970a0-413">*削除ビューの追加*</span><span class="sxs-lookup"><span data-stu-id="970a0-413">*Adding a Delete View*</span></span>
6. <span data-ttu-id="970a0-414">Delete テンプレートには、モデルからのすべてのフィールドが表示されます。</span><span class="sxs-lookup"><span data-stu-id="970a0-414">The Delete template shows all the fields from the model.</span></span> <span data-ttu-id="970a0-415">アルバムのタイトルのみが表示されます。</span><span class="sxs-lookup"><span data-stu-id="970a0-415">You will show only the album's title.</span></span> <span data-ttu-id="970a0-416">これを行うには、ビューの内容を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="970a0-416">To do this, replace the content of the view with the following code:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="970a0-417">タスク 2 - アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="970a0-417">Task 2 - Running the Application</span></span>

<span data-ttu-id="970a0-418">このタスクではテストを**StoreManager** **削除**ビュー ページ確認削除フォームを表示します。</span><span class="sxs-lookup"><span data-stu-id="970a0-418">In this task, you will test that the **StoreManager** **Delete** View page displays a confirmation deletion form.</span></span>

1. <span data-ttu-id="970a0-419">キーを押して**F5**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="970a0-419">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="970a0-420">ホーム ページで、プロジェクトが開始されます。</span><span class="sxs-lookup"><span data-stu-id="970a0-420">The project starts in the Home page.</span></span> <span data-ttu-id="970a0-421">URL を変更して **/StoreManager**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-421">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="970a0-422">1 つのアルバムをクリックして、削除を選択します。**削除**し、新しいビューがアップロードされたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="970a0-422">Select one album to delete by clicking **Delete** and verify that the new view is uploaded.</span></span>

    <span data-ttu-id="970a0-423">![アルバムを削除する](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "アルバムを削除します。")</span><span class="sxs-lookup"><span data-stu-id="970a0-423">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Deleting an Album")</span></span>

    <span data-ttu-id="970a0-424">*アルバムを削除します。*</span><span class="sxs-lookup"><span data-stu-id="970a0-424">*Deleting an Album*</span></span>

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a><span data-ttu-id="970a0-425">タスク 3 - Delete アクション メソッドを HTTP POST を実装します。</span><span class="sxs-lookup"><span data-stu-id="970a0-425">Task 3- Implementing the HTTP-POST Delete Action Method</span></span>

<span data-ttu-id="970a0-426">このタスクでは、ユーザーがクリックしたときに呼び出される Delete アクション メソッドの HTTP POST のバージョンを実装します、**削除**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="970a0-426">In this task, you will implement the HTTP-POST version of the Delete action method that will be invoked when a user clicks the **Delete** button.</span></span> <span data-ttu-id="970a0-427">メソッドは、データベースのアルバムを削除する必要があります。</span><span class="sxs-lookup"><span data-stu-id="970a0-427">The method should delete the album in the database.</span></span>

1. <span data-ttu-id="970a0-428">Visual Studio ウィンドウに戻りますが、必要な場合は、ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="970a0-428">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="970a0-429">開いている**StoreManagerController**クラス。</span><span class="sxs-lookup"><span data-stu-id="970a0-429">Open **StoreManagerController** class.</span></span> <span data-ttu-id="970a0-430">これを行うには、展開、**コント ローラー**フォルダーをダブルクリックします**StoreManagerController.cs**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-430">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="970a0-431">置換**HTTP POST Delete**を次のアクション メソッドのコード。</span><span class="sxs-lookup"><span data-stu-id="970a0-431">Replace **HTTP-POST Delete** action method code with the following:</span></span>

    <span data-ttu-id="970a0-432">(コード スニペット -*と ASP.NET MVC 4 ヘルパー、フォーム、および Ex5 処理削除 HTTP POST 削除アクションの検証 -*)</span><span class="sxs-lookup"><span data-stu-id="970a0-432">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-POST Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="970a0-433">タスク 4 - アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="970a0-433">Task 4 - Running the Application</span></span>

<span data-ttu-id="970a0-434">このタスクでは、テストを**StoreManager Delete**ビュー ページ アルバムを削除することができ、StoreManager インデックス ビューにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="970a0-434">In this task, you will test that the **StoreManager Delete** View page lets you delete an Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="970a0-435">キーを押して**F5**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="970a0-435">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="970a0-436">ホーム ページで、プロジェクトが開始されます。</span><span class="sxs-lookup"><span data-stu-id="970a0-436">The project starts in the Home page.</span></span> <span data-ttu-id="970a0-437">URL を変更して **/StoreManager**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-437">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="970a0-438">1 つのアルバムをクリックして、削除を選択します。**を削除します。**</span><span class="sxs-lookup"><span data-stu-id="970a0-438">Select one album to delete by clicking **Delete.**</span></span> <span data-ttu-id="970a0-439">クリックして、削除を確定します**削除**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="970a0-439">Confirm the deletion by clicking **Delete** button:</span></span>

    <span data-ttu-id="970a0-440">![アルバムを削除する](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "アルバムを削除します。")</span><span class="sxs-lookup"><span data-stu-id="970a0-440">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Deleting an Album")</span></span>

    <span data-ttu-id="970a0-441">*アルバムを削除します。*</span><span class="sxs-lookup"><span data-stu-id="970a0-441">*Deleting an Album*</span></span>
3. <span data-ttu-id="970a0-442">表示されないので、アルバムが削除されたことを確認、**インデックス**ページ。</span><span class="sxs-lookup"><span data-stu-id="970a0-442">Verify that the album was deleted since it does not appear in the **Index** page.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a><span data-ttu-id="970a0-443">手順 6: 検証の追加</span><span class="sxs-lookup"><span data-stu-id="970a0-443">Exercise 6: Adding Validation</span></span>

<span data-ttu-id="970a0-444">現時点では、場所にあるを作成および編集フォームには、任意の種類の検証は行いません。</span><span class="sxs-lookup"><span data-stu-id="970a0-444">Currently, the Create and Edit forms you have in place do not perform any kind of validation.</span></span> <span data-ttu-id="970a0-445">ユーザーの価格のフィールドに必要なフィールドを空白または型の文字が離れた場合、最初のエラーが表示されますが、データベースからになります。</span><span class="sxs-lookup"><span data-stu-id="970a0-445">If the user leaves a required field blank or type letters in the price field, the first error you will get will be from the database.</span></span>

<span data-ttu-id="970a0-446">アプリケーションに検証を追加するには、データの注釈モデル クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="970a0-446">You can add validation to the application by adding Data Annotations to your model class.</span></span> <span data-ttu-id="970a0-447">データ注釈は、モデルのプロパティに適用するルールを記述するを許可して、ASP.NET MVC が適用して、ユーザーに適切なメッセージを表示するが取得されます。</span><span class="sxs-lookup"><span data-stu-id="970a0-447">Data Annotations allow describing the rules you want applied to your model properties, and ASP.NET MVC will take care of enforcing and displaying appropriate message to users.</span></span>

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a><span data-ttu-id="970a0-448">タスク 1 - データ注釈の追加</span><span class="sxs-lookup"><span data-stu-id="970a0-448">Task 1 - Adding Data Annotations</span></span>

<span data-ttu-id="970a0-449">このタスクでは、Create と Edit ページと、アルバムのモデルにデータ注釈は、該当する場合に、検証メッセージを表示を追加します。</span><span class="sxs-lookup"><span data-stu-id="970a0-449">In this task, you will add Data Annotations to the Album Model that will make the Create and Edit page display validation messages when appropriate.</span></span>

<span data-ttu-id="970a0-450">単純なモデル クラスをだけを追加して処理されますデータ注釈を追加する、**を使用して**ステートメントの**System.ComponentModel.DataAnnotation**、配置、 **[必須]** 適切なプロパティの属性。</span><span class="sxs-lookup"><span data-stu-id="970a0-450">For a simple Model class, adding a Data Annotation is just handled by adding a **using** statement for **System.ComponentModel.DataAnnotation**, then placing a **[Required]** attribute on the appropriate properties.</span></span> <span data-ttu-id="970a0-451">次の例のように、**名前**プロパティ ビューでの必須フィールドです。</span><span class="sxs-lookup"><span data-stu-id="970a0-451">The following example would make the **Name** property a required field in the View.</span></span>

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

<span data-ttu-id="970a0-452">エンティティ データ モデルが生成されますこのアプリケーションのような場合、少し複雑です。</span><span class="sxs-lookup"><span data-stu-id="970a0-452">This is a little more complex in cases like this application where the Entity Data Model is generated.</span></span> <span data-ttu-id="970a0-453">モデル クラスに直接データ注釈を追加した場合、データベースからモデルを更新する場合、上書きが。</span><span class="sxs-lookup"><span data-stu-id="970a0-453">If you added Data Annotations directly to the model classes, they would be overwritten if you update the model from the database.</span></span> <span data-ttu-id="970a0-454">代わりに、行うことができますが、注釈を保持するために存在し、モデルに関連付けられているメタデータ部分クラスを使用してクラスを使用して、 **[MetadataType]** 属性。</span><span class="sxs-lookup"><span data-stu-id="970a0-454">Instead, you can make use of metadata partial classes which will exist to hold the annotations and are associated with the model classes using the **[MetadataType]** attribute.</span></span>

1. <span data-ttu-id="970a0-455">開く、**開始**ソリューションがある**ソース/Ex6-AddingValidation/開始/** フォルダー。</span><span class="sxs-lookup"><span data-stu-id="970a0-455">Open the **Begin** solution located at **Source/Ex6-AddingValidation/Begin/** folder.</span></span> <span data-ttu-id="970a0-456">使用を続ける可能性がありますそれ以外の場合、**エンド**ソリューションは、前の演習を完了して取得します。</span><span class="sxs-lookup"><span data-stu-id="970a0-456">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="970a0-457">指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。</span><span class="sxs-lookup"><span data-stu-id="970a0-457">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="970a0-458">これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-458">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="970a0-459">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。</span><span class="sxs-lookup"><span data-stu-id="970a0-459">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="970a0-460">最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-460">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="970a0-461">NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="970a0-461">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="970a0-462">With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="970a0-462">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="970a0-463">これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="970a0-463">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="970a0-464">開く、 **Album.cs**から、**モデル**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="970a0-464">Open the **Album.cs** from the **Models** folder.</span></span>
3. <span data-ttu-id="970a0-465">置換**Album.cs**次のように見えるように、強調表示されたコードを使用するコンテンツします。</span><span class="sxs-lookup"><span data-stu-id="970a0-465">Replace **Album.cs** content with the highlighted code, so that it looks like the following:</span></span>

    > [!NOTE]
    > <span data-ttu-id="970a0-466">行 **[DisplayFormat(ConvertEmptyStringToNull=false)]** データ ソースのデータ フィールドが更新されたときに、モデルから空の文字列を null に変換されないことを示します。</span><span class="sxs-lookup"><span data-stu-id="970a0-466">The line **[DisplayFormat(ConvertEmptyStringToNull=false)]** indicates that empty strings from the model won't be converted to null when the data field is updated in the data source.</span></span> <span data-ttu-id="970a0-467">Entity Framework は、データ注釈は、フィールドを検証する前にモデルに null 値を割り当てるときに、この設定は例外を回避します。</span><span class="sxs-lookup"><span data-stu-id="970a0-467">This setting will avoid an exception when the Entity Framework assigns null values to the model before Data Annotation validates the fields.</span></span>

    <span data-ttu-id="970a0-468">(コード スニペット -*と ASP.NET MVC 4 ヘルパー、フォームの検証 - Ex6 アルバム メタデータ部分クラスおよび*)</span><span class="sxs-lookup"><span data-stu-id="970a0-468">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex6 Album metadata partial class*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > <span data-ttu-id="970a0-469">これは、**アルバム**部分クラスには、 **MetadataType**属性を指す、 **AlbumMetaData**データ注釈クラス。</span><span class="sxs-lookup"><span data-stu-id="970a0-469">This **Album** partial class has a **MetadataType** attribute which points to the **AlbumMetaData** class for the Data Annotations.</span></span> <span data-ttu-id="970a0-470">これらは、アルバムのモデルの注釈を使用しているデータ注釈属性の一部を示します。</span><span class="sxs-lookup"><span data-stu-id="970a0-470">These are some of the Data Annotation attributes you are using to annotate the Album model:</span></span>
    > 
    > - <span data-ttu-id="970a0-471">必須 - プロパティが必須のフィールドであることを示します。</span><span class="sxs-lookup"><span data-stu-id="970a0-471">Required - Indicates that the property is a required field</span></span>
    > - <span data-ttu-id="970a0-472">DisplayName - は、フォーム フィールドと検証メッセージで使用するテキストを定義します。</span><span class="sxs-lookup"><span data-stu-id="970a0-472">DisplayName - Defines the text to be used on form fields and validation messages</span></span>
    > - <span data-ttu-id="970a0-473">DisplayFormat には、データ フィールドの表示し、書式設定する方法を指定します。</span><span class="sxs-lookup"><span data-stu-id="970a0-473">DisplayFormat - Specifies how data fields are displayed and formatted.</span></span>
    > - <span data-ttu-id="970a0-474">StringLength - は、文字列フィールドの最大長を定義します。</span><span class="sxs-lookup"><span data-stu-id="970a0-474">StringLength - Defines a maximum length for a string field</span></span>
    > - <span data-ttu-id="970a0-475">範囲 - 数値フィールドの最大値と最小値は、</span><span class="sxs-lookup"><span data-stu-id="970a0-475">Range - Gives a maximum and minimum value for a numeric field</span></span>
    > - <span data-ttu-id="970a0-476">ScaffoldColumn - により、エディターのフォームのフィールドを非表示</span><span class="sxs-lookup"><span data-stu-id="970a0-476">ScaffoldColumn - Allows hiding fields from editor forms</span></span>

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="970a0-477">タスク 2 - アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="970a0-477">Task 2 - Running the Application</span></span>

<span data-ttu-id="970a0-478">このタスクではテスト Create と Edit ページが、フィールドを検証することで最後のタスクを選択して表示名を使用します。</span><span class="sxs-lookup"><span data-stu-id="970a0-478">In this task, you will test that the Create and Edit pages validate fields, using the display names chosen in the last task.</span></span>

1. <span data-ttu-id="970a0-479">キーを押して**F5**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="970a0-479">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="970a0-480">ホーム ページで、プロジェクトが開始されます。</span><span class="sxs-lookup"><span data-stu-id="970a0-480">The project starts in the Home page.</span></span> <span data-ttu-id="970a0-481">URL を変更して**StoreManager/作成**です。</span><span class="sxs-lookup"><span data-stu-id="970a0-481">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="970a0-482">表示名が部分クラスにあるものと一致することを確認 (など**アルバム アート URL**の代わりに**AlbumArtUrl**)</span><span class="sxs-lookup"><span data-stu-id="970a0-482">Verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**)</span></span>
3. <span data-ttu-id="970a0-483">をクリックして**作成**フォームの入力なし。</span><span class="sxs-lookup"><span data-stu-id="970a0-483">Click **Create**, without filling the form.</span></span> <span data-ttu-id="970a0-484">対応する検証メッセージを取得することを確認します。</span><span class="sxs-lookup"><span data-stu-id="970a0-484">Verify that you get the corresponding validation messages.</span></span>

    <span data-ttu-id="970a0-485">![[作成] ページでフィールドを検証](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "の作成 ページでフィールドの検証")</span><span class="sxs-lookup"><span data-stu-id="970a0-485">![Validated fields in the Create page](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Validated fields in the Create page")</span></span>

    <span data-ttu-id="970a0-486">*[作成] ページで検証済みのフィールド*</span><span class="sxs-lookup"><span data-stu-id="970a0-486">*Validated fields in the Create page*</span></span>
4. <span data-ttu-id="970a0-487">発生する、同じことを確認することができます、**編集**ページ。</span><span class="sxs-lookup"><span data-stu-id="970a0-487">You can verify that the same occurs with the **Edit** page.</span></span> <span data-ttu-id="970a0-488">URL を変更して **/StoreManager/Edit/1**表示名が部分クラスにあるものと一致することを確認し、(など**アルバム アート URL**の代わりに**AlbumArtUrl**)。</span><span class="sxs-lookup"><span data-stu-id="970a0-488">Change the URL to **/StoreManager/Edit/1** and verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**).</span></span> <span data-ttu-id="970a0-489">空の**タイトル**と**価格**フィールドをクリックします**保存**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-489">Empty the **Title** and **Price** fields and click **Save**.</span></span> <span data-ttu-id="970a0-490">対応する検証メッセージを取得することを確認します。</span><span class="sxs-lookup"><span data-stu-id="970a0-490">Verify that you get the corresponding validation messages.</span></span>

    ![[編集] ページで検証済みのフィールド](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    <span data-ttu-id="970a0-492">*[編集] ページで検証済みのフィールド*</span><span class="sxs-lookup"><span data-stu-id="970a0-492">*Validated fields in the Edit page*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a><span data-ttu-id="970a0-493">手順 7: クライアント側での控えめな jQuery の使用</span><span class="sxs-lookup"><span data-stu-id="970a0-493">Exercise 7: Using Unobtrusive jQuery at Client Side</span></span>

<span data-ttu-id="970a0-494">この演習では、クライアント側での MVC 4 の控えめな jQuery 検証を有効にする方法を学びます。</span><span class="sxs-lookup"><span data-stu-id="970a0-494">In this exercise, you will learn how to enable MVC 4 Unobtrusive jQuery validation at client side.</span></span>

> [!NOTE]
> <span data-ttu-id="970a0-495">控えめな jQuery では、データ、ajax プレフィックス JavaScript を使用して、インライン クライアント スクリプトの出力をそのままの状態ではなく、サーバー上のアクション メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="970a0-495">The Unobtrusive jQuery uses data-ajax prefix JavaScript to invoke action methods on the server rather than intrusively emitting inline client scripts.</span></span>


<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a><span data-ttu-id="970a0-496">タスク 1 - jQuery Unobtrusive を有効にする前に、アプリケーションを実行しています。</span><span class="sxs-lookup"><span data-stu-id="970a0-496">Task 1 - Running the Application before Enabling Unobtrusive jQuery</span></span>

<span data-ttu-id="970a0-497">このタスクで検証の両方のモデルを比較するために jQuery を含める前にアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="970a0-497">In this task, you will run the application before including jQuery in order to compare both validation models.</span></span>

1. <span data-ttu-id="970a0-498">開く、**開始**ソリューションがある**ソース/Ex7-UnobtrusivejQueryValidation/開始/** フォルダー。</span><span class="sxs-lookup"><span data-stu-id="970a0-498">Open the **Begin** solution located at **Source/Ex7-UnobtrusivejQueryValidation/Begin/** folder.</span></span> <span data-ttu-id="970a0-499">使用を続ける可能性がありますそれ以外の場合、**エンド**ソリューションは、前の演習を完了して取得します。</span><span class="sxs-lookup"><span data-stu-id="970a0-499">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="970a0-500">指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。</span><span class="sxs-lookup"><span data-stu-id="970a0-500">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="970a0-501">これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-501">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="970a0-502">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。</span><span class="sxs-lookup"><span data-stu-id="970a0-502">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="970a0-503">最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-503">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="970a0-504">NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="970a0-504">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="970a0-505">With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="970a0-505">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="970a0-506">これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="970a0-506">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="970a0-507">**F5** キーを押してアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="970a0-507">Press **F5** to run the application.</span></span>
3. <span data-ttu-id="970a0-508">ホーム ページで、プロジェクトが開始されます。</span><span class="sxs-lookup"><span data-stu-id="970a0-508">The project starts in the Home page.</span></span> <span data-ttu-id="970a0-509">参照 **/StoreManager/作成** をクリック**作成**検証メッセージを取得することを確認するためのフォームの入力なし。</span><span class="sxs-lookup"><span data-stu-id="970a0-509">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="970a0-510">![クライアント検証を無効になっている](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "クライアント検証を無効になっています")</span><span class="sxs-lookup"><span data-stu-id="970a0-510">![Client validation disabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Client validation disabled")</span></span>

    <span data-ttu-id="970a0-511">*クライアント検証を無効になっています*</span><span class="sxs-lookup"><span data-stu-id="970a0-511">*Client validation disabled*</span></span>
4. <span data-ttu-id="970a0-512">ブラウザーで HTML のソース コードを開きます。</span><span class="sxs-lookup"><span data-stu-id="970a0-512">In the browser, open the HTML source code:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a><span data-ttu-id="970a0-513">タスク 2 - 控えめなクライアント検証を有効にします。</span><span class="sxs-lookup"><span data-stu-id="970a0-513">Task 2 - Enabling Unobtrusive Client Validation</span></span>

<span data-ttu-id="970a0-514">このタスクでは、jQuery を有効に**控えめなクライアント検証**から**Web.config**ファイル。 これは既定ではすべての新しい ASP.NET MVC 4 プロジェクトで false に設定します。</span><span class="sxs-lookup"><span data-stu-id="970a0-514">In this task, you will enable jQuery **unobtrusive client validation** from **Web.config** file, which is by default set to false in all new ASP.NET MVC 4 projects.</span></span> <span data-ttu-id="970a0-515">さらに、必要なスクリプト jQuery Unobtrusive Validation にクライアントの作業をするため、参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="970a0-515">Additionally, you will add the necessary scripts references to make jQuery Unobtrusive Client Validation work.</span></span>

1. <span data-ttu-id="970a0-516">開いている**Web.Config** 、プロジェクトのルートにファイルを確認して、 **ClientValidationEnabled**と**UnobtrusiveJavaScriptEnabled** にキー値が設定されて**true**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-516">Open **Web.Config** file at project root, and make sure that the **ClientValidationEnabled** and **UnobtrusiveJavaScriptEnabled** keys values are set to **true**.</span></span>

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > <span data-ttu-id="970a0-517">クライアント検証を有効に同じ結果を得るための Global.asax.cs でコードによってもできます。</span><span class="sxs-lookup"><span data-stu-id="970a0-517">You can also enable client validation by code at Global.asax.cs to get the same results:</span></span>
    > 
    > <span data-ttu-id="970a0-518">**HtmlHelper.ClientValidationEnabled = true;**</span><span class="sxs-lookup"><span data-stu-id="970a0-518">**HtmlHelper.ClientValidationEnabled = true;**</span></span>
    > 
    > <span data-ttu-id="970a0-519">さらに、カスタム動作を任意のコント ローラーに ClientValidationEnabled 属性を割り当てることができます。</span><span class="sxs-lookup"><span data-stu-id="970a0-519">Additionally, you can assign ClientValidationEnabled attribute into any controller to have a custom behavior.</span></span>
2. <span data-ttu-id="970a0-520">開いている**Create.cshtml**で**Views\StoreManager**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-520">Open **Create.cshtml** at **Views\StoreManager**.</span></span>
3. <span data-ttu-id="970a0-521">次のスクリプト ファイルを必ず**jquery.validate**と**jquery.validate.unobtrusive**、システム ビューのレジストリ設定で参照される、 &quot; **~/bundles/jqueryval**&quot;バンドルします。</span><span class="sxs-lookup"><span data-stu-id="970a0-521">Make sure the following script files, **jquery.validate** and **jquery.validate.unobtrusive**, are referenced in the view throught the &quot;**~/bundles/jqueryval**&quot; bundle.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="970a0-522">MVC 4 の新しいプロジェクトでは、これらすべての jQuery ライブラリが含まれます。</span><span class="sxs-lookup"><span data-stu-id="970a0-522">All these jQuery libraries are included in MVC 4 new projects.</span></span> <span data-ttu-id="970a0-523">より多くのライブラリを見つけることができます、 **スクリプト/** するプロジェクトのフォルダー。</span><span class="sxs-lookup"><span data-stu-id="970a0-523">You can find more libraries in the **/Scripts** folder of you project.</span></span>
    > 
    > <span data-ttu-id="970a0-524">この検証を行うためにライブラリの機能は、jQuery ライブラリへの参照を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="970a0-524">In order to make this validation libraries work, you need to add a reference to the jQuery framework library.</span></span> <span data-ttu-id="970a0-525">この参照が既に追加されているため、  **\_Layout.cshtml**ファイル必要はありません、このビューで追加します。</span><span class="sxs-lookup"><span data-stu-id="970a0-525">Since this reference is already added in the **\_Layout.cshtml** file, you do not need to add it in this particular view.</span></span>

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a><span data-ttu-id="970a0-526">タスク 3 - アプリケーションを使用した控えめな jQuery 検証を実行しています。</span><span class="sxs-lookup"><span data-stu-id="970a0-526">Task 3 - Running the Application Using Unobtrusive jQuery Validation</span></span>

<span data-ttu-id="970a0-527">このタスクではテストを**StoreManager**テンプレートは、ユーザーが新しいアルバムを作成するときに、jQuery ライブラリを使用してクライアント側の検証を実行します。 ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="970a0-527">In this task, you will test that the **StoreManager** create view template performs client side validation using jQuery libraries when the user creates a new album.</span></span>

1. <span data-ttu-id="970a0-528">**F5** キーを押してアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="970a0-528">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="970a0-529">ホーム ページで、プロジェクトが開始されます。</span><span class="sxs-lookup"><span data-stu-id="970a0-529">The project starts in the Home page.</span></span> <span data-ttu-id="970a0-530">参照 **/StoreManager/作成** をクリック**作成**検証メッセージを取得することを確認するためのフォームの入力なし。</span><span class="sxs-lookup"><span data-stu-id="970a0-530">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="970a0-531">![クライアント検証を有効になっている jQuery](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "有効になっている jQuery を使用したクライアントの検証")</span><span class="sxs-lookup"><span data-stu-id="970a0-531">![Client validation with jQuery enabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Client validation with jQuery enabled")</span></span>

    <span data-ttu-id="970a0-532">*有効になっている jQuery を使用したクライアントの検証*</span><span class="sxs-lookup"><span data-stu-id="970a0-532">*Client validation with jQuery enabled*</span></span>
3. <span data-ttu-id="970a0-533">ブラウザーで、作成ビューのソース コードを開きます。</span><span class="sxs-lookup"><span data-stu-id="970a0-533">In the browser, open the source code for Create view:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

   > [!NOTE]
   > <span data-ttu-id="970a0-534">各クライアント検証規則の控えめな jQuery はデータを使用して属性を追加します-val -*rulename*=&quot;*メッセージ*&quot;します。</span><span class="sxs-lookup"><span data-stu-id="970a0-534">For each client validation rule, Unobtrusive jQuery adds an attribute with data-val-*rulename*=&quot;*message*&quot;.</span></span> <span data-ttu-id="970a0-535">以下は、タグのリストをその Unobtrusive jQuery クライアント検証を実行する html 入力フィールドに挿入されます。</span><span class="sxs-lookup"><span data-stu-id="970a0-535">Below is a list of tags that Unobtrusive jQuery inserts into the html input field to perform client validation:</span></span>
   > 
   > - <span data-ttu-id="970a0-536">データ値</span><span class="sxs-lookup"><span data-stu-id="970a0-536">Data-val</span></span>
   > - <span data-ttu-id="970a0-537">データ val-番号</span><span class="sxs-lookup"><span data-stu-id="970a0-537">Data-val-number</span></span>
   > - <span data-ttu-id="970a0-538">データ値の範囲</span><span class="sxs-lookup"><span data-stu-id="970a0-538">Data-val-range</span></span>
   > - <span data-ttu-id="970a0-539">データ val 範囲 min/データ val range max</span><span class="sxs-lookup"><span data-stu-id="970a0-539">Data-val-range-min / Data-val-range-max</span></span>
   > - <span data-ttu-id="970a0-540">Val-必要なデータ</span><span class="sxs-lookup"><span data-stu-id="970a0-540">Data-val-required</span></span>
   > - <span data-ttu-id="970a0-541">データの長さ val</span><span class="sxs-lookup"><span data-stu-id="970a0-541">Data-val-length</span></span>
   > - <span data-ttu-id="970a0-542">データ値の長さ最大/データ val 長さ分</span><span class="sxs-lookup"><span data-stu-id="970a0-542">Data-val-length-max / Data-val-length-min</span></span>
   > 
   > <span data-ttu-id="970a0-543">すべてのデータ値がモデルで塗りつぶされます**データ注釈**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-543">All the data values are filled with model **Data Annotation**.</span></span> <span data-ttu-id="970a0-544">次に、サーバー側で動作するすべてのロジックは、クライアント側で実行できます。</span><span class="sxs-lookup"><span data-stu-id="970a0-544">Then, all the logic that works at server side can be run at client side.</span></span> <span data-ttu-id="970a0-545">たとえば、Price 属性では、モデルに、次のデータ注釈がいます。</span><span class="sxs-lookup"><span data-stu-id="970a0-545">For example, Price attribute has the following data annotation in the model:</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
   > 
   > <span data-ttu-id="970a0-546">控えめな jQuery を使用すると、生成されたコードは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="970a0-546">After using Unobtrusive jQuery, the generated code is:</span></span>
   > 
   > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="970a0-547">まとめ</span><span class="sxs-lookup"><span data-stu-id="970a0-547">Summary</span></span>

<span data-ttu-id="970a0-548">このハンズオン ラボは、次の使用量、データベースに格納されているデータを変更するユーザーを有効にする方法について説明しました。</span><span class="sxs-lookup"><span data-stu-id="970a0-548">By completing this Hands-On Lab you have learned how to enable users to change the data stored in the database with the use of the following:</span></span>

- <span data-ttu-id="970a0-549">コント ローラー アクションなどのインデックスを作成、編集、削除</span><span class="sxs-lookup"><span data-stu-id="970a0-549">Controller actions like Index, Create, Edit, Delete</span></span>
- <span data-ttu-id="970a0-550">HTML テーブルのプロパティを表示するための ASP.NET MVC のスキャフォールディング機能</span><span class="sxs-lookup"><span data-stu-id="970a0-550">ASP.NET MVC's scaffolding feature for displaying properties in an HTML table</span></span>
- <span data-ttu-id="970a0-551">ユーザーを向上させるためにカスタム HTML ヘルパーをエクスペリエンスします。</span><span class="sxs-lookup"><span data-stu-id="970a0-551">Custom HTML helpers to improve user experience</span></span>
- <span data-ttu-id="970a0-552">HTTP GET または HTTP POST 呼び出しのいずれかに対応するアクション メソッド</span><span class="sxs-lookup"><span data-stu-id="970a0-552">Action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="970a0-553">Create と Edit などの同様のビュー テンプレートの共有エディター テンプレート</span><span class="sxs-lookup"><span data-stu-id="970a0-553">A shared editor template for similar View templates like Create and Edit</span></span>
- <span data-ttu-id="970a0-554">ドロップダウン リスト フォームの要素</span><span class="sxs-lookup"><span data-stu-id="970a0-554">Form elements like drop-downs</span></span>
- <span data-ttu-id="970a0-555">モデル検証のためのデータ注釈</span><span class="sxs-lookup"><span data-stu-id="970a0-555">Data annotations for Model validation</span></span>
- <span data-ttu-id="970a0-556">JQuery Unobtrusive ライブラリを使用してクライアント側の検証</span><span class="sxs-lookup"><span data-stu-id="970a0-556">Client Side Validation using jQuery Unobtrusive library</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="970a0-557">付録 a: Installing Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="970a0-557">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="970a0-558">インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、 **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="970a0-558">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="970a0-559">次の手順をインストールするために必要な手順をガイドします。 *Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*します。</span><span class="sxs-lookup"><span data-stu-id="970a0-559">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="970a0-560">移動して[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)します。</span><span class="sxs-lookup"><span data-stu-id="970a0-560">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="970a0-561">または、既に Web Platform Installer をインストールした場合を開くことも、製品を検索して、 &quot; <em>Visual Studio Express 2012 for Web と Windows Azure SDK</em>&quot;します。</span><span class="sxs-lookup"><span data-stu-id="970a0-561">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="970a0-562">をクリックして**を今すぐインストール**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-562">Click on **Install Now**.</span></span> <span data-ttu-id="970a0-563">ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="970a0-563">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="970a0-564">1 回**Web Platform Installer**を開くと、クリックして**インストール**セットアップを開始します。</span><span class="sxs-lookup"><span data-stu-id="970a0-564">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="970a0-565">![Visual Studio Express のインストール](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Visual Studio Express のインストール")</span><span class="sxs-lookup"><span data-stu-id="970a0-565">![Install Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="970a0-566">*Visual Studio Express をインストールします。*</span><span class="sxs-lookup"><span data-stu-id="970a0-566">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="970a0-567">すべての製品のライセンスと使用条件を読み、クリックして**同意**を続行します。</span><span class="sxs-lookup"><span data-stu-id="970a0-567">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![ライセンス条項に同意](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    <span data-ttu-id="970a0-569">*ライセンス条項に同意*</span><span class="sxs-lookup"><span data-stu-id="970a0-569">*Accepting the license terms*</span></span>
5. <span data-ttu-id="970a0-570">ダウンロードとインストール プロセスが完了するまで待機します。</span><span class="sxs-lookup"><span data-stu-id="970a0-570">Wait until the downloading and installation process completes.</span></span>

    ![インストールの進行状況](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    <span data-ttu-id="970a0-572">*インストールの進行状況*</span><span class="sxs-lookup"><span data-stu-id="970a0-572">*Installation progress*</span></span>
6. <span data-ttu-id="970a0-573">インストールが完了したら、クリックして**完了**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-573">When the installation completes, click **Finish**.</span></span>

    ![インストールが完了しました](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    <span data-ttu-id="970a0-575">*インストールが完了しました*</span><span class="sxs-lookup"><span data-stu-id="970a0-575">*Installation completed*</span></span>
7. <span data-ttu-id="970a0-576">クリックして**終了**Web Platform Installer を閉じます。</span><span class="sxs-lookup"><span data-stu-id="970a0-576">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="970a0-577">Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、 をクリックし、 **VS Express for Web**並べて表示します。</span><span class="sxs-lookup"><span data-stu-id="970a0-577">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web のタイル](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    <span data-ttu-id="970a0-579">*VS Express for Web のタイル*</span><span class="sxs-lookup"><span data-stu-id="970a0-579">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="970a0-580">付録 b: コード スニペットの使用</span><span class="sxs-lookup"><span data-stu-id="970a0-580">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="970a0-581">コードのスニペットでは、指先ひとつで必要なすべてのコードがあります。</span><span class="sxs-lookup"><span data-stu-id="970a0-581">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="970a0-582">ラボ ドキュメントがわかりますだけをいつ使用できる、次の図に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="970a0-582">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="970a0-583">![Visual Studio コード スニペットを使用して、プロジェクトにコードを挿入する](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "プロジェクトにコードを挿入するコード スニペットを Visual Studio の使用")</span><span class="sxs-lookup"><span data-stu-id="970a0-583">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="970a0-584">*Visual Studio コード スニペットを使用して、プロジェクトにコードを挿入するには*</span><span class="sxs-lookup"><span data-stu-id="970a0-584">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="970a0-585">***キーボード (c# のみ) を使用するコード スニペットを追加するには***</span><span class="sxs-lookup"><span data-stu-id="970a0-585">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="970a0-586">コードを挿入するには、カーソルを置きます。</span><span class="sxs-lookup"><span data-stu-id="970a0-586">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="970a0-587">スニペットの名前 (なし、スペースやハイフン) の入力を開始します。</span><span class="sxs-lookup"><span data-stu-id="970a0-587">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="970a0-588">スニペットの名前に一致する IntelliSense の表示を確認します。</span><span class="sxs-lookup"><span data-stu-id="970a0-588">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="970a0-589">適切なスニペットを選択します (または全体のスニペットの名前が選択されるまで」と入力してください)。</span><span class="sxs-lookup"><span data-stu-id="970a0-589">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="970a0-590">カーソル位置にスニペットを挿入するには、2 回、Tab キーを押します。</span><span class="sxs-lookup"><span data-stu-id="970a0-590">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="970a0-591">![スニペットの名前の入力を開始](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "スニペット名の入力を開始")</span><span class="sxs-lookup"><span data-stu-id="970a0-591">![Start typing the snippet name](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="970a0-592">*スニペットの名前の入力を開始します。*</span><span class="sxs-lookup"><span data-stu-id="970a0-592">*Start typing the snippet name*</span></span>

<span data-ttu-id="970a0-593">![強調表示されているスニペットを選択して Tab キーを押して](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "キーを押してタブが強調表示されているスニペットを選択するには")</span><span class="sxs-lookup"><span data-stu-id="970a0-593">![Press Tab to select the highlighted snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="970a0-594">*Tab キーを押して、強調表示されているスニペットを選択します*</span><span class="sxs-lookup"><span data-stu-id="970a0-594">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="970a0-595">![キーを押して タブで再度とスニペットが展開](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "キーを押して タブで再度とスニペットが展開されます")</span><span class="sxs-lookup"><span data-stu-id="970a0-595">![Press Tab again and the snippet will expand](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="970a0-596">*キーを押して タブで再度とスニペットが展開されます。*</span><span class="sxs-lookup"><span data-stu-id="970a0-596">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="970a0-597">***(C#、Visual Basic および XML) にマウスを使用するコード スニペットを追加する***1。</span><span class="sxs-lookup"><span data-stu-id="970a0-597">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="970a0-598">コード スニペットを挿入するを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="970a0-598">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="970a0-599">選択**スニペットの挿入**続けて**マイ コード スニペット**します。</span><span class="sxs-lookup"><span data-stu-id="970a0-599">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="970a0-600">クリックして、一覧から関連するスニペットを選択します。</span><span class="sxs-lookup"><span data-stu-id="970a0-600">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="970a0-601">![コード スニペットを挿入し、スニペットの挿入を選択する場所を右クリックして](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "コード スニペットを挿入し、スニペットの挿入を選択する場所を右クリック")</span><span class="sxs-lookup"><span data-stu-id="970a0-601">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="970a0-602">*コード スニペットを挿入して、スニペットの挿入先の選択します。*</span><span class="sxs-lookup"><span data-stu-id="970a0-602">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="970a0-603">![クリックして、一覧から関連するスニペットを選択](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "クリックして、一覧から関連するスニペットを選択")</span><span class="sxs-lookup"><span data-stu-id="970a0-603">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="970a0-604">*クリックして、一覧から関連するスニペットを選択します。*</span><span class="sxs-lookup"><span data-stu-id="970a0-604">*Pick the relevant snippet from the list, by clicking on it*</span></span>
