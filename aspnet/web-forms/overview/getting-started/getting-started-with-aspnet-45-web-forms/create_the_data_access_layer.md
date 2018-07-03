---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
title: データ アクセス層の作成 |Microsoft Docs
author: Erikre
description: このチュートリアル シリーズでは、私たちの ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 0bbf7a6e-d7eb-4091-91e4-fff892777f32
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
msc.type: authoredcontent
ms.openlocfilehash: 5b4bb5bc89938836bc37e8ebd385fa966bc6f511
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390037"
---
<a name="create-the-data-access-layer"></a><span data-ttu-id="a9577-103">データ アクセス層を作成します。</span><span class="sxs-lookup"><span data-stu-id="a9577-103">Create the Data Access Layer</span></span>
====================
<span data-ttu-id="a9577-104">によって[Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="a9577-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="a9577-105">[Wingtip Toys のサンプル プロジェクト (c#) をダウンロード](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)または[電子書籍 (PDF) をダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="a9577-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="a9577-106">このチュートリアル シリーズでは、Web 用 ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明します。</span><span class="sxs-lookup"><span data-stu-id="a9577-106">This tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="a9577-107">Visual Studio 2013[プロジェクトと c# ソース コード](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)このチュートリアル シリーズをと共に使用できます。</span><span class="sxs-lookup"><span data-stu-id="a9577-107">A Visual Studio 2013 [project with C# source code](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) is available to accompany this tutorial series.</span></span>


<span data-ttu-id="a9577-108">このチュートリアルでは、作成、アクセス、および ASP.NET Web フォームと Entity Framework Code First を使用してデータベースからデータを確認する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="a9577-108">This tutorial describes how to create, access, and review data from a database using ASP.NET Web Forms and Entity Framework Code First.</span></span> <span data-ttu-id="a9577-109">このチュートリアルでは、「プロジェクトを作成する」前のチュートリアルに基づいており、Wingtip 玩具店のチュートリアル シリーズのパートです。</span><span class="sxs-lookup"><span data-stu-id="a9577-109">This tutorial builds on the previous tutorial "Create the Project" and is part of the Wingtip Toy Store tutorial series.</span></span> <span data-ttu-id="a9577-110">内にあるデータ アクセス クラスのグループが作成したが、このチュートリアルを完了すると、*モデル*プロジェクトのフォルダー。</span><span class="sxs-lookup"><span data-stu-id="a9577-110">When you've completed this tutorial, you will have built a group of data-access classes that are in the *Models* folder of the project.</span></span>

## <a name="what-youll-learn"></a><span data-ttu-id="a9577-111">学習内容。</span><span class="sxs-lookup"><span data-stu-id="a9577-111">What you'll learn:</span></span>

- <span data-ttu-id="a9577-112">データ モデルを作成する方法。</span><span class="sxs-lookup"><span data-stu-id="a9577-112">How to create the data models.</span></span>
- <span data-ttu-id="a9577-113">初期化し、データベースをシードする方法。</span><span class="sxs-lookup"><span data-stu-id="a9577-113">How to initialize and seed the database.</span></span>
- <span data-ttu-id="a9577-114">更新、およびデータベースをサポートするアプリケーションを構成する方法。</span><span class="sxs-lookup"><span data-stu-id="a9577-114">How to update and configure the application to support the database.</span></span>

### <a name="these-are-the-features-introduced-in-the-tutorial"></a><span data-ttu-id="a9577-115">このチュートリアルで導入された機能を次に示します。</span><span class="sxs-lookup"><span data-stu-id="a9577-115">These are the features introduced in the tutorial:</span></span>

- <span data-ttu-id="a9577-116">エンティティ フレームワーク コード ファースト</span><span class="sxs-lookup"><span data-stu-id="a9577-116">Entity Framework Code First</span></span>
- <span data-ttu-id="a9577-117">LocalDB</span><span class="sxs-lookup"><span data-stu-id="a9577-117">LocalDB</span></span>
- <span data-ttu-id="a9577-118">データの注釈</span><span class="sxs-lookup"><span data-stu-id="a9577-118">Data Annotations</span></span>

## <a name="creating-the-data-models"></a><span data-ttu-id="a9577-119">データ モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="a9577-119">Creating the Data Models</span></span>

<span data-ttu-id="a9577-120">[Entity Framework](https://msdn.microsoft.com/data/aa937723)はオブジェクト リレーショナル マッピング (ORM) フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="a9577-120">[Entity Framework](https://msdn.microsoft.com/data/aa937723) is an object-relational mapping (ORM) framework.</span></span> <span data-ttu-id="a9577-121">通常記述する必要のあるデータ アクセス コードの大半を排除することをオブジェクトとしてのリレーショナル データを操作できます。</span><span class="sxs-lookup"><span data-stu-id="a9577-121">It lets you work with relational data as objects, eliminating most of the data-access code that you'd usually need to write.</span></span> <span data-ttu-id="a9577-122">Entity Framework を使用して、使用してクエリを発行する[LINQ](https://msdn.microsoft.com/library/bb397926.aspx)、取得し、厳密に型指定されたオブジェクトとしてデータを操作します。</span><span class="sxs-lookup"><span data-stu-id="a9577-122">Using Entity Framework, you can issue queries using [LINQ](https://msdn.microsoft.com/library/bb397926.aspx), then retrieve and manipulate data as strongly typed objects.</span></span> <span data-ttu-id="a9577-123">LINQ は、クエリを実行して、データを更新するためのパターンを提供します。</span><span class="sxs-lookup"><span data-stu-id="a9577-123">LINQ provides patterns for querying and updating data.</span></span> <span data-ttu-id="a9577-124">Entity Framework を使用してアクセスの基本データに注目するのではなく、アプリケーションの残りの部分を作成に集中することができます。</span><span class="sxs-lookup"><span data-stu-id="a9577-124">Using Entity Framework allows you to focus on creating the rest of your application, rather than focusing on the data access fundamentals.</span></span> <span data-ttu-id="a9577-125">このチュートリアルのシリーズの後半では、データを使用して、ナビゲーションおよび製品のクエリを作成する方法を紹介します。</span><span class="sxs-lookup"><span data-stu-id="a9577-125">Later in this tutorial series, we'll show you how to use the data to populate navigation and product queries.</span></span>

<span data-ttu-id="a9577-126">Entity Framework と呼ばれる開発パラダイムをサポートしている*Code First*します。</span><span class="sxs-lookup"><span data-stu-id="a9577-126">Entity Framework supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="a9577-127">最初のコードではクラスを使用して、データ モデルを定義することができます。</span><span class="sxs-lookup"><span data-stu-id="a9577-127">Code First lets you define your data models using classes.</span></span> <span data-ttu-id="a9577-128">クラスは、コンス トラクターを他の型、メソッドおよびイベントの変数をまとめてグループ化して独自のカスタム型を作成することができます。</span><span class="sxs-lookup"><span data-stu-id="a9577-128">A class is a construct that enables you to create your own custom types by grouping together variables of other types, methods and events.</span></span> <span data-ttu-id="a9577-129">クラスを既存のデータベースにマップしたり、それらを使用してデータベースを生成できます。</span><span class="sxs-lookup"><span data-stu-id="a9577-129">You can map classes to an existing database or use them to generate a database.</span></span> <span data-ttu-id="a9577-130">このチュートリアルでは、データ モデル クラスを記述することで、データ モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="a9577-130">In this tutorial, you'll create the data models by writing data model classes.</span></span> <span data-ttu-id="a9577-131">次に、Entity Framework のこれらの新しいクラスからその場でデータベースを作成できるようにします。</span><span class="sxs-lookup"><span data-stu-id="a9577-131">Then, you'll let Entity Framework create the database on the fly from these new classes.</span></span>

<span data-ttu-id="a9577-132">Web フォーム アプリケーションのデータ モデルを定義するエンティティ クラスを作成して開始されます。</span><span class="sxs-lookup"><span data-stu-id="a9577-132">You will begin by creating the entity classes that define the data models for the Web Forms application.</span></span> <span data-ttu-id="a9577-133">エンティティ クラスを管理し、データベースへのデータ アクセスを提供するコンテキスト クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="a9577-133">Then you will create a context class that manages the entity classes and provides data access to the database.</span></span> <span data-ttu-id="a9577-134">また、データベースの設定を使用する初期化子クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="a9577-134">You will also create an initializer class that you will use to populate the database.</span></span>

### <a name="entity-framework-and-references"></a><span data-ttu-id="a9577-135">Entity Framework と参照</span><span class="sxs-lookup"><span data-stu-id="a9577-135">Entity Framework and References</span></span>

<span data-ttu-id="a9577-136">既定では、Entity Framework が含まれる新しいを作成するときに**ASP.NET Web アプリケーション**を使用して、 **Web フォーム**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="a9577-136">By default, Entity Framework is included when you create a new **ASP.NET Web Application** using the **Web Forms** template.</span></span> <span data-ttu-id="a9577-137">Entity Framework のインストール、アンインストール、および NuGet パッケージとして更新します。</span><span class="sxs-lookup"><span data-stu-id="a9577-137">Entity Framework can be installed, uninstalled, and updated as a NuGet package.</span></span>

<span data-ttu-id="a9577-138">この NuGet パッケージには、次が含まれています。**ランタイム**プロジェクト内のアセンブリ。</span><span class="sxs-lookup"><span data-stu-id="a9577-138">This NuGet package includes the following **runtime** assemblies within your project:</span></span>

- <span data-ttu-id="a9577-139">EntityFramework.dll – Entity Framework で使用されるすべての一般的なランタイム コード</span><span class="sxs-lookup"><span data-stu-id="a9577-139">EntityFramework.dll – All the common runtime code used by Entity Framework</span></span>
- <span data-ttu-id="a9577-140">EntityFramework.SqlServer.dll – Entity Framework 用 Microsoft SQL Server プロバイダー</span><span class="sxs-lookup"><span data-stu-id="a9577-140">EntityFramework.SqlServer.dll – The Microsoft SQL Server provider for Entity Framework</span></span>

### <a name="entity-classes"></a><span data-ttu-id="a9577-141">エンティティ クラス</span><span class="sxs-lookup"><span data-stu-id="a9577-141">Entity Classes</span></span>

<span data-ttu-id="a9577-142">データのスキーマの定義を作成するクラスには、エンティティ クラスが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a9577-142">The classes you create to define the schema of the data are called entity classes.</span></span> <span data-ttu-id="a9577-143">データベースの設計に慣れていない場合は、データベースのテーブルの定義とエンティティ クラスを考えます。</span><span class="sxs-lookup"><span data-stu-id="a9577-143">If you're new to database design, think of the entity classes as table definitions of a database.</span></span> <span data-ttu-id="a9577-144">クラス内の各プロパティには、データベースのテーブルの列を指定します。</span><span class="sxs-lookup"><span data-stu-id="a9577-144">Each property in the class specifies a column in the table of the database.</span></span> <span data-ttu-id="a9577-145">これらのクラスは、オブジェクト指向のコードとデータベースのリレーショナル テーブル構造の軽量のオブジェクト リレーショナル インターフェイスを提供します。</span><span class="sxs-lookup"><span data-stu-id="a9577-145">These classes provide a lightweight, object-relational interface between object-oriented code and the relational table structure of the database.</span></span>

<span data-ttu-id="a9577-146">このチュートリアルでは、製品およびカテゴリのスキーマを表す単純なエンティティ クラスを追加することで開始します。</span><span class="sxs-lookup"><span data-stu-id="a9577-146">In this tutorial, you'll start out by adding simple entity classes representing the schemas for products and categories.</span></span> <span data-ttu-id="a9577-147">製品クラスは、各製品の定義が格納されます。</span><span class="sxs-lookup"><span data-stu-id="a9577-147">The products class will contain definitions for each product.</span></span> <span data-ttu-id="a9577-148">各 product クラスのメンバーの名前になります`ProductID`、 `ProductName`、 `Description`、 `ImagePath`、 `UnitPrice`、 `CategoryID`、および`Category`します。</span><span class="sxs-lookup"><span data-stu-id="a9577-148">The name of each of the members of the product class will be `ProductID`, `ProductName`, `Description`, `ImagePath`, `UnitPrice`, `CategoryID`, and `Category`.</span></span> <span data-ttu-id="a9577-149">カテゴリのクラスは、自動車、ボート、平面など、製品が属することができる各カテゴリの定義が格納されます。</span><span class="sxs-lookup"><span data-stu-id="a9577-149">The category class will contain definitions for each category that a product can belong to, such as Car, Boat, or Plane.</span></span> <span data-ttu-id="a9577-150">各カテゴリのクラスのメンバーの名前になります`CategoryID`、 `CategoryName`、 `Description`、および`Products`します。</span><span class="sxs-lookup"><span data-stu-id="a9577-150">The name of each of the members of the category class will be `CategoryID`, `CategoryName`, `Description`, and `Products`.</span></span> <span data-ttu-id="a9577-151">各製品カテゴリのいずれかに属します。</span><span class="sxs-lookup"><span data-stu-id="a9577-151">Each product will belong to one of the categories.</span></span> <span data-ttu-id="a9577-152">これらのエンティティ クラスは、プロジェクトの既存に追加されます*モデル*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="a9577-152">These entity classes will be added to the project's existing *Models* folder.</span></span>

1. <span data-ttu-id="a9577-153">**ソリューション エクスプ ローラー**を右クリックし、*モデル*クリックしてフォルダー**追加** - &gt; **新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="a9577-153">In **Solution Explorer**, right-click the *Models* folder and then select **Add** -&gt; **New Item**.</span></span> 

    ![データ アクセス層 - 新しい項目 メニューの作成します。](create_the_data_access_layer/_static/image1.png)

   <span data-ttu-id="a9577-155">**[新しい項目の追加]** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a9577-155">The **Add New Item** dialog box is displayed.</span></span>
2. <span data-ttu-id="a9577-156">**Visual c#** から、**インストール済み**、左側のペイン**コード**します。</span><span class="sxs-lookup"><span data-stu-id="a9577-156">Under **Visual C#** from the **Installed** pane on the left, select **Code**.</span></span> 

    ![データ アクセス層 - 新しい項目 メニューの作成します。](create_the_data_access_layer/_static/image2.png)
3. <span data-ttu-id="a9577-158">選択**クラス**中央のペインからこの新しいクラスの名前と*Product.cs*します。</span><span class="sxs-lookup"><span data-stu-id="a9577-158">Select **Class** from the middle pane and name this new class *Product.cs*.</span></span>
4. <span data-ttu-id="a9577-159">**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a9577-159">Click **Add**.</span></span>  
   <span data-ttu-id="a9577-160">新しいクラス ファイルがエディターで表示されます。</span><span class="sxs-lookup"><span data-stu-id="a9577-160">The new class file is displayed in the editor.</span></span>
5. <span data-ttu-id="a9577-161">既定のコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a9577-161">Replace the default code with the following code:</span></span>   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample1.cs)]
6. <span data-ttu-id="a9577-162">ただし、名前、新しいクラスを手順 1. ~ 4. を繰り返して、別のクラスを作成する*Category.cs*既定のコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a9577-162">Create another class by repeating steps 1 through 4, however, name the new class *Category.cs* and replace the default code with the following code:</span></span>  

    [!code-csharp[Main](create_the_data_access_layer/samples/sample2.cs)]

<span data-ttu-id="a9577-163">既に触れましたが、`Category`クラスを表しますが、アプリケーションは、製品の種類は、販売するために設計されています (など<a id="a"> </a>&quot;自動車&quot;、&quot;ボート&quot;、 &quot;ロケット&quot;など)、および`Product`クラスは、データベース内の個々 の製品 (toys) を表します。</span><span class="sxs-lookup"><span data-stu-id="a9577-163">As previously mentioned, the `Category` class represents the type of product that the application is designed to sell (such as <a id="a"></a>&quot;Cars&quot;, &quot;Boats&quot;, &quot;Rockets&quot;, and so on), and the `Product` class represents the individual products (toys) in the database.</span></span> <span data-ttu-id="a9577-164">各インスタンスを`Product`オブジェクトは、リレーショナル データベース テーブル内の行に対応しているし、Product クラスの各プロパティは、リレーショナル データベース テーブル内の列にマップされます。</span><span class="sxs-lookup"><span data-stu-id="a9577-164">Each instance of a `Product` object will correspond to a row within a relational database table, and each property of the Product class will map to a column in the relational database table.</span></span> <span data-ttu-id="a9577-165">このチュートリアルの後半では、データベース内に含まれる製品データを確認します。</span><span class="sxs-lookup"><span data-stu-id="a9577-165">Later in this tutorial, you'll review the product data contained in the database.</span></span>

### <a name="data-annotations"></a><span data-ttu-id="a9577-166">データの注釈</span><span class="sxs-lookup"><span data-stu-id="a9577-166">Data Annotations</span></span>

<span data-ttu-id="a9577-167">お気付きかもしれませんクラスの特定のメンバーがあるなど、メンバーに関する詳細を指定する属性`[ScaffoldColumn(false)]`します。</span><span class="sxs-lookup"><span data-stu-id="a9577-167">You may have noticed that certain members of the classes have attributes specifying details about the member, such as `[ScaffoldColumn(false)]`.</span></span> <span data-ttu-id="a9577-168">これらは*データ注釈*します。</span><span class="sxs-lookup"><span data-stu-id="a9577-168">These are *data annotations*.</span></span> <span data-ttu-id="a9577-169">データ注釈属性は、書式を指定して、データベースが作成されるときのモデル方法を指定する、そのメンバーのユーザー入力を検証する方法を記述できます。</span><span class="sxs-lookup"><span data-stu-id="a9577-169">The data annotation attributes can describe how to validate user input for that member, to specify formatting for it, and to specify how it is modeled when the database is created.</span></span>

### <a name="context-class"></a><span data-ttu-id="a9577-170">Context クラス</span><span class="sxs-lookup"><span data-stu-id="a9577-170">Context Class</span></span>

<span data-ttu-id="a9577-171">データ アクセス クラスの使用を開始するには、コンテキスト クラスを定義する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a9577-171">To start using the classes for data access, you must define a context class.</span></span> <span data-ttu-id="a9577-172">コンテキスト クラスがエンティティ クラスを管理する前述のように、(など、`Product`クラスおよび`Category`クラス) し、データベースへのデータ アクセスを提供します。</span><span class="sxs-lookup"><span data-stu-id="a9577-172">As mentioned previously, the context class manages the entity classes (such as the `Product` class and the `Category` class) and provides data access to the database.</span></span>

<span data-ttu-id="a9577-173">この手順で、新しい c# コンテキスト クラスを追加、*モデル*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="a9577-173">This procedure adds a new C# context class to the *Models* folder.</span></span>

1. <span data-ttu-id="a9577-174">右クリックし、*モデル*クリックしてフォルダー**追加** - &gt; **新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="a9577-174">Right-click the *Models* folder and then select **Add** -&gt; **New Item**.</span></span>   
   <span data-ttu-id="a9577-175">**[新しい項目の追加]** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a9577-175">The **Add New Item** dialog box is displayed.</span></span>
2. <span data-ttu-id="a9577-176">選択**クラス**中央のペインから名前を付けます*ProductContext.cs*  をクリック**追加**します。</span><span class="sxs-lookup"><span data-stu-id="a9577-176">Select **Class** from the middle pane, name it *ProductContext.cs* and click **Add**.</span></span>
3. <span data-ttu-id="a9577-177">次のコードでクラスに含まれている既定のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a9577-177">Replace the default code contained in the class with the following code:</span></span>   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample3.cs)]

<span data-ttu-id="a9577-178">このコードを追加、`System.Data.Entity`名前空間をクエリする機能を含む、Entity Framework のすべてのコア機能にアクセスするように挿入、更新、および厳密に型指定されたオブジェクトを使用してデータを削除します。</span><span class="sxs-lookup"><span data-stu-id="a9577-178">This code adds the `System.Data.Entity` namespace so that you have access to all the core functionality of Entity Framework, which includes the capability to query, insert, update, and delete data by working with strongly typed objects.</span></span>

<span data-ttu-id="a9577-179">`ProductContext`クラスは、フェッチ、格納、および更新を処理する Entity Framework 製品データベース コンテキストを表します。`Product`クラス、データベース内のインスタンス。</span><span class="sxs-lookup"><span data-stu-id="a9577-179">The `ProductContext` class represents Entity Framework product database context, which handles fetching, storing, and updating `Product` class instances in the database.</span></span> <span data-ttu-id="a9577-180">`ProductContext`クラスから派生、`DbContext`基本クラスを Entity Framework によって提供されます。</span><span class="sxs-lookup"><span data-stu-id="a9577-180">The `ProductContext` class derives from the `DbContext` base class provided by Entity Framework.</span></span>

### <a name="initializer-class"></a><span data-ttu-id="a9577-181">初期化子クラス</span><span class="sxs-lookup"><span data-stu-id="a9577-181">Initializer Class</span></span>

<span data-ttu-id="a9577-182">コンテキストが使用されるデータベース最初の時間を初期化するためにいくつかのカスタム ロジックを実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a9577-182">You will need to run some custom logic to initialize the database the first time the context is used.</span></span> <span data-ttu-id="a9577-183">これで、製品と分類をすぐに表示できるように、データベースに追加するデータをシードします。</span><span class="sxs-lookup"><span data-stu-id="a9577-183">This will allow seed data to be added to the database so that you can immediately display products and categories.</span></span>

<span data-ttu-id="a9577-184">この手順で、新しい c# 初期化子クラスを追加、*モデル*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="a9577-184">This procedure adds a new C# initializer class to the *Models* folder.</span></span>

1. <span data-ttu-id="a9577-185">別の作成`Class`で、*モデル*フォルダーと名前を付けます*ProductDatabaseInitializer.cs*します。</span><span class="sxs-lookup"><span data-stu-id="a9577-185">Create another `Class` in the *Models* folder and name it *ProductDatabaseInitializer.cs*.</span></span>
2. <span data-ttu-id="a9577-186">次のコードでクラスに含まれている既定のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a9577-186">Replace the default code contained in the class with the following code:</span></span>   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample4.cs)]

<span data-ttu-id="a9577-187">データベースが作成され、初期化時に、上記のコードからご覧のとおり、`Seed`プロパティがオーバーライドされ、設定します。</span><span class="sxs-lookup"><span data-stu-id="a9577-187">As you can see from the above code, when the database is created and initialized, the `Seed` property is overridden and set.</span></span> <span data-ttu-id="a9577-188">ときに、`Seed`プロパティが設定されて、カテゴリおよび製品からの値は、データベースの作成に使用されます。</span><span class="sxs-lookup"><span data-stu-id="a9577-188">When the `Seed` property is set, the values from the categories and products are used to populate the database.</span></span> <span data-ttu-id="a9577-189">データベースが作成された後に、上記のコードを変更することで、登録されたデータを更新しようとした場合、Web アプリケーションを実行すると、更新プログラムは表示されません。</span><span class="sxs-lookup"><span data-stu-id="a9577-189">If you attempt to update the seed data by modifying the above code after the database has been created, you won't see any updates when you run the Web application.</span></span> <span data-ttu-id="a9577-190">理由は、上記のコードの実装を使用して、`DropCreateDatabaseIfModelChanges`シード データをリセットする前に、モデル (スキーマ) が変更されたかどうかを認識するクラス。</span><span class="sxs-lookup"><span data-stu-id="a9577-190">The reason is the above code uses an implementation of the `DropCreateDatabaseIfModelChanges` class to recognize if the model (schema) has changed before resetting the seed data.</span></span> <span data-ttu-id="a9577-191">変更されていない場合、`Category`と`Product`エンティティ クラス、データベースがシード データを再初期化されません。</span><span class="sxs-lookup"><span data-stu-id="a9577-191">If no changes are made to the `Category` and `Product` entity classes, the database will not be reinitialized with the seed data.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="a9577-192">アプリケーションを実行するたびに再作成するデータベースの場合は、使用する可能性があります、`DropCreateDatabaseAlways`クラスの代わりに、`DropCreateDatabaseIfModelChanges`クラス。</span><span class="sxs-lookup"><span data-stu-id="a9577-192">If you wanted the database to be recreated every time you ran the application, you could use the `DropCreateDatabaseAlways` class instead of the `DropCreateDatabaseIfModelChanges` class.</span></span> <span data-ttu-id="a9577-193">ただしこのチュートリアル シリーズで使用して、`DropCreateDatabaseIfModelChanges`クラス。</span><span class="sxs-lookup"><span data-stu-id="a9577-193">However for this tutorial series, use the `DropCreateDatabaseIfModelChanges` class.</span></span>


<span data-ttu-id="a9577-194">この時点で、このチュートリアルでは必要があります、*モデル*4 つの新しいクラスと 1 つの既定のクラスを使ってフォルダー。</span><span class="sxs-lookup"><span data-stu-id="a9577-194">At this point in this tutorial, you will have a *Models* folder with four new classes and one default class:</span></span>

![データ アクセス層の Models フォルダーを作成します。](create_the_data_access_layer/_static/image3.png)

### <a name="configuring-the-application-to-use-the-data-model"></a><span data-ttu-id="a9577-196">データ モデルを使用するアプリケーションを構成します。</span><span class="sxs-lookup"><span data-stu-id="a9577-196">Configuring the Application to Use the Data Model</span></span>

<span data-ttu-id="a9577-197">データを表すクラスを作成するので、クラスを使用してアプリケーションを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a9577-197">Now that you've created the classes that represent the data, you must configure the application to use the classes.</span></span> <span data-ttu-id="a9577-198">*Global.asax*ファイルはモデルを初期化するコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="a9577-198">In the *Global.asax* file, you add code that initializes the model.</span></span> <span data-ttu-id="a9577-199">*Web.config*ファイルが新しいデータ クラスによって表されるデータの格納を使用してどのようなデータベースをアプリケーションに通知する情報を追加します。</span><span class="sxs-lookup"><span data-stu-id="a9577-199">In the *Web.config* file you add information that tells the application what database you'll use to store the data that's represented by the new data classes.</span></span> <span data-ttu-id="a9577-200">*Global.asax*ファイルは、アプリケーションのイベントやメソッドを処理するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="a9577-200">The *Global.asax* file can be used to handle application events or methods.</span></span> <span data-ttu-id="a9577-201">*Web.config*ファイルでは、ASP.NET web アプリケーションの構成を制御できます。</span><span class="sxs-lookup"><span data-stu-id="a9577-201">The *Web.config* file allows you to control the configuration of your ASP.NET web application.</span></span>

#### <a name="updating-the-globalasax-file"></a><span data-ttu-id="a9577-202">Global.asax ファイルの更新</span><span class="sxs-lookup"><span data-stu-id="a9577-202">Updating the Global.asax file</span></span>

<span data-ttu-id="a9577-203">アプリケーションの起動時にデータ モデルを初期化するが更新されます、`Application_Start`ハンドラーで、 *Global.asax.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="a9577-203">To initialize the data models when the application starts, you will update the `Application_Start` handler in the *Global.asax.cs* file.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="a9577-204">ソリューション エクスプ ローラーで、いずれかを選択できます、 *Global.asax*ファイルまたは*Global.asax.cs*ファイルを編集する、 *Global.asax.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="a9577-204">In Solution Explorer, you can select either the *Global.asax* file or the *Global.asax.cs* file to edit the *Global.asax.cs* file.</span></span>


1. <span data-ttu-id="a9577-205">黄色で強調表示されている次のコードを追加、`Application_Start`メソッドで、 *Global.asax.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="a9577-205">Add the following code highlighted in yellow to the `Application_Start` method in the *Global.asax.cs* file.</span></span>   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample5.cs?highlight=9-10,22-23)]

> [!NOTE] 
> 
> <span data-ttu-id="a9577-206">ブラウザーでは、ブラウザーでこのチュートリアル シリーズを表示するときに、黄色で強調表示されているコードを表示する HTML5 をサポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="a9577-206">Your browser must support HTML5 to view the code highlighted in yellow when viewing this tutorial series in a browser.</span></span>


<span data-ttu-id="a9577-207">アプリケーションの起動時に、上記のコードに示すように、アプリケーションでは、データの最初の期間中に実行する初期化子のアクセスを指定します。</span><span class="sxs-lookup"><span data-stu-id="a9577-207">As shown in the above code, when the application starts, the application specifies the initializer that will run during the first time the data is accessed.</span></span> <span data-ttu-id="a9577-208">2 つの追加の名前空間のアクセスに必要な`Database`オブジェクトと`ProductDatabaseInitializer`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="a9577-208">The two additional namespaces are required to access the `Database` object and the `ProductDatabaseInitializer` object.</span></span>

 <span data-ttu-id="a9577-209">Web.Config ファイルを変更します。</span><span class="sxs-lookup"><span data-stu-id="a9577-209">Modifying the Web.Config File</span></span> 

<span data-ttu-id="a9577-210">Entity Framework Code First は生成、データベースの既定の場所にシード データをデータベースの内容が表示されたら、アプリケーションへの接続情報を追加できます、データベースの場所を制御します。</span><span class="sxs-lookup"><span data-stu-id="a9577-210">Although Entity Framework Code First will generate a database for you in a default location when the database is populated with seed data, adding your own connection information to your application gives you control of the database location.</span></span> <span data-ttu-id="a9577-211">アプリケーションの接続文字列を使用してこのデータベース接続を指定する*Web.config*プロジェクトのルートにあるファイル。</span><span class="sxs-lookup"><span data-stu-id="a9577-211">You specify this database connection using a connection string in the application's *Web.config* file at the root of the project.</span></span> <span data-ttu-id="a9577-212">新しい接続文字列を追加すると、データベースの場所を指示することができます (*wingtiptoys.mdf*) アプリケーションのデータ ディレクトリでビルドされる (*アプリ\_データ*)、既定ではなく場所。</span><span class="sxs-lookup"><span data-stu-id="a9577-212">By adding a new connection string, you can direct the location of the database (*wingtiptoys.mdf*) to be built in the application's data directory (*App\_Data*), rather than its default location.</span></span> <span data-ttu-id="a9577-213">この変更を行った使用すると、検索して、このチュートリアルの後半で、データベース ファイルを検査できます。</span><span class="sxs-lookup"><span data-stu-id="a9577-213">Making this change will allow you to find and inspect the database file later in this tutorial.</span></span>

1. <span data-ttu-id="a9577-214">**ソリューション エクスプ ローラー**を検索して開く、 *Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="a9577-214">In **Solution Explorer**, find and open the *Web.config* file.</span></span>
2. <span data-ttu-id="a9577-215">黄色で強調表示されている接続文字列を追加、`<connectionStrings>`のセクション、 *Web.config*ファイルの次のようにします。</span><span class="sxs-lookup"><span data-stu-id="a9577-215">Add the connection string highlighted in yellow to the `<connectionStrings>` section of the *Web.config* file as follows:</span></span>  

    [!code-xml[Main](create_the_data_access_layer/samples/sample6.xml?highlight=4-7)]

<span data-ttu-id="a9577-216">アプリケーションが実行される、最初には、接続文字列によって指定された場所にデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="a9577-216">When the application is run for the first time, it will build the database at the location specified by the connection string.</span></span> <span data-ttu-id="a9577-217">アプリケーションを実行する前にみましょう最初にビルド。</span><span class="sxs-lookup"><span data-stu-id="a9577-217">But before running the application, let's build it first.</span></span>

## <a name="building-the-application"></a><span data-ttu-id="a9577-218">アプリケーションのビルド</span><span class="sxs-lookup"><span data-stu-id="a9577-218">Building the Application</span></span>

<span data-ttu-id="a9577-219">クラスと、Web アプリケーションへの変更がすべて動作することを確認するには、アプリケーションを構築する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a9577-219">To make sure that all the classes and changes to your Web application work, you should build the application.</span></span>

1. <span data-ttu-id="a9577-220">**デバッグ**メニューの **ビルド WingtipToys**します。</span><span class="sxs-lookup"><span data-stu-id="a9577-220">From the **Debug** menu, select **Build WingtipToys**.</span></span>  
 <span data-ttu-id="a9577-221">**出力**ウィンドウが表示され、すべてうまくいけば、表示、*が成功した*メッセージ。</span><span class="sxs-lookup"><span data-stu-id="a9577-221">The **Output** window is displayed, and if all went well, you see a *succeeded* message.</span></span>  

    ![データ アクセス層 - Windows の出力を作成します。](create_the_data_access_layer/_static/image4.png)

<span data-ttu-id="a9577-223">エラーが発生した場合は、上記の手順を再度確認します。</span><span class="sxs-lookup"><span data-stu-id="a9577-223">If you run into an error, re-check the above steps.</span></span> <span data-ttu-id="a9577-224">内の情報、**出力**ウィンドウは、どのファイルに問題があるし、ファイルの変更が必要な場合。</span><span class="sxs-lookup"><span data-stu-id="a9577-224">The information in the **Output** window will indicate which file has a problem and where in the file a change is required.</span></span> <span data-ttu-id="a9577-225">この情報は、上記の手順のどの部分を確認して、プロジェクトで修正する必要がありますを決定するを有効になります。</span><span class="sxs-lookup"><span data-stu-id="a9577-225">This information will enable you to determine what part of the above steps need to be reviewed and fixed in your project.</span></span>

## <a name="summary"></a><span data-ttu-id="a9577-226">まとめ</span><span class="sxs-lookup"><span data-stu-id="a9577-226">Summary</span></span>

<span data-ttu-id="a9577-227">シリーズのこのチュートリアルではするが、データ モデルの作成だけでなく、初期化し、データベースのシードに使用されるコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="a9577-227">In this tutorial of the series you have created the data model, as well as, added the code that will be used to initialize and seed the database.</span></span> <span data-ttu-id="a9577-228">アプリケーションの実行時に、データ モデルを使用するアプリケーションを構成することもします。</span><span class="sxs-lookup"><span data-stu-id="a9577-228">You have also configured the application to use the data models when the application is run.</span></span>

<span data-ttu-id="a9577-229">次のチュートリアルで UI を更新、ナビゲーションを追加し、データベースからデータを取得します。</span><span class="sxs-lookup"><span data-stu-id="a9577-229">In the next tutorial, you'll update the UI, add navigation, and retrieve data from the database.</span></span> <span data-ttu-id="a9577-230">これにより、このチュートリアルで作成したエンティティ クラスに基づいて自動的に作成されているデータベースが発生します。</span><span class="sxs-lookup"><span data-stu-id="a9577-230">This will result in the database being automatically created based on the entity classes that you created in this tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a9577-231">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="a9577-231">Additional Resources</span></span>

<span data-ttu-id="a9577-232">[Entity Framework の概要](https://msdn.microsoft.com/library/bb399567.aspx) </span><span class="sxs-lookup"><span data-stu-id="a9577-232">[Entity Framework Overview](https://msdn.microsoft.com/library/bb399567.aspx) </span></span>  
<span data-ttu-id="a9577-233">[ADO.NET Entity Framework の初心者向けガイド](https://msdn.microsoft.com/data/ee712907) </span><span class="sxs-lookup"><span data-stu-id="a9577-233">[Beginner's Guide to the ADO.NET Entity Framework](https://msdn.microsoft.com/data/ee712907) </span></span>  
<span data-ttu-id="a9577-234">[Entity Framework を使用した最初の開発をコード](http://www.msteched.com/2010/Europe/DEV212)(ビデオ)</span><span class="sxs-lookup"><span data-stu-id="a9577-234">[Code First Development with Entity Framework](http://www.msteched.com/2010/Europe/DEV212) (video)</span></span>   
<span data-ttu-id="a9577-235">[リレーションシップ code First Fluent API](https://msdn.microsoft.com/data/hh134698) </span><span class="sxs-lookup"><span data-stu-id="a9577-235">[Code First Relationships Fluent API](https://msdn.microsoft.com/data/hh134698) </span></span>  
[<span data-ttu-id="a9577-236">Code First のデータ注釈</span><span class="sxs-lookup"><span data-stu-id="a9577-236">Code First Data Annotations</span></span>](https://msdn.microsoft.com/data/gg193958)  
[<span data-ttu-id="a9577-237">Entity Framework の生産性の向上</span><span class="sxs-lookup"><span data-stu-id="a9577-237">Productivity Improvements for the Entity Framework</span></span>](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0)

> [!div class="step-by-step"]
> <span data-ttu-id="a9577-238">[前へ](create-the-project.md)
> [次へ](ui_and_navigation.md)</span><span class="sxs-lookup"><span data-stu-id="a9577-238">[Previous](create-the-project.md)
[Next](ui_and_navigation.md)</span></span>
