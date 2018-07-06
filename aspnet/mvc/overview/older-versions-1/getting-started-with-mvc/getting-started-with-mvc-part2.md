---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
title: コント ローラーの追加 |Microsoft Docs
author: shanselman
description: このチュートリアルでは、Visual Studio 2013 を使用して、ここで使用可能な場合は、更新されたバージョン。 新しいチュートリアルでは、t に多くの機能強化を提供する ASP.NET MVC 5 を使用しています.
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: ff03dcc0-da97-458d-838f-0823e7482642
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
msc.type: authoredcontent
ms.openlocfilehash: afe1182bca0fe58e1df162c5027df35992eeef38
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809911"
---
<a name="adding-a-controller"></a><span data-ttu-id="5ae8f-104">コントローラーを追加する</span><span class="sxs-lookup"><span data-stu-id="5ae8f-104">Adding a Controller</span></span>
====================
<span data-ttu-id="5ae8f-105">によって[Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="5ae8f-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> > [!NOTE]
> > <span data-ttu-id="5ae8f-106">このチュートリアルでは、使用可能な場合は、更新されたバージョン[ここ](../../getting-started/introduction/getting-started.md)を使用して[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)します。</span><span class="sxs-lookup"><span data-stu-id="5ae8f-106">An updated version if this tutorial is available [here](../../getting-started/introduction/getting-started.md) using [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).</span></span> <span data-ttu-id="5ae8f-107">新しいチュートリアルでは、このチュートリアルで多くの機能強化を提供する ASP.NET MVC 5 を使用します。</span><span class="sxs-lookup"><span data-stu-id="5ae8f-107">The new tutorial uses ASP.NET MVC 5, which provides many improvements over this tutorial.</span></span>
> 
> 
> <span data-ttu-id="5ae8f-108">これは、ASP.NET MVC の基本を紹介する初心者向けチュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="5ae8f-108">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="5ae8f-109">読み取りと書き込みをデータベースから単純な web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="5ae8f-109">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="5ae8f-110">参照してください、 [ASP.NET MVC ラーニング センター](../../../index.md)チュートリアルとサンプルは、その他の ASP.NET MVC を検索します。</span><span class="sxs-lookup"><span data-stu-id="5ae8f-110">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="5ae8f-111">MVC は、モデル、ビュー、コント ローラーの略です。</span><span class="sxs-lookup"><span data-stu-id="5ae8f-111">MVC stands for Model, View, Controller.</span></span> <span data-ttu-id="5ae8f-112">MVC は、各パーツがある他とは異なる責任になるようにアプリケーションを開発するためのパターンです。</span><span class="sxs-lookup"><span data-stu-id="5ae8f-112">MVC is a pattern for developing applications such that each part has a responsibility that is different from another.</span></span>

- <span data-ttu-id="5ae8f-113">アプリケーションのデータ モデル:</span><span class="sxs-lookup"><span data-stu-id="5ae8f-113">Model: The data of your application</span></span>
- <span data-ttu-id="5ae8f-114">ビュー: 動的 HTML 応答を生成する、アプリケーションが使用されるテンプレート ファイル。</span><span class="sxs-lookup"><span data-stu-id="5ae8f-114">Views: The template files your application will use to dynamically generate HTML responses.</span></span>
- <span data-ttu-id="5ae8f-115">コント ローラー: アプリケーションへの受信 URL 要求を処理するクラスはモデル データを取得し、クライアントに応答をレンダリングするビュー テンプレートを指定</span><span class="sxs-lookup"><span data-stu-id="5ae8f-115">Controllers: Classes that handle incoming URL requests to the application, retrieve model data, and then specify view templates that render a response back to the client</span></span>

<span data-ttu-id="5ae8f-116">このチュートリアルでは、これらすべての概念をカバーするがされ、アプリケーションの構築に使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="5ae8f-116">We'll be covering all these concepts in this tutorial and show you how to use them to build an application.</span></span>

<span data-ttu-id="5ae8f-117">ソリューション エクスプ ローラーで controllers フォルダーを右クリックし、コント ローラーの追加を選択して、新しいコント ローラーを作成しましょう。</span><span class="sxs-lookup"><span data-stu-id="5ae8f-117">Let's create a new controller by right-clicking the controllers folder in the solution Explorer and selecting Add Controller.</span></span>

<span data-ttu-id="5ae8f-118">[![AddControllerRightClick](getting-started-with-mvc-part2/_static/image2.png)](getting-started-with-mvc-part2/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5ae8f-118">[![AddControllerRightClick](getting-started-with-mvc-part2/_static/image2.png)](getting-started-with-mvc-part2/_static/image1.png)</span></span>

<span data-ttu-id="5ae8f-119">新しいコント ローラー"HelloWorldController"の名前し、[追加] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5ae8f-119">Name your new controller "HelloWorldController" and click Add.</span></span>

<span data-ttu-id="5ae8f-120">[![[追加] ダイアログのコント ローラー](getting-started-with-mvc-part2/_static/image4.png)](getting-started-with-mvc-part2/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="5ae8f-120">[![Add Controller Dialog](getting-started-with-mvc-part2/_static/image4.png)](getting-started-with-mvc-part2/_static/image3.png)</span></span>

<span data-ttu-id="5ae8f-121">HelloWorldController.cs と呼ばれるの新しいファイルが作成されでそのファイルが開いている右側のソリューション エクスプ ローラーで、 **IDE**します。</span><span class="sxs-lookup"><span data-stu-id="5ae8f-121">Notice in the Solution Explorer on the right that a new file has been created for you called HelloWorldController.cs and that file is now opened in the **IDE**.</span></span>

<span data-ttu-id="5ae8f-122">[![HelloWorldControllerCode](getting-started-with-mvc-part2/_static/image6.png)](getting-started-with-mvc-part2/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="5ae8f-122">[![HelloWorldControllerCode](getting-started-with-mvc-part2/_static/image6.png)](getting-started-with-mvc-part2/_static/image5.png)</span></span>

<span data-ttu-id="5ae8f-123">新しいパブリック クラス HelloWorldController 内で次のように 2 つの新しいメソッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="5ae8f-123">Create two new methods that look like this inside of your new public class HelloWorldController.</span></span> <span data-ttu-id="5ae8f-124">例として、コント ローラーから直接 HTML の文字列を返しますします。</span><span class="sxs-lookup"><span data-stu-id="5ae8f-124">We'll return a string of HTML directly from our controller as an example.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample1.cs)]

<span data-ttu-id="5ae8f-125">HelloWorldController の名前は、コント ローラーと、新しいメソッドには、インデックスは呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="5ae8f-125">Your Controller is named HelloWorldController and your new Method is called Index.</span></span> <span data-ttu-id="5ae8f-126">アプリケーションを再実行以前と同様 (再生ボタンをクリックしてまたはこれを行うには F5 キーを押します)。</span><span class="sxs-lookup"><span data-stu-id="5ae8f-126">Run your application again, just as before (click the play button or press F5 to do this).</span></span> <span data-ttu-id="5ae8f-127">お使いのブラウザーが開始されると、アドレス バーのパスを変更します。 `http://localhost:xx/HelloWorld` xx は任意の数、コンピューターが選択されます。</span><span class="sxs-lookup"><span data-stu-id="5ae8f-127">Once your browser has started up, change the path in the address bar to `http://localhost:xx/HelloWorld` where xx is whatever number your computer has chosen.</span></span> <span data-ttu-id="5ae8f-128">今すぐお使いのブラウザーは、次のスクリーン ショットのようになります。</span><span class="sxs-lookup"><span data-stu-id="5ae8f-128">Now your browser should look like the screenshot below.</span></span> <span data-ttu-id="5ae8f-129">上記の方法で返された「コンテンツ」と呼ばれるメソッドに渡される文字列</span><span class="sxs-lookup"><span data-stu-id="5ae8f-129">In our method above we returned a string passed into a method called "Content."</span></span> <span data-ttu-id="5ae8f-130">先ほど言ったとおり、システムは、いくつかの HTML を返すだけおり、そうしました!</span><span class="sxs-lookup"><span data-stu-id="5ae8f-130">We told the system just returns some HTML, and it did!</span></span>

<span data-ttu-id="5ae8f-131">ASP.NET MVC では、着信 URL に応じて別のコント ローラー クラス (およびそれらに含まれる別のアクション メソッド) を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="5ae8f-131">ASP.NET MVC invokes different Controller classes (and different Action methods within them) depending on the incoming URL.</span></span> <span data-ttu-id="5ae8f-132">ASP.NET MVC で使用される既定のマッピングのロジックでは、このような形式を使用して、どのようなコードの実行を制御します。</span><span class="sxs-lookup"><span data-stu-id="5ae8f-132">The default mapping logic used by ASP.NET MVC uses a format like this to control what code is run:</span></span>

<span data-ttu-id="5ae8f-133">/[Controller]/[ActionName]/[パラメーター]</span><span class="sxs-lookup"><span data-stu-id="5ae8f-133">/[Controller]/[ActionName]/[Parameters]</span></span>

<span data-ttu-id="5ae8f-134">URL の最初の部分を実行するコント ローラー クラスを決定します。</span><span class="sxs-lookup"><span data-stu-id="5ae8f-134">The first part of the URL determines the Controller class to execute.</span></span> <span data-ttu-id="5ae8f-135">したがって、HelloWorldController クラス/HelloWorld にマップします。</span><span class="sxs-lookup"><span data-stu-id="5ae8f-135">So /HelloWorld maps to the HelloWorldController class.</span></span> <span data-ttu-id="5ae8f-136">URL の 2 番目の部分は、実行するには、クラスのアクション メソッドを決定します。</span><span class="sxs-lookup"><span data-stu-id="5ae8f-136">The second part of the URL determines the Action method on the class to execute.</span></span> <span data-ttu-id="5ae8f-137">/HelloWorld/Index を実行する HelloWorldcontroller クラスの Index() メソッド。</span><span class="sxs-lookup"><span data-stu-id="5ae8f-137">So /HelloWorld/Index would cause the Index() method of the HelloWorldcontroller class to execute.</span></span> <span data-ttu-id="5ae8f-138">上記/HelloWorld とインデックスが暗黙的に指定されたメソッドを参照してくださいにのみ必要があることを確認します。</span><span class="sxs-lookup"><span data-stu-id="5ae8f-138">Notice that we only had to visit /HelloWorld above and the method Index was implied.</span></span> <span data-ttu-id="5ae8f-139">これは、"Index"という名前のメソッドが明示的に指定されていない場合、コント ローラーで呼び出される既定の方法であるためです。</span><span class="sxs-lookup"><span data-stu-id="5ae8f-139">This is because a method named "Index" is the default method that will be called on a controller if one is not explicitly specified.</span></span>

<span data-ttu-id="5ae8f-140">[![これは、既定のアクション](getting-started-with-mvc-part2/_static/image8.png)](getting-started-with-mvc-part2/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="5ae8f-140">[![This is my default action](getting-started-with-mvc-part2/_static/image8.png)](getting-started-with-mvc-part2/_static/image7.png)</span></span>

<span data-ttu-id="5ae8f-141">ここで、次を参照してください`http://localhost:xx/HelloWorld/Welcome.`へようこそ の方法が実行され、HTML 文字列を返すようになりました。</span><span class="sxs-lookup"><span data-stu-id="5ae8f-141">Now, let's visit `http://localhost:xx/HelloWorld/Welcome.` Now our Welcome Method has executed and returned its HTML string.</span></span>

<span data-ttu-id="5ae8f-142">ここでも、/[Controller]/[ActionName]/[パラメーター] コント ローラーが HelloWorld とウェルカム メソッドをここでは、します。</span><span class="sxs-lookup"><span data-stu-id="5ae8f-142">Again, /[Controller]/[ActionName]/[Parameters] so Controller is HelloWorld and Welcome is the Method in this case.</span></span> <span data-ttu-id="5ae8f-143">パラメーターをまだ完了していないことはできます。</span><span class="sxs-lookup"><span data-stu-id="5ae8f-143">We haven't done Parameters yet.</span></span>

<span data-ttu-id="5ae8f-144">[![これは、[ようこそ] のアクション メソッドです。](getting-started-with-mvc-part2/_static/image10.png)](getting-started-with-mvc-part2/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="5ae8f-144">[![This is the Welcome action method](getting-started-with-mvc-part2/_static/image10.png)](getting-started-with-mvc-part2/_static/image9.png)</span></span>

<span data-ttu-id="5ae8f-145">みましょう URL からの一部の情報を渡すなどのように、コント ローラーをように、サンプルを少し変更:/helloworld/welcome でしょうか。 名前 = Scott&amp;numtimes 4 を = です。</span><span class="sxs-lookup"><span data-stu-id="5ae8f-145">Let's modify our sample slightly so that we can pass some information in from the URL to our controller, for example like this: /HelloWorld/Welcome?name=Scott&amp;numtimes=4.</span></span> <span data-ttu-id="5ae8f-146">2 つのパラメーターと以下のような更新プログラムを含めるへようこそ 方法を変更します。</span><span class="sxs-lookup"><span data-stu-id="5ae8f-146">Change your Welcome method to include two parameters and update it like below.</span></span> <span data-ttu-id="5ae8f-147">が渡されない場合 1 をパラメーター numTimes が既定を示す c# のオプション パラメーター機能を使用したことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="5ae8f-147">Note that we've used the C# optional parameter feature to indicate that the parameter numTimes should default to 1 if it's not passed in.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample2.cs)]

<span data-ttu-id="5ae8f-148">アプリケーションを実行しを参照してください`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`好きなように、名前と numtimes の値を変更します。</span><span class="sxs-lookup"><span data-stu-id="5ae8f-148">Run your application and visit `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` changing the value of name and numtimes as you like.</span></span> <span data-ttu-id="5ae8f-149">システムは、メソッドのパラメーターに、アドレス バーで、クエリ文字列から名前付きパラメーターを自動的にマップされます。</span><span class="sxs-lookup"><span data-stu-id="5ae8f-149">The system automatically mapped the named parameters from your query string in the address bar to parameters in your method.</span></span>

<span data-ttu-id="5ae8f-150">これら両方の例では、コント ローラーは、すべての作業を行ってされておりがされた HTML を直接返します。</span><span class="sxs-lookup"><span data-stu-id="5ae8f-150">In both these examples the controller has been doing all the work, and has been returning HTML directly.</span></span> <span data-ttu-id="5ae8f-151">通常、コント ローラーしたくないコードに非常に手間がかかるで終わるために、直接 HTML を返します。</span><span class="sxs-lookup"><span data-stu-id="5ae8f-151">Ordinarily we don't want our Controllers returning HTML directly - since that ends up being very cumbersome to code.</span></span> <span data-ttu-id="5ae8f-152">代わりに、HTML 応答を生成するために使用します別のビュー テンプレート ファイルを通常。</span><span class="sxs-lookup"><span data-stu-id="5ae8f-152">Instead we'll typically use a separate View template file to help generate the HTML response.</span></span> <span data-ttu-id="5ae8f-153">そのため方法を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="5ae8f-153">Let's look at how we can do this.</span></span> <span data-ttu-id="5ae8f-154">お使いのブラウザーを閉じて、IDE に戻ります。</span><span class="sxs-lookup"><span data-stu-id="5ae8f-154">Close your browser and return to the IDE.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5ae8f-155">[前へ](getting-started-with-mvc-part1.md)
> [次へ](getting-started-with-mvc-part3.md)</span><span class="sxs-lookup"><span data-stu-id="5ae8f-155">[Previous](getting-started-with-mvc-part1.md)
[Next](getting-started-with-mvc-part3.md)</span></span>
