---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: コント ローラーから、モデルのデータにアクセス |Microsoft ドキュメント
author: shanselman
description: これは、ASP.NET MVC の基本について説明する初心者向けチュートリアルです。 データベースから読み取り/書き込みする単純な web アプリケーションを作成します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 2ba1b73f40a920e27e4a03d9f703e62054d3f25c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/10/2018
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="48b73-104">コント ローラーから、モデルのデータにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="48b73-104">Accessing your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="48b73-105">によって[Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="48b73-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="48b73-106">これは、ASP.NET MVC の基本について説明する初心者向けチュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="48b73-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="48b73-107">データベースから読み取り/書き込みを単純な web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="48b73-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="48b73-108">参照してください、 [ASP.NET MVC ラーニング センター](../../../index.md)チュートリアルおよびサンプルは、その他の ASP.NET MVC を検索します。</span><span class="sxs-lookup"><span data-stu-id="48b73-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="48b73-109">このセクションの内容はここを新しい MoviesController クラスを作成し、ムービー データを取得し、ビュー テンプレートを使用してブラウザーに表示するいくつかのコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="48b73-109">In this section we are going to create a new MoviesController class, and write some code that retrieves our Movie data and displays it back to the browser using a View template.</span></span>

<span data-ttu-id="48b73-110">Controllers フォルダーを右クリックし、新しい MoviesController を作成します。</span><span class="sxs-lookup"><span data-stu-id="48b73-110">Right click on the Controllers folder and make a new MoviesController.</span></span>

<span data-ttu-id="48b73-111">[![コント ローラーを追加します。](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="48b73-111">[![Add Controller](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span></span>

<span data-ttu-id="48b73-112">これにより、プロジェクト内で当社 \Controllers フォルダーの下に新しい"MoviesController.cs"ファイルが作成されます。</span><span class="sxs-lookup"><span data-stu-id="48b73-112">This will create a new "MoviesController.cs" file underneath our \Controllers folder within our project.</span></span> <span data-ttu-id="48b73-113">新しく作成されたデータベースからムービーの一覧を取得する MovieController を更新してみましょう。</span><span class="sxs-lookup"><span data-stu-id="48b73-113">Let's update the MovieController to retrieve the list of movies from our newly populated database.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

<span data-ttu-id="48b73-114">お 1984 年の夏の後に発売された映画のみ取得されるように、LINQ クエリを実行します。</span><span class="sxs-lookup"><span data-stu-id="48b73-114">We are performing a LINQ query so that we only retrieve movies released after the summer of 1984.</span></span> <span data-ttu-id="48b73-115">映画に戻るには、この一覧を表示、そこでメソッドで右クリックし、それを作成する追加のビューを選択するビュー テンプレートが必要です。</span><span class="sxs-lookup"><span data-stu-id="48b73-115">We'll need a View template to render this list of movies back, so right-click in the method and select Add View to create it.</span></span>

<span data-ttu-id="48b73-116">ビューの追加 ダイアログ ボックスでおを示す一覧を渡している&lt;Movies.Models.Movie&gt;テンプレートの表示にします。</span><span class="sxs-lookup"><span data-stu-id="48b73-116">Within the Add View dialog we'll indicate that we are passing a List&lt;Movies.Models.Movie&gt; to our View template.</span></span> <span data-ttu-id="48b73-117">前の時間ビューの追加 ダイアログを使用して、"Empty"のテンプレートを作成することを選択とは異なりおすることを示すため、この時点で作成する Visual Studio で自動的に「スキャフォールディング」ビュー テンプレートの既定のコンテンツを。</span><span class="sxs-lookup"><span data-stu-id="48b73-117">Unlike the previous times we used the Add View dialog and chose to create an "Empty" template, this time we'll indicate that we want Visual Studio to automatically "scaffold" a view template for us with some default content.</span></span> <span data-ttu-id="48b73-118">それでは、"ビューのコンテンツのドロップダウン メニュー内の"List"項目を選択しています。</span><span class="sxs-lookup"><span data-stu-id="48b73-118">We'll do this by selecting the "List" item within the "View content dropdown menu.</span></span>

<span data-ttu-id="48b73-119">ただしがある場合、作成された新しいクラスをビューの追加 ダイアログ表示するのには、アプリケーションをコンパイルする必要があります。</span><span class="sxs-lookup"><span data-stu-id="48b73-119">Remember, when you have a created a new class you'll need to compile your application for it to show up in the Add View Dialog.</span></span>

![ビューを追加します。](getting-started-with-mvc-part5/_static/image3.png)

<span data-ttu-id="48b73-121">追加 をクリックし、システムはムービーの一覧を表示するうえでビューのコードを自動的に生成されます。</span><span class="sxs-lookup"><span data-stu-id="48b73-121">Click add and the system will automatically generate the code for a View for us that displays our list of movies.</span></span> <span data-ttu-id="48b73-122">これは、変更する絶好のタイミング、 &lt;h2&gt;前、Hello World ビューと同じように、「マイ ムービー リスト」のようなものに見出し。</span><span class="sxs-lookup"><span data-stu-id="48b73-122">This is a good time to change the &lt;h2&gt; heading to something like "My Movie List" like we did earlier with the Hello World view.</span></span>

<span data-ttu-id="48b73-123">[![映画 - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="48b73-123">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span></span>

<span data-ttu-id="48b73-124">アプリケーションを実行し、アドレス バーに/Movies を参照してください。</span><span class="sxs-lookup"><span data-stu-id="48b73-124">Run your application and visit /Movies in the address bar.</span></span> <span data-ttu-id="48b73-125">これで、コント ローラー内の基本的なクエリを使用して、データベースからデータを取得および映画について認識しているビューにデータが返されますしました。</span><span class="sxs-lookup"><span data-stu-id="48b73-125">Now we've retrieved data from the database using a basic query inside the Controller and returned the data to a View that knows about Movies.</span></span> <span data-ttu-id="48b73-126">そのビューは、ムービーの一覧をループし、ご利用の米国のデータのテーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="48b73-126">That View then spins through the list of Movies and creates a table of data for us.</span></span>

<span data-ttu-id="48b73-127">[![ムービーの一覧]-[Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="48b73-127">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span></span>

<span data-ttu-id="48b73-128">おされませんを実装するこのアプリケーションの編集、詳細、および Delete の機能のためご利用の米国 scaffold テンプレートが作成された既定のリンクを必要はありません。</span><span class="sxs-lookup"><span data-stu-id="48b73-128">We won't be implementing Edit, Details and Delete functionality with this application - so we don't need the default links that the scaffold template created for us.</span></span> <span data-ttu-id="48b73-129">/Movies/Index.aspx ファイルを開き、それらを削除します。</span><span class="sxs-lookup"><span data-stu-id="48b73-129">Open up the /Movies/Index.aspx file and remove them.</span></span>

<span data-ttu-id="48b73-130">次に、更新されたテンプレートの表示がどのようにしたら、これらの変更を行う場合のソース コードを示します。</span><span class="sxs-lookup"><span data-stu-id="48b73-130">Here is the source code for what our updated View template should look like once we make these changes:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

<span data-ttu-id="48b73-131">お必要はありません、リンクの作成ため、この例に削除されます。</span><span class="sxs-lookup"><span data-stu-id="48b73-131">It's creating links that we won't need, so we'll delete them for this example.</span></span> <span data-ttu-id="48b73-132">いきますの新規作成、次へ であるため!</span><span class="sxs-lookup"><span data-stu-id="48b73-132">We will keep our Create New link though, as that's next!</span></span> <span data-ttu-id="48b73-133">新機能とその列は削除されて、アプリの外観を次に示します。</span><span class="sxs-lookup"><span data-stu-id="48b73-133">Here's what our app looks like with that column removed.</span></span>

<span data-ttu-id="48b73-134">[![ムービーの一覧]-[Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="48b73-134">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span></span>

<span data-ttu-id="48b73-135">ムービー データの単純なリスト表示があるようになりました。</span><span class="sxs-lookup"><span data-stu-id="48b73-135">We now have a simple listing of our movie data.</span></span> <span data-ttu-id="48b73-136">ただし、「新規作成」のリンクをクリックしておが表示されますエラー接続されていないようです。</span><span class="sxs-lookup"><span data-stu-id="48b73-136">However, if we click the "Create New" link, we'll get an error as it's not hooked up!</span></span> <span data-ttu-id="48b73-137">作成アクション メソッドを実装し、ユーザーにデータベースに新しいムービーの入力を有効にしてみましょう。</span><span class="sxs-lookup"><span data-stu-id="48b73-137">Let's implement a Create Action method and enable a user to enter new movies in our database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="48b73-138">[前へ](getting-started-with-mvc-part4.md)
> [次へ](getting-started-with-mvc-part6.md)</span><span class="sxs-lookup"><span data-stu-id="48b73-138">[Previous](getting-started-with-mvc-part4.md)
[Next](getting-started-with-mvc-part6.md)</span></span>
