---
title: "ASP.NET MVC を持つコアを Entity Framework Core - 10 のチュートリアル 1"
author: tdykstra
description: 
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/intro
ms.openlocfilehash: 7de43a390ee0e11f6eda811b0774343ab330c53b
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="getting-started-with-aspnet-core-mvc-and-entity-framework-core-using-visual-studio-1-of-10"></a><span data-ttu-id="bb678-102">ASP.NET Core MVC と Visual Studio (10 の 1) を使用して Entity Framework Core の概要</span><span class="sxs-lookup"><span data-stu-id="bb678-102">Getting started with ASP.NET Core MVC and Entity Framework Core using Visual Studio (1 of 10)</span></span>

<span data-ttu-id="bb678-103">によって[Tom Dykstra](https://github.com/tdykstra)と[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bb678-103">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="bb678-104">このチュートリアルの Razor ページのバージョンが利用可能な[ここ](xref:data/ef-rp/intro)です。</span><span class="sxs-lookup"><span data-stu-id="bb678-104">A Razor Pages version of this tutorial is available [here](xref:data/ef-rp/intro).</span></span> <span data-ttu-id="bb678-105">Razor ページ バージョンの方がわかりやすく、多くの EF 機能について説明されています。</span><span class="sxs-lookup"><span data-stu-id="bb678-105">The Razor Pages version is easier to follow and covers more EF features.</span></span> <span data-ttu-id="bb678-106">従うことをお勧め、 [Razor ページ バージョンは、このチュートリアルの](xref:data/ef-rp/intro)します。</span><span class="sxs-lookup"><span data-stu-id="bb678-106">We recommend you follow the [Razor Pages version of this tutorial](xref:data/ef-rp/intro).</span></span>

<span data-ttu-id="bb678-107">Contoso 大学でサンプル web アプリケーションでは、Entity Framework (EF) コア 2.0 と Visual Studio 2017 を使用して ASP.NET Core 2.0 MVC web アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="bb678-107">The Contoso University sample web application demonstrates how to create ASP.NET Core 2.0 MVC web applications using Entity Framework (EF) Core 2.0 and Visual Studio 2017.</span></span>

<span data-ttu-id="bb678-108">サンプル アプリケーションは、架空の Contoso 大学の web サイトです。</span><span class="sxs-lookup"><span data-stu-id="bb678-108">The sample application is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="bb678-109">学生受付、コースの作成、およびインストラクター割り当てなどの機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="bb678-109">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="bb678-110">これは、最初の一連の最初から Contoso 大学サンプル アプリケーションをビルドする方法を説明するチュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="bb678-110">This is the first in a series of tutorials that explain how to build the Contoso University sample application from scratch.</span></span>

[<span data-ttu-id="bb678-111">ダウンロードまたは完成したアプリケーションを表示します。</span><span class="sxs-lookup"><span data-stu-id="bb678-111">Download or view the completed application.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

<span data-ttu-id="bb678-112">EF コア 2.0 が EF の最新バージョンであるが、EF のすべての機能をまだ存在しない 6.x です。</span><span class="sxs-lookup"><span data-stu-id="bb678-112">EF Core 2.0 is the latest version of EF but doesn't yet have all the features of EF 6.x.</span></span> <span data-ttu-id="bb678-113">EF との間を選択する方法については 6.x と EF コアを参照してください。 [EF コア vs です。EF6.x](https://docs.microsoft.com/ef/efcore-and-ef6/).</span><span class="sxs-lookup"><span data-stu-id="bb678-113">For information about how to choose between EF 6.x and EF Core, see [EF Core vs. EF6.x](https://docs.microsoft.com/ef/efcore-and-ef6/).</span></span> <span data-ttu-id="bb678-114">EF を選択した場合、6.x を参照してください[このチュートリアル シリーズの以前のバージョン](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application)します。</span><span class="sxs-lookup"><span data-stu-id="bb678-114">If you choose EF 6.x, see [the previous version of this tutorial series](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).</span></span>

> [!NOTE]
> * <span data-ttu-id="bb678-115">このチュートリアルの ASP.NET Core の 1.1 バージョンを参照してください、 [VS 2017 Update 2 のバージョンを PDF 形式では、このチュートリアルの](https://github.com/aspnet/Docs/blob/master/aspnetcore/data/ef-mvc/intro/_static/efmvc1.1.pdf)します。</span><span class="sxs-lookup"><span data-stu-id="bb678-115">For the ASP.NET Core 1.1 version of this tutorial, see the [VS 2017 Update 2 version of this tutorial in PDF format](https://github.com/aspnet/Docs/blob/master/aspnetcore/data/ef-mvc/intro/_static/efmvc1.1.pdf).</span></span>
> * <span data-ttu-id="bb678-116">このチュートリアルの Visual Studio 2015 バージョンについては、[VS 2015 バージョンの ASP.NET Core ドキュメント (PDF 形式)](https://github.com/aspnet/Docs/blob/master/aspnetcore/common/_static/aspnet-core-project-json.pdf) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="bb678-116">For the Visual Studio 2015 version of this tutorial, see the [VS 2015 version of ASP.NET Core documentation in PDF format](https://github.com/aspnet/Docs/blob/master/aspnetcore/common/_static/aspnet-core-project-json.pdf).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bb678-117">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="bb678-117">Prerequisites</span></span>

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="troubleshooting"></a><span data-ttu-id="bb678-118">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="bb678-118">Troubleshooting</span></span>

<span data-ttu-id="bb678-119">問題を解決できない場合に発生した場合、コードを比較することでのソリューションを見つけることは通常、[完成したプロジェクト](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)です。</span><span class="sxs-lookup"><span data-stu-id="bb678-119">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed project](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final).</span></span> <span data-ttu-id="bb678-120">一般的なエラーとそれらを解決する方法の一覧は、次を参照してください。[系列の最後のチュートリアルの「トラブルシューティング](advanced.md#common-errors)です。</span><span class="sxs-lookup"><span data-stu-id="bb678-120">For a list of common errors and how to solve them, see [the Troubleshooting section of the last tutorial in the series](advanced.md#common-errors).</span></span> <span data-ttu-id="bb678-121">必要なものがない場合は、StackOverflow.com に質問を投稿することができます[ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core)または[EF コア](https://stackoverflow.com/questions/tagged/entity-framework-core)です。</span><span class="sxs-lookup"><span data-stu-id="bb678-121">If you don't find what you need there, you can post a question to StackOverflow.com for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

> [!TIP] 
> <span data-ttu-id="bb678-122">これは、一連の 10 のチュートリアルでは、それぞれは、前のチュートリアルの処理に基づいています。</span><span class="sxs-lookup"><span data-stu-id="bb678-122">This is a series of 10 tutorials, each of which builds on what is done in earlier tutorials.</span></span> <span data-ttu-id="bb678-123">各チュートリアルが正常に完了した後、プロジェクトのコピーを保存することを検討してください。</span><span class="sxs-lookup"><span data-stu-id="bb678-123">Consider saving a copy of the project after each successful tutorial completion.</span></span> <span data-ttu-id="bb678-124">問題に遭遇した場合は、前のチュートリアルの一連の先頭に戻るとではなく、経由で開始できます。</span><span class="sxs-lookup"><span data-stu-id="bb678-124">Then if you run into problems, you can start over from the previous tutorial instead of going back to the beginning of the whole series.</span></span>

## <a name="the-contoso-university-web-application"></a><span data-ttu-id="bb678-125">Contoso 大学 web アプリケーション</span><span class="sxs-lookup"><span data-stu-id="bb678-125">The Contoso University web application</span></span>

<span data-ttu-id="bb678-126">これらのチュートリアルで構築するアプリケーションは、単純な大学 web サイトです。</span><span class="sxs-lookup"><span data-stu-id="bb678-126">The application you'll be building in these tutorials is a simple university web site.</span></span>

<span data-ttu-id="bb678-127">ユーザーでは、表示でき、学生、コース、インストラクターの情報を更新することができます。</span><span class="sxs-lookup"><span data-stu-id="bb678-127">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="bb678-128">ここでは、いくつかの画面を作成します。</span><span class="sxs-lookup"><span data-stu-id="bb678-128">Here are a few of the screens you'll create.</span></span>

![インデックス ページの受講者](intro/_static/students-index.png)

![受講者の編集 ページ](intro/_static/student-edit.png)

<span data-ttu-id="bb678-131">このサイトの UI スタイルを維持して組み込みのテンプレートによって生成されたものに近いチュートリアルは、Entity Framework を使用する方法の主に集中することです。</span><span class="sxs-lookup"><span data-stu-id="bb678-131">The UI style of this site has been kept close to what's generated by the built-in templates, so that the tutorial can focus mainly on how to use the Entity Framework.</span></span>

## <a name="create-an-aspnet-core-mvc-web-application"></a><span data-ttu-id="bb678-132">ASP.NET Core MVC web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="bb678-132">Create an ASP.NET Core MVC web application</span></span>

<span data-ttu-id="bb678-133">Visual Studio を開き、新しい ASP.NET Core c# web という名前のプロジェクト"ContosoUniversity"を作成します。</span><span class="sxs-lookup"><span data-stu-id="bb678-133">Open Visual Studio and create a new ASP.NET Core C# web project named "ContosoUniversity".</span></span>

* <span data-ttu-id="bb678-134">**ファイル**メニューの **新規 > プロジェクト**です。</span><span class="sxs-lookup"><span data-stu-id="bb678-134">From the **File** menu, select **New > Project**.</span></span>

* <span data-ttu-id="bb678-135">左側のウィンドウから次のように選択します。**インストール > Visual c# > Web**です。</span><span class="sxs-lookup"><span data-stu-id="bb678-135">From the left pane, select **Installed > Visual C# > Web**.</span></span>

* <span data-ttu-id="bb678-136">**[ASP.NET Core Web アプリケーション]** プロジェクト テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="bb678-136">Select the **ASP.NET Core Web Application** project template.</span></span>

* <span data-ttu-id="bb678-137">入力**ContosoUniversity**クリックと名前として**OK**です。</span><span class="sxs-lookup"><span data-stu-id="bb678-137">Enter **ContosoUniversity** as the name and click **OK**.</span></span>

  ![[新しいプロジェクト] ダイアログ](intro/_static/new-project.png)

* <span data-ttu-id="bb678-139">待って、**新しい ASP.NET Core Web アプリケーション (.NET Core)**ダイアログを表示するには</span><span class="sxs-lookup"><span data-stu-id="bb678-139">Wait for the **New ASP.NET Core Web Application (.NET Core)** dialog to appear</span></span>

* <span data-ttu-id="bb678-140">選択**ASP.NET Core 2.0**と**Web アプリケーション (モデル-ビュー-コント ローラー)**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="bb678-140">Select **ASP.NET Core 2.0** and the **Web Application (Model-View-Controller)** template.</span></span>

  <span data-ttu-id="bb678-141">**注:**このチュートリアルでは、ASP.NET Core 2.0 と EF コア 2.0 以降--、以下のことを確認が必要です**ASP.NET Core 1.1**が選択されていません。</span><span class="sxs-lookup"><span data-stu-id="bb678-141">**Note:** This tutorial requires ASP.NET Core 2.0 and EF Core 2.0 or later -- make sure that **ASP.NET Core 1.1** isn't selected.</span></span>

* <span data-ttu-id="bb678-142">確認**認証**に設定されている**認証なし**です。</span><span class="sxs-lookup"><span data-stu-id="bb678-142">Make sure **Authentication** is set to **No Authentication**.</span></span>

* <span data-ttu-id="bb678-143">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="bb678-143">Click **OK**</span></span>

  ![[新しい ASP.NET プロジェクト] ダイアログ](intro/_static/new-aspnet.png)

## <a name="set-up-the-site-style"></a><span data-ttu-id="bb678-145">サイトのスタイルを設定します。</span><span class="sxs-lookup"><span data-stu-id="bb678-145">Set up the site style</span></span>

<span data-ttu-id="bb678-146">いくつかの簡単な変更は、[サイト] メニューのレイアウト、およびホーム ページを設定します。</span><span class="sxs-lookup"><span data-stu-id="bb678-146">A few simple changes will set up the site menu, layout, and home page.</span></span>

<span data-ttu-id="bb678-147">開いている*Views/Shared/_Layout.cshtml*次の変更を加えます。</span><span class="sxs-lookup"><span data-stu-id="bb678-147">Open *Views/Shared/_Layout.cshtml* and make the following changes:</span></span>

* <span data-ttu-id="bb678-148">「Contoso 大学」を"ContosoUniversity"の各出現する位置を変更します。</span><span class="sxs-lookup"><span data-stu-id="bb678-148">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="bb678-149">次の 3 つの出現があります。</span><span class="sxs-lookup"><span data-stu-id="bb678-149">There are three occurrences.</span></span>

* <span data-ttu-id="bb678-150">メニュー エントリを追加**受講者**、**コース**、**講習においてインストラクター**、および**部門**、および削除、 **にお問い合わせください**メニュー エントリです。</span><span class="sxs-lookup"><span data-stu-id="bb678-150">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="bb678-151">変更が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="bb678-151">The changes are highlighted.</span></span>

[!code-cshtml[](intro/samples/cu/Views/Shared/_Layout.cshtml?highlight=6,30,36-39,48)]

<span data-ttu-id="bb678-152">*Views/Home/Index.cshtml*ファイルの内容をこのアプリケーションについてのテキストで ASP.NET と MVC に関するテキストを置き換える次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="bb678-152">In *Views/Home/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this application:</span></span>

[!code-cshtml[](intro/samples/cu/Views/Home/Index.cshtml)]

<span data-ttu-id="bb678-153">CTRL + f5 キーを押してプロジェクトを実行または選択**デバッグ > デバッグなしで開始** メニューからです。</span><span class="sxs-lookup"><span data-stu-id="bb678-153">Press CTRL+F5 to run the project or choose **Debug > Start Without Debugging** from the menu.</span></span> <span data-ttu-id="bb678-154">これらのチュートリアルで作成された、ページのタブで、ホーム ページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="bb678-154">You see the home page with tabs for the pages you'll create in these tutorials.</span></span>

![Contoso 大学のホーム ページ](intro/_static/home-page.png)

## <a name="entity-framework-core-nuget-packages"></a><span data-ttu-id="bb678-156">Entity Framework Core NuGet パッケージの管理</span><span class="sxs-lookup"><span data-stu-id="bb678-156">Entity Framework Core NuGet packages</span></span>

<span data-ttu-id="bb678-157">プロジェクトに EF Core のサポートを追加するには、対象となるデータベース プロバイダーをインストールします。</span><span class="sxs-lookup"><span data-stu-id="bb678-157">To add EF Core support to a project, install the database provider that you want to target.</span></span> <span data-ttu-id="bb678-158">このチュートリアルは、SQL Server を使用し、プロバイダー パッケージ[Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/)です。</span><span class="sxs-lookup"><span data-stu-id="bb678-158">This tutorial uses SQL Server, and the provider package is [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span> <span data-ttu-id="bb678-159">このパッケージに含まれる、 [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage、ためをインストールする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="bb678-159">This package is included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="bb678-160">このパッケージとその依存関係 (`Microsoft.EntityFrameworkCore`と`Microsoft.EntityFrameworkCore.Relational`) EF のランタイム サポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="bb678-160">This package and its dependencies (`Microsoft.EntityFrameworkCore` and `Microsoft.EntityFrameworkCore.Relational`) provide runtime support for EF.</span></span> <span data-ttu-id="bb678-161">ツール パッケージを追加後の「、[移行](migrations.md)チュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="bb678-161">You'll add a tooling package later, in the [Migrations](migrations.md) tutorial.</span></span> 

<span data-ttu-id="bb678-162">Entity Framework のコアで利用可能なその他のデータベース プロバイダーについては、次を参照してください。[データベース プロバイダー](https://docs.microsoft.com/ef/core/providers/)です。</span><span class="sxs-lookup"><span data-stu-id="bb678-162">For information about other database providers that are available for Entity Framework Core, see [Database providers](https://docs.microsoft.com/ef/core/providers/).</span></span>

## <a name="create-the-data-model"></a><span data-ttu-id="bb678-163">データ モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="bb678-163">Create the data model</span></span>

<span data-ttu-id="bb678-164">次に、Contoso 大学アプリケーション用にエンティティ クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="bb678-164">Next you'll create entity classes for the Contoso University application.</span></span> <span data-ttu-id="bb678-165">次の 3 つのエンティティを開始します。</span><span class="sxs-lookup"><span data-stu-id="bb678-165">You'll start with the following three entities.</span></span>

![受講者コース-登録データ モデルのダイアグラム](intro/_static/data-model-diagram.png)

<span data-ttu-id="bb678-167">一対多の関係がある`Student`と`Enrollment`エンティティ、一対多の関係があると`Course`と`Enrollment`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="bb678-167">There's a one-to-many relationship between `Student` and `Enrollment` entities, and there's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="bb678-168">つまり、任意の数に、コースの受講者を登録して、コースに受講者に、登録の任意の数を持つことができます。</span><span class="sxs-lookup"><span data-stu-id="bb678-168">In other words, a student can be enrolled in any number of courses, and a course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="bb678-169">次のセクションでは、これらのエンティティのいずれかのクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="bb678-169">In the following sections you'll create a class for each one of these entities.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="bb678-170">学生エンティティ</span><span class="sxs-lookup"><span data-stu-id="bb678-170">The Student entity</span></span>

![学生のエンティティの図](intro/_static/student-entity.png)

<span data-ttu-id="bb678-172">*モデル*フォルダー、という名前のクラス ファイルを作成する*Student.cs*テンプレート コードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="bb678-172">In the *Models* folder, create a class file named *Student.cs* and replace the template code with the following code.</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="bb678-173">`ID`プロパティは、このクラスに対応するデータベース テーブルの主キー列になります。</span><span class="sxs-lookup"><span data-stu-id="bb678-173">The `ID` property will become the primary key column of the database table that corresponds to this class.</span></span> <span data-ttu-id="bb678-174">既定では、Entity Framework では、解釈というプロパティ`ID`または`classnameID`主キーとして。</span><span class="sxs-lookup"><span data-stu-id="bb678-174">By default, the Entity Framework interprets a property that's named `ID` or `classnameID` as the primary key.</span></span>

<span data-ttu-id="bb678-175">`Enrollments`プロパティは、ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="bb678-175">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="bb678-176">ナビゲーション プロパティは、このエンティティに関連するその他のエンティティを保持します。</span><span class="sxs-lookup"><span data-stu-id="bb678-176">Navigation properties hold other entities that are related to this entity.</span></span> <span data-ttu-id="bb678-177">ここで、`Enrollments`のプロパティ、`Student entity`のすべてを保持する、`Enrollment`に関連付けられているエンティティ`Student`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="bb678-177">In this case, the `Enrollments` property of a `Student entity` will hold all of the `Enrollment` entities that are related to that `Student` entity.</span></span> <span data-ttu-id="bb678-178">つまり場合、データベース内の特定の学生行が関連する 2 つ登録行 (その StudentID 外部キー列に、そのスチューデントの主キーの値が含まれている行) を`Student`エンティティの`Enrollments`ナビゲーション プロパティで、それらが格納されます2 つ`Enrollment`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="bb678-178">In other words, if a given Student row in the database has two related Enrollment rows (rows that contain that student's primary key value in their StudentID foreign key column), that `Student` entity's `Enrollments` navigation property will contain those two `Enrollment` entities.</span></span>

<span data-ttu-id="bb678-179">その型が一覧にエントリを追加、削除すると、更新できるなどをする必要があります、ナビゲーション プロパティ (多対多または一対多のリレーションシップ) のように複数のエンティティに保持できる場合`ICollection<T>`です。</span><span class="sxs-lookup"><span data-stu-id="bb678-179">If a navigation property can hold multiple entities (as in many-to-many or one-to-many relationships), its type must be a list in which entries can be added, deleted, and updated, such as `ICollection<T>`.</span></span> <span data-ttu-id="bb678-180">指定できます`ICollection<T>`またはなど型`List<T>`または`HashSet<T>`です。</span><span class="sxs-lookup"><span data-stu-id="bb678-180">You can specify `ICollection<T>` or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="bb678-181">指定した場合`ICollection<T>`、EF の作成、`HashSet<T>`既定のコレクション。</span><span class="sxs-lookup"><span data-stu-id="bb678-181">If you specify `ICollection<T>`, EF creates a `HashSet<T>` collection by default.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="bb678-182">登録エンティティ</span><span class="sxs-lookup"><span data-stu-id="bb678-182">The Enrollment entity</span></span>

![エンティティの登録の図](intro/_static/enrollment-entity.png)

<span data-ttu-id="bb678-184">*モデル*フォルダー作成*Enrollment.cs*し、既存のコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="bb678-184">In the *Models* folder, create *Enrollment.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="bb678-185">`EnrollmentID`プロパティは、主キーになります。 このエンティティを使用して、`classnameID`パターンの代わりに`ID`で学習したとしてそれ自体で、`Student`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="bb678-185">The `EnrollmentID` property will be the primary key; this entity uses the `classnameID` pattern instead of `ID` by itself as you saw in the `Student` entity.</span></span> <span data-ttu-id="bb678-186">通常 1 つパターンを選択し、データ モデル全体で使用するとします。</span><span class="sxs-lookup"><span data-stu-id="bb678-186">Ordinarily you would choose one pattern and use it throughout your data model.</span></span> <span data-ttu-id="bb678-187">ここでは、いずれかのパターンを使用することができます、バリエーションを示しています。</span><span class="sxs-lookup"><span data-stu-id="bb678-187">Here, the variation illustrates that you can use either pattern.</span></span> <span data-ttu-id="bb678-188">[後のチュートリアル](inheritance.md)、classname せず ID を使用して簡単方法、データ モデルで継承の実装が表示されます。</span><span class="sxs-lookup"><span data-stu-id="bb678-188">In a [later tutorial](inheritance.md), you'll see how using ID without classname makes it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="bb678-189">`Grade`プロパティは、`enum`です。</span><span class="sxs-lookup"><span data-stu-id="bb678-189">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="bb678-190">後に疑問符 ()、`Grade`型宣言では、ことを示します、`Grade`プロパティが null 値を許容します。</span><span class="sxs-lookup"><span data-stu-id="bb678-190">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="bb678-191">Null である評価とは異なる、ゼロ グレード--null またはため、評価はまだ割り当てられていません。</span><span class="sxs-lookup"><span data-stu-id="bb678-191">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="bb678-192">`StudentID`プロパティは、foreign key、および対応するナビゲーション プロパティは`Student`します。</span><span class="sxs-lookup"><span data-stu-id="bb678-192">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="bb678-193">`Enrollment`エンティティが 1 つに関連付けられた`Student`エンティティ、プロパティは、1 つのみを保持できるように`Student`エンティティ (とは異なり、`Student.Enrollments`ナビゲーション プロパティ先ほど見た、複数を格納する`Enrollment`エンティティ)。</span><span class="sxs-lookup"><span data-stu-id="bb678-193">An `Enrollment` entity is associated with one `Student` entity, so the property can only hold a single `Student` entity (unlike the `Student.Enrollments` navigation property you saw earlier, which can hold multiple `Enrollment` entities).</span></span>

<span data-ttu-id="bb678-194">`CourseID`プロパティは、foreign key、および対応するナビゲーション プロパティは`Course`します。</span><span class="sxs-lookup"><span data-stu-id="bb678-194">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="bb678-195">`Enrollment`エンティティが 1 つに関連付けられた`Course`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="bb678-195">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="bb678-196">Entity Framework がという名前が場合に、外部キーのプロパティとしてプロパティを解釈`<navigation property name><primary key property name>`(たとえば、`StudentID`の`Student`以降のナビゲーション プロパティ、`Student`エンティティの主キーが`ID`)。</span><span class="sxs-lookup"><span data-stu-id="bb678-196">Entity Framework interprets a property as a foreign key property if it's named `<navigation property name><primary key property name>` (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="bb678-197">外部キー プロパティは単にも呼ばれます`<primary key property name>`(たとえば、`CourseID`ので、`Course`エンティティの主キーが`CourseID`)。</span><span class="sxs-lookup"><span data-stu-id="bb678-197">Foreign key properties can also be named simply `<primary key property name>` (for example, `CourseID` since the `Course` entity's primary key is `CourseID`).</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="bb678-198">Course エンティティ</span><span class="sxs-lookup"><span data-stu-id="bb678-198">The Course entity</span></span>

![Course エンティティの図](intro/_static/course-entity.png)

<span data-ttu-id="bb678-200">*モデル*フォルダー作成*Course.cs*し、既存のコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="bb678-200">In the *Models* folder, create *Course.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="bb678-201">`Enrollments`プロパティは、ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="bb678-201">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="bb678-202">A`Course`エンティティを任意の数に関連付けることができます`Enrollment`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="bb678-202">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="bb678-203">ここでの詳細について、`DatabaseGenerated`属性、[後のチュートリアル](complex-data-model.md)この系列にします。</span><span class="sxs-lookup"><span data-stu-id="bb678-203">We'll say more about the `DatabaseGenerated` attribute in a [later tutorial](complex-data-model.md) in this series.</span></span> <span data-ttu-id="bb678-204">基本的には、この属性には、それを生成、データベースのではなく、コースのプライマリ キーを入力することができます。</span><span class="sxs-lookup"><span data-stu-id="bb678-204">Basically, this attribute lets you enter the primary key for the course rather than having the database generate it.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="bb678-205">データベース コンテキストを作成します。</span><span class="sxs-lookup"><span data-stu-id="bb678-205">Create the Database Context</span></span>

<span data-ttu-id="bb678-206">指定されたデータ モデルの Entity Framework 機能を調整するのメイン クラスは、データベース コンテキスト クラスです。</span><span class="sxs-lookup"><span data-stu-id="bb678-206">The main class that coordinates Entity Framework functionality for a given data model is the database context class.</span></span> <span data-ttu-id="bb678-207">このクラスは、`Microsoft.EntityFrameworkCore.DbContext` クラスから派生させて作成します。</span><span class="sxs-lookup"><span data-stu-id="bb678-207">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span> <span data-ttu-id="bb678-208">コードでは、データ モデルのエンティティが含まれているを指定します。</span><span class="sxs-lookup"><span data-stu-id="bb678-208">In your code you specify which entities are included in the data model.</span></span> <span data-ttu-id="bb678-209">特定の Entity Framework の動作をカスタマイズすることもできます。</span><span class="sxs-lookup"><span data-stu-id="bb678-209">You can also customize certain Entity Framework behavior.</span></span> <span data-ttu-id="bb678-210">クラスの名前は、このプロジェクトで`SchoolContext`です。</span><span class="sxs-lookup"><span data-stu-id="bb678-210">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="bb678-211">プロジェクト フォルダー内には、という名前のフォルダーを作成*データ*です。</span><span class="sxs-lookup"><span data-stu-id="bb678-211">In the project folder, create a folder named *Data*.</span></span>

<span data-ttu-id="bb678-212">*データ*という新しいクラス ファイルを作成するフォルダー *SchoolContext.cs*、テンプレート コードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="bb678-212">In the *Data* folder create a new class file named *SchoolContext.cs*, and replace the template code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

<span data-ttu-id="bb678-213">このコードを作成、`DbSet`各エンティティ セットのプロパティです。</span><span class="sxs-lookup"><span data-stu-id="bb678-213">This code creates a `DbSet` property for each entity set.</span></span> <span data-ttu-id="bb678-214">Entity Framework の用語では、エンティティ セットは通常はデータベース テーブルに対応し、エンティティはテーブルの行に対応します。</span><span class="sxs-lookup"><span data-stu-id="bb678-214">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<span data-ttu-id="bb678-215">省略したことができ、`DbSet<Enrollment>`と`DbSet<Course>`ステートメントと同じように動作します。</span><span class="sxs-lookup"><span data-stu-id="bb678-215">You could've omitted the `DbSet<Enrollment>` and `DbSet<Course>` statements and it would work the same.</span></span> <span data-ttu-id="bb678-216">Entity Framework はそれらを含める暗黙的に、`Student`エンティティ参照、`Enrollment`エンティティと`Enrollment`エンティティ参照、`Course`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="bb678-216">The Entity Framework would include them implicitly because the `Student` entity references the `Enrollment` entity and the `Enrollment` entity references the `Course` entity.</span></span>

<span data-ttu-id="bb678-217">データベースが作成される、EF 作成と同じ名前を持つテーブルを`DbSet`プロパティの名前。</span><span class="sxs-lookup"><span data-stu-id="bb678-217">When the database is created, EF creates tables that have names the same as the `DbSet` property names.</span></span> <span data-ttu-id="bb678-218">コレクションのプロパティ名は、通常 (受講者ではなく学生)、複数形が開発者と異なるかどうかテーブル名をする複数化か。</span><span class="sxs-lookup"><span data-stu-id="bb678-218">Property names for collections are typically plural (Students rather than Student), but developers disagree about whether table names should be pluralized or not.</span></span> <span data-ttu-id="bb678-219">これらのチュートリアル DbContext で単数形のテーブル名を指定して既定の動作をオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="bb678-219">For these tutorials you'll override the default behavior by specifying singular table names in the DbContext.</span></span> <span data-ttu-id="bb678-220">実行するには、DbSet の最後のプロパティの後に、次の強調表示されたコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="bb678-220">To do that, add the following highlighted code after the last DbSet property.</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="bb678-221">依存関係の挿入をコンテキストに登録します。</span><span class="sxs-lookup"><span data-stu-id="bb678-221">Register the context with dependency injection</span></span>

<span data-ttu-id="bb678-222">ASP.NET Core を実装する[依存性の注入](../../fundamentals/dependency-injection.md)既定です。</span><span class="sxs-lookup"><span data-stu-id="bb678-222">ASP.NET Core implements [dependency injection](../../fundamentals/dependency-injection.md) by default.</span></span> <span data-ttu-id="bb678-223">EF データベース コンテキストで) などのサービスは、アプリケーションの起動時に依存関係の挿入に登録されます。</span><span class="sxs-lookup"><span data-stu-id="bb678-223">Services (such as the EF database context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="bb678-224">これらのサービス (など、MVC コント ローラー) を必要とするコンポーネントは、コンス トラクターのパラメーターを使用してこれらのサービスに提供されます。</span><span class="sxs-lookup"><span data-stu-id="bb678-224">Components that require these services (such as MVC controllers) are provided these services via constructor parameters.</span></span> <span data-ttu-id="bb678-225">このチュートリアルで後ほどコンテキストのインスタンスを取得するコント ローラーのコンス トラクター コードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="bb678-225">You'll see the controller constructor code that gets a context instance later in this tutorial.</span></span>

<span data-ttu-id="bb678-226">登録する`SchoolContext`をサービスとして開く*Startup.cs*を強調表示された行を追加し、`ConfigureServices`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="bb678-226">To register `SchoolContext` as a service, open *Startup.cs*, and add the highlighted lines to the `ConfigureServices` method.</span></span>

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

<span data-ttu-id="bb678-227">接続文字列の名前によって渡されるコンテキストでメソッドを呼び出す、`DbContextOptionsBuilder`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="bb678-227">The name of the connection string is passed in to the context by calling a method on a `DbContextOptionsBuilder` object.</span></span> <span data-ttu-id="bb678-228">ローカルの開発、 [ASP.NET Core 構成システム](xref:fundamentals/configuration/index)から接続文字列を読み取り、*される appsettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="bb678-228">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<span data-ttu-id="bb678-229">追加`using`に対してステートメントを`ContosoUniversity.Data`と`Microsoft.EntityFrameworkCore`名前空間、し、プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="bb678-229">Add `using` statements for `ContosoUniversity.Data` and `Microsoft.EntityFrameworkCore` namespaces, and then build the project.</span></span>

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Usings)]

<span data-ttu-id="bb678-230">開く、*される appsettings.json*ファイルし、次の例に示すように、接続文字列を追加します。</span><span class="sxs-lookup"><span data-stu-id="bb678-230">Open the *appsettings.json* file and add a connection string as shown in the following example.</span></span>

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

### <a name="sql-server-express-localdb"></a><span data-ttu-id="bb678-231">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="bb678-231">SQL Server Express LocalDB</span></span>

<span data-ttu-id="bb678-232">接続文字列では、SQL Server LocalDB データベースを指定します。</span><span class="sxs-lookup"><span data-stu-id="bb678-232">The connection string specifies a SQL Server LocalDB database.</span></span> <span data-ttu-id="bb678-233">LocalDB は、SQL Server Express データベース エンジンの簡易バージョンがあり、アプリケーションの開発では、実稼働環境を使用しないものです。</span><span class="sxs-lookup"><span data-stu-id="bb678-233">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for application development, not production use.</span></span> <span data-ttu-id="bb678-234">LocalDB は、要求時に開始され、ユーザー モードで実行されるため、複雑な構成はありません。</span><span class="sxs-lookup"><span data-stu-id="bb678-234">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="bb678-235">既定では、LocalDB が作成されます*.mdf*データベース内のファイル、`C:/Users/<user>`ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="bb678-235">By default, LocalDB creates *.mdf* database files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-database-with-test-data"></a><span data-ttu-id="bb678-236">データベースにテスト データを初期化するコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="bb678-236">Add code to initialize the database with test data</span></span>

<span data-ttu-id="bb678-237">Entity Framework を空のデータベースに作成されます。</span><span class="sxs-lookup"><span data-stu-id="bb678-237">The Entity Framework will create an empty database for you.</span></span> <span data-ttu-id="bb678-238">このセクションで、テスト データを設定するために、データベースが作成された後に呼び出されるメソッドを記述します。</span><span class="sxs-lookup"><span data-stu-id="bb678-238">In this section, you write a method that's called after the database is created in order to populate it with test data.</span></span>

<span data-ttu-id="bb678-239">ここで使用する、`EnsureCreated`メソッドを自動的にデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="bb678-239">Here you'll use the `EnsureCreated` method to automatically create the database.</span></span> <span data-ttu-id="bb678-240">[後のチュートリアル](migrations.md)を削除して、データベースを再作成ではなく、データベース スキーマを変更する Code First Migrations を使用してモデルの変更を処理する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="bb678-240">In a [later tutorial](migrations.md) you'll see how to handle model changes by using Code First Migrations to change the database schema instead of dropping and re-creating the database.</span></span>

<span data-ttu-id="bb678-241">*データ*フォルダー、という名前の新しいクラス ファイルを作成する*DbInitializer.cs*により必要な時に作成されるデータベースの次のコードで、テンプレート コードを置き換えるし、ロード テストを新しいデータデータベースです。</span><span class="sxs-lookup"><span data-stu-id="bb678-241">In the *Data* folder, create a new class file named *DbInitializer.cs* and replace the template code with the following code, which causes a database to be created when needed and loads test data into the new database.</span></span>

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="bb678-242">コードは、場合は、データベースが新しいと、テスト データをシード処理する必要がありますいない場合は、想定していますが、データベースの任意の受講者を確認します。</span><span class="sxs-lookup"><span data-stu-id="bb678-242">The code checks if there are any students in the database, and if not, it assumes the database is new and needs to be seeded with test data.</span></span> <span data-ttu-id="bb678-243">テスト データを配列に読み込むのではなく`List<T>`パフォーマンスを最適化するコレクション。</span><span class="sxs-lookup"><span data-stu-id="bb678-243">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="bb678-244">*Program.cs*、変更、`Main`メソッドをアプリケーションの起動時に次の操作します。</span><span class="sxs-lookup"><span data-stu-id="bb678-244">In *Program.cs*, modify the `Main` method to do the following on application startup:</span></span>

* <span data-ttu-id="bb678-245">依存関係性の注入コンテナーからのデータベース コンテキストのインスタンスを取得します。</span><span class="sxs-lookup"><span data-stu-id="bb678-245">Get a database context instance from the dependency injection container.</span></span>
* <span data-ttu-id="bb678-246">コンテキストを引き渡しシード メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="bb678-246">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="bb678-247">Seed メソッドを実行する場合は、コンテキストを破棄します。</span><span class="sxs-lookup"><span data-stu-id="bb678-247">Dispose the context when the seed method is done.</span></span>

[!code-csharp[Main](intro/samples/cu/Program.cs?name=snippet_Seed&highlight=3-20)]

<span data-ttu-id="bb678-248">追加`using`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="bb678-248">Add `using` statements:</span></span>

[!code-csharp[Main](intro/samples/cu/Program.cs?name=snippet_Usings)]

<span data-ttu-id="bb678-249">古いチュートリアルは、同様のコードを生じる可能性があります、`Configure`メソッド*Startup.cs*です。</span><span class="sxs-lookup"><span data-stu-id="bb678-249">In older tutorials, you may see similar code in the `Configure` method in *Startup.cs*.</span></span> <span data-ttu-id="bb678-250">使用することをお勧め、`Configure`メソッド、要求パイプラインを設定するだけです。</span><span class="sxs-lookup"><span data-stu-id="bb678-250">We recommend that you use the `Configure` method only to set up the request pipeline.</span></span> <span data-ttu-id="bb678-251">アプリケーションのスタートアップ コードが属している、`Main`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="bb678-251">Application startup code belongs in the `Main` method.</span></span>

<span data-ttu-id="bb678-252">今すぐ初めてアプリケーションを実行するデータベースが作成され、テスト データのシード処理します。</span><span class="sxs-lookup"><span data-stu-id="bb678-252">Now the first time you run the application, the database will be created and seeded with test data.</span></span> <span data-ttu-id="bb678-253">データ モデルを変更するたびには、データベースを削除して、そのシード メソッドを更新して開始した後もう一度新しいデータベースと同じ方法ことができます。</span><span class="sxs-lookup"><span data-stu-id="bb678-253">Whenever you change your data model, you can delete the database, update your seed method, and start afresh with a new database the same way.</span></span> <span data-ttu-id="bb678-254">以降のチュートリアルでは、データ モデルの変更、削除して再作成するときに、データベースを変更する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="bb678-254">In later tutorials, you'll see how to modify the database when the data model changes, without deleting and re-creating it.</span></span>

## <a name="create-a-controller-and-views"></a><span data-ttu-id="bb678-255">コント ローラーとビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="bb678-255">Create a controller and views</span></span>

<span data-ttu-id="bb678-256">次に、ある MVC コント ローラーとクエリおよびデータを保存するのに EF を使用するビューを追加するのに Visual Studio でスキャフォールディング エンジンを使用します。</span><span class="sxs-lookup"><span data-stu-id="bb678-256">Next, you'll use the scaffolding engine in Visual Studio to add an MVC controller and views that will use EF to query and save data.</span></span>

<span data-ttu-id="bb678-257">CRUD アクション メソッドとビューの自動作成は、スキャフォールディングと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="bb678-257">The automatic creation of CRUD action methods and views is known as scaffolding.</span></span> <span data-ttu-id="bb678-258">スキャフォールディングは、スキャフォールディング コードが通常生成されたコードを変更しない一方、独自の要件に合わせて変更できる開始点に、コードの生成とは異なります。</span><span class="sxs-lookup"><span data-stu-id="bb678-258">Scaffolding differs from code generation in that the scaffolded code is a starting point that you can modify to suit your own requirements, whereas you typically don't modify generated code.</span></span> <span data-ttu-id="bb678-259">生成されたコードをカスタマイズする必要がある場合は、部分クラスを使用するまたは変更発生時に、コードが再生成します。</span><span class="sxs-lookup"><span data-stu-id="bb678-259">When you need to customize generated code, you use partial classes or you regenerate the code when things change.</span></span>

* <span data-ttu-id="bb678-260">右クリックし、**コント ローラー**フォルダー**ソリューション エクスプ ローラー**選択**追加 > スキャフォールディングされた新しい項目**です。</span><span class="sxs-lookup"><span data-stu-id="bb678-260">Right-click the **Controllers** folder in **Solution Explorer** and select **Add > New Scaffolded Item**.</span></span>

<span data-ttu-id="bb678-261">**[MVC 依存関係の追加]** ダイアログ ボックスが表示された場合は、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="bb678-261">If the **Add MVC Dependencies** dialog appears:</span></span>

* <span data-ttu-id="bb678-262">[Visual Studio を最新バージョンに更新します](https://www.visualstudio.com/downloads/)。</span><span class="sxs-lookup"><span data-stu-id="bb678-262">[Update Visual Studio to the latest version](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="bb678-263">15.5 より前のバージョンの Visual Studio の場合はこのダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="bb678-263">Visual Studio versions prior to 15.5 show this dialog.</span></span>
* <span data-ttu-id="bb678-264">更新できない場合は、**[追加]** を選択してから、もう一度コントローラーの追加手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="bb678-264">If you can't update, select **ADD**, and then follow the add controller steps again.</span></span>

* <span data-ttu-id="bb678-265">**追加 Scaffold**  ダイアログ ボックス。</span><span class="sxs-lookup"><span data-stu-id="bb678-265">In the **Add Scaffold** dialog box:</span></span>

  * <span data-ttu-id="bb678-266">選択**Entity Framework を使用して、ビューがある MVC コント ローラー**です。</span><span class="sxs-lookup"><span data-stu-id="bb678-266">Select **MVC controller with views, using Entity Framework**.</span></span>

  * <span data-ttu-id="bb678-267">**[追加]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="bb678-267">Click **Add**.</span></span>

* <span data-ttu-id="bb678-268">**コント ローラーの追加** ダイアログ ボックス。</span><span class="sxs-lookup"><span data-stu-id="bb678-268">In the **Add Controller** dialog box:</span></span>

  * <span data-ttu-id="bb678-269">**モデル クラス**選択**学生**です。</span><span class="sxs-lookup"><span data-stu-id="bb678-269">In **Model class** select **Student**.</span></span>

  * <span data-ttu-id="bb678-270">**データ コンテキスト クラス**選択**SchoolContext**です。</span><span class="sxs-lookup"><span data-stu-id="bb678-270">In **Data context class** select **SchoolContext**.</span></span>

  * <span data-ttu-id="bb678-271">既定値を受け入れる**StudentsController**名として。</span><span class="sxs-lookup"><span data-stu-id="bb678-271">Accept the default **StudentsController** as the name.</span></span>

  * <span data-ttu-id="bb678-272">**[追加]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="bb678-272">Click **Add**.</span></span>

  ![Scaffold 受講者](intro/_static/scaffold-student.png)

  <span data-ttu-id="bb678-274">クリックすると、**追加**、Visual Studio のスキャフォールディング エンジンを作成、 *StudentsController.cs*ファイルと、一連のビュー (*.cshtml*ファイル)、コント ローラーで動作します。</span><span class="sxs-lookup"><span data-stu-id="bb678-274">When you click **Add**, the Visual Studio scaffolding engine creates a *StudentsController.cs* file and a set of views (*.cshtml* files) that work with the controller.</span></span>

<span data-ttu-id="bb678-275">(スキャフォールディング エンジンも、データベース コンテキストを作成手動で作成しない場合はこのチュートリアルの前に行ったようにまずします。</span><span class="sxs-lookup"><span data-stu-id="bb678-275">(The scaffolding engine can also create the database context for you if you don't create it manually first as you did earlier for this tutorial.</span></span> <span data-ttu-id="bb678-276">新しいコンテキスト クラスを指定することができます、**コント ローラーの追加**の右側にあるプラス記号をクリックしてボックス**データ コンテキスト クラス**です。</span><span class="sxs-lookup"><span data-stu-id="bb678-276">You can specify a new context class in the **Add Controller** box by clicking the plus sign to the right of **Data context class**.</span></span>  <span data-ttu-id="bb678-277">Visual Studio は作成し、`DbContext`コント ローラーとビューだけでなくクラスです)。</span><span class="sxs-lookup"><span data-stu-id="bb678-277">Visual Studio will then create your `DbContext` class as well as the controller and views.)</span></span>

<span data-ttu-id="bb678-278">コント ローラーを取ることがわかります、`SchoolContext`コンス トラクターのパラメーターとして。</span><span class="sxs-lookup"><span data-stu-id="bb678-278">You'll notice that the controller takes a `SchoolContext` as a constructor parameter.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Context&highlight=5,7,9)]

<span data-ttu-id="bb678-279">ASP.NET の依存関係の挿入のインスタンスを渡す場合の注意`SchoolContext`コント ローラーにします。</span><span class="sxs-lookup"><span data-stu-id="bb678-279">ASP.NET dependency injection will take care of passing an instance of `SchoolContext` into the controller.</span></span> <span data-ttu-id="bb678-280">構成されている、 *Startup.cs*前ファイルします。</span><span class="sxs-lookup"><span data-stu-id="bb678-280">You configured that in the *Startup.cs* file earlier.</span></span>

<span data-ttu-id="bb678-281">コント ローラーが含まれています、`Index`アクション メソッドは、データベース内のすべての受講者を表示します。</span><span class="sxs-lookup"><span data-stu-id="bb678-281">The controller contains an `Index` action method, which displays all students in the database.</span></span> <span data-ttu-id="bb678-282">メソッドは、受講者のエンティティを読み取ってセットから受講者の一覧を取得、`Students`データベース コンテキストのインスタンスのプロパティ。</span><span class="sxs-lookup"><span data-stu-id="bb678-282">The method gets a list of students from the Students entity set by reading the `Students` property of the database context instance:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex&highlight=3)]

<span data-ttu-id="bb678-283">このコードで非同期のプログラミング要素は、このチュートリアルで後ほどについて説明します。</span><span class="sxs-lookup"><span data-stu-id="bb678-283">You'll learn about the asynchronous programming elements in this code later in the tutorial.</span></span>

<span data-ttu-id="bb678-284">*Views/Students/Index.cshtml*ビューは、テーブルのこの一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="bb678-284">The *Views/Students/Index.cshtml* view displays this list in a table:</span></span>

[!code-cshtml[](intro/samples/cu/Views/Students/Index1.cshtml)]

<span data-ttu-id="bb678-285">CTRL + f5 キーを押してプロジェクトを実行または選択**デバッグ > デバッグなしで開始** メニューからです。</span><span class="sxs-lookup"><span data-stu-id="bb678-285">Press CTRL+F5 to run the project or choose **Debug > Start Without Debugging** from the menu.</span></span>

<span data-ttu-id="bb678-286">テスト データを表示する、受講者タブをクリックする、`DbInitializer.Initialize`メソッドを挿入します。</span><span class="sxs-lookup"><span data-stu-id="bb678-286">Click the Students tab to see the test data that the `DbInitializer.Initialize` method inserted.</span></span> <span data-ttu-id="bb678-287">どの幅の狭い、ブラウザー ウィンドウがによってが表示されます、`Student`ページの上部にあるタブ リンクは、リンクを表示する右上隅で、ナビゲーション アイコンをクリックする必要があります。</span><span class="sxs-lookup"><span data-stu-id="bb678-287">Depending on how narrow your browser window is, you'll see the `Student` tab link at the top of the page or you'll have to click the navigation icon in the upper right corner to see the link.</span></span>

![Contoso 大学のホーム ページの幅の狭い](intro/_static/home-page-narrow.png)

![インデックス ページの受講者](intro/_static/students-index.png)

## <a name="view-the-database"></a><span data-ttu-id="bb678-290">データベースを表示します。</span><span class="sxs-lookup"><span data-stu-id="bb678-290">View the Database</span></span>

<span data-ttu-id="bb678-291">アプリケーションを起動したときに、`DbInitializer.Initialize`メソッド呼び出し`EnsureCreated`です。</span><span class="sxs-lookup"><span data-stu-id="bb678-291">When you started the application, the `DbInitializer.Initialize` method calls `EnsureCreated`.</span></span> <span data-ttu-id="bb678-292">EF データベースがありませんでした。 そのために作成され、1 つの残りの部分の説明、`Initialize`メソッドのコードには、データベースにデータが設定されます。</span><span class="sxs-lookup"><span data-stu-id="bb678-292">EF saw that there was no database and so it created one, then the remainder of the `Initialize` method code populated the database with data.</span></span> <span data-ttu-id="bb678-293">使用することができます**SQL Server オブジェクト エクスプ ローラー** Visual Studio でデータベースを表示するには、(SSOX)。</span><span class="sxs-lookup"><span data-stu-id="bb678-293">You can use **SQL Server Object Explorer** (SSOX) to view the database in Visual Studio.</span></span>

<span data-ttu-id="bb678-294">ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="bb678-294">Close the browser.</span></span>

<span data-ttu-id="bb678-295">SSOX ウィンドウが開いていない場合を選択してから、**ビュー** Visual Studio のメニュー。</span><span class="sxs-lookup"><span data-stu-id="bb678-295">If the SSOX window isn't already open, select it from the **View** menu in Visual Studio.</span></span>

<span data-ttu-id="bb678-296">SSOX、クリックして**(localdb) \MSSQLLocalDB > データベース**、内の接続文字列に含まれるデータベース名のエントリをクリックして、*される appsettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="bb678-296">In SSOX, click **(localdb)\MSSQLLocalDB > Databases**, and then click the entry for the database name that's in the connection string in your *appsettings.json* file.</span></span>

<span data-ttu-id="bb678-297">展開して、**テーブル**ノードをデータベース内のテーブルを参照してください。</span><span class="sxs-lookup"><span data-stu-id="bb678-297">Expand the **Tables** node to see the tables in your database.</span></span>

![SSOX 内のテーブル](intro/_static/ssox-tables.png)

<span data-ttu-id="bb678-299">右クリックし、**学生**テーブルし、をクリックして**ビュー データ**に作成された列とテーブルに挿入された行を参照してください。</span><span class="sxs-lookup"><span data-stu-id="bb678-299">Right-click the **Student** table and click **View Data** to see the columns that were created and the rows that were inserted into the table.</span></span>

![SSOX で student テーブル](intro/_static/ssox-student-table.png)

<span data-ttu-id="bb678-301">*.Mdf*と*.ldf*データベース ファイルは、 *C:\Users\<yourusername >*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="bb678-301">The *.mdf* and *.ldf* database files are in the *C:\Users\<yourusername>* folder.</span></span>

<span data-ttu-id="bb678-302">呼び出しているため`EnsureCreated`アプリの起動で実行されている初期化メソッドででした今すぐ変更を行うには`Student`クラス、データベースを削除して、もう一度、アプリケーションを実行し、するとデータベースに自動的に変更内容を一致するように再作成します。</span><span class="sxs-lookup"><span data-stu-id="bb678-302">Because you're calling `EnsureCreated` in the initializer method that runs on app start, you could now make a change to the `Student` class, delete the database, run the application again, and the database would automatically be re-created to match your change.</span></span> <span data-ttu-id="bb678-303">たとえば、追加する場合、`EmailAddress`プロパティを`Student`クラスが表示されます、新しい`EmailAddress`再作成されたテーブル内の列です。</span><span class="sxs-lookup"><span data-stu-id="bb678-303">For example, if you add an `EmailAddress` property to the `Student` class, you'll see a new `EmailAddress` column in the re-created table.</span></span>

## <a name="conventions"></a><span data-ttu-id="bb678-304">規約</span><span class="sxs-lookup"><span data-stu-id="bb678-304">Conventions</span></span>

<span data-ttu-id="bb678-305">完全なデータベースを作成できるように、Entity Framework の順序で記述したコードの量は、規則、または Entity Framework は、前提を使用するためは最小限です。</span><span class="sxs-lookup"><span data-stu-id="bb678-305">The amount of code you had to write in order for the Entity Framework to be able to create a complete database for you is minimal because of the use of conventions, or assumptions that the Entity Framework makes.</span></span>

* <span data-ttu-id="bb678-306">名前`DbSet`プロパティ テーブル名として使用されます。</span><span class="sxs-lookup"><span data-stu-id="bb678-306">The names of `DbSet` properties are used as table names.</span></span> <span data-ttu-id="bb678-307">参照されていないエンティティに対して、`DbSet`プロパティ、エンティティ クラスの名前テーブル名として使用されます。</span><span class="sxs-lookup"><span data-stu-id="bb678-307">For entities not referenced by a `DbSet` property, entity class names are used as table names.</span></span>

* <span data-ttu-id="bb678-308">エンティティのプロパティ名は、列名に使用されます。</span><span class="sxs-lookup"><span data-stu-id="bb678-308">Entity property names are used for column names.</span></span>

* <span data-ttu-id="bb678-309">ID または classnameID という名前はエンティティのプロパティは、主キー プロパティとして認識されます。</span><span class="sxs-lookup"><span data-stu-id="bb678-309">Entity properties that are named ID or classnameID are recognized as primary key properties.</span></span>

* <span data-ttu-id="bb678-310">という名前が場合、プロパティが外部キーのプロパティとして解釈されます *<navigation property name> <primary key property name>*  (たとえば、`StudentID`の`Student`以降のナビゲーション プロパティ、`Student`エンティティの主キーとは`ID`).</span><span class="sxs-lookup"><span data-stu-id="bb678-310">A property is interpreted as a foreign key property if it's named *<navigation property name><primary key property name>* (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="bb678-311">外部キー プロパティは単にも呼ばれます *<primary key property name>*  (たとえば、`EnrollmentID`ので、`Enrollment`エンティティの主キーが`EnrollmentID`)。</span><span class="sxs-lookup"><span data-stu-id="bb678-311">Foreign key properties can also be named simply *<primary key property name>* (for example, `EnrollmentID` since the `Enrollment` entity's primary key is `EnrollmentID`).</span></span>

<span data-ttu-id="bb678-312">従来の動作をオーバーライドできます。</span><span class="sxs-lookup"><span data-stu-id="bb678-312">Conventional behavior can be overridden.</span></span> <span data-ttu-id="bb678-313">たとえば、このチュートリアルで既に説明したとおり、テーブル名を明示的に指定することができます。</span><span class="sxs-lookup"><span data-stu-id="bb678-313">For example, you can explicitly specify table names, as you saw earlier in this tutorial.</span></span> <span data-ttu-id="bb678-314">列名を設定してでわかる foreign key、または主キーとして任意のプロパティを設定し、[後のチュートリアル](complex-data-model.md)このシリーズのです。</span><span class="sxs-lookup"><span data-stu-id="bb678-314">And you can set column names and set any property as primary key or foreign key, as you'll see in a [later tutorial](complex-data-model.md) in this series.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="bb678-315">非同期コード</span><span class="sxs-lookup"><span data-stu-id="bb678-315">Asynchronous code</span></span>

<span data-ttu-id="bb678-316">非同期プログラミングは、ASP.NET Core と EF コアの既定モードです。</span><span class="sxs-lookup"><span data-stu-id="bb678-316">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="bb678-317">Web サーバーは、使用可能なスレッド数を限定を持ち、負荷が高い状況でのすべての利用可能なスレッドがありますで使用します。</span><span class="sxs-lookup"><span data-stu-id="bb678-317">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="bb678-318">そのような場合は、サーバーは、スレッドが解放されるまで新しい要求を処理できません。</span><span class="sxs-lookup"><span data-stu-id="bb678-318">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="bb678-319">同期コードはの I/O 完了を待機しているため、実際には、作業の実行されない中に多数のスレッド関連付ける可能性があります。</span><span class="sxs-lookup"><span data-stu-id="bb678-319">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="bb678-320">非同期コードは、プロセスが完了するには I/O の待機している場合、他の要求を処理するために使用するサーバー用に、スレッドが解放されます。</span><span class="sxs-lookup"><span data-stu-id="bb678-320">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="bb678-321">その結果、非同期コード サーバー リソースをより効率的に使用でき、遅延なしのより多くのトラフィックを処理するサーバーが有効になっています。</span><span class="sxs-lookup"><span data-stu-id="bb678-321">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="bb678-322">非同期のコードは、実行時に少量のオーバーヘッドを導入がトラフィックの少ない状況がパフォーマンスの低下はごくわずかであり、中に大量のトラフィックの場合、潜在的なパフォーマンスの向上は大きくします。</span><span class="sxs-lookup"><span data-stu-id="bb678-322">Asynchronous code does introduce a small amount of overhead at run time, but for low traffic situations the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="bb678-323">次のコードで、`async`キーワード、`Task<T>`値を返す`await`キーワード、および`ToListAsync`メソッドが非同期的に実行するコードを作成します。</span><span class="sxs-lookup"><span data-stu-id="bb678-323">In the following code, the `async` keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="bb678-324">`async`キーワード、コンパイラはメソッド本体の各部分のコールバックを生成して自動的に作成する、`Task<IActionResult>`返されるオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="bb678-324">The `async` keyword tells the compiler to generate callbacks for parts of the method body and to automatically create the `Task<IActionResult>` object that's returned.</span></span>

* <span data-ttu-id="bb678-325">戻り値の型`Task<IActionResult>`型の結果で進行中の作業を表す`IActionResult`です。</span><span class="sxs-lookup"><span data-stu-id="bb678-325">The return type `Task<IActionResult>` represents ongoing work with a result of type `IActionResult`.</span></span>

* <span data-ttu-id="bb678-326">`await`キーワードによって、コンパイラにメソッドを 2 つの部分に分割します。</span><span class="sxs-lookup"><span data-stu-id="bb678-326">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="bb678-327">最初の部分は、非同期的に開始される操作を終了します。</span><span class="sxs-lookup"><span data-stu-id="bb678-327">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="bb678-328">2 番目の部分は、操作が完了したときに呼び出されるコールバック メソッドに配置されます。</span><span class="sxs-lookup"><span data-stu-id="bb678-328">The second part is put into a callback method that's called when the operation completes.</span></span>

* <span data-ttu-id="bb678-329">`ToListAsync`非同期バージョンの`ToList`拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="bb678-329">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="bb678-330">Entity Framework を使用する非同期コードを作成する場合の注意すべき点がいくつか:</span><span class="sxs-lookup"><span data-stu-id="bb678-330">Some things to be aware of when you are writing asynchronous code that uses the Entity Framework:</span></span>

* <span data-ttu-id="bb678-331">クエリまたはコマンドのデータベースに送信されるステートメントだけが非同期的に実行されます。</span><span class="sxs-lookup"><span data-stu-id="bb678-331">Only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="bb678-332">たとえばを含む`ToListAsync`、 `SingleOrDefaultAsync`、および`SaveChangesAsync`です。</span><span class="sxs-lookup"><span data-stu-id="bb678-332">That includes, for example, `ToListAsync`, `SingleOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="bb678-333">これが含まれていない、たとえば、だけを変更するステートメントには、`IQueryable`など`var students = context.Students.Where(s => s.LastName == "Davolio")`です。</span><span class="sxs-lookup"><span data-stu-id="bb678-333">It doesn't include, for example, statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>

* <span data-ttu-id="bb678-334">EF コンテキストがスレッド セーフではない: を並列で複数の操作を行うにはしないでください。</span><span class="sxs-lookup"><span data-stu-id="bb678-334">An EF context isn't thread safe: don't try to do multiple operations in parallel.</span></span> <span data-ttu-id="bb678-335">ある非同期 EF メソッドを呼び出すときに、常に使用して、`await`キーワード。</span><span class="sxs-lookup"><span data-stu-id="bb678-335">When you call any async EF method, always use the `await` keyword.</span></span>

* <span data-ttu-id="bb678-336">非同期コードのパフォーマンスの利点を活用、任意のライブラリのパッケージにあるかどうかを確認する場合、(ページングなど) を使用している、データベースに送信されるクエリを Entity Framework メソッドを呼び出す場合にも非同期を使用します。</span><span class="sxs-lookup"><span data-stu-id="bb678-336">If you want to take advantage of the performance benefits of async code, make sure that any library packages that you're using (such as for paging), also use async if they call any Entity Framework methods that cause queries to be sent to the database.</span></span>

<span data-ttu-id="bb678-337">.NET における非同期プログラミングの詳細については、次を参照してください。 [Async 概要](https://docs.microsoft.com/dotnet/articles/standard/async)です。</span><span class="sxs-lookup"><span data-stu-id="bb678-337">For more information about asynchronous programming in .NET, see [Async Overview](https://docs.microsoft.com/dotnet/articles/standard/async).</span></span>

## <a name="summary"></a><span data-ttu-id="bb678-338">まとめ</span><span class="sxs-lookup"><span data-stu-id="bb678-338">Summary</span></span>

<span data-ttu-id="bb678-339">保存し、データを表示する、エンティティ フレームワークのコアと SQL Server Express LocalDB を使用する単純なアプリケーションが作成されました。</span><span class="sxs-lookup"><span data-stu-id="bb678-339">You've now created a simple application that uses the Entity Framework Core and SQL Server Express LocalDB to store and display data.</span></span> <span data-ttu-id="bb678-340">次のチュートリアルでは基本的な CRUD を実行する方法を学習 (作成、読み取り、更新、削除) 操作です。</span><span class="sxs-lookup"><span data-stu-id="bb678-340">In the following tutorial, you'll learn how to perform basic CRUD (create, read, update, delete) operations.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="bb678-341">次へ</span><span class="sxs-lookup"><span data-stu-id="bb678-341">Next</span></span>](crud.md)
