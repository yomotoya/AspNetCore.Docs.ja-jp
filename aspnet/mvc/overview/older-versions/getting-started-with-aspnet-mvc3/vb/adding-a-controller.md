---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
title: コント ローラー (VB) の追加 |Microsoft ドキュメント
author: Rick-Anderson
description: このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 741259e1-54ac-4f71-b4e8-2bd5560bb950
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 9a433083c31c7929f7599e52800c887f301d7727
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "30870610"
---
<a name="adding-a-controller-vb"></a><span data-ttu-id="a3d03-103">コント ローラー (VB) の追加</span><span class="sxs-lookup"><span data-stu-id="a3d03-103">Adding a Controller (VB)</span></span>
====================
<span data-ttu-id="a3d03-104">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="a3d03-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="a3d03-105">このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これは、無料版の Microsoft Visual Studio を使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。</span><span class="sxs-lookup"><span data-stu-id="a3d03-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="a3d03-106">開始する前に、以下に示す前提条件がインストールされていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="a3d03-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="a3d03-107">次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)です。</span><span class="sxs-lookup"><span data-stu-id="a3d03-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="a3d03-108">また、次のリンクを使用して、前提条件を個別にインストールできます。</span><span class="sxs-lookup"><span data-stu-id="a3d03-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="a3d03-109">Visual Studio Web Developer Express SP1 の前提条件</span><span class="sxs-lookup"><span data-stu-id="a3d03-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="a3d03-110">ASP.NET MVC 3 Tools Update します。</span><span class="sxs-lookup"><span data-stu-id="a3d03-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="a3d03-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)</span><span class="sxs-lookup"><span data-stu-id="a3d03-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="a3d03-112">Visual Web Developer 2010 ではなく Visual Studio 2010 を使用している場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)です。</span><span class="sxs-lookup"><span data-stu-id="a3d03-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="a3d03-113">VB.NET のソース コードと Visual Web Developer プロジェクトは、このトピックを使用できます。</span><span class="sxs-lookup"><span data-stu-id="a3d03-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="a3d03-114">[バージョンをダウンロードして VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)です。</span><span class="sxs-lookup"><span data-stu-id="a3d03-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="a3d03-115">C# を使用する場合に切り替え、 [c# バージョン](../cs/adding-a-controller.md)このチュートリアルのです。</span><span class="sxs-lookup"><span data-stu-id="a3d03-115">If you prefer C#, switch to the [C# version](../cs/adding-a-controller.md) of this tutorial.</span></span>


<span data-ttu-id="a3d03-116">MVC*モデル-ビュー-コント ローラー*です。</span><span class="sxs-lookup"><span data-stu-id="a3d03-116">MVC stands for *model-view-controller*.</span></span> <span data-ttu-id="a3d03-117">MVC では、各部分がある個別の責任を持つようにアプリケーションを開発するためのパターンを示します。</span><span class="sxs-lookup"><span data-stu-id="a3d03-117">MVC is a pattern for developing applications such that each part has a separate responsibility:</span></span>

- <span data-ttu-id="a3d03-118">モデル: アプリケーションのデータ。</span><span class="sxs-lookup"><span data-stu-id="a3d03-118">Model: The data for your application.</span></span>
- <span data-ttu-id="a3d03-119">ビュー: テンプレート ファイル、アプリケーションが HTML 応答を動的に生成に使用します。</span><span class="sxs-lookup"><span data-stu-id="a3d03-119">Views: The template files your application will use to dynamically generate HTML responses.</span></span>
- <span data-ttu-id="a3d03-120">コント ローラー: アプリケーションへの着信 URL 要求を処理するクラスは、モデル、データを取得し、クライアントへの応答をレンダリングするテンプレートの表示を指定します。</span><span class="sxs-lookup"><span data-stu-id="a3d03-120">Controllers: Classes that handle incoming URL requests to the application, retrieve model data, and then specify view templates that render a response to the client.</span></span>

<span data-ttu-id="a3d03-121">このチュートリアルでは、これらすべての概念をカバーして行いますを使用してアプリケーションを構築する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="a3d03-121">We'll be covering all these concepts in this tutorial and show you how to use them to build an application.</span></span>

<span data-ttu-id="a3d03-122">右クリックして、新しいコント ローラーを作成、*コント ローラー*フォルダー**ソリューション エクスプ ローラー**を選択し**コント ローラーの追加**です。</span><span class="sxs-lookup"><span data-stu-id="a3d03-122">Create a new controller by right-clicking the *Controllers* folder in **Solution Explorer** and then selecting **Add Controller**.</span></span>

<span data-ttu-id="a3d03-123">[![AddController](adding-a-controller/_static/image2.png "AddController")](adding-a-controller/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a3d03-123">[![AddController](adding-a-controller/_static/image2.png "AddController")](adding-a-controller/_static/image1.png)</span></span>

<span data-ttu-id="a3d03-124">新しいコント ローラーの名前を付けます&quot;HelloWorldController&quot;  をクリック**追加**です。</span><span class="sxs-lookup"><span data-stu-id="a3d03-124">Name your new controller &quot;HelloWorldController&quot; and click **Add**.</span></span>

<span data-ttu-id="a3d03-125">[![2AddEmptyController](adding-a-controller/_static/image4.png "2AddEmptyController")](adding-a-controller/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="a3d03-125">[![2AddEmptyController](adding-a-controller/_static/image4.png "2AddEmptyController")](adding-a-controller/_static/image3.png)</span></span>

<span data-ttu-id="a3d03-126">**ソリューション エクスプ ローラー** 、右側の新しいファイルが作成されている名前付き*HelloWorldController.cs*ファイルが IDE で開かれているとします。</span><span class="sxs-lookup"><span data-stu-id="a3d03-126">Notice in **Solution Explorer** on the right that a new file has been created for you named *HelloWorldController.cs* and that the file is open in the IDE.</span></span>

<span data-ttu-id="a3d03-127">新しい内部`public class HelloWorldController`ブロックは、次のコードのような 2 つの新しいメソッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="a3d03-127">Inside the new `public class HelloWorldController` block, create two new methods that look like the following code.</span></span> <span data-ttu-id="a3d03-128">例として、コント ローラーから直接 HTML の文字列を返します。</span><span class="sxs-lookup"><span data-stu-id="a3d03-128">We'll return a string of HTML directly from the controller as an example.</span></span>

[!code-vb[Main](adding-a-controller/samples/sample1.vb)]

<span data-ttu-id="a3d03-129">コント ローラーの名前は`HelloWorldController`および新しい方法が名前付き`Index`します。</span><span class="sxs-lookup"><span data-stu-id="a3d03-129">Your controller is named `HelloWorldController` and your new method is named `Index`.</span></span> <span data-ttu-id="a3d03-130">(F5 キーまたは ctrl キーを押しながら F5 キーを押して) アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="a3d03-130">Run the application (press F5 or Ctrl+F5).</span></span> <span data-ttu-id="a3d03-131">お使いのブラウザー開始されると、追加&quot;HelloWorld&quot;アドレス バーにパスにします。</span><span class="sxs-lookup"><span data-stu-id="a3d03-131">Once your browser has started up, append &quot;HelloWorld&quot; to the path in the address bar.</span></span> <span data-ttu-id="a3d03-132">(私のコンピューターでが`http://localhost:43246/HelloWorld`) お使いのブラウザーは次のスクリーン ショットのようになります。</span><span class="sxs-lookup"><span data-stu-id="a3d03-132">(On my computer, it's `http://localhost:43246/HelloWorld`) Your browser will look like the screenshot below.</span></span> <span data-ttu-id="a3d03-133">上記のメソッドで返される、コード、文字列直接です。</span><span class="sxs-lookup"><span data-stu-id="a3d03-133">In the method above, the code returned a string directly.</span></span> <span data-ttu-id="a3d03-134">だけの HTML を返すシステムと言いましたと同じです。</span><span class="sxs-lookup"><span data-stu-id="a3d03-134">We told the system to just return some HTML, and it did!</span></span>

![](adding-a-controller/_static/image5.png)

<span data-ttu-id="a3d03-135">ASP.NET MVC では、着信 URL に応じて別のコント ローラー クラス (およびそれらに含まれるさまざまなアクション メソッド) を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="a3d03-135">ASP.NET MVC invokes different controller classes (and different action methods within them) depending on the incoming URL.</span></span> <span data-ttu-id="a3d03-136">ASP.NET MVC で使用される既定のマッピングのロジックでは、次のような形式を使用して、どのようなコードが呼び出されるかを制御します。</span><span class="sxs-lookup"><span data-stu-id="a3d03-136">The default mapping logic used by ASP.NET MVC uses a format like this to control what code is invoked:</span></span>

`/[Controller]/[ActionName]/[Parameters]`

<span data-ttu-id="a3d03-137">URL の最初の部分を実行するコント ローラー クラスを決定します。</span><span class="sxs-lookup"><span data-stu-id="a3d03-137">The first part of the URL determines the controller class to execute.</span></span> <span data-ttu-id="a3d03-138">したがって */HelloWorld*にマップ、`HelloWorldController`クラスです。</span><span class="sxs-lookup"><span data-stu-id="a3d03-138">So */HelloWorld* maps to the `HelloWorldController` class.</span></span> <span data-ttu-id="a3d03-139">URL の 2 番目の部分では、実行するには、クラスにアクション メソッドを判断します。</span><span class="sxs-lookup"><span data-stu-id="a3d03-139">The second part of the URL determines the action method on the class to execute.</span></span> <span data-ttu-id="a3d03-140">したがって*HelloWorld/インデックス*が切り替わるところ、`Index`のメソッド、`HelloWorldController`を実行するクラス。</span><span class="sxs-lookup"><span data-stu-id="a3d03-140">So */HelloWorld/Index* would cause the `Index` method of the `HelloWorldController` class to execute.</span></span> <span data-ttu-id="a3d03-141">のみを参照してくださいしなければならなかったことを確認 */HelloWorld*上と`Index`メソッドは、既定で使用されました。</span><span class="sxs-lookup"><span data-stu-id="a3d03-141">Notice that we only had to visit */HelloWorld* above and the `Index` method was used by default.</span></span> <span data-ttu-id="a3d03-142">これは、という名前のメソッドであるため`Index`いずれかが明示的に指定されていない場合に、コント ローラーで呼び出される既定の方法です。</span><span class="sxs-lookup"><span data-stu-id="a3d03-142">This is because a method named `Index` is the default method that will be called on a controller if one is not explicitly specified.</span></span>

<span data-ttu-id="a3d03-143">`http://localhost:xxxx/HelloWorld/Welcome` を参照します。</span><span class="sxs-lookup"><span data-stu-id="a3d03-143">Browse to `http://localhost:xxxx/HelloWorld/Welcome`.</span></span> <span data-ttu-id="a3d03-144">`Welcome`メソッドが実行され、文字列を返します&quot;へようこそ のアクション メソッドは、このしています.&quot;.</span><span class="sxs-lookup"><span data-stu-id="a3d03-144">The `Welcome` method runs and returns the string &quot;This is the Welcome action method...&quot;.</span></span> <span data-ttu-id="a3d03-145">既定の MVC マッピングは`/[Controller]/[ActionName]/[Parameters]`します。</span><span class="sxs-lookup"><span data-stu-id="a3d03-145">The default MVC mapping is `/[Controller]/[ActionName]/[Parameters]`.</span></span> <span data-ttu-id="a3d03-146">コント ローラーは、この URL の`HelloWorld`と`Welcome`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="a3d03-146">For this URL, the controller is `HelloWorld` and `Welcome` is the method.</span></span> <span data-ttu-id="a3d03-147">使用していない、`[Parameters]`まだ URL の一部です。</span><span class="sxs-lookup"><span data-stu-id="a3d03-147">We haven't used the `[Parameters]` part of the URL yet.</span></span>

![](adding-a-controller/_static/image6.png)

<span data-ttu-id="a3d03-148">みましょうコント ローラーへの URL からの一部のパラメーター情報を渡すおができるように、例を少し変更 (たとえば、 *HelloWorld/ウェルカムしますか? 名前 Scott を =&amp;numtimes = 4*)。</span><span class="sxs-lookup"><span data-stu-id="a3d03-148">Let's modify the example slightly so that we can pass some parameter information in from the URL to the controller (for example, */HelloWorld/Welcome?name=Scott&amp;numtimes=4*).</span></span> <span data-ttu-id="a3d03-149">変更、`Welcome`メソッドを次に示すように、2 つのパラメーターを含めます。</span><span class="sxs-lookup"><span data-stu-id="a3d03-149">Change your `Welcome` method to include two parameters as shown below.</span></span> <span data-ttu-id="a3d03-150">示す、VB 省略可能なパラメーターの機能を使い切ってしまったことに注意してください、`numTimes`パラメーターが既定として設定を 1 にそのパラメーターの値が渡されない場合。</span><span class="sxs-lookup"><span data-stu-id="a3d03-150">Note that we've used the VB optional parameter feature to indicate that the `numTimes` parameter should default to 1 if no value is passed for that parameter.</span></span>

[!code-vb[Main](adding-a-controller/samples/sample2.vb)]

<span data-ttu-id="a3d03-151">アプリケーションを実行しを参照`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`**です。**</span><span class="sxs-lookup"><span data-stu-id="a3d03-151">Run your application and browse to `http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`**.**</span></span> <span data-ttu-id="a3d03-152">値が異なるを試みることができます`name`と`numtimes`です。</span><span class="sxs-lookup"><span data-stu-id="a3d03-152">You can try different values for `name` and `numtimes`.</span></span> <span data-ttu-id="a3d03-153">システムは、メソッドのパラメーターに、アドレス バーに、クエリ文字列から名前付きパラメーターを自動的にマップします。</span><span class="sxs-lookup"><span data-stu-id="a3d03-153">The system automatically maps the named parameters from your query string in the address bar to parameters in your method.</span></span>

![](adding-a-controller/_static/image7.png)

<span data-ttu-id="a3d03-154">これら両方の例で、コント ローラーが実行されて MVC の VC 部分: ビューとコント ローラーの作業です。</span><span class="sxs-lookup"><span data-stu-id="a3d03-154">In both these examples the controller has been doing the VC portion of MVC — that is the view and controller work.</span></span> <span data-ttu-id="a3d03-155">コント ローラーは、直接 HTML を返しています。</span><span class="sxs-lookup"><span data-stu-id="a3d03-155">The controller is returning HTML directly.</span></span> <span data-ttu-id="a3d03-156">通常はコント ローラーのコードに非常に複雑になるので、直接、HTML を返すしたくありません。</span><span class="sxs-lookup"><span data-stu-id="a3d03-156">Ordinarily we don't want controllers returning HTML directly, since that becomes very cumbersome to code.</span></span> <span data-ttu-id="a3d03-157">代わりに HTML 応答を生成するために使用されます、個別のビュー テンプレート ファイル通常。</span><span class="sxs-lookup"><span data-stu-id="a3d03-157">Instead we'll typically use a separate view template file to help generate the HTML response.</span></span> <span data-ttu-id="a3d03-158">結果がどのようにこれを見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="a3d03-158">Let's look at how we can do this.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a3d03-159">[前へ](intro-to-aspnet-mvc-3.md)
> [次へ](adding-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="a3d03-159">[Previous](intro-to-aspnet-mvc-3.md)
[Next](adding-a-view.md)</span></span>
