---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
title: "データベースのデータ (VB) のテーブルを表示する |Microsoft ドキュメント"
author: microsoft
description: "このチュートリアルでは、一連のデータベース レコードを表示する 2 つの方法を説明します。 書式の HTML 内のデータベース レコードのセット ta の 2 つの方法を説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: 5bb4587f-5bcd-44f5-b368-3c1709162b35
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 6dc0aa91cfb68d308ed098f429d3251d424ab778
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="displaying-a-table-of-database-data-vb"></a><span data-ttu-id="44b9e-104">データベースのデータ (VB) のテーブルを表示します。</span><span class="sxs-lookup"><span data-stu-id="44b9e-104">Displaying a Table of Database Data (VB)</span></span>
====================
<span data-ttu-id="44b9e-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="44b9e-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="44b9e-106">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="44b9e-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_VB.pdf)

> <span data-ttu-id="44b9e-107">このチュートリアルでは、一連のデータベース レコードを表示する 2 つの方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="44b9e-107">In this tutorial, I demonstrate two methods of displaying a set of database records.</span></span> <span data-ttu-id="44b9e-108">HTML テーブル内のデータベース レコードのセットを書式設定の 2 つの方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="44b9e-108">I show two methods of formatting a set of database records in an HTML table.</span></span> <span data-ttu-id="44b9e-109">最初に、ビュー内で直接、データベース レコードの書式を設定する方法をお見せします。</span><span class="sxs-lookup"><span data-stu-id="44b9e-109">First, I show how you can format the database records directly within a view.</span></span> <span data-ttu-id="44b9e-110">次に、デモが利用できるかパーシャルのデータベース レコードを書式設定する場合。</span><span class="sxs-lookup"><span data-stu-id="44b9e-110">Next, I demonstrate how you can take advantage of partials when formatting database records.</span></span>


<span data-ttu-id="44b9e-111">このチュートリアルの目的では、ASP.NET MVC アプリケーションで HTML テーブル データベースのデータを表示する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="44b9e-111">The goal of this tutorial is to explain how you can display an HTML table of database data in an ASP.NET MVC application.</span></span> <span data-ttu-id="44b9e-112">最初に、ツールを使用して、スキャフォールディング Visual Studio に含まれる一連のレコードを自動的に表示するビューを生成する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="44b9e-112">First, you learn how to use the scaffolding tools included in Visual Studio to generate a view that displays a set of records automatically.</span></span> <span data-ttu-id="44b9e-113">次に、データベース レコードをフォーマットするときに、部分的なをテンプレートとして使用する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="44b9e-113">Next, you learn how to use a partial as a template when formatting database records.</span></span>

## <a name="create-the-model-classes"></a><span data-ttu-id="44b9e-114">モデルのクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="44b9e-114">Create the Model Classes</span></span>

<span data-ttu-id="44b9e-115">映画のデータベース テーブルからレコードのセットを表示しようとしています。</span><span class="sxs-lookup"><span data-stu-id="44b9e-115">We are going to display the set of records from the Movies database table.</span></span> <span data-ttu-id="44b9e-116">映画のデータベース テーブルには、次の列が含まれています。</span><span class="sxs-lookup"><span data-stu-id="44b9e-116">The Movies database table contains the following columns:</span></span>

<a id="0.4_table01"></a>


| <span data-ttu-id="44b9e-117">**列名**</span><span class="sxs-lookup"><span data-stu-id="44b9e-117">**Column Name**</span></span> | <span data-ttu-id="44b9e-118">**データ型**</span><span class="sxs-lookup"><span data-stu-id="44b9e-118">**Data Type**</span></span> | <span data-ttu-id="44b9e-119">**Null を許容します。**</span><span class="sxs-lookup"><span data-stu-id="44b9e-119">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="44b9e-120">ID</span><span class="sxs-lookup"><span data-stu-id="44b9e-120">Id</span></span> | <span data-ttu-id="44b9e-121">Int</span><span class="sxs-lookup"><span data-stu-id="44b9e-121">Int</span></span> | <span data-ttu-id="44b9e-122">False</span><span class="sxs-lookup"><span data-stu-id="44b9e-122">False</span></span> |
| <span data-ttu-id="44b9e-123">タイトル</span><span class="sxs-lookup"><span data-stu-id="44b9e-123">Title</span></span> | <span data-ttu-id="44b9e-124">Nvarchar (200)</span><span class="sxs-lookup"><span data-stu-id="44b9e-124">Nvarchar(200)</span></span> | <span data-ttu-id="44b9e-125">False</span><span class="sxs-lookup"><span data-stu-id="44b9e-125">False</span></span> |
| <span data-ttu-id="44b9e-126">ディレクター</span><span class="sxs-lookup"><span data-stu-id="44b9e-126">Director</span></span> | <span data-ttu-id="44b9e-127">Nvarchar (50)</span><span class="sxs-lookup"><span data-stu-id="44b9e-127">NVarchar(50)</span></span> | <span data-ttu-id="44b9e-128">False</span><span class="sxs-lookup"><span data-stu-id="44b9e-128">False</span></span> |
| <span data-ttu-id="44b9e-129">DateReleased</span><span class="sxs-lookup"><span data-stu-id="44b9e-129">DateReleased</span></span> | <span data-ttu-id="44b9e-130">DateTime</span><span class="sxs-lookup"><span data-stu-id="44b9e-130">DateTime</span></span> | <span data-ttu-id="44b9e-131">False</span><span class="sxs-lookup"><span data-stu-id="44b9e-131">False</span></span> |


<span data-ttu-id="44b9e-132">ASP.NET MVC アプリケーションでは、ムービーのテーブルを表すためには、モデル クラスを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="44b9e-132">In order to represent the Movies table in our ASP.NET MVC application, we need to create a model class.</span></span> <span data-ttu-id="44b9e-133">このチュートリアルでは、Microsoft の Entity Framework を使用して、モデル クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="44b9e-133">In this tutorial, we use the Microsoft Entity Framework to create our model classes.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="44b9e-134">このチュートリアルでは、Microsoft の Entity Framework を使用します。</span><span class="sxs-lookup"><span data-stu-id="44b9e-134">In this tutorial, we use the Microsoft Entity Framework.</span></span> <span data-ttu-id="44b9e-135">ただし、LINQ to SQL、NHibernate、または ADO.NET を含む ASP.NET MVC アプリケーションからデータベースとやり取りするさまざまな異なるテクノロジを使用することができますを理解しておく必要があります。</span><span class="sxs-lookup"><span data-stu-id="44b9e-135">However, it is important to understand that you can use a variety of different technologies to interact with a database from an ASP.NET MVC application including LINQ to SQL, NHibernate, or ADO.NET.</span></span>


<span data-ttu-id="44b9e-136">Entity Data Model ウィザードを起動する次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="44b9e-136">Follow these steps to launch the Entity Data Model Wizard:</span></span>

1. <span data-ttu-id="44b9e-137">ソリューション エクスプ ローラー ウィンドウのオプションを選択し、メニューの モデル フォルダーを右クリックして**追加、新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="44b9e-137">Right-click the Models folder in the Solution Explorer window and the select the menu option **Add, New Item**.</span></span>
2. <span data-ttu-id="44b9e-138">選択、**データ**カテゴリを選択、 **ADO.NET エンティティ データ モデル**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="44b9e-138">Select the **Data** category and select the **ADO.NET Entity Data Model** template.</span></span>
3. <span data-ttu-id="44b9e-139">データ モデルに名前を付ける*MoviesDBModel.edmx*  をクリックし、**追加**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="44b9e-139">Give your data model the name *MoviesDBModel.edmx* and click the **Add** button.</span></span>

<span data-ttu-id="44b9e-140">Entity Data Model ウィザードでは、[追加] ボタンをクリックした後、(図 1 を参照) が表示されます。</span><span class="sxs-lookup"><span data-stu-id="44b9e-140">After you click the Add button, the Entity Data Model Wizard appears (see Figure 1).</span></span> <span data-ttu-id="44b9e-141">ウィザードを完了する手順に従います。</span><span class="sxs-lookup"><span data-stu-id="44b9e-141">Follow these steps to complete the wizard:</span></span>

1. <span data-ttu-id="44b9e-142">**モデルのコンテンツ**手順、select、**データベースから生成**オプション。</span><span class="sxs-lookup"><span data-stu-id="44b9e-142">In the **Choose Model Contents** step, select the **Generate from database** option.</span></span>
2. <span data-ttu-id="44b9e-143">**データ接続の選択**ステップを使用して、 *MoviesDB.mdf*データ接続と名前*MoviesDBEntities*接続の設定。</span><span class="sxs-lookup"><span data-stu-id="44b9e-143">In the **Choose Your Data Connection** step, use the *MoviesDB.mdf* data connection and the name *MoviesDBEntities* for the connection settings.</span></span> <span data-ttu-id="44b9e-144">クリックして、**次**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="44b9e-144">Click the **Next** button.</span></span>
3. <span data-ttu-id="44b9e-145">**データベース オブジェクトの選択**ステップ、[テーブル] ノードを展開し、映画のテーブルを選択します。</span><span class="sxs-lookup"><span data-stu-id="44b9e-145">In the **Choose Your Database Objects** step, expand the Tables node, select the Movies table.</span></span> <span data-ttu-id="44b9e-146">名前空間を入力*モデル* をクリックし、**完了**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="44b9e-146">Enter the namespace *Models* and click the **Finish** button.</span></span>


<span data-ttu-id="44b9e-147">[![LINQ to SQL クラスの作成](displaying-a-table-of-database-data-vb/_static/image1.jpg)](displaying-a-table-of-database-data-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="44b9e-147">[![Creating LINQ to SQL classes](displaying-a-table-of-database-data-vb/_static/image1.jpg)](displaying-a-table-of-database-data-vb/_static/image1.png)</span></span>

<span data-ttu-id="44b9e-148">**図 01**: LINQ to SQL クラスを作成する ([フルサイズのイメージを表示するをクリックして](displaying-a-table-of-database-data-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="44b9e-148">**Figure 01**: Creating LINQ to SQL classes ([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image2.png))</span></span>


<span data-ttu-id="44b9e-149">Entity Data Model ウィザードを完了すると、Entity Data Model デザイナーが開きます。</span><span class="sxs-lookup"><span data-stu-id="44b9e-149">After you complete the Entity Data Model Wizard, the Entity Data Model Designer opens.</span></span> <span data-ttu-id="44b9e-150">デザイナーは、映画エンティティを表示する必要があります (図 2 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="44b9e-150">The Designer should display the Movies entity (see Figure 2).</span></span>


<span data-ttu-id="44b9e-151">[![Entity Data Model Designer](displaying-a-table-of-database-data-vb/_static/image2.jpg)](displaying-a-table-of-database-data-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="44b9e-151">[![The Entity Data Model Designer](displaying-a-table-of-database-data-vb/_static/image2.jpg)](displaying-a-table-of-database-data-vb/_static/image3.png)</span></span>

<span data-ttu-id="44b9e-152">**図 02**: The Entity Data Model Designer ([フルサイズのイメージを表示するをクリックして](displaying-a-table-of-database-data-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="44b9e-152">**Figure 02**: The Entity Data Model Designer ([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image4.png))</span></span>


<span data-ttu-id="44b9e-153">続行する前に、1 つの変更を行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="44b9e-153">We need to make one change before we continue.</span></span> <span data-ttu-id="44b9e-154">エンティティ データ ウィザードという名前のモデル クラスを生成する*映画*映画データベース テーブルを表すです。</span><span class="sxs-lookup"><span data-stu-id="44b9e-154">The Entity Data Wizard generates a model class named *Movies* that represents the Movies database table.</span></span> <span data-ttu-id="44b9e-155">映画クラスを使用して、特定のムービーを表す、ためにクラスの名前を変更する必要があります*ムービー*の代わりに*映画*(単数形ではなく複数形)。</span><span class="sxs-lookup"><span data-stu-id="44b9e-155">Because we'll use the Movies class to represent a particular movie, we need to modify the name of the class to be *Movie* instead of *Movies* (singular rather than plural).</span></span>

<span data-ttu-id="44b9e-156">デザイナー画面にクラスの名前をダブルクリックし、ムービーからムービーにクラスの名前を変更します。</span><span class="sxs-lookup"><span data-stu-id="44b9e-156">Double-click the name of the class on the designer surface and change the name of the class from Movies to Movie.</span></span> <span data-ttu-id="44b9e-157">この変更を加えたらをクリックして、**保存**ムービー クラスを生成する (フロッピー ディスクのアイコン) ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="44b9e-157">After making this change, click the **Save** button (the icon of the floppy disk) to generate the Movie class.</span></span>

## <a name="create-the-movies-controller"></a><span data-ttu-id="44b9e-158">ビデオ コント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="44b9e-158">Create the Movies Controller</span></span>

<span data-ttu-id="44b9e-159">データベース レコードを表す方法がある、ムービーのコレクションを取得するコント ローラーを作成できます。</span><span class="sxs-lookup"><span data-stu-id="44b9e-159">Now that we have a way to represent our database records, we can create a controller that returns the collection of movies.</span></span> <span data-ttu-id="44b9e-160">Visual Studio ソリューション エクスプ ローラー ウィンドウ内でコント ローラーのフォルダーを右クリックし、メニュー オプションを選択**追加、コント ローラー** (図 3 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="44b9e-160">Within the Visual Studio Solution Explorer window, right-click the Controllers folder and select the menu option **Add, Controller** (see Figure 3).</span></span>


<span data-ttu-id="44b9e-161">[![コント ローラーのメニューに追加します。](displaying-a-table-of-database-data-vb/_static/image3.jpg)](displaying-a-table-of-database-data-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="44b9e-161">[![The Add Controller Menu](displaying-a-table-of-database-data-vb/_static/image3.jpg)](displaying-a-table-of-database-data-vb/_static/image5.png)</span></span>

<span data-ttu-id="44b9e-162">**図 03**: 追加のコント ローラー メニュー ([フルサイズのイメージを表示するをクリックして](displaying-a-table-of-database-data-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="44b9e-162">**Figure 03**: The Add Controller Menu ([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image6.png))</span></span>


<span data-ttu-id="44b9e-163">ときに、**コント ローラーの追加**MovieController コント ローラー名を入力してください ダイアログが表示されます (図 4 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="44b9e-163">When the **Add Controller** dialog appears, enter the controller name MovieController (see Figure 4).</span></span> <span data-ttu-id="44b9e-164">クリックして、**追加**を新しいコント ローラーを追加するボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="44b9e-164">Click the **Add** button to add the new controller.</span></span>


<span data-ttu-id="44b9e-165">[![コント ローラーの追加 ダイアログ](displaying-a-table-of-database-data-vb/_static/image4.jpg)](displaying-a-table-of-database-data-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="44b9e-165">[![The Add Controller dialog](displaying-a-table-of-database-data-vb/_static/image4.jpg)](displaying-a-table-of-database-data-vb/_static/image7.png)</span></span>

<span data-ttu-id="44b9e-166">**図 04**:「コント ローラーの追加 ダイアログ ([フルサイズのイメージを表示するには、をクリックして](displaying-a-table-of-database-data-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="44b9e-166">**Figure 04**: The Add Controller dialog([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image8.png))</span></span>


<span data-ttu-id="44b9e-167">データベース レコードのセットを返すようにビデオ コント ローラーによって公開される Index() アクションを変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="44b9e-167">We need to modify the Index() action exposed by the Movie controller so that it returns the set of database records.</span></span> <span data-ttu-id="44b9e-168">コント ローラーを変更して、リスト 1 のコント ローラーのようになります。</span><span class="sxs-lookup"><span data-stu-id="44b9e-168">Modify the controller so that it looks like the controller in Listing 1.</span></span>

<span data-ttu-id="44b9e-169">**1 – Controllers\MovieController.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="44b9e-169">**Listing 1 – Controllers\MovieController.vb**</span></span>

[!code-vb[Main](displaying-a-table-of-database-data-vb/samples/sample1.vb)]

<span data-ttu-id="44b9e-170">1 の一覧表示するのには、MoviesDBEntities クラスを使用してを MoviesDB データベースを表します。</span><span class="sxs-lookup"><span data-stu-id="44b9e-170">In Listing 1, the MoviesDBEntities class is used to represent the MoviesDB database.</span></span> <span data-ttu-id="44b9e-171">式*エンティティです。MovieSet.ToList()*映画データベース テーブルからすべてのムービーのセットを返します。</span><span class="sxs-lookup"><span data-stu-id="44b9e-171">The expression *entities.MovieSet.ToList()* returns the set of all movies from the Movies database table.</span></span>

## <a name="create-the-view"></a><span data-ttu-id="44b9e-172">ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="44b9e-172">Create the View</span></span>

<span data-ttu-id="44b9e-173">HTML テーブルにデータベース レコードのセットを表示する最も簡単な方法は、Visual Studio によって提供されるスキャフォールディングを活用するためにです。</span><span class="sxs-lookup"><span data-stu-id="44b9e-173">The easiest way to display a set of database records in an HTML table is to take advantage of the scaffolding provided by Visual Studio.</span></span>

<span data-ttu-id="44b9e-174">メニュー オプションを選択して、アプリケーションをビルド**ビルド、ソリューションのビルド**です。</span><span class="sxs-lookup"><span data-stu-id="44b9e-174">Build your application by selecting the menu option **Build, Build Solution**.</span></span> <span data-ttu-id="44b9e-175">開始する前に、アプリケーションをビルドする必要があります、**ビューの追加**ダイアログや、データ クラスは、ダイアログ ボックスに表示されません。</span><span class="sxs-lookup"><span data-stu-id="44b9e-175">You must build your application before opening the **Add View** dialog or your data classes won't appear in the dialog.</span></span>

<span data-ttu-id="44b9e-176">Index() アクションを右クリックし、メニュー オプションを選択**ビューの追加**(図 5 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="44b9e-176">Right-click the Index() action and select the menu option **Add View** (see Figure 5).</span></span>


<span data-ttu-id="44b9e-177">[![ビューの追加](displaying-a-table-of-database-data-vb/_static/image5.jpg)](displaying-a-table-of-database-data-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="44b9e-177">[![Adding a view](displaying-a-table-of-database-data-vb/_static/image5.jpg)](displaying-a-table-of-database-data-vb/_static/image9.png)</span></span>

<span data-ttu-id="44b9e-178">**図 05**: ビューの追加 ([フルサイズのイメージを表示するをクリックして](displaying-a-table-of-database-data-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="44b9e-178">**Figure 05**: Adding a view ([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image10.png))</span></span>


<span data-ttu-id="44b9e-179">**ビューの追加**ダイアログ ボックスで、チェック ボックス ラベルの付いた**厳密に型指定されたビューを作成**です。</span><span class="sxs-lookup"><span data-stu-id="44b9e-179">In the **Add View** dialog, check the checkbox labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="44b9e-180">としてムービー クラスを選択して、**データ クラスを表示**です。</span><span class="sxs-lookup"><span data-stu-id="44b9e-180">Select the Movie class as the **view data class**.</span></span> <span data-ttu-id="44b9e-181">選択*リスト*として、**コンテンツを表示**(図 6 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="44b9e-181">Select *List* as the **view content** (see Figure 6).</span></span> <span data-ttu-id="44b9e-182">これらのオプションを選択すると、ムービーの一覧を表示する厳密に型指定されたビューが生成されます。</span><span class="sxs-lookup"><span data-stu-id="44b9e-182">Selecting these options will generate a strongly-typed view that displays a list of movies.</span></span>


<span data-ttu-id="44b9e-183">[![ビューの追加 ダイアログ](displaying-a-table-of-database-data-vb/_static/image6.jpg)](displaying-a-table-of-database-data-vb/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="44b9e-183">[![The Add View dialog](displaying-a-table-of-database-data-vb/_static/image6.jpg)](displaying-a-table-of-database-data-vb/_static/image11.png)</span></span>

<span data-ttu-id="44b9e-184">**図 06**: ビューの追加 ダイアログ ([フルサイズのイメージを表示するをクリックして](displaying-a-table-of-database-data-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="44b9e-184">**Figure 06**: The Add View dialog([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image12.png))</span></span>


<span data-ttu-id="44b9e-185">クリックした後、**追加**ボタン、リスト 2 でビューが自動的に生成されます。</span><span class="sxs-lookup"><span data-stu-id="44b9e-185">After you click the **Add** button, the view in Listing 2 is generated automatically.</span></span> <span data-ttu-id="44b9e-186">このビューには、映画のコレクションを反復処理し、各ムービーのプロパティを表示するためのコードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="44b9e-186">This view contains the code required to iterate through the collection of movies and display each of the properties of a movie.</span></span>

<span data-ttu-id="44b9e-187">**2 – Views\Movie\Index.aspx を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="44b9e-187">**Listing 2 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample2.aspx)]

<span data-ttu-id="44b9e-188">メニュー オプションを選択してアプリケーションを実行することができます**デバッグ、デバッグの開始** (または F5 キーを押す) します。</span><span class="sxs-lookup"><span data-stu-id="44b9e-188">You can run the application by selecting the menu option **Debug, Start Debugging** (or hitting the F5 key).</span></span> <span data-ttu-id="44b9e-189">アプリケーションを実行すると、Internet Explorer が起動します。</span><span class="sxs-lookup"><span data-stu-id="44b9e-189">Running the application launches Internet Explorer.</span></span> <span data-ttu-id="44b9e-190">/Movie URL に移動すると、図 7 にページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="44b9e-190">If you navigate to the /Movie URL then you'll see the page in Figure 7.</span></span>


<span data-ttu-id="44b9e-191">[![ムービーのテーブル](displaying-a-table-of-database-data-vb/_static/image7.jpg)](displaying-a-table-of-database-data-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="44b9e-191">[![A table of movies](displaying-a-table-of-database-data-vb/_static/image7.jpg)](displaying-a-table-of-database-data-vb/_static/image13.png)</span></span>

<span data-ttu-id="44b9e-192">**図 07**: 映画のテーブル ([フルサイズのイメージを表示するをクリックして](displaying-a-table-of-database-data-vb/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="44b9e-192">**Figure 07**: A table of movies([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image14.png))</span></span>


<span data-ttu-id="44b9e-193">図 7 にデータベース レコードのグリッドの外観について何もたくない場合、インデックス ビューだけで変更できます。</span><span class="sxs-lookup"><span data-stu-id="44b9e-193">If you don't like anything about the appearance of the grid of database records in Figure 7 then you can simply modify the Index view.</span></span> <span data-ttu-id="44b9e-194">たとえば、変更することができます、 *DateReleased*ヘッダーを*リリース日*インデックス ビューを変更することによりします。</span><span class="sxs-lookup"><span data-stu-id="44b9e-194">For example, you can change the *DateReleased* header to *Date Released* by modifying the Index view.</span></span>

## <a name="create-a-template-with-a-partial"></a><span data-ttu-id="44b9e-195">一部のテンプレートを作成します。</span><span class="sxs-lookup"><span data-stu-id="44b9e-195">Create a Template with a Partial</span></span>

<span data-ttu-id="44b9e-196">ビューが複雑すぎるを取得するときは、パーシャルにビューを分割を開始することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="44b9e-196">When a view gets too complicated, it is a good idea to start breaking the view into partials.</span></span> <span data-ttu-id="44b9e-197">パーシャルを使用して容易ビューは、ユーザーに理解および保守します。</span><span class="sxs-lookup"><span data-stu-id="44b9e-197">Using partials makes your views easier to understand and maintain.</span></span> <span data-ttu-id="44b9e-198">ここで作成、部分的なのムービーのデータベース レコードのそれぞれの書式を設定するテンプレートとして使用することができます。</span><span class="sxs-lookup"><span data-stu-id="44b9e-198">We'll create a partial that we can use as a template to format each of the movie database records.</span></span>

<span data-ttu-id="44b9e-199">部分を作成する手順に従います。</span><span class="sxs-lookup"><span data-stu-id="44b9e-199">Follow these steps to create the partial:</span></span>

1. <span data-ttu-id="44b9e-200">Views\Movie フォルダーを右クリックし、メニュー オプションを選択**ビューの追加**です。</span><span class="sxs-lookup"><span data-stu-id="44b9e-200">Right-click the Views\Movie folder and select the menu option **Add View**.</span></span>
2. <span data-ttu-id="44b9e-201">チェック ボックス ラベルの付いた*部分ビュー (.ascx) を作成する*です。</span><span class="sxs-lookup"><span data-stu-id="44b9e-201">Check the checkbox labeled *Create a partial view (.ascx)*.</span></span>
3. <span data-ttu-id="44b9e-202">部分的な名前を付けます*MovieTemplate*です。</span><span class="sxs-lookup"><span data-stu-id="44b9e-202">Name the partial *MovieTemplate*.</span></span>
4. <span data-ttu-id="44b9e-203">チェック ボックス ラベルの付いた**厳密に型指定されたビューを作成する**です。</span><span class="sxs-lookup"><span data-stu-id="44b9e-203">Check the checkbox labeled **Create a strongly-typed view**.</span></span>
5. <span data-ttu-id="44b9e-204">ビデオとしてを選択して、*データ クラスを表示*です。</span><span class="sxs-lookup"><span data-stu-id="44b9e-204">Select Movie as the *view data class*.</span></span>
6. <span data-ttu-id="44b9e-205">として空の選択、*コンテンツを表示*です。</span><span class="sxs-lookup"><span data-stu-id="44b9e-205">Select Empty as the *view content*.</span></span>
7. <span data-ttu-id="44b9e-206">クリックして、**追加**部分的なプロジェクトを追加する、ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="44b9e-206">Click the **Add** button to add the partial to your project.</span></span>

<span data-ttu-id="44b9e-207">次の手順を完了すると、3 の一覧表示するように部分的な MovieTemplate を変更します。</span><span class="sxs-lookup"><span data-stu-id="44b9e-207">After you complete these steps, modify the MovieTemplate partial to look like Listing 3.</span></span>

<span data-ttu-id="44b9e-208">**3 – Views\Movie\MovieTemplate.ascx を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="44b9e-208">**Listing 3 – Views\Movie\MovieTemplate.ascx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample3.aspx)]

<span data-ttu-id="44b9e-209">リスト 3 の部分には、レコードの単一行のテンプレートが含まれています。</span><span class="sxs-lookup"><span data-stu-id="44b9e-209">The partial in Listing 3 contains a template for a single row of records.</span></span>

<span data-ttu-id="44b9e-210">インデックス ビューを一覧表示する 4 では、MovieTemplate 部分を使用します。</span><span class="sxs-lookup"><span data-stu-id="44b9e-210">The modified Index view in Listing 4 uses the MovieTemplate partial.</span></span>

<span data-ttu-id="44b9e-211">**4 – Views\Movie\Index.aspx を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="44b9e-211">**Listing 4 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample4.aspx)]

<span data-ttu-id="44b9e-212">ビュー一覧表示する 4 で、には For が含まれています、ムービーのすべてを反復処理する各ループします。</span><span class="sxs-lookup"><span data-stu-id="44b9e-212">The view in Listing 4 contains a For Each loop that iterates through all of the movies.</span></span> <span data-ttu-id="44b9e-213">各ムービー MovieTemplate 部分を使用して、ムービーを書式設定をします。</span><span class="sxs-lookup"><span data-stu-id="44b9e-213">For each movie, the MovieTemplate partial is used to format the movie.</span></span> <span data-ttu-id="44b9e-214">RenderPartial() ヘルパー メソッドを呼び出すことによって、MovieTemplate がレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="44b9e-214">The MovieTemplate is rendered by calling the RenderPartial() helper method.</span></span>

<span data-ttu-id="44b9e-215">変更したのインデックス ビューは、データベース レコードのまったく同じ HTML テーブルを表示します。</span><span class="sxs-lookup"><span data-stu-id="44b9e-215">The modified Index view renders the very same HTML table of database records.</span></span> <span data-ttu-id="44b9e-216">ただし、ビューを大幅に簡略化ができます。</span><span class="sxs-lookup"><span data-stu-id="44b9e-216">However, the view has been greatly simplified.</span></span>


<span data-ttu-id="44b9e-217">RenderPartial() メソッドではほとんどの他のヘルパー メソッドとは異なる文字列を返すことはできません。</span><span class="sxs-lookup"><span data-stu-id="44b9e-217">The RenderPartial() method is different than most of the other helper methods because it does not return a string.</span></span> <span data-ttu-id="44b9e-218">そのため、呼び出す必要があります RenderPartial() メソッドを使用して、 &lt;%html.renderpartial()&gt;の代わりに&lt;% = Html.RenderPartial() %&gt;です。</span><span class="sxs-lookup"><span data-stu-id="44b9e-218">Therefore, you must call the RenderPartial() method using &lt;% Html.RenderPartial() %&gt; instead of &lt;%= Html.RenderPartial() %&gt;.</span></span>


## <a name="summary"></a><span data-ttu-id="44b9e-219">概要</span><span class="sxs-lookup"><span data-stu-id="44b9e-219">Summary</span></span>

<span data-ttu-id="44b9e-220">このチュートリアルの目的は、HTML テーブルの一連のデータベース レコードを表示する方法を説明しました。</span><span class="sxs-lookup"><span data-stu-id="44b9e-220">The goal of this tutorial was to explain how you can display a set of database records in an HTML table.</span></span> <span data-ttu-id="44b9e-221">最初に、コント ローラーのアクションから Microsoft Entity Framework を活用して、データベース レコードのセットを返す方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="44b9e-221">First, you learned how to return a set of database records from a controller action by taking advantage of the Microsoft Entity Framework.</span></span> <span data-ttu-id="44b9e-222">次に、Visual Studio のスキャフォールディングを使用して項目のコレクションを自動的に表示するビューを生成する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="44b9e-222">Next, you learned how to use Visual Studio scaffolding to generate a view that displays a collection of items automatically.</span></span> <span data-ttu-id="44b9e-223">最後に、一部を利用して、ビューを簡略化する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="44b9e-223">Finally, you learned how to simplify the view by taking advantage of a partial.</span></span> <span data-ttu-id="44b9e-224">データベースの各レコードの書式を設定することができるように、テンプレートとして、部分的なを使用する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="44b9e-224">You learned how to use a partial as a template so that you can format each database record.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="44b9e-225">[前へ](creating-model-classes-with-linq-to-sql-vb.md)
[次へ](performing-simple-validation-vb.md)</span><span class="sxs-lookup"><span data-stu-id="44b9e-225">[Previous](creating-model-classes-with-linq-to-sql-vb.md)
[Next](performing-simple-validation-vb.md)</span></span>
