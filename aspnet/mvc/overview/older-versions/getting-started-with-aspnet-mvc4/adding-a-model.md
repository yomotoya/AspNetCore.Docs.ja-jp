---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: モデルを追加する |Microsoft ドキュメント
author: Rick-Anderson
description: '注: このチュートリアルの最新バージョンはここで ASP.NET MVC 5 と Visual Studio 2013 を使用します。 安全な非常に簡単に従い、デモをお勧めしています.'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 562a06e22aad62b6982aca3316a2dfe18a6eba2e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-model"></a><span data-ttu-id="8ce80-104">モデルを追加します。</span><span class="sxs-lookup"><span data-stu-id="8ce80-104">Adding a Model</span></span>
====================
<span data-ttu-id="8ce80-105">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="8ce80-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="8ce80-106">このチュートリアルの更新バージョンが利用可能な[ここ](../../getting-started/introduction/getting-started.md)ASP.NET MVC 5 と Visual Studio 2013 を使用します。</span><span class="sxs-lookup"><span data-stu-id="8ce80-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="8ce80-107">より安全な非常に簡単に従うしより多くの機能を示します。</span><span class="sxs-lookup"><span data-stu-id="8ce80-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="8ce80-108">このセクションでは、データベース内のムービーを管理するためのいくつかのクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="8ce80-108">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="8ce80-109">これらのクラスになります、&quot;モデル&quot;ASP.NET MVC アプリケーションの一部です。</span><span class="sxs-lookup"><span data-stu-id="8ce80-109">These classes will be the &quot;model&quot; part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="8ce80-110">呼ばれる .NET Framework データ アクセス テクノロジを使用、 [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx)を定義し、これらのモデル クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="8ce80-110">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) to define and work with these model classes.</span></span> <span data-ttu-id="8ce80-111">開発パラダイムと呼ばれる、Entity Framework (EF とも呼ばれます) によってサポート*Code First*です。</span><span class="sxs-lookup"><span data-stu-id="8ce80-111">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="8ce80-112">最初のコードでは単純なクラスを作成してモデル オブジェクトを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="8ce80-112">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="8ce80-113">(これらとも呼ばれる POCO クラスから&quot;従来の CLR オブジェクト&quot;)。これにより、非常にクリーン、迅速な開発ワークフローのクラスから実行時に作成されたデータベースを持つことができます。</span><span class="sxs-lookup"><span data-stu-id="8ce80-113">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="8ce80-114">モデル クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="8ce80-114">Adding Model Classes</span></span>

<span data-ttu-id="8ce80-115">**ソリューション エクスプ ローラー**を右クリックして、*モデル*フォルダーを選択**追加**、し、**クラス**です。</span><span class="sxs-lookup"><span data-stu-id="8ce80-115">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="8ce80-116">入力、*クラス*名前&quot;ムービー&quot;です。</span><span class="sxs-lookup"><span data-stu-id="8ce80-116">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="8ce80-117">次の 5 つのプロパティを追加、`Movie`クラス。</span><span class="sxs-lookup"><span data-stu-id="8ce80-117">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="8ce80-118">使用して、`Movie`データベースでムービーを表すクラス。</span><span class="sxs-lookup"><span data-stu-id="8ce80-118">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="8ce80-119">各インスタンス、`Movie`オブジェクトは、データベース テーブルとの各プロパティ内の行に対応している、`Movie`クラスは、テーブル内の列にマップされます。</span><span class="sxs-lookup"><span data-stu-id="8ce80-119">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="8ce80-120">同じファイルで次のコードを追加`MovieDBContext`クラス。</span><span class="sxs-lookup"><span data-stu-id="8ce80-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="8ce80-121">`MovieDBContext`クラスを表します。 Entity Framework ムービーのデータベース コンテキストのフェッチ、保存、および更新を処理する`Movie`クラスのインスタンス、データベースにします。</span><span class="sxs-lookup"><span data-stu-id="8ce80-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="8ce80-122">`MovieDBContext`から派生した、`DbContext`基本 Entity Framework によって提供されるクラスです。</span><span class="sxs-lookup"><span data-stu-id="8ce80-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="8ce80-123">参照できるように`DbContext`と`DbSet`、以下を追加する必要があります`using`ファイルの上部にあるステートメント。</span><span class="sxs-lookup"><span data-stu-id="8ce80-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="8ce80-124">完全な*Movie.cs*ファイルを次に示します。</span><span class="sxs-lookup"><span data-stu-id="8ce80-124">The complete *Movie.cs* file is shown below.</span></span> <span data-ttu-id="8ce80-125">(いくつかはステートメントを使用する必要がなくなりました)。</span><span class="sxs-lookup"><span data-stu-id="8ce80-125">(Several using statements that are not needed have been removed.)</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="8ce80-126">接続文字列を作成し、SQL Server LocalDB 協力</span><span class="sxs-lookup"><span data-stu-id="8ce80-126">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="8ce80-127">`MovieDBContext`作成したクラスは、データベースに接続し、タスクのマッピングを処理`Movie`データベース レコードへのオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="8ce80-127">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="8ce80-128">1 つに質問する可能性がありますには、接続先データベースを指定する方法です。</span><span class="sxs-lookup"><span data-stu-id="8ce80-128">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="8ce80-129">内の接続情報を追加することによって行うこと、 *Web.config*アプリケーションのファイルです。</span><span class="sxs-lookup"><span data-stu-id="8ce80-129">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="8ce80-130">アプリケーションのルートを開く*Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="8ce80-130">Open the application root *Web.config* file.</span></span> <span data-ttu-id="8ce80-131">(されません、 *Web.config*ファイルで、*ビュー*フォルダーです)。開く、 *Web.config*赤に記載されているファイル。</span><span class="sxs-lookup"><span data-stu-id="8ce80-131">(Not the *Web.config* file in the *Views* folder.) Open the *Web.config* file outlined in red.</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="8ce80-132">次の接続文字列を追加、`<connectionStrings>`内の要素、 *Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="8ce80-132">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="8ce80-133">次の例の一部を示しています、 *Web.config*新しい接続文字列を追加してファイル。</span><span class="sxs-lookup"><span data-stu-id="8ce80-133">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

<span data-ttu-id="8ce80-134">このような少量のコードおよび XML を表し、ムービー データをデータベースに格納するために記述する必要がありますすべてがあります。</span><span class="sxs-lookup"><span data-stu-id="8ce80-134">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="8ce80-135">新しいビルド次に、`MoviesController`ムービー データを表示し、新しいムービーの一覧を作成できるように使用できるクラスです。</span><span class="sxs-lookup"><span data-stu-id="8ce80-135">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8ce80-136">[前へ](adding-a-view.md)
> [次へ](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="8ce80-136">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
