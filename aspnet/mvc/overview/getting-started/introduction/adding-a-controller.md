---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: コント ローラーの追加 |Microsoft ドキュメント
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 3864bab284661b0c44f9e4cb363c2d60eccc7c66
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-controller"></a><span data-ttu-id="df7d1-102">コントローラーを追加する</span><span class="sxs-lookup"><span data-stu-id="df7d1-102">Adding a Controller</span></span>
====================
<span data-ttu-id="df7d1-103">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="df7d1-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="df7d1-104">MVC*モデル-ビュー-コント ローラー*です。</span><span class="sxs-lookup"><span data-stu-id="df7d1-104">MVC stands for *model-view-controller*.</span></span> <span data-ttu-id="df7d1-105">MVC が適切に設計された、テストが容易な保守し、やすいアプリケーションの開発パターンです。</span><span class="sxs-lookup"><span data-stu-id="df7d1-105">MVC is a pattern for developing applications that are well architected, testable and easy to maintain.</span></span> <span data-ttu-id="df7d1-106">MVC ベースのアプリケーションが含まれます。</span><span class="sxs-lookup"><span data-stu-id="df7d1-106">MVC-based applications contain:</span></span>

- <span data-ttu-id="df7d1-107">**M** odels: アプリケーションのデータを表すし、そのデータにビジネス ルールを適用する検証ロジックを使用するクラス。</span><span class="sxs-lookup"><span data-stu-id="df7d1-107">**M** odels: Classes that represent the data of the application and that use validation logic to enforce business rules for that data.</span></span>
- <span data-ttu-id="df7d1-108">**V** iews: アプリケーションが HTML 応答を動的に生成に使用されるテンプレート ファイル。</span><span class="sxs-lookup"><span data-stu-id="df7d1-108">**V** iews: Template files that your application uses to dynamically generate HTML responses.</span></span>
- <span data-ttu-id="df7d1-109">**C** ontrollers: ブラウザーの入力方向の要求を処理するクラスがモデルのデータを取得し、ブラウザーへの応答を返すビュー テンプレートを指定します。</span><span class="sxs-lookup"><span data-stu-id="df7d1-109">**C** ontrollers: Classes that handle incoming browser requests, retrieve model data, and then specify view templates that return a response to the browser.</span></span>

<span data-ttu-id="df7d1-110">このチュートリアルの系列でこれらすべての概念を説明して行いますを使用してアプリケーションを構築する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="df7d1-110">We'll be covering all these concepts in this tutorial series and show you how to use them to build an application.</span></span>

> [!NOTE]
> <span data-ttu-id="df7d1-111">前の手順では、既定の MVC テンプレートが選択されました。</span><span class="sxs-lookup"><span data-stu-id="df7d1-111">In the previous step the Default MVC template was selected.</span></span> <span data-ttu-id="df7d1-112">アカウントと既定ではコント ローラーの管理、作成します。</span><span class="sxs-lookup"><span data-stu-id="df7d1-112">This creates Home, Account and Manage Controllers by default.</span></span>

<span data-ttu-id="df7d1-113">コント ローラー クラスを作成してみましょう。</span><span class="sxs-lookup"><span data-stu-id="df7d1-113">Let's begin by creating a controller class.</span></span> <span data-ttu-id="df7d1-114">**ソリューション エクスプ ローラー**を右クリックし、*コント ローラー*フォルダーをクリックして**追加**、し**コント ローラー**です。</span><span class="sxs-lookup"><span data-stu-id="df7d1-114">In **Solution Explorer**, right-click the *Controllers* folder and then click **Add**, then **Controller**.</span></span>


![](adding-a-controller/_static/image1.png)

<span data-ttu-id="df7d1-115">**追加 Scaffold**ダイアログ ボックスで、をクリックして**MVC 5 コント ローラー - 空**、順にクリック**追加**です。</span><span class="sxs-lookup"><span data-stu-id="df7d1-115">In the **Add Scaffold** dialog box, click **MVC 5 Controller - Empty**, and then click **Add**.</span></span>

![](adding-a-controller/_static/image2.png)  
 

<span data-ttu-id="df7d1-116">新しいコント ローラー"HelloWorldController"という名前をクリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="df7d1-116">Name your new controller "HelloWorldController" and click **Add**.</span></span>

![コント ローラーを追加します。](adding-a-controller/_static/image3.png)

<span data-ttu-id="df7d1-118">**ソリューション エクスプ ローラー**新しいファイルが作成されている名前付き*HelloWorldController.cs*と新しいフォルダー *Views\HelloWorld*です。</span><span class="sxs-lookup"><span data-stu-id="df7d1-118">Notice in **Solution Explorer** that a new file has been created named *HelloWorldController.cs* and a new folder *Views\HelloWorld*.</span></span> <span data-ttu-id="df7d1-119">コント ローラーは、IDE で開いています。</span><span class="sxs-lookup"><span data-stu-id="df7d1-119">The controller is open in the IDE.</span></span>

![](adding-a-controller/_static/image4.png)

<span data-ttu-id="df7d1-120">ファイルの内容を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="df7d1-120">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

<span data-ttu-id="df7d1-121">コント ローラーのメソッドでは、例として、HTML の文字列を返します。</span><span class="sxs-lookup"><span data-stu-id="df7d1-121">The controller methods will return a string of HTML as an example.</span></span> <span data-ttu-id="df7d1-122">コント ローラーの名前は`HelloWorldController`で最初のメソッドの名前が`Index`です。</span><span class="sxs-lookup"><span data-stu-id="df7d1-122">The controller is named `HelloWorldController` and the first method is named `Index`.</span></span> <span data-ttu-id="df7d1-123">ブラウザーから呼び出すことしてみましょう。</span><span class="sxs-lookup"><span data-stu-id="df7d1-123">Let's invoke it from a browser.</span></span> <span data-ttu-id="df7d1-124">(F5 キーまたは ctrl キーを押しながら F5 キーを押して) アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="df7d1-124">Run the application (press F5 or Ctrl+F5).</span></span> <span data-ttu-id="df7d1-125">ブラウザーで、追加&quot;HelloWorld&quot;アドレス バーにパスにします。</span><span class="sxs-lookup"><span data-stu-id="df7d1-125">In the browser, append &quot;HelloWorld&quot; to the path in the address bar.</span></span> <span data-ttu-id="df7d1-126">(たとえば、以下の図で`http://localhost:1234/HelloWorld.`) ブラウザーでページは次のスクリーン ショットのようになります。</span><span class="sxs-lookup"><span data-stu-id="df7d1-126">(For example, in the illustration below, it's `http://localhost:1234/HelloWorld.`) The page in the browser will look like the following screenshot.</span></span> <span data-ttu-id="df7d1-127">上記のメソッドで返される、コード、文字列直接です。</span><span class="sxs-lookup"><span data-stu-id="df7d1-127">In the method above, the code returned a string directly.</span></span> <span data-ttu-id="df7d1-128">システムだけの HTML を返すように通知してと同じです。</span><span class="sxs-lookup"><span data-stu-id="df7d1-128">You told the system to just return some HTML, and it did!</span></span>

![](adding-a-controller/_static/image5.png)

<span data-ttu-id="df7d1-129">ASP.NET MVC では、着信 URL に応じて別のコント ローラー クラス (およびそれらに含まれるさまざまなアクション メソッド) を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="df7d1-129">ASP.NET MVC invokes different controller classes (and different action methods within them) depending on the incoming URL.</span></span> <span data-ttu-id="df7d1-130">ASP.NET MVC で使用される既定の URL ルーティング ロジックでは、次のような形式を使用して、呼び出すには、どのようなコードを確認します。</span><span class="sxs-lookup"><span data-stu-id="df7d1-130">The default URL routing logic used by ASP.NET MVC uses a format like this to determine what code to invoke:</span></span>

`/[Controller]/[ActionName]/[Parameters]`

<span data-ttu-id="df7d1-131">ルーティングの書式を設定する、*アプリ\_Start/RouteConfig.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="df7d1-131">You set the format for routing in the *App\_Start/RouteConfig.cs* file.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

<span data-ttu-id="df7d1-132">アプリケーションを実行し、URL セグメントを指定しないで、既定値は「ホーム」コント ローラーと"Index"アクション メソッドが上記のコードの既定値 セクションで指定されます。</span><span class="sxs-lookup"><span data-stu-id="df7d1-132">When you run the application and don't supply any URL segments, it defaults to the "Home" controller and the "Index" action method specified in the defaults section of the code above.</span></span>

<span data-ttu-id="df7d1-133">URL の最初の部分を実行するコント ローラー クラスを決定します。</span><span class="sxs-lookup"><span data-stu-id="df7d1-133">The first part of the URL determines the controller class to execute.</span></span> <span data-ttu-id="df7d1-134">したがって*/HelloWorld*にマップ、`HelloWorldController`クラスです。</span><span class="sxs-lookup"><span data-stu-id="df7d1-134">So */HelloWorld* maps to the `HelloWorldController` class.</span></span> <span data-ttu-id="df7d1-135">URL の 2 番目の部分では、実行するには、クラスにアクション メソッドを判断します。</span><span class="sxs-lookup"><span data-stu-id="df7d1-135">The second part of the URL determines the action method on the class to execute.</span></span> <span data-ttu-id="df7d1-136">したがって*HelloWorld/インデックス*が切り替わるところ、`Index`のメソッド、`HelloWorldController`を実行するクラス。</span><span class="sxs-lookup"><span data-stu-id="df7d1-136">So */HelloWorld/Index* would cause the `Index` method of the `HelloWorldController` class to execute.</span></span> <span data-ttu-id="df7d1-137">のみを参照しなければならなかったことを確認*/HelloWorld*と`Index`メソッドは、既定で使用されました。</span><span class="sxs-lookup"><span data-stu-id="df7d1-137">Notice that we only had to browse to */HelloWorld* and the `Index` method was used by default.</span></span> <span data-ttu-id="df7d1-138">これは、という名前のメソッドであるため`Index`いずれかが明示的に指定されていない場合に、コント ローラーで呼び出される既定の方法です。</span><span class="sxs-lookup"><span data-stu-id="df7d1-138">This is because a method named `Index` is the default method that will be called on a controller if one is not explicitly specified.</span></span> <span data-ttu-id="df7d1-139">URL セグメントの 3 番目の部分 (`Parameters`) はルート データ用です。</span><span class="sxs-lookup"><span data-stu-id="df7d1-139">The third part of the URL segment ( `Parameters`) is for route data.</span></span> <span data-ttu-id="df7d1-140">ルート データは、このチュートリアルで後で表示されます。</span><span class="sxs-lookup"><span data-stu-id="df7d1-140">We'll see route data later on in this tutorial.</span></span>

<span data-ttu-id="df7d1-141">`http://localhost:xxxx/HelloWorld/Welcome` を参照します。</span><span class="sxs-lookup"><span data-stu-id="df7d1-141">Browse to `http://localhost:xxxx/HelloWorld/Welcome`.</span></span> <span data-ttu-id="df7d1-142">`Welcome`メソッドが実行され、文字列を返します&quot;へようこそ のアクション メソッドは、このしています.&quot;.</span><span class="sxs-lookup"><span data-stu-id="df7d1-142">The `Welcome` method runs and returns the string &quot;This is the Welcome action method...&quot;.</span></span> <span data-ttu-id="df7d1-143">既定の MVC マッピングは`/[Controller]/[ActionName]/[Parameters]`します。</span><span class="sxs-lookup"><span data-stu-id="df7d1-143">The default MVC mapping is `/[Controller]/[ActionName]/[Parameters]`.</span></span> <span data-ttu-id="df7d1-144">この URL では、コントローラーは `HelloWorld` で、`Welcome` がアクション メソッドです。</span><span class="sxs-lookup"><span data-stu-id="df7d1-144">For this URL, the controller is `HelloWorld` and `Welcome` is the action method.</span></span> <span data-ttu-id="df7d1-145">URL の `[Parameters]` の部分はまだ使っていません。</span><span class="sxs-lookup"><span data-stu-id="df7d1-145">You haven't used the `[Parameters]` part of the URL yet.</span></span>

![](adding-a-controller/_static/image6.png)

<span data-ttu-id="df7d1-146">みましょうコント ローラーへの URL からいくつかのパラメーター情報を渡すことができるように、この例を少し変更 (たとえば、 *HelloWorld/ウェルカムしますか? 名前 Scott を =&amp;numtimes = 4*)。</span><span class="sxs-lookup"><span data-stu-id="df7d1-146">Let's modify the example slightly so that you can pass some parameter information from the URL to the controller (for example, */HelloWorld/Welcome?name=Scott&amp;numtimes=4*).</span></span> <span data-ttu-id="df7d1-147">変更、`Welcome`メソッドを次に示すように、2 つのパラメーターを含めます。</span><span class="sxs-lookup"><span data-stu-id="df7d1-147">Change your `Welcome` method to include two parameters as shown below.</span></span> <span data-ttu-id="df7d1-148">コードでは、C# で省略可能なパラメーターの機能を使用して示す注、`numTimes`パラメーターが既定として設定を 1 にそのパラメーターの値が渡されない場合。</span><span class="sxs-lookup"><span data-stu-id="df7d1-148">Note that the code uses the C# optional-parameter feature to indicate that the `numTimes` parameter should default to 1 if no value is passed for that parameter.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="df7d1-149">セキュリティに関する注意: 使用して上記コード[HttpUtility.HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx)悪意のある入力 (つまり JavaScript) から、アプリケーションを保護します。</span><span class="sxs-lookup"><span data-stu-id="df7d1-149">Security Note: The code above uses [HttpUtility.HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) to protect the application from malicious input (namely JavaScript).</span></span> <span data-ttu-id="df7d1-150">詳細については、次を参照してください。[する方法: 保護に対してスクリプトによる攻略の文字列を HTML エンコードを適用することによって、Web アプリケーションで](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="df7d1-150">For more information see [How to: Protect Against Script Exploits in a Web Application by Applying HTML Encoding to Strings](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).</span></span>


 <span data-ttu-id="df7d1-151">アプリケーションを実行し、URL の例を参照 (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`)。</span><span class="sxs-lookup"><span data-stu-id="df7d1-151">Run your application and browse to the example URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`).</span></span> <span data-ttu-id="df7d1-152">値が異なるを試みることができます`name`と`numtimes`URL にします。</span><span class="sxs-lookup"><span data-stu-id="df7d1-152">You can try different values for `name` and `numtimes` in the URL.</span></span> <span data-ttu-id="df7d1-153">[ASP.NET MVC モデル バインド システム](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)メソッドのパラメーターに、アドレス バーにクエリ文字列から名前付きパラメーターが自動的にマップします。</span><span class="sxs-lookup"><span data-stu-id="df7d1-153">The [ASP.NET MVC model binding system](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatically maps the named parameters from the query string in the address bar to parameters in your method.</span></span>

![](adding-a-controller/_static/image7.png)

<span data-ttu-id="df7d1-154">URL セグメントの上のサンプル ( `Parameters`) を使用していない、`name`と`numTimes`パラメーターとして渡されます[クエリ文字列](http://en.wikipedia.org/wiki/Query_string)です。</span><span class="sxs-lookup"><span data-stu-id="df7d1-154">In the sample above, the URL segment ( `Parameters`) is not used, the `name` and `numTimes` parameters are passed as [query strings](http://en.wikipedia.org/wiki/Query_string).</span></span> <span data-ttu-id="df7d1-155">?</span><span class="sxs-lookup"><span data-stu-id="df7d1-155">The ?</span></span> <span data-ttu-id="df7d1-156">(疑問符) 上記の URL では、区切り記号を次のクエリ文字列。</span><span class="sxs-lookup"><span data-stu-id="df7d1-156">(question mark) in the above URL is a separator, and the query strings follow.</span></span> <span data-ttu-id="df7d1-157">&amp; 文字は、クエリ文字列を区切ります。</span><span class="sxs-lookup"><span data-stu-id="df7d1-157">The &amp; character separates query strings.</span></span>

<span data-ttu-id="df7d1-158">ようこそメソッドを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="df7d1-158">Replace the Welcome method with the following code:</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

<span data-ttu-id="df7d1-159">アプリケーションを実行し、次の URL を入力します。 `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`</span><span class="sxs-lookup"><span data-stu-id="df7d1-159">Run the application and enter the following URL: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`</span></span>

![](adding-a-controller/_static/image8.png)

<span data-ttu-id="df7d1-160">今度は、3 番目の URL セグメントが一致するルートのパラメーター `ID.` 、`Welcome`アクション メソッドにパラメーターが含まれています (`ID`) の URL の仕様に一致する、`RegisterRoutes`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="df7d1-160">This time the third URL segment matched the route parameter `ID.` The `Welcome` action method contains a parameter (`ID`) that matched the URL specification in the `RegisterRoutes` method.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

<span data-ttu-id="df7d1-161">ASP.NET MVC アプリケーションでは、一般的なクエリ文字列として渡すことよりもルート データ (同じように上記の ID を持つ) としてパラメーターを渡す、します。</span><span class="sxs-lookup"><span data-stu-id="df7d1-161">In ASP.NET MVC applications, it's more typical to pass in parameters as route data (like we did with ID above) than passing them as query strings.</span></span> <span data-ttu-id="df7d1-162">両方を渡すへのルートを追加することも、`name`と`numtimes`url ルート データとして使用されるパラメーターにします。</span><span class="sxs-lookup"><span data-stu-id="df7d1-162">You could also add a route to pass both the `name` and `numtimes` in parameters as route data in the URL.</span></span> <span data-ttu-id="df7d1-163">*アプリ\_Start\RouteConfig.cs*ファイルに「こんにちは」のルートを追加します。</span><span class="sxs-lookup"><span data-stu-id="df7d1-163">In the *App\_Start\RouteConfig.cs* file, add the "Hello" route:</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

<span data-ttu-id="df7d1-164">アプリケーションを実行しを参照`/localhost:XXX/HelloWorld/Welcome/Scott/3`です。</span><span class="sxs-lookup"><span data-stu-id="df7d1-164">Run the application and browse to `/localhost:XXX/HelloWorld/Welcome/Scott/3`.</span></span>

![](adding-a-controller/_static/image9.png)

<span data-ttu-id="df7d1-165">多くの MVC アプリケーションの既定のルートは問題なく動作します。</span><span class="sxs-lookup"><span data-stu-id="df7d1-165">For many MVC applications, the default route works fine.</span></span> <span data-ttu-id="df7d1-166">モデル バインダーを使用してデータを渡すには、このチュートリアルで後ほど説明して、その既定のルートを変更する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="df7d1-166">You'll learn later in this tutorial to pass data using the model binder, and you won't have to modify the default route for that.</span></span>

<span data-ttu-id="df7d1-167">これらの例で、コント ローラーが実行されて、 &quot;VC&quot; MVC 一部分: ビューとコント ローラーの作業は、します。</span><span class="sxs-lookup"><span data-stu-id="df7d1-167">In these examples the controller has been doing the &quot;VC&quot; portion of MVC — that is, the view and controller work.</span></span> <span data-ttu-id="df7d1-168">コント ローラーは、直接 HTML を返しています。</span><span class="sxs-lookup"><span data-stu-id="df7d1-168">The controller is returning HTML directly.</span></span> <span data-ttu-id="df7d1-169">通常はコント ローラーのコードに非常に複雑になるので、直接、HTML を返す必要はありません。</span><span class="sxs-lookup"><span data-stu-id="df7d1-169">Ordinarily you don't want controllers returning HTML directly, since that becomes very cumbersome to code.</span></span> <span data-ttu-id="df7d1-170">代わりに HTML 応答を生成するために使用されます、個別のビュー テンプレート ファイル通常。</span><span class="sxs-lookup"><span data-stu-id="df7d1-170">Instead we'll typically use a separate view template file to help generate the HTML response.</span></span> <span data-ttu-id="df7d1-171">結果がどのようにこれを次のことを確認してみましょう。</span><span class="sxs-lookup"><span data-stu-id="df7d1-171">Let's look next at how we can do this.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="df7d1-172">[前へ](getting-started.md)
> [次へ](adding-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="df7d1-172">[Previous](getting-started.md)
[Next](adding-a-view.md)</span></span>
