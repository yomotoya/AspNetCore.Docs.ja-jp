---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-new-field
title: Movie モデルとデータベース テーブル (VB) に新しいフィールドの追加 |Microsoft Docs
author: Rick-Anderson
description: このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 28970e1b-1845-4015-86ef-121e52a6c397
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 06d9f08ea3e1a85327083639adc6aa0f2cfbaa48
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382945"
---
<a name="adding-a-new-field-to-the-movie-model-and-database-table-vb"></a><span data-ttu-id="02e5a-103">Movie モデルとデータベース テーブル (VB) に新しいフィールドの追加</span><span class="sxs-lookup"><span data-stu-id="02e5a-103">Adding a New Field to the Movie Model and Database Table (VB)</span></span>
====================
<span data-ttu-id="02e5a-104">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="02e5a-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="02e5a-105">このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、Microsoft Visual Studio の無料版であるを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。</span><span class="sxs-lookup"><span data-stu-id="02e5a-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="02e5a-106">始める前に、以下の前提条件がインストールされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="02e5a-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="02e5a-107">次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)します。</span><span class="sxs-lookup"><span data-stu-id="02e5a-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="02e5a-108">または、次のリンクを使用して、前提条件を個別にインストールできます。</span><span class="sxs-lookup"><span data-stu-id="02e5a-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="02e5a-109">Visual Studio Web Developer Express SP1 の前提条件</span><span class="sxs-lookup"><span data-stu-id="02e5a-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="02e5a-110">ASP.NET MVC 3 Tools Update します。</span><span class="sxs-lookup"><span data-stu-id="02e5a-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="02e5a-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)</span><span class="sxs-lookup"><span data-stu-id="02e5a-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="02e5a-112">Visual Web Developer 2010 ではなく Visual Studio 2010 を使用する場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)します。</span><span class="sxs-lookup"><span data-stu-id="02e5a-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="02e5a-113">VB.NET のソース コードでの Visual Web Developer プロジェクトは、このトピックと共に使用できます。</span><span class="sxs-lookup"><span data-stu-id="02e5a-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="02e5a-114">[VB.NET のバージョンをダウンロード](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)します。</span><span class="sxs-lookup"><span data-stu-id="02e5a-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="02e5a-115">C# を使用する場合に切り替えて、 [c# バージョン](../cs/adding-a-new-field.md)このチュートリアルの。</span><span class="sxs-lookup"><span data-stu-id="02e5a-115">If you prefer C#, switch to the [C# version](../cs/adding-a-new-field.md) of this tutorial.</span></span>


<span data-ttu-id="02e5a-116">このセクションではモデル クラスにいくつかの変更を加えるし、モデルの変更に合わせてデータベース スキーマを更新する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="02e5a-116">In this section you'll make some changes to the model classes and learn how you can update the database schema to match the model changes.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="02e5a-117">ムービー モデルへの評価プロパティの追加</span><span class="sxs-lookup"><span data-stu-id="02e5a-117">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="02e5a-118">まず、新しい追加`Rating`プロパティを既存の`Movie`クラス。</span><span class="sxs-lookup"><span data-stu-id="02e5a-118">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="02e5a-119">開く、 *Movie.cs*追加ファイルを開き、`Rating`次のようなプロパティ。</span><span class="sxs-lookup"><span data-stu-id="02e5a-119">Open the *Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-vb[Main](adding-a-new-field/samples/sample1.vb)]

<span data-ttu-id="02e5a-120">完全な`Movie`今のような次のコードをクラスします。</span><span class="sxs-lookup"><span data-stu-id="02e5a-120">The complete `Movie` class now looks like the following code:</span></span>

[!code-vb[Main](adding-a-new-field/samples/sample2.vb)]

<span data-ttu-id="02e5a-121">アプリケーションを使用して、再コンパイル、**デバッグ** &gt;**ビルド ムービー**メニュー コマンド。</span><span class="sxs-lookup"><span data-stu-id="02e5a-121">Recompile the application using the **Debug** &gt;**Build Movie** menu command.</span></span>

<span data-ttu-id="02e5a-122">更新した、`Model`クラスもする必要がある更新、 *\Views\Movies\Index.vbhtml*と*\Views\Movies\Create.vbhtml*新しいをサポートするためにテンプレートを表示`Rating`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="02e5a-122">Now that you've updated the `Model` class, you also need to update the *\Views\Movies\Index.vbhtml* and *\Views\Movies\Create.vbhtml* view templates in order to support the new `Rating` property.</span></span>

<span data-ttu-id="02e5a-123">開く、<em>\Views\Movies\Index.vbhtml</em>追加ファイルを開き、`<th>Rating</th>`直後の列ヘッダー、<strong>価格</strong>列。</span><span class="sxs-lookup"><span data-stu-id="02e5a-123">Open the<em>\Views\Movies\Index.vbhtml</em> file and add a `<th>Rating</th>` column heading just after the <strong>Price</strong> column.</span></span> <span data-ttu-id="02e5a-124">追加し、`<td>`をレンダリングするテンプレートの末尾付近の列、`@item.Rating`値。</span><span class="sxs-lookup"><span data-stu-id="02e5a-124">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="02e5a-125">以下はどのような更新<em>Index.vbhtml</em>ビュー テンプレートのようになります。</span><span class="sxs-lookup"><span data-stu-id="02e5a-125">Below is what the updated <em>Index.vbhtml</em> view template looks like:</span></span>

[!code-vbhtml[Main](adding-a-new-field/samples/sample3.vbhtml)]

<span data-ttu-id="02e5a-126">次に、開く、 *\Views\Movies\Create.vbhtml*ファイルを開き、フォームの末尾付近の次のマークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="02e5a-126">Next, open the *\Views\Movies\Create.vbhtml* file and add the following markup near the end of the form.</span></span> <span data-ttu-id="02e5a-127">これにより、新しいムービーの作成時に、評価を指定できるように、テキスト ボックスを表示します。</span><span class="sxs-lookup"><span data-stu-id="02e5a-127">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample4.cshtml)]

## <a name="managing-model-and-database-schema-differences"></a><span data-ttu-id="02e5a-128">モデルとデータベース スキーマの相違点を管理します。</span><span class="sxs-lookup"><span data-stu-id="02e5a-128">Managing Model and Database Schema Differences</span></span>

<span data-ttu-id="02e5a-129">新しいをサポートするアプリケーション コードを更新するようになりました`Rating`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="02e5a-129">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="02e5a-130">これで、アプリケーションを実行しに移動し、 */Movies* URL。</span><span class="sxs-lookup"><span data-stu-id="02e5a-130">Now run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="02e5a-131">ただし、これを行うときに、次のエラーを表示されます。</span><span class="sxs-lookup"><span data-stu-id="02e5a-131">When you do this, though, you'll see the following error:</span></span>

![](adding-a-new-field/_static/image1.png)

<span data-ttu-id="02e5a-132">ため、このエラーが表示されている更新された`Movie`アプリケーションでモデル クラスは、スキーマの異なる、`Movie`既存のデータベースのテーブル。</span><span class="sxs-lookup"><span data-stu-id="02e5a-132">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="02e5a-133">(データベース テーブルに `Rating` 列はありません)。</span><span class="sxs-lookup"><span data-stu-id="02e5a-133">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="02e5a-134">既定を使用する Entity Framework Code First のデータベースを自動的に作成するように、このチュートリアルで前に行ったときに Code First テーブル データベースに追加して、データベースのスキーマはから生成されたモデル クラスと同期されているかどうかを追跡できます。</span><span class="sxs-lookup"><span data-stu-id="02e5a-134">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="02e5a-135">同期していない、Entity Framework は、エラーをスローします。</span><span class="sxs-lookup"><span data-stu-id="02e5a-135">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="02e5a-136">これにより、表示されている可能性がありますそれ以外の場合のみ (原因不明のエラー) を実行時に、開発時に問題を追跡しやすくします。</span><span class="sxs-lookup"><span data-stu-id="02e5a-136">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span> <span data-ttu-id="02e5a-137">同期チェック機能は、原因は何を表示するエラー メッセージがあります。</span><span class="sxs-lookup"><span data-stu-id="02e5a-137">The sync-checking feature is what causes the error message to be displayed that you just saw.</span></span>

<span data-ttu-id="02e5a-138">エラーを解決する 2 つの方法はあります。</span><span class="sxs-lookup"><span data-stu-id="02e5a-138">There are two approaches to resolving the error:</span></span>

1. <span data-ttu-id="02e5a-139">Entity Framework に、新しいモデル クラス スキーマに基づいてデータベースを自動的にドロップさせ、再作成させます。</span><span class="sxs-lookup"><span data-stu-id="02e5a-139">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="02e5a-140">この方法は非常に便利な場合、テスト データベースでアクティブな開発を行うため、簡単にまとめてモデルとデータベース スキーマを進化させることができます。</span><span class="sxs-lookup"><span data-stu-id="02e5a-140">This approach is very convenient when doing active development on a test database, because it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="02e5a-141">短所は、データベース内の既存のデータが失われる-ようにする*しない*実稼働データベースでこのアプローチを使用する!</span><span class="sxs-lookup"><span data-stu-id="02e5a-141">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span>
2. <span data-ttu-id="02e5a-142">モデル クラスに一致するように、既存のデータベースのスキーマを明示的に変更します。</span><span class="sxs-lookup"><span data-stu-id="02e5a-142">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="02e5a-143">この手法の長所は、データが維持されることです。</span><span class="sxs-lookup"><span data-stu-id="02e5a-143">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="02e5a-144">この変更は手動で行うことも、データベース変更スクリプトを作成して行うこともできます。</span><span class="sxs-lookup"><span data-stu-id="02e5a-144">You can make this change either manually or by creating a database change script.</span></span>

<span data-ttu-id="02e5a-145">このチュートリアルでは、最初のアプローチを使用します:、Entity Framework Code First 自動的にモデルを変更するたびにデータベースを再作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="02e5a-145">For this tutorial, we'll use the first approach — you'll have the Entity Framework Code First automatically re-create the database anytime the model changes.</span></span>

## <a name="automatically-re-creating-the-database-on-model-changes"></a><span data-ttu-id="02e5a-146">モデルの変更のデータベースを自動的に再作成</span><span class="sxs-lookup"><span data-stu-id="02e5a-146">Automatically Re-Creating the Database on Model Changes</span></span>

<span data-ttu-id="02e5a-147">Code First 自動的に削除し、アプリケーションのモデルを変更するたびに、データベースを再作成できるようにアプリケーションを更新してみましょう。</span><span class="sxs-lookup"><span data-stu-id="02e5a-147">Let's update the application so that Code First automatically drops and re-creates the database anytime you change the model for the application.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="02e5a-148">**警告**を自動的に削除して、開発またはテスト データベースを使用している場合にのみ、データベースを再作成するには、この方法を有効にする必要がありますと*決して*実際のデータを格納している実稼働データベースでします。</span><span class="sxs-lookup"><span data-stu-id="02e5a-148">**Warning** You should enable this approach of automatically dropping and re-creating the database only when you're using a development or test database, and *never* on a production database that contains real data.</span></span> <span data-ttu-id="02e5a-149">実稼働サーバー上で使用すると、データ損失につながることができます。</span><span class="sxs-lookup"><span data-stu-id="02e5a-149">Using it on a production server can lead to data loss.</span></span>


<span data-ttu-id="02e5a-150">**ソリューション エクスプ ローラー**を右クリックして、*モデル*フォルダーで、**追加**、し、**クラス**します。</span><span class="sxs-lookup"><span data-stu-id="02e5a-150">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-new-field/_static/image2.png)

<span data-ttu-id="02e5a-151">クラスの名前&quot;MovieInitializer&quot;します。</span><span class="sxs-lookup"><span data-stu-id="02e5a-151">Name the class &quot;MovieInitializer&quot;.</span></span> <span data-ttu-id="02e5a-152">更新プログラム、`MovieInitializer`クラスに次のコードを含めます。</span><span class="sxs-lookup"><span data-stu-id="02e5a-152">Update the `MovieInitializer` class to contain the following code:</span></span>

[!code-vb[Main](adding-a-new-field/samples/sample5.vb)]

<span data-ttu-id="02e5a-153">`MovieInitializer`クラスは、モデルによって使用されるデータベースを削除してモデル クラスを変更する場合を自動的に再作成することを指定します。</span><span class="sxs-lookup"><span data-stu-id="02e5a-153">The `MovieInitializer` class specifies that the database used by the model should be dropped and automatically re-created if the model classes ever change.</span></span> <span data-ttu-id="02e5a-154">コードが含まれています、`Seed`時間がいくつか既定のデータを自動的にデータベースに追加する、いずれかを指定するメソッドが作成 (または再作成) します。</span><span class="sxs-lookup"><span data-stu-id="02e5a-154">The code includes a `Seed` method to specify some default data to automatically add to the database any time it's created (or re-created).</span></span> <span data-ttu-id="02e5a-155">これは、モデルを変更するたびに手動で設定することがなく、データベースにいくつかサンプル データを設定する便利な方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="02e5a-155">This provides a useful way to populate the database with some sample data, without requiring you to manually populate it each time you make a model change.</span></span>

<span data-ttu-id="02e5a-156">定義したところ、`MovieInitializer`クラスに確認するように、アプリケーションが実行されるたびに、モデル クラスが、データベース内のスキーマと異なるかどうかを接続する必要あります。</span><span class="sxs-lookup"><span data-stu-id="02e5a-156">Now that you've defined the `MovieInitializer` class, you'll want to wire it up so that each time the application runs, it checks whether the model classes are different from the schema in the database.</span></span> <span data-ttu-id="02e5a-157">その場合は、モデルと一致し、サンプル データをデータベースにデータベースを再作成する初期化子を実行できます。</span><span class="sxs-lookup"><span data-stu-id="02e5a-157">If they are, you can run the initializer to re-create the database to match the model and then populate the database with the sample data.</span></span>

<span data-ttu-id="02e5a-158">開く、 *Global.asax*ファイルのルートにある、`MvcMovies`プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="02e5a-158">Open the *Global.asax* file that's at the root of the `MvcMovies` project:</span></span>

<span data-ttu-id="02e5a-159">*Global.asax*ファイルが含まれています、プロジェクトの完全なアプリケーションを定義するクラスが含まれています、`Application_Start`初回起動時にアプリケーションを実行しているイベント ハンドラー。</span><span class="sxs-lookup"><span data-stu-id="02e5a-159">The *Global.asax* file contains the class that defines the entire application for the project, and contains an `Application_Start` event handler that runs when the application first starts.</span></span>

<span data-ttu-id="02e5a-160">検索、`Application_Start`メソッド呼び出しを追加および`Database.SetInitializer`次に示すよう、メソッドの先頭。</span><span class="sxs-lookup"><span data-stu-id="02e5a-160">Find the `Application_Start` method and add a call to `Database.SetInitializer` at the beginning of the method, as shown below:</span></span>

[!code-vb[Main](adding-a-new-field/samples/sample6.vb)]

<span data-ttu-id="02e5a-161">`Database.SetInitializer`ステートメントを追加したが、データベースを使用することを示します、`MovieDBContext`インスタンスは自動的に削除してスキーマとデータベースが一致しない場合を再作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="02e5a-161">The `Database.SetInitializer` statement you just added indicates that the database used by the `MovieDBContext` instance should be automatically deleted and re-created if the schema and the database don't match.</span></span> <span data-ttu-id="02e5a-162">で指定されているサンプル データでデータベースが設定されますも学習したように、`MovieInitializer`クラス。</span><span class="sxs-lookup"><span data-stu-id="02e5a-162">And as you saw, it will also populate the database with the sample data that's specified in the `MovieInitializer` class.</span></span>

<span data-ttu-id="02e5a-163">閉じる、 *Global.asax*ファイル。</span><span class="sxs-lookup"><span data-stu-id="02e5a-163">Close the *Global.asax* file.</span></span>

<span data-ttu-id="02e5a-164">アプリケーションを再実行しに移動し、 */Movies* URL。</span><span class="sxs-lookup"><span data-stu-id="02e5a-164">Re-run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="02e5a-165">アプリケーションの起動時には、モデル構造に、データベース スキーマが一致しなくを検出します。</span><span class="sxs-lookup"><span data-stu-id="02e5a-165">When the application starts, it detects that the model structure no longer matches the database schema.</span></span> <span data-ttu-id="02e5a-166">自動的に、新しいモデルの構造と一致するデータベースを再作成および、サンプルのムービーのデータベースを設定します。</span><span class="sxs-lookup"><span data-stu-id="02e5a-166">It automatically re-creates the database to match the new model structure and populates the database with the sample movies:</span></span>

![7_MyMovieList_SM](adding-a-new-field/_static/image3.png)

<span data-ttu-id="02e5a-168">をクリックして、**新規作成**のリンクを新しいムービーを追加します。</span><span class="sxs-lookup"><span data-stu-id="02e5a-168">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="02e5a-169">評価を追加できることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="02e5a-169">Note that you can add a rating.</span></span>

<span data-ttu-id="02e5a-170">[![7_CreateRioII](adding-a-new-field/_static/image5.png)](adding-a-new-field/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="02e5a-170">[![7_CreateRioII](adding-a-new-field/_static/image5.png)](adding-a-new-field/_static/image4.png)</span></span>

<span data-ttu-id="02e5a-171">**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="02e5a-171">Click **Create**.</span></span> <span data-ttu-id="02e5a-172">評価を含む、新しいムービーに表示されるビデオを一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="02e5a-172">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field/_static/image6.png)

<span data-ttu-id="02e5a-174">このセクションでは、モデル オブジェクトを変更し、データベースの変更との同期を維持する方法を説明しました。</span><span class="sxs-lookup"><span data-stu-id="02e5a-174">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="02e5a-175">シナリオを試すことができますので、サンプル データを使って新しく作成したデータベースを設定する方法も学習しました。</span><span class="sxs-lookup"><span data-stu-id="02e5a-175">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="02e5a-176">次に、高度な検証ロジックはモデル クラスを追加し、いくつかのビジネス ルールを適用するを有効にする方法を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="02e5a-176">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="02e5a-177">[前へ](examining-the-edit-methods-and-edit-view.md)
> [次へ](adding-validation-to-the-model.md)</span><span class="sxs-lookup"><span data-stu-id="02e5a-177">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-validation-to-the-model.md)</span></span>
