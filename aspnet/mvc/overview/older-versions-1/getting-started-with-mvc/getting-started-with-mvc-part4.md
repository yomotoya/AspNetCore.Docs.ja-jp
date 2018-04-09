---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: データベースを作成する |Microsoft ドキュメント
author: shanselman
description: これは、ASP.NET MVC の基本について説明する初心者向けチュートリアルです。 データベースから読み取り/書き込みする単純な web アプリケーションを作成します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: ff2a41803cd31ce50bbf79e630d827b6de441ba3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-database"></a><span data-ttu-id="213df-104">データベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="213df-104">Creating a Database</span></span>
====================
<span data-ttu-id="213df-105">によって[Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="213df-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="213df-106">これは、ASP.NET MVC の基本について説明する初心者向けチュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="213df-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="213df-107">データベースから読み取り/書き込みを単純な web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="213df-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="213df-108">参照してください、 [ASP.NET MVC ラーニング センター](../../../index.md)チュートリアルおよびサンプルは、その他の ASP.NET MVC を検索します。</span><span class="sxs-lookup"><span data-stu-id="213df-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="213df-109">このセクションで行う、ムービー データ格納および取得に使用する新しい SQL Express データベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="213df-109">In this section we are going to create a new SQL Express database that we'll use to store and retrieve our movie data.</span></span> <span data-ttu-id="213df-110">Visual Web Developer IDE 内でビューを選択 |サーバー エクスプ ローラー。</span><span class="sxs-lookup"><span data-stu-id="213df-110">From within the Visual Web Developer IDE, select View | Server Explorer.</span></span> <span data-ttu-id="213df-111">データ接続を右クリックし、[接続の追加] をクリックしてください.</span><span class="sxs-lookup"><span data-stu-id="213df-111">Right click on Data Connections and click Add Connection...</span></span>

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

<span data-ttu-id="213df-113">データ ソースの選択 ダイアログ ボックスでは、Microsoft SQL Server を選択し、続行 を選択します。</span><span class="sxs-lookup"><span data-stu-id="213df-113">In the Choose Data Source dialog, select Microsoft SQL Server and select Continue.</span></span>

![](getting-started-with-mvc-part4/_static/image2.png)

<span data-ttu-id="213df-114">接続の追加 ダイアログ ボックスで、次を入力してください。"。 \SQLEXPRESS"、サーバー名に対応し、新しいデータベースの名前として"映画"を入力します。</span><span class="sxs-lookup"><span data-stu-id="213df-114">In the Add Connection dialog, enter ".\SQLEXPRESS" for your Server Name, and enter "Movies" as the name for your new database.</span></span>

<span data-ttu-id="213df-115">[![追加の接続 ダイアログ ボックス](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="213df-115">[![Add Connection dialog](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span></span>

<span data-ttu-id="213df-116">[Ok] をクリックし、たずねられますをそのデータベースを作成するかどうか。</span><span class="sxs-lookup"><span data-stu-id="213df-116">Click OK and you'll be asked if you want to create that database.</span></span> <span data-ttu-id="213df-117">[はい] を選択します。</span><span class="sxs-lookup"><span data-stu-id="213df-117">Select yes.</span></span>

<span data-ttu-id="213df-118">[![ムービーを作成しますか。](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="213df-118">[![Create Movies?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span></span>

<span data-ttu-id="213df-119">作成できたところで、空のデータベース サーバー エクスプ ローラーにします。</span><span class="sxs-lookup"><span data-stu-id="213df-119">Now you've got an empty database in Server Explorer.</span></span>

![新しいテーブルを追加します。](getting-started-with-mvc-part4/_static/image7.png)

<span data-ttu-id="213df-121">テーブルを右クリックし、[テーブルの追加] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="213df-121">Right click on Tables and click Add Table.</span></span> <span data-ttu-id="213df-122">テーブル デザイナーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="213df-122">The Table Designer will appear.</span></span> <span data-ttu-id="213df-123">Id、タイトル、ReleaseDate、ジャンル、および価格の列を追加します。</span><span class="sxs-lookup"><span data-stu-id="213df-123">Add columns for Id, Title, ReleaseDate, Genre, and Price.</span></span> <span data-ttu-id="213df-124">ID 列を右クリックし、主キーを設定する をクリックします。</span><span class="sxs-lookup"><span data-stu-id="213df-124">Right click on the ID column and click set Primary Key.</span></span> <span data-ttu-id="213df-125">ようになります、どのようなデザイン領域を次に示します。</span><span class="sxs-lookup"><span data-stu-id="213df-125">Here's what my design areas looks like.</span></span>

<span data-ttu-id="213df-126">[![データベース テーブル エディター](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="213df-126">[![Database Table Editor](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span></span>

<span data-ttu-id="213df-127">また、Id 列を選択し、以下の列のプロパティには、"Yes"に「Identity の指定」を変更</span><span class="sxs-lookup"><span data-stu-id="213df-127">Also, select the Id column and under Column Properties below change "Identity Specification" to "Yes."</span></span>

<span data-ttu-id="213df-128">[![IsIdentity - 列のプロパティ](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="213df-128">[![IsIdentity - Column Properties](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span></span>

<span data-ttu-id="213df-129">これを取得したら、ツールバーの [保存] アイコンをクリックするかファイルを選択して |メニューから、保存し、テーブルの名前"**ムービー**"(単数形)。</span><span class="sxs-lookup"><span data-stu-id="213df-129">When you've got it done, click the Save icon in the toolbar or select File | Save from the menu, and name your table "**Movie**" (singular).</span></span> <span data-ttu-id="213df-130">データベースとテーブルを特定しました!</span><span class="sxs-lookup"><span data-stu-id="213df-130">We've got a database and a table!</span></span>

<span data-ttu-id="213df-131">[![名前を選択します。](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="213df-131">[![Choose Name](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span></span>

<span data-ttu-id="213df-132">サーバー エクスプ ローラーに戻る、ムービー テーブルを右クリックし、選択「テーブル データの表示」です。</span><span class="sxs-lookup"><span data-stu-id="213df-132">Go back to Server Explorer and right click the Movie table, then select "Show Table Data."</span></span> <span data-ttu-id="213df-133">データベースにいくつかのデータがあるため、いくつかの映画を入力します。</span><span class="sxs-lookup"><span data-stu-id="213df-133">Enter a few movies so our database has some data.</span></span>

<span data-ttu-id="213df-134">[![データベース テーブルの編集](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span><span class="sxs-lookup"><span data-stu-id="213df-134">[![Database Table Editing](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span></span>

## <a name="creating-a-model"></a><span data-ttu-id="213df-135">モデルの作成</span><span class="sxs-lookup"><span data-stu-id="213df-135">Creating a Model</span></span>

<span data-ttu-id="213df-136">ここで、IDE の右辺でソリューション エクスプ ローラーに戻りますし、モデル フォルダーを右クリックし、追加 |新しい項目。</span><span class="sxs-lookup"><span data-stu-id="213df-136">Now, switch back to the Solution Explorer on the right hand side of the IDE and right-click on the Models folder and select Add | New Item.</span></span>

<span data-ttu-id="213df-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="213df-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span></span>

<span data-ttu-id="213df-138">新しいデータベースから Entity Model を作成しようとしています。</span><span class="sxs-lookup"><span data-stu-id="213df-138">We're going to create an Entity Model from our new database.</span></span> <span data-ttu-id="213df-139">これでは、一連のクラスを容易にクエリを実行し、データベース内のデータを操作するうえで、プロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="213df-139">This will add a set of classes to our project that makes it easy for us to query and manipulate the data within our database.</span></span> <span data-ttu-id="213df-140">このダイアログの左側にあるデータ ノードを選択し、ADO.NET Entity Data Model の項目テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="213df-140">Select the Data node on the left-hand side of the dialog, and then select the ADO.NET Entity Data Model item template.</span></span> <span data-ttu-id="213df-141">Movies.edmx 名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="213df-141">Name it Movies.edmx.</span></span>

<span data-ttu-id="213df-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span><span class="sxs-lookup"><span data-stu-id="213df-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span></span>

<span data-ttu-id="213df-143">[追加] ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="213df-143">Click the "Add" button.</span></span> <span data-ttu-id="213df-144">これは、「Entity Data Model ウィザード」を開始されます。</span><span class="sxs-lookup"><span data-stu-id="213df-144">This will then launch the "Entity Data Model Wizard".</span></span>

<span data-ttu-id="213df-145">ポップアップ表示される新しい ダイアログでは、データベースから生成を選択します。</span><span class="sxs-lookup"><span data-stu-id="213df-145">In the new dialog that pops up, select Generate from Database.</span></span> <span data-ttu-id="213df-146">データベース、だけしましたのでおのみ必要があります、新しいデータベースとそのテーブルは、Entity Framework を伝えます。</span><span class="sxs-lookup"><span data-stu-id="213df-146">Since we've just made a database, we'll only need to tell the Entity Framework about our new database and its table.</span></span> <span data-ttu-id="213df-147">Web アプリケーションの構成では、データベース接続の横をクリックします。</span><span class="sxs-lookup"><span data-stu-id="213df-147">Click Next to save our database connection in our web application's configuration.</span></span> <span data-ttu-id="213df-148">テーブルとムービーを確認して、[完了] チェック ボックスをクリックします。</span><span class="sxs-lookup"><span data-stu-id="213df-148">Now, check the Tables and Movie checkbox and click Finish.</span></span>

<span data-ttu-id="213df-149">[![Entity Data Model ウィザード](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span><span class="sxs-lookup"><span data-stu-id="213df-149">[![Entity Data Model Wizard](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span></span>

<span data-ttu-id="213df-150">これで、Entity Framework Designer で、新しいムービー表を参照して、コードからアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="213df-150">Now we can see our new Movie table in the Entity Framework Designer and access it from code.</span></span>

<span data-ttu-id="213df-151">[![映画 - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="213df-151">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span></span>

<span data-ttu-id="213df-152">デザイン画面では、「ムービー」クラスを表示できます。</span><span class="sxs-lookup"><span data-stu-id="213df-152">On the design surface you can see a "Movie" class.</span></span> <span data-ttu-id="213df-153">このクラスがデータベースに「ムービー」テーブルにマップされ、その中の各プロパティがテーブルと列にマップします。</span><span class="sxs-lookup"><span data-stu-id="213df-153">This class maps to the "Movie" table in our database, and each property within it maps to a column with the table.</span></span> <span data-ttu-id="213df-154">「ムービー」クラスの各インスタンスは、「ムービー」テーブル内の行に対応します。</span><span class="sxs-lookup"><span data-stu-id="213df-154">Each instance of a "Movie" class will correspond to a row within the "Movie" table.</span></span>

<span data-ttu-id="213df-155">既定の名前付けと、Entity Framework で使用される規則のマッピングに満足できない場合は、変更またはカスタマイズするに Entity Framework デザイナーを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="213df-155">If you don't like the default naming and mapping conventions used by the Entity Framework, you can use the Entity Framework designer to change or customize them.</span></span> <span data-ttu-id="213df-156">このアプリケーションがうまく、既定値を使用し、ファイルとして保存-はします。</span><span class="sxs-lookup"><span data-stu-id="213df-156">For this application we'll use the defaults and just save the file as-is.</span></span>

<span data-ttu-id="213df-157">ここで、いくつかの実際のデータを操作できます。</span><span class="sxs-lookup"><span data-stu-id="213df-157">Now, let's work with some real data!</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="213df-158">[前へ](getting-started-with-mvc-part3.md)
> [次へ](getting-started-with-mvc-part5.md)</span><span class="sxs-lookup"><span data-stu-id="213df-158">[Previous](getting-started-with-mvc-part3.md)
[Next](getting-started-with-mvc-part5.md)</span></span>
