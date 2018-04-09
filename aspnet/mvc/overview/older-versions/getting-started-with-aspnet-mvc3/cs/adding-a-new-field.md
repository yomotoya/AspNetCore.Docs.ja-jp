---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
title: 新しいフィールドを追加すると、ムービーのモデルとテーブル (c#) |Microsoft ドキュメント
author: Rick-Anderson
description: このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: b4e76c1a-f66e-43a0-aa72-f39df79c07c1
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: f6364e438bbb7e128945255a5150e1e84e593ac4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-new-field-to-the-movie-model-and-table-c"></a><span data-ttu-id="89830-103">新しいフィールドを追加すると、ムービーのモデルとテーブル (c#)</span><span class="sxs-lookup"><span data-stu-id="89830-103">Adding a New Field to the Movie Model and Table (C#)</span></span>
====================
<span data-ttu-id="89830-104">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="89830-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="89830-105">このチュートリアルの更新バージョンが利用可能な[ここ](../../../getting-started/introduction/getting-started.md)ASP.NET MVC 5 と Visual Studio 2013 を使用します。</span><span class="sxs-lookup"><span data-stu-id="89830-105">An updated version of this tutorial is available [here](../../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="89830-106">より安全な非常に簡単に従うしより多くの機能を示します。</span><span class="sxs-lookup"><span data-stu-id="89830-106">It's more secure, much simpler to follow and demonstrates more features.</span></span>
> 
> 
> <span data-ttu-id="89830-107">このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これは、無料版の Microsoft Visual Studio を使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。</span><span class="sxs-lookup"><span data-stu-id="89830-107">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="89830-108">開始する前に、以下に示す前提条件がインストールされていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="89830-108">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="89830-109">次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)です。</span><span class="sxs-lookup"><span data-stu-id="89830-109">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="89830-110">また、次のリンクを使用して、前提条件を個別にインストールできます。</span><span class="sxs-lookup"><span data-stu-id="89830-110">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="89830-111">Visual Studio Web Developer Express SP1 の前提条件</span><span class="sxs-lookup"><span data-stu-id="89830-111">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="89830-112">ASP.NET MVC 3 Tools Update します。</span><span class="sxs-lookup"><span data-stu-id="89830-112">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="89830-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)</span><span class="sxs-lookup"><span data-stu-id="89830-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="89830-114">Visual Web Developer 2010 ではなく Visual Studio 2010 を使用している場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)です。</span><span class="sxs-lookup"><span data-stu-id="89830-114">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="89830-115">C# ソース コードの Visual Web Developer プロジェクトは、このトピックを使用できます。</span><span class="sxs-lookup"><span data-stu-id="89830-115">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="89830-116">[C# バージョンをダウンロード](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)です。</span><span class="sxs-lookup"><span data-stu-id="89830-116">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="89830-117">Visual Basic を使用する場合に切り替えて、 [Visual Basic バージョン](../vb/intro-to-aspnet-mvc-3.md)このチュートリアルのです。</span><span class="sxs-lookup"><span data-stu-id="89830-117">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>


<span data-ttu-id="89830-118">このセクションでは、モデルのクラスにいくつかの変更を加えるし、モデルの変更に合わせてデータベース スキーマを更新する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="89830-118">In this section you'll make some changes to the model classes and learn how you can update the database schema to match the model changes.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="89830-119">ムービー モデルへの評価プロパティの追加</span><span class="sxs-lookup"><span data-stu-id="89830-119">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="89830-120">新しく追加することによって開始`Rating`を既存のプロパティ`Movie`クラスです。</span><span class="sxs-lookup"><span data-stu-id="89830-120">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="89830-121">開く、 *Movie.cs*ファイルを追加、`Rating`次のようなプロパティ。</span><span class="sxs-lookup"><span data-stu-id="89830-121">Open the *Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

<span data-ttu-id="89830-122">完全な`Movie`クラスの現在のような次のコード。</span><span class="sxs-lookup"><span data-stu-id="89830-122">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

<span data-ttu-id="89830-123">アプリケーションを使用して、再コンパイル、**デバッグ** &gt;**作成の映画**メニュー コマンド。</span><span class="sxs-lookup"><span data-stu-id="89830-123">Recompile the application using the **Debug** &gt;**Build Movie** menu command.</span></span>

<span data-ttu-id="89830-124">これで、更新した、`Model`クラスも更新する必要が、 *\Views\Movies\Index.cshtml*と*\Views\Movies\Create.cshtml*新しいをサポートするためにテンプレートの表示`Rating`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="89830-124">Now that you've updated the `Model` class, you also need to update the *\Views\Movies\Index.cshtml* and *\Views\Movies\Create.cshtml* view templates in order to support the new `Rating` property.</span></span>

<span data-ttu-id="89830-125">開く、 *\Views\Movies\Index.cshtml*ファイルを追加、`<th>Rating</th>`列見出し直後、**価格**列です。</span><span class="sxs-lookup"><span data-stu-id="89830-125">Open the *\Views\Movies\Index.cshtml* file and add a `<th>Rating</th>` column heading just after the **Price** column.</span></span> <span data-ttu-id="89830-126">追加し、`<td>`をレンダリングするテンプレートの末尾付近の列、`@item.Rating`値。</span><span class="sxs-lookup"><span data-stu-id="89830-126">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="89830-127">以下はどのような更新*Index.cshtml*ビュー テンプレートはようになります。</span><span class="sxs-lookup"><span data-stu-id="89830-127">Below is what the updated *Index.cshtml* view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample3.cshtml)]

<span data-ttu-id="89830-128">次に、開く、 *\Views\Movies\Create.cshtml*ファイルおよびフォームの末尾付近の次のマークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="89830-128">Next, open the *\Views\Movies\Create.cshtml* file and add the following markup near the end of the form.</span></span> <span data-ttu-id="89830-129">これにより、評価を指定するには、新しいムービーの作成時にできるように、テキスト ボックスが表示します。</span><span class="sxs-lookup"><span data-stu-id="89830-129">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample4.cshtml)]

## <a name="managing-model-and-database-schema-differences"></a><span data-ttu-id="89830-130">モデルとデータベース スキーマの相違点を管理します。</span><span class="sxs-lookup"><span data-stu-id="89830-130">Managing Model and Database Schema Differences</span></span>

<span data-ttu-id="89830-131">新しいをサポートするアプリケーション コードを更新するようになりました`Rating`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="89830-131">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="89830-132">これで、アプリケーションを実行しに移動し、 */Movies* URL。</span><span class="sxs-lookup"><span data-stu-id="89830-132">Now run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="89830-133">ただし、これを行うときに、次のエラーを表示されます。</span><span class="sxs-lookup"><span data-stu-id="89830-133">When you do this, though, you'll see the following error:</span></span>

![](adding-a-new-field/_static/image1.png)

<span data-ttu-id="89830-134">このエラーが表示されている、更新された`Movie`アプリケーションのモデル クラスがのスキーマと異なるようになりました、`Movie`既存のデータベースのテーブルです。</span><span class="sxs-lookup"><span data-stu-id="89830-134">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="89830-135">(データベース テーブルに `Rating` 列はありません)。</span><span class="sxs-lookup"><span data-stu-id="89830-135">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="89830-136">既定を使用して Entity Framework Code First を自動的に、データベースの作成と同様、このチュートリアルで前に Code First テーブル データベースに追加して、データベースのスキーマはから生成されたモデルのクラスと同期されているかどうかを追跡できます。</span><span class="sxs-lookup"><span data-stu-id="89830-136">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="89830-137">同期そうでない場合、Entity Framework は、エラーをスローします。</span><span class="sxs-lookup"><span data-stu-id="89830-137">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="89830-138">これにより、それ以外の場合のみがあります (あいまいなエラー) を実行時に開発時に問題を追跡しやすくします。</span><span class="sxs-lookup"><span data-stu-id="89830-138">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span> <span data-ttu-id="89830-139">同期チェック機能は、何が原因で表示されるエラー メッセージだけが表示されます。</span><span class="sxs-lookup"><span data-stu-id="89830-139">The sync-checking feature is what causes the error message to be displayed that you just saw.</span></span>

<span data-ttu-id="89830-140">これには、エラーを解決するための 2 つの方法があります。</span><span class="sxs-lookup"><span data-stu-id="89830-140">There are two approaches to resolving the error:</span></span>

1. <span data-ttu-id="89830-141">Entity Framework に、新しいモデル クラス スキーマに基づいてデータベースを自動的にドロップさせ、再作成させます。</span><span class="sxs-lookup"><span data-stu-id="89830-141">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="89830-142">このので方法は非常に便利にテスト データベースに対するアクティブな開発を行うときに簡単にモデルとデータベースのスキーマを一緒に発展させることができます。</span><span class="sxs-lookup"><span data-stu-id="89830-142">This approach is very convenient when doing active development on a test database, because it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="89830-143">、欠点は、データベース内の既存のデータが失われること、— ようにする*しない*を実稼働データベースでこの方法を使用します。</span><span class="sxs-lookup"><span data-stu-id="89830-143">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span>
2. <span data-ttu-id="89830-144">モデル クラスに一致するように、既存のデータベースのスキーマを明示的に変更します。</span><span class="sxs-lookup"><span data-stu-id="89830-144">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="89830-145">この手法の長所は、データが維持されることです。</span><span class="sxs-lookup"><span data-stu-id="89830-145">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="89830-146">この変更は手動で行うことも、データベース変更スクリプトを作成して行うこともできます。</span><span class="sxs-lookup"><span data-stu-id="89830-146">You can make this change either manually or by creating a database change script.</span></span>

<span data-ttu-id="89830-147">このチュートリアルでは、最初の方法として使用されます:、Entity Framework Code First 自動的にモデルを変更するたびにデータベースを再作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="89830-147">For this tutorial, we'll use the first approach — you'll have the Entity Framework Code First automatically re-create the database anytime the model changes.</span></span>

## <a name="automatically-re-creating-the-database-on-model-changes"></a><span data-ttu-id="89830-148">モデルの変更のデータベースを自動的に再作成</span><span class="sxs-lookup"><span data-stu-id="89830-148">Automatically Re-Creating the Database on Model Changes</span></span>

<span data-ttu-id="89830-149">Code First 自動的に削除し、アプリケーションのモデルを変更するたびに、データベースを再作成できるようにアプリケーションを更新してみましょう。</span><span class="sxs-lookup"><span data-stu-id="89830-149">Let's update the application so that Code First automatically drops and re-creates the database anytime you change the model for the application.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="89830-150">**警告**を自動的に削除して、開発またはテスト データベースを使用している場合にのみ、データベースを再作成するには、この方法を有効にする必要がありますと*決して*実際のデータが含まれている実稼働データベースでします。</span><span class="sxs-lookup"><span data-stu-id="89830-150">**Warning** You should enable this approach of automatically dropping and re-creating the database only when you're using a development or test database, and *never* on a production database that contains real data.</span></span> <span data-ttu-id="89830-151">実稼働サーバー上で使用すると、データ損失につながることができます。</span><span class="sxs-lookup"><span data-stu-id="89830-151">Using it on a production server can lead to data loss.</span></span>


<span data-ttu-id="89830-152">**ソリューション エクスプ ローラー**を右クリックして、*モデル*フォルダーを選択**追加**、し、**クラス**です。</span><span class="sxs-lookup"><span data-stu-id="89830-152">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-new-field/_static/image2.png)

<span data-ttu-id="89830-153">クラス"MovieInitializer"の名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="89830-153">Name the class "MovieInitializer".</span></span> <span data-ttu-id="89830-154">更新プログラム、`MovieInitializer`クラスに次のコードを含めます。</span><span class="sxs-lookup"><span data-stu-id="89830-154">Update the `MovieInitializer` class to contain the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

<span data-ttu-id="89830-155">`MovieInitializer`クラスは、モデルによって使用されるデータベースを削除して自動的に再作成モデル クラスを変更することを指定します。</span><span class="sxs-lookup"><span data-stu-id="89830-155">The `MovieInitializer` class specifies that the database used by the model should be dropped and automatically re-created if the model classes ever change.</span></span> <span data-ttu-id="89830-156">コードが含まれています、`Seed`時間がいくつか自動的にデータベースに追加する、いずれかの既定のデータを指定するメソッドが作成 (または再作成) します。</span><span class="sxs-lookup"><span data-stu-id="89830-156">The code includes a `Seed` method to specify some default data to automatically add to the database any time it's created (or re-created).</span></span> <span data-ttu-id="89830-157">これは、モデルの変更を加えるたびに手動で設定することがなく、一部のサンプル データでは、データベースを設定する便利な方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="89830-157">This provides a useful way to populate the database with some sample data, without requiring you to manually populate it each time you make a model change.</span></span>

<span data-ttu-id="89830-158">これで定義した、`MovieInitializer`クラスにする、アプリケーションを実行するたびにチェックするようモデル クラスは、データベース内のスキーマと異なるかどうかに接続します。</span><span class="sxs-lookup"><span data-stu-id="89830-158">Now that you've defined the `MovieInitializer` class, you'll want to wire it up so that each time the application runs, it checks whether the model classes are different from the schema in the database.</span></span> <span data-ttu-id="89830-159">場合は、モデルと一致し、サンプル データをデータベースにデータベースを再作成する初期化子を実行することができます。</span><span class="sxs-lookup"><span data-stu-id="89830-159">If they are, you can run the initializer to re-create the database to match the model and then populate the database with the sample data.</span></span>

<span data-ttu-id="89830-160">開く、 *Global.asax*のルートにあるファイル、`MvcMovies`プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="89830-160">Open the *Global.asax* file that's at the root of the `MvcMovies` project:</span></span>

[![](adding-a-new-field/_static/image4.png)](adding-a-new-field/_static/image3.png)

<span data-ttu-id="89830-161">*Global.asax*ファイルには、プロジェクトの完全なアプリケーションを定義し、クラスが含まれています、`Application_Start`アプリケーションを初めて起動したときに実行するイベント ハンドラー。</span><span class="sxs-lookup"><span data-stu-id="89830-161">The *Global.asax* file contains the class that defines the entire application for the project, and contains an `Application_Start` event handler that runs when the application first starts.</span></span>

<span data-ttu-id="89830-162">2 つ追加してみましょう。 ファイルの先頭にステートメントを使用します。</span><span class="sxs-lookup"><span data-stu-id="89830-162">Let's add two using statements to the top of the file.</span></span> <span data-ttu-id="89830-163">最初は、Entity Framework の名前空間を参照して、2 つ目は、名前空間を参照場所、`MovieInitializer`生活がどれほどのクラスします。</span><span class="sxs-lookup"><span data-stu-id="89830-163">The first references the Entity Framework namespace, and the second references the namespace where our `MovieInitializer` class lives:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs)]

<span data-ttu-id="89830-164">検索、`Application_Start`メソッド呼び出しを追加および`Database.SetInitializer`次のように、メソッドの先頭に。</span><span class="sxs-lookup"><span data-stu-id="89830-164">Then find the `Application_Start` method and add a call to `Database.SetInitializer` at the beginning of the method, as shown below:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs)]

<span data-ttu-id="89830-165">`Database.SetInitializer`追加したステートメントがによってデータベースが使用されることを示す、`MovieDBContext`インスタンスは自動的に削除して、スキーマとデータベースが一致しない場合を再作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="89830-165">The `Database.SetInitializer` statement you just added indicates that the database used by the `MovieDBContext` instance should be automatically deleted and re-created if the schema and the database don't match.</span></span> <span data-ttu-id="89830-166">説明したとおりにでも、データベースに設定で指定されているサンプル データと、`MovieInitializer`クラスです。</span><span class="sxs-lookup"><span data-stu-id="89830-166">And as you saw, it will also populate the database with the sample data that's specified in the `MovieInitializer` class.</span></span>

<span data-ttu-id="89830-167">閉じる、 *Global.asax*ファイル。</span><span class="sxs-lookup"><span data-stu-id="89830-167">Close the *Global.asax* file.</span></span>

<span data-ttu-id="89830-168">アプリケーションを再実行しに移動し、 */Movies* URL。</span><span class="sxs-lookup"><span data-stu-id="89830-168">Re-run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="89830-169">アプリケーションの起動時には、モデルの構造が不要になったデータベース スキーマと一致することを検出します。</span><span class="sxs-lookup"><span data-stu-id="89830-169">When the application starts, it detects that the model structure no longer matches the database schema.</span></span> <span data-ttu-id="89830-170">自動的に新しいモデル構造と一致するデータベースを再作成され、データベース、サンプルのムービーに設定されます。</span><span class="sxs-lookup"><span data-stu-id="89830-170">It automatically re-creates the database to match the new model structure and populates the database with the sample movies:</span></span>

![7_MyMovieList_SM](adding-a-new-field/_static/image5.png)

<span data-ttu-id="89830-172">をクリックして、**新規作成**リンクを新しいムービーを追加します。</span><span class="sxs-lookup"><span data-stu-id="89830-172">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="89830-173">評価を追加できることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="89830-173">Note that you can add a rating.</span></span>

<span data-ttu-id="89830-174">[![7_CreateRioII](adding-a-new-field/_static/image7.png)](adding-a-new-field/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="89830-174">[![7_CreateRioII](adding-a-new-field/_static/image7.png)](adding-a-new-field/_static/image6.png)</span></span>

<span data-ttu-id="89830-175">**[作成]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="89830-175">Click **Create**.</span></span> <span data-ttu-id="89830-176">この評価を含む、新しいムービーに表示されます、映画を一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="89830-176">The new movie, including the rating, now shows up in the movies listing:</span></span>

<span data-ttu-id="89830-177">[![7_ourNewMovie_SM](adding-a-new-field/_static/image9.png)](adding-a-new-field/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="89830-177">[![7_ourNewMovie_SM](adding-a-new-field/_static/image9.png)](adding-a-new-field/_static/image8.png)</span></span>

<span data-ttu-id="89830-178">このセクションではモデル オブジェクトを変更し、データベースの変更との同期を維持する方法を説明しました。</span><span class="sxs-lookup"><span data-stu-id="89830-178">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="89830-179">また、シナリオを実行するためのサンプル データ、新しく作成されたデータベースに設定する方法も学習しました。</span><span class="sxs-lookup"><span data-stu-id="89830-179">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="89830-180">次に、モデル クラスをより詳細な検証ロジックを追加し、適用するビジネス ルールの一部を有効にする方法を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="89830-180">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="89830-181">[前へ](examining-the-edit-methods-and-edit-view.md)
> [次へ](adding-validation-to-the-model.md)</span><span class="sxs-lookup"><span data-stu-id="89830-181">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-validation-to-the-model.md)</span></span>
