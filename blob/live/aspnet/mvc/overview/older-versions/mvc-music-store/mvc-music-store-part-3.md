---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: "パート 3: ビューと ViewModels |Microsoft ドキュメント"
author: jongalloway
description: "このチュートリアルの系列では、すべての ASP.NET MVC Music Store サンプル アプリケーションをビルドする手順について説明します。 パート 3 は、ビューと ViewModels について説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 5b38f88283c5d2d93f0bab283dbd9451855d95e3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="part-3-views-and-viewmodels"></a><span data-ttu-id="e6172-104">パート 3: ビューと ViewModels</span><span class="sxs-lookup"><span data-stu-id="e6172-104">Part 3: Views and ViewModels</span></span>
====================
<span data-ttu-id="e6172-105">によって[Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="e6172-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="e6172-106">MVC Music Store は、チュートリアル アプリケーションを紹介し、ASP.NET MVC と Visual Studio web 開発を使用する方法の詳細な手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="e6172-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="e6172-107">MVC 音楽ストアは、オンラインにして、音楽のアルバムを販売し、基本的なサイトの管理、ユーザー サインインおよび買い物カゴの機能を実装する軽量なサンプル ストアの実装です。</span><span class="sxs-lookup"><span data-stu-id="e6172-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="e6172-108">このチュートリアルの系列では、すべての ASP.NET MVC Music Store サンプル アプリケーションをビルドする手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="e6172-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="e6172-109">パート 3 は、ビューと ViewModels について説明します。</span><span class="sxs-lookup"><span data-stu-id="e6172-109">Part 3 covers Views and ViewModels.</span></span>


<span data-ttu-id="e6172-110">これまでおしただけされた文字列から返すコント ローラーのアクション。</span><span class="sxs-lookup"><span data-stu-id="e6172-110">So far we've just been returning strings from controller actions.</span></span> <span data-ttu-id="e6172-111">コント ローラーのしくみを理解する便利な方法がない方法する実際の web アプリケーションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="e6172-111">That's a nice way to get an idea of how controllers work, but it's not how you'd want to build a real web application.</span></span> <span data-ttu-id="e6172-112">サイト、ブラウザーに HTML を生成する優れた方法としての目的に – いずれかのテンプレート ファイルを使用して、HTML コンテンツをより簡単にカスタマイズおを返信します。</span><span class="sxs-lookup"><span data-stu-id="e6172-112">We are going to want a better way to generate HTML back to browsers visiting our site – one where we can use template files to more easily customize the HTML content send back.</span></span> <span data-ttu-id="e6172-113">まさにビューが何を実行します。</span><span class="sxs-lookup"><span data-stu-id="e6172-113">That's exactly what Views do.</span></span>

## <a name="adding-a-view-template"></a><span data-ttu-id="e6172-114">ビューのテンプレートを追加します。</span><span class="sxs-lookup"><span data-stu-id="e6172-114">Adding a View template</span></span>

<span data-ttu-id="e6172-115">ビュー テンプレートを使用するを返す、ActionResult HomeController インデックス メソッドを変更し、以下のように、View() が返されるようにおします。</span><span class="sxs-lookup"><span data-stu-id="e6172-115">To use a view-template, we'll change the HomeController Index method to return an ActionResult, and have it return View(), like below:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

<span data-ttu-id="e6172-116">上記の変更では、文字列を返すのではなくを示します、代わりに、結果を生成する"View"を使用します。</span><span class="sxs-lookup"><span data-stu-id="e6172-116">The above change indicates that instead of returned a string, we instead want to use a "View" to generate a result back.</span></span>

<span data-ttu-id="e6172-117">プロジェクトに適切なビュー テンプレートを追加します。</span><span class="sxs-lookup"><span data-stu-id="e6172-117">We'll now add an appropriate View template to our project.</span></span> <span data-ttu-id="e6172-118">これを行うおありますインデックス アクション メソッド内でテキストのカーソルの位置を右クリックし、「ビューの追加」です。</span><span class="sxs-lookup"><span data-stu-id="e6172-118">To do this we'll position the text cursor within the Index action method, then right-click and select "Add View".</span></span> <span data-ttu-id="e6172-119">ビューの追加 ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e6172-119">This will bring up the Add View dialog:</span></span>

<span data-ttu-id="e6172-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e6172-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span></span>

<span data-ttu-id="e6172-121">「ビューの追加」ダイアログでは、迅速かつ簡単には、テンプレート ファイルの表示を生成できます。</span><span class="sxs-lookup"><span data-stu-id="e6172-121">The "Add View" dialog allows us to quickly and easily generate View template files.</span></span> <span data-ttu-id="e6172-122">既定では、「ビューの追加」ダイアログでは、それを使用するアクション メソッドに一致するように作成するビュー テンプレートの名前が事前に入力します。</span><span class="sxs-lookup"><span data-stu-id="e6172-122">By default the "Add View" dialog pre-populates the name of the View template to create so that it matches the action method that will use it.</span></span> <span data-ttu-id="e6172-123">当社 HomeController の Index() アクション メソッド内で「ビューの追加」のコンテキスト メニューを使用しているため「ビューの追加」このダイアログ ボックスは既定ではあらかじめ値が入力されたビュー名と"Index"を持ちます。</span><span class="sxs-lookup"><span data-stu-id="e6172-123">Because we used the "Add View" context menu within the Index() action method of our HomeController, the "Add View" dialog above has "Index" as the view name pre-populated by default.</span></span> <span data-ttu-id="e6172-124">このダイアログ ボックスでオプションのいずれかを変更するために、[追加] ボタンをクリックして必要はありません。</span><span class="sxs-lookup"><span data-stu-id="e6172-124">We don't need to change any of the options on this dialog, so click the Add button.</span></span>

<span data-ttu-id="e6172-125">[追加] ボタンをクリックしてお Visual Web Developer は、フォルダーを作成する場合、\Views\Home ディレクトリにご利用の米国ビュー テンプレートが存在しない新しい Index.cshtml を作成します。</span><span class="sxs-lookup"><span data-stu-id="e6172-125">When we click the Add button, Visual Web Developer will create a new Index.cshtml view template for us in the \Views\Home directory, creating the folder if doesn't already exist.</span></span>

![](mvc-music-store-part-3/_static/image2.png)

<span data-ttu-id="e6172-126">"Index.cshtml"ファイルの名前とフォルダーの場所は重要であり、既定の ASP.NET MVC の名前付け規則に従います。</span><span class="sxs-lookup"><span data-stu-id="e6172-126">The name and folder location of the "Index.cshtml" file is important, and follows the default ASP.NET MVC naming conventions.</span></span> <span data-ttu-id="e6172-127">ディレクトリ名、\Views\Home、コント ローラー - HomeController という名前に一致します。</span><span class="sxs-lookup"><span data-stu-id="e6172-127">The directory name, \Views\Home, matches the controller - which is named HomeController.</span></span> <span data-ttu-id="e6172-128">ビューのテンプレート名、インデックス、ビューを表示するコント ローラー アクション メソッドに一致します。</span><span class="sxs-lookup"><span data-stu-id="e6172-128">The view template name, Index, matches the controller action method which will be displaying the view.</span></span>

<span data-ttu-id="e6172-129">ASP.NET MVC では、この名前付け規則を使用してビューを返すときに、名またはビュー テンプレートの場所を明示的に指定する必要があるようにすることができます。</span><span class="sxs-lookup"><span data-stu-id="e6172-129">ASP.NET MVC allows us to avoid having to explicitly specify the name or location of a view template when we use this naming convention to return a view.</span></span> <span data-ttu-id="e6172-130">既定で表示する \Views\Home\Index.cshtml ビュー テンプレート、HomeController 内で次のようなコードを記述するとき。</span><span class="sxs-lookup"><span data-stu-id="e6172-130">It will by default render the \Views\Home\Index.cshtml view template when we write code like below within our HomeController:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

<span data-ttu-id="e6172-131">Visual Web Developer は、作成され、「ビューの追加」ダイアログ ボックスで [追加] ボタンをクリックした後に"Index.cshtml"ビューのテンプレートを開きます。</span><span class="sxs-lookup"><span data-stu-id="e6172-131">Visual Web Developer created and opened the "Index.cshtml" view template after we clicked the "Add" button within the "Add View" dialog.</span></span> <span data-ttu-id="e6172-132">Index.cshtml の内容は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="e6172-132">The contents of Index.cshtml are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

<span data-ttu-id="e6172-133">このビューは、ASP.NET Web フォームおよび ASP.NET MVC の以前のバージョンで使用される、Web フォーム ビュー エンジンよりも簡潔である Razor 構文を使用しています。</span><span class="sxs-lookup"><span data-stu-id="e6172-133">This view is using the Razor syntax, which is more concise than the Web Forms view engine used in ASP.NET Web Forms and previous versions of ASP.NET MVC.</span></span> <span data-ttu-id="e6172-134">Web フォーム ビュー エンジンは ASP.NET MVC 3 で使用できますが、多くの開発者が、Razor ビュー エンジンが ASP.NET MVC の開発を非常に収まることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e6172-134">The Web Forms view engine is still available in ASP.NET MVC 3, but many developers find that the Razor view engine fits ASP.NET MVC development really well.</span></span>

<span data-ttu-id="e6172-135">最初の 3 つの行は、ViewBag.Title を使用してページのタイトルを設定します。</span><span class="sxs-lookup"><span data-stu-id="e6172-135">The first three lines set the page title using ViewBag.Title.</span></span> <span data-ttu-id="e6172-136">拝見このしくみさらに詳しくすぐに、まずみましょう更新テキスト見出しのテキストし、ページを表示します。</span><span class="sxs-lookup"><span data-stu-id="e6172-136">We'll look at how this works in more detail soon, but first let's update the text heading text and view the page.</span></span> <span data-ttu-id="e6172-137">更新プログラム、 &lt;h2&gt;を次に示すように、「このは、ホーム ページ」を言うタグ。</span><span class="sxs-lookup"><span data-stu-id="e6172-137">Update the &lt;h2&gt; tag to say "This is the Home Page" as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

<span data-ttu-id="e6172-138">新しいテキストがホーム ページに表示されているアプリケーションの表示を実行します。</span><span class="sxs-lookup"><span data-stu-id="e6172-138">Running the application shows that our new text is visible on the home page.</span></span>

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a><span data-ttu-id="e6172-139">一般的なサイトの要素のレイアウトを使用します。</span><span class="sxs-lookup"><span data-stu-id="e6172-139">Using a Layout for common site elements</span></span>

<span data-ttu-id="e6172-140">ほとんどの web サイトは多くのページ間で共有されるコンテンツがある: ナビゲーション、フッター、ロゴのイメージ、スタイル シート参照などです。Razor ビュー エンジンにより、これと呼ばれるページを使用して管理しやすい\_Layout.cshtml/ビュー/共有フォルダー内にご利用の米国自動的に作成されました。</span><span class="sxs-lookup"><span data-stu-id="e6172-140">Most websites have content which is shared between many pages: navigation, footers, logo images, stylesheet references, etc. The Razor view engine makes this easy to manage using a page called \_Layout.cshtml which has automatically been created for us inside the /Views/Shared folder.</span></span>

![](mvc-music-store-part-3/_static/image4.png)

<span data-ttu-id="e6172-141">下に表示される内容を表示するには、このフォルダーをダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="e6172-141">Double-click on this folder to view the contents, which are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

<span data-ttu-id="e6172-142">個々 のビューからコンテンツを表示する、 @RenderBody() コマンドとの外側に記述する共通の内容に追加することができます、 \_Layout.cshtml マークアップ。</span><span class="sxs-lookup"><span data-stu-id="e6172-142">The content from our individual views will be displayed by the @RenderBody() command, and any common content that we want to appear outside of that can be added to the \_Layout.cshtml markup.</span></span> <span data-ttu-id="e6172-143">サイトでは、すべてのページで、ホーム ページおよびストアの領域にリンクに共通のヘッダーを持つテンプレートをすぐ上に追加する予定の MVC Music Store がいいでしょう@RenderBody() のステートメント。</span><span class="sxs-lookup"><span data-stu-id="e6172-143">We'll want our MVC Music Store to have a common header with links to our Home page and Store area on all pages in the site, so we'll add that to the template directly above that @RenderBody() statement.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a><span data-ttu-id="e6172-144">スタイル シートを更新</span><span class="sxs-lookup"><span data-stu-id="e6172-144">Updating the StyleSheet</span></span>

<span data-ttu-id="e6172-145">空のプロジェクト テンプレートには、検証メッセージを表示するために使用するスタイルだけを含む非常に簡素化された CSS ファイルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="e6172-145">The empty project template includes a very streamlined CSS file which just includes styles used to display validation messages.</span></span> <span data-ttu-id="e6172-146">このデザイナーは、いくつか追加 CSS およびイメージを今すぐに追加されますので、サイトのルック アンド フィールを定義を提供しています。</span><span class="sxs-lookup"><span data-stu-id="e6172-146">Our designer has provided some additional CSS and images to define the look and feel for our site, so we'll add those in now.</span></span>

<span data-ttu-id="e6172-147">更新された CSS ファイルとイメージが記載されている MvcMusicStore Assets.zip のコンテンツのディレクトリに含まれる[MVC Music Store](https://github.com/evilDave/MVC-Music-Store)です。</span><span class="sxs-lookup"><span data-stu-id="e6172-147">The updated CSS file and Images are included in the Content directory of MvcMusicStore-Assets.zip which is available at [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span> <span data-ttu-id="e6172-148">Windows エクスプ ローラーで、これらの両方を選択し、次のように Visual Web Developer で、このソリューションのコンテンツ フォルダーにドロップおします。</span><span class="sxs-lookup"><span data-stu-id="e6172-148">We'll select both of them in Windows Explorer and drop them into our Solution's Content folder in Visual Web Developer, as shown below:</span></span>

![](mvc-music-store-part-3/_static/image5.png)

<span data-ttu-id="e6172-149">Site.css ファイルを上書きするかどうかを確認するよう求められます。</span><span class="sxs-lookup"><span data-stu-id="e6172-149">You'll be asked to confirm if you want to overwrite the existing Site.css file.</span></span> <span data-ttu-id="e6172-150">[はい] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e6172-150">Click Yes.</span></span>

![](mvc-music-store-part-3/_static/image6.png)

<span data-ttu-id="e6172-151">アプリケーションのコンテンツのフォルダーは次のように表示されます。</span><span class="sxs-lookup"><span data-stu-id="e6172-151">The Content folder of your application will now appear as follows:</span></span>

![](mvc-music-store-part-3/_static/image7.png)

<span data-ttu-id="e6172-152">今すぐアプリケーションを実行し、ホーム ページで、変更の外観を確認してみましょう。</span><span class="sxs-lookup"><span data-stu-id="e6172-152">Now let's run the application and see how our changes look on the Home page.</span></span>

![](mvc-music-store-part-3/_static/image8.png)

- <span data-ttu-id="e6172-153">変更点を確認してみましょう。「HomeController の Index アクション メソッドを見つけて、テンプレートの表示後、標準の名前付け規則ので、コードに"戻り View()"が呼び出されていなくても、\Views\Home\Index.cshtmlView テンプレートが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e6172-153">Let's review what's changed: The HomeController's Index action method found and displayed the \Views\Home\Index.cshtmlView template, even though our code called "return View()", because our View template followed the standard naming convention.</span></span>
- <span data-ttu-id="e6172-154">ホーム ページには、\Views\Home\Index.cshtml ビュー テンプレート内で定義されている単純なへようこそ メッセージが表示されています。</span><span class="sxs-lookup"><span data-stu-id="e6172-154">The Home Page is displaying a simple welcome message that is defined within the \Views\Home\Index.cshtml view template.</span></span>
- <span data-ttu-id="e6172-155">ホーム ページを使用して、 \_Layout.cshtml テンプレートおよびウェルカム メッセージが標準サイト HTML レイアウト内に含まれるようにします。</span><span class="sxs-lookup"><span data-stu-id="e6172-155">The Home Page is using our \_Layout.cshtml template, and so the welcome message is contained within the standard site HTML layout.</span></span>

## <a name="using-a-model-to-pass-information-to-our-view"></a><span data-ttu-id="e6172-156">モデルを使用して、ビューに情報を渡す</span><span class="sxs-lookup"><span data-stu-id="e6172-156">Using a Model to pass information to our View</span></span>

<span data-ttu-id="e6172-157">だけハードコードされた HTML を表示するビュー テンプレートがない非常に興味深い web サイトを作成しようとしています。</span><span class="sxs-lookup"><span data-stu-id="e6172-157">A View template that just displays hardcoded HTML isn't going to make a very interesting web site.</span></span> <span data-ttu-id="e6172-158">動的な web サイトを作成するには代わりにします、コント ローラーのアクションからテンプレートを表示する情報を渡します。</span><span class="sxs-lookup"><span data-stu-id="e6172-158">To create a dynamic web site, we'll instead want to pass information from our controller actions to our view templates.</span></span>

<span data-ttu-id="e6172-159">パターンでは、モデル ビュー コント ローラー、モデルを指す用語は、アプリケーションでデータを表現するオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="e6172-159">In the Model-View-Controller pattern, the term Model refers to objects which represent the data in the application.</span></span> <span data-ttu-id="e6172-160">多くの場合、モデル オブジェクトは、データベースのテーブルに対応しますが、する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="e6172-160">Often, model objects correspond to tables in your database, but they don't have to.</span></span>

<span data-ttu-id="e6172-161">ActionResult を返すことがコント ローラー アクション メソッドは、ビューにモデル オブジェクトを渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="e6172-161">Controller action methods which return an ActionResult can pass a model object to the view.</span></span> <span data-ttu-id="e6172-162">これにより、コント ローラーをクリーンにパッケージを応答を生成し、ビュー テンプレートにこの情報を渡すために必要なすべての情報を使用して、適切な HTML 応答を生成します。</span><span class="sxs-lookup"><span data-stu-id="e6172-162">This allows a Controller to cleanly package up all the information needed to generate a response, and then pass this information off to a View template to use to generate the appropriate HTML response.</span></span> <span data-ttu-id="e6172-163">これは、最も簡単に理解操作で確認することによって、それでは始めましょう。</span><span class="sxs-lookup"><span data-stu-id="e6172-163">This is easiest to understand by seeing it in action, so let's get started.</span></span>

<span data-ttu-id="e6172-164">まず、ストア内のジャンルおよびアルバムを表すために一部のモデル クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="e6172-164">First we'll create some Model classes to represent Genres and Albums within our store.</span></span> <span data-ttu-id="e6172-165">ジャンル クラスを作成して開始しましょう。</span><span class="sxs-lookup"><span data-stu-id="e6172-165">Let's start by creating a Genre class.</span></span> <span data-ttu-id="e6172-166">プロジェクト内で"Models"フォルダーを右クリックして、「クラスの追加」オプションを選択して、ファイルの名前を"Genre.cs"。</span><span class="sxs-lookup"><span data-stu-id="e6172-166">Right-click the "Models" folder within your project, choose the "Add Class" option, and name the file "Genre.cs".</span></span>

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

<span data-ttu-id="e6172-167">作成されたクラスにパブリックの文字列名プロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="e6172-167">Then add a public string Name property to the class that was created:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

<span data-ttu-id="e6172-168">*注: 参考までに、、{get; に設定} 表記が行っている # の自動実装プロパティの機能を使用します。これにより、プロパティの利点をバッキング フィールドを宣言することを必要とせずします。*</span><span class="sxs-lookup"><span data-stu-id="e6172-168">*Note: In case you're wondering, the { get; set; } notation is making use of C#'s auto-implemented properties feature. This gives us the benefits of a property without requiring us to declare a backing field.*</span></span>

<span data-ttu-id="e6172-169">次に、タイトルとジャンル プロパティを持つアルバム クラス (Album.cs という名前) を作成する同じ手順に従います。</span><span class="sxs-lookup"><span data-stu-id="e6172-169">Next, follow the same steps to create an Album class (named Album.cs) that has a Title and a Genre property:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

<span data-ttu-id="e6172-170">今すぐ StoreController モデルからの動的な情報を表示するビューを使用して変更できます。</span><span class="sxs-lookup"><span data-stu-id="e6172-170">Now we can modify the StoreController to use Views which display dynamic information from our Model.</span></span> <span data-ttu-id="e6172-171">デモンストレーション用に今すぐおという名前の要求 ID に基づいた、アルバム場合は、以下のビューと同様にその情報を表示おでした。</span><span class="sxs-lookup"><span data-stu-id="e6172-171">If - for demonstration purposes right now - we named our Albums based on the request ID, we could display that information as in the view below.</span></span>

![](mvc-music-store-part-3/_static/image10.png)

<span data-ttu-id="e6172-172">1 枚のアルバムの情報が表示されるように、ストアの詳細アクションを変更することで始めます。</span><span class="sxs-lookup"><span data-stu-id="e6172-172">We'll start by changing the Store Details action so it shows the information for a single album.</span></span> <span data-ttu-id="e6172-173">先頭に"using"ステートメントを追加、 **StoreControllers**クラス MvcMusicStore.Models 名前空間を含めるために、アルバム クラスを使用するたびに、MvcMusicStore.Models.Album を入力する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="e6172-173">Add a "using" statement to the top of the **StoreControllers** class to include the MvcMusicStore.Models namespace, so we don't need to type MvcMusicStore.Models.Album every time we want to use the album class.</span></span> <span data-ttu-id="e6172-174">そのクラスの"using"セクションが表示されます以下のようです。</span><span class="sxs-lookup"><span data-stu-id="e6172-174">The "usings" section of that class should now appear as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

<span data-ttu-id="e6172-175">次に、更新されます詳細コント ローラーのアクション、文字列ではなく ActionResult を返すように HomeController のインデックスのメソッドで行ったようにします。</span><span class="sxs-lookup"><span data-stu-id="e6172-175">Next, we'll update the Details controller action so that it returns an ActionResult rather than a string, as we did with the HomeController's Index method.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

<span data-ttu-id="e6172-176">今すぐをビューにアルバム オブジェクトを返すロジックを変更できます。</span><span class="sxs-lookup"><span data-stu-id="e6172-176">Now we can modify the logic to return an Album object to the view.</span></span> <span data-ttu-id="e6172-177">このチュートリアルで後ほどおはするデータを取得する、データベースからは、今すぐおは「ダミー データ」作業を開始します。</span><span class="sxs-lookup"><span data-stu-id="e6172-177">Later in this tutorial we will be retrieving the data from a database – but for right now we will use "dummy data" to get started.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

<span data-ttu-id="e6172-178">*注: c# 慣れていない場合は、可能性がありますを想定する、var を使用してあることを意味、アルバム変数遅延バインディング。正しくありません: - 内容に基づいておしている変数を割り当てると、そのアルバムがアルバムの種類を決定するため、コンパイル時のチェックと Visual Studio コード エディター、アルバムの種類として、アルバムのローカル変数をコンパイルする型の推定を使用して、c# コンパイラサポートしてください。*</span><span class="sxs-lookup"><span data-stu-id="e6172-178">*Note: If you're unfamiliar with C#, you may assume that using var means that our album variable is late-bound. That's not correct – the C# compiler is using type-inference based on what we're assigning to the variable to determine that album is of type Album and compiling the local album variable as an Album type, so we get compile-time checking and Visual Studio code-editor support.*</span></span>

<span data-ttu-id="e6172-179">HTML 応答を生成する、アルバムを使用する表示テンプレートを今すぐ作成してみましょう。</span><span class="sxs-lookup"><span data-stu-id="e6172-179">Let's now create a View template that uses our Album to generate an HTML response.</span></span> <span data-ttu-id="e6172-180">その前に、新しく作成したアルバム クラスの概要ビューの追加 ダイアログを認識するようにプロジェクトをビルドする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e6172-180">Before we do that we need to build the project so that the Add View dialog knows about our newly created Album class.</span></span> <span data-ttu-id="e6172-181">ことができます、プロジェクトを選択してビルド Debug⇨Build MvcMusicStore メニュー項目 (、余分なクレジットのすることができます、ctrl キーを押し-b Shift ショートカットを使用してプロジェクトをビルドします)。</span><span class="sxs-lookup"><span data-stu-id="e6172-181">You can build the project by selecting the Debug⇨Build MvcMusicStore menu item (for extra credit, you can use the Ctrl-Shift-B shortcut to build the project).</span></span>

![](mvc-music-store-part-3/_static/image11.png)

<span data-ttu-id="e6172-182">これで、サポートするクラスを設定しました。 は、当社ビュー テンプレートをビルドする準備ができました。</span><span class="sxs-lookup"><span data-stu-id="e6172-182">Now that we've set up our supporting classes, we're ready to build our View template.</span></span> <span data-ttu-id="e6172-183">詳細メソッド内で右クリックし、[ビューの追加] を選択</span><span class="sxs-lookup"><span data-stu-id="e6172-183">Right-click within the Details method and select "Add View…"</span></span> <span data-ttu-id="e6172-184">コンテキスト メニュー。</span><span class="sxs-lookup"><span data-stu-id="e6172-184">from the context menu.</span></span>

![](mvc-music-store-part-3/_static/image12.png)

<span data-ttu-id="e6172-185">同じように、テンプレートを使用するには、新しいビュー テンプレートを作成しようとしています。</span><span class="sxs-lookup"><span data-stu-id="e6172-185">We are going to create a new View template like we did before with the HomeController.</span></span> <span data-ttu-id="e6172-186">StoreController から、作成するためが既定では生成されます \Views\Store\Index.cshtml ファイルで。</span><span class="sxs-lookup"><span data-stu-id="e6172-186">Because we are creating it from the StoreController it will by default be generated in a \Views\Store\Index.cshtml file.</span></span>

<span data-ttu-id="e6172-187">異なり前に、なっています「作成は、厳密に型指定された」ビューのチェック ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="e6172-187">Unlike before, we are going to check the "Create a strongly-typed" view checkbox.</span></span> <span data-ttu-id="e6172-188">「ビューのデータのクラス」ドロップ ダウン リスト内で、「アルバム」クラスを選択し、いきます。</span><span class="sxs-lookup"><span data-stu-id="e6172-188">We are then going to select our "Album" class within the "View data-class" drop-downlist.</span></span> <span data-ttu-id="e6172-189">これにより、「ビューの追加」ダイアログを使用するようにオブジェクトが渡されるアルバムを期待しているビュー テンプレートを作成します。</span><span class="sxs-lookup"><span data-stu-id="e6172-189">This will cause the "Add View" dialog to create a View template that expects that an Album object will be passed to it to use.</span></span>

![](mvc-music-store-part-3/_static/image13.png)

<span data-ttu-id="e6172-190">[追加] ボタンをクリックしたとき、\Views\Store\Details.cshtml ビュー テンプレートが作成された、次のコードを含むです。</span><span class="sxs-lookup"><span data-stu-id="e6172-190">When we click the "Add" button our \Views\Store\Details.cshtml View template will be created, containing the following code.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

<span data-ttu-id="e6172-191">このビューが厳密に型指定された、アルバム クラスであることを示します、最初の行に注意してください。</span><span class="sxs-lookup"><span data-stu-id="e6172-191">Notice the first line, which indicates that this view is strongly-typed to our Album class.</span></span> <span data-ttu-id="e6172-192">されて成功したアルバムのオブジェクト モデルのプロパティを簡単にアクセスしても、Visual Web Developer エディターの IntelliSense の利点があるため、Razor ビュー エンジンが認識しています。</span><span class="sxs-lookup"><span data-stu-id="e6172-192">The Razor view engine understands that it has been passed an Album object, so we can easily access model properties and even have the benefit of IntelliSense in the Visual Web Developer editor.</span></span>

<span data-ttu-id="e6172-193">更新プログラム、 &lt;h2&gt;タグを次のように表示するには、その行を変更することにより、アルバムのタイトルのプロパティが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e6172-193">Update the &lt;h2&gt; tag so it displays the Album's Title property by modifying that line to appear as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

<span data-ttu-id="e6172-194">後にピリオドを入力すると、IntelliSense がトリガーされることに注意してください、@Modelキーワード、プロパティとアルバム クラスでサポートされるメソッドを表示します。</span><span class="sxs-lookup"><span data-stu-id="e6172-194">Notice that IntelliSense is triggered when you enter the period after the @Model keyword, showing the properties and methods that the Album class supports.</span></span>

<span data-ttu-id="e6172-195">みましょう今すぐプロジェクトを再実行してストア/詳細/5 URL を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e6172-195">Let's now re-run our project and visit the /Store/Details/5 URL.</span></span> <span data-ttu-id="e6172-196">次のようなアルバムの詳細を会いしましょう。</span><span class="sxs-lookup"><span data-stu-id="e6172-196">We'll see details of an Album like below.</span></span>

![](mvc-music-store-part-3/_static/image14.png)

<span data-ttu-id="e6172-197">今すぐストア参照アクション メソッドのような更新プログラムにしましょう。</span><span class="sxs-lookup"><span data-stu-id="e6172-197">Now we'll make a similar update for the Store Browse action method.</span></span> <span data-ttu-id="e6172-198">ActionResult、それを返すように、メソッドを更新し、新しいジャンル オブジェクトを作成し、ビューを返します、メソッドのロジックを変更します。</span><span class="sxs-lookup"><span data-stu-id="e6172-198">Update the method so it returns an ActionResult, and modify the method logic so it creates a new Genre object and returns it to the View.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

<span data-ttu-id="e6172-199">参照メソッドを右クリックし、[ビューの追加] を選択</span><span class="sxs-lookup"><span data-stu-id="e6172-199">Right-click in the Browse method and select "Add View…"</span></span> <span data-ttu-id="e6172-200">コンテキスト メニューからは厳密に型指定されたビューを追加し、厳密に型指定をジャンル クラスに追加します。</span><span class="sxs-lookup"><span data-stu-id="e6172-200">from the context menu, then add a View that is strongly-typed add a strongly typed to the Genre class.</span></span>

![](mvc-music-store-part-3/_static/image15.png)

<span data-ttu-id="e6172-201">更新プログラム、 &lt;h2&gt;ジャンル情報を表示する (/Views/Store/Browse.cshtml) でコードのビュー内の要素。</span><span class="sxs-lookup"><span data-stu-id="e6172-201">Update the &lt;h2&gt; element in the view code (in /Views/Store/Browse.cshtml) to display the Genre information.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

<span data-ttu-id="e6172-202">今すぐみましょう、プロジェクトを再実行し、ストア/[参照] を参照しますか?ジャンル Disco URL を = です。</span><span class="sxs-lookup"><span data-stu-id="e6172-202">Now let's re-run our project and browse to the /Store/Browse?Genre=Disco URL.</span></span> <span data-ttu-id="e6172-203">以下のように表示される [参照] ページは表示されます。</span><span class="sxs-lookup"><span data-stu-id="e6172-203">We'll see the Browse page displayed like below.</span></span>

![](mvc-music-store-part-3/_static/image16.png)

<span data-ttu-id="e6172-204">最後に少し複雑な更新プログラムを確認してみましょう、**インデックスの格納**アクション メソッドと、ストア内のすべてのジャンルの一覧を表示するビュー。</span><span class="sxs-lookup"><span data-stu-id="e6172-204">Finally, let's make a slightly more complex update to the **Store Index** action method and view to display a list of all the Genres in our store.</span></span> <span data-ttu-id="e6172-205">1 つのジャンルだけではなく、モデル オブジェクトとしてジャンルの一覧を使用して、実行します。</span><span class="sxs-lookup"><span data-stu-id="e6172-205">We'll do that by using a List of Genres as our model object, rather than just a single Genre.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

<span data-ttu-id="e6172-206">ストアのインデックス アクション メソッドを右クリックし、ビューのようにする前には、モデル クラスとしてジャンルを選択し、追加 を選択します。</span><span class="sxs-lookup"><span data-stu-id="e6172-206">Right-click in the Store Index action method and select Add View as before, select Genre as the Model class, and press the Add button.</span></span>

![](mvc-music-store-part-3/_static/image17.png)

<span data-ttu-id="e6172-207">最初に変更します、@modelビューがいくつかのジャンルを予想することを示すために宣言が 1 つだけではなくオブジェクトします。</span><span class="sxs-lookup"><span data-stu-id="e6172-207">First we'll change the @model declaration to indicate that the view will be expecting several Genre objects rather than just one.</span></span> <span data-ttu-id="e6172-208">次のように/Store/Index.cshtml の最初の行を変更します。</span><span class="sxs-lookup"><span data-stu-id="e6172-208">Change the first line of /Store/Index.cshtml to read as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

<span data-ttu-id="e6172-209">これは、これは、ジャンルのいくつかのオブジェクトが保持できるモデル オブジェクトを作成する、Razor ビュー エンジンを指示します。</span><span class="sxs-lookup"><span data-stu-id="e6172-209">This tells the Razor view engine that it will be working with a model object that can hold several Genre objects.</span></span> <span data-ttu-id="e6172-210">お使いの IEnumerable&lt;ジャンル&gt;一覧ではなく&lt;ジャンル&gt;より汎用的なので、IEnumerable インターフェイスをサポートするオブジェクトの種類に後で、モデルの種類を変更することを許可します。</span><span class="sxs-lookup"><span data-stu-id="e6172-210">We're using an IEnumerable&lt;Genre&gt; rather than a List&lt;Genre&gt; since it's more generic, allowing us to change our model type later to any object type that supports the IEnumerable interface.</span></span>

<span data-ttu-id="e6172-211">次に、下の完成したビューのコードで示すように、モデル内のジャンル オブジェクトをループ処理します。</span><span class="sxs-lookup"><span data-stu-id="e6172-211">Next, we'll loop through the Genre objects in the model as shown in the completed view code below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

<span data-ttu-id="e6172-212">通知がある完全に IntelliSense サポートはこのコードを入力して、ときに入力できるように"@Model"。</span><span class="sxs-lookup"><span data-stu-id="e6172-212">Notice that we have full IntelliSense support as we enter this code, so that when we type "@Model."</span></span> <span data-ttu-id="e6172-213">ここには、すべてのメソッドと型ジャンルの IEnumerable でサポートされるプロパティが参照してください。</span><span class="sxs-lookup"><span data-stu-id="e6172-213">we see all methods and properties supported by an IEnumerable of type Genre.</span></span>

![](mvc-music-store-part-3/_static/image18.png)

<span data-ttu-id="e6172-214">"Foreach"ループ内では、Visual Web Developer は、各項目の型はジャンル、IntelliSence ごとに表示するため、ジャンル型を認識します。</span><span class="sxs-lookup"><span data-stu-id="e6172-214">Within our "foreach" loop, Visual Web Developer knows that each item is of type Genre, so we see IntelliSence for each the Genre type.</span></span>

![](mvc-music-store-part-3/_static/image19.png)

<span data-ttu-id="e6172-215">次に、スキャフォールディング機能は、ジャンル オブジェクトを検査し、それぞれが持つこと、Name プロパティをループ処理して出力するようにを決定します。また、個々 の項目を編集、詳細、および削除のリンクを生成します。</span><span class="sxs-lookup"><span data-stu-id="e6172-215">Next, the scaffolding feature examined the Genre object and determined that each will have a Name property, so it loops through and writes them out. It also generates Edit, Details, and Delete links to each individual item.</span></span> <span data-ttu-id="e6172-216">活用を後で、ストア マネージャーで、移動しますが、ここでは代わりに、単純なリストがあるしたい場合がします。</span><span class="sxs-lookup"><span data-stu-id="e6172-216">We'll take advantage of that later in our store manager, but for now we'd like to have a simple list instead.</span></span>

<span data-ttu-id="e6172-217">アプリケーションを実行お/Store を参照すると、数とジャンルの一覧の両方が表示されることがわかります。</span><span class="sxs-lookup"><span data-stu-id="e6172-217">When we run the application and browse to /Store, we see that both the count and list of Genres is displayed.</span></span>

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a><span data-ttu-id="e6172-218">ページ間のリンクの追加</span><span class="sxs-lookup"><span data-stu-id="e6172-218">Adding Links between pages</span></span>

<span data-ttu-id="e6172-219">当社/Store URL を現在ジャンルの一覧を表示するには、単にプレーン テキストとしてジャンル名が一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="e6172-219">Our /Store URL that lists Genres currently lists the Genre names simply as plain text.</span></span> <span data-ttu-id="e6172-220">みましょうようにプレーン テキストではなく代わりに、ジャンル名へのリンク、適切なストア/参照 URL されるように変更音楽のジャンル"Disco"をストア/参照に移動などをクリックするとしますか? ジャンル Disco URL を = です。</span><span class="sxs-lookup"><span data-stu-id="e6172-220">Let's change this so that instead of plain text we instead have the Genre names link to the appropriate /Store/Browse URL, so that clicking on a music genre like "Disco" will navigate to the /Store/Browse?genre=Disco URL.</span></span> <span data-ttu-id="e6172-221">当社 \Views\Store\Index.cshtml ビュー テンプレートを使用してこれらのリンクは以下のようなコードの出力を更新すること**(これには入力しないで - は、パフォーマンスを向上すること)**:</span><span class="sxs-lookup"><span data-stu-id="e6172-221">We could update our \Views\Store\Index.cshtml View template to output these links using code like below **(don't type this in - we're going to improve on it)**:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

<span data-ttu-id="e6172-222">機能するが、ハードコーディングされた文字列に依存しているので、後で問題になる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e6172-222">That works, but it could lead to trouble later since it relies on a hardcoded string.</span></span> <span data-ttu-id="e6172-223">たとえば、コント ローラーの名前を変更する場合、更新する必要があるリンクを検索しているコード全体を検索お必要があります。</span><span class="sxs-lookup"><span data-stu-id="e6172-223">For instance, if we wanted to rename the Controller, we'd need to search through our code looking for links that need to be updated.</span></span>

<span data-ttu-id="e6172-224">その他の方法を使用して HTML ヘルパー メソッドを利用を開始します。</span><span class="sxs-lookup"><span data-stu-id="e6172-224">An alternative approach we can use is to take advantage of an HTML Helper method.</span></span> <span data-ttu-id="e6172-225">ASP.NET MVC には、さまざまな次のように一般的なタスクを実行するビュー テンプレート コードから使用できる HTML ヘルパー メソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="e6172-225">ASP.NET MVC includes HTML Helper methods which are available from our View template code to perform a variety of common tasks just like this.</span></span> <span data-ttu-id="e6172-226">Html.ActionLink() ヘルパー メソッドは HTML をビルドしやすく、特に有用なものでは、 &lt;、&gt;リンク、URL パスは、正しく URL エンコードされていることを確認するのと同様に面倒な場合の詳細を管理します。</span><span class="sxs-lookup"><span data-stu-id="e6172-226">The Html.ActionLink() helper method is a particularly useful one, and makes it easy to build HTML &lt;a&gt; links and takes care of annoying details like making sure URL paths are properly URL encoded.</span></span>

<span data-ttu-id="e6172-227">Html.ActionLink() には、リンクに必要な多くの情報を指定することをいくつかの異なるオーバー ロードがあります。</span><span class="sxs-lookup"><span data-stu-id="e6172-227">Html.ActionLink() has several different overloads to allow specifying as much information as you need for your links.</span></span> <span data-ttu-id="e6172-228">最も簡単な場合は、リンク テキストだけと、クライアントでハイパーリンクがクリックされたときに移動するアクション メソッドを指定します。</span><span class="sxs-lookup"><span data-stu-id="e6172-228">In the simplest case, you'll supply just the link text and the Action method to go to when the hyperlink is clicked on the client.</span></span> <span data-ttu-id="e6172-229">たとえば、「ストア/」というリンク テキストは"移動する、ストア"を使用してインデックスは次の呼び出しとストアの詳細ページの Index() メソッドにリンクできます。</span><span class="sxs-lookup"><span data-stu-id="e6172-229">For example, we can link to "/Store/" Index() method on the Store Details page with the link text "Go to the Store Index" using the following call:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

<span data-ttu-id="e6172-230">*注: このケースでは、していない必要がありますおだけとリンクしている現在のビューをレンダリングは、同じコント ローラー内の別のアクション、コント ローラー名を指定します。*</span><span class="sxs-lookup"><span data-stu-id="e6172-230">*Note: In this case, we didn't need to specify the controller name because we're just linking to another action within the same controller that's rendering the current view.*</span></span>

<span data-ttu-id="e6172-231">[参照] ページに、リンクを 3 つのパラメーターを受け取る Html.ActionLink メソッドの別のオーバー ロードを使用して、、ただし、パラメーターを渡す必要があります。</span><span class="sxs-lookup"><span data-stu-id="e6172-231">Our links to the Browse page will need to pass a parameter, though, so we'll use another overload of the Html.ActionLink method that takes three parameters:</span></span>

- 1. <span data-ttu-id="e6172-232">リンク テキストは、ジャンル名前が表示されます。</span><span class="sxs-lookup"><span data-stu-id="e6172-232">Link text, which will display the Genre name</span></span>
- 2. <span data-ttu-id="e6172-233">コント ローラー アクションの名前 (参照)</span><span class="sxs-lookup"><span data-stu-id="e6172-233">Controller action name (Browse)</span></span>
- 3. <span data-ttu-id="e6172-234">ルートのパラメーター値、(ジャンル) の名前と値 (ジャンル名) の両方を指定します。</span><span class="sxs-lookup"><span data-stu-id="e6172-234">Route parameter values, specifying both the name (Genre) and the value (Genre name)</span></span>

<span data-ttu-id="e6172-235">すべて同時には、ここでは、ストアのインデックス ビューにこれらのリンクを書き込む方法を配置します。</span><span class="sxs-lookup"><span data-stu-id="e6172-235">Putting that all together, here's how we'll write those links to the Store Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

<span data-ttu-id="e6172-236">今すぐ、プロジェクトをもう一度実行して/Store/URL へのアクセスとは、ジャンルの一覧を表示おされます。</span><span class="sxs-lookup"><span data-stu-id="e6172-236">Now when we run our project again and access the /Store/ URL we will see a list of genres.</span></span> <span data-ttu-id="e6172-237">クリックされたときに、各ジャンルにはハイパーリンク – us にかかる、ストア/参照? ジャンル =*[ジャンル]* URL。</span><span class="sxs-lookup"><span data-stu-id="e6172-237">Each genre is a hyperlink – when clicked it will take us to our /Store/Browse?genre=*[genre]* URL.</span></span>

![](mvc-music-store-part-3/_static/image3.jpg)

<span data-ttu-id="e6172-238">次のような一覧については、ジャンル HTML が表示。</span><span class="sxs-lookup"><span data-stu-id="e6172-238">The HTML for the genre list looks like this:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]


>[!div class="step-by-step"]
<span data-ttu-id="e6172-239">[前へ](mvc-music-store-part-2.md)
[次へ](mvc-music-store-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="e6172-239">[Previous](mvc-music-store-part-2.md)
[Next](mvc-music-store-part-4.md)</span></span>
