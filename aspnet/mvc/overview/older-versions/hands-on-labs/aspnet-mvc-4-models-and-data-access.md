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
ms.openlocfilehash: 88b3316b116962dd35031f4b971dbfe31ed0e010
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/18/2018
ms.locfileid: "34306739"
---
# <a name="aspnet-mvc-4-models-and-data-access"></a><span data-ttu-id="eaa1c-104">ASP.NET MVC 4 モデルおよびデータ アクセス</span><span class="sxs-lookup"><span data-stu-id="eaa1c-104">ASP.NET MVC 4 Models and Data Access</span></span>

<span data-ttu-id="eaa1c-105">によって[Web キャンプ チーム](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="eaa1c-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="eaa1c-106">Web キャンプ トレーニング キットをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="eaa1c-107">このハンズオン ラボは、の基本的な知識がある前提としています。 **ASP.NET MVC**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-107">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="eaa1c-108">使用していない場合**ASP.NET MVC**経由で移動する前をお勧め**ASP.NET MVC 4 基礎**ハンズオン ラボ。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-108">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="eaa1c-109">このラボには、機能強化と軽微な変更を元のフォルダーで提供されるサンプル Web アプリケーションに適用することで前に説明した新しい機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-109">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="eaa1c-110">すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[Microsoft の Web/WebCampTrainingKit リリース](https://aka.ms/webcamps-training-kit)です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-110">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="eaa1c-111">このラボに固有のプロジェクトは[ASP.NET MVC 4 モデルおよびデータ アクセス](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess)です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-111">The project specific to this lab is available at [ASP.NET MVC 4 Models and Data Access](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).</span></span>

<span data-ttu-id="eaa1c-112">**ASP.NET MVC の基本事項**ハンズオン ラボがされて渡そうとしてハード コードされたデータのコント ローラーからテンプレートの表示にします。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-112">In **ASP.NET MVC Fundamentals** Hands-on Lab, you have been passing hard-coded data from the Controllers to the View templates.</span></span> <span data-ttu-id="eaa1c-113">しかし、実際の Web アプリケーションを構築するために実際のデータベースを使用する場合があります。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-113">But, in order to build a real Web application, you might want to use a real database.</span></span>

<span data-ttu-id="eaa1c-114">このハンズオン ラボでは、格納および音楽ストア アプリケーションに必要なデータを取得するためにデータベース エンジンを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-114">This Hands-on Lab will show you how to use a database engine in order to store and retrieve the data needed for the Music Store application.</span></span> <span data-ttu-id="eaa1c-115">そのため、既存のデータベースで開始してから、Entity Data Model を作成します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-115">To accomplish that, you will start with an existing database and create the Entity Data Model from it.</span></span> <span data-ttu-id="eaa1c-116">この演習では、全体で満たすことになります、 **Database First**方法だけでなく**Code First**アプローチです。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-116">Throughout this lab, you will meet the **Database First** approach as well as the **Code First** approach.</span></span>

<span data-ttu-id="eaa1c-117">ただし、使用することできますも、 **Model First**アプローチ、ツールを使用して、同じモデルの作成、およびそこから、データベースを生成します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-117">However, you can also use the **Model First** approach, create the same model using the tools, and then generate the database from it.</span></span>

<span data-ttu-id="eaa1c-118">![データベースの最初とします。最初のモデル](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First vs です。最初のモデルします。")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-118">![Database First vs. Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First vs. Model First")</span></span>

<span data-ttu-id="eaa1c-119">*データベースの最初とします。最初のモデルします。*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-119">*Database First vs. Model First*</span></span>

<span data-ttu-id="eaa1c-120">モデルを生成した後にハード コードされたデータを使用する代わりに、データベースから取得されたデータ ストアのビューを提供する StoreController で適切な調整が作成されます。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-120">After generating the Model, you will make the proper adjustments in the StoreController to provide the Store Views with the data taken from the database, instead of using hard-coded data.</span></span> <span data-ttu-id="eaa1c-121">変更するビュー テンプレート ビュー テンプレートに同じ ViewModels が戻される、StoreController のためこの時点のデータは、データベースから取得されますが、する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-121">You will not need to make any change to the View templates because the StoreController will be returning the same ViewModels to the View templates, although this time the data will come from the database.</span></span>

<span data-ttu-id="eaa1c-122">**コードの最初の方法**</span><span class="sxs-lookup"><span data-stu-id="eaa1c-122">**The Code First Approach**</span></span>

<span data-ttu-id="eaa1c-123">コードの最初の方法では、フレームワークと組み合わせると、通常のクラスを生成せず、コードからモデルを定義できます。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-123">The Code First approach allows us to define the model from the code without generating classes that are generally coupled with the framework.</span></span>

<span data-ttu-id="eaa1c-124">コードで最初に、モデル オブジェクトで定義した poco から、 &quot;Plain Old CLR Object&quot;です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-124">In code first, model objects are defined with POCOs, &quot;Plain Old CLR Objects&quot;.</span></span> <span data-ttu-id="eaa1c-125">した poco からは、単純なの plain クラスを継承がないインターフェイスを実装していないです。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-125">POCOs are simple plain classes that have no inheritance and do not implement interfaces.</span></span> <span data-ttu-id="eaa1c-126">これらのファイルからデータベースを自動的に生成することができます、または既存のデータベースを使用して、コードから、クラスへのマッピングを生成します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-126">We can automatically generate the database from them, or we can use an existing database and generate the class mapping from the code.</span></span>

<span data-ttu-id="eaa1c-127">この方法を使用する利点は、すること、モデルを引き続き (ここでは、Entity Framework)、永続化フレームワークから独立した poco からクラスは、マッピング framework と関連していないようです。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-127">The benefits of using this approach is that the Model remains independent from the persistence framework (in this case, Entity Framework), as the POCOs classes are not coupled with the mapping framework.</span></span>

> [!NOTE]
> <span data-ttu-id="eaa1c-128">このラボは ASP.NET MVC 4 に基づいており、音楽ストア サンプル アプリケーションのバージョンがカスタマイズされ、このハンズオン ラボで示されている機能のみに合わせて最小限に抑えられます。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-128">This Lab is based on ASP.NET MVC 4 and a version of the Music Store sample application customized and minimized to fit only the features shown in this Hands-On Lab.</span></span>
> 
> <span data-ttu-id="eaa1c-129">全体を調査したい場合**Music Store**で検索できるチュートリアル アプリケーション[MVC Music Store](https://github.com/evilDave/MVC-Music-Store)です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-129">If you wish to explore the whole **Music Store** tutorial application you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="eaa1c-130">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="eaa1c-130">Prerequisites</span></span>

<span data-ttu-id="eaa1c-131">このラボを完成させるのには、次の項目が必要です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-131">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="eaa1c-132">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)または上位 (読み取り[付録 A](#AppendixA)をインストールする方法について)。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-132">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="eaa1c-133">セットアップ</span><span class="sxs-lookup"><span data-stu-id="eaa1c-133">Setup</span></span>

<span data-ttu-id="eaa1c-134">**コード スニペットをインストールします。**</span><span class="sxs-lookup"><span data-stu-id="eaa1c-134">**Installing Code Snippets**</span></span>

<span data-ttu-id="eaa1c-135">便宜上、このラボに沿ったを管理するコードの多くは、Visual Studio のコード スニペットとして利用できます。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-135">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="eaa1c-136">実行のコード スニペットをインストールする **.\Source\Setup\CodeSnippets.vsi**ファイル。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-136">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="eaa1c-137">このドキュメントの付録を参照することができます、Visual Studio のコード スニペットとその使用方法を学習するに慣れていない場合&quot;[付録 c: を使用してコード スニペット](#AppendixC)&quot;です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-137">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="eaa1c-138">演習</span><span class="sxs-lookup"><span data-stu-id="eaa1c-138">Exercises</span></span>

<span data-ttu-id="eaa1c-139">次の演習では、このハンズオン ラボから成ります。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-139">This Hands-on Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="eaa1c-140">手順 1: データベースに追加します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-140">Exercise 1: Adding a Database</span></span>](#Exercise1)
2. [<span data-ttu-id="eaa1c-141">手順 2: Code First を使用してデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-141">Exercise 2: Creating a Database using Code First</span></span>](#Exercise2)
3. [<span data-ttu-id="eaa1c-142">手順 3: パラメーターを持つデータベースの照会</span><span class="sxs-lookup"><span data-stu-id="eaa1c-142">Exercise 3: Querying the Database with Parameters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="eaa1c-143">各演習が組み込まれた、**終了**演習を完了した後に取得する必要があります、結果として得られるソリューションを含むフォルダー。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-143">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="eaa1c-144">演習では操作のヘルプを参照する必要がある場合は、このソリューションをガイドとして使用できます。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-144">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="eaa1c-145">この演習を完了する時間を推定: **35 分**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-145">Estimated time to complete this lab: **35 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a><span data-ttu-id="eaa1c-146">手順 1: データベースに追加します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-146">Exercise 1: Adding a Database</span></span>

<span data-ttu-id="eaa1c-147">この演習では、そのデータを使用するために、ソリューションに MusicStore アプリケーションのテーブルを持つデータベースを追加する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-147">In this exercise, you will learn how to add a database with the tables of the MusicStore application to the solution in order to consume its data.</span></span> <span data-ttu-id="eaa1c-148">データベースは、モデルを使用して生成され、ソリューションに追加、ハード コーディングされた値を使用する代わりに、データベースから取得したデータをビュー テンプレートを提供する StoreController クラスを変更します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-148">Once the database is generated with the model, and added to the solution, you will modify the StoreController class to provide the View template with the data taken from the database, instead of using hard-coded values.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a><span data-ttu-id="eaa1c-149">タスク 1 - データベースに追加します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-149">Task 1 - Adding a Database</span></span>

<span data-ttu-id="eaa1c-150">このタスクでは、ソリューションに MusicStore アプリケーションのメイン テーブルを作成済みのデータベースを追加します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-150">In this task, you will add an already created database with the main tables of the MusicStore application to the solution.</span></span>

1. <span data-ttu-id="eaa1c-151">開く、**開始**ソリューションにある**ソース/Ex1-AddingADatabaseDBFirst/開始/** フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-151">Open the **Begin** solution located at **Source/Ex1-AddingADatabaseDBFirst/Begin/** folder.</span></span>

   1. <span data-ttu-id="eaa1c-152">いくつか不足している NuGet パッケージをダウンロードする必要がありますが続行前にします。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-152">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="eaa1c-153">これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-153">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="eaa1c-154">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-154">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="eaa1c-155">最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-155">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="eaa1c-156">NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-156">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="eaa1c-157">NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-157">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="eaa1c-158">これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-158">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="eaa1c-159">追加**MvcMusicStore**データベース ファイル。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-159">Add **MvcMusicStore** database file.</span></span> <span data-ttu-id="eaa1c-160">このハンズオン ラボと呼ばれる作成済みのデータベースを使用して**MvcMusicStore.mdf**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-160">In this Hands-on Lab, you will use an already created database called **MvcMusicStore.mdf**.</span></span> <span data-ttu-id="eaa1c-161">を右クリックして**アプリ\_データ**フォルダーを指す**追加** をクリックし、**既存項目の**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-161">To do that, right-click **App\_Data** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="eaa1c-162">参照**\Source\Assets**を選択し、 **MvcMusicStore.mdf**ファイル。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-162">Browse to **\Source\Assets** and select the **MvcMusicStore.mdf** file.</span></span>

    <span data-ttu-id="eaa1c-163">![既存の項目を追加する](aspnet-mvc-4-models-and-data-access/_static/image2.png "既存の項目を追加します。")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-163">![Adding an Existing Item](aspnet-mvc-4-models-and-data-access/_static/image2.png "Adding an Existing Item")</span></span>

    <span data-ttu-id="eaa1c-164">*既存の項目を追加します。*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-164">*Adding an Existing Item*</span></span>

    <span data-ttu-id="eaa1c-165">![データベース ファイルの MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf データベース ファイル")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-165">![MvcMusicStore.mdf database file](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf database file")</span></span>

    <span data-ttu-id="eaa1c-166">*MvcMusicStore.mdf データベース ファイル*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-166">*MvcMusicStore.mdf database file*</span></span>

    <span data-ttu-id="eaa1c-167">データベースがプロジェクトに追加されました。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-167">The database has been added to the project.</span></span> <span data-ttu-id="eaa1c-168">ソリューション内のデータベースが配置されている場合でもクエリを実行し、別のデータベース サーバーでホストされているように更新できます。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-168">Even when the database is located inside the solution, you can query and update it as it was hosted in a different database server.</span></span>

    <span data-ttu-id="eaa1c-169">![ソリューション エクスプ ローラーでデータベースを MvcMusicStore](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore ソリューション エクスプ ローラーでデータベース")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-169">![MvcMusicStore database in Solution Explorer](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore database in Solution Explorer")</span></span>

    <span data-ttu-id="eaa1c-170">*ソリューション エクスプ ローラーで MvcMusicStore データベース*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-170">*MvcMusicStore database in Solution Explorer*</span></span>
3. <span data-ttu-id="eaa1c-171">データベースへの接続を確認します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-171">Verify the connection to the database.</span></span> <span data-ttu-id="eaa1c-172">これを行うをダブルクリックして**MvcMusicStore.mdf**の接続を確立します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-172">To do this, double-click **MvcMusicStore.mdf** to establish a connection.</span></span>

    <span data-ttu-id="eaa1c-173">![MvcMusicStore.mdf に接続する](aspnet-mvc-4-models-and-data-access/_static/image5.png "MvcMusicStore.mdf への接続")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-173">![Connecting to MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "Connecting to MvcMusicStore.mdf")</span></span>

    <span data-ttu-id="eaa1c-174">*MvcMusicStore.mdf への接続*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-174">*Connecting to MvcMusicStore.mdf*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a><span data-ttu-id="eaa1c-175">タスク 2 - データ モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-175">Task 2 - Creating a Data Model</span></span>

<span data-ttu-id="eaa1c-176">このタスクでは、前のタスクに追加されたデータベースとやり取りするデータ モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-176">In this task, you will create a data model to interact with the database added in the previous task.</span></span>

1. <span data-ttu-id="eaa1c-177">データベースを表すデータ モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-177">Create a data model that will represent the database.</span></span> <span data-ttu-id="eaa1c-178">ソリューション エクスプ ローラーを右クリックで、**モデル**フォルダーを指す**追加**順にクリック**新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-178">To do this, in Solution Explorer right-click the **Models** folder, point to **Add** and then click **New Item**.</span></span> <span data-ttu-id="eaa1c-179">**新しい項目の追加**ダイアログで、選択、**データ**テンプレートし、 **ADO.NET エンティティ データ モデル**項目。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-179">In the **Add New Item** dialog, select the **Data** template and then the **ADO.NET Entity Data Model** item.</span></span> <span data-ttu-id="eaa1c-180">データ モデル名に変更**StoreDB.edmx**  をクリック**追加**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-180">Change the data model name to **StoreDB.edmx** and click **Add**.</span></span>

    <span data-ttu-id="eaa1c-181">![StoreDB ADO.NET エンティティ データ モデルを追加する](aspnet-mvc-4-models-and-data-access/_static/image6.png "StoreDB ADO.NET エンティティ データ モデルを追加します。")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-181">![Adding the StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "Adding the StoreDB ADO.NET Entity Data Model")</span></span>

    <span data-ttu-id="eaa1c-182">*StoreDB ADO.NET エンティティ データ モデルを追加します。*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-182">*Adding the StoreDB ADO.NET Entity Data Model*</span></span>
2. <span data-ttu-id="eaa1c-183">**Entity Data Model ウィザード**が表示されます。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-183">The **Entity Data Model Wizard** will appear.</span></span> <span data-ttu-id="eaa1c-184">このウィザードをモデル層の作成に使用します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-184">This wizard will guide you through the creation of the model layer.</span></span> <span data-ttu-id="eaa1c-185">追加された既存のデータベース recentyl に基づいてモデルを作成する必要があります、ために選択**データベースから生成** をクリック**次**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-185">Since the model should be created based on the existing database recentyl added, select **Generate from database** and click **Next**.</span></span>

    <span data-ttu-id="eaa1c-186">![モデル コンテンツを選択する](aspnet-mvc-4-models-and-data-access/_static/image7.png "モデル コンテンツを選択します。")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-186">![Choosing the model content](aspnet-mvc-4-models-and-data-access/_static/image7.png "Choosing the model content")</span></span>

    <span data-ttu-id="eaa1c-187">*モデル コンテンツを選択します。*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-187">*Choosing the model content*</span></span>
3. <span data-ttu-id="eaa1c-188">データベースからモデルを生成するためを使用する接続を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-188">Since you are generating a model from a database, you will need to specify the connection to use.</span></span> <span data-ttu-id="eaa1c-189">をクリックして**新しい接続**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-189">Click **New Connection**.</span></span>
4. <span data-ttu-id="eaa1c-190">選択**Microsoft SQL Server データベース ファイル** をクリック**続行**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-190">Select **Microsoft SQL Server Database File** and click **Continue**.</span></span>

    <span data-ttu-id="eaa1c-191">![データ ソースの選択](aspnet-mvc-4-models-and-data-access/_static/image8.png "データ ソースの選択")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-191">![Choose data source](aspnet-mvc-4-models-and-data-access/_static/image8.png "Choose data source")</span></span>

    <span data-ttu-id="eaa1c-192">*データ ソース ダイアログを選択します。*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-192">*Choose data source dialog*</span></span>
5. <span data-ttu-id="eaa1c-193">をクリックして**参照**にデータベースを選択**MvcMusicStore.mdf**内にある、**アプリ\_データ**フォルダーをクリック**OK**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-193">Click **Browse** and select the database **MvcMusicStore.mdf** you located in the **App\_Data** folder and click **OK**.</span></span>

    <span data-ttu-id="eaa1c-194">![接続プロパティ](aspnet-mvc-4-models-and-data-access/_static/image9.png "接続のプロパティ")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-194">![Connection properties](aspnet-mvc-4-models-and-data-access/_static/image9.png "Connection properties")</span></span>

    <span data-ttu-id="eaa1c-195">*接続のプロパティ*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-195">*Connection properties*</span></span>
6. <span data-ttu-id="eaa1c-196">生成されたクラスはエンティティ接続文字列と同じ名前を付ける、名前を変更する必要があります**MusicStoreEntities**  をクリック**次**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-196">The generated class should have the same name as the entity connection string, so change its name to **MusicStoreEntities** and click **Next**.</span></span>

    <span data-ttu-id="eaa1c-197">![データ接続を選択する](aspnet-mvc-4-models-and-data-access/_static/image10.png "データ接続を選択します。")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-197">![Choosing the data connection](aspnet-mvc-4-models-and-data-access/_static/image10.png "Choosing the data connection")</span></span>

    <span data-ttu-id="eaa1c-198">*データ接続を選択します。*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-198">*Choosing the data connection*</span></span>
7. <span data-ttu-id="eaa1c-199">使用するデータベース オブジェクトを選択します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-199">Choose the database objects to use.</span></span> <span data-ttu-id="eaa1c-200">エンティティ モデルには、データベースのテーブルだけで使用する、必要に応じて、選択、**テーブル**オプション、およびことを確認して、**モデルに外部キー列を含める**と**複数化または単数化する生成オブジェクト名**オプションも選択します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-200">As the Entity Model will use just the database's tables, select the **Tables** option, and make sure that the **Include foreign key columns in the model** and **Pluralize or singularize generated object names** options are also selected.</span></span> <span data-ttu-id="eaa1c-201">モデルの Namespace を変更する**MvcMusicStore.Model**  をクリック**完了**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-201">Change the Model Namespace to **MvcMusicStore.Model** and click **Finish**.</span></span>

    <span data-ttu-id="eaa1c-202">![データベース オブジェクトを選択する](aspnet-mvc-4-models-and-data-access/_static/image11.png "データベース オブジェクトを選択します。")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-202">![Choosing the database objects](aspnet-mvc-4-models-and-data-access/_static/image11.png "Choosing the database objects")</span></span>

    <span data-ttu-id="eaa1c-203">*データベース オブジェクトを選択します。*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-203">*Choosing the database objects*</span></span>

    > [!NOTE]
    > <span data-ttu-id="eaa1c-204">セキュリティ警告ダイアログ ボックスが表示されている場合にをクリックして**OK**をテンプレートを実行し、モデルのエンティティ クラスを生成します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-204">If a Security Warning dialog is shown, click **OK** to run the template and generate the classes for the model entities.</span></span>
8. <span data-ttu-id="eaa1c-205">データベースを各テーブルにマップする別のクラスが作成するときに、データベースのエンティティの図が表示されます。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-205">An entity diagram for the database will appear, while a separate class that maps each table to the database will be created.</span></span> <span data-ttu-id="eaa1c-206">たとえば、**アルバム**テーブルはで表されます、**アルバム**クラス、テーブル内の各列がクラス プロパティにマップされます。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-206">For example, the **Albums** table will be represented by an **Album** class, where each column in the table will map to a class property.</span></span> <span data-ttu-id="eaa1c-207">こうとをクエリして、データベース内の行を表すオブジェクトを使用します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-207">This will allow you to query and work with objects that represent rows in the database.</span></span>

    <span data-ttu-id="eaa1c-208">![エンティティの図](aspnet-mvc-4-models-and-data-access/_static/image12.png "エンティティの図")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-208">![Entity diagram](aspnet-mvc-4-models-and-data-access/_static/image12.png "Entity diagram")</span></span>

    <span data-ttu-id="eaa1c-209">*エンティティの図*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-209">*Entity diagram*</span></span>

    > [!NOTE]
    > <span data-ttu-id="eaa1c-210">T4 テンプレート (.tt) は、エンティティ クラスを生成するコードを実行し、同じ名前の既存のクラスが上書きされます。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-210">The T4 templates (.tt) run code to generate the entities classes and will overwrite the existing classes with the same name.</span></span> <span data-ttu-id="eaa1c-211">クラスは、この例で&quot;アルバム&quot;、&quot;ジャンル&quot;と&quot;アーティスト&quot;生成されたコードで上書きされています。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-211">In this example, the classes &quot;Album&quot;, &quot;Genre&quot; and &quot;Artist&quot; were overwritten with the generated code.</span></span>


<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a><span data-ttu-id="eaa1c-212">タスク 3 - アプリケーションのビルド</span><span class="sxs-lookup"><span data-stu-id="eaa1c-212">Task 3 - Building the Application</span></span>

<span data-ttu-id="eaa1c-213">このタスクでは、確認をモデルの生成が削除されますが、**アルバム**、**ジャンル**と**アーティスト**モデル クラス プロジェクトが正常にビルドを使用して新しいデータ モデル クラス。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-213">In this task, you will check that, although the model generation have removed the **Album**, **Genre** and **Artist** model classes, the project builds successfully by using the new data model classes.</span></span>

1. <span data-ttu-id="eaa1c-214">選択して、プロジェクトをビルド、**ビルド**メニュー項目し**ビルド MvcMusicStore**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-214">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="eaa1c-215">![プロジェクトのビルド](aspnet-mvc-4-models-and-data-access/_static/image13.png "プロジェクトのビルド")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-215">![Building the project](aspnet-mvc-4-models-and-data-access/_static/image13.png "Building the project")</span></span>

    <span data-ttu-id="eaa1c-216">*プロジェクトのビルド*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-216">*Building the project*</span></span>
2. <span data-ttu-id="eaa1c-217">プロジェクトが正常にビルドします。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-217">The project builds successfully.</span></span> <span data-ttu-id="eaa1c-218">なぜ引き続き動作しますか。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-218">Why does it still work?</span></span> <span data-ttu-id="eaa1c-219">データベースのテーブルが削除されたクラスに使用していたプロパティが含まれるフィールドを持つために、それと**アルバム**と**ジャンル**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-219">It works because the database tables have fields that include the properties that you were using in the removed classes **Album** and **Genre**.</span></span>

    <span data-ttu-id="eaa1c-220">![ビルドが成功した](aspnet-mvc-4-models-and-data-access/_static/image14.png "が成功したビルド")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-220">![Builds succeeded](aspnet-mvc-4-models-and-data-access/_static/image14.png "Builds succeeded")</span></span>

    <span data-ttu-id="eaa1c-221">*ビルドが成功しました*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-221">*Builds succeeded*</span></span>
3. <span data-ttu-id="eaa1c-222">デザイナーはダイアグラム形式でのエンティティが表示されますが、これらは実際に c# クラスです。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-222">While the designer displays the entities in a diagram format, they are really C# classes.</span></span> <span data-ttu-id="eaa1c-223">展開して、 **StoreDB.edmx**ソリューション エクスプ ローラーでノードし**StoreDB.tt**、新規生成されたエンティティが表示されます。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-223">Expand the **StoreDB.edmx** node in the Solution Explorer and then **StoreDB.tt**, you will see the new generated entities.</span></span>

    <span data-ttu-id="eaa1c-224">![生成されたファイル](aspnet-mvc-4-models-and-data-access/_static/image15.png "によって生成されたファイル")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-224">![Generated files](aspnet-mvc-4-models-and-data-access/_static/image15.png "Generated files")</span></span>

    <span data-ttu-id="eaa1c-225">*生成されたファイル*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-225">*Generated files*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="eaa1c-226">タスク 4 - データベースの照会</span><span class="sxs-lookup"><span data-stu-id="eaa1c-226">Task 4 - Querying the Database</span></span>

<span data-ttu-id="eaa1c-227">このタスクでハードコーディングされたデータを使用する代わりにこれはデータベースにクエリ情報を取得するように、StoreController クラスが更新されます。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-227">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will query the database to retrieve the information.</span></span>

1. <span data-ttu-id="eaa1c-228">開いている**Controllers\StoreController.cs**のインスタンスを保持するクラスに次のフィールドを追加し、 **MusicStoreEntities**という名前のクラス**storeDB**:</span><span class="sxs-lookup"><span data-stu-id="eaa1c-228">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="eaa1c-229">(コード スニペットの*モデルおよびデータ アクセス-Ex1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="eaa1c-229">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
2. <span data-ttu-id="eaa1c-230">**MusicStoreEntities**クラスは、データベース内の各テーブルのコレクション プロパティを公開します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-230">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="eaa1c-231">更新**参照**アクションを取得する方法ですべてのジャンル、**アルバム**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-231">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="eaa1c-232">(コード スニペットの*モデルおよびデータ アクセス - Ex1 ストア参照*)</span><span class="sxs-lookup"><span data-stu-id="eaa1c-232">(Code Snippet - *Models And Data Access - Ex1 Store Browse*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

    > [!NOTE]
    > <span data-ttu-id="eaa1c-233">.Net と呼ばれる機能を使用している**LINQ** (言語統合クエリ) は、データベースに対してコードを実行し、返されますが、これらのコレクションに対して厳密に型指定されたクエリ式を作成するオブジェクトをプログラミングできます比較</span><span class="sxs-lookup"><span data-stu-id="eaa1c-233">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
    > 
    > <span data-ttu-id="eaa1c-234">LINQ の概要の詳細については、次を参照してください、 [msdn サイト](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-234">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span></span>
3. <span data-ttu-id="eaa1c-235">更新**インデックス**アクション メソッドは、すべてのジャンルを取得します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-235">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="eaa1c-236">(コード スニペットの*モデルおよびデータ アクセス - Ex1 ストア インデックス*)</span><span class="sxs-lookup"><span data-stu-id="eaa1c-236">(Code Snippet - *Models And Data Access - Ex1 Store Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
4. <span data-ttu-id="eaa1c-237">更新**インデックス**アクション メソッドのすべてのジャンルを取得し、一覧にコレクションを変換します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-237">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="eaa1c-238">(コード スニペットの*モデルおよびデータ アクセス - Ex1 ストア GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="eaa1c-238">(Code Snippet - *Models And Data Access - Ex1 Store GenreMenu*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="eaa1c-239">タスク 5 - アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-239">Task 5 - Running the Application</span></span>

<span data-ttu-id="eaa1c-240">このタスクでは、ストア インデックスのページをジャンルのハードコードされたものではなく、データベースに格納されているようになりました表示するかを確認します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-240">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="eaa1c-241">に、ビューのテンプレートを変更する必要はありません、 **StoreController**はエンティティを取得する同じ以前と同様が、この時点のデータは、データベースから取得されます。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-241">There is no need to change the View template because the **StoreController** is returning the same entities as before, although this time the data will come from the database.</span></span>

1. <span data-ttu-id="eaa1c-242">キーを押して、ソリューションを再構築**f5 キーを押して**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-242">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="eaa1c-243">ホーム ページで、プロジェクトを開始します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-243">The project starts in the Home page.</span></span> <span data-ttu-id="eaa1c-244">いることを確認のメニュー**ジャンル**ハードコードされた一覧で、できなくなり、データがデータベースから直接取得します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-244">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    <span data-ttu-id="eaa1c-246">*データベースからジャンルをブラウズ*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-246">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="eaa1c-247">ここで、ジャンルを参照し、アルバムがデータベースから生成されますを確認します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-247">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="eaa1c-248">![データベースからアルバムを閲覧](aspnet-mvc-4-models-and-data-access/_static/image17.png "データベースからアルバムを参照")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-248">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image17.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="eaa1c-249">*データベースからアルバムを参照*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-249">*Browsing Albums from the database*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a><span data-ttu-id="eaa1c-250">手順 2: 最初にコードを使用してデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-250">Exercise 2: Creating a Database Using Code First</span></span>

<span data-ttu-id="eaa1c-251">この演習では、コードの最初の方法を使用して、MusicStore アプリケーションのテーブルを含むデータベースを作成する方法とそのデータにアクセスする方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-251">In this exercise, you will learn how to use the Code First approach to create a database with the tables of the MusicStore application, and how to access its data.</span></span>

<span data-ttu-id="eaa1c-252">モデルが生成されると、ハードコードされた値を使用する代わりに、データベースから取得したデータをビュー テンプレートを提供する StoreController を変更します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-252">Once the model is generated, you will modify the StoreController to provide the View template with the data taken from the database, instead of using hardcoded values.</span></span>

> [!NOTE]
> <span data-ttu-id="eaa1c-253">手順 1 を完了し、データベースの最初の方法を使用してきた既にする場合は、ここで、別のプロセスで同じ結果を取得する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-253">If you have completed Exercise 1 and have already worked with the Database First approach, you will now learn how to get the same results with a different process.</span></span> <span data-ttu-id="eaa1c-254">演習 1 と同じようなタスクは、読み取りが容易に設定されています。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-254">The tasks that are in common with Exercise 1 have been marked to make your reading easier.</span></span> <span data-ttu-id="eaa1c-255">手順 1 を完了していないコードの最初の方法を学習したい場合は、ここから開始し、このトピックの完全なカバレッジを取得できます。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-255">If you have not completed Exercise 1 but would like to learn the Code First approach, you can start from this exercise and get a full coverage of the topic.</span></span>


<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a><span data-ttu-id="eaa1c-256">タスク 1 - サンプル データを取り込み</span><span class="sxs-lookup"><span data-stu-id="eaa1c-256">Task 1 - Populating Sample Data</span></span>

<span data-ttu-id="eaa1c-257">このタスクの作成時に初期状態で 1 コード優先を使用してデータベースにサンプル データを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-257">In this task, you will populate the database with sample data when it is intially created using Code-First.</span></span>

1. <span data-ttu-id="eaa1c-258">開く、**開始**ソリューションにある**ソース/Ex2-CreatingADatabaseCodeFirst/開始/** フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-258">Open the **Begin** solution located at **Source/Ex2-CreatingADatabaseCodeFirst/Begin/** folder.</span></span> <span data-ttu-id="eaa1c-259">それ以外の場合、作業を続行できますを使用して、**終了**ソリューションは、前の手順を完了して取得します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-259">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="eaa1c-260">指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-260">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="eaa1c-261">これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-261">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="eaa1c-262">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-262">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="eaa1c-263">最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-263">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="eaa1c-264">NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-264">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="eaa1c-265">NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-265">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="eaa1c-266">これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-266">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="eaa1c-267">追加、 **SampleData.cs**ファイルの名前を**モデル**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-267">Add the **SampleData.cs** file to the **Models** folder.</span></span> <span data-ttu-id="eaa1c-268">を右クリックして**モデル**フォルダーを指す**追加** をクリックし、**既存項目の**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-268">To do that, right-click **Models** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="eaa1c-269">参照**\Source\Assets**を選択し、 **SampleData.cs**ファイル。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-269">Browse to **\Source\Assets** and select the **SampleData.cs** file.</span></span>

    <span data-ttu-id="eaa1c-270">![サンプル データへの追加コード](aspnet-mvc-4-models-and-data-access/_static/image18.png "サンプル データへのコードの追加")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-270">![Sample data populate code](aspnet-mvc-4-models-and-data-access/_static/image18.png "Sample data populate code")</span></span>

    <span data-ttu-id="eaa1c-271">*サンプル データへのコードを追加します。*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-271">*Sample data populate code*</span></span>
3. <span data-ttu-id="eaa1c-272">開く、 **Global.asax.cs**ファイルし、次の追加*を使用して*ステートメントです。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-272">Open the **Global.asax.cs** file and add the following *using* statements.</span></span>

    <span data-ttu-id="eaa1c-273">(コード スニペットの*モデルおよびデータ アクセス - Ex2 グローバル Asax Using*)</span><span class="sxs-lookup"><span data-stu-id="eaa1c-273">(Code Snippet - *Models And Data Access - Ex2 Global Asax Usings*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
4. <span data-ttu-id="eaa1c-274">**アプリケーション\_Start()** メソッドは、データベース初期化子を設定する次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-274">In the **Application\_Start()** method add the following line to set the database initializer.</span></span>

    <span data-ttu-id="eaa1c-275">(コード スニペットの*モデルおよびデータ アクセス - Ex2 グローバル Asax SetInitializer*)</span><span class="sxs-lookup"><span data-stu-id="eaa1c-275">(Code Snippet - *Models And Data Access - Ex2 Global Asax SetInitializer*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a><span data-ttu-id="eaa1c-276">タスク 2 - データベースへの接続の構成</span><span class="sxs-lookup"><span data-stu-id="eaa1c-276">Task 2 - Configuring the connection to the Database</span></span>

<span data-ttu-id="eaa1c-277">プロジェクトにデータベースが既に追加されたに記述、 **Web.config**ファイルの接続文字列。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-277">Now that you have already added a database to our project, you will write in the **Web.config** file the connection string.</span></span>

1. <span data-ttu-id="eaa1c-278">接続文字列を追加する**Web.config**です。実行するには、開く**Web.config**プロジェクトのルートと置換、接続文字列を次の行で DefaultConnection をという名前で、 **&lt;connectionStrings&gt;** セクション。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-278">Add a connection string at **Web.config**. To do that, open **Web.config** at project root and replace the connection string named DefaultConnection with this line in the **&lt;connectionStrings&gt;** section:</span></span>

    <span data-ttu-id="eaa1c-279">![Web.config ファイルの場所](aspnet-mvc-4-models-and-data-access/_static/image19.png "Web.config ファイルの場所")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-279">![Web.config file location](aspnet-mvc-4-models-and-data-access/_static/image19.png "Web.config file location")</span></span>

    <span data-ttu-id="eaa1c-280">*web.config ファイルの場所*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-280">*Web.config file location*</span></span>

    [!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a><span data-ttu-id="eaa1c-281">タスク 3 - モデルを操作</span><span class="sxs-lookup"><span data-stu-id="eaa1c-281">Task 3 - Working with the Model</span></span>

<span data-ttu-id="eaa1c-282">データベースへの接続を構成しておくことは、データベース テーブルを使用してモデルをリンクします。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-282">Now that you have already configured the connection to the database, you will link the model with the database tables.</span></span> <span data-ttu-id="eaa1c-283">このタスクでは、Code First でデータベースにリンクするクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-283">In this task, you will create a class that will be linked to the database with Code First.</span></span> <span data-ttu-id="eaa1c-284">変更する必要がある、既存モデルの POCO クラスがあることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-284">Remember that there is an existent POCO model class that should be modified.</span></span>

> [!NOTE]
> <span data-ttu-id="eaa1c-285">演習 1 を完了する場合は、ウィザードによって、この手順が行われたことがわかります。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-285">If you completed Exercise 1, you will note that this step was performed by a wizard.</span></span> <span data-ttu-id="eaa1c-286">Code First によりデータ エンティティにリンクするクラスを手動で作成されます。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-286">By doing Code First, you will manually create classes that will be linked to data entities.</span></span>

1. <span data-ttu-id="eaa1c-287">POCO モデル クラスを開く**ジャンル**から**モデル**フォルダーをプロジェクトし、ID を含める</span><span class="sxs-lookup"><span data-stu-id="eaa1c-287">Open the POCO model class **Genre** from **Models** project folder and include an ID.</span></span> <span data-ttu-id="eaa1c-288">Int 型のプロパティを使用して、名前を持つ**GenreId**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-288">Use an int property with the name **GenreId**.</span></span>

    <span data-ttu-id="eaa1c-289">(コード スニペットの*モデルおよびデータ アクセス - Ex2 コードの最初のジャンル*)</span><span class="sxs-lookup"><span data-stu-id="eaa1c-289">(Code Snippet - *Models And Data Access - Ex2 Code First Genre*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

    > [!NOTE]
    > <span data-ttu-id="eaa1c-290">Code First 規約に従ってを操作するには、自動的に検出される主キーのプロパティがクラス ジャンルに必要です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-290">To work with Code First conventions, the class Genre must have a primary key property that will be automatically detected.</span></span>
    > 
    > <span data-ttu-id="eaa1c-291">詳細を読み取ることができます First 規約に従ってこのコードに関する[msdn の記事](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-291">You can read more about Code First Conventions in this [msdn article](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).</span></span>
2. <span data-ttu-id="eaa1c-292">これで、POCO のモデル クラスを開く**アルバム**から**モデル**プロジェクト フォルダーと外部キーが含まれて、名前のプロパティを作成する**GenreId**と**ArtistId**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-292">Now, open the POCO model class **Album** from **Models** project folder and include the foreign keys, create properties with the names **GenreId** and **ArtistId**.</span></span> <span data-ttu-id="eaa1c-293">このクラスが既にある、 **GenreId**の主キー。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-293">This class already have the **GenreId** for the primary key.</span></span>

    <span data-ttu-id="eaa1c-294">(コード スニペットの*モデルおよびデータ アクセス - Ex2 コードの最初のアルバム*)</span><span class="sxs-lookup"><span data-stu-id="eaa1c-294">(Code Snippet - *Models And Data Access - Ex2 Code First Album*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
3. <span data-ttu-id="eaa1c-295">POCO モデル クラスを開く**アーティスト**を含めると、 **ArtistId**プロパティです。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-295">Open the POCO model class **Artist** and include the **ArtistId** property.</span></span>

    <span data-ttu-id="eaa1c-296">(コード スニペットの*モデルおよびデータ アクセス - Ex2 コードの最初のアーティスト*)</span><span class="sxs-lookup"><span data-stu-id="eaa1c-296">(Code Snippet - *Models And Data Access - Ex2 Code First Artist*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
4. <span data-ttu-id="eaa1c-297">右クリックし、**モデル**プロジェクト フォルダーを選択**追加 |クラス**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-297">Right-click the **Models** project folder and select **Add | Class**.</span></span> <span data-ttu-id="eaa1c-298">ファイルの名前を付けます**MusicStoreEntities.cs**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-298">Name the file **MusicStoreEntities.cs**.</span></span> <span data-ttu-id="eaa1c-299">をクリックし、**追加します。**</span><span class="sxs-lookup"><span data-stu-id="eaa1c-299">Then, click **Add.**</span></span>

    <span data-ttu-id="eaa1c-300">![クラスの追加](aspnet-mvc-4-models-and-data-access/_static/image20.png "クラスの追加")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-300">![Adding a class](aspnet-mvc-4-models-and-data-access/_static/image20.png "Adding a class")</span></span>

    <span data-ttu-id="eaa1c-301">*新しい項目の追加*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-301">*Adding a new item*</span></span>

    <span data-ttu-id="eaa1c-302">![Class2 を追加する](aspnet-mvc-4-models-and-data-access/_static/image21.png "class2 を追加します。")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-302">![Adding a class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Adding a class2")</span></span>

    <span data-ttu-id="eaa1c-303">*クラスの追加*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-303">*Adding a class*</span></span>
5. <span data-ttu-id="eaa1c-304">先ほど作成したクラスを開く**MusicStoreEntities.cs**、名前空間を含めると**System.Data.Entity**と**System.Data.Entity.Infrastructure**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-304">Open the class you have just created, **MusicStoreEntities.cs**, and include the namespaces **System.Data.Entity** and **System.Data.Entity.Infrastructure**.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
6. <span data-ttu-id="eaa1c-305">拡張するクラスの宣言を置き換える、 **DbContext**クラス: パブリックに宣言**DBSet**オーバーライドと**OnModelCreating**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-305">Replace the class declaration to extend the **DbContext** class: declare a public **DBSet** and override **OnModelCreating** method.</span></span> <span data-ttu-id="eaa1c-306">この手順の後に、Entity Framework でモデルをリンクしているドメイン クラスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-306">After this step you will get a domain class that will link your model with the Entity Framework.</span></span> <span data-ttu-id="eaa1c-307">そのためには、次のようにクラス コードを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-307">In order to do that, replace the class code with the following:</span></span>

    <span data-ttu-id="eaa1c-308">(コード スニペットの*モデルおよびデータ アクセス - Ex2 コード最初 MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="eaa1c-308">(Code Snippet - *Models And Data Access - Ex2 Code First MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> <span data-ttu-id="eaa1c-309">Entity Framework と**DbContext**と**DBSet** POCO クラス ジャンルを照会することができます。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-309">With Entity Framework **DbContext** and **DBSet** you will be able to query the POCO class Genre.</span></span> <span data-ttu-id="eaa1c-310">拡張することによって**OnModelCreating**メソッドで指定する、**コード**ジャンルがデータベース テーブルにマップする方法です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-310">By extending **OnModelCreating** method, you are specifying in the **code** how Genre will be mapped to a database table.</span></span> <span data-ttu-id="eaa1c-311">Msdn の「の詳細については、DBContext および DBSet を見つけることができます:[リンク](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)</span><span class="sxs-lookup"><span data-stu-id="eaa1c-311">You can find more information about DBContext and DBSet in this msdn article: [link](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="eaa1c-312">タスク 4 - データベースの照会</span><span class="sxs-lookup"><span data-stu-id="eaa1c-312">Task 4 - Querying the Database</span></span>

<span data-ttu-id="eaa1c-313">これにより、ハードコードされたデータを使用する代わりに、そのデータベースから取得する、このタスクで StoreController クラスが更新されます。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-313">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will retrieve it from the database.</span></span>

> [!NOTE]
> <span data-ttu-id="eaa1c-314">このタスクでは、手順 1 と共通します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-314">This task is in common with Exercise 1.</span></span>
> 
> <span data-ttu-id="eaa1c-315">演習 1 が完了した場合これらの手順は、同じ両方の方法で注意してくださいされます (データベースの最初または Code first)。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-315">If you completed Exercise 1 you will note these steps are the same in both approaches (Database first or Code first).</span></span> <span data-ttu-id="eaa1c-316">モデルでは、データをリンクする方法では異なるが、データ エンティティへのアクセスは、コント ローラーから透過的まだ。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-316">They are different in how the data is linked with the model, but the access to data entities is yet transparent from the controller.</span></span>


1. <span data-ttu-id="eaa1c-317">開いている**Controllers\StoreController.cs**のインスタンスを保持するクラスに次のフィールドを追加し、 **MusicStoreEntities**という名前のクラス**storeDB**:</span><span class="sxs-lookup"><span data-stu-id="eaa1c-317">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="eaa1c-318">(コード スニペットの*モデルおよびデータ アクセス-Ex1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="eaa1c-318">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
2. <span data-ttu-id="eaa1c-319">**MusicStoreEntities**クラスは、データベース内の各テーブルのコレクション プロパティを公開します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-319">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="eaa1c-320">更新**参照**アクションを取得する方法ですべてのジャンル、**アルバム**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-320">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="eaa1c-321">(コード スニペットの*モデルおよびデータ アクセス - Ex2 ストア参照*)</span><span class="sxs-lookup"><span data-stu-id="eaa1c-321">(Code Snippet - *Models And Data Access - Ex2 Store Browse*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="eaa1c-322">.Net と呼ばれる機能を使用している**LINQ** (言語統合クエリ) は、データベースに対してコードを実行し、返されますが、これらのコレクションに対して厳密に型指定されたクエリ式を作成するオブジェクトをプログラミングできます比較</span><span class="sxs-lookup"><span data-stu-id="eaa1c-322">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
    > 
    > <span data-ttu-id="eaa1c-323">LINQ の概要の詳細については、次を参照してください、 [msdn サイト](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-323">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).</span></span>
3. <span data-ttu-id="eaa1c-324">更新**インデックス**アクション メソッドは、すべてのジャンルを取得します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-324">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="eaa1c-325">(コード スニペットの*モデルおよびデータ アクセス - Ex2 ストア インデックス*)</span><span class="sxs-lookup"><span data-stu-id="eaa1c-325">(Code Snippet - *Models And Data Access - Ex2 Store Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
4. <span data-ttu-id="eaa1c-326">更新**インデックス**アクション メソッドのすべてのジャンルを取得し、一覧にコレクションを変換します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-326">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="eaa1c-327">(コード スニペットの*モデルおよびデータ アクセス - Ex2 ストア GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="eaa1c-327">(Code Snippet - *Models And Data Access - Ex2 Store GenreMenu*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="eaa1c-328">タスク 5 - アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-328">Task 5 - Running the Application</span></span>

<span data-ttu-id="eaa1c-329">このタスクでは、ストア インデックスのページをジャンルのハードコードされたものではなく、データベースに格納されているようになりました表示するかを確認します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-329">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="eaa1c-330">に、ビューのテンプレートを変更する必要はありません、 **StoreController**同じを返して**StoreIndexViewModel**以前と同様、今度は、データは、データベースから取得されます。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-330">There is no need to change the View template because the **StoreController** is returning the same **StoreIndexViewModel** as before, but this time the data will come from the database.</span></span>

1. <span data-ttu-id="eaa1c-331">キーを押して、ソリューションを再構築**f5 キーを押して**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-331">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="eaa1c-332">ホーム ページで、プロジェクトを開始します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-332">The project starts in the Home page.</span></span> <span data-ttu-id="eaa1c-333">いることを確認のメニュー**ジャンル**ハードコードされた一覧で、できなくなり、データがデータベースから直接取得します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-333">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    <span data-ttu-id="eaa1c-335">*データベースからジャンルをブラウズ*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-335">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="eaa1c-336">ここで、ジャンルを参照し、アルバムがデータベースから生成されますを確認します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-336">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="eaa1c-337">![データベースからアルバムを閲覧](aspnet-mvc-4-models-and-data-access/_static/image23.png "データベースからアルバムを参照")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-337">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image23.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="eaa1c-338">*データベースからアルバムを参照*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-338">*Browsing Albums from the database*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a><span data-ttu-id="eaa1c-339">手順 3: パラメーターを持つデータベースの照会</span><span class="sxs-lookup"><span data-stu-id="eaa1c-339">Exercise 3: Querying the Database with Parameters</span></span>

<span data-ttu-id="eaa1c-340">この演習では、パラメーターを使用してデータベースを照会する方法およびクエリ結果の整形を使用する方法を学習、番号のデータベースを小さく機能がより効率的な方法でのデータの取得をアクセスします。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-340">In this exercise, you will learn how to query the database using parameters, and how to use Query Result Shaping, a feature that reduces the number database accesses retrieving data in a more efficient way.</span></span>

> [!NOTE]
> <span data-ttu-id="eaa1c-341">クエリ結果のシェイプの詳細については、次を参照してください。 [msdn の記事](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-341">For further information on Query Result Shaping, visit the following [msdn article](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a><span data-ttu-id="eaa1c-342">タスク 1 - 変更 StoreController データベースからアルバムを取得するには</span><span class="sxs-lookup"><span data-stu-id="eaa1c-342">Task 1 - Modifying StoreController to Retrieve Albums from Database</span></span>

<span data-ttu-id="eaa1c-343">このタスクでは、変更、 **StoreController**特定ジャンルからアルバムを取得するデータベースにアクセスするクラス。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-343">In this task, you will change the **StoreController** class to access the database to retrieve albums from a specific genre.</span></span>

1. <span data-ttu-id="eaa1c-344">開く、**開始**ソリューションがある、 **Source\Ex3 QueryingTheDatabaseWithParametersCodeFirst\Begin**コード優先のアプローチを使用する場合はフォルダーまたは**Source\Ex3 QueryingTheDatabaseWithParametersDBFirst\Begin**データベース優先のアプローチを使用する場合はフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-344">Open the **Begin** solution located at the **Source\Ex3-QueryingTheDatabaseWithParametersCodeFirst\Begin** folder if you want to use Code-First approach or **Source\Ex3-QueryingTheDatabaseWithParametersDBFirst\Begin** folder if you want to use Database-First approach.</span></span> <span data-ttu-id="eaa1c-345">それ以外の場合、作業を続行できますを使用して、**終了**ソリューションは、前の手順を完了して取得します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-345">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="eaa1c-346">指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-346">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="eaa1c-347">これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-347">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="eaa1c-348">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-348">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="eaa1c-349">最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-349">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="eaa1c-350">NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-350">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="eaa1c-351">NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-351">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="eaa1c-352">これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-352">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="eaa1c-353">開く、 **StoreController**を変更するクラス、**参照**アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-353">Open the **StoreController** class to change the **Browse** action method.</span></span> <span data-ttu-id="eaa1c-354">これを行うには**ソリューション エクスプ ローラー**、展開、**コント ローラー**フォルダーをダブルクリック**StoreController.cs**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-354">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
3. <span data-ttu-id="eaa1c-355">変更、**参照**アクション メソッドを特定のジャンルのアルバムを取得します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-355">Change the **Browse** action method to retrieve albums for a specific genre.</span></span> <span data-ttu-id="eaa1c-356">これを行うには、次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-356">To do this, replace the following code:</span></span>

    <span data-ttu-id="eaa1c-357">(コード スニペットの*モデルおよびデータ アクセス - Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="eaa1c-357">(Code Snippet - *Models And Data Access - Ex3 StoreController BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> <span data-ttu-id="eaa1c-358">エンティティのコレクションを指定するには、使用する必要があります、 **Include**すぎる、アルバムを取得するを指定します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-358">To populate a collection of the entity, you need to use the **Include** method to specify you want to retrieve the albums too.</span></span> <span data-ttu-id="eaa1c-359">使用することができます、します。**Single()** LINQ の拡張機能アルバムの 1 つだけジャンルがここで予想されるためです。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-359">You can use the .**Single()** extension in LINQ because in this case only one genre is expected for an album.</span></span> <span data-ttu-id="eaa1c-360">**Single()** メソッドは、ここでは、名前が定義されている値と一致するように 1 つのジャンル オブジェクトを指定するパラメーターとしてラムダ式を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-360">The **Single()** method takes a Lambda expression as a parameter, which in this case specifies a single Genre object such that its name matches the value defined.</span></span>
> 
> <span data-ttu-id="eaa1c-361">ジャンル オブジェクトを取得するときにもアンロードするその他の関連エンティティを示すために使用できる機能の利用されます。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-361">You will take advantage of a feature that allows you to indicate other related entities you want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="eaa1c-362">この機能が呼び出されます**クエリ結果の整形**情報を取得するデータベースにアクセスするために必要な回数を削減することができます。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-362">This feature is called **Query Result Shaping**, and enables you to reduce the number of times needed to access the database to retrieve information.</span></span> <span data-ttu-id="eaa1c-363">このシナリオを取得するジャンルのアルバムをプリフェッチするされます。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-363">In this scenario, you will want to pre-fetch the Albums for the Genre you retrieve.</span></span>
> 
> <span data-ttu-id="eaa1c-364">クエリが含まれる**Genres.Include (&quot;アルバム&quot;)** を関連するアルバムもすることを示します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-364">The query includes **Genres.Include(&quot;Albums&quot;)** to indicate that you want related albums as well.</span></span> <span data-ttu-id="eaa1c-365">これが 1 つのデータベースの要求でジャンルとアルバムの両方のデータが取得されますのでより効率的なアプリケーションで発生します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-365">This will result in a more efficient application, since it will retrieve both Genre and Album data in a single database request.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="eaa1c-366">タスク 2 - アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-366">Task 2 - Running the Application</span></span>

<span data-ttu-id="eaa1c-367">このタスクでは、アプリケーションを実行し、特定のジャンルのアルバムをデータベースから取得します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-367">In this task, you will run the application and retrieve albums of a specific genre from the database.</span></span>

1. <span data-ttu-id="eaa1c-368">キーを押して**f5 キーを押して**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-368">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="eaa1c-369">ホーム ページで、プロジェクトを開始します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-369">The project starts in the Home page.</span></span> <span data-ttu-id="eaa1c-370">URL を変更して**ストア/参照? ジャンル = Pop**結果は、データベースから取得されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-370">Change the URL to **/Store/Browse?genre=Pop** to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="eaa1c-371">![ジャンル参照](aspnet-mvc-4-models-and-data-access/_static/image24.png "ジャンルの閲覧")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-371">![Browsing by genre](aspnet-mvc-4-models-and-data-access/_static/image24.png "Browsing by genre")</span></span>

    <span data-ttu-id="eaa1c-372">*参照/ストア/参照? ジャンル Pop を =*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-372">*Browsing /Store/Browse?genre=Pop*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a><span data-ttu-id="eaa1c-373">タスク 3 - Id でのアルバムへのアクセス</span><span class="sxs-lookup"><span data-stu-id="eaa1c-373">Task 3 - Accessing Albums by Id</span></span>

<span data-ttu-id="eaa1c-374">このタスクでは、アルバムの id を取得する前の手順を繰り返します</span><span class="sxs-lookup"><span data-stu-id="eaa1c-374">In this task, you will repeat the previous procedure to get albums by their Id.</span></span>

1. <span data-ttu-id="eaa1c-375">Visual Studio に戻るには、必要な場合は、ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-375">Close the browser if needed, to return to Visual Studio.</span></span> <span data-ttu-id="eaa1c-376">開く、 **StoreController**を変更するクラス、**詳細**アクション メソッド。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-376">Open the **StoreController** class to change the **Details** action method.</span></span> <span data-ttu-id="eaa1c-377">これを行うには**ソリューション エクスプ ローラー**、展開、**コント ローラー**フォルダーをダブルクリック**StoreController.cs**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-377">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
2. <span data-ttu-id="eaa1c-378">変更、**詳細**アルバムの詳細を取得するアクション メソッドがに基づいて、 **Id**です。これを行うには、次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-378">Change the **Details** action method to retrieve albums details based on their **Id**. To do this, replace the following code:</span></span>

    <span data-ttu-id="eaa1c-379">(コード スニペットの*モデルおよびデータ アクセス - Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="eaa1c-379">(Code Snippet - *Models And Data Access - Ex3 StoreController DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="eaa1c-380">タスク 4 - アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-380">Task 4 - Running the Application</span></span>

<span data-ttu-id="eaa1c-381">このタスクでは、web ブラウザーでアプリケーションを実行し、その id でアルバムの詳細の取得</span><span class="sxs-lookup"><span data-stu-id="eaa1c-381">In this task, you will run the Application in a web browser and obtain album details by their Id.</span></span>

1. <span data-ttu-id="eaa1c-382">キーを押して**f5 キーを押して**アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-382">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="eaa1c-383">ホーム ページで、プロジェクトを開始します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-383">The project starts in the Home page.</span></span> <span data-ttu-id="eaa1c-384">URL を変更して **/Store/Details/51**か、ジャンルを参照し、結果をデータベースから取得することを確認するアルバムを選択します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-384">Change the URL to **/Store/Details/51** or browse the genres and select an album to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="eaa1c-385">![詳細情報を閲覧](aspnet-mvc-4-models-and-data-access/_static/image25.png "の詳細を参照")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-385">![Browsing Details](aspnet-mvc-4-models-and-data-access/_static/image25.png "Browsing Details")</span></span>

    <span data-ttu-id="eaa1c-386">*/Store/Details/51 をブラウズ*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-386">*Browsing /Store/Details/51*</span></span>

> [!NOTE]
> <span data-ttu-id="eaa1c-387">Windows Azure Web サイトの次にこのアプリケーションを展開するさらに、[付録 b: 公開 Web Deploy を使用して ASP.NET MVC 4 アプリケーション](#AppendixB)です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-387">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="eaa1c-388">まとめ</span><span class="sxs-lookup"><span data-stu-id="eaa1c-388">Summary</span></span>

<span data-ttu-id="eaa1c-389">ASP.NET MVC モデルおよびデータ アクセスの基礎を学習したこのハンズオン ラボを完了するを使用して、 **Database First**方法だけでなく**Code First**方法。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-389">By completing this Hands-on Lab you have learned the fundamentals of ASP.NET MVC Models and Data Access, using the **Database First** approach as well as the **Code First** Approach:</span></span>

- <span data-ttu-id="eaa1c-390">そのデータを使用するために、ソリューションにデータベースを追加する方法</span><span class="sxs-lookup"><span data-stu-id="eaa1c-390">How to add a database to the solution in order to consume its data</span></span>
- <span data-ttu-id="eaa1c-391">テンプレートの表示をハード コーディングされた 1 つではなく、データベースから取得したデータを提供するコント ローラーを更新する方法</span><span class="sxs-lookup"><span data-stu-id="eaa1c-391">How to update Controllers to provide View templates with the data taken from the database instead of hard-coded one</span></span>
- <span data-ttu-id="eaa1c-392">パラメーターを使用してデータベースを照会する方法</span><span class="sxs-lookup"><span data-stu-id="eaa1c-392">How to query the database using parameters</span></span>
- <span data-ttu-id="eaa1c-393">クエリ結果の整形、データベースへのアクセスの数を削減する機能より効率的な方法でデータの取得を使用する方法</span><span class="sxs-lookup"><span data-stu-id="eaa1c-393">How to use the Query Result Shaping, a feature that reduces the number of database accesses, retrieving data in a more efficient way</span></span>
- <span data-ttu-id="eaa1c-394">Microsoft Entity Framework での Database First と Code First の両方のアプローチを使用して、モデルとデータベースをリンクする方法</span><span class="sxs-lookup"><span data-stu-id="eaa1c-394">How to use both Database First and Code First approaches in Microsoft Entity Framework to link the database with the model</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="eaa1c-395">付録 a: をインストールする Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="eaa1c-395">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="eaa1c-396">インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、 **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="eaa1c-396">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="eaa1c-397">次の手順を案内するインストールに必要な手順*Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-397">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="eaa1c-398">移動して[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-398">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="eaa1c-399">または、既に Web Platform Installer をインストールした場合、開くことができ、製品を検索&quot; <em>Visual Studio Express 2012 for Web と Windows Azure SDK</em>&quot;です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-399">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="eaa1c-400">をクリックして**を今すぐインストール**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-400">Click on **Install Now**.</span></span> <span data-ttu-id="eaa1c-401">ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-401">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="eaa1c-402">1 回**Web Platform Installer**が開いて、をクリックして**インストール**セットアップを開始します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-402">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="eaa1c-403">![Visual Studio Express インストール](aspnet-mvc-4-models-and-data-access/_static/image26.png "Visual Studio Express のインストール")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-403">![Install Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="eaa1c-404">*Visual Studio Express をインストールします。*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-404">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="eaa1c-405">すべての製品のライセンスと条項を読み、クリックして**同意**を続行します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-405">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![ライセンス条項に同意](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    <span data-ttu-id="eaa1c-407">*ライセンス条項に同意*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-407">*Accepting the license terms*</span></span>
5. <span data-ttu-id="eaa1c-408">ダウンロードとインストール プロセスが完了するまで待機します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-408">Wait until the downloading and installation process completes.</span></span>

    ![インストールの進行状況](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    <span data-ttu-id="eaa1c-410">*インストールの進行状況*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-410">*Installation progress*</span></span>
6. <span data-ttu-id="eaa1c-411">インストールが完了したらをクリックして**完了**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-411">When the installation completes, click **Finish**.</span></span>

    ![インストールが完了しました](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    <span data-ttu-id="eaa1c-413">*インストールが完了しました*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-413">*Installation completed*</span></span>
7. <span data-ttu-id="eaa1c-414">をクリックして**終了**Web Platform Installer を閉じます。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-414">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="eaa1c-415">Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、順にクリックして、 **VS Express for Web**並べて表示します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-415">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express Web タイルを](aspnet-mvc-4-models-and-data-access/_static/image30.png)

    <span data-ttu-id="eaa1c-417">*VS Express Web タイルを*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-417">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="eaa1c-418">付録 b: Web Deploy を使用して ASP.NET MVC 4 アプリケーションを発行します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-418">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="eaa1c-419">この付録では、Windows Azure 管理ポータルから新しい web サイトを作成し、Windows Azure によって提供される Web 配置発行機能を活用して、次の演習では、取得したアプリケーションを公開する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-419">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="eaa1c-420">タスク 1 - Windows から新しい Web サイトを作成する Azure ポータル</span><span class="sxs-lookup"><span data-stu-id="eaa1c-420">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="eaa1c-421">移動して、 [Windows Azure 管理ポータル](https://manage.windowsazure.com/)サブスクリプションに関連付けられている Microsoft の資格情報を使用してサインインします。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-421">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="eaa1c-422">Windows Azure 無料で 10 個の ASP.NET Web サイトをホストでき、トラフィックの増大に応じて拡大縮小できます。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-422">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="eaa1c-423">サインアップする[ここ](http://aka.ms/aspnet-hol-azure)です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-423">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="eaa1c-424">![Windows Azure ポータルにログオンする](aspnet-mvc-4-models-and-data-access/_static/image31.png "Windows Azure ポータルにログオンします。")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-424">![Log on to Windows Azure portal](aspnet-mvc-4-models-and-data-access/_static/image31.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="eaa1c-425">*Windows Azure 管理ポータルにログオンします。*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-425">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="eaa1c-426">をクリックして**新規**コマンド バーでします。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-426">Click **New** on the command bar.</span></span>

    <span data-ttu-id="eaa1c-427">![新しい Web サイトを作成する](aspnet-mvc-4-models-and-data-access/_static/image32.png "新しい Web サイトを作成します。")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-427">![Creating a new Web Site](aspnet-mvc-4-models-and-data-access/_static/image32.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="eaa1c-428">*新しい Web サイトを作成します。*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-428">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="eaa1c-429">をクリックして**コンピューティング** | **Web サイト**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-429">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="eaa1c-430">選択し、**簡易作成**オプション。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-430">Then select **Quick Create** option.</span></span> <span data-ttu-id="eaa1c-431">新しい web サイトの利用可能な URL を指定して、をクリックして**Web サイトの作成**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-431">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="eaa1c-432">Windows Azure Web サイトは、制御および管理できる、クラウドで実行されている web アプリケーションのホストです。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-432">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="eaa1c-433">簡易作成 オプションでは、完成した web アプリケーションを Windows Azure Web サイトからポータル外を展開することができます。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-433">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="eaa1c-434">データベースを設定するための手順は含まれません。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-434">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="eaa1c-435">![簡易作成を使用して新しい Web サイトを作成する](aspnet-mvc-4-models-and-data-access/_static/image33.png "簡易作成を使用して新しい Web サイトを作成します。")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-435">![Creating a new Web Site using Quick Create](aspnet-mvc-4-models-and-data-access/_static/image33.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="eaa1c-436">*簡易作成を使用して新しい Web サイトを作成します。*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-436">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="eaa1c-437">新しいまで待つ**Web サイト**を作成します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-437">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="eaa1c-438">Web サイトを作成した後は、下のリンクをクリックして、 **URL**列です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-438">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="eaa1c-439">新しい Web サイトが動作していることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-439">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="eaa1c-440">![新しい web サイトを参照して](aspnet-mvc-4-models-and-data-access/_static/image34.png "新しい web サイトを参照")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-440">![Browsing to the new web site](aspnet-mvc-4-models-and-data-access/_static/image34.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="eaa1c-441">*新しい web サイトを参照*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-441">*Browsing to the new web site*</span></span>

    <span data-ttu-id="eaa1c-442">![Web サイトを実行している](aspnet-mvc-4-models-and-data-access/_static/image35.png "を実行している Web サイト")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-442">![Web site running](aspnet-mvc-4-models-and-data-access/_static/image35.png "Web site running")</span></span>

    <span data-ttu-id="eaa1c-443">*実行している web サイト*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-443">*Web site running*</span></span>
6. <span data-ttu-id="eaa1c-444">ポータルに戻るし、下にある web サイトの名前をクリックして、**名前**管理ページを表示する列。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-444">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="eaa1c-445">![Web サイトの管理ページを開く](aspnet-mvc-4-models-and-data-access/_static/image36.png "web サイトの管理ページを開く")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-445">![Opening the web site management pages](aspnet-mvc-4-models-and-data-access/_static/image36.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="eaa1c-446">*Web サイトの管理ページを開く*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-446">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="eaa1c-447">**ダッシュ ボード**] ページの [、**クイック グランス**セクションで、をクリックして、**発行プロファイルのダウンロード**リンクします。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-447">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="eaa1c-448">*発行プロファイル*すべてのパブリケーションを有効になっているメソッドの各 Windows Azure web サイトに web アプリケーションを発行するために必要な情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-448">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="eaa1c-449">発行プロファイルには、Url、ユーザーの資格情報およびに接続し、各発行メソッドが有効になっているエンドポイントに対して認証するために必要なデータベース文字列が含まれています。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-449">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="eaa1c-450">**Microsoft WebMatrix 2**、 **Microsoft Visual Studio Express for Web**と**Microsoft Visual Studio 2012**読み取りのサポートをこれらのプログラムの構成を自動化するプロファイルの発行Windows Azure の web サイトに web アプリケーションを公開します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-450">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="eaa1c-451">![発行プロファイルを web サイトをダウンロードする](aspnet-mvc-4-models-and-data-access/_static/image37.png "発行プロファイルを web サイトをダウンロードします。")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-451">![Downloading the web site publish profile](aspnet-mvc-4-models-and-data-access/_static/image37.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="eaa1c-452">*発行プロファイルを Web サイトをダウンロードします。*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-452">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="eaa1c-453">既知の場所に発行プロファイル ファイルをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-453">Download the publish profile file to a known location.</span></span> <span data-ttu-id="eaa1c-454">さらにこの練習では、このファイルを使用して、Visual Studio から web アプリケーション、Windows Azure Web サイトを発行する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-454">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="eaa1c-455">![発行プロファイル ファイルを保存](aspnet-mvc-4-models-and-data-access/_static/image38.png "発行プロファイルの保存")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-455">![Saving the publish profile file](aspnet-mvc-4-models-and-data-access/_static/image38.png "Saving the publish profile")</span></span>

    <span data-ttu-id="eaa1c-456">*発行プロファイル ファイルを保存します。*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-456">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="eaa1c-457">タスク 2 - データベース サーバーの構成</span><span class="sxs-lookup"><span data-stu-id="eaa1c-457">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="eaa1c-458">アプリケーションが SQL Server を使用する場合は、データベースの SQL データベース サーバーを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-458">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="eaa1c-459">SQL Server を使用しない簡単なアプリケーションを展開する場合は、このタスクをスキップする場合があります。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-459">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="eaa1c-460">SQL データベース サーバーは、アプリケーション データベースを格納する必要があります。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-460">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="eaa1c-461">SQL データベース サーバーを表示するには、Windows Azure 管理ポータルで自分のサブスクリプションから**Sql データベース** | **サーバー** | **サーバーのダッシュ ボード**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-461">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="eaa1c-462">使用して 1 つを作成するには作成されたサーバーがない、**追加**コマンド バーのボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-462">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="eaa1c-463">注意してください、**サーバー名および URL、管理者のログイン名とパスワード**、次のタスクで使用することができます。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-463">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="eaa1c-464">データベースを作成しない、まだと後の段階で作成されます。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-464">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="eaa1c-465">![SQL データベース サーバーのダッシュ ボード](aspnet-mvc-4-models-and-data-access/_static/image39.png "SQL データベース サーバーのダッシュ ボード")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-465">![SQL Database Server Dashboard](aspnet-mvc-4-models-and-data-access/_static/image39.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="eaa1c-466">*SQL データベース サーバーのダッシュ ボード*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-466">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="eaa1c-467">次のタスクでは、サーバーの一覧に、ローカル IP アドレスを含める必要があるため、Visual Studio からデータベース接続をテストします**使用できる IP アドレス**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-467">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="eaa1c-468">実行するには、クリックして**構成**から IP アドレスを選択する**現在のクライアント IP アドレス**に貼り付けます、**開始 IP アドレス**と**終了IPアドレス**のテキスト ボックスをクリックして、 ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png)ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-468">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) button.</span></span>

    ![クライアントの IP アドレスを追加します。](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    <span data-ttu-id="eaa1c-470">*クライアントの IP アドレスを追加します。*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-470">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="eaa1c-471">1 回、**クライアント IP アドレス**が許可される IP アドレスに追加一覧で、をクリックして**保存**変更を確認します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-471">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![変更を確認します。](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    <span data-ttu-id="eaa1c-473">*変更を確認します。*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-473">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="eaa1c-474">タスク 3 - Web Deploy を使用して ASP.NET MVC 4 アプリケーションの発行</span><span class="sxs-lookup"><span data-stu-id="eaa1c-474">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="eaa1c-475">ASP.NET MVC 4 ソリューションに戻ります。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-475">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="eaa1c-476">**ソリューション エクスプ ローラー**を web サイト プロジェクトを右クリックし、**発行**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-476">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="eaa1c-477">![アプリケーションの発行](aspnet-mvc-4-models-and-data-access/_static/image43.png "アプリケーションの発行")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-477">![Publishing the Application](aspnet-mvc-4-models-and-data-access/_static/image43.png "Publishing the Application")</span></span>

    <span data-ttu-id="eaa1c-478">*Web サイトの発行*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-478">*Publishing the web site*</span></span>
2. <span data-ttu-id="eaa1c-479">最初の作業を保存した発行プロファイルをインポートします。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-479">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="eaa1c-480">![発行プロファイルをインポートする](aspnet-mvc-4-models-and-data-access/_static/image44.png "発行プロファイルをインポートします。")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-480">![Importing the publish profile](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importing the publish profile")</span></span>

    <span data-ttu-id="eaa1c-481">*発行プロファイルのインポート*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-481">*Importing publish profile*</span></span>
3. <span data-ttu-id="eaa1c-482">をクリックして**への接続検証**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-482">Click **Validate Connection**.</span></span> <span data-ttu-id="eaa1c-483">検証が完了したらクリックして**次**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-483">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="eaa1c-484">検証が完了すると、接続の検証 ボタンの横に表示される緑色のチェック マークを参照してください。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-484">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="eaa1c-485">![接続の検証](aspnet-mvc-4-models-and-data-access/_static/image45.png "接続の検証")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-485">![Validating connection](aspnet-mvc-4-models-and-data-access/_static/image45.png "Validating connection")</span></span>

    <span data-ttu-id="eaa1c-486">*接続の検証*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-486">*Validating connection*</span></span>
4. <span data-ttu-id="eaa1c-487">**設定**] ページの [、**データベース**セクションで、データベース接続のテキスト ボックスの横にあるボタンをクリックして (つまり**DefaultConnection**)。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-487">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="eaa1c-488">![Web 配置の構成](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web 配置の構成")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-488">![Web deploy configuration](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web deploy configuration")</span></span>

    <span data-ttu-id="eaa1c-489">*Web 配置の構成*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-489">*Web deploy configuration*</span></span>
5. <span data-ttu-id="eaa1c-490">次のように、データベースの接続を構成します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-490">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="eaa1c-491">**サーバー名**SQL データベース サーバーの URL を使用して、入力、 *tcp:* プレフィックス。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-491">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="eaa1c-492">**ユーザー名**サーバー管理者のログイン名を入力します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-492">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="eaa1c-493">**パスワード**サーバー管理者のログイン パスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-493">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="eaa1c-494">新しいデータベース名を入力します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-494">Type a new database name.</span></span>

     <span data-ttu-id="eaa1c-495">![対象の接続文字列を構成する](aspnet-mvc-4-models-and-data-access/_static/image47.png "対象の接続文字列を構成します。")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-495">![Configuring destination connection string](aspnet-mvc-4-models-and-data-access/_static/image47.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="eaa1c-496">*対象の接続文字列を構成します。*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-496">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="eaa1c-497">次に、 **[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-497">Then click **OK**.</span></span> <span data-ttu-id="eaa1c-498">データベースをクリックを作成するように求められたら**はい**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-498">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="eaa1c-499">![データベースを作成する](aspnet-mvc-4-models-and-data-access/_static/image48.png "データベース文字列を作成します。")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-499">![Creating the database](aspnet-mvc-4-models-and-data-access/_static/image48.png "Creating the database string")</span></span>

    <span data-ttu-id="eaa1c-500">*データベースの作成*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-500">*Creating the database*</span></span>
7. <span data-ttu-id="eaa1c-501">接続の既定のテキスト ボックス内では、Windows azure SQL データベースへの接続に使用する接続文字列が表示されます。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-501">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="eaa1c-502">その後、 **[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-502">Then click **Next**.</span></span>

    <span data-ttu-id="eaa1c-503">![SQL データベースを指す接続文字列](aspnet-mvc-4-models-and-data-access/_static/image49.png "SQL データベースを指す接続文字列")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-503">![Connection string pointing to SQL Database](aspnet-mvc-4-models-and-data-access/_static/image49.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="eaa1c-504">*SQL データベースを指す接続文字列*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-504">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="eaa1c-505">**プレビュー** ] ページで [**発行**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-505">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="eaa1c-506">![Web アプリケーションの発行](aspnet-mvc-4-models-and-data-access/_static/image50.png "web アプリケーションの発行")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-506">![Publishing the web application](aspnet-mvc-4-models-and-data-access/_static/image50.png "Publishing the web application")</span></span>

    <span data-ttu-id="eaa1c-507">*Web アプリケーションの発行*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-507">*Publishing the web application*</span></span>
9. <span data-ttu-id="eaa1c-508">発行プロセスが終了すると、既定のブラウザーが公開された web サイトを開きます。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-508">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="eaa1c-509">付録 c: コード スニペットの使用</span><span class="sxs-lookup"><span data-stu-id="eaa1c-509">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="eaa1c-510">コード スニペットでは、すぐに利用できる必要があるすべてのコードがあります。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-510">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="eaa1c-511">ラボ ドキュメントがわかります正確に使用する場合が、次の図に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-511">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="eaa1c-512">![Visual Studio のコード スニペットを使用して、プロジェクトにコードを挿入する](aspnet-mvc-4-models-and-data-access/_static/image51.png "プロジェクトにコードを挿入する Visual Studio を使ってコード スニペット")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-512">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-models-and-data-access/_static/image51.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="eaa1c-513">*Visual Studio のコード スニペットを使用して、プロジェクトにコードを挿入するには*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-513">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="eaa1c-514">***キーボード (C# の場合のみ) を使用してコード スニペットを追加するには***</span><span class="sxs-lookup"><span data-stu-id="eaa1c-514">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="eaa1c-515">コードを挿入するには、カーソルを置きます。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-515">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="eaa1c-516">(なし、スペースやハイフン) スニペット名を入力してを起動します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-516">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="eaa1c-517">スニペットの名前に一致する IntelliSense 表示を確認します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-517">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="eaa1c-518">正しいスニペットを選択 (または、全体のスニペットの名前を選択するまで」と入力してください)。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-518">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="eaa1c-519">カーソル位置にスニペットを挿入するには、2 回、Tab キーを押します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-519">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="eaa1c-520">![スニペット名を入力する開始](aspnet-mvc-4-models-and-data-access/_static/image52.png "スニペット名の入力を開始")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-520">![Start typing the snippet name](aspnet-mvc-4-models-and-data-access/_static/image52.png "Start typing the snippet name")</span></span>

<span data-ttu-id="eaa1c-521">*スニペット名の入力を開始します。*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-521">*Start typing the snippet name*</span></span>

<span data-ttu-id="eaa1c-522">![Tab キーを押して、強調表示されているスニペットを選択する](aspnet-mvc-4-models-and-data-access/_static/image53.png "強調表示されているスニペットを選択するキーを押してタブ")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-522">![Press Tab to select the highlighted snippet](aspnet-mvc-4-models-and-data-access/_static/image53.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="eaa1c-523">*Tab キーを押して、強調表示されているスニペットを選択するには*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-523">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="eaa1c-524">![キーを押して タブで再度と、スニペットが展開](aspnet-mvc-4-models-and-data-access/_static/image54.png "キーを押して タブで再度と、スニペットが展開されます")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-524">![Press Tab again and the snippet will expand](aspnet-mvc-4-models-and-data-access/_static/image54.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="eaa1c-525">*キーを押して タブで再度と、スニペットが展開されます。*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-525">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="eaa1c-526">***(C#、Visual Basic および XML) にマウスを使用してコード スニペットを追加する***1 です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-526">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="eaa1c-527">コード スニペットを挿入する場所を右クリックします。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-527">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="eaa1c-528">選択**スニペットの挿入**続く**マイ コード スニペット**です。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-528">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="eaa1c-529">クリックして一覧から、関連するスニペットを選択します。</span><span class="sxs-lookup"><span data-stu-id="eaa1c-529">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="eaa1c-530">![コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリックして](aspnet-mvc-4-models-and-data-access/_static/image55.png "コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリック")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-530">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-models-and-data-access/_static/image55.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="eaa1c-531">*コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリックします。*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-531">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="eaa1c-532">![クリックして一覧から、関連するスニペットを選択](aspnet-mvc-4-models-and-data-access/_static/image56.png "クリックして一覧から、関連するスニペットを選択")</span><span class="sxs-lookup"><span data-stu-id="eaa1c-532">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-models-and-data-access/_static/image56.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="eaa1c-533">*クリックして一覧から、関連するスニペットを選択します。*</span><span class="sxs-lookup"><span data-stu-id="eaa1c-533">*Pick the relevant snippet from the list, by clicking on it*</span></span>
