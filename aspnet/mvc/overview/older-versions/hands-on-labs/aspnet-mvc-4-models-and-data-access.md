---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: ASP.NET MVC 4 モデルおよびデータ アクセス |Microsoft ドキュメント
author: rick-anderson
description: '注: このハンズオン ラボでは、ASP.NET MVC の基本的な知識がある前提としています。 前に ASP.NET MVC を使用していない場合をお勧めすると、ASP.NET MVC 4 経由しています.'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: 081a71ef67a6eee6c84058c30f9e15301afbed23
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-mvc-4-models-and-data-access"></a><span data-ttu-id="05ccb-104">ASP.NET MVC 4 モデルおよびデータ アクセス</span><span class="sxs-lookup"><span data-stu-id="05ccb-104">ASP.NET MVC 4 Models and Data Access</span></span>

<span data-ttu-id="05ccb-105">によって[Web キャンプ チーム](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="05ccb-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="05ccb-106">Web キャンプ トレーニング キットをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="05ccb-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="05ccb-107">このハンズオン ラボは、の基本的な知識がある前提としています。 **ASP.NET MVC**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-107">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="05ccb-108">使用していない場合**ASP.NET MVC**経由で移動する前をお勧め**ASP.NET MVC 4 基礎**ハンズオン ラボ。</span><span class="sxs-lookup"><span data-stu-id="05ccb-108">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="05ccb-109">このラボには、機能強化と軽微な変更を元のフォルダーで提供されるサンプル Web アプリケーションに適用することで前に説明した新しい機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-109">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="05ccb-110">すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[Microsoft の Web/WebCampTrainingKit リリース](https://aka.ms/webcamps-training-kit)です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-110">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="05ccb-111">このラボに固有のプロジェクトは[ASP.NET MVC 4 モデルおよびデータ アクセス](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess)です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-111">The project specific to this lab is available at [ASP.NET MVC 4 Models and Data Access](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).</span></span>

<span data-ttu-id="05ccb-112">**ASP.NET MVC の基本事項**ハンズオン ラボがされて渡そうとしてハード コードされたデータのコント ローラーからテンプレートの表示にします。</span><span class="sxs-lookup"><span data-stu-id="05ccb-112">In **ASP.NET MVC Fundamentals** Hands-on Lab, you have been passing hard-coded data from the Controllers to the View templates.</span></span> <span data-ttu-id="05ccb-113">しかし、実際の Web アプリケーションを構築するために実際のデータベースを使用する場合があります。</span><span class="sxs-lookup"><span data-stu-id="05ccb-113">But, in order to build a real Web application, you might want to use a real database.</span></span>

<span data-ttu-id="05ccb-114">このハンズオン ラボでは、格納および音楽ストア アプリケーションに必要なデータを取得するためにデータベース エンジンを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-114">This Hands-on Lab will show you how to use a database engine in order to store and retrieve the data needed for the Music Store application.</span></span> <span data-ttu-id="05ccb-115">そのため、既存のデータベースで開始してから、Entity Data Model を作成します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-115">To accomplish that, you will start with an existing database and create the Entity Data Model from it.</span></span> <span data-ttu-id="05ccb-116">この演習では、全体で満たすことになります、 **Database First**方法だけでなく**Code First**アプローチです。</span><span class="sxs-lookup"><span data-stu-id="05ccb-116">Throughout this lab, you will meet the **Database First** approach as well as the **Code First** approach.</span></span>

<span data-ttu-id="05ccb-117">ただし、使用することできますも、 **Model First**アプローチ、ツールを使用して、同じモデルの作成、およびそこから、データベースを生成します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-117">However, you can also use the **Model First** approach, create the same model using the tools, and then generate the database from it.</span></span>

<span data-ttu-id="05ccb-118">![データベースの最初とします。最初のモデル](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First vs です。最初のモデルします。")</span><span class="sxs-lookup"><span data-stu-id="05ccb-118">![Database First vs. Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First vs. Model First")</span></span>

<span data-ttu-id="05ccb-119">*データベースの最初とします。最初のモデルします。*</span><span class="sxs-lookup"><span data-stu-id="05ccb-119">*Database First vs. Model First*</span></span>

<span data-ttu-id="05ccb-120">モデルを生成した後にハード コードされたデータを使用する代わりに、データベースから取得されたデータ ストアのビューを提供する StoreController で適切な調整が作成されます。</span><span class="sxs-lookup"><span data-stu-id="05ccb-120">After generating the Model, you will make the proper adjustments in the StoreController to provide the Store Views with the data taken from the database, instead of using hard-coded data.</span></span> <span data-ttu-id="05ccb-121">変更するビュー テンプレート ビュー テンプレートに同じ ViewModels が戻される、StoreController のためこの時点のデータは、データベースから取得されますが、する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="05ccb-121">You will not need to make any change to the View templates because the StoreController will be returning the same ViewModels to the View templates, although this time the data will come from the database.</span></span>

<span data-ttu-id="05ccb-122">**コードの最初の方法**</span><span class="sxs-lookup"><span data-stu-id="05ccb-122">**The Code First Approach**</span></span>

<span data-ttu-id="05ccb-123">コードの最初の方法では、フレームワークと組み合わせると、通常のクラスを生成せず、コードからモデルを定義できます。</span><span class="sxs-lookup"><span data-stu-id="05ccb-123">The Code First approach allows us to define the model from the code without generating classes that are generally coupled with the framework.</span></span>

<span data-ttu-id="05ccb-124">コードで最初に、モデル オブジェクトで定義した poco から、 &quot;Plain Old CLR Object&quot;です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-124">In code first, model objects are defined with POCOs, &quot;Plain Old CLR Objects&quot;.</span></span> <span data-ttu-id="05ccb-125">した poco からは、単純なの plain クラスを継承がないインターフェイスを実装していないです。</span><span class="sxs-lookup"><span data-stu-id="05ccb-125">POCOs are simple plain classes that have no inheritance and do not implement interfaces.</span></span> <span data-ttu-id="05ccb-126">これらのファイルからデータベースを自動的に生成することができます、または既存のデータベースを使用して、コードから、クラスへのマッピングを生成します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-126">We can automatically generate the database from them, or we can use an existing database and generate the class mapping from the code.</span></span>

<span data-ttu-id="05ccb-127">この方法を使用する利点は、すること、モデルを引き続き (ここでは、Entity Framework)、永続化フレームワークから独立した poco からクラスは、マッピング framework と関連していないようです。</span><span class="sxs-lookup"><span data-stu-id="05ccb-127">The benefits of using this approach is that the Model remains independent from the persistence framework (in this case, Entity Framework), as the POCOs classes are not coupled with the mapping framework.</span></span>

> [!NOTE]
> <span data-ttu-id="05ccb-128">このラボは ASP.NET MVC 4 に基づいており、音楽ストア サンプル アプリケーションのバージョンがカスタマイズされ、このハンズオン ラボで示されている機能のみに合わせて最小限に抑えられます。</span><span class="sxs-lookup"><span data-stu-id="05ccb-128">This Lab is based on ASP.NET MVC 4 and a version of the Music Store sample application customized and minimized to fit only the features shown in this Hands-On Lab.</span></span>
> 
> <span data-ttu-id="05ccb-129">全体を調査したい場合**Music Store**で検索できるチュートリアル アプリケーション[MVC Music Store](https://github.com/evilDave/MVC-Music-Store)です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-129">If you wish to explore the whole **Music Store** tutorial application you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="05ccb-130">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="05ccb-130">Prerequisites</span></span>

<span data-ttu-id="05ccb-131">このラボを完成させるのには、次の項目が必要です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-131">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="05ccb-132">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)または上位 (読み取り[付録 A](#AppendixA)をインストールする方法について)。</span><span class="sxs-lookup"><span data-stu-id="05ccb-132">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="05ccb-133">セットアップ</span><span class="sxs-lookup"><span data-stu-id="05ccb-133">Setup</span></span>

<span data-ttu-id="05ccb-134">**コード スニペットをインストールします。**</span><span class="sxs-lookup"><span data-stu-id="05ccb-134">**Installing Code Snippets**</span></span>

<span data-ttu-id="05ccb-135">便宜上、このラボに沿ったを管理するコードの多くは、Visual Studio のコード スニペットとして利用できます。</span><span class="sxs-lookup"><span data-stu-id="05ccb-135">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="05ccb-136">実行のコード スニペットをインストールする**.\Source\Setup\CodeSnippets.vsi**ファイル。</span><span class="sxs-lookup"><span data-stu-id="05ccb-136">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="05ccb-137">このドキュメントの付録を参照することができます、Visual Studio のコード スニペットとその使用方法を学習するに慣れていない場合&quot;[付録 c: を使用してコード スニペット](#AppendixC)&quot;です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-137">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="05ccb-138">演習</span><span class="sxs-lookup"><span data-stu-id="05ccb-138">Exercises</span></span>

<span data-ttu-id="05ccb-139">次の演習では、このハンズオン ラボから成ります。</span><span class="sxs-lookup"><span data-stu-id="05ccb-139">This Hands-on Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="05ccb-140">手順 1: データベースに追加します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-140">Exercise 1: Adding a Database</span></span>](#Exercise1)
2. [<span data-ttu-id="05ccb-141">手順 2: Code First を使用してデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-141">Exercise 2: Creating a Database using Code First</span></span>](#Exercise2)
3. [<span data-ttu-id="05ccb-142">手順 3: パラメーターを持つデータベースの照会</span><span class="sxs-lookup"><span data-stu-id="05ccb-142">Exercise 3: Querying the Database with Parameters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="05ccb-143">各演習が組み込まれた、**終了**演習を完了した後に取得する必要があります、結果として得られるソリューションを含むフォルダー。</span><span class="sxs-lookup"><span data-stu-id="05ccb-143">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="05ccb-144">演習では操作のヘルプを参照する必要がある場合は、このソリューションをガイドとして使用できます。</span><span class="sxs-lookup"><span data-stu-id="05ccb-144">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="05ccb-145">この演習を完了する時間を推定: **35 分**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-145">Estimated time to complete this lab: **35 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a><span data-ttu-id="05ccb-146">手順 1: データベースに追加します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-146">Exercise 1: Adding a Database</span></span>

<span data-ttu-id="05ccb-147">この演習では、そのデータを使用するために、ソリューションに MusicStore アプリケーションのテーブルを持つデータベースを追加する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-147">In this exercise, you will learn how to add a database with the tables of the MusicStore application to the solution in order to consume its data.</span></span> <span data-ttu-id="05ccb-148">データベースは、モデルを使用して生成され、ソリューションに追加、ハード コーディングされた値を使用する代わりに、データベースから取得したデータをビュー テンプレートを提供する StoreController クラスを変更します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-148">Once the database is generated with the model, and added to the solution, you will modify the StoreController class to provide the View template with the data taken from the database, instead of using hard-coded values.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a><span data-ttu-id="05ccb-149">タスク 1 - データベースに追加します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-149">Task 1 - Adding a Database</span></span>

<span data-ttu-id="05ccb-150">このタスクでは、ソリューションに MusicStore アプリケーションのメイン テーブルを作成済みのデータベースを追加します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-150">In this task, you will add an already created database with the main tables of the MusicStore application to the solution.</span></span>

1. <span data-ttu-id="05ccb-151">開く、**開始**ソリューションにある**ソース/Ex1-AddingADatabaseDBFirst/開始/**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="05ccb-151">Open the **Begin** solution located at **Source/Ex1-AddingADatabaseDBFirst/Begin/** folder.</span></span>

   1. <span data-ttu-id="05ccb-152">いくつか不足している NuGet パッケージをダウンロードする必要がありますが続行前にします。</span><span class="sxs-lookup"><span data-stu-id="05ccb-152">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="05ccb-153">これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-153">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="05ccb-154">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。</span><span class="sxs-lookup"><span data-stu-id="05ccb-154">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="05ccb-155">最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-155">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="05ccb-156">NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="05ccb-156">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="05ccb-157">NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="05ccb-157">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="05ccb-158">これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-158">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="05ccb-159">追加**MvcMusicStore**データベース ファイル。</span><span class="sxs-lookup"><span data-stu-id="05ccb-159">Add **MvcMusicStore** database file.</span></span> <span data-ttu-id="05ccb-160">このハンズオン ラボと呼ばれる作成済みのデータベースを使用して**MvcMusicStore.mdf**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-160">In this Hands-on Lab, you will use an already created database called **MvcMusicStore.mdf**.</span></span> <span data-ttu-id="05ccb-161">を右クリックして**アプリ\_データ**フォルダーを指す**追加** をクリックし、**既存項目の**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-161">To do that, right-click **App\_Data** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="05ccb-162">参照**\Source\Assets**を選択し、 **MvcMusicStore.mdf**ファイル。</span><span class="sxs-lookup"><span data-stu-id="05ccb-162">Browse to **\Source\Assets** and select the **MvcMusicStore.mdf** file.</span></span>

    <span data-ttu-id="05ccb-163">![既存の項目を追加する](aspnet-mvc-4-models-and-data-access/_static/image2.png "既存の項目を追加します。")</span><span class="sxs-lookup"><span data-stu-id="05ccb-163">![Adding an Existing Item](aspnet-mvc-4-models-and-data-access/_static/image2.png "Adding an Existing Item")</span></span>

    <span data-ttu-id="05ccb-164">*既存の項目を追加します。*</span><span class="sxs-lookup"><span data-stu-id="05ccb-164">*Adding an Existing Item*</span></span>

    <span data-ttu-id="05ccb-165">![データベース ファイルの MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf データベース ファイル")</span><span class="sxs-lookup"><span data-stu-id="05ccb-165">![MvcMusicStore.mdf database file](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf database file")</span></span>

    <span data-ttu-id="05ccb-166">*MvcMusicStore.mdf データベース ファイル*</span><span class="sxs-lookup"><span data-stu-id="05ccb-166">*MvcMusicStore.mdf database file*</span></span>

    <span data-ttu-id="05ccb-167">データベースがプロジェクトに追加されました。</span><span class="sxs-lookup"><span data-stu-id="05ccb-167">The database has been added to the project.</span></span> <span data-ttu-id="05ccb-168">ソリューション内のデータベースが配置されている場合でもクエリを実行し、別のデータベース サーバーでホストされているように更新できます。</span><span class="sxs-lookup"><span data-stu-id="05ccb-168">Even when the database is located inside the solution, you can query and update it as it was hosted in a different database server.</span></span>

    <span data-ttu-id="05ccb-169">![ソリューション エクスプ ローラーでデータベースを MvcMusicStore](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore ソリューション エクスプ ローラーでデータベース")</span><span class="sxs-lookup"><span data-stu-id="05ccb-169">![MvcMusicStore database in Solution Explorer](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore database in Solution Explorer")</span></span>

    <span data-ttu-id="05ccb-170">*ソリューション エクスプ ローラーで MvcMusicStore データベース*</span><span class="sxs-lookup"><span data-stu-id="05ccb-170">*MvcMusicStore database in Solution Explorer*</span></span>
3. <span data-ttu-id="05ccb-171">データベースへの接続を確認します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-171">Verify the connection to the database.</span></span> <span data-ttu-id="05ccb-172">これを行うをダブルクリックして**MvcMusicStore.mdf**の接続を確立します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-172">To do this, double-click **MvcMusicStore.mdf** to establish a connection.</span></span>

    <span data-ttu-id="05ccb-173">![MvcMusicStore.mdf に接続する](aspnet-mvc-4-models-and-data-access/_static/image5.png "MvcMusicStore.mdf への接続")</span><span class="sxs-lookup"><span data-stu-id="05ccb-173">![Connecting to MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "Connecting to MvcMusicStore.mdf")</span></span>

    <span data-ttu-id="05ccb-174">*MvcMusicStore.mdf への接続*</span><span class="sxs-lookup"><span data-stu-id="05ccb-174">*Connecting to MvcMusicStore.mdf*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a><span data-ttu-id="05ccb-175">タスク 2 - データ モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-175">Task 2 - Creating a Data Model</span></span>

<span data-ttu-id="05ccb-176">このタスクでは、前のタスクに追加されたデータベースとやり取りするデータ モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-176">In this task, you will create a data model to interact with the database added in the previous task.</span></span>

1. <span data-ttu-id="05ccb-177">データベースを表すデータ モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-177">Create a data model that will represent the database.</span></span> <span data-ttu-id="05ccb-178">ソリューション エクスプ ローラーを右クリックで、**モデル**フォルダーを指す**追加**順にクリック**新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-178">To do this, in Solution Explorer right-click the **Models** folder, point to **Add** and then click **New Item**.</span></span> <span data-ttu-id="05ccb-179">**新しい項目の追加**ダイアログで、選択、**データ**テンプレートし、 **ADO.NET エンティティ データ モデル**項目。</span><span class="sxs-lookup"><span data-stu-id="05ccb-179">In the **Add New Item** dialog, select the **Data** template and then the **ADO.NET Entity Data Model** item.</span></span> <span data-ttu-id="05ccb-180">データ モデル名に変更**StoreDB.edmx**  をクリック**追加**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-180">Change the data model name to **StoreDB.edmx** and click **Add**.</span></span>

    <span data-ttu-id="05ccb-181">![StoreDB ADO.NET エンティティ データ モデルを追加する](aspnet-mvc-4-models-and-data-access/_static/image6.png "StoreDB ADO.NET エンティティ データ モデルを追加します。")</span><span class="sxs-lookup"><span data-stu-id="05ccb-181">![Adding the StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "Adding the StoreDB ADO.NET Entity Data Model")</span></span>

    <span data-ttu-id="05ccb-182">*StoreDB ADO.NET エンティティ データ モデルを追加します。*</span><span class="sxs-lookup"><span data-stu-id="05ccb-182">*Adding the StoreDB ADO.NET Entity Data Model*</span></span>
2. <span data-ttu-id="05ccb-183">**Entity Data Model ウィザード**が表示されます。</span><span class="sxs-lookup"><span data-stu-id="05ccb-183">The **Entity Data Model Wizard** will appear.</span></span> <span data-ttu-id="05ccb-184">このウィザードをモデル層の作成に使用します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-184">This wizard will guide you through the creation of the model layer.</span></span> <span data-ttu-id="05ccb-185">追加された既存のデータベース recentyl に基づいてモデルを作成する必要があります、ために選択**データベースから生成** をクリック**次**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-185">Since the model should be created based on the existing database recentyl added, select **Generate from database** and click **Next**.</span></span>

    <span data-ttu-id="05ccb-186">![モデル コンテンツを選択する](aspnet-mvc-4-models-and-data-access/_static/image7.png "モデル コンテンツを選択します。")</span><span class="sxs-lookup"><span data-stu-id="05ccb-186">![Choosing the model content](aspnet-mvc-4-models-and-data-access/_static/image7.png "Choosing the model content")</span></span>

    <span data-ttu-id="05ccb-187">*モデル コンテンツを選択します。*</span><span class="sxs-lookup"><span data-stu-id="05ccb-187">*Choosing the model content*</span></span>
3. <span data-ttu-id="05ccb-188">データベースからモデルを生成するためを使用する接続を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="05ccb-188">Since you are generating a model from a database, you will need to specify the connection to use.</span></span> <span data-ttu-id="05ccb-189">をクリックして**新しい接続**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-189">Click **New Connection**.</span></span>
4. <span data-ttu-id="05ccb-190">選択**Microsoft SQL Server データベース ファイル** をクリック**続行**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-190">Select **Microsoft SQL Server Database File** and click **Continue**.</span></span>

    <span data-ttu-id="05ccb-191">![データ ソースの選択](aspnet-mvc-4-models-and-data-access/_static/image8.png "データ ソースの選択")</span><span class="sxs-lookup"><span data-stu-id="05ccb-191">![Choose data source](aspnet-mvc-4-models-and-data-access/_static/image8.png "Choose data source")</span></span>

    <span data-ttu-id="05ccb-192">*データ ソース ダイアログを選択します。*</span><span class="sxs-lookup"><span data-stu-id="05ccb-192">*Choose data source dialog*</span></span>
5. <span data-ttu-id="05ccb-193">をクリックして**参照**にデータベースを選択**MvcMusicStore.mdf**内にある、**アプリ\_データ**フォルダーをクリック**OK**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-193">Click **Browse** and select the database **MvcMusicStore.mdf** you located in the **App\_Data** folder and click **OK**.</span></span>

    <span data-ttu-id="05ccb-194">![接続プロパティ](aspnet-mvc-4-models-and-data-access/_static/image9.png "接続のプロパティ")</span><span class="sxs-lookup"><span data-stu-id="05ccb-194">![Connection properties](aspnet-mvc-4-models-and-data-access/_static/image9.png "Connection properties")</span></span>

    <span data-ttu-id="05ccb-195">*接続のプロパティ*</span><span class="sxs-lookup"><span data-stu-id="05ccb-195">*Connection properties*</span></span>
6. <span data-ttu-id="05ccb-196">生成されたクラスはエンティティ接続文字列と同じ名前を付ける、名前を変更する必要があります**MusicStoreEntities**  をクリック**次**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-196">The generated class should have the same name as the entity connection string, so change its name to **MusicStoreEntities** and click **Next**.</span></span>

    <span data-ttu-id="05ccb-197">![データ接続を選択する](aspnet-mvc-4-models-and-data-access/_static/image10.png "データ接続を選択します。")</span><span class="sxs-lookup"><span data-stu-id="05ccb-197">![Choosing the data connection](aspnet-mvc-4-models-and-data-access/_static/image10.png "Choosing the data connection")</span></span>

    <span data-ttu-id="05ccb-198">*データ接続を選択します。*</span><span class="sxs-lookup"><span data-stu-id="05ccb-198">*Choosing the data connection*</span></span>
7. <span data-ttu-id="05ccb-199">使用するデータベース オブジェクトを選択します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-199">Choose the database objects to use.</span></span> <span data-ttu-id="05ccb-200">エンティティ モデルには、データベースのテーブルだけで使用する、必要に応じて、選択、**テーブル**オプション、およびことを確認して、**モデルに外部キー列を含める**と**複数化または単数化する生成オブジェクト名**オプションも選択します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-200">As the Entity Model will use just the database's tables, select the **Tables** option, and make sure that the **Include foreign key columns in the model** and **Pluralize or singularize generated object names** options are also selected.</span></span> <span data-ttu-id="05ccb-201">モデルの Namespace を変更する**MvcMusicStore.Model**  をクリック**完了**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-201">Change the Model Namespace to **MvcMusicStore.Model** and click **Finish**.</span></span>

    <span data-ttu-id="05ccb-202">![データベース オブジェクトを選択する](aspnet-mvc-4-models-and-data-access/_static/image11.png "データベース オブジェクトを選択します。")</span><span class="sxs-lookup"><span data-stu-id="05ccb-202">![Choosing the database objects](aspnet-mvc-4-models-and-data-access/_static/image11.png "Choosing the database objects")</span></span>

    <span data-ttu-id="05ccb-203">*データベース オブジェクトを選択します。*</span><span class="sxs-lookup"><span data-stu-id="05ccb-203">*Choosing the database objects*</span></span>

    > [!NOTE]
    > <span data-ttu-id="05ccb-204">セキュリティ警告ダイアログ ボックスが表示されている場合にをクリックして**OK**をテンプレートを実行し、モデルのエンティティ クラスを生成します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-204">If a Security Warning dialog is shown, click **OK** to run the template and generate the classes for the model entities.</span></span>
8. <span data-ttu-id="05ccb-205">データベースを各テーブルにマップする別のクラスが作成するときに、データベースのエンティティの図が表示されます。</span><span class="sxs-lookup"><span data-stu-id="05ccb-205">An entity diagram for the database will appear, while a separate class that maps each table to the database will be created.</span></span> <span data-ttu-id="05ccb-206">たとえば、**アルバム**テーブルはで表されます、**アルバム**クラス、テーブル内の各列がクラス プロパティにマップされます。</span><span class="sxs-lookup"><span data-stu-id="05ccb-206">For example, the **Albums** table will be represented by an **Album** class, where each column in the table will map to a class property.</span></span> <span data-ttu-id="05ccb-207">こうとをクエリして、データベース内の行を表すオブジェクトを使用します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-207">This will allow you to query and work with objects that represent rows in the database.</span></span>

    <span data-ttu-id="05ccb-208">![エンティティの図](aspnet-mvc-4-models-and-data-access/_static/image12.png "エンティティの図")</span><span class="sxs-lookup"><span data-stu-id="05ccb-208">![Entity diagram](aspnet-mvc-4-models-and-data-access/_static/image12.png "Entity diagram")</span></span>

    <span data-ttu-id="05ccb-209">*エンティティの図*</span><span class="sxs-lookup"><span data-stu-id="05ccb-209">*Entity diagram*</span></span>

    > [!NOTE]
    > <span data-ttu-id="05ccb-210">T4 テンプレート (.tt) は、エンティティ クラスを生成するコードを実行し、同じ名前の既存のクラスが上書きされます。</span><span class="sxs-lookup"><span data-stu-id="05ccb-210">The T4 templates (.tt) run code to generate the entities classes and will overwrite the existing classes with the same name.</span></span> <span data-ttu-id="05ccb-211">クラスは、この例で&quot;アルバム&quot;、&quot;ジャンル&quot;と&quot;アーティスト&quot;生成されたコードで上書きされています。</span><span class="sxs-lookup"><span data-stu-id="05ccb-211">In this example, the classes &quot;Album&quot;, &quot;Genre&quot; and &quot;Artist&quot; were overwritten with the generated code.</span></span>


<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a><span data-ttu-id="05ccb-212">タスク 3 - アプリケーションのビルド</span><span class="sxs-lookup"><span data-stu-id="05ccb-212">Task 3 - Building the Application</span></span>

<span data-ttu-id="05ccb-213">このタスクでは、確認をモデルの生成が削除されますが、**アルバム**、**ジャンル**と**アーティスト**モデル クラス プロジェクトが正常にビルドを使用して新しいデータ モデル クラス。</span><span class="sxs-lookup"><span data-stu-id="05ccb-213">In this task, you will check that, although the model generation have removed the **Album**, **Genre** and **Artist** model classes, the project builds successfully by using the new data model classes.</span></span>

1. <span data-ttu-id="05ccb-214">選択して、プロジェクトをビルド、**ビルド**メニュー項目し**ビルド MvcMusicStore**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-214">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="05ccb-215">![プロジェクトのビルド](aspnet-mvc-4-models-and-data-access/_static/image13.png "プロジェクトのビルド")</span><span class="sxs-lookup"><span data-stu-id="05ccb-215">![Building the project](aspnet-mvc-4-models-and-data-access/_static/image13.png "Building the project")</span></span>

    <span data-ttu-id="05ccb-216">*プロジェクトのビルド*</span><span class="sxs-lookup"><span data-stu-id="05ccb-216">*Building the project*</span></span>
2. <span data-ttu-id="05ccb-217">プロジェクトが正常にビルドします。</span><span class="sxs-lookup"><span data-stu-id="05ccb-217">The project builds successfully.</span></span> <span data-ttu-id="05ccb-218">なぜ引き続き動作しますか。</span><span class="sxs-lookup"><span data-stu-id="05ccb-218">Why does it still work?</span></span> <span data-ttu-id="05ccb-219">データベースのテーブルが削除されたクラスに使用していたプロパティが含まれるフィールドを持つために、それと**アルバム**と**ジャンル**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-219">It works because the database tables have fields that include the properties that you were using in the removed classes **Album** and **Genre**.</span></span>

    <span data-ttu-id="05ccb-220">![ビルドが成功した](aspnet-mvc-4-models-and-data-access/_static/image14.png "が成功したビルド")</span><span class="sxs-lookup"><span data-stu-id="05ccb-220">![Builds succeeded](aspnet-mvc-4-models-and-data-access/_static/image14.png "Builds succeeded")</span></span>

    <span data-ttu-id="05ccb-221">*ビルドが成功しました*</span><span class="sxs-lookup"><span data-stu-id="05ccb-221">*Builds succeeded*</span></span>
3. <span data-ttu-id="05ccb-222">デザイナーはダイアグラム形式でのエンティティが表示されますが、これらは実際に c# クラスです。</span><span class="sxs-lookup"><span data-stu-id="05ccb-222">While the designer displays the entities in a diagram format, they are really C# classes.</span></span> <span data-ttu-id="05ccb-223">展開して、 **StoreDB.edmx**ソリューション エクスプ ローラーでノードし**StoreDB.tt**、新規生成されたエンティティが表示されます。</span><span class="sxs-lookup"><span data-stu-id="05ccb-223">Expand the **StoreDB.edmx** node in the Solution Explorer and then **StoreDB.tt**, you will see the new generated entities.</span></span>

    <span data-ttu-id="05ccb-224">![生成されたファイル](aspnet-mvc-4-models-and-data-access/_static/image15.png "によって生成されたファイル")</span><span class="sxs-lookup"><span data-stu-id="05ccb-224">![Generated files](aspnet-mvc-4-models-and-data-access/_static/image15.png "Generated files")</span></span>

    <span data-ttu-id="05ccb-225">*生成されたファイル*</span><span class="sxs-lookup"><span data-stu-id="05ccb-225">*Generated files*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="05ccb-226">タスク 4 - データベースの照会</span><span class="sxs-lookup"><span data-stu-id="05ccb-226">Task 4 - Querying the Database</span></span>

<span data-ttu-id="05ccb-227">このタスクでハードコーディングされたデータを使用する代わりにこれはデータベースにクエリ情報を取得するように、StoreController クラスが更新されます。</span><span class="sxs-lookup"><span data-stu-id="05ccb-227">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will query the database to retrieve the information.</span></span>

1. <span data-ttu-id="05ccb-228">開いている**Controllers\StoreController.cs**のインスタンスを保持するクラスに次のフィールドを追加し、 **MusicStoreEntities**という名前のクラス**storeDB**:</span><span class="sxs-lookup"><span data-stu-id="05ccb-228">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="05ccb-229">(コード スニペットの*モデルおよびデータ アクセス-Ex1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="05ccb-229">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
~~~
2. <span data-ttu-id="05ccb-230">**MusicStoreEntities**クラスは、データベース内の各テーブルのコレクション プロパティを公開します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-230">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="05ccb-231">更新**参照**アクションを取得する方法ですべてのジャンル、**アルバム**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-231">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="05ccb-232">(コード スニペットの*モデルおよびデータ アクセス - Ex1 ストア参照*)</span><span class="sxs-lookup"><span data-stu-id="05ccb-232">(Code Snippet - *Models And Data Access - Ex1 Store Browse*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

> [!NOTE]
> You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.
> 
> For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).
~~~
3. <span data-ttu-id="05ccb-233">更新**インデックス**アクション メソッドは、すべてのジャンルを取得します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-233">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="05ccb-234">(コード スニペットの*モデルおよびデータ アクセス - Ex1 ストア インデックス*)</span><span class="sxs-lookup"><span data-stu-id="05ccb-234">(Code Snippet - *Models And Data Access - Ex1 Store Index*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
~~~
4. <span data-ttu-id="05ccb-235">更新**インデックス**アクション メソッドのすべてのジャンルを取得し、一覧にコレクションを変換します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-235">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="05ccb-236">(コード スニペットの*モデルおよびデータ アクセス - Ex1 ストア GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="05ccb-236">(Code Snippet - *Models And Data Access - Ex1 Store GenreMenu*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]
~~~

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="05ccb-237">タスク 5 - アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-237">Task 5 - Running the Application</span></span>

<span data-ttu-id="05ccb-238">このタスクでは、ストア インデックスのページをジャンルのハードコードされたものではなく、データベースに格納されているようになりました表示するかを確認します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-238">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="05ccb-239">に、ビューのテンプレートを変更する必要はありません、 **StoreController**はエンティティを取得する同じ以前と同様が、この時点のデータは、データベースから取得されます。</span><span class="sxs-lookup"><span data-stu-id="05ccb-239">There is no need to change the View template because the **StoreController** is returning the same entities as before, although this time the data will come from the database.</span></span>

1. <span data-ttu-id="05ccb-240">キーを押して、ソリューションを再構築**f5 キーを押して**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-240">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="05ccb-241">ホーム ページで、プロジェクトを開始します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-241">The project starts in the Home page.</span></span> <span data-ttu-id="05ccb-242">いることを確認のメニュー**ジャンル**ハードコードされた一覧で、できなくなり、データがデータベースから直接取得します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-242">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    <span data-ttu-id="05ccb-244">*データベースからジャンルをブラウズ*</span><span class="sxs-lookup"><span data-stu-id="05ccb-244">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="05ccb-245">ここで、ジャンルを参照し、アルバムがデータベースから生成されますを確認します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-245">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="05ccb-246">![データベースからアルバムを閲覧](aspnet-mvc-4-models-and-data-access/_static/image17.png "データベースからアルバムを参照")</span><span class="sxs-lookup"><span data-stu-id="05ccb-246">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image17.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="05ccb-247">*データベースからアルバムを参照*</span><span class="sxs-lookup"><span data-stu-id="05ccb-247">*Browsing Albums from the database*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a><span data-ttu-id="05ccb-248">手順 2: 最初にコードを使用してデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-248">Exercise 2: Creating a Database Using Code First</span></span>

<span data-ttu-id="05ccb-249">この演習では、コードの最初の方法を使用して、MusicStore アプリケーションのテーブルを含むデータベースを作成する方法とそのデータにアクセスする方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-249">In this exercise, you will learn how to use the Code First approach to create a database with the tables of the MusicStore application, and how to access its data.</span></span>

<span data-ttu-id="05ccb-250">モデルが生成されると、ハードコードされた値を使用する代わりに、データベースから取得したデータをビュー テンプレートを提供する StoreController を変更します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-250">Once the model is generated, you will modify the StoreController to provide the View template with the data taken from the database, instead of using hardcoded values.</span></span>

> [!NOTE]
> <span data-ttu-id="05ccb-251">手順 1 を完了し、データベースの最初の方法を使用してきた既にする場合は、ここで、別のプロセスで同じ結果を取得する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-251">If you have completed Exercise 1 and have already worked with the Database First approach, you will now learn how to get the same results with a different process.</span></span> <span data-ttu-id="05ccb-252">演習 1 と同じようなタスクは、読み取りが容易に設定されています。</span><span class="sxs-lookup"><span data-stu-id="05ccb-252">The tasks that are in common with Exercise 1 have been marked to make your reading easier.</span></span> <span data-ttu-id="05ccb-253">手順 1 を完了していないコードの最初の方法を学習したい場合は、ここから開始し、このトピックの完全なカバレッジを取得できます。</span><span class="sxs-lookup"><span data-stu-id="05ccb-253">If you have not completed Exercise 1 but would like to learn the Code First approach, you can start from this exercise and get a full coverage of the topic.</span></span>


<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a><span data-ttu-id="05ccb-254">タスク 1 - サンプル データを取り込み</span><span class="sxs-lookup"><span data-stu-id="05ccb-254">Task 1 - Populating Sample Data</span></span>

<span data-ttu-id="05ccb-255">このタスクの作成時に初期状態で 1 コード優先を使用してデータベースにサンプル データを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="05ccb-255">In this task, you will populate the database with sample data when it is intially created using Code-First.</span></span>

1. <span data-ttu-id="05ccb-256">開く、**開始**ソリューションにある**ソース/Ex2-CreatingADatabaseCodeFirst/開始/**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="05ccb-256">Open the **Begin** solution located at **Source/Ex2-CreatingADatabaseCodeFirst/Begin/** folder.</span></span> <span data-ttu-id="05ccb-257">それ以外の場合、作業を続行できますを使用して、**終了**ソリューションは、前の手順を完了して取得します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-257">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="05ccb-258">指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。</span><span class="sxs-lookup"><span data-stu-id="05ccb-258">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="05ccb-259">これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-259">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="05ccb-260">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。</span><span class="sxs-lookup"><span data-stu-id="05ccb-260">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="05ccb-261">最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-261">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="05ccb-262">NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="05ccb-262">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="05ccb-263">NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="05ccb-263">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="05ccb-264">これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-264">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="05ccb-265">追加、 **SampleData.cs**ファイルの名前を**モデル**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="05ccb-265">Add the **SampleData.cs** file to the **Models** folder.</span></span> <span data-ttu-id="05ccb-266">を右クリックして**モデル**フォルダーを指す**追加** をクリックし、**既存項目の**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-266">To do that, right-click **Models** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="05ccb-267">参照**\Source\Assets**を選択し、 **SampleData.cs**ファイル。</span><span class="sxs-lookup"><span data-stu-id="05ccb-267">Browse to **\Source\Assets** and select the **SampleData.cs** file.</span></span>

    <span data-ttu-id="05ccb-268">![サンプル データへの追加コード](aspnet-mvc-4-models-and-data-access/_static/image18.png "サンプル データへのコードの追加")</span><span class="sxs-lookup"><span data-stu-id="05ccb-268">![Sample data populate code](aspnet-mvc-4-models-and-data-access/_static/image18.png "Sample data populate code")</span></span>

    <span data-ttu-id="05ccb-269">*サンプル データへのコードを追加します。*</span><span class="sxs-lookup"><span data-stu-id="05ccb-269">*Sample data populate code*</span></span>
3. <span data-ttu-id="05ccb-270">開く、 **Global.asax.cs**ファイルし、次の追加*を使用して*ステートメントです。</span><span class="sxs-lookup"><span data-stu-id="05ccb-270">Open the **Global.asax.cs** file and add the following *using* statements.</span></span>

    <span data-ttu-id="05ccb-271">(コード スニペットの*モデルおよびデータ アクセス - Ex2 グローバル Asax Using*)</span><span class="sxs-lookup"><span data-stu-id="05ccb-271">(Code Snippet - *Models And Data Access - Ex2 Global Asax Usings*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
~~~
4. <span data-ttu-id="05ccb-272">**アプリケーション\_Start()**メソッドは、データベース初期化子を設定する次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-272">In the **Application\_Start()** method add the following line to set the database initializer.</span></span>

    <span data-ttu-id="05ccb-273">(コード スニペットの*モデルおよびデータ アクセス - Ex2 グローバル Asax SetInitializer*)</span><span class="sxs-lookup"><span data-stu-id="05ccb-273">(Code Snippet - *Models And Data Access - Ex2 Global Asax SetInitializer*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]
~~~

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a><span data-ttu-id="05ccb-274">タスク 2 - データベースへの接続の構成</span><span class="sxs-lookup"><span data-stu-id="05ccb-274">Task 2 - Configuring the connection to the Database</span></span>

<span data-ttu-id="05ccb-275">プロジェクトにデータベースが既に追加されたに記述、 **Web.config**ファイルの接続文字列。</span><span class="sxs-lookup"><span data-stu-id="05ccb-275">Now that you have already added a database to our project, you will write in the **Web.config** file the connection string.</span></span>

1. <span data-ttu-id="05ccb-276">接続文字列を追加する**Web.config**です。実行するには、開く**Web.config**プロジェクトのルートと置換、接続文字列を次の行で DefaultConnection をという名前で、 **&lt;connectionStrings&gt;**セクション。</span><span class="sxs-lookup"><span data-stu-id="05ccb-276">Add a connection string at **Web.config**. To do that, open **Web.config** at project root and replace the connection string named DefaultConnection with this line in the **&lt;connectionStrings&gt;** section:</span></span>

    <span data-ttu-id="05ccb-277">![Web.config ファイルの場所](aspnet-mvc-4-models-and-data-access/_static/image19.png "Web.config ファイルの場所")</span><span class="sxs-lookup"><span data-stu-id="05ccb-277">![Web.config file location](aspnet-mvc-4-models-and-data-access/_static/image19.png "Web.config file location")</span></span>

    <span data-ttu-id="05ccb-278">*web.config ファイルの場所*</span><span class="sxs-lookup"><span data-stu-id="05ccb-278">*Web.config file location*</span></span>


~~~
[!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]
~~~

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a><span data-ttu-id="05ccb-279">タスク 3 - モデルを操作</span><span class="sxs-lookup"><span data-stu-id="05ccb-279">Task 3 - Working with the Model</span></span>

<span data-ttu-id="05ccb-280">データベースへの接続を構成しておくことは、データベース テーブルを使用してモデルをリンクします。</span><span class="sxs-lookup"><span data-stu-id="05ccb-280">Now that you have already configured the connection to the database, you will link the model with the database tables.</span></span> <span data-ttu-id="05ccb-281">このタスクでは、Code First でデータベースにリンクするクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-281">In this task, you will create a class that will be linked to the database with Code First.</span></span> <span data-ttu-id="05ccb-282">変更する必要がある、既存モデルの POCO クラスがあることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="05ccb-282">Remember that there is an existent POCO model class that should be modified.</span></span>

   > [!NOTE]
> <span data-ttu-id="05ccb-283">演習 1 を完了する場合は、ウィザードによって、この手順が行われたことがわかります。</span><span class="sxs-lookup"><span data-stu-id="05ccb-283">If you completed Exercise 1, you will note that this step was performed by a wizard.</span></span> <span data-ttu-id="05ccb-284">Code First によりデータ エンティティにリンクするクラスを手動で作成されます。</span><span class="sxs-lookup"><span data-stu-id="05ccb-284">By doing Code First, you will manually create classes that will be linked to data entities.</span></span>


1. <span data-ttu-id="05ccb-285">POCO モデル クラスを開く**ジャンル**から**モデル**フォルダーをプロジェクトし、ID を含める</span><span class="sxs-lookup"><span data-stu-id="05ccb-285">Open the POCO model class **Genre** from **Models** project folder and include an ID.</span></span> <span data-ttu-id="05ccb-286">Int 型のプロパティを使用して、名前を持つ**GenreId**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-286">Use an int property with the name **GenreId**.</span></span>

    <span data-ttu-id="05ccb-287">(コード スニペットの*モデルおよびデータ アクセス - Ex2 コードの最初のジャンル*)</span><span class="sxs-lookup"><span data-stu-id="05ccb-287">(Code Snippet - *Models And Data Access - Ex2 Code First Genre*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

> [!NOTE]
> To work with Code First conventions, the class Genre must have a primary key property that will be automatically detected.
> 
> You can read more about Code First Conventions in this [msdn article](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).
~~~
2. <span data-ttu-id="05ccb-288">これで、POCO のモデル クラスを開く**アルバム**から**モデル**プロジェクト フォルダーと外部キーが含まれて、名前のプロパティを作成する**GenreId**と**ArtistId**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-288">Now, open the POCO model class **Album** from **Models** project folder and include the foreign keys, create properties with the names **GenreId** and **ArtistId**.</span></span> <span data-ttu-id="05ccb-289">このクラスが既にある、 **GenreId**の主キー。</span><span class="sxs-lookup"><span data-stu-id="05ccb-289">This class already have the **GenreId** for the primary key.</span></span>

    <span data-ttu-id="05ccb-290">(コード スニペットの*モデルおよびデータ アクセス - Ex2 コードの最初のアルバム*)</span><span class="sxs-lookup"><span data-stu-id="05ccb-290">(Code Snippet - *Models And Data Access - Ex2 Code First Album*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
~~~
3. <span data-ttu-id="05ccb-291">POCO モデル クラスを開く**アーティスト**を含めると、 **ArtistId**プロパティです。</span><span class="sxs-lookup"><span data-stu-id="05ccb-291">Open the POCO model class **Artist** and include the **ArtistId** property.</span></span>

    <span data-ttu-id="05ccb-292">(コード スニペットの*モデルおよびデータ アクセス - Ex2 コードの最初のアーティスト*)</span><span class="sxs-lookup"><span data-stu-id="05ccb-292">(Code Snippet - *Models And Data Access - Ex2 Code First Artist*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
~~~
4. <span data-ttu-id="05ccb-293">右クリックし、**モデル**プロジェクト フォルダーを選択**追加 |クラス**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-293">Right-click the **Models** project folder and select **Add | Class**.</span></span> <span data-ttu-id="05ccb-294">ファイルの名前を付けます**MusicStoreEntities.cs**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-294">Name the file **MusicStoreEntities.cs**.</span></span> <span data-ttu-id="05ccb-295">をクリックし、**追加します。**</span><span class="sxs-lookup"><span data-stu-id="05ccb-295">Then, click **Add.**</span></span>

    <span data-ttu-id="05ccb-296">![クラスの追加](aspnet-mvc-4-models-and-data-access/_static/image20.png "クラスの追加")</span><span class="sxs-lookup"><span data-stu-id="05ccb-296">![Adding a class](aspnet-mvc-4-models-and-data-access/_static/image20.png "Adding a class")</span></span>

    <span data-ttu-id="05ccb-297">*新しい項目の追加*</span><span class="sxs-lookup"><span data-stu-id="05ccb-297">*Adding a new item*</span></span>

    <span data-ttu-id="05ccb-298">![Class2 を追加する](aspnet-mvc-4-models-and-data-access/_static/image21.png "class2 を追加します。")</span><span class="sxs-lookup"><span data-stu-id="05ccb-298">![Adding a class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Adding a class2")</span></span>

    <span data-ttu-id="05ccb-299">*クラスの追加*</span><span class="sxs-lookup"><span data-stu-id="05ccb-299">*Adding a class*</span></span>
5. <span data-ttu-id="05ccb-300">先ほど作成したクラスを開く**MusicStoreEntities.cs**、名前空間を含めると**System.Data.Entity**と**System.Data.Entity.Infrastructure**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-300">Open the class you have just created, **MusicStoreEntities.cs**, and include the namespaces **System.Data.Entity** and **System.Data.Entity.Infrastructure**.</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
~~~
6. <span data-ttu-id="05ccb-301">拡張するクラスの宣言を置き換える、 **DbContext**クラス: パブリックに宣言**DBSet**オーバーライドと**OnModelCreating**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="05ccb-301">Replace the class declaration to extend the **DbContext** class: declare a public **DBSet** and override **OnModelCreating** method.</span></span> <span data-ttu-id="05ccb-302">この手順の後に、Entity Framework でモデルをリンクしているドメイン クラスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="05ccb-302">After this step you will get a domain class that will link your model with the Entity Framework.</span></span> <span data-ttu-id="05ccb-303">そのためには、次のようにクラス コードを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="05ccb-303">In order to do that, replace the class code with the following:</span></span>

    <span data-ttu-id="05ccb-304">(コード スニペットの*モデルおよびデータ アクセス - Ex2 コード最初 MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="05ccb-304">(Code Snippet - *Models And Data Access - Ex2 Code First MusicStoreEntities*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> With Entity Framework **DbContext** and **DBSet** you will be able to query the POCO class Genre. By extending **OnModelCreating** method, you are specifying in the **code** how Genre will be mapped to a database table. You can find more information about DBContext and DBSet in this msdn article: [link](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)
~~~

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="05ccb-305">タスク 4 - データベースの照会</span><span class="sxs-lookup"><span data-stu-id="05ccb-305">Task 4 - Querying the Database</span></span>

<span data-ttu-id="05ccb-306">これにより、ハードコードされたデータを使用する代わりに、そのデータベースから取得する、このタスクで StoreController クラスが更新されます。</span><span class="sxs-lookup"><span data-stu-id="05ccb-306">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will retrieve it from the database.</span></span>

> [!NOTE]
> <span data-ttu-id="05ccb-307">このタスクでは、手順 1 と共通します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-307">This task is in common with Exercise 1.</span></span>
> 
> <span data-ttu-id="05ccb-308">演習 1 が完了した場合これらの手順は、同じ両方の方法で注意してくださいされます (データベースの最初または Code first)。</span><span class="sxs-lookup"><span data-stu-id="05ccb-308">If you completed Exercise 1 you will note these steps are the same in both approaches (Database first or Code first).</span></span> <span data-ttu-id="05ccb-309">モデルでは、データをリンクする方法では異なるが、データ エンティティへのアクセスは、コント ローラーから透過的まだ。</span><span class="sxs-lookup"><span data-stu-id="05ccb-309">They are different in how the data is linked with the model, but the access to data entities is yet transparent from the controller.</span></span>


1. <span data-ttu-id="05ccb-310">開いている**Controllers\StoreController.cs**のインスタンスを保持するクラスに次のフィールドを追加し、 **MusicStoreEntities**という名前のクラス**storeDB**:</span><span class="sxs-lookup"><span data-stu-id="05ccb-310">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="05ccb-311">(コード スニペットの*モデルおよびデータ アクセス-Ex1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="05ccb-311">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
~~~
2. <span data-ttu-id="05ccb-312">**MusicStoreEntities**クラスは、データベース内の各テーブルのコレクション プロパティを公開します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-312">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="05ccb-313">更新**参照**アクションを取得する方法ですべてのジャンル、**アルバム**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-313">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="05ccb-314">(コード スニペットの*モデルおよびデータ アクセス - Ex2 ストア参照*)</span><span class="sxs-lookup"><span data-stu-id="05ccb-314">(Code Snippet - *Models And Data Access - Ex2 Store Browse*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

> [!NOTE]
> You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.
> 
> For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).
~~~
3. <span data-ttu-id="05ccb-315">更新**インデックス**アクション メソッドは、すべてのジャンルを取得します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-315">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="05ccb-316">(コード スニペットの*モデルおよびデータ アクセス - Ex2 ストア インデックス*)</span><span class="sxs-lookup"><span data-stu-id="05ccb-316">(Code Snippet - *Models And Data Access - Ex2 Store Index*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
~~~
4. <span data-ttu-id="05ccb-317">更新**インデックス**アクション メソッドのすべてのジャンルを取得し、一覧にコレクションを変換します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-317">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="05ccb-318">(コード スニペットの*モデルおよびデータ アクセス - Ex2 ストア GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="05ccb-318">(Code Snippet - *Models And Data Access - Ex2 Store GenreMenu*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]
~~~

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="05ccb-319">タスク 5 - アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-319">Task 5 - Running the Application</span></span>

<span data-ttu-id="05ccb-320">このタスクでは、ストア インデックスのページをジャンルのハードコードされたものではなく、データベースに格納されているようになりました表示するかを確認します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-320">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="05ccb-321">に、ビューのテンプレートを変更する必要はありません、 **StoreController**同じを返して**StoreIndexViewModel**以前と同様、今度は、データは、データベースから取得されます。</span><span class="sxs-lookup"><span data-stu-id="05ccb-321">There is no need to change the View template because the **StoreController** is returning the same **StoreIndexViewModel** as before, but this time the data will come from the database.</span></span>

1. <span data-ttu-id="05ccb-322">キーを押して、ソリューションを再構築**f5 キーを押して**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-322">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="05ccb-323">ホーム ページで、プロジェクトを開始します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-323">The project starts in the Home page.</span></span> <span data-ttu-id="05ccb-324">いることを確認のメニュー**ジャンル**ハードコードされた一覧で、できなくなり、データがデータベースから直接取得します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-324">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    <span data-ttu-id="05ccb-326">*データベースからジャンルをブラウズ*</span><span class="sxs-lookup"><span data-stu-id="05ccb-326">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="05ccb-327">ここで、ジャンルを参照し、アルバムがデータベースから生成されますを確認します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-327">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="05ccb-328">![データベースからアルバムを閲覧](aspnet-mvc-4-models-and-data-access/_static/image23.png "データベースからアルバムを参照")</span><span class="sxs-lookup"><span data-stu-id="05ccb-328">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image23.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="05ccb-329">*データベースからアルバムを参照*</span><span class="sxs-lookup"><span data-stu-id="05ccb-329">*Browsing Albums from the database*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a><span data-ttu-id="05ccb-330">手順 3: パラメーターを持つデータベースの照会</span><span class="sxs-lookup"><span data-stu-id="05ccb-330">Exercise 3: Querying the Database with Parameters</span></span>

<span data-ttu-id="05ccb-331">この演習では、パラメーターを使用してデータベースを照会する方法およびクエリ結果の整形を使用する方法を学習、番号のデータベースを小さく機能がより効率的な方法でのデータの取得をアクセスします。</span><span class="sxs-lookup"><span data-stu-id="05ccb-331">In this exercise, you will learn how to query the database using parameters, and how to use Query Result Shaping, a feature that reduces the number database accesses retrieving data in a more efficient way.</span></span>

> [!NOTE]
> <span data-ttu-id="05ccb-332">クエリ結果のシェイプの詳細については、次を参照してください。 [msdn の記事](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-332">For further information on Query Result Shaping, visit the following [msdn article](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span></span>


<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a><span data-ttu-id="05ccb-333">タスク 1 - 変更 StoreController データベースからアルバムを取得するには</span><span class="sxs-lookup"><span data-stu-id="05ccb-333">Task 1 - Modifying StoreController to Retrieve Albums from Database</span></span>

<span data-ttu-id="05ccb-334">このタスクでは、変更、 **StoreController**特定ジャンルからアルバムを取得するデータベースにアクセスするクラス。</span><span class="sxs-lookup"><span data-stu-id="05ccb-334">In this task, you will change the **StoreController** class to access the database to retrieve albums from a specific genre.</span></span>

1. <span data-ttu-id="05ccb-335">開く、**開始**ソリューションがある、 **Source\Ex3 QueryingTheDatabaseWithParametersCodeFirst\Begin**コード優先のアプローチを使用する場合はフォルダーまたは**Source\Ex3 QueryingTheDatabaseWithParametersDBFirst\Begin**データベース優先のアプローチを使用する場合はフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="05ccb-335">Open the **Begin** solution located at the **Source\Ex3-QueryingTheDatabaseWithParametersCodeFirst\Begin** folder if you want to use Code-First approach or **Source\Ex3-QueryingTheDatabaseWithParametersDBFirst\Begin** folder if you want to use Database-First approach.</span></span> <span data-ttu-id="05ccb-336">それ以外の場合、作業を続行できますを使用して、**終了**ソリューションは、前の手順を完了して取得します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-336">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="05ccb-337">指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。</span><span class="sxs-lookup"><span data-stu-id="05ccb-337">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="05ccb-338">これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-338">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="05ccb-339">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。</span><span class="sxs-lookup"><span data-stu-id="05ccb-339">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="05ccb-340">最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-340">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="05ccb-341">NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="05ccb-341">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="05ccb-342">NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="05ccb-342">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="05ccb-343">これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-343">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="05ccb-344">開く、 **StoreController**を変更するクラス、**参照**アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="05ccb-344">Open the **StoreController** class to change the **Browse** action method.</span></span> <span data-ttu-id="05ccb-345">これを行うには**ソリューション エクスプ ローラー**、展開、**コント ローラー**フォルダーをダブルクリック**StoreController.cs**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-345">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
3. <span data-ttu-id="05ccb-346">変更、**参照**アクション メソッドを特定のジャンルのアルバムを取得します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-346">Change the **Browse** action method to retrieve albums for a specific genre.</span></span> <span data-ttu-id="05ccb-347">これを行うには、次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="05ccb-347">To do this, replace the following code:</span></span>

    <span data-ttu-id="05ccb-348">(コード スニペットの*モデルおよびデータ アクセス - Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="05ccb-348">(Code Snippet - *Models And Data Access - Ex3 StoreController BrowseMethod*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> To populate a collection of the entity, you need to use the **Include** method to specify you want to retrieve the albums too. You can use the .**Single()** extension in LINQ because in this case only one genre is expected for an album. The **Single()** method takes a Lambda expression as a parameter, which in this case specifies a single Genre object such that its name matches the value defined.
> 
> You will take advantage of a feature that allows you to indicate other related entities you want loaded as well when the Genre object is retrieved. This feature is called **Query Result Shaping**, and enables you to reduce the number of times needed to access the database to retrieve information. In this scenario, you will want to pre-fetch the Albums for the Genre you retrieve.
> 
> The query includes **Genres.Include(&quot;Albums&quot;)** to indicate that you want related albums as well. This will result in a more efficient application, since it will retrieve both Genre and Album data in a single database request.
~~~

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="05ccb-349">タスク 2 - アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-349">Task 2 - Running the Application</span></span>

<span data-ttu-id="05ccb-350">このタスクでは、アプリケーションを実行し、特定のジャンルのアルバムをデータベースから取得します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-350">In this task, you will run the application and retrieve albums of a specific genre from the database.</span></span>

1. <span data-ttu-id="05ccb-351">キーを押して**f5 キーを押して**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-351">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="05ccb-352">ホーム ページで、プロジェクトを開始します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-352">The project starts in the Home page.</span></span> <span data-ttu-id="05ccb-353">URL を変更して**ストア/参照? ジャンル = Pop**結果は、データベースから取得されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-353">Change the URL to **/Store/Browse?genre=Pop** to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="05ccb-354">![ジャンル参照](aspnet-mvc-4-models-and-data-access/_static/image24.png "ジャンルの閲覧")</span><span class="sxs-lookup"><span data-stu-id="05ccb-354">![Browsing by genre](aspnet-mvc-4-models-and-data-access/_static/image24.png "Browsing by genre")</span></span>

    <span data-ttu-id="05ccb-355">*Browsing /Store/Browse?genre=Pop*</span><span class="sxs-lookup"><span data-stu-id="05ccb-355">*Browsing /Store/Browse?genre=Pop*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a><span data-ttu-id="05ccb-356">タスク 3 - Id でのアルバムへのアクセス</span><span class="sxs-lookup"><span data-stu-id="05ccb-356">Task 3 - Accessing Albums by Id</span></span>

<span data-ttu-id="05ccb-357">このタスクでは、アルバムの id を取得する前の手順を繰り返します</span><span class="sxs-lookup"><span data-stu-id="05ccb-357">In this task, you will repeat the previous procedure to get albums by their Id.</span></span>

1. <span data-ttu-id="05ccb-358">Visual Studio に戻るには、必要な場合は、ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="05ccb-358">Close the browser if needed, to return to Visual Studio.</span></span> <span data-ttu-id="05ccb-359">開く、 **StoreController**を変更するクラス、**詳細**アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="05ccb-359">Open the **StoreController** class to change the **Details** action method.</span></span> <span data-ttu-id="05ccb-360">これを行うには**ソリューション エクスプ ローラー**、展開、**コント ローラー**フォルダーをダブルクリック**StoreController.cs**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-360">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
2. <span data-ttu-id="05ccb-361">変更、**詳細**アルバムの詳細を取得するアクション メソッドがに基づいて、 **Id**です。これを行うには、次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="05ccb-361">Change the **Details** action method to retrieve albums details based on their **Id**. To do this, replace the following code:</span></span>

    <span data-ttu-id="05ccb-362">(コード スニペットの*モデルおよびデータ アクセス - Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="05ccb-362">(Code Snippet - *Models And Data Access - Ex3 StoreController DetailsMethod*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]
~~~

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="05ccb-363">タスク 4 - アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-363">Task 4 - Running the Application</span></span>

<span data-ttu-id="05ccb-364">このタスクでは、web ブラウザーでアプリケーションを実行し、その id でアルバムの詳細の取得</span><span class="sxs-lookup"><span data-stu-id="05ccb-364">In this task, you will run the Application in a web browser and obtain album details by their Id.</span></span>

1. <span data-ttu-id="05ccb-365">キーを押して**f5 キーを押して**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-365">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="05ccb-366">ホーム ページで、プロジェクトを開始します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-366">The project starts in the Home page.</span></span> <span data-ttu-id="05ccb-367">URL を変更して**/Store/Details/51**か、ジャンルを参照し、結果をデータベースから取得することを確認するアルバムを選択します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-367">Change the URL to **/Store/Details/51** or browse the genres and select an album to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="05ccb-368">![詳細情報を閲覧](aspnet-mvc-4-models-and-data-access/_static/image25.png "の詳細を参照")</span><span class="sxs-lookup"><span data-stu-id="05ccb-368">![Browsing Details](aspnet-mvc-4-models-and-data-access/_static/image25.png "Browsing Details")</span></span>

    <span data-ttu-id="05ccb-369">*/Store/Details/51 をブラウズ*</span><span class="sxs-lookup"><span data-stu-id="05ccb-369">*Browsing /Store/Details/51*</span></span>

> [!NOTE]
> <span data-ttu-id="05ccb-370">Windows Azure Web サイトの次にこのアプリケーションを展開するさらに、[付録 b: 公開 Web Deploy を使用して ASP.NET MVC 4 アプリケーション](#AppendixB)です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-370">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="05ccb-371">まとめ</span><span class="sxs-lookup"><span data-stu-id="05ccb-371">Summary</span></span>

<span data-ttu-id="05ccb-372">ASP.NET MVC モデルおよびデータ アクセスの基礎を学習したこのハンズオン ラボを完了するを使用して、 **Database First**方法だけでなく**Code First**方法。</span><span class="sxs-lookup"><span data-stu-id="05ccb-372">By completing this Hands-on Lab you have learned the fundamentals of ASP.NET MVC Models and Data Access, using the **Database First** approach as well as the **Code First** Approach:</span></span>

- <span data-ttu-id="05ccb-373">そのデータを使用するために、ソリューションにデータベースを追加する方法</span><span class="sxs-lookup"><span data-stu-id="05ccb-373">How to add a database to the solution in order to consume its data</span></span>
- <span data-ttu-id="05ccb-374">テンプレートの表示をハード コーディングされた 1 つではなく、データベースから取得したデータを提供するコント ローラーを更新する方法</span><span class="sxs-lookup"><span data-stu-id="05ccb-374">How to update Controllers to provide View templates with the data taken from the database instead of hard-coded one</span></span>
- <span data-ttu-id="05ccb-375">パラメーターを使用してデータベースを照会する方法</span><span class="sxs-lookup"><span data-stu-id="05ccb-375">How to query the database using parameters</span></span>
- <span data-ttu-id="05ccb-376">クエリ結果の整形、データベースへのアクセスの数を削減する機能より効率的な方法でデータの取得を使用する方法</span><span class="sxs-lookup"><span data-stu-id="05ccb-376">How to use the Query Result Shaping, a feature that reduces the number of database accesses, retrieving data in a more efficient way</span></span>
- <span data-ttu-id="05ccb-377">Microsoft Entity Framework での Database First と Code First の両方のアプローチを使用して、モデルとデータベースをリンクする方法</span><span class="sxs-lookup"><span data-stu-id="05ccb-377">How to use both Database First and Code First approaches in Microsoft Entity Framework to link the database with the model</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="05ccb-378">付録 a: をインストールする Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="05ccb-378">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="05ccb-379">インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、 **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="05ccb-379">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="05ccb-380">次の手順を案内するインストールに必要な手順*Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-380">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="05ccb-381">移動して[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-381">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="05ccb-382">または、既に Web Platform Installer をインストールした場合、開くことができ、製品を検索&quot; <em>Visual Studio Express 2012 for Web と Windows Azure SDK</em>&quot;です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-382">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="05ccb-383">をクリックして**を今すぐインストール**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-383">Click on **Install Now**.</span></span> <span data-ttu-id="05ccb-384">ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="05ccb-384">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="05ccb-385">1 回**Web Platform Installer**が開いて、をクリックして**インストール**セットアップを開始します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-385">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="05ccb-386">![Visual Studio Express インストール](aspnet-mvc-4-models-and-data-access/_static/image26.png "Visual Studio Express のインストール")</span><span class="sxs-lookup"><span data-stu-id="05ccb-386">![Install Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="05ccb-387">*Visual Studio Express をインストールします。*</span><span class="sxs-lookup"><span data-stu-id="05ccb-387">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="05ccb-388">すべての製品のライセンスと条項を読み、クリックして**同意**を続行します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-388">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![ライセンス条項に同意](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    <span data-ttu-id="05ccb-390">*ライセンス条項に同意*</span><span class="sxs-lookup"><span data-stu-id="05ccb-390">*Accepting the license terms*</span></span>
5. <span data-ttu-id="05ccb-391">ダウンロードとインストール プロセスが完了するまで待機します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-391">Wait until the downloading and installation process completes.</span></span>

    ![インストールの進行状況](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    <span data-ttu-id="05ccb-393">*インストールの進行状況*</span><span class="sxs-lookup"><span data-stu-id="05ccb-393">*Installation progress*</span></span>
6. <span data-ttu-id="05ccb-394">インストールが完了したらをクリックして**完了**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-394">When the installation completes, click **Finish**.</span></span>

    ![インストールが完了しました](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    <span data-ttu-id="05ccb-396">*インストールが完了しました*</span><span class="sxs-lookup"><span data-stu-id="05ccb-396">*Installation completed*</span></span>
7. <span data-ttu-id="05ccb-397">をクリックして**終了**Web Platform Installer を閉じます。</span><span class="sxs-lookup"><span data-stu-id="05ccb-397">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="05ccb-398">Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、順にクリックして、 **VS Express for Web**並べて表示します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-398">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express Web タイルを](aspnet-mvc-4-models-and-data-access/_static/image30.png)

    <span data-ttu-id="05ccb-400">*VS Express Web タイルを*</span><span class="sxs-lookup"><span data-stu-id="05ccb-400">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="05ccb-401">付録 b: Web Deploy を使用して ASP.NET MVC 4 アプリケーションを発行します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-401">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="05ccb-402">この付録では、Windows Azure 管理ポータルから新しい web サイトを作成し、Windows Azure によって提供される Web 配置発行機能を活用して、次の演習では、取得したアプリケーションを公開する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-402">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="05ccb-403">タスク 1 - Windows から新しい Web サイトを作成する Azure ポータル</span><span class="sxs-lookup"><span data-stu-id="05ccb-403">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="05ccb-404">移動して、 [Windows Azure 管理ポータル](https://manage.windowsazure.com/)サブスクリプションに関連付けられている Microsoft の資格情報を使用してサインインします。</span><span class="sxs-lookup"><span data-stu-id="05ccb-404">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="05ccb-405">Windows Azure 無料で 10 個の ASP.NET Web サイトをホストでき、トラフィックの増大に応じて拡大縮小できます。</span><span class="sxs-lookup"><span data-stu-id="05ccb-405">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="05ccb-406">サインアップする[ここ](http://aka.ms/aspnet-hol-azure)です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-406">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="05ccb-407">![Windows Azure ポータルにログオンする](aspnet-mvc-4-models-and-data-access/_static/image31.png "Windows Azure ポータルにログオンします。")</span><span class="sxs-lookup"><span data-stu-id="05ccb-407">![Log on to Windows Azure portal](aspnet-mvc-4-models-and-data-access/_static/image31.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="05ccb-408">*Windows Azure 管理ポータルにログオンします。*</span><span class="sxs-lookup"><span data-stu-id="05ccb-408">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="05ccb-409">をクリックして**新規**コマンド バーでします。</span><span class="sxs-lookup"><span data-stu-id="05ccb-409">Click **New** on the command bar.</span></span>

    <span data-ttu-id="05ccb-410">![新しい Web サイトを作成する](aspnet-mvc-4-models-and-data-access/_static/image32.png "新しい Web サイトを作成します。")</span><span class="sxs-lookup"><span data-stu-id="05ccb-410">![Creating a new Web Site](aspnet-mvc-4-models-and-data-access/_static/image32.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="05ccb-411">*新しい Web サイトを作成します。*</span><span class="sxs-lookup"><span data-stu-id="05ccb-411">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="05ccb-412">をクリックして**コンピューティング** | **Web サイト**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-412">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="05ccb-413">選択し、**簡易作成**オプション。</span><span class="sxs-lookup"><span data-stu-id="05ccb-413">Then select **Quick Create** option.</span></span> <span data-ttu-id="05ccb-414">新しい web サイトの利用可能な URL を指定して、をクリックして**Web サイトの作成**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-414">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="05ccb-415">Windows Azure Web サイトは、制御および管理できる、クラウドで実行されている web アプリケーションのホストです。</span><span class="sxs-lookup"><span data-stu-id="05ccb-415">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="05ccb-416">簡易作成 オプションでは、完成した web アプリケーションを Windows Azure Web サイトからポータル外を展開することができます。</span><span class="sxs-lookup"><span data-stu-id="05ccb-416">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="05ccb-417">データベースを設定するための手順は含まれません。</span><span class="sxs-lookup"><span data-stu-id="05ccb-417">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="05ccb-418">![簡易作成を使用して新しい Web サイトを作成する](aspnet-mvc-4-models-and-data-access/_static/image33.png "簡易作成を使用して新しい Web サイトを作成します。")</span><span class="sxs-lookup"><span data-stu-id="05ccb-418">![Creating a new Web Site using Quick Create](aspnet-mvc-4-models-and-data-access/_static/image33.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="05ccb-419">*簡易作成を使用して新しい Web サイトを作成します。*</span><span class="sxs-lookup"><span data-stu-id="05ccb-419">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="05ccb-420">新しいまで待つ**Web サイト**を作成します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-420">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="05ccb-421">Web サイトを作成した後は、下のリンクをクリックして、 **URL**列です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-421">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="05ccb-422">新しい Web サイトが動作していることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="05ccb-422">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="05ccb-423">![新しい web サイトを参照して](aspnet-mvc-4-models-and-data-access/_static/image34.png "新しい web サイトを参照")</span><span class="sxs-lookup"><span data-stu-id="05ccb-423">![Browsing to the new web site](aspnet-mvc-4-models-and-data-access/_static/image34.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="05ccb-424">*新しい web サイトを参照*</span><span class="sxs-lookup"><span data-stu-id="05ccb-424">*Browsing to the new web site*</span></span>

    <span data-ttu-id="05ccb-425">![Web サイトを実行している](aspnet-mvc-4-models-and-data-access/_static/image35.png "を実行している Web サイト")</span><span class="sxs-lookup"><span data-stu-id="05ccb-425">![Web site running](aspnet-mvc-4-models-and-data-access/_static/image35.png "Web site running")</span></span>

    <span data-ttu-id="05ccb-426">*実行している web サイト*</span><span class="sxs-lookup"><span data-stu-id="05ccb-426">*Web site running*</span></span>
6. <span data-ttu-id="05ccb-427">ポータルに戻るし、下にある web サイトの名前をクリックして、**名前**管理ページを表示する列。</span><span class="sxs-lookup"><span data-stu-id="05ccb-427">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="05ccb-428">![Web サイトの管理ページを開く](aspnet-mvc-4-models-and-data-access/_static/image36.png "web サイトの管理ページを開く")</span><span class="sxs-lookup"><span data-stu-id="05ccb-428">![Opening the web site management pages](aspnet-mvc-4-models-and-data-access/_static/image36.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="05ccb-429">*Web サイトの管理ページを開く*</span><span class="sxs-lookup"><span data-stu-id="05ccb-429">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="05ccb-430">**ダッシュ ボード**] ページの [、**クイック グランス**セクションで、をクリックして、**発行プロファイルのダウンロード**リンクします。</span><span class="sxs-lookup"><span data-stu-id="05ccb-430">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="05ccb-431">*発行プロファイル*すべてのパブリケーションを有効になっているメソッドの各 Windows Azure web サイトに web アプリケーションを発行するために必要な情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="05ccb-431">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="05ccb-432">発行プロファイルには、Url、ユーザーの資格情報およびに接続し、各発行メソッドが有効になっているエンドポイントに対して認証するために必要なデータベース文字列が含まれています。</span><span class="sxs-lookup"><span data-stu-id="05ccb-432">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="05ccb-433">**Microsoft WebMatrix 2**、 **Microsoft Visual Studio Express for Web**と**Microsoft Visual Studio 2012**読み取りのサポートをこれらのプログラムの構成を自動化するプロファイルの発行Windows Azure の web サイトに web アプリケーションを公開します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-433">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="05ccb-434">![発行プロファイルを web サイトをダウンロードする](aspnet-mvc-4-models-and-data-access/_static/image37.png "発行プロファイルを web サイトをダウンロードします。")</span><span class="sxs-lookup"><span data-stu-id="05ccb-434">![Downloading the web site publish profile](aspnet-mvc-4-models-and-data-access/_static/image37.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="05ccb-435">*発行プロファイルを Web サイトをダウンロードします。*</span><span class="sxs-lookup"><span data-stu-id="05ccb-435">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="05ccb-436">既知の場所に発行プロファイル ファイルをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="05ccb-436">Download the publish profile file to a known location.</span></span> <span data-ttu-id="05ccb-437">さらにこの練習では、このファイルを使用して、Visual Studio から web アプリケーション、Windows Azure Web サイトを発行する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="05ccb-437">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="05ccb-438">![発行プロファイル ファイルを保存](aspnet-mvc-4-models-and-data-access/_static/image38.png "発行プロファイルの保存")</span><span class="sxs-lookup"><span data-stu-id="05ccb-438">![Saving the publish profile file](aspnet-mvc-4-models-and-data-access/_static/image38.png "Saving the publish profile")</span></span>

    <span data-ttu-id="05ccb-439">*発行プロファイル ファイルを保存します。*</span><span class="sxs-lookup"><span data-stu-id="05ccb-439">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="05ccb-440">タスク 2 - データベース サーバーの構成</span><span class="sxs-lookup"><span data-stu-id="05ccb-440">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="05ccb-441">アプリケーションが SQL Server を使用する場合は、データベースの SQL データベース サーバーを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="05ccb-441">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="05ccb-442">SQL Server を使用しない簡単なアプリケーションを展開する場合は、このタスクをスキップする場合があります。</span><span class="sxs-lookup"><span data-stu-id="05ccb-442">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="05ccb-443">SQL データベース サーバーは、アプリケーション データベースを格納する必要があります。</span><span class="sxs-lookup"><span data-stu-id="05ccb-443">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="05ccb-444">SQL データベース サーバーを表示するには、Windows Azure 管理ポータルで自分のサブスクリプションから**Sql データベース** | **サーバー** | **サーバーのダッシュ ボード**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-444">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="05ccb-445">使用して 1 つを作成するには作成されたサーバーがない、**追加**コマンド バーのボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="05ccb-445">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="05ccb-446">注意してください、**サーバー名および URL、管理者のログイン名とパスワード**、次のタスクで使用することができます。</span><span class="sxs-lookup"><span data-stu-id="05ccb-446">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="05ccb-447">データベースを作成しない、まだと後の段階で作成されます。</span><span class="sxs-lookup"><span data-stu-id="05ccb-447">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="05ccb-448">![SQL データベース サーバーのダッシュ ボード](aspnet-mvc-4-models-and-data-access/_static/image39.png "SQL データベース サーバーのダッシュ ボード")</span><span class="sxs-lookup"><span data-stu-id="05ccb-448">![SQL Database Server Dashboard](aspnet-mvc-4-models-and-data-access/_static/image39.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="05ccb-449">*SQL データベース サーバーのダッシュ ボード*</span><span class="sxs-lookup"><span data-stu-id="05ccb-449">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="05ccb-450">次のタスクでは、サーバーの一覧に、ローカル IP アドレスを含める必要があるため、Visual Studio からデータベース接続をテストします**使用できる IP アドレス**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-450">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="05ccb-451">実行するには、クリックして**構成**から IP アドレスを選択する**現在のクライアント IP アドレス**に貼り付けます、**開始 IP アドレス**と**終了IPアドレス**のテキスト ボックスをクリックして、 ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png)ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="05ccb-451">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) button.</span></span>

    ![クライアントの IP アドレスを追加します。](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    <span data-ttu-id="05ccb-453">*クライアントの IP アドレスを追加します。*</span><span class="sxs-lookup"><span data-stu-id="05ccb-453">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="05ccb-454">1 回、**クライアント IP アドレス**が許可される IP アドレスに追加一覧で、をクリックして**保存**変更を確認します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-454">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![変更を確認します。](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    <span data-ttu-id="05ccb-456">*変更を確認します。*</span><span class="sxs-lookup"><span data-stu-id="05ccb-456">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="05ccb-457">タスク 3 - Web Deploy を使用して ASP.NET MVC 4 アプリケーションの発行</span><span class="sxs-lookup"><span data-stu-id="05ccb-457">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="05ccb-458">ASP.NET MVC 4 ソリューションに戻ります。</span><span class="sxs-lookup"><span data-stu-id="05ccb-458">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="05ccb-459">**ソリューション エクスプ ローラー**を web サイト プロジェクトを右クリックし、**発行**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-459">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="05ccb-460">![アプリケーションの発行](aspnet-mvc-4-models-and-data-access/_static/image43.png "アプリケーションの発行")</span><span class="sxs-lookup"><span data-stu-id="05ccb-460">![Publishing the Application](aspnet-mvc-4-models-and-data-access/_static/image43.png "Publishing the Application")</span></span>

    <span data-ttu-id="05ccb-461">*Web サイトの発行*</span><span class="sxs-lookup"><span data-stu-id="05ccb-461">*Publishing the web site*</span></span>
2. <span data-ttu-id="05ccb-462">最初の作業を保存した発行プロファイルをインポートします。</span><span class="sxs-lookup"><span data-stu-id="05ccb-462">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="05ccb-463">![発行プロファイルをインポートする](aspnet-mvc-4-models-and-data-access/_static/image44.png "発行プロファイルをインポートします。")</span><span class="sxs-lookup"><span data-stu-id="05ccb-463">![Importing the publish profile](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importing the publish profile")</span></span>

    <span data-ttu-id="05ccb-464">*発行プロファイルのインポート*</span><span class="sxs-lookup"><span data-stu-id="05ccb-464">*Importing publish profile*</span></span>
3. <span data-ttu-id="05ccb-465">をクリックして**への接続検証**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-465">Click **Validate Connection**.</span></span> <span data-ttu-id="05ccb-466">検証が完了したらクリックして**次**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-466">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="05ccb-467">検証が完了すると、接続の検証 ボタンの横に表示される緑色のチェック マークを参照してください。</span><span class="sxs-lookup"><span data-stu-id="05ccb-467">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="05ccb-468">![接続の検証](aspnet-mvc-4-models-and-data-access/_static/image45.png "接続の検証")</span><span class="sxs-lookup"><span data-stu-id="05ccb-468">![Validating connection](aspnet-mvc-4-models-and-data-access/_static/image45.png "Validating connection")</span></span>

    <span data-ttu-id="05ccb-469">*接続の検証*</span><span class="sxs-lookup"><span data-stu-id="05ccb-469">*Validating connection*</span></span>
4. <span data-ttu-id="05ccb-470">**設定**] ページの [、**データベース**セクションで、データベース接続のテキスト ボックスの横にあるボタンをクリックして (つまり**DefaultConnection**)。</span><span class="sxs-lookup"><span data-stu-id="05ccb-470">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="05ccb-471">![Web 配置の構成](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web 配置の構成")</span><span class="sxs-lookup"><span data-stu-id="05ccb-471">![Web deploy configuration](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web deploy configuration")</span></span>

    <span data-ttu-id="05ccb-472">*Web 配置の構成*</span><span class="sxs-lookup"><span data-stu-id="05ccb-472">*Web deploy configuration*</span></span>
5. <span data-ttu-id="05ccb-473">次のように、データベースの接続を構成します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-473">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="05ccb-474">**サーバー名**SQL データベース サーバーの URL を使用して、入力、 *tcp:*プレフィックス。</span><span class="sxs-lookup"><span data-stu-id="05ccb-474">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="05ccb-475">**ユーザー名**サーバー管理者のログイン名を入力します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-475">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="05ccb-476">**パスワード**サーバー管理者のログイン パスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-476">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="05ccb-477">新しいデータベース名を入力します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-477">Type a new database name.</span></span>

     <span data-ttu-id="05ccb-478">![対象の接続文字列を構成する](aspnet-mvc-4-models-and-data-access/_static/image47.png "対象の接続文字列を構成します。")</span><span class="sxs-lookup"><span data-stu-id="05ccb-478">![Configuring destination connection string](aspnet-mvc-4-models-and-data-access/_static/image47.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="05ccb-479">*対象の接続文字列を構成します。*</span><span class="sxs-lookup"><span data-stu-id="05ccb-479">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="05ccb-480">次に、 **[OK]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="05ccb-480">Then click **OK**.</span></span> <span data-ttu-id="05ccb-481">データベースをクリックを作成するように求められたら**はい**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-481">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="05ccb-482">![データベースを作成する](aspnet-mvc-4-models-and-data-access/_static/image48.png "データベース文字列を作成します。")</span><span class="sxs-lookup"><span data-stu-id="05ccb-482">![Creating the database](aspnet-mvc-4-models-and-data-access/_static/image48.png "Creating the database string")</span></span>

    <span data-ttu-id="05ccb-483">*データベースの作成*</span><span class="sxs-lookup"><span data-stu-id="05ccb-483">*Creating the database*</span></span>
7. <span data-ttu-id="05ccb-484">接続の既定のテキスト ボックス内では、Windows azure SQL データベースへの接続に使用する接続文字列が表示されます。</span><span class="sxs-lookup"><span data-stu-id="05ccb-484">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="05ccb-485">その後、 **[次へ]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="05ccb-485">Then click **Next**.</span></span>

    <span data-ttu-id="05ccb-486">![SQL データベースを指す接続文字列](aspnet-mvc-4-models-and-data-access/_static/image49.png "SQL データベースを指す接続文字列")</span><span class="sxs-lookup"><span data-stu-id="05ccb-486">![Connection string pointing to SQL Database](aspnet-mvc-4-models-and-data-access/_static/image49.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="05ccb-487">*SQL データベースを指す接続文字列*</span><span class="sxs-lookup"><span data-stu-id="05ccb-487">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="05ccb-488">**プレビュー** ] ページで [**発行**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-488">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="05ccb-489">![Web アプリケーションの発行](aspnet-mvc-4-models-and-data-access/_static/image50.png "web アプリケーションの発行")</span><span class="sxs-lookup"><span data-stu-id="05ccb-489">![Publishing the web application](aspnet-mvc-4-models-and-data-access/_static/image50.png "Publishing the web application")</span></span>

    <span data-ttu-id="05ccb-490">*Web アプリケーションの発行*</span><span class="sxs-lookup"><span data-stu-id="05ccb-490">*Publishing the web application*</span></span>
9. <span data-ttu-id="05ccb-491">発行プロセスが終了すると、既定のブラウザーが公開された web サイトを開きます。</span><span class="sxs-lookup"><span data-stu-id="05ccb-491">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="05ccb-492">付録 c: コード スニペットの使用</span><span class="sxs-lookup"><span data-stu-id="05ccb-492">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="05ccb-493">コード スニペットでは、すぐに利用できる必要があるすべてのコードがあります。</span><span class="sxs-lookup"><span data-stu-id="05ccb-493">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="05ccb-494">ラボ ドキュメントがわかります正確に使用する場合が、次の図に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="05ccb-494">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="05ccb-495">![Visual Studio のコード スニペットを使用して、プロジェクトにコードを挿入する](aspnet-mvc-4-models-and-data-access/_static/image51.png "プロジェクトにコードを挿入する Visual Studio を使ってコード スニペット")</span><span class="sxs-lookup"><span data-stu-id="05ccb-495">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-models-and-data-access/_static/image51.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="05ccb-496">*Visual Studio のコード スニペットを使用して、プロジェクトにコードを挿入するには*</span><span class="sxs-lookup"><span data-stu-id="05ccb-496">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="05ccb-497">***キーボード (C# の場合のみ) を使用してコード スニペットを追加するには***</span><span class="sxs-lookup"><span data-stu-id="05ccb-497">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="05ccb-498">コードを挿入するには、カーソルを置きます。</span><span class="sxs-lookup"><span data-stu-id="05ccb-498">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="05ccb-499">(なし、スペースやハイフン) スニペット名を入力してを起動します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-499">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="05ccb-500">スニペットの名前に一致する IntelliSense 表示を確認します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-500">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="05ccb-501">正しいスニペットを選択 (または、全体のスニペットの名前を選択するまで」と入力してください)。</span><span class="sxs-lookup"><span data-stu-id="05ccb-501">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="05ccb-502">カーソル位置にスニペットを挿入するには、2 回、Tab キーを押します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-502">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="05ccb-503">![スニペット名を入力する開始](aspnet-mvc-4-models-and-data-access/_static/image52.png "スニペット名の入力を開始")</span><span class="sxs-lookup"><span data-stu-id="05ccb-503">![Start typing the snippet name](aspnet-mvc-4-models-and-data-access/_static/image52.png "Start typing the snippet name")</span></span>

<span data-ttu-id="05ccb-504">*スニペット名の入力を開始します。*</span><span class="sxs-lookup"><span data-stu-id="05ccb-504">*Start typing the snippet name*</span></span>

<span data-ttu-id="05ccb-505">![Tab キーを押して、強調表示されているスニペットを選択する](aspnet-mvc-4-models-and-data-access/_static/image53.png "強調表示されているスニペットを選択するキーを押してタブ")</span><span class="sxs-lookup"><span data-stu-id="05ccb-505">![Press Tab to select the highlighted snippet](aspnet-mvc-4-models-and-data-access/_static/image53.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="05ccb-506">*Tab キーを押して、強調表示されているスニペットを選択するには*</span><span class="sxs-lookup"><span data-stu-id="05ccb-506">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="05ccb-507">![キーを押して タブで再度と、スニペットが展開](aspnet-mvc-4-models-and-data-access/_static/image54.png "キーを押して タブで再度と、スニペットが展開されます")</span><span class="sxs-lookup"><span data-stu-id="05ccb-507">![Press Tab again and the snippet will expand](aspnet-mvc-4-models-and-data-access/_static/image54.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="05ccb-508">*キーを押して タブで再度と、スニペットが展開されます。*</span><span class="sxs-lookup"><span data-stu-id="05ccb-508">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="05ccb-509">***(C#、Visual Basic および XML) にマウスを使用してコード スニペットを追加する***1 です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-509">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="05ccb-510">コード スニペットを挿入する場所を右クリックします。</span><span class="sxs-lookup"><span data-stu-id="05ccb-510">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="05ccb-511">選択**スニペットの挿入**続く**マイ コード スニペット**です。</span><span class="sxs-lookup"><span data-stu-id="05ccb-511">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="05ccb-512">クリックして一覧から、関連するスニペットを選択します。</span><span class="sxs-lookup"><span data-stu-id="05ccb-512">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="05ccb-513">![コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリックして](aspnet-mvc-4-models-and-data-access/_static/image55.png "コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリック")</span><span class="sxs-lookup"><span data-stu-id="05ccb-513">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-models-and-data-access/_static/image55.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="05ccb-514">*コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリックします。*</span><span class="sxs-lookup"><span data-stu-id="05ccb-514">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="05ccb-515">![クリックして一覧から、関連するスニペットを選択](aspnet-mvc-4-models-and-data-access/_static/image56.png "クリックして一覧から、関連するスニペットを選択")</span><span class="sxs-lookup"><span data-stu-id="05ccb-515">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-models-and-data-access/_static/image56.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="05ccb-516">*クリックして一覧から、関連するスニペットを選択します。*</span><span class="sxs-lookup"><span data-stu-id="05ccb-516">*Pick the relevant snippet from the list, by clicking on it*</span></span>
