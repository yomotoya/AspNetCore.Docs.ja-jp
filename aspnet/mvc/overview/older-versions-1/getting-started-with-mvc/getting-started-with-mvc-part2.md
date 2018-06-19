---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
title: コント ローラーの追加 |Microsoft ドキュメント
author: shanselman
description: このチュートリアルは Visual Studio 2013 を使用して、ここで使用可能な場合は、更新されたバージョンです。 新しいチュートリアルを使用して ASP.NET MVC 5 は、t に対して多くの機能強化を提供しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: ff03dcc0-da97-458d-838f-0823e7482642
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
msc.type: authoredcontent
ms.openlocfilehash: c6ecd1ffdd53a629d0079d57b85c7f6db2f316ce
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869024"
---
<a name="adding-a-controller"></a><span data-ttu-id="f3378-104">コントローラーを追加する</span><span class="sxs-lookup"><span data-stu-id="f3378-104">Adding a Controller</span></span>
====================
<span data-ttu-id="f3378-105">によって[Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="f3378-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> > [!NOTE]
> > <span data-ttu-id="f3378-106">このチュートリアルでは使用可能な場合、更新されたバージョン[ここ](../../getting-started/introduction/getting-started.md)を使用して[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)です。</span><span class="sxs-lookup"><span data-stu-id="f3378-106">An updated version if this tutorial is available [here](../../getting-started/introduction/getting-started.md) using [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).</span></span> <span data-ttu-id="f3378-107">新しいチュートリアルでは、ASP.NET MVC 5 は、このチュートリアルを超える多くの機能強化を提供を使用します。</span><span class="sxs-lookup"><span data-stu-id="f3378-107">The new tutorial uses ASP.NET MVC 5, which provides many improvements over this tutorial.</span></span>
> 
> 
> <span data-ttu-id="f3378-108">これは、ASP.NET MVC の基本について説明する初心者向けチュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="f3378-108">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="f3378-109">データベースから読み取り/書き込みを単純な web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="f3378-109">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="f3378-110">参照してください、 [ASP.NET MVC ラーニング センター](../../../index.md)チュートリアルおよびサンプルは、その他の ASP.NET MVC を検索します。</span><span class="sxs-lookup"><span data-stu-id="f3378-110">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="f3378-111">MVC は、モデル、ビュー、コント ローラーを表します。</span><span class="sxs-lookup"><span data-stu-id="f3378-111">MVC stands for Model, View, Controller.</span></span> <span data-ttu-id="f3378-112">MVC は、パターンを各部分とは別に異なる責任を持つようにアプリケーションを開発するためです。</span><span class="sxs-lookup"><span data-stu-id="f3378-112">MVC is a pattern for developing applications such that each part has a responsibility that is different from another.</span></span>

- <span data-ttu-id="f3378-113">アプリケーションのデータのモデルです。</span><span class="sxs-lookup"><span data-stu-id="f3378-113">Model: The data of your application</span></span>
- <span data-ttu-id="f3378-114">ビュー: テンプレート ファイル、アプリケーションが HTML 応答を動的に生成に使用します。</span><span class="sxs-lookup"><span data-stu-id="f3378-114">Views: The template files your application will use to dynamically generate HTML responses.</span></span>
- <span data-ttu-id="f3378-115">コント ローラー: アプリケーションへの着信 URL 要求を処理するクラスはモデル データを取得し、クライアントへの応答をレンダリングするテンプレートの表示を指定</span><span class="sxs-lookup"><span data-stu-id="f3378-115">Controllers: Classes that handle incoming URL requests to the application, retrieve model data, and then specify view templates that render a response back to the client</span></span>

<span data-ttu-id="f3378-116">このチュートリアルでは、これらすべての概念をカバーして行いますを使用してアプリケーションを構築する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="f3378-116">We'll be covering all these concepts in this tutorial and show you how to use them to build an application.</span></span>

<span data-ttu-id="f3378-117">ソリューション エクスプ ローラーで controllers フォルダーを右クリックし、コント ローラーの追加を選択して、新しいコント ローラーを作成してみましょう。</span><span class="sxs-lookup"><span data-stu-id="f3378-117">Let's create a new controller by right-clicking the controllers folder in the solution Explorer and selecting Add Controller.</span></span>

<span data-ttu-id="f3378-118">[![AddControllerRightClick](getting-started-with-mvc-part2/_static/image2.png)](getting-started-with-mvc-part2/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f3378-118">[![AddControllerRightClick](getting-started-with-mvc-part2/_static/image2.png)](getting-started-with-mvc-part2/_static/image1.png)</span></span>

<span data-ttu-id="f3378-119">新しいコント ローラー"HelloWorldController"の名前し、[追加] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f3378-119">Name your new controller "HelloWorldController" and click Add.</span></span>

<span data-ttu-id="f3378-120">[![[追加] ダイアログのコント ローラー](getting-started-with-mvc-part2/_static/image4.png)](getting-started-with-mvc-part2/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="f3378-120">[![Add Controller Dialog](getting-started-with-mvc-part2/_static/image4.png)](getting-started-with-mvc-part2/_static/image3.png)</span></span>

<span data-ttu-id="f3378-121">ソリューション エクスプ ローラーで、右側の HelloWorldController.cs と呼ばれる新しいファイルが作成されでそのファイルを開くようになりましたことに注意してください、 **IDE**です。</span><span class="sxs-lookup"><span data-stu-id="f3378-121">Notice in the Solution Explorer on the right that a new file has been created for you called HelloWorldController.cs and that file is now opened in the **IDE**.</span></span>

<span data-ttu-id="f3378-122">[![HelloWorldControllerCode](getting-started-with-mvc-part2/_static/image6.png)](getting-started-with-mvc-part2/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="f3378-122">[![HelloWorldControllerCode](getting-started-with-mvc-part2/_static/image6.png)](getting-started-with-mvc-part2/_static/image5.png)</span></span>

<span data-ttu-id="f3378-123">新しいパブリック クラス HelloWorldController の内部で次のように 2 つの新しいメソッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="f3378-123">Create two new methods that look like this inside of your new public class HelloWorldController.</span></span> <span data-ttu-id="f3378-124">例として、コント ローラーから直接 HTML の文字列を返します。</span><span class="sxs-lookup"><span data-stu-id="f3378-124">We'll return a string of HTML directly from our controller as an example.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample1.cs)]

<span data-ttu-id="f3378-125">HelloWorldController の名前は、コント ローラーと、新しいメソッドには、インデックスは呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="f3378-125">Your Controller is named HelloWorldController and your new Method is called Index.</span></span> <span data-ttu-id="f3378-126">アプリケーションを実行して、もう一度同じように (再生ボタンをクリックしてまたはこれを行うには F5 キーを押します)。</span><span class="sxs-lookup"><span data-stu-id="f3378-126">Run your application again, just as before (click the play button or press F5 to do this).</span></span> <span data-ttu-id="f3378-127">お使いのブラウザーが起動した後は、アドレス バーのパスを変更`http://localhost:xx/HelloWorld`xx は任意の番号、コンピューターが選択されます。</span><span class="sxs-lookup"><span data-stu-id="f3378-127">Once your browser has started up, change the path in the address bar to `http://localhost:xx/HelloWorld` where xx is whatever number your computer has chosen.</span></span> <span data-ttu-id="f3378-128">今すぐお使いのブラウザーは、次のスクリーン ショットのようになります。</span><span class="sxs-lookup"><span data-stu-id="f3378-128">Now your browser should look like the screenshot below.</span></span> <span data-ttu-id="f3378-129">上記このメソッドでは返される「コンテンツ」と呼ばれるメソッドに渡される文字列</span><span class="sxs-lookup"><span data-stu-id="f3378-129">In our method above we returned a string passed into a method called "Content."</span></span> <span data-ttu-id="f3378-130">システムでは、いくつかの HTML だけを返し、同じと言いました。</span><span class="sxs-lookup"><span data-stu-id="f3378-130">We told the system just returns some HTML, and it did!</span></span>

<span data-ttu-id="f3378-131">ASP.NET MVC では、着信 URL に応じて別のコント ローラー クラス (およびそれらに含まれる別のアクション メソッド) を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="f3378-131">ASP.NET MVC invokes different Controller classes (and different Action methods within them) depending on the incoming URL.</span></span> <span data-ttu-id="f3378-132">ASP.NET MVC で使用される既定のマッピングのロジックでは、次のような形式を使用して、どのようなコードの実行を制御します。</span><span class="sxs-lookup"><span data-stu-id="f3378-132">The default mapping logic used by ASP.NET MVC uses a format like this to control what code is run:</span></span>

<span data-ttu-id="f3378-133">/[Controller]/[ActionName]/[Parameters]</span><span class="sxs-lookup"><span data-stu-id="f3378-133">/[Controller]/[ActionName]/[Parameters]</span></span>

<span data-ttu-id="f3378-134">URL の最初の部分を実行するコント ローラー クラスを決定します。</span><span class="sxs-lookup"><span data-stu-id="f3378-134">The first part of the URL determines the Controller class to execute.</span></span> <span data-ttu-id="f3378-135">したがって/HelloWorld は HelloWorldController クラスにマップされます。</span><span class="sxs-lookup"><span data-stu-id="f3378-135">So /HelloWorld maps to the HelloWorldController class.</span></span> <span data-ttu-id="f3378-136">URL の 2 番目の部分では、実行するには、クラスにアクション メソッドを判断します。</span><span class="sxs-lookup"><span data-stu-id="f3378-136">The second part of the URL determines the Action method on the class to execute.</span></span> <span data-ttu-id="f3378-137">したがって/HelloWorld/Index とで実行する HelloWorldcontroller クラスの Index() メソッドになります。</span><span class="sxs-lookup"><span data-stu-id="f3378-137">So /HelloWorld/Index would cause the Index() method of the HelloWorldcontroller class to execute.</span></span> <span data-ttu-id="f3378-138">のみしなければならないこと/HelloWorld 上記とインデックスは暗黙的であったメソッドを参照してくださいに注意してください。</span><span class="sxs-lookup"><span data-stu-id="f3378-138">Notice that we only had to visit /HelloWorld above and the method Index was implied.</span></span> <span data-ttu-id="f3378-139">これは、"Index"という名前のメソッドがいずれかが明示的に指定されていない場合に、コント ローラーで呼び出される既定の方法であるためです。</span><span class="sxs-lookup"><span data-stu-id="f3378-139">This is because a method named "Index" is the default method that will be called on a controller if one is not explicitly specified.</span></span>

<span data-ttu-id="f3378-140">[![これは、既定のアクション](getting-started-with-mvc-part2/_static/image8.png)](getting-started-with-mvc-part2/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="f3378-140">[![This is my default action](getting-started-with-mvc-part2/_static/image8.png)](getting-started-with-mvc-part2/_static/image7.png)</span></span>

<span data-ttu-id="f3378-141">ここで、次を参照してください`http://localhost:xx/HelloWorld/Welcome.`これで、このようこそメソッドが実行され、HTML 文字列が返されました。</span><span class="sxs-lookup"><span data-stu-id="f3378-141">Now, let's visit `http://localhost:xx/HelloWorld/Welcome.` Now our Welcome Method has executed and returned its HTML string.</span></span>

<span data-ttu-id="f3378-142">もう一度、/[コント ローラー]/[ActionName]/[パラメーター] コント ローラーが HelloWorld、[ようこそ] が、メソッドをここではします。</span><span class="sxs-lookup"><span data-stu-id="f3378-142">Again, /[Controller]/[ActionName]/[Parameters] so Controller is HelloWorld and Welcome is the Method in this case.</span></span> <span data-ttu-id="f3378-143">パラメーターはまだ完成はしていません。</span><span class="sxs-lookup"><span data-stu-id="f3378-143">We haven't done Parameters yet.</span></span>

<span data-ttu-id="f3378-144">[![これは、ようこそアクション メソッドです。](getting-started-with-mvc-part2/_static/image10.png)](getting-started-with-mvc-part2/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="f3378-144">[![This is the Welcome action method](getting-started-with-mvc-part2/_static/image10.png)](getting-started-with-mvc-part2/_static/image9.png)</span></span>

<span data-ttu-id="f3378-145">みましょうので、たとえば次のように、コント ローラーに、URL からの一部の情報を渡すお、この例を少し変更: HelloWorld/ウェルカムしますか? 名 = Scott&amp;numtimes 4 を = です。</span><span class="sxs-lookup"><span data-stu-id="f3378-145">Let's modify our sample slightly so that we can pass some information in from the URL to our controller, for example like this: /HelloWorld/Welcome?name=Scott&amp;numtimes=4.</span></span> <span data-ttu-id="f3378-146">など、2 つのパラメーター以下のような更新プログラムへようこそ、メソッドを変更します。</span><span class="sxs-lookup"><span data-stu-id="f3378-146">Change your Welcome method to include two parameters and update it like below.</span></span> <span data-ttu-id="f3378-147">が渡されない場合パラメーター numTimes が 1 を既定ことを示すために、C# で省略可能なパラメーター機能を使い切ってしまったことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="f3378-147">Note that we've used the C# optional parameter feature to indicate that the parameter numTimes should default to 1 if it's not passed in.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample2.cs)]

<span data-ttu-id="f3378-148">アプリケーションを実行しを参照してください`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`好きなように、名前と numtimes の値を変更します。</span><span class="sxs-lookup"><span data-stu-id="f3378-148">Run your application and visit `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` changing the value of name and numtimes as you like.</span></span> <span data-ttu-id="f3378-149">システムは、メソッドのパラメーターに、アドレス バーに、クエリ文字列から名前付きパラメーターを自動的にマップされます。</span><span class="sxs-lookup"><span data-stu-id="f3378-149">The system automatically mapped the named parameters from your query string in the address bar to parameters in your method.</span></span>

<span data-ttu-id="f3378-150">これら両方の例では、コント ローラーは、すべての作業を行ってされておりが直接に HTML を返すことされました。</span><span class="sxs-lookup"><span data-stu-id="f3378-150">In both these examples the controller has been doing all the work, and has been returning HTML directly.</span></span> <span data-ttu-id="f3378-151">通常、コント ローラーしたくありませんを最終的にコードに非常に手間がかかるので、HTML を直接返します。</span><span class="sxs-lookup"><span data-stu-id="f3378-151">Ordinarily we don't want our Controllers returning HTML directly - since that ends up being very cumbersome to code.</span></span> <span data-ttu-id="f3378-152">代わりに別のビュー テンプレート ファイルを HTML 応答を生成するために通常使用おします。</span><span class="sxs-lookup"><span data-stu-id="f3378-152">Instead we'll typically use a separate View template file to help generate the HTML response.</span></span> <span data-ttu-id="f3378-153">結果がどのようにこれを見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="f3378-153">Let's look at how we can do this.</span></span> <span data-ttu-id="f3378-154">お使いのブラウザーを閉じて、IDE に戻ります。</span><span class="sxs-lookup"><span data-stu-id="f3378-154">Close your browser and return to the IDE.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f3378-155">[前へ](getting-started-with-mvc-part1.md)
> [次へ](getting-started-with-mvc-part3.md)</span><span class="sxs-lookup"><span data-stu-id="f3378-155">[Previous](getting-started-with-mvc-part1.md)
[Next](getting-started-with-mvc-part3.md)</span></span>
