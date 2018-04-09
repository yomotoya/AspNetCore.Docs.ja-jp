---
uid: web-pages/overview/data/5-working-with-data
title: ASP.NET Web でのデータベース操作の概要ページ (Razor) サイト |Microsoft ドキュメント
author: tfitzmac
description: この章では、データベースからデータにアクセスし、ASP.NET Web Pages を使用して表示する方法について説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2014
ms.topic: article
ms.assetid: 673d502f-2c16-4a6f-bb63-dbfd9a77ef47
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/data/5-working-with-data
msc.type: authoredcontent
ms.openlocfilehash: 563074cf3e60717c2e6c336a2c282b4203f73b8b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="introduction-to-working-with-a-database-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="1d4b3-103">ASP.NET Web でのデータベース操作の概要ページ (Razor) サイト</span><span class="sxs-lookup"><span data-stu-id="1d4b3-103">Introduction to Working with a Database in ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="1d4b3-104">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="1d4b3-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="1d4b3-105">この記事では、Microsoft WebMatrix のツールを使用して、ASP.NET Web Pages (Razor) の web サイトでデータベースを作成する方法と表示、追加、編集、およびデータを削除することができるページを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-105">This article describes how to use Microsoft WebMatrix tools to create a database in an ASP.NET Web Pages (Razor) website, and how to create pages that let you display, add, edit, and delete data.</span></span>
> 
> <span data-ttu-id="1d4b3-106">**学習する内容。**</span><span class="sxs-lookup"><span data-stu-id="1d4b3-106">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="1d4b3-107">データベースを作成する方法。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-107">How to create a database.</span></span>
> - <span data-ttu-id="1d4b3-108">データベースに接続する方法です。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-108">How to connect to a database.</span></span>
> - <span data-ttu-id="1d4b3-109">Web ページにデータを表示する方法です。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-109">How to display data in a web page.</span></span>
> - <span data-ttu-id="1d4b3-110">挿入、更新、およびデータベースのレコードを削除する方法。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-110">How to insert, update, and delete database records.</span></span>
> 
> <span data-ttu-id="1d4b3-111">これらは、アーティクルで導入された機能です。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-111">These are the features introduced in the article:</span></span>
> 
> - <span data-ttu-id="1d4b3-112">Microsoft SQL Server Compact Edition データベースで操作します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-112">Working with a Microsoft SQL Server Compact Edition database.</span></span>
> - <span data-ttu-id="1d4b3-113">SQL クエリを使用します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-113">Working with SQL queries.</span></span>
> - <span data-ttu-id="1d4b3-114">`Database` クラス</span><span class="sxs-lookup"><span data-stu-id="1d4b3-114">The `Database` class.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="1d4b3-115">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="1d4b3-115">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="1d4b3-116">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="1d4b3-116">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="1d4b3-117">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="1d4b3-117">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="1d4b3-118">このチュートリアルは、WebMatrix 3 でも動作します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-118">This tutorial also works with WebMatrix 3.</span></span> <span data-ttu-id="1d4b3-119">ASP.NET Web Pages 3 と Visual Studio 2013 (または Visual Studio Express 2013 for Web) を使用することができます。ただし、ユーザー インターフェイスは別になります。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-119">You can use ASP.NET Web Pages 3 and Visual Studio 2013 (or Visual Studio Express 2013 for Web); however, the user interface will be different.</span></span>


## <a name="introduction-to-databases"></a><span data-ttu-id="1d4b3-120">データベースの概要</span><span class="sxs-lookup"><span data-stu-id="1d4b3-120">Introduction to Databases</span></span>

<span data-ttu-id="1d4b3-121">通常、アドレス帳を想像してください。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-121">Imagine a typical address book.</span></span> <span data-ttu-id="1d4b3-122">アドレス帳の各エントリについて (つまり、各ユーザーに対して) が複数ある名、姓、名、アドレス、電子メール アドレス、および電話番号などの情報をします。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-122">For each entry in the address book (that is, for each person) you have several pieces of information such as first name, last name, address, email address, and phone number.</span></span>

<span data-ttu-id="1d4b3-123">次のように画像データへの一般的な方法は、行と列を含むテーブルとしてです。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-123">A typical way to picture data like this is as a table with rows and columns.</span></span> <span data-ttu-id="1d4b3-124">データベース用語では、各行は多くの場合と呼びますレコード。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-124">In database terms, each row is often referred to as a record.</span></span> <span data-ttu-id="1d4b3-125">(フィールドとも呼ばれます) の各列には、データの種類ごとに値が含まれています: 名、姓の名、およびなどです。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-125">Each column (sometimes referred to as fields) contains a value for each type of data: first name, last name, and so on.</span></span>

| <span data-ttu-id="1d4b3-126">**ID**</span><span class="sxs-lookup"><span data-stu-id="1d4b3-126">**ID**</span></span> | <span data-ttu-id="1d4b3-127">**FirstName**</span><span class="sxs-lookup"><span data-stu-id="1d4b3-127">**FirstName**</span></span> | <span data-ttu-id="1d4b3-128">**LastName**</span><span class="sxs-lookup"><span data-stu-id="1d4b3-128">**LastName**</span></span> | <span data-ttu-id="1d4b3-129">**アドレス**</span><span class="sxs-lookup"><span data-stu-id="1d4b3-129">**Address**</span></span> | <span data-ttu-id="1d4b3-130">**電子メール**</span><span class="sxs-lookup"><span data-stu-id="1d4b3-130">**Email**</span></span> | <span data-ttu-id="1d4b3-131">**電話**</span><span class="sxs-lookup"><span data-stu-id="1d4b3-131">**Phone**</span></span> |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="1d4b3-132">1</span><span class="sxs-lookup"><span data-stu-id="1d4b3-132">1</span></span> | <span data-ttu-id="1d4b3-133">Jim</span><span class="sxs-lookup"><span data-stu-id="1d4b3-133">Jim</span></span> | <span data-ttu-id="1d4b3-134">Abrus</span><span class="sxs-lookup"><span data-stu-id="1d4b3-134">Abrus</span></span> | <span data-ttu-id="1d4b3-135">210 100th St SE Orcas WA 98031</span><span class="sxs-lookup"><span data-stu-id="1d4b3-135">210 100th St SE Orcas WA 98031</span></span> | jim@contoso.com | <span data-ttu-id="1d4b3-136">555 0100</span><span class="sxs-lookup"><span data-stu-id="1d4b3-136">555 0100</span></span> |
| <span data-ttu-id="1d4b3-137">2</span><span class="sxs-lookup"><span data-stu-id="1d4b3-137">2</span></span> | <span data-ttu-id="1d4b3-138">Terry</span><span class="sxs-lookup"><span data-stu-id="1d4b3-138">Terry</span></span> | <span data-ttu-id="1d4b3-139">Adams</span><span class="sxs-lookup"><span data-stu-id="1d4b3-139">Adams</span></span> | <span data-ttu-id="1d4b3-140">1234 Main St. Seattle WA 99011</span><span class="sxs-lookup"><span data-stu-id="1d4b3-140">1234 Main St. Seattle WA 99011</span></span> | terry@cohowinery.com | <span data-ttu-id="1d4b3-141">555 0101</span><span class="sxs-lookup"><span data-stu-id="1d4b3-141">555 0101</span></span> |

<span data-ttu-id="1d4b3-142">ほとんどのデータベース テーブルの表に、必要と顧客番号、アカウント番号などの一意の識別子を含む列です。これは、テーブルと呼ばれます*主キー*テーブル内の各行の識別に使用するとします。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-142">For most database tables, the table has to have a column that contains a unique identifier, like a customer number, account number, etc. This is known as the table's *primary key*, and you use it to identify each row in the table.</span></span> <span data-ttu-id="1d4b3-143">例では、ID 列は、アドレス帳の主キーがします。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-143">In the example, the ID column is the primary key for the address book.</span></span>

<span data-ttu-id="1d4b3-144">データベースの基本的な理解するおくには、単純なデータベースを作成し、追加、変更、およびデータの削除などの操作を実行する方法を学習する準備ができました。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-144">With this basic understanding of databases, you're ready to learn how to create a simple database and perform operations such as adding, modifying, and deleting data.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="1d4b3-145">**リレーショナル データベース**</span><span class="sxs-lookup"><span data-stu-id="1d4b3-145">**Relational Databases**</span></span>
> 
> <span data-ttu-id="1d4b3-146">多数の方法があり、スプレッドシートやテキスト ファイルにデータを格納することができます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-146">You can store data in lots of ways, including text files and spreadsheets.</span></span> <span data-ttu-id="1d4b3-147">ほとんどのビジネス用途のため、データが保存リレーショナル データベースにします。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-147">For most business uses, though, data is stored in a relational database.</span></span>
> 
> <span data-ttu-id="1d4b3-148">この記事はしないデータベースに非常に密接に移動します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-148">This article doesn't go very deeply into databases.</span></span> <span data-ttu-id="1d4b3-149">ただし、する可能性がありますが役に立つそれらについて少し理解します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-149">However, you might find it useful to understand a little about them.</span></span> <span data-ttu-id="1d4b3-150">リレーショナル データベースでは、個別のテーブルに情報が論理的に分けられます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-150">In a relational database, information is logically divided into separate tables.</span></span> <span data-ttu-id="1d4b3-151">たとえば、学校用のデータベースは、受講者とでクラスの内容、個別のテーブルを含めることがします。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-151">For example, a database for a school might contain separate tables for students and for class offerings.</span></span> <span data-ttu-id="1d4b3-152">データベース (SQL Server) などのソフトウェア サポート強力なコマンドを動的に可能にするには、テーブル間のリレーションシップが確立します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-152">The database software (such as SQL Server) supports powerful commands that let you dynamically establish relationships between the tables.</span></span> <span data-ttu-id="1d4b3-153">たとえば、スケジュールを作成するために受講者およびクラス間に論理リレーションシップを確立するために、リレーショナル データベースを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-153">For example, you can use the relational database to establish a logical relationship between students and classes in order to create a schedule.</span></span> <span data-ttu-id="1d4b3-154">別のテーブルにデータを格納するテーブルの構造の複雑さを軽減し、テーブルに冗長なデータを保持する必要性が軽減します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-154">Storing data in separate tables reduces the complexity of the table structure and reduces the need to keep redundant data in tables.</span></span>


## <a name="creating-a-database"></a><span data-ttu-id="1d4b3-155">データベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-155">Creating a Database</span></span>

<span data-ttu-id="1d4b3-156">この手順では、WebMatrix に含まれている SQL Server Compact データベース デザイン ツールを使用して SmallBakery をという名前のデータベースを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-156">This procedure shows you how to create a database named SmallBakery by using the SQL Server Compact Database design tool that's included in WebMatrix.</span></span> <span data-ttu-id="1d4b3-157">コードを使用してデータベースを作成することができますが、データベースおよび WebMatrix のようにデザイン ツールを使用してデータベース テーブルを作成する一般的なです。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-157">Although you can create a database using code, it's more typical to create the database and database tables using a design tool like WebMatrix.</span></span>

1. <span data-ttu-id="1d4b3-158">WebMatrix を起動し、[クイック スタート] ページで、をクリックして**サイトからテンプレート**です。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-158">Start WebMatrix, and on the Quick Start page, click **Site From Template**.</span></span>
2. <span data-ttu-id="1d4b3-159">選択**空のサイト**、し、、**サイト名**ボックス"SmallBakery"を入力し、をクリックして**OK**です。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-159">Select **Empty Site**, and in the **Site Name** box enter "SmallBakery" and then click **OK**.</span></span> <span data-ttu-id="1d4b3-160">サイトが作成され、WebMatrix で表示されます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-160">The site is created and displayed in WebMatrix.</span></span>
3. <span data-ttu-id="1d4b3-161">左側のウィンドウでをクリックして、**データベース**ワークスペース。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-161">In the left pane, click the **Databases** workspace.</span></span>
4. <span data-ttu-id="1d4b3-162">リボンで、をクリックして**新しいデータベース**です。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-162">In the ribbon, click **New Database**.</span></span> <span data-ttu-id="1d4b3-163">サイトと同じ名前の空のデータベースが作成されます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-163">An empty database is created with the same name as your site.</span></span>
5. <span data-ttu-id="1d4b3-164">左側のウィンドウで、展開、 **SmallBakery.sdf**ノードをクリックして**テーブル**です。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-164">In the left pane, expand the **SmallBakery.sdf** node and then click **Tables**.</span></span>
6. <span data-ttu-id="1d4b3-165">リボンで、をクリックして**新しいテーブル**です。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-165">In the ribbon, click **New Table**.</span></span> <span data-ttu-id="1d4b3-166">WebMatrix は、テーブル デザイナーを開きます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-166">WebMatrix opens the table designer.</span></span>

    ![[イメージ]](5-working-with-data/_static/image1.jpg)
7. <span data-ttu-id="1d4b3-168">をクリックして、**名前**列と入力&quot;Id&quot;です。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-168">Click in the **Name** column and enter &quot;Id&quot;.</span></span>
8. <span data-ttu-id="1d4b3-169">**データ型**列をオン**int**です。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-169">In the **Data Type** column, select **int**.</span></span>
9. <span data-ttu-id="1d4b3-170">設定、**主キーであるですか?**と**を識別するですか?**オプションを**はい**です。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-170">Set the **Is Primary Key?** and **Is Identify?** options to **Yes**.</span></span>

    <span data-ttu-id="1d4b3-171">名前からわかるように、**主キーである**これは、テーブルの主キーであることをデータベースに指示します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-171">As the name suggests, **Is Primary Key** tells the database that this will be the table's primary key.</span></span> <span data-ttu-id="1d4b3-172">**[Is Identity]**すべての新しいレコードの ID 番号を自動的に作成して (1 から始まります) 次の連番を割り当てるために、データベースに指示します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-172">**Is Identity** tells the database to automatically create an ID number for every new record and to assign it the next sequential number (starting at 1).</span></span>
10. <span data-ttu-id="1d4b3-173">次の行をクリックします。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-173">Click in the next row.</span></span> <span data-ttu-id="1d4b3-174">エディターでは、新しい列の定義を開始します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-174">The editor starts a new column definition.</span></span>
11. <span data-ttu-id="1d4b3-175">名前の値を入力してください。&quot;名前&quot;です。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-175">For the Name value, enter &quot;Name&quot;.</span></span>
12. <span data-ttu-id="1d4b3-176">**データ型**、選択&quot;nvarchar&quot;し、長さは 50 に設定します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-176">For **Data Type**, choose &quot;nvarchar&quot; and set the length to 50.</span></span> <span data-ttu-id="1d4b3-177">*Var*の一部`nvarchar`この列のデータがサイズが異なるレコード間を文字列になるデータベースに指示します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-177">The *var* part of `nvarchar` tells the database that the data for this column will be a string whose size might vary from record to record.</span></span> <span data-ttu-id="1d4b3-178">(、 *N*プレフィックスを表す*national*フィールドがすべてアルファベット順を表す文字データを保持できることを示す、または書記体系&#8212;は、フィールドが Unicode データを保持している)。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-178">(The *n* prefix represents *national*, indicating that the field can hold character data that represents any alphabet or writing system &#8212; that is, that the field holds Unicode data.)</span></span>
13. <span data-ttu-id="1d4b3-179">設定、 **null を許容**オプションを**いいえ**です。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-179">Set the **Allow Nulls** option to **No**.</span></span> <span data-ttu-id="1d4b3-180">これが適用されます、*名前*列がない空白のままにします。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-180">This will enforce that the *Name* column is not left blank.</span></span>
14. <span data-ttu-id="1d4b3-181">この同じプロセスを使用してという名前の列を作成*説明*です。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-181">Using this same process, create a column named *Description*.</span></span> <span data-ttu-id="1d4b3-182">設定**データ型**"nvarchar"を長さ、およびセットの 50 **Null を許容**を false に設定します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-182">Set **Data Type** to "nvarchar" and 50 for the length, and set **Allow Nulls** to false.</span></span>
15. <span data-ttu-id="1d4b3-183">という名前の列を作成する*価格*です。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-183">Create a column named *Price*.</span></span> <span data-ttu-id="1d4b3-184">設定**"money"データ型**設定と**Null を許容**を false に設定します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-184">Set **Data Type to "money"** and set **Allow Nulls** to false.</span></span>
16. <span data-ttu-id="1d4b3-185">上部のボックスで、テーブルの名前を&quot;製品&quot;です。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-185">In the box at the top, name the table &quot;Product&quot;.</span></span>

    <span data-ttu-id="1d4b3-186">完了したら、定義は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-186">When you're done, the definition will look like this:</span></span>

    ![[イメージ]](5-working-with-data/_static/image2.jpg)
17. <span data-ttu-id="1d4b3-188">テーブルを保存するには、Ctrl + S キーを押します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-188">Press Ctrl+S to save the table.</span></span>

## <a name="adding-data-to-the-database"></a><span data-ttu-id="1d4b3-189">データベースにデータを追加します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-189">Adding Data to the Database</span></span>

<span data-ttu-id="1d4b3-190">今すぐ演習、記事の後半では、データベースにいくつかサンプル データを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-190">Now you can add some sample data to your database that you'll work with later in the article.</span></span>

1. <span data-ttu-id="1d4b3-191">左側のウィンドウで、展開、 **SmallBakery.sdf**ノードをクリックして**テーブル**です。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-191">In the left pane, expand the **SmallBakery.sdf** node and then click **Tables**.</span></span>
2. <span data-ttu-id="1d4b3-192">Product テーブルを右クリックし、をクリックして**データ**です。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-192">Right-click the Product table and then click **Data**.</span></span>
3. <span data-ttu-id="1d4b3-193">編集ウィンドウで、次のレコードを入力します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-193">In the edit pane, enter the following records:</span></span>

    | <span data-ttu-id="1d4b3-194">**Name**</span><span class="sxs-lookup"><span data-stu-id="1d4b3-194">**Name**</span></span> | <span data-ttu-id="1d4b3-195">**説明**</span><span class="sxs-lookup"><span data-stu-id="1d4b3-195">**Description**</span></span> | <span data-ttu-id="1d4b3-196">**価格**</span><span class="sxs-lookup"><span data-stu-id="1d4b3-196">**Price**</span></span> |
    | --- | --- | --- |
    | <span data-ttu-id="1d4b3-197">パン</span><span class="sxs-lookup"><span data-stu-id="1d4b3-197">Bread</span></span> | <span data-ttu-id="1d4b3-198">毎日の焼き立て新鮮です。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-198">Baked fresh every day.</span></span> | <span data-ttu-id="1d4b3-199">2.99</span><span class="sxs-lookup"><span data-stu-id="1d4b3-199">2.99</span></span> |
    | <span data-ttu-id="1d4b3-200">ストロベリー ショート ケーキ</span><span class="sxs-lookup"><span data-stu-id="1d4b3-200">Strawberry Shortcake</span></span> | <span data-ttu-id="1d4b3-201">当社の庭から有機 strawberries で行われます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-201">Made with organic strawberries from our garden.</span></span> | <span data-ttu-id="1d4b3-202">9.99</span><span class="sxs-lookup"><span data-stu-id="1d4b3-202">9.99</span></span> |
    | <span data-ttu-id="1d4b3-203">Apple Pie</span><span class="sxs-lookup"><span data-stu-id="1d4b3-203">Apple Pie</span></span> | <span data-ttu-id="1d4b3-204">母親の円グラフにのみ 2 番目です。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-204">Second only to your mom's pie.</span></span> | <span data-ttu-id="1d4b3-205">12.99</span><span class="sxs-lookup"><span data-stu-id="1d4b3-205">12.99</span></span> |
    | <span data-ttu-id="1d4b3-206">Pecan 円</span><span class="sxs-lookup"><span data-stu-id="1d4b3-206">Pecan Pie</span></span> | <span data-ttu-id="1d4b3-207">Pecans 場合に、これが役立ちます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-207">If you like pecans, this is for you.</span></span> | <span data-ttu-id="1d4b3-208">10.99</span><span class="sxs-lookup"><span data-stu-id="1d4b3-208">10.99</span></span> |
    | <span data-ttu-id="1d4b3-209">レモンの円</span><span class="sxs-lookup"><span data-stu-id="1d4b3-209">Lemon Pie</span></span> | <span data-ttu-id="1d4b3-210">世界中で最適な lemons で行われます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-210">Made with the best lemons in the world.</span></span> | <span data-ttu-id="1d4b3-211">11.99</span><span class="sxs-lookup"><span data-stu-id="1d4b3-211">11.99</span></span> |
    | <span data-ttu-id="1d4b3-212">すてきなカップケーキ</span><span class="sxs-lookup"><span data-stu-id="1d4b3-212">Cupcakes</span></span> | <span data-ttu-id="1d4b3-213">お子供様たちとするで kid がこれら大好きです。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-213">Your kids and the kid in you will love these.</span></span> | <span data-ttu-id="1d4b3-214">7.99</span><span class="sxs-lookup"><span data-stu-id="1d4b3-214">7.99</span></span> |

    <span data-ttu-id="1d4b3-215">注意を何も入力する必要はありません、 *Id*列です。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-215">Remember that you don't have to enter anything for the *Id* column.</span></span> <span data-ttu-id="1d4b3-216">作成したときに、 *Id*設定する列で、その**Is Identity**プロパティを true の場合、これにより、自動的に入力することです。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-216">When you created the *Id* column, you set its **Is Identity** property to true, which causes it to automatically be filled in.</span></span>

    <span data-ttu-id="1d4b3-217">データ入力が完了したら、テーブル デザイナーは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-217">When you're finished entering the data, the table designer will look like this:</span></span>

    ![[イメージ]](5-working-with-data/_static/image3.jpg)
4. <span data-ttu-id="1d4b3-219">データベースのデータを含むタブを閉じます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-219">Close the tab that contains the database data.</span></span>

## <a name="displaying-data-from-a-database"></a><span data-ttu-id="1d4b3-220">データベースからデータを表示します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-220">Displaying Data from a Database</span></span>

<span data-ttu-id="1d4b3-221">データベースにデータを取得したらは、ASP.NET web ページで、データを表示できます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-221">Once you've got a database with data in it, you can display the data in an ASP.NET web page.</span></span> <span data-ttu-id="1d4b3-222">表示するテーブルの行を選択するには、データベースに渡すコマンドは、SQL ステートメントを使用します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-222">To select the table rows to display, you use a SQL statement, which is a command that you pass to the database.</span></span>

1. <span data-ttu-id="1d4b3-223">左側のウィンドウでをクリックして、**ファイル**ワークスペース。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-223">In the left pane, click the **Files** workspace.</span></span>
2. <span data-ttu-id="1d4b3-224">Web サイトのルートで、作成という名前の新しい CSHTML ページ*ListProducts.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-224">In the root of the website, create a new CSHTML page named *ListProducts.cshtml*.</span></span>
3. <span data-ttu-id="1d4b3-225">既存のマークアップを次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-225">Replace the existing markup with the following:</span></span>

    [!code-cshtml[Main](5-working-with-data/samples/sample1.cshtml)]

    <span data-ttu-id="1d4b3-226">最初のコード ブロックで開く、 *SmallBakery.sdf*先ほど作成したファイル (データベース)。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-226">In the first code block, you open the *SmallBakery.sdf* file (database) that you created earlier.</span></span> <span data-ttu-id="1d4b3-227">`Database.Open`メソッドを想定する、 *.sdf*ファイルは、web サイトの*アプリ\_データ*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-227">The `Database.Open` method assumes that the *.sdf* file is in your website's *App\_Data* folder.</span></span> <span data-ttu-id="1d4b3-228">(ことを指定する必要はありません、 *.sdf*拡張子&#8212;ファクトを指定する場合に、`Open`メソッドは機能しません)。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-228">(Notice that you don't need to specify the *.sdf* extension &#8212; in fact, if you do, the `Open` method won't work.)</span></span>

    > [!NOTE]
    > <span data-ttu-id="1d4b3-229">*アプリ\_データ*フォルダーは、データ ファイルの格納に使用される ASP.NET で特別なフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-229">The *App\_Data* folder is a special folder in ASP.NET that's used to store data files.</span></span> <span data-ttu-id="1d4b3-230">詳細については、次を参照してください。[データベースへの接続](#SB_ConnectingToADatabase)この記事で後述します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-230">For more information, see [Connecting to a Database](#SB_ConnectingToADatabase) later in this article.</span></span>

    <span data-ttu-id="1d4b3-231">次の SQL を使用してデータベースを照会する要求を行う、`Select`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-231">You then make a request to query the database using the following SQL `Select` statement:</span></span>

    [!code-sql[Main](5-working-with-data/samples/sample2.sql)]

    <span data-ttu-id="1d4b3-232">ステートメントでは、`Product`クエリにテーブルを識別します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-232">In the statement, `Product` identifies the table to query.</span></span> <span data-ttu-id="1d4b3-233">`*`文字では、クエリが、テーブルからすべての列を返すことを指定します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-233">The `*` character specifies that the query should return all the columns from the table.</span></span> <span data-ttu-id="1d4b3-234">(でしたも一覧表示列に個別に、一部の列のみを表示する場合は、コンマで区切る。)`Order By`句では、データの並べ替え方法を示します&#8212;ここでは、によって、*名前*列です。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-234">(You could also list columns individually, separated by commas, if you wanted to see only some of the columns.) The `Order By` clause indicates how the data should be sorted &#8212; in this case, by the *Name* column.</span></span> <span data-ttu-id="1d4b3-235">これは、データがの値に基づいてアルファベット順に並べ替えが意味、*名前*各行の列です。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-235">This means that the data is sorted alphabetically based on the value of the *Name* column for each row.</span></span>

    <span data-ttu-id="1d4b3-236">ページの本文には、マークアップは、データの表示に使用される HTML テーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-236">In the body of the page, the markup creates an HTML table that will be used to display the data.</span></span> <span data-ttu-id="1d4b3-237">内部、`<tbody>`要素を使用する、`foreach`ループを個別に、クエリによって返される各データ行を取得します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-237">Inside the `<tbody>` element, you use a `foreach` loop to individually get each data row that's returned by the query.</span></span> <span data-ttu-id="1d4b3-238">HTML テーブルの行を作成する各データ行 (`<tr>`要素)。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-238">For each data row, you create an HTML table row (`<tr>` element).</span></span> <span data-ttu-id="1d4b3-239">HTML テーブルのセルを作成してから (`<td>`要素) の各列にします。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-239">Then you create HTML table cells (`<td>` elements) for each column.</span></span> <span data-ttu-id="1d4b3-240">ループを通過するたびに、データベースから [次へ]、使用可能な行が、`row`変数 (で設定したこの、`foreach`ステートメント)。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-240">Each time you go through the loop, the next available row from the database is in the `row` variable (you set this up in the `foreach` statement).</span></span> <span data-ttu-id="1d4b3-241">使用することができます、個々 の列の行を取得する`row.Name`または`row.Description`かする列は、任意の名前。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-241">To get an individual column from the row, you can use `row.Name` or `row.Description` or whatever the name is of the column you want.</span></span>
4. <span data-ttu-id="1d4b3-242">ブラウザーでページを実行します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-242">Run the page in a browser.</span></span> <span data-ttu-id="1d4b3-243">(ページが選択されて、必ず、**ファイル**ワークスペースを実行する前にします)。ページには、次のような一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-243">(Make sure the page is selected in the **Files** workspace before you run it.) The page displays a list like the following:</span></span>

    ![[イメージ]](5-working-with-data/_static/image4.jpg)

> [!TIP] 
> 
> <span data-ttu-id="1d4b3-245">**構造化照会言語 (SQL)**</span><span class="sxs-lookup"><span data-stu-id="1d4b3-245">**Structured Query Language (SQL)**</span></span>
> 
> <span data-ttu-id="1d4b3-246">SQL は、データベース内のデータを管理するため、ほとんどのリレーショナル データベースで使用されている言語です。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-246">SQL is a language that's used in most relational databases for managing data in a database.</span></span> <span data-ttu-id="1d4b3-247">これには、データを取得し、更新するための作成、変更、およびデータベースのテーブルを管理するためのコマンドが含まれます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-247">It includes commands that let you retrieve data and update it, and that let you create, modify, and manage database tables.</span></span> <span data-ttu-id="1d4b3-248">SQL とは異なる (webmatrix を使用しているような) のプログラミング言語、考え方は、データベースを指定すると、目的の動作、データを取得またはタスクを実行する方法を理解するデータベースのジョブは、SQL を使用したためです。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-248">SQL is different than a programming language (like the one you're using in WebMatrix) because with SQL, the idea is that you tell the database what you want, and it's the database's job to figure out how to get the data or perform the task.</span></span> <span data-ttu-id="1d4b3-249">一部の SQL コマンドの例とその実行内容を次に示します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-249">Here are examples of some SQL commands and what they do:</span></span>
> 
> `SELECT Id, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> <span data-ttu-id="1d4b3-250">これをフェッチ、 *Id*、*名前*、および*価格*内のレコードからの列、*製品*場合のテーブルの値*価格*は 10 個を超えると、結果の値に基づいてアルファベット順に返します、*名前*列です。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-250">This fetches the *Id*, *Name*, and *Price* columns from records in the *Product* table if the value of *Price* is more than 10, and returns the results in alphabetical order based on the values of the *Name* column.</span></span> <span data-ttu-id="1d4b3-251">このコマンドでは、レコードと一致しない場合、条件、または空のセットに一致するレコードを含む結果セットを返します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-251">This command will return a result set that contains the records that meet the criteria, or an empty set if no records match.</span></span>
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ("Croissant", "A flaky delight", 1.99)`
> 
> <span data-ttu-id="1d4b3-252">新しいレコードを挿入しますこの、*製品*設定、テーブル、*名前*列を&quot;クロワッサン&quot;、*説明*列。&quot;不安定な楽しめます&quot;とから 1.99 する価格。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-252">This inserts a new record into the *Product* table, setting the *Name* column to &quot;Croissant&quot;, the *Description* column to &quot;A flaky delight&quot;, and the price to 1.99.</span></span>
> 
> `DELETE FROM Product WHERE ExpirationDate < "01/01/2008"`
> 
> <span data-ttu-id="1d4b3-253">このコマンドは、内のレコードを削除、*製品*有効期限の日付列が含まれるは 2008 年 1 月 1 日より前のテーブルです。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-253">This command deletes records in the *Product* table whose expiration date column is earlier than January 1, 2008.</span></span> <span data-ttu-id="1d4b3-254">(これが想定する、*製品*テーブルにあるこのような列は、当然のことながら)。/MM/DD/YYYY 形式で日付がここに入力したが、ロケールに使用される形式で入力する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-254">(This assumes that the *Product* table has such a column, of course.) The date is entered here in MM/DD/YYYY format, but it should be entered in the format that's used for your locale.</span></span>
> 
> <span data-ttu-id="1d4b3-255">`Insert Into`と`Delete`コマンドの結果セットを返しません。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-255">The `Insert Into` and `Delete` commands don't return result sets.</span></span> <span data-ttu-id="1d4b3-256">代わりに、これらは、コマンドの影響を受けたレコードの数を示す番号を返します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-256">Instead, they return a number that tells you how many records were affected by the command.</span></span>
> 
> <span data-ttu-id="1d4b3-257">これらの操作 (挿入して、レコードを削除する) などの一部で、操作を要求しているプロセスの適切なアクセス許可を持つデータベースではします。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-257">For some of these operations (like inserting and deleting records), the process that's requesting the operation has to have appropriate permissions in the database.</span></span> <span data-ttu-id="1d4b3-258">これは、実稼働データベースの多くの場合、データベースに接続するときに、ユーザー名とパスワードを指定する必要があるためです。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-258">This is why for production databases you often have to supply a username and password when you connect to the database.</span></span>
> 
> <span data-ttu-id="1d4b3-259">重複除去は、SQL コマンドがありますが、このようなパターンに従うすべて。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-259">There are dozens of SQL commands, but they all follow a pattern like this.</span></span> <span data-ttu-id="1d4b3-260">SQL コマンドを使用して、データベース テーブルを作成、テーブル内のレコードの数をカウント、価格、計算、および多くの操作を実行することができます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-260">You can use SQL commands to create database tables, count the number of records in a table, calculate prices, and perform many more operations.</span></span>


## <a name="inserting-data-in-a-database"></a><span data-ttu-id="1d4b3-261">データベースのデータを挿入します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-261">Inserting Data in a Database</span></span>

<span data-ttu-id="1d4b3-262">このセクションでは、ユーザーが新しい製品を追加できるページを作成する方法を示します、*製品*データベース テーブルです。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-262">This section shows how to create a page that lets users add a new product to the *Product* database table.</span></span> <span data-ttu-id="1d4b3-263">更新されたテーブルを使用して、新しい製品レコードが挿入されると、ページに表示されます、 *ListProducts.cshtml*前のセクションで作成したページ。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-263">After a new product record is inserted, the page displays the updated table using the *ListProducts.cshtml* page that you created in the previous section.</span></span>

<span data-ttu-id="1d4b3-264">このページには、ユーザーが入力したデータがデータベースに対して有効であるかどうかを確認する検証が含まれます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-264">The page includes validation to make sure that the data that the user enters is valid for the database.</span></span> <span data-ttu-id="1d4b3-265">たとえば、ページ内のコードは、すべての必要な列の値が入力されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-265">For example, code in the page makes sure that a value has been entered for all required columns.</span></span>

1. <span data-ttu-id="1d4b3-266">という名前の新しい CSHTML ファイルを作成、web サイトで*InsertProducts.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-266">In the website, create a new CSHTML file named *InsertProducts.cshtml*.</span></span>
2. <span data-ttu-id="1d4b3-267">既存のマークアップを次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-267">Replace the existing markup with the following:</span></span>

    [!code-cshtml[Main](5-working-with-data/samples/sample3.cshtml)]

    <span data-ttu-id="1d4b3-268">ページの本文には、ユーザー名、説明、および価格を入力できるように次の 3 つのテキスト ボックスの HTML フォームが含まれています。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-268">The body of the page contains an HTML form with three text boxes that let users enter a name, description, and price.</span></span> <span data-ttu-id="1d4b3-269">ユーザーがクリックして、**挿入**ボタン、ページの上部にあるコードがへの接続を開きます、 *SmallBakery.sdf*データベース。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-269">When users click the **Insert** button, the code at the top of the page opens a connection to the *SmallBakery.sdf* database.</span></span> <span data-ttu-id="1d4b3-270">使用して、ユーザーが送信する値を取得する、`Request`オブジェクトし、それらの値をローカル変数に割り当てます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-270">You then get the values that the user has submitted by using the `Request` object and assign those values to local variables.</span></span>

    <span data-ttu-id="1d4b3-271">各ユーザーが必要な各列の値を入力することを検証するには、登録する`<input>`を検証する要素。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-271">To validate that the user entered a value for each required column, you register each `<input>` element that you want to validate:</span></span>

    [!code-csharp[Main](5-working-with-data/samples/sample4.cs)]

    <span data-ttu-id="1d4b3-272">`Validation`ヘルパーは、各登録したフィールドの値があることを確認します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-272">The `Validation` helper checks that there is a value in each of the fields that you've registered.</span></span> <span data-ttu-id="1d4b3-273">すべてのフィールドをチェックして検証を渡されるかどうかをテストする`Validation.IsValid()`、どのする通常の方法で、ユーザーから取得した情報を処理する前に。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-273">You can test whether all the fields passed validation by checking `Validation.IsValid()`, which you typically do before you process the information you get from the user:</span></span>

    [!code-csharp[Main](5-working-with-data/samples/sample5.cs)]

    <span data-ttu-id="1d4b3-274">(、`&&`演算子は AND — このテストは*フォームの送信接続が、すべてのフィールドが検証に合格しました*)。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-274">(The `&&` operator means AND — this test is *If this is a form submission AND all the fields have passed validation*.)</span></span>

    <span data-ttu-id="1d4b3-275">すべての列が検証された場合 (がなかった空)、データを挿入し、次に示すように実行する SQL ステートメントを作成してください。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-275">If all the columns validated (none were empty), you go ahead and create a SQL statement to insert the data and then execute it as shown next:</span></span>

    [!code-csharp[Main](5-working-with-data/samples/sample6.cs)]

    <span data-ttu-id="1d4b3-276">挿入する値は、パラメーターのプレース ホルダーを含める (`@0`、 `@1`、 `@2`)。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-276">For the values to insert, you include parameter placeholders (`@0`, `@1`, `@2`).</span></span>

    > [!NOTE]
    > <span data-ttu-id="1d4b3-277">セキュリティ上の予防措置として常に値を渡すパラメーターを使用して SQL ステートメントにように前の例を参照してください。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-277">As a security precaution, always pass values to a SQL statement using parameters, as you see in the preceding example.</span></span> <span data-ttu-id="1d4b3-278">これは、方法により、ユーザーのデータを検証すると、悪意のあるコマンドを (SQL インジェクション攻撃とも呼ばれます)、データベースに送信する試行を防ぐのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-278">This gives you a chance to validate the user's data, plus it helps protect against attempts to send malicious commands to your database (sometimes referred to as SQL injection attacks).</span></span>

    <span data-ttu-id="1d4b3-279">クエリを実行するには、ステートメントを使用するこのプレース ホルダーの代わりに値を含む変数を渡します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-279">To execute the query, you use this statement, passing to it the variables that contain the values to substitute for the placeholders:</span></span>

    [!code-csharp[Main](5-working-with-data/samples/sample7.cs)]

    <span data-ttu-id="1d4b3-280">後に、`Insert Into`ステートメントが実行される、行を使用して製品を一覧表示するページへユーザーを送信します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-280">After the `Insert Into` statement has executed, you send the user to the page that lists the products using this line:</span></span>

    [!code-javascript[Main](5-working-with-data/samples/sample8.js)]

    <span data-ttu-id="1d4b3-281">検証が成功しなかった場合は、挿入をスキップします。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-281">If validation didn't succeed, you skip the insert.</span></span> <span data-ttu-id="1d4b3-282">代わりに、蓄積されたエラー メッセージ (存在する場合) を表示できるページに、ヘルパーがあります。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-282">Instead, you have a helper in the page that can display the accumulated error messages (if any):</span></span>

    [!code-cshtml[Main](5-working-with-data/samples/sample9.cshtml)]

    <span data-ttu-id="1d4b3-283">マークアップにスタイル ブロックに名前付き CSS クラスの定義が含まれることに注意してください`.validation-summary-errors`です。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-283">Notice that the style block in the markup includes a CSS class definition named `.validation-summary-errors`.</span></span> <span data-ttu-id="1d4b3-284">これは、既定で使用される CSS クラスの名前、`<div>`検証エラーを含む要素です。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-284">This is the name of the CSS class that's used by default for the `<div>` element that contains any validation errors.</span></span> <span data-ttu-id="1d4b3-285">この例では、CSS クラスことを指定検証概要エラーが赤で表示されます、太字には定義することができます、`.validation-summary-errors`か書式を表示するクラス。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-285">In this case, the CSS class specifies that validation summary errors are displayed in red and in bold, but you can define the `.validation-summary-errors` class to display any formatting you like.</span></span>

### <a name="testing-the-insert-page"></a><span data-ttu-id="1d4b3-286">Insert ページのテスト</span><span class="sxs-lookup"><span data-stu-id="1d4b3-286">Testing the Insert Page</span></span>

1. <span data-ttu-id="1d4b3-287">ブラウザーでページを表示します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-287">View the page in a browser.</span></span> <span data-ttu-id="1d4b3-288">次の図に示すようにしているフォームを表示します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-288">The page displays a form that's similar to the one that's shown in the following illustration.</span></span>

    ![[イメージ]](5-working-with-data/_static/image5.jpg)
2. <span data-ttu-id="1d4b3-290">すべての列の値を入力がしておくことを確認してください、*価格*列は空白です。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-290">Enter values for all the columns, but make sure that you leave the *Price* column blank.</span></span>
3. <span data-ttu-id="1d4b3-291">**[挿入]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-291">Click **Insert**.</span></span> <span data-ttu-id="1d4b3-292">ページは、次の図に示すように、エラー メッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-292">The page displays an error message, as shown in the following illustration.</span></span> <span data-ttu-id="1d4b3-293">(新しいレコードは作成されません。)</span><span class="sxs-lookup"><span data-stu-id="1d4b3-293">(No new record is created.)</span></span>

    ![[イメージ]](5-working-with-data/_static/image6.jpg)
4. <span data-ttu-id="1d4b3-295">クリックして、完全にフォームを記入**挿入**です。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-295">Fill the form out completely, and then click **Insert**.</span></span> <span data-ttu-id="1d4b3-296">このとき、 *ListProducts.cshtml*ページが表示され、新しいレコードを示しています。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-296">This time, the *ListProducts.cshtml* page is displayed and shows the new record.</span></span>

## <a name="updating-data-in-a-database"></a><span data-ttu-id="1d4b3-297">データベース内のデータの更新</span><span class="sxs-lookup"><span data-stu-id="1d4b3-297">Updating Data in a Database</span></span>

<span data-ttu-id="1d4b3-298">テーブルにデータが入力された後は、更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-298">After data has been entered into a table, you might need to update it.</span></span> <span data-ttu-id="1d4b3-299">この手順では、データの挿入先用に作成したものに類似する 2 つのページを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-299">This procedure shows you how to create two pages that are similar to the ones you created for data insertion earlier.</span></span> <span data-ttu-id="1d4b3-300">最初のページでは、製品を表示し、ユーザーを変更する 1 つを選択します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-300">The first page displays products and lets users select one to change.</span></span> <span data-ttu-id="1d4b3-301">2 番目のページには、実際に編集を行うし、それらを保存、ユーザーが使用できます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-301">The second page lets the users actually make the edits and save them.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="1d4b3-302">**重要な**実稼働 web サイトで通常を制限する、データ変更を許可するユーザー。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-302">**Important** In a production website, you typically restrict who's allowed to make changes to the data.</span></span> <span data-ttu-id="1d4b3-303">メンバーシップを設定する方法の詳細と、サイトでタスクを実行するユーザーを承認する方法について、次を参照してください。[追加のセキュリティと ASP.NET Web Pages サイトにメンバーシップ](https://go.microsoft.com/fwlink/?LinkId=202904)です。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-303">For information about how to set up membership and about ways to authorize users to perform tasks on the site, see [Adding Security and Membership to an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=202904).</span></span>


1. <span data-ttu-id="1d4b3-304">という名前の新しい CSHTML ファイルを作成、web サイトで*EditProducts.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-304">In the website, create a new CSHTML file named *EditProducts.cshtml*.</span></span>
2. <span data-ttu-id="1d4b3-305">次のように、ファイルの既存のマークアップに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-305">Replace the existing markup in the file with the following:</span></span>

    [!code-cshtml[Main](5-working-with-data/samples/sample10.cshtml)]

    <span data-ttu-id="1d4b3-306">このページの唯一の違い、および*ListProducts.cshtml*ページから前はこのページの HTML テーブルが含まれる、余分な列を表示する、**編集**リンクします。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-306">The only difference between this page and the *ListProducts.cshtml* page from earlier is that the HTML table in this page includes an extra column that displays an **Edit** link.</span></span> <span data-ttu-id="1d4b3-307">するにはこのリンクをクリックすると、 *UpdateProducts.cshtml*  ページ (次を作成します)、選択したレコードを編集することができます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-307">When you click this link, it takes you to the *UpdateProducts.cshtml* page (which you'll create next) where you can edit the selected record.</span></span>

    <span data-ttu-id="1d4b3-308">作成するコードを見て、**編集**リンク。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-308">Look at the code that creates the **Edit** link:</span></span>

    [!code-cshtml[Main](5-working-with-data/samples/sample11.cshtml)]

    <span data-ttu-id="1d4b3-309">こうと、HTML`<a>`要素が`href`属性が動的に設定します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-309">This creates an HTML `<a>` element whose `href` attribute is set dynamically.</span></span> <span data-ttu-id="1d4b3-310">`href`属性は、ユーザーがリンクをクリックしたときに表示するページを指定します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-310">The `href` attribute specifies the page to display when the user clicks the link.</span></span> <span data-ttu-id="1d4b3-311">渡しても、`Id`リンクには、現在の行の値。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-311">It also passes the `Id` value of the current row to the link.</span></span> <span data-ttu-id="1d4b3-312">ページを実行すると、ページのソースには、上記のようなリンクが含まれます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-312">When the page runs, the page source might contain links like these:</span></span>

    [!code-html[Main](5-working-with-data/samples/sample12.html)]

    <span data-ttu-id="1d4b3-313">注意して、`href`属性に設定されている`UpdateProducts/n`ここで、 *n*製品番号です。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-313">Notice that the `href` attribute is set to `UpdateProducts/n`, where *n* is a product number.</span></span> <span data-ttu-id="1d4b3-314">ユーザーをクリックすると、これらのリンクのいずれかが、結果の URL は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-314">When a user clicks one of these links, the resulting URL will look something like this:</span></span>

    `http://localhost:18816/UpdateProducts/6`

    <span data-ttu-id="1d4b3-315">つまり、製品番号を編集することは、URL で渡されます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-315">In other words, the product number to be edited will be passed in the URL.</span></span>
3. <span data-ttu-id="1d4b3-316">ブラウザーでページを表示します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-316">View the page in a browser.</span></span> <span data-ttu-id="1d4b3-317">ページには、次のような形式で、データが表示されます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-317">The page displays the data in a format like this:</span></span>

    ![[イメージ]](5-working-with-data/_static/image7.jpg)

    <span data-ttu-id="1d4b3-319">次に、ユーザーが実際には、データを更新できるページを作成します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-319">Next, you'll create the page that lets users actually update the data.</span></span> <span data-ttu-id="1d4b3-320">更新プログラム ページには、ユーザーが入力したデータを検証する検証が含まれています。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-320">The update page includes validation to validate the data that the user enters.</span></span> <span data-ttu-id="1d4b3-321">たとえば、ページ内のコードは、すべての必要な列の値が入力されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-321">For example, code in the page makes sure that a value has been entered for all required columns.</span></span>
4. <span data-ttu-id="1d4b3-322">という名前の新しい CSHTML ファイルを作成、web サイトで*UpdateProducts.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-322">In the website, create a new CSHTML file named *UpdateProducts.cshtml*.</span></span>
5. <span data-ttu-id="1d4b3-323">次のファイルの既存のマークアップに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-323">Replace the existing markup in the file with the following.</span></span>

    [!code-cshtml[Main](5-working-with-data/samples/sample13.cshtml)]

    <span data-ttu-id="1d4b3-324">ページの本文には、製品を表示する場所とユーザーが編集できる HTML フォームが含まれています。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-324">The body of the page contains an HTML form where a product is displayed and where users can edit it.</span></span> <span data-ttu-id="1d4b3-325">表示する製品を取得するには、この SQL ステートメントを使用します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-325">To get the product to display, you use this SQL statement:</span></span>

    [!code-sql[Main](5-working-with-data/samples/sample14.sql)]

    <span data-ttu-id="1d4b3-326">これにより、ID が渡される値に一致する製品を選択、`@0`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-326">This will select the product whose ID matches the value that's passed in the `@0` parameter.</span></span> <span data-ttu-id="1d4b3-327">(ため*Id*は主キー、したがって必要がありますで一意である、1 つの製品レコードがこの方法を選択するれたことができます)。これに渡す ID 値を取得する`Select`ステートメントに渡されるページの URL の一部として、次の構文を使用して値を読み取ることができます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-327">(Because *Id* is the primary key and therefore must be unique, only one product record can ever be selected this way.) To get the ID value to pass to this `Select` statement, you can read the value that's passed to the page as part of the URL, using the following syntax:</span></span>

    [!code-csharp[Main](5-working-with-data/samples/sample15.cs)]

    <span data-ttu-id="1d4b3-328">製品レコードを実際にフェッチするには、使用する、`QuerySingle`メソッドで、1 つのレコードを返します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-328">To actually fetch the product record, you use the `QuerySingle` method, which will return just one record:</span></span>

    [!code-csharp[Main](5-working-with-data/samples/sample16.cs)]

    <span data-ttu-id="1d4b3-329">1 つの行が返されます、`row`変数。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-329">The single row is returned into the `row` variable.</span></span> <span data-ttu-id="1d4b3-330">各列からデータを取得し、次のようにローカル変数に割り当てることができます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-330">You can get data out of each column and assign it to local variables like this:</span></span>

    [!code-csharp[Main](5-working-with-data/samples/sample17.cs)]

    <span data-ttu-id="1d4b3-331">フォームのマークアップでこれらの値が表示されます自動的に個々 のテキスト ボックスで、次のように埋め込みコードを使用しています。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-331">In the markup for the form, these values are displayed automatically in individual text boxes by using embedded code like the following:</span></span>

    [!code-html[Main](5-working-with-data/samples/sample18.html)]

    <span data-ttu-id="1d4b3-332">コードの部分には、更新する製品レコードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-332">That part of the code displays the product record to be updated.</span></span> <span data-ttu-id="1d4b3-333">レコードが表示された後、ユーザーは、個々 の列を編集できます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-333">Once the record has been displayed, the user can edit individual columns.</span></span>

    <span data-ttu-id="1d4b3-334">クリックして、ユーザーがフォームを送信した場合、**更新**ボタン、コード、`if(IsPost)`ブロックが実行されます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-334">When the user submits the form by clicking the **Update** button, the code in the `if(IsPost)` block runs.</span></span> <span data-ttu-id="1d4b3-335">これにより、ユーザーの値が取得、`Request`オブジェクトは、変数値を格納し各列が満杯になったことを検証します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-335">This gets the user's values from the `Request` object, stores the values in variables, and validates that each column has been filled in.</span></span> <span data-ttu-id="1d4b3-336">検証に合格した場合、コードは次の SQL の Update ステートメントを作成します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-336">If validation passes, the code creates the following SQL Update statement:</span></span>

    [!code-sql[Main](5-working-with-data/samples/sample19.sql)]

    <span data-ttu-id="1d4b3-337">Sql`Update`ステートメント設定する値で更新するには、各列を指定します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-337">In a SQL `Update` statement, you specify each column to update and the value to set it to.</span></span> <span data-ttu-id="1d4b3-338">このコードで、値は、パラメーター プレース ホルダーを使用して`@0`、 `@1`、`@2`のようにします。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-338">In this code, the values are specified using the parameter placeholders `@0`, `@1`, `@2`, and so on.</span></span> <span data-ttu-id="1d4b3-339">(述べたとおり、セキュリティをする必要があります常に値を渡す SQL ステートメントにパラメーターを使用しています。)</span><span class="sxs-lookup"><span data-stu-id="1d4b3-339">(As noted earlier, for security, you should always pass values to a SQL statement by using parameters.)</span></span>

    <span data-ttu-id="1d4b3-340">呼び出すと、`db.Execute`メソッド、順番は、SQL ステートメントのパラメーターに対応する値を含む変数を渡します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-340">When you call the `db.Execute` method, you pass the variables that contain the values in the order that corresponds to the parameters in the SQL statement:</span></span>

    [!code-csharp[Main](5-working-with-data/samples/sample20.cs)]

    <span data-ttu-id="1d4b3-341">後に、`Update`ステートメントが実行される、ユーザーを編集 ページにリダイレクトするために、次のメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-341">After the `Update` statement has been executed, you call the following method in order to redirect the user back to the edit page:</span></span>

    [!code-cshtml[Main](5-working-with-data/samples/sample21.cshtml)]

    <span data-ttu-id="1d4b3-342">効果は、ユーザーがデータベース内のデータの更新の一覧を表示し、別の製品を編集できます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-342">The effect is that the user sees an updated listing of the data in the database and can edit another product.</span></span>
6. <span data-ttu-id="1d4b3-343">ページを保存します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-343">Save the page.</span></span>
7. <span data-ttu-id="1d4b3-344">実行、 *EditProducts.cshtml*ページ ([更新] ページではない) をクリックして**編集**を編集する製品を選択します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-344">Run the *EditProducts.cshtml* page (not the update page) and then click **Edit** to select a product to edit.</span></span> <span data-ttu-id="1d4b3-345">*UpdateProducts.cshtml*ページが表示されたら、選択したレコードを表示します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-345">The *UpdateProducts.cshtml* page is displayed, showing the record you selected.</span></span>

    ![[イメージ]](5-working-with-data/_static/image8.jpg)
8. <span data-ttu-id="1d4b3-347">変更を行い、をクリックして**更新**です。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-347">Make a change and click **Update**.</span></span> <span data-ttu-id="1d4b3-348">製品一覧が更新されたデータをもう一度表示されます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-348">The products list is shown again with your updated data.</span></span>

## <a name="deleting-data-in-a-database"></a><span data-ttu-id="1d4b3-349">データベース内のデータを削除します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-349">Deleting Data in a Database</span></span>

<span data-ttu-id="1d4b3-350">このセクションでは、ユーザーから製品を削除できるようにする方法を示します、*製品*データベース テーブルです。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-350">This section shows how to let users delete a product from the *Product* database table.</span></span> <span data-ttu-id="1d4b3-351">この例は、2 つのページで構成されます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-351">The example consists of two pages.</span></span> <span data-ttu-id="1d4b3-352">最初のページでは、ユーザーは、削除するレコードを選択します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-352">In the first page, users select a record to delete.</span></span> <span data-ttu-id="1d4b3-353">削除するレコードはレコードを削除することを確認することができ、2 番目のページに表示されます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-353">The record to be deleted is then displayed in a second page that lets them confirm that they want to delete the record.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="1d4b3-354">**重要な**実稼働 web サイトで通常を制限する、データ変更を許可するユーザー。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-354">**Important** In a production website, you typically restrict who's allowed to make changes to the data.</span></span> <span data-ttu-id="1d4b3-355">メンバーシップを設定する方法の詳細と、サイトでタスクを実行するユーザーを承認する方法について、次を参照してください。[追加のセキュリティと ASP.NET Web Pages サイトにメンバーシップ](https://go.microsoft.com/fwlink/?LinkId=202904)です。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-355">For information about how to set up membership and about ways to authorize user to perform tasks on the site, see [Adding Security and Membership to an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=202904).</span></span>


1. <span data-ttu-id="1d4b3-356">という名前の新しい CSHTML ファイルを作成、web サイトで*ListProductsForDelete.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-356">In the website, create a new CSHTML file named *ListProductsForDelete.cshtml*.</span></span>
2. <span data-ttu-id="1d4b3-357">既存のマークアップを次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-357">Replace the existing markup with the following:</span></span>

    [!code-cshtml[Main](5-working-with-data/samples/sample22.cshtml)]

    <span data-ttu-id="1d4b3-358">このページがに似ていますが、 *EditProducts.cshtml*から前のページです。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-358">This page is similar to the *EditProducts.cshtml* page from earlier.</span></span> <span data-ttu-id="1d4b3-359">表示するのではなく、ただし、**編集**リンクについて、各製品が表示されます、**削除**リンクします。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-359">However, instead of displaying an **Edit** link for each product, it displays a **Delete** link.</span></span> <span data-ttu-id="1d4b3-360">**削除**マークアップに次の埋め込みコードを使用してリンクを作成します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-360">The **Delete** link is created using the following embedded code in the markup:</span></span>

    [!code-cshtml[Main](5-working-with-data/samples/sample23.cshtml)]

    <span data-ttu-id="1d4b3-361">これには、ユーザーのリンクをクリックすると、次のような URL を作成します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-361">This creates a URL that looks like this when users click the link:</span></span>

    `http://<server>/DeleteProduct/4`

    <span data-ttu-id="1d4b3-362">URL は、という名前のページを呼び出す*DeleteProduct.cshtml* (これは、次を作成します) を削除する、製品の ID を渡します (ここでは、4)。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-362">The URL calls a page named *DeleteProduct.cshtml* (which you'll create next) and passes it the ID of the product to delete (here, 4).</span></span>
3. <span data-ttu-id="1d4b3-363">ファイルの保存が開いたままにしておきます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-363">Save the file, but leave it open.</span></span>
4. <span data-ttu-id="1d4b3-364">という名前の別の CHTML ファイルを作成する*DeleteProduct.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-364">Create another CHTML file named *DeleteProduct.cshtml*.</span></span> <span data-ttu-id="1d4b3-365">次のように、既存のコンテンツを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-365">Replace the existing content with the following:</span></span>

    [!code-cshtml[Main](5-working-with-data/samples/sample24.cshtml)]

    <span data-ttu-id="1d4b3-366">このページは、によって呼び出されます。 *ListProductsForDelete.cshtml*でき、ユーザーが製品を削除することを確認します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-366">This page is called by *ListProductsForDelete.cshtml* and lets users confirm that they want to delete a product.</span></span> <span data-ttu-id="1d4b3-367">削除する製品を一覧表示するには、ID を取得した製品の URL から削除する次のコードを使用します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-367">To list the product to be deleted, you get the ID of the product to delete from the URL using the following code:</span></span>

    [!code-csharp[Main](5-working-with-data/samples/sample25.cs)]

    <span data-ttu-id="1d4b3-368">ページは、ユーザーを実際には、レコードを削除するためのボタンをクリックしてを求めます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-368">The page then asks the user to click a button to actually delete the record.</span></span> <span data-ttu-id="1d4b3-369">これは、重要なセキュリティ対策: これらの操作を行う常に、web サイトを更新したり、データの削除などの機密性の高い操作を実行するとき、POST 操作、GET 操作ではなくを使用します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-369">This is an important security measure: when you perform sensitive operations in your website like updating or deleting data, these operations should always be done using a POST operation, not a GET operation.</span></span> <span data-ttu-id="1d4b3-370">GET 操作で削除操作を行えるように、サイトのセットアップ、すべてのユーザーとのように URL を渡すことができます`http://<server>/DeleteProduct/4`し、必要なものをデータベースから削除します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-370">If your site is set up so that a delete operation can be performed using a GET operation, anyone can pass a URL like `http://<server>/DeleteProduct/4` and delete anything they want from your database.</span></span> <span data-ttu-id="1d4b3-371">追加して、確認のみ POST を使用して、削除を実行できるように、ページをコーディング、サイト セキュリティのメジャーを追加します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-371">By adding the confirmation and coding the page so that the deletion can be performed only by using a POST, you add a measure of security to your site.</span></span>

    <span data-ttu-id="1d4b3-372">実際の削除操作は、まず post 操作であるし、ID が空でないことを確認する次のコードを使用して実行されます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-372">The actual delete operation is performed using the following code, which first confirms that this is a post operation and that the ID isn't empty:</span></span>

    [!code-csharp[Main](5-working-with-data/samples/sample26.cs)]

    <span data-ttu-id="1d4b3-373">コードでは、指定されたレコードを削除し、一覧のページに戻る、ユーザーをリダイレクトする SQL ステートメントを実行します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-373">The code runs a SQL statement that deletes the specified record and then redirects the user back to the listing page.</span></span>
5. <span data-ttu-id="1d4b3-374">実行*ListProductsForDelete.cshtml*ブラウザーでします。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-374">Run *ListProductsForDelete.cshtml* in a browser.</span></span>

    ![[イメージ]](5-working-with-data/_static/image9.jpg)
6. <span data-ttu-id="1d4b3-376">クリックして、**削除**製品のいずれかのリンク。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-376">Click the **Delete** link for one of the products.</span></span> <span data-ttu-id="1d4b3-377">*DeleteProduct.cshtml*そのレコードを削除することを確認ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-377">The *DeleteProduct.cshtml* page is displayed to confirm that you want to delete that record.</span></span>
7. <span data-ttu-id="1d4b3-378">クリックして、**削除**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-378">Click the **Delete** button.</span></span> <span data-ttu-id="1d4b3-379">製品レコードが削除され、更新された製品一覧と、ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-379">The product record is deleted and the page is refreshed with an updated product listing.</span></span>

> [!TIP]
> 
> <a id="SB_ConnectingToADatabase"></a>
> ### <a name="connecting-to-a-database"></a><span data-ttu-id="1d4b3-380">データベースへの接続</span><span class="sxs-lookup"><span data-stu-id="1d4b3-380">Connecting to a Database</span></span>
> 
> <span data-ttu-id="1d4b3-381">2 つの方法でデータベースに接続することができます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-381">You can connect to a database in two ways.</span></span> <span data-ttu-id="1d4b3-382">最初は、使用する、`Database.Open`メソッドと、データベース ファイルの名前を指定する (以下、 *.sdf*拡張機能)。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-382">The first is to use the `Database.Open` method and to specify the name of the database file (less the *.sdf* extension):</span></span>
> 
> `var db = Database.Open("SmallBakery");`
> 
> <span data-ttu-id="1d4b3-383">`Open`メソッドでは、いるものとします*。sdf*ファイルは、web サイトの*アプリ\_データ*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-383">The `Open` method assumes that the .*sdf* file is in the website's *App\_Data* folder.</span></span> <span data-ttu-id="1d4b3-384">このフォルダーはデータを保持する専用に設計されています。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-384">This folder is designed specifically for holding data.</span></span> <span data-ttu-id="1d4b3-385">たとえば、データを読み書きする web サイトを許可する適切なアクセス許可があるし、セキュリティ対策として WebMatrix いないアクセス許可ファイルにこのフォルダーから。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-385">For example, it has appropriate permissions to allow the website to read and write data, and as a security measure, WebMatrix does not allow access to files from this folder.</span></span>
> 
> <span data-ttu-id="1d4b3-386">2 番目の方法では、接続文字列を使用します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-386">The second way is to use a connection string.</span></span> <span data-ttu-id="1d4b3-387">接続文字列には、データベースに接続する方法に関する情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-387">A connection string contains information about how to connect to a database.</span></span> <span data-ttu-id="1d4b3-388">これは、ファイルのパスを含めることができます、またはユーザー名とそのサーバーに接続するパスワードと共に、ローカルまたはリモート サーバー上の SQL Server データベースの名前を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-388">This can include a file path, or it can include the name of a SQL Server database on a local or remote server, along with a user name and password to connect to that server.</span></span> <span data-ttu-id="1d4b3-389">(一元管理型のバージョンの SQL Server でデータを保持する場合など、ホスティング プロバイダーのサイトでは、必ず使用する接続文字列、データベース接続情報を指定する。)</span><span class="sxs-lookup"><span data-stu-id="1d4b3-389">(If you keep data in a centrally managed version of SQL Server, such as on a hosting provider's site, you always use a connection string to specify the database connection information.)</span></span>
> 
> <span data-ttu-id="1d4b3-390">WebMatrix で接続文字列は通常、という名前の XML ファイルに格納*Web.config*です。名前が示すように、使用できます、 *Web.config*サイトが必要となる任意の接続文字列を含む、サイトの構成情報を格納する web サイトのルートにあるファイルです。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-390">In WebMatrix, connection strings are usually stored in an XML file named *Web.config*. As the name implies, you can use a *Web.config* file in the root of your website to store the site's configuration information, including any connection strings that your site might require.</span></span> <span data-ttu-id="1d4b3-391">内の接続文字列の例、 *Web.config*ファイルは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-391">An example of a connection string in a *Web.config* file might look like the following:</span></span>
> 
> [!code-xml[Main](5-working-with-data/samples/sample27.xml)]
> 
> <span data-ttu-id="1d4b3-392">例では、接続文字列はどこかに、サーバーで実行されている SQL Server のインスタンスにデータベースを指す (ローカルではなく*.sdf*ファイル)。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-392">In the example, the connection string points to a database in an instance of SQL Server that's running on a server somewhere (as opposed to a local *.sdf* file).</span></span> <span data-ttu-id="1d4b3-393">適切な名前の代わりに使用する必要があります`myServer`と`myDatabase`、SQL Server ログインの値を指定して`username`と`password`です。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-393">You would need to substitute the appropriate names for `myServer` and `myDatabase`, and specify SQL Server login values for `username` and `password`.</span></span> <span data-ttu-id="1d4b3-394">(ユーザー名とパスワードの値は必ずしも同じ Windows 資格情報として、または、サーバーへのログインに、ホスティング プロバイダーから提供された値として</span><span class="sxs-lookup"><span data-stu-id="1d4b3-394">(The username and password values are not necessarily the same as your Windows credentials or as the values that your hosting provider has given you for logging in to their servers.</span></span> <span data-ttu-id="1d4b3-395">管理者に確認する必要がある正確な値にします。)</span><span class="sxs-lookup"><span data-stu-id="1d4b3-395">Check with the administrator for the exact values you need.)</span></span>
> 
> <span data-ttu-id="1d4b3-396">`Database.Open`メソッドは、データベースの名前を渡すことができるため、柔軟*.sdf*ファイルまたはに格納されている接続文字列の名前、 *Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-396">The `Database.Open` method is flexible, because it lets you pass either the name of a database *.sdf* file or the name of a connection string that's stored in the *Web.config* file.</span></span> <span data-ttu-id="1d4b3-397">次の例では、前の例に示す接続文字列を使用して、データベースに接続する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-397">The following example shows how to connect to the database using the connection string illustrated in the previous example:</span></span>
> 
> [!code-cshtml[Main](5-working-with-data/samples/sample28.cshtml)]
> 
> <span data-ttu-id="1d4b3-398">前述のように、`Database.Open`メソッドでは、データベース名または接続文字列を渡すことができ、それを算出を使用します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-398">As noted, the `Database.Open` method lets you pass either a database name or a connection string, and it'll figure out which to use.</span></span> <span data-ttu-id="1d4b3-399">これは、展開するときに非常に便利です (公開)、web サイトです。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-399">This is very useful when you deploy (publish) your website.</span></span> <span data-ttu-id="1d4b3-400">使用することができます、 *.sdf*ファイルで、*アプリ\_データ*フォルダーの開発およびサイトのテスト時にします。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-400">You can use an *.sdf* file in the *App\_Data* folder when you're developing and testing your site.</span></span> <span data-ttu-id="1d4b3-401">内の接続文字列を使用して、サイトを実稼働サーバーに移動すると、 *Web.config*ファイルと同じ名前を持つ、 *.sdf* &#8212;せずに、コードを変更します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-401">Then when you move your site to a production server, you can use a connection string in the *Web.config* file that has the same name as your *.sdf* file but that points to the hosting provider's database &#8212; all without having to change your code.</span></span>
> 
> <span data-ttu-id="1d4b3-402">最後に、接続文字列を直接操作する場合を呼び出すことができます、`Database.OpenConnectionString`メソッドと実際の接続が文字列で 1 つの名前だけではなく、パス、 *Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-402">Finally, if you want to work directly with a connection string, you can call the `Database.OpenConnectionString` method and pass it the actual connection string instead of just the name of one in the *Web.config* file.</span></span> <span data-ttu-id="1d4b3-403">何らかの理由がない、接続文字列へのアクセス状況で便利なことが考えられます (などの値、または、 *.sdf*ファイル名)、ページが実行されるまでです。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-403">This might be useful in situations where for some reason you don't have access to the connection string (or values in it, such as the *.sdf* file name) until the page is running.</span></span> <span data-ttu-id="1d4b3-404">ただし、ほとんどのシナリオで使用できます`Database.Open`ようにこの記事で説明します。</span><span class="sxs-lookup"><span data-stu-id="1d4b3-404">However, for most scenarios, you can use `Database.Open` as described in this article.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="1d4b3-405">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="1d4b3-405">Additional Resources</span></span>

- [<span data-ttu-id="1d4b3-406">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="1d4b3-406">SQL Server Compact</span></span>](https://www.microsoft.com/sqlserver/2008/en/us/compact.aspx)
- [<span data-ttu-id="1d4b3-407">SQL Server または WebMatrix の MySQL データベースへの接続</span><span class="sxs-lookup"><span data-stu-id="1d4b3-407">Connecting to a SQL Server or MySQL Database in WebMatrix</span></span>](https://go.microsoft.com/fwlink/?LinkId=208661)
- [<span data-ttu-id="1d4b3-408">ASP.NET Web ページにおけるユーザー入力の検証</span><span class="sxs-lookup"><span data-stu-id="1d4b3-408">Validating User Input in ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkId=253002)
