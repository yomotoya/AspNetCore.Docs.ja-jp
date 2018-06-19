---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 'パート 4: モデルおよびデータ アクセス |Microsoft ドキュメント'
author: jongalloway
description: このチュートリアルの系列では、すべての ASP.NET MVC Music Store サンプル アプリケーションをビルドする手順について説明します。 パート 4 では、モデルおよびデータ アクセスについて説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 76671bbc7050d111b4d156c45584ba5aa4f1ea8f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879479"
---
<a name="part-4-models-and-data-access"></a><span data-ttu-id="16ed5-104">パート 4: モデルおよびデータ アクセス</span><span class="sxs-lookup"><span data-stu-id="16ed5-104">Part 4: Models and Data Access</span></span>
====================
<span data-ttu-id="16ed5-105">によって[Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="16ed5-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="16ed5-106">MVC Music Store は、チュートリアル アプリケーションを紹介し、ASP.NET MVC と Visual Studio web 開発を使用する方法の詳細な手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="16ed5-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="16ed5-107">MVC 音楽ストアは、オンラインにして、音楽のアルバムを販売し、基本的なサイトの管理、ユーザー サインインおよび買い物カゴの機能を実装する軽量なサンプル ストアの実装です。</span><span class="sxs-lookup"><span data-stu-id="16ed5-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>
> 
> <span data-ttu-id="16ed5-108">このチュートリアルの系列では、すべての ASP.NET MVC Music Store サンプル アプリケーションをビルドする手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="16ed5-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="16ed5-109">パート 4 では、モデルおよびデータ アクセスについて説明します。</span><span class="sxs-lookup"><span data-stu-id="16ed5-109">Part 4 covers Models and Data Access.</span></span>


<span data-ttu-id="16ed5-110">これまでおしただけされてデータを渡す"ダミー"、コント ローラーからテンプレートを表示します。</span><span class="sxs-lookup"><span data-stu-id="16ed5-110">So far, we've just been passing "dummy data" from our Controllers to our View templates.</span></span> <span data-ttu-id="16ed5-111">実際のデータベースをフックする準備ができました。</span><span class="sxs-lookup"><span data-stu-id="16ed5-111">Now we're ready to hook up a real database.</span></span> <span data-ttu-id="16ed5-112">このチュートリアルでのテーマは、SQL Server Compact Edition (SQL CE と呼びます) を使用する方法、データベース エンジンとします。</span><span class="sxs-lookup"><span data-stu-id="16ed5-112">In this tutorial we'll be covering how to use SQL Server Compact Edition (often called SQL CE) as our database engine.</span></span> <span data-ttu-id="16ed5-113">SQL CE は、任意のインストールまたは構成で、ローカルの開発は本当に容易に必要としない、無料の埋め込み、ファイルのベース データベースです。</span><span class="sxs-lookup"><span data-stu-id="16ed5-113">SQL CE is a free, embedded, file based database that doesn't require any installation or configuration, which makes it really convenient for local development.</span></span>

## <a name="database-access-with-entity-framework-code-first"></a><span data-ttu-id="16ed5-114">データベースへのアクセスと Entity Framework コード優先</span><span class="sxs-lookup"><span data-stu-id="16ed5-114">Database access with Entity Framework Code-First</span></span>

<span data-ttu-id="16ed5-115">クエリおよびデータベースを更新する ASP.NET MVC 3 プロジェクトに含まれている Entity Framework (EF) のサポートを使用します。</span><span class="sxs-lookup"><span data-stu-id="16ed5-115">We'll use the Entity Framework (EF) support that is included in ASP.NET MVC 3 projects to query and update the database.</span></span> <span data-ttu-id="16ed5-116">EF は、柔軟なオブジェクト リレーショナル マッピング (ORM) データを開発者が、オブジェクト指向の方法でデータベースに格納されているデータのクエリと更新プログラムを有効にする API です。</span><span class="sxs-lookup"><span data-stu-id="16ed5-116">EF is a flexible object relational mapping (ORM) data API that enables developers to query and update data stored in a database in an object-oriented way.</span></span>

<span data-ttu-id="16ed5-117">Entity Framework version 4 には、コード優先と呼ばれる開発パラダイムがサポートしています。</span><span class="sxs-lookup"><span data-stu-id="16ed5-117">Entity Framework version 4 supports a development paradigm called code-first.</span></span> <span data-ttu-id="16ed5-118">コード優先では、単純なクラスとも呼ばれる POCO ("プレーン old"CLR オブジェクトから) を作成してモデル オブジェクトを作成することができ、クラスから実行時にデータベースを作成することもできます。</span><span class="sxs-lookup"><span data-stu-id="16ed5-118">Code-first allows you to create model object by writing simple classes (also known as POCO from "plain-old" CLR objects), and can even create the database on the fly from your classes.</span></span>

### <a name="changes-to-our-model-classes"></a><span data-ttu-id="16ed5-119">モデル クラスへの変更</span><span class="sxs-lookup"><span data-stu-id="16ed5-119">Changes to our Model Classes</span></span>

<span data-ttu-id="16ed5-120">おを利用して、データベース作成機能 Entity Framework でこのチュートリアルでします。</span><span class="sxs-lookup"><span data-stu-id="16ed5-120">We will be leveraging the database creation feature in Entity Framework in this tutorial.</span></span> <span data-ttu-id="16ed5-121">その前は、後で使用するいくつかの点で追加するには、このモデル クラスをいくつかの軽微な変更を行うただし、みましょう。</span><span class="sxs-lookup"><span data-stu-id="16ed5-121">Before we do that, though, let's make a few minor changes to our model classes to add in some things we'll be using later on.</span></span>

#### <a name="adding-the-artist-model-classes"></a><span data-ttu-id="16ed5-122">アーティスト モデル クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="16ed5-122">Adding the Artist Model Classes</span></span>

<span data-ttu-id="16ed5-123">アルバムは単純なモデル クラスをアーティストに依頼を記述する追加されますので、アーティストに関連付けられます。</span><span class="sxs-lookup"><span data-stu-id="16ed5-123">Our Albums will be associated with Artists, so we'll add a simple model class to describe an Artist.</span></span> <span data-ttu-id="16ed5-124">新しいクラスを次に示すコードを使用して Artist.cs をという名前のモデル フォルダーに追加します。</span><span class="sxs-lookup"><span data-stu-id="16ed5-124">Add a new class to the Models folder named Artist.cs using the code shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a><span data-ttu-id="16ed5-125">モデル クラスの更新</span><span class="sxs-lookup"><span data-stu-id="16ed5-125">Updating our Model Classes</span></span>

<span data-ttu-id="16ed5-126">アルバム クラスは、次に示すように更新します。</span><span class="sxs-lookup"><span data-stu-id="16ed5-126">Update the Album class as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

<span data-ttu-id="16ed5-127">次に、ジャンル クラスに次の更新を行います。</span><span class="sxs-lookup"><span data-stu-id="16ed5-127">Next, make the following updates to the Genre class.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-appdata-folder"></a><span data-ttu-id="16ed5-128">アプリを追加する\_データ フォルダー</span><span class="sxs-lookup"><span data-stu-id="16ed5-128">Adding the App\_Data folder</span></span>

<span data-ttu-id="16ed5-129">アプリを追加します\_SQL Server Express データベース ファイルを保持するために、プロジェクトにデータ ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="16ed5-129">We'll add an App\_Data directory to our project to hold our SQL Server Express database files.</span></span> <span data-ttu-id="16ed5-130">アプリ\_データが既にデータベースへのアクセスの適切なセキュリティ アクセス許可を持っている ASP.NET の特別なディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="16ed5-130">App\_Data is a special directory in ASP.NET which already has the correct security access permissions for database access.</span></span> <span data-ttu-id="16ed5-131">[プロジェクト] メニューから選択し、ASP.NET フォルダーの追加、アプリ\_データ。</span><span class="sxs-lookup"><span data-stu-id="16ed5-131">From the Project menu, select Add ASP.NET Folder, then App\_Data.</span></span>

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a><span data-ttu-id="16ed5-132">Web.config ファイルで接続文字列の作成</span><span class="sxs-lookup"><span data-stu-id="16ed5-132">Creating a Connection String in the web.config file</span></span>

<span data-ttu-id="16ed5-133">Entity Framework は、データベースに接続する方法を認識できるように、web サイトの構成ファイルにいくつかの行を追加おされます。</span><span class="sxs-lookup"><span data-stu-id="16ed5-133">We will add a few lines to the website's configuration file so that Entity Framework knows how to connect to our database.</span></span> <span data-ttu-id="16ed5-134">プロジェクトのルートにある Web.config ファイルをダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="16ed5-134">Double-click on the Web.config file located in the root of the project.</span></span>

![](mvc-music-store-part-4/_static/image2.png)

<span data-ttu-id="16ed5-135">このファイルの一番下までスクロールし、追加、 &lt;connectionStrings&gt;次に示すように、最後の行の上に直接セクションです。</span><span class="sxs-lookup"><span data-stu-id="16ed5-135">Scroll to the bottom of this file and add a &lt;connectionStrings&gt; section directly above the last line, as shown below.</span></span>

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a><span data-ttu-id="16ed5-136">コンテキスト クラスの追加</span><span class="sxs-lookup"><span data-stu-id="16ed5-136">Adding a Context Class</span></span>

<span data-ttu-id="16ed5-137">モデル フォルダーを右クリックし、MusicStoreEntities.cs をという名前の新しいクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="16ed5-137">Right-click the Models folder and add a new class named MusicStoreEntities.cs.</span></span>

![](mvc-music-store-part-4/_static/image3.png)

<span data-ttu-id="16ed5-138">このクラスは、Entity Framework データベースのコンテキストを表すとは、作成を処理、読み取り、更新、および削除操作ご利用の米国。</span><span class="sxs-lookup"><span data-stu-id="16ed5-138">This class will represent the Entity Framework database context, and will handle our create, read, update, and delete operations for us.</span></span> <span data-ttu-id="16ed5-139">このクラスのコードは、以下に示します。</span><span class="sxs-lookup"><span data-stu-id="16ed5-139">The code for this class is shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

<span data-ttu-id="16ed5-140">それで完了です - その他の構成、特殊なインターフェイスなどはありません。DbContext の基本クラスを拡張するには、MusicStoreEntities クラスは、ご利用の米国のデータベース操作を処理することです。</span><span class="sxs-lookup"><span data-stu-id="16ed5-140">That's it - there's no other configuration, special interfaces, etc. By extending the DbContext base class, our MusicStoreEntities class is able to handle our database operations for us.</span></span> <span data-ttu-id="16ed5-141">すでに持っているフック、これで、いくつかのプロパティを利用するために必要な追加情報の一部のデータベースにモデル クラスに追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="16ed5-141">Now that we've got that hooked up, let's add a few more properties to our model classes to take advantage of some of the additional information in our database.</span></span>

### <a name="adding-our-store-catalog-data"></a><span data-ttu-id="16ed5-142">このストアのカタログ データを追加します。</span><span class="sxs-lookup"><span data-stu-id="16ed5-142">Adding our store catalog data</span></span>

<span data-ttu-id="16ed5-143">新しく作成されたデータベースにデータを「シード」が追加される Entity Framework の機能を活用おかかります。</span><span class="sxs-lookup"><span data-stu-id="16ed5-143">We will take advantage of a feature in Entity Framework which adds "seed" data to a newly created database.</span></span> <span data-ttu-id="16ed5-144">これが事前ジャンル、アーティスト、アルバムの一覧で、ストアのカタログに設定します。</span><span class="sxs-lookup"><span data-stu-id="16ed5-144">This will pre-populate our store catalog with a list of Genres, Artists, and Albums.</span></span> <span data-ttu-id="16ed5-145">-これは、このチュートリアルで以前に使用する、サイト設計ファイルが含まれている - MvcMusicStore Assets.zip ダウンロードには、コードをという名前のフォルダー内にあるこのシード データでクラス ファイルがあります。</span><span class="sxs-lookup"><span data-stu-id="16ed5-145">The MvcMusicStore-Assets.zip download - which included our site design files used earlier in this tutorial - has a class file with this seed data, located in a folder named Code.</span></span>

<span data-ttu-id="16ed5-146">コード内で/[モデル] フォルダー、SampleData.cs ファイルを見つけて次のように、プロジェクトで、Models フォルダーにドロップします。</span><span class="sxs-lookup"><span data-stu-id="16ed5-146">Within the Code / Models folder, locate the SampleData.cs file and drop it into the Models folder in our project, as shown below.</span></span>

![](mvc-music-store-part-4/_static/image4.png)

<span data-ttu-id="16ed5-147">Entity Framework をその SampleData クラスに紹介するコードの 1 つの行を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="16ed5-147">Now we need to add one line of code to tell Entity Framework about that SampleData class.</span></span> <span data-ttu-id="16ed5-148">開き、アプリケーションを先頭に次の行を追加するプロジェクトのルートで Global.asax ファイルをダブルクリックして\_メソッドを開始します。</span><span class="sxs-lookup"><span data-stu-id="16ed5-148">Double-click on the Global.asax file in the root of the project to open it and add the following line to the top the Application\_Start method.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

<span data-ttu-id="16ed5-149">この時点では、Entity Framework プロジェクトを構成するのに必要な作業が終了しました。</span><span class="sxs-lookup"><span data-stu-id="16ed5-149">At this point, we've completed the work necessary to configure Entity Framework for our project.</span></span>

## <a name="querying-the-database"></a><span data-ttu-id="16ed5-150">データベースに対するクエリの実行</span><span class="sxs-lookup"><span data-stu-id="16ed5-150">Querying the Database</span></span>

<span data-ttu-id="16ed5-151">今すぐみましょうではなく「ダミー データ」を使用して、代わりに呼び出してすべての情報を照会するデータベース内にするように、StoreController を更新します。</span><span class="sxs-lookup"><span data-stu-id="16ed5-151">Now let's update our StoreController so that instead of using "dummy data" it instead calls into our database to query all of its information.</span></span> <span data-ttu-id="16ed5-152">まず始めにフィールドを宣言することによって、 **StoreController** storeDB をという名前の MusicStoreEntities クラスのインスタンスを保持します。</span><span class="sxs-lookup"><span data-stu-id="16ed5-152">We'll start by declaring a field on the **StoreController** to hold an instance of the MusicStoreEntities class, named storeDB:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a><span data-ttu-id="16ed5-153">データベースを照会するストア インデックスを更新します。</span><span class="sxs-lookup"><span data-stu-id="16ed5-153">Updating the Store Index to query the database</span></span>

<span data-ttu-id="16ed5-154">MusicStoreEntities クラスは、Entity Framework で保持され、データベース内の各テーブルのコレクション プロパティを公開します。</span><span class="sxs-lookup"><span data-stu-id="16ed5-154">The MusicStoreEntities class is maintained by the Entity Framework and exposes a collection property for each table in our database.</span></span> <span data-ttu-id="16ed5-155">弊社のデータベース内のすべてのジャンルを取得する、StoreController のインデックスの操作を更新してみましょう。</span><span class="sxs-lookup"><span data-stu-id="16ed5-155">Let's update our StoreController's Index action to retrieve all Genres in our database.</span></span> <span data-ttu-id="16ed5-156">以前この文字列のデータをハード コーディングしていました。</span><span class="sxs-lookup"><span data-stu-id="16ed5-156">Previously we did this by hard-coding string data.</span></span> <span data-ttu-id="16ed5-157">今すぐお代わりにだけ使用できます、Entity Framework コンテキスト Generes コレクション。</span><span class="sxs-lookup"><span data-stu-id="16ed5-157">Now we can instead just use the Entity Framework context Generes collection:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

<span data-ttu-id="16ed5-158">変更をまだだけを返しているライブ データ データベースから今すぐ前に、返されるお同じ StoreIndexViewModel を返しているので、ビュー テンプレートに発生する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="16ed5-158">No changes need to happen to our View template since we're still returning the same StoreIndexViewModel we returned before - we're just returning live data from our database now.</span></span>

<span data-ttu-id="16ed5-159">プロジェクトをもう一度実行して、「/格納」url、データベースにすべてジャンルの一覧が表示おされますようになりました。</span><span class="sxs-lookup"><span data-stu-id="16ed5-159">When we run the project again and visit the "/Store" URL, we'll now see a list of all Genres in our database:</span></span>

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a><span data-ttu-id="16ed5-160">ストアを参照および更新の詳細をライブ データを使用するには</span><span class="sxs-lookup"><span data-stu-id="16ed5-160">Updating Store Browse and Details to use live data</span></span>

<span data-ttu-id="16ed5-161">ストア/参照しますか? ジャンル =*[一部ジャンル]* 、アクション メソッドを検索するジャンル名でします。</span><span class="sxs-lookup"><span data-stu-id="16ed5-161">With the /Store/Browse?genre=*[some-genre]* action method, we're searching for a Genre by name.</span></span> <span data-ttu-id="16ed5-162">のみが予定 1 つの結果、同じジャンル名に対して 2 つのエントリはならないことがあるため、および使用できるようにします。次のように適切なジャンル オブジェクトに対するクエリを LINQ で Single() 拡張機能 (入力せずにこのまだ)。</span><span class="sxs-lookup"><span data-stu-id="16ed5-162">We only expect one result, since we shouldn't ever have two entries for the same Genre name, and so we can use the .Single() extension in LINQ to query for the appropriate Genre object like this (don't type this yet):</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

<span data-ttu-id="16ed5-163">1 つのメソッドは、その名前が定義した値と一致するよう、単一のジャンル オブジェクトすることを指定するパラメーターとしてラムダ式を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="16ed5-163">The Single method takes a Lambda expression as a parameter, which specifies that we want a single Genre object such that its name matches the value we've defined.</span></span> <span data-ttu-id="16ed5-164">上記の例では、Disco と一致する名前値の 1 つのジャンル オブジェクトを読み込んでおいます。</span><span class="sxs-lookup"><span data-stu-id="16ed5-164">In the case above, we are loading a single Genre object with a Name value matching Disco.</span></span>

<span data-ttu-id="16ed5-165">使用すると、ジャンル オブジェクトを取得するときにも読み込まれたたい他の関連エンティティを示すために Entity Framework 機能を移動します。</span><span class="sxs-lookup"><span data-stu-id="16ed5-165">We'll take advantage of an Entity Framework feature that allows us to indicate other related entities we want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="16ed5-166">この機能は、クエリ結果の整形が呼び出され、すべての必要な情報を取得するデータベースにアクセスする必要があります回数を削減することにより、します。</span><span class="sxs-lookup"><span data-stu-id="16ed5-166">This feature is called Query Result Shaping, and enables us to reduce the number of times we need to access the database to retrieve all of the information we need.</span></span> <span data-ttu-id="16ed5-167">更新、クエリに関連するアルバムもすることを示すために Genres.Include("Albums") から含まれるように取得、ジャンルのアルバムをプリフェッチすることができます。</span><span class="sxs-lookup"><span data-stu-id="16ed5-167">We want to pre-fetch the Albums for Genre we retrieve, so we'll update our query to include from Genres.Include("Albums") to indicate that we want related albums as well.</span></span> <span data-ttu-id="16ed5-168">これは、1 つのデータベースの要求で、ジャンル、およびアルバムの両方のデータが取得されますのでより効率的です。</span><span class="sxs-lookup"><span data-stu-id="16ed5-168">This is more efficient, since it will retrieve both our Genre and Album data in a single database request.</span></span>

<span data-ttu-id="16ed5-169">場所には、説明を次に、更新された参照コント ローラー アクションの外観を示します。</span><span class="sxs-lookup"><span data-stu-id="16ed5-169">With the explanations out of the way, here's how our updated Browse controller action looks:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

<span data-ttu-id="16ed5-170">各ジャンルの使用可能なアルバムを表示するストア参照ビューを更新できます。</span><span class="sxs-lookup"><span data-stu-id="16ed5-170">We can now update the Store Browse View to display the albums which are available in each Genre.</span></span> <span data-ttu-id="16ed5-171">ビューのテンプレート (内で見つかった/Views/Store/Browse.cshtml) を開き、次に示すように、アルバムの箇条書きリストを追加します。</span><span class="sxs-lookup"><span data-stu-id="16ed5-171">Open the view template (found in /Views/Store/Browse.cshtml) and add a bulleted list of Albums as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

<span data-ttu-id="16ed5-172">このアプリケーションを実行し、ストア/[参照] を参照しますか? ジャンル = 結果が、選択したジャンルのすべてのアルバムを表示する、データベースからプルするようになりましたことジャズを示しています。</span><span class="sxs-lookup"><span data-stu-id="16ed5-172">Running our application and browsing to /Store/Browse?genre=Jazz shows that our results are now being pulled from the database, displaying all albums in our selected Genre.</span></span>

![](mvc-music-store-part-4/_static/image2.jpg)

<span data-ttu-id="16ed5-173">しましょう同じ、/Store 詳細/[id] URL に変更し、ID を持つパラメーター値に一致するアルバムに読み込み、データベース クエリで、ダミーのデータを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="16ed5-173">We'll make the same change to our /Store/Details/[id] URL, and replace our dummy data with a database query which loads an Album whose ID matches the parameter value.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

<span data-ttu-id="16ed5-174">アプリケーションを実行して、/Store/Details/1 を参照して、結果がデータベースからプルするようになりましたことを示します。</span><span class="sxs-lookup"><span data-stu-id="16ed5-174">Running our application and browsing to /Store/Details/1 shows that our results are now being pulled from the database.</span></span>

![](mvc-music-store-part-4/_static/image5.png)

<span data-ttu-id="16ed5-175">これで、ストアの詳細ページがセットアップでアルバム ID で、アルバムを表示するを更新してみましょう、**参照**詳細ビューへのリンクを表示します。</span><span class="sxs-lookup"><span data-stu-id="16ed5-175">Now that our Store Details page is set up to display an album by the Album ID, let's update the **Browse** view to link to the Details view.</span></span> <span data-ttu-id="16ed5-176">ストアのインデックスからリンク前のセクションの最後にストアを参照することとまったく同じ状態 Html.ActionLink を使用します。</span><span class="sxs-lookup"><span data-stu-id="16ed5-176">We will use Html.ActionLink, exactly as we did to link from Store Index to Store Browse at the end of the previous section.</span></span> <span data-ttu-id="16ed5-177">[参照] ビューの完全なソースは、以下が表示されます。</span><span class="sxs-lookup"><span data-stu-id="16ed5-177">The complete source for the Browse view appears below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

<span data-ttu-id="16ed5-178">ストア ページで、使用可能なアルバムを表示するには、ジャンルのページを参照できないできましたおよびアルバムをクリックするとは、そのアルバムの詳細を表示できます。</span><span class="sxs-lookup"><span data-stu-id="16ed5-178">We're now able to browse from our Store page to a Genre page, which lists the available albums, and by clicking on an album we can view details for that album.</span></span>

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> <span data-ttu-id="16ed5-179">[前へ](mvc-music-store-part-3.md)
> [次へ](mvc-music-store-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="16ed5-179">[Previous](mvc-music-store-part-3.md)
[Next](mvc-music-store-part-5.md)</span></span>
