---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
title: "モデル (VB) を追加する |Microsoft ドキュメント"
author: Rick-Anderson
description: "このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: b3aa7720-5c78-4ca2-baef-9a52234fb7ce
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: efc18dd71e29d12dacc6cf84a1d3c7f7e92f520d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-model-vb"></a><span data-ttu-id="95fa9-103">モデル (VB) を追加します。</span><span class="sxs-lookup"><span data-stu-id="95fa9-103">Adding a Model (VB)</span></span>
====================
<span data-ttu-id="95fa9-104">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="95fa9-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="95fa9-105">このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これは、無料版の Microsoft Visual Studio を使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。</span><span class="sxs-lookup"><span data-stu-id="95fa9-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="95fa9-106">開始する前に、以下に示す前提条件がインストールされていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="95fa9-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="95fa9-107">次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)です。</span><span class="sxs-lookup"><span data-stu-id="95fa9-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="95fa9-108">また、次のリンクを使用して、前提条件を個別にインストールできます。</span><span class="sxs-lookup"><span data-stu-id="95fa9-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="95fa9-109">Visual Studio Web Developer Express SP1 の前提条件</span><span class="sxs-lookup"><span data-stu-id="95fa9-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="95fa9-110">ASP.NET MVC 3 Tools Update します。</span><span class="sxs-lookup"><span data-stu-id="95fa9-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="95fa9-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)</span><span class="sxs-lookup"><span data-stu-id="95fa9-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="95fa9-112">Visual Web Developer 2010 ではなく Visual Studio 2010 を使用している場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)です。</span><span class="sxs-lookup"><span data-stu-id="95fa9-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="95fa9-113">VB.NET のソース コードと Visual Web Developer プロジェクトは、このトピックを使用できます。</span><span class="sxs-lookup"><span data-stu-id="95fa9-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="95fa9-114">[バージョンをダウンロードして VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)です。</span><span class="sxs-lookup"><span data-stu-id="95fa9-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="95fa9-115">C# を使用する場合に切り替え、 [c# バージョン](../cs/adding-a-model.md)このチュートリアルのです。</span><span class="sxs-lookup"><span data-stu-id="95fa9-115">If you prefer C#, switch to the [C# version](../cs/adding-a-model.md) of this tutorial.</span></span>


## <a name="adding-a-model"></a><span data-ttu-id="95fa9-116">モデルを追加します。</span><span class="sxs-lookup"><span data-stu-id="95fa9-116">Adding a Model</span></span>

<span data-ttu-id="95fa9-117">このセクションでは、データベース内のムービーを管理するためのいくつかのクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="95fa9-117">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="95fa9-118">これらのクラスは、ASP.NET MVC アプリケーションの「モデル」部分になります。</span><span class="sxs-lookup"><span data-stu-id="95fa9-118">These classes will be the "model" part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="95fa9-119">定義し、これらのモデル クラスを使用するには、Entity Framework と呼ばれる .NET Framework データ アクセス テクノロジを使用します。</span><span class="sxs-lookup"><span data-stu-id="95fa9-119">You'll use a .NET Framework data-access technology known as the Entity Framework to define and work with these model classes.</span></span> <span data-ttu-id="95fa9-120">開発パラダイムと呼ばれる、Entity Framework (EF とも呼ばれます) によってサポート*Code First*です。</span><span class="sxs-lookup"><span data-stu-id="95fa9-120">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="95fa9-121">最初のコードでは単純なクラスを作成してモデル オブジェクトを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="95fa9-121">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="95fa9-122">(これらとも呼ばれる従来の CLR オブジェクト"の"から、POCO クラス)これにより、非常にクリーン、迅速な開発ワークフローのクラスから実行時に作成されたデータベースを持つことができます。</span><span class="sxs-lookup"><span data-stu-id="95fa9-122">(These are also known as POCO classes, from "plain-old CLR objects.") You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="95fa9-123">モデル クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="95fa9-123">Adding Model Classes</span></span>

<span data-ttu-id="95fa9-124">**ソリューション エクスプ ローラー**を右クリックして、*モデル*フォルダーを選択**追加**、し、**クラス**です。</span><span class="sxs-lookup"><span data-stu-id="95fa9-124">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="95fa9-125">クラス「ムービー」の名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="95fa9-125">Name the class "Movie".</span></span>

<span data-ttu-id="95fa9-126">次の 5 つのプロパティを追加、`Movie`クラス。</span><span class="sxs-lookup"><span data-stu-id="95fa9-126">Add the following five properties to the `Movie` class:</span></span>

[!code-vb[Main](adding-a-model/samples/sample1.vb)]

<span data-ttu-id="95fa9-127">使用して、`Movie`データベースでムービーを表すクラス。</span><span class="sxs-lookup"><span data-stu-id="95fa9-127">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="95fa9-128">各インスタンス、`Movie`オブジェクトは、データベース テーブルとの各プロパティ内の行に対応している、`Movie`クラスは、テーブル内の列にマップされます。</span><span class="sxs-lookup"><span data-stu-id="95fa9-128">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="95fa9-129">同じファイルで次のコードを追加`MovieDBContext`クラス。</span><span class="sxs-lookup"><span data-stu-id="95fa9-129">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-vb[Main](adding-a-model/samples/sample2.vb)]

<span data-ttu-id="95fa9-130">`MovieDBContext`クラスを表します。 Entity Framework ムービーのデータベース コンテキストのフェッチ、保存、および更新を処理する`Movie`クラスのインスタンス、データベースにします。</span><span class="sxs-lookup"><span data-stu-id="95fa9-130">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="95fa9-131">`MovieDBContext`から派生した、`DbContext`基本 Entity Framework によって提供されるクラスです。</span><span class="sxs-lookup"><span data-stu-id="95fa9-131">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span> <span data-ttu-id="95fa9-132">詳細については`DbContext`と`DbSet`を参照してください[Entity Framework 用の生産性向上](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0)です。</span><span class="sxs-lookup"><span data-stu-id="95fa9-132">For more information about `DbContext` and `DbSet`, see [Productivity Improvements for the Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span></span>

<span data-ttu-id="95fa9-133">参照できるように`DbContext`と`DbSet`、以下を追加する必要があります`imports`ファイルの上部にあるステートメント。</span><span class="sxs-lookup"><span data-stu-id="95fa9-133">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `imports` statement at the top of the file:</span></span>

[!code-vb[Main](adding-a-model/samples/sample3.vb)]

<span data-ttu-id="95fa9-134">完全な*Movie.vb*ファイルを次に示します。</span><span class="sxs-lookup"><span data-stu-id="95fa9-134">The complete *Movie.vb* file is shown below.</span></span>

[!code-vb[Main](adding-a-model/samples/sample4.vb)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a><span data-ttu-id="95fa9-135">接続文字列を作成し、SQL Server Compact 協力</span><span class="sxs-lookup"><span data-stu-id="95fa9-135">Creating a Connection String and Working with SQL Server Compact</span></span>

<span data-ttu-id="95fa9-136">`MovieDBContext`作成したクラスは、データベースに接続し、タスクのマッピングを処理`Movie`データベース レコードへのオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="95fa9-136">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="95fa9-137">1 つに質問する可能性がありますには、接続先データベースを指定する方法です。</span><span class="sxs-lookup"><span data-stu-id="95fa9-137">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="95fa9-138">内の接続情報を追加することによって行うこと、 *Web.config*アプリケーションのファイルです。</span><span class="sxs-lookup"><span data-stu-id="95fa9-138">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="95fa9-139">アプリケーションのルートを開く*Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="95fa9-139">Open the application root *Web.config* file.</span></span> <span data-ttu-id="95fa9-140">(されません、 *Web.config*ファイルで、*ビュー*フォルダーです)。次の図を 両方表示*Web.config* ; 開いているファイル、 *Web.config*赤い円で囲まれたファイルです。</span><span class="sxs-lookup"><span data-stu-id="95fa9-140">(Not the *Web.config* file in the *Views* folder.) The image below show both *Web.config* files; open the *Web.config* file circled in red.</span></span>

![](adding-a-model/_static/image2.png)

## 

<span data-ttu-id="95fa9-141">次の接続文字列を追加、`<connectionStrings>`内の要素、 *Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="95fa9-141">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="95fa9-142">次の例の一部を示しています、 *Web.config*新しい接続文字列を追加してファイル。</span><span class="sxs-lookup"><span data-stu-id="95fa9-142">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

<span data-ttu-id="95fa9-143">このような少量のコードおよび XML を表し、ムービー データをデータベースに格納するために記述する必要がありますすべてがあります。</span><span class="sxs-lookup"><span data-stu-id="95fa9-143">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="95fa9-144">新しいビルド次に、`MoviesController`ムービー データを表示し、新しいムービーの一覧を作成できるように使用できるクラスです。</span><span class="sxs-lookup"><span data-stu-id="95fa9-144">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="95fa9-145">[前へ](adding-a-view.md)
[次へ](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="95fa9-145">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
