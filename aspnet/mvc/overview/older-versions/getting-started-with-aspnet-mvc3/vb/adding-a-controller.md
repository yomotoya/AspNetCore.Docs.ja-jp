---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
title: コント ローラー (VB) の追加 |Microsoft Docs
author: Rick-Anderson
description: このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明しています.
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 741259e1-54ac-4f71-b4e8-2bd5560bb950
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: b38f3c4051af426e471568b8a25a471986f07209
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/04/2018
ms.locfileid: "48576231"
---
<a name="adding-a-controller-vb"></a><span data-ttu-id="22592-103">コント ローラー (VB) の追加</span><span class="sxs-lookup"><span data-stu-id="22592-103">Adding a Controller (VB)</span></span>
====================
<span data-ttu-id="22592-104">によって[Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="22592-104">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

> <span data-ttu-id="22592-105">このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、Microsoft Visual Studio の無料版であるを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。</span><span class="sxs-lookup"><span data-stu-id="22592-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="22592-106">始める前に、以下の前提条件がインストールされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="22592-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="22592-107">次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)します。</span><span class="sxs-lookup"><span data-stu-id="22592-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="22592-108">または、次のリンクを使用して、前提条件を個別にインストールできます。</span><span class="sxs-lookup"><span data-stu-id="22592-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="22592-109">Visual Studio Web Developer Express SP1 の前提条件</span><span class="sxs-lookup"><span data-stu-id="22592-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="22592-110">ASP.NET MVC 3 Tools Update します。</span><span class="sxs-lookup"><span data-stu-id="22592-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="22592-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)</span><span class="sxs-lookup"><span data-stu-id="22592-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="22592-112">Visual Web Developer 2010 ではなく Visual Studio 2010 を使用する場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)します。</span><span class="sxs-lookup"><span data-stu-id="22592-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="22592-113">VB.NET のソース コードでの Visual Web Developer プロジェクトは、このトピックと共に使用できます。</span><span class="sxs-lookup"><span data-stu-id="22592-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="22592-114">[VB.NET のバージョンをダウンロード](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)します。</span><span class="sxs-lookup"><span data-stu-id="22592-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="22592-115">C# を使用する場合に切り替えて、 [c# バージョン](../cs/adding-a-controller.md)このチュートリアルの。</span><span class="sxs-lookup"><span data-stu-id="22592-115">If you prefer C#, switch to the [C# version](../cs/adding-a-controller.md) of this tutorial.</span></span>


<span data-ttu-id="22592-116">MVC の略*モデル-ビュー-コント ローラー*します。</span><span class="sxs-lookup"><span data-stu-id="22592-116">MVC stands for *model-view-controller*.</span></span> <span data-ttu-id="22592-117">MVC は、各部分がある別の責任になるようにアプリケーションを開発するためのパターンです。</span><span class="sxs-lookup"><span data-stu-id="22592-117">MVC is a pattern for developing applications such that each part has a separate responsibility:</span></span>

- <span data-ttu-id="22592-118">モデル: アプリケーションのデータ。</span><span class="sxs-lookup"><span data-stu-id="22592-118">Model: The data for your application.</span></span>
- <span data-ttu-id="22592-119">ビュー: 動的 HTML 応答を生成する、アプリケーションが使用されるテンプレート ファイル。</span><span class="sxs-lookup"><span data-stu-id="22592-119">Views: The template files your application will use to dynamically generate HTML responses.</span></span>
- <span data-ttu-id="22592-120">コント ローラー: アプリケーションへの受信 URL 要求を処理するクラスは、モデル、データを取得し、クライアントへの応答をレンダリングするビュー テンプレートを指定します。</span><span class="sxs-lookup"><span data-stu-id="22592-120">Controllers: Classes that handle incoming URL requests to the application, retrieve model data, and then specify view templates that render a response to the client.</span></span>

<span data-ttu-id="22592-121">このチュートリアルでは、これらすべての概念をカバーするがされ、アプリケーションの構築に使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="22592-121">We'll be covering all these concepts in this tutorial and show you how to use them to build an application.</span></span>

<span data-ttu-id="22592-122">右クリックし、新しいコント ローラーを作成、*コント ローラー*フォルダー**ソリューション エクスプ ローラー**選択**コント ローラーの追加**します。</span><span class="sxs-lookup"><span data-stu-id="22592-122">Create a new controller by right-clicking the *Controllers* folder in **Solution Explorer** and then selecting **Add Controller**.</span></span>

<span data-ttu-id="22592-123">[![AddController](adding-a-controller/_static/image2.png "AddController")](adding-a-controller/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="22592-123">[![AddController](adding-a-controller/_static/image2.png "AddController")](adding-a-controller/_static/image1.png)</span></span>

<span data-ttu-id="22592-124">新しいコント ローラー &quot;HelloWorldController&quot;クリック**追加**します。</span><span class="sxs-lookup"><span data-stu-id="22592-124">Name your new controller &quot;HelloWorldController&quot; and click **Add**.</span></span>

<span data-ttu-id="22592-125">[![2AddEmptyController](adding-a-controller/_static/image4.png "2AddEmptyController")](adding-a-controller/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="22592-125">[![2AddEmptyController](adding-a-controller/_static/image4.png "2AddEmptyController")](adding-a-controller/_static/image3.png)</span></span>

<span data-ttu-id="22592-126">**ソリューション エクスプ ローラー** 、右側の新しいファイルが作成されたことという名前*HelloWorldController.cs*ファイルが IDE で開かれているとします。</span><span class="sxs-lookup"><span data-stu-id="22592-126">Notice in **Solution Explorer** on the right that a new file has been created for you named *HelloWorldController.cs* and that the file is open in the IDE.</span></span>

<span data-ttu-id="22592-127">新しい内部`public class HelloWorldController`ブロックで、次のコードのように 2 つの新しいメソッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="22592-127">Inside the new `public class HelloWorldController` block, create two new methods that look like the following code.</span></span> <span data-ttu-id="22592-128">例として、コント ローラーから直接 HTML の文字列を返しますします。</span><span class="sxs-lookup"><span data-stu-id="22592-128">We'll return a string of HTML directly from the controller as an example.</span></span>

[!code-vb[Main](adding-a-controller/samples/sample1.vb)]

<span data-ttu-id="22592-129">コント ローラーの名前は`HelloWorldController`と新しいメソッドが名前付き`Index`します。</span><span class="sxs-lookup"><span data-stu-id="22592-129">Your controller is named `HelloWorldController` and your new method is named `Index`.</span></span> <span data-ttu-id="22592-130">(F5 キーまたは ctrl キーを押しながら f5 キーを押して) アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="22592-130">Run the application (press F5 or Ctrl+F5).</span></span> <span data-ttu-id="22592-131">お使いのブラウザーが開始されると追加&quot;HelloWorld&quot;アドレス バーにパスにします。</span><span class="sxs-lookup"><span data-stu-id="22592-131">Once your browser has started up, append &quot;HelloWorld&quot; to the path in the address bar.</span></span> <span data-ttu-id="22592-132">(私のコンピューターでが`http://localhost:43246/HelloWorld`)、ブラウザーは次のスクリーン ショットのようになります。</span><span class="sxs-lookup"><span data-stu-id="22592-132">(On my computer, it's `http://localhost:43246/HelloWorld`) Your browser will look like the screenshot below.</span></span> <span data-ttu-id="22592-133">上記のメソッドでは、コードは、文字列を直接返します。</span><span class="sxs-lookup"><span data-stu-id="22592-133">In the method above, the code returned a string directly.</span></span> <span data-ttu-id="22592-134">いくつかの HTML を返すだけに、システムと言いましたおり、そうしました。</span><span class="sxs-lookup"><span data-stu-id="22592-134">We told the system to just return some HTML, and it did!</span></span>

![](adding-a-controller/_static/image5.png)

<span data-ttu-id="22592-135">ASP.NET MVC では、着信 URL に応じて別のコント ローラー クラス (およびそれらに含まれるさまざまなアクション メソッド) を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="22592-135">ASP.NET MVC invokes different controller classes (and different action methods within them) depending on the incoming URL.</span></span> <span data-ttu-id="22592-136">ASP.NET MVC で使用される既定のマッピングのロジックでは、このような形式を使用して、どのようなコードが呼び出されるかを制御します。</span><span class="sxs-lookup"><span data-stu-id="22592-136">The default mapping logic used by ASP.NET MVC uses a format like this to control what code is invoked:</span></span>

`/[Controller]/[ActionName]/[Parameters]`

<span data-ttu-id="22592-137">URL の最初の部分を実行するコント ローラー クラスを決定します。</span><span class="sxs-lookup"><span data-stu-id="22592-137">The first part of the URL determines the controller class to execute.</span></span> <span data-ttu-id="22592-138">したがって */HelloWorld*にマップされます、`HelloWorldController`クラス。</span><span class="sxs-lookup"><span data-stu-id="22592-138">So */HelloWorld* maps to the `HelloWorldController` class.</span></span> <span data-ttu-id="22592-139">URL の 2 番目の部分は、実行するには、クラスのアクション メソッドを決定します。</span><span class="sxs-lookup"><span data-stu-id="22592-139">The second part of the URL determines the action method on the class to execute.</span></span> <span data-ttu-id="22592-140">したがって */HelloWorld/インデックス*なる、`Index`のメソッド、`HelloWorldController`を実行するクラス。</span><span class="sxs-lookup"><span data-stu-id="22592-140">So */HelloWorld/Index* would cause the `Index` method of the `HelloWorldController` class to execute.</span></span> <span data-ttu-id="22592-141">のみを参照してくださいあったことに注意してください */HelloWorld*上、`Index`メソッドが既定で使用されました。</span><span class="sxs-lookup"><span data-stu-id="22592-141">Notice that we only had to visit */HelloWorld* above and the `Index` method was used by default.</span></span> <span data-ttu-id="22592-142">これは、という名前のメソッドであるため`Index`いずれかが明示的に指定されていない場合、コント ローラーで呼び出される既定のメソッドします。</span><span class="sxs-lookup"><span data-stu-id="22592-142">This is because a method named `Index` is the default method that will be called on a controller if one is not explicitly specified.</span></span>

<span data-ttu-id="22592-143">`http://localhost:xxxx/HelloWorld/Welcome` を参照します。</span><span class="sxs-lookup"><span data-stu-id="22592-143">Browse to `http://localhost:xxxx/HelloWorld/Welcome`.</span></span> <span data-ttu-id="22592-144">`Welcome`メソッドを実行し、文字列を返します&quot;へようこそ のアクション メソッドになります.&quot;.</span><span class="sxs-lookup"><span data-stu-id="22592-144">The `Welcome` method runs and returns the string &quot;This is the Welcome action method...&quot;.</span></span> <span data-ttu-id="22592-145">既定の MVC のマッピングは`/[Controller]/[ActionName]/[Parameters]`します。</span><span class="sxs-lookup"><span data-stu-id="22592-145">The default MVC mapping is `/[Controller]/[ActionName]/[Parameters]`.</span></span> <span data-ttu-id="22592-146">コント ローラーは、この url で`HelloWorld`と`Welcome`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="22592-146">For this URL, the controller is `HelloWorld` and `Welcome` is the method.</span></span> <span data-ttu-id="22592-147">私たちが使用されていない、`[Parameters]`まだ URL の一部です。</span><span class="sxs-lookup"><span data-stu-id="22592-147">We haven't used the `[Parameters]` part of the URL yet.</span></span>

![](adding-a-controller/_static/image6.png)

<span data-ttu-id="22592-148">みましょうコント ローラーに、URL からの一部のパラメーター情報を渡すことができるように、例を少し変更 (たとえば、 */helloworld/welcome でしょうか。 名前 = Scott&amp;numtimes = 4*)。</span><span class="sxs-lookup"><span data-stu-id="22592-148">Let's modify the example slightly so that we can pass some parameter information in from the URL to the controller (for example, */HelloWorld/Welcome?name=Scott&amp;numtimes=4*).</span></span> <span data-ttu-id="22592-149">変更、`Welcome`メソッドを次に示すように、2 つのパラメーターを含めます。</span><span class="sxs-lookup"><span data-stu-id="22592-149">Change your `Welcome` method to include two parameters as shown below.</span></span> <span data-ttu-id="22592-150">いることを示す、VB の省略可能なパラメーターの機能を使用したことに注意してください、`numTimes`そのパラメーターの値が渡されない場合、1 にパラメーターが既定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="22592-150">Note that we've used the VB optional parameter feature to indicate that the `numTimes` parameter should default to 1 if no value is passed for that parameter.</span></span>

[!code-vb[Main](adding-a-controller/samples/sample2.vb)]

<span data-ttu-id="22592-151">アプリケーションを実行しを参照する`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`**します。**</span><span class="sxs-lookup"><span data-stu-id="22592-151">Run your application and browse to `http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`**.**</span></span> <span data-ttu-id="22592-152">さまざまな値を試みることができます`name`と`numtimes`します。</span><span class="sxs-lookup"><span data-stu-id="22592-152">You can try different values for `name` and `numtimes`.</span></span> <span data-ttu-id="22592-153">システムでは、メソッドのパラメーターに、アドレス バーで、クエリ文字列から名前付きパラメーターが自動的にマップされます。</span><span class="sxs-lookup"><span data-stu-id="22592-153">The system automatically maps the named parameters from your query string in the address bar to parameters in your method.</span></span>

![](adding-a-controller/_static/image7.png)

<span data-ttu-id="22592-154">これら両方の例で、コント ローラーが実行されて MVC の VC 部分: ビューとコント ローラーの作業です。</span><span class="sxs-lookup"><span data-stu-id="22592-154">In both these examples the controller has been doing the VC portion of MVC — that is the view and controller work.</span></span> <span data-ttu-id="22592-155">コント ローラーは、HTML を直接返しています。</span><span class="sxs-lookup"><span data-stu-id="22592-155">The controller is returning HTML directly.</span></span> <span data-ttu-id="22592-156">通常のコードに非常に面倒になるため、直接 HTML を返すコント ローラーのたくはありません。</span><span class="sxs-lookup"><span data-stu-id="22592-156">Ordinarily we don't want controllers returning HTML directly, since that becomes very cumbersome to code.</span></span> <span data-ttu-id="22592-157">代わりに、HTML 応答を生成するために使用します別のビュー テンプレート ファイルを通常。</span><span class="sxs-lookup"><span data-stu-id="22592-157">Instead we'll typically use a separate view template file to help generate the HTML response.</span></span> <span data-ttu-id="22592-158">そのため方法を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="22592-158">Let's look at how we can do this.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="22592-159">[前へ](intro-to-aspnet-mvc-3.md)
> [次へ](adding-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="22592-159">[Previous](intro-to-aspnet-mvc-3.md)
[Next](adding-a-view.md)</span></span>
