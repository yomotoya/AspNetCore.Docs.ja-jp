---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: データベースを作成する |Microsoft Docs
author: shanselman
description: これは、ASP.NET MVC の基本を紹介する初心者向けチュートリアルです。 読み取りと書き込みをデータベースから単純な web アプリケーションを作成します。
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: fff974edfaffeb164879c10b8e70bd3482c47554
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813761"
---
<a name="creating-a-database"></a><span data-ttu-id="40f62-104">データベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="40f62-104">Creating a Database</span></span>
====================
<span data-ttu-id="40f62-105">によって[Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="40f62-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="40f62-106">これは、ASP.NET MVC の基本を紹介する初心者向けチュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="40f62-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="40f62-107">読み取りと書き込みをデータベースから単純な web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="40f62-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="40f62-108">参照してください、 [ASP.NET MVC ラーニング センター](../../../index.md)チュートリアルとサンプルは、その他の ASP.NET MVC を検索します。</span><span class="sxs-lookup"><span data-stu-id="40f62-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="40f62-109">このセクションでは、ムービー データ格納および取得に使用する新しい SQL Express データベースを作成するいきます。</span><span class="sxs-lookup"><span data-stu-id="40f62-109">In this section we are going to create a new SQL Express database that we'll use to store and retrieve our movie data.</span></span> <span data-ttu-id="40f62-110">Visual Web Developer IDE 内での選択ビュー |サーバー エクスプ ローラー。</span><span class="sxs-lookup"><span data-stu-id="40f62-110">From within the Visual Web Developer IDE, select View | Server Explorer.</span></span> <span data-ttu-id="40f62-111">データ接続を右クリックし、[接続の追加] をクリックしてください.</span><span class="sxs-lookup"><span data-stu-id="40f62-111">Right click on Data Connections and click Add Connection...</span></span>

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

<span data-ttu-id="40f62-113">データ ソースの選択 ダイアログ ボックスで、Microsoft SQL Server を選択し、続行 を選択します。</span><span class="sxs-lookup"><span data-stu-id="40f62-113">In the Choose Data Source dialog, select Microsoft SQL Server and select Continue.</span></span>

![](getting-started-with-mvc-part4/_static/image2.png)

<span data-ttu-id="40f62-114">接続の追加 ダイアログ ボックスで次のように入力します。"。 \SQLEXPRESS"、サーバー名に対応し、新しいデータベースの名前として"Movies"を入力します。</span><span class="sxs-lookup"><span data-stu-id="40f62-114">In the Add Connection dialog, enter ".\SQLEXPRESS" for your Server Name, and enter "Movies" as the name for your new database.</span></span>

<span data-ttu-id="40f62-115">[![追加の接続 ダイアログ ボックス](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="40f62-115">[![Add Connection dialog](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span></span>

<span data-ttu-id="40f62-116">[OK] をクリックし、求められますかどうかは、そのデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="40f62-116">Click OK and you'll be asked if you want to create that database.</span></span> <span data-ttu-id="40f62-117">[はい] を選択します。</span><span class="sxs-lookup"><span data-stu-id="40f62-117">Select yes.</span></span>

<span data-ttu-id="40f62-118">[![ムービーを作成しますか。](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="40f62-118">[![Create Movies?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span></span>

<span data-ttu-id="40f62-119">サーバー エクスプ ローラーで、空のデータベースがあります。</span><span class="sxs-lookup"><span data-stu-id="40f62-119">Now you've got an empty database in Server Explorer.</span></span>

![新しいテーブルを追加します。](getting-started-with-mvc-part4/_static/image7.png)

<span data-ttu-id="40f62-121">テーブルを右クリックし、[テーブルの追加] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="40f62-121">Right click on Tables and click Add Table.</span></span> <span data-ttu-id="40f62-122">テーブル デザイナーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="40f62-122">The Table Designer will appear.</span></span> <span data-ttu-id="40f62-123">Id、タイトル、ReleaseDate、ジャンル、および価格の列を追加します。</span><span class="sxs-lookup"><span data-stu-id="40f62-123">Add columns for Id, Title, ReleaseDate, Genre, and Price.</span></span> <span data-ttu-id="40f62-124">ID 列を右クリックし、主キーの設定 をクリックします。</span><span class="sxs-lookup"><span data-stu-id="40f62-124">Right click on the ID column and click set Primary Key.</span></span> <span data-ttu-id="40f62-125">次のようになります、どのようなデザイン領域に示します。</span><span class="sxs-lookup"><span data-stu-id="40f62-125">Here's what my design areas looks like.</span></span>

<span data-ttu-id="40f62-126">[![データベース テーブル エディター](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="40f62-126">[![Database Table Editor](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span></span>

<span data-ttu-id="40f62-127">また、Id 列を選択し、以下の列のプロパティを"Yes"にします「Id の指定」を変更します。</span><span class="sxs-lookup"><span data-stu-id="40f62-127">Also, select the Id column and under Column Properties below change "Identity Specification" to "Yes."</span></span>

<span data-ttu-id="40f62-128">[![IsIdentity - 列のプロパティ](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="40f62-128">[![IsIdentity - Column Properties](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span></span>

<span data-ttu-id="40f62-129">これを取得したら、ツールバーの [保存] アイコンをクリックしますまたはファイルを選択します |。メニューから、保存し、テーブルの名前"**ムービー**"(単数形)。</span><span class="sxs-lookup"><span data-stu-id="40f62-129">When you've got it done, click the Save icon in the toolbar or select File | Save from the menu, and name your table "**Movie**" (singular).</span></span> <span data-ttu-id="40f62-130">データベースとテーブルを作成しました!</span><span class="sxs-lookup"><span data-stu-id="40f62-130">We've got a database and a table!</span></span>

<span data-ttu-id="40f62-131">[![名前を選択します。](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="40f62-131">[![Choose Name](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span></span>

<span data-ttu-id="40f62-132">サーバー エクスプ ローラーに戻り、Movie テーブルを右クリックし、"テーブル データの表示。 を選択します。</span><span class="sxs-lookup"><span data-stu-id="40f62-132">Go back to Server Explorer and right click the Movie table, then select "Show Table Data."</span></span> <span data-ttu-id="40f62-133">データベースにいくつかのデータがあるため、いくつかの映画を入力します。</span><span class="sxs-lookup"><span data-stu-id="40f62-133">Enter a few movies so our database has some data.</span></span>

<span data-ttu-id="40f62-134">[![データベース テーブルの編集](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span><span class="sxs-lookup"><span data-stu-id="40f62-134">[![Database Table Editing](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span></span>

## <a name="creating-a-model"></a><span data-ttu-id="40f62-135">モデルの作成</span><span class="sxs-lookup"><span data-stu-id="40f62-135">Creating a Model</span></span>

<span data-ttu-id="40f62-136">ここで、IDE の右側に、ソリューション エクスプ ローラーに戻るし、Models フォルダーを右クリックし、追加 |新しい項目。</span><span class="sxs-lookup"><span data-stu-id="40f62-136">Now, switch back to the Solution Explorer on the right hand side of the IDE and right-click on the Models folder and select Add | New Item.</span></span>

<span data-ttu-id="40f62-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="40f62-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span></span>

<span data-ttu-id="40f62-138">新しいデータベースからエンティティ モデルを作成しようとしています。</span><span class="sxs-lookup"><span data-stu-id="40f62-138">We're going to create an Entity Model from our new database.</span></span> <span data-ttu-id="40f62-139">これにより、一連のクラスを照会して、データベース内のデータを操作するが容易にするプロジェクトに追加されます。</span><span class="sxs-lookup"><span data-stu-id="40f62-139">This will add a set of classes to our project that makes it easy for us to query and manipulate the data within our database.</span></span> <span data-ttu-id="40f62-140">ダイアログの左側にあるデータ ノードを選択し、ADO.NET Entity Data Model 項目テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="40f62-140">Select the Data node on the left-hand side of the dialog, and then select the ADO.NET Entity Data Model item template.</span></span> <span data-ttu-id="40f62-141">Movies.edmx 名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="40f62-141">Name it Movies.edmx.</span></span>

<span data-ttu-id="40f62-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span><span class="sxs-lookup"><span data-stu-id="40f62-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span></span>

<span data-ttu-id="40f62-143">[追加] ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="40f62-143">Click the "Add" button.</span></span> <span data-ttu-id="40f62-144">これが、「Entity Data Model ウィザード」を起動しています。</span><span class="sxs-lookup"><span data-stu-id="40f62-144">This will then launch the "Entity Data Model Wizard".</span></span>

<span data-ttu-id="40f62-145">新しいダイアログが表示されたらでは、データベースから生成を選択します。</span><span class="sxs-lookup"><span data-stu-id="40f62-145">In the new dialog that pops up, select Generate from Database.</span></span> <span data-ttu-id="40f62-146">データベースだけしていますので、新しいデータベースとそのテーブルについて、Entity Framework にのみ必要あります。</span><span class="sxs-lookup"><span data-stu-id="40f62-146">Since we've just made a database, we'll only need to tell the Entity Framework about our new database and its table.</span></span> <span data-ttu-id="40f62-147">Web アプリケーションの構成では、データベース接続の横をクリックします。</span><span class="sxs-lookup"><span data-stu-id="40f62-147">Click Next to save our database connection in our web application's configuration.</span></span> <span data-ttu-id="40f62-148">ここで、テーブルとムービーを確認 [完了] チェック ボックスをクリックします。</span><span class="sxs-lookup"><span data-stu-id="40f62-148">Now, check the Tables and Movie checkbox and click Finish.</span></span>

<span data-ttu-id="40f62-149">[![Entity Data Model ウィザード](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span><span class="sxs-lookup"><span data-stu-id="40f62-149">[![Entity Data Model Wizard](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span></span>

<span data-ttu-id="40f62-150">ここで、Entity Framework デザイナーで新しいムービー テーブルを参照してください。 し、コードからアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="40f62-150">Now we can see our new Movie table in the Entity Framework Designer and access it from code.</span></span>

<span data-ttu-id="40f62-151">[![ビデオ - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="40f62-151">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span></span>

<span data-ttu-id="40f62-152">デザイン画面では、「ビデオ」クラスを確認できます。</span><span class="sxs-lookup"><span data-stu-id="40f62-152">On the design surface you can see a "Movie" class.</span></span> <span data-ttu-id="40f62-153">このクラスは、データベース内の「ムービー」テーブルにマップし、内の各プロパティは、テーブルの列にマップします。</span><span class="sxs-lookup"><span data-stu-id="40f62-153">This class maps to the "Movie" table in our database, and each property within it maps to a column with the table.</span></span> <span data-ttu-id="40f62-154">「ビデオ」クラスの各インスタンスは"Movie"テーブル内の行に対応します。</span><span class="sxs-lookup"><span data-stu-id="40f62-154">Each instance of a "Movie" class will correspond to a row within the "Movie" table.</span></span>

<span data-ttu-id="40f62-155">既定の名前付けと Entity Framework によって使用される規則のマッピングを希望しない場合は、変更またはそれらをカスタマイズする Entity Framework デザイナーを使用できます。</span><span class="sxs-lookup"><span data-stu-id="40f62-155">If you don't like the default naming and mapping conventions used by the Entity Framework, you can use the Entity Framework designer to change or customize them.</span></span> <span data-ttu-id="40f62-156">このアプリケーションの既定値を使用して、なりますファイルとして保存-です。</span><span class="sxs-lookup"><span data-stu-id="40f62-156">For this application we'll use the defaults and just save the file as-is.</span></span>

<span data-ttu-id="40f62-157">ここで、実際のデータはいくつか見ていきましょう。</span><span class="sxs-lookup"><span data-stu-id="40f62-157">Now, let's work with some real data!</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="40f62-158">[前へ](getting-started-with-mvc-part3.md)
> [次へ](getting-started-with-mvc-part5.md)</span><span class="sxs-lookup"><span data-stu-id="40f62-158">[Previous](getting-started-with-mvc-part3.md)
[Next](getting-started-with-mvc-part5.md)</span></span>
