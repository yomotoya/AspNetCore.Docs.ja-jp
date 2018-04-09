---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
title: コント ローラー (c#) の追加 |Microsoft ドキュメント
author: Rick-Anderson
description: このチュートリアルでは、Microsoft Visual Web Developer 2010 Express サービス パック 1、どの i を使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 0b8c56b5-fdf3-42dd-a866-98fbe0ab78a0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 963d3bbbadf408d7045c50bfd693069e4097d45d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-controller-c"></a><span data-ttu-id="9c074-103">コント ローラー (c#) の追加</span><span class="sxs-lookup"><span data-stu-id="9c074-103">Adding a Controller (C#)</span></span>
====================
<span data-ttu-id="9c074-104">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="9c074-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="9c074-105">このチュートリアルの更新バージョンが利用可能な[ここ](../../../getting-started/introduction/getting-started.md)ASP.NET MVC 5 と Visual Studio 2013 を使用します。</span><span class="sxs-lookup"><span data-stu-id="9c074-105">An updated version of this tutorial is available [here](../../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="9c074-106">より安全な非常に簡単に従うしより多くの機能を示します。</span><span class="sxs-lookup"><span data-stu-id="9c074-106">It's more secure, much simpler to follow and demonstrates more features.</span></span>
> 
> 
> <span data-ttu-id="9c074-107">このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これは、無料版の Microsoft Visual Studio を使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。</span><span class="sxs-lookup"><span data-stu-id="9c074-107">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="9c074-108">開始する前に、以下に示す前提条件がインストールされていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="9c074-108">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="9c074-109">次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)です。</span><span class="sxs-lookup"><span data-stu-id="9c074-109">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="9c074-110">また、次のリンクを使用して、前提条件を個別にインストールできます。</span><span class="sxs-lookup"><span data-stu-id="9c074-110">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="9c074-111">Visual Studio Web Developer Express SP1 の前提条件</span><span class="sxs-lookup"><span data-stu-id="9c074-111">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="9c074-112">ASP.NET MVC 3 Tools Update します。</span><span class="sxs-lookup"><span data-stu-id="9c074-112">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="9c074-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)</span><span class="sxs-lookup"><span data-stu-id="9c074-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="9c074-114">Visual Web Developer 2010 ではなく Visual Studio 2010 を使用している場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)です。</span><span class="sxs-lookup"><span data-stu-id="9c074-114">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="9c074-115">C# ソース コードの Visual Web Developer プロジェクトは、このトピックを使用できます。</span><span class="sxs-lookup"><span data-stu-id="9c074-115">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="9c074-116">[C# バージョンをダウンロード](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)です。</span><span class="sxs-lookup"><span data-stu-id="9c074-116">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="9c074-117">Visual Basic を使用する場合に切り替えて、 [Visual Basic バージョン](../vb/intro-to-aspnet-mvc-3.md)このチュートリアルのです。</span><span class="sxs-lookup"><span data-stu-id="9c074-117">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>


<span data-ttu-id="9c074-118">MVC*モデル-ビュー-コント ローラー*です。</span><span class="sxs-lookup"><span data-stu-id="9c074-118">MVC stands for *model-view-controller*.</span></span> <span data-ttu-id="9c074-119">MVC は、適切に設計された、簡単に管理は、アプリケーションの開発パターンです。</span><span class="sxs-lookup"><span data-stu-id="9c074-119">MVC is a pattern for developing applications that are well architected and easy to maintain.</span></span> <span data-ttu-id="9c074-120">MVC ベースのアプリケーションが含まれます。</span><span class="sxs-lookup"><span data-stu-id="9c074-120">MVC-based applications contain:</span></span>

- <span data-ttu-id="9c074-121">コント ローラー: アプリケーションへの着信要求を処理するクラスはモデル データを取得し、クライアントへの応答を返すビュー テンプレートを指定します。</span><span class="sxs-lookup"><span data-stu-id="9c074-121">Controllers: Classes that handle incoming requests to the application, retrieve model data, and then specify view templates that return a response to the client.</span></span>
- <span data-ttu-id="9c074-122">モデルのクラスを表すアプリケーションのデータとそのデータにビジネス ルールを適用する検証ロジックを使用します。</span><span class="sxs-lookup"><span data-stu-id="9c074-122">Models: Classes that represent the data of the application and that use validation logic to enforce business rules for that data.</span></span>
- <span data-ttu-id="9c074-123">アプリケーションが HTML 応答を動的に生成に使用されるテンプレート ファイルのビュー:</span><span class="sxs-lookup"><span data-stu-id="9c074-123">Views: Template files that your application uses to dynamically generate HTML responses.</span></span>

<span data-ttu-id="9c074-124">このチュートリアルの系列でこれらすべての概念を説明して行いますを使用してアプリケーションを構築する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="9c074-124">We'll be covering all these concepts in this tutorial series and show you how to use them to build an application.</span></span>

<span data-ttu-id="9c074-125">コント ローラー クラスを作成してみましょう。</span><span class="sxs-lookup"><span data-stu-id="9c074-125">Let's begin by creating a controller class.</span></span> <span data-ttu-id="9c074-126">**ソリューション エクスプ ローラー**を右クリックし、*コント ローラー*クリックしてフォルダー**コント ローラーの追加**です。</span><span class="sxs-lookup"><span data-stu-id="9c074-126">In **Solution Explorer**, right-click the *Controllers* folder and then select **Add Controller**.</span></span>

[![](adding-a-controller/_static/image2.png)](adding-a-controller/_static/image1.png)

<span data-ttu-id="9c074-127">新しいコント ローラー"HelloWorldController"の名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="9c074-127">Name your new controller "HelloWorldController".</span></span> <span data-ttu-id="9c074-128">として既定のテンプレートのままにして**空のコント ローラー**  をクリック**追加**です。</span><span class="sxs-lookup"><span data-stu-id="9c074-128">Leave the default template as **Empty controller** and click **Add**.</span></span>

<span data-ttu-id="9c074-129">[![AddHelloWorldController](adding-a-controller/_static/image4.png)](adding-a-controller/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="9c074-129">[![AddHelloWorldController](adding-a-controller/_static/image4.png)](adding-a-controller/_static/image3.png)</span></span>

<span data-ttu-id="9c074-130">**ソリューション エクスプ ローラー**新しいファイルが作成されている名前付き*HelloWorldController.cs*です。</span><span class="sxs-lookup"><span data-stu-id="9c074-130">Notice in **Solution Explorer** that a new file has been created named *HelloWorldController.cs*.</span></span> <span data-ttu-id="9c074-131">ファイルは、IDE で開いています。</span><span class="sxs-lookup"><span data-stu-id="9c074-131">The file is open in the IDE.</span></span>

![](adding-a-controller/_static/image5.png)

<span data-ttu-id="9c074-132">内部、`public class HelloWorldController`ブロックは、次のコードのような 2 つのメソッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="9c074-132">Inside the `public class HelloWorldController` block, create two methods that look like the following code.</span></span> <span data-ttu-id="9c074-133">コント ローラーでは、例として、HTML の文字列を返します。</span><span class="sxs-lookup"><span data-stu-id="9c074-133">The controller will return a string of HTML as an example.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

<span data-ttu-id="9c074-134">コント ローラーの名前は`HelloWorldController`で上記の最初のメソッドの名前が`Index`です。</span><span class="sxs-lookup"><span data-stu-id="9c074-134">Your controller is named `HelloWorldController` and the first method above is named `Index`.</span></span> <span data-ttu-id="9c074-135">ブラウザーから呼び出すことしてみましょう。</span><span class="sxs-lookup"><span data-stu-id="9c074-135">Let's invoke it from a browser.</span></span> <span data-ttu-id="9c074-136">(F5 キーまたは ctrl キーを押しながら F5 キーを押して) アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="9c074-136">Run the application (press F5 or Ctrl+F5).</span></span> <span data-ttu-id="9c074-137">ブラウザーでは、アドレス バーにパスに"HelloWorld"を追加します。</span><span class="sxs-lookup"><span data-stu-id="9c074-137">In the browser, append "HelloWorld" to the path in the address bar.</span></span> <span data-ttu-id="9c074-138">(たとえば、以下の図で`http://localhost:43246/HelloWorld.`) ブラウザーでページは次のスクリーン ショットのようになります。</span><span class="sxs-lookup"><span data-stu-id="9c074-138">(For example, in the illustration below, it's `http://localhost:43246/HelloWorld.`) The page in the browser will look like the following screenshot.</span></span> <span data-ttu-id="9c074-139">上記のメソッドで返される、コード、文字列直接です。</span><span class="sxs-lookup"><span data-stu-id="9c074-139">In the method above, the code returned a string directly.</span></span> <span data-ttu-id="9c074-140">システムだけの HTML を返すように通知してと同じです。</span><span class="sxs-lookup"><span data-stu-id="9c074-140">You told the system to just return some HTML, and it did!</span></span>

![](adding-a-controller/_static/image6.png)

<span data-ttu-id="9c074-141">ASP.NET MVC では、着信 URL に応じて別のコント ローラー クラス (およびそれらに含まれるさまざまなアクション メソッド) を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="9c074-141">ASP.NET MVC invokes different controller classes (and different action methods within them) depending on the incoming URL.</span></span> <span data-ttu-id="9c074-142">ASP.NET MVC で使用される既定のマッピングのロジックでは、次のような形式を使用して、呼び出すには、どのようなコードを確認します。</span><span class="sxs-lookup"><span data-stu-id="9c074-142">The default mapping logic used by ASP.NET MVC uses a format like this to determine what code to invoke:</span></span>

`/[Controller]/[ActionName]/[Parameters]`

<span data-ttu-id="9c074-143">URL の最初の部分を実行するコント ローラー クラスを決定します。</span><span class="sxs-lookup"><span data-stu-id="9c074-143">The first part of the URL determines the controller class to execute.</span></span> <span data-ttu-id="9c074-144">したがって*/HelloWorld*にマップ、`HelloWorldController`クラスです。</span><span class="sxs-lookup"><span data-stu-id="9c074-144">So */HelloWorld* maps to the `HelloWorldController` class.</span></span> <span data-ttu-id="9c074-145">URL の 2 番目の部分では、実行するには、クラスにアクション メソッドを判断します。</span><span class="sxs-lookup"><span data-stu-id="9c074-145">The second part of the URL determines the action method on the class to execute.</span></span> <span data-ttu-id="9c074-146">したがって*HelloWorld/インデックス*が切り替わるところ、`Index`のメソッド、`HelloWorldController`を実行するクラス。</span><span class="sxs-lookup"><span data-stu-id="9c074-146">So */HelloWorld/Index* would cause the `Index` method of the `HelloWorldController` class to execute.</span></span> <span data-ttu-id="9c074-147">のみを参照しなければならなかったことを確認*/HelloWorld*と`Index`メソッドは、既定で使用されました。</span><span class="sxs-lookup"><span data-stu-id="9c074-147">Notice that we only had to browse to */HelloWorld* and the `Index` method was used by default.</span></span> <span data-ttu-id="9c074-148">これは、という名前のメソッドであるため`Index`いずれかが明示的に指定されていない場合に、コント ローラーで呼び出される既定の方法です。</span><span class="sxs-lookup"><span data-stu-id="9c074-148">This is because a method named `Index` is the default method that will be called on a controller if one is not explicitly specified.</span></span>

<span data-ttu-id="9c074-149">`http://localhost:xxxx/HelloWorld/Welcome` を参照します。</span><span class="sxs-lookup"><span data-stu-id="9c074-149">Browse to `http://localhost:xxxx/HelloWorld/Welcome`.</span></span> <span data-ttu-id="9c074-150">`Welcome` メソッドが実行され、"This is the Welcome action method..." という文字列を返します。</span><span class="sxs-lookup"><span data-stu-id="9c074-150">The `Welcome` method runs and returns the string "This is the Welcome action method...".</span></span> <span data-ttu-id="9c074-151">既定の MVC マッピングは`/[Controller]/[ActionName]/[Parameters]`します。</span><span class="sxs-lookup"><span data-stu-id="9c074-151">The default MVC mapping is `/[Controller]/[ActionName]/[Parameters]`.</span></span> <span data-ttu-id="9c074-152">この URL では、コントローラーは `HelloWorld` で、`Welcome` がアクション メソッドです。</span><span class="sxs-lookup"><span data-stu-id="9c074-152">For this URL, the controller is `HelloWorld` and `Welcome` is the action method.</span></span> <span data-ttu-id="9c074-153">URL の `[Parameters]` の部分はまだ使っていません。</span><span class="sxs-lookup"><span data-stu-id="9c074-153">You haven't used the `[Parameters]` part of the URL yet.</span></span>

![](adding-a-controller/_static/image7.png)

<span data-ttu-id="9c074-154">みましょうコント ローラーへの URL からいくつかのパラメーター情報を渡すことができるように、この例を少し変更 (たとえば、 *HelloWorld/ウェルカムしますか? 名前 Scott を =&amp;numtimes = 4*)。</span><span class="sxs-lookup"><span data-stu-id="9c074-154">Let's modify the example slightly so that you can pass some parameter information from the URL to the controller (for example, */HelloWorld/Welcome?name=Scott&amp;numtimes=4*).</span></span> <span data-ttu-id="9c074-155">変更、`Welcome`メソッドを次に示すように、2 つのパラメーターを含めます。</span><span class="sxs-lookup"><span data-stu-id="9c074-155">Change your `Welcome` method to include two parameters as shown below.</span></span> <span data-ttu-id="9c074-156">コードでは、C# で省略可能なパラメーターの機能を使用して示す注、`numTimes`パラメーターが既定として設定を 1 にそのパラメーターの値が渡されない場合。</span><span class="sxs-lookup"><span data-stu-id="9c074-156">Note that the code uses the C# optional-parameter feature to indicate that the `numTimes` parameter should default to 1 if no value is passed for that parameter.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

<span data-ttu-id="9c074-157">アプリケーションを実行し、URL の例を参照 (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`です。</span><span class="sxs-lookup"><span data-stu-id="9c074-157">Run your application and browse to the example URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`.</span></span> <span data-ttu-id="9c074-158">値が異なるを試みることができます`name`と`numtimes`URL にします。</span><span class="sxs-lookup"><span data-stu-id="9c074-158">You can try different values for `name` and `numtimes` in the URL.</span></span> <span data-ttu-id="9c074-159">システムは、メソッドのパラメーターに、アドレス バーにクエリ文字列から名前付きパラメーターを自動的にマップします。</span><span class="sxs-lookup"><span data-stu-id="9c074-159">The system automatically maps the named parameters from the query string in the address bar to parameters in your method.</span></span>

![](adding-a-controller/_static/image8.png)

<span data-ttu-id="9c074-160">これら両方の例で、コント ローラーが実行されて、"VC"の部分 MVC: ビューとコント ローラーの作業は、します。</span><span class="sxs-lookup"><span data-stu-id="9c074-160">In both these examples the controller has been doing the "VC" portion of MVC — that is, the view and controller work.</span></span> <span data-ttu-id="9c074-161">コント ローラーは、直接 HTML を返しています。</span><span class="sxs-lookup"><span data-stu-id="9c074-161">The controller is returning HTML directly.</span></span> <span data-ttu-id="9c074-162">通常はコント ローラーのコードに非常に複雑になるので、直接、HTML を返す必要はありません。</span><span class="sxs-lookup"><span data-stu-id="9c074-162">Ordinarily you don't want controllers returning HTML directly, since that becomes very cumbersome to code.</span></span> <span data-ttu-id="9c074-163">代わりに HTML 応答を生成するために使用されます、個別のビュー テンプレート ファイル通常。</span><span class="sxs-lookup"><span data-stu-id="9c074-163">Instead we'll typically use a separate view template file to help generate the HTML response.</span></span> <span data-ttu-id="9c074-164">結果がどのようにこれを次のことを確認してみましょう。</span><span class="sxs-lookup"><span data-stu-id="9c074-164">Let's look next at how we can do this.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9c074-165">[前へ](intro-to-aspnet-mvc-3.md)
> [次へ](adding-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="9c074-165">[Previous](intro-to-aspnet-mvc-3.md)
[Next](adding-a-view.md)</span></span>
