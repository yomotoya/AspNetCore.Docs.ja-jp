---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
title: "追加するメソッドを作成し、ビューを作成 |Microsoft ドキュメント"
author: shanselman
description: "これは、ASP.NET MVC の基本について説明する初心者向けチュートリアルです。 データベースから読み取り/書き込みする単純な web アプリケーションを作成します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: a3a90963-0286-4fa0-9b3d-c230cc18b0a3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
msc.type: authoredcontent
ms.openlocfilehash: 36b3d6ef0432292f21ecd8f29ea2d88ee8867436
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
<a name="adding-a-create-method-and-create-view"></a><span data-ttu-id="d4c19-104">追加するメソッドを作成し、ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="d4c19-104">Adding a Create Method and Create View</span></span>
====================
<span data-ttu-id="d4c19-105">によって[Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="d4c19-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="d4c19-106">これは、ASP.NET MVC の基本について説明する初心者向けチュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="d4c19-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="d4c19-107">データベースから読み取り/書き込みを単純な web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="d4c19-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="d4c19-108">参照してください、 [ASP.NET MVC ラーニング センター](../../../index.md)チュートリアルおよびサンプルは、その他の ASP.NET MVC を検索します。</span><span class="sxs-lookup"><span data-stu-id="d4c19-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="d4c19-109">このセクションで行う、データベースに新しいムービーを作成するユーザーを有効にするために必要なサポートを実装します。</span><span class="sxs-lookup"><span data-stu-id="d4c19-109">In this section we are going to implement the support necessary to enable users to create new movies in our database.</span></span> <span data-ttu-id="d4c19-110">映画/作成 URL アクションを実装することによってこれを実行します。</span><span class="sxs-lookup"><span data-stu-id="d4c19-110">We'll do this by implementing the /Movies/Create URL action.</span></span>

<span data-ttu-id="d4c19-111">/作成の映画の URL を実装することは、次の 2 ステップ プロセスです。</span><span class="sxs-lookup"><span data-stu-id="d4c19-111">Implementing the /Movies/Create URL is a two step process.</span></span> <span data-ttu-id="d4c19-112">ユーザーはまず/作成の映画の URL を訪問したときに、新しいムービーの入力を記入できる HTML フォームに表示するとします。</span><span class="sxs-lookup"><span data-stu-id="d4c19-112">When a user first visits the /Movies/Create URL we want to show them an HTML form that they can fill out to enter a new movie.</span></span> <span data-ttu-id="d4c19-113">次に、ユーザーは、フォームや、データをサーバーに戻すの投稿を送信するときにポストされた内容を取得し、データベース内に保存します。</span><span class="sxs-lookup"><span data-stu-id="d4c19-113">Then, when the user submits the form and posts the data back to the server, we want to retrieve the posted contents and save it into our database.</span></span>

<span data-ttu-id="d4c19-114">MoviesController クラス内で 2 つの Create() メソッド内でこれら 2 つの手順を実装します。</span><span class="sxs-lookup"><span data-stu-id="d4c19-114">We'll implement these two steps within two Create() methods within our MoviesController class.</span></span> <span data-ttu-id="d4c19-115">表示する 1 つのメソッドは、&lt;フォーム&gt;ユーザーが新しいムービーの作成に記入する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d4c19-115">One method will show the &lt;form&gt; that the user should fill out to create a new movie.</span></span> <span data-ttu-id="d4c19-116">2 番目のメソッドは、ユーザーが送信するときに、ポストされたデータの処理を処理する、&lt;フォーム&gt;サーバーにバックアップし、データベース内で新しいムービーを保存します。</span><span class="sxs-lookup"><span data-stu-id="d4c19-116">The second method will handle processing the posted data when the user submits the &lt;form&gt; back to the server, and save a new Movie within our database.</span></span>

<span data-ttu-id="d4c19-117">以下のコードは、これを実装する、MoviesController クラスに追加されます。</span><span class="sxs-lookup"><span data-stu-id="d4c19-117">Below is the code we'll add to our MoviesController class to implement this:</span></span>

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample1.cs)]

<span data-ttu-id="d4c19-118">上記のコードには、すべてのしなければならない、コント ローラー内でコードが含まれます。</span><span class="sxs-lookup"><span data-stu-id="d4c19-118">The above code contains all of the code that we'll need within our Controller.</span></span>

<span data-ttu-id="d4c19-119">使用して、ユーザーに、フォームを表示するビューの作成テンプレートを実装してみましょう。</span><span class="sxs-lookup"><span data-stu-id="d4c19-119">Let's now implement the Create View template that we'll use to display a form to the user.</span></span> <span data-ttu-id="d4c19-120">おによってを最初の Create メソッドを右クリックし、ムービー フォーム用のビュー テンプレートを作成する「ビューの追加」を選択します。</span><span class="sxs-lookup"><span data-stu-id="d4c19-120">We'll right click in the first Create method and select "Add View" to create the view template for our Movie form.</span></span>

<span data-ttu-id="d4c19-121">おしようとしてビュー テンプレート「ムービー」を渡すと、そのビューのデータ クラスとは、「作成」テンプレートを「スキャフォールディング」することを指定することを選択します。</span><span class="sxs-lookup"><span data-stu-id="d4c19-121">We'll select that we are going to pass the view template a "Movie" as its view data class, and indicate that we want to "scaffold" a "Create" template.</span></span>

<span data-ttu-id="d4c19-122">[![ビューを追加します。](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d4c19-122">[![Add View](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)</span></span>

<span data-ttu-id="d4c19-123">[追加] ボタンをクリックすると、\Movies\Create.aspx ビュー テンプレートが作成されます。</span><span class="sxs-lookup"><span data-stu-id="d4c19-123">After you click the Add button, \Movies\Create.aspx View template will be created for you.</span></span> <span data-ttu-id="d4c19-124">"コンテンツを表示 ドロップダウン リストから、作成 を選択すると、ためビューの追加 ダイアログに自動的に「スキャフォールディングされました」既定のコンテンツをいくつかご利用の米国。</span><span class="sxs-lookup"><span data-stu-id="d4c19-124">Because we selected "Create" from the "view content" dropdown, the Add View dialog automatically "scaffolded" some default content for us.</span></span> <span data-ttu-id="d4c19-125">スキャフォールディング作成 HTML&lt;フォーム&gt;、進むには、メッセージの検証エラーの場所で、クラスの各プロパティのラベルとフィールドを作成してムービーのスキャフォールディングを認識、ためです。</span><span class="sxs-lookup"><span data-stu-id="d4c19-125">The scaffolding created an HTML &lt;form&gt;, a place for validation error messages to go, and since scaffolding knows about Movies, it created Label and Fields for each property of our class.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part6/samples/sample2.aspx)]

<span data-ttu-id="d4c19-126">データベースに自動的に利用できますが、ムービー、ID、ためみましょうモデルは削除それらのフィールドを参照。このビューを作成するの id。</span><span class="sxs-lookup"><span data-stu-id="d4c19-126">Since our database automatically gives a Movie an ID, let's remove those fields that reference model.Id from our Create View.</span></span> <span data-ttu-id="d4c19-127">削除後に 7 行&lt;凡例&gt;フィールド&lt;/legend&gt;されないようにするため、ID フィールドが表示されます。</span><span class="sxs-lookup"><span data-stu-id="d4c19-127">Remove the 7 lines after &lt;legend&gt;Fields&lt;/legend&gt; as they show the ID field that we don't want.</span></span>

<span data-ttu-id="d4c19-128">みましょう今すぐ新しいムービーを作成し、データベースに追加します。</span><span class="sxs-lookup"><span data-stu-id="d4c19-128">Let's now create a new movie and add it to the database.</span></span> <span data-ttu-id="d4c19-129">これには、アプリケーションを再実行している、うまくし、参照してください、、"/映画"「作成」に新しいビデオを追加するリンクの URL をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d4c19-129">We'll do this by running the application again and visit the "/Movies" URL and click the "Create" link to add a new Movie.</span></span>

<span data-ttu-id="d4c19-130">[![Windows Internet Explorer の作成-](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="d4c19-130">[![Create - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)</span></span>

<span data-ttu-id="d4c19-131">作成ボタンをクリックしておある投稿するときに戻る (HTTP POST) を介してこのフォームを作成したばかり/Movies/Create メソッド上のデータ。</span><span class="sxs-lookup"><span data-stu-id="d4c19-131">When we click the Create button, we'll be posting back (via HTTP POST) the data on this form to the /Movies/Create method that we just created.</span></span> <span data-ttu-id="d4c19-132">まったく同様にシステムに自動的に URL から"numTimes"と"name"パラメーターを要したし、前のメソッドのパラメーターにマップして、システムは自動的に投稿からはフォームのフィールドを取得してオブジェクトにマップします。</span><span class="sxs-lookup"><span data-stu-id="d4c19-132">Just like when the system automatically took the "numTimes" and "name " parameter out of the URL and mapped them to parameters on a method earlier, the system will automatically take the Form Fields from a POST and map them to an object.</span></span> <span data-ttu-id="d4c19-133">この場合、"ReleaseDate"や"Title"のような HTML のフィールドの値は、ムービーの新しいインスタンスの正しいプロパティを自動的に配置されます。</span><span class="sxs-lookup"><span data-stu-id="d4c19-133">In this case, values from fields in HTML like "ReleaseDate" and "Title" will automatically be put into the correct properties of a new instance of a Movie.</span></span>

<span data-ttu-id="d4c19-134">2 番目の作成方法、MoviesController からもう一度見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="d4c19-134">Let's look at the second Create method from our MoviesController again.</span></span> <span data-ttu-id="d4c19-135">「ムービー」オブジェクトを引数としてかかる方法に注意してください。</span><span class="sxs-lookup"><span data-stu-id="d4c19-135">Notice how it takes a "Movie" object as an argument:</span></span>

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample3.cs)]

<span data-ttu-id="d4c19-136">このムービー オブジェクトが、作成するアクション メソッドの [HttpPost] バージョンに渡された、およびデータベースに保存し、ムービーの一覧で、保存された結果を表示する Index() アクション メソッドに、ユーザーをリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="d4c19-136">This Movie object was then passed to the [HttpPost] version of our Create action method, and we saved it in the database and then redirected the user back to the Index() action method which will show the saved result in the movie list:</span></span>

<span data-ttu-id="d4c19-137">[![ムービーの一覧]-[Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="d4c19-137">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)</span></span>

<span data-ttu-id="d4c19-138">おチェックされないかどうか、映画が正しくただし、および、データベースがタイトルなしでムービーを保存することを許可しません。</span><span class="sxs-lookup"><span data-stu-id="d4c19-138">We aren't checking if our movies are correct, though, and the database won't allow us to save a movie with no Title.</span></span> <span data-ttu-id="d4c19-139">説明し、ユーザー データベースの前に、エラーをスローした場合は便利になります。</span><span class="sxs-lookup"><span data-stu-id="d4c19-139">It'd be nice if we could tell the user that before the database threw an error.</span></span> <span data-ttu-id="d4c19-140">アプリケーションに検証のサポートを追加することで、次を実行します。</span><span class="sxs-lookup"><span data-stu-id="d4c19-140">We'll do this next by adding validation support to our application.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="d4c19-141">[前へ](getting-started-with-mvc-part5.md)
[次へ](getting-started-with-mvc-part7.md)</span><span class="sxs-lookup"><span data-stu-id="d4c19-141">[Previous](getting-started-with-mvc-part5.md)
[Next](getting-started-with-mvc-part7.md)</span></span>
