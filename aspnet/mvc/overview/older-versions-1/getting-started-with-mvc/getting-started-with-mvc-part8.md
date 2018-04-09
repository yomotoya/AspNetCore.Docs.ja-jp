---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: モデルに列を追加する |Microsoft ドキュメント
author: shanselman
description: これは、ASP.NET MVC の基本について説明する初心者向けチュートリアルです。 データベースから読み取り/書き込みする単純な web アプリケーションを作成します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 7a0b64ce00fc5ee6d49990f1d4d93a154c467bf5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-column-to-the-model"></a><span data-ttu-id="723c4-104">モデルに列を追加します。</span><span class="sxs-lookup"><span data-stu-id="723c4-104">Adding a Column to the Model</span></span>
====================
<span data-ttu-id="723c4-105">によって[Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="723c4-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="723c4-106">これは、ASP.NET MVC の基本について説明する初心者向けチュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="723c4-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="723c4-107">データベースから読み取り/書き込みを単純な web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="723c4-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="723c4-108">参照してください、 [ASP.NET MVC ラーニング センター](../../../index.md)チュートリアルおよびサンプルは、その他の ASP.NET MVC を検索します。</span><span class="sxs-lookup"><span data-stu-id="723c4-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="723c4-109">このセクションで行う、データベースのスキーマを変更して、アプリケーション内で、変更を処理した方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="723c4-109">In this section we are going to walk through how we can make changes to the schema of our database, and handle the changes within our application.</span></span>

<span data-ttu-id="723c4-110">ムービーの表に、「評価」の列を追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="723c4-110">Let's add a "Rating" Colum to the Movie table.</span></span> <span data-ttu-id="723c4-111">IDE に戻るし、[データベース エクスプ ローラー] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="723c4-111">Go back to the IDE and click the Database Explorer.</span></span> <span data-ttu-id="723c4-112">ムービー テーブルを右クリックし、開くテーブルの定義を選択します。</span><span class="sxs-lookup"><span data-stu-id="723c4-112">Right click the Movie table and select Open Table Definition.</span></span>

<span data-ttu-id="723c4-113">以下に示すように、「評価」列を追加します。</span><span class="sxs-lookup"><span data-stu-id="723c4-113">Add a "Rating" column as seen below.</span></span> <span data-ttu-id="723c4-114">これで任意の評価があるありません、ために、列は null を許容できます。</span><span class="sxs-lookup"><span data-stu-id="723c4-114">Since we don't have any Ratings now, the column can allow nulls.</span></span> <span data-ttu-id="723c4-115">[保存] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="723c4-115">Click Save.</span></span>

<span data-ttu-id="723c4-116">[![映画のテーブルの編集](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="723c4-116">[![Editing Movies Table](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span></span>

<span data-ttu-id="723c4-117">次に、ソリューション エクスプ ローラーに返し、Movies.edmx ファイル (は \Models フォルダーにあります) を開きます。</span><span class="sxs-lookup"><span data-stu-id="723c4-117">Next, return to the Solution Explorer and open up the Movies.edmx file (which is in the \Models folder).</span></span> <span data-ttu-id="723c4-118">デザイン サーフェイス (白の領域) を右クリックし、データベースから更新プログラムのモデルを選択します。</span><span class="sxs-lookup"><span data-stu-id="723c4-118">Right click on the design surface (the white area) and select Update Model from Database.</span></span>

<span data-ttu-id="723c4-119">[![映画 - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="723c4-119">[![Movies - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span></span>

<span data-ttu-id="723c4-120">これにより、「更新ウィザード」が起動します。</span><span class="sxs-lookup"><span data-stu-id="723c4-120">This will launch the "Update Wizard".</span></span> <span data-ttu-id="723c4-121">内で、[更新] タブをクリックし、[完了] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="723c4-121">Click the Refresh tab within it and click Finish.</span></span> <span data-ttu-id="723c4-122">ムービー モデル クラスは、新しい列と共に更新されます。</span><span class="sxs-lookup"><span data-stu-id="723c4-122">Our Movie model class will then be updated with the new column.</span></span>

![(2) の更新ウィザード](getting-started-with-mvc-part8/_static/image5.png)

<span data-ttu-id="723c4-124">[完了] をクリックすると、表示できます、ムービー エンティティ モデルに新しい [評価] 列が追加されました。</span><span class="sxs-lookup"><span data-stu-id="723c4-124">After clicking Finish, you can see the new Rating Column has been added to the Movie Entity in our model.</span></span>

<span data-ttu-id="723c4-125">[![ムービーのエンティティ](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="723c4-125">[![Movie Entity](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span></span>

<span data-ttu-id="723c4-126">データベース モデルの列が追加されましたが、ビューには、把握できていないこと。</span><span class="sxs-lookup"><span data-stu-id="723c4-126">We've added a column in the database model, but the Views don't know about it.</span></span>

## <a name="update-views-with-model-changes"></a><span data-ttu-id="723c4-127">モデルの変更と更新の表示</span><span class="sxs-lookup"><span data-stu-id="723c4-127">Update Views with Model Changes</span></span>

<span data-ttu-id="723c4-128">いくつかの方法が、新しい [評価] 列を反映するように、テンプレートの表示を更新することができます。</span><span class="sxs-lookup"><span data-stu-id="723c4-128">There are a few ways we could update our view templates to reflect the new Rating column.</span></span> <span data-ttu-id="723c4-129">ビューの追加 ダイアログを使用してそれらを生成することによってこれらのビューを作成した後は、それらを削除しはもう一度再おでした。</span><span class="sxs-lookup"><span data-stu-id="723c4-129">Since we created these Views by generating them via the Add View dialog, we could delete them and recreate them again.</span></span> <span data-ttu-id="723c4-130">ただし、通常のユーザーが既に変更を加えました、テンプレートを表示する最初のスキャフォールディング生成から、追加したり、作成、ID フィールドに行ったように、手動でフィールドを削除する必要があります。</span><span class="sxs-lookup"><span data-stu-id="723c4-130">However, typically people will have already made modifications to their View templates from the initial scaffolded generation and will want to add or delete fields manually, just as we did with the ID field for Create.</span></span>

<span data-ttu-id="723c4-131">\Views\Movies\Index.aspx テンプレートを開き、追加、 &lt;th&gt;評価&lt;/th&gt;ムービー テーブルの先頭にします。</span><span class="sxs-lookup"><span data-stu-id="723c4-131">Open up the \Views\Movies\Index.aspx template and add a &lt;th&gt;Rating&lt;/th&gt; to the head of the Movie table.</span></span> <span data-ttu-id="723c4-132">ジャンル後、私は追加されます。</span><span class="sxs-lookup"><span data-stu-id="723c4-132">I added mine after Genre.</span></span> <span data-ttu-id="723c4-133">次に、同じ列の位置が下位に、行を追加して、新しい評価を出力します。</span><span class="sxs-lookup"><span data-stu-id="723c4-133">Then, in the same column position but lower down, add a line to output our new Rating.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

<span data-ttu-id="723c4-134">最終的な Index.aspx テンプレートは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="723c4-134">Our final Index.aspx template will look like this:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

<span data-ttu-id="723c4-135">みましょう \Views\Movies\Create.aspx テンプレートを開き、新しい評価プロパティのラベルとテキスト ボックスを追加します。</span><span class="sxs-lookup"><span data-stu-id="723c4-135">Let's then open up the \Views\Movies\Create.aspx template and add a Label and Textbox for our new Rating property:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

<span data-ttu-id="723c4-136">最終的な Create.aspx テンプレートが次のように、され、ブラウザーのタイトルとセカンダリを変更してみましょう&lt;h2&gt;映画のように"を作成、"ここにいるときに何かにタイトルです。</span><span class="sxs-lookup"><span data-stu-id="723c4-136">Our final Create.aspx template will look like this, and let's change our browser's title and secondary &lt;h2&gt; title to something like "Create a Movie" while we're in here!</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

<span data-ttu-id="723c4-137">アプリを実行し、作成できたところで、[作成] ページに追加されているデータベースの新しいフィールドです。</span><span class="sxs-lookup"><span data-stu-id="723c4-137">Run your app and now you've got a new field in the database that's been added to the Create page.</span></span> <span data-ttu-id="723c4-138">新しいムービーの評価の場合は、この時間を追加し、作成 をクリックします。</span><span class="sxs-lookup"><span data-stu-id="723c4-138">Add a new Movie - this time with a Rating - and click Create.</span></span>

<span data-ttu-id="723c4-139">[![Windows Internet Explorer - ムービーを作成します。](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="723c4-139">[![Create a Movie - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span></span>

<span data-ttu-id="723c4-140">作成 をクリックすると、インデックス ページに送信している、新しいムービーが記載されている場合、新しい評価データベース内の列</span><span class="sxs-lookup"><span data-stu-id="723c4-140">After you click Create, you're sent to the Index page where you new Movie is listed with the new Rating Column in the database</span></span>

<span data-ttu-id="723c4-141">[![ムービーの一覧]-[Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="723c4-141">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span></span>

<span data-ttu-id="723c4-142">この基本的なチュートリアルでは、コント ローラー、ビューに関連付けると、ハード コードされたデータを渡すことを行う作業を開始する取得しました。</span><span class="sxs-lookup"><span data-stu-id="723c4-142">This basic tutorial got you started making Controllers, associating them with Views and passing around hard-coded data.</span></span> <span data-ttu-id="723c4-143">お作成およびデータベースを設計および一部のデータを格納しでします。</span><span class="sxs-lookup"><span data-stu-id="723c4-143">Then we created and designed a Database and put some data it in.</span></span> <span data-ttu-id="723c4-144">データベースからデータを取得し、HTML テーブルに表示します。</span><span class="sxs-lookup"><span data-stu-id="723c4-144">We retrieved the data from the database and displayed it in an HTML table.</span></span> <span data-ttu-id="723c4-145">データを追加、データベース自体から Web アプリケーション内でユーザーを許可するフォームの作成が追加されました。</span><span class="sxs-lookup"><span data-stu-id="723c4-145">Then we added a Create form that let the user add data to the database themselves from within the Web Application.</span></span> <span data-ttu-id="723c4-146">追加の検証、その後、クライアント側で JavaScript を使用して、検証が行われました。</span><span class="sxs-lookup"><span data-stu-id="723c4-146">We added validation, then made the validation use JavaScript on the client-side.</span></span> <span data-ttu-id="723c4-147">最後に、おに、データの新しい列を含めるデータベースを変更し、作成し、この新しいデータを表示する、2 つのページを更新します。</span><span class="sxs-lookup"><span data-stu-id="723c4-147">Finally, we changed the database to include a new column of data, then updated our two pages to create and display this new data.</span></span>

<span data-ttu-id="723c4-148">今すぐみて、中間レベルのチュートリアルへお進み"[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)"だけでなく多くのビデオおよびにリソースを[ https://asp.net/mvc ](https://asp.net/mvc) ASP.NET MVC についてさらに学習します。</span><span class="sxs-lookup"><span data-stu-id="723c4-148">I now encourage you to move on to our intermediate-level tutorial "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" as well as the many videos and resources at [https://asp.net/mvc](https://asp.net/mvc) to learn even more about ASP.NET MVC!</span></span>

<span data-ttu-id="723c4-149">それではお楽しみください。</span><span class="sxs-lookup"><span data-stu-id="723c4-149">Enjoy!</span></span>

- <span data-ttu-id="723c4-150">Scott Hanselman - [ http://hanselman.com ](http://hanselman.com)と[ @shanselman ](http://twitter.com/shanselman) Twitter でします。</span><span class="sxs-lookup"><span data-stu-id="723c4-150">Scott Hanselman - [http://hanselman.com](http://hanselman.com) and [@shanselman](http://twitter.com/shanselman) on Twitter.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="723c4-151">前へ</span><span class="sxs-lookup"><span data-stu-id="723c4-151">Previous</span></span>](getting-started-with-mvc-part7.md)
