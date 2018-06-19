---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: モデルの検証の追加 |Microsoft ドキュメント
author: shanselman
description: これは、ASP.NET MVC の基本について説明する初心者向けチュートリアルです。 データベースから読み取り/書き込みする単純な web アプリケーションを作成します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 78dd6bdd81fcb51a3a21a8f1ee12b4b2bfc37db5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871949"
---
<a name="adding-validation-to-the-model"></a><span data-ttu-id="afeb2-104">モデルの検証の追加</span><span class="sxs-lookup"><span data-stu-id="afeb2-104">Adding Validation to the Model</span></span>
====================
<span data-ttu-id="afeb2-105">によって[Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="afeb2-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="afeb2-106">これは、ASP.NET MVC の基本について説明する初心者向けチュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="afeb2-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="afeb2-107">データベースから読み取り/書き込みを単純な web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="afeb2-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="afeb2-108">参照してください、 [ASP.NET MVC ラーニング センター](../../../index.md)チュートリアルおよびサンプルは、その他の ASP.NET MVC を検索します。</span><span class="sxs-lookup"><span data-stu-id="afeb2-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="afeb2-109">このセクションで行う、アプリケーション内で入力検証を有効にするために必要なサポートを実装します。</span><span class="sxs-lookup"><span data-stu-id="afeb2-109">In this section we are going to implement the support necessary to enable input validation within our application.</span></span> <span data-ttu-id="afeb2-110">おすることを確認、データベースの内容が正しいこと、常を再試行してください、無効なムービー データを入力すると、エンドユーザーに有用なエラー メッセージを提供します。</span><span class="sxs-lookup"><span data-stu-id="afeb2-110">We'll ensure that our database content is always correct, and provide helpful error messages to end users when they try and enter Movie data which is not valid.</span></span> <span data-ttu-id="afeb2-111">まず、ムービー クラスに少しの検証ロジックを追加します。</span><span class="sxs-lookup"><span data-stu-id="afeb2-111">We'll begin by adding a little validation logic to the Movie class.</span></span>

<span data-ttu-id="afeb2-112">モデル フォルダーを右クリックし、クラスの追加を選択します。</span><span class="sxs-lookup"><span data-stu-id="afeb2-112">Right click on the Model folder and select Add Class.</span></span> <span data-ttu-id="afeb2-113">ムービー、クラスの名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="afeb2-113">Name your class Movie.</span></span>

<span data-ttu-id="afeb2-114">ムービー Entity Model を以前に作成したときに、IDE はムービー クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="afeb2-114">When we created the Movie Entity Model earlier, the IDE created a Movie class.</span></span> <span data-ttu-id="afeb2-115">実際には、ムービー クラスの一部は、1 つのファイルおよび他の部分で指定できます。</span><span class="sxs-lookup"><span data-stu-id="afeb2-115">In fact, part of the Movie class can be in one file and part in another.</span></span> <span data-ttu-id="afeb2-116">これは、部分クラスで呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="afeb2-116">This is called a Partial Class.</span></span> <span data-ttu-id="afeb2-117">別のファイルからムービー クラスを拡張しようとしています。</span><span class="sxs-lookup"><span data-stu-id="afeb2-117">We're going to extend the Movie class from another file.</span></span>

<span data-ttu-id="afeb2-118">システムに検証のヒントを与えるいくつかの属性を持つムービーの部分クラスを指す「関連クラス」を作成します。</span><span class="sxs-lookup"><span data-stu-id="afeb2-118">We'll create a partial movie class that points to a "buddy class" with some attributes that will give validation hints to the system.</span></span> <span data-ttu-id="afeb2-119">タイトルと価格を必須とマークを付けるうまくし、価格が特定の範囲内にあることを要求します。</span><span class="sxs-lookup"><span data-stu-id="afeb2-119">We'll mark the Title and Price as Required, and also insist that the Price be within a certain range.</span></span> <span data-ttu-id="afeb2-120">モデル フォルダーを右クリックし、クラスの追加を選択します。</span><span class="sxs-lookup"><span data-stu-id="afeb2-120">Right click the Models folder and select Add Class.</span></span> <span data-ttu-id="afeb2-121">ムービー、クラスの名前し、[ok] ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="afeb2-121">Name your class Movie and Click the OK button.</span></span> <span data-ttu-id="afeb2-122">ここではどのような部分的なムービー クラスのようになります。</span><span class="sxs-lookup"><span data-stu-id="afeb2-122">Here's what our partial Movie class looks like.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

<span data-ttu-id="afeb2-123">アプリケーションを再実行して、価格が 100 以上とするムービーを入力しようとしてください。</span><span class="sxs-lookup"><span data-stu-id="afeb2-123">Re-Run your application and try to enter a movie with a price over 100.</span></span> <span data-ttu-id="afeb2-124">フォームを送信した後にエラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="afeb2-124">You'll get an error after you've submitted the form.</span></span> <span data-ttu-id="afeb2-125">エラーは、サーバー側で検出され、フォームを送信した後に発生します。</span><span class="sxs-lookup"><span data-stu-id="afeb2-125">The error is caught on the server side and occurs after the Form is POSTed.</span></span> <span data-ttu-id="afeb2-126">ASP.NET MVC の組み込み HTML ヘルパーがあるため、エラー メッセージが表示され、ご利用の米国 textbox 要素内の値を維持する方法に注意してください。</span><span class="sxs-lookup"><span data-stu-id="afeb2-126">Notice how ASP.NET MVC's built-in HTML helpers were smart enough to display the error message and maintain the values for us within the textbox elements:</span></span>

<span data-ttu-id="afeb2-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="afeb2-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span></span>

<span data-ttu-id="afeb2-128">これはとても便利が、できれば便利なことが通知され、ユーザー、クライアント側で、すぐに、サーバーが関与する前にします。</span><span class="sxs-lookup"><span data-stu-id="afeb2-128">This works great, but it'd be nice if we could tell the user on the client-side, immediately, before the server gets involved.</span></span>

<span data-ttu-id="afeb2-129">JavaScript でのいくつかのクライアント側検証を有効にしてみましょう。</span><span class="sxs-lookup"><span data-stu-id="afeb2-129">Let's enable some client-side validation with JavaScript.</span></span>

## <a name="adding-client-side-validation"></a><span data-ttu-id="afeb2-130">クライアント側の検証を追加します。</span><span class="sxs-lookup"><span data-stu-id="afeb2-130">Adding Client-Side Validation</span></span>

<span data-ttu-id="afeb2-131">ムービー クラスは、いくつかの検証属性を既に持っているのでおするだけで済みます、Create.aspx ビュー テンプレートに、いくつかの JavaScript ファイルを追加しを実行するクライアント側の検証を有効にするコードの行を追加します。</span><span class="sxs-lookup"><span data-stu-id="afeb2-131">Since our Movie class already has some validation attributes, we'll just need to add a few JavaScript files to our Create.aspx View template and add a line of code to enable client-side validation to take place.</span></span>

<span data-ttu-id="afeb2-132">内からは、VWD はムービー ビュー/フォルダーを移動し、Create.aspx を開きます。</span><span class="sxs-lookup"><span data-stu-id="afeb2-132">From within VWD go our Views/Movie folder and open up Create.aspx.</span></span>

<span data-ttu-id="afeb2-133">ソリューション エクスプ ローラーで、Scripts フォルダーを開きに次の 3 つのスクリプト内でドラッグして、&lt;ヘッド&gt;タグ。</span><span class="sxs-lookup"><span data-stu-id="afeb2-133">Open up the Scripts folder in the Solution Explorer and drag the following three scripts to within the &lt;head&gt; tag.</span></span>

- <span data-ttu-id="afeb2-134">MicrosoftAjax.js</span><span class="sxs-lookup"><span data-stu-id="afeb2-134">MicrosoftAjax.js</span></span>
- <span data-ttu-id="afeb2-135">MicrosoftMvcValidation.js</span><span class="sxs-lookup"><span data-stu-id="afeb2-135">MicrosoftMvcValidation.js</span></span>

<span data-ttu-id="afeb2-136">これらのスクリプト ファイルをこの順序で表示します。</span><span class="sxs-lookup"><span data-stu-id="afeb2-136">You want these script files to appear in this order.</span></span>

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

<span data-ttu-id="afeb2-137">また、この単一行、Html.BeginForm 上を追加します。</span><span class="sxs-lookup"><span data-stu-id="afeb2-137">Also, add this single line above the Html.BeginForm:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

<span data-ttu-id="afeb2-138">IDE 内でのコードを次に示します。</span><span class="sxs-lookup"><span data-stu-id="afeb2-138">Here's the code shown within the IDE.</span></span>

<span data-ttu-id="afeb2-139">[![映画 - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="afeb2-139">[![Movies - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span></span>

<span data-ttu-id="afeb2-140">アプリケーションを実行し、/Movies/Create をもう一度を参照してください、すべてのデータを入力することがなく作成 をクリックします。</span><span class="sxs-lookup"><span data-stu-id="afeb2-140">Run your application and visit /Movies/Create again, and click Create without entering any data.</span></span> <span data-ttu-id="afeb2-141">エラー メッセージを表示すぐにデータの送信と関連付けることができましたフラッシュ ページなしサーバーに至るまでさかのぼってです。</span><span class="sxs-lookup"><span data-stu-id="afeb2-141">The error messages appear immediately without the page flash that we associate with sending data all the way back to the server.</span></span> <span data-ttu-id="afeb2-142">これはため、ASP.NET MVC は今すぐ入力を検証する両方で JavaScript を使用して)、クライアントとサーバーです。</span><span class="sxs-lookup"><span data-stu-id="afeb2-142">This is because ASP.NET MVC is now validating the input on both the client (using JavaScript) and on the server.</span></span>

<span data-ttu-id="afeb2-143">[![Windows Internet Explorer の作成-](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="afeb2-143">[![Create - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span></span>

<span data-ttu-id="afeb2-144">これは、適切なが探して!</span><span class="sxs-lookup"><span data-stu-id="afeb2-144">This is looking good!</span></span> <span data-ttu-id="afeb2-145">データベースに 1 つの列の追加を今すぐ追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="afeb2-145">Let's now add one additional column to the database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="afeb2-146">[前へ](getting-started-with-mvc-part6.md)
> [次へ](getting-started-with-mvc-part8.md)</span><span class="sxs-lookup"><span data-stu-id="afeb2-146">[Previous](getting-started-with-mvc-part6.md)
[Next](getting-started-with-mvc-part8.md)</span></span>
