---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 'パート 4: モデルとデータ アクセス |Microsoft Docs'
author: jongalloway
description: このチュートリアル シリーズでは、すべての ASP.NET MVC のミュージック ストア サンプル アプリケーションをビルドする手順について説明します。 パート 4 では、モデルとデータ アクセスについて説明します。
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 023350e882afe049ce3800921825b1b2bec8e415
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818954"
---
<a name="part-4-models-and-data-access"></a><span data-ttu-id="5e9c8-104">パート 4: モデルとデータ アクセス</span><span class="sxs-lookup"><span data-stu-id="5e9c8-104">Part 4: Models and Data Access</span></span>
====================
<span data-ttu-id="5e9c8-105">によって[Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="5e9c8-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="5e9c8-106">MVC のミュージック ストアは、チュートリアル アプリケーションを紹介し、web 開発用の ASP.NET MVC と Visual Studio を使用する方法をステップ バイ ステップについて説明します。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="5e9c8-107">MVC のミュージック ストアは、オンラインで音楽のアルバムを販売し、基本的なサイトの管理、ユーザー サインインし、買い物カゴの機能を実装する軽量サンプル ストア実装です。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>
> 
> <span data-ttu-id="5e9c8-108">このチュートリアル シリーズでは、すべての ASP.NET MVC のミュージック ストア サンプル アプリケーションをビルドする手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="5e9c8-109">パート 4 では、モデルとデータ アクセスについて説明します。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-109">Part 4 covers Models and Data Access.</span></span>


<span data-ttu-id="5e9c8-110">これまでにしただけされて渡して「ダミー データ」、コント ローラーからテンプレートを表示します。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-110">So far, we've just been passing "dummy data" from our Controllers to our View templates.</span></span> <span data-ttu-id="5e9c8-111">これで、実際のデータベースをフックする準備ができました。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-111">Now we're ready to hook up a real database.</span></span> <span data-ttu-id="5e9c8-112">このチュートリアルでのテーマは、SQL Server Compact Edition (SQL CE とよく呼ばれます) を使用する方法として、データベース エンジン。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-112">In this tutorial we'll be covering how to use SQL Server Compact Edition (often called SQL CE) as our database engine.</span></span> <span data-ttu-id="5e9c8-113">SQL CE は、任意のインストールや構成は、ローカルの開発は非常に便利ですが不要の無料の埋め込み、ファイルのベース データベースです。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-113">SQL CE is a free, embedded, file based database that doesn't require any installation or configuration, which makes it really convenient for local development.</span></span>

## <a name="database-access-with-entity-framework-code-first"></a><span data-ttu-id="5e9c8-114">データベースへのアクセスに Entity Framework Code First</span><span class="sxs-lookup"><span data-stu-id="5e9c8-114">Database access with Entity Framework Code-First</span></span>

<span data-ttu-id="5e9c8-115">クエリを実行し、データベースを更新する ASP.NET MVC 3 プロジェクトに含まれている Entity Framework (EF) のサポートを使用します。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-115">We'll use the Entity Framework (EF) support that is included in ASP.NET MVC 3 projects to query and update the database.</span></span> <span data-ttu-id="5e9c8-116">EF は、柔軟なオブジェクト リレーショナル マッピング (ORM) データ開発者は、オブジェクト指向の手法で、データベースに保存データのクエリと更新 API です。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-116">EF is a flexible object relational mapping (ORM) data API that enables developers to query and update data stored in a database in an object-oriented way.</span></span>

<span data-ttu-id="5e9c8-117">Entity Framework バージョン 4 では、code first と呼ばれる開発パラダイムをサポートします。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-117">Entity Framework version 4 supports a development paradigm called code-first.</span></span> <span data-ttu-id="5e9c8-118">Code first では、単純なクラスとも呼ばれる POCO ("plain-old"CLR object から) を作成してモデル オブジェクトを作成することができ、クラスからその場でデータベースを作成することもします。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-118">Code-first allows you to create model object by writing simple classes (also known as POCO from "plain-old" CLR objects), and can even create the database on the fly from your classes.</span></span>

### <a name="changes-to-our-model-classes"></a><span data-ttu-id="5e9c8-119">モデル クラスへの変更</span><span class="sxs-lookup"><span data-stu-id="5e9c8-119">Changes to our Model Classes</span></span>

<span data-ttu-id="5e9c8-120">私たちを利用する Entity Framework でのデータベース作成機能このチュートリアルでは。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-120">We will be leveraging the database creation feature in Entity Framework in this tutorial.</span></span> <span data-ttu-id="5e9c8-121">その前に、モデル クラスには、後で使用するものを追加するいくつかの軽微な変更を作成、みましょう。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-121">Before we do that, though, let's make a few minor changes to our model classes to add in some things we'll be using later on.</span></span>

#### <a name="adding-the-artist-model-classes"></a><span data-ttu-id="5e9c8-122">アーティストのモデル クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-122">Adding the Artist Model Classes</span></span>

<span data-ttu-id="5e9c8-123">アルバムに関連付けるアーティスト、ため、アーティストを記述する単純なモデル クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-123">Our Albums will be associated with Artists, so we'll add a simple model class to describe an Artist.</span></span> <span data-ttu-id="5e9c8-124">次に示すコードを使用して Artist.cs という名前の Models フォルダーに新しいクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-124">Add a new class to the Models folder named Artist.cs using the code shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a><span data-ttu-id="5e9c8-125">モデル クラスを更新しています</span><span class="sxs-lookup"><span data-stu-id="5e9c8-125">Updating our Model Classes</span></span>

<span data-ttu-id="5e9c8-126">アルバム クラスは、次に示すように更新します。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-126">Update the Album class as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

<span data-ttu-id="5e9c8-127">次に、ジャンル クラスに次の更新プログラムを加えます。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-127">Next, make the following updates to the Genre class.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-appdata-folder"></a><span data-ttu-id="5e9c8-128">アプリの追加\_データ フォルダー</span><span class="sxs-lookup"><span data-stu-id="5e9c8-128">Adding the App\_Data folder</span></span>

<span data-ttu-id="5e9c8-129">アプリを追加します\_データ ディレクトリ、SQL Server Express のデータベース ファイルを保持するために、プロジェクトにします。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-129">We'll add an App\_Data directory to our project to hold our SQL Server Express database files.</span></span> <span data-ttu-id="5e9c8-130">アプリ\_データが既にデータベースへのアクセスの適切なセキュリティ アクセス許可のある ASP.NET の特殊なディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-130">App\_Data is a special directory in ASP.NET which already has the correct security access permissions for database access.</span></span> <span data-ttu-id="5e9c8-131">[プロジェクト] メニューから選択し、ASP.NET フォルダーの追加、アプリ\_データ。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-131">From the Project menu, select Add ASP.NET Folder, then App\_Data.</span></span>

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a><span data-ttu-id="5e9c8-132">Web.config ファイルで接続文字列の作成</span><span class="sxs-lookup"><span data-stu-id="5e9c8-132">Creating a Connection String in the web.config file</span></span>

<span data-ttu-id="5e9c8-133">Entity Framework は、データベースに接続する方法を認識できるように、web サイトの構成ファイルに数行を追加します。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-133">We will add a few lines to the website's configuration file so that Entity Framework knows how to connect to our database.</span></span> <span data-ttu-id="5e9c8-134">プロジェクトのルートにある Web.config ファイルをダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-134">Double-click on the Web.config file located in the root of the project.</span></span>

![](mvc-music-store-part-4/_static/image2.png)

<span data-ttu-id="5e9c8-135">このファイルの一番下までスクロールし、追加、 &lt;connectionStrings&gt;次に示すように、最後の行の真上セクションします。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-135">Scroll to the bottom of this file and add a &lt;connectionStrings&gt; section directly above the last line, as shown below.</span></span>

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a><span data-ttu-id="5e9c8-136">コンテキスト クラスの追加</span><span class="sxs-lookup"><span data-stu-id="5e9c8-136">Adding a Context Class</span></span>

<span data-ttu-id="5e9c8-137">Models フォルダーを右クリックし、MusicStoreEntities.cs をという名前の新しいクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-137">Right-click the Models folder and add a new class named MusicStoreEntities.cs.</span></span>

![](mvc-music-store-part-4/_static/image3.png)

<span data-ttu-id="5e9c8-138">このクラスは、Entity Framework データベース コンテキストを表すとは、作成を処理、読み取り、更新、および削除操作をします。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-138">This class will represent the Entity Framework database context, and will handle our create, read, update, and delete operations for us.</span></span> <span data-ttu-id="5e9c8-139">このクラスのコードは、以下に示します。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-139">The code for this class is shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

<span data-ttu-id="5e9c8-140">これで他の構成、特殊なインターフェイスなどはありません。DbContext 基底クラスを拡張することによって、MusicStoreEntities クラスは、私たちにとって、データベース操作を処理できません。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-140">That's it - there's no other configuration, special interfaces, etc. By extending the DbContext base class, our MusicStoreEntities class is able to handle our database operations for us.</span></span> <span data-ttu-id="5e9c8-141">フックするが、これより多くのプロパティをデータベースにいくつかの追加情報を利用するモデル クラスに追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-141">Now that we've got that hooked up, let's add a few more properties to our model classes to take advantage of some of the additional information in our database.</span></span>

### <a name="adding-our-store-catalog-data"></a><span data-ttu-id="5e9c8-142">ストア カタログ データを追加します。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-142">Adding our store catalog data</span></span>

<span data-ttu-id="5e9c8-143">「シード」のデータを新しく作成したデータベースに追加する Entity Framework の機能を活用しましたかかります。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-143">We will take advantage of a feature in Entity Framework which adds "seed" data to a newly created database.</span></span> <span data-ttu-id="5e9c8-144">ジャンル、アーティスト、アルバムの一覧で、ストアのカタログが事前設定されます。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-144">This will pre-populate our store catalog with a list of Genres, Artists, and Albums.</span></span> <span data-ttu-id="5e9c8-145">-これは、このチュートリアルで前に使用される、サイト設計ファイルが含まれている - MvcMusicStore Assets.zip ダウンロードには、コードをという名前のフォルダー内にある、このシード データをクラス ファイルがあります。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-145">The MvcMusicStore-Assets.zip download - which included our site design files used earlier in this tutorial - has a class file with this seed data, located in a folder named Code.</span></span>

<span data-ttu-id="5e9c8-146">コード内で、Models フォルダーが SampleData.cs ファイルを探し、次に示すようにこのプロジェクトで、Models フォルダーにドロップします。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-146">Within the Code / Models folder, locate the SampleData.cs file and drop it into the Models folder in our project, as shown below.</span></span>

![](mvc-music-store-part-4/_static/image4.png)

<span data-ttu-id="5e9c8-147">その SampleData クラスを Entity Framework コードの 1 つの行を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-147">Now we need to add one line of code to tell Entity Framework about that SampleData class.</span></span> <span data-ttu-id="5e9c8-148">開き、アプリケーションを先頭に次の行を追加するプロジェクトのルートでは、Global.asax ファイルをダブルクリックして\_メソッドを開始します。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-148">Double-click on the Global.asax file in the root of the project to open it and add the following line to the top the Application\_Start method.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

<span data-ttu-id="5e9c8-149">この時点で、プロジェクトの Entity Framework を構成するために必要な作業が終了しました。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-149">At this point, we've completed the work necessary to configure Entity Framework for our project.</span></span>

## <a name="querying-the-database"></a><span data-ttu-id="5e9c8-150">データベースに対するクエリの実行</span><span class="sxs-lookup"><span data-stu-id="5e9c8-150">Querying the Database</span></span>

<span data-ttu-id="5e9c8-151">ここで「ダミー データ」を使用する代わりに代わりに呼び出すようにすべての情報を照会するマイクロソフトのデータベースに、StoreController を更新しましょう。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-151">Now let's update our StoreController so that instead of using "dummy data" it instead calls into our database to query all of its information.</span></span> <span data-ttu-id="5e9c8-152">フィールドを宣言することから始めます、 **StoreController** storeDB という名前の MusicStoreEntities クラスのインスタンスを保持します。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-152">We'll start by declaring a field on the **StoreController** to hold an instance of the MusicStoreEntities class, named storeDB:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a><span data-ttu-id="5e9c8-153">データベースにクエリのストア インデックスの更新</span><span class="sxs-lookup"><span data-stu-id="5e9c8-153">Updating the Store Index to query the database</span></span>

<span data-ttu-id="5e9c8-154">MusicStoreEntities クラスは Entity Framework によって保持され、データベース内の各テーブルのコレクション プロパティを公開します。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-154">The MusicStoreEntities class is maintained by the Entity Framework and exposes a collection property for each table in our database.</span></span> <span data-ttu-id="5e9c8-155">データベース内のすべてのジャンルを取得する、StoreController の Index アクションを更新してみましょう。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-155">Let's update our StoreController's Index action to retrieve all Genres in our database.</span></span> <span data-ttu-id="5e9c8-156">以前これを行う文字列データをハード コーディングします。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-156">Previously we did this by hard-coding string data.</span></span> <span data-ttu-id="5e9c8-157">代わりにだけを使えます Entity Framework コンテキスト Generes コレクション。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-157">Now we can instead just use the Entity Framework context Generes collection:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

<span data-ttu-id="5e9c8-158">だけを返しているライブ データ データベースから今すぐ前に、返された同じ StoreIndexViewModel まま返しているので、ビュー テンプレートで発生する変更が不要です。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-158">No changes need to happen to our View template since we're still returning the same StoreIndexViewModel we returned before - we're just returning live data from our database now.</span></span>

<span data-ttu-id="5e9c8-159">プロジェクトをもう一度実行して、"/store"の URL を参照してください。 データベースにすべてのジャンルの一覧が表示されますようになりました。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-159">When we run the project again and visit the "/Store" URL, we'll now see a list of all Genres in our database:</span></span>

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a><span data-ttu-id="5e9c8-160">ストアの参照や詳細情報を使用して、ライブ データを更新しています</span><span class="sxs-lookup"><span data-stu-id="5e9c8-160">Updating Store Browse and Details to use live data</span></span>

<span data-ttu-id="5e9c8-161">ストア/参照で? ジャンル =*[一部ジャンル]* 、アクション メソッドを検索するジャンルを名前でします。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-161">With the /Store/Browse?genre=*[some-genre]* action method, we're searching for a Genre by name.</span></span> <span data-ttu-id="5e9c8-162">のみが期待 1 つの結果、これまでジャンルの同名の 2 つのエントリがあるべきではないため、使用できるようにします。このような適切なジャンル オブジェクトのクエリを LINQ で Single() 拡張機能 (入力せずにこのまだ)。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-162">We only expect one result, since we shouldn't ever have two entries for the same Genre name, and so we can use the .Single() extension in LINQ to query for the appropriate Genre object like this (don't type this yet):</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

<span data-ttu-id="5e9c8-163">1 つのメソッドは、その名前が定義した値と一致するよう、1 つのジャンル オブジェクトすることを指定するパラメーターとしてラムダ式を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-163">The Single method takes a Lambda expression as a parameter, which specifies that we want a single Genre object such that its name matches the value we've defined.</span></span> <span data-ttu-id="5e9c8-164">上記のケースで Disco を一致する名前値の 1 つのジャンル オブジェクト読み込みです。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-164">In the case above, we are loading a single Genre object with a Name value matching Disco.</span></span>

<span data-ttu-id="5e9c8-165">ジャンル オブジェクトを取得するときにも読み込まれたたいその他の関連エンティティを指定できるようにする Entity Framework 機能をご説明しましょう。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-165">We'll take advantage of an Entity Framework feature that allows us to indicate other related entities we want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="5e9c8-166">この機能は、クエリ結果のシェイプは呼び出され、により、すべての必要な情報を取得するデータベースにアクセスする必要があります回数の合計を小さくこと。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-166">This feature is called Query Result Shaping, and enables us to reduce the number of times we need to access the database to retrieve all of the information we need.</span></span> <span data-ttu-id="5e9c8-167">関連アルバムもすることを示す Genres.Include("Albums") からは、前述のクエリを更新するために、アルバム、ジャンルを取得しますをプリフェッチします。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-167">We want to pre-fetch the Albums for Genre we retrieve, so we'll update our query to include from Genres.Include("Albums") to indicate that we want related albums as well.</span></span> <span data-ttu-id="5e9c8-168">これは、1 つのデータベースの要求で、ジャンルとアルバムの両方のデータを取得するためより効率的です。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-168">This is more efficient, since it will retrieve both our Genre and Album data in a single database request.</span></span>

<span data-ttu-id="5e9c8-169">説明を次の更新された参照のコント ローラー アクションの外観に示します。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-169">With the explanations out of the way, here's how our updated Browse controller action looks:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

<span data-ttu-id="5e9c8-170">各ジャンルにあるアルバムを表示するストア参照ビューを更新できます。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-170">We can now update the Store Browse View to display the albums which are available in each Genre.</span></span> <span data-ttu-id="5e9c8-171">ビュー テンプレート (内で見つかった/Views/Store/Browse.cshtml) を開き、次に示すように、アルバムの箇条書きを追加します。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-171">Open the view template (found in /Views/Store/Browse.cshtml) and add a bulleted list of Albums as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

<span data-ttu-id="5e9c8-172">アプリケーションの実行、/ストア/参照に移動するとしますか? ジャンル = 結果は、選択されたジャンルのすべてのアルバムを表示する、データベースからプルされるようになりましたこと Jazz を示しています。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-172">Running our application and browsing to /Store/Browse?genre=Jazz shows that our results are now being pulled from the database, displaying all albums in our selected Genre.</span></span>

![](mvc-music-store-part-4/_static/image2.jpg)

<span data-ttu-id="5e9c8-173">同じ、/Store/詳細/[id] URL に変更し、ダミー データ ID を持つパラメーター値に一致するアルバムに読み込み、データベース クエリを置き換えることを行います。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-173">We'll make the same change to our /Store/Details/[id] URL, and replace our dummy data with a database query which loads an Album whose ID matches the parameter value.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

<span data-ttu-id="5e9c8-174">アプリケーションの実行、/Store/Details/1 に移動して、結果がデータベースから取得されるようになりましたことを示します。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-174">Running our application and browsing to /Store/Details/1 shows that our results are now being pulled from the database.</span></span>

![](mvc-music-store-part-4/_static/image5.png)

<span data-ttu-id="5e9c8-175">できたので、ストアの詳細ページは、アルバムの ID で、アルバムを表示する設定は、更新、**参照**詳細ビューへのリンクを表示します。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-175">Now that our Store Details page is set up to display an album by the Album ID, let's update the **Browse** view to link to the Details view.</span></span> <span data-ttu-id="5e9c8-176">Html.ActionLink、ストアのインデックスから、前のセクションの末尾にストアを参照するリンクを扱ったように正確に使用します。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-176">We will use Html.ActionLink, exactly as we did to link from Store Index to Store Browse at the end of the previous section.</span></span> <span data-ttu-id="5e9c8-177">[参照] ビューの完全なソースは、以下が表示されます。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-177">The complete source for the Browse view appears below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

<span data-ttu-id="5e9c8-178">ジャンル ページで、使用可能なアルバムの一覧を [ストア] ページから参照できないできましたし、アルバムをクリックしてそのアルバムの詳細を表示できます。</span><span class="sxs-lookup"><span data-stu-id="5e9c8-178">We're now able to browse from our Store page to a Genre page, which lists the available albums, and by clicking on an album we can view details for that album.</span></span>

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> <span data-ttu-id="5e9c8-179">[前へ](mvc-music-store-part-3.md)
> [次へ](mvc-music-store-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="5e9c8-179">[Previous](mvc-music-store-part-3.md)
[Next](mvc-music-store-part-5.md)</span></span>
