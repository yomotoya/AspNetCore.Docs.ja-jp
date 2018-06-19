---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: プログラミングの ASP.NET Web Pages (Razor) を使用してクトリ |Microsoft ドキュメント
author: tfitzmac
description: この付録では、Razor 構文を使用するプログラミング ASP.NET Web Pages をな Visual Studio 2010 または Visual Web Developer 2010 Express を使用する方法について説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2014
ms.topic: article
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: eb17c8cc1fab5b552c8495e74bb86ae9dbc5b972
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30896584"
---
<a name="programming-aspnet-web-pages-razor-using-visual-studio"></a><span data-ttu-id="208b9-103">Visual Studio を使用してプログラミングする ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="208b9-103">Programming ASP.NET Web Pages (Razor) Using Visual Studio</span></span>
====================
<span data-ttu-id="208b9-104">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="208b9-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="208b9-105">この記事では、Visual Studio または Visual Web Developer Express を web サイトの ASP.NET Web Pages (Razor) のプログラムに使用する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="208b9-105">This article explains how you can use Visual Studio or Visual Web Developer Express to program ASP.NET Web Pages (Razor) websites.</span></span>
> 
> <span data-ttu-id="208b9-106">学習する内容</span><span class="sxs-lookup"><span data-stu-id="208b9-106">What you'll learn</span></span>
> 
> - <span data-ttu-id="208b9-107">Visual Studio のバージョンの ASP.NET Web Pages を使用する (ある場合) をインストールするために必要とします。</span><span class="sxs-lookup"><span data-stu-id="208b9-107">What you need to install (if anything) to work with ASP.NET Web Pages in your version of Visual Studio.</span></span>
> - <span data-ttu-id="208b9-108">Visual Web Developer 2010 Express を ASP.NET Web Pages のサポートを追加する方法です。</span><span class="sxs-lookup"><span data-stu-id="208b9-108">How to add support for ASP.NET Web Pages to Visual Web Developer 2010 Express.</span></span>
> - <span data-ttu-id="208b9-109">され、デバッガー、IntelliSense など、ASP.NET Razor ページを使用する Visual Studio の機能を使用する方法。</span><span class="sxs-lookup"><span data-stu-id="208b9-109">How to use features in Visual Studio to work with ASP.NET Razor pages, including IntelliSense and the debugger.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="208b9-110">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="208b9-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="208b9-111">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="208b9-111">ASP.NET Web Pages (Razor) 3</span></span>
> - <span data-ttu-id="208b9-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="208b9-112">Visual Studio 2013</span></span>
> - <span data-ttu-id="208b9-113">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="208b9-113">WebMatrix 3</span></span>
>   
> 
> <span data-ttu-id="208b9-114">このチュートリアルは、ASP.NET Web Pages 2、Visual Studio 2012、Visual Studio 2010、および WebMatrix 2 のでも動作します。</span><span class="sxs-lookup"><span data-stu-id="208b9-114">This tutorial also works with ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010, and WebMatrix 2.</span></span>


<span data-ttu-id="208b9-115">WebMatrix またはその他の多くのコード エディターを使用して、Razor 構文を使用する ASP.NET Web pages をプログラミングできます。</span><span class="sxs-lookup"><span data-stu-id="208b9-115">You can program ASP.NET Web pages with Razor syntax using WebMatrix or many other code editors.</span></span> <span data-ttu-id="208b9-116">また、さまざまな種類のアプリケーション (だけでなく web サイト) を作成するための強力なツール セットを提供する全機能を備えた統合開発環境 (IDE) である Microsoft Visual Studio を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="208b9-116">You can also use Microsoft Visual Studio which is a full-featured integrated development environment (IDE) that provides a powerful set of tools for creating many types of applications (not just websites).</span></span> <span data-ttu-id="208b9-117">ASP.NET Razor ページを使用するには、Visual Studio の全エディションのいずれかを使用または無料[Visual Studio Express for Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express)エディションです。</span><span class="sxs-lookup"><span data-stu-id="208b9-117">To work with ASP.NET Razor pages, you can either use one of the full editions of Visual Studio or the free [Visual Studio Express for Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express) edition.</span></span>

<span data-ttu-id="208b9-118">Visual Studio は、ASP.NET Razor web ページを使用したプログラミングを提供する 2 つの特に便利な機能は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="208b9-118">Two particularly useful features that Visual Studio provides for programming with ASP.NET Razor web pages are:</span></span>

- <span data-ttu-id="208b9-119">*IntelliSense*です。</span><span class="sxs-lookup"><span data-stu-id="208b9-119">*IntelliSense*.</span></span> <span data-ttu-id="208b9-120">Visual Studio に組み込まれている、IntelliSense 機能は、WebMatrix では、IntelliSense より包括的なです。</span><span class="sxs-lookup"><span data-stu-id="208b9-120">The IntelliSense feature built into Visual Studio is more comprehensive than IntelliSense in WebMatrix.</span></span>
- <span data-ttu-id="208b9-121">*デバッガー*です。</span><span class="sxs-lookup"><span data-stu-id="208b9-121">*Debugger*.</span></span> <span data-ttu-id="208b9-122">デバッガーでは、実行、変数を調べること、およびコードの 1 行ずつステップ実行中に、プログラムを停止することによって、コードのトラブルシューティングを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="208b9-122">The debugger lets you troubleshoot your code by stopping a program while it's running, examining variables, and stepping through the code line by line.</span></span>

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a><span data-ttu-id="208b9-123">ASP.NET Web ページのさまざまなバージョンの Visual Studio の使用</span><span class="sxs-lookup"><span data-stu-id="208b9-123">Using Visual Studio with Different Versions of ASP.NET Web Pages</span></span>

<span data-ttu-id="208b9-124">Visual Studio 2012 と Visual Studio 2013 には、ASP.NET Web Pages のサポートが含まれます。</span><span class="sxs-lookup"><span data-stu-id="208b9-124">Visual Studio 2012 and Visual Studio 2013 include support for ASP.NET Web Pages.</span></span> <span data-ttu-id="208b9-125">(ASP.NET Web Pages をサポートするために必要なパッケージがインストールされている Visual Studio をインストールするときにします。)</span><span class="sxs-lookup"><span data-stu-id="208b9-125">(The packages that are required in order to support ASP.NET Web Pages are installed when you install Visual Studio.)</span></span>

<span data-ttu-id="208b9-126">Visual Studio 2010 サポートは含まれません既定では ASP.NET Web Pages のです。</span><span class="sxs-lookup"><span data-stu-id="208b9-126">Visual Studio 2010 does not include support by default for ASP.NET Web Pages.</span></span> <span data-ttu-id="208b9-127">Visual Studio 2010 では、ASP.NET Web Pages を使用するには、ASP.NET MVC のパッケージをインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="208b9-127">To use ASP.NET Web Pages with Visual Studio 2010, you must install the ASP.NET MVC package.</span></span> <span data-ttu-id="208b9-128">ASP.NET Web Pages 2 を取得するには、ASP.NET MVC 4 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="208b9-128">To get ASP.NET Web Pages 2, you install ASP.NET MVC 4.</span></span>

<span data-ttu-id="208b9-129">次の表は、ASP.NET Web Pages の異なるバージョンの Visual Studio でのサポートをまとめたものです。</span><span class="sxs-lookup"><span data-stu-id="208b9-129">The following table summarizes the support for ASP.NET Web Pages in different versions of Visual Studio.</span></span>

|  | <span data-ttu-id="208b9-130">Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="208b9-130">Visual Studio 2010</span></span> | <span data-ttu-id="208b9-131">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="208b9-131">Visual Studio 2012</span></span> | <span data-ttu-id="208b9-132">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="208b9-132">Visual Studio 2013</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="208b9-133">**ASP.NET Web Pages 2**</span><span class="sxs-lookup"><span data-stu-id="208b9-133">**ASP.NET Web Pages 2**</span></span> | <span data-ttu-id="208b9-134">ASP.NET MVC 4 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="208b9-134">Install ASP.NET MVC 4</span></span> | <span data-ttu-id="208b9-135">(インクルードされる)</span><span class="sxs-lookup"><span data-stu-id="208b9-135">(Included)</span></span> | <span data-ttu-id="208b9-136">(インクルードされる)</span><span class="sxs-lookup"><span data-stu-id="208b9-136">(Included)</span></span> |
| <span data-ttu-id="208b9-137">**ASP.NET Web Pages 3**</span><span class="sxs-lookup"><span data-stu-id="208b9-137">**ASP.NET Web Pages 3**</span></span> |  | <span data-ttu-id="208b9-138">更新プログラムを ASP.NET Web Pages 3 から NuGet</span><span class="sxs-lookup"><span data-stu-id="208b9-138">Update to ASP.NET Web Pages 3 through NuGet</span></span> | <span data-ttu-id="208b9-139">(インクルードされる)</span><span class="sxs-lookup"><span data-stu-id="208b9-139">(Included)</span></span> |

<span data-ttu-id="208b9-140">Visual Studio 2010 を使用する、次を参照してください。 [Visual Studio 2010 の ASP.NET Web Pages のサポートのインストール](#vs2010support)です。</span><span class="sxs-lookup"><span data-stu-id="208b9-140">To work with Visual Studio 2010, see [Installing Support for ASP.NET Web Pages in Visual Studio 2010](#vs2010support).</span></span>

## <a name="launching-visual-studio-from-webmatrix"></a><span data-ttu-id="208b9-141">WebMatrix から Visual Studio を起動します。</span><span class="sxs-lookup"><span data-stu-id="208b9-141">Launching Visual Studio from WebMatrix</span></span>

<span data-ttu-id="208b9-142">WebMatrix で、プロジェクトを開始する場合に、Visual Studio に切り替えする WebMatrix は、簡単に Visual Studio でプロジェクトを開く ボタンを提供します。</span><span class="sxs-lookup"><span data-stu-id="208b9-142">If you have started a project in WebMatrix and want to switch to Visual Studio, WebMatrix provides a button to easily open the project in Visual Studio.</span></span> <span data-ttu-id="208b9-143">有効にするには、このボタンは、コンピューターにインストールされている Visual Studio が必要です。</span><span class="sxs-lookup"><span data-stu-id="208b9-143">You must have Visual Studio installed on your computer for this button to be enabled.</span></span> <span data-ttu-id="208b9-144">次の図では、WebMatrix で、ボタンを示しています。</span><span class="sxs-lookup"><span data-stu-id="208b9-144">The following image shows the button in WebMatrix.</span></span>

![Visual Studio を起動します。](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

<span data-ttu-id="208b9-146">ボタンをクリックすると、プロジェクトが Visual Studio で開きます。</span><span class="sxs-lookup"><span data-stu-id="208b9-146">When you click the button, the project is opened in Visual Studio.</span></span> <span data-ttu-id="208b9-147">切り替えることができます前後 WebMatrix や Visual Studio、問題なくです。</span><span class="sxs-lookup"><span data-stu-id="208b9-147">You can switch back and forth between WebMatrix and Visual Studio without any problems.</span></span> <span data-ttu-id="208b9-148">すべてのファイルが、他方の環境で変更された、最新の変更の取得を再読み込みする必要がある場合、通知されます。</span><span class="sxs-lookup"><span data-stu-id="208b9-148">You will be notified if any files have changed in the other environment and need to be reloaded to get the latest changes.</span></span>

## <a name="creating-aspnet-razor-site-in-visual-studio"></a><span data-ttu-id="208b9-149">Visual Studio での ASP.NET Razor のサイトの作成</span><span class="sxs-lookup"><span data-stu-id="208b9-149">Creating ASP.NET Razor Site in Visual Studio</span></span>

<span data-ttu-id="208b9-150">Visual Studio で、ASP.NET Razor web サイトを作成します。</span><span class="sxs-lookup"><span data-stu-id="208b9-150">To create an ASP.NET Razor website in Visual Studio:</span></span>

1. <span data-ttu-id="208b9-151">Visual Studio または Visual Web Developer を開始します。</span><span class="sxs-lookup"><span data-stu-id="208b9-151">Start Visual Studio or Visual Web Developer.</span></span>
2. <span data-ttu-id="208b9-152">**ファイル** メニューをクリックして**新しい Web サイト**です。</span><span class="sxs-lookup"><span data-stu-id="208b9-152">In the **File** menu, click **New Web Site**.</span></span>

    ![新しい web サイトを作成します。](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. <span data-ttu-id="208b9-154">**新しい Web サイト** ダイアログ ボックスで、(Visual c# または Visual Basic) を使用する言語を選択します。</span><span class="sxs-lookup"><span data-stu-id="208b9-154">In the **New Web Site** dialog box, select the language to use (Visual C# or Visual Basic).</span></span>
4. <span data-ttu-id="208b9-155">選択、 **ASP.NET Web サイト (Razor)** テンプレート。</span><span class="sxs-lookup"><span data-stu-id="208b9-155">Select the **ASP.NET Web Site (Razor)** template.</span></span>

    ![razor サイト](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. <span data-ttu-id="208b9-157">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="208b9-157">Click **OK**.</span></span>

<span data-ttu-id="208b9-158">新しいプロジェクトが存在し、いくつか作業を開始するための既定の web ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="208b9-158">Your new project exists and is populated with some default web pages to help you get started.</span></span>

### <a name="using-intellisense"></a><span data-ttu-id="208b9-159">Using IntelliSense</span><span class="sxs-lookup"><span data-stu-id="208b9-159">Using IntelliSense</span></span>

<span data-ttu-id="208b9-160">これで、サイトを作成した、Visual Studio での IntelliSense の操作を確認できます。</span><span class="sxs-lookup"><span data-stu-id="208b9-160">Now that you've created a site, you can see how IntelliSense works in Visual Studio.</span></span>

1. <span data-ttu-id="208b9-161">作成した web サイトで開く、 *Default.cshtml*ページ。</span><span class="sxs-lookup"><span data-stu-id="208b9-161">In the website you just created, open the *Default.cshtml* page.</span></span>
2. <span data-ttu-id="208b9-162">後に、 `<h3>`  ページで、タグを入力`@ServerInfo.`(ドットを含む)。</span><span class="sxs-lookup"><span data-stu-id="208b9-162">After the `<h3>` tags in the page, type `@ServerInfo.` (including the dot).</span></span> <span data-ttu-id="208b9-163">IntelliSense で使用できるメソッドを表示する方法に注意してください、`ServerInfo`ドロップダウン リストでヘルパー。</span><span class="sxs-lookup"><span data-stu-id="208b9-163">Notice how IntelliSense displays the available methods for the `ServerInfo` helper in a drop-down list.</span></span> 

    ![intellisense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. <span data-ttu-id="208b9-165">選択、`GetHtml`し、Enter キーを押して、一覧からのメソッドです。</span><span class="sxs-lookup"><span data-stu-id="208b9-165">Select the `GetHtml` method from the list and then press Enter.</span></span> <span data-ttu-id="208b9-166">IntelliSense は、メソッドで自動的に入力します。</span><span class="sxs-lookup"><span data-stu-id="208b9-166">IntelliSense automatically fills in the method.</span></span> <span data-ttu-id="208b9-167">(C# での任意のメソッドに追加する必要がありますと`()`メソッドの後に文字です)。</span><span class="sxs-lookup"><span data-stu-id="208b9-167">(As with any method in C#, you must add `()` characters after the method.)</span></span>  
   <span data-ttu-id="208b9-168">完全なコード、`GetHtml`メソッドは次の例のようになります。</span><span class="sxs-lookup"><span data-stu-id="208b9-168">The completed code for the `GetHtml` method looks like the following example:</span></span>  

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. <span data-ttu-id="208b9-169">Ctrl + f5 キーを押してページを実行します。</span><span class="sxs-lookup"><span data-stu-id="208b9-169">Press Ctrl+F5 to run the page.</span></span> <span data-ttu-id="208b9-170">これは、ページの外観と、ブラウザーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="208b9-170">This is what the page looks like when displayed in a browser:</span></span> 

    ![ブラウザーで既定のページ](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. <span data-ttu-id="208b9-172">ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="208b9-172">Close the browser.</span></span>

### <a name="using-the-debugger"></a><span data-ttu-id="208b9-173">デバッガーの使用方法</span><span class="sxs-lookup"><span data-stu-id="208b9-173">Using the Debugger</span></span>

1. <span data-ttu-id="208b9-174">上部にある、 *Default.cshtml*ページで後で始まる行`Page.Title`、次のコード行を追加します。</span><span class="sxs-lookup"><span data-stu-id="208b9-174">At the top of the *Default.cshtml* page, after the line that begins with `Page.Title`, add the following line of code:</span></span> 

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. <span data-ttu-id="208b9-175">追加するためにこの新しい行の横にあるをクリックして、エディターで、コードの左側の灰色の余白で、*ブレークポイント*です。</span><span class="sxs-lookup"><span data-stu-id="208b9-175">In the gray margin of the editor to the left of the code, click next to this new line in order to add a *breakpoint*.</span></span> <span data-ttu-id="208b9-176">ブレークポイントは、何が起こっているかを確認できるようにその時点で、プログラムの実行を停止するデバッガーに指示するマーカーです。</span><span class="sxs-lookup"><span data-stu-id="208b9-176">A breakpoint is a marker that tells the debugger to stop running the program at that point so you can see what's happening.</span></span>

    ![ブレークポイントの設定](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. <span data-ttu-id="208b9-178">呼び出しを削除、`ServerInfo.GetHtml`メソッド呼び出しを追加し、`@myTime`代わりに変数。</span><span class="sxs-lookup"><span data-stu-id="208b9-178">Remove the call to the `ServerInfo.GetHtml` method, and add a call to the `@myTime` variable in its place.</span></span> <span data-ttu-id="208b9-179">この呼び出しは、新しい行のコードによって返される現在の時刻値を表示します。</span><span class="sxs-lookup"><span data-stu-id="208b9-179">This call displays the current time value that's returned by the new line of code.</span></span>
4. <span data-ttu-id="208b9-180">F5 キーを押して、デバッガーでページを実行します。</span><span class="sxs-lookup"><span data-stu-id="208b9-180">Press F5 to run the page in the debugger.</span></span> <span data-ttu-id="208b9-181">ページは、設定したブレークポイントで停止します。</span><span class="sxs-lookup"><span data-stu-id="208b9-181">The page stops on the breakpoint that you set.</span></span> <span data-ttu-id="208b9-182">次の図は、ページ内でどのようにエディター (黄) 内のブレークポイントとします。</span><span class="sxs-lookup"><span data-stu-id="208b9-182">The following image shows what the page looks like in the editor with the breakpoint (in yellow).</span></span> 

    ![デバッグのブレークポイント](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. <span data-ttu-id="208b9-184">デバッグ ツールバーで、をクリックして、**ステップ イン**を次のコード行を実行するボタン (または F11 キーを押します)。</span><span class="sxs-lookup"><span data-stu-id="208b9-184">In the Debug toolbar, click the **Step Into** button (or press F11) to run the next line of code.</span></span> <span data-ttu-id="208b9-185">このボタンをクリックするたびに実行を次のコード行に進みます。</span><span class="sxs-lookup"><span data-stu-id="208b9-185">Each time you click this button, you advance the execution to the next line of code.</span></span>

    ![ボタンにステップ インします。](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. <span data-ttu-id="208b9-187">値を調べて、`myTime`変数の上にマウス ポインターを保持しているかに表示される値を調べることによって、**ローカル**と**呼び出し履歴**windows です。</span><span class="sxs-lookup"><span data-stu-id="208b9-187">Examine the value of the `myTime` variable by holding your mouse pointer over it or by inspecting the values displayed in the **Locals** and **Call Stack** windows.</span></span> <span data-ttu-id="208b9-188">Visual Studio では、変数の値を表示します。</span><span class="sxs-lookup"><span data-stu-id="208b9-188">Visual Studio display the value of the variable.</span></span>

    ![時間を示さない値](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. <span data-ttu-id="208b9-190">変数を確認して、コードのステップ実行を完了したら、f5 キーを押して 1 行ずつ停止することがなく、ページの実行を継続します。</span><span class="sxs-lookup"><span data-stu-id="208b9-190">When you're done examining the variable and stepping through code, press F5 to continue running the page without stopping at each line.</span></span> <span data-ttu-id="208b9-191">すべてのコードをステップ実行が完了したら、ブラウザーには、ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="208b9-191">When you've finished stepping through all the code, the browser displays the page.</span></span>

<span data-ttu-id="208b9-192">詳細については、デバッガーおよび Visual Studio でコードをデバッグする方法について、次を参照してください。[チュートリアル: Visual Web Developer で Web ページをデバッグ](https://msdn.microsoft.com/library/z9e7w6cs.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="208b9-192">To learn more about the debugger and about how to debug code in Visual Studio, see [Walkthrough: Debugging Web Pages in Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span></span>

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a><span data-ttu-id="208b9-193">Visual Studio での ASP.NET MVC プロジェクトで Razor を使用します。</span><span class="sxs-lookup"><span data-stu-id="208b9-193">Using Razor in ASP.NET MVC projects with Visual Studio</span></span>

<span data-ttu-id="208b9-194">ASP.NET MVC プロジェクトで Razor 構文が広範囲に使用されます。</span><span class="sxs-lookup"><span data-stu-id="208b9-194">The Razor syntax is also used extensively in ASP.NET MVC projects.</span></span> <span data-ttu-id="208b9-195">MVC は、動的な web サイトを構築する、パターンに基づく強力な方法です。</span><span class="sxs-lookup"><span data-stu-id="208b9-195">MVC is a powerful, patterns-based way to build dynamic websites.</span></span> <span data-ttu-id="208b9-196">ASP.NET Web Pages サイトが管理するが困難になると、ASP.NET MVC アプリケーションに変換することを検討することができます。</span><span class="sxs-lookup"><span data-stu-id="208b9-196">If your ASP.NET Web Pages site becomes difficult to maintain, you might want to consider converting it to an ASP.NET MVC application.</span></span> <span data-ttu-id="208b9-197">MVC アプリケーションの作成の例は、次を参照してください。 [ASP.NET MVC 5 の概要](../../../mvc/overview/getting-started/introduction/getting-started.md)です。</span><span class="sxs-lookup"><span data-stu-id="208b9-197">For an example of creating an MVC application, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a><span data-ttu-id="208b9-198">Visual Studio 2010 の ASP.NET Web Pages のサポートのインストール</span><span class="sxs-lookup"><span data-stu-id="208b9-198">Installing Support for ASP.NET Web Pages in Visual Studio 2010</span></span>

<span data-ttu-id="208b9-199">このセクションでは、Visual Studio の Visual Web Developer Express 2010 と ASP.NET Web Pages のツールをインストールする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="208b9-199">This section shows how to install Visual Web Developer Express 2010 and the ASP.NET Web Pages Tools for Visual Studio.</span></span>

1. <span data-ttu-id="208b9-200">Web Platform Installer がいない場合は、次の URL からダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="208b9-200">If you don't already have the Web Platform Installer, download it from the following URL:</span></span>

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. <span data-ttu-id="208b9-201">Web Platform Installer を実行します。</span><span class="sxs-lookup"><span data-stu-id="208b9-201">Run the Web Platform Installer.</span></span>
3. <span data-ttu-id="208b9-202">クリックして、**製品**タブです。</span><span class="sxs-lookup"><span data-stu-id="208b9-202">Click the **Products** tab.</span></span>

    ![WebPI 製品 タブ](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. <span data-ttu-id="208b9-204">検索**ASP.NET MVC 4** (ASP.NET Web Pages 2) の順にクリック**追加**です。</span><span class="sxs-lookup"><span data-stu-id="208b9-204">Search for **ASP.NET MVC 4** (for ASP.NET Web Pages 2) and then click **Add**.</span></span> <span data-ttu-id="208b9-205">これらの製品には、ASP.NET Razor web サイトを構築するための Visual Studio のツールが含まれます。</span><span class="sxs-lookup"><span data-stu-id="208b9-205">These products include Visual Studio tools for building ASP.NET Razor websites.</span></span>

    ![WebPi のインストール オプション](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. <span data-ttu-id="208b9-207">をクリックして**インストール**インストールを完了します。</span><span class="sxs-lookup"><span data-stu-id="208b9-207">Click **Install** to complete the installation.</span></span>
