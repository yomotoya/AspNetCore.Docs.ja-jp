---
uid: web-pages/overview/data/5-working-with-data
title: ページ (Razor) サイトを ASP.NET Web でのデータベース操作の概要 |Microsoft Docs
author: tfitzmac
description: この章では、データベースからデータにアクセスし、ASP.NET Web Pages を使用して表示する方法について説明します。
ms.author: riande
ms.date: 02/18/2014
ms.assetid: 673d502f-2c16-4a6f-bb63-dbfd9a77ef47
msc.legacyurl: /web-pages/overview/data/5-working-with-data
msc.type: authoredcontent
ms.openlocfilehash: b6db23c6f9bba418dff7e6b50bbc1c54f13ccb70
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831564"
---
<a name="introduction-to-working-with-a-database-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="f992d-103">ページ (Razor) サイトを ASP.NET Web でのデータベース操作の概要</span><span class="sxs-lookup"><span data-stu-id="f992d-103">Introduction to Working with a Database in ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="f992d-104">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f992d-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="f992d-105">この記事では、Microsoft WebMatrix ツールを使用して、ASP.NET Web Pages (Razor) の web サイトでデータベースを作成する方法と表示、追加、編集、およびデータを削除することができるページを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="f992d-105">This article describes how to use Microsoft WebMatrix tools to create a database in an ASP.NET Web Pages (Razor) website, and how to create pages that let you display, add, edit, and delete data.</span></span>
> 
> <span data-ttu-id="f992d-106">**学習内容。**</span><span class="sxs-lookup"><span data-stu-id="f992d-106">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="f992d-107">データベースを作成する方法。</span><span class="sxs-lookup"><span data-stu-id="f992d-107">How to create a database.</span></span>
> - <span data-ttu-id="f992d-108">データベースに接続する方法。</span><span class="sxs-lookup"><span data-stu-id="f992d-108">How to connect to a database.</span></span>
> - <span data-ttu-id="f992d-109">Web ページにデータを表示する方法。</span><span class="sxs-lookup"><span data-stu-id="f992d-109">How to display data in a web page.</span></span>
> - <span data-ttu-id="f992d-110">挿入、更新、およびデータベースのレコードを削除する方法。</span><span class="sxs-lookup"><span data-stu-id="f992d-110">How to insert, update, and delete database records.</span></span>
> 
> <span data-ttu-id="f992d-111">この記事で導入された機能を次に示します。</span><span class="sxs-lookup"><span data-stu-id="f992d-111">These are the features introduced in the article:</span></span>
> 
> - <span data-ttu-id="f992d-112">Microsoft SQL Server Compact Edition データベースを使用します。</span><span class="sxs-lookup"><span data-stu-id="f992d-112">Working with a Microsoft SQL Server Compact Edition database.</span></span>
> - <span data-ttu-id="f992d-113">SQL クエリを使用します。</span><span class="sxs-lookup"><span data-stu-id="f992d-113">Working with SQL queries.</span></span>
> - <span data-ttu-id="f992d-114">`Database` クラス</span><span class="sxs-lookup"><span data-stu-id="f992d-114">The `Database` class.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f992d-115">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="f992d-115">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="f992d-116">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="f992d-116">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="f992d-117">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="f992d-117">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="f992d-118">このチュートリアルは、WebMatrix 3 でも機能します。</span><span class="sxs-lookup"><span data-stu-id="f992d-118">This tutorial also works with WebMatrix 3.</span></span> <span data-ttu-id="f992d-119">ASP.NET Web ページ 3 と Visual Studio 2013 (または Visual Studio Express 2013 for Web) を使用することができます。ただし、ユーザー インターフェイスは別になります。</span><span class="sxs-lookup"><span data-stu-id="f992d-119">You can use ASP.NET Web Pages 3 and Visual Studio 2013 (or Visual Studio Express 2013 for Web); however, the user interface will be different.</span></span>


## <a name="introduction-to-databases"></a><span data-ttu-id="f992d-120">データベースの概要</span><span class="sxs-lookup"><span data-stu-id="f992d-120">Introduction to Databases</span></span>

<span data-ttu-id="f992d-121">通常、アドレス帳を想像してください。</span><span class="sxs-lookup"><span data-stu-id="f992d-121">Imagine a typical address book.</span></span> <span data-ttu-id="f992d-122">アドレス帳のエントリごとに (つまり、各人の) 名、姓、住所、電子メール アドレス、および電話番号などのいくつかの情報があります。</span><span class="sxs-lookup"><span data-stu-id="f992d-122">For each entry in the address book (that is, for each person) you have several pieces of information such as first name, last name, address, email address, and phone number.</span></span>

<span data-ttu-id="f992d-123">このような画像データへの一般的な方法は、行と列を含むテーブルとしてです。</span><span class="sxs-lookup"><span data-stu-id="f992d-123">A typical way to picture data like this is as a table with rows and columns.</span></span> <span data-ttu-id="f992d-124">データベース用語では、各行は多くの場合にレコードとして参照します。</span><span class="sxs-lookup"><span data-stu-id="f992d-124">In database terms, each row is often referred to as a record.</span></span> <span data-ttu-id="f992d-125">(フィールドとも呼ばれます) の各列には、データの種類ごとの値が含まれています。 名、姓の名、およびなど。</span><span class="sxs-lookup"><span data-stu-id="f992d-125">Each column (sometimes referred to as fields) contains a value for each type of data: first name, last name, and so on.</span></span>

| <span data-ttu-id="f992d-126">**ID**</span><span class="sxs-lookup"><span data-stu-id="f992d-126">**ID**</span></span> | <span data-ttu-id="f992d-127">**FirstName**</span><span class="sxs-lookup"><span data-stu-id="f992d-127">**FirstName**</span></span> | <span data-ttu-id="f992d-128">**LastName**</span><span class="sxs-lookup"><span data-stu-id="f992d-128">**LastName**</span></span> | <span data-ttu-id="f992d-129">**アドレス**</span><span class="sxs-lookup"><span data-stu-id="f992d-129">**Address**</span></span> | <span data-ttu-id="f992d-130">**電子メール**</span><span class="sxs-lookup"><span data-stu-id="f992d-130">**Email**</span></span> | <span data-ttu-id="f992d-131">**電話**</span><span class="sxs-lookup"><span data-stu-id="f992d-131">**Phone**</span></span> |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="f992d-132">1</span><span class="sxs-lookup"><span data-stu-id="f992d-132">1</span></span> | <span data-ttu-id="f992d-133">Jim</span><span class="sxs-lookup"><span data-stu-id="f992d-133">Jim</span></span> | <span data-ttu-id="f992d-134">Abrus</span><span class="sxs-lookup"><span data-stu-id="f992d-134">Abrus</span></span> | <span data-ttu-id="f992d-135">210 100th St SE Orcas WA 98031</span><span class="sxs-lookup"><span data-stu-id="f992d-135">210 100th St SE Orcas WA 98031</span></span> | jim@contoso.com | <span data-ttu-id="f992d-136">555 0100</span><span class="sxs-lookup"><span data-stu-id="f992d-136">555 0100</span></span> |
| <span data-ttu-id="f992d-137">2</span><span class="sxs-lookup"><span data-stu-id="f992d-137">2</span></span> | <span data-ttu-id="f992d-138">Terry</span><span class="sxs-lookup"><span data-stu-id="f992d-138">Terry</span></span> | <span data-ttu-id="f992d-139">Adams</span><span class="sxs-lookup"><span data-stu-id="f992d-139">Adams</span></span> | <span data-ttu-id="f992d-140">1234 Main st. Seattle WA 99011</span><span class="sxs-lookup"><span data-stu-id="f992d-140">1234 Main St. Seattle WA 99011</span></span> | terry@cohowinery.com | <span data-ttu-id="f992d-141">555 0101</span><span class="sxs-lookup"><span data-stu-id="f992d-141">555 0101</span></span> |

<span data-ttu-id="f992d-142">ほとんどのデータベース テーブル、テーブルは、顧客番号、口座番号などのように、一意の識別子を含む列がある必要があります。これは、テーブルのと呼ばれます*主キー*テーブル内の各行を識別するために使用するとします。</span><span class="sxs-lookup"><span data-stu-id="f992d-142">For most database tables, the table has to have a column that contains a unique identifier, like a customer number, account number, etc. This is known as the table's *primary key*, and you use it to identify each row in the table.</span></span> <span data-ttu-id="f992d-143">例では、ID 列は、アドレス帳の主キーです。</span><span class="sxs-lookup"><span data-stu-id="f992d-143">In the example, the ID column is the primary key for the address book.</span></span>

<span data-ttu-id="f992d-144">データベースの基本的な理解するおくと、単純なデータベースを作成し、追加、変更、およびデータの削除などの操作を実行する方法を学習する準備ができました。</span><span class="sxs-lookup"><span data-stu-id="f992d-144">With this basic understanding of databases, you're ready to learn how to create a simple database and perform operations such as adding, modifying, and deleting data.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="f992d-145">**リレーショナル データベース**</span><span class="sxs-lookup"><span data-stu-id="f992d-145">**Relational Databases**</span></span>
> 
> <span data-ttu-id="f992d-146">多くの方法があり、テキスト ファイル、およびスプレッドシートでデータを格納することができます。</span><span class="sxs-lookup"><span data-stu-id="f992d-146">You can store data in lots of ways, including text files and spreadsheets.</span></span> <span data-ttu-id="f992d-147">ほとんどのビジネス用途のため、データはリレーショナル データベースでします。</span><span class="sxs-lookup"><span data-stu-id="f992d-147">For most business uses, though, data is stored in a relational database.</span></span>
> 
> <span data-ttu-id="f992d-148">この記事では、データベースに非常に密接に移動しません。</span><span class="sxs-lookup"><span data-stu-id="f992d-148">This article doesn't go very deeply into databases.</span></span> <span data-ttu-id="f992d-149">ただし、役立つことについて少し理解します。</span><span class="sxs-lookup"><span data-stu-id="f992d-149">However, you might find it useful to understand a little about them.</span></span> <span data-ttu-id="f992d-150">リレーショナル データベースでは、別々 のテーブルに情報が論理的に分割されます。</span><span class="sxs-lookup"><span data-stu-id="f992d-150">In a relational database, information is logically divided into separate tables.</span></span> <span data-ttu-id="f992d-151">たとえば、学校のデータベースは、受講者とクラスのソリューションの別個のテーブルを含む可能性があります。</span><span class="sxs-lookup"><span data-stu-id="f992d-151">For example, a database for a school might contain separate tables for students and for class offerings.</span></span> <span data-ttu-id="f992d-152">データベース (SQL Server) などのソフトウェア サポート強力なコマンドを動的には、テーブル間のリレーションシップを確立します。</span><span class="sxs-lookup"><span data-stu-id="f992d-152">The database software (such as SQL Server) supports powerful commands that let you dynamically establish relationships between the tables.</span></span> <span data-ttu-id="f992d-153">たとえば、スケジュールを作成するには、生徒やクラス間に論理リレーションシップを確立するために、リレーショナル データベースを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="f992d-153">For example, you can use the relational database to establish a logical relationship between students and classes in order to create a schedule.</span></span> <span data-ttu-id="f992d-154">別のテーブルにデータを格納するテーブル構造の複雑さを軽減し、冗長なデータをテーブルに保持する必要性を軽減します。</span><span class="sxs-lookup"><span data-stu-id="f992d-154">Storing data in separate tables reduces the complexity of the table structure and reduces the need to keep redundant data in tables.</span></span>


## <a name="creating-a-database"></a><span data-ttu-id="f992d-155">データベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="f992d-155">Creating a Database</span></span>

<span data-ttu-id="f992d-156">この手順では、WebMatrix に含まれている SQL Server Compact データベース設計ツールを使用して SmallBakery をという名前のデータベースを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="f992d-156">This procedure shows you how to create a database named SmallBakery by using the SQL Server Compact Database design tool that's included in WebMatrix.</span></span> <span data-ttu-id="f992d-157">コードを使用してデータベースを作成できますが、データベースと WebMatrix のようなデザイン ツールを使用してデータベース テーブルを作成するより一般的です。</span><span class="sxs-lookup"><span data-stu-id="f992d-157">Although you can create a database using code, it's more typical to create the database and database tables using a design tool like WebMatrix.</span></span>

1. <span data-ttu-id="f992d-158">WebMatrix を起動し、クイック スタート ページで、**テンプレートからサイト**します。</span><span class="sxs-lookup"><span data-stu-id="f992d-158">Start WebMatrix, and on the Quick Start page, click **Site From Template**.</span></span>
2. <span data-ttu-id="f992d-159">選択**空のサイト**、し、**サイト名**ボックスは、"SmallBakery"を入力し、クリックして **[ok]** します。</span><span class="sxs-lookup"><span data-stu-id="f992d-159">Select **Empty Site**, and in the **Site Name** box enter "SmallBakery" and then click **OK**.</span></span> <span data-ttu-id="f992d-160">サイトが作成され、WebMatrix で表示されます。</span><span class="sxs-lookup"><span data-stu-id="f992d-160">The site is created and displayed in WebMatrix.</span></span>
3. <span data-ttu-id="f992d-161">左側のウィンドウでをクリックして、**データベース**ワークスペース。</span><span class="sxs-lookup"><span data-stu-id="f992d-161">In the left pane, click the **Databases** workspace.</span></span>
4. <span data-ttu-id="f992d-162">リボンで、次のようにクリックします。**新しいデータベース**します。</span><span class="sxs-lookup"><span data-stu-id="f992d-162">In the ribbon, click **New Database**.</span></span> <span data-ttu-id="f992d-163">サイトと同じ名前の空のデータベースが作成されます。</span><span class="sxs-lookup"><span data-stu-id="f992d-163">An empty database is created with the same name as your site.</span></span>
5. <span data-ttu-id="f992d-164">左側のウィンドウで、展開、 **SmallBakery.sdf**ノードをクリック**テーブル**します。</span><span class="sxs-lookup"><span data-stu-id="f992d-164">In the left pane, expand the **SmallBakery.sdf** node and then click **Tables**.</span></span>
6. <span data-ttu-id="f992d-165">リボンで、次のようにクリックします。**新しいテーブル**します。</span><span class="sxs-lookup"><span data-stu-id="f992d-165">In the ribbon, click **New Table**.</span></span> <span data-ttu-id="f992d-166">WebMatrix では、テーブル デザイナーが開きます。</span><span class="sxs-lookup"><span data-stu-id="f992d-166">WebMatrix opens the table designer.</span></span>

    ![[イメージ]](5-working-with-data/_static/image1.jpg)
7. <span data-ttu-id="f992d-168">をクリックして、**名前**列を入力し、 &quot;Id&quot;します。</span><span class="sxs-lookup"><span data-stu-id="f992d-168">Click in the **Name** column and enter &quot;Id&quot;.</span></span>
8. <span data-ttu-id="f992d-169">**データ型**列で、 **int**します。</span><span class="sxs-lookup"><span data-stu-id="f992d-169">In the **Data Type** column, select **int**.</span></span>
9. <span data-ttu-id="f992d-170">設定、**プライマリ キーであるか?** と**を識別するでしょうか。** オプションを**はい**。</span><span class="sxs-lookup"><span data-stu-id="f992d-170">Set the **Is Primary Key?** and **Is Identify?** options to **Yes**.</span></span>

    <span data-ttu-id="f992d-171">名前からわかるように、**プライマリ キーである**設定により、データベース テーブルの主キーなります。</span><span class="sxs-lookup"><span data-stu-id="f992d-171">As the name suggests, **Is Primary Key** tells the database that this will be the table's primary key.</span></span> <span data-ttu-id="f992d-172">**[Is Identity]** 設定により、データベース (1 から始まります)、次の連続番号を割り当てると、すべての新しいレコードの ID 番号を自動的に作成します。</span><span class="sxs-lookup"><span data-stu-id="f992d-172">**Is Identity** tells the database to automatically create an ID number for every new record and to assign it the next sequential number (starting at 1).</span></span>
10. <span data-ttu-id="f992d-173">次の行をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f992d-173">Click in the next row.</span></span> <span data-ttu-id="f992d-174">エディターでは、新しい列定義を開始します。</span><span class="sxs-lookup"><span data-stu-id="f992d-174">The editor starts a new column definition.</span></span>
11. <span data-ttu-id="f992d-175">名前の値を入力&quot;名前&quot;します。</span><span class="sxs-lookup"><span data-stu-id="f992d-175">For the Name value, enter &quot;Name&quot;.</span></span>
12. <span data-ttu-id="f992d-176">**データ型**、選択&quot;nvarchar&quot;と長さを 50 に設定します。</span><span class="sxs-lookup"><span data-stu-id="f992d-176">For **Data Type**, choose &quot;nvarchar&quot; and set the length to 50.</span></span> <span data-ttu-id="f992d-177">*Var*一部分`nvarchar`データベースにこの列のデータ サイズは、レコード間を異なる場合があります文字列になることを指示します。</span><span class="sxs-lookup"><span data-stu-id="f992d-177">The *var* part of `nvarchar` tells the database that the data for this column will be a string whose size might vary from record to record.</span></span> <span data-ttu-id="f992d-178">(、 *N*プレフィックスの表す*national*フィールドがすべてアルファベットを表す文字データを保持できることを示す、または、書記&#8212;は、フィールドが Unicode データを保持している)。</span><span class="sxs-lookup"><span data-stu-id="f992d-178">(The *n* prefix represents *national*, indicating that the field can hold character data that represents any alphabet or writing system &#8212; that is, that the field holds Unicode data.)</span></span>
13. <span data-ttu-id="f992d-179">設定、 **Null を許容**オプションを**いいえ**します。</span><span class="sxs-lookup"><span data-stu-id="f992d-179">Set the **Allow Nulls** option to **No**.</span></span> <span data-ttu-id="f992d-180">これは、テンプレートを適用する、*名前*列がない空白のままにします。</span><span class="sxs-lookup"><span data-stu-id="f992d-180">This will enforce that the *Name* column is not left blank.</span></span>
14. <span data-ttu-id="f992d-181">という名前の列を作成するこの同じプロセスを使用して*説明*します。</span><span class="sxs-lookup"><span data-stu-id="f992d-181">Using this same process, create a column named *Description*.</span></span> <span data-ttu-id="f992d-182">設定**データ型**"nvarchar"の長さ、およびセット 50 を**Null を許容**を false にします。</span><span class="sxs-lookup"><span data-stu-id="f992d-182">Set **Data Type** to "nvarchar" and 50 for the length, and set **Allow Nulls** to false.</span></span>
15. <span data-ttu-id="f992d-183">という名前の列を作成する*価格*します。</span><span class="sxs-lookup"><span data-stu-id="f992d-183">Create a column named *Price*.</span></span> <span data-ttu-id="f992d-184">設定 **"money"データ型**設定と**Null を許容**を false にします。</span><span class="sxs-lookup"><span data-stu-id="f992d-184">Set **Data Type to "money"** and set **Allow Nulls** to false.</span></span>
16. <span data-ttu-id="f992d-185">上部にあるボックスで、テーブルの名前を&quot;製品&quot;します。</span><span class="sxs-lookup"><span data-stu-id="f992d-185">In the box at the top, name the table &quot;Product&quot;.</span></span>

    <span data-ttu-id="f992d-186">完了すると、定義は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="f992d-186">When you're done, the definition will look like this:</span></span>

    ![[イメージ]](5-working-with-data/_static/image2.jpg)
17. <span data-ttu-id="f992d-188">テーブルを保存するには、Ctrl + S キーを押します。</span><span class="sxs-lookup"><span data-stu-id="f992d-188">Press Ctrl+S to save the table.</span></span>

## <a name="adding-data-to-the-database"></a><span data-ttu-id="f992d-189">データベースへのデータの追加</span><span class="sxs-lookup"><span data-stu-id="f992d-189">Adding Data to the Database</span></span>

<span data-ttu-id="f992d-190">これで、記事の後半で使用するのデータベースにいくつかのサンプル データを追加できます。</span><span class="sxs-lookup"><span data-stu-id="f992d-190">Now you can add some sample data to your database that you'll work with later in the article.</span></span>

1. <span data-ttu-id="f992d-191">左側のウィンドウで、展開、 **SmallBakery.sdf**ノードをクリック**テーブル**します。</span><span class="sxs-lookup"><span data-stu-id="f992d-191">In the left pane, expand the **SmallBakery.sdf** node and then click **Tables**.</span></span>
2. <span data-ttu-id="f992d-192">Product テーブルを右クリックし、をクリックし、**データ**します。</span><span class="sxs-lookup"><span data-stu-id="f992d-192">Right-click the Product table and then click **Data**.</span></span>
3. <span data-ttu-id="f992d-193">[編集] ペインで、次のレコードを入力します。</span><span class="sxs-lookup"><span data-stu-id="f992d-193">In the edit pane, enter the following records:</span></span>

    | <span data-ttu-id="f992d-194">**Name**</span><span class="sxs-lookup"><span data-stu-id="f992d-194">**Name**</span></span> | <span data-ttu-id="f992d-195">**説明**</span><span class="sxs-lookup"><span data-stu-id="f992d-195">**Description**</span></span> | <span data-ttu-id="f992d-196">**価格**</span><span class="sxs-lookup"><span data-stu-id="f992d-196">**Price**</span></span> |
    | --- | --- | --- |
    | <span data-ttu-id="f992d-197">パン</span><span class="sxs-lookup"><span data-stu-id="f992d-197">Bread</span></span> | <span data-ttu-id="f992d-198">毎日斬新組み込まれています。</span><span class="sxs-lookup"><span data-stu-id="f992d-198">Baked fresh every day.</span></span> | <span data-ttu-id="f992d-199">2.99</span><span class="sxs-lookup"><span data-stu-id="f992d-199">2.99</span></span> |
    | <span data-ttu-id="f992d-200">ストロベリー ショート ケーキ</span><span class="sxs-lookup"><span data-stu-id="f992d-200">Strawberry Shortcake</span></span> | <span data-ttu-id="f992d-201">ガーデンから有機的な strawberries で行われます。</span><span class="sxs-lookup"><span data-stu-id="f992d-201">Made with organic strawberries from our garden.</span></span> | <span data-ttu-id="f992d-202">9.99</span><span class="sxs-lookup"><span data-stu-id="f992d-202">9.99</span></span> |
    | <span data-ttu-id="f992d-203">Apple の円</span><span class="sxs-lookup"><span data-stu-id="f992d-203">Apple Pie</span></span> | <span data-ttu-id="f992d-204">Mom の円グラフにのみ 2 番目です。</span><span class="sxs-lookup"><span data-stu-id="f992d-204">Second only to your mom's pie.</span></span> | <span data-ttu-id="f992d-205">12.99</span><span class="sxs-lookup"><span data-stu-id="f992d-205">12.99</span></span> |
    | <span data-ttu-id="f992d-206">Pecan 円</span><span class="sxs-lookup"><span data-stu-id="f992d-206">Pecan Pie</span></span> | <span data-ttu-id="f992d-207">Pecans 場合は、これは。</span><span class="sxs-lookup"><span data-stu-id="f992d-207">If you like pecans, this is for you.</span></span> | <span data-ttu-id="f992d-208">10.99</span><span class="sxs-lookup"><span data-stu-id="f992d-208">10.99</span></span> |
    | <span data-ttu-id="f992d-209">レモンの円</span><span class="sxs-lookup"><span data-stu-id="f992d-209">Lemon Pie</span></span> | <span data-ttu-id="f992d-210">世界中で最適な lemons で行われます。</span><span class="sxs-lookup"><span data-stu-id="f992d-210">Made with the best lemons in the world.</span></span> | <span data-ttu-id="f992d-211">11.99</span><span class="sxs-lookup"><span data-stu-id="f992d-211">11.99</span></span> |
    | <span data-ttu-id="f992d-212">Cupcakes</span><span class="sxs-lookup"><span data-stu-id="f992d-212">Cupcakes</span></span> | <span data-ttu-id="f992d-213">自分の子供とで kid は、これらでが大好きです。</span><span class="sxs-lookup"><span data-stu-id="f992d-213">Your kids and the kid in you will love these.</span></span> | <span data-ttu-id="f992d-214">7.99</span><span class="sxs-lookup"><span data-stu-id="f992d-214">7.99</span></span> |

    <span data-ttu-id="f992d-215">何も入力する必要はないことに注意してください、 *Id*列。</span><span class="sxs-lookup"><span data-stu-id="f992d-215">Remember that you don't have to enter anything for the *Id* column.</span></span> <span data-ttu-id="f992d-216">作成したときに、 *Id*列で、設定、 **Id**プロパティに自動的に入力するには、true をします。</span><span class="sxs-lookup"><span data-stu-id="f992d-216">When you created the *Id* column, you set its **Is Identity** property to true, which causes it to automatically be filled in.</span></span>

    <span data-ttu-id="f992d-217">データの入力が完了したら、テーブル デザイナーは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="f992d-217">When you're finished entering the data, the table designer will look like this:</span></span>

    ![[イメージ]](5-working-with-data/_static/image3.jpg)
4. <span data-ttu-id="f992d-219">データベースのデータを含むタブを閉じます。</span><span class="sxs-lookup"><span data-stu-id="f992d-219">Close the tab that contains the database data.</span></span>

## <a name="displaying-data-from-a-database"></a><span data-ttu-id="f992d-220">データベースからデータを表示します。</span><span class="sxs-lookup"><span data-stu-id="f992d-220">Displaying Data from a Database</span></span>

<span data-ttu-id="f992d-221">これで、データベースにデータをした後は、ASP.NET web ページで、データを表示できます。</span><span class="sxs-lookup"><span data-stu-id="f992d-221">Once you've got a database with data in it, you can display the data in an ASP.NET web page.</span></span> <span data-ttu-id="f992d-222">表示するテーブルの行を選択するには、データベースに渡すコマンドは、SQL ステートメントを使用します。</span><span class="sxs-lookup"><span data-stu-id="f992d-222">To select the table rows to display, you use a SQL statement, which is a command that you pass to the database.</span></span>

1. <span data-ttu-id="f992d-223">左側のウィンドウでをクリックして、**ファイル**ワークスペース。</span><span class="sxs-lookup"><span data-stu-id="f992d-223">In the left pane, click the **Files** workspace.</span></span>
2. <span data-ttu-id="f992d-224">Web サイトのルートに作成するという名前の新しい CSHTML ページ*ListProducts.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="f992d-224">In the root of the website, create a new CSHTML page named *ListProducts.cshtml*.</span></span>
3. <span data-ttu-id="f992d-225">次のように既存のマークアップに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="f992d-225">Replace the existing markup with the following:</span></span>

    [!code-cshtml[Main](5-working-with-data/samples/sample1.cshtml)]

    <span data-ttu-id="f992d-226">最初のコード ブロックで開く、 *SmallBakery.sdf*先ほど作成したファイル (データベース)。</span><span class="sxs-lookup"><span data-stu-id="f992d-226">In the first code block, you open the *SmallBakery.sdf* file (database) that you created earlier.</span></span> <span data-ttu-id="f992d-227">`Database.Open`メソッドを前提としていますが、 *.sdf*ファイルは、web サイトの*アプリ\_データ*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="f992d-227">The `Database.Open` method assumes that the *.sdf* file is in your website's *App\_Data* folder.</span></span> <span data-ttu-id="f992d-228">(を指定する必要はありませんが、 *.sdf*拡張子&#8212;実のところ、この場合、`Open`メソッドは機能しません)。</span><span class="sxs-lookup"><span data-stu-id="f992d-228">(Notice that you don't need to specify the *.sdf* extension &#8212; in fact, if you do, the `Open` method won't work.)</span></span>

    > [!NOTE]
    > <span data-ttu-id="f992d-229">*アプリ\_データ*データ ファイルを格納するために使用する asp.net の特別なフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="f992d-229">The *App\_Data* folder is a special folder in ASP.NET that's used to store data files.</span></span> <span data-ttu-id="f992d-230">詳細については、次を参照してください。[データベースに接続する](#SB_ConnectingToADatabase)この記事で後述します。</span><span class="sxs-lookup"><span data-stu-id="f992d-230">For more information, see [Connecting to a Database](#SB_ConnectingToADatabase) later in this article.</span></span>

    <span data-ttu-id="f992d-231">次の SQL を使用してデータベースを照会する要求を行うし`Select`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="f992d-231">You then make a request to query the database using the following SQL `Select` statement:</span></span>

    [!code-sql[Main](5-working-with-data/samples/sample2.sql)]

    <span data-ttu-id="f992d-232">ステートメントでは、`Product`クエリにテーブルを識別します。</span><span class="sxs-lookup"><span data-stu-id="f992d-232">In the statement, `Product` identifies the table to query.</span></span> <span data-ttu-id="f992d-233">`*`文字は、クエリがテーブルからすべての列を返す必要がありますを指定します。</span><span class="sxs-lookup"><span data-stu-id="f992d-233">The `*` character specifies that the query should return all the columns from the table.</span></span> <span data-ttu-id="f992d-234">(でしたもを一覧表示の列に個別に、一部の列のみを表示する場合は、コンマで区切っています。)`Order By`句では、データの並べ替え方法を示します&#8212;ここでは、によって、*名前*列。</span><span class="sxs-lookup"><span data-stu-id="f992d-234">(You could also list columns individually, separated by commas, if you wanted to see only some of the columns.) The `Order By` clause indicates how the data should be sorted &#8212; in this case, by the *Name* column.</span></span> <span data-ttu-id="f992d-235">これは、データの値に基づいてアルファベット順にソートされていることを意味、*名前*各行の列。</span><span class="sxs-lookup"><span data-stu-id="f992d-235">This means that the data is sorted alphabetically based on the value of the *Name* column for each row.</span></span>

    <span data-ttu-id="f992d-236">ページの本文には、マークアップは、データを表示するために使用される HTML テーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="f992d-236">In the body of the page, the markup creates an HTML table that will be used to display the data.</span></span> <span data-ttu-id="f992d-237">内で、`<tbody>`要素を使用する、`foreach`ループを使用して、クエリによって返されるデータ行ごとに個別に取得します。</span><span class="sxs-lookup"><span data-stu-id="f992d-237">Inside the `<tbody>` element, you use a `foreach` loop to individually get each data row that's returned by the query.</span></span> <span data-ttu-id="f992d-238">HTML テーブルの行を作成する各データ行 (`<tr>`要素)。</span><span class="sxs-lookup"><span data-stu-id="f992d-238">For each data row, you create an HTML table row (`<tr>` element).</span></span> <span data-ttu-id="f992d-239">HTML テーブルのセルを作成し、(`<td>`要素) の各列。</span><span class="sxs-lookup"><span data-stu-id="f992d-239">Then you create HTML table cells (`<td>` elements) for each column.</span></span> <span data-ttu-id="f992d-240">ループでアクセスするたび、データベースから、[次へ] の使用可能な行が、`row`変数 (に設定する、`foreach`ステートメント)。</span><span class="sxs-lookup"><span data-stu-id="f992d-240">Each time you go through the loop, the next available row from the database is in the `row` variable (you set this up in the `foreach` statement).</span></span> <span data-ttu-id="f992d-241">使用することができます、個々 の列行からを取得する`row.Name`または`row.Description`またはする列は、任意の名前。</span><span class="sxs-lookup"><span data-stu-id="f992d-241">To get an individual column from the row, you can use `row.Name` or `row.Description` or whatever the name is of the column you want.</span></span>
4. <span data-ttu-id="f992d-242">ブラウザーでページを実行します。</span><span class="sxs-lookup"><span data-stu-id="f992d-242">Run the page in a browser.</span></span> <span data-ttu-id="f992d-243">(内でページが選択されていることを確認、**ファイル**ワークスペースを実行する前にします)。ページには、次のような一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="f992d-243">(Make sure the page is selected in the **Files** workspace before you run it.) The page displays a list like the following:</span></span>

    ![[イメージ]](5-working-with-data/_static/image4.jpg)

> [!TIP] 
> 
> <span data-ttu-id="f992d-245">**構造化照会言語 (SQL)**</span><span class="sxs-lookup"><span data-stu-id="f992d-245">**Structured Query Language (SQL)**</span></span>
> 
> <span data-ttu-id="f992d-246">SQL は、データベース内のデータを管理するために、ほとんどのリレーショナル データベースで使用される言語です。</span><span class="sxs-lookup"><span data-stu-id="f992d-246">SQL is a language that's used in most relational databases for managing data in a database.</span></span> <span data-ttu-id="f992d-247">これには、データを取得し、それを更新できるようにして、作成、変更、およびデータベースのテーブルを管理するためのコマンドが含まれます。</span><span class="sxs-lookup"><span data-stu-id="f992d-247">It includes commands that let you retrieve data and update it, and that let you create, modify, and manage database tables.</span></span> <span data-ttu-id="f992d-248">SQL が (webmatrix を使用している 1 つ) などのプログラミング言語と異なるための考え方は、データベースを指示すること、データベースのジョブ、データの取得や、タスクを実行する方法を把握するは SQL とします。</span><span class="sxs-lookup"><span data-stu-id="f992d-248">SQL is different than a programming language (like the one you're using in WebMatrix) because with SQL, the idea is that you tell the database what you want, and it's the database's job to figure out how to get the data or perform the task.</span></span> <span data-ttu-id="f992d-249">一部の SQL コマンドの例とその実行内容を次に示します。</span><span class="sxs-lookup"><span data-stu-id="f992d-249">Here are examples of some SQL commands and what they do:</span></span>
> 
> `SELECT Id, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> <span data-ttu-id="f992d-250">これをフェッチ、 *Id*、*名前*、および*価格*列内のレコードから、*製品*場合のテーブルの値*価格* 10 以上の値に基づいてアルファベット順で結果を返す、*名前*列。</span><span class="sxs-lookup"><span data-stu-id="f992d-250">This fetches the *Id*, *Name*, and *Price* columns from records in the *Product* table if the value of *Price* is more than 10, and returns the results in alphabetical order based on the values of the *Name* column.</span></span> <span data-ttu-id="f992d-251">このコマンドでは、レコードが一致しない場合、条件、または空のセットに一致するレコードを含む結果セットを返します。</span><span class="sxs-lookup"><span data-stu-id="f992d-251">This command will return a result set that contains the records that meet the criteria, or an empty set if no records match.</span></span>
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ("Croissant", "A flaky delight", 1.99)`
> 
> <span data-ttu-id="f992d-252">これに新しいレコードが挿入、*製品*テーブル、設定、*名前*列を&quot;クロワッサン&quot;、*説明*列&quot;不安定な満足&quot;とから 1.99 する価格。</span><span class="sxs-lookup"><span data-stu-id="f992d-252">This inserts a new record into the *Product* table, setting the *Name* column to &quot;Croissant&quot;, the *Description* column to &quot;A flaky delight&quot;, and the price to 1.99.</span></span>
> 
> `DELETE FROM Product WHERE ExpirationDate < "01/01/2008"`
> 
> <span data-ttu-id="f992d-253">このコマンドのレコードの削除、*製品*が 2008 年 1 月 1 日より前で有効期限の日付列が含まれるテーブル。</span><span class="sxs-lookup"><span data-stu-id="f992d-253">This command deletes records in the *Product* table whose expiration date column is earlier than January 1, 2008.</span></span> <span data-ttu-id="f992d-254">(これで、*製品*テーブルにはこのような列は、もちろん)。/MM/DD/YYYY の形式で日付がここに入力したが、現在のロケールに使用される形式で入力してください。</span><span class="sxs-lookup"><span data-stu-id="f992d-254">(This assumes that the *Product* table has such a column, of course.) The date is entered here in MM/DD/YYYY format, but it should be entered in the format that's used for your locale.</span></span>
> 
> <span data-ttu-id="f992d-255">`Insert Into`と`Delete`コマンドの結果セットを返しません。</span><span class="sxs-lookup"><span data-stu-id="f992d-255">The `Insert Into` and `Delete` commands don't return result sets.</span></span> <span data-ttu-id="f992d-256">代わりに、コマンドの影響を受けたレコードの数を示す数値が返されます。</span><span class="sxs-lookup"><span data-stu-id="f992d-256">Instead, they return a number that tells you how many records were affected by the command.</span></span>
> 
> <span data-ttu-id="f992d-257">これらの操作 (挿入や、レコードを削除する) などの一部は、操作を要求しているプロセスは、データベース内に適切なアクセス許可があります。</span><span class="sxs-lookup"><span data-stu-id="f992d-257">For some of these operations (like inserting and deleting records), the process that's requesting the operation has to have appropriate permissions in the database.</span></span> <span data-ttu-id="f992d-258">これは、実稼働データベースの多くの場合、データベースに接続するときに、ユーザー名とパスワードを指定する必要がある理由です。</span><span class="sxs-lookup"><span data-stu-id="f992d-258">This is why for production databases you often have to supply a username and password when you connect to the database.</span></span>
> 
> <span data-ttu-id="f992d-259">数十個の SQL コマンドがありますが、このようなパターンに従うすべて。</span><span class="sxs-lookup"><span data-stu-id="f992d-259">There are dozens of SQL commands, but they all follow a pattern like this.</span></span> <span data-ttu-id="f992d-260">SQL コマンドを使用して、データベース テーブルを作成、テーブル内のレコードの数をカウント、価格、計算、および多くの操作を実行することができます。</span><span class="sxs-lookup"><span data-stu-id="f992d-260">You can use SQL commands to create database tables, count the number of records in a table, calculate prices, and perform many more operations.</span></span>


## <a name="inserting-data-in-a-database"></a><span data-ttu-id="f992d-261">データベースでデータを挿入</span><span class="sxs-lookup"><span data-stu-id="f992d-261">Inserting Data in a Database</span></span>

<span data-ttu-id="f992d-262">このセクションでは、ユーザーが新しい製品を追加できるページを作成する方法を示しています、*製品*データベース テーブル。</span><span class="sxs-lookup"><span data-stu-id="f992d-262">This section shows how to create a page that lets users add a new product to the *Product* database table.</span></span> <span data-ttu-id="f992d-263">更新されたテーブルを使用して、新しい製品レコードが挿入されると、ページに表示されます、 *ListProducts.cshtml*前のセクションで作成したページです。</span><span class="sxs-lookup"><span data-stu-id="f992d-263">After a new product record is inserted, the page displays the updated table using the *ListProducts.cshtml* page that you created in the previous section.</span></span>

<span data-ttu-id="f992d-264">ページには、ユーザーが入力したデータが、データベースに対して有効であるかどうかを確認する検証が含まれます。</span><span class="sxs-lookup"><span data-stu-id="f992d-264">The page includes validation to make sure that the data that the user enters is valid for the database.</span></span> <span data-ttu-id="f992d-265">たとえば、ページ内のコードは、必要なすべての列の値が入力されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="f992d-265">For example, code in the page makes sure that a value has been entered for all required columns.</span></span>

1. <span data-ttu-id="f992d-266">という名前の新しい CSHTML ファイルを作成、web サイトで*InsertProducts.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="f992d-266">In the website, create a new CSHTML file named *InsertProducts.cshtml*.</span></span>
2. <span data-ttu-id="f992d-267">次のように既存のマークアップに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="f992d-267">Replace the existing markup with the following:</span></span>

    [!code-cshtml[Main](5-working-with-data/samples/sample3.cshtml)]

    <span data-ttu-id="f992d-268">ページの本文には、ユーザー名、説明、および価格を入力できるようにする 3 つのテキスト ボックスに HTML フォームが含まれています。</span><span class="sxs-lookup"><span data-stu-id="f992d-268">The body of the page contains an HTML form with three text boxes that let users enter a name, description, and price.</span></span> <span data-ttu-id="f992d-269">ユーザーがクリックすると、**挿入**ボタン、ページの上部にあるコードへの接続が表示されます、 *SmallBakery.sdf*データベース。</span><span class="sxs-lookup"><span data-stu-id="f992d-269">When users click the **Insert** button, the code at the top of the page opens a connection to the *SmallBakery.sdf* database.</span></span> <span data-ttu-id="f992d-270">使用して、ユーザーが送信される値を取得、`Request`オブジェクトし、それらの値をローカル変数に割り当てます。</span><span class="sxs-lookup"><span data-stu-id="f992d-270">You then get the values that the user has submitted by using the `Request` object and assign those values to local variables.</span></span>

    <span data-ttu-id="f992d-271">各ユーザーが必要な列の値を入力することを検証するには、登録する`<input>`検証する要素。</span><span class="sxs-lookup"><span data-stu-id="f992d-271">To validate that the user entered a value for each required column, you register each `<input>` element that you want to validate:</span></span>

    [!code-csharp[Main](5-working-with-data/samples/sample4.cs)]

    <span data-ttu-id="f992d-272">`Validation`ヘルパーは、各登録したフィールドの値があることを確認します。</span><span class="sxs-lookup"><span data-stu-id="f992d-272">The `Validation` helper checks that there is a value in each of the fields that you've registered.</span></span> <span data-ttu-id="f992d-273">すべてのフィールドを確認して検証に合格するかどうかをテストする`Validation.IsValid()`、通常は、ユーザーから取得した情報を処理する前に。</span><span class="sxs-lookup"><span data-stu-id="f992d-273">You can test whether all the fields passed validation by checking `Validation.IsValid()`, which you typically do before you process the information you get from the user:</span></span>

    [!code-csharp[Main](5-working-with-data/samples/sample5.cs)]

    <span data-ttu-id="f992d-274">(、`&&`演算子は AND — このテストが*フォームの送信接続が、すべてのフィールドが検証に合格*)。</span><span class="sxs-lookup"><span data-stu-id="f992d-274">(The `&&` operator means AND — this test is *If this is a form submission AND all the fields have passed validation*.)</span></span>

    <span data-ttu-id="f992d-275">すべての列を検証する場合 (なかった空)、データを挿入し、次に示すように実行する SQL ステートメントを作成してください。</span><span class="sxs-lookup"><span data-stu-id="f992d-275">If all the columns validated (none were empty), you go ahead and create a SQL statement to insert the data and then execute it as shown next:</span></span>

    [!code-csharp[Main](5-working-with-data/samples/sample6.cs)]

    <span data-ttu-id="f992d-276">値を挿入するには、パラメーターのプレース ホルダーが含まれます。 (`@0`、 `@1`、 `@2`)。</span><span class="sxs-lookup"><span data-stu-id="f992d-276">For the values to insert, you include parameter placeholders (`@0`, `@1`, `@2`).</span></span>

    > [!NOTE]
    > <span data-ttu-id="f992d-277">セキュリティの予防措置として常に値を渡すパラメーターを使用して SQL ステートメントにように前の例を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f992d-277">As a security precaution, always pass values to a SQL statement using parameters, as you see in the preceding example.</span></span> <span data-ttu-id="f992d-278">これにより、ユーザーのデータを検証することと、データベース (SQL インジェクション攻撃とも呼ばれます) に悪意のあるコマンドの送信を防ぐのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="f992d-278">This gives you a chance to validate the user's data, plus it helps protect against attempts to send malicious commands to your database (sometimes referred to as SQL injection attacks).</span></span>

    <span data-ttu-id="f992d-279">クエリを実行するには、ステートメントを使用するこのプレース ホルダーの代わりに値を格納する変数を渡します。</span><span class="sxs-lookup"><span data-stu-id="f992d-279">To execute the query, you use this statement, passing to it the variables that contain the values to substitute for the placeholders:</span></span>

    [!code-csharp[Main](5-working-with-data/samples/sample7.cs)]

    <span data-ttu-id="f992d-280">後に、`Insert Into`ステートメントが実行される、この行を使用して製品を一覧表示されたページにユーザーを送信します。</span><span class="sxs-lookup"><span data-stu-id="f992d-280">After the `Insert Into` statement has executed, you send the user to the page that lists the products using this line:</span></span>

    [!code-javascript[Main](5-working-with-data/samples/sample8.js)]

    <span data-ttu-id="f992d-281">検証に成功しなかった場合は、挿入をスキップします。</span><span class="sxs-lookup"><span data-stu-id="f992d-281">If validation didn't succeed, you skip the insert.</span></span> <span data-ttu-id="f992d-282">代わりに、蓄積されたエラー メッセージ (あれば) を表示できるページに、ヘルパーがあります。</span><span class="sxs-lookup"><span data-stu-id="f992d-282">Instead, you have a helper in the page that can display the accumulated error messages (if any):</span></span>

    [!code-cshtml[Main](5-working-with-data/samples/sample9.cshtml)]

    <span data-ttu-id="f992d-283">マークアップにスタイル ブロックがという名前の CSS クラス定義が含まれることに注意してください`.validation-summary-errors`します。</span><span class="sxs-lookup"><span data-stu-id="f992d-283">Notice that the style block in the markup includes a CSS class definition named `.validation-summary-errors`.</span></span> <span data-ttu-id="f992d-284">これは、既定で使用される CSS クラスの名前、`<div>`検証エラーを含む要素。</span><span class="sxs-lookup"><span data-stu-id="f992d-284">This is the name of the CSS class that's used by default for the `<div>` element that contains any validation errors.</span></span> <span data-ttu-id="f992d-285">この場合は、CSS クラスすることを指定検証概要エラーが赤で表示され、太字で、定義することができます、`.validation-summary-errors`たい書式を表示するクラス。</span><span class="sxs-lookup"><span data-stu-id="f992d-285">In this case, the CSS class specifies that validation summary errors are displayed in red and in bold, but you can define the `.validation-summary-errors` class to display any formatting you like.</span></span>

### <a name="testing-the-insert-page"></a><span data-ttu-id="f992d-286">Insert ページのテスト</span><span class="sxs-lookup"><span data-stu-id="f992d-286">Testing the Insert Page</span></span>

1. <span data-ttu-id="f992d-287">ブラウザーでページを表示します。</span><span class="sxs-lookup"><span data-stu-id="f992d-287">View the page in a browser.</span></span> <span data-ttu-id="f992d-288">次の図に示すようなフォームを表示します。</span><span class="sxs-lookup"><span data-stu-id="f992d-288">The page displays a form that's similar to the one that's shown in the following illustration.</span></span>

    ![[イメージ]](5-working-with-data/_static/image5.jpg)
2. <span data-ttu-id="f992d-290">すべての列の値を入力しますが、しておくことを確認、*価格*列は空白です。</span><span class="sxs-lookup"><span data-stu-id="f992d-290">Enter values for all the columns, but make sure that you leave the *Price* column blank.</span></span>
3. <span data-ttu-id="f992d-291">**[挿入]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f992d-291">Click **Insert**.</span></span> <span data-ttu-id="f992d-292">ページでは、次の図に示すように、エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f992d-292">The page displays an error message, as shown in the following illustration.</span></span> <span data-ttu-id="f992d-293">(新しいレコードは作成されません。)</span><span class="sxs-lookup"><span data-stu-id="f992d-293">(No new record is created.)</span></span>

    ![[イメージ]](5-working-with-data/_static/image6.jpg)
4. <span data-ttu-id="f992d-295">フォームに完全に入力し、**挿入**します。</span><span class="sxs-lookup"><span data-stu-id="f992d-295">Fill the form out completely, and then click **Insert**.</span></span> <span data-ttu-id="f992d-296">今回は、 *ListProducts.cshtml*ページが表示され、新しいレコードを示しています。</span><span class="sxs-lookup"><span data-stu-id="f992d-296">This time, the *ListProducts.cshtml* page is displayed and shows the new record.</span></span>

## <a name="updating-data-in-a-database"></a><span data-ttu-id="f992d-297">データベース内のデータを更新しています</span><span class="sxs-lookup"><span data-stu-id="f992d-297">Updating Data in a Database</span></span>

<span data-ttu-id="f992d-298">テーブルにデータを入力すると、更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f992d-298">After data has been entered into a table, you might need to update it.</span></span> <span data-ttu-id="f992d-299">この手順では、データの挿入前に作成したものに類似した 2 つのページを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="f992d-299">This procedure shows you how to create two pages that are similar to the ones you created for data insertion earlier.</span></span> <span data-ttu-id="f992d-300">最初のページでは、製品を表示し、ユーザーを変更する 1 つを選択します。</span><span class="sxs-lookup"><span data-stu-id="f992d-300">The first page displays products and lets users select one to change.</span></span> <span data-ttu-id="f992d-301">2 番目のページには、実際には、編集し、保存、ユーザーことができます。</span><span class="sxs-lookup"><span data-stu-id="f992d-301">The second page lets the users actually make the edits and save them.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="f992d-302">**重要な**を運用環境の web サイトを通常できるユーザーを制限、データを変更します。</span><span class="sxs-lookup"><span data-stu-id="f992d-302">**Important** In a production website, you typically restrict who's allowed to make changes to the data.</span></span> <span data-ttu-id="f992d-303">サイトのタスクを実行するユーザーを承認する方法についてとメンバーシップを設定する方法については、次を参照してください。[追加のセキュリティと ASP.NET Web ページ サイトには、メンバーシップ](https://go.microsoft.com/fwlink/?LinkId=202904)します。</span><span class="sxs-lookup"><span data-stu-id="f992d-303">For information about how to set up membership and about ways to authorize users to perform tasks on the site, see [Adding Security and Membership to an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=202904).</span></span>


1. <span data-ttu-id="f992d-304">という名前の新しい CSHTML ファイルを作成、web サイトで*EditProducts.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="f992d-304">In the website, create a new CSHTML file named *EditProducts.cshtml*.</span></span>
2. <span data-ttu-id="f992d-305">次のように、ファイルの既存のマークアップに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="f992d-305">Replace the existing markup in the file with the following:</span></span>

    [!code-cshtml[Main](5-working-with-data/samples/sample10.cshtml)]

    <span data-ttu-id="f992d-306">このページの唯一の違い、 *ListProducts.cshtml*ページから前は、このページの HTML テーブルに表示する追加の列が含まれている、**編集**リンク。</span><span class="sxs-lookup"><span data-stu-id="f992d-306">The only difference between this page and the *ListProducts.cshtml* page from earlier is that the HTML table in this page includes an extra column that displays an **Edit** link.</span></span> <span data-ttu-id="f992d-307">かかるこのリンクをクリックすると、 *UpdateProducts.cshtml*ページ (次に作成します) が選択されているレコードを編集することができます。</span><span class="sxs-lookup"><span data-stu-id="f992d-307">When you click this link, it takes you to the *UpdateProducts.cshtml* page (which you'll create next) where you can edit the selected record.</span></span>

    <span data-ttu-id="f992d-308">作成するコードを見て、**編集**リンク。</span><span class="sxs-lookup"><span data-stu-id="f992d-308">Look at the code that creates the **Edit** link:</span></span>

    [!code-cshtml[Main](5-working-with-data/samples/sample11.cshtml)]

    <span data-ttu-id="f992d-309">これは、HTML を作成します。`<a>`要素が`href`属性が動的に設定します。</span><span class="sxs-lookup"><span data-stu-id="f992d-309">This creates an HTML `<a>` element whose `href` attribute is set dynamically.</span></span> <span data-ttu-id="f992d-310">`href`属性は、ユーザーがリンクをクリックしたときに表示するページを指定します。</span><span class="sxs-lookup"><span data-stu-id="f992d-310">The `href` attribute specifies the page to display when the user clicks the link.</span></span> <span data-ttu-id="f992d-311">また渡します、`Id`のリンクを現在の行の値。</span><span class="sxs-lookup"><span data-stu-id="f992d-311">It also passes the `Id` value of the current row to the link.</span></span> <span data-ttu-id="f992d-312">ページを実行すると、ページのソースには、上記のようなリンクが含まれます。</span><span class="sxs-lookup"><span data-stu-id="f992d-312">When the page runs, the page source might contain links like these:</span></span>

    [!code-html[Main](5-working-with-data/samples/sample12.html)]

    <span data-ttu-id="f992d-313">注意、`href`属性に設定されて`UpdateProducts/n`ここで、 *n*は製品の数です。</span><span class="sxs-lookup"><span data-stu-id="f992d-313">Notice that the `href` attribute is set to `UpdateProducts/n`, where *n* is a product number.</span></span> <span data-ttu-id="f992d-314">ユーザーには、これらのリンクをクリックすると、結果の URL は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="f992d-314">When a user clicks one of these links, the resulting URL will look something like this:</span></span>

    `http://localhost:18816/UpdateProducts/6`

    <span data-ttu-id="f992d-315">つまり、製品番号の編集は、URL で渡されます。</span><span class="sxs-lookup"><span data-stu-id="f992d-315">In other words, the product number to be edited will be passed in the URL.</span></span>
3. <span data-ttu-id="f992d-316">ブラウザーでページを表示します。</span><span class="sxs-lookup"><span data-stu-id="f992d-316">View the page in a browser.</span></span> <span data-ttu-id="f992d-317">ページには、このような形式で、データが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f992d-317">The page displays the data in a format like this:</span></span>

    ![[イメージ]](5-working-with-data/_static/image7.jpg)

    <span data-ttu-id="f992d-319">次に、ユーザーが実際には、データを更新できるページを作成します。</span><span class="sxs-lookup"><span data-stu-id="f992d-319">Next, you'll create the page that lets users actually update the data.</span></span> <span data-ttu-id="f992d-320">更新プログラム ページには、ユーザーが入力したデータを検証する検証が含まれています。</span><span class="sxs-lookup"><span data-stu-id="f992d-320">The update page includes validation to validate the data that the user enters.</span></span> <span data-ttu-id="f992d-321">たとえば、ページ内のコードは、必要なすべての列の値が入力されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="f992d-321">For example, code in the page makes sure that a value has been entered for all required columns.</span></span>
4. <span data-ttu-id="f992d-322">という名前の新しい CSHTML ファイルを作成、web サイトで*UpdateProducts.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="f992d-322">In the website, create a new CSHTML file named *UpdateProducts.cshtml*.</span></span>
5. <span data-ttu-id="f992d-323">次のように、ファイルの既存のマークアップを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="f992d-323">Replace the existing markup in the file with the following.</span></span>

    [!code-cshtml[Main](5-working-with-data/samples/sample13.cshtml)]

    <span data-ttu-id="f992d-324">ページの本文には、製品が表示され、ユーザーが編集できる HTML フォームが含まれています。</span><span class="sxs-lookup"><span data-stu-id="f992d-324">The body of the page contains an HTML form where a product is displayed and where users can edit it.</span></span> <span data-ttu-id="f992d-325">表示する製品を取得するには、この SQL ステートメントを使用します。</span><span class="sxs-lookup"><span data-stu-id="f992d-325">To get the product to display, you use this SQL statement:</span></span>

    [!code-sql[Main](5-working-with-data/samples/sample14.sql)]

    <span data-ttu-id="f992d-326">これにより、ID が渡される値に一致する製品を選択、`@0`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="f992d-326">This will select the product whose ID matches the value that's passed in the `@0` parameter.</span></span> <span data-ttu-id="f992d-327">(ため*Id*は主キーとそのため必要がありますで一意である、1 つの製品レコードがこの方法を選択するこれまでことができます)。これに渡す ID 値を取得する`Select`ステートメントに渡されるページの URL の一部として、次の構文を使用して値を読み取ることができます。</span><span class="sxs-lookup"><span data-stu-id="f992d-327">(Because *Id* is the primary key and therefore must be unique, only one product record can ever be selected this way.) To get the ID value to pass to this `Select` statement, you can read the value that's passed to the page as part of the URL, using the following syntax:</span></span>

    [!code-csharp[Main](5-working-with-data/samples/sample15.cs)]

    <span data-ttu-id="f992d-328">製品レコードを実際にフェッチするを使用する、`QuerySingle`メソッドは、1 つのレコードが返されます。</span><span class="sxs-lookup"><span data-stu-id="f992d-328">To actually fetch the product record, you use the `QuerySingle` method, which will return just one record:</span></span>

    [!code-csharp[Main](5-working-with-data/samples/sample16.cs)]

    <span data-ttu-id="f992d-329">1 つの行が返されます、`row`変数。</span><span class="sxs-lookup"><span data-stu-id="f992d-329">The single row is returned into the `row` variable.</span></span> <span data-ttu-id="f992d-330">各列からデータを取得し、次のようにローカル変数に割り当てることができます。</span><span class="sxs-lookup"><span data-stu-id="f992d-330">You can get data out of each column and assign it to local variables like this:</span></span>

    [!code-csharp[Main](5-working-with-data/samples/sample17.cs)]

    <span data-ttu-id="f992d-331">フォームのマークアップでは、これらの値は、次のような埋め込みコードを使用して、個々 のテキスト ボックスで自動的に表示します。</span><span class="sxs-lookup"><span data-stu-id="f992d-331">In the markup for the form, these values are displayed automatically in individual text boxes by using embedded code like the following:</span></span>

    [!code-html[Main](5-working-with-data/samples/sample18.html)]

    <span data-ttu-id="f992d-332">コードの部分には、更新する製品レコードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f992d-332">That part of the code displays the product record to be updated.</span></span> <span data-ttu-id="f992d-333">レコードが表示された後、ユーザーは、個々 の列を編集できます。</span><span class="sxs-lookup"><span data-stu-id="f992d-333">Once the record has been displayed, the user can edit individual columns.</span></span>

    <span data-ttu-id="f992d-334">クリックして、ユーザーがフォームを送信するときに、**更新**コードでは、ボタン、`if(IsPost)`ブロックが実行されます。</span><span class="sxs-lookup"><span data-stu-id="f992d-334">When the user submits the form by clicking the **Update** button, the code in the `if(IsPost)` block runs.</span></span> <span data-ttu-id="f992d-335">これから、ユーザーの値を取得、`Request`オブジェクトは、変数値を格納し各列が入力されていることを検証します。</span><span class="sxs-lookup"><span data-stu-id="f992d-335">This gets the user's values from the `Request` object, stores the values in variables, and validates that each column has been filled in.</span></span> <span data-ttu-id="f992d-336">検証に合格した場合、コードは、次の SQL の Update ステートメントを作成します。</span><span class="sxs-lookup"><span data-stu-id="f992d-336">If validation passes, the code creates the following SQL Update statement:</span></span>

    [!code-sql[Main](5-working-with-data/samples/sample19.sql)]

    <span data-ttu-id="f992d-337">SQL で`Update`ステートメントに設定する値で更新するには、各列を指定します。</span><span class="sxs-lookup"><span data-stu-id="f992d-337">In a SQL `Update` statement, you specify each column to update and the value to set it to.</span></span> <span data-ttu-id="f992d-338">このコードで、値はパラメーターのプレース ホルダーを使用して`@0`、 `@1`、`@2`など。</span><span class="sxs-lookup"><span data-stu-id="f992d-338">In this code, the values are specified using the parameter placeholders `@0`, `@1`, `@2`, and so on.</span></span> <span data-ttu-id="f992d-339">(セキュリティのため、前述したようを渡す必要があります常に値を SQL ステートメントにパラメーターを使用しています。)</span><span class="sxs-lookup"><span data-stu-id="f992d-339">(As noted earlier, for security, you should always pass values to a SQL statement by using parameters.)</span></span>

    <span data-ttu-id="f992d-340">呼び出すと、`db.Execute`メソッドでは、SQL ステートメント内のパラメーターに対応する順序で値を格納する変数を渡します。</span><span class="sxs-lookup"><span data-stu-id="f992d-340">When you call the `db.Execute` method, you pass the variables that contain the values in the order that corresponds to the parameters in the SQL statement:</span></span>

    [!code-csharp[Main](5-working-with-data/samples/sample20.cs)]

    <span data-ttu-id="f992d-341">後に、`Update`ステートメントが実行された、ユーザーを編集 ページにリダイレクトするには、次のメソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="f992d-341">After the `Update` statement has been executed, you call the following method in order to redirect the user back to the edit page:</span></span>

    [!code-cshtml[Main](5-working-with-data/samples/sample21.cshtml)]

    <span data-ttu-id="f992d-342">効果は、ユーザーがデータベース内のデータの更新の一覧が表示し、別の製品を編集することができます。</span><span class="sxs-lookup"><span data-stu-id="f992d-342">The effect is that the user sees an updated listing of the data in the database and can edit another product.</span></span>
6. <span data-ttu-id="f992d-343">ページを保存します。</span><span class="sxs-lookup"><span data-stu-id="f992d-343">Save the page.</span></span>
7. <span data-ttu-id="f992d-344">実行、 *EditProducts.cshtml*ページ (更新プログラム ページ) をクリック**編集**を編集する製品を選択します。</span><span class="sxs-lookup"><span data-stu-id="f992d-344">Run the *EditProducts.cshtml* page (not the update page) and then click **Edit** to select a product to edit.</span></span> <span data-ttu-id="f992d-345">*UpdateProducts.cshtml*ページを表示すると、選択したレコードを表示します。</span><span class="sxs-lookup"><span data-stu-id="f992d-345">The *UpdateProducts.cshtml* page is displayed, showing the record you selected.</span></span>

    ![[イメージ]](5-working-with-data/_static/image8.jpg)
8. <span data-ttu-id="f992d-347">変更を加えるし、クリックして**Update**します。</span><span class="sxs-lookup"><span data-stu-id="f992d-347">Make a change and click **Update**.</span></span> <span data-ttu-id="f992d-348">製品一覧が更新されたデータをもう一度表示されます。</span><span class="sxs-lookup"><span data-stu-id="f992d-348">The products list is shown again with your updated data.</span></span>

## <a name="deleting-data-in-a-database"></a><span data-ttu-id="f992d-349">データベース内のデータを削除します。</span><span class="sxs-lookup"><span data-stu-id="f992d-349">Deleting Data in a Database</span></span>

<span data-ttu-id="f992d-350">このセクションではユーザーからの製品を削除できるようにする方法、*製品*データベース テーブル。</span><span class="sxs-lookup"><span data-stu-id="f992d-350">This section shows how to let users delete a product from the *Product* database table.</span></span> <span data-ttu-id="f992d-351">この例は、2 つのページで構成されます。</span><span class="sxs-lookup"><span data-stu-id="f992d-351">The example consists of two pages.</span></span> <span data-ttu-id="f992d-352">最初のページでは、ユーザーは、削除するレコードを選択します。</span><span class="sxs-lookup"><span data-stu-id="f992d-352">In the first page, users select a record to delete.</span></span> <span data-ttu-id="f992d-353">レコードを削除するレコードを削除することを確認することができます、2 番目のページに表示されます。</span><span class="sxs-lookup"><span data-stu-id="f992d-353">The record to be deleted is then displayed in a second page that lets them confirm that they want to delete the record.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="f992d-354">**重要な**を運用環境の web サイトを通常できるユーザーを制限、データを変更します。</span><span class="sxs-lookup"><span data-stu-id="f992d-354">**Important** In a production website, you typically restrict who's allowed to make changes to the data.</span></span> <span data-ttu-id="f992d-355">サイトのタスクを実行するユーザーを承認する方法についてとメンバーシップを設定する方法については、次を参照してください。[追加のセキュリティと ASP.NET Web ページ サイトには、メンバーシップ](https://go.microsoft.com/fwlink/?LinkId=202904)します。</span><span class="sxs-lookup"><span data-stu-id="f992d-355">For information about how to set up membership and about ways to authorize user to perform tasks on the site, see [Adding Security and Membership to an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=202904).</span></span>


1. <span data-ttu-id="f992d-356">という名前の新しい CSHTML ファイルを作成、web サイトで*ListProductsForDelete.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="f992d-356">In the website, create a new CSHTML file named *ListProductsForDelete.cshtml*.</span></span>
2. <span data-ttu-id="f992d-357">次のように既存のマークアップに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="f992d-357">Replace the existing markup with the following:</span></span>

    [!code-cshtml[Main](5-working-with-data/samples/sample22.cshtml)]

    <span data-ttu-id="f992d-358">このページと似ています、 *EditProducts.cshtml*前のページ。</span><span class="sxs-lookup"><span data-stu-id="f992d-358">This page is similar to the *EditProducts.cshtml* page from earlier.</span></span> <span data-ttu-id="f992d-359">表示する代わりに、ただし、**編集**リンク、各製品が表示されます、**削除**リンク。</span><span class="sxs-lookup"><span data-stu-id="f992d-359">However, instead of displaying an **Edit** link for each product, it displays a **Delete** link.</span></span> <span data-ttu-id="f992d-360">**削除**マークアップに埋め込まれた次のコードを使用してリンクを作成します。</span><span class="sxs-lookup"><span data-stu-id="f992d-360">The **Delete** link is created using the following embedded code in the markup:</span></span>

    [!code-cshtml[Main](5-working-with-data/samples/sample23.cshtml)]

    <span data-ttu-id="f992d-361">これには、ユーザーがリンクをクリックすると次のような URL が作成されます。</span><span class="sxs-lookup"><span data-stu-id="f992d-361">This creates a URL that looks like this when users click the link:</span></span>

    `http://<server>/DeleteProduct/4`

    <span data-ttu-id="f992d-362">URL がという名前のページを呼び出す*DeleteProduct.cshtml* (これは、次に作成します) を削除する、製品の ID を渡します (ここで、4)。</span><span class="sxs-lookup"><span data-stu-id="f992d-362">The URL calls a page named *DeleteProduct.cshtml* (which you'll create next) and passes it the ID of the product to delete (here, 4).</span></span>
3. <span data-ttu-id="f992d-363">ファイルを保存しますが、開いたままにします。</span><span class="sxs-lookup"><span data-stu-id="f992d-363">Save the file, but leave it open.</span></span>
4. <span data-ttu-id="f992d-364">という名前の別の CHTML ファイル作成*DeleteProduct.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="f992d-364">Create another CHTML file named *DeleteProduct.cshtml*.</span></span> <span data-ttu-id="f992d-365">次のように、既存のコンテンツを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="f992d-365">Replace the existing content with the following:</span></span>

    [!code-cshtml[Main](5-working-with-data/samples/sample24.cshtml)]

    <span data-ttu-id="f992d-366">このページは、によって呼び出されます。 *ListProductsForDelete.cshtml*でき、ユーザーが製品を削除することを確認します。</span><span class="sxs-lookup"><span data-stu-id="f992d-366">This page is called by *ListProductsForDelete.cshtml* and lets users confirm that they want to delete a product.</span></span> <span data-ttu-id="f992d-367">削除する製品を一覧表示するには、ID を入手する、製品の URL から削除する次のコードを使用します。</span><span class="sxs-lookup"><span data-stu-id="f992d-367">To list the product to be deleted, you get the ID of the product to delete from the URL using the following code:</span></span>

    [!code-csharp[Main](5-working-with-data/samples/sample25.cs)]

    <span data-ttu-id="f992d-368">ページには、実際にレコードを削除するボタンをクリックするユーザーが求められます。</span><span class="sxs-lookup"><span data-stu-id="f992d-368">The page then asks the user to click a button to actually delete the record.</span></span> <span data-ttu-id="f992d-369">これは、重要なセキュリティ対策: これらの操作を行う常に更新またはデータを削除するように、web サイトで機密情報の操作を実行すると POST 操作で GET 操作ではありません。</span><span class="sxs-lookup"><span data-stu-id="f992d-369">This is an important security measure: when you perform sensitive operations in your website like updating or deleting data, these operations should always be done using a POST operation, not a GET operation.</span></span> <span data-ttu-id="f992d-370">GET 操作を使用して削除操作を実行できるように、サイトを設定する、すべてのユーザーとのような URL を渡すことが`http://<server>/DeleteProduct/4`をデータベースから削除する必要なものとします。</span><span class="sxs-lookup"><span data-stu-id="f992d-370">If your site is set up so that a delete operation can be performed using a GET operation, anyone can pass a URL like `http://<server>/DeleteProduct/4` and delete anything they want from your database.</span></span> <span data-ttu-id="f992d-371">確認を追加して、削除は、POST を使用してのみ実行できるように、ページのコーディングで、サイト セキュリティのメジャーを追加します。</span><span class="sxs-lookup"><span data-stu-id="f992d-371">By adding the confirmation and coding the page so that the deletion can be performed only by using a POST, you add a measure of security to your site.</span></span>

    <span data-ttu-id="f992d-372">実際の削除操作は、まず post 操作であるし、ID が空でないことを確認する次のコードを使用して実行されます。</span><span class="sxs-lookup"><span data-stu-id="f992d-372">The actual delete operation is performed using the following code, which first confirms that this is a post operation and that the ID isn't empty:</span></span>

    [!code-csharp[Main](5-working-with-data/samples/sample26.cs)]

    <span data-ttu-id="f992d-373">コードでは、指定されたレコードを削除し、一覧ページに戻るユーザーをリダイレクトする SQL ステートメントを実行します。</span><span class="sxs-lookup"><span data-stu-id="f992d-373">The code runs a SQL statement that deletes the specified record and then redirects the user back to the listing page.</span></span>
5. <span data-ttu-id="f992d-374">実行*ListProductsForDelete.cshtml*ブラウザーでします。</span><span class="sxs-lookup"><span data-stu-id="f992d-374">Run *ListProductsForDelete.cshtml* in a browser.</span></span>

    ![[イメージ]](5-working-with-data/_static/image9.jpg)
6. <span data-ttu-id="f992d-376">をクリックして、**削除**製品のいずれかのリンク。</span><span class="sxs-lookup"><span data-stu-id="f992d-376">Click the **Delete** link for one of the products.</span></span> <span data-ttu-id="f992d-377">*DeleteProduct.cshtml*ページが表示され、そのレコードを削除することを確認します。</span><span class="sxs-lookup"><span data-stu-id="f992d-377">The *DeleteProduct.cshtml* page is displayed to confirm that you want to delete that record.</span></span>
7. <span data-ttu-id="f992d-378">をクリックして、**削除**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="f992d-378">Click the **Delete** button.</span></span> <span data-ttu-id="f992d-379">製品レコードが削除され、製品の更新の一覧で、ページが更新されます。</span><span class="sxs-lookup"><span data-stu-id="f992d-379">The product record is deleted and the page is refreshed with an updated product listing.</span></span>

> [!TIP]
> 
> <a id="SB_ConnectingToADatabase"></a>
> ### <a name="connecting-to-a-database"></a><span data-ttu-id="f992d-380">データベースへの接続</span><span class="sxs-lookup"><span data-stu-id="f992d-380">Connecting to a Database</span></span>
> 
> <span data-ttu-id="f992d-381">2 つの方法でデータベースに接続することができます。</span><span class="sxs-lookup"><span data-stu-id="f992d-381">You can connect to a database in two ways.</span></span> <span data-ttu-id="f992d-382">1 つ目は、使用する、`Database.Open`メソッドと、データベース ファイルの名前を指定する (以下、 *.sdf*拡張機能)。</span><span class="sxs-lookup"><span data-stu-id="f992d-382">The first is to use the `Database.Open` method and to specify the name of the database file (less the *.sdf* extension):</span></span>
> 
> `var db = Database.Open("SmallBakery");`
> 
> <span data-ttu-id="f992d-383">`Open`メソッドには、ことが前提とします *。sdf*ファイルは、web サイトの*アプリ\_データ*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="f992d-383">The `Open` method assumes that the .*sdf* file is in the website's *App\_Data* folder.</span></span> <span data-ttu-id="f992d-384">このフォルダーは、データを保持する向けに設計されています。</span><span class="sxs-lookup"><span data-stu-id="f992d-384">This folder is designed specifically for holding data.</span></span> <span data-ttu-id="f992d-385">たとえばに必要なデータ、読み取りし、書き込みを許可する適切なアクセス許可があるし、セキュリティ対策として WebMatrix もアクセスできないファイルをこのフォルダーから。</span><span class="sxs-lookup"><span data-stu-id="f992d-385">For example, it has appropriate permissions to allow the website to read and write data, and as a security measure, WebMatrix does not allow access to files from this folder.</span></span>
> 
> <span data-ttu-id="f992d-386">2 番目の方法では、接続文字列を使用します。</span><span class="sxs-lookup"><span data-stu-id="f992d-386">The second way is to use a connection string.</span></span> <span data-ttu-id="f992d-387">接続文字列には、データベースに接続する方法に関する情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="f992d-387">A connection string contains information about how to connect to a database.</span></span> <span data-ttu-id="f992d-388">これは、ファイルのパスを含めることができます、またはユーザー名とそのサーバーに接続するためのパスワードと共に、ローカルまたはリモート サーバー上の SQL Server データベースの名前を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="f992d-388">This can include a file path, or it can include the name of a SQL Server database on a local or remote server, along with a user name and password to connect to that server.</span></span> <span data-ttu-id="f992d-389">(一元管理されたバージョンの SQL Server にデータを保持する場合など、ホスティング プロバイダーのサイトで常に使用する接続文字列をデータベース接続情報を指定する。)</span><span class="sxs-lookup"><span data-stu-id="f992d-389">(If you keep data in a centrally managed version of SQL Server, such as on a hosting provider's site, you always use a connection string to specify the database connection information.)</span></span>
> 
> <span data-ttu-id="f992d-390">WebMatrix で、接続文字列は、通常はという XML ファイルに格納*Web.config*します。使用することができます、名のとおり、 *Web.config*サイトを必要とする接続文字列など、サイトの構成情報を格納する web サイトのルートにあるファイル。</span><span class="sxs-lookup"><span data-stu-id="f992d-390">In WebMatrix, connection strings are usually stored in an XML file named *Web.config*. As the name implies, you can use a *Web.config* file in the root of your website to store the site's configuration information, including any connection strings that your site might require.</span></span> <span data-ttu-id="f992d-391">内の接続文字列の例を*Web.config*ファイルは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="f992d-391">An example of a connection string in a *Web.config* file might look like the following:</span></span>
> 
> [!code-xml[Main](5-working-with-data/samples/sample27.xml)]
> 
> <span data-ttu-id="f992d-392">例では、接続文字列がどこかに、サーバーで実行されている SQL Server のインスタンスでデータベースを指す (ローカルではなく *.sdf*ファイル)。</span><span class="sxs-lookup"><span data-stu-id="f992d-392">In the example, the connection string points to a database in an instance of SQL Server that's running on a server somewhere (as opposed to a local *.sdf* file).</span></span> <span data-ttu-id="f992d-393">適切な名前を置き換える必要があります`myServer`と`myDatabase`、SQL Server ログインの値を指定し、`username`と`password`します。</span><span class="sxs-lookup"><span data-stu-id="f992d-393">You would need to substitute the appropriate names for `myServer` and `myDatabase`, and specify SQL Server login values for `username` and `password`.</span></span> <span data-ttu-id="f992d-394">(ユーザー名とパスワードの値が必ずしも同じでなければ、Windows 資格情報として、または、サーバーへのログインに提供された、ホスティング プロバイダーの値として。</span><span class="sxs-lookup"><span data-stu-id="f992d-394">(The username and password values are not necessarily the same as your Windows credentials or as the values that your hosting provider has given you for logging in to their servers.</span></span> <span data-ttu-id="f992d-395">管理者に確認する必要がある正確な値にします。)</span><span class="sxs-lookup"><span data-stu-id="f992d-395">Check with the administrator for the exact values you need.)</span></span>
> 
> <span data-ttu-id="f992d-396">`Database.Open`メソッドは、データベースの名前を渡すことができるので柔軟性が高く、 *.sdf*ファイルまたはに格納されている接続文字列の名前、 *Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="f992d-396">The `Database.Open` method is flexible, because it lets you pass either the name of a database *.sdf* file or the name of a connection string that's stored in the *Web.config* file.</span></span> <span data-ttu-id="f992d-397">次の例では、前の例に示す接続文字列を使用してデータベースに接続する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="f992d-397">The following example shows how to connect to the database using the connection string illustrated in the previous example:</span></span>
> 
> [!code-cshtml[Main](5-working-with-data/samples/sample28.cshtml)]
> 
> <span data-ttu-id="f992d-398">前述のように、`Database.Open`メソッドでは、データベース名または接続文字列のいずれかを渡すことができ、それがどれを使用するを算出します。</span><span class="sxs-lookup"><span data-stu-id="f992d-398">As noted, the `Database.Open` method lets you pass either a database name or a connection string, and it'll figure out which to use.</span></span> <span data-ttu-id="f992d-399">これは、展開する場合に非常に便利です (発行)、web サイト。</span><span class="sxs-lookup"><span data-stu-id="f992d-399">This is very useful when you deploy (publish) your website.</span></span> <span data-ttu-id="f992d-400">使用することができます、 *.sdf*ファイル、*アプリ\_データ*フォルダーを開発して、サイトのテストをします。</span><span class="sxs-lookup"><span data-stu-id="f992d-400">You can use an *.sdf* file in the *App\_Data* folder when you're developing and testing your site.</span></span> <span data-ttu-id="f992d-401">内の接続文字列を使用するには、サイトを実稼働サーバーに移動すると、その後、 *Web.config*ファイルと同じ名前を持つ、 *.sdf* &#8212;せずに、コードを変更します。</span><span class="sxs-lookup"><span data-stu-id="f992d-401">Then when you move your site to a production server, you can use a connection string in the *Web.config* file that has the same name as your *.sdf* file but that points to the hosting provider's database &#8212; all without having to change your code.</span></span>
> 
> <span data-ttu-id="f992d-402">最後に、接続文字列を直接操作する場合を呼び出すことができます、`Database.OpenConnectionString`メソッドと実際の接続が文字列で 1 つの名だけでなく、パス、 *Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="f992d-402">Finally, if you want to work directly with a connection string, you can call the `Database.OpenConnectionString` method and pass it the actual connection string instead of just the name of one in the *Web.config* file.</span></span> <span data-ttu-id="f992d-403">何らかの理由はありませんがある場合、接続文字列へのアクセスに役立つことが考えられます (などの値、または、 *.sdf*ファイル名)、ページが実行されるまでです。</span><span class="sxs-lookup"><span data-stu-id="f992d-403">This might be useful in situations where for some reason you don't have access to the connection string (or values in it, such as the *.sdf* file name) until the page is running.</span></span> <span data-ttu-id="f992d-404">ただし、ほとんどのシナリオで使用できます`Database.Open`この記事で説明します。</span><span class="sxs-lookup"><span data-stu-id="f992d-404">However, for most scenarios, you can use `Database.Open` as described in this article.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="f992d-405">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="f992d-405">Additional Resources</span></span>

- [<span data-ttu-id="f992d-406">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="f992d-406">SQL Server Compact</span></span>](https://www.microsoft.com/sqlserver/2008/en/us/compact.aspx)
- [<span data-ttu-id="f992d-407">SQL Server または WebMatrix の MySQL データベースに接続します。</span><span class="sxs-lookup"><span data-stu-id="f992d-407">Connecting to a SQL Server or MySQL Database in WebMatrix</span></span>](https://go.microsoft.com/fwlink/?LinkId=208661)
- [<span data-ttu-id="f992d-408">ASP.NET Web ページにおけるユーザー入力の検証</span><span class="sxs-lookup"><span data-stu-id="f992d-408">Validating User Input in ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkId=253002)
