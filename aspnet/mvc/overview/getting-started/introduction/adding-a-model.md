---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: モデルを追加する |Microsoft ドキュメント
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: b3ef871c4d7627a03c8f0fd8cce9d3e97fc1a4ba
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30867451"
---
<a name="adding-a-model"></a><span data-ttu-id="2d684-102">モデルを追加します。</span><span class="sxs-lookup"><span data-stu-id="2d684-102">Adding a Model</span></span>
====================
<span data-ttu-id="2d684-103">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="2d684-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="2d684-104">このセクションでは、データベース内のムービーを管理するためのいくつかのクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="2d684-104">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="2d684-105">これらのクラスになります、&quot;モデル&quot;ASP.NET MVC アプリの一部です。</span><span class="sxs-lookup"><span data-stu-id="2d684-105">These classes will be the &quot;model&quot; part of the ASP.NET MVC app.</span></span>

<span data-ttu-id="2d684-106">呼ばれる .NET Framework データ アクセス テクノロジを使用、 [Entity Framework](https://docs.microsoft.com/ef/)を定義し、これらのモデル クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="2d684-106">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://docs.microsoft.com/ef/) to define and work with these model classes.</span></span> <span data-ttu-id="2d684-107">開発パラダイムと呼ばれる、Entity Framework (EF とも呼ばれます) によってサポート*Code First*です。</span><span class="sxs-lookup"><span data-stu-id="2d684-107">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="2d684-108">最初のコードでは単純なクラスを作成してモデル オブジェクトを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="2d684-108">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="2d684-109">(これらとも呼ばれる POCO クラスから&quot;従来の CLR オブジェクト&quot;)。これにより、非常にクリーン、迅速な開発ワークフローのクラスから実行時に作成されたデータベースを持つことができます。</span><span class="sxs-lookup"><span data-stu-id="2d684-109">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span> <span data-ttu-id="2d684-110">まず、データベースを作成するために必要な場合は、MVC および EF アプリの開発に関する学習するのには、このチュートリアルをまだに従ってできます。</span><span class="sxs-lookup"><span data-stu-id="2d684-110">If you are required to create the database first, you can still follow this tutorial to learn about MVC and EF app development.</span></span> <span data-ttu-id="2d684-111">Tom Fizmakens を行うことができる、 [ASP.NET スキャフォールディング](xref:visual-studio/overview/2013/aspnet-scaffolding-overview)チュートリアル」では、データベースの最初の方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="2d684-111">You can then follow Tom Fizmakens [ASP.NET Scaffolding](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) tutorial, which covers the database first approach.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="2d684-112">モデル クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="2d684-112">Adding Model Classes</span></span>

<span data-ttu-id="2d684-113">**ソリューション エクスプ ローラー**を右クリックして、*モデル*フォルダーを選択**追加**、し、**クラス**です。</span><span class="sxs-lookup"><span data-stu-id="2d684-113">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="2d684-114">入力、*クラス*名前&quot;ムービー&quot;です。</span><span class="sxs-lookup"><span data-stu-id="2d684-114">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="2d684-115">次の 5 つのプロパティを追加、`Movie`クラス。</span><span class="sxs-lookup"><span data-stu-id="2d684-115">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="2d684-116">使用して、`Movie`データベースでムービーを表すクラス。</span><span class="sxs-lookup"><span data-stu-id="2d684-116">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="2d684-117">各インスタンス、`Movie`オブジェクトは、データベース テーブルとの各プロパティ内の行に対応している、`Movie`クラスは、テーブル内の列にマップされます。</span><span class="sxs-lookup"><span data-stu-id="2d684-117">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="2d684-118">注: System.Data.Entity、および関連クラスを使用するためにインストールする必要が、 [Entity Framework の NuGet パッケージ](https://www.nuget.org/packages/EntityFramework/)です。</span><span class="sxs-lookup"><span data-stu-id="2d684-118">Note: In order to use System.Data.Entity, and the related class, you need to install the [Entity Framework NuGet Package](https://www.nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="2d684-119">詳細については、リンクに従います。</span><span class="sxs-lookup"><span data-stu-id="2d684-119">Follow the link for further instructions.</span></span>

<span data-ttu-id="2d684-120">同じファイルで次のコードを追加`MovieDBContext`クラス。</span><span class="sxs-lookup"><span data-stu-id="2d684-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

<span data-ttu-id="2d684-121">`MovieDBContext`クラスを表します。 Entity Framework ムービーのデータベース コンテキストのフェッチ、保存、および更新を処理する`Movie`クラスのインスタンス、データベースにします。</span><span class="sxs-lookup"><span data-stu-id="2d684-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="2d684-122">`MovieDBContext`から派生した、`DbContext`基本 Entity Framework によって提供されるクラスです。</span><span class="sxs-lookup"><span data-stu-id="2d684-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="2d684-123">参照できるように`DbContext`と`DbSet`、以下を追加する必要があります`using`ファイルの上部にあるステートメント。</span><span class="sxs-lookup"><span data-stu-id="2d684-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="2d684-124">使用して、手動で追加してこれを行うステートメント、またはすることができます赤の波線を合わせるをクリックして`Show potential fixes` をクリック `using System.Data.Entity;`</span><span class="sxs-lookup"><span data-stu-id="2d684-124">You can do this by manually adding the using statement, or you can hover over the red squiggly lines, click `Show potential fixes` and click `using System.Data.Entity;`</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="2d684-125">注: いくつか使用されていない`using`ステートメントが削除されました。</span><span class="sxs-lookup"><span data-stu-id="2d684-125">Note: Several unused `using` statements have been removed.</span></span> <span data-ttu-id="2d684-126">Visual Studio では、依存関係で未使用を灰色で表示します。</span><span class="sxs-lookup"><span data-stu-id="2d684-126">Visual Studio will show unused dependencies as gray.</span></span> <span data-ttu-id="2d684-127">灰色の依存関係を合わせることで未使用の依存関係を削除するか、をクリックして`Show potential fixes` をクリック**未使用の Using の削除します。**</span><span class="sxs-lookup"><span data-stu-id="2d684-127">You can remove unused dependencies by hovering over the gray dependencies, click `Show potential fixes` and click **Remove Unused Usings.**</span></span>

![](adding-a-model/_static/image3.png)

<span data-ttu-id="2d684-128">最後に、モデル (MVC で M) が追加されました。</span><span class="sxs-lookup"><span data-stu-id="2d684-128">We've finally added a model (the M in MVC).</span></span> <span data-ttu-id="2d684-129">次のセクションでは、データベース接続文字列を使用します。</span><span class="sxs-lookup"><span data-stu-id="2d684-129">In the next section you'll work with the database connection string.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2d684-130">[前へ](adding-a-view.md)
> [次へ](creating-a-connection-string.md)</span><span class="sxs-lookup"><span data-stu-id="2d684-130">[Previous](adding-a-view.md)
[Next](creating-a-connection-string.md)</span></span>
