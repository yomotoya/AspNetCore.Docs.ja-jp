---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
title: "モデル (c#) を追加する |Microsoft ドキュメント"
author: Rick-Anderson
description: "注: このチュートリアルの最新バージョンはここで ASP.NET MVC 5 と Visual Studio 2013 を使用します。 安全な非常に簡単に従い、デモをお勧めしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 42355b95-5f1f-413e-8d16-14cdfaaefcd8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 32f2fcdae9d92b84dcd9be8746e416cdf9a14c48
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-model-c"></a><span data-ttu-id="d2a81-104">モデル (c#) を追加します。</span><span class="sxs-lookup"><span data-stu-id="d2a81-104">Adding a Model (C#)</span></span>
====================
<span data-ttu-id="d2a81-105">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="d2a81-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="d2a81-106">このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これは、無料版の Microsoft Visual Studio を使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。</span><span class="sxs-lookup"><span data-stu-id="d2a81-106">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="d2a81-107">開始する前に、以下に示す前提条件がインストールされていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="d2a81-107">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="d2a81-108">次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)です。</span><span class="sxs-lookup"><span data-stu-id="d2a81-108">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="d2a81-109">また、次のリンクを使用して、前提条件を個別にインストールできます。</span><span class="sxs-lookup"><span data-stu-id="d2a81-109">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="d2a81-110">Visual Studio Web Developer Express SP1 の前提条件</span><span class="sxs-lookup"><span data-stu-id="d2a81-110">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="d2a81-111">ASP.NET MVC 3 Tools Update します。</span><span class="sxs-lookup"><span data-stu-id="d2a81-111">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="d2a81-112">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)</span><span class="sxs-lookup"><span data-stu-id="d2a81-112">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="d2a81-113">Visual Web Developer 2010 ではなく Visual Studio 2010 を使用している場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)です。</span><span class="sxs-lookup"><span data-stu-id="d2a81-113">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="d2a81-114">C# ソース コードの Visual Web Developer プロジェクトは、このトピックを使用できます。</span><span class="sxs-lookup"><span data-stu-id="d2a81-114">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="d2a81-115">[C# バージョンをダウンロード](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)です。</span><span class="sxs-lookup"><span data-stu-id="d2a81-115">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="d2a81-116">Visual Basic を使用する場合に切り替えて、 [Visual Basic バージョン](../vb/adding-a-model.md)このチュートリアルのです。</span><span class="sxs-lookup"><span data-stu-id="d2a81-116">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/adding-a-model.md) of this tutorial.</span></span>


## <a name="adding-a-model"></a><span data-ttu-id="d2a81-117">モデルを追加します。</span><span class="sxs-lookup"><span data-stu-id="d2a81-117">Adding a Model</span></span>

<span data-ttu-id="d2a81-118">このセクションでは、データベース内のムービーを管理するためのいくつかのクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="d2a81-118">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="d2a81-119">これらのクラスは、ASP.NET MVC アプリケーションの「モデル」部分になります。</span><span class="sxs-lookup"><span data-stu-id="d2a81-119">These classes will be the "model" part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="d2a81-120">定義し、これらのモデル クラスを使用するには、Entity Framework と呼ばれる .NET Framework データ アクセス テクノロジを使用します。</span><span class="sxs-lookup"><span data-stu-id="d2a81-120">You'll use a .NET Framework data-access technology known as the Entity Framework to define and work with these model classes.</span></span> <span data-ttu-id="d2a81-121">開発パラダイムと呼ばれる、Entity Framework (EF とも呼ばれます) によってサポート*Code First*です。</span><span class="sxs-lookup"><span data-stu-id="d2a81-121">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="d2a81-122">最初のコードでは単純なクラスを作成してモデル オブジェクトを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="d2a81-122">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="d2a81-123">(これらとも呼ばれる従来の CLR オブジェクト"の"から、POCO クラス)これにより、非常にクリーン、迅速な開発ワークフローのクラスから実行時に作成されたデータベースを持つことができます。</span><span class="sxs-lookup"><span data-stu-id="d2a81-123">(These are also known as POCO classes, from "plain-old CLR objects.") You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="d2a81-124">モデル クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="d2a81-124">Adding Model Classes</span></span>

<span data-ttu-id="d2a81-125">**ソリューション エクスプ ローラー**を右クリックして、*モデル*フォルダーを選択**追加**、し、**クラス**です。</span><span class="sxs-lookup"><span data-stu-id="d2a81-125">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="d2a81-126">名前、*クラス*「ムービー」です。</span><span class="sxs-lookup"><span data-stu-id="d2a81-126">Name the *class* "Movie".</span></span>

<span data-ttu-id="d2a81-127">[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="d2a81-127">[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)</span></span>

<span data-ttu-id="d2a81-128">次の 5 つのプロパティを追加、`Movie`クラス。</span><span class="sxs-lookup"><span data-stu-id="d2a81-128">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="d2a81-129">使用して、`Movie`データベースでムービーを表すクラス。</span><span class="sxs-lookup"><span data-stu-id="d2a81-129">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="d2a81-130">各インスタンス、`Movie`オブジェクトは、データベース テーブルとの各プロパティ内の行に対応している、`Movie`クラスは、テーブル内の列にマップされます。</span><span class="sxs-lookup"><span data-stu-id="d2a81-130">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="d2a81-131">同じファイルで次のコードを追加`MovieDBContext`クラス。</span><span class="sxs-lookup"><span data-stu-id="d2a81-131">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="d2a81-132">`MovieDBContext`クラスを表します。 Entity Framework ムービーのデータベース コンテキストのフェッチ、保存、および更新を処理する`Movie`クラスのインスタンス、データベースにします。</span><span class="sxs-lookup"><span data-stu-id="d2a81-132">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="d2a81-133">`MovieDBContext`から派生した、`DbContext`基本 Entity Framework によって提供されるクラスです。</span><span class="sxs-lookup"><span data-stu-id="d2a81-133">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span> <span data-ttu-id="d2a81-134">詳細については`DbContext`と`DbSet`を参照してください[Entity Framework 用の生産性向上](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0)です。</span><span class="sxs-lookup"><span data-stu-id="d2a81-134">For more information about `DbContext` and `DbSet`, see [Productivity Improvements for the Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span></span>

<span data-ttu-id="d2a81-135">参照できるように`DbContext`と`DbSet`、以下を追加する必要があります`using`ファイルの上部にあるステートメント。</span><span class="sxs-lookup"><span data-stu-id="d2a81-135">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="d2a81-136">完全な*Movie.cs*ファイルを次に示します。</span><span class="sxs-lookup"><span data-stu-id="d2a81-136">The complete *Movie.cs* file is shown below.</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a><span data-ttu-id="d2a81-137">接続文字列を作成し、SQL Server Compact 協力</span><span class="sxs-lookup"><span data-stu-id="d2a81-137">Creating a Connection String and Working with SQL Server Compact</span></span>

<span data-ttu-id="d2a81-138">`MovieDBContext`作成したクラスは、データベースに接続し、タスクのマッピングを処理`Movie`データベース レコードへのオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="d2a81-138">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="d2a81-139">1 つに質問する可能性がありますには、接続先データベースを指定する方法です。</span><span class="sxs-lookup"><span data-stu-id="d2a81-139">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="d2a81-140">内の接続情報を追加することによって行うこと、 *Web.config*アプリケーションのファイルです。</span><span class="sxs-lookup"><span data-stu-id="d2a81-140">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="d2a81-141">アプリケーションのルートを開く*Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="d2a81-141">Open the application root *Web.config* file.</span></span> <span data-ttu-id="d2a81-142">(されません、 *Web.config*ファイルで、*ビュー*フォルダーです)。次の図を 両方表示*Web.config* ; 開いているファイル、 *Web.config*赤い円で囲まれたファイルです。</span><span class="sxs-lookup"><span data-stu-id="d2a81-142">(Not the *Web.config* file in the *Views* folder.) The image below show both *Web.config* files; open the *Web.config* file circled in red.</span></span>

![](adding-a-model/_static/image4.png)

### 

<span data-ttu-id="d2a81-143">次の接続文字列を追加、`<connectionStrings>`内の要素、 *Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="d2a81-143">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="d2a81-144">次の例の一部を示しています、 *Web.config*新しい接続文字列を追加してファイル。</span><span class="sxs-lookup"><span data-stu-id="d2a81-144">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

<span data-ttu-id="d2a81-145">このような少量のコードおよび XML を表し、ムービー データをデータベースに格納するために記述する必要がありますすべてがあります。</span><span class="sxs-lookup"><span data-stu-id="d2a81-145">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="d2a81-146">新しいビルド次に、`MoviesController`ムービー データを表示し、新しいムービーの一覧を作成できるように使用できるクラスです。</span><span class="sxs-lookup"><span data-stu-id="d2a81-146">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="d2a81-147">[前へ](adding-a-view.md)
[次へ](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="d2a81-147">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
