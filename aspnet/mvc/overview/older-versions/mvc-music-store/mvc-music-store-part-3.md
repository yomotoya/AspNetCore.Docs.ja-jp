---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: 'パート 3: Views と ViewModels |Microsoft Docs'
author: jongalloway
description: このチュートリアル シリーズでは、すべての ASP.NET MVC のミュージック ストア サンプル アプリケーションをビルドする手順について説明します。 第 3 部では、Views と ViewModels について説明します。
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 828ff18abcc5932f82be71a45ebde589eeb051fa
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827277"
---
<a name="part-3-views-and-viewmodels"></a><span data-ttu-id="53dd0-104">パート 3: Views と ViewModels</span><span class="sxs-lookup"><span data-stu-id="53dd0-104">Part 3: Views and ViewModels</span></span>
====================
<span data-ttu-id="53dd0-105">によって[Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="53dd0-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="53dd0-106">MVC のミュージック ストアは、チュートリアル アプリケーションを紹介し、web 開発用の ASP.NET MVC と Visual Studio を使用する方法をステップ バイ ステップについて説明します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="53dd0-107">MVC のミュージック ストアは、オンラインで音楽のアルバムを販売し、基本的なサイトの管理、ユーザー サインインし、買い物カゴの機能を実装する軽量サンプル ストア実装です。</span><span class="sxs-lookup"><span data-stu-id="53dd0-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="53dd0-108">このチュートリアル シリーズでは、すべての ASP.NET MVC のミュージック ストア サンプル アプリケーションをビルドする手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="53dd0-109">第 3 部では、Views と ViewModels について説明します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-109">Part 3 covers Views and ViewModels.</span></span>


<span data-ttu-id="53dd0-110">これまでに私たちしただけされてを返す文字列コント ローラー アクションから。</span><span class="sxs-lookup"><span data-stu-id="53dd0-110">So far we've just been returning strings from controller actions.</span></span> <span data-ttu-id="53dd0-111">コント ローラーのしくみを理解できる優れた手段ですが、いないする方法が、実際の web アプリケーションを構築します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-111">That's a nice way to get an idea of how controllers work, but it's not how you'd want to build a real web application.</span></span> <span data-ttu-id="53dd0-112">私たちのサイトにアクセスしてブラウザーに HTML を生成する方法の向上を目的に – 1 つの HTML コンテンツをより簡単にカスタマイズするテンプレート ファイルを使用できます返信します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-112">We are going to want a better way to generate HTML back to browsers visiting our site – one where we can use template files to more easily customize the HTML content send back.</span></span> <span data-ttu-id="53dd0-113">これには正確に表示します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-113">That's exactly what Views do.</span></span>

## <a name="adding-a-view-template"></a><span data-ttu-id="53dd0-114">ビュー テンプレートを追加します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-114">Adding a View template</span></span>

<span data-ttu-id="53dd0-115">ビュー テンプレートを使用するには、ActionResult を返す HomeController の Index メソッドを変更し、View() が以下のように返されるように。</span><span class="sxs-lookup"><span data-stu-id="53dd0-115">To use a view-template, we'll change the HomeController Index method to return an ActionResult, and have it return View(), like below:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

<span data-ttu-id="53dd0-116">上記の変更では、文字列を返すの代わりを示します、代わりに、結果を生成する"View"を使用します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-116">The above change indicates that instead of returned a string, we instead want to use a "View" to generate a result back.</span></span>

<span data-ttu-id="53dd0-117">プロジェクトに適切なテンプレートの表示を追加します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-117">We'll now add an appropriate View template to our project.</span></span> <span data-ttu-id="53dd0-118">これを行うこと、インデックス アクション メソッド内にテキスト カーソルを配置しますし、右クリックし、「ビューの追加」を選択します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-118">To do this we'll position the text cursor within the Index action method, then right-click and select "Add View".</span></span> <span data-ttu-id="53dd0-119">ビューの追加 ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="53dd0-119">This will bring up the Add View dialog:</span></span>

<span data-ttu-id="53dd0-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="53dd0-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span></span>

<span data-ttu-id="53dd0-121">「ビューの追加」ダイアログでは、迅速かつ簡単には、ビュー テンプレート ファイルを生成できます。</span><span class="sxs-lookup"><span data-stu-id="53dd0-121">The "Add View" dialog allows us to quickly and easily generate View template files.</span></span> <span data-ttu-id="53dd0-122">既定では、「ビューの追加」ダイアログでは、それを使用するアクション メソッドに一致するように作成するビュー テンプレートの名前が事前設定します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-122">By default the "Add View" dialog pre-populates the name of the View template to create so that it matches the action method that will use it.</span></span> <span data-ttu-id="53dd0-123">HomeController の Index() アクション メソッド内の「ビューの追加」コンテキスト メニューを使用したため、前述の"ビューの追加 ダイアログ ボックスは既定で事前作成ビューの名前として"Index"があります。</span><span class="sxs-lookup"><span data-stu-id="53dd0-123">Because we used the "Add View" context menu within the Index() action method of our HomeController, the "Add View" dialog above has "Index" as the view name pre-populated by default.</span></span> <span data-ttu-id="53dd0-124">このダイアログ ボックスでオプションのいずれかを変更するには、そのため、[追加] をクリックしては不要です。</span><span class="sxs-lookup"><span data-stu-id="53dd0-124">We don't need to change any of the options on this dialog, so click the Add button.</span></span>

<span data-ttu-id="53dd0-125">[追加] ボタンをクリックすると、Visual Web Developer は、フォルダーを作成する場合、\Views\Home ディレクトリ内のビュー テンプレートが存在しない新しい Index.cshtml を作成します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-125">When we click the Add button, Visual Web Developer will create a new Index.cshtml view template for us in the \Views\Home directory, creating the folder if doesn't already exist.</span></span>

![](mvc-music-store-part-3/_static/image2.png)

<span data-ttu-id="53dd0-126">"Index.cshtml"ファイルの名前とフォルダーの場所は重要であり、既定の ASP.NET MVC の名前付け規則に従います。</span><span class="sxs-lookup"><span data-stu-id="53dd0-126">The name and folder location of the "Index.cshtml" file is important, and follows the default ASP.NET MVC naming conventions.</span></span> <span data-ttu-id="53dd0-127">ディレクトリ名、\Views\Home、コント ローラー - HomeController という名前に一致します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-127">The directory name, \Views\Home, matches the controller - which is named HomeController.</span></span> <span data-ttu-id="53dd0-128">インデックス、ビュー テンプレート名では、ビューを表示するコント ローラー アクション メソッドと一致します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-128">The view template name, Index, matches the controller action method which will be displaying the view.</span></span>

<span data-ttu-id="53dd0-129">ASP.NET MVC では、この名前付け規則を使用してビューを返すときに、名前またはビュー テンプレートの場所を明示的に指定しなくてもすむようにできます。</span><span class="sxs-lookup"><span data-stu-id="53dd0-129">ASP.NET MVC allows us to avoid having to explicitly specify the name or location of a view template when we use this naming convention to return a view.</span></span> <span data-ttu-id="53dd0-130">既定で表示する \Views\Home\Index.cshtml テンプレートの表示、HomeController 内で次のようなコードを記述する場合。</span><span class="sxs-lookup"><span data-stu-id="53dd0-130">It will by default render the \Views\Home\Index.cshtml view template when we write code like below within our HomeController:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

<span data-ttu-id="53dd0-131">Visual Web Developer では、作成され、"ビューの追加 ダイアログ ボックスで 追加 ボタンをクリックした後に"Index.cshtml"テンプレートの表示を開きます。</span><span class="sxs-lookup"><span data-stu-id="53dd0-131">Visual Web Developer created and opened the "Index.cshtml" view template after we clicked the "Add" button within the "Add View" dialog.</span></span> <span data-ttu-id="53dd0-132">Index.cshtml の内容は、以下に示します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-132">The contents of Index.cshtml are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

<span data-ttu-id="53dd0-133">このビューは、ASP.NET Web フォームと ASP.NET MVC の以前のバージョンで使用される Web フォーム ビュー エンジンよりも簡潔である Razor 構文を使用しています。</span><span class="sxs-lookup"><span data-stu-id="53dd0-133">This view is using the Razor syntax, which is more concise than the Web Forms view engine used in ASP.NET Web Forms and previous versions of ASP.NET MVC.</span></span> <span data-ttu-id="53dd0-134">Web フォームのビュー エンジンは ASP.NET MVC 3 では使用できますが、多くの開発者は、Razor ビュー エンジンが ASP.NET MVC の開発を非常にうまく収まることを確認します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-134">The Web Forms view engine is still available in ASP.NET MVC 3, but many developers find that the Razor view engine fits ASP.NET MVC development really well.</span></span>

<span data-ttu-id="53dd0-135">最初の 3 つの行では、ViewBag.Title を使用して、ページ タイトルを設定します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-135">The first three lines set the page title using ViewBag.Title.</span></span> <span data-ttu-id="53dd0-136">見てこのしくみについて詳しくがすぐに、まずみましょう更新テキストの見出しテキストがされページを表示します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-136">We'll look at how this works in more detail soon, but first let's update the text heading text and view the page.</span></span> <span data-ttu-id="53dd0-137">更新プログラム、 &lt;h2&gt;タグを次に示すように「このは、ホーム ページ」と答えるとします。</span><span class="sxs-lookup"><span data-stu-id="53dd0-137">Update the &lt;h2&gt; tag to say "This is the Home Page" as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

<span data-ttu-id="53dd0-138">新しいテキストがホーム ページに表示されるアプリケーションの表示を実行します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-138">Running the application shows that our new text is visible on the home page.</span></span>

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a><span data-ttu-id="53dd0-139">サイトの一般的な要素のレイアウトを使用します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-139">Using a Layout for common site elements</span></span>

<span data-ttu-id="53dd0-140">ほとんどの web サイトは、多くのページ間で共有されるコンテンツがある: ナビゲーション、ページ フッター、ロゴのイメージ、スタイル シートの参照など。Razor ビュー エンジンは、これと呼ばれるページを使用して管理する簡単な\_Layout.cshtml/ビュー/共有フォルダー内を自動的に作成されました。</span><span class="sxs-lookup"><span data-stu-id="53dd0-140">Most websites have content which is shared between many pages: navigation, footers, logo images, stylesheet references, etc. The Razor view engine makes this easy to manage using a page called \_Layout.cshtml which has automatically been created for us inside the /Views/Shared folder.</span></span>

![](mvc-music-store-part-3/_static/image4.png)

<span data-ttu-id="53dd0-141">次に示す、内容を表示するには、このフォルダーをダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="53dd0-141">Double-click on this folder to view the contents, which are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

<span data-ttu-id="53dd0-142">によって、個々 のビューからのコンテンツが表示されます、 @RenderBody() コマンド、およびの外側に記述する必要がある共通の内容に追加できる、 \_Layout.cshtml マークアップ。</span><span class="sxs-lookup"><span data-stu-id="53dd0-142">The content from our individual views will be displayed by the @RenderBody() command, and any common content that we want to appear outside of that can be added to the \_Layout.cshtml markup.</span></span> <span data-ttu-id="53dd0-143">当社の MVC Music Store 上のテンプレートに追加する予定、サイト内のすべてのページで、ホーム ページとストアの領域へのリンクに共通のヘッダーを持つ必要があります@RenderBody() のステートメント。</span><span class="sxs-lookup"><span data-stu-id="53dd0-143">We'll want our MVC Music Store to have a common header with links to our Home page and Store area on all pages in the site, so we'll add that to the template directly above that @RenderBody() statement.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a><span data-ttu-id="53dd0-144">スタイル シートを更新しています</span><span class="sxs-lookup"><span data-stu-id="53dd0-144">Updating the StyleSheet</span></span>

<span data-ttu-id="53dd0-145">空のプロジェクト テンプレートには、検証メッセージを表示するために使用するスタイルが含まれている非常に簡素化された CSS ファイルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="53dd0-145">The empty project template includes a very streamlined CSS file which just includes styles used to display validation messages.</span></span> <span data-ttu-id="53dd0-146">このデザイナーは、now でそれらを追加しますので、弊社サイトの外観を定義する追加 CSS とイメージを提供しています。</span><span class="sxs-lookup"><span data-stu-id="53dd0-146">Our designer has provided some additional CSS and images to define the look and feel for our site, so we'll add those in now.</span></span>

<span data-ttu-id="53dd0-147">更新された CSS ファイルとイメージがの MvcMusicStore Assets.zip で利用可能なコンテンツのディレクトリ内に含まれる[MVC-ミュージック ストア](https://github.com/evilDave/MVC-Music-Store)します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-147">The updated CSS file and Images are included in the Content directory of MvcMusicStore-Assets.zip which is available at [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span> <span data-ttu-id="53dd0-148">Windows エクスプ ローラーでこれらの両方を選択して、次に示すように Visual Web developer で、ソリューションのコンテンツのフォルダーにドロップしましたします。</span><span class="sxs-lookup"><span data-stu-id="53dd0-148">We'll select both of them in Windows Explorer and drop them into our Solution's Content folder in Visual Web Developer, as shown below:</span></span>

![](mvc-music-store-part-3/_static/image5.png)

<span data-ttu-id="53dd0-149">Site.css ファイルを上書きするかどうかの確認を求め。</span><span class="sxs-lookup"><span data-stu-id="53dd0-149">You'll be asked to confirm if you want to overwrite the existing Site.css file.</span></span> <span data-ttu-id="53dd0-150">[はい] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="53dd0-150">Click Yes.</span></span>

![](mvc-music-store-part-3/_static/image6.png)

<span data-ttu-id="53dd0-151">アプリケーションのコンテンツのフォルダーは、次のように表示されます。</span><span class="sxs-lookup"><span data-stu-id="53dd0-151">The Content folder of your application will now appear as follows:</span></span>

![](mvc-music-store-part-3/_static/image7.png)

<span data-ttu-id="53dd0-152">これでアプリケーションを実行し、ホーム ページで、変更点の外観を確認してみましょう。</span><span class="sxs-lookup"><span data-stu-id="53dd0-152">Now let's run the application and see how our changes look on the Home page.</span></span>

![](mvc-music-store-part-3/_static/image8.png)

- <span data-ttu-id="53dd0-153">変更内容を確認しましょう。「HomeController の Index アクション メソッドが見つかり、テンプレートの表示後、標準的な名前付け規則ので、コードに"戻り View()"と呼ばれる場合でも、\Views\Home\Index.cshtmlView テンプレートが表示されます。</span><span class="sxs-lookup"><span data-stu-id="53dd0-153">Let's review what's changed: The HomeController's Index action method found and displayed the \Views\Home\Index.cshtmlView template, even though our code called "return View()", because our View template followed the standard naming convention.</span></span>
- <span data-ttu-id="53dd0-154">ホーム ページは \Views\Home\Index.cshtml ビュー テンプレート内で定義されている単純なようこそメッセージを表示しています。</span><span class="sxs-lookup"><span data-stu-id="53dd0-154">The Home Page is displaying a simple welcome message that is defined within the \Views\Home\Index.cshtml view template.</span></span>
- <span data-ttu-id="53dd0-155">ホーム ページを使用して、 \_Layout.cshtml テンプレート、ようこそメッセージは、標準のサイトの HTML レイアウト内に含まれるためです。</span><span class="sxs-lookup"><span data-stu-id="53dd0-155">The Home Page is using our \_Layout.cshtml template, and so the welcome message is contained within the standard site HTML layout.</span></span>

## <a name="using-a-model-to-pass-information-to-our-view"></a><span data-ttu-id="53dd0-156">このビューに情報を渡すモデルを使用します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-156">Using a Model to pass information to our View</span></span>

<span data-ttu-id="53dd0-157">単純にハードコーディングされた HTML を表示します。 テンプレートの表示はない非常に興味深い web サイトを作成しようとしています。</span><span class="sxs-lookup"><span data-stu-id="53dd0-157">A View template that just displays hardcoded HTML isn't going to make a very interesting web site.</span></span> <span data-ttu-id="53dd0-158">動的な web サイトを作成するには代わりにするために、ビュー テンプレートに、コント ローラー アクションから情報を渡す。</span><span class="sxs-lookup"><span data-stu-id="53dd0-158">To create a dynamic web site, we'll instead want to pass information from our controller actions to our view templates.</span></span>

<span data-ttu-id="53dd0-159">モデル-ビュー-コント ローラー パターンでモデルを指す用語オブジェクトをアプリケーション内のデータを表します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-159">In the Model-View-Controller pattern, the term Model refers to objects which represent the data in the application.</span></span> <span data-ttu-id="53dd0-160">多くの場合、モデル オブジェクトが、データベース内のテーブルに対応していますが、する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="53dd0-160">Often, model objects correspond to tables in your database, but they don't have to.</span></span>

<span data-ttu-id="53dd0-161">ActionResult を返すコント ローラー アクション メソッドは、モデル オブジェクトをビューに渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="53dd0-161">Controller action methods which return an ActionResult can pass a model object to the view.</span></span> <span data-ttu-id="53dd0-162">これにより、応答を生成し、ビュー テンプレートにこの情報を渡すために必要なすべての情報を明確にパッケージを使用して、適切な HTML 応答を生成するコント ローラーです。</span><span class="sxs-lookup"><span data-stu-id="53dd0-162">This allows a Controller to cleanly package up all the information needed to generate a response, and then pass this information off to a View template to use to generate the appropriate HTML response.</span></span> <span data-ttu-id="53dd0-163">これは、最も簡単に理解の動作確認することによって、開始しましょう。</span><span class="sxs-lookup"><span data-stu-id="53dd0-163">This is easiest to understand by seeing it in action, so let's get started.</span></span>

<span data-ttu-id="53dd0-164">まず、ストア内のジャンルとアルバムを表すいくつかのモデル クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-164">First we'll create some Model classes to represent Genres and Albums within our store.</span></span> <span data-ttu-id="53dd0-165">ジャンル クラスを作成してみましょう。</span><span class="sxs-lookup"><span data-stu-id="53dd0-165">Let's start by creating a Genre class.</span></span> <span data-ttu-id="53dd0-166">プロジェクト内で"Models"フォルダーを右クリックし、「クラスの追加」を選択して、および"Genre.cs"ファイルの名前します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-166">Right-click the "Models" folder within your project, choose the "Add Class" option, and name the file "Genre.cs".</span></span>

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

<span data-ttu-id="53dd0-167">作成されたクラスをパブリック文字列プロパティの名前を追加します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-167">Then add a public string Name property to the class that was created:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

<span data-ttu-id="53dd0-168">*注: 参考までに、、{get; 設定。} 表記は、# の自動実装プロパティの機能を使用します。これにより、プロパティの利点、バッキング フィールドを宣言することを必要とせず。*</span><span class="sxs-lookup"><span data-stu-id="53dd0-168">*Note: In case you're wondering, the { get; set; } notation is making use of C#'s auto-implemented properties feature. This gives us the benefits of a property without requiring us to declare a backing field.*</span></span>

<span data-ttu-id="53dd0-169">次に、タイトルとジャンル プロパティを持つアルバム クラス (Album.cs という名前) を作成する同じ手順に従います。</span><span class="sxs-lookup"><span data-stu-id="53dd0-169">Next, follow the same steps to create an Album class (named Album.cs) that has a Title and a Genre property:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

<span data-ttu-id="53dd0-170">ここでモデルからの動的な情報を表示するビューを使用して StoreController を変更することができます。</span><span class="sxs-lookup"><span data-stu-id="53dd0-170">Now we can modify the StoreController to use Views which display dynamic information from our Model.</span></span> <span data-ttu-id="53dd0-171">デモンストレーションを目的として今 - という名前の要求 ID に基づく、アルバム場合、は、以下のビューのようにその情報を表示しますでした。</span><span class="sxs-lookup"><span data-stu-id="53dd0-171">If - for demonstration purposes right now - we named our Albums based on the request ID, we could display that information as in the view below.</span></span>

![](mvc-music-store-part-3/_static/image10.png)

<span data-ttu-id="53dd0-172">1 つのアルバムの情報が表示されるように、ストアの詳細アクションを変更することで始めます。</span><span class="sxs-lookup"><span data-stu-id="53dd0-172">We'll start by changing the Store Details action so it shows the information for a single album.</span></span> <span data-ttu-id="53dd0-173">先頭に"using"ステートメントを追加、 **StoreControllers**アルバム クラスを使用するたびに MvcMusicStore.Models.Album を入力する必要はありませんので、MvcMusicStore.Models 名前空間を含めるクラス。</span><span class="sxs-lookup"><span data-stu-id="53dd0-173">Add a "using" statement to the top of the **StoreControllers** class to include the MvcMusicStore.Models namespace, so we don't need to type MvcMusicStore.Models.Album every time we want to use the album class.</span></span> <span data-ttu-id="53dd0-174">そのクラスの"using"セクションが表示されます以下のとおりです。</span><span class="sxs-lookup"><span data-stu-id="53dd0-174">The "usings" section of that class should now appear as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

<span data-ttu-id="53dd0-175">次に、更新します、Details コント ローラー アクション、文字列ではなく、ActionResult を返すように HomeController の Index メソッドと同様。</span><span class="sxs-lookup"><span data-stu-id="53dd0-175">Next, we'll update the Details controller action so that it returns an ActionResult rather than a string, as we did with the HomeController's Index method.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

<span data-ttu-id="53dd0-176">ここでビューにアルバム オブジェクトを返すロジックを変更することができます。</span><span class="sxs-lookup"><span data-stu-id="53dd0-176">Now we can modify the logic to return an Album object to the view.</span></span> <span data-ttu-id="53dd0-177">このチュートリアルの後半で私たちがするからデータを取得: データベースが、今では使用「ダミー データ」を開始します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-177">Later in this tutorial we will be retrieving the data from a database – but for right now we will use "dummy data" to get started.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

<span data-ttu-id="53dd0-178">*注: C# を使い慣れていない場合は、可能性がありますと想定すること、アルバムの変数がある遅延バインディングを意味 var の使用します。正しくありません-C# コンパイラを使用して型推論に基づいてどのアルバムがアルバムの種類を決定する、変数に代入しています、アルバムの種類として、アルバムのローカル変数をコンパイルするため、コンパイル時チェックと Visual Studio コード エディターを取得しますサポート。*</span><span class="sxs-lookup"><span data-stu-id="53dd0-178">*Note: If you're unfamiliar with C#, you may assume that using var means that our album variable is late-bound. That's not correct – the C# compiler is using type-inference based on what we're assigning to the variable to determine that album is of type Album and compiling the local album variable as an Album type, so we get compile-time checking and Visual Studio code-editor support.*</span></span>

<span data-ttu-id="53dd0-179">当社のアルバムを使用して、HTML 応答を生成するテンプレートの表示を今すぐ作成しましょう。</span><span class="sxs-lookup"><span data-stu-id="53dd0-179">Let's now create a View template that uses our Album to generate an HTML response.</span></span> <span data-ttu-id="53dd0-180">その前に、新しく作成したアルバム クラスの概要ビューの追加 ダイアログを認識するようにプロジェクトをビルドする必要があります。</span><span class="sxs-lookup"><span data-stu-id="53dd0-180">Before we do that we need to build the project so that the Add View dialog knows about our newly created Album class.</span></span> <span data-ttu-id="53dd0-181">プロジェクトをビルドする Debug⇨Build MvcMusicStore を選択してメニュー項目 (追加の手順を使用できます Ctrl + SHIFT+B ショートカット プロジェクトをビルドします)。</span><span class="sxs-lookup"><span data-stu-id="53dd0-181">You can build the project by selecting the Debug⇨Build MvcMusicStore menu item (for extra credit, you can use the Ctrl-Shift-B shortcut to build the project).</span></span>

![](mvc-music-store-part-3/_static/image11.png)

<span data-ttu-id="53dd0-182">これで、サポートするクラスを設定したは、ビュー テンプレートを作成する準備ができました。</span><span class="sxs-lookup"><span data-stu-id="53dd0-182">Now that we've set up our supporting classes, we're ready to build our View template.</span></span> <span data-ttu-id="53dd0-183">Details メソッド内で右クリックし、コンテキスト メニューから [ビューの追加] を選択します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-183">Right-click within the Details method and select "Add View…" from the context menu.</span></span>

![](mvc-music-store-part-3/_static/image12.png)

<span data-ttu-id="53dd0-184">HomeController の前に行ったように、新しいビュー テンプレートを作成するでしょう。</span><span class="sxs-lookup"><span data-stu-id="53dd0-184">We are going to create a new View template like we did before with the HomeController.</span></span> <span data-ttu-id="53dd0-185">StoreController から作成するためが既定で生成されます \Views\Store\Index.cshtml ファイル。</span><span class="sxs-lookup"><span data-stu-id="53dd0-185">Because we are creating it from the StoreController it will by default be generated in a \Views\Store\Index.cshtml file.</span></span>

<span data-ttu-id="53dd0-186">異なり、前にここ「厳密に型を作成する」ビューのチェック ボックスをオンします。</span><span class="sxs-lookup"><span data-stu-id="53dd0-186">Unlike before, we are going to check the "Create a strongly-typed" view checkbox.</span></span> <span data-ttu-id="53dd0-187">「View データ クラス」ドロップ ダウン リスト内で、「アルバム」クラスを選択し、ここです。</span><span class="sxs-lookup"><span data-stu-id="53dd0-187">We are then going to select our "Album" class within the "View data-class" drop-downlist.</span></span> <span data-ttu-id="53dd0-188">いるものと想定するビュー テンプレートを使用するように渡されるオブジェクトのアルバムを作成する「ビューの追加」ダイアログになります。</span><span class="sxs-lookup"><span data-stu-id="53dd0-188">This will cause the "Add View" dialog to create a View template that expects that an Album object will be passed to it to use.</span></span>

![](mvc-music-store-part-3/_static/image13.png)

<span data-ttu-id="53dd0-189">[追加] ボタンをクリックしたとき、\Views\Store\Details.cshtml ビュー テンプレートが作成された、次のコードを格納しています。</span><span class="sxs-lookup"><span data-stu-id="53dd0-189">When we click the "Add" button our \Views\Store\Details.cshtml View template will be created, containing the following code.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

<span data-ttu-id="53dd0-190">このビューがアルバム クラスに厳密に型であることを示す最初の行に注意してください。</span><span class="sxs-lookup"><span data-stu-id="53dd0-190">Notice the first line, which indicates that this view is strongly-typed to our Album class.</span></span> <span data-ttu-id="53dd0-191">渡されたアルバム オブジェクトでは、モデルのプロパティを簡単にアクセスしても、Visual Web Developer のエディターで IntelliSense の利点があるため、Razor ビュー エンジンが理解しています。</span><span class="sxs-lookup"><span data-stu-id="53dd0-191">The Razor view engine understands that it has been passed an Album object, so we can easily access model properties and even have the benefit of IntelliSense in the Visual Web Developer editor.</span></span>

<span data-ttu-id="53dd0-192">更新プログラム、 &lt;h2&gt;タグを次のように表示するには、その行を変更することで、アルバムのタイトルのプロパティが表示されるようにします。</span><span class="sxs-lookup"><span data-stu-id="53dd0-192">Update the &lt;h2&gt; tag so it displays the Album's Title property by modifying that line to appear as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

<span data-ttu-id="53dd0-193">後にピリオドを入力すると、IntelliSense がトリガーされることに注意してください、@Modelキーワード、アルバム クラスをサポートするメソッドとプロパティを表示します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-193">Notice that IntelliSense is triggered when you enter the period after the @Model keyword, showing the properties and methods that the Album class supports.</span></span>

<span data-ttu-id="53dd0-194">みましょうこれで、プロジェクトを再実行してストア/詳細/5 URL を参照してください。</span><span class="sxs-lookup"><span data-stu-id="53dd0-194">Let's now re-run our project and visit the /Store/Details/5 URL.</span></span> <span data-ttu-id="53dd0-195">次のようなアルバムの詳細が表示されます。</span><span class="sxs-lookup"><span data-stu-id="53dd0-195">We'll see details of an Album like below.</span></span>

![](mvc-music-store-part-3/_static/image14.png)

<span data-ttu-id="53dd0-196">今すぐストア参照アクション メソッドのような更新をしましょう。</span><span class="sxs-lookup"><span data-stu-id="53dd0-196">Now we'll make a similar update for the Store Browse action method.</span></span> <span data-ttu-id="53dd0-197">、ActionResult を返すように、メソッドを更新し、新しいジャンル オブジェクトを作成し、ビューに戻ることは、メソッドのロジックを変更します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-197">Update the method so it returns an ActionResult, and modify the method logic so it creates a new Genre object and returns it to the View.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

<span data-ttu-id="53dd0-198">厳密に型指定されたビューを追加し、参照メソッドを右クリックし、コンテキスト メニューから [ビューの追加] を選択して、厳密に型指定されたジャンル クラスに追加します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-198">Right-click in the Browse method and select "Add View…" from the context menu, then add a View that is strongly-typed add a strongly typed to the Genre class.</span></span>

![](mvc-music-store-part-3/_static/image15.png)

<span data-ttu-id="53dd0-199">更新プログラム、 &lt;h2&gt;ジャンルの情報を表示する (/Views/Store/Browse.cshtml) でコードのビュー内の要素。</span><span class="sxs-lookup"><span data-stu-id="53dd0-199">Update the &lt;h2&gt; element in the view code (in /Views/Store/Browse.cshtml) to display the Genre information.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

<span data-ttu-id="53dd0-200">これで、プロジェクトを再実行し、ストア/参照を参照してみましょうか。ジャンル ディスコの URL を = です。</span><span class="sxs-lookup"><span data-stu-id="53dd0-200">Now let's re-run our project and browse to the /Store/Browse?Genre=Disco URL.</span></span> <span data-ttu-id="53dd0-201">以下のように表示される参照ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="53dd0-201">We'll see the Browse page displayed like below.</span></span>

![](mvc-music-store-part-3/_static/image16.png)

<span data-ttu-id="53dd0-202">最後に、少し複雑な更新を行います、**インデックスの格納**アクション メソッドと、ストア内のすべてのジャンルの一覧を表示するビュー。</span><span class="sxs-lookup"><span data-stu-id="53dd0-202">Finally, let's make a slightly more complex update to the **Store Index** action method and view to display a list of all the Genres in our store.</span></span> <span data-ttu-id="53dd0-203">ジャンルを 1 つだけではなく、モデル オブジェクトとしてジャンルのリストを使用して、実行します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-203">We'll do that by using a List of Genres as our model object, rather than just a single Genre.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

<span data-ttu-id="53dd0-204">ストア インデックスのアクション メソッドを右クリックし、追加のビュー モデル クラスとジャンルを選択する前に、し、[追加] を選択します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-204">Right-click in the Store Index action method and select Add View as before, select Genre as the Model class, and press the Add button.</span></span>

![](mvc-music-store-part-3/_static/image17.png)

<span data-ttu-id="53dd0-205">最初は変更し、@modelビューがいくつかのジャンルを予想することを示す宣言が 1 つだけではなくオブジェクトします。</span><span class="sxs-lookup"><span data-stu-id="53dd0-205">First we'll change the @model declaration to indicate that the view will be expecting several Genre objects rather than just one.</span></span> <span data-ttu-id="53dd0-206">次のように/Store/Index.cshtml の最初の行を変更します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-206">Change the first line of /Store/Index.cshtml to read as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

<span data-ttu-id="53dd0-207">これは、いくつかのジャンル オブジェクトを保持するモデル オブジェクトで使用されることを Razor ビュー エンジンを指示します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-207">This tells the Razor view engine that it will be working with a model object that can hold several Genre objects.</span></span> <span data-ttu-id="53dd0-208">IEnumerable を使用して&lt;ジャンル&gt;一覧ではなく&lt;ジャンル&gt;より汎用的なので、IEnumerable インターフェイスをサポートするオブジェクトの種類を後で、モデルの種類を変更することを許可します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-208">We're using an IEnumerable&lt;Genre&gt; rather than a List&lt;Genre&gt; since it's more generic, allowing us to change our model type later to any object type that supports the IEnumerable interface.</span></span>

<span data-ttu-id="53dd0-209">次に、次のコードの完成した表示に示すように、モデル内のジャンル オブジェクトをループ処理します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-209">Next, we'll loop through the Genre objects in the model as shown in the completed view code below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

<span data-ttu-id="53dd0-210">ある完全な IntelliSense のサポート、このコードに入るときに入力するために注意してください"@Model"。</span><span class="sxs-lookup"><span data-stu-id="53dd0-210">Notice that we have full IntelliSense support as we enter this code, so that when we type "@Model."</span></span> <span data-ttu-id="53dd0-211">私たちは、すべてのメソッドと型ジャンルの IEnumerable でサポートされるプロパティを参照してください。</span><span class="sxs-lookup"><span data-stu-id="53dd0-211">we see all methods and properties supported by an IEnumerable of type Genre.</span></span>

![](mvc-music-store-part-3/_static/image18.png)

<span data-ttu-id="53dd0-212">"Foreach"ループ内では、Visual Web Developer は、各項目がジャンル、型の IntelliSense をそれぞれ確認ため、ジャンル型であるを認識します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-212">Within our "foreach" loop, Visual Web Developer knows that each item is of type Genre, so we see IntelliSense for each the Genre type.</span></span>

![](mvc-music-store-part-3/_static/image19.png)

<span data-ttu-id="53dd0-213">次に、スキャフォールディング機能では、ジャンル オブジェクトを検査し、各必要がある、Name プロパティをループ処理し、それらを決定します。また、個々 の項目を編集、詳細、および削除のリンクも生成されます。</span><span class="sxs-lookup"><span data-stu-id="53dd0-213">Next, the scaffolding feature examined the Genre object and determined that each will have a Name property, so it loops through and writes them out. It also generates Edit, Details, and Delete links to each individual item.</span></span> <span data-ttu-id="53dd0-214">その後で、ストア マネージャーでの恩恵はすぐがここでは、単純なリストの代わりが今回は。</span><span class="sxs-lookup"><span data-stu-id="53dd0-214">We'll take advantage of that later in our store manager, but for now we'd like to have a simple list instead.</span></span>

<span data-ttu-id="53dd0-215">アプリケーションを実行して、/Store を参照、数とジャンルの一覧が表示されることがわかります。</span><span class="sxs-lookup"><span data-stu-id="53dd0-215">When we run the application and browse to /Store, we see that both the count and list of Genres is displayed.</span></span>

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a><span data-ttu-id="53dd0-216">ページ間のリンクの追加</span><span class="sxs-lookup"><span data-stu-id="53dd0-216">Adding Links between pages</span></span>

<span data-ttu-id="53dd0-217">/Store URL を現在のジャンルを一覧表示するには、単にプレーン テキストとしてジャンル名が一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-217">Our /Store URL that lists Genres currently lists the Genre names simply as plain text.</span></span> <span data-ttu-id="53dd0-218">みましょうかをプレーン テキストではなく代わりに、適切なストア/参照 URL へのジャンル名リンクされるように変更音楽のジャンル、ストア/参照に移動します"Disco"などをクリックするとしますか? ジャンル Disco URL を = です。</span><span class="sxs-lookup"><span data-stu-id="53dd0-218">Let's change this so that instead of plain text we instead have the Genre names link to the appropriate /Store/Browse URL, so that clicking on a music genre like "Disco" will navigate to the /Store/Browse?genre=Disco URL.</span></span> <span data-ttu-id="53dd0-219">これらのリンクを使用して以下のようなコードの出力に \Views\Store\Index.cshtml ビュー テンプレート更新 **(これには入力しないでください - これを改善していきます)**:</span><span class="sxs-lookup"><span data-stu-id="53dd0-219">We could update our \Views\Store\Index.cshtml View template to output these links using code like below **(don't type this in - we're going to improve on it)**:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

<span data-ttu-id="53dd0-220">機能はしますが、ハードコーディングされた文字列があったために、後で問題になる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="53dd0-220">That works, but it could lead to trouble later since it relies on a hardcoded string.</span></span> <span data-ttu-id="53dd0-221">たとえば、コント ローラーの名前を変更する場合は、検索、コードを更新する必要があるリンクを検索して必要があります。</span><span class="sxs-lookup"><span data-stu-id="53dd0-221">For instance, if we wanted to rename the Controller, we'd need to search through our code looking for links that need to be updated.</span></span>

<span data-ttu-id="53dd0-222">別のアプローチを使用して、HTML ヘルパー メソッドを利用することです。</span><span class="sxs-lookup"><span data-stu-id="53dd0-222">An alternative approach we can use is to take advantage of an HTML Helper method.</span></span> <span data-ttu-id="53dd0-223">ASP.NET MVC には、さまざまな次のように、一般的なタスクを実行する、コードの表示テンプレートから使用できる HTML ヘルパー メソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="53dd0-223">ASP.NET MVC includes HTML Helper methods which are available from our View template code to perform a variety of common tasks just like this.</span></span> <span data-ttu-id="53dd0-224">Html.ActionLink() のヘルパー メソッドは簡単に HTML を構築すること、便利な&lt;、&gt;リンク、URL パスが正しく URL エンコードを行うような面倒な詳細を管理します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-224">The Html.ActionLink() helper method is a particularly useful one, and makes it easy to build HTML &lt;a&gt; links and takes care of annoying details like making sure URL paths are properly URL encoded.</span></span>

<span data-ttu-id="53dd0-225">Html.ActionLink() には、リンクに必要な限り多くの情報を指定することをいくつかの異なるオーバー ロードがあります。</span><span class="sxs-lookup"><span data-stu-id="53dd0-225">Html.ActionLink() has several different overloads to allow specifying as much information as you need for your links.</span></span> <span data-ttu-id="53dd0-226">最も簡単な場合は、リンクのテキストのみと、クライアントで、ハイパーリンクがクリックされたときに移動するアクション メソッドを入力します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-226">In the simplest case, you'll supply just the link text and the Action method to go to when the hyperlink is clicked on the client.</span></span> <span data-ttu-id="53dd0-227">たとえば、「ストア/」Index() メソッド リンク テキスト「移動するのストア インデックス」次の呼び出しを使用して、ストアの詳細ページにリンクできます。</span><span class="sxs-lookup"><span data-stu-id="53dd0-227">For example, we can link to "/Store/" Index() method on the Store Details page with the link text "Go to the Store Index" using the following call:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

<span data-ttu-id="53dd0-228">*注: この場合、していない必要があります現在のビューをレンダリングしている同じコント ローラー内の別のアクション リンクしているだけであるため、コント ローラー名を指定します。*</span><span class="sxs-lookup"><span data-stu-id="53dd0-228">*Note: In this case, we didn't need to specify the controller name because we're just linking to another action within the same controller that's rendering the current view.*</span></span>

<span data-ttu-id="53dd0-229">[参照] ページに、リンクを 3 つのパラメーターを受け取る Html.ActionLink メソッドの別のオーバー ロードを使用するために、ただし、パラメーターを渡す必要があります。</span><span class="sxs-lookup"><span data-stu-id="53dd0-229">Our links to the Browse page will need to pass a parameter, though, so we'll use another overload of the Html.ActionLink method that takes three parameters:</span></span>

- 1. <span data-ttu-id="53dd0-230">リンク テキストは、ジャンル名が表示されます。</span><span class="sxs-lookup"><span data-stu-id="53dd0-230">Link text, which will display the Genre name</span></span>
- 2. <span data-ttu-id="53dd0-231">コント ローラー アクションの名前 (参照)</span><span class="sxs-lookup"><span data-stu-id="53dd0-231">Controller action name (Browse)</span></span>
- 3. <span data-ttu-id="53dd0-232">ルート パラメーターの値、(ジャンル) の名前と値 (ジャンル名) の両方を指定します。</span><span class="sxs-lookup"><span data-stu-id="53dd0-232">Route parameter values, specifying both the name (Genre) and the value (Genre name)</span></span>

<span data-ttu-id="53dd0-233">すべてをまとめて、ここでは、ストアのインデックス ビューにこれらのリンクを書き込む方法を配置するには。</span><span class="sxs-lookup"><span data-stu-id="53dd0-233">Putting that all together, here's how we'll write those links to the Store Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

<span data-ttu-id="53dd0-234">これで、プロジェクトをもう一度実行して/Store/URL へのアクセス ジャンルの一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="53dd0-234">Now when we run our project again and access the /Store/ URL we will see a list of genres.</span></span> <span data-ttu-id="53dd0-235">クリックされたときに、各ジャンルが – ハイパーリンク、ストア/参照にかかることですか? ジャンル =*[ジャンル]* URL。</span><span class="sxs-lookup"><span data-stu-id="53dd0-235">Each genre is a hyperlink – when clicked it will take us to our /Store/Browse?genre=*[genre]* URL.</span></span>

![](mvc-music-store-part-3/_static/image3.jpg)

<span data-ttu-id="53dd0-236">ジャンルの一覧については、HTML のようになります。</span><span class="sxs-lookup"><span data-stu-id="53dd0-236">The HTML for the genre list looks like this:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]


> [!div class="step-by-step"]
> <span data-ttu-id="53dd0-237">[前へ](mvc-music-store-part-2.md)
> [次へ](mvc-music-store-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="53dd0-237">[Previous](mvc-music-store-part-2.md)
[Next](mvc-music-store-part-4.md)</span></span>
