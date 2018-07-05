---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: データベースを作成する |Microsoft Docs
author: microsoft
description: 手順 2 では、dinner のすべてを保持しているデータベースを作成し、NerdDinner アプリケーションのデータを RSVP の手順を示します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: 01c6d3c63780e492b97aa54a92f3982d4c18f9e5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371750"
---
<a name="create-a-database"></a><span data-ttu-id="531a8-103">データベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="531a8-103">Create a Database</span></span>
====================
<span data-ttu-id="531a8-104">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="531a8-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="531a8-105">PDF のダウンロード</span><span class="sxs-lookup"><span data-stu-id="531a8-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="531a8-106">これは、無料の手順 2 ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク スルーの小さなをビルドしても、ASP.NET MVC 1 を使用して web アプリケーションを実行する方法。</span><span class="sxs-lookup"><span data-stu-id="531a8-106">This is step 2 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="531a8-107">手順 2 では、dinner のすべてを保持しているデータベースを作成し、NerdDinner アプリケーションのデータを RSVP の手順を示します。</span><span class="sxs-lookup"><span data-stu-id="531a8-107">Step 2 shows the steps to create the database holding all of the dinner and RSVP data for our NerdDinner application.</span></span>
> 
> <span data-ttu-id="531a8-108">次のことをお勧め ASP.NET MVC 3 を使用している場合、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアル。</span><span class="sxs-lookup"><span data-stu-id="531a8-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-2-creating-the-database"></a><span data-ttu-id="531a8-109">NerdDinner 手順 2: データベースの作成</span><span class="sxs-lookup"><span data-stu-id="531a8-109">NerdDinner Step 2: Creating the Database</span></span>

<span data-ttu-id="531a8-110">使用するデータベースすべて Dinner および RSVP のデータのアプリケーションで NerdDinner を格納します。</span><span class="sxs-lookup"><span data-stu-id="531a8-110">We'll be using a database to store all of the Dinner and RSVP data for our NerdDinner application.</span></span>

<span data-ttu-id="531a8-111">以下の手順は、無料の SQL Server Express edition を使用してデータベースの作成を示します (の V2 を使用して簡単にインストールできますが、 [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx))。</span><span class="sxs-lookup"><span data-stu-id="531a8-111">The steps below show creating the database using the free SQL Server Express edition (which you can easily install using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)).</span></span> <span data-ttu-id="531a8-112">SQL Server Express と完全な SQL Server の両方で動作するすべてのコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="531a8-112">All of the code we'll write works with both SQL Server Express and the full SQL Server.</span></span>

### <a name="creating-a-new-sql-server-express-database"></a><span data-ttu-id="531a8-113">新しい SQL Server Express データベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="531a8-113">Creating a new SQL Server Express database</span></span>

<span data-ttu-id="531a8-114">右クリックし、web プロジェクトを開始し、いますが、**追加 -&gt;新しい項目の**メニュー コマンド。</span><span class="sxs-lookup"><span data-stu-id="531a8-114">We'll begin by right-clicking on our web project, and then select the **Add-&gt;New Item** menu command:</span></span>

![](create-a-database/_static/image1.png)

<span data-ttu-id="531a8-115">Visual Studio の「新しい項目の追加」ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="531a8-115">This will bring up Visual Studio's "Add New Item" dialog.</span></span> <span data-ttu-id="531a8-116">「データ」カテゴリでフィルター処理、「SQL Server データベース」の項目テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="531a8-116">We'll filter by the "Data" category and select the "SQL Server Database" item template:</span></span>

![](create-a-database/_static/image2.png)

<span data-ttu-id="531a8-117">という名前を SQL Server Express のデータベースを"NerdDinner.mdf"を作成し、[ok] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="531a8-117">We'll name the SQL Server Express database we want to create "NerdDinner.mdf" and hit ok.</span></span> <span data-ttu-id="531a8-118">Visual Studio は、ご質問、\App にこのファイルを追加するかどうかは\_データ ディレクトリ (ディレクトリは既に読み書きの両方でのセットアップし、セキュリティ Acl を記述)。</span><span class="sxs-lookup"><span data-stu-id="531a8-118">Visual Studio will then ask us if we want to add this file to our \App\_Data directory (which is a directory already setup with both read and write security ACLs):</span></span>

![](create-a-database/_static/image3.png)

<span data-ttu-id="531a8-119">[はい] をクリックし、新しいデータベースが作成され、ソリューション エクスプ ローラーに追加します。</span><span class="sxs-lookup"><span data-stu-id="531a8-119">We'll click "Yes" and our new database will be created and added to our Solution Explorer:</span></span>

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a><span data-ttu-id="531a8-120">データベース内のテーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="531a8-120">Creating Tables within our Database</span></span>

<span data-ttu-id="531a8-121">新しい空のデータベースがあるようになりました。</span><span class="sxs-lookup"><span data-stu-id="531a8-121">We now have a new empty database.</span></span> <span data-ttu-id="531a8-122">いくつかのテーブルを追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="531a8-122">Let's add some tables to it.</span></span>

<span data-ttu-id="531a8-123">これを行う、データベースとサーバーの管理により、Visual Studio 内の [サーバー エクスプ ローラー] タブ ウィンドウへ移動します。</span><span class="sxs-lookup"><span data-stu-id="531a8-123">To do this we'll navigate to the "Server Explorer" tab window within Visual Studio, which enables us to manage databases and servers.</span></span> <span data-ttu-id="531a8-124">SQL Server Express のデータベース、\App に格納されている\_サーバー エクスプ ローラーで、アプリケーションのデータ フォルダーが自動的に表示。</span><span class="sxs-lookup"><span data-stu-id="531a8-124">SQL Server Express databases stored in the \App\_Data folder of our application will automatically show up within the Server Explorer.</span></span> <span data-ttu-id="531a8-125">必要に応じてもリストに追加の SQL Server データベース (ローカルおよびリモート) を追加するのに"サーバー エクスプ ローラー ウィンドウの上部にある「データベースに接続」アイコンを使用できます。</span><span class="sxs-lookup"><span data-stu-id="531a8-125">We can optionally use the "Connect to Database" icon on the top of the "Server Explorer" window to add additional SQL Server databases (both local and remote) to the list as well:</span></span>

![](create-a-database/_static/image5.png)

<span data-ttu-id="531a8-126">NerdDinner データベース –、Dinners、およびその他、RSVP の承諾を追跡を格納する 1 つを 2 つのテーブルに追加します。</span><span class="sxs-lookup"><span data-stu-id="531a8-126">We will add two tables to our NerdDinner database – one to store our Dinners, and the other to track RSVP acceptances to them.</span></span> <span data-ttu-id="531a8-127">データベース内の「テーブル」フォルダーを右クリックして、"新しいテーブルの追加 メニューのコマンドを選択して新しいテーブルを作成できます。</span><span class="sxs-lookup"><span data-stu-id="531a8-127">We can create new tables by right-clicking on the "Tables" folder within our database and choosing the "Add New Table" menu command:</span></span>

![](create-a-database/_static/image6.png)

<span data-ttu-id="531a8-128">テーブルのスキーマを構成することができるようにする、テーブル デザイナーが開きます。</span><span class="sxs-lookup"><span data-stu-id="531a8-128">This will open up a table designer that allows us to configure the schema of our table.</span></span> <span data-ttu-id="531a8-129">"Dinners"テーブルは 10 個の列のデータの追加します。</span><span class="sxs-lookup"><span data-stu-id="531a8-129">For our "Dinners" table we will add 10 columns of data:</span></span>

![](create-a-database/_static/image7.png)

<span data-ttu-id="531a8-130">テーブルの一意の主キー列に"DinnerID"します。</span><span class="sxs-lookup"><span data-stu-id="531a8-130">We want the "DinnerID" column to be a unique primary key for the table.</span></span> <span data-ttu-id="531a8-131">この"DinnerID"列を右クリックし、「主キーの設定」メニュー項目を選択して構成できます。</span><span class="sxs-lookup"><span data-stu-id="531a8-131">We can configure this by right-clicking on the "DinnerID" column and choosing the "Set Primary Key" menu item:</span></span>

![](create-a-database/_static/image8.png)

<span data-ttu-id="531a8-132">DinnerID 主キーを作成に加えたいことも、テーブルに新しいデータ行が追加されると、値が自動的にインクリメントされる"id"列として構成する場合 (つまり、最初に挿入された Dinner 行が 1 の DinnerID が、2 番目の挿入された行DinnerID が 2 など)。</span><span class="sxs-lookup"><span data-stu-id="531a8-132">In addition to making DinnerID a primary key, we also want configure it as an "identity" column whose value is automatically incremented as new rows of data are added to the table (meaning the first inserted Dinner row will have a DinnerID of 1, the second inserted row will have a DinnerID of 2, etc).</span></span>

<span data-ttu-id="531a8-133">"DinnerID"列を選択してこれを"Yes"には、列に「(Id である)」プロパティを設定する [列のプロパティ] エディターを使用できます。</span><span class="sxs-lookup"><span data-stu-id="531a8-133">We can do this by selecting the "DinnerID" column and then use the "Column Properties" editor to set the "(Is Identity)" property on the column to "Yes".</span></span> <span data-ttu-id="531a8-134">標準の id の既定値を使用して、(1 から始まりますおよび新しい Dinner 行ごとに 1 インクリメント)。</span><span class="sxs-lookup"><span data-stu-id="531a8-134">We will use the standard identity defaults (start at 1 and increment 1 on each new Dinner row):</span></span>

![](create-a-database/_static/image9.png)

<span data-ttu-id="531a8-135">CTRL-S を入力するかを使用していますし、テーブルを保存します、**ファイル -&gt;保存**メニュー コマンド。</span><span class="sxs-lookup"><span data-stu-id="531a8-135">We'll then save our table by typing Ctrl-S or by using the **File-&gt;Save** menu command.</span></span> <span data-ttu-id="531a8-136">テーブルの名前を入力を求められます。</span><span class="sxs-lookup"><span data-stu-id="531a8-136">This will prompt us to name the table.</span></span> <span data-ttu-id="531a8-137">という名前に"Dinners"。</span><span class="sxs-lookup"><span data-stu-id="531a8-137">We'll name it "Dinners":</span></span>

![](create-a-database/_static/image10.png)

<span data-ttu-id="531a8-138">新しい Dinners テーブルも、サーバー エクスプ ローラーで、データベース内で表示されます。</span><span class="sxs-lookup"><span data-stu-id="531a8-138">Our new Dinners table will then show up within our database in the server explorer.</span></span>

<span data-ttu-id="531a8-139">上記の手順を繰り返して、され"RSVP"テーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="531a8-139">We'll then repeat the above steps and create a "RSVP" table.</span></span> <span data-ttu-id="531a8-140">このテーブルでは、3 つの列があります。</span><span class="sxs-lookup"><span data-stu-id="531a8-140">This table with have 3 columns.</span></span> <span data-ttu-id="531a8-141">RsvpID 列、主キーとしてセットアップされ、id 列にすることもされます。</span><span class="sxs-lookup"><span data-stu-id="531a8-141">We will setup the RsvpID column as the primary key, and also make it an identity column:</span></span>

![](create-a-database/_static/image11.png)

<span data-ttu-id="531a8-142">保存がされ"RSVP"という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="531a8-142">We'll save it and give it the name "RSVP".</span></span>

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a><span data-ttu-id="531a8-143">テーブル間に外部キー リレーションシップの設定</span><span class="sxs-lookup"><span data-stu-id="531a8-143">Setting up a Foreign Key Relationship between Tables</span></span>

<span data-ttu-id="531a8-144">これで 2 つのテーブルがある、データベース内で。</span><span class="sxs-lookup"><span data-stu-id="531a8-144">We now have two tables within our database.</span></span> <span data-ttu-id="531a8-145">Dinner 行ごとに適用する 0 個以上の RSVP の行に関連付けられたできますように – 2 つのテーブル間の「1 対多」リレーションシップを設定する、最後のスキーマのデザイン手順がなります。</span><span class="sxs-lookup"><span data-stu-id="531a8-145">Our last schema design step will be to setup a "one-to-many" relationship between these two tables – so that we can associate each Dinner row with zero or more RSVP rows that apply to it.</span></span> <span data-ttu-id="531a8-146">"Dinners"テーブルに"DinnerID"列に外部キー リレーションシップが RSVP テーブルの"DinnerID"列を構成することによって行います。</span><span class="sxs-lookup"><span data-stu-id="531a8-146">We will do this by configuring the RSVP table's "DinnerID" column to have a foreign-key relationship to the "DinnerID" column in the "Dinners" table.</span></span>

<span data-ttu-id="531a8-147">これを行うサーバー エクスプ ローラーでダブルクリックして、テーブル デザイナー内で予約テーブルを開くします。</span><span class="sxs-lookup"><span data-stu-id="531a8-147">To do this we'll open up the RSVP table within the table designer by double-clicking it in the server explorer.</span></span> <span data-ttu-id="531a8-148">"DinnerID"の列選択し、右クリックし、「Relationshps...」のコンテキスト メニュー コマンドを選択。</span><span class="sxs-lookup"><span data-stu-id="531a8-148">We'll then select the "DinnerID" column within it, right-click, and choose the "Relationshps…" context menu command:</span></span>

![](create-a-database/_static/image12.png)

<span data-ttu-id="531a8-149">テーブル間のリレーションシップをセットアップするために使用できるダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="531a8-149">This will bring up a dialog that we can use to setup relationships between tables:</span></span>

![](create-a-database/_static/image13.png)

<span data-ttu-id="531a8-150">ダイアログ ボックスに新しいリレーションシップを追加する、[追加] ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="531a8-150">We'll click the "Add" button to add a new relationship to the dialog.</span></span> <span data-ttu-id="531a8-151">リレーションシップを追加するは、ダイアログ ボックスの右側にプロパティ グリッド内で「テーブルと列の仕様」ツリー ビュー ノードを展開し、右側に「...」ボタンをクリックしましたします。</span><span class="sxs-lookup"><span data-stu-id="531a8-151">Once a relationship has been added, we'll expand the "Tables and Column Specification" tree-view node within the property grid to the right of the dialog, and then click the "…" button to the right of it:</span></span>

![](create-a-database/_static/image14.png)

<span data-ttu-id="531a8-152">「...」ボタンをクリックすると、どのテーブルと列は、リレーションシップに関係するだけでなく、リレーションシップの名前を指定できるようにする別のダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="531a8-152">Clicking the "…" button will bring up another dialog that allows us to specify which tables and columns are involved in the relationship, as well as allow us to name the relationship.</span></span>

<span data-ttu-id="531a8-153">主キー テーブルの"Dinners"を変更いたします。 その主キーとして Dinners テーブル内の"DinnerID"列を選択します。</span><span class="sxs-lookup"><span data-stu-id="531a8-153">We will change the Primary Key Table to be "Dinners", and select the "DinnerID" column within the Dinners table as the primary key.</span></span> <span data-ttu-id="531a8-154">RSVP テーブルは、外部キー テーブルと、予約になります。外部キーとして関連付けられている DinnerID 列になります。</span><span class="sxs-lookup"><span data-stu-id="531a8-154">Our RSVP table will be the foreign-key table, and the RSVP.DinnerID column will be associated as the foreign-key:</span></span>

![](create-a-database/_static/image15.png)

<span data-ttu-id="531a8-155">ようになりました RSVP テーブルの各行の Dinner テーブルの行に関連付けられます。</span><span class="sxs-lookup"><span data-stu-id="531a8-155">Now each row in the RSVP table will be associated with a row in the Dinner table.</span></span> <span data-ttu-id="531a8-156">SQL Server は私たちの参照整合性を維持し、これが有効な Dinner 行を指していない場合に新しい RSVP 行を追加することはできません。</span><span class="sxs-lookup"><span data-stu-id="531a8-156">SQL Server will maintain referential integrity for us – and prevent us from adding a new RSVP row if it does not point to a valid Dinner row.</span></span> <span data-ttu-id="531a8-157">これはもすることはできませんからまだ行が参照することを RSVP がある場合は、Dinner 行を削除します。</span><span class="sxs-lookup"><span data-stu-id="531a8-157">It will also prevent us from deleting a Dinner row if there are still RSVP rows referring to it.</span></span>

### <a name="adding-data-to-our-tables"></a><span data-ttu-id="531a8-158">データ テーブルを追加します。</span><span class="sxs-lookup"><span data-stu-id="531a8-158">Adding Data to our Tables</span></span>

<span data-ttu-id="531a8-159">[完了]、Dinners テーブルにサンプル データを追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="531a8-159">Let's finish by adding some sample data to our Dinners table.</span></span> <span data-ttu-id="531a8-160">サーバー エクスプ ローラー内で右クリックし、「テーブル データの表示」コマンドを選択して、テーブルにデータを追加します。</span><span class="sxs-lookup"><span data-stu-id="531a8-160">We can add data to a table by right-clicking on it within the Server Explorer and choosing the "Show Table Data" command:</span></span>

![](create-a-database/_static/image16.png)

<span data-ttu-id="531a8-161">アプリケーションの実装を開始すると後で使用できるデータを Dinner の少数の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="531a8-161">We'll add a few rows of Dinner data that we can use later as we start implementing the application:</span></span>

![](create-a-database/_static/image17.png)

### <a name="next-step"></a><span data-ttu-id="531a8-162">次の手順</span><span class="sxs-lookup"><span data-stu-id="531a8-162">Next Step</span></span>

<span data-ttu-id="531a8-163">データベースの作成は終了しました。</span><span class="sxs-lookup"><span data-stu-id="531a8-163">We've finished creating our database.</span></span> <span data-ttu-id="531a8-164">クエリを実行し、更新するために使用できるモデル クラスを今すぐ作成しましょう。</span><span class="sxs-lookup"><span data-stu-id="531a8-164">Let's now create model classes that we can use to query and update it.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="531a8-165">[前へ](create-a-new-aspnet-mvc-project.md)
> [次へ](build-a-model-with-business-rule-validations.md)</span><span class="sxs-lookup"><span data-stu-id="531a8-165">[Previous](create-a-new-aspnet-mvc-project.md)
[Next](build-a-model-with-business-rule-validations.md)</span></span>
