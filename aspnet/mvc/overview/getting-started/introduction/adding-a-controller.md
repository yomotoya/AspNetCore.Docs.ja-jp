---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: コント ローラーの追加 |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: be8b21b4b8db60b1a67918d2df8779e729243fc8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374673"
---
<a name="adding-a-controller"></a><span data-ttu-id="e6da9-102">コントローラーを追加する</span><span class="sxs-lookup"><span data-stu-id="e6da9-102">Adding a Controller</span></span>
====================
<span data-ttu-id="e6da9-103">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="e6da9-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="e6da9-104">MVC の略*モデル-ビュー-コント ローラー*します。</span><span class="sxs-lookup"><span data-stu-id="e6da9-104">MVC stands for *model-view-controller*.</span></span> <span data-ttu-id="e6da9-105">MVC は、うまく設計された、テスト可能な簡単に管理されるアプリケーションを開発するためのパターンです。</span><span class="sxs-lookup"><span data-stu-id="e6da9-105">MVC is a pattern for developing applications that are well architected, testable and easy to maintain.</span></span> <span data-ttu-id="e6da9-106">MVC ベースのアプリケーションが含まれます。</span><span class="sxs-lookup"><span data-stu-id="e6da9-106">MVC-based applications contain:</span></span>

- <span data-ttu-id="e6da9-107">**M**デル: アプリケーションのデータを表すし、そのデータに対するビジネス ルールを適用する検証ロジックを使用するクラス。</span><span class="sxs-lookup"><span data-stu-id="e6da9-107">**M** odels: Classes that represent the data of the application and that use validation logic to enforce business rules for that data.</span></span>
- <span data-ttu-id="e6da9-108">**V**ュー: アプリケーションは、HTML 応答を動的に生成に使用されるテンプレート ファイル。</span><span class="sxs-lookup"><span data-stu-id="e6da9-108">**V** iews: Template files that your application uses to dynamically generate HTML responses.</span></span>
- <span data-ttu-id="e6da9-109">**C**ントローラー: ブラウザーの着信要求を処理するクラスがモデルのデータを取得し、ブラウザーへの応答を返すビュー テンプレートを指定します。</span><span class="sxs-lookup"><span data-stu-id="e6da9-109">**C** ontrollers: Classes that handle incoming browser requests, retrieve model data, and then specify view templates that return a response to the browser.</span></span>

<span data-ttu-id="e6da9-110">このチュートリアル シリーズでこれらすべての概念についての説明がされ、アプリケーションの構築に使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="e6da9-110">We'll be covering all these concepts in this tutorial series and show you how to use them to build an application.</span></span>

> [!NOTE]
> <span data-ttu-id="e6da9-111">前の手順では、既定の MVC テンプレートが選択されました。</span><span class="sxs-lookup"><span data-stu-id="e6da9-111">In the previous step the Default MVC template was selected.</span></span> <span data-ttu-id="e6da9-112">アカウントと既定のコント ローラーの管理、作成します。</span><span class="sxs-lookup"><span data-stu-id="e6da9-112">This creates Home, Account and Manage Controllers by default.</span></span>

<span data-ttu-id="e6da9-113">コント ローラー クラスを作成してから始めましょう。</span><span class="sxs-lookup"><span data-stu-id="e6da9-113">Let's begin by creating a controller class.</span></span> <span data-ttu-id="e6da9-114">**ソリューション エクスプ ローラー**を右クリックし、*コント ローラー*フォルダーをクリック**追加**、し**コント ローラー**します。</span><span class="sxs-lookup"><span data-stu-id="e6da9-114">In **Solution Explorer**, right-click the *Controllers* folder and then click **Add**, then **Controller**.</span></span>


![](adding-a-controller/_static/image1.png)

<span data-ttu-id="e6da9-115">**スキャフォールディングの追加**ダイアログ ボックスで、をクリックして**MVC 5 コント ローラー - 空**、 をクリックし、**追加**。</span><span class="sxs-lookup"><span data-stu-id="e6da9-115">In the **Add Scaffold** dialog box, click **MVC 5 Controller - Empty**, and then click **Add**.</span></span>

![](adding-a-controller/_static/image2.png)  
 

<span data-ttu-id="e6da9-116">新しいコント ローラー"HelloWorldController"という名前を**追加**します。</span><span class="sxs-lookup"><span data-stu-id="e6da9-116">Name your new controller "HelloWorldController" and click **Add**.</span></span>

![コント ローラーを追加します。](adding-a-controller/_static/image3.png)

<span data-ttu-id="e6da9-118">**ソリューション エクスプ ローラー**新しいファイルが作成されたこと名前付き*HelloWorldController.cs*と新しいフォルダー *Views\HelloWorld*します。</span><span class="sxs-lookup"><span data-stu-id="e6da9-118">Notice in **Solution Explorer** that a new file has been created named *HelloWorldController.cs* and a new folder *Views\HelloWorld*.</span></span> <span data-ttu-id="e6da9-119">コント ローラーは、IDE で開いています。</span><span class="sxs-lookup"><span data-stu-id="e6da9-119">The controller is open in the IDE.</span></span>

![](adding-a-controller/_static/image4.png)

<span data-ttu-id="e6da9-120">ファイルの内容を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="e6da9-120">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

<span data-ttu-id="e6da9-121">コント ローラーのメソッドでは、例として、HTML の文字列を返します。</span><span class="sxs-lookup"><span data-stu-id="e6da9-121">The controller methods will return a string of HTML as an example.</span></span> <span data-ttu-id="e6da9-122">コント ローラーの名前は`HelloWorldController`最初のメソッドの名前は`Index`します。</span><span class="sxs-lookup"><span data-stu-id="e6da9-122">The controller is named `HelloWorldController` and the first method is named `Index`.</span></span> <span data-ttu-id="e6da9-123">ブラウザーから呼び出すことしましょう。</span><span class="sxs-lookup"><span data-stu-id="e6da9-123">Let's invoke it from a browser.</span></span> <span data-ttu-id="e6da9-124">(F5 キーまたは ctrl キーを押しながら f5 キーを押して) アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="e6da9-124">Run the application (press F5 or Ctrl+F5).</span></span> <span data-ttu-id="e6da9-125">ブラウザーで、追加&quot;HelloWorld&quot;アドレス バーにパスにします。</span><span class="sxs-lookup"><span data-stu-id="e6da9-125">In the browser, append &quot;HelloWorld&quot; to the path in the address bar.</span></span> <span data-ttu-id="e6da9-126">(たとえば、次の図で`http://localhost:1234/HelloWorld.`)、ブラウザーでページが次のスクリーン ショットのようになります。</span><span class="sxs-lookup"><span data-stu-id="e6da9-126">(For example, in the illustration below, it's `http://localhost:1234/HelloWorld.`) The page in the browser will look like the following screenshot.</span></span> <span data-ttu-id="e6da9-127">上記のメソッドでは、コードは、文字列を直接返します。</span><span class="sxs-lookup"><span data-stu-id="e6da9-127">In the method above, the code returned a string directly.</span></span> <span data-ttu-id="e6da9-128">いくつかの HTML を返すだけに、システムの指示おり、そうしました。</span><span class="sxs-lookup"><span data-stu-id="e6da9-128">You told the system to just return some HTML, and it did!</span></span>

![](adding-a-controller/_static/image5.png)

<span data-ttu-id="e6da9-129">ASP.NET MVC では、着信 URL に応じて別のコント ローラー クラス (およびそれらに含まれるさまざまなアクション メソッド) を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="e6da9-129">ASP.NET MVC invokes different controller classes (and different action methods within them) depending on the incoming URL.</span></span> <span data-ttu-id="e6da9-130">ASP.NET MVC で使用される既定の URL ルーティング ロジックでは、このような形式を使用して、呼び出すには、どのようなコードを確認します。</span><span class="sxs-lookup"><span data-stu-id="e6da9-130">The default URL routing logic used by ASP.NET MVC uses a format like this to determine what code to invoke:</span></span>

`/[Controller]/[ActionName]/[Parameters]`

<span data-ttu-id="e6da9-131">ルーティングの形式を設定する、*アプリ\_Start/RouteConfig.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="e6da9-131">You set the format for routing in the *App\_Start/RouteConfig.cs* file.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

<span data-ttu-id="e6da9-132">アプリケーションを実行しない URL セグメントを指定すると、既定値は、"Home"コント ローラーと"Index"アクション メソッドには、上記のコードの既定値のセクションで指定します。</span><span class="sxs-lookup"><span data-stu-id="e6da9-132">When you run the application and don't supply any URL segments, it defaults to the "Home" controller and the "Index" action method specified in the defaults section of the code above.</span></span>

<span data-ttu-id="e6da9-133">URL の最初の部分を実行するコント ローラー クラスを決定します。</span><span class="sxs-lookup"><span data-stu-id="e6da9-133">The first part of the URL determines the controller class to execute.</span></span> <span data-ttu-id="e6da9-134">したがって */HelloWorld*にマップされます、`HelloWorldController`クラス。</span><span class="sxs-lookup"><span data-stu-id="e6da9-134">So */HelloWorld* maps to the `HelloWorldController` class.</span></span> <span data-ttu-id="e6da9-135">URL の 2 番目の部分は、実行するには、クラスのアクション メソッドを決定します。</span><span class="sxs-lookup"><span data-stu-id="e6da9-135">The second part of the URL determines the action method on the class to execute.</span></span> <span data-ttu-id="e6da9-136">したがって */HelloWorld/インデックス*なる、`Index`のメソッド、`HelloWorldController`を実行するクラス。</span><span class="sxs-lookup"><span data-stu-id="e6da9-136">So */HelloWorld/Index* would cause the `Index` method of the `HelloWorldController` class to execute.</span></span> <span data-ttu-id="e6da9-137">のみを参照しなければならなかったこと */HelloWorld*と`Index`メソッドが既定で使用されました。</span><span class="sxs-lookup"><span data-stu-id="e6da9-137">Notice that we only had to browse to */HelloWorld* and the `Index` method was used by default.</span></span> <span data-ttu-id="e6da9-138">これは、という名前のメソッドであるため`Index`いずれかが明示的に指定されていない場合、コント ローラーで呼び出される既定のメソッドします。</span><span class="sxs-lookup"><span data-stu-id="e6da9-138">This is because a method named `Index` is the default method that will be called on a controller if one is not explicitly specified.</span></span> <span data-ttu-id="e6da9-139">URL セグメントの 3 番目の部分 (`Parameters`) はルート データ用です。</span><span class="sxs-lookup"><span data-stu-id="e6da9-139">The third part of the URL segment ( `Parameters`) is for route data.</span></span> <span data-ttu-id="e6da9-140">ルート データは、このチュートリアルで後で表示されます。</span><span class="sxs-lookup"><span data-stu-id="e6da9-140">We'll see route data later on in this tutorial.</span></span>

<span data-ttu-id="e6da9-141">`http://localhost:xxxx/HelloWorld/Welcome` を参照します。</span><span class="sxs-lookup"><span data-stu-id="e6da9-141">Browse to `http://localhost:xxxx/HelloWorld/Welcome`.</span></span> <span data-ttu-id="e6da9-142">`Welcome`メソッドを実行し、文字列を返します&quot;へようこそ のアクション メソッドになります.&quot;.</span><span class="sxs-lookup"><span data-stu-id="e6da9-142">The `Welcome` method runs and returns the string &quot;This is the Welcome action method...&quot;.</span></span> <span data-ttu-id="e6da9-143">既定の MVC のマッピングは`/[Controller]/[ActionName]/[Parameters]`します。</span><span class="sxs-lookup"><span data-stu-id="e6da9-143">The default MVC mapping is `/[Controller]/[ActionName]/[Parameters]`.</span></span> <span data-ttu-id="e6da9-144">この URL では、コントローラーは `HelloWorld` で、`Welcome` がアクション メソッドです。</span><span class="sxs-lookup"><span data-stu-id="e6da9-144">For this URL, the controller is `HelloWorld` and `Welcome` is the action method.</span></span> <span data-ttu-id="e6da9-145">URL の `[Parameters]` の部分はまだ使っていません。</span><span class="sxs-lookup"><span data-stu-id="e6da9-145">You haven't used the `[Parameters]` part of the URL yet.</span></span>

![](adding-a-controller/_static/image6.png)

<span data-ttu-id="e6da9-146">みましょうコント ローラーに、URL からいくつかのパラメーター情報を渡すことができるように、例を少し変更 (たとえば、 */helloworld/welcome でしょうか。 名前 = Scott&amp;numtimes = 4*)。</span><span class="sxs-lookup"><span data-stu-id="e6da9-146">Let's modify the example slightly so that you can pass some parameter information from the URL to the controller (for example, */HelloWorld/Welcome?name=Scott&amp;numtimes=4*).</span></span> <span data-ttu-id="e6da9-147">変更、`Welcome`メソッドを次に示すように、2 つのパラメーターを含めます。</span><span class="sxs-lookup"><span data-stu-id="e6da9-147">Change your `Welcome` method to include two parameters as shown below.</span></span> <span data-ttu-id="e6da9-148">コードでは、C# で省略可能なパラメーターの機能を使用していることを示す注、`numTimes`そのパラメーターの値が渡されない場合、1 にパラメーターが既定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e6da9-148">Note that the code uses the C# optional-parameter feature to indicate that the `numTimes` parameter should default to 1 if no value is passed for that parameter.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="e6da9-149">セキュリティに関する注意: 使用して上記コード[HttpUtility.HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx)悪意のある入力 (つまり JavaScript) からアプリケーションを保護します。</span><span class="sxs-lookup"><span data-stu-id="e6da9-149">Security Note: The code above uses [HttpUtility.HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) to protect the application from malicious input (namely JavaScript).</span></span> <span data-ttu-id="e6da9-150">詳細については、次を参照してください。[方法: 保護に対してスクリプトによる攻略の文字列を HTML エンコードを適用することによって Web アプリケーションで](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="e6da9-150">For more information see [How to: Protect Against Script Exploits in a Web Application by Applying HTML Encoding to Strings](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).</span></span>


 <span data-ttu-id="e6da9-151">アプリケーションを実行し、URL の例を参照 (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`)。</span><span class="sxs-lookup"><span data-stu-id="e6da9-151">Run your application and browse to the example URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`).</span></span> <span data-ttu-id="e6da9-152">さまざまな値を試みることができます`name`と`numtimes`URL にします。</span><span class="sxs-lookup"><span data-stu-id="e6da9-152">You can try different values for `name` and `numtimes` in the URL.</span></span> <span data-ttu-id="e6da9-153">[ASP.NET MVC モデル バインド システム](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)メソッドのパラメーターに、アドレス バーにクエリ文字列から名前付きパラメーターを自動的にマップされます。</span><span class="sxs-lookup"><span data-stu-id="e6da9-153">The [ASP.NET MVC model binding system](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatically maps the named parameters from the query string in the address bar to parameters in your method.</span></span>

![](adding-a-controller/_static/image7.png)

<span data-ttu-id="e6da9-154">URL セグメント上のサンプル ( `Parameters`) が使用されていない、`name`と`numTimes`パラメーターとして渡されます[クエリ文字列](http://en.wikipedia.org/wiki/Query_string)します。</span><span class="sxs-lookup"><span data-stu-id="e6da9-154">In the sample above, the URL segment ( `Parameters`) is not used, the `name` and `numTimes` parameters are passed as [query strings](http://en.wikipedia.org/wiki/Query_string).</span></span> <span data-ttu-id="e6da9-155">?</span><span class="sxs-lookup"><span data-stu-id="e6da9-155">The ?</span></span> <span data-ttu-id="e6da9-156">(疑問符) 区切り記号では、上記の URL とクエリ文字列に従ってください。</span><span class="sxs-lookup"><span data-stu-id="e6da9-156">(question mark) in the above URL is a separator, and the query strings follow.</span></span> <span data-ttu-id="e6da9-157">&amp; 文字は、クエリ文字列を区切ります。</span><span class="sxs-lookup"><span data-stu-id="e6da9-157">The &amp; character separates query strings.</span></span>

<span data-ttu-id="e6da9-158">[ようこそ] のメソッドを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="e6da9-158">Replace the Welcome method with the following code:</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

<span data-ttu-id="e6da9-159">アプリケーションを実行し、次の URL を入力します。 `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`</span><span class="sxs-lookup"><span data-stu-id="e6da9-159">Run the application and enter the following URL: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`</span></span>

![](adding-a-controller/_static/image8.png)

<span data-ttu-id="e6da9-160">この時間 3 番目の URL セグメントに一致するルート パラメーター `ID.` 、`Welcome`アクション メソッドにパラメーターが含まれています (`ID`) 内の URL 仕様に一致する、`RegisterRoutes`メソッド。</span><span class="sxs-lookup"><span data-stu-id="e6da9-160">This time the third URL segment matched the route parameter `ID.` The `Welcome` action method contains a parameter (`ID`) that matched the URL specification in the `RegisterRoutes` method.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

<span data-ttu-id="e6da9-161">ASP.NET MVC アプリケーションでは、クエリ文字列として渡すよりも、ルート データ (上記の ID を使用したとき) などとしてパラメーターを渡すより一般的です。</span><span class="sxs-lookup"><span data-stu-id="e6da9-161">In ASP.NET MVC applications, it's more typical to pass in parameters as route data (like we did with ID above) than passing them as query strings.</span></span> <span data-ttu-id="e6da9-162">両方に渡すルートを追加することも、`name`と`numtimes`url ルート データとしてパラメーター。</span><span class="sxs-lookup"><span data-stu-id="e6da9-162">You could also add a route to pass both the `name` and `numtimes` in parameters as route data in the URL.</span></span> <span data-ttu-id="e6da9-163">*アプリ\_\routeconfig.cs*ファイルに、「こんにちは」のルートを追加します。</span><span class="sxs-lookup"><span data-stu-id="e6da9-163">In the *App\_Start\RouteConfig.cs* file, add the "Hello" route:</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

<span data-ttu-id="e6da9-164">アプリケーションを実行しを参照する`/localhost:XXX/HelloWorld/Welcome/Scott/3`します。</span><span class="sxs-lookup"><span data-stu-id="e6da9-164">Run the application and browse to `/localhost:XXX/HelloWorld/Welcome/Scott/3`.</span></span>

![](adding-a-controller/_static/image9.png)

<span data-ttu-id="e6da9-165">多くの MVC アプリケーションでは、既定のルートが正しく動作します。</span><span class="sxs-lookup"><span data-stu-id="e6da9-165">For many MVC applications, the default route works fine.</span></span> <span data-ttu-id="e6da9-166">モデル バインダーを使用してデータを渡すには、このチュートリアルの後半でを学習して、そのため、既定のルートを変更する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="e6da9-166">You'll learn later in this tutorial to pass data using the model binder, and you won't have to modify the default route for that.</span></span>

<span data-ttu-id="e6da9-167">これらの例で、コント ローラーが実行されて、 &quot;VC&quot;の MVC 部分: ビューとコント ローラーの作業は、します。</span><span class="sxs-lookup"><span data-stu-id="e6da9-167">In these examples the controller has been doing the &quot;VC&quot; portion of MVC — that is, the view and controller work.</span></span> <span data-ttu-id="e6da9-168">コント ローラーは、HTML を直接返しています。</span><span class="sxs-lookup"><span data-stu-id="e6da9-168">The controller is returning HTML directly.</span></span> <span data-ttu-id="e6da9-169">通常のコードに非常に面倒になるため、直接 HTML を返すコント ローラーは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="e6da9-169">Ordinarily you don't want controllers returning HTML directly, since that becomes very cumbersome to code.</span></span> <span data-ttu-id="e6da9-170">代わりに、HTML 応答を生成するために使用します別のビュー テンプレート ファイルを通常。</span><span class="sxs-lookup"><span data-stu-id="e6da9-170">Instead we'll typically use a separate view template file to help generate the HTML response.</span></span> <span data-ttu-id="e6da9-171">そのため方法で [次へ] を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="e6da9-171">Let's look next at how we can do this.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e6da9-172">[前へ](getting-started.md)
> [次へ](adding-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="e6da9-172">[Previous](getting-started.md)
[Next](adding-a-view.md)</span></span>
