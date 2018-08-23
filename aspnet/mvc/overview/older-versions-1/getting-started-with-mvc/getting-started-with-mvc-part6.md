---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
title: 追加するメソッドを作成し、ビューを作成する |Microsoft Docs
author: shanselman
description: これは、ASP.NET MVC の基本を紹介する初心者向けチュートリアルです。 読み取りと書き込みをデータベースから単純な web アプリケーションを作成します。
ms.author: riande
ms.date: 08/14/2010
ms.assetid: a3a90963-0286-4fa0-9b3d-c230cc18b0a3
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
msc.type: authoredcontent
ms.openlocfilehash: 546c3e0a24ecd0d916c79e9ad12f62b926c760c5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832788"
---
<a name="adding-a-create-method-and-create-view"></a><span data-ttu-id="aa334-104">追加するメソッドを作成し、ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="aa334-104">Adding a Create Method and Create View</span></span>
====================
<span data-ttu-id="aa334-105">によって[Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="aa334-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="aa334-106">これは、ASP.NET MVC の基本を紹介する初心者向けチュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="aa334-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="aa334-107">読み取りと書き込みをデータベースから単純な web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="aa334-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="aa334-108">参照してください、 [ASP.NET MVC ラーニング センター](../../../index.md)チュートリアルとサンプルは、その他の ASP.NET MVC を検索します。</span><span class="sxs-lookup"><span data-stu-id="aa334-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="aa334-109">このセクションでは、データベースに新しいムービーを作成するユーザーを有効にするために必要なサポートを実装するでしょう。</span><span class="sxs-lookup"><span data-stu-id="aa334-109">In this section we are going to implement the support necessary to enable users to create new movies in our database.</span></span> <span data-ttu-id="aa334-110">映画/作成 URL 動作を実装することでこれを実行します。</span><span class="sxs-lookup"><span data-stu-id="aa334-110">We'll do this by implementing the /Movies/Create URL action.</span></span>

<span data-ttu-id="aa334-111">映画/作成 URL の実装は、2 ステップのプロセスです。</span><span class="sxs-lookup"><span data-stu-id="aa334-111">Implementing the /Movies/Create URL is a two step process.</span></span> <span data-ttu-id="aa334-112">場合ユーザーに最初のムービーの作成/URL にアクセスすることに、新しいムービーの入力に記入する HTML フォームに表示します。</span><span class="sxs-lookup"><span data-stu-id="aa334-112">When a user first visits the /Movies/Create URL we want to show them an HTML form that they can fill out to enter a new movie.</span></span> <span data-ttu-id="aa334-113">次に、ユーザーは、フォームと、データをサーバーに投稿を送信するときにするポストされた内容を取得し、データベースに保存します。</span><span class="sxs-lookup"><span data-stu-id="aa334-113">Then, when the user submits the form and posts the data back to the server, we want to retrieve the posted contents and save it into our database.</span></span>

<span data-ttu-id="aa334-114">2 つの Create() メソッド内でこれら 2 つの手順、MoviesController クラス内で実装します。</span><span class="sxs-lookup"><span data-stu-id="aa334-114">We'll implement these two steps within two Create() methods within our MoviesController class.</span></span> <span data-ttu-id="aa334-115">表示する 1 つのメソッドは、&lt;フォーム&gt;ユーザーが新しいムービーの作成に記入する必要があります。</span><span class="sxs-lookup"><span data-stu-id="aa334-115">One method will show the &lt;form&gt; that the user should fill out to create a new movie.</span></span> <span data-ttu-id="aa334-116">2 番目のメソッドは、ユーザーが送信するときに、ポストされたデータの処理を処理する、&lt;フォーム&gt;サーバーに戻すし、データベース内で新しいムービーを保存します。</span><span class="sxs-lookup"><span data-stu-id="aa334-116">The second method will handle processing the posted data when the user submits the &lt;form&gt; back to the server, and save a new Movie within our database.</span></span>

<span data-ttu-id="aa334-117">以下のコードは、これを実装する MoviesController クラスに追加します。</span><span class="sxs-lookup"><span data-stu-id="aa334-117">Below is the code we'll add to our MoviesController class to implement this:</span></span>

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample1.cs)]

<span data-ttu-id="aa334-118">上記のコードには、すべての必要があります、コント ローラー内でコードが含まれます。</span><span class="sxs-lookup"><span data-stu-id="aa334-118">The above code contains all of the code that we'll need within our Controller.</span></span>

<span data-ttu-id="aa334-119">使用して、フォームをユーザーに表示するビューの作成テンプレートを今すぐ実装してみましょう。</span><span class="sxs-lookup"><span data-stu-id="aa334-119">Let's now implement the Create View template that we'll use to display a form to the user.</span></span> <span data-ttu-id="aa334-120">最初の Create メソッドを右クリックして、ムービー、フォーム ビュー テンプレートを作成する「ビューの追加」を選択します。</span><span class="sxs-lookup"><span data-stu-id="aa334-120">We'll right click in the first Create method and select "Add View" to create the view template for our Movie form.</span></span>

<span data-ttu-id="aa334-121">そのビューのデータ クラスと「ムービー」ビュー テンプレートを渡すことは、テンプレートの「作成」を「スキャフォールディング」することを示すことを選択します。</span><span class="sxs-lookup"><span data-stu-id="aa334-121">We'll select that we are going to pass the view template a "Movie" as its view data class, and indicate that we want to "scaffold" a "Create" template.</span></span>

<span data-ttu-id="aa334-122">[![ビューを追加します。](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="aa334-122">[![Add View](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)</span></span>

<span data-ttu-id="aa334-123">[追加] ボタンをクリックした後 \Movies\Create.aspx ビュー テンプレートが作成されます。</span><span class="sxs-lookup"><span data-stu-id="aa334-123">After you click the Add button, \Movies\Create.aspx View template will be created for you.</span></span> <span data-ttu-id="aa334-124">"コンテンツの表示 ドロップダウン リストから 作成 を選択したため、ビューの追加 ダイアログに自動的に「スキャフォールディングされた」既定のコンテンツをします。</span><span class="sxs-lookup"><span data-stu-id="aa334-124">Because we selected "Create" from the "view content" dropdown, the Add View dialog automatically "scaffolded" some default content for us.</span></span> <span data-ttu-id="aa334-125">スキャフォールディングが作成、HTML&lt;フォーム&gt;進むには、メッセージの検証エラーの場所で、クラスの各プロパティのラベルとフィールドを作成して映画のスキャフォールディングを知っている、ため、します。</span><span class="sxs-lookup"><span data-stu-id="aa334-125">The scaffolding created an HTML &lt;form&gt;, a place for validation error messages to go, and since scaffolding knows about Movies, it created Label and Fields for each property of our class.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part6/samples/sample2.aspx)]

<span data-ttu-id="aa334-126">データベースでは、ID、ムービーが自動的には、みましょう削除してそれらのフィールド参照モデル。ビューを作成するの id。</span><span class="sxs-lookup"><span data-stu-id="aa334-126">Since our database automatically gives a Movie an ID, let's remove those fields that reference model.Id from our Create View.</span></span> <span data-ttu-id="aa334-127">後の 7 行を削除&lt;凡例&gt;フィールド&lt;/legend&gt;されないようにするため、ID フィールドを表示するようにします。</span><span class="sxs-lookup"><span data-stu-id="aa334-127">Remove the 7 lines after &lt;legend&gt;Fields&lt;/legend&gt; as they show the ID field that we don't want.</span></span>

<span data-ttu-id="aa334-128">みましょう今すぐ新しいムービーを作成し、データベースに追加します。</span><span class="sxs-lookup"><span data-stu-id="aa334-128">Let's now create a new movie and add it to the database.</span></span> <span data-ttu-id="aa334-129">これには、アプリケーションを再度実行しているを参照してくださいし、"/映画"「作成」が、新しいムービーを追加するリンクの URL をクリックします。</span><span class="sxs-lookup"><span data-stu-id="aa334-129">We'll do this by running the application again and visit the "/Movies" URL and click the "Create" link to add a new Movie.</span></span>

<span data-ttu-id="aa334-130">[![Windows Internet Explorer の作成-](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="aa334-130">[![Create - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)</span></span>

<span data-ttu-id="aa334-131">[作成] ボタンをクリックしてしましたと版もご利用戻る (HTTP POST 経由) を作成した/Movies/Create メソッドには、このフォーム上のデータ。</span><span class="sxs-lookup"><span data-stu-id="aa334-131">When we click the Create button, we'll be posting back (via HTTP POST) the data on this form to the /Movies/Create method that we just created.</span></span> <span data-ttu-id="aa334-132">ときに、システムは自動的に URL から"numTimes"および"name"パラメーターを要したし、前のメソッドのパラメーターにマップすると同様、システムが自動的に投稿から、フォームのフィールドを取得、オブジェクトにマップされます。</span><span class="sxs-lookup"><span data-stu-id="aa334-132">Just like when the system automatically took the "numTimes" and "name " parameter out of the URL and mapped them to parameters on a method earlier, the system will automatically take the Form Fields from a POST and map them to an object.</span></span> <span data-ttu-id="aa334-133">この場合は、"ReleaseDate"や"Title"のような HTML のフィールドの値は、自動的に新しいムービーのインスタンスの適切なプロパティに格納されます。</span><span class="sxs-lookup"><span data-stu-id="aa334-133">In this case, values from fields in HTML like "ReleaseDate" and "Title" will automatically be put into the correct properties of a new instance of a Movie.</span></span>

<span data-ttu-id="aa334-134">2 番目の作成方法、MoviesController からもう一度見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="aa334-134">Let's look at the second Create method from our MoviesController again.</span></span> <span data-ttu-id="aa334-135">「ビデオ」オブジェクトを引数としてかかる方法に注意してください。</span><span class="sxs-lookup"><span data-stu-id="aa334-135">Notice how it takes a "Movie" object as an argument:</span></span>

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample3.cs)]

<span data-ttu-id="aa334-136">このムービー オブジェクトは、Create アクション メソッドの [HttpPost] バージョンに渡されたし、データベースに保存し、ムービーの一覧で、保存された結果を表示する Index() アクション メソッドに、ユーザーがリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="aa334-136">This Movie object was then passed to the [HttpPost] version of our Create action method, and we saved it in the database and then redirected the user back to the Index() action method which will show the saved result in the movie list:</span></span>

<span data-ttu-id="aa334-137">[![ムービーの一覧 - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="aa334-137">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)</span></span>

<span data-ttu-id="aa334-138">私たちは、ムービーが正しいこと、ただし、そのデータベースでタイトルなしでムービーを保存することは許可されませんにチェックインしていません。</span><span class="sxs-lookup"><span data-stu-id="aa334-138">We aren't checking if our movies are correct, though, and the database won't allow us to save a movie with no Title.</span></span> <span data-ttu-id="aa334-139">私たちの知るユーザー データベースの前にエラーをスローする場合は便利になります。</span><span class="sxs-lookup"><span data-stu-id="aa334-139">It'd be nice if we could tell the user that before the database threw an error.</span></span> <span data-ttu-id="aa334-140">検証のサポートをアプリケーションに追加することで、次を実行します。</span><span class="sxs-lookup"><span data-stu-id="aa334-140">We'll do this next by adding validation support to our application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="aa334-141">[前へ](getting-started-with-mvc-part5.md)
> [次へ](getting-started-with-mvc-part7.md)</span><span class="sxs-lookup"><span data-stu-id="aa334-141">[Previous](getting-started-with-mvc-part5.md)
[Next](getting-started-with-mvc-part7.md)</span></span>
