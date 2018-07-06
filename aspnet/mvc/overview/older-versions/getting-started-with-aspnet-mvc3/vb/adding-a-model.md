---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
title: モデル (VB) の追加 |Microsoft Docs
author: Rick-Anderson
description: このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
ms.date: 01/12/2011
ms.assetid: b3aa7720-5c78-4ca2-baef-9a52234fb7ce
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 138d21d5e33384fbc0dd99c33a9e43a95976ed06
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827807"
---
<a name="adding-a-model-vb"></a><span data-ttu-id="71b48-103">モデル (VB) を追加します。</span><span class="sxs-lookup"><span data-stu-id="71b48-103">Adding a Model (VB)</span></span>
====================
<span data-ttu-id="71b48-104">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="71b48-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="71b48-105">このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、Microsoft Visual Studio の無料版であるを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。</span><span class="sxs-lookup"><span data-stu-id="71b48-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="71b48-106">始める前に、以下の前提条件がインストールされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="71b48-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="71b48-107">次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)します。</span><span class="sxs-lookup"><span data-stu-id="71b48-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="71b48-108">または、次のリンクを使用して、前提条件を個別にインストールできます。</span><span class="sxs-lookup"><span data-stu-id="71b48-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="71b48-109">Visual Studio Web Developer Express SP1 の前提条件</span><span class="sxs-lookup"><span data-stu-id="71b48-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="71b48-110">ASP.NET MVC 3 Tools Update します。</span><span class="sxs-lookup"><span data-stu-id="71b48-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="71b48-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)</span><span class="sxs-lookup"><span data-stu-id="71b48-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="71b48-112">Visual Web Developer 2010 ではなく Visual Studio 2010 を使用する場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)します。</span><span class="sxs-lookup"><span data-stu-id="71b48-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="71b48-113">VB.NET のソース コードでの Visual Web Developer プロジェクトは、このトピックと共に使用できます。</span><span class="sxs-lookup"><span data-stu-id="71b48-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="71b48-114">[VB.NET のバージョンをダウンロード](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)します。</span><span class="sxs-lookup"><span data-stu-id="71b48-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="71b48-115">C# を使用する場合に切り替えて、 [c# バージョン](../cs/adding-a-model.md)このチュートリアルの。</span><span class="sxs-lookup"><span data-stu-id="71b48-115">If you prefer C#, switch to the [C# version](../cs/adding-a-model.md) of this tutorial.</span></span>


## <a name="adding-a-model"></a><span data-ttu-id="71b48-116">モデルの追加</span><span class="sxs-lookup"><span data-stu-id="71b48-116">Adding a Model</span></span>

<span data-ttu-id="71b48-117">このセクションでは、データベースのムービーを管理するためのいくつかのクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="71b48-117">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="71b48-118">これらのクラスには、ASP.NET MVC アプリケーションの「モデル」の部分があります。</span><span class="sxs-lookup"><span data-stu-id="71b48-118">These classes will be the "model" part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="71b48-119">Entity Framework と呼ばれる .NET Framework データ アクセス テクノロジは、これらのモデル クラスの定義および操作を使用します。</span><span class="sxs-lookup"><span data-stu-id="71b48-119">You'll use a .NET Framework data-access technology known as the Entity Framework to define and work with these model classes.</span></span> <span data-ttu-id="71b48-120">開発パラダイムと呼ばれる、(EF とも呼ばれます)、Entity Framework がサポート*Code First*します。</span><span class="sxs-lookup"><span data-stu-id="71b48-120">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="71b48-121">まず、コードを使用すると、単純なクラスを記述することで、モデル オブジェクトを作成できます。</span><span class="sxs-lookup"><span data-stu-id="71b48-121">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="71b48-122">(これらとも呼ばれます"plain-old CLR object"からの POCO クラス)データベースが非常にクリーンで迅速な開発ワークフローを有効にするクラスからその場で作成することができます。</span><span class="sxs-lookup"><span data-stu-id="71b48-122">(These are also known as POCO classes, from "plain-old CLR objects.") You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="71b48-123">モデル クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="71b48-123">Adding Model Classes</span></span>

<span data-ttu-id="71b48-124">**ソリューション エクスプ ローラー**を右クリックして、*モデル*フォルダーで、**追加**、し、**クラス**します。</span><span class="sxs-lookup"><span data-stu-id="71b48-124">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="71b48-125">「ビデオ」クラスの名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="71b48-125">Name the class "Movie".</span></span>

<span data-ttu-id="71b48-126">次の 5 つのプロパティを追加、`Movie`クラス。</span><span class="sxs-lookup"><span data-stu-id="71b48-126">Add the following five properties to the `Movie` class:</span></span>

[!code-vb[Main](adding-a-model/samples/sample1.vb)]

<span data-ttu-id="71b48-127">使用して、`Movie`をデータベースのムービーを表すクラス。</span><span class="sxs-lookup"><span data-stu-id="71b48-127">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="71b48-128">各インスタンスを`Movie`オブジェクトは、データベース テーブルとの各プロパティ内の行に対応している、`Movie`クラスは、テーブル内の列にマップされます。</span><span class="sxs-lookup"><span data-stu-id="71b48-128">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="71b48-129">同じファイルで次の追加`MovieDBContext`クラス。</span><span class="sxs-lookup"><span data-stu-id="71b48-129">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-vb[Main](adding-a-model/samples/sample2.vb)]

<span data-ttu-id="71b48-130">`MovieDBContext`クラスは、フェッチ、格納、および更新を処理する Entity Framework ムービー データベース コンテキストを表します。`Movie`クラス、データベース内のインスタンス。</span><span class="sxs-lookup"><span data-stu-id="71b48-130">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="71b48-131">`MovieDBContext`から派生した、`DbContext`基本クラスを Entity Framework によって提供されます。</span><span class="sxs-lookup"><span data-stu-id="71b48-131">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span> <span data-ttu-id="71b48-132">詳細については`DbContext`と`DbSet`を参照してください[Entity Framework の生産性の向上](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0)します。</span><span class="sxs-lookup"><span data-stu-id="71b48-132">For more information about `DbContext` and `DbSet`, see [Productivity Improvements for the Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span></span>

<span data-ttu-id="71b48-133">参照できるようにするためには`DbContext`と`DbSet`、以下を追加する必要がある`imports`ファイルの上部にあるステートメント。</span><span class="sxs-lookup"><span data-stu-id="71b48-133">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `imports` statement at the top of the file:</span></span>

[!code-vb[Main](adding-a-model/samples/sample3.vb)]

<span data-ttu-id="71b48-134">完全な*Movie.vb*ファイルを次に示します。</span><span class="sxs-lookup"><span data-stu-id="71b48-134">The complete *Movie.vb* file is shown below.</span></span>

[!code-vb[Main](adding-a-model/samples/sample4.vb)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a><span data-ttu-id="71b48-135">接続文字列を作成し、SQL Server Compact の使用</span><span class="sxs-lookup"><span data-stu-id="71b48-135">Creating a Connection String and Working with SQL Server Compact</span></span>

<span data-ttu-id="71b48-136">`MovieDBContext`クラスを作成したデータベースへの接続とマッピングのタスクを処理する`Movie`データベース レコードへのオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="71b48-136">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="71b48-137">疑問に思うかもしれません質問の 1 つは、接続先データベースを指定する方法です。</span><span class="sxs-lookup"><span data-stu-id="71b48-137">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="71b48-138">内の接続情報を追加することによって行います、 *Web.config*アプリケーションのファイル。</span><span class="sxs-lookup"><span data-stu-id="71b48-138">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="71b48-139">アプリケーションのルートを開く*Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="71b48-139">Open the application root *Web.config* file.</span></span> <span data-ttu-id="71b48-140">(いない、 *Web.config*ファイル、*ビュー*フォルダー)。次の図は、両方を表示*Web.config*ファイル; 開く、 *Web.config*ファイル赤い丸でします。</span><span class="sxs-lookup"><span data-stu-id="71b48-140">(Not the *Web.config* file in the *Views* folder.) The image below show both *Web.config* files; open the *Web.config* file circled in red.</span></span>

![](adding-a-model/_static/image2.png)

## 

<span data-ttu-id="71b48-141">次の接続文字列を追加、`<connectionStrings>`内の要素、 *Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="71b48-141">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="71b48-142">次の例の一部を示しています、 *Web.config*ファイルが追加された新しい接続文字列に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="71b48-142">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

<span data-ttu-id="71b48-143">この少量のコードと XML は、すべてを表し、映画のデータをデータベースに格納するために記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="71b48-143">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="71b48-144">新しいを作成します次に、`MoviesController`ムービー データを表示し、新しいムービーの一覧の作成を許可するのに使用できるクラスです。</span><span class="sxs-lookup"><span data-stu-id="71b48-144">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="71b48-145">[前へ](adding-a-view.md)
> [次へ](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="71b48-145">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
