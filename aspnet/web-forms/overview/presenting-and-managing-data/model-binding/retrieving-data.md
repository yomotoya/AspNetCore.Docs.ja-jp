---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: 取得してを使用してデータを表示するモデル バインドと web フォーム |Microsoft ドキュメント
author: tfitzmac
description: このチュートリアルの系列では、モデル バインディングを使用して ASP.NET Web フォーム プロジェクトとの基本的な側面について説明します。 モデル バインドは、データの操作詳細直線-しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: 26873efa5dbfdbdab39a52cfb9cfd5a65c8231a3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a><span data-ttu-id="f9037-104">取得して、モデル バインディング機能と web フォームでデータの表示</span><span class="sxs-lookup"><span data-stu-id="f9037-104">Retrieving and displaying data with model binding and web forms</span></span>
====================
<span data-ttu-id="f9037-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f9037-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="f9037-106">このチュートリアルの系列では、モデル バインディングを使用して ASP.NET Web フォーム プロジェクトとの基本的な側面について説明します。</span><span class="sxs-lookup"><span data-stu-id="f9037-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="f9037-107">モデル バインディングは、ObjectDataSource または SqlDataSource) などのソース オブジェクトにデータを処理するよりもより簡単にデータの操作にします。</span><span class="sxs-lookup"><span data-stu-id="f9037-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="f9037-108">この系列は入門資料で始まるし、後のチュートリアルより高度な概念に移動されます。</span><span class="sxs-lookup"><span data-stu-id="f9037-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="f9037-109">モデル バインド パターンは、任意のデータ アクセス テクノロジと連携します。</span><span class="sxs-lookup"><span data-stu-id="f9037-109">The model binding pattern works with any data access technology.</span></span> <span data-ttu-id="f9037-110">このチュートリアルでは、Entity Framework に使用するが、最もなじみのあるデータ アクセス テクノロジを使用する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="f9037-110">In this tutorial, you will use Entity Framework, but you could use the data access technology that is most familiar to you.</span></span> <span data-ttu-id="f9037-111">GridView、ListView、DetailsView、またはフォーム ビューのコントロールなどのデータ バインドされたサーバー コントロールからを選択すると、更新、削除、およびデータの作成に使用するメソッドの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="f9037-111">From a data-bound server control, such as a GridView, ListView, DetailsView, or FormView control, you specify the names of the methods to use for selecting, updating, deleting, and creating data.</span></span> <span data-ttu-id="f9037-112">このチュートリアルでは、SelectMethod の値を指定します。</span><span class="sxs-lookup"><span data-stu-id="f9037-112">In this tutorial, you will specify a value for the SelectMethod.</span></span>
> 
> <span data-ttu-id="f9037-113">このメソッドは、内でデータを取得するロジックを提供します。</span><span class="sxs-lookup"><span data-stu-id="f9037-113">Within that method, you provide the logic for retrieving the data.</span></span> <span data-ttu-id="f9037-114">チュートリアルでは、[次へ]、UpdateMethod、DeleteMethod および InsertMethod の値を設定します。</span><span class="sxs-lookup"><span data-stu-id="f9037-114">In the next tutorial, you will set values for UpdateMethod, DeleteMethod and InsertMethod.</span></span>
> 
> <span data-ttu-id="f9037-115">実行できます[ダウンロード](https://go.microsoft.com/fwlink/?LinkId=286116)プロジェクト、完全な c# または vb です。</span><span class="sxs-lookup"><span data-stu-id="f9037-115">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="f9037-116">ダウンロード可能なコードは、Visual Studio 2012 または Visual Studio 2013 のいずれかで動作します。</span><span class="sxs-lookup"><span data-stu-id="f9037-116">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="f9037-117">これは、このチュートリアルで示すように Visual Studio 2013 のテンプレートよりも若干異なる Visual Studio 2012 テンプレートを使用します。</span><span class="sxs-lookup"><span data-stu-id="f9037-117">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>
> 
> <span data-ttu-id="f9037-118">このチュートリアルでは、Visual Studio でアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="f9037-118">In the tutorial you run the application in Visual Studio.</span></span> <span data-ttu-id="f9037-119">利用できるアプリケーション、インターネット経由でホスティング プロバイダーに展開することで。</span><span class="sxs-lookup"><span data-stu-id="f9037-119">You can also make the application available over the Internet by deploying it to a hosting provider.</span></span> <span data-ttu-id="f9037-120">マイクロソフトでは、無料の web ホスティングで最大 10 個の web サイトを提供しています、</span><span class="sxs-lookup"><span data-stu-id="f9037-120">Microsoft offers free web hosting for up to 10 web sites in a</span></span>  
>  <span data-ttu-id="f9037-121">[無料試用版の Azure アカウント](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)です。</span><span class="sxs-lookup"><span data-stu-id="f9037-121">[free Azure trial account](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="f9037-122">Azure App Service Web アプリを Visual Studio web プロジェクトを展開する方法については、次を参照してください。、 [Visual Studio を使用して ASP.NET Web 配置](../../deployment/visual-studio-web-deployment/introduction.md)系列です。</span><span class="sxs-lookup"><span data-stu-id="f9037-122">For information about how to deploy a Visual Studio web project to Azure App Service Web Apps, see the [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) series.</span></span> <span data-ttu-id="f9037-123">そのチュートリアルでは、Entity Framework Code First Migrations を使用して Azure SQL Database に、SQL Server データベースを展開する方法も示します。</span><span class="sxs-lookup"><span data-stu-id="f9037-123">That tutorial also shows how to use Entity Framework Code First Migrations to deploy your SQL Server database to Azure SQL Database.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f9037-124">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="f9037-124">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="f9037-125">Microsoft Visual Studio 2013 または Microsoft Visual Studio Express 2013 for Web</span><span class="sxs-lookup"><span data-stu-id="f9037-125">Microsoft Visual Studio 2013 or Microsoft Visual Studio Express 2013 for Web</span></span>
>   
> 
> <span data-ttu-id="f9037-126">このチュートリアルでは Visual Studio 2012 もが、ユーザー インターフェイスとプロジェクト テンプレートでは、いくつか違いがあります。</span><span class="sxs-lookup"><span data-stu-id="f9037-126">This tutorial also works with Visual Studio 2012 but there will be some differences in the user interface and project template.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="f9037-127">新機能のビルド</span><span class="sxs-lookup"><span data-stu-id="f9037-127">What you'll build</span></span>

<span data-ttu-id="f9037-128">このチュートリアルでは、次の手順を。</span><span class="sxs-lookup"><span data-stu-id="f9037-128">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="f9037-129">受講者コースに登録されていると、大学を反映するデータ オブジェクトを構築します。</span><span class="sxs-lookup"><span data-stu-id="f9037-129">Build data objects that reflect a university with students enrolled in courses</span></span>
2. <span data-ttu-id="f9037-130">データベース テーブル オブジェクトを構築します。</span><span class="sxs-lookup"><span data-stu-id="f9037-130">Build database tables from the objects</span></span>
3. <span data-ttu-id="f9037-131">データベースにテスト データへの追加します。</span><span class="sxs-lookup"><span data-stu-id="f9037-131">Populate the database with test data</span></span>
4. <span data-ttu-id="f9037-132">Web フォームにデータを表示します。</span><span class="sxs-lookup"><span data-stu-id="f9037-132">Display data in a web form</span></span>

## <a name="set-up-project"></a><span data-ttu-id="f9037-133">プロジェクトを設定します。</span><span class="sxs-lookup"><span data-stu-id="f9037-133">Set up project</span></span>

<span data-ttu-id="f9037-134">Visual Studio 2013 で作成、新しい**ASP.NET Web アプリケーション**と呼ばれる**ContosoUniversityModelBinding**です。</span><span class="sxs-lookup"><span data-stu-id="f9037-134">In Visual Studio 2013, create a new **ASP.NET Web Application** called **ContosoUniversityModelBinding**.</span></span>

![プロジェクトを作成します。](retrieving-data/_static/image2.png)

<span data-ttu-id="f9037-136">Web フォーム テンプレートを選択し、その他の既定のオプションのままにします。</span><span class="sxs-lookup"><span data-stu-id="f9037-136">Select the Web Forms template, and leave the other default options.</span></span> <span data-ttu-id="f9037-137">プロジェクトをセットアップするには、[ok] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f9037-137">Click OK to setup the project.</span></span>

![web フォームを選択します。](retrieving-data/_static/image3.png)

<span data-ttu-id="f9037-139">最初に、いくつかのサイトの外観のカスタマイズを少し変更してください。</span><span class="sxs-lookup"><span data-stu-id="f9037-139">First, you will make a couple of small changes to customize the appearance of the site.</span></span> <span data-ttu-id="f9037-140">開く、 **Site.Master**ファイル、および含めるマイ ASP.NET アプリケーションではなく Contoso 大学のタイトルを変更します。</span><span class="sxs-lookup"><span data-stu-id="f9037-140">Open the **Site.Master** file, and change the title to include Contoso University instead of My ASP.NET Application.</span></span>

[!code-aspx[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

<span data-ttu-id="f9037-141">次に、ヘッダー テキストを変更します**アプリケーション名**に**Contoso 大学**です。</span><span class="sxs-lookup"><span data-stu-id="f9037-141">Then, change the header text from **Application name** to **Contoso University**.</span></span>

[!code-aspx[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

<span data-ttu-id="f9037-142">また、Site.Master では、このサイトに関連するページを反映するように、ヘッダーに表示されるナビゲーション リンクを変更します。</span><span class="sxs-lookup"><span data-stu-id="f9037-142">Also in Site.Master, change the navigation links that appear in the header to reflect the pages that are relevant to this site.</span></span> <span data-ttu-id="f9037-143">いずれかの必要はありません、**に関する**ページまたは**連絡先** ページが表示これらのリンクを削除することができます。</span><span class="sxs-lookup"><span data-stu-id="f9037-143">You will not need either the **About** page or the **Contact** page so those links can be removed.</span></span> <span data-ttu-id="f9037-144">代わりに、必要がありますと呼ばれるページへのリンク**受講者**です。</span><span class="sxs-lookup"><span data-stu-id="f9037-144">Instead, you will need a link to a page called **Students**.</span></span> <span data-ttu-id="f9037-145">このページはまだ作成されていません。</span><span class="sxs-lookup"><span data-stu-id="f9037-145">This page has not been created yet.</span></span>

[!code-aspx[Main](retrieving-data/samples/sample3.aspx)]

<span data-ttu-id="f9037-146">保存して Site.Master を閉じます。</span><span class="sxs-lookup"><span data-stu-id="f9037-146">Save and close Site.Master.</span></span>

<span data-ttu-id="f9037-147">ここで、学生のデータを表示するための web フォームを作成します。</span><span class="sxs-lookup"><span data-stu-id="f9037-147">Now, you'll create the web form for displaying student data.</span></span> <span data-ttu-id="f9037-148">プロジェクトを右クリックし、**追加**、**新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="f9037-148">Right-click your project, and **Add** a **New Item**.</span></span> <span data-ttu-id="f9037-149">選択、**マスター ページを含む Web フォーム**テンプレート、名前を付けます**Students.aspx**です。</span><span class="sxs-lookup"><span data-stu-id="f9037-149">Select the **Web Form with Master Page** template, and name it **Students.aspx**.</span></span>

![作成 ページ](retrieving-data/_static/image5.png)

<span data-ttu-id="f9037-151">選択**Site.Master**新しい web フォームのマスター ページとして。</span><span class="sxs-lookup"><span data-stu-id="f9037-151">Select **Site.Master** as the master page for the new web form.</span></span>

## <a name="create-the-data-models-and-database"></a><span data-ttu-id="f9037-152">データ モデルとデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="f9037-152">Create the data models and database</span></span>

<span data-ttu-id="f9037-153">オブジェクトと、対応するデータベース テーブルを作成するのには、Code First Migrations を使用します。</span><span class="sxs-lookup"><span data-stu-id="f9037-153">You will use Code First Migrations to create objects and the corresponding database tables.</span></span> <span data-ttu-id="f9037-154">これらのテーブルでは、受講者と、コースに関する情報を格納します。</span><span class="sxs-lookup"><span data-stu-id="f9037-154">These tables will store information about the students and their courses.</span></span>

<span data-ttu-id="f9037-155">という名前の新しいクラスを追加、Models フォルダーに**UniversityModels.cs**です。</span><span class="sxs-lookup"><span data-stu-id="f9037-155">In the Models folder, add a new class named **UniversityModels.cs**.</span></span>

![モデル クラスを作成します。](retrieving-data/_static/image7.png)

<span data-ttu-id="f9037-157">このファイルには、次のよう SchoolContext、学生、登録、および Course クラスを定義します。</span><span class="sxs-lookup"><span data-stu-id="f9037-157">In this file, define the SchoolContext, Student, Enrollment, and Course classes as follows:</span></span>

[!code-csharp[Main](retrieving-data/samples/sample4.cs)]

<span data-ttu-id="f9037-158">SchoolContext クラスは、データベースの接続とデータの変更を管理する DbContext から派生します。</span><span class="sxs-lookup"><span data-stu-id="f9037-158">The SchoolContext class derives from DbContext, which manages the database connection and changes in the data.</span></span>

<span data-ttu-id="f9037-159">Student クラスで属性に適用されていることに注意してください。、 **FirstName**、 **LastName**、および**年**プロパティです。</span><span class="sxs-lookup"><span data-stu-id="f9037-159">In the Student class, notice the attributes that have been applied to the **FirstName**, **LastName**, and **Year** properties.</span></span> <span data-ttu-id="f9037-160">これらの属性は、このチュートリアルでのデータ検証に使用されます。</span><span class="sxs-lookup"><span data-stu-id="f9037-160">These attributes will be used for data validation in this tutorial.</span></span> <span data-ttu-id="f9037-161">このデモ プロジェクトのコードを簡略化これらのプロパティだけがデータ検証属性でマークされています。</span><span class="sxs-lookup"><span data-stu-id="f9037-161">To simplify the code for this demonstration project, only these properties were marked with data-validation attributes.</span></span> <span data-ttu-id="f9037-162">実際のプロジェクトなど、登録およびコース クラスのプロパティの検証済みのデータが必要なすべてのプロパティに検証属性を適用します。</span><span class="sxs-lookup"><span data-stu-id="f9037-162">In a real project, you would apply validation attributes to all properties that need validated data, such as properties in the Enrollment and Course classes.</span></span>

<span data-ttu-id="f9037-163">UniversityModels.cs を保存します。</span><span class="sxs-lookup"><span data-stu-id="f9037-163">Save UniversityModels.cs.</span></span>

<span data-ttu-id="f9037-164">Code First Migrations のツールを使用するこれらのクラスに基づくデータベースを設定します。</span><span class="sxs-lookup"><span data-stu-id="f9037-164">You will use the tools for Code First Migrations to set up a database based on these classes.</span></span>

<span data-ttu-id="f9037-165">**Package Manager Console**コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="f9037-165">In **Package Manager Console**, run the command:</span></span>  
`enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

<span data-ttu-id="f9037-166">コマンドが正常に完了した場合は、移行を有効になっていることを示すメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f9037-166">If the command completes successfully you will receive a message stating migrations have been enabled,</span></span>

![移行を有効にします。](retrieving-data/_static/image8.png)

<span data-ttu-id="f9037-168">新しいファイルの名前に注意してください**される Configuration.cs**が作成されました。</span><span class="sxs-lookup"><span data-stu-id="f9037-168">Notice that a new file named **Configuration.cs** has been created.</span></span> <span data-ttu-id="f9037-169">Visual Studio で、作成した後、このファイルが自動的に開きます。</span><span class="sxs-lookup"><span data-stu-id="f9037-169">In Visual Studio, this file is automatically opened after it is created.</span></span> <span data-ttu-id="f9037-170">構成クラスが含まれています、**シード**メソッドを事前にテスト データ、データベース テーブルに設定することができます。</span><span class="sxs-lookup"><span data-stu-id="f9037-170">The Configuration class contains a **Seed** method which enables you to pre-populate the database tables with test data.</span></span>

<span data-ttu-id="f9037-171">Seed メソッドに次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="f9037-171">Add the following code to the Seed method.</span></span> <span data-ttu-id="f9037-172">追加する必要があります、**を使用して**のステートメント、 **ContosoUniversityModelBinding.Models**名前空間。</span><span class="sxs-lookup"><span data-stu-id="f9037-172">You'll need to add a **using** statement for the **ContosoUniversityModelBinding.Models** namespace.</span></span>

[!code-csharp[Main](retrieving-data/samples/sample5.cs)]

<span data-ttu-id="f9037-173">される Configuration.cs を保存します。</span><span class="sxs-lookup"><span data-stu-id="f9037-173">Save Configuration.cs.</span></span>

<span data-ttu-id="f9037-174">パッケージ マネージャー コンソールで、コマンドを実行`add-migration initial`です。</span><span class="sxs-lookup"><span data-stu-id="f9037-174">In the Package Manager Console, run the command `add-migration initial`.</span></span>

<span data-ttu-id="f9037-175">コマンドを実行して、`update-database`です。</span><span class="sxs-lookup"><span data-stu-id="f9037-175">Then, run the command `update-database`.</span></span>

<span data-ttu-id="f9037-176">このコマンドを実行している場合に、例外が発生する場合は、可能であればシード メソッド内の値から StudentID、CourseID の値が異なりますをします。</span><span class="sxs-lookup"><span data-stu-id="f9037-176">If you receive an exception when running this command, it is possible that the values for StudentID and CourseID have varied from the values in the Seed method.</span></span> <span data-ttu-id="f9037-177">データベースでそれらのテーブルを開き、StudentID、CourseID の既存の値を検索します。</span><span class="sxs-lookup"><span data-stu-id="f9037-177">Open those tables in the database and find existing values for StudentID and CourseID.</span></span> <span data-ttu-id="f9037-178">「登録」表は、シード処理のコードでそれら値を追加します。</span><span class="sxs-lookup"><span data-stu-id="f9037-178">Add those values in the code for seeding the Enrollments table.</span></span>

<span data-ttu-id="f9037-179">データベース ファイルが追加されましたが現在のプロジェクトで非表示します。</span><span class="sxs-lookup"><span data-stu-id="f9037-179">The database file has been added, but it is currently hidden in the project.</span></span> <span data-ttu-id="f9037-180">をクリックして**すべてのファイル**ファイルを表示します。</span><span class="sxs-lookup"><span data-stu-id="f9037-180">Click **Show All Files** to display the file.</span></span>

![すべてのファイルを表示します。](retrieving-data/_static/image10.png)

<span data-ttu-id="f9037-182">.Mdf ファイルをアプリに表示されるように注意してください。\_データ フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="f9037-182">Notice the .mdf file now appears in the App\_Data folder.</span></span>

![データベース ファイル](retrieving-data/_static/image12.png)

<span data-ttu-id="f9037-184">.Mdf ファイルをダブルクリックし、サーバー エクスプ ローラーを開きます。</span><span class="sxs-lookup"><span data-stu-id="f9037-184">Double-click the .mdf file and open the Server Explorer.</span></span> <span data-ttu-id="f9037-185">テーブルは今すぐに存在し、データが挿入されます。</span><span class="sxs-lookup"><span data-stu-id="f9037-185">The tables now exist and are populated with data.</span></span>

![データベース テーブル](retrieving-data/_static/image14.png)

## <a name="display-data-from-students-and-related-tables"></a><span data-ttu-id="f9037-187">受講者との関連テーブルからデータを表示します。</span><span class="sxs-lookup"><span data-stu-id="f9037-187">Display data from Students and related tables</span></span>

<span data-ttu-id="f9037-188">データベース内のデータでデータを取得して、web ページに表示する準備ができました。</span><span class="sxs-lookup"><span data-stu-id="f9037-188">With data in the database, you are now ready to retrieve that data and display it in a web page.</span></span> <span data-ttu-id="f9037-189">使用して、 **GridView**列および行のデータを表示するコントロール。</span><span class="sxs-lookup"><span data-stu-id="f9037-189">You will use a **GridView** control to display the data in columns and rows.</span></span>

<span data-ttu-id="f9037-190">Students.aspx を開き、検索、**メイン**プレース ホルダーです。</span><span class="sxs-lookup"><span data-stu-id="f9037-190">Open Students.aspx, and locate the **MainContent** placeholder.</span></span> <span data-ttu-id="f9037-191">そのプレース ホルダー内に、追加、 **GridView**コントロールを次のコードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="f9037-191">Within that placeholder, add a **GridView** control that includes the following code.</span></span>

[!code-aspx[Main](retrieving-data/samples/sample6.aspx)]

<span data-ttu-id="f9037-192">このマークアップ コードを確認するための重要な概念についていくつかあります。</span><span class="sxs-lookup"><span data-stu-id="f9037-192">There are a couple of important concepts in this markup code for you to notice.</span></span> <span data-ttu-id="f9037-193">最初に、値が に設定されていることに注意してください、 **SelectMethod** GridView 要素内のプロパティです。</span><span class="sxs-lookup"><span data-stu-id="f9037-193">First, notice that a value is set for the **SelectMethod** property in the GridView element.</span></span> <span data-ttu-id="f9037-194">この値は、GridView のデータの取得に使用されるメソッドの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="f9037-194">This value specifies the name of the method that is used for retrieving data for the GridView.</span></span> <span data-ttu-id="f9037-195">このメソッドは、次の手順で作成します。</span><span class="sxs-lookup"><span data-stu-id="f9037-195">You will create this method in the next step.</span></span> <span data-ttu-id="f9037-196">次に、ことに注意して、 **ItemType**プロパティが以前に作成した Student クラスに設定します。</span><span class="sxs-lookup"><span data-stu-id="f9037-196">Second, notice that the **ItemType** property is set to the Student class that you created earlier.</span></span> <span data-ttu-id="f9037-197">この値を設定するでは、マークアップのコードでは、そのクラスのプロパティを参照できます。</span><span class="sxs-lookup"><span data-stu-id="f9037-197">By setting this value, you can refer to properties of that class in the markup code.</span></span> <span data-ttu-id="f9037-198">たとえば、Student クラスには、「登録」という名前コレクションが含まれていますいます。</span><span class="sxs-lookup"><span data-stu-id="f9037-198">For example, the Student class contains a collection named Enrollments.</span></span> <span data-ttu-id="f9037-199">使用することができます**Item.Enrollments**をそのコレクションを取得し、LINQ 構文を使用して、各学生の登録済みのクレジットの合計を取得します。</span><span class="sxs-lookup"><span data-stu-id="f9037-199">You can use **Item.Enrollments** to retrieve that collection and then use LINQ syntax to retrieve the sum of enrolled credits for each student.</span></span>

<span data-ttu-id="f9037-200">分離コード ファイルで指定されているメソッドを追加する必要があります、 **SelectMethod**値。</span><span class="sxs-lookup"><span data-stu-id="f9037-200">In the code-behind file, you need to add the method that is specified for the **SelectMethod** value.</span></span> <span data-ttu-id="f9037-201">開いている**Students.aspx.cs**、し、追加**を使用して**のステートメント、 **ContosoUniversityModelBinding.Models**と**System.Data.Entity**名前空間。</span><span class="sxs-lookup"><span data-stu-id="f9037-201">Open **Students.aspx.cs**, and add **using** statements for the **ContosoUniversityModelBinding.Models** and **System.Data.Entity** namespaces.</span></span>

[!code-csharp[Main](retrieving-data/samples/sample7.cs)]

<span data-ttu-id="f9037-202">次に、次のメソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="f9037-202">Then, add the following method.</span></span> <span data-ttu-id="f9037-203">このメソッドの名前が、SelectMethod の指定した名前と一致することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="f9037-203">Notice that the name of this method matches the name you provided for SelectMethod.</span></span>

[!code-csharp[Main](retrieving-data/samples/sample8.cs)]

<span data-ttu-id="f9037-204">**Include**句はこのクエリのパフォーマンスが向上しますが、作業するクエリは必須ではありません。</span><span class="sxs-lookup"><span data-stu-id="f9037-204">The **Include** clause improves the performance of this query but is not essential for the query to work.</span></span> <span data-ttu-id="f9037-205">Include 句を使用せず、データをデータベースを別のクエリごとに関連するデータの取得の送信が行われる、遅延読み込みを使用して取得はします。</span><span class="sxs-lookup"><span data-stu-id="f9037-205">Without the Include clause, the data would be retrieved using lazy loading, which involves sending a separate query to the database each time related data is retrieved.</span></span> <span data-ttu-id="f9037-206">Include 句を指定して、データベースの 1 つのクエリからのすべての関連するデータを取得することを意味する一括読み込みを使用してデータを取得します。</span><span class="sxs-lookup"><span data-stu-id="f9037-206">By providing the Include clause, the data is retrieved using eager loading, which means all of the related data is retrieved through a single query of the database.</span></span> <span data-ttu-id="f9037-207">関連するデータの大部分ができない場合に多くのデータが取得されるので、使用される、一括読み込みは効率が低下することができます。</span><span class="sxs-lookup"><span data-stu-id="f9037-207">In cases where most of the related data is not be used, eager loading can be less efficient because more data is retrieved.</span></span> <span data-ttu-id="f9037-208">ただし、この場合、一括読み込みは、最適なパフォーマンス レコードごとに、関連するデータが表示されるためです。</span><span class="sxs-lookup"><span data-stu-id="f9037-208">However, in this case, eager loading provides the best performance because the related data is displayed for each record.</span></span>

<span data-ttu-id="f9037-209">読み込むときのパフォーマンスに関する考慮事項の詳細については、データを関連、というタイトルのセクションを参照してください**Lazy、Eager、および明示的な読み込みの関連するデータ**で、 [、ASP では、Entity Framework と関連するデータの読み取り.NET MVC アプリケーション](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)トピックです。</span><span class="sxs-lookup"><span data-stu-id="f9037-209">For more information about performance consideration when loading related data, see the section titled **Lazy, Eager, and Explicit Loading of Related Data** in the [Reading Related Data with the Entity Framework in an ASP.NET MVC Application](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) topic.</span></span>

<span data-ttu-id="f9037-210">既定では、データは、キーとしてマークされているプロパティの値によって並べ替えられます。</span><span class="sxs-lookup"><span data-stu-id="f9037-210">By default, the data is sorted by the values of the property marked as the key.</span></span> <span data-ttu-id="f9037-211">並べ替え用の別の値を指定する、OrderBy 句を追加することができます。</span><span class="sxs-lookup"><span data-stu-id="f9037-211">You can add an OrderBy clause to specify a different value for sorting.</span></span> <span data-ttu-id="f9037-212">この例では既定の学生 Id プロパティは並べ替えに使用されます。</span><span class="sxs-lookup"><span data-stu-id="f9037-212">In this example, the default StudentID property is used for sorting.</span></span> <span data-ttu-id="f9037-213">[並べ替え、ページング、およびデータのフィルター処理](sorting-paging-and-filtering-data.md)トピックを有効にしたユーザーを並べ替え列を選択します。</span><span class="sxs-lookup"><span data-stu-id="f9037-213">In the [Sorting, Paging, and Filtering Data](sorting-paging-and-filtering-data.md) topic, you will enable the user to select a column for sorting.</span></span>

<span data-ttu-id="f9037-214">Web アプリを実行し、受講者のページに移動します。</span><span class="sxs-lookup"><span data-stu-id="f9037-214">Run your web application, and navigate to the Students page.</span></span> <span data-ttu-id="f9037-215">受講者ページには、次の受講者情報が表示されます。</span><span class="sxs-lookup"><span data-stu-id="f9037-215">The Students page displays the following student information.</span></span>

![データを表示します。](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a><span data-ttu-id="f9037-217">モデル バインド メソッドの自動生成</span><span class="sxs-lookup"><span data-stu-id="f9037-217">Automatic generation of model binding methods</span></span>

<span data-ttu-id="f9037-218">このチュートリアルの一連の操作、チュートリアルからプロジェクトに、コードを単にコピーできます。</span><span class="sxs-lookup"><span data-stu-id="f9037-218">When working through this tutorial series, you can simply copy the code from the tutorial to your project.</span></span> <span data-ttu-id="f9037-219">ただし、このアプローチの欠点の 1 つではモデル バインド メソッドのコードを自動的に生成する Visual Studio によって提供される機能の対応はならないことです。</span><span class="sxs-lookup"><span data-stu-id="f9037-219">However, one disadvantage of this approach is that you may not become aware of the feature provided by Visual Studio to automatically generate code for model binding methods.</span></span> <span data-ttu-id="f9037-220">使用する場合、独自のプロジェクトで、自動コード生成は時間と、操作を実装する方法の意味を獲得するヘルプするを保存できます。</span><span class="sxs-lookup"><span data-stu-id="f9037-220">When working on your own projects, automatic code generation can save you time and help you gain a sense of how to implement an operation.</span></span> <span data-ttu-id="f9037-221">このセクションでは、自動コード生成機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="f9037-221">This section describes the automatic code generation feature.</span></span> <span data-ttu-id="f9037-222">ここでは情報提供のみし、プロジェクトに実装する必要があります。 コードが含まれていません。</span><span class="sxs-lookup"><span data-stu-id="f9037-222">This section is only informational and does not contain any code you need to implement in your project.</span></span>

<span data-ttu-id="f9037-223">値を設定するときに、 **SelectMethod**、 **UpdateMethod**、 **InsertMethod**、または**DeleteMethod**マークアップのコードでのプロパティ選択することができます、**新しいメソッドの作成**オプション。</span><span class="sxs-lookup"><span data-stu-id="f9037-223">When setting a value for the **SelectMethod**, **UpdateMethod**, **InsertMethod**, or **DeleteMethod** properties in the markup code, you can select the **Create New Method** option.</span></span>

![新しいメソッドを作成します。](retrieving-data/_static/image18.png)

<span data-ttu-id="f9037-225">Visual Studio は、分離コードで適切なシグネチャを持つメソッドを作成するだけでなく、操作の実行に役立つ実装コードも生成されます。</span><span class="sxs-lookup"><span data-stu-id="f9037-225">Visual Studio not only creates a method in the code-behind with the proper signature, but also generates implementation code to assist you with performing the operation.</span></span> <span data-ttu-id="f9037-226">最初に設定する場合、 **ItemType** 、生成されたコードの自動コード生成機能を使用する前にプロパティは、操作の型を使用します。</span><span class="sxs-lookup"><span data-stu-id="f9037-226">If you first set the **ItemType** property before using the automatic code generation feature, the generated code will use that type for the operations.</span></span> <span data-ttu-id="f9037-227">たとえば、UpdateMethod プロパティを設定するときに自動的に次のコードが生成されます。</span><span class="sxs-lookup"><span data-stu-id="f9037-227">For example, when setting the UpdateMethod property, the following code is automatically generated:</span></span>

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

<span data-ttu-id="f9037-228">もう一度、上記のコードは、プロジェクトに追加する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="f9037-228">Again, the above code does not need to be added to your project.</span></span> <span data-ttu-id="f9037-229">次のチュートリアルでは、更新、削除、および新しいデータを追加するためのメソッドを実装します。</span><span class="sxs-lookup"><span data-stu-id="f9037-229">In the next tutorial, you will implement methods for updating, deleting and adding new data.</span></span>

## <a name="conclusion"></a><span data-ttu-id="f9037-230">まとめ</span><span class="sxs-lookup"><span data-stu-id="f9037-230">Conclusion</span></span>

<span data-ttu-id="f9037-231">このチュートリアルでは、データ モデル クラスを作成し、それらのクラスからデータベースを生成します。</span><span class="sxs-lookup"><span data-stu-id="f9037-231">In this tutorial, you created data model classes and generated a database from those classes.</span></span> <span data-ttu-id="f9037-232">データベースのテーブルは、テスト データが入力されます。</span><span class="sxs-lookup"><span data-stu-id="f9037-232">You filled the database tables with test data.</span></span> <span data-ttu-id="f9037-233">データベースからデータを取得するモデル バインドを使用して GridView にデータを表示します。</span><span class="sxs-lookup"><span data-stu-id="f9037-233">You used model binding to retrieve data from the database, and then displayed the data in a GridView.</span></span>

<span data-ttu-id="f9037-234">次の[チュートリアル](updating-deleting-and-creating-data.md)この系列には有効にする更新、削除、およびデータを作成します。</span><span class="sxs-lookup"><span data-stu-id="f9037-234">In the next [tutorial](updating-deleting-and-creating-data.md) in this series, you will enable updating, deleting, and creating data.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f9037-235">次へ</span><span class="sxs-lookup"><span data-stu-id="f9037-235">Next</span></span>](updating-deleting-and-creating-data.md)
