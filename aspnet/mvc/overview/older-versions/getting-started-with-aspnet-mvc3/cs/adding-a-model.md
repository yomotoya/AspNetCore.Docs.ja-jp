---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
title: モデルを追加する (c#) |Microsoft Docs
author: Rick-Anderson
description: '注: このチュートリアルの最新バージョンは ASP.NET MVC 5 と Visual Studio 2013 を使用します。 安全なはるかに簡単に従い、デモをお勧めしています.'
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 42355b95-5f1f-413e-8d16-14cdfaaefcd8
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: b68f857777b1b69ca401c29261211f40df253e7a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835646"
---
<a name="adding-a-model-c"></a><span data-ttu-id="58f10-104">モデル (c#) を追加します。</span><span class="sxs-lookup"><span data-stu-id="58f10-104">Adding a Model (C#)</span></span>
====================
<span data-ttu-id="58f10-105">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="58f10-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="58f10-106">このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、Microsoft Visual Studio の無料版であるを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。</span><span class="sxs-lookup"><span data-stu-id="58f10-106">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="58f10-107">始める前に、以下の前提条件がインストールされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="58f10-107">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="58f10-108">次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)します。</span><span class="sxs-lookup"><span data-stu-id="58f10-108">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="58f10-109">または、次のリンクを使用して、前提条件を個別にインストールできます。</span><span class="sxs-lookup"><span data-stu-id="58f10-109">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="58f10-110">Visual Studio Web Developer Express SP1 の前提条件</span><span class="sxs-lookup"><span data-stu-id="58f10-110">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="58f10-111">ASP.NET MVC 3 Tools Update します。</span><span class="sxs-lookup"><span data-stu-id="58f10-111">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="58f10-112">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)</span><span class="sxs-lookup"><span data-stu-id="58f10-112">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="58f10-113">Visual Web Developer 2010 ではなく Visual Studio 2010 を使用する場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)します。</span><span class="sxs-lookup"><span data-stu-id="58f10-113">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="58f10-114">C# ソース コードでの Visual Web Developer プロジェクトは、このトピックと共に使用できます。</span><span class="sxs-lookup"><span data-stu-id="58f10-114">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="58f10-115">[C# バージョンをダウンロード](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)します。</span><span class="sxs-lookup"><span data-stu-id="58f10-115">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="58f10-116">Visual Basic を使用する場合に切り替えて、 [Visual Basic バージョン](../vb/adding-a-model.md)このチュートリアルの。</span><span class="sxs-lookup"><span data-stu-id="58f10-116">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/adding-a-model.md) of this tutorial.</span></span>


## <a name="adding-a-model"></a><span data-ttu-id="58f10-117">モデルの追加</span><span class="sxs-lookup"><span data-stu-id="58f10-117">Adding a Model</span></span>

<span data-ttu-id="58f10-118">このセクションでは、データベースのムービーを管理するためのいくつかのクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="58f10-118">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="58f10-119">これらのクラスには、ASP.NET MVC アプリケーションの「モデル」の部分があります。</span><span class="sxs-lookup"><span data-stu-id="58f10-119">These classes will be the "model" part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="58f10-120">Entity Framework と呼ばれる .NET Framework データ アクセス テクノロジは、これらのモデル クラスの定義および操作を使用します。</span><span class="sxs-lookup"><span data-stu-id="58f10-120">You'll use a .NET Framework data-access technology known as the Entity Framework to define and work with these model classes.</span></span> <span data-ttu-id="58f10-121">開発パラダイムと呼ばれる、(EF とも呼ばれます)、Entity Framework がサポート*Code First*します。</span><span class="sxs-lookup"><span data-stu-id="58f10-121">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="58f10-122">まず、コードを使用すると、単純なクラスを記述することで、モデル オブジェクトを作成できます。</span><span class="sxs-lookup"><span data-stu-id="58f10-122">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="58f10-123">(これらとも呼ばれます"plain-old CLR object"からの POCO クラス)データベースが非常にクリーンで迅速な開発ワークフローを有効にするクラスからその場で作成することができます。</span><span class="sxs-lookup"><span data-stu-id="58f10-123">(These are also known as POCO classes, from "plain-old CLR objects.") You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="58f10-124">モデル クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="58f10-124">Adding Model Classes</span></span>

<span data-ttu-id="58f10-125">**ソリューション エクスプ ローラー**を右クリックして、*モデル*フォルダーで、**追加**、し、**クラス**します。</span><span class="sxs-lookup"><span data-stu-id="58f10-125">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="58f10-126">名前、*クラス*「ムービー」。</span><span class="sxs-lookup"><span data-stu-id="58f10-126">Name the *class* "Movie".</span></span>

<span data-ttu-id="58f10-127">[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="58f10-127">[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)</span></span>

<span data-ttu-id="58f10-128">次の 5 つのプロパティを追加、`Movie`クラス。</span><span class="sxs-lookup"><span data-stu-id="58f10-128">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="58f10-129">使用して、`Movie`をデータベースのムービーを表すクラス。</span><span class="sxs-lookup"><span data-stu-id="58f10-129">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="58f10-130">各インスタンスを`Movie`オブジェクトは、データベース テーブルとの各プロパティ内の行に対応している、`Movie`クラスは、テーブル内の列にマップされます。</span><span class="sxs-lookup"><span data-stu-id="58f10-130">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="58f10-131">同じファイルで次の追加`MovieDBContext`クラス。</span><span class="sxs-lookup"><span data-stu-id="58f10-131">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="58f10-132">`MovieDBContext`クラスは、フェッチ、格納、および更新を処理する Entity Framework ムービー データベース コンテキストを表します。`Movie`クラス、データベース内のインスタンス。</span><span class="sxs-lookup"><span data-stu-id="58f10-132">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="58f10-133">`MovieDBContext`から派生した、`DbContext`基本クラスを Entity Framework によって提供されます。</span><span class="sxs-lookup"><span data-stu-id="58f10-133">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span> <span data-ttu-id="58f10-134">詳細については`DbContext`と`DbSet`を参照してください[Entity Framework の生産性の向上](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0)します。</span><span class="sxs-lookup"><span data-stu-id="58f10-134">For more information about `DbContext` and `DbSet`, see [Productivity Improvements for the Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span></span>

<span data-ttu-id="58f10-135">参照できるようにするためには`DbContext`と`DbSet`、以下を追加する必要がある`using`ファイルの上部にあるステートメント。</span><span class="sxs-lookup"><span data-stu-id="58f10-135">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="58f10-136">完全な*Movie.cs*ファイルを次に示します。</span><span class="sxs-lookup"><span data-stu-id="58f10-136">The complete *Movie.cs* file is shown below.</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a><span data-ttu-id="58f10-137">接続文字列を作成し、SQL Server Compact の使用</span><span class="sxs-lookup"><span data-stu-id="58f10-137">Creating a Connection String and Working with SQL Server Compact</span></span>

<span data-ttu-id="58f10-138">`MovieDBContext`クラスを作成したデータベースへの接続とマッピングのタスクを処理する`Movie`データベース レコードへのオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="58f10-138">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="58f10-139">疑問に思うかもしれません質問の 1 つは、接続先データベースを指定する方法です。</span><span class="sxs-lookup"><span data-stu-id="58f10-139">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="58f10-140">内の接続情報を追加することによって行います、 *Web.config*アプリケーションのファイル。</span><span class="sxs-lookup"><span data-stu-id="58f10-140">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="58f10-141">アプリケーションのルートを開く*Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="58f10-141">Open the application root *Web.config* file.</span></span> <span data-ttu-id="58f10-142">(いない、 *Web.config*ファイル、*ビュー*フォルダー)。次の図は、両方を表示*Web.config*ファイル; 開く、 *Web.config*ファイル赤い丸でします。</span><span class="sxs-lookup"><span data-stu-id="58f10-142">(Not the *Web.config* file in the *Views* folder.) The image below show both *Web.config* files; open the *Web.config* file circled in red.</span></span>

![](adding-a-model/_static/image4.png)

### 

<span data-ttu-id="58f10-143">次の接続文字列を追加、`<connectionStrings>`内の要素、 *Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="58f10-143">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="58f10-144">次の例の一部を示しています、 *Web.config*ファイルが追加された新しい接続文字列に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="58f10-144">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

<span data-ttu-id="58f10-145">この少量のコードと XML は、すべてを表し、映画のデータをデータベースに格納するために記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="58f10-145">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="58f10-146">新しいを作成します次に、`MoviesController`ムービー データを表示し、新しいムービーの一覧の作成を許可するのに使用できるクラスです。</span><span class="sxs-lookup"><span data-stu-id="58f10-146">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="58f10-147">[前へ](adding-a-view.md)
> [次へ](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="58f10-147">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
