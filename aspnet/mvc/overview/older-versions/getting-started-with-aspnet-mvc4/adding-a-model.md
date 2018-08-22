---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: モデルの追加 |Microsoft Docs
author: Rick-Anderson
description: '注: このチュートリアルの最新バージョンは ASP.NET MVC 5 と Visual Studio 2013 を使用します。 安全なはるかに簡単に従い、デモをお勧めしています.'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: a7e732da3b056980c87660f6b80366438b5823c1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823884"
---
<a name="adding-a-model"></a><span data-ttu-id="d0f42-104">モデルの追加</span><span class="sxs-lookup"><span data-stu-id="d0f42-104">Adding a Model</span></span>
====================
<span data-ttu-id="d0f42-105">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="d0f42-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="d0f42-106">このチュートリアルの更新バージョンが利用可能な[ここ](../../getting-started/introduction/getting-started.md)ASP.NET MVC 5 と Visual Studio 2013 を使用します。</span><span class="sxs-lookup"><span data-stu-id="d0f42-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="d0f42-107">より安全ではるかに簡単に従うしより多くの機能を示します。</span><span class="sxs-lookup"><span data-stu-id="d0f42-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="d0f42-108">このセクションでは、データベースのムービーを管理するためのいくつかのクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="d0f42-108">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="d0f42-109">これらのクラスがありますが、&quot;モデル&quot;ASP.NET MVC アプリケーションの一部です。</span><span class="sxs-lookup"><span data-stu-id="d0f42-109">These classes will be the &quot;model&quot; part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="d0f42-110">呼ばれる .NET Framework データ アクセス テクノロジを使用します、 [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx)を定義し、これらのモデル クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="d0f42-110">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) to define and work with these model classes.</span></span> <span data-ttu-id="d0f42-111">開発パラダイムと呼ばれる、(EF とも呼ばれます)、Entity Framework がサポート*Code First*します。</span><span class="sxs-lookup"><span data-stu-id="d0f42-111">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="d0f42-112">まず、コードを使用すると、単純なクラスを記述することで、モデル オブジェクトを作成できます。</span><span class="sxs-lookup"><span data-stu-id="d0f42-112">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="d0f42-113">(これらとも呼ばれる、POCO クラスから&quot;plain-old CLR object&quot;)。データベースが非常にクリーンで迅速な開発ワークフローを有効にするクラスからその場で作成することができます。</span><span class="sxs-lookup"><span data-stu-id="d0f42-113">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="d0f42-114">モデル クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="d0f42-114">Adding Model Classes</span></span>

<span data-ttu-id="d0f42-115">**ソリューション エクスプ ローラー**を右クリックして、*モデル*フォルダーで、**追加**、し、**クラス**します。</span><span class="sxs-lookup"><span data-stu-id="d0f42-115">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="d0f42-116">入力、*クラス*名前&quot;ムービー&quot;します。</span><span class="sxs-lookup"><span data-stu-id="d0f42-116">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="d0f42-117">次の 5 つのプロパティを追加、`Movie`クラス。</span><span class="sxs-lookup"><span data-stu-id="d0f42-117">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="d0f42-118">使用して、`Movie`をデータベースのムービーを表すクラス。</span><span class="sxs-lookup"><span data-stu-id="d0f42-118">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="d0f42-119">各インスタンスを`Movie`オブジェクトは、データベース テーブルとの各プロパティ内の行に対応している、`Movie`クラスは、テーブル内の列にマップされます。</span><span class="sxs-lookup"><span data-stu-id="d0f42-119">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="d0f42-120">同じファイルで次の追加`MovieDBContext`クラス。</span><span class="sxs-lookup"><span data-stu-id="d0f42-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="d0f42-121">`MovieDBContext`クラスは、フェッチ、格納、および更新を処理する Entity Framework ムービー データベース コンテキストを表します。`Movie`クラス、データベース内のインスタンス。</span><span class="sxs-lookup"><span data-stu-id="d0f42-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="d0f42-122">`MovieDBContext`から派生した、`DbContext`基本クラスを Entity Framework によって提供されます。</span><span class="sxs-lookup"><span data-stu-id="d0f42-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="d0f42-123">参照できるようにするためには`DbContext`と`DbSet`、以下を追加する必要がある`using`ファイルの上部にあるステートメント。</span><span class="sxs-lookup"><span data-stu-id="d0f42-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="d0f42-124">完全な*Movie.cs*ファイルを次に示します。</span><span class="sxs-lookup"><span data-stu-id="d0f42-124">The complete *Movie.cs* file is shown below.</span></span> <span data-ttu-id="d0f42-125">(いくつかはステートメントを使用する必要がなくなりました)。</span><span class="sxs-lookup"><span data-stu-id="d0f42-125">(Several using statements that are not needed have been removed.)</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="d0f42-126">接続文字列を作成し、SQL Server LocalDB の使用</span><span class="sxs-lookup"><span data-stu-id="d0f42-126">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="d0f42-127">`MovieDBContext`クラスを作成したデータベースへの接続とマッピングのタスクを処理する`Movie`データベース レコードへのオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="d0f42-127">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="d0f42-128">疑問に思うかもしれません質問の 1 つは、接続先データベースを指定する方法です。</span><span class="sxs-lookup"><span data-stu-id="d0f42-128">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="d0f42-129">内の接続情報を追加することによって行います、 *Web.config*アプリケーションのファイル。</span><span class="sxs-lookup"><span data-stu-id="d0f42-129">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="d0f42-130">アプリケーションのルートを開く*Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="d0f42-130">Open the application root *Web.config* file.</span></span> <span data-ttu-id="d0f42-131">(いない、 *Web.config*ファイル、*ビュー*フォルダー)。開く、 *Web.config* red に記載されているファイル。</span><span class="sxs-lookup"><span data-stu-id="d0f42-131">(Not the *Web.config* file in the *Views* folder.) Open the *Web.config* file outlined in red.</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="d0f42-132">次の接続文字列を追加、`<connectionStrings>`内の要素、 *Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="d0f42-132">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="d0f42-133">次の例の一部を示しています、 *Web.config*ファイルが追加された新しい接続文字列に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="d0f42-133">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

<span data-ttu-id="d0f42-134">この少量のコードと XML は、すべてを表し、映画のデータをデータベースに格納するために記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d0f42-134">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="d0f42-135">新しいを作成します次に、`MoviesController`ムービー データを表示し、新しいムービーの一覧の作成を許可するのに使用できるクラスです。</span><span class="sxs-lookup"><span data-stu-id="d0f42-135">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d0f42-136">[前へ](adding-a-view.md)
> [次へ](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="d0f42-136">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
