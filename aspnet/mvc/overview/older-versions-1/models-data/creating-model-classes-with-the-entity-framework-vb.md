---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-vb
title: Entity Framework (VB) でモデル クラスを作成する |Microsoft ドキュメント
author: microsoft
description: このチュートリアルでは、Microsoft の Entity Framework での ASP.NET MVC を使用する方法を学習します。 ウィザードを使用して、エンティティを ADO.NET エンティティ Da を作成する方法を説明するとしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: ff8322c9-12f3-4e24-aba6-a38046b9bb0d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-vb
msc.type: authoredcontent
ms.openlocfilehash: 3442435c7b2b9ce2ce6bd016ba74fe671eb76f62
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874448"
---
<a name="creating-model-classes-with-the-entity-framework-vb"></a><span data-ttu-id="7e7a7-104">Entity Framework (VB) でモデル クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-104">Creating Model Classes with the Entity Framework (VB)</span></span>
====================
<span data-ttu-id="7e7a7-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="7e7a7-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="7e7a7-106">このチュートリアルでは、Microsoft の Entity Framework での ASP.NET MVC を使用する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-106">In this tutorial, you learn how to use ASP.NET MVC with the Microsoft Entity Framework.</span></span> <span data-ttu-id="7e7a7-107">ウィザードを使用して、エンティティを ADO.NET エンティティ データ モデルを作成する方法を学びます。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-107">You learn how to use the Entity Wizard to create an ADO.NET Entity Data Model.</span></span> <span data-ttu-id="7e7a7-108">このチュートリアルの過程で、ここを選択、挿入、更新、および Entity Framework を使用してデータベースのデータを削除する方法について説明する web アプリケーションを構築します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-108">Over the course of this tutorial, we build a web application that illustrates how to select, insert, update, and delete database data by using the Entity Framework.</span></span>


<span data-ttu-id="7e7a7-109">このチュートリアルの目的では、ASP.NET MVC アプリケーションを構築するときに、Microsoft の Entity Framework を使用してデータ アクセス クラスを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-109">The goal of this tutorial is to explain how you can create data access classes using the Microsoft Entity Framework when building an ASP.NET MVC application.</span></span> <span data-ttu-id="7e7a7-110">このチュートリアルは、Microsoft の Entity Framework の以前の知識を負いません。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-110">This tutorial assumes no previous knowledge of the Microsoft Entity Framework.</span></span> <span data-ttu-id="7e7a7-111">このチュートリアルの目的は、Entity Framework を使用して、選択、挿入、更新、およびデータベースのレコードを削除する方法を理解します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-111">By the end of this tutorial, you'll understand how to use the Entity Framework to select, insert, update, and delete database records.</span></span>

<span data-ttu-id="7e7a7-112">Microsoft の Entity Framework は、オブジェクト リレーショナル マッピング (O/RM) ツールをデータベースからデータ アクセス層を自動的に生成することができます。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-112">The Microsoft Entity Framework is an Object Relational Mapping (O/RM) tool that enables you to generate a data access layer from a database automatically.</span></span> <span data-ttu-id="7e7a7-113">Entity Framework では、データ アクセス クラスを手動で構築の面倒な作業を回避することができます。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-113">The Entity Framework enables you to avoid the tedious work of building your data access classes by hand.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="7e7a7-114">ASP.NET MVC と Microsoft Entity Framework の不可欠な接続がありません。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-114">There is no essential connection between ASP.NET MVC and the Microsoft Entity Framework.</span></span> <span data-ttu-id="7e7a7-115">ASP.NET MVC で使用できるエンティティ フレームワークのいくつかの選択肢があります。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-115">There are several alternatives to the Entity Framework that you can use with ASP.NET MVC.</span></span> <span data-ttu-id="7e7a7-116">たとえば、SQL、NHibernate または SubSonic Microsoft LINQ などその他の O/RM ツールを使用して MVC モデル クラスをビルドすることができます。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-116">For example, you can build your MVC Model classes using other O/RM tools such as Microsoft LINQ to SQL, NHibernate, or SubSonic.</span></span>


<span data-ttu-id="7e7a7-117">ASP.NET MVC で Microsoft Entity Framework を使用する方法がわかるようにするためにここには、簡単なサンプル アプリケーションを構築します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-117">In order to illustrate how you can use the Microsoft Entity Framework with ASP.NET MVC, we'll build a simple sample application.</span></span> <span data-ttu-id="7e7a7-118">ムービーのデータベース アプリケーションを表示し、ムービーのデータベース レコードを編集することができますを作成します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-118">We'll create a Movie Database application that enables you to display and edit movie database records.</span></span>

<span data-ttu-id="7e7a7-119">このチュートリアルでは、Visual Studio 2008 または Visual Web Developer 2008 Service Pack 1 があることを前提としています。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-119">This tutorial assumes that you have Visual Studio 2008 or Visual Web Developer 2008 with Service Pack 1.</span></span> <span data-ttu-id="7e7a7-120">Entity Framework を使用するために Service Pack 1 を必要とします。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-120">You need Service Pack 1 in order to use the Entity Framework.</span></span> <span data-ttu-id="7e7a7-121">Visual Studio 2008 Service Pack 1 または Service Pack 1 の Visual Web Developer は、次のアドレスからダウンロードできます。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-121">You can download Visual Studio 2008 Service Pack 1 or Visual Web Developer with Service Pack 1 from the following address:</span></span>

> [https://www.asp.net/downloads/](https://www.asp.net/downloads)


## <a name="creating-the-movie-sample-database"></a><span data-ttu-id="7e7a7-122">ムービーのサンプル データベースの作成</span><span class="sxs-lookup"><span data-stu-id="7e7a7-122">Creating the Movie Sample Database</span></span>

<span data-ttu-id="7e7a7-123">ムービーのデータベース アプリケーションでは、ムービーをという名前を次の列を含むデータベース テーブルを使用します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-123">The Movie Database application uses a database table named Movies that contains the following columns:</span></span>

| <span data-ttu-id="7e7a7-124">列名</span><span class="sxs-lookup"><span data-stu-id="7e7a7-124">Column Name</span></span> | <span data-ttu-id="7e7a7-125">データの種類</span><span class="sxs-lookup"><span data-stu-id="7e7a7-125">Data Type</span></span> | <span data-ttu-id="7e7a7-126">Null 値を許可しますか。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-126">Allow Nulls?</span></span> | <span data-ttu-id="7e7a7-127">プライマリ キーとは</span><span class="sxs-lookup"><span data-stu-id="7e7a7-127">Is Primary Key?</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="7e7a7-128">ID</span><span class="sxs-lookup"><span data-stu-id="7e7a7-128">Id</span></span> | <span data-ttu-id="7e7a7-129">int</span><span class="sxs-lookup"><span data-stu-id="7e7a7-129">int</span></span> | <span data-ttu-id="7e7a7-130">False</span><span class="sxs-lookup"><span data-stu-id="7e7a7-130">False</span></span> | <span data-ttu-id="7e7a7-131">True</span><span class="sxs-lookup"><span data-stu-id="7e7a7-131">True</span></span> |
| <span data-ttu-id="7e7a7-132">Title</span><span class="sxs-lookup"><span data-stu-id="7e7a7-132">Title</span></span> | <span data-ttu-id="7e7a7-133">nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="7e7a7-133">nvarchar(100)</span></span> | <span data-ttu-id="7e7a7-134">False</span><span class="sxs-lookup"><span data-stu-id="7e7a7-134">False</span></span> | <span data-ttu-id="7e7a7-135">False</span><span class="sxs-lookup"><span data-stu-id="7e7a7-135">False</span></span> |
| <span data-ttu-id="7e7a7-136">ディレクター</span><span class="sxs-lookup"><span data-stu-id="7e7a7-136">Director</span></span> | <span data-ttu-id="7e7a7-137">nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="7e7a7-137">nvarchar(100)</span></span> | <span data-ttu-id="7e7a7-138">False</span><span class="sxs-lookup"><span data-stu-id="7e7a7-138">False</span></span> | <span data-ttu-id="7e7a7-139">False</span><span class="sxs-lookup"><span data-stu-id="7e7a7-139">False</span></span> |

<span data-ttu-id="7e7a7-140">このテーブルは、次の手順で ASP.NET MVC プロジェクトに追加できます。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-140">You can add this table to an ASP.NET MVC project by following these steps:</span></span>

1. <span data-ttu-id="7e7a7-141">アプリケーションを右クリックして\_メニュー オプションを選択し、ソリューション エクスプ ローラーのウィンドウで、データ フォルダー**追加]、[新しい項目。**</span><span class="sxs-lookup"><span data-stu-id="7e7a7-141">Right-click the App\_Data folder in the Solution Explorer window and select the menu option **Add, New Item.**</span></span>
2. <span data-ttu-id="7e7a7-142">**新しい項目の追加**ダイアログ ボックスで、 **SQL Server データベース**名前 MoviesDB.mdf、データベースを指定し、クリックして、**追加**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-142">From the **Add New Item** dialog box, select **SQL Server Database**, give the database the name MoviesDB.mdf, and click the **Add** button.</span></span>
3. <span data-ttu-id="7e7a7-143">サーバー エクスプ ローラー/データベース エクスプ ローラー ウィンドウを開いて MoviesDB.mdf ファイルをダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-143">Double-click the MoviesDB.mdf file to open the Server Explorer/Database Explorer window.</span></span>
4. <span data-ttu-id="7e7a7-144">MoviesDB.mdf データベース接続を展開し、[テーブル] フォルダーを右クリックしてメニュー オプションを選択**新しいテーブルの追加**です。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-144">Expand the MoviesDB.mdf database connection, right-click the Tables folder, and select the menu option **Add New Table**.</span></span>
5. <span data-ttu-id="7e7a7-145">テーブル デザイナーでは、Id、タイトル、およびディレクター列を追加します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-145">In the Table Designer, add the Id, Title, and Director columns.</span></span>
6. <span data-ttu-id="7e7a7-146">をクリックして、**保存**ボタン (フロッピー ディスクのアイコンがある) 名前ムービーと共に、新しいテーブルを保存します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-146">Click the **Save** button (it has the icon of the floppy) to save the new table with the name Movies.</span></span>

<span data-ttu-id="7e7a7-147">映画のデータベース テーブルを作成した後は、テーブルにサンプル データを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-147">After you create the Movies database table, you should add some sample data to the table.</span></span> <span data-ttu-id="7e7a7-148">映画テーブルを右クリックし、メニュー オプションを選択**テーブル データの表示**です。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-148">Right-click the Movies table and select the menu option **Show Table Data**.</span></span> <span data-ttu-id="7e7a7-149">表示されたグリッドには、偽のムービー データを入力できます。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-149">You can enter fake movie data into the grid that appears.</span></span>

## <a name="creating-the-adonet-entity-data-model"></a><span data-ttu-id="7e7a7-150">ADO.NET エンティティ データ モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-150">Creating the ADO.NET Entity Data Model</span></span>

<span data-ttu-id="7e7a7-151">Entity Framework を使用するのには、Entity Data Model を作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-151">In order to use the Entity Framework, you need to create an Entity Data Model.</span></span> <span data-ttu-id="7e7a7-152">Visual Studio の活用*Entity Data Model ウィザード*エンティティ データ モデルをデータベースから自動的に生成します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-152">You can take advantage of the Visual Studio *Entity Data Model Wizard* to generate an Entity Data Model from a database automatically.</span></span>

<span data-ttu-id="7e7a7-153">この場合は、以下の手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-153">Follow these steps:</span></span>

1. <span data-ttu-id="7e7a7-154">ソリューション エクスプ ローラー ウィンドウで モデル フォルダーを右クリックし、メニュー オプションを選択**追加、新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-154">Right-click the Models folder in the Solution Explorer window and select the menu option **Add, New Item**.</span></span>
2. <span data-ttu-id="7e7a7-155">**新しい項目の追加**ダイアログ ボックスで、データ カテゴリを選択 (図 1 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-155">In the **Add New Item** dialog, select the Data category (see Figure 1).</span></span>
3. <span data-ttu-id="7e7a7-156">選択、 **ADO.NET エンティティ データ モデル**テンプレート、名前、Entity Data Model、MoviesDBModel.edmx、しをクリックして、**追加**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-156">Select the **ADO.NET Entity Data Model** template, give the Entity Data Model the name MoviesDBModel.edmx, and click the **Add** button.</span></span> <span data-ttu-id="7e7a7-157">クリックすると、**追加**ボタンは、データ モデル ウィザードを起動します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-157">Clicking the **Add** button launches the Data Model Wizard.</span></span>
4. <span data-ttu-id="7e7a7-158">**[モデルのコンテンツ**ステップで、選択、**データベースから生成**オプションをクリックして、 **[次へ]** (図 2 を参照してください)] ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-158">In the **Choose Model Contents** step, choose the **Generate from a database** option and click the **Next** button (see Figure 2).</span></span>
5. <span data-ttu-id="7e7a7-159">**データ接続の選択**ステップ、MoviesDB.mdf データベース接続を選択し、接続の設定が MoviesDBEntities の名前を指定しをクリックしてエンティティを入力、**次**(図 3 を参照してください) ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-159">In the **Choose Your Data Connection** step, select the MoviesDB.mdf database connection, enter the entities connection settings name MoviesDBEntities, and click the **Next** button (see Figure 3).</span></span>
6. <span data-ttu-id="7e7a7-160">**データベース オブジェクトの選択**ステップ、ムービーのデータベース テーブルを選択してをクリックして、**完了**(図 4 を参照してください) ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-160">In the **Choose Your Database Objects** step, select the Movie database table and click the **Finish** button (see Figure 4).</span></span>

<span data-ttu-id="7e7a7-161">次の手順を完了すると、ADO.NET Entity Data Model デザイナー (エンティティ デザイナー) が開きます。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-161">After you complete these steps, the ADO.NET Entity Data Model Designer (Entity Designer) opens.</span></span>

<span data-ttu-id="7e7a7-162">**図 1 – 新しいエンティティ データ モデルを作成します。**</span><span class="sxs-lookup"><span data-stu-id="7e7a7-162">**Figure 1 – Creating a new Entity Data Model**</span></span>

![clip_image002](creating-model-classes-with-the-entity-framework-vb/_static/image1.jpg)

<span data-ttu-id="7e7a7-164">**図 2 – モデル内容のステップを選択**</span><span class="sxs-lookup"><span data-stu-id="7e7a7-164">**Figure 2 – Choose Model Contents Step**</span></span>

![clip_image004](creating-model-classes-with-the-entity-framework-vb/_static/image2.jpg)

<span data-ttu-id="7e7a7-166">**図 3 – データ接続の選択**</span><span class="sxs-lookup"><span data-stu-id="7e7a7-166">**Figure 3 – Choose Your Data Connection**</span></span>

![clip_image006](creating-model-classes-with-the-entity-framework-vb/_static/image3.jpg)

<span data-ttu-id="7e7a7-168">**図 4-データベース オブジェクトの選択**</span><span class="sxs-lookup"><span data-stu-id="7e7a7-168">**Figure 4 – Choose Your Database Objects**</span></span>

![clip_image008](creating-model-classes-with-the-entity-framework-vb/_static/image4.jpg)

## <a name="modifying-the-adonet-entity-data-model"></a><span data-ttu-id="7e7a7-170">ADO.NET Entity Data Model を変更します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-170">Modifying the ADO.NET Entity Data Model</span></span>

<span data-ttu-id="7e7a7-171">エンティティ デザイナーを利用して、モデルを変更するには Entity Data Model を作成した後 (図 5 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-171">After you create an Entity Data Model, you can modify the model by taking advantage of the Entity Designer (see Figure 5).</span></span> <span data-ttu-id="7e7a7-172">ソリューション エクスプ ローラー ウィンドウ内で、Models フォルダーに含まれている MoviesDBModel.edmx ファイルをダブルクリックして、いつでも、エンティティ デザイナーを開くことができます。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-172">You can open the Entity Designer at any time by double-clicking the MoviesDBModel.edmx file contained in the Models folder within the Solution Explorer window.</span></span>

<span data-ttu-id="7e7a7-173">**図 5 – に ADO.NET Entity Data Model デザイナー**</span><span class="sxs-lookup"><span data-stu-id="7e7a7-173">**Figure 5 – The ADO.NET Entity Data Model Designer**</span></span>

![clip_image010](creating-model-classes-with-the-entity-framework-vb/_static/image5.jpg)

<span data-ttu-id="7e7a7-175">たとえば、エンティティ デザイナーを使用して、エンティティ モデルのデータ ウィザードで生成されるクラスの名前を変更することができます。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-175">For example, you can use the Entity Designer to change the names of the classes that the Entity Model Data Wizard generates.</span></span> <span data-ttu-id="7e7a7-176">ウィザードでは、ムービーをという名前の新しいデータ アクセス クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-176">The Wizard created a new data access class named Movies.</span></span> <span data-ttu-id="7e7a7-177">つまり、ウィザードは、非常に同じ名前のデータベース テーブル クラスできました。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-177">In other words, the Wizard gave the class the very same name as the database table.</span></span> <span data-ttu-id="7e7a7-178">おをこのクラスを使用して、ムービーの特定のインスタンスを表す、ためムービーからクラスをムービーにお名前を変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-178">Because we will use this class to represent a particular Movie instance, we should rename the class from Movies to Movie.</span></span>

<span data-ttu-id="7e7a7-179">エンティティ クラスの名前を変更するには、エンティティ デザイナーでクラス名をダブルクリックし、新しい名前を入力することができます (図 6 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-179">If you want to rename an entity class, you can double-click on the class name in the Entity Designer and enter a new name (see Figure 6).</span></span> <span data-ttu-id="7e7a7-180">また、エンティティ デザイナーでエンティティを選択した後、[プロパティ] ウィンドウ内のエンティティの名前を変更できます。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-180">Alternatively, you can change the name of an entity in the Properties window after selecting an entity in the Entity Designer.</span></span>

<span data-ttu-id="7e7a7-181">**図 6 – エンティティ名を変更します。**</span><span class="sxs-lookup"><span data-stu-id="7e7a7-181">**Figure 6 – Changing an entity name**</span></span>

![clip_image012](creating-model-classes-with-the-entity-framework-vb/_static/image6.jpg)

<span data-ttu-id="7e7a7-183">[保存] ボタン (フロッピー ディスクのアイコン) をクリックして、変更を行った後、エンティティ データ モデルを保存してください。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-183">Remember to save your Entity Data Model after making a modification by clicking the Save button (the icon of the floppy disk).</span></span> <span data-ttu-id="7e7a7-184">バック グラウンドでは、エンティティ デザイナーは、Visual Basic .NET クラスのセットを生成します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-184">Behind the scenes, the Entity Designer generates a set of Visual Basic .NET classes.</span></span> <span data-ttu-id="7e7a7-185">これらのクラスを表示するには、ソリューション エクスプ ローラー ウィンドウから MoviesDBModel.Designer.vb ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-185">You can view these classes by opening the MoviesDBModel.Designer.vb file from the Solution Explorer window.</span></span>


<span data-ttu-id="7e7a7-186">次に、エンティティ デザイナーを使用するときに、変更が上書きされますのでがファイル内のコードを変更しないでください。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-186">Don't modify the code in the Designer.vb file since your changes will be overwritten the next time you use the Entity Designer.</span></span> <span data-ttu-id="7e7a7-187">ファイルで定義されたエンティティ クラスの機能を拡張するかどうかは、作成することができます*部分クラス*で個別のファイルです。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-187">If you want to extend the functionality of the entity classes defined in the Designer.vb file then you can create *partial classes* in separate files.</span></span>


#### <a name="selecting-database-records-with-the-entity-framework"></a><span data-ttu-id="7e7a7-188">Entity Framework でのデータベース レコードを選択します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-188">Selecting Database Records with the Entity Framework</span></span>

<span data-ttu-id="7e7a7-189">ムービーのレコードの一覧を表示するページを作成することで、ムービーのデータベース アプリケーションの構築を開始しましょう。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-189">Let's start building our Movie Database application by creating a page that displays a list of movie records.</span></span> <span data-ttu-id="7e7a7-190">1 の一覧で、Home コント ローラーは、Index() をという名前のアクションを公開します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-190">The Home controller in Listing 1 exposes an action named Index().</span></span> <span data-ttu-id="7e7a7-191">Index() アクションに、ムービーのデータベース テーブルからのムービー レコードはすべて、Entity Framework を活用してに返します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-191">The Index() action returns all of the movie records from the Movie database table by taking advantage of the Entity Framework.</span></span>

<span data-ttu-id="7e7a7-192">**1 – Controllers\HomeController.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="7e7a7-192">**Listing 1 – Controllers\HomeController.vb**</span></span>

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample1.vb)]

<span data-ttu-id="7e7a7-193">リスト 1 のコント ローラーに、コンス トラクターが含まれることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-193">Notice that the controller in Listing 1 includes a constructor.</span></span> <span data-ttu-id="7e7a7-194">コンス トラクターは、という名前のクラス レベル フィールド\_db します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-194">The constructor initializes a class-level field named \_db.</span></span> <span data-ttu-id="7e7a7-195">\_Db フィールドは、Microsoft の Entity Framework によって生成されるデータベース エンティティを表します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-195">The \_db field represents the database entities generated by the Microsoft Entity Framework.</span></span> <span data-ttu-id="7e7a7-196">\_Db フィールドがエンティティ デザイナーで生成された MoviesDBEntities クラスのインスタンス。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-196">The \_db field is an instance of the MoviesDBEntities class that was generated by the Entity Designer.</span></span>

<span data-ttu-id="7e7a7-197">\_Db フィールドは、ムービーのデータベース テーブルからレコードを取得する Index() 操作内で使用します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-197">The \_db field is used within the Index() action to retrieve the records from the Movies database table.</span></span> <span data-ttu-id="7e7a7-198">式\_db します。MovieSet は、ムービーのデータベース テーブルのすべてのレコードを表します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-198">The expression \_db.MovieSet represents all of the records from the Movies database table.</span></span> <span data-ttu-id="7e7a7-199">ToList() メソッドを使用して、ムービーのセットに映画オブジェクトのジェネリック コレクション変換: (のムービー) ボックスの一覧です。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-199">The ToList() method is used to convert the set of movies into a generic collection of Movie objects: List( Of Movie).</span></span>

<span data-ttu-id="7e7a7-200">ムービーのレコードは、LINQ to Entities を利用して取得されます。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-200">The movie records are retrieved with the help of LINQ to Entities.</span></span> <span data-ttu-id="7e7a7-201">LINQ を使用するリスト 1 の Index() アクション*メソッド構文*データベース レコードのセットを取得します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-201">The Index() action in Listing 1 uses LINQ *method syntax* to retrieve the set of database records.</span></span> <span data-ttu-id="7e7a7-202">LINQ を使用することができる場合は、*クエリの構文*代わりにします。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-202">If you prefer, you can use LINQ *query syntax* instead.</span></span> <span data-ttu-id="7e7a7-203">次の 2 つのステートメントでは、同じ操作を行います。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-203">The following two statements do the very same thing:</span></span>

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample2.vb)]

<span data-ttu-id="7e7a7-204">どの LINQ 構文を使用 – メソッド構文またはクエリの構文: 最も直観的な表示されています。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-204">Use whichever LINQ syntax – method syntax or query syntax – that you find most intuitive.</span></span> <span data-ttu-id="7e7a7-205">2 つの方法のパフォーマンスに違いはありません: 唯一の違いはスタイル。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-205">There is no difference in performance between the two approaches – the only difference is style.</span></span>

<span data-ttu-id="7e7a7-206">2 を一覧表示するビューを使用すると、ムービーのレコードを表示します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-206">The view in Listing 2 is used to display the movie records.</span></span>

<span data-ttu-id="7e7a7-207">**Listing 2 – Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="7e7a7-207">**Listing 2 – Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample3.aspx)]

<span data-ttu-id="7e7a7-208">2 のリスト ビューに含まれる、**ごと**ループをムービーの各レコードを反復処理し、ムービー レコードのタイトルおよびディレクター プロパティの値を表示します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-208">The view in Listing 2 contains a **For Each** loop that iterates through each movie record and displays the values of the movie record's Title and Director properties.</span></span> <span data-ttu-id="7e7a7-209">各レコードの横にある編集および削除のリンクが表示されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-209">Notice that an Edit and Delete link is displayed next to each record.</span></span> <span data-ttu-id="7e7a7-210">ビューの下部にさらに、ムービーの追加リンクが表示されます (図 7 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-210">Furthermore, an Add Movie link appears at the bottom of the view (see Figure 7).</span></span>

<span data-ttu-id="7e7a7-211">**図 7 –、インデックス ビュー**</span><span class="sxs-lookup"><span data-stu-id="7e7a7-211">**Figure 7 – The Index view**</span></span>

![clip_image014](creating-model-classes-with-the-entity-framework-vb/_static/image7.jpg)

<span data-ttu-id="7e7a7-213">インデックス ビューは、*ビューに型指定された*です。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-213">The Index view is a *typed view*.</span></span> <span data-ttu-id="7e7a7-214">インデックス ビューには、 &lt;% ページ % @&gt; Inherits 属性が含まれるディレクティブです。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-214">The Index view has a &lt;%@ Page %&gt; directive that includes an Inherits attribute.</span></span> <span data-ttu-id="7e7a7-215">Inherits 属性は、厳密に型指定されたジェネリック リスト オブジェクトのコレクションにムービー – 一覧 (のムービー) ViewData.Model プロパティをキャストします。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-215">The Inherits attribute casts the ViewData.Model property to a strongly typed generic List collection of Movie objects – a List(Of Movie).</span></span>

## <a name="inserting-database-records-with-the-entity-framework"></a><span data-ttu-id="7e7a7-216">Entity Framework でのデータベース レコードを挿入します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-216">Inserting Database Records with the Entity Framework</span></span>

<span data-ttu-id="7e7a7-217">Entity Framework を使用すると、データベース テーブルに新しいレコードを挿入しやすきます。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-217">You can use the Entity Framework to make it easy to insert new records into a database table.</span></span> <span data-ttu-id="7e7a7-218">3 を一覧表示するには、ムービーのデータベース テーブルに新しいレコードを挿入する際、ホーム コント ローラー クラスに追加された 2 つの新しいアクションが含まれます。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-218">Listing 3 contains two new actions added to the Home controller class that you can use to insert new records into the Movie database table.</span></span>

<span data-ttu-id="7e7a7-219">**3 – Controllers\HomeController.vb (メソッドの追加) を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="7e7a7-219">**Listing 3 – Controllers\HomeController.vb (Add methods)**</span></span>

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample4.vb)]

<span data-ttu-id="7e7a7-220">Add() の最初のアクションは、単にビューを返します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-220">The first Add() action simply returns a view.</span></span> <span data-ttu-id="7e7a7-221">ビューには、新しいムービーのデータベースを追加するためのフォームが含まれています (図 8 を参照してください) を記録します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-221">The view contains a form for adding a new movie database record (see Figure 8).</span></span> <span data-ttu-id="7e7a7-222">フォームを送信するときに、2 番目の Add() アクションが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-222">When you submit the form, the second Add() action is invoked.</span></span>

<span data-ttu-id="7e7a7-223">2 番目の Add() アクションが AcceptVerbs 属性で修飾されたことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-223">Notice that the second Add() action is decorated with the AcceptVerbs attribute.</span></span> <span data-ttu-id="7e7a7-224">HTTP POST 操作を実行する場合にのみ、この操作を呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-224">This action can be invoked only when performing an HTTP POST operation.</span></span> <span data-ttu-id="7e7a7-225">つまり、この操作は、HTML フォームを送信する場合にのみ呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-225">In other words, this action can only be invoked when posting an HTML form.</span></span>

<span data-ttu-id="7e7a7-226">2 番目の Add() アクションは、ASP.NET MVC TryUpdateModel() メソッドのヘルプを Entity Framework ムービー クラスの新しいインスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-226">The second Add() action creates a new instance of the Entity Framework Movie class with the help of the ASP.NET MVC TryUpdateModel() method.</span></span> <span data-ttu-id="7e7a7-227">TryUpdateModel() メソッドは Add() メソッドに渡される FormCollection でフィールドを受け取りし、ムービー クラスに HTML フォーム フィールドの値を割り当てます。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-227">The TryUpdateModel() method takes the fields in the FormCollection passed to the Add() method and assigns the values of these HTML form fields to the Movie class.</span></span>


<span data-ttu-id="7e7a7-228">Entity Framework を使用する場合は、TryUpdateModel または UpdateModel メソッドを使用してエンティティ クラスのプロパティを更新する場合、"空白"プロパティの一覧を指定してください。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-228">When using the Entity Framework, you must supply a "white list" of properties when using the TryUpdateModel or UpdateModel methods to update the properties of an entity class.</span></span>


<span data-ttu-id="7e7a7-229">次に、Add() アクションでは、いくつかの簡単なフォーム検証を実行します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-229">Next, the Add() action performs some simple form validation.</span></span> <span data-ttu-id="7e7a7-230">アクションは、タイトルとディレクターの両方のプロパティに値があることを確認します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-230">The action verifies that both the Title and Director properties have values.</span></span> <span data-ttu-id="7e7a7-231">検証エラーがある場合、検証エラー メッセージは ModelState に追加されます。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-231">If there is a validation error, then a validation error message is added to ModelState.</span></span>

<span data-ttu-id="7e7a7-232">検証エラーがない場合、新しいムービーのレコードは、Entity Framework を利用して、ムービー データベース テーブルに追加されます。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-232">If there are no validation errors then a new movie record is added to the Movies database table with the help of the Entity Framework.</span></span> <span data-ttu-id="7e7a7-233">新しいレコードは、次の 2 つの行のコードで、データベースに追加されます。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-233">The new record is added to the database with the following two lines of code:</span></span>

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample5.vb)]

<span data-ttu-id="7e7a7-234">コードの最初の行は、Entity Framework によって追跡されている映画のセットに映画の新しいエンティティを追加します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-234">The first line of code adds the new Movie entity to the set of movies being tracked by the Entity Framework.</span></span> <span data-ttu-id="7e7a7-235">コードの 2 行目では、基になるデータベースに追跡されている映画にどのような変更が加えられたを保存します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-235">The second line of code saves whatever changes have been made to the Movies being tracked back to the underlying database.</span></span>

<span data-ttu-id="7e7a7-236">**図 8-追加ビュー**</span><span class="sxs-lookup"><span data-stu-id="7e7a7-236">**Figure 8 – The Add view**</span></span>

![clip_image016](creating-model-classes-with-the-entity-framework-vb/_static/image8.jpg)

## <a name="updating-database-records-with-the-entity-framework"></a><span data-ttu-id="7e7a7-238">Entity Framework でのデータベース レコードの更新</span><span class="sxs-lookup"><span data-stu-id="7e7a7-238">Updating Database Records with the Entity Framework</span></span>

<span data-ttu-id="7e7a7-239">新しいデータベース レコードを挿入するだけに従うと、アプローチとして、Entity Framework とデータベース レコードを編集するほぼ同じアプローチを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-239">You can follow almost the same approach to edit a database record with the Entity Framework as the approach that we just followed to insert a new database record.</span></span> <span data-ttu-id="7e7a7-240">4 を一覧表示するには、次の 2 つの新しいコント ローラー アクション Edit() をという名前が含まれます。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-240">Listing 4 contains two new controller actions named Edit().</span></span> <span data-ttu-id="7e7a7-241">最初の Edit() アクションでは、ムービーのレコードを編集するため、HTML フォームを返します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-241">The first Edit() action returns an HTML form for editing a movie record.</span></span> <span data-ttu-id="7e7a7-242">2 番目の Edit() アクションは、データベースを更新しようとします。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-242">The second Edit() action attempts to update the database.</span></span>

<span data-ttu-id="7e7a7-243">**4 – Controllers\HomeController.vb (編集メソッド) を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="7e7a7-243">**Listing 4 – Controllers\HomeController.vb (Edit methods)**</span></span>

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample6.vb)]

<span data-ttu-id="7e7a7-244">2 番目の Edit() アクションは、編集されている映画の Id に一致するデータベースからムービー レコードを取得することによって開始されます。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-244">The second Edit() action starts by retrieving the Movie record from the database that matches the Id of the movie being edited.</span></span> <span data-ttu-id="7e7a7-245">エンティティ ステートメントに次の LINQ では、特定の Id と一致する最初のデータベース レコードを取得します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-245">The following LINQ to Entities statement grabs the first database record that matches a particular Id:</span></span>

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample7.vb)]

<span data-ttu-id="7e7a7-246">次に、TryUpdateModel() メソッドを使用して、ムービーのエンティティのプロパティには HTML フォームのフィールドの値を割り当てます。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-246">Next, the TryUpdateModel() method is used to assign the values of the HTML form fields to the properties of the movie entity.</span></span> <span data-ttu-id="7e7a7-247">更新する正確なプロパティを指定する、ホワイト リストを提供することを確認します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-247">Notice that a white list is supplied to specify the exact properties to update.</span></span>

<span data-ttu-id="7e7a7-248">次に、いくつかの単純な検証を実行して、ムービー タイトルとディレクターの両方のプロパティに値があることを確認します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-248">Next, some simple validation is performed to verify that both the Movie Title and Director properties have values.</span></span> <span data-ttu-id="7e7a7-249">いずれかのプロパティの値がない場合は、ModelState に検証エラー メッセージを追加し、ModelState.IsValid 値 false が返されます。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-249">If either property is missing a value, then a validation error message is added to ModelState and ModelState.IsValid returns the value false.</span></span>

<span data-ttu-id="7e7a7-250">最後に、検証エラーがない場合は、基になる映画データベース テーブルが更新で変更された SaveChanges() メソッドを呼び出すことによってです。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-250">Finally, if there are no validation errors, then the underlying Movies database table is updated with any changes by calling the SaveChanges() method.</span></span>

<span data-ttu-id="7e7a7-251">データベース レコードを編集するときは、データベースの更新を実行するコント ローラーのアクションを編集中のレコードの Id を渡す必要があります。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-251">When editing database records, you need to pass the Id of the record being edited to the controller action that performs the database update.</span></span> <span data-ttu-id="7e7a7-252">それ以外の場合、コント ローラーのアクションでは、基になるデータベースで更新するためにどのレコードがわかりません。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-252">Otherwise, the controller action will not know which record to update in the underlying database.</span></span> <span data-ttu-id="7e7a7-253">5 の一覧に含まれている、編集ビューには、編集中のデータベース レコードの Id を表す隠しフォーム フィールドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-253">The Edit view, contained in Listing 5, includes a hidden form field that represents the Id of the database record being edited.</span></span>

<span data-ttu-id="7e7a7-254">**Listing 5 – Views\Home\Edit.aspx**</span><span class="sxs-lookup"><span data-stu-id="7e7a7-254">**Listing 5 – Views\Home\Edit.aspx**</span></span>

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample8.aspx)]

## <a name="deleting-database-records-with-the-entity-framework"></a><span data-ttu-id="7e7a7-255">Entity Framework でのデータベース レコードを削除します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-255">Deleting Database Records with the Entity Framework</span></span>

<span data-ttu-id="7e7a7-256">このチュートリアルで取り組む必要があります、最終的なデータベースの操作は、データベース レコードを削除しています。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-256">The final database operation, which we need to tackle in this tutorial, is deleting database records.</span></span> <span data-ttu-id="7e7a7-257">リスト 6 でコント ローラーのアクションを使用すると、特定のデータベース レコードを削除します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-257">You can use the controller action in Listing 6 to delete a particular database record.</span></span>

<span data-ttu-id="7e7a7-258">**Listing 6 -- \Controllers\HomeController.vb (Delete action)**</span><span class="sxs-lookup"><span data-stu-id="7e7a7-258">**Listing 6 -- \Controllers\HomeController.vb (Delete action)**</span></span>

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample9.vb)]

<span data-ttu-id="7e7a7-259">Delete() 操作は、アクションに渡される Id と一致するエンティティ、ムービーを最初に取得します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-259">The Delete() action first retrieves the Movie entity that matches the Id passed to the action.</span></span> <span data-ttu-id="7e7a7-260">次に、ムービーは SaveChanges() メソッドを続けて DeleteObject() メソッドを呼び出すことによって、データベースから削除されます。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-260">Next, the movie is deleted from the database by calling the DeleteObject() method followed by the SaveChanges() method.</span></span> <span data-ttu-id="7e7a7-261">最後に、インデックス ビューに戻る、ユーザーがリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-261">Finally, the user is redirected back to the Index view.</span></span>

## <a name="summary"></a><span data-ttu-id="7e7a7-262">まとめ</span><span class="sxs-lookup"><span data-stu-id="7e7a7-262">Summary</span></span>

<span data-ttu-id="7e7a7-263">このチュートリアルの目的は、ASP.NET MVC および Microsoft Entity Framework を活用して、データベースに基づく web アプリケーションを構築する方法を示すためにしました。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-263">The purpose of this tutorial was to demonstrate how you can build database-driven web applications by taking advantage of ASP.NET MVC and the Microsoft Entity Framework.</span></span> <span data-ttu-id="7e7a7-264">使用すると、選択、挿入、更新、アプリケーションを構築およびデータベースのレコードを削除する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-264">You learned how to build an application that enables you to select, insert, update, and delete database records.</span></span>

<span data-ttu-id="7e7a7-265">最初に、Entity Data Model ウィザードを使用して、Visual Studio 内からのエンティティ データ モデルを生成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-265">First, we discussed how you can use the Entity Data Model Wizard to generate an Entity Data Model from within Visual Studio.</span></span> <span data-ttu-id="7e7a7-266">次に、LINQ to Entities を使用して、データベース テーブルから特定のデータベース レコード セットを取得する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-266">Next, you learn how to use LINQ to Entities to retrieve a set of database records from a database table.</span></span> <span data-ttu-id="7e7a7-267">最後に、Entity Framework を使用して挿入、更新、およびデータベースのレコードを削除しました。</span><span class="sxs-lookup"><span data-stu-id="7e7a7-267">Finally, we used the Entity Framework to insert, update, and delete database records.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7e7a7-268">[前へ](validation-with-the-data-annotation-validators-cs.md)
> [次へ](creating-model-classes-with-linq-to-sql-vb.md)</span><span class="sxs-lookup"><span data-stu-id="7e7a7-268">[Previous](validation-with-the-data-annotation-validators-cs.md)
[Next](creating-model-classes-with-linq-to-sql-vb.md)</span></span>
