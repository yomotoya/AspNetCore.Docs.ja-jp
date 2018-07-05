---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: ASP.NET MVC 4 モデルとデータ アクセス |Microsoft Docs
author: rick-anderson
description: '注: このハンズオン ラボでは、ASP.NET MVC の基本的な知識がある前提としています。 前に ASP.NET MVC を使用していない場合をお勧めすると、ASP.NET MVC 4 を経由しています.'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: afc03d87431632bbb3ab59241de0edb4bb7af12d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371130"
---
# <a name="aspnet-mvc-4-models-and-data-access"></a><span data-ttu-id="94cee-104">ASP.NET MVC 4 モデルとデータ アクセス</span><span class="sxs-lookup"><span data-stu-id="94cee-104">ASP.NET MVC 4 Models and Data Access</span></span>

<span data-ttu-id="94cee-105">によって[Web キャンプ チーム](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="94cee-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="94cee-106">Web のキャンプ トレーニング キットをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="94cee-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="94cee-107">このハンズオン ラボは、の基本的な知識があることを前提**ASP.NET MVC**します。</span><span class="sxs-lookup"><span data-stu-id="94cee-107">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="94cee-108">使用していない場合**ASP.NET MVC**前を経由することを勧めします。 **ASP.NET MVC 4 の基礎**ハンズオン ラボ。</span><span class="sxs-lookup"><span data-stu-id="94cee-108">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="94cee-109">このラボでは、拡張機能とソース フォルダーにサンプル Web アプリケーションに軽微な変更を適用することで以前に説明する新機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="94cee-109">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="94cee-110">すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[Microsoft の Web/WebCampTrainingKit リリース](https://aka.ms/webcamps-training-kit)します。</span><span class="sxs-lookup"><span data-stu-id="94cee-110">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="94cee-111">このラボに固有のプロジェクトは、「 [ASP.NET MVC 4 モデルとデータ アクセス](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess)します。</span><span class="sxs-lookup"><span data-stu-id="94cee-111">The project specific to this lab is available at [ASP.NET MVC 4 Models and Data Access](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).</span></span>

<span data-ttu-id="94cee-112">**ASP.NET MVC の基礎**のハンズオン ラボを渡そうとしてがされているハード コードされたデータ、コント ローラーからテンプレートの表示にします。</span><span class="sxs-lookup"><span data-stu-id="94cee-112">In **ASP.NET MVC Fundamentals** Hands-on Lab, you have been passing hard-coded data from the Controllers to the View templates.</span></span> <span data-ttu-id="94cee-113">しかし、実際の Web アプリケーションを構築するために実際のデータベースを使用したい場合があります。</span><span class="sxs-lookup"><span data-stu-id="94cee-113">But, in order to build a real Web application, you might want to use a real database.</span></span>

<span data-ttu-id="94cee-114">このハンズオン ラボでは、ミュージック ストア アプリケーションに必要なデータ格納および取得するためにデータベース エンジンを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="94cee-114">This Hands-on Lab will show you how to use a database engine in order to store and retrieve the data needed for the Music Store application.</span></span> <span data-ttu-id="94cee-115">これを実現がするには、既存のデータベースで開始し、そこから、Entity Data Model を作成します。</span><span class="sxs-lookup"><span data-stu-id="94cee-115">To accomplish that, you will start with an existing database and create the Entity Data Model from it.</span></span> <span data-ttu-id="94cee-116">この演習では、全体で変化する、 **Database First**方法だけでなく**Code First**アプローチします。</span><span class="sxs-lookup"><span data-stu-id="94cee-116">Throughout this lab, you will meet the **Database First** approach as well as the **Code First** approach.</span></span>

<span data-ttu-id="94cee-117">ただし、使用することできますも、 **Model First**アプローチや、ツールを使用して、同じモデルを作成し、そこから、データベースを生成します。</span><span class="sxs-lookup"><span data-stu-id="94cee-117">However, you can also use the **Model First** approach, create the same model using the tools, and then generate the database from it.</span></span>

<span data-ttu-id="94cee-118">![Vs を最初にデータベース。モデル ファースト](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First とします。まずモデルします。")</span><span class="sxs-lookup"><span data-stu-id="94cee-118">![Database First vs. Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First vs. Model First")</span></span>

<span data-ttu-id="94cee-119">*Vs を最初にデータベース。まずモデルします。*</span><span class="sxs-lookup"><span data-stu-id="94cee-119">*Database First vs. Model First*</span></span>

<span data-ttu-id="94cee-120">モデルを生成した後にハード コーディングされたデータを使用する代わりに、データベースから取得したデータ ストアのビューを提供する StoreController で適切な調整が作成されます。</span><span class="sxs-lookup"><span data-stu-id="94cee-120">After generating the Model, you will make the proper adjustments in the StoreController to provide the Store Views with the data taken from the database, instead of using hard-coded data.</span></span> <span data-ttu-id="94cee-121">今度は、データは、データベースから取得されますが、ビュー テンプレートに同じビューモデルが戻される、StoreController のため、テンプレートの表示に変更を加えることは必要はありません。</span><span class="sxs-lookup"><span data-stu-id="94cee-121">You will not need to make any change to the View templates because the StoreController will be returning the same ViewModels to the View templates, although this time the data will come from the database.</span></span>

<span data-ttu-id="94cee-122">**Code First 手法**</span><span class="sxs-lookup"><span data-stu-id="94cee-122">**The Code First Approach**</span></span>

<span data-ttu-id="94cee-123">Code First アプローチでは、フレームワークを組み合わせて、一般的にクラスを生成せず、コードからモデルを定義できます。</span><span class="sxs-lookup"><span data-stu-id="94cee-123">The Code First approach allows us to define the model from the code without generating classes that are generally coupled with the framework.</span></span>

<span data-ttu-id="94cee-124">コードで最初に、モデル オブジェクトが定義されて、Poco で&quot;Plain Old CLR Object&quot;します。</span><span class="sxs-lookup"><span data-stu-id="94cee-124">In code first, model objects are defined with POCOs, &quot;Plain Old CLR Objects&quot;.</span></span> <span data-ttu-id="94cee-125">Poco は、単純なのプレーンなクラスを継承がないインターフェイスを実装していません。</span><span class="sxs-lookup"><span data-stu-id="94cee-125">POCOs are simple plain classes that have no inheritance and do not implement interfaces.</span></span> <span data-ttu-id="94cee-126">これらのファイルからデータベースを自動的に生成することができます。 または既存のデータベースを使用して、コードからクラスのマッピングを生成します。</span><span class="sxs-lookup"><span data-stu-id="94cee-126">We can automatically generate the database from them, or we can use an existing database and generate the class mapping from the code.</span></span>

<span data-ttu-id="94cee-127">このアプローチを使用する利点は、マッピング フレームワークでは、Poco クラスは結合されていないと、モデルは (この例では Entity Framework) では、永続化フレームワークから独立してです。</span><span class="sxs-lookup"><span data-stu-id="94cee-127">The benefits of using this approach is that the Model remains independent from the persistence framework (in this case, Entity Framework), as the POCOs classes are not coupled with the mapping framework.</span></span>

> [!NOTE]
> <span data-ttu-id="94cee-128">このラボは ASP.NET MVC 4 に基づくし、ミュージック ストア サンプル アプリケーションのバージョンに合わせてこのハンズオン ラボで表示されている機能のみを最小限に抑えます。</span><span class="sxs-lookup"><span data-stu-id="94cee-128">This Lab is based on ASP.NET MVC 4 and a version of the Music Store sample application customized and minimized to fit only the features shown in this Hands-On Lab.</span></span>
> 
> <span data-ttu-id="94cee-129">全体を検証する場合**ミュージック ストア**で見つかりますチュートリアル アプリケーション[MVC-ミュージック ストア](https://github.com/evilDave/MVC-Music-Store)します。</span><span class="sxs-lookup"><span data-stu-id="94cee-129">If you wish to explore the whole **Music Store** tutorial application you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="94cee-130">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="94cee-130">Prerequisites</span></span>

<span data-ttu-id="94cee-131">このラボを完成させるのには、次の項目が必要です。</span><span class="sxs-lookup"><span data-stu-id="94cee-131">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="94cee-132">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)または上位 (読み取り[付録 A](#AppendixA)をインストールする方法について)。</span><span class="sxs-lookup"><span data-stu-id="94cee-132">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="94cee-133">セットアップ</span><span class="sxs-lookup"><span data-stu-id="94cee-133">Setup</span></span>

<span data-ttu-id="94cee-134">**コード スニペットをインストールします。**</span><span class="sxs-lookup"><span data-stu-id="94cee-134">**Installing Code Snippets**</span></span>

<span data-ttu-id="94cee-135">便宜上、このラボに管理するコードの多くは、Visual Studio コード スニペットとして利用できます。</span><span class="sxs-lookup"><span data-stu-id="94cee-135">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="94cee-136">実行コード スニペットをインストールする **.\Source\Setup\CodeSnippets.vsi**ファイル。</span><span class="sxs-lookup"><span data-stu-id="94cee-136">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="94cee-137">このドキュメントの付録を参照することができます、Visual Studio のコード スニペットとその使用方法を学習するに慣れていない場合&quot;[付録 c: を使用してコード スニペット](#AppendixC)&quot;します。</span><span class="sxs-lookup"><span data-stu-id="94cee-137">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="94cee-138">演習</span><span class="sxs-lookup"><span data-stu-id="94cee-138">Exercises</span></span>

<span data-ttu-id="94cee-139">次の演習では、このハンズオン ラボから成ります。</span><span class="sxs-lookup"><span data-stu-id="94cee-139">This Hands-on Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="94cee-140">手順 1: データベースの追加</span><span class="sxs-lookup"><span data-stu-id="94cee-140">Exercise 1: Adding a Database</span></span>](#Exercise1)
2. [<span data-ttu-id="94cee-141">手順 2: Code First を使用してデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="94cee-141">Exercise 2: Creating a Database using Code First</span></span>](#Exercise2)
3. [<span data-ttu-id="94cee-142">手順 3: パラメーターを持つデータベースの照会</span><span class="sxs-lookup"><span data-stu-id="94cee-142">Exercise 3: Querying the Database with Parameters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="94cee-143">各演習が用意されており、**エンド**演習を完了した後に取得する必要があります、結果として得られるソリューションに含まれているフォルダー。</span><span class="sxs-lookup"><span data-stu-id="94cee-143">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="94cee-144">作業、演習を通じて追加のヘルプが必要な場合は、このソリューションをガイドとして使用できます。</span><span class="sxs-lookup"><span data-stu-id="94cee-144">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="94cee-145">この演習の所要時間を推定: **35 分**します。</span><span class="sxs-lookup"><span data-stu-id="94cee-145">Estimated time to complete this lab: **35 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a><span data-ttu-id="94cee-146">手順 1: データベースの追加</span><span class="sxs-lookup"><span data-stu-id="94cee-146">Exercise 1: Adding a Database</span></span>

<span data-ttu-id="94cee-147">この演習では、そのデータを使用するために、ソリューションに MusicStore アプリケーションのテーブルを持つデータベースを追加する方法を学びます。</span><span class="sxs-lookup"><span data-stu-id="94cee-147">In this exercise, you will learn how to add a database with the tables of the MusicStore application to the solution in order to consume its data.</span></span> <span data-ttu-id="94cee-148">データベースがモデルでは、生成され、ソリューションに追加、ハード コーディングされた値を使用する代わりに、データベースから取得されたデータにビュー テンプレートを提供する StoreController クラスを変更します。</span><span class="sxs-lookup"><span data-stu-id="94cee-148">Once the database is generated with the model, and added to the solution, you will modify the StoreController class to provide the View template with the data taken from the database, instead of using hard-coded values.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a><span data-ttu-id="94cee-149">タスク 1 - データベースの追加</span><span class="sxs-lookup"><span data-stu-id="94cee-149">Task 1 - Adding a Database</span></span>

<span data-ttu-id="94cee-150">このタスクでは、ソリューションに MusicStore アプリケーションのメインのテーブルでは、既に作成したデータベースを追加します。</span><span class="sxs-lookup"><span data-stu-id="94cee-150">In this task, you will add an already created database with the main tables of the MusicStore application to the solution.</span></span>

1. <span data-ttu-id="94cee-151">開く、**開始**ソリューションがある**ソース/Ex1-AddingADatabaseDBFirst/開始/** フォルダー。</span><span class="sxs-lookup"><span data-stu-id="94cee-151">Open the **Begin** solution located at **Source/Ex1-AddingADatabaseDBFirst/Begin/** folder.</span></span>

   1. <span data-ttu-id="94cee-152">いくつか不足している NuGet パッケージをダウンロードする必要がありますが続行する前にします。</span><span class="sxs-lookup"><span data-stu-id="94cee-152">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="94cee-153">これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。</span><span class="sxs-lookup"><span data-stu-id="94cee-153">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="94cee-154">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。</span><span class="sxs-lookup"><span data-stu-id="94cee-154">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="94cee-155">最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。</span><span class="sxs-lookup"><span data-stu-id="94cee-155">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="94cee-156">NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="94cee-156">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="94cee-157">With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="94cee-157">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="94cee-158">これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="94cee-158">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="94cee-159">追加**MvcMusicStore**データベース ファイル。</span><span class="sxs-lookup"><span data-stu-id="94cee-159">Add **MvcMusicStore** database file.</span></span> <span data-ttu-id="94cee-160">このハンズオン ラボと呼ばれる作成済みのデータベースを使用して**MvcMusicStore.mdf**します。</span><span class="sxs-lookup"><span data-stu-id="94cee-160">In this Hands-on Lab, you will use an already created database called **MvcMusicStore.mdf**.</span></span> <span data-ttu-id="94cee-161">そのためには、右クリック**アプリ\_データ**フォルダーをポイントして**追加**順にクリックします**既存項目の**します。</span><span class="sxs-lookup"><span data-stu-id="94cee-161">To do that, right-click **App\_Data** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="94cee-162">参照する**\Source\Assets**を選択し、 **MvcMusicStore.mdf**ファイル。</span><span class="sxs-lookup"><span data-stu-id="94cee-162">Browse to **\Source\Assets** and select the **MvcMusicStore.mdf** file.</span></span>

    <span data-ttu-id="94cee-163">![既存の項目を追加する](aspnet-mvc-4-models-and-data-access/_static/image2.png "既存の項目の追加")</span><span class="sxs-lookup"><span data-stu-id="94cee-163">![Adding an Existing Item](aspnet-mvc-4-models-and-data-access/_static/image2.png "Adding an Existing Item")</span></span>

    <span data-ttu-id="94cee-164">*既存の項目を追加します。*</span><span class="sxs-lookup"><span data-stu-id="94cee-164">*Adding an Existing Item*</span></span>

    <span data-ttu-id="94cee-165">![データベース ファイルの MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf データベース ファイル")</span><span class="sxs-lookup"><span data-stu-id="94cee-165">![MvcMusicStore.mdf database file](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf database file")</span></span>

    <span data-ttu-id="94cee-166">*MvcMusicStore.mdf データベース ファイル*</span><span class="sxs-lookup"><span data-stu-id="94cee-166">*MvcMusicStore.mdf database file*</span></span>

    <span data-ttu-id="94cee-167">データベースがプロジェクトに追加されました。</span><span class="sxs-lookup"><span data-stu-id="94cee-167">The database has been added to the project.</span></span> <span data-ttu-id="94cee-168">データベースが、ソリューション内にある場合でもは、クエリを実行し、別のデータベース サーバーでホストされているように更新します。</span><span class="sxs-lookup"><span data-stu-id="94cee-168">Even when the database is located inside the solution, you can query and update it as it was hosted in a different database server.</span></span>

    <span data-ttu-id="94cee-169">![ソリューション エクスプ ローラーでデータベースを MvcMusicStore](aspnet-mvc-4-models-and-data-access/_static/image4.png "ソリューション エクスプ ローラーで MvcMusicStore データベース")</span><span class="sxs-lookup"><span data-stu-id="94cee-169">![MvcMusicStore database in Solution Explorer](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore database in Solution Explorer")</span></span>

    <span data-ttu-id="94cee-170">*ソリューション エクスプ ローラーで MvcMusicStore データベース*</span><span class="sxs-lookup"><span data-stu-id="94cee-170">*MvcMusicStore database in Solution Explorer*</span></span>
3. <span data-ttu-id="94cee-171">データベースへの接続を確認します。</span><span class="sxs-lookup"><span data-stu-id="94cee-171">Verify the connection to the database.</span></span> <span data-ttu-id="94cee-172">これを行うには、ダブルクリック**MvcMusicStore.mdf**接続を確立します。</span><span class="sxs-lookup"><span data-stu-id="94cee-172">To do this, double-click **MvcMusicStore.mdf** to establish a connection.</span></span>

    <span data-ttu-id="94cee-173">![MvcMusicStore.mdf への接続](aspnet-mvc-4-models-and-data-access/_static/image5.png "MvcMusicStore.mdf への接続")</span><span class="sxs-lookup"><span data-stu-id="94cee-173">![Connecting to MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "Connecting to MvcMusicStore.mdf")</span></span>

    <span data-ttu-id="94cee-174">*MvcMusicStore.mdf への接続*</span><span class="sxs-lookup"><span data-stu-id="94cee-174">*Connecting to MvcMusicStore.mdf*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a><span data-ttu-id="94cee-175">タスク 2 - データ モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="94cee-175">Task 2 - Creating a Data Model</span></span>

<span data-ttu-id="94cee-176">このタスクでは、前のタスクで追加されたデータベースと対話するデータ モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="94cee-176">In this task, you will create a data model to interact with the database added in the previous task.</span></span>

1. <span data-ttu-id="94cee-177">データベースを表すデータ モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="94cee-177">Create a data model that will represent the database.</span></span> <span data-ttu-id="94cee-178">ソリューション エクスプ ローラーを右クリックで、**モデル**フォルダーを指す**追加**順にクリックします**新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="94cee-178">To do this, in Solution Explorer right-click the **Models** folder, point to **Add** and then click **New Item**.</span></span> <span data-ttu-id="94cee-179">**新しい項目の追加**ダイアログ ボックスで、**データ**テンプレートをクリックし、 **ADO.NET Entity Data Model**項目。</span><span class="sxs-lookup"><span data-stu-id="94cee-179">In the **Add New Item** dialog, select the **Data** template and then the **ADO.NET Entity Data Model** item.</span></span> <span data-ttu-id="94cee-180">データ モデル名を変更して**StoreDB.edmx**クリック**追加**します。</span><span class="sxs-lookup"><span data-stu-id="94cee-180">Change the data model name to **StoreDB.edmx** and click **Add**.</span></span>

    <span data-ttu-id="94cee-181">![StoreDB ADO.NET Entity Data Model を追加する](aspnet-mvc-4-models-and-data-access/_static/image6.png "StoreDB ADO.NET エンティティ データ モデルの追加")</span><span class="sxs-lookup"><span data-stu-id="94cee-181">![Adding the StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "Adding the StoreDB ADO.NET Entity Data Model")</span></span>

    <span data-ttu-id="94cee-182">*StoreDB ADO.NET エンティティ データ モデルの追加*</span><span class="sxs-lookup"><span data-stu-id="94cee-182">*Adding the StoreDB ADO.NET Entity Data Model*</span></span>
2. <span data-ttu-id="94cee-183">**Entity Data Model ウィザード**が表示されます。</span><span class="sxs-lookup"><span data-stu-id="94cee-183">The **Entity Data Model Wizard** will appear.</span></span> <span data-ttu-id="94cee-184">このウィザードでモデル レイヤーの作成手順を説明します。</span><span class="sxs-lookup"><span data-stu-id="94cee-184">This wizard will guide you through the creation of the model layer.</span></span> <span data-ttu-id="94cee-185">追加、既存のデータベース recentyl に基づいてモデルを作成する必要があります、ために選択**データベースから生成** をクリック**次**します。</span><span class="sxs-lookup"><span data-stu-id="94cee-185">Since the model should be created based on the existing database recentyl added, select **Generate from database** and click **Next**.</span></span>

    <span data-ttu-id="94cee-186">![モデル コンテンツを選択する](aspnet-mvc-4-models-and-data-access/_static/image7.png "モデル コンテンツを選択します。")</span><span class="sxs-lookup"><span data-stu-id="94cee-186">![Choosing the model content](aspnet-mvc-4-models-and-data-access/_static/image7.png "Choosing the model content")</span></span>

    <span data-ttu-id="94cee-187">*モデル コンテンツを選択します。*</span><span class="sxs-lookup"><span data-stu-id="94cee-187">*Choosing the model content*</span></span>
3. <span data-ttu-id="94cee-188">データベースからモデルを生成するため使用する接続を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="94cee-188">Since you are generating a model from a database, you will need to specify the connection to use.</span></span> <span data-ttu-id="94cee-189">クリックして**新しい接続**します。</span><span class="sxs-lookup"><span data-stu-id="94cee-189">Click **New Connection**.</span></span>
4. <span data-ttu-id="94cee-190">選択**Microsoft SQL Server データベース ファイル**クリック**続行**します。</span><span class="sxs-lookup"><span data-stu-id="94cee-190">Select **Microsoft SQL Server Database File** and click **Continue**.</span></span>

    <span data-ttu-id="94cee-191">![データ ソース](aspnet-mvc-4-models-and-data-access/_static/image8.png "データ ソースの選択")</span><span class="sxs-lookup"><span data-stu-id="94cee-191">![Choose data source](aspnet-mvc-4-models-and-data-access/_static/image8.png "Choose data source")</span></span>

    <span data-ttu-id="94cee-192">*データ ソース ダイアログを選択します。*</span><span class="sxs-lookup"><span data-stu-id="94cee-192">*Choose data source dialog*</span></span>
5. <span data-ttu-id="94cee-193">をクリックして**参照**、データベースを選択します**MvcMusicStore.mdf**内にある、**アプリ\_データ**フォルダーをクリックします**OK**します。</span><span class="sxs-lookup"><span data-stu-id="94cee-193">Click **Browse** and select the database **MvcMusicStore.mdf** you located in the **App\_Data** folder and click **OK**.</span></span>

    <span data-ttu-id="94cee-194">![接続プロパティ](aspnet-mvc-4-models-and-data-access/_static/image9.png "接続のプロパティ")</span><span class="sxs-lookup"><span data-stu-id="94cee-194">![Connection properties](aspnet-mvc-4-models-and-data-access/_static/image9.png "Connection properties")</span></span>

    <span data-ttu-id="94cee-195">*接続のプロパティ*</span><span class="sxs-lookup"><span data-stu-id="94cee-195">*Connection properties*</span></span>
6. <span data-ttu-id="94cee-196">生成されたクラスする必要がありますエンティティ接続文字列と同じ名前を指定しているため、その名前を**MusicStoreEntities**  をクリック**次**します。</span><span class="sxs-lookup"><span data-stu-id="94cee-196">The generated class should have the same name as the entity connection string, so change its name to **MusicStoreEntities** and click **Next**.</span></span>

    <span data-ttu-id="94cee-197">![データ接続を選択する](aspnet-mvc-4-models-and-data-access/_static/image10.png "データ接続を選択します。")</span><span class="sxs-lookup"><span data-stu-id="94cee-197">![Choosing the data connection](aspnet-mvc-4-models-and-data-access/_static/image10.png "Choosing the data connection")</span></span>

    <span data-ttu-id="94cee-198">*データ接続を選択します。*</span><span class="sxs-lookup"><span data-stu-id="94cee-198">*Choosing the data connection*</span></span>
7. <span data-ttu-id="94cee-199">使用するデータベース オブジェクトを選択します。</span><span class="sxs-lookup"><span data-stu-id="94cee-199">Choose the database objects to use.</span></span> <span data-ttu-id="94cee-200">エンティティ モデルは、データベースのテーブルだけを使用して、選択、**テーブル**オプションを確認して、**モデルに外部キー列を含める**と**化または単数生成オブジェクト名**もオプションが選択されます。</span><span class="sxs-lookup"><span data-stu-id="94cee-200">As the Entity Model will use just the database's tables, select the **Tables** option, and make sure that the **Include foreign key columns in the model** and **Pluralize or singularize generated object names** options are also selected.</span></span> <span data-ttu-id="94cee-201">変更をモデル Namespace **MvcMusicStore.Model**  をクリック**完了**します。</span><span class="sxs-lookup"><span data-stu-id="94cee-201">Change the Model Namespace to **MvcMusicStore.Model** and click **Finish**.</span></span>

    <span data-ttu-id="94cee-202">![データベース オブジェクトを選択する](aspnet-mvc-4-models-and-data-access/_static/image11.png "データベース オブジェクトを選択します。")</span><span class="sxs-lookup"><span data-stu-id="94cee-202">![Choosing the database objects](aspnet-mvc-4-models-and-data-access/_static/image11.png "Choosing the database objects")</span></span>

    <span data-ttu-id="94cee-203">*データベース オブジェクトを選択します。*</span><span class="sxs-lookup"><span data-stu-id="94cee-203">*Choosing the database objects*</span></span>

    > [!NOTE]
    > <span data-ttu-id="94cee-204">セキュリティ警告ダイアログが表示されている場合にクリックします**OK**をテンプレートを実行し、モデルのエンティティ クラスを生成します。</span><span class="sxs-lookup"><span data-stu-id="94cee-204">If a Security Warning dialog is shown, click **OK** to run the template and generate the classes for the model entities.</span></span>
8. <span data-ttu-id="94cee-205">各テーブルをデータベースにマップする別のクラスが作成するときに、データベースのエンティティの図が表示されます。</span><span class="sxs-lookup"><span data-stu-id="94cee-205">An entity diagram for the database will appear, while a separate class that maps each table to the database will be created.</span></span> <span data-ttu-id="94cee-206">たとえば、**アルバム**テーブルによって表されます、**アルバム**クラスは、テーブル内の各列がクラス プロパティにマップされます。</span><span class="sxs-lookup"><span data-stu-id="94cee-206">For example, the **Albums** table will be represented by an **Album** class, where each column in the table will map to a class property.</span></span> <span data-ttu-id="94cee-207">これは、使用すると、クエリを実行し、データベース内の行を表すオブジェクトを使用できます。</span><span class="sxs-lookup"><span data-stu-id="94cee-207">This will allow you to query and work with objects that represent rows in the database.</span></span>

    <span data-ttu-id="94cee-208">![エンティティの図](aspnet-mvc-4-models-and-data-access/_static/image12.png "エンティティ図")</span><span class="sxs-lookup"><span data-stu-id="94cee-208">![Entity diagram](aspnet-mvc-4-models-and-data-access/_static/image12.png "Entity diagram")</span></span>

    <span data-ttu-id="94cee-209">*エンティティの図*</span><span class="sxs-lookup"><span data-stu-id="94cee-209">*Entity diagram*</span></span>

    > [!NOTE]
    > <span data-ttu-id="94cee-210">T4 テンプレート (.tt) は、エンティティ クラスを生成するコードを実行し、同じ名前の既存のクラスが上書きされます。</span><span class="sxs-lookup"><span data-stu-id="94cee-210">The T4 templates (.tt) run code to generate the entities classes and will overwrite the existing classes with the same name.</span></span> <span data-ttu-id="94cee-211">この例では、クラスで&quot;アルバム&quot;、&quot;ジャンル&quot;と&quot;アーティスト&quot;が生成されたコードで上書きされます。</span><span class="sxs-lookup"><span data-stu-id="94cee-211">In this example, the classes &quot;Album&quot;, &quot;Genre&quot; and &quot;Artist&quot; were overwritten with the generated code.</span></span>


<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a><span data-ttu-id="94cee-212">タスク 3 - アプリケーションのビルド</span><span class="sxs-lookup"><span data-stu-id="94cee-212">Task 3 - Building the Application</span></span>

<span data-ttu-id="94cee-213">このタスクでは、確認が、モデルの生成が削除、**アルバム**、**ジャンル**と**アーティスト**モデル クラスでは、プロジェクトが正常にビルドを使用して新しいデータ モデル クラス。</span><span class="sxs-lookup"><span data-stu-id="94cee-213">In this task, you will check that, although the model generation have removed the **Album**, **Genre** and **Artist** model classes, the project builds successfully by using the new data model classes.</span></span>

1. <span data-ttu-id="94cee-214">選択してプロジェクトをビルド、**ビルド**メニュー項目をクリックし**ビルド MvcMusicStore**します。</span><span class="sxs-lookup"><span data-stu-id="94cee-214">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="94cee-215">![プロジェクトのビルド](aspnet-mvc-4-models-and-data-access/_static/image13.png "プロジェクトのビルド")</span><span class="sxs-lookup"><span data-stu-id="94cee-215">![Building the project](aspnet-mvc-4-models-and-data-access/_static/image13.png "Building the project")</span></span>

    <span data-ttu-id="94cee-216">*プロジェクトのビルド*</span><span class="sxs-lookup"><span data-stu-id="94cee-216">*Building the project*</span></span>
2. <span data-ttu-id="94cee-217">プロジェクトが正常にビルドします。</span><span class="sxs-lookup"><span data-stu-id="94cee-217">The project builds successfully.</span></span> <span data-ttu-id="94cee-218">理由は、引き続き動作しますか。</span><span class="sxs-lookup"><span data-stu-id="94cee-218">Why does it still work?</span></span> <span data-ttu-id="94cee-219">データベース テーブルが削除されたクラスに使用していたプロパティを含むフィールドをうまく**アルバム**と**ジャンル**します。</span><span class="sxs-lookup"><span data-stu-id="94cee-219">It works because the database tables have fields that include the properties that you were using in the removed classes **Album** and **Genre**.</span></span>

    <span data-ttu-id="94cee-220">![ビルドが成功した](aspnet-mvc-4-models-and-data-access/_static/image14.png "ビルドに成功しました")</span><span class="sxs-lookup"><span data-stu-id="94cee-220">![Builds succeeded](aspnet-mvc-4-models-and-data-access/_static/image14.png "Builds succeeded")</span></span>

    <span data-ttu-id="94cee-221">*ビルドに成功しました*</span><span class="sxs-lookup"><span data-stu-id="94cee-221">*Builds succeeded*</span></span>
3. <span data-ttu-id="94cee-222">デザイナーがダイアグラム形式のエンティティを表示する、c# のクラスでは本当に。</span><span class="sxs-lookup"><span data-stu-id="94cee-222">While the designer displays the entities in a diagram format, they are really C# classes.</span></span> <span data-ttu-id="94cee-223">展開、 **StoreDB.edmx**ソリューション エクスプ ローラーでノードをクリックし**StoreDB.tt**、新規生成されたエンティティが表示されます。</span><span class="sxs-lookup"><span data-stu-id="94cee-223">Expand the **StoreDB.edmx** node in the Solution Explorer and then **StoreDB.tt**, you will see the new generated entities.</span></span>

    <span data-ttu-id="94cee-224">![生成されたファイル](aspnet-mvc-4-models-and-data-access/_static/image15.png "によって生成されたファイル")</span><span class="sxs-lookup"><span data-stu-id="94cee-224">![Generated files](aspnet-mvc-4-models-and-data-access/_static/image15.png "Generated files")</span></span>

    <span data-ttu-id="94cee-225">*生成されたファイル*</span><span class="sxs-lookup"><span data-stu-id="94cee-225">*Generated files*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="94cee-226">タスク 4 - データベースのクエリを実行します。</span><span class="sxs-lookup"><span data-stu-id="94cee-226">Task 4 - Querying the Database</span></span>

<span data-ttu-id="94cee-227">このタスクでは、ハードコーディングされたデータを使用する代わりには、データベース情報を取得するクエリ実行できるように StoreController クラスを更新します。</span><span class="sxs-lookup"><span data-stu-id="94cee-227">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will query the database to retrieve the information.</span></span>

1. <span data-ttu-id="94cee-228">開いている**Controllers\StoreController.cs**のインスタンスを保持するクラスに次のフィールドを追加し、 **MusicStoreEntities**という名前のクラス**storeDB**:</span><span class="sxs-lookup"><span data-stu-id="94cee-228">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="94cee-229">(コード スニペット -*モデルとデータへのアクセス - Ex1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="94cee-229">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
2. <span data-ttu-id="94cee-230">**MusicStoreEntities**クラスは、データベース内の各テーブルのコレクション プロパティを公開します。</span><span class="sxs-lookup"><span data-stu-id="94cee-230">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="94cee-231">Update**参照**のすべてのジャンルを取得するアクション メソッド、**アルバム**します。</span><span class="sxs-lookup"><span data-stu-id="94cee-231">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="94cee-232">(コード スニペット -*モデルとデータ アクセス - Ex1 ストア参照*)</span><span class="sxs-lookup"><span data-stu-id="94cee-232">(Code Snippet - *Models And Data Access - Ex1 Store Browse*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

    > [!NOTE]
    > <span data-ttu-id="94cee-233">.NET と呼ばれる機能を使用している**LINQ** (統合言語クエリ) に対しては、データベースに対してコードを実行し、返すこのコレクションは、厳密に型指定されたクエリ式を記述するオブジェクトをプログラミングできますに対して。</span><span class="sxs-lookup"><span data-stu-id="94cee-233">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
    > 
    > <span data-ttu-id="94cee-234">LINQ の詳細についてを参照してください、 [msdn サイト](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="94cee-234">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span></span>
3. <span data-ttu-id="94cee-235">Update**インデックス**アクション メソッドは、すべてのジャンルを取得します。</span><span class="sxs-lookup"><span data-stu-id="94cee-235">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="94cee-236">(コード スニペット -*モデルとデータ アクセス - Ex1 ストア インデックス*)</span><span class="sxs-lookup"><span data-stu-id="94cee-236">(Code Snippet - *Models And Data Access - Ex1 Store Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
4. <span data-ttu-id="94cee-237">Update**インデックス**アクション メソッドをすべてのジャンルを取得し、一覧のコレクションを変換します。</span><span class="sxs-lookup"><span data-stu-id="94cee-237">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="94cee-238">(コード スニペット -*モデルとデータ アクセス - Ex1 ストア GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="94cee-238">(Code Snippet - *Models And Data Access - Ex1 Store GenreMenu*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="94cee-239">タスク 5 - アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="94cee-239">Task 5 - Running the Application</span></span>

<span data-ttu-id="94cee-240">このタスクでは、ストアのインデックス ページをハードコードされたものではなく、データベースに格納されているジャンルようになりました表示するかを確認します。</span><span class="sxs-lookup"><span data-stu-id="94cee-240">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="94cee-241">にビュー テンプレートを変更する必要はありません、 **StoreController**はエンティティを取得する同じと同様が、今度は、データは、データベースから取得されます。</span><span class="sxs-lookup"><span data-stu-id="94cee-241">There is no need to change the View template because the **StoreController** is returning the same entities as before, although this time the data will come from the database.</span></span>

1. <span data-ttu-id="94cee-242">ソリューションとキーを押してリビルド**F5**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="94cee-242">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="94cee-243">ホーム ページで、プロジェクトが開始されます。</span><span class="sxs-lookup"><span data-stu-id="94cee-243">The project starts in the Home page.</span></span> <span data-ttu-id="94cee-244">いることを確認のメニュー**ジャンル**一覧をハード コーディングをして、データは、データベースから直接取得します。</span><span class="sxs-lookup"><span data-stu-id="94cee-244">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    <span data-ttu-id="94cee-246">*データベースからジャンルを参照*</span><span class="sxs-lookup"><span data-stu-id="94cee-246">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="94cee-247">これで、ジャンルを参照し、アルバムがデータベースから設定されますを確認します。</span><span class="sxs-lookup"><span data-stu-id="94cee-247">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="94cee-248">![データベースからアルバムを閲覧](aspnet-mvc-4-models-and-data-access/_static/image17.png "データベースからアルバムを参照")</span><span class="sxs-lookup"><span data-stu-id="94cee-248">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image17.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="94cee-249">*データベースからアルバムを参照*</span><span class="sxs-lookup"><span data-stu-id="94cee-249">*Browsing Albums from the database*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a><span data-ttu-id="94cee-250">手順 2: 最初のコードを使用してデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="94cee-250">Exercise 2: Creating a Database Using Code First</span></span>

<span data-ttu-id="94cee-251">この演習では、Code First アプローチを使用して、MusicStore アプリケーションのテーブルを含むデータベースを作成する方法とそのデータにアクセスする方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="94cee-251">In this exercise, you will learn how to use the Code First approach to create a database with the tables of the MusicStore application, and how to access its data.</span></span>

<span data-ttu-id="94cee-252">モデルが生成されると、ハードコードされた値を使用する代わりに、データベースから取得されたデータにビュー テンプレートを提供する StoreController を変更します。</span><span class="sxs-lookup"><span data-stu-id="94cee-252">Once the model is generated, you will modify the StoreController to provide the View template with the data taken from the database, instead of using hardcoded values.</span></span>

> [!NOTE]
> <span data-ttu-id="94cee-253">演習 1 を完了したし、Database First アプローチでは以前、今すぐに別のプロセスで同じ結果を取得する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="94cee-253">If you have completed Exercise 1 and have already worked with the Database First approach, you will now learn how to get the same results with a different process.</span></span> <span data-ttu-id="94cee-254">手順 1 と共通のタスクは、読みやすくマークされています。</span><span class="sxs-lookup"><span data-stu-id="94cee-254">The tasks that are in common with Exercise 1 have been marked to make your reading easier.</span></span> <span data-ttu-id="94cee-255">手順 1 を完了していないコードの最初の方法を学習したい場合は、この演習からを起動し、トピックの完全なカバレッジを取得できます。</span><span class="sxs-lookup"><span data-stu-id="94cee-255">If you have not completed Exercise 1 but would like to learn the Code First approach, you can start from this exercise and get a full coverage of the topic.</span></span>


<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a><span data-ttu-id="94cee-256">タスク 1 - サンプル データを追加</span><span class="sxs-lookup"><span data-stu-id="94cee-256">Task 1 - Populating Sample Data</span></span>

<span data-ttu-id="94cee-257">このタスクで Code First を使用して最初作成時に、データベースにサンプル データを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="94cee-257">In this task, you will populate the database with sample data when it is intially created using Code-First.</span></span>

1. <span data-ttu-id="94cee-258">開く、**開始**ソリューションがある**ソース/Ex2-CreatingADatabaseCodeFirst/開始/** フォルダー。</span><span class="sxs-lookup"><span data-stu-id="94cee-258">Open the **Begin** solution located at **Source/Ex2-CreatingADatabaseCodeFirst/Begin/** folder.</span></span> <span data-ttu-id="94cee-259">使用を続ける可能性がありますそれ以外の場合、**エンド**ソリューションは、前の演習を完了して取得します。</span><span class="sxs-lookup"><span data-stu-id="94cee-259">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="94cee-260">指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。</span><span class="sxs-lookup"><span data-stu-id="94cee-260">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="94cee-261">これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。</span><span class="sxs-lookup"><span data-stu-id="94cee-261">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="94cee-262">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。</span><span class="sxs-lookup"><span data-stu-id="94cee-262">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="94cee-263">最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。</span><span class="sxs-lookup"><span data-stu-id="94cee-263">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="94cee-264">NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="94cee-264">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="94cee-265">With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="94cee-265">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="94cee-266">これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="94cee-266">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="94cee-267">追加、 **SampleData.cs**ファイルを**モデル**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="94cee-267">Add the **SampleData.cs** file to the **Models** folder.</span></span> <span data-ttu-id="94cee-268">そのためには、右クリック**モデル**フォルダーをポイントして**追加**順にクリックします**既存項目の**します。</span><span class="sxs-lookup"><span data-stu-id="94cee-268">To do that, right-click **Models** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="94cee-269">参照する**\Source\Assets**を選択し、 **SampleData.cs**ファイル。</span><span class="sxs-lookup"><span data-stu-id="94cee-269">Browse to **\Source\Assets** and select the **SampleData.cs** file.</span></span>

    <span data-ttu-id="94cee-270">![サンプル データは、コードを入力](aspnet-mvc-4-models-and-data-access/_static/image18.png "サンプル データは、コードを設定")</span><span class="sxs-lookup"><span data-stu-id="94cee-270">![Sample data populate code](aspnet-mvc-4-models-and-data-access/_static/image18.png "Sample data populate code")</span></span>

    <span data-ttu-id="94cee-271">*データのサンプル コードを設定します。*</span><span class="sxs-lookup"><span data-stu-id="94cee-271">*Sample data populate code*</span></span>
3. <span data-ttu-id="94cee-272">開く、 **Global.asax.cs**ファイルを開き、次の追加*を使用して*ステートメント。</span><span class="sxs-lookup"><span data-stu-id="94cee-272">Open the **Global.asax.cs** file and add the following *using* statements.</span></span>

    <span data-ttu-id="94cee-273">(コード スニペット -*モデルとデータ アクセス - Ex2 グローバル Asax Using*)</span><span class="sxs-lookup"><span data-stu-id="94cee-273">(Code Snippet - *Models And Data Access - Ex2 Global Asax Usings*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
4. <span data-ttu-id="94cee-274">**アプリケーション\_Start()** メソッドは、データベース初期化子を設定するのには、次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="94cee-274">In the **Application\_Start()** method add the following line to set the database initializer.</span></span>

    <span data-ttu-id="94cee-275">(コード スニペット -*モデルとデータ アクセス - Ex2 グローバル Asax SetInitializer*)</span><span class="sxs-lookup"><span data-stu-id="94cee-275">(Code Snippet - *Models And Data Access - Ex2 Global Asax SetInitializer*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a><span data-ttu-id="94cee-276">タスク 2 - データベースへの接続を構成します。</span><span class="sxs-lookup"><span data-stu-id="94cee-276">Task 2 - Configuring the connection to the Database</span></span>

<span data-ttu-id="94cee-277">作成できたので、プロジェクトに既にデータベースを追加した、 **Web.config**ファイルの接続文字列。</span><span class="sxs-lookup"><span data-stu-id="94cee-277">Now that you have already added a database to our project, you will write in the **Web.config** file the connection string.</span></span>

1. <span data-ttu-id="94cee-278">接続文字列に追加**Web.config**します。そのために開く**Web.config**プロジェクトのルートと置換の接続文字列で示される行で DefaultConnection をという名前で、 **&lt;connectionStrings&gt;** セクション。</span><span class="sxs-lookup"><span data-stu-id="94cee-278">Add a connection string at **Web.config**. To do that, open **Web.config** at project root and replace the connection string named DefaultConnection with this line in the **&lt;connectionStrings&gt;** section:</span></span>

    <span data-ttu-id="94cee-279">![Web.config ファイルの場所](aspnet-mvc-4-models-and-data-access/_static/image19.png "Web.config ファイルの場所")</span><span class="sxs-lookup"><span data-stu-id="94cee-279">![Web.config file location](aspnet-mvc-4-models-and-data-access/_static/image19.png "Web.config file location")</span></span>

    <span data-ttu-id="94cee-280">*Web.config ファイルの場所*</span><span class="sxs-lookup"><span data-stu-id="94cee-280">*Web.config file location*</span></span>

    [!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a><span data-ttu-id="94cee-281">タスク 3: モデルの使用</span><span class="sxs-lookup"><span data-stu-id="94cee-281">Task 3 - Working with the Model</span></span>

<span data-ttu-id="94cee-282">データベースのテーブル モデルをリンクする、データベースへの接続を構成しておくことようになりました。</span><span class="sxs-lookup"><span data-stu-id="94cee-282">Now that you have already configured the connection to the database, you will link the model with the database tables.</span></span> <span data-ttu-id="94cee-283">このタスクでは、Code First とデータベースにリンクするクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="94cee-283">In this task, you will create a class that will be linked to the database with Code First.</span></span> <span data-ttu-id="94cee-284">変更する必要があります、既存モデルの POCO クラスがあることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="94cee-284">Remember that there is an existent POCO model class that should be modified.</span></span>

> [!NOTE]
> <span data-ttu-id="94cee-285">演習 1 を完了すると場合、は、ウィザードによってこの手順が実行されたことがわかります。</span><span class="sxs-lookup"><span data-stu-id="94cee-285">If you completed Exercise 1, you will note that this step was performed by a wizard.</span></span> <span data-ttu-id="94cee-286">Code First により、データ エンティティにリンクするクラスを手動で作成されます。</span><span class="sxs-lookup"><span data-stu-id="94cee-286">By doing Code First, you will manually create classes that will be linked to data entities.</span></span>

1. <span data-ttu-id="94cee-287">POCO のモデル クラスを開きます**ジャンル**から**モデル**プロジェクト フォルダーと ID を含める</span><span class="sxs-lookup"><span data-stu-id="94cee-287">Open the POCO model class **Genre** from **Models** project folder and include an ID.</span></span> <span data-ttu-id="94cee-288">Int 型のプロパティを使用して、名前を持つ**GenreId**します。</span><span class="sxs-lookup"><span data-stu-id="94cee-288">Use an int property with the name **GenreId**.</span></span>

    <span data-ttu-id="94cee-289">(コード スニペット -*モデルとデータ アクセス - Ex2 コードの最初のジャンル*)</span><span class="sxs-lookup"><span data-stu-id="94cee-289">(Code Snippet - *Models And Data Access - Ex2 Code First Genre*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

    > [!NOTE]
    > <span data-ttu-id="94cee-290">Code First 規約を使用するには、クラスのジャンルに主キー プロパティが自動的に検出することが必要です。</span><span class="sxs-lookup"><span data-stu-id="94cee-290">To work with Code First conventions, the class Genre must have a primary key property that will be automatically detected.</span></span>
    > 
    > <span data-ttu-id="94cee-291">詳細については、この最初の規則をコード[msdn の記事](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="94cee-291">You can read more about Code First Conventions in this [msdn article](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).</span></span>
2. <span data-ttu-id="94cee-292">これで、POCO のモデル クラスを開く**アルバム**から**モデル**プロジェクト フォルダー、外部キーを含めると、名前のプロパティを作成**GenreId**と**ArtistId**します。</span><span class="sxs-lookup"><span data-stu-id="94cee-292">Now, open the POCO model class **Album** from **Models** project folder and include the foreign keys, create properties with the names **GenreId** and **ArtistId**.</span></span> <span data-ttu-id="94cee-293">このクラスが既にある、 **GenreId**の主キー。</span><span class="sxs-lookup"><span data-stu-id="94cee-293">This class already have the **GenreId** for the primary key.</span></span>

    <span data-ttu-id="94cee-294">(コード スニペット -*モデルとデータ アクセス - Ex2 コードの最初のアルバム*)</span><span class="sxs-lookup"><span data-stu-id="94cee-294">(Code Snippet - *Models And Data Access - Ex2 Code First Album*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
3. <span data-ttu-id="94cee-295">POCO のモデル クラスを開きます**アーティスト**を含めると、 **ArtistId**プロパティ。</span><span class="sxs-lookup"><span data-stu-id="94cee-295">Open the POCO model class **Artist** and include the **ArtistId** property.</span></span>

    <span data-ttu-id="94cee-296">(コード スニペット -*モデルとデータ アクセス - Ex2 コードの最初のアーティスト*)</span><span class="sxs-lookup"><span data-stu-id="94cee-296">(Code Snippet - *Models And Data Access - Ex2 Code First Artist*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
4. <span data-ttu-id="94cee-297">右クリックし、**モデル**プロジェクト フォルダーと選択**追加 |クラス**します。</span><span class="sxs-lookup"><span data-stu-id="94cee-297">Right-click the **Models** project folder and select **Add | Class**.</span></span> <span data-ttu-id="94cee-298">ファイルに名前を**MusicStoreEntities.cs**します。</span><span class="sxs-lookup"><span data-stu-id="94cee-298">Name the file **MusicStoreEntities.cs**.</span></span> <span data-ttu-id="94cee-299">をクリックし、**追加します。**</span><span class="sxs-lookup"><span data-stu-id="94cee-299">Then, click **Add.**</span></span>

    <span data-ttu-id="94cee-300">![クラスの追加](aspnet-mvc-4-models-and-data-access/_static/image20.png "クラスの追加")</span><span class="sxs-lookup"><span data-stu-id="94cee-300">![Adding a class](aspnet-mvc-4-models-and-data-access/_static/image20.png "Adding a class")</span></span>

    <span data-ttu-id="94cee-301">*新しい項目の追加*</span><span class="sxs-lookup"><span data-stu-id="94cee-301">*Adding a new item*</span></span>

    <span data-ttu-id="94cee-302">![追加、class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "class2 を追加します。")</span><span class="sxs-lookup"><span data-stu-id="94cee-302">![Adding a class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Adding a class2")</span></span>

    <span data-ttu-id="94cee-303">*クラスの追加*</span><span class="sxs-lookup"><span data-stu-id="94cee-303">*Adding a class*</span></span>
5. <span data-ttu-id="94cee-304">先ほど作成したクラスを開きます**MusicStoreEntities.cs**、名前空間を含めると**System.Data.Entity**と**System.Data.Entity.Infrastructure**します。</span><span class="sxs-lookup"><span data-stu-id="94cee-304">Open the class you have just created, **MusicStoreEntities.cs**, and include the namespaces **System.Data.Entity** and **System.Data.Entity.Infrastructure**.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
6. <span data-ttu-id="94cee-305">拡張するクラスの宣言を置き換える、 **DbContext**クラス: パブリック宣言**DBSet**オーバーライドと**OnModelCreating**メソッド。</span><span class="sxs-lookup"><span data-stu-id="94cee-305">Replace the class declaration to extend the **DbContext** class: declare a public **DBSet** and override **OnModelCreating** method.</span></span> <span data-ttu-id="94cee-306">この手順の後に、Entity Framework を使用したモデルにリンクされるドメイン クラスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="94cee-306">After this step you will get a domain class that will link your model with the Entity Framework.</span></span> <span data-ttu-id="94cee-307">そのためには、次のようにクラス コードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="94cee-307">In order to do that, replace the class code with the following:</span></span>

    <span data-ttu-id="94cee-308">(コード スニペット -*モデルとデータ アクセス - Ex2 コードの最初の MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="94cee-308">(Code Snippet - *Models And Data Access - Ex2 Code First MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> <span data-ttu-id="94cee-309">Entity Framework で**DbContext**と**DBSet**ジャンル POCO クラスを照会することができます。</span><span class="sxs-lookup"><span data-stu-id="94cee-309">With Entity Framework **DbContext** and **DBSet** you will be able to query the POCO class Genre.</span></span> <span data-ttu-id="94cee-310">拡張することによって**OnModelCreating**で指定するメソッド、**コード**ジャンルがデータベース テーブルにマップする方法。</span><span class="sxs-lookup"><span data-stu-id="94cee-310">By extending **OnModelCreating** method, you are specifying in the **code** how Genre will be mapped to a database table.</span></span> <span data-ttu-id="94cee-311">この msdn の記事で DBContext、DBSet の詳細が見つかります:[リンク](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)</span><span class="sxs-lookup"><span data-stu-id="94cee-311">You can find more information about DBContext and DBSet in this msdn article: [link](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="94cee-312">タスク 4 - データベースのクエリを実行します。</span><span class="sxs-lookup"><span data-stu-id="94cee-312">Task 4 - Querying the Database</span></span>

<span data-ttu-id="94cee-313">このタスクでは、ハードコーディングされたデータを使用する代わりがから取得するために、データベース StoreController クラスを更新します。</span><span class="sxs-lookup"><span data-stu-id="94cee-313">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will retrieve it from the database.</span></span>

> [!NOTE]
> <span data-ttu-id="94cee-314">このタスクは、演習 1 と共通です。</span><span class="sxs-lookup"><span data-stu-id="94cee-314">This task is in common with Exercise 1.</span></span>
> 
> <span data-ttu-id="94cee-315">演習 1 を完了する場合は、次の手順は、両方の方法で同じを記録しては (データベースの最初の Code first または)。</span><span class="sxs-lookup"><span data-stu-id="94cee-315">If you completed Exercise 1 you will note these steps are the same in both approaches (Database first or Code first).</span></span> <span data-ttu-id="94cee-316">これらは同じで、モデルでは、データをリンクする方法が、データ エンティティへのアクセスは、コント ローラーから透過的ながまだします。</span><span class="sxs-lookup"><span data-stu-id="94cee-316">They are different in how the data is linked with the model, but the access to data entities is yet transparent from the controller.</span></span>


1. <span data-ttu-id="94cee-317">開いている**Controllers\StoreController.cs**のインスタンスを保持するクラスに次のフィールドを追加し、 **MusicStoreEntities**という名前のクラス**storeDB**:</span><span class="sxs-lookup"><span data-stu-id="94cee-317">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="94cee-318">(コード スニペット -*モデルとデータへのアクセス - Ex1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="94cee-318">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
2. <span data-ttu-id="94cee-319">**MusicStoreEntities**クラスは、データベース内の各テーブルのコレクション プロパティを公開します。</span><span class="sxs-lookup"><span data-stu-id="94cee-319">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="94cee-320">Update**参照**のすべてのジャンルを取得するアクション メソッド、**アルバム**します。</span><span class="sxs-lookup"><span data-stu-id="94cee-320">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="94cee-321">(コード スニペット -*モデルとデータ アクセス - Ex2 ストア参照*)</span><span class="sxs-lookup"><span data-stu-id="94cee-321">(Code Snippet - *Models And Data Access - Ex2 Store Browse*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="94cee-322">.NET と呼ばれる機能を使用している**LINQ** (統合言語クエリ) に対しては、データベースに対してコードを実行し、返すこのコレクションは、厳密に型指定されたクエリ式を記述するオブジェクトをプログラミングできますに対して。</span><span class="sxs-lookup"><span data-stu-id="94cee-322">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
    > 
    > <span data-ttu-id="94cee-323">LINQ の詳細についてを参照してください、 [msdn サイト](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="94cee-323">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).</span></span>
3. <span data-ttu-id="94cee-324">Update**インデックス**アクション メソッドは、すべてのジャンルを取得します。</span><span class="sxs-lookup"><span data-stu-id="94cee-324">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="94cee-325">(コード スニペット -*モデルとデータ アクセス - Ex2 ストア インデックス*)</span><span class="sxs-lookup"><span data-stu-id="94cee-325">(Code Snippet - *Models And Data Access - Ex2 Store Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
4. <span data-ttu-id="94cee-326">Update**インデックス**アクション メソッドをすべてのジャンルを取得し、一覧のコレクションを変換します。</span><span class="sxs-lookup"><span data-stu-id="94cee-326">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="94cee-327">(コード スニペット -*モデルとデータ アクセス - Ex2 ストア GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="94cee-327">(Code Snippet - *Models And Data Access - Ex2 Store GenreMenu*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="94cee-328">タスク 5 - アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="94cee-328">Task 5 - Running the Application</span></span>

<span data-ttu-id="94cee-329">このタスクでは、ストアのインデックス ページをハードコードされたものではなく、データベースに格納されているジャンルようになりました表示するかを確認します。</span><span class="sxs-lookup"><span data-stu-id="94cee-329">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="94cee-330">にビュー テンプレートを変更する必要はありません、 **StoreController**同じを返して**StoreIndexViewModel**と同様に、今回はデータベースからデータが取得されます。</span><span class="sxs-lookup"><span data-stu-id="94cee-330">There is no need to change the View template because the **StoreController** is returning the same **StoreIndexViewModel** as before, but this time the data will come from the database.</span></span>

1. <span data-ttu-id="94cee-331">ソリューションとキーを押してリビルド**F5**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="94cee-331">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="94cee-332">ホーム ページで、プロジェクトが開始されます。</span><span class="sxs-lookup"><span data-stu-id="94cee-332">The project starts in the Home page.</span></span> <span data-ttu-id="94cee-333">いることを確認のメニュー**ジャンル**一覧をハード コーディングをして、データは、データベースから直接取得します。</span><span class="sxs-lookup"><span data-stu-id="94cee-333">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    <span data-ttu-id="94cee-335">*データベースからジャンルを参照*</span><span class="sxs-lookup"><span data-stu-id="94cee-335">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="94cee-336">これで、ジャンルを参照し、アルバムがデータベースから設定されますを確認します。</span><span class="sxs-lookup"><span data-stu-id="94cee-336">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="94cee-337">![データベースからアルバムを閲覧](aspnet-mvc-4-models-and-data-access/_static/image23.png "データベースからアルバムを参照")</span><span class="sxs-lookup"><span data-stu-id="94cee-337">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image23.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="94cee-338">*データベースからアルバムを参照*</span><span class="sxs-lookup"><span data-stu-id="94cee-338">*Browsing Albums from the database*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a><span data-ttu-id="94cee-339">手順 3: パラメーターを持つデータベースの照会</span><span class="sxs-lookup"><span data-stu-id="94cee-339">Exercise 3: Querying the Database with Parameters</span></span>

<span data-ttu-id="94cee-340">この演習では、パラメーターを使用してデータベースを照会する方法とクエリ結果の整形を使用する方法について説明しますが、データベース数を削減する機能がより効率的な方法でデータの取得にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="94cee-340">In this exercise, you will learn how to query the database using parameters, and how to use Query Result Shaping, a feature that reduces the number database accesses retrieving data in a more efficient way.</span></span>

> [!NOTE]
> <span data-ttu-id="94cee-341">クエリ結果の形成について詳しくは、次を参照してください。 [msdn の記事](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="94cee-341">For further information on Query Result Shaping, visit the following [msdn article](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a><span data-ttu-id="94cee-342">タスク 1 - 変更 StoreController アルバムをデータベースから取得するには</span><span class="sxs-lookup"><span data-stu-id="94cee-342">Task 1 - Modifying StoreController to Retrieve Albums from Database</span></span>

<span data-ttu-id="94cee-343">このタスクでは、変更、 **StoreController**特定のジャンルからのアルバムを取得するデータベースにアクセスするクラス。</span><span class="sxs-lookup"><span data-stu-id="94cee-343">In this task, you will change the **StoreController** class to access the database to retrieve albums from a specific genre.</span></span>

1. <span data-ttu-id="94cee-344">開く、**開始**ソリューションがある、 **Source\Ex3 QueryingTheDatabaseWithParametersCodeFirst\Begin** Code First アプローチを使用する場合はフォルダーまたは**Source\Ex3 QueryingTheDatabaseWithParametersDBFirst\Begin**データベース優先のアプローチを使用する場合はフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="94cee-344">Open the **Begin** solution located at the **Source\Ex3-QueryingTheDatabaseWithParametersCodeFirst\Begin** folder if you want to use Code-First approach or **Source\Ex3-QueryingTheDatabaseWithParametersDBFirst\Begin** folder if you want to use Database-First approach.</span></span> <span data-ttu-id="94cee-345">使用を続ける可能性がありますそれ以外の場合、**エンド**ソリューションは、前の演習を完了して取得します。</span><span class="sxs-lookup"><span data-stu-id="94cee-345">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="94cee-346">指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。</span><span class="sxs-lookup"><span data-stu-id="94cee-346">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="94cee-347">これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。</span><span class="sxs-lookup"><span data-stu-id="94cee-347">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="94cee-348">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。</span><span class="sxs-lookup"><span data-stu-id="94cee-348">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="94cee-349">最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。</span><span class="sxs-lookup"><span data-stu-id="94cee-349">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="94cee-350">NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="94cee-350">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="94cee-351">With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="94cee-351">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="94cee-352">これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="94cee-352">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="94cee-353">開く、 **StoreController**クラスを変更する、**参照**アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="94cee-353">Open the **StoreController** class to change the **Browse** action method.</span></span> <span data-ttu-id="94cee-354">この場合に、**ソリューション エクスプ ローラー**、展開、**コント ローラー**フォルダーをダブルクリックします**StoreController.cs**。</span><span class="sxs-lookup"><span data-stu-id="94cee-354">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
3. <span data-ttu-id="94cee-355">変更、**参照**アクション メソッドを特定のジャンルのアルバムを取得します。</span><span class="sxs-lookup"><span data-stu-id="94cee-355">Change the **Browse** action method to retrieve albums for a specific genre.</span></span> <span data-ttu-id="94cee-356">これを行うには、次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="94cee-356">To do this, replace the following code:</span></span>

    <span data-ttu-id="94cee-357">(コード スニペット -*モデルとデータ アクセス - Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="94cee-357">(Code Snippet - *Models And Data Access - Ex3 StoreController BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> <span data-ttu-id="94cee-358">エンティティのコレクションを設定するには使用する必要があります、 **Include**すぎる、アルバムを取得するを指定します。</span><span class="sxs-lookup"><span data-stu-id="94cee-358">To populate a collection of the entity, you need to use the **Include** method to specify you want to retrieve the albums too.</span></span> <span data-ttu-id="94cee-359">使用することができます、します。**Single()** LINQ の拡張機能、アルバムの 1 つだけのジャンルがここで予想されるためです。</span><span class="sxs-lookup"><span data-stu-id="94cee-359">You can use the .**Single()** extension in LINQ because in this case only one genre is expected for an album.</span></span> <span data-ttu-id="94cee-360">**Single()** メソッドは、ここでその名前が定義されている値と一致するように 1 つのジャンル オブジェクトを指定するパラメーターとしてラムダ式を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="94cee-360">The **Single()** method takes a Lambda expression as a parameter, which in this case specifies a single Genre object such that its name matches the value defined.</span></span>
> 
> <span data-ttu-id="94cee-361">ジャンル オブジェクトを取得するときにも読み込まれたたいその他の関連エンティティを示すために使用できる機能の利点になります。</span><span class="sxs-lookup"><span data-stu-id="94cee-361">You will take advantage of a feature that allows you to indicate other related entities you want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="94cee-362">この機能は呼**クエリ結果の整形**情報を取得するデータベースにアクセスするために必要な回数を削減することができます。</span><span class="sxs-lookup"><span data-stu-id="94cee-362">This feature is called **Query Result Shaping**, and enables you to reduce the number of times needed to access the database to retrieve information.</span></span> <span data-ttu-id="94cee-363">このシナリオでは、アルバム、ジャンルを取得するをプリフェッチするでしょう。</span><span class="sxs-lookup"><span data-stu-id="94cee-363">In this scenario, you will want to pre-fetch the Albums for the Genre you retrieve.</span></span>
> 
> <span data-ttu-id="94cee-364">クエリが含まれる**Genres.Include (&quot;アルバム&quot;)** 関連アルバムもすることを指定します。</span><span class="sxs-lookup"><span data-stu-id="94cee-364">The query includes **Genres.Include(&quot;Albums&quot;)** to indicate that you want related albums as well.</span></span> <span data-ttu-id="94cee-365">これが 1 つのデータベース要求のジャンルとアルバムの両方のデータを取得するためより効率的なアプリケーションで発生します。</span><span class="sxs-lookup"><span data-stu-id="94cee-365">This will result in a more efficient application, since it will retrieve both Genre and Album data in a single database request.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="94cee-366">タスク 2 - アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="94cee-366">Task 2 - Running the Application</span></span>

<span data-ttu-id="94cee-367">このタスクでは、アプリケーションを実行し、特定のジャンルのアルバムをデータベースから取得されます。</span><span class="sxs-lookup"><span data-stu-id="94cee-367">In this task, you will run the application and retrieve albums of a specific genre from the database.</span></span>

1. <span data-ttu-id="94cee-368">キーを押して**F5**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="94cee-368">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="94cee-369">ホーム ページで、プロジェクトが開始されます。</span><span class="sxs-lookup"><span data-stu-id="94cee-369">The project starts in the Home page.</span></span> <span data-ttu-id="94cee-370">URL を変更して **/ストア/参照? ジャンル = Pop**結果をデータベースから取得することを確認します。</span><span class="sxs-lookup"><span data-stu-id="94cee-370">Change the URL to **/Store/Browse?genre=Pop** to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="94cee-371">![ジャンルによる参照](aspnet-mvc-4-models-and-data-access/_static/image24.png "ジャンルによる参照")</span><span class="sxs-lookup"><span data-stu-id="94cee-371">![Browsing by genre](aspnet-mvc-4-models-and-data-access/_static/image24.png "Browsing by genre")</span></span>

    <span data-ttu-id="94cee-372">*参照/ストア/参照? ジャンル Pop を =*</span><span class="sxs-lookup"><span data-stu-id="94cee-372">*Browsing /Store/Browse?genre=Pop*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a><span data-ttu-id="94cee-373">タスク 3 - Id でのアルバムへのアクセス</span><span class="sxs-lookup"><span data-stu-id="94cee-373">Task 3 - Accessing Albums by Id</span></span>

<span data-ttu-id="94cee-374">このタスクでは、アルバムの id を取得するには、前の手順を繰り返します</span><span class="sxs-lookup"><span data-stu-id="94cee-374">In this task, you will repeat the previous procedure to get albums by their Id.</span></span>

1. <span data-ttu-id="94cee-375">Visual Studio に戻りますが、必要な場合は、ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="94cee-375">Close the browser if needed, to return to Visual Studio.</span></span> <span data-ttu-id="94cee-376">開く、 **StoreController**クラスを変更する、**詳細**アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="94cee-376">Open the **StoreController** class to change the **Details** action method.</span></span> <span data-ttu-id="94cee-377">この場合に、**ソリューション エクスプ ローラー**、展開、**コント ローラー**フォルダーをダブルクリックします**StoreController.cs**。</span><span class="sxs-lookup"><span data-stu-id="94cee-377">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
2. <span data-ttu-id="94cee-378">変更、**詳細**アルバムの詳細を取得するアクション メソッドがに基づいて、 **Id**します。これを行うには、次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="94cee-378">Change the **Details** action method to retrieve albums details based on their **Id**. To do this, replace the following code:</span></span>

    <span data-ttu-id="94cee-379">(コード スニペット -*モデルとデータ アクセス - Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="94cee-379">(Code Snippet - *Models And Data Access - Ex3 StoreController DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="94cee-380">タスク 4 - アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="94cee-380">Task 4 - Running the Application</span></span>

<span data-ttu-id="94cee-381">このタスクでは、web ブラウザーでアプリケーションを実行し、その id でアルバムの詳細を取得</span><span class="sxs-lookup"><span data-stu-id="94cee-381">In this task, you will run the Application in a web browser and obtain album details by their Id.</span></span>

1. <span data-ttu-id="94cee-382">キーを押して**F5**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="94cee-382">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="94cee-383">ホーム ページで、プロジェクトが開始されます。</span><span class="sxs-lookup"><span data-stu-id="94cee-383">The project starts in the Home page.</span></span> <span data-ttu-id="94cee-384">URL を変更して **/Store/Details/51**ジャンルを参照して、結果をデータベースから取得することを確認するアルバムを選択します。</span><span class="sxs-lookup"><span data-stu-id="94cee-384">Change the URL to **/Store/Details/51** or browse the genres and select an album to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="94cee-385">![詳細を閲覧](aspnet-mvc-4-models-and-data-access/_static/image25.png "の詳細を参照")</span><span class="sxs-lookup"><span data-stu-id="94cee-385">![Browsing Details](aspnet-mvc-4-models-and-data-access/_static/image25.png "Browsing Details")</span></span>

    <span data-ttu-id="94cee-386">*閲覧/Store/Details/51*</span><span class="sxs-lookup"><span data-stu-id="94cee-386">*Browsing /Store/Details/51*</span></span>

> [!NOTE]
> <span data-ttu-id="94cee-387">また、Windows Azure Web サイトの次に、このアプリケーションを展開できます[付録 b: 発行 Web Deploy を使用して ASP.NET MVC 4 アプリケーション](#AppendixB)します。</span><span class="sxs-lookup"><span data-stu-id="94cee-387">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="94cee-388">まとめ</span><span class="sxs-lookup"><span data-stu-id="94cee-388">Summary</span></span>

<span data-ttu-id="94cee-389">ASP.NET MVC のモデルとデータ アクセスの基礎を学習するこのハンズオン ラボを使用して、によって、 **Database First**方法だけでなく**Code First**アプローチ。</span><span class="sxs-lookup"><span data-stu-id="94cee-389">By completing this Hands-on Lab you have learned the fundamentals of ASP.NET MVC Models and Data Access, using the **Database First** approach as well as the **Code First** Approach:</span></span>

- <span data-ttu-id="94cee-390">そのデータを使用するために、ソリューションにデータベースを追加する方法</span><span class="sxs-lookup"><span data-stu-id="94cee-390">How to add a database to the solution in order to consume its data</span></span>
- <span data-ttu-id="94cee-391">1 つのハード コーディングされたのではなく、データベースから取得されたデータにビュー テンプレートを提供するコント ローラーを更新する方法</span><span class="sxs-lookup"><span data-stu-id="94cee-391">How to update Controllers to provide View templates with the data taken from the database instead of hard-coded one</span></span>
- <span data-ttu-id="94cee-392">パラメーターを使用してデータベースを照会する方法</span><span class="sxs-lookup"><span data-stu-id="94cee-392">How to query the database using parameters</span></span>
- <span data-ttu-id="94cee-393">クエリ結果の整形、データベースへのアクセスの数を削減する機能をより効率的な方法でデータの取得を使用する方法</span><span class="sxs-lookup"><span data-stu-id="94cee-393">How to use the Query Result Shaping, a feature that reduces the number of database accesses, retrieving data in a more efficient way</span></span>
- <span data-ttu-id="94cee-394">Microsoft Entity Framework の Code First と Database First の両方の方法を使用して、モデルとデータベースをリンクする方法</span><span class="sxs-lookup"><span data-stu-id="94cee-394">How to use both Database First and Code First approaches in Microsoft Entity Framework to link the database with the model</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="94cee-395">付録 a: Installing Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="94cee-395">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="94cee-396">インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、 **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="94cee-396">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="94cee-397">次の手順をインストールするために必要な手順をガイドします。 *Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*します。</span><span class="sxs-lookup"><span data-stu-id="94cee-397">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="94cee-398">移動して[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)します。</span><span class="sxs-lookup"><span data-stu-id="94cee-398">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="94cee-399">または、既に Web Platform Installer をインストールした場合を開くことも、製品を検索して、 &quot; <em>Visual Studio Express 2012 for Web と Windows Azure SDK</em>&quot;します。</span><span class="sxs-lookup"><span data-stu-id="94cee-399">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="94cee-400">をクリックして**を今すぐインストール**します。</span><span class="sxs-lookup"><span data-stu-id="94cee-400">Click on **Install Now**.</span></span> <span data-ttu-id="94cee-401">ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="94cee-401">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="94cee-402">1 回**Web Platform Installer**を開くと、クリックして**インストール**セットアップを開始します。</span><span class="sxs-lookup"><span data-stu-id="94cee-402">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="94cee-403">![Visual Studio Express のインストール](aspnet-mvc-4-models-and-data-access/_static/image26.png "Visual Studio Express のインストール")</span><span class="sxs-lookup"><span data-stu-id="94cee-403">![Install Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="94cee-404">*Visual Studio Express をインストールします。*</span><span class="sxs-lookup"><span data-stu-id="94cee-404">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="94cee-405">すべての製品のライセンスと使用条件を読み、クリックして**同意**を続行します。</span><span class="sxs-lookup"><span data-stu-id="94cee-405">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![ライセンス条項に同意](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    <span data-ttu-id="94cee-407">*ライセンス条項に同意*</span><span class="sxs-lookup"><span data-stu-id="94cee-407">*Accepting the license terms*</span></span>
5. <span data-ttu-id="94cee-408">ダウンロードとインストール プロセスが完了するまで待機します。</span><span class="sxs-lookup"><span data-stu-id="94cee-408">Wait until the downloading and installation process completes.</span></span>

    ![インストールの進行状況](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    <span data-ttu-id="94cee-410">*インストールの進行状況*</span><span class="sxs-lookup"><span data-stu-id="94cee-410">*Installation progress*</span></span>
6. <span data-ttu-id="94cee-411">インストールが完了したら、クリックして**完了**します。</span><span class="sxs-lookup"><span data-stu-id="94cee-411">When the installation completes, click **Finish**.</span></span>

    ![インストールが完了しました](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    <span data-ttu-id="94cee-413">*インストールが完了しました*</span><span class="sxs-lookup"><span data-stu-id="94cee-413">*Installation completed*</span></span>
7. <span data-ttu-id="94cee-414">クリックして**終了**Web Platform Installer を閉じます。</span><span class="sxs-lookup"><span data-stu-id="94cee-414">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="94cee-415">Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、 をクリックし、 **VS Express for Web**並べて表示します。</span><span class="sxs-lookup"><span data-stu-id="94cee-415">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web のタイル](aspnet-mvc-4-models-and-data-access/_static/image30.png)

    <span data-ttu-id="94cee-417">*VS Express for Web のタイル*</span><span class="sxs-lookup"><span data-stu-id="94cee-417">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="94cee-418">付録 b: Web Deploy を使用して ASP.NET MVC 4 アプリケーションの発行</span><span class="sxs-lookup"><span data-stu-id="94cee-418">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="94cee-419">この付録では、Windows Azure 管理ポータルから新しい web サイトを作成して Windows Azure によって提供される、Web 配置発行機能を活用して、次の演習では、取得したアプリケーションを発行する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="94cee-419">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="94cee-420">タスク 1 - 新しい Web サイトを作成する、Windows から Azure Portal</span><span class="sxs-lookup"><span data-stu-id="94cee-420">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="94cee-421">移動して、 [Windows Azure 管理ポータル](https://manage.windowsazure.com/)サブスクリプションに関連付けられている Microsoft の資格情報を使用してサインインします。</span><span class="sxs-lookup"><span data-stu-id="94cee-421">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="94cee-422">Windows Azure を無料で 10 個の ASP.NET Web サイトをホストでき、トラフィックの増加に応じてスケールできます。</span><span class="sxs-lookup"><span data-stu-id="94cee-422">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="94cee-423">サインアップする[ここ](http://aka.ms/aspnet-hol-azure)します。</span><span class="sxs-lookup"><span data-stu-id="94cee-423">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="94cee-424">![Windows Azure ポータルにログオン](aspnet-mvc-4-models-and-data-access/_static/image31.png "Windows Azure ポータルにログオン")</span><span class="sxs-lookup"><span data-stu-id="94cee-424">![Log on to Windows Azure portal](aspnet-mvc-4-models-and-data-access/_static/image31.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="94cee-425">*Windows Azure 管理ポータルにログオン*</span><span class="sxs-lookup"><span data-stu-id="94cee-425">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="94cee-426">クリックして**新規**コマンド バーにします。</span><span class="sxs-lookup"><span data-stu-id="94cee-426">Click **New** on the command bar.</span></span>

    <span data-ttu-id="94cee-427">![新しい Web サイトを作成する](aspnet-mvc-4-models-and-data-access/_static/image32.png "新しい Web サイトを作成します。")</span><span class="sxs-lookup"><span data-stu-id="94cee-427">![Creating a new Web Site](aspnet-mvc-4-models-and-data-access/_static/image32.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="94cee-428">*新しい Web サイトを作成します。*</span><span class="sxs-lookup"><span data-stu-id="94cee-428">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="94cee-429">クリックして**コンピューティング** | **Web サイト**します。</span><span class="sxs-lookup"><span data-stu-id="94cee-429">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="94cee-430">選び**簡易作成**オプション。</span><span class="sxs-lookup"><span data-stu-id="94cee-430">Then select **Quick Create** option.</span></span> <span data-ttu-id="94cee-431">新しい web サイトの URL を指定し、をクリックして**Web サイトの作成**です。</span><span class="sxs-lookup"><span data-stu-id="94cee-431">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="94cee-432">Windows Azure の Web サイトには、制御および管理できる、クラウドで実行されている web アプリケーション、ホストです。</span><span class="sxs-lookup"><span data-stu-id="94cee-432">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="94cee-433">簡易作成 オプションを使用すると、完成した web アプリケーションを Windows Azure の Web サイトから、ポータル外からデプロイできます。</span><span class="sxs-lookup"><span data-stu-id="94cee-433">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="94cee-434">データベースを設定するための手順は含まれません。</span><span class="sxs-lookup"><span data-stu-id="94cee-434">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="94cee-435">![簡易作成を使用して新しい Web サイトを作成する](aspnet-mvc-4-models-and-data-access/_static/image33.png "簡易作成を使用して新しい Web サイトを作成します。")</span><span class="sxs-lookup"><span data-stu-id="94cee-435">![Creating a new Web Site using Quick Create](aspnet-mvc-4-models-and-data-access/_static/image33.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="94cee-436">*簡易作成を使用して新しい Web サイトを作成します。*</span><span class="sxs-lookup"><span data-stu-id="94cee-436">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="94cee-437">新しいまで待つ**Web サイト**が作成されます。</span><span class="sxs-lookup"><span data-stu-id="94cee-437">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="94cee-438">Web サイトを作成した後は、下のリンクをクリックして、 **URL**列。</span><span class="sxs-lookup"><span data-stu-id="94cee-438">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="94cee-439">新しい Web サイトが動作していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="94cee-439">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="94cee-440">![新しい web サイトを参照](aspnet-mvc-4-models-and-data-access/_static/image34.png "新しい web サイトを参照")</span><span class="sxs-lookup"><span data-stu-id="94cee-440">![Browsing to the new web site](aspnet-mvc-4-models-and-data-access/_static/image34.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="94cee-441">*新しい web サイトを参照*</span><span class="sxs-lookup"><span data-stu-id="94cee-441">*Browsing to the new web site*</span></span>

    <span data-ttu-id="94cee-442">![Web サイトが実行されている](aspnet-mvc-4-models-and-data-access/_static/image35.png "実行している Web サイト")</span><span class="sxs-lookup"><span data-stu-id="94cee-442">![Web site running](aspnet-mvc-4-models-and-data-access/_static/image35.png "Web site running")</span></span>

    <span data-ttu-id="94cee-443">*実行している web サイト*</span><span class="sxs-lookup"><span data-stu-id="94cee-443">*Web site running*</span></span>
6. <span data-ttu-id="94cee-444">ポータルに戻り、下にある web サイトの名前をクリックして、**名前**管理ページを表示する列。</span><span class="sxs-lookup"><span data-stu-id="94cee-444">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="94cee-445">![Web サイトの管理ページを開く](aspnet-mvc-4-models-and-data-access/_static/image36.png "web サイトの管理ページを開く")</span><span class="sxs-lookup"><span data-stu-id="94cee-445">![Opening the web site management pages](aspnet-mvc-4-models-and-data-access/_static/image36.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="94cee-446">*Web サイトの管理ページを開く*</span><span class="sxs-lookup"><span data-stu-id="94cee-446">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="94cee-447">**ダッシュ ボード**ページの**概要**セクションで、、**発行プロファイルのダウンロード**リンク。</span><span class="sxs-lookup"><span data-stu-id="94cee-447">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="94cee-448">*発行プロファイル*すべてのパブリケーションを有効になっているメソッドごとに Windows Azure web サイトに web アプリケーションを発行するために必要な情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="94cee-448">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="94cee-449">発行プロファイルには、Url、ユーザーの資格情報およびに接続し、各発行メソッドが有効になっているエンドポイントに対して認証するために必要なデータベースの文字列が含まれています。</span><span class="sxs-lookup"><span data-stu-id="94cee-449">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="94cee-450">**Microsoft WebMatrix 2**、 **Microsoft Visual Studio Express for Web**と**Microsoft Visual Studio 2012**読み取りのサポートと、発行プロファイルのこれらのプログラムの構成を自動化するにはWindows Azure の web サイトへの web アプリケーションを発行しています。</span><span class="sxs-lookup"><span data-stu-id="94cee-450">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="94cee-451">![発行プロファイルのダウンロード web サイト](aspnet-mvc-4-models-and-data-access/_static/image37.png "発行プロファイルの web サイトのダウンロード")</span><span class="sxs-lookup"><span data-stu-id="94cee-451">![Downloading the web site publish profile](aspnet-mvc-4-models-and-data-access/_static/image37.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="94cee-452">*発行プロファイルの Web サイトのダウンロード*</span><span class="sxs-lookup"><span data-stu-id="94cee-452">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="94cee-453">既知の場所に発行プロファイル ファイルをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="94cee-453">Download the publish profile file to a known location.</span></span> <span data-ttu-id="94cee-454">さらにこの演習ではこのファイルを使用して、Visual Studio から Windows Azure Web サイトに web アプリケーションを発行する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="94cee-454">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="94cee-455">![発行プロファイル ファイルを保存する](aspnet-mvc-4-models-and-data-access/_static/image38.png "発行プロファイルを保存しています")</span><span class="sxs-lookup"><span data-stu-id="94cee-455">![Saving the publish profile file](aspnet-mvc-4-models-and-data-access/_static/image38.png "Saving the publish profile")</span></span>

    <span data-ttu-id="94cee-456">*発行プロファイル ファイルを保存しています*</span><span class="sxs-lookup"><span data-stu-id="94cee-456">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="94cee-457">タスク 2 - データベース サーバーの構成</span><span class="sxs-lookup"><span data-stu-id="94cee-457">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="94cee-458">アプリケーションが SQL Server を使用する場合のデータベースは SQL Database サーバーを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="94cee-458">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="94cee-459">SQL Server を使用しない単純なアプリケーションをデプロイする場合は、このタスクをスキップする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="94cee-459">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="94cee-460">SQL Database サーバーは、アプリケーション データベースを格納する必要があります。</span><span class="sxs-lookup"><span data-stu-id="94cee-460">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="94cee-461">Windows Azure 管理ポータル内のサブスクリプションから SQL Database サーバーを表示する**Sql データベース** | **サーバー** | **サーバーのダッシュ ボード**します。</span><span class="sxs-lookup"><span data-stu-id="94cee-461">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="94cee-462">使用して 1 つを作成するには作成したサーバーがない、**追加**コマンド バーのボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="94cee-462">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="94cee-463">メモ、**サーバー名および URL、管理者のログイン名とパスワード**次のタスクで使用すると、します。</span><span class="sxs-lookup"><span data-stu-id="94cee-463">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="94cee-464">データベースを作成しない、まだ、後の段階でそれが作成されます。</span><span class="sxs-lookup"><span data-stu-id="94cee-464">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="94cee-465">![SQL データベース サーバーのダッシュ ボード](aspnet-mvc-4-models-and-data-access/_static/image39.png "SQL データベース サーバーのダッシュ ボード")</span><span class="sxs-lookup"><span data-stu-id="94cee-465">![SQL Database Server Dashboard](aspnet-mvc-4-models-and-data-access/_static/image39.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="94cee-466">*SQL データベース サーバーのダッシュ ボード*</span><span class="sxs-lookup"><span data-stu-id="94cee-466">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="94cee-467">次のタスクでは、サーバーの一覧で、ローカル IP アドレスを含める必要があるため、Visual Studio からデータベース接続をテストします**使用できる IP アドレス**します。</span><span class="sxs-lookup"><span data-stu-id="94cee-467">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="94cee-468">次のようにクリックします**構成**、から IP アドレスを選択して**現在のクライアント IP アドレス**で貼り付けます、**開始 IP アドレス**と**終了 IP アドレス。** テキスト ボックスをクリックして、 ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png)ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="94cee-468">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) button.</span></span>

    ![クライアント IP アドレスを追加します。](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    <span data-ttu-id="94cee-470">*クライアント IP アドレスを追加します。*</span><span class="sxs-lookup"><span data-stu-id="94cee-470">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="94cee-471">1 回、**クライアント IP アドレス**が許可された IP アドレスに追加一覧で、をクリックして**保存**変更を確認します。</span><span class="sxs-lookup"><span data-stu-id="94cee-471">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![変更を確認します。](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    <span data-ttu-id="94cee-473">*変更を確認します。*</span><span class="sxs-lookup"><span data-stu-id="94cee-473">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="94cee-474">タスク 3 - Web Deploy を使用して ASP.NET MVC 4 アプリケーションの発行</span><span class="sxs-lookup"><span data-stu-id="94cee-474">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="94cee-475">ASP.NET MVC 4 のソリューションに戻ります。</span><span class="sxs-lookup"><span data-stu-id="94cee-475">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="94cee-476">**ソリューション エクスプ ローラー**を web サイト プロジェクトを右クリックし、**発行**します。</span><span class="sxs-lookup"><span data-stu-id="94cee-476">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="94cee-477">![アプリケーションの発行](aspnet-mvc-4-models-and-data-access/_static/image43.png "アプリケーションの発行")</span><span class="sxs-lookup"><span data-stu-id="94cee-477">![Publishing the Application](aspnet-mvc-4-models-and-data-access/_static/image43.png "Publishing the Application")</span></span>

    <span data-ttu-id="94cee-478">*Web サイトの発行*</span><span class="sxs-lookup"><span data-stu-id="94cee-478">*Publishing the web site*</span></span>
2. <span data-ttu-id="94cee-479">最初のタスクで保存した発行プロファイルをインポートします。</span><span class="sxs-lookup"><span data-stu-id="94cee-479">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="94cee-480">![発行プロファイルをインポートする](aspnet-mvc-4-models-and-data-access/_static/image44.png "発行プロファイルをインポートします。")</span><span class="sxs-lookup"><span data-stu-id="94cee-480">![Importing the publish profile](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importing the publish profile")</span></span>

    <span data-ttu-id="94cee-481">*発行プロファイルのインポート*</span><span class="sxs-lookup"><span data-stu-id="94cee-481">*Importing publish profile*</span></span>
3. <span data-ttu-id="94cee-482">クリックして**接続の検証**です。</span><span class="sxs-lookup"><span data-stu-id="94cee-482">Click **Validate Connection**.</span></span> <span data-ttu-id="94cee-483">検証が完了したら、クリックして**次**します。</span><span class="sxs-lookup"><span data-stu-id="94cee-483">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="94cee-484">接続の検証ボタンの横に表示される緑のチェック マークが表示されたら、検証が完了しました。</span><span class="sxs-lookup"><span data-stu-id="94cee-484">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="94cee-485">![接続の検証](aspnet-mvc-4-models-and-data-access/_static/image45.png "接続の検証")</span><span class="sxs-lookup"><span data-stu-id="94cee-485">![Validating connection](aspnet-mvc-4-models-and-data-access/_static/image45.png "Validating connection")</span></span>

    <span data-ttu-id="94cee-486">*接続の検証*</span><span class="sxs-lookup"><span data-stu-id="94cee-486">*Validating connection*</span></span>
4. <span data-ttu-id="94cee-487">**設定**ページの**データベース**セクションで、データベース接続のテキスト ボックスの横にあるボタンをクリックします (つまり**DefaultConnection**)。</span><span class="sxs-lookup"><span data-stu-id="94cee-487">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="94cee-488">![Web 配置の構成](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web 配置の構成")</span><span class="sxs-lookup"><span data-stu-id="94cee-488">![Web deploy configuration](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web deploy configuration")</span></span>

    <span data-ttu-id="94cee-489">*Web 配置の構成*</span><span class="sxs-lookup"><span data-stu-id="94cee-489">*Web deploy configuration*</span></span>
5. <span data-ttu-id="94cee-490">データベース接続を次のように構成します。</span><span class="sxs-lookup"><span data-stu-id="94cee-490">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="94cee-491">**サーバー名**SQL Database サーバーの URL を使用して、入力、 *tcp:* プレフィックス。</span><span class="sxs-lookup"><span data-stu-id="94cee-491">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="94cee-492">**ユーザー名**サーバー管理者ログイン名を入力します。</span><span class="sxs-lookup"><span data-stu-id="94cee-492">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="94cee-493">**パスワード**サーバー管理者ログイン パスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="94cee-493">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="94cee-494">新しいデータベース名を入力します。</span><span class="sxs-lookup"><span data-stu-id="94cee-494">Type a new database name.</span></span>

     <span data-ttu-id="94cee-495">![ターゲットの接続文字列を構成する](aspnet-mvc-4-models-and-data-access/_static/image47.png "ターゲットの接続文字列を構成します。")</span><span class="sxs-lookup"><span data-stu-id="94cee-495">![Configuring destination connection string](aspnet-mvc-4-models-and-data-access/_static/image47.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="94cee-496">*ターゲットの接続文字列を構成します。*</span><span class="sxs-lookup"><span data-stu-id="94cee-496">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="94cee-497">次に、 **[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="94cee-497">Then click **OK**.</span></span> <span data-ttu-id="94cee-498">データベースのクリックを作成するように求められたら**はい**します。</span><span class="sxs-lookup"><span data-stu-id="94cee-498">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="94cee-499">![データベースを作成する](aspnet-mvc-4-models-and-data-access/_static/image48.png "データベース文字列を作成します。")</span><span class="sxs-lookup"><span data-stu-id="94cee-499">![Creating the database](aspnet-mvc-4-models-and-data-access/_static/image48.png "Creating the database string")</span></span>

    <span data-ttu-id="94cee-500">*データベースの作成*</span><span class="sxs-lookup"><span data-stu-id="94cee-500">*Creating the database*</span></span>
7. <span data-ttu-id="94cee-501">Windows azure SQL Database への接続に使用する接続文字列は、接続の既定のテキスト ボックス内に表示されます。</span><span class="sxs-lookup"><span data-stu-id="94cee-501">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="94cee-502">その後、 **[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="94cee-502">Then click **Next**.</span></span>

    <span data-ttu-id="94cee-503">![SQL データベースを指す接続文字列](aspnet-mvc-4-models-and-data-access/_static/image49.png "SQL データベースを指す接続文字列")</span><span class="sxs-lookup"><span data-stu-id="94cee-503">![Connection string pointing to SQL Database](aspnet-mvc-4-models-and-data-access/_static/image49.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="94cee-504">*SQL データベースを指す接続文字列*</span><span class="sxs-lookup"><span data-stu-id="94cee-504">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="94cee-505">**プレビュー** ] ページで [**発行**します。</span><span class="sxs-lookup"><span data-stu-id="94cee-505">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="94cee-506">![Web アプリケーションの発行](aspnet-mvc-4-models-and-data-access/_static/image50.png "web アプリケーションの発行")</span><span class="sxs-lookup"><span data-stu-id="94cee-506">![Publishing the web application](aspnet-mvc-4-models-and-data-access/_static/image50.png "Publishing the web application")</span></span>

    <span data-ttu-id="94cee-507">*Web アプリケーションの発行*</span><span class="sxs-lookup"><span data-stu-id="94cee-507">*Publishing the web application*</span></span>
9. <span data-ttu-id="94cee-508">発行プロセスが完了すると、既定のブラウザーが発行した web サイトを開きます。</span><span class="sxs-lookup"><span data-stu-id="94cee-508">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="94cee-509">付録 c: コード スニペットの使用</span><span class="sxs-lookup"><span data-stu-id="94cee-509">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="94cee-510">コードのスニペットでは、指先ひとつで必要なすべてのコードがあります。</span><span class="sxs-lookup"><span data-stu-id="94cee-510">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="94cee-511">ラボ ドキュメントがわかりますだけをいつ使用できる、次の図に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="94cee-511">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="94cee-512">![Visual Studio コード スニペットを使用して、プロジェクトにコードを挿入する](aspnet-mvc-4-models-and-data-access/_static/image51.png "プロジェクトにコードを挿入するコード スニペットを Visual Studio の使用")</span><span class="sxs-lookup"><span data-stu-id="94cee-512">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-models-and-data-access/_static/image51.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="94cee-513">*Visual Studio コード スニペットを使用して、プロジェクトにコードを挿入するには*</span><span class="sxs-lookup"><span data-stu-id="94cee-513">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="94cee-514">***キーボード (c# のみ) を使用するコード スニペットを追加するには***</span><span class="sxs-lookup"><span data-stu-id="94cee-514">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="94cee-515">コードを挿入するには、カーソルを置きます。</span><span class="sxs-lookup"><span data-stu-id="94cee-515">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="94cee-516">スニペットの名前 (なし、スペースやハイフン) の入力を開始します。</span><span class="sxs-lookup"><span data-stu-id="94cee-516">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="94cee-517">スニペットの名前に一致する IntelliSense の表示を確認します。</span><span class="sxs-lookup"><span data-stu-id="94cee-517">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="94cee-518">適切なスニペットを選択します (または全体のスニペットの名前が選択されるまで」と入力してください)。</span><span class="sxs-lookup"><span data-stu-id="94cee-518">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="94cee-519">カーソル位置にスニペットを挿入するには、2 回、Tab キーを押します。</span><span class="sxs-lookup"><span data-stu-id="94cee-519">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="94cee-520">![スニペットの名前の入力を開始](aspnet-mvc-4-models-and-data-access/_static/image52.png "スニペット名の入力を開始")</span><span class="sxs-lookup"><span data-stu-id="94cee-520">![Start typing the snippet name](aspnet-mvc-4-models-and-data-access/_static/image52.png "Start typing the snippet name")</span></span>

<span data-ttu-id="94cee-521">*スニペットの名前の入力を開始します。*</span><span class="sxs-lookup"><span data-stu-id="94cee-521">*Start typing the snippet name*</span></span>

<span data-ttu-id="94cee-522">![強調表示されているスニペットを選択して Tab キーを押して](aspnet-mvc-4-models-and-data-access/_static/image53.png "キーを押してタブが強調表示されているスニペットを選択するには")</span><span class="sxs-lookup"><span data-stu-id="94cee-522">![Press Tab to select the highlighted snippet](aspnet-mvc-4-models-and-data-access/_static/image53.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="94cee-523">*Tab キーを押して、強調表示されているスニペットを選択します*</span><span class="sxs-lookup"><span data-stu-id="94cee-523">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="94cee-524">![キーを押して タブで再度とスニペットが展開](aspnet-mvc-4-models-and-data-access/_static/image54.png "キーを押して タブで再度とスニペットが展開されます")</span><span class="sxs-lookup"><span data-stu-id="94cee-524">![Press Tab again and the snippet will expand](aspnet-mvc-4-models-and-data-access/_static/image54.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="94cee-525">*キーを押して タブで再度とスニペットが展開されます。*</span><span class="sxs-lookup"><span data-stu-id="94cee-525">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="94cee-526">***(C#、Visual Basic および XML) にマウスを使用するコード スニペットを追加する***1。</span><span class="sxs-lookup"><span data-stu-id="94cee-526">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="94cee-527">コード スニペットを挿入するを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="94cee-527">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="94cee-528">選択**スニペットの挿入**続けて**マイ コード スニペット**します。</span><span class="sxs-lookup"><span data-stu-id="94cee-528">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="94cee-529">クリックして、一覧から関連するスニペットを選択します。</span><span class="sxs-lookup"><span data-stu-id="94cee-529">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="94cee-530">![コード スニペットを挿入し、スニペットの挿入を選択する場所を右クリックして](aspnet-mvc-4-models-and-data-access/_static/image55.png "コード スニペットを挿入し、スニペットの挿入を選択する場所を右クリック")</span><span class="sxs-lookup"><span data-stu-id="94cee-530">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-models-and-data-access/_static/image55.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="94cee-531">*コード スニペットを挿入して、スニペットの挿入先の選択します。*</span><span class="sxs-lookup"><span data-stu-id="94cee-531">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="94cee-532">![クリックして、一覧から関連するスニペットを選択](aspnet-mvc-4-models-and-data-access/_static/image56.png "クリックして、一覧から関連するスニペットを選択")</span><span class="sxs-lookup"><span data-stu-id="94cee-532">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-models-and-data-access/_static/image56.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="94cee-533">*クリックして、一覧から関連するスニペットを選択します。*</span><span class="sxs-lookup"><span data-stu-id="94cee-533">*Pick the relevant snippet from the list, by clicking on it*</span></span>
