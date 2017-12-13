---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
title: "ASP.NET Web Pages の概要 - データの表示 |Microsoft ドキュメント"
author: tfitzmac
description: "このチュートリアルでは、WebMatrix で、データベースを作成する方法と ASP.NET Web Pages (Razor) を使用すると、ページにデータベースのデータを表示する方法を説明します。 Y が前提としています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: b3a006a0-3ea2-4d45-b833-e20e3a3c0a1a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
msc.type: authoredcontent
ms.openlocfilehash: fdb9af0ba87c7802c63451ac7aa422e0020b5719
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="introducing-aspnet-web-pages---displaying-data"></a><span data-ttu-id="9f3d7-104">ASP.NET Web ページのデータの表示の概要</span><span class="sxs-lookup"><span data-stu-id="9f3d7-104">Introducing ASP.NET Web Pages - Displaying Data</span></span>
====================
<span data-ttu-id="9f3d7-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="9f3d7-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="9f3d7-106">このチュートリアルでは、WebMatrix で、データベースを作成する方法と ASP.NET Web Pages (Razor) を使用すると、ページにデータベースのデータを表示する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-106">This tutorial shows you how to create a database in WebMatrix and how to display database data in a page when you use ASP.NET Web Pages (Razor).</span></span> <span data-ttu-id="9f3d7-107">を通じてシリーズを完了すると想定[ASP.NET Web Pages のプログラミングの概要](../introducing-razor-syntax-c.md)です。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-107">It assumes you have completed the series through [Introduction to ASP.NET Web Pages Programming](../introducing-razor-syntax-c.md).</span></span>
> 
> <span data-ttu-id="9f3d7-108">学習する内容。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="9f3d7-109">WebMatrix のツールを使用して、データベースとデータベースのテーブルを作成する方法。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-109">How to use WebMatrix tools to create a database and database tables.</span></span>
> - <span data-ttu-id="9f3d7-110">WebMatrix のツールを使用してデータベースにデータを追加する方法。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-110">How to use WebMatrix tools to add data to a database.</span></span>
> - <span data-ttu-id="9f3d7-111">ページで、データベースからデータを表示する方法です。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-111">How to display data from the database on a page.</span></span>
> - <span data-ttu-id="9f3d7-112">ASP.NET Web Pages での SQL コマンドを実行する方法。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-112">How to run SQL commands in ASP.NET Web Pages.</span></span>
> - <span data-ttu-id="9f3d7-113">カスタマイズする方法、`WebGrid`ヘルパー データ表示を変更して、ページングや並べ替えを追加します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-113">How to customize the `WebGrid` helper to change the data display and to add paging and sorting.</span></span>
>   
> 
> <span data-ttu-id="9f3d7-114">説明されている機能/テクノロジ:</span><span class="sxs-lookup"><span data-stu-id="9f3d7-114">Features/technologies discussed:</span></span>
> 
> - <span data-ttu-id="9f3d7-115">WebMatrix データベース ツールです。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-115">WebMatrix database tools.</span></span>
> - <span data-ttu-id="9f3d7-116">`WebGrid`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-116">`WebGrid` helper.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="9f3d7-117">新機能のビルドします。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-117">What You'll Build</span></span>

<span data-ttu-id="9f3d7-118">前のチュートリアルでは ASP.NET Web Pages に導入された (*.cshtml*ファイル)、Razor 構文の基本およびヘルパー。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-118">In the previous tutorial, you were introduced to ASP.NET Web Pages (*.cshtml* files), to the basics of Razor syntax, and to helpers.</span></span> <span data-ttu-id="9f3d7-119">このチュートリアルでは、系列の残りの部分に使用する実際の web アプリケーションの作成が始めます。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-119">In this tutorial, you'll begin creating the actual web application that you'll use for the rest of the series.</span></span> <span data-ttu-id="9f3d7-120">アプリは、単純なムービー アプリケーションを表示、追加、変更、およびムービーに関する情報を削除できます。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-120">The app is a simple movie application that lets you view, add, change, and delete information about movies.</span></span>

<span data-ttu-id="9f3d7-121">このチュートリアルを完了すると、このページのようなムービーの一覧を表示することができます。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-121">When you're done with this tutorial, you'll be able to view a movie listing that looks like this page:</span></span>

![パラメーターが含まれている WebGrid ディスプレイ CSS クラス名に設定](displaying-data/_static/image1.png)

<span data-ttu-id="9f3d7-123">作業を開始するには、データベースを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-123">But to begin, you have to create a database.</span></span>

## <a name="a-very-brief-introduction-to-databases"></a><span data-ttu-id="9f3d7-124">データベースの非常に簡単な概要</span><span class="sxs-lookup"><span data-stu-id="9f3d7-124">A Very Brief Introduction to Databases</span></span>

<span data-ttu-id="9f3d7-125">このチュートリアルでは、データベースに短い概要のみを提供します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-125">This tutorial will provide only the briefest introduction to databases.</span></span> <span data-ttu-id="9f3d7-126">データベースの経験があれば、この短いセクションを省略できます。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-126">If you have database experience, you can skip this short section.</span></span>

<span data-ttu-id="9f3d7-127">データベースに情報を含む 1 つまたは複数のテーブルが含まれている&mdash;たとえば、customers、orders、および仕入先、または学生、教師、クラス、および成績をテーブルです。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-127">A database contains one or more tables that contain information &mdash; for example, tables for customers, orders, and vendors, or for students, teachers, classes, and grades.</span></span> <span data-ttu-id="9f3d7-128">構造的に、データベース テーブルは、スプレッドシートに似ています。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-128">Structurally, a database table is like a spreadsheet.</span></span> <span data-ttu-id="9f3d7-129">通常、アドレス帳を想像してください。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-129">Imagine a typical address book.</span></span> <span data-ttu-id="9f3d7-130">アドレス帳の各エントリについて (つまり、各ユーザーに対して) が複数ある名、姓、名、アドレス、電子メール アドレス、および電話番号などの情報をします。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-130">For each entry in the address book (that is, for each person) you have several pieces of information such as first name, last name, address, email address, and phone number.</span></span>

![単純なグリッドとしてサンプル データベースのテーブル](displaying-data/_static/image2.png)

<span data-ttu-id="9f3d7-132">(行とも呼ば*レコード*、列と呼ばれることもありますし*フィールド*)。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-132">(Rows are sometimes referred to as *records*, and columns are sometimes referred to as *fields*.)</span></span>

<span data-ttu-id="9f3d7-133">ほとんどのデータベース テーブルのテーブルに顧客番号、アカウント番号などの一意の値を含む列にあります。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-133">For most database tables, the table has to have a column that contains a unique value, like a customer number, account number, and so on.</span></span> <span data-ttu-id="9f3d7-134">この値は、テーブルと呼ばれる*主キー*テーブル内の各行の識別に使用するとします。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-134">This value is known as the table's *primary key*, and you use it to identify each row in the table.</span></span> <span data-ttu-id="9f3d7-135">例では、ID 列は、前の例に示すように、アドレス帳の主キーがします。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-135">In the example, the ID column is the primary key for the address book shown in the previous example.</span></span>

<span data-ttu-id="9f3d7-136">Web アプリケーションで行う作業の多くは、データベースから情報を読み取ると、ページに表示するので構成されます。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-136">Much of the work you do in web applications consists of reading information out of the database and displaying it on a page.</span></span> <span data-ttu-id="9f3d7-137">多くの場合、ユーザーから情報を収集し、データベースに追加またはデータベースに既に存在するレコードを変更してみます。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-137">You'll also often gather information from users and add it to a database, or you'll modify records that are already in the database.</span></span> <span data-ttu-id="9f3d7-138">(こここれらすべての操作中は、このチュートリアルのセットです。)</span><span class="sxs-lookup"><span data-stu-id="9f3d7-138">(We'll cover all of these operations in the course of this tutorial set.)</span></span>

<span data-ttu-id="9f3d7-139">データベースの作業では、非常に複雑になることができ、専門知識が必要なことができます。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-139">Database work can be enormously complex and can require specialized knowledge.</span></span> <span data-ttu-id="9f3d7-140">このチュートリアルのセットには、理解する必要のみ基本的な概念はすべて、説明を進めながらです。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-140">For this tutorial set, though, you have to understand only basic concepts, which will all be explained as you go.</span></span>

## <a name="creating-a-database"></a><span data-ttu-id="9f3d7-141">データベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-141">Creating a Database</span></span>

<span data-ttu-id="9f3d7-142">WebMatrix には、簡単なは、データベースを作成して、データベースにテーブルを作成するためのツールが含まれています。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-142">WebMatrix includes tools that make it easy to create a database and to create tables in the database.</span></span> <span data-ttu-id="9f3d7-143">(データベースの構造をデータベースのと呼びます*スキーマ*)。このチュートリアルのセットを作成する 1 つのテーブルが含まれるデータベース&mdash;映画します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-143">(The structure of a database is referred to as the database's *schema*.) For this tutorial set, you'll create a database that has only one table in it &mdash; Movies.</span></span>

<span data-ttu-id="9f3d7-144">まだ行っていない場合、WebMatrix を開き、前のチュートリアルで作成した WebPagesMovies サイトを開きます。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-144">Open WebMatrix if you haven't already done so, and open the WebPagesMovies site that you created in the previous tutorial.</span></span>

<span data-ttu-id="9f3d7-145">左側のウィンドウでをクリックして、**データベース**ワークスペース。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-145">In the left pane, click the **Database** workspace.</span></span>

![WebMatrix データベース ワークスペース タブ](displaying-data/_static/image3.png)

<span data-ttu-id="9f3d7-147">データベースに関連するタスクを表示するリボン変更します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-147">The ribbon changes to show database-related tasks.</span></span> <span data-ttu-id="9f3d7-148">リボンで、をクリックして**新しいデータベース**です。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-148">In the ribbon, click **New Database**.</span></span>

!['新しい Database' WebMatrix リボン ボタンをクリックします。](displaying-data/_static/image4.png)

<span data-ttu-id="9f3d7-150">WebMatrix には、SQL Server CE データベースが作成されます (、 *.sdf*ファイル)、そのサイトと同じ名前を持つ&mdash; *WebPagesMovies.sdf*です。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-150">WebMatrix creates a SQL Server CE database (an *.sdf* file) that has the same name as your site &mdash; *WebPagesMovies.sdf*.</span></span> <span data-ttu-id="9f3d7-151">(されませんこうと、ここがある限り、必要な場合、何もするファイルを変更することができます、 *.sdf*拡張機能です)。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-151">(You won't do this here, but you can rename the file to anything you like, as long as it has an *.sdf* extension.)</span></span>

## <a name="creating-a-table"></a><span data-ttu-id="9f3d7-152">テーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-152">Creating a Table</span></span>

<span data-ttu-id="9f3d7-153">リボンで、をクリックして**新しいテーブル**です。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-153">In the ribbon, click **New Table**.</span></span> <span data-ttu-id="9f3d7-154">WebMatrix は、新しいタブで、テーブル デザイナーを開きます。([新しいテーブル] オプションを使用できない場合は、左側のツリー ビューで、新しいデータベースが選択されていることを確認) します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-154">WebMatrix opens the table designer in a new tab. (If the New Table option isn't available, make sure that the new database is selected in the tree view on the left.)</span></span>

!['新しいテーブル' WebMatrix リボン ボタンをクリックします。](displaying-data/_static/image5.png)

<span data-ttu-id="9f3d7-156">(ウォーターマークは「Enter テーブル名」) の上部にあるテキスト ボックスで、「映画」を入力します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-156">In the text box at the top (where the watermark says "Enter table name"), enter "Movies".</span></span>

![WebMatrix データベース デザイナーでテーブル名を入力します。](displaying-data/_static/image6.png)

<span data-ttu-id="9f3d7-158">テーブル名の下のウィンドウは、個々 の列を定義した場合です。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-158">The pane underneath the table name is where you define individual columns.</span></span> <span data-ttu-id="9f3d7-159">*映画*テーブルこのチュートリアルでは、少数の列のみを作成します: *ID*、*タイトル*、*ジャンル*、および*年*.</span><span class="sxs-lookup"><span data-stu-id="9f3d7-159">For the *Movies* table in this tutorial, you'll create only a few columns: *ID*, *Title*, *Genre*, and *Year*.</span></span>

<span data-ttu-id="9f3d7-160">**名前**ボックスに、"ID"を入力します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-160">In the **Name** box, enter "ID".</span></span> <span data-ttu-id="9f3d7-161">ここに値を入力するには、新しい列のすべてのコントロールがアクティブにします。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-161">Entering a value here activates all the controls for the new column.</span></span>

<span data-ttu-id="9f3d7-162">タブ、**データ型**一覧**int**です。この値は、ID 列が整数 (数) データを含めることを指定します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-162">Tab to the **Data Type** list and choose **int**. This value specifies that the ID column will contain integer (number) data.</span></span>

> [!NOTE]
> <span data-ttu-id="9f3d7-163">されません呼んで、ここをクリック (高) が、このグリッド内で移動する Windows の標準のキーボード ジェスチャを使用できます。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-163">We won't call it out any more here (much), but you can use standard Windows keyboard gestures to navigate in this grid.</span></span> <span data-ttu-id="9f3d7-164">たとえば、フィールド間タブできますでするだけの一覧で項目を選択するために入力を開始したりできます。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-164">For example, you can tab between fields, you can just start typing in order to select an item in a list, and so on.</span></span>


<span data-ttu-id="9f3d7-165">Tab キーを押した、**既定値**ボックス (つまり、空白のままに)。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-165">Tab past the **Default Value** box (that is, leave it blank).</span></span> <span data-ttu-id="9f3d7-166">タブ、**主キーである**チェック ボックスをオンし、それを選択します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-166">Tab to the **Is Primary Key** check box and select it.</span></span> <span data-ttu-id="9f3d7-167">このオプションにより、データベースを*ID*列は個々 の行を識別するデータが格納されます。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-167">This option tells the database that the *ID* column will contain the data that identifies individual rows.</span></span> <span data-ttu-id="9f3d7-168">(は、各行が一意の値その行を検索するのに使用できる [ID] 列にします。)</span><span class="sxs-lookup"><span data-stu-id="9f3d7-168">(That is, each row will have a unique value in the ID column that you can use to find that row.)</span></span>

<span data-ttu-id="9f3d7-169">選択、 **Is Identity**オプション。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-169">Choose the **Is Identity** option.</span></span> <span data-ttu-id="9f3d7-170">このオプションは、新しい行ごとに次の連番を自動的に生成する必要があります、データベースを指定します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-170">This option tells the database that it should automatically generate the next sequential number for each new row.</span></span> <span data-ttu-id="9f3d7-171">(、 **Is Identity**オプションも選択されている場合にのみを使用できる、**主キーである**オプションです)。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-171">(The **Is Identity** option works only if you've also selected the **Is Primary Key** option.)</span></span>

<span data-ttu-id="9f3d7-172">次のグリッド行のか、現在の行を終了するには、2 回 Tab キーを押してください。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-172">Click in the next grid row, or press Tab twice to finish the current row.</span></span> <span data-ttu-id="9f3d7-173">いずれかのジェスチャでは、現在の行を保存し、[次へ] のいずれかを開始します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-173">Either gesture saves the current row and starts the next one.</span></span> <span data-ttu-id="9f3d7-174">注意して、**既定値**列伝える**Null**です。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-174">Notice that the **Default Value** column now says **Null**.</span></span> <span data-ttu-id="9f3d7-175">(Null、既定値は、既定値のいわば。)</span><span class="sxs-lookup"><span data-stu-id="9f3d7-175">(Null is the default value for the default value, so to speak.)</span></span>

<span data-ttu-id="9f3d7-176">新しい定義を終了したら**ID**デザイナーは列で、次の図のようになります。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-176">When you've finished defining the new **ID** column, the designer will look like this illustration:</span></span>

![映画テーブルの ID 列を定義した後に WebMatrix データベース デザイナー](displaying-data/_static/image7.png)

<span data-ttu-id="9f3d7-178">次の列を作成するには、ボックスでクリックして、**名前**列です。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-178">To create the next column, click in the box in the **Name** column.</span></span> <span data-ttu-id="9f3d7-179">列の「タイトル」を入力し、 **nvarchar**の**データ型**値。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-179">Enter "Title" for the column and then select **nvarchar** for the **Data Type** value.</span></span> <span data-ttu-id="9f3d7-180">"Var"部分**nvarchar**この列のデータがサイズが異なるレコード間を文字列になるデータベースに指示します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-180">The "var" part of **nvarchar** tells the database that the data for this column will be a string whose size might vary from record to record.</span></span> <span data-ttu-id="9f3d7-181">("N"プレフィックスを表す「各国語」フィールドがすべてアルファベットまたは書記体系の文字データを保持できることを示します: つまり、フィールドが Unicode データを保持します)。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-181">(The "n" prefix represents "national," which indicates that the field can hold character data for any alphabet or writing system — that is, the field holds Unicode data.)</span></span>

<span data-ttu-id="9f3d7-182">選択すると**nvarchar**、別のボックスでは、フィールドの最大長を入力する場所が表示されます。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-182">When you choose **nvarchar**, another box appears where you can enter the maximum length for the field.</span></span> <span data-ttu-id="9f3d7-183">このチュートリアルで扱うムービーのタイトルがれないこと 50 文字より長いことを前提に、50」と入力します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-183">Enter 50, on the assumption that no movie title that you'll work with in this tutorial will be longer than 50 characters.</span></span>

<span data-ttu-id="9f3d7-184">Skip**既定値**をオフにし、 **null を許容**オプション。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-184">Skip **Default Value** and clear the **Allow Nulls** option.</span></span> <span data-ttu-id="9f3d7-185">タイトルがないデータベースに入力するムービーを許可するデータベースをしたくありません。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-185">You don't want the database to allow any movies to be entered into the database that don't have a title.</span></span>

<span data-ttu-id="9f3d7-186">完了したら、次の行に移動すると、デザイナーは、次の図のようになります。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-186">When you're done and move to the next row, the designer looks like this illustration:</span></span>

![映画テーブルのタイトル列を定義した後に WebMatrix データベース デザイナー](displaying-data/_static/image8.png)

<span data-ttu-id="9f3d7-188">長さを除く「ジャンル」をという名前の列を作成する手順だけ 30 に設定を繰り返します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-188">Repeat these steps to create a column named "Genre", except for the length, set it to just 30.</span></span>

<span data-ttu-id="9f3d7-189">"Year"をという名前の別の列を作成します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-189">Create another column named "Year."</span></span> <span data-ttu-id="9f3d7-190">データ型を選択**nchar** (いない**nvarchar**) し、長さが 4 に設定します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-190">For the data type, choose **nchar** (not **nvarchar**) and set the length to 4.</span></span> <span data-ttu-id="9f3d7-191">年度のしようとしている「1995」または「2010」のように 4 桁の数字を使用して可変サイズの列を必要としないようにします。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-191">For the year, you're going to use a 4-digit number like "1995" or "2010", so you don't require a variable-sized column.</span></span>

<span data-ttu-id="9f3d7-192">完成した設計は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-192">Here's what the finished design looks like:</span></span>

![映画テーブルのすべてのフィールドを定義したら、WebMatrix データベース デザイナー](displaying-data/_static/image9.png)

<span data-ttu-id="9f3d7-194">Ctrl キーを押しながら S キーを押すかをクリックして、**保存**クイック アクセス ツールバーのボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-194">Press Ctrl+S or click the **Save** button in the Quick Access toolbar.</span></span> <span data-ttu-id="9f3d7-195">タブを閉じることで、データベース デザイナーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-195">Close the database designer by closing the tab.</span></span>

## <a name="adding-some-example-data"></a><span data-ttu-id="9f3d7-196">一部の例のデータを追加します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-196">Adding Some Example Data</span></span>

<span data-ttu-id="9f3d7-197">このチュートリアル シリーズの後で、フォームに新しいムービーを入力できるページを作成します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-197">Later in this tutorial series you'll create a page where you can enter new movies in a form.</span></span> <span data-ttu-id="9f3d7-198">ただし、ここには、ページに表示できる、いくつかのサンプル データを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-198">For now, however, you can add some example data that you can then display on a page.</span></span>

<span data-ttu-id="9f3d7-199">**データベース**ワークスペースを表示するツリーがあることに注意してください、WebMatrix で、 *.sdf*先ほど作成したファイルです。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-199">In the **Database** workspace in WebMatrix, notice that there's a tree that shows you the *.sdf* file you created earlier.</span></span> <span data-ttu-id="9f3d7-200">新しいノードを開いて*.sdf*ファイルを開き、**テーブル**ノード。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-200">Open the node for your new *.sdf* file, and then open the **Tables** node.</span></span>

![ツリーの映画のテーブルを開くと WebMatrix データベース ワークスペース](displaying-data/_static/image10.png)

<span data-ttu-id="9f3d7-202">右クリックし、**映画**ノードを選択し**データ**です。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-202">Right-click the **Movies** node and then choose **Data**.</span></span> <span data-ttu-id="9f3d7-203">WebMatrix のデータを入力するグリッドを開き、*映画*テーブル。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-203">WebMatrix opens a grid where you can enter data for the *Movies* table:</span></span>

![WebMatrix (空) のデータベース エントリ グリッド](displaying-data/_static/image11.png)

<span data-ttu-id="9f3d7-205">をクリックして、**タイトル**列"と Harry 満たす Sally"を入力します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-205">Click the **Title** column and enter "When Harry Met Sally".</span></span> <span data-ttu-id="9f3d7-206">移動、**ジャンル**列 (Tab キーを使用することができます) と「Romantic コメディ」を入力します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-206">Move to the **Genre** column (you can use the Tab key) and enter "Romantic Comedy".</span></span> <span data-ttu-id="9f3d7-207">移動、**年**列「1989」を入力します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-207">Move to the **Year** column and enter "1989":</span></span>

![WebMatrix で 1 つのレコードでのデータベース エントリ グリッド](displaying-data/_static/image12.png)

<span data-ttu-id="9f3d7-209">Enter キーを押して、および WebMatrix は、新しいムービーを保存します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-209">Press Enter, and WebMatrix saves the new movie.</span></span> <span data-ttu-id="9f3d7-210">注意して、 **ID**列が入力されています。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-210">Notice that the **ID** column has been filled in.</span></span>

![1 つのレコードと自動生成された ID を持つ、WebMatrix でデータベース エントリ グリッド](displaying-data/_static/image13.png)

<span data-ttu-id="9f3d7-212">別のビデオ (たとえば、「風と共に去り」、「ドラマ ・」、「1939」) を入力します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-212">Enter another movie (for example, "Gone with the Wind", "Drama", "1939").</span></span> <span data-ttu-id="9f3d7-213">ID 列はもう一度入力されます。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-213">The ID column is filled in again:</span></span>

![WebMatrix で 2 つのレコードと、自動生成された Id のデータベース エントリ グリッド](displaying-data/_static/image14.png)

<span data-ttu-id="9f3d7-215">3 番目の映画 (たとえば、"Ghostbusters"、「コメディ」) を入力します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-215">Enter a third movie (for example, "Ghostbusters", "Comedy").</span></span> <span data-ttu-id="9f3d7-216">試しのままにして、**年**列は空白し、し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-216">As an experiment, leave the **Year** column blank and then press Enter.</span></span> <span data-ttu-id="9f3d7-217">選択を解除するため、 **null を許容**オプション、データベースは、エラーを表示します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-217">Because you unselected the **Allow Nulls** option, the database shows an error:</span></span>

![必要な列の値が空欄の場合、無効なデータのエラーが表示されます。](displaying-data/_static/image15.png)

<span data-ttu-id="9f3d7-219">をクリックして**OK**前に戻ってと ("Ghostbusters"の年は 1984) エントリを修正し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-219">Click **OK** to go back and fix the entry (the year for "Ghostbusters" is 1984), and then press Enter.</span></span>

<span data-ttu-id="9f3d7-220">8 になるまで、またはために、いくつかの映画を入力します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-220">Fill in several movies until you have 8 or so.</span></span> <span data-ttu-id="9f3d7-221">(8 を入力しやすく後でページングを使用します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-221">(Entering 8 makes it easier to work with paging later.</span></span> <span data-ttu-id="9f3d7-222">多すぎる場合は、ここでは一部だけを入力してください)。実際のデータは関係ありません。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-222">But if that's too many, enter just a few for now.) The actual data doesn't matter.</span></span>

![WebMatrix でいずれかのレコードとのデータベース エントリ グリッド](displaying-data/_static/image16.png)

<span data-ttu-id="9f3d7-224">入力した場合、エラーがないすべてのムービー、ID の値は順次です。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-224">If you entered all the movies without any errors, the ID values are sequential.</span></span> <span data-ttu-id="9f3d7-225">不完全なムービー レコードを保存しようとすると、ID 番号はシーケンシャルできない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-225">If you tried to save an incomplete movie record, the ID numbers might not be sequential.</span></span> <span data-ttu-id="9f3d7-226">場合は、ことができます。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-226">If so, that's okay.</span></span> <span data-ttu-id="9f3d7-227">数値は、本質的な意味がない、重要となる唯一の機能内で一意であること、*映画*テーブル。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-227">The numbers don't have any inherent meaning, and the only thing that's important is that they're unique in the *Movies* table.</span></span>

<span data-ttu-id="9f3d7-228">データベース デザイナーを含むタブを閉じます。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-228">Close the tab that contains the database designer.</span></span>

<span data-ttu-id="9f3d7-229">今すぐ web ページ上のこのデータの表示を有効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-229">Now you can turn to displaying this data on a web page.</span></span>

## <a name="displaying-data-in-a-page-by-using-the-webgrid-helper"></a><span data-ttu-id="9f3d7-230">ページの WebGrid ヘルパーを使用してデータを表示します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-230">Displaying Data in a Page by Using the WebGrid Helper</span></span>

<span data-ttu-id="9f3d7-231">利用することを表示するにはデータ ページで、`WebGrid`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-231">To display data in a page, you're going to use the `WebGrid` helper.</span></span> <span data-ttu-id="9f3d7-232">このヘルパーは、グリッドやテーブル (行と列) の表示を生成します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-232">This helper produces a display in a grid or table (rows and columns).</span></span> <span data-ttu-id="9f3d7-233">説明するように、できる絞り込み書式およびその他の機能を持つグリッドができます。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-233">As you'll see, you'll be able refine the grid with formatting and other features.</span></span>

<span data-ttu-id="9f3d7-234">実行するには、グリッドには、数行のコードを記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-234">To run the grid, you'll have to write a few lines of code.</span></span> <span data-ttu-id="9f3d7-235">これらのいくつかの行は、このチュートリアルで行うのデータ アクセスのほとんどすべてのパターンの種類として使用されます。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-235">These few lines will serve as a kind of pattern for almost all of the data access that you do in this tutorial.</span></span>

> [!NOTE]
> <span data-ttu-id="9f3d7-236">実際には、ページ上のデータを表示するための多くのオプションがあります。`WebGrid`ヘルパーは、1 つにすぎません。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-236">You actually have many options for displaying data on a page; the `WebGrid` helper is just one.</span></span> <span data-ttu-id="9f3d7-237">選びました、このチュートリアルではデータを表示する最も簡単な方法になっているため、ある程度柔軟になっているためです。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-237">We chose it for this tutorial because it's the easiest way to display data and because it's reasonably flexible.</span></span> <span data-ttu-id="9f3d7-238">次のチュートリアルのセットに制御する直接的なデータを表示する方法 ページでデータを操作する複数の"manual"方法で使用する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-238">In the next tutorial set, you'll see how to use a more "manual" way to work with data in the page, which gives you more direct control over how to display the data.</span></span>


<span data-ttu-id="9f3d7-239">WebMatrix で左側のウィンドウでをクリックして、**ファイル**ワークスペース。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-239">In the left pane in WebMatrix, click the **Files** workspace.</span></span>

<span data-ttu-id="9f3d7-240">作成した新しいデータベースは、*アプリ\_データ*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-240">The new database you created is in the *App\_Data* folder.</span></span> <span data-ttu-id="9f3d7-241">フォルダーが存在しない場合、WebMatrix により、新しいデータベースに作成されます。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-241">If the folder didn't already exist, WebMatrix created it for your new database.</span></span> <span data-ttu-id="9f3d7-242">(フォルダーが存在していたヘルパーをインストールした場合。)</span><span class="sxs-lookup"><span data-stu-id="9f3d7-242">(The folder might have existed if you'd previously installed helpers.)</span></span>

<span data-ttu-id="9f3d7-243">ツリー ビューでは、web サイトのルートを選択します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-243">In the tree view, select the root of the website.</span></span> <span data-ttu-id="9f3d7-244">Web サイトのルートを選択する必要がありますそれ以外の場合、アプリに新しいファイルを追加する可能性があります\_データ フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-244">You must select the root of the website; otherwise, the new file might be added to the App\_Data folder.</span></span>

<span data-ttu-id="9f3d7-245">リボンで、をクリックして**新規**です。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-245">In the ribbon, click **New**.</span></span> <span data-ttu-id="9f3d7-246">**ファイルの種類を選択して**ボックスで、選択**CSHTML**です。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-246">In the **Choose a File Type** box, choose **CSHTML**.</span></span>

<span data-ttu-id="9f3d7-247">**名前**ボックスに、新しいページには"Movies.cshtml"の名前します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-247">In the **Name** box, name the new page "Movies.cshtml":</span></span>

!['映画' ページの表示 を選択するファイルの種類' ダイアログ ボックス](displaying-data/_static/image17.png)

<span data-ttu-id="9f3d7-249">クリックして、 **OK**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-249">Click the **OK** button.</span></span> <span data-ttu-id="9f3d7-250">WebMatrix は、いくつかのスケルトン要素内で新しいファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-250">WebMatrix opens a new file with some skeleton elements in it.</span></span> <span data-ttu-id="9f3d7-251">最初にデータベースからデータを取得するコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-251">First you'll write some code to go get the data from the database.</span></span> <span data-ttu-id="9f3d7-252">マークアップを実際にデータを表示するページに追加します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-252">Then you'll add markup to the page to actually display the data.</span></span>

### <a name="writing-the-data-query-code"></a><span data-ttu-id="9f3d7-253">データのクエリのコードの記述</span><span class="sxs-lookup"><span data-stu-id="9f3d7-253">Writing the Data Query Code</span></span>

<span data-ttu-id="9f3d7-254">ページの上部にある間、`@{`と`}`文字は、次のコードを入力します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-254">At the top of the page, between the `@{` and `}` characters, enter the following code.</span></span> <span data-ttu-id="9f3d7-255">(開始タグと右中かっこの間でこのコードを入力することを確認してください。)</span><span class="sxs-lookup"><span data-stu-id="9f3d7-255">(Make sure that you enter this code between the opening and closing braces.)</span></span>

[!code-csharp[Main](displaying-data/samples/sample1.cs)]

<span data-ttu-id="9f3d7-256">最初の行は、これは常に、まず、データベースを処理する前に、先ほど作成したデータベースを開きます。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-256">The first line opens the database that you created earlier, which is always the first step before doing something with the database.</span></span> <span data-ttu-id="9f3d7-257">指定する、`Database.Open`を開くには、データベースのメソッド名。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-257">You tell the `Database.Open` method name of the database to open.</span></span> <span data-ttu-id="9f3d7-258">通知が含まれない*.sdf*名にします。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-258">Notice that you don't include *.sdf* in the name.</span></span> <span data-ttu-id="9f3d7-259">`Open`方法では、それを探して、 *.sdf*ファイル (つまり、 *WebPagesMovies.sdf*) ことと、 *.sdf*ファイルが、*アプリ\_データ*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-259">The `Open` method assumes that it's looking for an *.sdf* file (that is, *WebPagesMovies.sdf*) and that the *.sdf* file is in the *App\_Data* folder.</span></span> <span data-ttu-id="9f3d7-260">(前に述べたを*アプリ\_データ*フォルダーが予約されていますこのシナリオは、ASP.NET がその名に関する前提条件を、場所の 1 つです。)。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-260">(Earlier we noted that the *App\_Data* folder is reserved; this scenario is one of the places where ASP.NET makes assumptions about that name.)</span></span>

<span data-ttu-id="9f3d7-261">参照はという名前の変数に格納されているデータベースが開かれたときに`db`です。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-261">When the database is opened, a reference to it is put into the variable named `db`.</span></span> <span data-ttu-id="9f3d7-262">(をでした命名されているものです。)`db`変数がどのようにすることになります、データベースとやり取りします。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-262">(Which could be named anything.) The `db` variable is how you'll end up interacting with the database.</span></span>

<span data-ttu-id="9f3d7-263">2 番目の行が実際には、データベースのデータをフェッチを使用して、`Query`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-263">The second line actually fetches the database data by using the `Query` method.</span></span> <span data-ttu-id="9f3d7-264">このコードの動作に注意してください:`db`変数、開いているデータベースへの参照があり、呼び出し、`Query`メソッドを使用して、`db`変数 (`db.Query`)。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-264">Notice how this code works: the `db` variable has a reference to the opened database, and you invoke the `Query` method by using the `db` variable (`db.Query`).</span></span>

<span data-ttu-id="9f3d7-265">クエリ自体は、SQL`Select`ステートメントです。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-265">The query itself is a SQL `Select` statement.</span></span> <span data-ttu-id="9f3d7-266">(SQL に関する予備知識については後を参照してください)。ステートメントでは、`Movies`クエリにテーブルを識別します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-266">(For a little background about SQL, see the explanation later.) In the statement, `Movies` identifies the table to query.</span></span> <span data-ttu-id="9f3d7-267">`*`文字では、クエリが、テーブルからすべての列を返すことを指定します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-267">The `*` character specifies that the query should return all the columns from the table.</span></span> <span data-ttu-id="9f3d7-268">(でしたまた一覧表示する列に個別に、コンマで区切っています。)</span><span class="sxs-lookup"><span data-stu-id="9f3d7-268">(You could also list columns individually, separated by commas.)</span></span>

<span data-ttu-id="9f3d7-269">クエリの結果存在する場合、返されで利用できる、`selectedData`変数。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-269">The results of the query, if any, are returned and made available in the `selectedData` variable.</span></span> <span data-ttu-id="9f3d7-270">もう一度、変数の名前を使用ものです。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-270">Again, the variable could be named anything.</span></span>

<span data-ttu-id="9f3d7-271">最後に、3 行目指示 ASP.NET のインスタンスを使用すること、`WebGrid`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-271">Finally, the third line tells ASP.NET that you want to use an instance of the `WebGrid` helper.</span></span> <span data-ttu-id="9f3d7-272">作成する (*をインスタンス化*) を使用して、ヘルパー オブジェクト、`new`キーワードを使用して、クエリの結果を渡すと、`selectedData`変数。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-272">You create (*instantiate*) the helper object by using the `new` keyword and pass it the query results via the `selectedData` variable.</span></span> <span data-ttu-id="9f3d7-273">新しい`WebGrid`オブジェクト、データベース クエリの結果で利用可能な`grid`変数。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-273">The new `WebGrid` object, along with the results of the database query, are made available in the `grid` variable.</span></span> <span data-ttu-id="9f3d7-274">実際にデータを表示 ページですぐにその結果を必要があります。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-274">You'll need that result in a moment to actually display the data in the page.</span></span>

<span data-ttu-id="9f3d7-275">この段階で、データベースが開かれて、データを取得したし、準備した、`WebGrid`そのデータをヘルパー。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-275">At this stage, the database has been opened, you've gotten the data you want, and you've prepared the `WebGrid` helper with that data.</span></span> <span data-ttu-id="9f3d7-276">次に、ページのマークアップを作成します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-276">Next is to create the markup in the page.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="9f3d7-277">**構造化照会言語 (SQL)**</span><span class="sxs-lookup"><span data-stu-id="9f3d7-277">**Structured Query Language (SQL)**</span></span>
> 
> <span data-ttu-id="9f3d7-278">SQL は、データベース内のデータを管理するため、ほとんどのリレーショナル データベースで使用されている言語です。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-278">SQL is a language that's used in most relational databases for managing data in a database.</span></span> <span data-ttu-id="9f3d7-279">これには、データを取得し、更新するための作成、変更、およびデータベース テーブル内のデータを管理するためのコマンドが含まれます。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-279">It includes commands that let you retrieve data and update it, and that let you create, modify, and manage data in database tables.</span></span> <span data-ttu-id="9f3d7-280">SQL は、(c# のような) のプログラミング言語と異なります。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-280">SQL is different than a programming language (like C#).</span></span> <span data-ttu-id="9f3d7-281">SQL では、データベースを指定すると、目的の動作と、データの取得や、タスクを実行する方法を理解するデータベースのジョブであります。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-281">With SQL, you tell the database what you want, and it's the database's job to figure out how to get the data or perform the task.</span></span> <span data-ttu-id="9f3d7-282">一部の SQL コマンドの例とその実行内容を次に示します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-282">Here are examples of some SQL commands and what they do:</span></span>
> 
> `Select * From Movies`
> 
> `SELECT ID, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> <span data-ttu-id="9f3d7-283">最初の`Select`ステートメントは、すべての列を取得 (で指定した`*`) から、*映画*テーブル。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-283">The first `Select` statement gets all the columns (specified by `*`) from the *Movies* table.</span></span>
> 
> <span data-ttu-id="9f3d7-284">2 番目`Select`ステートメント内のレコードから、ID、名、および Price 列をフェッチし、*製品*価格列値は 10 個以上のテーブルです。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-284">The second `Select` statement fetches the ID, Name, and Price columns from records in the *Product* table whose Price column value is more than 10.</span></span> <span data-ttu-id="9f3d7-285">コマンドは、[名前] 列の値に基づいてアルファベット順に結果を返します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-285">The command returns the results in alphabetical order based on the values of the Name column.</span></span> <span data-ttu-id="9f3d7-286">価格条件に一致するレコードがない場合、このコマンドは、空のセットを返します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-286">If no records match the price criteria, the command returns an empty set.</span></span>
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ('Croissant', 'A flaky delight', 1.99)`
> 
> <span data-ttu-id="9f3d7-287">このコマンドに新しいレコードを挿入する、*製品*テーブル、"Croissant"、「A 不安定な楽しめます」に説明の列と価格を名前の列から 1.99 に設定します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-287">This command inserts a new record into the *Product* table, setting the Name column to "Croissant", the Description column to "A flaky delight", and the price to 1.99.</span></span>
> 
> <span data-ttu-id="9f3d7-288">数値以外の値を指定するしている場合に、値を単一引用符 (いない二重引用符、C# の場合と同様に) で囲むことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-288">Notice that when you're specifying a non-numeric value, the value is enclosed in single quotation marks (not double quotation marks, as in C#).</span></span> <span data-ttu-id="9f3d7-289">周囲の数値ではなくが、テキストや日付の値を囲むこれらの引用符を使用するとします。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-289">You use these quotation marks around text or date values, but not around numbers.</span></span>
> 
> `DELETE FROM Product WHERE ExpirationDate < '01/01/2008'`
> 
> <span data-ttu-id="9f3d7-290">このコマンドは、内のレコードを削除、*製品*有効期限の日付列が含まれるは 2008 年 1 月 1 日より前のテーブルです。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-290">This command deletes records in the *Product* table whose expiration date column is earlier than January 1, 2008.</span></span> <span data-ttu-id="9f3d7-291">(コマンドが想定する、*製品*テーブルにあるこのような列は、もちろんです)。/MM/DD/YYYY 形式で日付がここに入力したが、ロケールに使用される形式で入力する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-291">(The command assumes that the *Product* table has such a column, of course.) The date is entered here in MM/DD/YYYY format, but it should be entered in the format that's used for your locale.</span></span>
> 
> <span data-ttu-id="9f3d7-292">`Insert`と`Delete`コマンドの結果セットを返しません。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-292">The `Insert` and `Delete` commands don't return result sets.</span></span> <span data-ttu-id="9f3d7-293">代わりに、これらは、コマンドの影響を受けたレコードの数を示す番号を返します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-293">Instead, they return a number that tells you how many records were affected by the command.</span></span>
> 
> <span data-ttu-id="9f3d7-294">これらの操作 (挿入して、レコードを削除する) などの一部で、操作を要求しているプロセスの適切なアクセス許可を持つデータベースではします。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-294">For some of these operations (like inserting and deleting records), the process that's requesting the operation has to have appropriate permissions in the database.</span></span> <span data-ttu-id="9f3d7-295">実稼働データベースの多くの場合、データベースに接続するときに、ユーザー名とパスワードを指定する必要があるためです。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-295">That's why for production databases you often have to supply a user name and password when you connect to the database.</span></span>
> 
> <span data-ttu-id="9f3d7-296">重複除去は、SQL コマンドがありますが、ここに表示されるコマンドと同様に、パターンに従うすべて。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-296">There are dozens of SQL commands, but they all follow a pattern like the commands you see here.</span></span> <span data-ttu-id="9f3d7-297">SQL コマンドを使用して、データベース テーブルを作成、テーブル内のレコードの数をカウント、価格、計算、および多くの操作を実行することができます。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-297">You can use SQL commands to create database tables, count the number of records in a table, calculate prices, and perform many more operations.</span></span>


### <a name="adding-markup-to-display-the-data"></a><span data-ttu-id="9f3d7-298">データを表示するマークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-298">Adding Markup to Display the Data</span></span>

<span data-ttu-id="9f3d7-299">内部、`<head>`要素のセットの内容、 `<title>` 「映画」要素。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-299">Inside the `<head>` element, set contents of the `<title>` element to "Movies":</span></span>

[!code-html[Main](displaying-data/samples/sample2.html?highlight=3)]

<span data-ttu-id="9f3d7-300">内部、`<body>`ページの要素は、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-300">Inside the `<body>` element of the page, add the following:</span></span>

[!code-html[Main](displaying-data/samples/sample3.html)]

<span data-ttu-id="9f3d7-301">それで完了です。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-301">That's it.</span></span> <span data-ttu-id="9f3d7-302">`grid`変数を作成したときに作成した値は、`WebGrid`前のコード内のオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-302">The `grid` variable is the value you created when you created the `WebGrid` object in code earlier.</span></span>

<span data-ttu-id="9f3d7-303">WebMatrix ツリー ビューでページを右クリックし、選択**ブラウザーで起動**です。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-303">In the WebMatrix tree view, right-click the page and select **Launch in browser**.</span></span> <span data-ttu-id="9f3d7-304">このページのようなものが表示されます。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-304">You'll see something like this page:</span></span>

![映画テーブルから既定の WebGrid ヘルパーの出力](displaying-data/_static/image18.png)

<span data-ttu-id="9f3d7-306">その列を並べ替える列見出しのリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-306">Click a column heading link to sort by that column.</span></span> <span data-ttu-id="9f3d7-307">組み込まれている機能は、見出しをクリックして並べ替えられる、 **WebGrid**ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-307">Being able to sort by clicking a heading is a feature that's built into the **WebGrid** helper.</span></span>

<span data-ttu-id="9f3d7-308">`GetHtml`メソッドにその名前からわかるようには、データを表示するマークアップを生成します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-308">The `GetHtml` method, as its name suggests, generates markup that displays the data.</span></span> <span data-ttu-id="9f3d7-309">既定では、`GetHtml`メソッドは HTML を生成`<table>`要素。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-309">By default, the `GetHtml` method generates an HTML `<table>` element.</span></span> <span data-ttu-id="9f3d7-310">(する場合を確認できますレンダリング ブラウザー内のページのソースを確認しています。)</span><span class="sxs-lookup"><span data-stu-id="9f3d7-310">(If you want, you can verify the rendering by looking at the source of the page in the browser.)</span></span>

## <a name="modifying-the-look-of-the-grid"></a><span data-ttu-id="9f3d7-311">グリッドの外観を変更します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-311">Modifying the Look of the Grid</span></span>

<span data-ttu-id="9f3d7-312">使用して、`WebGrid`だけ同様ヘルパーは簡単ですが結果の表示が普通です。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-312">Using the `WebGrid` helper like you just did is easy, but the resulting display is plain.</span></span> <span data-ttu-id="9f3d7-313">`WebGrid`ヘルパーがあらゆる種類のオプションのデータの表示方法を制御することができます。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-313">The `WebGrid` helper has all sorts of options that let you control how the data is displayed.</span></span> <span data-ttu-id="9f3d7-314">このチュートリアルでは探索に非常に多くはありますが、このセクションの内容がこれらのオプションのいくつかのアイデアを与えます。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-314">There are far too many to explore in this tutorial, but this section will give you an idea of some of those options.</span></span> <span data-ttu-id="9f3d7-315">いくつかの追加オプションは、このシリーズの後のチュートリアルで取り上げます。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-315">A few additional options will be covered in later tutorials in this series.</span></span>

### <a name="specifying-individual-columns-to-display"></a><span data-ttu-id="9f3d7-316">表示する個々 の列を指定します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-316">Specifying Individual Columns to Display</span></span>

<span data-ttu-id="9f3d7-317">開始するには、特定の列のみを表示することを指定できます。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-317">To start, you can specify that you want to display only certain columns.</span></span> <span data-ttu-id="9f3d7-318">既定で、既に説明したようがグリッドに表示から列の 4 つのすべての*映画*テーブル。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-318">By default, as you've seen, the grid shows all four of the columns from the *Movies* table.</span></span>

<span data-ttu-id="9f3d7-319">*Movies.cshtml*ファイルで置き換えます、`@grid.GetHtml()`マークアップに次の追加しました。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-319">In the *Movies.cshtml* file, replace the `@grid.GetHtml()` markup that you just added with the following:</span></span>

[!code-css[Main](displaying-data/samples/sample4.css)]

<span data-ttu-id="9f3d7-320">含めるヘルパーを表示する列を確認する、`columns`のパラメーター、`GetHtml`メソッドと列のコレクションを渡します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-320">To tell the helper which columns to display, you include a `columns` parameter for the `GetHtml` method and pass in a collection of columns.</span></span> <span data-ttu-id="9f3d7-321">コレクション内に含める各列を指定します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-321">In the collection, you specify each column to include.</span></span> <span data-ttu-id="9f3d7-322">含めることで表示する個々 の列を指定する、`grid.Column`オブジェクト、およびデータ列名引数として受け取る。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-322">You specify an individual column to display by including a `grid.Column` object, and pass in the name of the data column you want.</span></span> <span data-ttu-id="9f3d7-323">(これらの列は、SQL クエリの結果に含める必要があります:、`WebGrid`ヘルパーは、クエリによって返されたいない列を表示できません)。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-323">(These columns must be included in the SQL query results — the `WebGrid` helper cannot display columns that were not returned by the query.)</span></span>

<span data-ttu-id="9f3d7-324">起動して、 *Movies.cshtml*今度は次のような表示を取得し、もう一度、ブラウザー内のページ (ID 列が表示されていないことに注意してください)。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-324">Launch the *Movies.cshtml* page in the browser again, and this time you get a display like the following one (notice that no ID column is displayed):</span></span>

![選択した列のみを示す WebGrid を表示](displaying-data/_static/image19.png)

### <a name="changing-the-look-of-the-grid"></a><span data-ttu-id="9f3d7-326">グリッドの外観を変更します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-326">Changing the Look of the Grid</span></span>

<span data-ttu-id="9f3d7-327">このセットの後のチュートリアルで詳しく説明するいくつかの列を表示するための多数の他のオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-327">There are quite a few more options for displaying columns, some of which will be explored in later tutorials in this set.</span></span> <span data-ttu-id="9f3d7-328">ここでは、このセクションでは説明方法は、グリッド全体のスタイルを設定できます。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-328">For now, this section will introduce you to ways in which you can style the grid as a whole.</span></span>

<span data-ttu-id="9f3d7-329">内部、`<head>`終了の直前に、ページのセクション`</head>`タグは、次の追加`<style>`要素。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-329">Inside the `<head>` section of the page, just before the closing `</head>` tag, add the following `<style>` element:</span></span>

[!code-css[Main](displaying-data/samples/sample5.css)]

<span data-ttu-id="9f3d7-330">この CSS マークアップがという名前のクラスを定義`grid`、`head`のようにします。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-330">This CSS markup defines classes named `grid`, `head`, and so on.</span></span> <span data-ttu-id="9f3d7-331">別個のこれらのスタイルの定義を配置することもでした*.css*ファイルし、ページにリンクします。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-331">You could also put these style definitions in a separate *.css* file and link that to the page.</span></span> <span data-ttu-id="9f3d7-332">(実際を後ではこのチュートリアルのセットです。)簡単に処理このチュートリアルでは、データを表示する同じページ内います。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-332">(In fact, you'll do that later in this tutorial set.) But to make things easy for this tutorial, they're inside the same page that displays the data.</span></span>

<span data-ttu-id="9f3d7-333">これで、取得することができます、`WebGrid`これらのスタイル クラスを使用するヘルパー。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-333">Now you can get the `WebGrid` helper to use these style classes.</span></span> <span data-ttu-id="9f3d7-334">ヘルパーがいくつかのプロパティ (たとえば、 `tableStyle`) ちょうどこの目的の-には、CSS スタイル クラス名を割り当てるし、そのクラス名は、ヘルパーによって表示されるマークアップの一部として表示されます。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-334">The helper has a number of properties (for example, `tableStyle`) for just this purpose — you assign a CSS style class name to them, and that class name is rendered as part of the markup that's rendered by the helper.</span></span>

<span data-ttu-id="9f3d7-335">変更、`grid.GetHtml`マークアップため、it が次のコードのようになりますの。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-335">Change the `grid.GetHtml` markup so that it now looks like this code:</span></span>

[!code-css[Main](displaying-data/samples/sample6.css)]

<span data-ttu-id="9f3d7-336">どのような新機能は追加した`tableStyle`、 `headerStyle`、および`alternatingRowStyle`パラメーターを`GetHtml`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-336">What's new here is that you've added `tableStyle`, `headerStyle`, and `alternatingRowStyle` parameters to the `GetHtml` method.</span></span> <span data-ttu-id="9f3d7-337">これらのパラメーターは、少し前に追加した CSS スタイルの名前に設定されています。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-337">These parameters have been set to the names of the CSS styles that you added a moment ago.</span></span>

<span data-ttu-id="9f3d7-338">ページを実行し、この時間よりも大幅に低下 plain 前に、を検索するグリッドを表示します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-338">Run the page, and this time you see a grid that looks much less plain than before:</span></span>

![パラメーターが含まれている WebGrid ディスプレイ CSS クラス名に設定](displaying-data/_static/image20.png)

<span data-ttu-id="9f3d7-340">新機能を表示する、`GetHtml`生成されたメソッド、ブラウザー内のページのソースで検索できます。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-340">To see what the `GetHtml` method generated, you can look at the source of the page in the browser.</span></span> <span data-ttu-id="9f3d7-341">ここでは触れませんですが、重要な点のようにパラメーターを指定して`tableStyle`、次のような HTML タグを生成するグリッドの原因とします。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-341">We won't go into detail here, but the important point is that by specifying parameters like `tableStyle`, you caused the grid to generate HTML tags like the following:</span></span>

`<table class="grid">`

<span data-ttu-id="9f3d7-342">`<table>`タグの必要があるが、`class`属性を追加する前に追加した CSS 規則のいずれかを参照します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-342">The `<table>` tag has had a `class` attribute added to it that references one of the CSS rules that you added earlier.</span></span> <span data-ttu-id="9f3d7-343">このコードは、基本的なパターンを示します&mdash;に異なるパラメーター、`GetHtml`メソッド使用すると、参照する CSS クラスのメソッドは、マークアップとし、生成されます。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-343">This code shows you the basic pattern &mdash; different parameters for the `GetHtml` method let you reference CSS classes that the method then generates along with the markup.</span></span> <span data-ttu-id="9f3d7-344">何が CSS クラスを使用する責任です。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-344">What you do with the CSS classes is up to you.</span></span>

## <a name="adding-paging"></a><span data-ttu-id="9f3d7-345">ページングの追加</span><span class="sxs-lookup"><span data-stu-id="9f3d7-345">Adding Paging</span></span>

<span data-ttu-id="9f3d7-346">このチュートリアルの最後のタスクとして、ページングをグリッドに追加します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-346">As the last task for this tutorial, you'll add paging to the grid.</span></span> <span data-ttu-id="9f3d7-347">ここでは、すべての映画を一度に表示する問題ではありません。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-347">Right now it's no problem to display all your movies at once.</span></span> <span data-ttu-id="9f3d7-348">ページの表示が長い取得が何百もの映画を追加した場合。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-348">But if you added hundreds of movies, the page display would get long.</span></span>

<span data-ttu-id="9f3d7-349">ページ コードで作成する 1 行を変更する、`WebGrid`オブジェクトを次のコードに。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-349">In the page code, change the line that creates the `WebGrid` object to the following code:</span></span>

[!code-csharp[Main](displaying-data/samples/sample7.cs)]

<span data-ttu-id="9f3d7-350">唯一の違いから前に追加されていること、 `rowsPerPage` 3 に設定されたパラメーターです。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-350">The only difference from before is that you've added a `rowsPerPage` parameter that's set to 3.</span></span>

<span data-ttu-id="9f3d7-351">ページを実行します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-351">Run the page.</span></span> <span data-ttu-id="9f3d7-352">グリッドには、時間、および、データベース内のムービーをページングするのに便利なナビゲーション リンクに 3 つの行が表示されます。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-352">The grid displays 3 rows at a time, plus navigation links that let you page through the movies in your database:</span></span>

![ページングで WebGrid 表示](displaying-data/_static/image21.png)

## <a name="coming-up-next"></a><span data-ttu-id="9f3d7-354">次へ直近の見通し</span><span class="sxs-lookup"><span data-stu-id="9f3d7-354">Coming Up Next</span></span>

<span data-ttu-id="9f3d7-355">次のチュートリアルでは、Razor と c# コードを使用して、フォームのユーザー入力を取得する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-355">In the next tutorial, you'll learn how to use Razor and C# code to get user input in a form.</span></span> <span data-ttu-id="9f3d7-356">タイトルまたはジャンルで映画を検索できるように、ムービーのページに検索ボックスを追加します。</span><span class="sxs-lookup"><span data-stu-id="9f3d7-356">You'll add a search box to the Movies page so that you can find movies by title or genre.</span></span>

## <a name="complete-listing-for-movies-page"></a><span data-ttu-id="9f3d7-357">ビデオ ページの完全な一覧</span><span class="sxs-lookup"><span data-stu-id="9f3d7-357">Complete Listing for Movies Page</span></span>

[!code-cshtml[Main](displaying-data/samples/sample8.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="9f3d7-358">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="9f3d7-358">Additional Resources</span></span>

- [<span data-ttu-id="9f3d7-359">Razor 構文を使用して ASP.NET Web プログラミングの概要</span><span class="sxs-lookup"><span data-stu-id="9f3d7-359">Introduction to ASP.NET Web Programming Using the Razor Syntax</span></span>](https://go.microsoft.com/fwlink/?LinkID=202890)

>[!div class="step-by-step"]
<span data-ttu-id="9f3d7-360">[前へ](intro-to-web-pages-programming.md)
[次へ](form-basics.md)</span><span class="sxs-lookup"><span data-stu-id="9f3d7-360">[Previous](intro-to-web-pages-programming.md)
[Next](form-basics.md)</span></span>
