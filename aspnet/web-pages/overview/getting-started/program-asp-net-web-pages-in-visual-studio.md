---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: プログラミングの ASP.NET Web Pages (Razor) を使用して Visual Studio |Microsoft Docs
author: Rick-Anderson
description: この付録では、Visual Studio 2010 または Visual Web Developer 2010 Express を Razor 構文を使用して ASP.NET Web Pages をプログラムに使用する方法について説明します。
ms.author: riande
ms.date: 02/13/2014
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: 5b8df17ec1021d133579e23cb4f5b0d0f67d4c7c
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020456"
---
<a name="programming-aspnet-web-pages-razor-using-visual-studio"></a><span data-ttu-id="764ef-103">Visual Studio を使用して ASP.NET Web ページ (Razor) のプログラミング</span><span class="sxs-lookup"><span data-stu-id="764ef-103">Programming ASP.NET Web Pages (Razor) Using Visual Studio</span></span>
====================
<span data-ttu-id="764ef-104">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="764ef-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="764ef-105">この記事では、Visual Studio または Visual Web Developer Express を web サイトの ASP.NET Web Pages (Razor) のプログラムに使用する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="764ef-105">This article explains how you can use Visual Studio or Visual Web Developer Express to program ASP.NET Web Pages (Razor) websites.</span></span>
>
> <span data-ttu-id="764ef-106">学習内容</span><span class="sxs-lookup"><span data-stu-id="764ef-106">What you'll learn</span></span>
>
> - <span data-ttu-id="764ef-107">Visual Studio のバージョンの ASP.NET Web ページを操作する (ある場合) をインストールするために必要とします。</span><span class="sxs-lookup"><span data-stu-id="764ef-107">What you need to install (if anything) to work with ASP.NET Web Pages in your version of Visual Studio.</span></span>
> - <span data-ttu-id="764ef-108">Visual Web Developer 2010 Express を ASP.NET Web Pages のサポートを追加する方法。</span><span class="sxs-lookup"><span data-stu-id="764ef-108">How to add support for ASP.NET Web Pages to Visual Web Developer 2010 Express.</span></span>
> - <span data-ttu-id="764ef-109">Visual Studio で ASP.NET Razor ページ, IntelliSense や、デバッガーを使用する機能を使用する方法。</span><span class="sxs-lookup"><span data-stu-id="764ef-109">How to use features in Visual Studio to work with ASP.NET Razor pages, including IntelliSense and the debugger.</span></span>
>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="764ef-110">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="764ef-110">Software versions used in the tutorial</span></span>
>
>
> - <span data-ttu-id="764ef-111">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="764ef-111">ASP.NET Web Pages (Razor) 3</span></span>
> - <span data-ttu-id="764ef-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="764ef-112">Visual Studio 2013</span></span>
> - <span data-ttu-id="764ef-113">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="764ef-113">WebMatrix 3</span></span>
>
>
> <span data-ttu-id="764ef-114">このチュートリアルは、ASP.NET Web Pages 2、Visual Studio 2012、Visual Studio 2010、および WebMatrix 2 により連携します。</span><span class="sxs-lookup"><span data-stu-id="764ef-114">This tutorial also works with ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010, and WebMatrix 2.</span></span>


<span data-ttu-id="764ef-115">WebMatrix またはその他の多くのコード エディターを使用して、Razor 構文を使用する ASP.NET Web pages をプログラミングできます。</span><span class="sxs-lookup"><span data-stu-id="764ef-115">You can program ASP.NET Web pages with Razor syntax using WebMatrix or many other code editors.</span></span> <span data-ttu-id="764ef-116">さまざまな種類のアプリケーション (web サイトがだけでなく) を作成するための強力なツール一式を提供するすべての機能を備えた統合開発環境 (IDE) である Microsoft Visual Studio を使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="764ef-116">You can also use Microsoft Visual Studio which is a full-featured integrated development environment (IDE) that provides a powerful set of tools for creating many types of applications (not just websites).</span></span> <span data-ttu-id="764ef-117">ASP.NET Razor ページを使用することができますを使用する[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)します。</span><span class="sxs-lookup"><span data-stu-id="764ef-117">To work with ASP.NET Razor pages, you can use [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).</span></span>

<span data-ttu-id="764ef-118">ASP.NET Razor web ページを使用したプログラミングの Visual Studio が提供されている 2 つの特に便利な機能は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="764ef-118">Two particularly useful features that Visual Studio provides for programming with ASP.NET Razor web pages are:</span></span>

- <span data-ttu-id="764ef-119">*IntelliSense*します。</span><span class="sxs-lookup"><span data-stu-id="764ef-119">*IntelliSense*.</span></span> <span data-ttu-id="764ef-120">Visual Studio に組み込まれている IntelliSense 機能は、WebMatrix では、IntelliSense のより包括的なです。</span><span class="sxs-lookup"><span data-stu-id="764ef-120">The IntelliSense feature built into Visual Studio is more comprehensive than IntelliSense in WebMatrix.</span></span>
- <span data-ttu-id="764ef-121">*デバッガー*します。</span><span class="sxs-lookup"><span data-stu-id="764ef-121">*Debugger*.</span></span> <span data-ttu-id="764ef-122">デバッガーでは、実行、変数を調べること、およびコード 1 行ずつステップ実行中にプログラムを停止することで、コードのトラブルシューティングを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="764ef-122">The debugger lets you troubleshoot your code by stopping a program while it's running, examining variables, and stepping through the code line by line.</span></span>

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a><span data-ttu-id="764ef-123">Visual Studio を使用して、さまざまなバージョンの ASP.NET Web ページ</span><span class="sxs-lookup"><span data-stu-id="764ef-123">Using Visual Studio with Different Versions of ASP.NET Web Pages</span></span>

<span data-ttu-id="764ef-124">Visual Studio 2017 での ASP.NET web アプリを開発するには、インストール、 **ASP.NET および web 開発**ワークロード。</span><span class="sxs-lookup"><span data-stu-id="764ef-124">To develop ASP.NET web apps in Visual Studio 2017, install the **ASP.NET and web development** workload.</span></span>

<span data-ttu-id="764ef-125">Visual Studio 2012 と Visual Studio 2013 には、ASP.NET Web Pages のサポートが含まれます。</span><span class="sxs-lookup"><span data-stu-id="764ef-125">Visual Studio 2012 and Visual Studio 2013 include support for ASP.NET Web Pages.</span></span> <span data-ttu-id="764ef-126">(ASP.NET Web Pages をサポートするために必要なパッケージは、Visual Studio をインストールするときにインストールされます)。</span><span class="sxs-lookup"><span data-stu-id="764ef-126">(The packages that are required in order to support ASP.NET Web Pages are installed when you install Visual Studio.)</span></span>

<span data-ttu-id="764ef-127">Visual Studio 2010 サポートは含まれません既定の ASP.NET Web ページ。</span><span class="sxs-lookup"><span data-stu-id="764ef-127">Visual Studio 2010 does not include support by default for ASP.NET Web Pages.</span></span> <span data-ttu-id="764ef-128">Visual Studio 2010 では、ASP.NET Web Pages を使用するには、ASP.NET MVC パッケージをインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="764ef-128">To use ASP.NET Web Pages with Visual Studio 2010, you must install the ASP.NET MVC package.</span></span> <span data-ttu-id="764ef-129">ASP.NET Web Pages 2 を取得するには、ASP.NET MVC 4 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="764ef-129">To get ASP.NET Web Pages 2, you install ASP.NET MVC 4.</span></span>

<span data-ttu-id="764ef-130">次の表では、異なるバージョンの Visual Studio で ASP.NET Web Pages のサポートをまとめたものです。</span><span class="sxs-lookup"><span data-stu-id="764ef-130">The following table summarizes the support for ASP.NET Web Pages in different versions of Visual Studio.</span></span>

|  | <span data-ttu-id="764ef-131">Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="764ef-131">Visual Studio 2010</span></span> | <span data-ttu-id="764ef-132">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="764ef-132">Visual Studio 2012</span></span> | <span data-ttu-id="764ef-133">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="764ef-133">Visual Studio 2013</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="764ef-134">**ASP.NET Web ページ 2**</span><span class="sxs-lookup"><span data-stu-id="764ef-134">**ASP.NET Web Pages 2**</span></span> | <span data-ttu-id="764ef-135">ASP.NET MVC 4 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="764ef-135">Install ASP.NET MVC 4</span></span> | <span data-ttu-id="764ef-136">(含まれています)</span><span class="sxs-lookup"><span data-stu-id="764ef-136">(Included)</span></span> | <span data-ttu-id="764ef-137">(含まれています)</span><span class="sxs-lookup"><span data-stu-id="764ef-137">(Included)</span></span> |
| <span data-ttu-id="764ef-138">**ASP.NET Web ページ 3**</span><span class="sxs-lookup"><span data-stu-id="764ef-138">**ASP.NET Web Pages 3**</span></span> |  | <span data-ttu-id="764ef-139">更新プログラムを ASP.NET Web ページから 3 つの NuGet</span><span class="sxs-lookup"><span data-stu-id="764ef-139">Update to ASP.NET Web Pages 3 through NuGet</span></span> | <span data-ttu-id="764ef-140">(含まれています)</span><span class="sxs-lookup"><span data-stu-id="764ef-140">(Included)</span></span> |

<span data-ttu-id="764ef-141">Visual Studio 2010 を使用する、次を参照してください。 [Visual Studio 2010 で ASP.NET Web Pages のサポートのインストール](#vs2010support)します。</span><span class="sxs-lookup"><span data-stu-id="764ef-141">To work with Visual Studio 2010, see [Installing Support for ASP.NET Web Pages in Visual Studio 2010](#vs2010support).</span></span>

## <a name="launching-visual-studio-from-webmatrix"></a><span data-ttu-id="764ef-142">WebMatrix から Visual Studio を起動します。</span><span class="sxs-lookup"><span data-stu-id="764ef-142">Launching Visual Studio from WebMatrix</span></span>

<span data-ttu-id="764ef-143">WebMatrix で、プロジェクトの開始が Visual Studio に切り替える場合、WebMatrix が簡単に Visual Studio でプロジェクトを開く ボタンを提供します。</span><span class="sxs-lookup"><span data-stu-id="764ef-143">If you have started a project in WebMatrix and want to switch to Visual Studio, WebMatrix provides a button to easily open the project in Visual Studio.</span></span> <span data-ttu-id="764ef-144">有効にするには、このボタンは、コンピューターにインストールされている Visual Studio が必要です。</span><span class="sxs-lookup"><span data-stu-id="764ef-144">You must have Visual Studio installed on your computer for this button to be enabled.</span></span> <span data-ttu-id="764ef-145">次の図では、WebMatrix で、ボタンを示します。</span><span class="sxs-lookup"><span data-stu-id="764ef-145">The following image shows the button in WebMatrix.</span></span>

![Visual Studio を起動します。](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

<span data-ttu-id="764ef-147">ボタンをクリックすると、プロジェクトは Visual Studio で開きます。</span><span class="sxs-lookup"><span data-stu-id="764ef-147">When you click the button, the project is opened in Visual Studio.</span></span> <span data-ttu-id="764ef-148">切り替えることができますを行き来 WebMatrix や Visual Studio は問題なく。</span><span class="sxs-lookup"><span data-stu-id="764ef-148">You can switch back and forth between WebMatrix and Visual Studio without any problems.</span></span> <span data-ttu-id="764ef-149">すべてのファイルが他の環境で変更された最新の変更の取得を再読み込みする必要がある場合に通知されます。</span><span class="sxs-lookup"><span data-stu-id="764ef-149">You will be notified if any files have changed in the other environment and need to be reloaded to get the latest changes.</span></span>

## <a name="creating-aspnet-razor-site-in-visual-studio"></a><span data-ttu-id="764ef-150">Visual Studio で ASP.NET Razor のサイトを作成します。</span><span class="sxs-lookup"><span data-stu-id="764ef-150">Creating ASP.NET Razor Site in Visual Studio</span></span>

<span data-ttu-id="764ef-151">Visual Studio で ASP.NET Razor の web サイトを作成します。</span><span class="sxs-lookup"><span data-stu-id="764ef-151">To create an ASP.NET Razor website in Visual Studio:</span></span>

1. <span data-ttu-id="764ef-152">Visual Studio を開きます。</span><span class="sxs-lookup"><span data-stu-id="764ef-152">Open Visual Studio.</span></span>
2. <span data-ttu-id="764ef-153">**ファイル** メニューのをクリックして**新しい Web サイト**します。</span><span class="sxs-lookup"><span data-stu-id="764ef-153">In the **File** menu, click **New Web Site**.</span></span>

    ![新しい web サイトを作成します。](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. <span data-ttu-id="764ef-155">**新しい Web サイト** ダイアログ ボックスで、(Visual C# または Visual Basic) を使用する言語を選択します。</span><span class="sxs-lookup"><span data-stu-id="764ef-155">In the **New Web Site** dialog box, select the language to use (Visual C# or Visual Basic).</span></span>
4. <span data-ttu-id="764ef-156">選択、 **ASP.NET Web サイト (Razor)** テンプレート。</span><span class="sxs-lookup"><span data-stu-id="764ef-156">Select the **ASP.NET Web Site (Razor)** template.</span></span>

    ![razor のサイト](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. <span data-ttu-id="764ef-158">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="764ef-158">Click **OK**.</span></span>

<span data-ttu-id="764ef-159">新しいプロジェクトが存在し、既定の web ページを簡単には格納されます。</span><span class="sxs-lookup"><span data-stu-id="764ef-159">Your new project exists and is populated with some default web pages to help you get started.</span></span>

### <a name="using-intellisense"></a><span data-ttu-id="764ef-160">Using IntelliSense</span><span class="sxs-lookup"><span data-stu-id="764ef-160">Using IntelliSense</span></span>

<span data-ttu-id="764ef-161">サイトを作成するので、Visual Studio で IntelliSense がどのように動作するかが確認できます。</span><span class="sxs-lookup"><span data-stu-id="764ef-161">Now that you've created a site, you can see how IntelliSense works in Visual Studio.</span></span>

1. <span data-ttu-id="764ef-162">作成した web サイトで開く、 *Default.cshtml*ページ。</span><span class="sxs-lookup"><span data-stu-id="764ef-162">In the website you just created, open the *Default.cshtml* page.</span></span>
2. <span data-ttu-id="764ef-163">後に、 `<h3>`  ページで、タグを入力`@ServerInfo.`(ドットを含む)。</span><span class="sxs-lookup"><span data-stu-id="764ef-163">After the `<h3>` tags in the page, type `@ServerInfo.` (including the dot).</span></span> <span data-ttu-id="764ef-164">IntelliSense の使用可能なメソッドの表示方法に注意してください、`ServerInfo`ドロップダウン リストでヘルパー。</span><span class="sxs-lookup"><span data-stu-id="764ef-164">Notice how IntelliSense displays the available methods for the `ServerInfo` helper in a drop-down list.</span></span>

    ![intellisense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. <span data-ttu-id="764ef-166">選択、`GetHtml`メソッドを一覧し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="764ef-166">Select the `GetHtml` method from the list and then press Enter.</span></span> <span data-ttu-id="764ef-167">IntelliSense は、メソッドで自動的に入力されます。</span><span class="sxs-lookup"><span data-stu-id="764ef-167">IntelliSense automatically fills in the method.</span></span> <span data-ttu-id="764ef-168">(C# での任意のメソッドに追加する必要がありますと`()`メソッドの後の文字)。完成したコード、`GetHtml`メソッドは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="764ef-168">(As with any method in C#, you must add `()` characters after the method.) The completed code for the `GetHtml` method looks like the following example:</span></span>

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. <span data-ttu-id="764ef-169">Ctrl + f5 キーを押してページを実行します。</span><span class="sxs-lookup"><span data-stu-id="764ef-169">Press Ctrl+F5 to run the page.</span></span> <span data-ttu-id="764ef-170">これは、ページがどのようにと、ブラウザーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="764ef-170">This is what the page looks like when displayed in a browser:</span></span>

    ![ブラウザーで既定のページ](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. <span data-ttu-id="764ef-172">ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="764ef-172">Close the browser.</span></span>

### <a name="using-the-debugger"></a><span data-ttu-id="764ef-173">デバッガーを使用します。</span><span class="sxs-lookup"><span data-stu-id="764ef-173">Using the Debugger</span></span>

1. <span data-ttu-id="764ef-174">上部にある、 *Default.cshtml*ページで後で始まる行`Page.Title`、次のコード行を追加します。</span><span class="sxs-lookup"><span data-stu-id="764ef-174">At the top of the *Default.cshtml* page, after the line that begins with `Page.Title`, add the following line of code:</span></span>

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. <span data-ttu-id="764ef-175">エディター、コードの左側の灰色の余白を追加するにはこの新しい行の横にあるクリックして、*ブレークポイント*します。</span><span class="sxs-lookup"><span data-stu-id="764ef-175">In the gray margin of the editor to the left of the code, click next to this new line in order to add a *breakpoint*.</span></span> <span data-ttu-id="764ef-176">ブレークポイントは、何が起こっているかを確認できるようにその時点で、プログラムの実行を停止するデバッガーを示すマーカーです。</span><span class="sxs-lookup"><span data-stu-id="764ef-176">A breakpoint is a marker that tells the debugger to stop running the program at that point so you can see what's happening.</span></span>

    ![ブレークポイントの設定](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. <span data-ttu-id="764ef-178">呼び出しを削除、`ServerInfo.GetHtml`メソッド呼び出しを追加し、`@myTime`代わりに変数。</span><span class="sxs-lookup"><span data-stu-id="764ef-178">Remove the call to the `ServerInfo.GetHtml` method, and add a call to the `@myTime` variable in its place.</span></span> <span data-ttu-id="764ef-179">この呼び出しは、新しい行のコードによって返される現在の時刻値を表示します。</span><span class="sxs-lookup"><span data-stu-id="764ef-179">This call displays the current time value that's returned by the new line of code.</span></span>
4. <span data-ttu-id="764ef-180">F5 キーを押して、デバッガーでページを実行します。</span><span class="sxs-lookup"><span data-stu-id="764ef-180">Press F5 to run the page in the debugger.</span></span> <span data-ttu-id="764ef-181">ページは、設定したブレークポイントで停止します。</span><span class="sxs-lookup"><span data-stu-id="764ef-181">The page stops on the breakpoint that you set.</span></span> <span data-ttu-id="764ef-182">次の図は、ページ内でどのようにエディター (黄色) でブレークポイントを設定しました。</span><span class="sxs-lookup"><span data-stu-id="764ef-182">The following image shows what the page looks like in the editor with the breakpoint (in yellow).</span></span>

    ![デバッグのブレークポイント](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. <span data-ttu-id="764ef-184">[デバッグ] ツールバーをクリックして、**ステップ イン**次のコード行を実行するボタン (または F11 キーを押します)。</span><span class="sxs-lookup"><span data-stu-id="764ef-184">In the Debug toolbar, click the **Step Into** button (or press F11) to run the next line of code.</span></span> <span data-ttu-id="764ef-185">このボタンをクリックするたびに、実行を次のコード行に進みます。</span><span class="sxs-lookup"><span data-stu-id="764ef-185">Each time you click this button, you advance the execution to the next line of code.</span></span>

    ![ステップ イン ボタン](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. <span data-ttu-id="764ef-187">値を調べて、`myTime`変数の上にマウス ポインターを保持しているかに表示される値を調べることによって、**ローカル**と**呼び出し履歴**windows。</span><span class="sxs-lookup"><span data-stu-id="764ef-187">Examine the value of the `myTime` variable by holding your mouse pointer over it or by inspecting the values displayed in the **Locals** and **Call Stack** windows.</span></span> <span data-ttu-id="764ef-188">Visual Studio では、変数の値を表示します。</span><span class="sxs-lookup"><span data-stu-id="764ef-188">Visual Studio display the value of the variable.</span></span>

    ![表示時刻の値](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. <span data-ttu-id="764ef-190">変数を調べると、コードのステップが完了したら、f5 キーを押して 1 行ずつ停止することがなく、ページの実行を継続します。</span><span class="sxs-lookup"><span data-stu-id="764ef-190">When you're done examining the variable and stepping through code, press F5 to continue running the page without stopping at each line.</span></span> <span data-ttu-id="764ef-191">すべてのコードのステップが完了したら、ブラウザーにページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="764ef-191">When you've finished stepping through all the code, the browser displays the page.</span></span>

<span data-ttu-id="764ef-192">デバッガーについてと、Visual Studio でコードをデバッグする方法についての詳細についてを参照してください。[チュートリアル: Visual Web Developer で Web ページのデバッグ](https://msdn.microsoft.com/library/z9e7w6cs.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="764ef-192">To learn more about the debugger and about how to debug code in Visual Studio, see [Walkthrough: Debugging Web Pages in Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span></span>

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a><span data-ttu-id="764ef-193">Visual Studio を使用した ASP.NET MVC プロジェクトでの Razor の使用</span><span class="sxs-lookup"><span data-stu-id="764ef-193">Using Razor in ASP.NET MVC projects with Visual Studio</span></span>

<span data-ttu-id="764ef-194">ASP.NET MVC プロジェクトで幅広く、Razor 構文が使用されます。</span><span class="sxs-lookup"><span data-stu-id="764ef-194">The Razor syntax is also used extensively in ASP.NET MVC projects.</span></span> <span data-ttu-id="764ef-195">MVC は、動的な web サイトを構築する、パターン ベースの強力な方法です。</span><span class="sxs-lookup"><span data-stu-id="764ef-195">MVC is a powerful, patterns-based way to build dynamic websites.</span></span> <span data-ttu-id="764ef-196">ASP.NET Web Pages サイトの保守が困難になると、ASP.NET MVC アプリケーションに変換することを検討する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="764ef-196">If your ASP.NET Web Pages site becomes difficult to maintain, you might want to consider converting it to an ASP.NET MVC application.</span></span> <span data-ttu-id="764ef-197">MVC アプリケーションの作成の例は、次を参照してください。 [ASP.NET MVC 5 の概要](../../../mvc/overview/getting-started/introduction/getting-started.md)します。</span><span class="sxs-lookup"><span data-stu-id="764ef-197">For an example of creating an MVC application, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a><span data-ttu-id="764ef-198">Visual Studio 2010 での ASP.NET Web ページのサポートのインストール</span><span class="sxs-lookup"><span data-stu-id="764ef-198">Installing Support for ASP.NET Web Pages in Visual Studio 2010</span></span>

<span data-ttu-id="764ef-199">このセクションでは、Visual Studio の Visual Web Developer Express 2010 と ASP.NET Web ページのツールをインストールする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="764ef-199">This section shows how to install Visual Web Developer Express 2010 and the ASP.NET Web Pages Tools for Visual Studio.</span></span>

1. <span data-ttu-id="764ef-200">Web Platform Installer がまだしていない場合は、次の URL からダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="764ef-200">If you don't already have the Web Platform Installer, download it from the following URL:</span></span>

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. <span data-ttu-id="764ef-201">Web Platform Installer を実行します。</span><span class="sxs-lookup"><span data-stu-id="764ef-201">Run the Web Platform Installer.</span></span>
3. <span data-ttu-id="764ef-202">をクリックして、**製品**タブ。</span><span class="sxs-lookup"><span data-stu-id="764ef-202">Click the **Products** tab.</span></span>

    ![WebPI 製品 タブ](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. <span data-ttu-id="764ef-204">検索**ASP.NET MVC 4** (ASP.NET Web ページ 2) の順にクリックします**追加**します。</span><span class="sxs-lookup"><span data-stu-id="764ef-204">Search for **ASP.NET MVC 4** (for ASP.NET Web Pages 2) and then click **Add**.</span></span> <span data-ttu-id="764ef-205">これらの製品には、ASP.NET Razor web サイトを構築するための Visual Studio ツールが含まれます。</span><span class="sxs-lookup"><span data-stu-id="764ef-205">These products include Visual Studio tools for building ASP.NET Razor websites.</span></span>

    ![WebPi のインストール オプション](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. <span data-ttu-id="764ef-207">クリックして**インストール**インストールを完了します。</span><span class="sxs-lookup"><span data-stu-id="764ef-207">Click **Install** to complete the installation.</span></span>
