---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: "ASP.NET MVC 4 エンティティ フレームワークのスキャフォールディングと移行 |Microsoft ドキュメント"
author: rick-anderson
description: "ASP.NET MVC 4 コント ローラーのメソッドに慣れているか、完了したかどうか、&quot;ヘルパー、フォーム、および検証&quot;ハンズオン ラボは、注意してください."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 15db1589eb90739458b430c35cea38e93e3dec5a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a><span data-ttu-id="c2e34-103">ASP.NET MVC 4 エンティティ フレームワークのスキャフォールディングと移行</span><span class="sxs-lookup"><span data-stu-id="c2e34-103">ASP.NET MVC 4 Entity Framework Scaffolding and Migrations</span></span>
====================
<span data-ttu-id="c2e34-104">によって[Web キャンプ チーム](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="c2e34-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="c2e34-105">ASP.NET MVC 4 コント ローラーのメソッドに慣れているか、完了したかどうか、&quot;ヘルパー、フォーム、および検証&quot;ハンズオン ラボは、注意すべきことを作成するためのロジックの多くの更新、ボックスの一覧およびが繰り返される任意のデータ エンティティを削除します。間でのアプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="c2e34-105">If you are familiar with ASP.NET MVC 4 controller methods, or have completed the &quot;Helpers, Forms and Validation&quot; Hands-On lab, you should be aware that many of the logic to create, update, list and remove any data entity it is repeated among the application.</span></span> <span data-ttu-id="c2e34-106">モデルにいくつかのクラスを操作する場合はもちろんのことの各エンティティ操作だけでなく、ビューの各アクション メソッド POST および GET を書き込み、かなりの時間を費やす可能性がありますができます。</span><span class="sxs-lookup"><span data-stu-id="c2e34-106">Not to mention that, if your model has several classes to manipulate, you will be likely to spend a considerable time writing the POST and GET action methods for each entity operation, as well as each of the views.</span></span>
> 
> <span data-ttu-id="c2e34-107">このラボでは、ASP.NET MVC 4 のスキャフォールディングを使用して、アプリケーションの CRUD (作成、読み取り、更新、削除) のベースラインを自動的に生成する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="c2e34-107">In this lab you will learn how to use the ASP.NET MVC 4 scaffolding to automatically generate the baseline of your application's CRUD (Create, Read, Update and Delete).</span></span> <span data-ttu-id="c2e34-108">以降は、単純なモデル クラス、および、1 行のコードを記述することがなく、すべての CRUD 操作だけでなく、すべての必要なビューを格納するコント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="c2e34-108">Starting from a simple model class, and, without writing a single line of code, you will create a controller that will contain all the CRUD operations, as well as the all the necessary views.</span></span> <span data-ttu-id="c2e34-109">構築し、単純なソリューションを実行すると、生成されると、MVC ロジックとデータ操作にビューと共に、アプリケーション データベースがあります。</span><span class="sxs-lookup"><span data-stu-id="c2e34-109">After building and running the simple solution, you will have the application database generated, together with the MVC logic and views for data manipulation.</span></span>
> 
> <span data-ttu-id="c2e34-110">さらに、Entity Framework の移行を使用して、アプリケーション全体のモデルの更新を実行するは簡単な方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="c2e34-110">In addition, you will learn how easy it is to use Entity Framework Migrations to perform model updates throughout your entire application.</span></span> <span data-ttu-id="c2e34-111">Entity Framework の移行を使用すると、簡単な手順と、モデルが変更された後に、データベースを変更できます。</span><span class="sxs-lookup"><span data-stu-id="c2e34-111">Entity Framework Migrations will let you modify your database after the model has changed with simple steps.</span></span> <span data-ttu-id="c2e34-112">これらすべての点に注意、ことができますをビルドして、web アプリケーションをより効率的に管理する ASP.NET MVC 4 の最新の機能を活用します。</span><span class="sxs-lookup"><span data-stu-id="c2e34-112">With all these in mind, you will be able to build and maintain web applications more efficiently, taking advantage of the latest features of ASP.NET MVC 4.</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="c2e34-113">目的</span><span class="sxs-lookup"><span data-stu-id="c2e34-113">Objectives</span></span>

<span data-ttu-id="c2e34-114">このハンズオン ラボでは学習する方法。</span><span class="sxs-lookup"><span data-stu-id="c2e34-114">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="c2e34-115">コント ローラーでの CRUD 操作のためには、ASP.NET のスキャフォールディングを使用します。</span><span class="sxs-lookup"><span data-stu-id="c2e34-115">Use ASP.NET scaffolding for CRUD operations in controllers.</span></span>
- <span data-ttu-id="c2e34-116">Entity Framework Migrations を使用して、データベース モデルを変更します。</span><span class="sxs-lookup"><span data-stu-id="c2e34-116">Change the database model using Entity Framework Migrations.</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="c2e34-117">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="c2e34-117">Prerequisites</span></span>

<span data-ttu-id="c2e34-118">このラボを完成させるのには、次の項目が必要です。</span><span class="sxs-lookup"><span data-stu-id="c2e34-118">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="c2e34-119">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)または上位 (読み取り[付録 A](#AppendixA)をインストールする方法について)。</span><span class="sxs-lookup"><span data-stu-id="c2e34-119">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="c2e34-120">セットアップ</span><span class="sxs-lookup"><span data-stu-id="c2e34-120">Setup</span></span>

<span data-ttu-id="c2e34-121">**コード スニペットをインストールします。**</span><span class="sxs-lookup"><span data-stu-id="c2e34-121">**Installing Code Snippets**</span></span>

<span data-ttu-id="c2e34-122">便宜上、このラボに沿ったを管理するコードの多くは、Visual Studio のコード スニペットとして利用できます。</span><span class="sxs-lookup"><span data-stu-id="c2e34-122">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="c2e34-123">実行のコード スニペットをインストールする**.\Source\Setup\CodeSnippets.vsi**ファイル。</span><span class="sxs-lookup"><span data-stu-id="c2e34-123">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="c2e34-124">このドキュメントの付録を参照することができます、Visual Studio のコード スニペットとその使用方法を学習するに慣れていない場合&quot;[付録 b: を使用してコード スニペット](#AppendixB)&quot;です。</span><span class="sxs-lookup"><span data-stu-id="c2e34-124">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="c2e34-125">演習</span><span class="sxs-lookup"><span data-stu-id="c2e34-125">Exercises</span></span>

<span data-ttu-id="c2e34-126">次の手順は、このハンズオン ラボを構成します。</span><span class="sxs-lookup"><span data-stu-id="c2e34-126">The following exercise make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="c2e34-127">Entity Framework の移行で ASP.NET MVC 4 のスキャフォールディングを使用します。</span><span class="sxs-lookup"><span data-stu-id="c2e34-127">Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>](#Exercise1)

> [!NOTE]
> <span data-ttu-id="c2e34-128">この練習が組み込まれた、**終了**演習を完了した後に取得する必要があります、結果として得られるソリューションを含むフォルダー。</span><span class="sxs-lookup"><span data-stu-id="c2e34-128">This exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercise.</span></span> <span data-ttu-id="c2e34-129">演習によって、作業のヘルプを参照する必要がある場合は、このソリューションをガイドとして使用できます。</span><span class="sxs-lookup"><span data-stu-id="c2e34-129">You can use this solution as a guide if you need additional help working through the exercise.</span></span>


<span data-ttu-id="c2e34-130">この演習を完了する時間を推定: **30 分**</span><span class="sxs-lookup"><span data-stu-id="c2e34-130">Estimated time to complete this lab: **30 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a><span data-ttu-id="c2e34-131">手順 1: Entity Framework の移行で ASP.NET MVC 4 のスキャフォールディングを使用します。</span><span class="sxs-lookup"><span data-stu-id="c2e34-131">Exercise 1: Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>

<span data-ttu-id="c2e34-132">ASP.NET MVC のスキャフォールディングでは、により、アプリケーションは、データベース層と対話するために必要なロジックを作成する標準化された方法での CRUD 操作を生成する簡単な方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="c2e34-132">ASP.NET MVC scaffolding provides a quick way to generate the CRUD operations in a standardized way, creating the necessary logic that lets your application interact with the database layer.</span></span>

<span data-ttu-id="c2e34-133">この演習では、CRUD メソッドを作成する最初のコードで ASP.NET MVC 4 のスキャフォールディングを使用する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="c2e34-133">In this exercise, you will learn how to use ASP.NET MVC 4 scaffolding with code first to create the CRUD methods.</span></span> <span data-ttu-id="c2e34-134">次に、Entity Framework の移行を使用して、データベース内の変更を適用するモデルを更新する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="c2e34-134">Then, you will learn how to update your model applying the changes in the database by using Entity Framework Migrations.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a><span data-ttu-id="c2e34-135">スキャフォールディングを使用してタスク 1 で作成する新しい ASP.NET MVC 4 プロジェクト</span><span class="sxs-lookup"><span data-stu-id="c2e34-135">Task 1- Creating a new ASP.NET MVC 4 project using Scaffolding</span></span>

1. <span data-ttu-id="c2e34-136">まだ開いていない場合は開始**Visual Studio 2012**です。</span><span class="sxs-lookup"><span data-stu-id="c2e34-136">If not already open, start **Visual Studio 2012**.</span></span>
2. <span data-ttu-id="c2e34-137">選択**ファイル |新しいプロジェクト**です。</span><span class="sxs-lookup"><span data-stu-id="c2e34-137">Select **File | New Project**.</span></span> <span data-ttu-id="c2e34-138">[新規プロジェクト] ダイアログで、 **Visual c# |Web**セクションで、 **ASP.NET MVC 4 Web Application**です。</span><span class="sxs-lookup"><span data-stu-id="c2e34-138">In the New Project dialog, under the **Visual C# | Web** section, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="c2e34-139">プロジェクトに名前を**MVC4andEFMigrations**の場所を設定および**Source\Ex1 UsingMVC4ScaffoldingEFMigrations**このラボのフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="c2e34-139">Name the project to **MVC4andEFMigrations** and set the location to **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** folder of this lab.</span></span> <span data-ttu-id="c2e34-140">設定、**ソリューション名**に**開始**を確認してください**ソリューションのディレクトリを作成**がオンになっています。</span><span class="sxs-lookup"><span data-stu-id="c2e34-140">Set the **Solution name** to **Begin** and ensure **Create directory for solution** is checked.</span></span> <span data-ttu-id="c2e34-141">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="c2e34-141">Click **OK**.</span></span>

    <span data-ttu-id="c2e34-142">![新しい ASP.NET MVC 4 プロジェクト ダイアログ ボックス](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "新しい ASP.NET MVC 4 プロジェクト ダイアログ ボックス")</span><span class="sxs-lookup"><span data-stu-id="c2e34-142">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="c2e34-143">*新しい ASP.NET MVC 4 プロジェクト ダイアログ ボックス*</span><span class="sxs-lookup"><span data-stu-id="c2e34-143">*New ASP.NET MVC 4 Project Dialog Box*</span></span>
3. <span data-ttu-id="c2e34-144">**新しい ASP.NET MVC 4 プロジェクト**ダイアログ ボックスの 、**インターネット アプリケーション**テンプレート、ことを確認および**Razor**は、選択した**ビュー エンジン**.</span><span class="sxs-lookup"><span data-stu-id="c2e34-144">In the **New ASP.NET MVC 4 Project** dialog box select the **Internet Application** template, and make sure that **Razor** is the selected **View engine**.</span></span> <span data-ttu-id="c2e34-145">をクリックして**OK**プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="c2e34-145">Click **OK** to create the project.</span></span>

    <span data-ttu-id="c2e34-146">![新しい ASP.NET MVC 4 のインターネット アプリケーション](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "新しい ASP.NET MVC 4 のインターネット アプリケーション")</span><span class="sxs-lookup"><span data-stu-id="c2e34-146">![New ASP.NET MVC 4 Internet Application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "New ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="c2e34-147">*新しい ASP.NET MVC 4 のインターネット アプリケーション*</span><span class="sxs-lookup"><span data-stu-id="c2e34-147">*New ASP.NET MVC 4 Internet Application*</span></span>
4. <span data-ttu-id="c2e34-148">ソリューション エクスプ ローラーで右クリック**モデル**選択**追加 |クラス**を単純なクラス人 (POCO) を作成します。</span><span class="sxs-lookup"><span data-stu-id="c2e34-148">In the Solution Explorer, right-click **Models** and select **Add | Class** to create a simple class person (POCO).</span></span> <span data-ttu-id="c2e34-149">名前を付けます**Person**  をクリック**OK**です。</span><span class="sxs-lookup"><span data-stu-id="c2e34-149">Name it **Person** and click **OK**.</span></span>
5. <span data-ttu-id="c2e34-150">ユーザー クラスを開き、次のプロパティを挿入します。</span><span class="sxs-lookup"><span data-stu-id="c2e34-150">Open the Person class and insert the following properties.</span></span>

    <span data-ttu-id="c2e34-151">(コード スニペットの*ASP.NET MVC 4 および Entity Framework の移行 - Ex1 Person プロパティ*)</span><span class="sxs-lookup"><span data-stu-id="c2e34-151">(Code Snippet - *ASP.NET MVC 4 and Entity Framework Migrations - Ex1 Person Properties*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. <span data-ttu-id="c2e34-152">をクリックして**ビルド |ソリューションをビルド**変更を保存し、プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="c2e34-152">Click **Build | Build Solution** to save the changes and build the project.</span></span>

    <span data-ttu-id="c2e34-153">![アプリケーションのビルド](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "アプリケーションのビルド")</span><span class="sxs-lookup"><span data-stu-id="c2e34-153">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Building the application")</span></span>

    <span data-ttu-id="c2e34-154">*アプリケーションのビルド*</span><span class="sxs-lookup"><span data-stu-id="c2e34-154">*Building the Application*</span></span>
7. <span data-ttu-id="c2e34-155">ソリューション エクスプ ローラーで、controllers フォルダーを右クリックして**追加 |コント ローラー**です。</span><span class="sxs-lookup"><span data-stu-id="c2e34-155">In the Solution Explorer, right-click the controllers folder and select **Add | Controller**.</span></span>
8. <span data-ttu-id="c2e34-156">コント ローラーの名前を付けます*PersonController*を完了し、**スキャフォールディング オプション**次の値。</span><span class="sxs-lookup"><span data-stu-id="c2e34-156">Name the controller *PersonController* and complete the **Scaffolding options** with the following values.</span></span>

    1. <span data-ttu-id="c2e34-157">**テンプレート**ドロップダウン リストで、**読み取り/書き込みアクションと Entity Framework を使用して、ビューの MVC コント ローラー**オプション。</span><span class="sxs-lookup"><span data-stu-id="c2e34-157">In the **Template** drop-down list, select the **MVC controller with read/write actions and views, using Entity Framework** option.</span></span>
    2. <span data-ttu-id="c2e34-158">**モデル クラス**ドロップダウン リストで、 **Person**クラスです。</span><span class="sxs-lookup"><span data-stu-id="c2e34-158">In the **Model class** drop-down list, select the **Person** class.</span></span>
    3. <span data-ttu-id="c2e34-159">**データ コンテキスト クラス**一覧で、 **&lt;新しいデータ コンテキストしています.&gt;**.</span><span class="sxs-lookup"><span data-stu-id="c2e34-159">In the **Data Context class** list, select **&lt;New data context...&gt;**.</span></span> <span data-ttu-id="c2e34-160">任意の名前を選択し、クリックして**OK**です。</span><span class="sxs-lookup"><span data-stu-id="c2e34-160">Choose any name and click **OK**.</span></span>
    4. <span data-ttu-id="c2e34-161">**ビュー**ドロップダウン リストで、ことを確認して**Razor**が選択されています。</span><span class="sxs-lookup"><span data-stu-id="c2e34-161">In the **Views** drop-down list, make sure that **Razor** is selected.</span></span>

    <span data-ttu-id="c2e34-162">![スキャフォールディングがある人コント ローラーを追加する](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Person コント ローラーのスキャフォールディングを追加します。")</span><span class="sxs-lookup"><span data-stu-id="c2e34-162">![Adding the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Adding the Person controller with scaffolding")</span></span>

    <span data-ttu-id="c2e34-163">*スキャフォールディングがある人コント ローラーを追加します。*</span><span class="sxs-lookup"><span data-stu-id="c2e34-163">*Adding the Person controller with scaffolding*</span></span>
9. <span data-ttu-id="c2e34-164">をクリックして**追加**スキャフォールディングとユーザーを新しいコント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="c2e34-164">Click **Add** to create the new controller for Person with scaffolding.</span></span> <span data-ttu-id="c2e34-165">コント ローラーのアクションだけでなく、ビューを生成したようになりました。</span><span class="sxs-lookup"><span data-stu-id="c2e34-165">You have now generated the controller actions as well as the views.</span></span>

    <span data-ttu-id="c2e34-166">![スキャフォールディングを含む Person コント ローラーを作成したら](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "スキャフォールディングで人コント ローラーを作成した後")</span><span class="sxs-lookup"><span data-stu-id="c2e34-166">![After creating the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "After creating the Person controller with scaffolding")</span></span>

    <span data-ttu-id="c2e34-167">*スキャフォールディングで人コント ローラーを作成した後*</span><span class="sxs-lookup"><span data-stu-id="c2e34-167">*After creating the Person controller with scaffolding*</span></span>
10. <span data-ttu-id="c2e34-168">開いている**PersonController**クラスです。</span><span class="sxs-lookup"><span data-stu-id="c2e34-168">Open **PersonController** class.</span></span> <span data-ttu-id="c2e34-169">完全 CRUD アクション メソッドが自動的に生成されたことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="c2e34-169">Notice that the full CRUD action methods have been generated automatically.</span></span>

    <span data-ttu-id="c2e34-170">![Person コント ローラー内](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "ユーザーの内部コント ローラー")</span><span class="sxs-lookup"><span data-stu-id="c2e34-170">![Inside the Person controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "Inside the Person controller")</span></span>

    <span data-ttu-id="c2e34-171">*Person コント ローラー内*</span><span class="sxs-lookup"><span data-stu-id="c2e34-171">*Inside the Person controller*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a><span data-ttu-id="c2e34-172">タスク 2-実行中のアプリケーション</span><span class="sxs-lookup"><span data-stu-id="c2e34-172">Task 2- Running the application</span></span>

<span data-ttu-id="c2e34-173">この時点では、データベースはまだは作成されません。</span><span class="sxs-lookup"><span data-stu-id="c2e34-173">At this point, the database is not yet created.</span></span> <span data-ttu-id="c2e34-174">このタスクでは、最初にアプリケーションを実行し、CRUD 操作をテストします。</span><span class="sxs-lookup"><span data-stu-id="c2e34-174">In this task, you will run the application for the first time and test the CRUD operations.</span></span> <span data-ttu-id="c2e34-175">データベースは、Code First で即座に作成されます。</span><span class="sxs-lookup"><span data-stu-id="c2e34-175">The database will be created on the fly with Code First.</span></span>

1. <span data-ttu-id="c2e34-176">**F5** キーを押してアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="c2e34-176">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="c2e34-177">ブラウザーで、追加**/Person**ユーザー ページを開くための URL にします。</span><span class="sxs-lookup"><span data-stu-id="c2e34-177">In the browser, add **/Person** to the URL to open the Person page.</span></span>

    <span data-ttu-id="c2e34-178">![アプリケーションの初回実行](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "アプリケーションの初回実行")</span><span class="sxs-lookup"><span data-stu-id="c2e34-178">![Application first run](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Application first run")</span></span>

    <span data-ttu-id="c2e34-179">*アプリケーション: 最初の実行します。*</span><span class="sxs-lookup"><span data-stu-id="c2e34-179">*Application: first run*</span></span>
3. <span data-ttu-id="c2e34-180">ようになりましたが、ユーザーのページの調査および、CRUD 操作をテストします。</span><span class="sxs-lookup"><span data-stu-id="c2e34-180">You will now explore the Person pages and test the CRUD operations.</span></span>

    1. <span data-ttu-id="c2e34-181">をクリックして**新規作成**を新しいユーザーを追加します。</span><span class="sxs-lookup"><span data-stu-id="c2e34-181">Click **Create New** to add a new person.</span></span> <span data-ttu-id="c2e34-182">名前、姓と名を入力し、クリックして**作成**です。</span><span class="sxs-lookup"><span data-stu-id="c2e34-182">Enter a first name and a last name and click **Create**.</span></span>

        <span data-ttu-id="c2e34-183">![新しいユーザーを追加する](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "新しいユーザーを追加します。")</span><span class="sxs-lookup"><span data-stu-id="c2e34-183">![Adding a new person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Adding a new person")</span></span>

        <span data-ttu-id="c2e34-184">*新しいユーザーを追加します。*</span><span class="sxs-lookup"><span data-stu-id="c2e34-184">*Adding a new person*</span></span>
    2. <span data-ttu-id="c2e34-185">人の一覧で、削除、編集または項目を追加することができます。</span><span class="sxs-lookup"><span data-stu-id="c2e34-185">In the person's list, you can delete, edit or add items.</span></span>

        <span data-ttu-id="c2e34-186">![人物リスト](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "人物リスト")</span><span class="sxs-lookup"><span data-stu-id="c2e34-186">![person list](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "person list")</span></span>

        <span data-ttu-id="c2e34-187">*人物リスト*</span><span class="sxs-lookup"><span data-stu-id="c2e34-187">*Person list*</span></span>
    3. <span data-ttu-id="c2e34-188">をクリックして**詳細**人の詳細を表示します。</span><span class="sxs-lookup"><span data-stu-id="c2e34-188">Click **Details** to open the person's details.</span></span>

        <span data-ttu-id="c2e34-189">![ユーザーの詳細](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "人の詳細")</span><span class="sxs-lookup"><span data-stu-id="c2e34-189">![Person's details](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Person's details")</span></span>

        <span data-ttu-id="c2e34-190">*ユーザーの詳細*</span><span class="sxs-lookup"><span data-stu-id="c2e34-190">*Person's details*</span></span>
4. <span data-ttu-id="c2e34-191">ブラウザーを閉じて、Visual Studio に戻ります。</span><span class="sxs-lookup"><span data-stu-id="c2e34-191">Close the browser and return to Visual Studio.</span></span> <span data-ttu-id="c2e34-192">Person エンティティに対して CRUD 全体を 1 行のコードを記述しなくても、モデル、ビューから、アプリケーション全体で作成したことを確認します。</span><span class="sxs-lookup"><span data-stu-id="c2e34-192">Notice that you have created the whole CRUD for the person entity throughout your application -from the model to the views- without having to write a single line of code!</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a><span data-ttu-id="c2e34-193">タスク 3-更新 Entity Framework Migrations を使用して、データベース</span><span class="sxs-lookup"><span data-stu-id="c2e34-193">Task 3- Updating the database using Entity Framework Migrations</span></span>

<span data-ttu-id="c2e34-194">このタスクでは、Entity Framework Migrations を使用してデータベースが更新されます。</span><span class="sxs-lookup"><span data-stu-id="c2e34-194">In this task you will update the database using Entity Framework Migrations.</span></span> <span data-ttu-id="c2e34-195">どのように簡単にモデルを変更し、Entity Framework 移行機能を使用して、データベースの変更が反映することがわかります。</span><span class="sxs-lookup"><span data-stu-id="c2e34-195">You will discover how easy it is to change the model and reflect the changes in your databases by using the Entity Framework Migrations feature.</span></span>

1. <span data-ttu-id="c2e34-196">パッケージ マネージャー コンソールを開きます。</span><span class="sxs-lookup"><span data-stu-id="c2e34-196">Open the Package Manager Console.</span></span> <span data-ttu-id="c2e34-197">選択**ツール |ライブラリ パッケージ マネージャー |パッケージ マネージャー コンソール**です。</span><span class="sxs-lookup"><span data-stu-id="c2e34-197">Select **Tools | Library Package Manager | Package Manager Console**.</span></span>
2. <span data-ttu-id="c2e34-198">パッケージ マネージャー コンソールで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="c2e34-198">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="c2e34-199">PMC</span><span class="sxs-lookup"><span data-stu-id="c2e34-199">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    <span data-ttu-id="c2e34-200">![移行を有効にする](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "移行を有効にします。")</span><span class="sxs-lookup"><span data-stu-id="c2e34-200">![Enabling Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Enabling Migrations")</span></span>

    <span data-ttu-id="c2e34-201">*移行を有効にします。*</span><span class="sxs-lookup"><span data-stu-id="c2e34-201">*Enabling migrations*</span></span>

    <span data-ttu-id="c2e34-202">移行を有効にするコマンドは、作成、**移行**フォルダーで、データベースを初期化するためのスクリプトが含まれています。</span><span class="sxs-lookup"><span data-stu-id="c2e34-202">The Enable-Migration command creates the **Migrations** folder, which contains a script to initialize the database.</span></span>

    <span data-ttu-id="c2e34-203">![Migrations フォルダー](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Migrations フォルダー")</span><span class="sxs-lookup"><span data-stu-id="c2e34-203">![Migrations folder](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Migrations folder")</span></span>

    <span data-ttu-id="c2e34-204">*Migrations フォルダー*</span><span class="sxs-lookup"><span data-stu-id="c2e34-204">*Migrations folder*</span></span>
3. <span data-ttu-id="c2e34-205">開く、**される Configuration.cs** Migrations フォルダー内のファイルです。</span><span class="sxs-lookup"><span data-stu-id="c2e34-205">Open the **Configuration.cs** file in the Migrations folder.</span></span> <span data-ttu-id="c2e34-206">クラスのコンス トラクターを検索し、変更、 **AutomaticMigrationsEnabled**値*true*です。</span><span class="sxs-lookup"><span data-stu-id="c2e34-206">Locate the class constructor and change the **AutomaticMigrationsEnabled** value to *true*.</span></span>


    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. <span data-ttu-id="c2e34-207">ユーザー クラスを開き、個人のミドル ネームの属性を追加します。</span><span class="sxs-lookup"><span data-stu-id="c2e34-207">Open the Person class and add an attribute for the person's middle name.</span></span> <span data-ttu-id="c2e34-208">この新しい属性を持つモデルも変更されます。</span><span class="sxs-lookup"><span data-stu-id="c2e34-208">With this new attribute, you are changing the model.</span></span>


    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. <span data-ttu-id="c2e34-209">選択**ビルド |ソリューションをビルド**メニューにアプリケーションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="c2e34-209">Select **Build | Build Solution** on the menu to build the application.</span></span>

    <span data-ttu-id="c2e34-210">![アプリケーションのビルド](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "アプリケーションのビルド")</span><span class="sxs-lookup"><span data-stu-id="c2e34-210">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Building the application")</span></span>

    <span data-ttu-id="c2e34-211">*アプリケーションのビルド*</span><span class="sxs-lookup"><span data-stu-id="c2e34-211">*Building the application*</span></span>
6. <span data-ttu-id="c2e34-212">パッケージ マネージャー コンソールで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="c2e34-212">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="c2e34-213">PMC</span><span class="sxs-lookup"><span data-stu-id="c2e34-213">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    <span data-ttu-id="c2e34-214">このコマンドはデータ オブジェクトの変更を検索し、その後、それに応じてデータベースを変更するために必要なコマンドが追加します。</span><span class="sxs-lookup"><span data-stu-id="c2e34-214">This command will look for changes in the data objects, and then, it will add the necessary commands to modify the database accordingly.</span></span>

    <span data-ttu-id="c2e34-215">![ミドル ネームを追加する](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "ミドル ネームを追加します。")</span><span class="sxs-lookup"><span data-stu-id="c2e34-215">![Adding a middle name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Adding a middle name")</span></span>

    <span data-ttu-id="c2e34-216">*ミドル ネームを追加します。*</span><span class="sxs-lookup"><span data-stu-id="c2e34-216">*Adding a middle name*</span></span>
7. <span data-ttu-id="c2e34-217">(省略可能)差分更新を含む SQL スクリプトを生成するには、次のコマンドを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="c2e34-217">(Optional) You can run the following command to generate a SQL script with the differential update.</span></span> <span data-ttu-id="c2e34-218">これにより、データベースを手動で更新する (この場合は必要はありません)、またはその他のデータベースの変更を適用します。</span><span class="sxs-lookup"><span data-stu-id="c2e34-218">This will let you update the database manually (In this case it's not necessary), or apply the changes in other databases:</span></span>

    <span data-ttu-id="c2e34-219">PMC</span><span class="sxs-lookup"><span data-stu-id="c2e34-219">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    <span data-ttu-id="c2e34-220">![SQL スクリプトを生成して](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "SQL スクリプトの生成")</span><span class="sxs-lookup"><span data-stu-id="c2e34-220">![Generating a SQL script](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generating a SQL script")</span></span>

    <span data-ttu-id="c2e34-221">*SQL スクリプトの生成*</span><span class="sxs-lookup"><span data-stu-id="c2e34-221">*Generating a SQL script*</span></span>

    <span data-ttu-id="c2e34-222">![SQL スクリプトの update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "SQL スクリプトの更新")</span><span class="sxs-lookup"><span data-stu-id="c2e34-222">![SQL Script update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "SQL Script update")</span></span>

    <span data-ttu-id="c2e34-223">*SQL スクリプトの更新*</span><span class="sxs-lookup"><span data-stu-id="c2e34-223">*SQL Script update*</span></span>
8. <span data-ttu-id="c2e34-224">パッケージ マネージャー コンソールで、データベースを更新するには、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="c2e34-224">In the Package Manager Console, enter the following command to update the database:</span></span>

    <span data-ttu-id="c2e34-225">PMC</span><span class="sxs-lookup"><span data-stu-id="c2e34-225">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    <span data-ttu-id="c2e34-226">![データベースの更新](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "データベースの更新")</span><span class="sxs-lookup"><span data-stu-id="c2e34-226">![Updating the Database](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Updating the Database")</span></span>

    <span data-ttu-id="c2e34-227">*データベースの更新*</span><span class="sxs-lookup"><span data-stu-id="c2e34-227">*Updating the Database*</span></span>

    <span data-ttu-id="c2e34-228">これが追加されます、 **MiddleName**内の列、**人**の現在の定義と一致するテーブル、 **Person**クラスです。</span><span class="sxs-lookup"><span data-stu-id="c2e34-228">This will add the **MiddleName** column in the **People** table to match the current definition of the **Person** class.</span></span>
9. <span data-ttu-id="c2e34-229">データベースが更新されると、コント ローラーのフォルダーを右クリックし **追加 |コント ローラー** Person コント ローラー再度 (同じ値を持つ完了) を追加します。</span><span class="sxs-lookup"><span data-stu-id="c2e34-229">Once the database is updated, right-click the Controller folder and select **Add | Controller** to add the Person controller again (Complete with the same values).</span></span> <span data-ttu-id="c2e34-230">これは、既存のメソッドと、新しい属性を追加するビューで更新されます。</span><span class="sxs-lookup"><span data-stu-id="c2e34-230">This will update the existing methods and views adding the new attribute.</span></span>

    <span data-ttu-id="c2e34-231">![コント ローラーの更新を追加する](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "コント ローラーの更新を追加します。")</span><span class="sxs-lookup"><span data-stu-id="c2e34-231">![Adding a controller update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Adding a controller update")</span></span>

    <span data-ttu-id="c2e34-232">*コント ローラーの更新*</span><span class="sxs-lookup"><span data-stu-id="c2e34-232">*Updating the controller*</span></span>
10. <span data-ttu-id="c2e34-233">**[追加]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="c2e34-233">Click **Add**.</span></span> <span data-ttu-id="c2e34-234">値を選択して、**上書き PersonController.cs**と**上書きするには、ビューが関連付けられている** をクリック**ok**です。</span><span class="sxs-lookup"><span data-stu-id="c2e34-234">Then, select the values **Overwrite PersonController.cs** and the **Overwrite associated views** and click **OK**.</span></span>

    ![コント ローラーの上書きを追加します。](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

    <span data-ttu-id="c2e34-236">*コント ローラーの更新*</span><span class="sxs-lookup"><span data-stu-id="c2e34-236">*Updating the controller*</span></span>

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a><span data-ttu-id="c2e34-237">Task4 - アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="c2e34-237">Task4- Running the application</span></span>

1. <span data-ttu-id="c2e34-238">**F5** キーを押してアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="c2e34-238">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="c2e34-239">開いている**/Person**です。</span><span class="sxs-lookup"><span data-stu-id="c2e34-239">Open **/Person**.</span></span> <span data-ttu-id="c2e34-240">データが保持されて、ミドル ネーム列が追加されたときに注意してください。</span><span class="sxs-lookup"><span data-stu-id="c2e34-240">Notice that the data was preserved, while the middle name column was added.</span></span>

    <span data-ttu-id="c2e34-241">![追加のミドル ネーム](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "ミドル ネームの追加")</span><span class="sxs-lookup"><span data-stu-id="c2e34-241">![Middle Name added](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Middle Name added")</span></span>

    <span data-ttu-id="c2e34-242">*ミドル ネームの追加*</span><span class="sxs-lookup"><span data-stu-id="c2e34-242">*Middle Name added*</span></span>
3. <span data-ttu-id="c2e34-243">クリックすると**編集**、ミドル ネームを現在のユーザーに追加することができます。</span><span class="sxs-lookup"><span data-stu-id="c2e34-243">If you click **Edit**, you will be able to add a middle name to the current person.</span></span>

    <span data-ttu-id="c2e34-244">![ミドル ネーム エディション](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "ミドル ネームのエディション")</span><span class="sxs-lookup"><span data-stu-id="c2e34-244">![Middle Name edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Middle Name edition")</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="c2e34-245">概要</span><span class="sxs-lookup"><span data-stu-id="c2e34-245">Summary</span></span>

<span data-ttu-id="c2e34-246">このハンズオン ラボでは、どのモデル クラスを使用して ASP.NET MVC 4 スキャフォールディングでの CRUD 操作を作成する簡単な手順を学習しました。</span><span class="sxs-lookup"><span data-stu-id="c2e34-246">In this Hands-On lab, you have learned simple steps to create CRUD operations with ASP.NET MVC 4 Scaffolding using any model class.</span></span> <span data-ttu-id="c2e34-247">次に、Entity Framework の移行を使用して、ビューへのデータベースから、アプリケーションでエンド ツー エンドの更新プログラムを実行する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="c2e34-247">Then, you have learned how to perform an end to end update in your application -from the database to the views- by using Entity Framework Migrations.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="c2e34-248">付録 a: をインストールする Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="c2e34-248">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="c2e34-249">インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、  **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="c2e34-249">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="c2e34-250">次の手順を案内するインストールに必要な手順*Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*です。</span><span class="sxs-lookup"><span data-stu-id="c2e34-250">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="c2e34-251">移動して[ [https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)です。</span><span class="sxs-lookup"><span data-stu-id="c2e34-251">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="c2e34-252">または、既に Web Platform Installer をインストールした場合、開くことができ、製品を検索&quot; *Visual Studio Express 2012 for Web と Windows Azure SDK*&quot;です。</span><span class="sxs-lookup"><span data-stu-id="c2e34-252">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="c2e34-253">をクリックして**を今すぐインストール**です。</span><span class="sxs-lookup"><span data-stu-id="c2e34-253">Click on **Install Now**.</span></span> <span data-ttu-id="c2e34-254">ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="c2e34-254">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="c2e34-255">1 回**Web Platform Installer**が開いて、をクリックして**インストール**セットアップを開始します。</span><span class="sxs-lookup"><span data-stu-id="c2e34-255">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="c2e34-256">![Visual Studio Express インストール](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Visual Studio Express のインストール")</span><span class="sxs-lookup"><span data-stu-id="c2e34-256">![Install Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="c2e34-257">*Visual Studio Express をインストールします。*</span><span class="sxs-lookup"><span data-stu-id="c2e34-257">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="c2e34-258">すべての製品のライセンスと条項を読み、クリックして**同意**を続行します。</span><span class="sxs-lookup"><span data-stu-id="c2e34-258">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![ライセンス条項に同意](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    <span data-ttu-id="c2e34-260">*ライセンス条項に同意*</span><span class="sxs-lookup"><span data-stu-id="c2e34-260">*Accepting the license terms*</span></span>
5. <span data-ttu-id="c2e34-261">ダウンロードとインストール プロセスが完了するまで待機します。</span><span class="sxs-lookup"><span data-stu-id="c2e34-261">Wait until the downloading and installation process completes.</span></span>

    ![インストールの進行状況](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    <span data-ttu-id="c2e34-263">*インストールの進行状況*</span><span class="sxs-lookup"><span data-stu-id="c2e34-263">*Installation progress*</span></span>
6. <span data-ttu-id="c2e34-264">インストールが完了したらをクリックして**完了**です。</span><span class="sxs-lookup"><span data-stu-id="c2e34-264">When the installation completes, click **Finish**.</span></span>

    ![インストールが完了しました](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    <span data-ttu-id="c2e34-266">*インストールが完了しました*</span><span class="sxs-lookup"><span data-stu-id="c2e34-266">*Installation completed*</span></span>
7. <span data-ttu-id="c2e34-267">をクリックして**終了**Web Platform Installer を閉じます。</span><span class="sxs-lookup"><span data-stu-id="c2e34-267">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="c2e34-268">Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、順にクリックして、 **VS Express for Web**並べて表示します。</span><span class="sxs-lookup"><span data-stu-id="c2e34-268">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express Web タイルを](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    <span data-ttu-id="c2e34-270">*VS Express Web タイルを*</span><span class="sxs-lookup"><span data-stu-id="c2e34-270">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="c2e34-271">付録 b: コード スニペットの使用</span><span class="sxs-lookup"><span data-stu-id="c2e34-271">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="c2e34-272">コード スニペットでは、すぐに利用できる必要があるすべてのコードがあります。</span><span class="sxs-lookup"><span data-stu-id="c2e34-272">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="c2e34-273">ラボ ドキュメントがわかります正確に使用する場合が、次の図に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="c2e34-273">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="c2e34-274">![Visual Studio のコード スニペットを使用して、プロジェクトにコードを挿入する](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "プロジェクトにコードを挿入する Visual Studio を使ってコード スニペット")</span><span class="sxs-lookup"><span data-stu-id="c2e34-274">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="c2e34-275">*Visual Studio のコード スニペットを使用して、プロジェクトにコードを挿入するには*</span><span class="sxs-lookup"><span data-stu-id="c2e34-275">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="c2e34-276">***キーボード (C# の場合のみ) を使用してコード スニペットを追加するには***</span><span class="sxs-lookup"><span data-stu-id="c2e34-276">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="c2e34-277">コードを挿入するには、カーソルを置きます。</span><span class="sxs-lookup"><span data-stu-id="c2e34-277">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="c2e34-278">(なし、スペースやハイフン) スニペット名を入力してを起動します。</span><span class="sxs-lookup"><span data-stu-id="c2e34-278">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="c2e34-279">スニペットの名前に一致する IntelliSense 表示を確認します。</span><span class="sxs-lookup"><span data-stu-id="c2e34-279">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="c2e34-280">正しいスニペットを選択 (または、全体のスニペットの名前を選択するまで」と入力してください)。</span><span class="sxs-lookup"><span data-stu-id="c2e34-280">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="c2e34-281">カーソル位置にスニペットを挿入するには、2 回、Tab キーを押します。</span><span class="sxs-lookup"><span data-stu-id="c2e34-281">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="c2e34-282">![スニペット名を入力する開始](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "スニペット名の入力を開始")</span><span class="sxs-lookup"><span data-stu-id="c2e34-282">![Start typing the snippet name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="c2e34-283">*スニペット名の入力を開始します。*</span><span class="sxs-lookup"><span data-stu-id="c2e34-283">*Start typing the snippet name*</span></span>

<span data-ttu-id="c2e34-284">![Tab キーを押して、強調表示されているスニペットを選択する](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "強調表示されているスニペットを選択するキーを押してタブ")</span><span class="sxs-lookup"><span data-stu-id="c2e34-284">![Press Tab to select the highlighted snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="c2e34-285">*Tab キーを押して、強調表示されているスニペットを選択するには*</span><span class="sxs-lookup"><span data-stu-id="c2e34-285">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="c2e34-286">![キーを押して タブで再度と、スニペットが展開](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "キーを押して タブで再度と、スニペットが展開されます")</span><span class="sxs-lookup"><span data-stu-id="c2e34-286">![Press Tab again and the snippet will expand](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="c2e34-287">*キーを押して タブで再度と、スニペットが展開されます。*</span><span class="sxs-lookup"><span data-stu-id="c2e34-287">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="c2e34-288">***(C#、Visual Basic および XML) にマウスを使用してコード スニペットを追加する***1 です。</span><span class="sxs-lookup"><span data-stu-id="c2e34-288">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="c2e34-289">コード スニペットを挿入する場所を右クリックします。</span><span class="sxs-lookup"><span data-stu-id="c2e34-289">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="c2e34-290">選択**スニペットの挿入**続く**マイ コード スニペット**です。</span><span class="sxs-lookup"><span data-stu-id="c2e34-290">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="c2e34-291">クリックして一覧から、関連するスニペットを選択します。</span><span class="sxs-lookup"><span data-stu-id="c2e34-291">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="c2e34-292">![コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリックして](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリック")</span><span class="sxs-lookup"><span data-stu-id="c2e34-292">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="c2e34-293">*コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリックします。*</span><span class="sxs-lookup"><span data-stu-id="c2e34-293">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="c2e34-294">![クリックして一覧から、関連するスニペットを選択](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "クリックして一覧から、関連するスニペットを選択")</span><span class="sxs-lookup"><span data-stu-id="c2e34-294">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="c2e34-295">*クリックして一覧から、関連するスニペットを選択します。*</span><span class="sxs-lookup"><span data-stu-id="c2e34-295">*Pick the relevant snippet from the list, by clicking on it*</span></span>
