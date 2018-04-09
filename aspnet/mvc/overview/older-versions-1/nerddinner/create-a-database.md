---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: データベースを作成 |Microsoft ドキュメント
author: microsoft
description: 手順 2 では、dinner のすべてを保持しているデータベースを作成し、NerdDinner アプリケーション用のデータを RSVP 手順を示します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: ba28d671bf13ec54b83b876462e2c23f90310037
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="create-a-database"></a><span data-ttu-id="410d7-103">データベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="410d7-103">Create a Database</span></span>
====================
<span data-ttu-id="410d7-104">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="410d7-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="410d7-105">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="410d7-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="410d7-106">これは、無料のステップ 2 ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク-にする方法を小規模の構築が完了すると、ASP.NET MVC 1 を使用して web アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="410d7-106">This is step 2 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="410d7-107">手順 2 では、dinner のすべてを保持しているデータベースを作成し、NerdDinner アプリケーション用のデータを RSVP 手順を示します。</span><span class="sxs-lookup"><span data-stu-id="410d7-107">Step 2 shows the steps to create the database holding all of the dinner and RSVP data for our NerdDinner application.</span></span>
> 
> <span data-ttu-id="410d7-108">ASP.NET MVC 3 を使用している場合ことをお勧めする、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="410d7-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-2-creating-the-database"></a><span data-ttu-id="410d7-109">NerdDinner 手順 2: データベースの作成</span><span class="sxs-lookup"><span data-stu-id="410d7-109">NerdDinner Step 2: Creating the Database</span></span>

<span data-ttu-id="410d7-110">使用するデータベースすべて Dinner および RSVP のデータの NerdDinner アプリケーションを格納します。</span><span class="sxs-lookup"><span data-stu-id="410d7-110">We'll be using a database to store all of the Dinner and RSVP data for our NerdDinner application.</span></span>

<span data-ttu-id="410d7-111">無料の SQL Server Express edition を使用してデータベースを作成する次の手順を表示する (の V2 を使用して簡単にインストールできますが、 [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx))。</span><span class="sxs-lookup"><span data-stu-id="410d7-111">The steps below show creating the database using the free SQL Server Express edition (which you can easily install using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)).</span></span> <span data-ttu-id="410d7-112">SQL Server Express と完全 SQL サーバーの両方で動作するすべてのコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="410d7-112">All of the code we'll write works with both SQL Server Express and the full SQL Server.</span></span>

### <a name="creating-a-new-sql-server-express-database"></a><span data-ttu-id="410d7-113">新しい SQL Server Express データベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="410d7-113">Creating a new SQL Server Express database</span></span>

<span data-ttu-id="410d7-114">うまく、web プロジェクトを右クリックして開始しを選択、**追加 -&gt;新しい項目の**メニュー コマンド。</span><span class="sxs-lookup"><span data-stu-id="410d7-114">We'll begin by right-clicking on our web project, and then select the **Add-&gt;New Item** menu command:</span></span>

![](create-a-database/_static/image1.png)

<span data-ttu-id="410d7-115">Visual Studio の新しい項目の追加 ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="410d7-115">This will bring up Visual Studio's "Add New Item" dialog.</span></span> <span data-ttu-id="410d7-116">「データ」カテゴリでフィルタ リングして、「SQL Server データベース」の項目テンプレートを選択してお行います。</span><span class="sxs-lookup"><span data-stu-id="410d7-116">We'll filter by the "Data" category and select the "SQL Server Database" item template:</span></span>

![](create-a-database/_static/image2.png)

<span data-ttu-id="410d7-117">"NerdDinner.mdf"を作成し、[ok] をクリックすると SQL Server Express データベースという名前です。</span><span class="sxs-lookup"><span data-stu-id="410d7-117">We'll name the SQL Server Express database we want to create "NerdDinner.mdf" and hit ok.</span></span> <span data-ttu-id="410d7-118">Visual Studio は、ご質問、\App にこのファイルを追加するかどうかは\_データ ディレクトリ (ディレクトリは既に読み書きの両方でのセットアップし、セキュリティ Acl の書き込み)。</span><span class="sxs-lookup"><span data-stu-id="410d7-118">Visual Studio will then ask us if we want to add this file to our \App\_Data directory (which is a directory already setup with both read and write security ACLs):</span></span>

![](create-a-database/_static/image3.png)

<span data-ttu-id="410d7-119">ここをクリックして、"Yes"と、新しいデータベースが作成され、ソリューション エクスプ ローラーに追加。</span><span class="sxs-lookup"><span data-stu-id="410d7-119">We'll click "Yes" and our new database will be created and added to our Solution Explorer:</span></span>

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a><span data-ttu-id="410d7-120">データベース内のテーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="410d7-120">Creating Tables within our Database</span></span>

<span data-ttu-id="410d7-121">新しい空のデータベースがあるようになりました。</span><span class="sxs-lookup"><span data-stu-id="410d7-121">We now have a new empty database.</span></span> <span data-ttu-id="410d7-122">一部のテーブルを追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="410d7-122">Let's add some tables to it.</span></span>

<span data-ttu-id="410d7-123">これを行うには、"サーバー"タブ エクスプ ローラー、データベースとサーバーを管理することにより、Visual Studio 内に移動おをします。</span><span class="sxs-lookup"><span data-stu-id="410d7-123">To do this we'll navigate to the "Server Explorer" tab window within Visual Studio, which enables us to manage databases and servers.</span></span> <span data-ttu-id="410d7-124">\App に格納されている SQL Server Express データベース\_サーバー エクスプ ローラーで、アプリケーションのデータ フォルダーが自動的に表示。</span><span class="sxs-lookup"><span data-stu-id="410d7-124">SQL Server Express databases stored in the \App\_Data folder of our application will automatically show up within the Server Explorer.</span></span> <span data-ttu-id="410d7-125">必要に応じても一覧に追加の SQL Server データベース (ローカルおよびリモート) を追加するのに"[サーバー エクスプ ローラー] ウィンドウの上部にある「データベースへの接続」アイコンを使用できます。</span><span class="sxs-lookup"><span data-stu-id="410d7-125">We can optionally use the "Connect to Database" icon on the top of the "Server Explorer" window to add additional SQL Server databases (both local and remote) to the list as well:</span></span>

![](create-a-database/_static/image5.png)

<span data-ttu-id="410d7-126">NerdDinner データベース –、ディナー、もう一方に RSVP 承諾を追跡するためを格納する 1 つに 2 つのテーブルを追加します。</span><span class="sxs-lookup"><span data-stu-id="410d7-126">We will add two tables to our NerdDinner database – one to store our Dinners, and the other to track RSVP acceptances to them.</span></span> <span data-ttu-id="410d7-127">データベース内の"Tables"フォルダーを右クリックして、「新しいテーブルの追加」メニュー コマンド を選択して新しいテーブルを作成できます。</span><span class="sxs-lookup"><span data-stu-id="410d7-127">We can create new tables by right-clicking on the "Tables" folder within our database and choosing the "Add New Table" menu command:</span></span>

![](create-a-database/_static/image6.png)

<span data-ttu-id="410d7-128">テーブルのスキーマを構成することができるテーブル デザイナーが開きます。</span><span class="sxs-lookup"><span data-stu-id="410d7-128">This will open up a table designer that allows us to configure the schema of our table.</span></span> <span data-ttu-id="410d7-129">お「ディナー」テーブルでは、10 列のデータを追加します。</span><span class="sxs-lookup"><span data-stu-id="410d7-129">For our "Dinners" table we will add 10 columns of data:</span></span>

![](create-a-database/_static/image7.png)

<span data-ttu-id="410d7-130">テーブルの一意の主キー列に"DinnerID"します。</span><span class="sxs-lookup"><span data-stu-id="410d7-130">We want the "DinnerID" column to be a unique primary key for the table.</span></span> <span data-ttu-id="410d7-131">"DinnerID"列を右クリックし、「主キーの設定」メニュー項目を選択してこれを構成できます。</span><span class="sxs-lookup"><span data-stu-id="410d7-131">We can configure this by right-clicking on the "DinnerID" column and choosing the "Set Primary Key" menu item:</span></span>

![](create-a-database/_static/image8.png)

<span data-ttu-id="410d7-132">DinnerID 主キーを行うだけでなくたいことも、テーブルに新しい行のデータが追加されると、値が自動的に増分されます"id"列として構成する (つまり、最初に挿入された Dinner 行が 1 の DinnerID が、もう 1 つ挿入された行DinnerID 2 など)。</span><span class="sxs-lookup"><span data-stu-id="410d7-132">In addition to making DinnerID a primary key, we also want configure it as an "identity" column whose value is automatically incremented as new rows of data are added to the table (meaning the first inserted Dinner row will have a DinnerID of 1, the second inserted row will have a DinnerID of 2, etc).</span></span>

<span data-ttu-id="410d7-133">これには、"DinnerID"列を選択すると、プロパティを設定する、「(された Id)」を"Yes"の列に「列のプロパティ」エディターを使用してしたりできます。</span><span class="sxs-lookup"><span data-stu-id="410d7-133">We can do this by selecting the "DinnerID" column and then use the "Column Properties" editor to set the "(Is Identity)" property on the column to "Yes".</span></span> <span data-ttu-id="410d7-134">標準の id の既定値を使用して (1 から始まり、新しい Dinner 各行に 1 をインクリメント)。</span><span class="sxs-lookup"><span data-stu-id="410d7-134">We will use the standard identity defaults (start at 1 and increment 1 on each new Dinner row):</span></span>

![](create-a-database/_static/image9.png)

<span data-ttu-id="410d7-135">CTRL-S を入力するかを使用しておし、テーブルを保存します、**ファイル -&gt;保存**メニュー コマンド。</span><span class="sxs-lookup"><span data-stu-id="410d7-135">We'll then save our table by typing Ctrl-S or by using the **File-&gt;Save** menu command.</span></span> <span data-ttu-id="410d7-136">テーブルの名前を求められます。</span><span class="sxs-lookup"><span data-stu-id="410d7-136">This will prompt us to name the table.</span></span> <span data-ttu-id="410d7-137">という名前に「ディナー」。</span><span class="sxs-lookup"><span data-stu-id="410d7-137">We'll name it "Dinners":</span></span>

![](create-a-database/_static/image10.png)

<span data-ttu-id="410d7-138">この新しいディナー テーブルは、サーバー エクスプ ローラーで、データベース内で表示されます。</span><span class="sxs-lookup"><span data-stu-id="410d7-138">Our new Dinners table will then show up within our database in the server explorer.</span></span>

<span data-ttu-id="410d7-139">この手順を繰り返し、"RSVP"テーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="410d7-139">We'll then repeat the above steps and create a "RSVP" table.</span></span> <span data-ttu-id="410d7-140">このテーブルでは、3 つの列があります。</span><span class="sxs-lookup"><span data-stu-id="410d7-140">This table with have 3 columns.</span></span> <span data-ttu-id="410d7-141">RsvpID 列、主キーとして設定して、id 列にすることもおされます。</span><span class="sxs-lookup"><span data-stu-id="410d7-141">We will setup the RsvpID column as the primary key, and also make it an identity column:</span></span>

![](create-a-database/_static/image11.png)

<span data-ttu-id="410d7-142">保存し、"RSVP"の名前を付けることがあります。</span><span class="sxs-lookup"><span data-stu-id="410d7-142">We'll save it and give it the name "RSVP".</span></span>

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a><span data-ttu-id="410d7-143">テーブル間に外部キー リレーションシップを設定します。</span><span class="sxs-lookup"><span data-stu-id="410d7-143">Setting up a Foreign Key Relationship between Tables</span></span>

<span data-ttu-id="410d7-144">データベース内で 2 つのテーブルであるおようになりました。</span><span class="sxs-lookup"><span data-stu-id="410d7-144">We now have two tables within our database.</span></span> <span data-ttu-id="410d7-145">最後のスキーマ デザイン手順がする – これら 2 つのテーブル間の「一対多」リレーションシップを設定するように Dinner 行ごとに適用する 0 個以上の RSVP 行に関連付けられたことができます。</span><span class="sxs-lookup"><span data-stu-id="410d7-145">Our last schema design step will be to setup a "one-to-many" relationship between these two tables – so that we can associate each Dinner row with zero or more RSVP rows that apply to it.</span></span> <span data-ttu-id="410d7-146">RSVP テーブルの"DinnerID"列「ディナー」テーブルに"DinnerID"列に外部キー関係を構成することによってこの作業が行うされます。</span><span class="sxs-lookup"><span data-stu-id="410d7-146">We will do this by configuring the RSVP table's "DinnerID" column to have a foreign-key relationship to the "DinnerID" column in the "Dinners" table.</span></span>

<span data-ttu-id="410d7-147">サーバー エクスプ ローラーでダブルクリックして、テーブル デザイナー内で RSVP テーブルを開くことがこれを行うには。</span><span class="sxs-lookup"><span data-stu-id="410d7-147">To do this we'll open up the RSVP table within the table designer by double-clicking it in the server explorer.</span></span> <span data-ttu-id="410d7-148">"DinnerID"の列がしを選択し、右クリック「Relationshps…」を選択</span><span class="sxs-lookup"><span data-stu-id="410d7-148">We'll then select the "DinnerID" column within it, right-click, and choose the "Relationshps…"</span></span> <span data-ttu-id="410d7-149">コンテキスト メニューのコマンド:</span><span class="sxs-lookup"><span data-stu-id="410d7-149">context menu command:</span></span>

![](create-a-database/_static/image12.png)

<span data-ttu-id="410d7-150">テーブル間のリレーションシップのセットアップに使用できるダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="410d7-150">This will bring up a dialog that we can use to setup relationships between tables:</span></span>

![](create-a-database/_static/image13.png)

<span data-ttu-id="410d7-151">ダイアログ ボックスに新しいリレーションシップを追加する「追加」ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="410d7-151">We'll click the "Add" button to add a new relationship to the dialog.</span></span> <span data-ttu-id="410d7-152">リレーションシップを追加すると、ダイアログ ボックスの右側に、プロパティ グリッド内で「テーブルと列の仕様」ツリー ビュー ノードを展開し、その右側にある [...] ボタンをクリックおします。</span><span class="sxs-lookup"><span data-stu-id="410d7-152">Once a relationship has been added, we'll expand the "Tables and Column Specification" tree-view node within the property grid to the right of the dialog, and then click the "…" button to the right of it:</span></span>

![](create-a-database/_static/image14.png)

<span data-ttu-id="410d7-153">「...」ボタンをクリックすると、指定のテーブルと列は、リレーションシップに関係するだけでなく、リレーションシップの名前を付けることにより、別のダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="410d7-153">Clicking the "…" button will bring up another dialog that allows us to specify which tables and columns are involved in the relationship, as well as allow us to name the relationship.</span></span>

<span data-ttu-id="410d7-154">「ディナー」である主キー テーブルを変更し、主キーとしてディナー テーブル内の"DinnerID"列を選択おはします。</span><span class="sxs-lookup"><span data-stu-id="410d7-154">We will change the Primary Key Table to be "Dinners", and select the "DinnerID" column within the Dinners table as the primary key.</span></span> <span data-ttu-id="410d7-155">RSVP テーブルは、外部キー テーブル、および、RSVP になります。DinnerID 列は、外部キーとして関連付けられているになります。</span><span class="sxs-lookup"><span data-stu-id="410d7-155">Our RSVP table will be the foreign-key table, and the RSVP.DinnerID column will be associated as the foreign-key:</span></span>

![](create-a-database/_static/image15.png)

<span data-ttu-id="410d7-156">RSVP テーブルの各行に関連付けられる Dinner テーブルの行にします。</span><span class="sxs-lookup"><span data-stu-id="410d7-156">Now each row in the RSVP table will be associated with a row in the Dinner table.</span></span> <span data-ttu-id="410d7-157">SQL Server はご利用の米国 – 参照整合性を維持し、有効な夕食行を指定していない場合は、新しい RSVP 行を追加することを防ぐ。</span><span class="sxs-lookup"><span data-stu-id="410d7-157">SQL Server will maintain referential integrity for us – and prevent us from adding a new RSVP row if it does not point to a valid Dinner row.</span></span> <span data-ttu-id="410d7-158">できなくなります us からまだ行が参照することを RSVP がある場合は、Dinner 行を削除します。</span><span class="sxs-lookup"><span data-stu-id="410d7-158">It will also prevent us from deleting a Dinner row if there are still RSVP rows referring to it.</span></span>

### <a name="adding-data-to-our-tables"></a><span data-ttu-id="410d7-159">データは、テーブルを追加します。</span><span class="sxs-lookup"><span data-stu-id="410d7-159">Adding Data to our Tables</span></span>

<span data-ttu-id="410d7-160">このディナー テーブルにサンプル データを追加することで完了してみましょう。</span><span class="sxs-lookup"><span data-stu-id="410d7-160">Let's finish by adding some sample data to our Dinners table.</span></span> <span data-ttu-id="410d7-161">サーバー エクスプ ローラー内でそれを右クリックし、「テーブルのデータを表示する」コマンドを選択するデータをテーブルに追加できます。</span><span class="sxs-lookup"><span data-stu-id="410d7-161">We can add data to a table by right-clicking on it within the Server Explorer and choosing the "Show Table Data" command:</span></span>

![](create-a-database/_static/image16.png)

<span data-ttu-id="410d7-162">数行のように、アプリケーションの実装を開始して後で使用できる Dinner データ追加されます。</span><span class="sxs-lookup"><span data-stu-id="410d7-162">We'll add a few rows of Dinner data that we can use later as we start implementing the application:</span></span>

![](create-a-database/_static/image17.png)

### <a name="next-step"></a><span data-ttu-id="410d7-163">次の手順</span><span class="sxs-lookup"><span data-stu-id="410d7-163">Next Step</span></span>

<span data-ttu-id="410d7-164">データベースの作成が完了したらです。</span><span class="sxs-lookup"><span data-stu-id="410d7-164">We've finished creating our database.</span></span> <span data-ttu-id="410d7-165">クエリを実行し、更新に使用できるモデル クラスを今すぐ作成してみましょう。</span><span class="sxs-lookup"><span data-stu-id="410d7-165">Let's now create model classes that we can use to query and update it.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="410d7-166">[前へ](create-a-new-aspnet-mvc-project.md)
> [次へ](build-a-model-with-business-rule-validations.md)</span><span class="sxs-lookup"><span data-stu-id="410d7-166">[Previous](create-a-new-aspnet-mvc-project.md)
[Next](build-a-model-with-business-rule-validations.md)</span></span>
