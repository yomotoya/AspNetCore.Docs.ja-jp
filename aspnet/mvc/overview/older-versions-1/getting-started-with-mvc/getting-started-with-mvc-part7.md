---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: モデル検証の追加 |Microsoft Docs
author: shanselman
description: これは、ASP.NET MVC の基本を紹介する初心者向けチュートリアルです。 読み取りと書き込みをデータベースから単純な web アプリケーションを作成します。
ms.author: riande
ms.date: 08/14/2010
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 4c7867587ba0610f0f1c23d9a0b9fbdc4040de7c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835347"
---
<a name="adding-validation-to-the-model"></a><span data-ttu-id="4347f-104">モデル検証の追加</span><span class="sxs-lookup"><span data-stu-id="4347f-104">Adding Validation to the Model</span></span>
====================
<span data-ttu-id="4347f-105">によって[Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="4347f-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="4347f-106">これは、ASP.NET MVC の基本を紹介する初心者向けチュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="4347f-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="4347f-107">読み取りと書き込みをデータベースから単純な web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="4347f-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="4347f-108">参照してください、 [ASP.NET MVC ラーニング センター](../../../index.md)チュートリアルとサンプルは、その他の ASP.NET MVC を検索します。</span><span class="sxs-lookup"><span data-stu-id="4347f-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="4347f-109">このセクションでは、アプリケーション内での入力の検証を有効にするために必要なサポートを実装するでしょう。</span><span class="sxs-lookup"><span data-stu-id="4347f-109">In this section we are going to implement the support necessary to enable input validation within our application.</span></span> <span data-ttu-id="4347f-110">しますが、データベースの内容が正しい常として無効なムービー データを入力するときに役に立つエラー メッセージをエンドユーザーに提供できます。</span><span class="sxs-lookup"><span data-stu-id="4347f-110">We'll ensure that our database content is always correct, and provide helpful error messages to end users when they try and enter Movie data which is not valid.</span></span> <span data-ttu-id="4347f-111">まず、ムービー クラスに少しの検証ロジックを追加します。</span><span class="sxs-lookup"><span data-stu-id="4347f-111">We'll begin by adding a little validation logic to the Movie class.</span></span>

<span data-ttu-id="4347f-112">モデル フォルダーを右クリックし、クラスの追加を選択します。</span><span class="sxs-lookup"><span data-stu-id="4347f-112">Right click on the Model folder and select Add Class.</span></span> <span data-ttu-id="4347f-113">ムービー、クラスの名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="4347f-113">Name your class Movie.</span></span>

<span data-ttu-id="4347f-114">前に、ムービー エンティティ モデルを作成したときに、IDE はムービー クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="4347f-114">When we created the Movie Entity Model earlier, the IDE created a Movie class.</span></span> <span data-ttu-id="4347f-115">実際には、ムービー クラスの一部は、1 つのファイルおよび別の部分で指定できます。</span><span class="sxs-lookup"><span data-stu-id="4347f-115">In fact, part of the Movie class can be in one file and part in another.</span></span> <span data-ttu-id="4347f-116">これは、部分クラスと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="4347f-116">This is called a Partial Class.</span></span> <span data-ttu-id="4347f-117">別のファイルからムービー クラスを拡張しようとしています。</span><span class="sxs-lookup"><span data-stu-id="4347f-117">We're going to extend the Movie class from another file.</span></span>

<span data-ttu-id="4347f-118">システムに検証のヒントを与えるいくつかの属性を持つムービーの部分クラスを指す「buddy クラス」を作成します。</span><span class="sxs-lookup"><span data-stu-id="4347f-118">We'll create a partial movie class that points to a "buddy class" with some attributes that will give validation hints to the system.</span></span> <span data-ttu-id="4347f-119">タイトルと、必要に応じて、価格のマークがされも、価格が特定の範囲内にあると主張します。</span><span class="sxs-lookup"><span data-stu-id="4347f-119">We'll mark the Title and Price as Required, and also insist that the Price be within a certain range.</span></span> <span data-ttu-id="4347f-120">Models フォルダーを右クリックし、クラスの追加を選択します。</span><span class="sxs-lookup"><span data-stu-id="4347f-120">Right click the Models folder and select Add Class.</span></span> <span data-ttu-id="4347f-121">ムービー、クラスの名前し、[ok] ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="4347f-121">Name your class Movie and Click the OK button.</span></span> <span data-ttu-id="4347f-122">どのような部分的なムービー クラスのようになります次に示します。</span><span class="sxs-lookup"><span data-stu-id="4347f-122">Here's what our partial Movie class looks like.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

<span data-ttu-id="4347f-123">アプリケーションを再実行して、価格が 100 を超えるビデオを入力しようとしてください。</span><span class="sxs-lookup"><span data-stu-id="4347f-123">Re-Run your application and try to enter a movie with a price over 100.</span></span> <span data-ttu-id="4347f-124">フォームを送信した後、エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="4347f-124">You'll get an error after you've submitted the form.</span></span> <span data-ttu-id="4347f-125">エラーは、サーバー側で検出され、フォームが投稿された後に発生します。</span><span class="sxs-lookup"><span data-stu-id="4347f-125">The error is caught on the server side and occurs after the Form is POSTed.</span></span> <span data-ttu-id="4347f-126">ASP.NET MVC の組み込み HTML ヘルパーが、エラー メッセージが表示され、テキスト ボックス要素内の値を維持する方法に注意してください。</span><span class="sxs-lookup"><span data-stu-id="4347f-126">Notice how ASP.NET MVC's built-in HTML helpers were smart enough to display the error message and maintain the values for us within the textbox elements:</span></span>

<span data-ttu-id="4347f-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4347f-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span></span>

<span data-ttu-id="4347f-128">すばらしく、機能しますが、できれば言う場合は、私たちの知る、ユーザー、クライアント側ですぐに、サーバーが関与する前にします。</span><span class="sxs-lookup"><span data-stu-id="4347f-128">This works great, but it'd be nice if we could tell the user on the client-side, immediately, before the server gets involved.</span></span>

<span data-ttu-id="4347f-129">JavaScript でいくつかのクライアント側検証を有効にしてみましょう。</span><span class="sxs-lookup"><span data-stu-id="4347f-129">Let's enable some client-side validation with JavaScript.</span></span>

## <a name="adding-client-side-validation"></a><span data-ttu-id="4347f-130">クライアント側の検証を追加します。</span><span class="sxs-lookup"><span data-stu-id="4347f-130">Adding Client-Side Validation</span></span>

<span data-ttu-id="4347f-131">ムービー クラスは、いくつかの検証属性を既に持っているのでだけ Create.aspx ビュー テンプレートにいくつかの JavaScript ファイルを追加し、実行するクライアント側の検証を有効にするコード行を追加する必要あります。</span><span class="sxs-lookup"><span data-stu-id="4347f-131">Since our Movie class already has some validation attributes, we'll just need to add a few JavaScript files to our Create.aspx View template and add a line of code to enable client-side validation to take place.</span></span>

<span data-ttu-id="4347f-132">内からは、VWD は、ムービー ビュー/フォルダーを移動し、Create.aspx を開きます。</span><span class="sxs-lookup"><span data-stu-id="4347f-132">From within VWD go our Views/Movie folder and open up Create.aspx.</span></span>

<span data-ttu-id="4347f-133">ソリューション エクスプ ローラーで、Scripts フォルダーを開きに次の 3 つのスクリプト内でのドラッグ、&lt;ヘッド&gt;タグ。</span><span class="sxs-lookup"><span data-stu-id="4347f-133">Open up the Scripts folder in the Solution Explorer and drag the following three scripts to within the &lt;head&gt; tag.</span></span>

- <span data-ttu-id="4347f-134">MicrosoftAjax.js</span><span class="sxs-lookup"><span data-stu-id="4347f-134">MicrosoftAjax.js</span></span>
- <span data-ttu-id="4347f-135">MicrosoftMvcValidation.js</span><span class="sxs-lookup"><span data-stu-id="4347f-135">MicrosoftMvcValidation.js</span></span>

<span data-ttu-id="4347f-136">これらのスクリプト ファイルをこの順序で表示します。</span><span class="sxs-lookup"><span data-stu-id="4347f-136">You want these script files to appear in this order.</span></span>

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

<span data-ttu-id="4347f-137">また、この 1 行、Html.BeginForm 上を追加します。</span><span class="sxs-lookup"><span data-stu-id="4347f-137">Also, add this single line above the Html.BeginForm:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

<span data-ttu-id="4347f-138">IDE 内でのコードを次に示します。</span><span class="sxs-lookup"><span data-stu-id="4347f-138">Here's the code shown within the IDE.</span></span>

<span data-ttu-id="4347f-139">[![ビデオ - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="4347f-139">[![Movies - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span></span>

<span data-ttu-id="4347f-140">アプリケーションを実行し、/Movies/Create をもう一度、アクセスしすべてのデータを入力しなくても作成 をクリックします。</span><span class="sxs-lookup"><span data-stu-id="4347f-140">Run your application and visit /Movies/Create again, and click Create without entering any data.</span></span> <span data-ttu-id="4347f-141">エラー メッセージを表示すぐに、ページのデータの送信に関連付けられたことをフラッシュせず、サーバーに至るまでさかのぼって。</span><span class="sxs-lookup"><span data-stu-id="4347f-141">The error messages appear immediately without the page flash that we associate with sending data all the way back to the server.</span></span> <span data-ttu-id="4347f-142">これは ASP.NET MVC は、両方の入力の (JavaScript を使用して) クライアントを検証ようになりましたがあるため、サーバー上です。</span><span class="sxs-lookup"><span data-stu-id="4347f-142">This is because ASP.NET MVC is now validating the input on both the client (using JavaScript) and on the server.</span></span>

<span data-ttu-id="4347f-143">[![Windows Internet Explorer の作成-](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="4347f-143">[![Create - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span></span>

<span data-ttu-id="4347f-144">これは順調です!</span><span class="sxs-lookup"><span data-stu-id="4347f-144">This is looking good!</span></span> <span data-ttu-id="4347f-145">データベースに 1 つの列の追加を今すぐ追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="4347f-145">Let's now add one additional column to the database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4347f-146">[前へ](getting-started-with-mvc-part6.md)
> [次へ](getting-started-with-mvc-part8.md)</span><span class="sxs-lookup"><span data-stu-id="4347f-146">[Previous](getting-started-with-mvc-part6.md)
[Next](getting-started-with-mvc-part8.md)</span></span>
