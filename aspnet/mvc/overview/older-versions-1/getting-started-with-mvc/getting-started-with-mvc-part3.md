---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
title: ビューの追加 |Microsoft Docs
author: shanselman
description: これは、ASP.NET MVC の基本を紹介する初心者向けチュートリアルです。 読み取りと書き込みをデータベースから単純な web アプリケーションを作成します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: e8f1515c-c277-47ff-a23e-224118f13f02
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
msc.type: authoredcontent
ms.openlocfilehash: 942d329cf0451101a24da4c3facd38b2813653d1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365673"
---
<a name="adding-a-view"></a><span data-ttu-id="f0d27-104">ビューの追加</span><span class="sxs-lookup"><span data-stu-id="f0d27-104">Adding a View</span></span>
====================
<span data-ttu-id="f0d27-105">によって[Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="f0d27-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="f0d27-106">これは、ASP.NET MVC の基本を紹介する初心者向けチュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="f0d27-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="f0d27-107">読み取りと書き込みをデータベースから単純な web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="f0d27-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="f0d27-108">参照してください、 [ASP.NET MVC ラーニング センター](../../../index.md)チュートリアルとサンプルは、その他の ASP.NET MVC を検索します。</span><span class="sxs-lookup"><span data-stu-id="f0d27-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="f0d27-109">このセクションでは、ビュー テンプレート ファイルを使用して、クライアントへの生成の HTML 応答を明確にカプセル化、HelloWorldController クラスがあっても方法を見てはいきます。</span><span class="sxs-lookup"><span data-stu-id="f0d27-109">In this section we are going to look at how we can have our HelloWorldController class use a View template file to cleanly encapsulate generating HTML responses back to a client.</span></span>

<span data-ttu-id="f0d27-110">Index メソッドをテンプレートの表示を使用して開始しましょう。</span><span class="sxs-lookup"><span data-stu-id="f0d27-110">Let's start by using a View template with our Index method.</span></span> <span data-ttu-id="f0d27-111">メソッドには、インデックスが呼び出され、HelloWorldController になります。</span><span class="sxs-lookup"><span data-stu-id="f0d27-111">Our method is called Index and it's in the HelloWorldController.</span></span> <span data-ttu-id="f0d27-112">現在、Index() メソッドは、コント ローラー クラス内でハードコーディングされているメッセージ文字列を返します。</span><span class="sxs-lookup"><span data-stu-id="f0d27-112">Currently our Index() method returns a string with a message that is hardcoded within the Controller class.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample1.cs)]

<span data-ttu-id="f0d27-113">代わりに次のように Index メソッドを今すぐ変更してみましょう。</span><span class="sxs-lookup"><span data-stu-id="f0d27-113">Let's now change the Index method to instead look like this:</span></span>

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample2.cs)]

<span data-ttu-id="f0d27-114">Index() 方法を使用できるプロジェクトにビュー テンプレートを今すぐ追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="f0d27-114">Let's now add a View template to our project that we can use for our Index() method.</span></span> <span data-ttu-id="f0d27-115">Index メソッドの途中でどこかにマウスを右クリックし、[ビューの追加] をクリックしてください.</span><span class="sxs-lookup"><span data-stu-id="f0d27-115">To do this, right-click with your mouse somewhere in the middle of the Index method and click Add View...</span></span>

![イメージ](getting-started-with-mvc-part3/_static/image1.png)

<span data-ttu-id="f0d27-117">私たちの Index メソッドで使用できるテンプレートの表示を作成する方法の一部のオプションを提供する「ビューの追加」ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f0d27-117">This will bring up the "Add View" dialog which provides us some options for how we want to create a view template that can be used by our Index method.</span></span> <span data-ttu-id="f0d27-118">ここでは、しないは、内容を変更し、[追加] ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="f0d27-118">For now, don't change anything, and just click the Add button.</span></span>

<span data-ttu-id="f0d27-119">[![[追加] ダイアログの表示](getting-started-with-mvc-part3/_static/image3.png)](getting-started-with-mvc-part3/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="f0d27-119">[![Add View Dialog](getting-started-with-mvc-part3/_static/image3.png)](getting-started-with-mvc-part3/_static/image2.png)</span></span>

<span data-ttu-id="f0d27-120">[追加] をクリックすると、新しいフォルダーと新しいファイルに表示されます、ソリューション フォルダー以下のようです。</span><span class="sxs-lookup"><span data-stu-id="f0d27-120">After you click Add, a new folder and a new file will appear in the Solution Folder, as seen here.</span></span> <span data-ttu-id="f0d27-121">そのフォルダー内で下にあるビュー、および、Index.aspx ファイル HelloWorld フォルダーがあります。</span><span class="sxs-lookup"><span data-stu-id="f0d27-121">I now have a HelloWorld folder under Views, and an Index.aspx file inside that folder.</span></span>

<span data-ttu-id="f0d27-122">[![solutionexplorerwithindex](getting-started-with-mvc-part3/_static/image5.png)](getting-started-with-mvc-part3/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="f0d27-122">[![solutionexplorerwithindex](getting-started-with-mvc-part3/_static/image5.png)](getting-started-with-mvc-part3/_static/image4.png)</span></span>

<span data-ttu-id="f0d27-123">新しいインデックスのファイルも既に開かれていると編集を開始できます。</span><span class="sxs-lookup"><span data-stu-id="f0d27-123">The new Index file is also already opened and ready for editing.</span></span> <span data-ttu-id="f0d27-124">最初のいくつかのテキストを追加する&lt;h2&gt;インデックス&lt;/h2&gt;などの"Hello World."</span><span class="sxs-lookup"><span data-stu-id="f0d27-124">Add some text under the first &lt;h2&gt;Index&lt;/h2&gt; like "Hello World."</span></span>

[!code-html[Main](getting-started-with-mvc-part3/samples/sample3.html)]

<span data-ttu-id="f0d27-125">アプリケーションを実行しを参照してください[ `http://localhost:xx/HelloWorld` ](http://localhostxx)お使いのブラウザーでもう一度です。</span><span class="sxs-lookup"><span data-stu-id="f0d27-125">Run your application and visit [`http://localhost:xx/HelloWorld`](http://localhostxx) again in your browser.</span></span> <span data-ttu-id="f0d27-126">この例では、コント ローラーに Index メソッドは、作業を実行していないが、"戻り View()"応答をクライアントにレンダリングするビュー テンプレート ファイルを使用したいことが示されているを呼び出すでした。</span><span class="sxs-lookup"><span data-stu-id="f0d27-126">The Index method in our controller in this example didn't do any work, but did call "return View()" which indicated that we wanted to use a view template file to render a response back to the client.</span></span> <span data-ttu-id="f0d27-127">明示的に使用するビュー テンプレート ファイルの名前を指定しましたいない、ため \Views\HelloWorld フォルダー内、Index.aspx ビュー ファイルを使用する ASP.NET MVC が既定値。</span><span class="sxs-lookup"><span data-stu-id="f0d27-127">Because we did not explicitly specify the name of the view template file to use, ASP.NET MVC defaulted to using the Index.aspx view file within the \Views\HelloWorld folder.</span></span> <span data-ttu-id="f0d27-128">これで、文字列をハードコーディングしたため、ビューに表示されています。</span><span class="sxs-lookup"><span data-stu-id="f0d27-128">Now we see the string we hard-coded in our View.</span></span>

<span data-ttu-id="f0d27-129">[![Windows Internet Explorer のインデックス](getting-started-with-mvc-part3/_static/image7.png)](getting-started-with-mvc-part3/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="f0d27-129">[![Index - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image7.png)](getting-started-with-mvc-part3/_static/image6.png)</span></span>

<span data-ttu-id="f0d27-130">ように見えますね。</span><span class="sxs-lookup"><span data-stu-id="f0d27-130">Looks pretty good.</span></span> <span data-ttu-id="f0d27-131">ただしに、ブラウザーのタイトルは"Index"、ページの大きなタイトルは"My MVC Application です"注意してください。</span><span class="sxs-lookup"><span data-stu-id="f0d27-131">However, notice that the Browser's title says "Index" and the big title on the page says "My MVC Application."</span></span> <span data-ttu-id="f0d27-132">これらの名前を変更してみましょう。</span><span class="sxs-lookup"><span data-stu-id="f0d27-132">Let's change those.</span></span>

### <a name="changing-views-and-master-pages"></a><span data-ttu-id="f0d27-133">ビューおよびマスター ページを変更します。</span><span class="sxs-lookup"><span data-stu-id="f0d27-133">Changing Views and Master Pages</span></span>

<span data-ttu-id="f0d27-134">最初に、"My MVC Application です"テキストを変更してみましょう。</span><span class="sxs-lookup"><span data-stu-id="f0d27-134">First, let's change the text "My MVC Application."</span></span> <span data-ttu-id="f0d27-135">そのテキストは、共有され、すべてのページに表示されます。</span><span class="sxs-lookup"><span data-stu-id="f0d27-135">That text is shared and appears on every page.</span></span> <span data-ttu-id="f0d27-136">アプリ内の各ページ上にある場合でも実際に、コード内の 1 か所で表示します。</span><span class="sxs-lookup"><span data-stu-id="f0d27-136">It actually appears in only one place in our code, even though it's on every page in our app.</span></span> <span data-ttu-id="f0d27-137">ソリューション エクスプ ローラーで/Views/Shared フォルダーに移動し、Site.Master ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="f0d27-137">Go to the /Views/Shared folder in the Solution Explorer and open the Site.Master file.</span></span> <span data-ttu-id="f0d27-138">このファイルは、マスター ページと呼ばれ、共有"シェルが"、その他のすべてのページを使用します。</span><span class="sxs-lookup"><span data-stu-id="f0d27-138">This file is called a Master Page and it's the shared "shell" that all our other pages use.</span></span>

<span data-ttu-id="f0d27-139">このファイルには、プレース ホルダー"MainContent"と書かれたテキストに注意してください。</span><span class="sxs-lookup"><span data-stu-id="f0d27-139">Notice some text that says ContentPlaceholder "MainContent" in this file.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample4.aspx)]

<span data-ttu-id="f0d27-140">プレース ホルダーは、場所を作成するすべてのページが表示されます、マスター ページで「ラップされた」です。</span><span class="sxs-lookup"><span data-stu-id="f0d27-140">That placeholder is where all the pages you create will show up, "wrapped" in the master page.</span></span> <span data-ttu-id="f0d27-141">アプリを実行し、複数のページを参照してください。 その後、タイトルを変更してみてください。</span><span class="sxs-lookup"><span data-stu-id="f0d27-141">Try changing the title, then run your app and visit multiple pages.</span></span> <span data-ttu-id="f0d27-142">複数のページで、1 つの変更が表示されることがわかります。</span><span class="sxs-lookup"><span data-stu-id="f0d27-142">You'll notice that your one change appears on multiple pages.</span></span>

[!code-html[Main](getting-started-with-mvc-part3/samples/sample5.html)]

<span data-ttu-id="f0d27-143">「My MVC ムービー アプリケーションです」- H1 はすべてのページは、プライマリ見出しの必要があります。</span><span class="sxs-lookup"><span data-stu-id="f0d27-143">Now every page will have the Primary Heading - that's H1 - of "My MVC Movie Application."</span></span> <span data-ttu-id="f0d27-144">上部にあるすべてのページで共有されている白いテキストを処理するとします。</span><span class="sxs-lookup"><span data-stu-id="f0d27-144">That handles the white text at the top there that's shared across all the pages.</span></span>

<span data-ttu-id="f0d27-145">変更されたタイトルで全体、Site.Master を示します。</span><span class="sxs-lookup"><span data-stu-id="f0d27-145">Here is the Site.Master in its entirety with our changed title:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample6.aspx)]

<span data-ttu-id="f0d27-146">ここで、インデックス ページのタイトルを変更してみましょう。</span><span class="sxs-lookup"><span data-stu-id="f0d27-146">Now, let's change the title of the Index page.</span></span>

<span data-ttu-id="f0d27-147">/HelloWorld/Index.aspx を開きます。</span><span class="sxs-lookup"><span data-stu-id="f0d27-147">Open /HelloWorld/Index.aspx.</span></span> <span data-ttu-id="f0d27-148">これは、2 つの場所を変更します。</span><span class="sxs-lookup"><span data-stu-id="f0d27-148">There's two places to change.</span></span> <span data-ttu-id="f0d27-149">最初に、タイトルに表示される H2 でもあるセカンダリのヘッダーに、ブラウザーのタイトル。</span><span class="sxs-lookup"><span data-stu-id="f0d27-149">First, the Title that appears in the title of the browser, then the secondary header - that's H2 - as well.</span></span> <span data-ttu-id="f0d27-150">アプリのどの部分を変更するビットのコードを表示できるように、若干異なる各描いておきます。</span><span class="sxs-lookup"><span data-stu-id="f0d27-150">I'll make them each slightly different so you can see which bit of code changes which part of the app.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample7.aspx)]

<span data-ttu-id="f0d27-151">アプリを実行し、/Movies を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f0d27-151">Run your app and visit /Movies.</span></span> <span data-ttu-id="f0d27-152">ブラウザーのタイトル、プライマリ見出し、およびセカンダリ見出しが変更されたことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="f0d27-152">Notice that the browser title, the primary heading and the secondary headings have changed.</span></span> <span data-ttu-id="f0d27-153">ビューに小さな変更を使用してアプリケーションで大きな変更を加えるには簡単です。</span><span class="sxs-lookup"><span data-stu-id="f0d27-153">It's easy to make big changes in your app with small changes to your view.</span></span>

<span data-ttu-id="f0d27-154">[![ムービーの一覧 - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image9.png)](getting-started-with-mvc-part3/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="f0d27-154">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image9.png)](getting-started-with-mvc-part3/_static/image8.png)</span></span>

<span data-ttu-id="f0d27-155">(この場合は、"Hello World!"を「データ」のごく一部</span><span class="sxs-lookup"><span data-stu-id="f0d27-155">Our little bit of "data" (in this case the "Hello World!"</span></span> <span data-ttu-id="f0d27-156">メッセージ) は、コード化されたハードでした。</span><span class="sxs-lookup"><span data-stu-id="f0d27-156">message) was hard coded though.</span></span> <span data-ttu-id="f0d27-157">V (ビュー) が発生しましたし、C (コント ローラー) がないの M (モデル) まだがあります。</span><span class="sxs-lookup"><span data-stu-id="f0d27-157">We've got V (Views) and we've got C (Controllers), but no M (Model) yet.</span></span> <span data-ttu-id="f0d27-158">方法を説明しますすぐデータベースを作成し、モデル データを取得します。</span><span class="sxs-lookup"><span data-stu-id="f0d27-158">We'll shortly walk through how create a database and retrieve model data from it.</span></span>

## <a name="passing-a-viewmodel"></a><span data-ttu-id="f0d27-159">ViewModel を渡す</span><span class="sxs-lookup"><span data-stu-id="f0d27-159">Passing a ViewModel</span></span>

<span data-ttu-id="f0d27-160">データベースに移動してモデルについて説明する前に、まずについて説明しましょう"ViewModels"</span><span class="sxs-lookup"><span data-stu-id="f0d27-160">Before we go to a database and talk about Models, though, let's first talk about "ViewModels."</span></span> <span data-ttu-id="f0d27-161">これらは、クライアントに、HTML 応答を表示するために必要とビュー テンプレートを表すオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="f0d27-161">These are objects that represent what a View template requires to render an HTML response back to a client.</span></span> <span data-ttu-id="f0d27-162">コント ローラー クラスによって、ビュー テンプレートに渡され、テンプレートの表示が必要です - データと以上のみを含める必要があります、通常は作成されます。</span><span class="sxs-lookup"><span data-stu-id="f0d27-162">They are typically created and passed by a Controller class to a View template, and should only contain the data that the View template requires - and no more.</span></span>

<span data-ttu-id="f0d27-163">以前、HelloWorld サンプルを使用して、Welcome() アクション メソッドが名前と numTimes パラメーターと、ブラウザーに出力します。</span><span class="sxs-lookup"><span data-stu-id="f0d27-163">Previously with our HelloWorld sample, our Welcome() action method took a name and a numTimes parameter and output it to the browser.</span></span> <span data-ttu-id="f0d27-164">この応答を直接表示するために引き続きコント ローラーがあるのではなく、小さなクラスにそのデータを保持し、バックアップを使用して、HTML 応答をレンダリングするビュー テンプレートを経由で渡すことを加えてみましょう代わりに。</span><span class="sxs-lookup"><span data-stu-id="f0d27-164">Rather than have the Controller continue to render this response directly,let's instead make a small class to hold that data and then pass it over to a View template to render back the HTML response using it.</span></span> <span data-ttu-id="f0d27-165">こうすると、コント ローラーは 1 つと、ビュー テンプレートに関係別 – クリーン""関心の分離、アプリケーション内で維持するためにできるようになりました。</span><span class="sxs-lookup"><span data-stu-id="f0d27-165">This way the Controller is concerned with one thing and the View template another – enabling us to maintain clean "separation of concerns" within our application.</span></span>

<span data-ttu-id="f0d27-166">HelloWorldController.cs ファイルに戻ると新しい"WelcomeViewModel"クラスを追加し、コント ローラー内の開始方法を変更します。</span><span class="sxs-lookup"><span data-stu-id="f0d27-166">Return to the HelloWorldController.cs file and add a new "WelcomeViewModel" class and change the Welcome method inside your controller.</span></span> <span data-ttu-id="f0d27-167">同じファイル内の新しいクラスを使用して完全な HelloWorldController.cs を次に示します。</span><span class="sxs-lookup"><span data-stu-id="f0d27-167">Here is the complete HelloWorldController.cs with the new class in the same file.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample8.cs)]

<span data-ttu-id="f0d27-168">場合でも、複数の行には、開始の方法が本当に 2 つだけのコード ステートメントにします。</span><span class="sxs-lookup"><span data-stu-id="f0d27-168">Even though it's on multiple lines, our Welcome method is really only two code statements.</span></span> <span data-ttu-id="f0d27-169">最初のステートメントは、結果のオブジェクトをビューに ViewModel オブジェクト、および 2 番目のパスに 2 つのパラメーターをパッケージ化します。</span><span class="sxs-lookup"><span data-stu-id="f0d27-169">The first statement packages up our two parameters into a ViewModel object, and the second passes the resulting object onto the View.</span></span>

<span data-ttu-id="f0d27-170">[ようこそ] ビュー テンプレート必要があります。</span><span class="sxs-lookup"><span data-stu-id="f0d27-170">Now we need a Welcome View template!</span></span> <span data-ttu-id="f0d27-171">[ようこそ] のメソッドを右クリックし、ビューの追加を選択します。</span><span class="sxs-lookup"><span data-stu-id="f0d27-171">Right click in the Welcome method and select Add View.</span></span> <span data-ttu-id="f0d27-172">今回が「厳密に型指定されたビューの作成」のチェックされ、ドロップダウン リストから、WelcomeViewModel クラスを選択します。</span><span class="sxs-lookup"><span data-stu-id="f0d27-172">This time, we'll check "Create a strongly-typed view" and select our WelcomeViewModel class from the drop down list.</span></span> <span data-ttu-id="f0d27-173">この新しいビューが認識するだけ WelcomeViewModels とオブジェクトの他の種類がありません。</span><span class="sxs-lookup"><span data-stu-id="f0d27-173">This new view will only know about WelcomeViewModels and no other types of objects.</span></span>

> <span data-ttu-id="f0d27-174">*注: ドロップダウン リストに表示するため、WelcomeViewModel を追加した後に 1 回コンパイルする必要があります。*</span><span class="sxs-lookup"><span data-stu-id="f0d27-174">*NOTE: You'll need to have compiled once after adding your WelcomeViewModel for to show up in the drop down list.*</span></span>


<span data-ttu-id="f0d27-175">次のように確認する必要があります、ビューの追加 ダイアログに示します。</span><span class="sxs-lookup"><span data-stu-id="f0d27-175">Here's what your Add View dialog should look like.</span></span> <span data-ttu-id="f0d27-176">[追加] ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="f0d27-176">Click the Add button.</span></span> ![ビューの丸を追加します。](getting-started-with-mvc-part3/_static/image10.png)

<span data-ttu-id="f0d27-178">下には、このコードを追加、 &lt;h2&gt;新しい Welcome.aspx でします。</span><span class="sxs-lookup"><span data-stu-id="f0d27-178">Add this code under the &lt;h2&gt; in your new Welcome.aspx.</span></span> <span data-ttu-id="f0d27-179">ループの作成をユーザーの質問が何回にあいさつします!</span><span class="sxs-lookup"><span data-stu-id="f0d27-179">We'll make a loop and say Hello as many times as the user says we should!</span></span>

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample9.aspx)]

<span data-ttu-id="f0d27-180">また、ためこの先ほど言ったとおり、WelcomeViewModel に関するビューを入力している間に注意してください (既婚に注意してください)。 以下のスクリーン ショットに見られるよう、モデル オブジェクトを参照するたびに、便利な Intellisense を取得します。</span><span class="sxs-lookup"><span data-stu-id="f0d27-180">Also, notice while you're typing that because we told this View about the WelcomeViewModel (they are married, remember?) that we get helpful Intellisense each time we reference our Model object as seen in the screenshot below:</span></span>

<span data-ttu-id="f0d27-181">[![NumTime ソース コード](getting-started-with-mvc-part3/_static/image12.png)](getting-started-with-mvc-part3/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="f0d27-181">[![NumTime Source Code](getting-started-with-mvc-part3/_static/image12.png)](getting-started-with-mvc-part3/_static/image11.png)</span></span>

<span data-ttu-id="f0d27-182">アプリケーションを実行しを参照してください`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`もう一度です。</span><span class="sxs-lookup"><span data-stu-id="f0d27-182">Run your application and visit `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` again.</span></span> <span data-ttu-id="f0d27-183">URL からデータを移動しています、これで、コント ローラーに自動的に渡されるという、コント ローラーは ViewModel にデータをパッケージ化し、そのオブジェクトのビューに渡します。</span><span class="sxs-lookup"><span data-stu-id="f0d27-183">Now we're taking data from the URL, it's passed into our Controller automatically, our Controller packages up the data into a ViewModel and passes that object onto our View.</span></span> <span data-ttu-id="f0d27-184">ビューよりも、ユーザーに HTML としてデータが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f0d27-184">The View than displays the data as HTML to the user.</span></span>

<span data-ttu-id="f0d27-185">[![Windows Internet Explorer の開始](getting-started-with-mvc-part3/_static/image14.png)](getting-started-with-mvc-part3/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="f0d27-185">[![Welcome - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image14.png)](getting-started-with-mvc-part3/_static/image13.png)</span></span>

<span data-ttu-id="f0d27-186">ある種のモデルでは、"M"がデータベースの種類ではありませんでした。</span><span class="sxs-lookup"><span data-stu-id="f0d27-186">Well, that was a kind of an "M" for Model, but not the database kind.</span></span> <span data-ttu-id="f0d27-187">学習した確認し、ムービーのデータベースを作成してみましょう。</span><span class="sxs-lookup"><span data-stu-id="f0d27-187">Let's take what we've learned and create a database of Movies.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f0d27-188">[前へ](getting-started-with-mvc-part2.md)
> [次へ](getting-started-with-mvc-part4.md)</span><span class="sxs-lookup"><span data-stu-id="f0d27-188">[Previous](getting-started-with-mvc-part2.md)
[Next](getting-started-with-mvc-part4.md)</span></span>
