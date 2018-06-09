---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
title: コント ローラーの追加 |Microsoft ドキュメント
author: Rick-Anderson
description: '注: このチュートリアルの最新バージョンはここで ASP.NET MVC 5 と Visual Studio 2013 を使用します。 安全な非常に簡単に従い、デモをお勧めしています.'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 0267d31c-892f-49a1-9e7a-3ae8cc12b2ca
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: bb76c0a87d935322406b9d8e18fbdb3e41f327f5
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "30868868"
---
<a name="adding-a-controller"></a><span data-ttu-id="a311c-104">コントローラーを追加する</span><span class="sxs-lookup"><span data-stu-id="a311c-104">Adding a Controller</span></span>
====================
<span data-ttu-id="a311c-105">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="a311c-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="a311c-106">このチュートリアルの更新バージョンが利用可能な[ここ](../../getting-started/introduction/getting-started.md)ASP.NET MVC 5 と Visual Studio 2013 を使用します。</span><span class="sxs-lookup"><span data-stu-id="a311c-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="a311c-107">より安全な非常に簡単に従うしより多くの機能を示します。</span><span class="sxs-lookup"><span data-stu-id="a311c-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="a311c-108">MVC*モデル-ビュー-コント ローラー*です。</span><span class="sxs-lookup"><span data-stu-id="a311c-108">MVC stands for *model-view-controller*.</span></span> <span data-ttu-id="a311c-109">MVC が適切に設計された、テストが容易な保守し、やすいアプリケーションの開発パターンです。</span><span class="sxs-lookup"><span data-stu-id="a311c-109">MVC is a pattern for developing applications that are well architected, testable and easy to maintain.</span></span> <span data-ttu-id="a311c-110">MVC ベースのアプリケーションが含まれます。</span><span class="sxs-lookup"><span data-stu-id="a311c-110">MVC-based applications contain:</span></span>

- <span data-ttu-id="a311c-111">**M** odels: アプリケーションのデータを表すし、そのデータにビジネス ルールを適用する検証ロジックを使用するクラス。</span><span class="sxs-lookup"><span data-stu-id="a311c-111">**M** odels: Classes that represent the data of the application and that use validation logic to enforce business rules for that data.</span></span>
- <span data-ttu-id="a311c-112">**V** iews: アプリケーションが HTML 応答を動的に生成に使用されるテンプレート ファイル。</span><span class="sxs-lookup"><span data-stu-id="a311c-112">**V** iews: Template files that your application uses to dynamically generate HTML responses.</span></span>
- <span data-ttu-id="a311c-113">**C** ontrollers: ブラウザーの入力方向の要求を処理するクラスがモデルのデータを取得し、ブラウザーへの応答を返すビュー テンプレートを指定します。</span><span class="sxs-lookup"><span data-stu-id="a311c-113">**C** ontrollers: Classes that handle incoming browser requests, retrieve model data, and then specify view templates that return a response to the browser.</span></span>

<span data-ttu-id="a311c-114">このチュートリアルの系列でこれらすべての概念を説明して行いますを使用してアプリケーションを構築する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="a311c-114">We'll be covering all these concepts in this tutorial series and show you how to use them to build an application.</span></span>

<span data-ttu-id="a311c-115">コント ローラー クラスを作成してみましょう。</span><span class="sxs-lookup"><span data-stu-id="a311c-115">Let's begin by creating a controller class.</span></span> <span data-ttu-id="a311c-116">**ソリューション エクスプ ローラー**を右クリックし、*コント ローラー*クリックしてフォルダー**コント ローラーの追加**です。</span><span class="sxs-lookup"><span data-stu-id="a311c-116">In **Solution Explorer**, right-click the *Controllers* folder and then select **Add Controller**.</span></span>

![](adding-a-controller/_static/image1.png)

<span data-ttu-id="a311c-117">新しいコント ローラーの名前を付けます&quot;HelloWorldController&quot;です。</span><span class="sxs-lookup"><span data-stu-id="a311c-117">Name your new controller &quot;HelloWorldController&quot;.</span></span> <span data-ttu-id="a311c-118">として既定のテンプレートのままにして**空の MVC コント ローラー**  をクリック**追加**です。</span><span class="sxs-lookup"><span data-stu-id="a311c-118">Leave the default template as **Empty MVC controller** and click **Add**.</span></span>

![コント ローラーを追加します。](adding-a-controller/_static/image2.png)

<span data-ttu-id="a311c-120">**ソリューション エクスプ ローラー**新しいファイルが作成されている名前付き*HelloWorldController.cs*です。</span><span class="sxs-lookup"><span data-stu-id="a311c-120">Notice in **Solution Explorer** that a new file has been created named *HelloWorldController.cs*.</span></span> <span data-ttu-id="a311c-121">ファイルは、IDE で開いています。</span><span class="sxs-lookup"><span data-stu-id="a311c-121">The file is open in the IDE.</span></span>

![](adding-a-controller/_static/image3.png)

<span data-ttu-id="a311c-122">ファイルの内容を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a311c-122">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

<span data-ttu-id="a311c-123">コント ローラーのメソッドでは、例として、HTML の文字列を返します。</span><span class="sxs-lookup"><span data-stu-id="a311c-123">The controller methods will return a string of HTML as an example.</span></span> <span data-ttu-id="a311c-124">コント ローラーの名前は`HelloWorldController`で上記の最初のメソッドの名前が`Index`です。</span><span class="sxs-lookup"><span data-stu-id="a311c-124">The controller is named `HelloWorldController` and the first method above is named `Index`.</span></span> <span data-ttu-id="a311c-125">ブラウザーから呼び出すことしてみましょう。</span><span class="sxs-lookup"><span data-stu-id="a311c-125">Let's invoke it from a browser.</span></span> <span data-ttu-id="a311c-126">(F5 キーまたは ctrl キーを押しながら F5 キーを押して) アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="a311c-126">Run the application (press F5 or Ctrl+F5).</span></span> <span data-ttu-id="a311c-127">ブラウザーで、追加&quot;HelloWorld&quot;アドレス バーにパスにします。</span><span class="sxs-lookup"><span data-stu-id="a311c-127">In the browser, append &quot;HelloWorld&quot; to the path in the address bar.</span></span> <span data-ttu-id="a311c-128">(たとえば、以下の図で`http://localhost:1234/HelloWorld.`) ブラウザーでページは次のスクリーン ショットのようになります。</span><span class="sxs-lookup"><span data-stu-id="a311c-128">(For example, in the illustration below, it's `http://localhost:1234/HelloWorld.`) The page in the browser will look like the following screenshot.</span></span> <span data-ttu-id="a311c-129">上記のメソッドで返される、コード、文字列直接です。</span><span class="sxs-lookup"><span data-stu-id="a311c-129">In the method above, the code returned a string directly.</span></span> <span data-ttu-id="a311c-130">システムだけの HTML を返すように通知してと同じです。</span><span class="sxs-lookup"><span data-stu-id="a311c-130">You told the system to just return some HTML, and it did!</span></span>

![](adding-a-controller/_static/image4.png)

<span data-ttu-id="a311c-131">ASP.NET MVC では、着信 URL に応じて別のコント ローラー クラス (およびそれらに含まれるさまざまなアクション メソッド) を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="a311c-131">ASP.NET MVC invokes different controller classes (and different action methods within them) depending on the incoming URL.</span></span> <span data-ttu-id="a311c-132">ASP.NET MVC で使用される既定の URL ルーティング ロジックでは、次のような形式を使用して、呼び出すには、どのようなコードを確認します。</span><span class="sxs-lookup"><span data-stu-id="a311c-132">The default URL routing logic used by ASP.NET MVC uses a format like this to determine what code to invoke:</span></span>

`/[Controller]/[ActionName]/[Parameters]`

<span data-ttu-id="a311c-133">URL の最初の部分を実行するコント ローラー クラスを決定します。</span><span class="sxs-lookup"><span data-stu-id="a311c-133">The first part of the URL determines the controller class to execute.</span></span> <span data-ttu-id="a311c-134">したがって */HelloWorld*にマップ、`HelloWorldController`クラスです。</span><span class="sxs-lookup"><span data-stu-id="a311c-134">So */HelloWorld* maps to the `HelloWorldController` class.</span></span> <span data-ttu-id="a311c-135">URL の 2 番目の部分では、実行するには、クラスにアクション メソッドを判断します。</span><span class="sxs-lookup"><span data-stu-id="a311c-135">The second part of the URL determines the action method on the class to execute.</span></span> <span data-ttu-id="a311c-136">したがって*HelloWorld/インデックス*が切り替わるところ、`Index`のメソッド、`HelloWorldController`を実行するクラス。</span><span class="sxs-lookup"><span data-stu-id="a311c-136">So */HelloWorld/Index* would cause the `Index` method of the `HelloWorldController` class to execute.</span></span> <span data-ttu-id="a311c-137">のみを参照しなければならなかったことを確認 */HelloWorld*と`Index`メソッドは、既定で使用されました。</span><span class="sxs-lookup"><span data-stu-id="a311c-137">Notice that we only had to browse to */HelloWorld* and the `Index` method was used by default.</span></span> <span data-ttu-id="a311c-138">これは、という名前のメソッドであるため`Index`いずれかが明示的に指定されていない場合に、コント ローラーで呼び出される既定の方法です。</span><span class="sxs-lookup"><span data-stu-id="a311c-138">This is because a method named `Index` is the default method that will be called on a controller if one is not explicitly specified.</span></span>

<span data-ttu-id="a311c-139">`http://localhost:xxxx/HelloWorld/Welcome` を参照します。</span><span class="sxs-lookup"><span data-stu-id="a311c-139">Browse to `http://localhost:xxxx/HelloWorld/Welcome`.</span></span> <span data-ttu-id="a311c-140">`Welcome`メソッドが実行され、文字列を返します&quot;へようこそ のアクション メソッドは、このしています.&quot;.</span><span class="sxs-lookup"><span data-stu-id="a311c-140">The `Welcome` method runs and returns the string &quot;This is the Welcome action method...&quot;.</span></span> <span data-ttu-id="a311c-141">既定の MVC マッピングは`/[Controller]/[ActionName]/[Parameters]`します。</span><span class="sxs-lookup"><span data-stu-id="a311c-141">The default MVC mapping is `/[Controller]/[ActionName]/[Parameters]`.</span></span> <span data-ttu-id="a311c-142">この URL では、コントローラーは `HelloWorld` で、`Welcome` がアクション メソッドです。</span><span class="sxs-lookup"><span data-stu-id="a311c-142">For this URL, the controller is `HelloWorld` and `Welcome` is the action method.</span></span> <span data-ttu-id="a311c-143">URL の `[Parameters]` の部分はまだ使っていません。</span><span class="sxs-lookup"><span data-stu-id="a311c-143">You haven't used the `[Parameters]` part of the URL yet.</span></span>

![](adding-a-controller/_static/image5.png)

<span data-ttu-id="a311c-144">みましょうコント ローラーへの URL からいくつかのパラメーター情報を渡すことができるように、この例を少し変更 (たとえば、 *HelloWorld/ウェルカムしますか? 名前 Scott を =&amp;numtimes = 4*)。</span><span class="sxs-lookup"><span data-stu-id="a311c-144">Let's modify the example slightly so that you can pass some parameter information from the URL to the controller (for example, */HelloWorld/Welcome?name=Scott&amp;numtimes=4*).</span></span> <span data-ttu-id="a311c-145">変更、`Welcome`メソッドを次に示すように、2 つのパラメーターを含めます。</span><span class="sxs-lookup"><span data-stu-id="a311c-145">Change your `Welcome` method to include two parameters as shown below.</span></span> <span data-ttu-id="a311c-146">コードでは、C# で省略可能なパラメーターの機能を使用して示す注、`numTimes`パラメーターが既定として設定を 1 にそのパラメーターの値が渡されない場合。</span><span class="sxs-lookup"><span data-stu-id="a311c-146">Note that the code uses the C# optional-parameter feature to indicate that the `numTimes` parameter should default to 1 if no value is passed for that parameter.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

<span data-ttu-id="a311c-147">アプリケーションを実行し、URL の例を参照 (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`です。</span><span class="sxs-lookup"><span data-stu-id="a311c-147">Run your application and browse to the example URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`.</span></span> <span data-ttu-id="a311c-148">値が異なるを試みることができます`name`と`numtimes`URL にします。</span><span class="sxs-lookup"><span data-stu-id="a311c-148">You can try different values for `name` and `numtimes` in the URL.</span></span> <span data-ttu-id="a311c-149">[ASP.NET MVC モデル バインド システム](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx)メソッドのパラメーターに、アドレス バーにクエリ文字列から名前付きパラメーターが自動的にマップします。</span><span class="sxs-lookup"><span data-stu-id="a311c-149">The [ASP.NET MVC model binding system](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatically maps the named parameters from the query string in the address bar to parameters in your method.</span></span>

![](adding-a-controller/_static/image6.png)

<span data-ttu-id="a311c-150">これら両方の例で、コント ローラーが実行されて、 &quot;VC&quot; MVC 一部分: ビューとコント ローラーの作業は、します。</span><span class="sxs-lookup"><span data-stu-id="a311c-150">In both these examples the controller has been doing the &quot;VC&quot; portion of MVC — that is, the view and controller work.</span></span> <span data-ttu-id="a311c-151">コント ローラーは、直接 HTML を返しています。</span><span class="sxs-lookup"><span data-stu-id="a311c-151">The controller is returning HTML directly.</span></span> <span data-ttu-id="a311c-152">通常はコント ローラーのコードに非常に複雑になるので、直接、HTML を返す必要はありません。</span><span class="sxs-lookup"><span data-stu-id="a311c-152">Ordinarily you don't want controllers returning HTML directly, since that becomes very cumbersome to code.</span></span> <span data-ttu-id="a311c-153">代わりに HTML 応答を生成するために使用されます、個別のビュー テンプレート ファイル通常。</span><span class="sxs-lookup"><span data-stu-id="a311c-153">Instead we'll typically use a separate view template file to help generate the HTML response.</span></span> <span data-ttu-id="a311c-154">結果がどのようにこれを次のことを確認してみましょう。</span><span class="sxs-lookup"><span data-stu-id="a311c-154">Let's look next at how we can do this.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a311c-155">[前へ](intro-to-aspnet-mvc-4.md)
> [次へ](adding-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="a311c-155">[Previous](intro-to-aspnet-mvc-4.md)
[Next](adding-a-view.md)</span></span>
