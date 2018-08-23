---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
title: ASP.NET Web Pages の概要 - データの表示 |Microsoft Docs
author: tfitzmac
description: このチュートリアルでは、WebMatrix で、データベースを作成する方法と ASP.NET Web Pages (Razor) を使用すると、データベースのデータをページに表示する方法を説明します。 これは、y が前提としています.
ms.author: riande
ms.date: 05/28/2015
ms.assetid: b3a006a0-3ea2-4d45-b833-e20e3a3c0a1a
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
msc.type: authoredcontent
ms.openlocfilehash: 864b9f7826763e307368706116458678abf50d3b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828071"
---
<a name="introducing-aspnet-web-pages---displaying-data"></a><span data-ttu-id="70dda-104">ASP.NET Web ページの概要 - データを表示します。</span><span class="sxs-lookup"><span data-stu-id="70dda-104">Introducing ASP.NET Web Pages - Displaying Data</span></span>
====================
<span data-ttu-id="70dda-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="70dda-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="70dda-106">このチュートリアルでは、WebMatrix で、データベースを作成する方法と ASP.NET Web Pages (Razor) を使用すると、データベースのデータをページに表示する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="70dda-106">This tutorial shows you how to create a database in WebMatrix and how to display database data in a page when you use ASP.NET Web Pages (Razor).</span></span> <span data-ttu-id="70dda-107">を通じてシリーズを完了したと想定して[ASP.NET Web Pages のプログラミングの概要](../introducing-razor-syntax-c.md)します。</span><span class="sxs-lookup"><span data-stu-id="70dda-107">It assumes you have completed the series through [Introduction to ASP.NET Web Pages Programming](../introducing-razor-syntax-c.md).</span></span>
> 
> <span data-ttu-id="70dda-108">学習内容。</span><span class="sxs-lookup"><span data-stu-id="70dda-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="70dda-109">WebMatrix ツールを使用して、データベースとデータベース テーブルを作成する方法。</span><span class="sxs-lookup"><span data-stu-id="70dda-109">How to use WebMatrix tools to create a database and database tables.</span></span>
> - <span data-ttu-id="70dda-110">WebMatrix ツールを使用してデータベースにデータを追加する方法。</span><span class="sxs-lookup"><span data-stu-id="70dda-110">How to use WebMatrix tools to add data to a database.</span></span>
> - <span data-ttu-id="70dda-111">ページに、データベースからデータを表示する方法。</span><span class="sxs-lookup"><span data-stu-id="70dda-111">How to display data from the database on a page.</span></span>
> - <span data-ttu-id="70dda-112">ASP.NET Web Pages での SQL コマンドを実行する方法。</span><span class="sxs-lookup"><span data-stu-id="70dda-112">How to run SQL commands in ASP.NET Web Pages.</span></span>
> - <span data-ttu-id="70dda-113">カスタマイズする方法、`WebGrid`ヘルパー データ表示を変更して、ページングと並べ替えを追加します。</span><span class="sxs-lookup"><span data-stu-id="70dda-113">How to customize the `WebGrid` helper to change the data display and to add paging and sorting.</span></span>
>   
> 
> <span data-ttu-id="70dda-114">説明した機能/テクノロジ:</span><span class="sxs-lookup"><span data-stu-id="70dda-114">Features/technologies discussed:</span></span>
> 
> - <span data-ttu-id="70dda-115">WebMatrix のデータベース ツールです。</span><span class="sxs-lookup"><span data-stu-id="70dda-115">WebMatrix database tools.</span></span>
> - <span data-ttu-id="70dda-116">`WebGrid` ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="70dda-116">`WebGrid` helper.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="70dda-117">構築します</span><span class="sxs-lookup"><span data-stu-id="70dda-117">What You'll Build</span></span>

<span data-ttu-id="70dda-118">前のチュートリアルでは ASP.NET Web ページに導入された (*.cshtml*ファイル)、Razor の構文の基本およびヘルパー。</span><span class="sxs-lookup"><span data-stu-id="70dda-118">In the previous tutorial, you were introduced to ASP.NET Web Pages (*.cshtml* files), to the basics of Razor syntax, and to helpers.</span></span> <span data-ttu-id="70dda-119">このチュートリアルでは、残りの系列に使用する実際の web アプリケーションを作成するが始めます。</span><span class="sxs-lookup"><span data-stu-id="70dda-119">In this tutorial, you'll begin creating the actual web application that you'll use for the rest of the series.</span></span> <span data-ttu-id="70dda-120">アプリは、簡単なムービー アプリケーションを表示、追加、変更、および映画に関する情報を削除することができます。</span><span class="sxs-lookup"><span data-stu-id="70dda-120">The app is a simple movie application that lets you view, add, change, and delete information about movies.</span></span>

<span data-ttu-id="70dda-121">このチュートリアルを完了したら、このページのようなムービーの一覧を表示することができます。</span><span class="sxs-lookup"><span data-stu-id="70dda-121">When you're done with this tutorial, you'll be able to view a movie listing that looks like this page:</span></span>

![CSS クラス名に設定するパラメーターが含まれている WebGrid の表示](displaying-data/_static/image1.png)

<span data-ttu-id="70dda-123">開始するにはデータベースを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="70dda-123">But to begin, you have to create a database.</span></span>

## <a name="a-very-brief-introduction-to-databases"></a><span data-ttu-id="70dda-124">データベースを非常に簡単に紹介</span><span class="sxs-lookup"><span data-stu-id="70dda-124">A Very Brief Introduction to Databases</span></span>

<span data-ttu-id="70dda-125">このチュートリアルでは、データベースを簡単に述べます概要のみを提供します。</span><span class="sxs-lookup"><span data-stu-id="70dda-125">This tutorial will provide only the briefest introduction to databases.</span></span> <span data-ttu-id="70dda-126">データベースの経験がある場合は、この短いセクションをスキップできます。</span><span class="sxs-lookup"><span data-stu-id="70dda-126">If you have database experience, you can skip this short section.</span></span>

<span data-ttu-id="70dda-127">データベースに情報を含む 1 つまたは複数のテーブルが含まれている&mdash;などを顧客、注文、および仕入先、またはの学生、教員、クラス、および成績をテーブルします。</span><span class="sxs-lookup"><span data-stu-id="70dda-127">A database contains one or more tables that contain information &mdash; for example, tables for customers, orders, and vendors, or for students, teachers, classes, and grades.</span></span> <span data-ttu-id="70dda-128">構造上、データベース テーブルは、スプレッドシートのようにします。</span><span class="sxs-lookup"><span data-stu-id="70dda-128">Structurally, a database table is like a spreadsheet.</span></span> <span data-ttu-id="70dda-129">通常、アドレス帳を想像してください。</span><span class="sxs-lookup"><span data-stu-id="70dda-129">Imagine a typical address book.</span></span> <span data-ttu-id="70dda-130">アドレス帳のエントリごとに (つまり、各人の) 名、姓、住所、電子メール アドレス、および電話番号などのいくつかの情報があります。</span><span class="sxs-lookup"><span data-stu-id="70dda-130">For each entry in the address book (that is, for each person) you have several pieces of information such as first name, last name, address, email address, and phone number.</span></span>

![サンプル データベースのテーブルとして単純なグリッド](displaying-data/_static/image2.png)

<span data-ttu-id="70dda-132">(行と呼ばれることもあります*レコード*、列と呼ばれることがありますと*フィールド*)。</span><span class="sxs-lookup"><span data-stu-id="70dda-132">(Rows are sometimes referred to as *records*, and columns are sometimes referred to as *fields*.)</span></span>

<span data-ttu-id="70dda-133">ほとんどのデータベース テーブル、テーブルは、顧客番号、アカウントの数などの一意の値を含む列がある必要があります。</span><span class="sxs-lookup"><span data-stu-id="70dda-133">For most database tables, the table has to have a column that contains a unique value, like a customer number, account number, and so on.</span></span> <span data-ttu-id="70dda-134">この値が、テーブルのと呼ばれる*主キー*テーブル内の各行を識別するために使用するとします。</span><span class="sxs-lookup"><span data-stu-id="70dda-134">This value is known as the table's *primary key*, and you use it to identify each row in the table.</span></span> <span data-ttu-id="70dda-135">例では、ID 列は、前の例に示すように、アドレス帳の主キーです。</span><span class="sxs-lookup"><span data-stu-id="70dda-135">In the example, the ID column is the primary key for the address book shown in the previous example.</span></span>

<span data-ttu-id="70dda-136">Web アプリケーションでは、作業の多くは、データベースから情報を読み取ると、ページに表示することで構成されます。</span><span class="sxs-lookup"><span data-stu-id="70dda-136">Much of the work you do in web applications consists of reading information out of the database and displaying it on a page.</span></span> <span data-ttu-id="70dda-137">多くの場合もユーザーから情報を収集し、データベースに追加します。 または、データベースに既にあるレコードを変更します。</span><span class="sxs-lookup"><span data-stu-id="70dda-137">You'll also often gather information from users and add it to a database, or you'll modify records that are already in the database.</span></span> <span data-ttu-id="70dda-138">(すべて取り上げますこの一連のチュートリアルの過程でこれらの操作です。)</span><span class="sxs-lookup"><span data-stu-id="70dda-138">(We'll cover all of these operations in the course of this tutorial set.)</span></span>

<span data-ttu-id="70dda-139">データベースの作業は非常に複雑になることができ、専門知識を要求できます。</span><span class="sxs-lookup"><span data-stu-id="70dda-139">Database work can be enormously complex and can require specialized knowledge.</span></span> <span data-ttu-id="70dda-140">このチュートリアルのセットが、理解する必要のみ基本的な概念はすべて説明します。</span><span class="sxs-lookup"><span data-stu-id="70dda-140">For this tutorial set, though, you have to understand only basic concepts, which will all be explained as you go.</span></span>

## <a name="creating-a-database"></a><span data-ttu-id="70dda-141">データベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="70dda-141">Creating a Database</span></span>

<span data-ttu-id="70dda-142">WebMatrix には、簡単にデータベースを作成して、データベースにテーブルを作成するためのツールが含まれています。</span><span class="sxs-lookup"><span data-stu-id="70dda-142">WebMatrix includes tools that make it easy to create a database and to create tables in the database.</span></span> <span data-ttu-id="70dda-143">(データベースの構造は、データベースと呼ばれます*スキーマ*)。このチュートリアルのセットが 1 つのテーブルを含むデータベースを作成します&mdash;ビデオ。</span><span class="sxs-lookup"><span data-stu-id="70dda-143">(The structure of a database is referred to as the database's *schema*.) For this tutorial set, you'll create a database that has only one table in it &mdash; Movies.</span></span>

<span data-ttu-id="70dda-144">これをまだ完了していない場合は、WebMatrix を開き、前のチュートリアルで作成した WebPagesMovies サイトを開きます。</span><span class="sxs-lookup"><span data-stu-id="70dda-144">Open WebMatrix if you haven't already done so, and open the WebPagesMovies site that you created in the previous tutorial.</span></span>

<span data-ttu-id="70dda-145">左側のウィンドウでをクリックして、**データベース**ワークスペース。</span><span class="sxs-lookup"><span data-stu-id="70dda-145">In the left pane, click the **Database** workspace.</span></span>

![WebMatrix の Database ワークスペース タブ](displaying-data/_static/image3.png)

<span data-ttu-id="70dda-147">データベース関連のタスクを表示するリボン変更します。</span><span class="sxs-lookup"><span data-stu-id="70dda-147">The ribbon changes to show database-related tasks.</span></span> <span data-ttu-id="70dda-148">リボンで、次のようにクリックします。**新しいデータベース**します。</span><span class="sxs-lookup"><span data-stu-id="70dda-148">In the ribbon, click **New Database**.</span></span>

![WebMatrix リボンの [新しいデータベース] ボタンをクリックします。](displaying-data/_static/image4.png)

<span data-ttu-id="70dda-150">WebMatrix には、SQL Server CE データベースが作成されます (、 *.sdf*ファイル)、サイトと同じ名前を持つ&mdash; *WebPagesMovies.sdf*します。</span><span class="sxs-lookup"><span data-stu-id="70dda-150">WebMatrix creates a SQL Server CE database (an *.sdf* file) that has the same name as your site &mdash; *WebPagesMovies.sdf*.</span></span> <span data-ttu-id="70dda-151">(ここでは、この操作も行いませんがある限り、好きなデザインにファイルを変更することができます、 *.sdf*拡張機能です)。</span><span class="sxs-lookup"><span data-stu-id="70dda-151">(You won't do this here, but you can rename the file to anything you like, as long as it has an *.sdf* extension.)</span></span>

## <a name="creating-a-table"></a><span data-ttu-id="70dda-152">テーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="70dda-152">Creating a Table</span></span>

<span data-ttu-id="70dda-153">リボンで、次のようにクリックします。**新しいテーブル**します。</span><span class="sxs-lookup"><span data-stu-id="70dda-153">In the ribbon, click **New Table**.</span></span> <span data-ttu-id="70dda-154">WebMatrix は、新しいタブで、テーブル デザイナーを開きます。(新しいテーブル オプションを使用できない場合は、左側のツリー ビューで、新しいデータベースが選択されていることを確認)。</span><span class="sxs-lookup"><span data-stu-id="70dda-154">WebMatrix opens the table designer in a new tab. (If the New Table option isn't available, make sure that the new database is selected in the tree view on the left.)</span></span>

!['新しいテーブル' WebMatrix リボンをボタンします。](displaying-data/_static/image5.png)

<span data-ttu-id="70dda-156">(ウォーターマークはという「」と入力テーブル名」) の上部にあるテキスト ボックスで、"Movies"を入力します。</span><span class="sxs-lookup"><span data-stu-id="70dda-156">In the text box at the top (where the watermark says "Enter table name"), enter "Movies".</span></span>

![WebMatrix のデータベース デザイナーでテーブル名を入力します。](displaying-data/_static/image6.png)

<span data-ttu-id="70dda-158">テーブル名の下のウィンドウでは、個々 の列を定義します。</span><span class="sxs-lookup"><span data-stu-id="70dda-158">The pane underneath the table name is where you define individual columns.</span></span> <span data-ttu-id="70dda-159">*映画*テーブルでは、このチュートリアルには、少数の列を作成します: *ID*、*タイトル*、*ジャンル*、および*年*.</span><span class="sxs-lookup"><span data-stu-id="70dda-159">For the *Movies* table in this tutorial, you'll create only a few columns: *ID*, *Title*, *Genre*, and *Year*.</span></span>

<span data-ttu-id="70dda-160">**名前**ボックスに、"ID"を入力します。</span><span class="sxs-lookup"><span data-stu-id="70dda-160">In the **Name** box, enter "ID".</span></span> <span data-ttu-id="70dda-161">ここで値を入力するには、新しい列のすべてのコントロールがアクティブにします。</span><span class="sxs-lookup"><span data-stu-id="70dda-161">Entering a value here activates all the controls for the new column.</span></span>

<span data-ttu-id="70dda-162">タブ、**データ型**一覧**int**します。この値は、ID 列が整数型 (数値) のデータを含めることを指定します。</span><span class="sxs-lookup"><span data-stu-id="70dda-162">Tab to the **Data Type** list and choose **int**. This value specifies that the ID column will contain integer (number) data.</span></span>

> [!NOTE]
> <span data-ttu-id="70dda-163">しません名前にしたいずれかを確認してください (あまり) 言うが標準 Windows キーボードのジェスチャを使用して、このグリッド内で移動することができます。</span><span class="sxs-lookup"><span data-stu-id="70dda-163">We won't call it out any more here (much), but you can use standard Windows keyboard gestures to navigate in this grid.</span></span> <span data-ttu-id="70dda-164">たとえば、フィールド間にタブは、だけ、一覧で項目を選択するために入力を開始し、そのことができます。</span><span class="sxs-lookup"><span data-stu-id="70dda-164">For example, you can tab between fields, you can just start typing in order to select an item in a list, and so on.</span></span>


<span data-ttu-id="70dda-165">過去のタブ、**既定値**ボックス (つまり、空白のままに)。</span><span class="sxs-lookup"><span data-stu-id="70dda-165">Tab past the **Default Value** box (that is, leave it blank).</span></span> <span data-ttu-id="70dda-166">タブ、**プライマリ キーである** チェック ボックスを選択します。</span><span class="sxs-lookup"><span data-stu-id="70dda-166">Tab to the **Is Primary Key** check box and select it.</span></span> <span data-ttu-id="70dda-167">このオプションにより、データベースを*ID*列は個々 の行を識別するデータが格納されます。</span><span class="sxs-lookup"><span data-stu-id="70dda-167">This option tells the database that the *ID* column will contain the data that identifies individual rows.</span></span> <span data-ttu-id="70dda-168">(は、各行が一意の値をその行を検索するのに使用できる ID 列。)</span><span class="sxs-lookup"><span data-stu-id="70dda-168">(That is, each row will have a unique value in the ID column that you can use to find that row.)</span></span>

<span data-ttu-id="70dda-169">選択、 **Id**オプション。</span><span class="sxs-lookup"><span data-stu-id="70dda-169">Choose the **Is Identity** option.</span></span> <span data-ttu-id="70dda-170">このオプションは、新しい行ごとに次の連番を自動的に生成することをデータベースに指示します。</span><span class="sxs-lookup"><span data-stu-id="70dda-170">This option tells the database that it should automatically generate the next sequential number for each new row.</span></span> <span data-ttu-id="70dda-171">(、 **Id**オプションを選択した場合にのみ使用できる、**プライマリ キーである**オプション)。</span><span class="sxs-lookup"><span data-stu-id="70dda-171">(The **Is Identity** option works only if you've also selected the **Is Primary Key** option.)</span></span>

<span data-ttu-id="70dda-172">次のグリッド行をクリックしてまたは現在の行を終了するには、2 回 Tab キーを押します。</span><span class="sxs-lookup"><span data-stu-id="70dda-172">Click in the next grid row, or press Tab twice to finish the current row.</span></span> <span data-ttu-id="70dda-173">いずれかのジェスチャは、現在の行を保存し、[次へ] の 1 つを開始します。</span><span class="sxs-lookup"><span data-stu-id="70dda-173">Either gesture saves the current row and starts the next one.</span></span> <span data-ttu-id="70dda-174">注意、**既定値**列伝える**Null**します。</span><span class="sxs-lookup"><span data-stu-id="70dda-174">Notice that the **Default Value** column now says **Null**.</span></span> <span data-ttu-id="70dda-175">(Null、既定値は、既定値のいわば。)</span><span class="sxs-lookup"><span data-stu-id="70dda-175">(Null is the default value for the default value, so to speak.)</span></span>

<span data-ttu-id="70dda-176">新しい定義が完了したら**ID**デザイナーが次の図のようになりますが、列。</span><span class="sxs-lookup"><span data-stu-id="70dda-176">When you've finished defining the new **ID** column, the designer will look like this illustration:</span></span>

![映画テーブルの ID 列を定義した後、WebMatrix データベース デザイナー](displaying-data/_static/image7.png)

<span data-ttu-id="70dda-178">次の列を作成するのボックス内をクリックして、**名前**列。</span><span class="sxs-lookup"><span data-stu-id="70dda-178">To create the next column, click in the box in the **Name** column.</span></span> <span data-ttu-id="70dda-179">列の"Title"を入力し、 **nvarchar**の**データ型**値。</span><span class="sxs-lookup"><span data-stu-id="70dda-179">Enter "Title" for the column and then select **nvarchar** for the **Data Type** value.</span></span> <span data-ttu-id="70dda-180">"Var"一部**nvarchar**データベースにこの列のデータ サイズは、レコード間を異なる場合があります文字列になることを指示します。</span><span class="sxs-lookup"><span data-stu-id="70dda-180">The "var" part of **nvarchar** tells the database that the data for this column will be a string whose size might vary from record to record.</span></span> <span data-ttu-id="70dda-181">("N"プレフィックスを表すフィールドがすべてアルファベットまたは書記体系の文字データを保持できることを示す「国」など、フィールドは、Unicode データを保持する)。</span><span class="sxs-lookup"><span data-stu-id="70dda-181">(The "n" prefix represents "national," which indicates that the field can hold character data for any alphabet or writing system — that is, the field holds Unicode data.)</span></span>

<span data-ttu-id="70dda-182">選択すると**nvarchar**、もう 1 つのボックスは、フィールドの最大長を入力できますが表示されます。</span><span class="sxs-lookup"><span data-stu-id="70dda-182">When you choose **nvarchar**, another box appears where you can enter the maximum length for the field.</span></span> <span data-ttu-id="70dda-183">このチュートリアルで使用するムービーのタイトルはれないこと 50 文字より長くを前提としての 50」と入力します。</span><span class="sxs-lookup"><span data-stu-id="70dda-183">Enter 50, on the assumption that no movie title that you'll work with in this tutorial will be longer than 50 characters.</span></span>

<span data-ttu-id="70dda-184">Skip**既定値**をオフにし、 **Null を許容**オプション。</span><span class="sxs-lookup"><span data-stu-id="70dda-184">Skip **Default Value** and clear the **Allow Nulls** option.</span></span> <span data-ttu-id="70dda-185">タイトルがないようなデータベースに入力される映画を許可するデータベースは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="70dda-185">You don't want the database to allow any movies to be entered into the database that don't have a title.</span></span>

<span data-ttu-id="70dda-186">完了したら、次の行に移動すると、デザイナーは、次の図のようになります。</span><span class="sxs-lookup"><span data-stu-id="70dda-186">When you're done and move to the next row, the designer looks like this illustration:</span></span>

![映画テーブルのタイトル列を定義した後、WebMatrix データベース デザイナー](displaying-data/_static/image8.png)

<span data-ttu-id="70dda-188">長さを除く"Genre"をという名前の列を作成する手順だけ 30 に設定を繰り返します。</span><span class="sxs-lookup"><span data-stu-id="70dda-188">Repeat these steps to create a column named "Genre", except for the length, set it to just 30.</span></span>

<span data-ttu-id="70dda-189">"Year"という名前の別の列を作成します。</span><span class="sxs-lookup"><span data-stu-id="70dda-189">Create another column named "Year."</span></span> <span data-ttu-id="70dda-190">データ型では、選択**nchar** (いない**nvarchar**) と長さは 4 に設定します。</span><span class="sxs-lookup"><span data-stu-id="70dda-190">For the data type, choose **nchar** (not **nvarchar**) and set the length to 4.</span></span> <span data-ttu-id="70dda-191">年、しようとしている「1995」または「2010」のように 4 桁の数字を使用して、可変サイズの列を必要としないようにします。</span><span class="sxs-lookup"><span data-stu-id="70dda-191">For the year, you're going to use a 4-digit number like "1995" or "2010", so you don't require a variable-sized column.</span></span>

<span data-ttu-id="70dda-192">完成した設計がどのようにを次に示します。</span><span class="sxs-lookup"><span data-stu-id="70dda-192">Here's what the finished design looks like:</span></span>

![映画のテーブルのすべてのフィールドを定義した後、WebMatrix データベース デザイナー](displaying-data/_static/image9.png)

<span data-ttu-id="70dda-194">Ctrl キーを押しながら S キーを押すか、クリックして、**保存**クイック アクセス ツールバーのボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="70dda-194">Press Ctrl+S or click the **Save** button in the Quick Access toolbar.</span></span> <span data-ttu-id="70dda-195">タブを閉じるには、データベース デザイナーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="70dda-195">Close the database designer by closing the tab.</span></span>

## <a name="adding-some-example-data"></a><span data-ttu-id="70dda-196">いくつかの例のデータを追加します。</span><span class="sxs-lookup"><span data-stu-id="70dda-196">Adding Some Example Data</span></span>

<span data-ttu-id="70dda-197">このチュートリアル シリーズの後半では、フォームに新しい映画を入力 ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="70dda-197">Later in this tutorial series you'll create a page where you can enter new movies in a form.</span></span> <span data-ttu-id="70dda-198">ただし、ここには、ページに表示できる、いくつかのサンプル データを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="70dda-198">For now, however, you can add some example data that you can then display on a page.</span></span>

<span data-ttu-id="70dda-199">**データベース** ワークスペースで、WebMatrix では、ツリーがあることに注意してください、 *.sdf*以前に作成したファイル。</span><span class="sxs-lookup"><span data-stu-id="70dda-199">In the **Database** workspace in WebMatrix, notice that there's a tree that shows you the *.sdf* file you created earlier.</span></span> <span data-ttu-id="70dda-200">新しいノードを開いて *.sdf*ファイルを開き、**テーブル**ノード。</span><span class="sxs-lookup"><span data-stu-id="70dda-200">Open the node for your new *.sdf* file, and then open the **Tables** node.</span></span>

![ツリーの映画のテーブルを開くと WebMatrix の Database ワークスペース](displaying-data/_static/image10.png)

<span data-ttu-id="70dda-202">右クリックし、**映画**ノード選び、**データ**します。</span><span class="sxs-lookup"><span data-stu-id="70dda-202">Right-click the **Movies** node and then choose **Data**.</span></span> <span data-ttu-id="70dda-203">WebMatrix は、グリッドのデータを入力することができますを開き、*映画*テーブル。</span><span class="sxs-lookup"><span data-stu-id="70dda-203">WebMatrix opens a grid where you can enter data for the *Movies* table:</span></span>

![WebMatrix (空) のデータベース エントリ グリッド](displaying-data/_static/image11.png)

<span data-ttu-id="70dda-205">をクリックして、**タイトル**列"と Harry 満たす Sally"を入力します。</span><span class="sxs-lookup"><span data-stu-id="70dda-205">Click the **Title** column and enter "When Harry Met Sally".</span></span> <span data-ttu-id="70dda-206">移動、**ジャンル**(Tab キーを使用することができます) の列「Romantic コメディ」を入力します。</span><span class="sxs-lookup"><span data-stu-id="70dda-206">Move to the **Genre** column (you can use the Tab key) and enter "Romantic Comedy".</span></span> <span data-ttu-id="70dda-207">移動、**年**列「1989」を入力します。</span><span class="sxs-lookup"><span data-stu-id="70dda-207">Move to the **Year** column and enter "1989":</span></span>

![1 つのレコードを WebMatrix でのデータベース エントリ グリッド](displaying-data/_static/image12.png)

<span data-ttu-id="70dda-209">WebMatrix と enter キーを押して、新しいムービーを保存します。</span><span class="sxs-lookup"><span data-stu-id="70dda-209">Press Enter, and WebMatrix saves the new movie.</span></span> <span data-ttu-id="70dda-210">注意、 **ID**列が入力されています。</span><span class="sxs-lookup"><span data-stu-id="70dda-210">Notice that the **ID** column has been filled in.</span></span>

![1 つのレコードと自動生成された ID を持つ WebMatrix でのデータベース エントリ グリッド](displaying-data/_static/image13.png)

<span data-ttu-id="70dda-212">別のビデオ (たとえば、「風と共に去り」、「ドラマ」、「1939」) を入力します。</span><span class="sxs-lookup"><span data-stu-id="70dda-212">Enter another movie (for example, "Gone with the Wind", "Drama", "1939").</span></span> <span data-ttu-id="70dda-213">ID 列はもう一度入力されます。</span><span class="sxs-lookup"><span data-stu-id="70dda-213">The ID column is filled in again:</span></span>

![WebMatrix で 2 つのレコードと、自動生成された Id のデータベース エントリ グリッド](displaying-data/_static/image14.png)

<span data-ttu-id="70dda-215">3 番目のビデオ (たとえば、"Ghostbusters"、「コメディ」) を入力します。</span><span class="sxs-lookup"><span data-stu-id="70dda-215">Enter a third movie (for example, "Ghostbusters", "Comedy").</span></span> <span data-ttu-id="70dda-216">試しのままに、**年**列空白し、し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="70dda-216">As an experiment, leave the **Year** column blank and then press Enter.</span></span> <span data-ttu-id="70dda-217">選択を解除するため、 **Null を許容**オプション、データベースには、エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="70dda-217">Because you unselected the **Allow Nulls** option, the database shows an error:</span></span>

![必要な列の値が空白の場合、「データが無効です」エラーが表示されます。](displaying-data/_static/image15.png)

<span data-ttu-id="70dda-219">クリックして**OK**に戻るし、("Ghostbusters"の年は 1984) エントリを修正し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="70dda-219">Click **OK** to go back and fix the entry (the year for "Ghostbusters" is 1984), and then press Enter.</span></span>

<span data-ttu-id="70dda-220">8 になるまで、いくつかの映画を入力します。</span><span class="sxs-lookup"><span data-stu-id="70dda-220">Fill in several movies until you have 8 or so.</span></span> <span data-ttu-id="70dda-221">(8 を入力しやすく後でページングを使用します。</span><span class="sxs-lookup"><span data-stu-id="70dda-221">(Entering 8 makes it easier to work with paging later.</span></span> <span data-ttu-id="70dda-222">多すぎる場合は、ここで少しだけを入力します)。実際のデータは関係ありません。</span><span class="sxs-lookup"><span data-stu-id="70dda-222">But if that's too many, enter just a few for now.) The actual data doesn't matter.</span></span>

![いずれかのレコードを WebMatrix でのデータベース エントリ グリッド](displaying-data/_static/image16.png)

<span data-ttu-id="70dda-224">エラーを発生させず、すべての映画を入力した場合、ID の値はシーケンシャルです。</span><span class="sxs-lookup"><span data-stu-id="70dda-224">If you entered all the movies without any errors, the ID values are sequential.</span></span> <span data-ttu-id="70dda-225">ムービーの不完全なレコードを保存しようとすると、ID 番号はシーケンシャルできない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="70dda-225">If you tried to save an incomplete movie record, the ID numbers might not be sequential.</span></span> <span data-ttu-id="70dda-226">場合は、それでも大丈夫です。</span><span class="sxs-lookup"><span data-stu-id="70dda-226">If so, that's okay.</span></span> <span data-ttu-id="70dda-227">数値は、固有の意味がないし、内で一意であるは重要であるだけです、*映画*テーブル。</span><span class="sxs-lookup"><span data-stu-id="70dda-227">The numbers don't have any inherent meaning, and the only thing that's important is that they're unique in the *Movies* table.</span></span>

<span data-ttu-id="70dda-228">データベース デザイナーを含むタブを閉じます。</span><span class="sxs-lookup"><span data-stu-id="70dda-228">Close the tab that contains the database designer.</span></span>

<span data-ttu-id="70dda-229">これで web ページでこのデータの表示にできます。</span><span class="sxs-lookup"><span data-stu-id="70dda-229">Now you can turn to displaying this data on a web page.</span></span>

## <a name="displaying-data-in-a-page-by-using-the-webgrid-helper"></a><span data-ttu-id="70dda-230">ページの WebGrid ヘルパーを使用してデータを表示します。</span><span class="sxs-lookup"><span data-stu-id="70dda-230">Displaying Data in a Page by Using the WebGrid Helper</span></span>

<span data-ttu-id="70dda-231">ページにデータを表示することを使用する、`WebGrid`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="70dda-231">To display data in a page, you're going to use the `WebGrid` helper.</span></span> <span data-ttu-id="70dda-232">このヘルパーは、グリッドまたはテーブル (行および列) での表示を生成します。</span><span class="sxs-lookup"><span data-stu-id="70dda-232">This helper produces a display in a grid or table (rows and columns).</span></span> <span data-ttu-id="70dda-233">後ほど説明するようにできる絞り込み、グリッドに書式設定、およびその他の機能になります。</span><span class="sxs-lookup"><span data-stu-id="70dda-233">As you'll see, you'll be able refine the grid with formatting and other features.</span></span>

<span data-ttu-id="70dda-234">グリッドを実行するには、数行のコードを記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="70dda-234">To run the grid, you'll have to write a few lines of code.</span></span> <span data-ttu-id="70dda-235">これらのいくつかの行は、このチュートリアルでは、データ アクセスのほぼすべてのパターンの一種として使用されます。</span><span class="sxs-lookup"><span data-stu-id="70dda-235">These few lines will serve as a kind of pattern for almost all of the data access that you do in this tutorial.</span></span>

> [!NOTE]
> <span data-ttu-id="70dda-236">実際には、ページ上のデータを表示するためのさまざまなオプションがあります。`WebGrid`ヘルパーは、1 つにすぎません。</span><span class="sxs-lookup"><span data-stu-id="70dda-236">You actually have many options for displaying data on a page; the `WebGrid` helper is just one.</span></span> <span data-ttu-id="70dda-237">選択した、このチュートリアルではデータを表示する最も簡単な方法であるとある程度の柔軟性があるためです。</span><span class="sxs-lookup"><span data-stu-id="70dda-237">We chose it for this tutorial because it's the easiest way to display data and because it's reasonably flexible.</span></span> <span data-ttu-id="70dda-238">次のチュートリアルのセットでは、複数の「手動」方法 ページで、データを表示する方法をより直接的に制御を提供するデータを操作する方法を確認します。</span><span class="sxs-lookup"><span data-stu-id="70dda-238">In the next tutorial set, you'll see how to use a more "manual" way to work with data in the page, which gives you more direct control over how to display the data.</span></span>


<span data-ttu-id="70dda-239">WebMatrix で左側のウィンドウでをクリックして、**ファイル**ワークスペース。</span><span class="sxs-lookup"><span data-stu-id="70dda-239">In the left pane in WebMatrix, click the **Files** workspace.</span></span>

<span data-ttu-id="70dda-240">作成した新しいデータベースが、*アプリ\_データ*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="70dda-240">The new database you created is in the *App\_Data* folder.</span></span> <span data-ttu-id="70dda-241">フォルダーが存在しない場合、WebMatrix により、新しいデータベースに作成されます。</span><span class="sxs-lookup"><span data-stu-id="70dda-241">If the folder didn't already exist, WebMatrix created it for your new database.</span></span> <span data-ttu-id="70dda-242">(フォルダーがありますが存在してヘルパーをインストールした場合。)</span><span class="sxs-lookup"><span data-stu-id="70dda-242">(The folder might have existed if you'd previously installed helpers.)</span></span>

<span data-ttu-id="70dda-243">ツリー ビューでは、web サイトのルートを選択します。</span><span class="sxs-lookup"><span data-stu-id="70dda-243">In the tree view, select the root of the website.</span></span> <span data-ttu-id="70dda-244">Web サイトのルートを選択する必要がありますアプリに新しいファイルを追加する場合がありますそれ以外の場合、\_データ フォルダー。</span><span class="sxs-lookup"><span data-stu-id="70dda-244">You must select the root of the website; otherwise, the new file might be added to the App\_Data folder.</span></span>

<span data-ttu-id="70dda-245">リボンで、次のようにクリックします。**新規**します。</span><span class="sxs-lookup"><span data-stu-id="70dda-245">In the ribbon, click **New**.</span></span> <span data-ttu-id="70dda-246">**ファイルの種類を選択**ボックスで、選択**CSHTML**します。</span><span class="sxs-lookup"><span data-stu-id="70dda-246">In the **Choose a File Type** box, choose **CSHTML**.</span></span>

<span data-ttu-id="70dda-247">**名前**ボックスに、新しいページ"Movies.cshtml"の名前します。</span><span class="sxs-lookup"><span data-stu-id="70dda-247">In the **Name** box, name the new page "Movies.cshtml":</span></span>

!['映画' ページを表示するファイルの種類の選択 ダイアログ ボックス](displaying-data/_static/image17.png)

<span data-ttu-id="70dda-249">をクリックして、 **OK**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="70dda-249">Click the **OK** button.</span></span> <span data-ttu-id="70dda-250">WebMatrix は、いくつかスケルトン要素で新しいファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="70dda-250">WebMatrix opens a new file with some skeleton elements in it.</span></span> <span data-ttu-id="70dda-251">まず、データベースからデータを取得するためのコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="70dda-251">First you'll write some code to go get the data from the database.</span></span> <span data-ttu-id="70dda-252">実際にデータを表示するページにマークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="70dda-252">Then you'll add markup to the page to actually display the data.</span></span>

### <a name="writing-the-data-query-code"></a><span data-ttu-id="70dda-253">データのクエリのコードの記述</span><span class="sxs-lookup"><span data-stu-id="70dda-253">Writing the Data Query Code</span></span>

<span data-ttu-id="70dda-254">ページの上部にある間、`@{`と`}`文字は、次のコードを入力します。</span><span class="sxs-lookup"><span data-stu-id="70dda-254">At the top of the page, between the `@{` and `}` characters, enter the following code.</span></span> <span data-ttu-id="70dda-255">(作成と右中かっこの間、このコードを入力してください)。</span><span class="sxs-lookup"><span data-stu-id="70dda-255">(Make sure that you enter this code between the opening and closing braces.)</span></span>

[!code-csharp[Main](displaying-data/samples/sample1.cs)]

<span data-ttu-id="70dda-256">最初の行では、これは常に、まず、データベースで何かを実行する前に、先ほど作成したデータベースを開きます。</span><span class="sxs-lookup"><span data-stu-id="70dda-256">The first line opens the database that you created earlier, which is always the first step before doing something with the database.</span></span> <span data-ttu-id="70dda-257">わかる、`Database.Open`データベースを開くのメソッド名。</span><span class="sxs-lookup"><span data-stu-id="70dda-257">You tell the `Database.Open` method name of the database to open.</span></span> <span data-ttu-id="70dda-258">含まれていないことに注意してください *.sdf*名にします。</span><span class="sxs-lookup"><span data-stu-id="70dda-258">Notice that you don't include *.sdf* in the name.</span></span> <span data-ttu-id="70dda-259">`Open`メソッド探すことを想定しています、 *.sdf*ファイル (つまり、 *WebPagesMovies.sdf*) と、 *.sdf*ファイルは、 *アプリ\_データ*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="70dda-259">The `Open` method assumes that it's looking for an *.sdf* file (that is, *WebPagesMovies.sdf*) and that the *.sdf* file is in the *App\_Data* folder.</span></span> <span data-ttu-id="70dda-260">(先ほど述べたようにする、*アプリ\_データ*フォルダーが予約されていますこのシナリオでは、ASP.NET は、その名前を推測する場所の 1 つです。)。</span><span class="sxs-lookup"><span data-stu-id="70dda-260">(Earlier we noted that the *App\_Data* folder is reserved; this scenario is one of the places where ASP.NET makes assumptions about that name.)</span></span>

<span data-ttu-id="70dda-261">参照がという名前の変数に配置されているデータベースが開かれると、`db`します。</span><span class="sxs-lookup"><span data-stu-id="70dda-261">When the database is opened, a reference to it is put into the variable named `db`.</span></span> <span data-ttu-id="70dda-262">(でしたが名前付きものです。)`db`変数がどのデータベースと対話できるようになります。</span><span class="sxs-lookup"><span data-stu-id="70dda-262">(Which could be named anything.) The `db` variable is how you'll end up interacting with the database.</span></span>

<span data-ttu-id="70dda-263">2 番目の行が実際には、データベースのデータをフェッチを使用して、`Query`メソッド。</span><span class="sxs-lookup"><span data-stu-id="70dda-263">The second line actually fetches the database data by using the `Query` method.</span></span> <span data-ttu-id="70dda-264">このコードの動作に注意してください。、`db`変数が開かれているのデータベースへの参照を、を起動すると、`Query`メソッドを使用して、`db`変数 (`db.Query`)。</span><span class="sxs-lookup"><span data-stu-id="70dda-264">Notice how this code works: the `db` variable has a reference to the opened database, and you invoke the `Query` method by using the `db` variable (`db.Query`).</span></span>

<span data-ttu-id="70dda-265">クエリ自体は、SQL`Select`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="70dda-265">The query itself is a SQL `Select` statement.</span></span> <span data-ttu-id="70dda-266">(少しの背景については、SQL では、後の説明を参照してください)。ステートメントでは、`Movies`クエリにテーブルを識別します。</span><span class="sxs-lookup"><span data-stu-id="70dda-266">(For a little background about SQL, see the explanation later.) In the statement, `Movies` identifies the table to query.</span></span> <span data-ttu-id="70dda-267">`*`文字は、クエリがテーブルからすべての列を返す必要がありますを指定します。</span><span class="sxs-lookup"><span data-stu-id="70dda-267">The `*` character specifies that the query should return all the columns from the table.</span></span> <span data-ttu-id="70dda-268">(でしたもを一覧表示の列に個別に、コンマで区切られた。)</span><span class="sxs-lookup"><span data-stu-id="70dda-268">(You could also list columns individually, separated by commas.)</span></span>

<span data-ttu-id="70dda-269">クエリの結果存在する場合は返されで利用できるよう、`selectedData`変数。</span><span class="sxs-lookup"><span data-stu-id="70dda-269">The results of the query, if any, are returned and made available in the `selectedData` variable.</span></span> <span data-ttu-id="70dda-270">ここでも、変数の名前を使用何か。</span><span class="sxs-lookup"><span data-stu-id="70dda-270">Again, the variable could be named anything.</span></span>

<span data-ttu-id="70dda-271">最後に、3 行目は ASP.NET のインスタンスを使用する、`WebGrid`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="70dda-271">Finally, the third line tells ASP.NET that you want to use an instance of the `WebGrid` helper.</span></span> <span data-ttu-id="70dda-272">作成する (*をインスタンス化*) を使用して、ヘルパー オブジェクト、`new`キーワードを使用してクエリ結果を渡すと、`selectedData`変数。</span><span class="sxs-lookup"><span data-stu-id="70dda-272">You create (*instantiate*) the helper object by using the `new` keyword and pass it the query results via the `selectedData` variable.</span></span> <span data-ttu-id="70dda-273">新しい`WebGrid`データベース クエリの結果と共に、オブジェクトで可能な`grid`変数。</span><span class="sxs-lookup"><span data-stu-id="70dda-273">The new `WebGrid` object, along with the results of the database query, are made available in the `grid` variable.</span></span> <span data-ttu-id="70dda-274">その結果は、実際に ページで、データを表示するこの後すぐに必要があります。</span><span class="sxs-lookup"><span data-stu-id="70dda-274">You'll need that result in a moment to actually display the data in the page.</span></span>

<span data-ttu-id="70dda-275">データベースが開かれたこの段階で、データが完成し、準備した、`WebGrid`ヘルパーがそのデータ。</span><span class="sxs-lookup"><span data-stu-id="70dda-275">At this stage, the database has been opened, you've gotten the data you want, and you've prepared the `WebGrid` helper with that data.</span></span> <span data-ttu-id="70dda-276">次に、ページのマークアップを作成することです。</span><span class="sxs-lookup"><span data-stu-id="70dda-276">Next is to create the markup in the page.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="70dda-277">**構造化照会言語 (SQL)**</span><span class="sxs-lookup"><span data-stu-id="70dda-277">**Structured Query Language (SQL)**</span></span>
> 
> <span data-ttu-id="70dda-278">SQL は、データベース内のデータを管理するために、ほとんどのリレーショナル データベースで使用される言語です。</span><span class="sxs-lookup"><span data-stu-id="70dda-278">SQL is a language that's used in most relational databases for managing data in a database.</span></span> <span data-ttu-id="70dda-279">これには、データを取得し、それを更新できるようにして、作成、変更、およびデータベース テーブル内のデータを管理するためのコマンドが含まれます。</span><span class="sxs-lookup"><span data-stu-id="70dda-279">It includes commands that let you retrieve data and update it, and that let you create, modify, and manage data in database tables.</span></span> <span data-ttu-id="70dda-280">SQL は、プログラミング言語 (c#) のように異なります。</span><span class="sxs-lookup"><span data-stu-id="70dda-280">SQL is different than a programming language (like C#).</span></span> <span data-ttu-id="70dda-281">SQL と、データベースを指示することとは、データの取得や、タスクを実行する方法を確認するデータベースの仕事です。</span><span class="sxs-lookup"><span data-stu-id="70dda-281">With SQL, you tell the database what you want, and it's the database's job to figure out how to get the data or perform the task.</span></span> <span data-ttu-id="70dda-282">一部の SQL コマンドの例とその実行内容を次に示します。</span><span class="sxs-lookup"><span data-stu-id="70dda-282">Here are examples of some SQL commands and what they do:</span></span>
> 
> `Select * From Movies`
> 
> `SELECT ID, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> <span data-ttu-id="70dda-283">最初の`Select`ステートメントは、すべての列を取得します (で指定された`*`) から、*映画*テーブル。</span><span class="sxs-lookup"><span data-stu-id="70dda-283">The first `Select` statement gets all the columns (specified by `*`) from the *Movies* table.</span></span>
> 
> <span data-ttu-id="70dda-284">2 番目の`Select`ステートメント内のレコードからの ID、名、および Price 列のフェッチ、*製品*価格列値が 10 以上のテーブル。</span><span class="sxs-lookup"><span data-stu-id="70dda-284">The second `Select` statement fetches the ID, Name, and Price columns from records in the *Product* table whose Price column value is more than 10.</span></span> <span data-ttu-id="70dda-285">[名前] 列の値に基づいてアルファベット順に結果が返されます。</span><span class="sxs-lookup"><span data-stu-id="70dda-285">The command returns the results in alphabetical order based on the values of the Name column.</span></span> <span data-ttu-id="70dda-286">価格条件に一致するレコードがない場合は、空のセットが返されます。</span><span class="sxs-lookup"><span data-stu-id="70dda-286">If no records match the price criteria, the command returns an empty set.</span></span>
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ('Croissant', 'A flaky delight', 1.99)`
> 
> <span data-ttu-id="70dda-287">このコマンドに新しいレコードを挿入する、*製品*から 1.99「クロワッサン」、「A 不安定な満足」に説明の列と価格を名前列に設定されているテーブル。</span><span class="sxs-lookup"><span data-stu-id="70dda-287">This command inserts a new record into the *Product* table, setting the Name column to "Croissant", the Description column to "A flaky delight", and the price to 1.99.</span></span>
> 
> <span data-ttu-id="70dda-288">数値以外の値を指定する場合に、値を単一引用符 (いない二重引用符、c# と同様) で囲むことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="70dda-288">Notice that when you're specifying a non-numeric value, the value is enclosed in single quotation marks (not double quotation marks, as in C#).</span></span> <span data-ttu-id="70dda-289">テキストまたは日付の値を囲むが周囲の数値ではなくこれらの引用符を使用するとします。</span><span class="sxs-lookup"><span data-stu-id="70dda-289">You use these quotation marks around text or date values, but not around numbers.</span></span>
> 
> `DELETE FROM Product WHERE ExpirationDate < '01/01/2008'`
> 
> <span data-ttu-id="70dda-290">このコマンドのレコードの削除、*製品*が 2008 年 1 月 1 日より前で有効期限の日付列が含まれるテーブル。</span><span class="sxs-lookup"><span data-stu-id="70dda-290">This command deletes records in the *Product* table whose expiration date column is earlier than January 1, 2008.</span></span> <span data-ttu-id="70dda-291">(コマンドを前提としていますが、*製品*テーブルにはこのような列は、もちろん)。/MM/DD/YYYY の形式で日付がここに入力したが、現在のロケールに使用される形式で入力してください。</span><span class="sxs-lookup"><span data-stu-id="70dda-291">(The command assumes that the *Product* table has such a column, of course.) The date is entered here in MM/DD/YYYY format, but it should be entered in the format that's used for your locale.</span></span>
> 
> <span data-ttu-id="70dda-292">`Insert`と`Delete`コマンドの結果セットを返しません。</span><span class="sxs-lookup"><span data-stu-id="70dda-292">The `Insert` and `Delete` commands don't return result sets.</span></span> <span data-ttu-id="70dda-293">代わりに、コマンドの影響を受けたレコードの数を示す数値が返されます。</span><span class="sxs-lookup"><span data-stu-id="70dda-293">Instead, they return a number that tells you how many records were affected by the command.</span></span>
> 
> <span data-ttu-id="70dda-294">これらの操作 (挿入や、レコードを削除する) などの一部は、操作を要求しているプロセスは、データベース内に適切なアクセス許可があります。</span><span class="sxs-lookup"><span data-stu-id="70dda-294">For some of these operations (like inserting and deleting records), the process that's requesting the operation has to have appropriate permissions in the database.</span></span> <span data-ttu-id="70dda-295">実稼働データベースの多くの場合、データベースに接続するときに、ユーザー名とパスワードを指定する必要がある理由です。</span><span class="sxs-lookup"><span data-stu-id="70dda-295">That's why for production databases you often have to supply a user name and password when you connect to the database.</span></span>
> 
> <span data-ttu-id="70dda-296">数十個の SQL コマンドがありますが、ここに表示するコマンドと同様に、パターンに従うすべて。</span><span class="sxs-lookup"><span data-stu-id="70dda-296">There are dozens of SQL commands, but they all follow a pattern like the commands you see here.</span></span> <span data-ttu-id="70dda-297">SQL コマンドを使用して、データベース テーブルを作成、テーブル内のレコードの数をカウント、価格、計算、および多くの操作を実行することができます。</span><span class="sxs-lookup"><span data-stu-id="70dda-297">You can use SQL commands to create database tables, count the number of records in a table, calculate prices, and perform many more operations.</span></span>


### <a name="adding-markup-to-display-the-data"></a><span data-ttu-id="70dda-298">データを表示するマークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="70dda-298">Adding Markup to Display the Data</span></span>

<span data-ttu-id="70dda-299">内で、`<head>`要素のセットの内容、 `<title>` "Movies"する要素。</span><span class="sxs-lookup"><span data-stu-id="70dda-299">Inside the `<head>` element, set contents of the `<title>` element to "Movies":</span></span>

[!code-html[Main](displaying-data/samples/sample2.html?highlight=3)]

<span data-ttu-id="70dda-300">内で、`<body>`ページの要素は、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="70dda-300">Inside the `<body>` element of the page, add the following:</span></span>

[!code-html[Main](displaying-data/samples/sample3.html)]

<span data-ttu-id="70dda-301">それで完了です。</span><span class="sxs-lookup"><span data-stu-id="70dda-301">That's it.</span></span> <span data-ttu-id="70dda-302">`grid`変数を作成したときに作成した値は、`WebGrid`前のコード内のオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="70dda-302">The `grid` variable is the value you created when you created the `WebGrid` object in code earlier.</span></span>

<span data-ttu-id="70dda-303">WebMatrix のツリー ビューでは、ページを右クリックして**ブラウザーで起動**します。</span><span class="sxs-lookup"><span data-stu-id="70dda-303">In the WebMatrix tree view, right-click the page and select **Launch in browser**.</span></span> <span data-ttu-id="70dda-304">このページのようなものが表示されます。</span><span class="sxs-lookup"><span data-stu-id="70dda-304">You'll see something like this page:</span></span>

![映画テーブルからの既定の WebGrid ヘルパーの出力](displaying-data/_static/image18.png)

<span data-ttu-id="70dda-306">その列で並べ替える列見出しリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="70dda-306">Click a column heading link to sort by that column.</span></span> <span data-ttu-id="70dda-307">組み込まれている機能は、見出しをクリックして並べ替えられる、 **WebGrid**ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="70dda-307">Being able to sort by clicking a heading is a feature that's built into the **WebGrid** helper.</span></span>

<span data-ttu-id="70dda-308">`GetHtml`メソッド、その名前からわかるようには、データを表示するマークアップを生成します。</span><span class="sxs-lookup"><span data-stu-id="70dda-308">The `GetHtml` method, as its name suggests, generates markup that displays the data.</span></span> <span data-ttu-id="70dda-309">既定で、`GetHtml`メソッドは、HTML を生成します`<table>`要素。</span><span class="sxs-lookup"><span data-stu-id="70dda-309">By default, the `GetHtml` method generates an HTML `<table>` element.</span></span> <span data-ttu-id="70dda-310">(する場合は、確認できます、レンダリング、ブラウザーでページのソースを調べることで。)</span><span class="sxs-lookup"><span data-stu-id="70dda-310">(If you want, you can verify the rendering by looking at the source of the page in the browser.)</span></span>

## <a name="modifying-the-look-of-the-grid"></a><span data-ttu-id="70dda-311">グリッドの外観を変更します。</span><span class="sxs-lookup"><span data-stu-id="70dda-311">Modifying the Look of the Grid</span></span>

<span data-ttu-id="70dda-312">使用して、`WebGrid`行ったように、ヘルパーは簡単ですが、結果の表示はプレーンな。</span><span class="sxs-lookup"><span data-stu-id="70dda-312">Using the `WebGrid` helper like you just did is easy, but the resulting display is plain.</span></span> <span data-ttu-id="70dda-313">`WebGrid`ヘルパーがあらゆる種類のオプション データの表示方法を制御することができます。</span><span class="sxs-lookup"><span data-stu-id="70dda-313">The `WebGrid` helper has all sorts of options that let you control how the data is displayed.</span></span> <span data-ttu-id="70dda-314">このチュートリアルで調査する非常に多くがありますが、このセクションがこれらのオプションのいくつかのアイデアを提供します。</span><span class="sxs-lookup"><span data-stu-id="70dda-314">There are far too many to explore in this tutorial, but this section will give you an idea of some of those options.</span></span> <span data-ttu-id="70dda-315">いくつか追加のオプションについては、このシリーズの以降のチュートリアルで説明します。</span><span class="sxs-lookup"><span data-stu-id="70dda-315">A few additional options will be covered in later tutorials in this series.</span></span>

### <a name="specifying-individual-columns-to-display"></a><span data-ttu-id="70dda-316">表示する個々 の列を指定します。</span><span class="sxs-lookup"><span data-stu-id="70dda-316">Specifying Individual Columns to Display</span></span>

<span data-ttu-id="70dda-317">開始するには、特定の列のみを表示することを指定できます。</span><span class="sxs-lookup"><span data-stu-id="70dda-317">To start, you can specify that you want to display only certain columns.</span></span> <span data-ttu-id="70dda-318">既定で、説明したように、グリッドに表示されます 4 つの列のすべて、*映画*テーブル。</span><span class="sxs-lookup"><span data-stu-id="70dda-318">By default, as you've seen, the grid shows all four of the columns from the *Movies* table.</span></span>

<span data-ttu-id="70dda-319">*Movies.cshtml*ファイルで、置換、`@grid.GetHtml()`を次に追加したマークアップ。</span><span class="sxs-lookup"><span data-stu-id="70dda-319">In the *Movies.cshtml* file, replace the `@grid.GetHtml()` markup that you just added with the following:</span></span>

[!code-css[Main](displaying-data/samples/sample4.css)]

<span data-ttu-id="70dda-320">ヘルパーを表示する列を言うに含める、`columns`のパラメーター、`GetHtml`メソッドと列のコレクションを渡します。</span><span class="sxs-lookup"><span data-stu-id="70dda-320">To tell the helper which columns to display, you include a `columns` parameter for the `GetHtml` method and pass in a collection of columns.</span></span> <span data-ttu-id="70dda-321">コレクション内に含めるには、各列を指定します。</span><span class="sxs-lookup"><span data-stu-id="70dda-321">In the collection, you specify each column to include.</span></span> <span data-ttu-id="70dda-322">含めることで表示する個々 の列を指定する、`grid.Column`オブジェクト、およびデータの列の名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="70dda-322">You specify an individual column to display by including a `grid.Column` object, and pass in the name of the data column you want.</span></span> <span data-ttu-id="70dda-323">(これらの列は、SQL クエリの結果に含める必要があります:、`WebGrid`ヘルパーは、クエリによって返されたいない列を表示できません)。</span><span class="sxs-lookup"><span data-stu-id="70dda-323">(These columns must be included in the SQL query results — the `WebGrid` helper cannot display columns that were not returned by the query.)</span></span>

<span data-ttu-id="70dda-324">起動、 *Movies.cshtml* 、もう一度、今度は次のいずれかのような表示を取得するブラウザーのページ (ID 列が表示されていないことに注意してください)。</span><span class="sxs-lookup"><span data-stu-id="70dda-324">Launch the *Movies.cshtml* page in the browser again, and this time you get a display like the following one (notice that no ID column is displayed):</span></span>

![選択した列のみを表示している WebGrid 画面](displaying-data/_static/image19.png)

### <a name="changing-the-look-of-the-grid"></a><span data-ttu-id="70dda-326">グリッドの外観の変更</span><span class="sxs-lookup"><span data-stu-id="70dda-326">Changing the Look of the Grid</span></span>

<span data-ttu-id="70dda-327">このセットの後のチュートリアルで紹介うちいくつかの列を表示するための他多数のオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="70dda-327">There are quite a few more options for displaying columns, some of which will be explored in later tutorials in this set.</span></span> <span data-ttu-id="70dda-328">ここでは、このセクションでは紹介方法は、グリッド全体のスタイルを設定できます。</span><span class="sxs-lookup"><span data-stu-id="70dda-328">For now, this section will introduce you to ways in which you can style the grid as a whole.</span></span>

<span data-ttu-id="70dda-329">内で、`<head>`終了直前に、ページのセクション`</head>`タグは、次の追加`<style>`要素。</span><span class="sxs-lookup"><span data-stu-id="70dda-329">Inside the `<head>` section of the page, just before the closing `</head>` tag, add the following `<style>` element:</span></span>

[!code-css[Main](displaying-data/samples/sample5.css)]

<span data-ttu-id="70dda-330">この CSS マークアップがという名前のクラスを定義`grid`、`head`など。</span><span class="sxs-lookup"><span data-stu-id="70dda-330">This CSS markup defines classes named `grid`, `head`, and so on.</span></span> <span data-ttu-id="70dda-331">個別のこれらのスタイル定義を配置することもできます *.css*ファイルを開き、ページにリンクします。</span><span class="sxs-lookup"><span data-stu-id="70dda-331">You could also put these style definitions in a separate *.css* file and link that to the page.</span></span> <span data-ttu-id="70dda-332">(実際、行いますこの一連のチュートリアルの後半。)確かに物事を簡単にこのチュートリアルでは、データを表示するのと同じページ内で。</span><span class="sxs-lookup"><span data-stu-id="70dda-332">(In fact, you'll do that later in this tutorial set.) But to make things easy for this tutorial, they're inside the same page that displays the data.</span></span>

<span data-ttu-id="70dda-333">受けることができます、`WebGrid`これらのスタイル クラスを使用するヘルパー。</span><span class="sxs-lookup"><span data-stu-id="70dda-333">Now you can get the `WebGrid` helper to use these style classes.</span></span> <span data-ttu-id="70dda-334">ヘルパーがいくつかのプロパティ (たとえば、 `tableStyle`) ちょうどこの目的の-には、CSS スタイル クラス名を割り当てるし、クラス名には、ヘルパーによって表示されるマークアップの一部としてレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="70dda-334">The helper has a number of properties (for example, `tableStyle`) for just this purpose — you assign a CSS style class name to them, and that class name is rendered as part of the markup that's rendered by the helper.</span></span>

<span data-ttu-id="70dda-335">変更、`grid.GetHtml`マークアップため、it ではこのコードのようになりますの。</span><span class="sxs-lookup"><span data-stu-id="70dda-335">Change the `grid.GetHtml` markup so that it now looks like this code:</span></span>

[!code-css[Main](displaying-data/samples/sample6.css)]

<span data-ttu-id="70dda-336">追加したことは、新`tableStyle`、 `headerStyle`、および`alternatingRowStyle`パラメーターを`GetHtml`メソッド。</span><span class="sxs-lookup"><span data-stu-id="70dda-336">What's new here is that you've added `tableStyle`, `headerStyle`, and `alternatingRowStyle` parameters to the `GetHtml` method.</span></span> <span data-ttu-id="70dda-337">これらのパラメーターは、少し前に追加した CSS スタイルの名前に設定されています。</span><span class="sxs-lookup"><span data-stu-id="70dda-337">These parameters have been set to the names of the CSS styles that you added a moment ago.</span></span>

<span data-ttu-id="70dda-338">ページを実行し、この時間を検索する前よりも大幅に低下プレーンなグリッドを表示します。</span><span class="sxs-lookup"><span data-stu-id="70dda-338">Run the page, and this time you see a grid that looks much less plain than before:</span></span>

![CSS クラス名に設定するパラメーターが含まれている WebGrid の表示](displaying-data/_static/image20.png)

<span data-ttu-id="70dda-340">何を表示する、`GetHtml`生成されたメソッド、ブラウザーでページのソースで検索できます。</span><span class="sxs-lookup"><span data-stu-id="70dda-340">To see what the `GetHtml` method generated, you can look at the source of the page in the browser.</span></span> <span data-ttu-id="70dda-341">ここでは、詳しく説明しませんが、重要な点はのようにパラメーターを指定して`tableStyle`、次のような HTML タグを生成するグリッドの原因とします。</span><span class="sxs-lookup"><span data-stu-id="70dda-341">We won't go into detail here, but the important point is that by specifying parameters like `tableStyle`, you caused the grid to generate HTML tags like the following:</span></span>

`<table class="grid">`

<span data-ttu-id="70dda-342">`<table>`タグの必要があるが、`class`先ほど追加した CSS 規則のいずれかを参照する属性を追加します。</span><span class="sxs-lookup"><span data-stu-id="70dda-342">The `<table>` tag has had a `class` attribute added to it that references one of the CSS rules that you added earlier.</span></span> <span data-ttu-id="70dda-343">このコードは、基本的なパターンを示します&mdash;ごとに異なるパラメーター、`GetHtml`メソッドにより参照する CSS クラス、メソッド、マークアップと共に生成されます。</span><span class="sxs-lookup"><span data-stu-id="70dda-343">This code shows you the basic pattern &mdash; different parameters for the `GetHtml` method let you reference CSS classes that the method then generates along with the markup.</span></span> <span data-ttu-id="70dda-344">CSS クラスを持つことは自由です。</span><span class="sxs-lookup"><span data-stu-id="70dda-344">What you do with the CSS classes is up to you.</span></span>

## <a name="adding-paging"></a><span data-ttu-id="70dda-345">ページングの追加</span><span class="sxs-lookup"><span data-stu-id="70dda-345">Adding Paging</span></span>

<span data-ttu-id="70dda-346">このチュートリアルの最後のタスクとしてグリッドにページングを追加します。</span><span class="sxs-lookup"><span data-stu-id="70dda-346">As the last task for this tutorial, you'll add paging to the grid.</span></span> <span data-ttu-id="70dda-347">ここでは、すべての映画を一度に表示する問題はありません。</span><span class="sxs-lookup"><span data-stu-id="70dda-347">Right now it's no problem to display all your movies at once.</span></span> <span data-ttu-id="70dda-348">何百もの動画を追加した場合はページの表示が長くなるとします。</span><span class="sxs-lookup"><span data-stu-id="70dda-348">But if you added hundreds of movies, the page display would get long.</span></span>

<span data-ttu-id="70dda-349">ページのコードで作成する行を変更する、`WebGrid`次のコードにオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="70dda-349">In the page code, change the line that creates the `WebGrid` object to the following code:</span></span>

[!code-csharp[Main](displaying-data/samples/sample7.cs)]

<span data-ttu-id="70dda-350">前に、の唯一の違いが追加されていること、`rowsPerPage`パラメーター 3 に設定されています。</span><span class="sxs-lookup"><span data-stu-id="70dda-350">The only difference from before is that you've added a `rowsPerPage` parameter that's set to 3.</span></span>

<span data-ttu-id="70dda-351">ページを実行します。</span><span class="sxs-lookup"><span data-stu-id="70dda-351">Run the page.</span></span> <span data-ttu-id="70dda-352">グリッドには、時間、およびデータベースのムービーからのページのナビゲーション リンクに 3 つの行が表示されます。</span><span class="sxs-lookup"><span data-stu-id="70dda-352">The grid displays 3 rows at a time, plus navigation links that let you page through the movies in your database:</span></span>

![ページングを使用した WebGrid 表示](displaying-data/_static/image21.png)

## <a name="coming-up-next"></a><span data-ttu-id="70dda-354">次回について</span><span class="sxs-lookup"><span data-stu-id="70dda-354">Coming Up Next</span></span>

<span data-ttu-id="70dda-355">次のチュートリアルでは、Razor および c# のコードを使用して、フォーム内のユーザー入力を取得する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="70dda-355">In the next tutorial, you'll learn how to use Razor and C# code to get user input in a form.</span></span> <span data-ttu-id="70dda-356">タイトルまたはジャンルで映画を検索できるように、映画のページに検索ボックスを追加します。</span><span class="sxs-lookup"><span data-stu-id="70dda-356">You'll add a search box to the Movies page so that you can find movies by title or genre.</span></span>

## <a name="complete-listing-for-movies-page"></a><span data-ttu-id="70dda-357">ムービー ページ全体を示します</span><span class="sxs-lookup"><span data-stu-id="70dda-357">Complete Listing for Movies Page</span></span>

[!code-cshtml[Main](displaying-data/samples/sample8.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="70dda-358">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="70dda-358">Additional Resources</span></span>

- [<span data-ttu-id="70dda-359">Razor 構文を使用して ASP.NET Web プログラミングの概要</span><span class="sxs-lookup"><span data-stu-id="70dda-359">Introduction to ASP.NET Web Programming Using the Razor Syntax</span></span>](https://go.microsoft.com/fwlink/?LinkID=202890)

> [!div class="step-by-step"]
> <span data-ttu-id="70dda-360">[前へ](intro-to-web-pages-programming.md)
> [次へ](form-basics.md)</span><span class="sxs-lookup"><span data-stu-id="70dda-360">[Previous](intro-to-web-pages-programming.md)
[Next](form-basics.md)</span></span>
