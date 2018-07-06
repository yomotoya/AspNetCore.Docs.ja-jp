---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
title: コント ローラーの追加 |Microsoft Docs
author: Rick-Anderson
description: '注: このチュートリアルの最新バージョンは ASP.NET MVC 5 と Visual Studio 2013 を使用します。 安全なはるかに簡単に従い、デモをお勧めしています.'
ms.author: aspnetcontent
ms.date: 08/28/2012
ms.assetid: 0267d31c-892f-49a1-9e7a-3ae8cc12b2ca
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: b9ba4336b2239d835b648b82674bd4cfa8ca43dc
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809035"
---
<a name="adding-a-controller"></a><span data-ttu-id="d5889-104">コントローラーを追加する</span><span class="sxs-lookup"><span data-stu-id="d5889-104">Adding a Controller</span></span>
====================
<span data-ttu-id="d5889-105">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="d5889-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="d5889-106">このチュートリアルの更新バージョンが利用可能な[ここ](../../getting-started/introduction/getting-started.md)ASP.NET MVC 5 と Visual Studio 2013 を使用します。</span><span class="sxs-lookup"><span data-stu-id="d5889-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="d5889-107">より安全ではるかに簡単に従うしより多くの機能を示します。</span><span class="sxs-lookup"><span data-stu-id="d5889-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="d5889-108">MVC の略*モデル-ビュー-コント ローラー*します。</span><span class="sxs-lookup"><span data-stu-id="d5889-108">MVC stands for *model-view-controller*.</span></span> <span data-ttu-id="d5889-109">MVC は、うまく設計された、テスト可能な簡単に管理されるアプリケーションを開発するためのパターンです。</span><span class="sxs-lookup"><span data-stu-id="d5889-109">MVC is a pattern for developing applications that are well architected, testable and easy to maintain.</span></span> <span data-ttu-id="d5889-110">MVC ベースのアプリケーションが含まれます。</span><span class="sxs-lookup"><span data-stu-id="d5889-110">MVC-based applications contain:</span></span>

- <span data-ttu-id="d5889-111">**M**デル: アプリケーションのデータを表すし、そのデータに対するビジネス ルールを適用する検証ロジックを使用するクラス。</span><span class="sxs-lookup"><span data-stu-id="d5889-111">**M** odels: Classes that represent the data of the application and that use validation logic to enforce business rules for that data.</span></span>
- <span data-ttu-id="d5889-112">**V**ュー: アプリケーションは、HTML 応答を動的に生成に使用されるテンプレート ファイル。</span><span class="sxs-lookup"><span data-stu-id="d5889-112">**V** iews: Template files that your application uses to dynamically generate HTML responses.</span></span>
- <span data-ttu-id="d5889-113">**C**ントローラー: ブラウザーの着信要求を処理するクラスがモデルのデータを取得し、ブラウザーへの応答を返すビュー テンプレートを指定します。</span><span class="sxs-lookup"><span data-stu-id="d5889-113">**C** ontrollers: Classes that handle incoming browser requests, retrieve model data, and then specify view templates that return a response to the browser.</span></span>

<span data-ttu-id="d5889-114">このチュートリアル シリーズでこれらすべての概念についての説明がされ、アプリケーションの構築に使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="d5889-114">We'll be covering all these concepts in this tutorial series and show you how to use them to build an application.</span></span>

<span data-ttu-id="d5889-115">コント ローラー クラスを作成してから始めましょう。</span><span class="sxs-lookup"><span data-stu-id="d5889-115">Let's begin by creating a controller class.</span></span> <span data-ttu-id="d5889-116">**ソリューション エクスプ ローラー**を右クリックし、*コント ローラー*クリックしてフォルダー**コント ローラーの追加**します。</span><span class="sxs-lookup"><span data-stu-id="d5889-116">In **Solution Explorer**, right-click the *Controllers* folder and then select **Add Controller**.</span></span>

![](adding-a-controller/_static/image1.png)

<span data-ttu-id="d5889-117">新しいコント ローラー &quot;HelloWorldController&quot;します。</span><span class="sxs-lookup"><span data-stu-id="d5889-117">Name your new controller &quot;HelloWorldController&quot;.</span></span> <span data-ttu-id="d5889-118">既定のテンプレートとしてのままに**空の MVC コント ローラー**  をクリック**追加**します。</span><span class="sxs-lookup"><span data-stu-id="d5889-118">Leave the default template as **Empty MVC controller** and click **Add**.</span></span>

![コント ローラーを追加します。](adding-a-controller/_static/image2.png)

<span data-ttu-id="d5889-120">**ソリューション エクスプ ローラー**新しいファイルが作成されたこと名前付き*HelloWorldController.cs*します。</span><span class="sxs-lookup"><span data-stu-id="d5889-120">Notice in **Solution Explorer** that a new file has been created named *HelloWorldController.cs*.</span></span> <span data-ttu-id="d5889-121">ファイルは、IDE で開いています。</span><span class="sxs-lookup"><span data-stu-id="d5889-121">The file is open in the IDE.</span></span>

![](adding-a-controller/_static/image3.png)

<span data-ttu-id="d5889-122">ファイルの内容を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="d5889-122">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

<span data-ttu-id="d5889-123">コント ローラーのメソッドでは、例として、HTML の文字列を返します。</span><span class="sxs-lookup"><span data-stu-id="d5889-123">The controller methods will return a string of HTML as an example.</span></span> <span data-ttu-id="d5889-124">コント ローラーの名前は`HelloWorldController`と上記の最初のメソッドが名前付き`Index`します。</span><span class="sxs-lookup"><span data-stu-id="d5889-124">The controller is named `HelloWorldController` and the first method above is named `Index`.</span></span> <span data-ttu-id="d5889-125">ブラウザーから呼び出すことしましょう。</span><span class="sxs-lookup"><span data-stu-id="d5889-125">Let's invoke it from a browser.</span></span> <span data-ttu-id="d5889-126">(F5 キーまたは ctrl キーを押しながら f5 キーを押して) アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="d5889-126">Run the application (press F5 or Ctrl+F5).</span></span> <span data-ttu-id="d5889-127">ブラウザーで、追加&quot;HelloWorld&quot;アドレス バーにパスにします。</span><span class="sxs-lookup"><span data-stu-id="d5889-127">In the browser, append &quot;HelloWorld&quot; to the path in the address bar.</span></span> <span data-ttu-id="d5889-128">(たとえば、次の図で`http://localhost:1234/HelloWorld.`)、ブラウザーでページが次のスクリーン ショットのようになります。</span><span class="sxs-lookup"><span data-stu-id="d5889-128">(For example, in the illustration below, it's `http://localhost:1234/HelloWorld.`) The page in the browser will look like the following screenshot.</span></span> <span data-ttu-id="d5889-129">上記のメソッドでは、コードは、文字列を直接返します。</span><span class="sxs-lookup"><span data-stu-id="d5889-129">In the method above, the code returned a string directly.</span></span> <span data-ttu-id="d5889-130">いくつかの HTML を返すだけに、システムの指示おり、そうしました。</span><span class="sxs-lookup"><span data-stu-id="d5889-130">You told the system to just return some HTML, and it did!</span></span>

![](adding-a-controller/_static/image4.png)

<span data-ttu-id="d5889-131">ASP.NET MVC では、着信 URL に応じて別のコント ローラー クラス (およびそれらに含まれるさまざまなアクション メソッド) を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="d5889-131">ASP.NET MVC invokes different controller classes (and different action methods within them) depending on the incoming URL.</span></span> <span data-ttu-id="d5889-132">ASP.NET MVC で使用される既定の URL ルーティング ロジックでは、このような形式を使用して、呼び出すには、どのようなコードを確認します。</span><span class="sxs-lookup"><span data-stu-id="d5889-132">The default URL routing logic used by ASP.NET MVC uses a format like this to determine what code to invoke:</span></span>

`/[Controller]/[ActionName]/[Parameters]`

<span data-ttu-id="d5889-133">URL の最初の部分を実行するコント ローラー クラスを決定します。</span><span class="sxs-lookup"><span data-stu-id="d5889-133">The first part of the URL determines the controller class to execute.</span></span> <span data-ttu-id="d5889-134">したがって */HelloWorld*にマップされます、`HelloWorldController`クラス。</span><span class="sxs-lookup"><span data-stu-id="d5889-134">So */HelloWorld* maps to the `HelloWorldController` class.</span></span> <span data-ttu-id="d5889-135">URL の 2 番目の部分は、実行するには、クラスのアクション メソッドを決定します。</span><span class="sxs-lookup"><span data-stu-id="d5889-135">The second part of the URL determines the action method on the class to execute.</span></span> <span data-ttu-id="d5889-136">したがって */HelloWorld/インデックス*なる、`Index`のメソッド、`HelloWorldController`を実行するクラス。</span><span class="sxs-lookup"><span data-stu-id="d5889-136">So */HelloWorld/Index* would cause the `Index` method of the `HelloWorldController` class to execute.</span></span> <span data-ttu-id="d5889-137">のみを参照しなければならなかったこと */HelloWorld*と`Index`メソッドが既定で使用されました。</span><span class="sxs-lookup"><span data-stu-id="d5889-137">Notice that we only had to browse to */HelloWorld* and the `Index` method was used by default.</span></span> <span data-ttu-id="d5889-138">これは、という名前のメソッドであるため`Index`いずれかが明示的に指定されていない場合、コント ローラーで呼び出される既定のメソッドします。</span><span class="sxs-lookup"><span data-stu-id="d5889-138">This is because a method named `Index` is the default method that will be called on a controller if one is not explicitly specified.</span></span>

<span data-ttu-id="d5889-139">`http://localhost:xxxx/HelloWorld/Welcome` を参照します。</span><span class="sxs-lookup"><span data-stu-id="d5889-139">Browse to `http://localhost:xxxx/HelloWorld/Welcome`.</span></span> <span data-ttu-id="d5889-140">`Welcome`メソッドを実行し、文字列を返します&quot;へようこそ のアクション メソッドになります.&quot;.</span><span class="sxs-lookup"><span data-stu-id="d5889-140">The `Welcome` method runs and returns the string &quot;This is the Welcome action method...&quot;.</span></span> <span data-ttu-id="d5889-141">既定の MVC のマッピングは`/[Controller]/[ActionName]/[Parameters]`します。</span><span class="sxs-lookup"><span data-stu-id="d5889-141">The default MVC mapping is `/[Controller]/[ActionName]/[Parameters]`.</span></span> <span data-ttu-id="d5889-142">この URL では、コントローラーは `HelloWorld` で、`Welcome` がアクション メソッドです。</span><span class="sxs-lookup"><span data-stu-id="d5889-142">For this URL, the controller is `HelloWorld` and `Welcome` is the action method.</span></span> <span data-ttu-id="d5889-143">URL の `[Parameters]` の部分はまだ使っていません。</span><span class="sxs-lookup"><span data-stu-id="d5889-143">You haven't used the `[Parameters]` part of the URL yet.</span></span>

![](adding-a-controller/_static/image5.png)

<span data-ttu-id="d5889-144">みましょうコント ローラーに、URL からいくつかのパラメーター情報を渡すことができるように、例を少し変更 (たとえば、 */helloworld/welcome でしょうか。 名前 = Scott&amp;numtimes = 4*)。</span><span class="sxs-lookup"><span data-stu-id="d5889-144">Let's modify the example slightly so that you can pass some parameter information from the URL to the controller (for example, */HelloWorld/Welcome?name=Scott&amp;numtimes=4*).</span></span> <span data-ttu-id="d5889-145">変更、`Welcome`メソッドを次に示すように、2 つのパラメーターを含めます。</span><span class="sxs-lookup"><span data-stu-id="d5889-145">Change your `Welcome` method to include two parameters as shown below.</span></span> <span data-ttu-id="d5889-146">コードでは、C# で省略可能なパラメーターの機能を使用していることを示す注、`numTimes`そのパラメーターの値が渡されない場合、1 にパラメーターが既定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d5889-146">Note that the code uses the C# optional-parameter feature to indicate that the `numTimes` parameter should default to 1 if no value is passed for that parameter.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

<span data-ttu-id="d5889-147">アプリケーションを実行し、URL の例を参照 (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`します。</span><span class="sxs-lookup"><span data-stu-id="d5889-147">Run your application and browse to the example URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`.</span></span> <span data-ttu-id="d5889-148">さまざまな値を試みることができます`name`と`numtimes`URL にします。</span><span class="sxs-lookup"><span data-stu-id="d5889-148">You can try different values for `name` and `numtimes` in the URL.</span></span> <span data-ttu-id="d5889-149">[ASP.NET MVC モデル バインド システム](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)メソッドのパラメーターに、アドレス バーにクエリ文字列から名前付きパラメーターを自動的にマップされます。</span><span class="sxs-lookup"><span data-stu-id="d5889-149">The [ASP.NET MVC model binding system](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatically maps the named parameters from the query string in the address bar to parameters in your method.</span></span>

![](adding-a-controller/_static/image6.png)

<span data-ttu-id="d5889-150">これら両方の例で、コント ローラーが実行されて、 &quot;VC&quot;の MVC 部分: ビューとコント ローラーの作業は、します。</span><span class="sxs-lookup"><span data-stu-id="d5889-150">In both these examples the controller has been doing the &quot;VC&quot; portion of MVC — that is, the view and controller work.</span></span> <span data-ttu-id="d5889-151">コント ローラーは、HTML を直接返しています。</span><span class="sxs-lookup"><span data-stu-id="d5889-151">The controller is returning HTML directly.</span></span> <span data-ttu-id="d5889-152">通常のコードに非常に面倒になるため、直接 HTML を返すコント ローラーは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="d5889-152">Ordinarily you don't want controllers returning HTML directly, since that becomes very cumbersome to code.</span></span> <span data-ttu-id="d5889-153">代わりに、HTML 応答を生成するために使用します別のビュー テンプレート ファイルを通常。</span><span class="sxs-lookup"><span data-stu-id="d5889-153">Instead we'll typically use a separate view template file to help generate the HTML response.</span></span> <span data-ttu-id="d5889-154">そのため方法で [次へ] を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="d5889-154">Let's look next at how we can do this.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d5889-155">[前へ](intro-to-aspnet-mvc-4.md)
> [次へ](adding-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="d5889-155">[Previous](intro-to-aspnet-mvc-4.md)
[Next](adding-a-view.md)</span></span>
