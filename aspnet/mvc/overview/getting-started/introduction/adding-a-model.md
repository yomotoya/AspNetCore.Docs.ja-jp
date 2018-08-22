---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: モデルの追加 |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: a7326cefd52feb63fc2f602210ed560cdf28706a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830309"
---
<a name="adding-a-model"></a><span data-ttu-id="94fac-102">モデルの追加</span><span class="sxs-lookup"><span data-stu-id="94fac-102">Adding a Model</span></span>
====================
<span data-ttu-id="94fac-103">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="94fac-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="94fac-104">このセクションでは、データベースのムービーを管理するためのいくつかのクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="94fac-104">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="94fac-105">これらのクラスがありますが、&quot;モデル&quot;ASP.NET MVC アプリの一部です。</span><span class="sxs-lookup"><span data-stu-id="94fac-105">These classes will be the &quot;model&quot; part of the ASP.NET MVC app.</span></span>

<span data-ttu-id="94fac-106">呼ばれる .NET Framework データ アクセス テクノロジを使用します、 [Entity Framework](https://docs.microsoft.com/ef/)を定義し、これらのモデル クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="94fac-106">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://docs.microsoft.com/ef/) to define and work with these model classes.</span></span> <span data-ttu-id="94fac-107">開発パラダイムと呼ばれる、(EF とも呼ばれます)、Entity Framework がサポート*Code First*します。</span><span class="sxs-lookup"><span data-stu-id="94fac-107">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="94fac-108">まず、コードを使用すると、単純なクラスを記述することで、モデル オブジェクトを作成できます。</span><span class="sxs-lookup"><span data-stu-id="94fac-108">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="94fac-109">(これらとも呼ばれる、POCO クラスから&quot;plain-old CLR object&quot;)。データベースが非常にクリーンで迅速な開発ワークフローを有効にするクラスからその場で作成することができます。</span><span class="sxs-lookup"><span data-stu-id="94fac-109">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span> <span data-ttu-id="94fac-110">まずデータベースを作成するために必要な場合は、MVC と EF のアプリ開発の詳細については、このチュートリアルを引き続きに従ってできます。</span><span class="sxs-lookup"><span data-stu-id="94fac-110">If you are required to create the database first, you can still follow this tutorial to learn about MVC and EF app development.</span></span> <span data-ttu-id="94fac-111">その後 Tom Fizmakens [ASP.NET スキャフォールディング](xref:visual-studio/overview/2013/aspnet-scaffolding-overview)チュートリアルでは、データベースの最初のアプローチを対象とします。</span><span class="sxs-lookup"><span data-stu-id="94fac-111">You can then follow Tom Fizmakens [ASP.NET Scaffolding](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) tutorial, which covers the database first approach.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="94fac-112">モデル クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="94fac-112">Adding Model Classes</span></span>

<span data-ttu-id="94fac-113">**ソリューション エクスプ ローラー**を右クリックして、*モデル*フォルダーで、**追加**、し、**クラス**します。</span><span class="sxs-lookup"><span data-stu-id="94fac-113">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="94fac-114">入力、*クラス*名前&quot;ムービー&quot;します。</span><span class="sxs-lookup"><span data-stu-id="94fac-114">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="94fac-115">次の 5 つのプロパティを追加、`Movie`クラス。</span><span class="sxs-lookup"><span data-stu-id="94fac-115">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="94fac-116">使用して、`Movie`をデータベースのムービーを表すクラス。</span><span class="sxs-lookup"><span data-stu-id="94fac-116">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="94fac-117">各インスタンスを`Movie`オブジェクトは、データベース テーブルとの各プロパティ内の行に対応している、`Movie`クラスは、テーブル内の列にマップされます。</span><span class="sxs-lookup"><span data-stu-id="94fac-117">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="94fac-118">注: System.Data.Entity、および関連するクラスを使用するためにインストールする必要が、 [Entity Framework NuGet パッケージ](https://www.nuget.org/packages/EntityFramework/)します。</span><span class="sxs-lookup"><span data-stu-id="94fac-118">Note: In order to use System.Data.Entity, and the related class, you need to install the [Entity Framework NuGet Package](https://www.nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="94fac-119">詳細については、リンクに従います。</span><span class="sxs-lookup"><span data-stu-id="94fac-119">Follow the link for further instructions.</span></span>

<span data-ttu-id="94fac-120">同じファイルで次の追加`MovieDBContext`クラス。</span><span class="sxs-lookup"><span data-stu-id="94fac-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

<span data-ttu-id="94fac-121">`MovieDBContext`クラスは、フェッチ、格納、および更新を処理する Entity Framework ムービー データベース コンテキストを表します。`Movie`クラス、データベース内のインスタンス。</span><span class="sxs-lookup"><span data-stu-id="94fac-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="94fac-122">`MovieDBContext`から派生した、`DbContext`基本クラスを Entity Framework によって提供されます。</span><span class="sxs-lookup"><span data-stu-id="94fac-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="94fac-123">参照できるようにするためには`DbContext`と`DbSet`、以下を追加する必要がある`using`ファイルの上部にあるステートメント。</span><span class="sxs-lookup"><span data-stu-id="94fac-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="94fac-124">使用して、手動で追加することでこれを行うステートメント、またはすることができます合わせると赤色の波線をクリックします`Show potential fixes` をクリック `using System.Data.Entity;`</span><span class="sxs-lookup"><span data-stu-id="94fac-124">You can do this by manually adding the using statement, or you can hover over the red squiggly lines, click `Show potential fixes` and click `using System.Data.Entity;`</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="94fac-125">注: いくつか使用されていない`using`ステートメントが削除されました。</span><span class="sxs-lookup"><span data-stu-id="94fac-125">Note: Several unused `using` statements have been removed.</span></span> <span data-ttu-id="94fac-126">Visual Studio は、未使用の依存関係を灰色で表示されます。</span><span class="sxs-lookup"><span data-stu-id="94fac-126">Visual Studio will show unused dependencies as gray.</span></span> <span data-ttu-id="94fac-127">ポインターを合わせると灰色の依存関係で使用されていない依存関係を削除するか、をクリックして`Show potential fixes`クリック**未使用の Using の削除します。**</span><span class="sxs-lookup"><span data-stu-id="94fac-127">You can remove unused dependencies by hovering over the gray dependencies, click `Show potential fixes` and click **Remove Unused Usings.**</span></span>

![](adding-a-model/_static/image3.png)

<span data-ttu-id="94fac-128">最後にモデル (MVC で M) が追加されました。</span><span class="sxs-lookup"><span data-stu-id="94fac-128">We've finally added a model (the M in MVC).</span></span> <span data-ttu-id="94fac-129">次のセクションでは、データベース接続文字列を使用します。</span><span class="sxs-lookup"><span data-stu-id="94fac-129">In the next section you'll work with the database connection string.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="94fac-130">[前へ](adding-a-view.md)
> [次へ](creating-a-connection-string.md)</span><span class="sxs-lookup"><span data-stu-id="94fac-130">[Previous](adding-a-view.md)
[Next](creating-a-connection-string.md)</span></span>
