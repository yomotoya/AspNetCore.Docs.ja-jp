---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: ASP.NET Web API のヘルプ ページの作成 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 04/01/2013
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 3e8a0e6c4555e2b8d880118ca499b801ac8be63e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811945"
---
<a name="creating-help-pages-for-aspnet-web-api"></a><span data-ttu-id="777ab-102">ASP.NET Web API のヘルプ ページの作成</span><span class="sxs-lookup"><span data-stu-id="777ab-102">Creating Help Pages for ASP.NET Web API</span></span>
====================
<span data-ttu-id="777ab-103">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="777ab-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="777ab-104">Web API を作成するときに多くの場合のヘルプ ページを作成すると便利他の開発者は、API を呼び出す方法を認識できるようにします。</span><span class="sxs-lookup"><span data-stu-id="777ab-104">When you create a web API, it is often useful to create a help page, so that other developers will know how to call your API.</span></span> <span data-ttu-id="777ab-105">すべてのドキュメントを手動で作成する可能性がありますが、できるだけ多くの自動生成することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="777ab-105">You could create all of the documentation manually, but it is better to autogenerate as much as possible.</span></span>

<span data-ttu-id="777ab-106">この作業を容易にするには、ASP.NET Web API は、実行時のヘルプ ページの自動生成するライブラリを提供します。</span><span class="sxs-lookup"><span data-stu-id="777ab-106">To make this task easier, ASP.NET Web API provides a library for auto-generating help pages at run time.</span></span>

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a><span data-ttu-id="777ab-107">API ヘルプ ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="777ab-107">Creating API Help Pages</span></span>

<span data-ttu-id="777ab-108">インストール[ASP.NET および Web Tools 2012.2 の Update](https://go.microsoft.com/fwlink/?LinkId=282650)します。</span><span class="sxs-lookup"><span data-stu-id="777ab-108">Install [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="777ab-109">この更新プログラムは、Web API プロジェクト テンプレートに、ヘルプ ページを統合します。</span><span class="sxs-lookup"><span data-stu-id="777ab-109">This update integrates help pages into the Web API project template.</span></span>

<span data-ttu-id="777ab-110">次に、新しい ASP.NET MVC 4 プロジェクトを作成し、Web API プロジェクト テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="777ab-110">Next, create a new ASP.NET MVC 4 project and select the Web API project template.</span></span> <span data-ttu-id="777ab-111">プロジェクト テンプレートの作成という名前の API の例のコント ローラーを`ValuesController`します。</span><span class="sxs-lookup"><span data-stu-id="777ab-111">The project template creates an example API controller named `ValuesController`.</span></span> <span data-ttu-id="777ab-112">テンプレートには、API のヘルプ ページも作成します。</span><span class="sxs-lookup"><span data-stu-id="777ab-112">The template also creates the API help pages.</span></span> <span data-ttu-id="777ab-113">すべてのヘルプ ページのコード ファイルは、プロジェクトの [区分] フォルダーに配置されます。</span><span class="sxs-lookup"><span data-stu-id="777ab-113">All of the code files for the help page are placed in the Areas folder of the project.</span></span>

![](creating-api-help-pages/_static/image2.png)

<span data-ttu-id="777ab-114">アプリケーションを実行すると、ホーム ページには、API ヘルプ ページへのリンクが含まれています。</span><span class="sxs-lookup"><span data-stu-id="777ab-114">When you run the application, the home page contains a link to the API help page.</span></span> <span data-ttu-id="777ab-115">ホーム ページからは、相対パスは、/Help が。</span><span class="sxs-lookup"><span data-stu-id="777ab-115">From the home page, the relative path is /Help.</span></span>

![](creating-api-help-pages/_static/image3.png)

<span data-ttu-id="777ab-116">このリンクでは、API の概要ページに表示されます。</span><span class="sxs-lookup"><span data-stu-id="777ab-116">This link brings you to an API summary page.</span></span>

![](creating-api-help-pages/_static/image4.png)

<span data-ttu-id="777ab-117">このページの MVC ビューは、Areas/HelpPage/Views/Help/Index.cshtml で定義されます。</span><span class="sxs-lookup"><span data-stu-id="777ab-117">The MVC view for this page is defined in Areas/HelpPage/Views/Help/Index.cshtml.</span></span> <span data-ttu-id="777ab-118">レイアウト、概要、タイトル、スタイル、およびなどを変更するには、このページを編集することができます。</span><span class="sxs-lookup"><span data-stu-id="777ab-118">You can edit this page to modify the layout, introduction, title, styles, and so forth.</span></span>

<span data-ttu-id="777ab-119">ページの主要部分では、コント ローラーでグループ化された Api のテーブルです。</span><span class="sxs-lookup"><span data-stu-id="777ab-119">The main part of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="777ab-120">テーブルのエントリも動的に生成されたを使用して、 **IApiExplorer**インターフェイス。</span><span class="sxs-lookup"><span data-stu-id="777ab-120">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="777ab-121">(説明しますこのインターフェイスについては後で。)新しい API コント ローラーを追加すると、実行時に、テーブルが自動的に更新します。</span><span class="sxs-lookup"><span data-stu-id="777ab-121">(I'll talk more about this interface later.) If you add a new API controller, the table is automatically updated at run time.</span></span>

<span data-ttu-id="777ab-122">"API"の列には、HTTP メソッドと相対 URI が表示されます。</span><span class="sxs-lookup"><span data-stu-id="777ab-122">The "API" column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="777ab-123">「説明」列には、各 API のドキュメントが含まれています。</span><span class="sxs-lookup"><span data-stu-id="777ab-123">The "Description" column contains documentation for each API.</span></span> <span data-ttu-id="777ab-124">最初に、ドキュメントは、プレース ホルダー テキストだけです。</span><span class="sxs-lookup"><span data-stu-id="777ab-124">Initially, the documentation is just placeholder text.</span></span> <span data-ttu-id="777ab-125">次のセクションでは、XML のコメントからドキュメントを追加する方法を紹介します。</span><span class="sxs-lookup"><span data-stu-id="777ab-125">In the next section, I'll show you how to add documentation from XML comments.</span></span>

<span data-ttu-id="777ab-126">各 API では、例の要求および応答の本文を含む、詳細な情報をページにリンクがあります。</span><span class="sxs-lookup"><span data-stu-id="777ab-126">Each API has a link to a page with more detailed information, including example request and response bodies.</span></span>

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a><span data-ttu-id="777ab-127">既存のプロジェクトに追加のヘルプ ページ</span><span class="sxs-lookup"><span data-stu-id="777ab-127">Adding Help Pages to an Existing Project</span></span>

<span data-ttu-id="777ab-128">ヘルプ ページは、NuGet パッケージ マネージャーを使用して既存の Web API プロジェクトに追加できます。</span><span class="sxs-lookup"><span data-stu-id="777ab-128">You can add help pages to an existing Web API project by using NuGet Package Manager.</span></span> <span data-ttu-id="777ab-129">このオプションは、"Web API"テンプレートよりも、別のプロジェクト テンプレートから開始するは便利です。</span><span class="sxs-lookup"><span data-stu-id="777ab-129">This option is useful you start from a different project template than the "Web API" template.</span></span>

<span data-ttu-id="777ab-130">**ツール**メニューの **ライブラリ パッケージ マネージャー**、し、**パッケージ マネージャー コンソール**します。</span><span class="sxs-lookup"><span data-stu-id="777ab-130">From the **Tools** menu, select **Library Package Manager**, and then select **Package Manager Console**.</span></span> <span data-ttu-id="777ab-131">[パッケージ マネージャー コンソール](http://docs.nuget.org/docs/start-here/using-the-package-manager-console)ウィンドウで、次のコマンドのいずれかを入力します。</span><span class="sxs-lookup"><span data-stu-id="777ab-131">In the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) window, type one of the following commands:</span></span>

<span data-ttu-id="777ab-132">**C#** アプリケーション。 `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span><span class="sxs-lookup"><span data-stu-id="777ab-132">For a **C#** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span></span>

<span data-ttu-id="777ab-133">**Visual Basic**アプリケーション。 `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span><span class="sxs-lookup"><span data-stu-id="777ab-133">For a **Visual Basic** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span></span>

<span data-ttu-id="777ab-134">C# と Visual basic の 2 つのパッケージがあります。</span><span class="sxs-lookup"><span data-stu-id="777ab-134">There are two packages, one for C# and one for Visual Basic.</span></span> <span data-ttu-id="777ab-135">プロジェクトに一致する 1 つを使用してください。</span><span class="sxs-lookup"><span data-stu-id="777ab-135">Make sure to use the one that matches your project.</span></span>

<span data-ttu-id="777ab-136">このコマンドは、必要なアセンブリをインストールし、(領域/HelpPage フォルダーにあります) のヘルプ ページの MVC ビューを追加します。</span><span class="sxs-lookup"><span data-stu-id="777ab-136">This command installs the necessary assemblies and adds the MVC views for the help pages (located in the Areas/HelpPage folder).</span></span> <span data-ttu-id="777ab-137">ヘルプ ページへのリンクを手動で追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="777ab-137">You'll need to manually add a link to the Help page.</span></span> <span data-ttu-id="777ab-138">URI は、/Help です。</span><span class="sxs-lookup"><span data-stu-id="777ab-138">The URI is /Help.</span></span> <span data-ttu-id="777ab-139">Razor ビューで、リンクを作成するには、次のように追加します。</span><span class="sxs-lookup"><span data-stu-id="777ab-139">To create a link in a razor view, add the following:</span></span>

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

<span data-ttu-id="777ab-140">また、領域を登録することを確認してください。</span><span class="sxs-lookup"><span data-stu-id="777ab-140">Also, make sure to register areas.</span></span> <span data-ttu-id="777ab-141">Global.asax ファイルで次のコードを追加、**アプリケーション\_開始**メソッドが存在しない場合。</span><span class="sxs-lookup"><span data-stu-id="777ab-141">In the Global.asax file, add the following code to the **Application\_Start** method, if it is not there already:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a><span data-ttu-id="777ab-142">API のドキュメントを追加します。</span><span class="sxs-lookup"><span data-stu-id="777ab-142">Adding API Documentation</span></span>

<span data-ttu-id="777ab-143">既定では、ヘルプ ページがあるドキュメントのプレース ホルダー文字列。</span><span class="sxs-lookup"><span data-stu-id="777ab-143">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="777ab-144">使用することができます[XML ドキュメント コメント](https://msdn.microsoft.com/library/b2s063f7.aspx)ドキュメントを作成します。</span><span class="sxs-lookup"><span data-stu-id="777ab-144">You can use [XML documentation comments](https://msdn.microsoft.com/library/b2s063f7.aspx) to create the documentation.</span></span> <span data-ttu-id="777ab-145">この機能を有効にするには、ファイル領域/HelpPage/アプリを開く\_Start/HelpPageConfig.cs、次の行をコメント解除します。</span><span class="sxs-lookup"><span data-stu-id="777ab-145">To enable this feature, open the file Areas/HelpPage/App\_Start/HelpPageConfig.cs and uncomment the following line:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

<span data-ttu-id="777ab-146">XML ドキュメントを有効にするようになりました。</span><span class="sxs-lookup"><span data-stu-id="777ab-146">Now enable XML documentation.</span></span> <span data-ttu-id="777ab-147">ソリューション エクスプ ローラーでプロジェクトを右クリックし、選択**プロパティ**します。</span><span class="sxs-lookup"><span data-stu-id="777ab-147">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="777ab-148">選択、**ビルド**ページ。</span><span class="sxs-lookup"><span data-stu-id="777ab-148">Select the **Build** page.</span></span>

![](creating-api-help-pages/_static/image6.png)

<span data-ttu-id="777ab-149">**出力**、確認**XML ドキュメント ファイル**します。</span><span class="sxs-lookup"><span data-stu-id="777ab-149">Under **Output**, check **XML documentation file**.</span></span> <span data-ttu-id="777ab-150">編集ボックスで、次のように入力します。"アプリ\_Data/XmlDocument.xml"。</span><span class="sxs-lookup"><span data-stu-id="777ab-150">In the edit box, type "App\_Data/XmlDocument.xml".</span></span>

![](creating-api-help-pages/_static/image7.png)

<span data-ttu-id="777ab-151">コードを次に、開く、 `ValuesController` /Controllers/ValuesControler.cs で定義されている API コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="777ab-151">Next, open the code for the `ValuesController` API controller, which is defined in /Controllers/ValuesControler.cs.</span></span> <span data-ttu-id="777ab-152">コント ローラーのメソッドには、いくつかのドキュメントのコメントを追加します。</span><span class="sxs-lookup"><span data-stu-id="777ab-152">Add some documentation comments to the controller methods.</span></span> <span data-ttu-id="777ab-153">例えば:</span><span class="sxs-lookup"><span data-stu-id="777ab-153">For example:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="777ab-154">ヒント: メソッド上の行にキャレットを配置する 3 つのスラッシュを入力すると、Visual Studio が自動的に挿入する XML 要素。</span><span class="sxs-lookup"><span data-stu-id="777ab-154">Tip: If you position the caret on the line above the method and type three forward slashes, Visual Studio automatically inserts the XML elements.</span></span> <span data-ttu-id="777ab-155">空白を入力します。</span><span class="sxs-lookup"><span data-stu-id="777ab-155">Then you can fill in the blanks.</span></span>


<span data-ttu-id="777ab-156">今すぐビルドし、もう一度アプリケーションを実行し、ヘルプ ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="777ab-156">Now build and run the application again, and navigate to the help pages.</span></span> <span data-ttu-id="777ab-157">API テーブル内のドキュメント文字列が表示されます。</span><span class="sxs-lookup"><span data-stu-id="777ab-157">The documentation strings should appear in the API table.</span></span>

![](creating-api-help-pages/_static/image8.png)

<span data-ttu-id="777ab-158">ヘルプ ページは、実行時に、XML ファイルから文字列を読み取ります。</span><span class="sxs-lookup"><span data-stu-id="777ab-158">The help page reads the strings from the XML file at run time.</span></span> <span data-ttu-id="777ab-159">(アプリケーションを展開するときに必ず XML ファイルをデプロイする。)</span><span class="sxs-lookup"><span data-stu-id="777ab-159">(When you deploy the application, make sure to deploy the XML file.)</span></span>

## <a name="under-the-hood"></a><span data-ttu-id="777ab-160">内部的には</span><span class="sxs-lookup"><span data-stu-id="777ab-160">Under the Hood</span></span>

<span data-ttu-id="777ab-161">ヘルプ ページがの上に構築された、**ための ApiExplorer**クラスは、Web API フレームワークの一部です。</span><span class="sxs-lookup"><span data-stu-id="777ab-161">The help pages are built on top of the **ApiExplorer** class, which is part of the Web API framework.</span></span> <span data-ttu-id="777ab-162">**ための ApiExplorer**クラスは、ヘルプ ページを作成するため、原材料を提供します。</span><span class="sxs-lookup"><span data-stu-id="777ab-162">The **ApiExplorer** class provides the raw material for creating a help page.</span></span> <span data-ttu-id="777ab-163">各 api では、**ための ApiExplorer**が含まれています、 **ApiDescription** API を記述します。</span><span class="sxs-lookup"><span data-stu-id="777ab-163">For each API, **ApiExplorer** contains an **ApiDescription** that describes the API.</span></span> <span data-ttu-id="777ab-164">このため、"API"は、HTTP メソッドと相対 URI の組み合わせとして定義されます。</span><span class="sxs-lookup"><span data-stu-id="777ab-164">For this purpose, an "API" is defined as the combination of HTTP method and relative URI.</span></span> <span data-ttu-id="777ab-165">たとえば、異なる Api がいくつか次に示します。</span><span class="sxs-lookup"><span data-stu-id="777ab-165">For example, here are some distinct APIs:</span></span>

- <span data-ttu-id="777ab-166">/Api/Products を取得します。</span><span class="sxs-lookup"><span data-stu-id="777ab-166">GET /api/Products</span></span>
- <span data-ttu-id="777ab-167">GET /api/Products/{id}</span><span class="sxs-lookup"><span data-stu-id="777ab-167">GET /api/Products/{id}</span></span>
- <span data-ttu-id="777ab-168">Api/製品を投稿します。</span><span class="sxs-lookup"><span data-stu-id="777ab-168">POST /api/Products</span></span>

<span data-ttu-id="777ab-169">コント ローラーのアクションは、複数の HTTP メソッドをサポートしている場合、**ための ApiExplorer**各メソッドを個別の API として扱われます。</span><span class="sxs-lookup"><span data-stu-id="777ab-169">If a controller action supports multiple HTTP methods, the **ApiExplorer** treats each method as a distinct API.</span></span>

<span data-ttu-id="777ab-170">API を非表示にする、**ための ApiExplorer**、追加、 **ApiExplorerSettings**属性を設定しアクション*IgnoreApi*を true にします。</span><span class="sxs-lookup"><span data-stu-id="777ab-170">To hide an API from the **ApiExplorer**, add the **ApiExplorerSettings** attribute to the action and set *IgnoreApi* to true.</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

<span data-ttu-id="777ab-171">コント ローラー全体を除外する、コント ローラーにこの属性を追加することもできます。</span><span class="sxs-lookup"><span data-stu-id="777ab-171">You can also add this attribute to the controller, to exclude the entire controller.</span></span>

<span data-ttu-id="777ab-172">ドキュメント文字列を取得するための ApiExplorer クラス、 **IDocumentationProvider**インターフェイス。</span><span class="sxs-lookup"><span data-stu-id="777ab-172">The ApiExplorer class gets documentation strings from the **IDocumentationProvider** interface.</span></span> <span data-ttu-id="777ab-173">ヘルプ ページ ライブラリは、既に説明したように**IDocumentationProvider** XML ドキュメントの文字列からドキュメントを取得します。</span><span class="sxs-lookup"><span data-stu-id="777ab-173">As you saw earlier, the Help Pages library provides an **IDocumentationProvider** that gets documentation from XML documentation strings.</span></span> <span data-ttu-id="777ab-174">コードは/Areas/HelpPage/XmlDocumentationProvider.cs にあります。</span><span class="sxs-lookup"><span data-stu-id="777ab-174">The code is located in /Areas/HelpPage/XmlDocumentationProvider.cs.</span></span> <span data-ttu-id="777ab-175">取得できますドキュメントの別のソースを記述して、独自**IDocumentationProvider**します。</span><span class="sxs-lookup"><span data-stu-id="777ab-175">You can get documentation from another source by writing your own **IDocumentationProvider**.</span></span> <span data-ttu-id="777ab-176">結び付けるを呼び出し、 **SetDocumentationProvider**で定義された拡張機能メソッド**HelpPageConfigurationExtensions**</span><span class="sxs-lookup"><span data-stu-id="777ab-176">To wire it up, call the **SetDocumentationProvider** extension method, defined in **HelpPageConfigurationExtensions**</span></span>

<span data-ttu-id="777ab-177">**ための ApiExplorer**を自動的に呼び出す、 **IDocumentationProvider**各 api のドキュメント文字列を取得するインターフェイス。</span><span class="sxs-lookup"><span data-stu-id="777ab-177">**ApiExplorer** automatically calls into the **IDocumentationProvider** interface to get documentation strings for each API.</span></span> <span data-ttu-id="777ab-178">それらを保存、**ドキュメント**のプロパティ、 **ApiDescription**と**ApiParameterDescription**オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="777ab-178">It stores them in the **Documentation** property of the **ApiDescription** and **ApiParameterDescription** objects.</span></span>

## <a name="next-steps"></a><span data-ttu-id="777ab-179">次の手順</span><span class="sxs-lookup"><span data-stu-id="777ab-179">Next Steps</span></span>

<span data-ttu-id="777ab-180">方法は、次のとおりのヘルプ ページに限定されません。</span><span class="sxs-lookup"><span data-stu-id="777ab-180">You aren't limited to the help pages shown here.</span></span> <span data-ttu-id="777ab-181">実際、**ための ApiExplorer**ヘルプ ページを作成するのには限定されません。</span><span class="sxs-lookup"><span data-stu-id="777ab-181">In fact, **ApiExplorer** is not limited to creating help pages.</span></span> <span data-ttu-id="777ab-182">Yao Huang Lin はすぐ概説するいくつかの優れたブログの投稿を書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="777ab-182">Yao Huang Lin has written some great blog posts to get you thinking out of the box:</span></span>

- [<span data-ttu-id="777ab-183">ASP.NET Web API ヘルプ ページへの単純なテスト クライアントの追加</span><span class="sxs-lookup"><span data-stu-id="777ab-183">Adding a simple Test Client to ASP.NET Web API Help Page</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [<span data-ttu-id="777ab-184">ASP.NET Web API ヘルプ ページ自己ホスト型サービスで作業を行う</span><span class="sxs-lookup"><span data-stu-id="777ab-184">Making ASP.NET Web API Help Page work on self-hosted services</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [<span data-ttu-id="777ab-185">ヘルプ ページ (またはクライアント) の ASP.NET Web API のデザイン時の生成</span><span class="sxs-lookup"><span data-stu-id="777ab-185">Design-time generation of help page (or client) for ASP.NET Web API</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [<span data-ttu-id="777ab-186">高度なヘルプ ページのカスタマイズ</span><span class="sxs-lookup"><span data-stu-id="777ab-186">Advanced Help Page customizations</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
