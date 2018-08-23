---
uid: web-forms/overview/getting-started/creating-a-basic-web-forms-page
title: 基本的な ASP.NET 4.5 Web フォーム ページの作成では、Visual Studio 2013 |Microsoft Docs
author: Erikre
description: ''
ms.author: riande
ms.date: 03/03/2014
ms.assetid: a2f1c635-0817-4a9a-8c13-d5b5d29727c0
msc.legacyurl: /web-forms/overview/getting-started/creating-a-basic-web-forms-page
msc.type: authoredcontent
ms.openlocfilehash: 3efd772854cb97c805d61b63c03f6bae031619de
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835636"
---
<a name="creating-a-basic-aspnet-45-web-forms-page-in-visual-studio-2013"></a><span data-ttu-id="48c03-102">基本的な ASP.NET 4.5 Web フォーム ページの作成では、Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="48c03-102">Creating a Basic ASP.NET 4.5 Web Forms Page in Visual Studio 2013</span></span>
====================
<span data-ttu-id="48c03-103">によって[Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="48c03-103">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="48c03-104">このチュートリアルの概要を Web の開発環境を提供します。 [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs)し[Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)します。</span><span class="sxs-lookup"><span data-stu-id="48c03-104">This walkthrough provides you with an introduction to the Web development environment in [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) and in [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span> <span data-ttu-id="48c03-105">このチュートリアルでは、単純な ASP.NET Web フォーム ページを作成する方法し、の新しいページを作成するコントロールを追加、コードを記述する基本的な方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="48c03-105">This walkthrough guides you through creating a simple ASP.NET Web Forms page and illustrates the basic techniques of creating a new page, adding controls, and writing code.</span></span>

<span data-ttu-id="48c03-106">このチュートリアルでは、以下のタスクを行います。</span><span class="sxs-lookup"><span data-stu-id="48c03-106">Tasks illustrated in this walkthrough include:</span></span>

- <span data-ttu-id="48c03-107">ファイル システム Web フォーム アプリケーション プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="48c03-107">Creating a file system Web Forms application project.</span></span>
- <span data-ttu-id="48c03-108">Visual Studio を理解します。</span><span class="sxs-lookup"><span data-stu-id="48c03-108">Familiarizing yourself with Visual Studio.</span></span>
- <span data-ttu-id="48c03-109">ASP.NET ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="48c03-109">Creating an ASP.NET page.</span></span>
- <span data-ttu-id="48c03-110">コントロールを追加します。</span><span class="sxs-lookup"><span data-stu-id="48c03-110">Adding controls.</span></span>
- <span data-ttu-id="48c03-111">イベント ハンドラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="48c03-111">Adding event handlers.</span></span>
- <span data-ttu-id="48c03-112">実行していると、Visual Studio からページをテストします。</span><span class="sxs-lookup"><span data-stu-id="48c03-112">Running and testing a page from Visual Studio.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="48c03-113">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="48c03-113">Prerequisites</span></span>


<span data-ttu-id="48c03-114">このチュートリアルを完了するための要件は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="48c03-114">In order to complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="48c03-115">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs)または[Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)します。</span><span class="sxs-lookup"><span data-stu-id="48c03-115">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) or [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span> <span data-ttu-id="48c03-116">.NET Framework は、自動的にインストールされます。</span><span class="sxs-lookup"><span data-stu-id="48c03-116">The .NET Framework is installed automatically.</span></span> 

    > [!NOTE] 
    > 
    > <span data-ttu-id="48c03-117">Microsoft Visual Studio 2013 および Microsoft Visual Studio Express 2013 for Web は多くの場合、呼びます Visual Studio とこのチュートリアル シリーズです。</span><span class="sxs-lookup"><span data-stu-id="48c03-117">Microsoft Visual Studio 2013 and Microsoft Visual Studio Express 2013 for Web will often be referred to as Visual Studio throughout this tutorial series.</span></span>  
    >   
    > <span data-ttu-id="48c03-118">Visual Studio を使用している場合は、このチュートリアルの前提条件が選択されている、 **Web 開発**設定のコレクションを初めて Visual Studio を起動します。</span><span class="sxs-lookup"><span data-stu-id="48c03-118">If you are using Visual Studio, this walkthrough assumes that you selected the **Web Development** collection of settings the first time that you started Visual Studio.</span></span> <span data-ttu-id="48c03-119">詳細については、次を参照してください。[方法: Web 開発環境の設定の選択](https://msdn.microsoft.com/library/ff521558.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="48c03-119">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>


## <a name="creating-a-web-application-project-and-a-page"></a><span data-ttu-id="48c03-120">Web アプリケーション プロジェクトと、ページの作成</span><span class="sxs-lookup"><span data-stu-id="48c03-120">Creating a Web application project and a Page</span></span>

<a id="sectionToggle0"></a>

<span data-ttu-id="48c03-121">チュートリアルのこの部分では、Web アプリケーション プロジェクトを作成し、新しいページを追加します。</span><span class="sxs-lookup"><span data-stu-id="48c03-121">In this part of the walkthrough, you will create a Web application project and add a new page to it.</span></span> <span data-ttu-id="48c03-122">また、HTML テキストを追加し、お使いのブラウザーでページを実行します。</span><span class="sxs-lookup"><span data-stu-id="48c03-122">You will also add HTML text and run the page in your browser.</span></span>

### <a name="to-create-a-web-application-project"></a><span data-ttu-id="48c03-123">Web アプリケーション プロジェクトを作成するには</span><span class="sxs-lookup"><span data-stu-id="48c03-123">To create a Web application project</span></span>

1. <span data-ttu-id="48c03-124">Microsoft Visual Studio を開きます。</span><span class="sxs-lookup"><span data-stu-id="48c03-124">Open Microsoft Visual Studio.</span></span>
2. <span data-ttu-id="48c03-125">**[ファイル]** メニューの **[新しいプロジェクト]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="48c03-125">On the **File** menu, select **New Project**.</span></span>  
    <span data-ttu-id="48c03-126">![[ファイル] メニュー](creating-a-basic-web-forms-page/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="48c03-126">![File Menu](creating-a-basic-web-forms-page/_static/image1.png)</span></span>

    <span data-ttu-id="48c03-127">**[新しいプロジェクト]** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="48c03-127">The **New Project** dialog box appears.</span></span>
3. <span data-ttu-id="48c03-128">選択、**テンプレート** - &gt; **Visual c#**  - &gt; **Web**左側のテンプレート グループ。</span><span class="sxs-lookup"><span data-stu-id="48c03-128">Select the **Templates** -&gt; **Visual C#** -&gt; **Web** templates group on the left.</span></span>
4. <span data-ttu-id="48c03-129">選択、 **ASP.NET Web アプリケーション**中央の列のテンプレート。</span><span class="sxs-lookup"><span data-stu-id="48c03-129">Choose the **ASP.NET Web Application** template in the center column.</span></span>
5. <span data-ttu-id="48c03-130">プロジェクトに名前を***BasicWebApp***  をクリックし、 **OK**ボタン。</span><span class="sxs-lookup"><span data-stu-id="48c03-130">Name your project ***BasicWebApp*** and click the **OK** button.</span></span>   
<span data-ttu-id="48c03-131">![新しいプロジェクト ダイアログ ボックス](creating-a-basic-web-forms-page/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="48c03-131">![New Project dialog box](creating-a-basic-web-forms-page/_static/image2.png)</span></span>
6. <span data-ttu-id="48c03-132">次に、選択、 **Web フォーム**テンプレートをクリックして、 **OK**プロジェクトを作成するボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="48c03-132">Next, select the **Web Forms** template and click the **OK** button to create the project.</span></span>  
<span data-ttu-id="48c03-133">![新しい ASP.NET プロジェクト ダイアログ ボックス](creating-a-basic-web-forms-page/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="48c03-133">![New ASP.NET Project dialog box](creating-a-basic-web-forms-page/_static/image3.png)</span></span>  

    <span data-ttu-id="48c03-134">Visual Studio では、Web フォーム テンプレートに基づく事前構築済みの機能を含む新しいプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="48c03-134">Visual Studio creates a new project that includes prebuilt functionality based on the Web Forms template.</span></span> <span data-ttu-id="48c03-135">提供するだけでなく、 *Home.aspx*  ページで、 *About.aspx*  ページで、 *Contact.aspx* 、ページしますが、ユーザーを登録し、保存するメンバーシップ機能も含まれています自分の資格情報、web サイトにログインできるようにします。</span><span class="sxs-lookup"><span data-stu-id="48c03-135">It not only provides you with a *Home.aspx* page, an *About.aspx* page, a *Contact.aspx* page, but also includes membership functionality that registers users and saves their credentials so that they can log in to your website.</span></span> <span data-ttu-id="48c03-136">既定では、Visual Studio が表示、ページで新しいページが作成されると、**ソース**ビュー、ページの HTML 要素を確認できます。</span><span class="sxs-lookup"><span data-stu-id="48c03-136">When a new page is created, by default Visual Studio displays the page in **Source** view, where you can see the page's HTML elements.</span></span> <span data-ttu-id="48c03-137">次の図で表示される**ソース**という名前の新しい Web ページを作成した場合に表示*BasicWebApp.aspx*します。</span><span class="sxs-lookup"><span data-stu-id="48c03-137">The following illustration shows what you would see in **Source** view if you created a new Web page named *BasicWebApp.aspx*.</span></span>  
    <span data-ttu-id="48c03-138">![ソース ビュー](creating-a-basic-web-forms-page/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="48c03-138">![Source View](creating-a-basic-web-forms-page/_static/image4.png)</span></span>


### <a name="a-tour-of-the-visual-studio-web-development-environment"></a><span data-ttu-id="48c03-139">Visual Studio の Web 開発環境のツアー</span><span class="sxs-lookup"><span data-stu-id="48c03-139">A Tour of the Visual Studio Web Development Environment</span></span>


<span data-ttu-id="48c03-140">ページの変更を続行する前に、Visual Studio 開発環境について理解するおくと便利です。</span><span class="sxs-lookup"><span data-stu-id="48c03-140">Before you proceed by modifying the page, it is useful to familiarize yourself with the Visual Studio development environment.</span></span> <span data-ttu-id="48c03-141">次の図は、windows と Visual Studio と Visual Studio Express for Web で利用できるツールを示しています。</span><span class="sxs-lookup"><span data-stu-id="48c03-141">The following illustration shows you the windows and tools that are available in Visual Studio and Visual Studio Express for Web.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="48c03-142">この図では、既定の windows とウィンドウの場所を示します。</span><span class="sxs-lookup"><span data-stu-id="48c03-142">This diagram shows default windows and window locations.</span></span> <span data-ttu-id="48c03-143">**ビュー**メニューでは、追加のウィンドウを表示および再配置し、好みに合わせて大きさを変更することができます。</span><span class="sxs-lookup"><span data-stu-id="48c03-143">The **View** menu allows you to display additional windows, and to rearrange and resize windows to suit your preferences.</span></span> <span data-ttu-id="48c03-144">既に変更されて場合、ウィンドウを整列する、表示される内容と図が一致しません。</span><span class="sxs-lookup"><span data-stu-id="48c03-144">If changes have already been made to the window arrangement, what you see will not match the illustration.</span></span>


 <span data-ttu-id="48c03-145">Visual Studio 環境</span><span class="sxs-lookup"><span data-stu-id="48c03-145">The Visual Studio environment</span></span>

![Visual Studio 環境](creating-a-basic-web-forms-page/_static/image5.png)

### <a name="familiarize-yourself-with-the-web-designer"></a><span data-ttu-id="48c03-147">Web デザイナーに慣れ親しみ</span><span class="sxs-lookup"><span data-stu-id="48c03-147">Familiarize yourself with the Web designer</span></span>

<span data-ttu-id="48c03-148">上の図を確認し、次の一覧に一致するテキスト ウィンドウやツールを使用する、最もよくについて説明します。</span><span class="sxs-lookup"><span data-stu-id="48c03-148">Examine the above illustration and match the text to the following list, which describes the most commonly used windows and tools.</span></span> <span data-ttu-id="48c03-149">(すべての windows とここは表示されているツールだけでマーク、前の図。)</span><span class="sxs-lookup"><span data-stu-id="48c03-149">(Not all windows and tools that you see are listed here, only those marked in the preceding illustration.)</span></span>

- <span data-ttu-id="48c03-150">ツールバー。</span><span class="sxs-lookup"><span data-stu-id="48c03-150">Toolbars.</span></span> <span data-ttu-id="48c03-151">テキストや、検索テキストを書式設定するためのコマンドを提供します。</span><span class="sxs-lookup"><span data-stu-id="48c03-151">Provide commands for formatting text, finding text, and so on.</span></span> <span data-ttu-id="48c03-152">一部のツールバーで作業している場合にのみ使用可能な**デザイン**ビュー。</span><span class="sxs-lookup"><span data-stu-id="48c03-152">Some toolbars are available only when you are working in **Design** view.</span></span>
- <span data-ttu-id="48c03-153">**ソリューション エクスプ ローラー**ウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="48c03-153">**Solution Explorer** window.</span></span> <span data-ttu-id="48c03-154">Web アプリケーションでは、ファイルとフォルダーを表示します。</span><span class="sxs-lookup"><span data-stu-id="48c03-154">Displays the files and folders in your Web application.</span></span>
- <span data-ttu-id="48c03-155">ドキュメント ウィンドウです。</span><span class="sxs-lookup"><span data-stu-id="48c03-155">Document window.</span></span> <span data-ttu-id="48c03-156">タブ付きウィンドウで作業しているドキュメントを表示します。</span><span class="sxs-lookup"><span data-stu-id="48c03-156">Displays the documents you are working on in tabbed windows.</span></span> <span data-ttu-id="48c03-157">タブをクリックしてドキュメントを切り替えることができます。</span><span class="sxs-lookup"><span data-stu-id="48c03-157">You can switch between documents by clicking tabs.</span></span>
- <span data-ttu-id="48c03-158">**プロパティ**ウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="48c03-158">**Properties** window.</span></span> <span data-ttu-id="48c03-159">ページ、HTML 要素、コントロール、およびその他のオブジェクトの設定を変更することができます。</span><span class="sxs-lookup"><span data-stu-id="48c03-159">Allows you to change settings for the page, HTML elements, controls, and other objects.</span></span>
- <span data-ttu-id="48c03-160">タブを表示します。</span><span class="sxs-lookup"><span data-stu-id="48c03-160">View tabs.</span></span> <span data-ttu-id="48c03-161">同じドキュメントのさまざまなビューが表示されます。</span><span class="sxs-lookup"><span data-stu-id="48c03-161">Present you with different views of the same document.</span></span> <span data-ttu-id="48c03-162">**デザイン**ビューは、WYSIWYG に近い編集サーフェイス。</span><span class="sxs-lookup"><span data-stu-id="48c03-162">**Design** view is a near-WYSIWYG editing surface.</span></span> <span data-ttu-id="48c03-163">**ソース**ビューは、ページの HTML エディター。</span><span class="sxs-lookup"><span data-stu-id="48c03-163">**Source** view is the HTML editor for the page.</span></span> <span data-ttu-id="48c03-164">**分割**ビューでは、両方が表示されます、**デザイン**ビューと**ソース**ドキュメントのビュー。</span><span class="sxs-lookup"><span data-stu-id="48c03-164">**Split** view displays both the **Design** view and the **Source** view for the document.</span></span> <span data-ttu-id="48c03-165">使用する、**デザイン**と**ソース**このチュートリアルの後半でのビュー。</span><span class="sxs-lookup"><span data-stu-id="48c03-165">You will work with the **Design** and **Source** views later in this walkthrough.</span></span> <span data-ttu-id="48c03-166">Web ページを開きたい場合**デザイン**ビュー、**ツール** メニューのをクリックして**オプション**を選択、 **HTML デザイナー**ノード、および変更、**ページで開始**オプション。</span><span class="sxs-lookup"><span data-stu-id="48c03-166">If you prefer to open Web pages in **Design** view, on the **Tools** menu, click **Options**, select the **HTML Designer** node, and change the **Start Pages In** option.</span></span>
- <span data-ttu-id="48c03-167">**ツールボックス**します。</span><span class="sxs-lookup"><span data-stu-id="48c03-167">**ToolBox**.</span></span> <span data-ttu-id="48c03-168">コントロールと、ページ上にドラッグできる HTML 要素を提供します。</span><span class="sxs-lookup"><span data-stu-id="48c03-168">Provides controls and HTML elements that you can drag onto your page.</span></span> <span data-ttu-id="48c03-169">**ツールボックス**要素が共通の関数によってグループ化されます。</span><span class="sxs-lookup"><span data-stu-id="48c03-169">**Toolbox** elements are grouped by common function.</span></span>
- <span data-ttu-id="48c03-170">S **ーバー エクスプ ローラー**します。</span><span class="sxs-lookup"><span data-stu-id="48c03-170">S **erver Explorer**.</span></span> <span data-ttu-id="48c03-171">データベース接続が表示されます。</span><span class="sxs-lookup"><span data-stu-id="48c03-171">Displays database connections.</span></span> <span data-ttu-id="48c03-172">サーバー エクスプ ローラーが表示されない場合は、表示 メニューで、サーバー エクスプ ローラー をクリックします。</span><span class="sxs-lookup"><span data-stu-id="48c03-172">If Server Explorer is not visible, on the View menu, click Server Explorer.</span></span>


### <a name="creating-a-new-aspnet-web-forms-page"></a><span data-ttu-id="48c03-173">新しい ASP.NET Web フォーム ページの作成</span><span class="sxs-lookup"><span data-stu-id="48c03-173">Creating a new ASP.NET Web Forms Page</span></span>


<span data-ttu-id="48c03-174">新しい Web フォーム アプリケーションを作成する場合、 **ASP.NET Web アプリケーション**プロジェクト テンプレートを Visual Studio の ASP.NET ページ (Web フォーム ページ) という名前を追加します*Default.aspx*他のいくつかのファイルだけでなく、フォルダーを選択します。</span><span class="sxs-lookup"><span data-stu-id="48c03-174">When you create a new Web Forms application using the **ASP.NET Web Application** project template, Visual Studio adds an ASP.NET page (Web Forms page) named *Default.aspx*, as well as several other files and folders.</span></span> <span data-ttu-id="48c03-175">使用することができます、 *Default.aspx*ページ、Web アプリケーションのホーム ページとして。</span><span class="sxs-lookup"><span data-stu-id="48c03-175">You can use the *Default.aspx* page as the home page for your Web application.</span></span> <span data-ttu-id="48c03-176">ただし、このチュートリアルでは作成し、新しいページを使用します。</span><span class="sxs-lookup"><span data-stu-id="48c03-176">However, for this walkthrough, you will create and work with a new page.</span></span>

### <a name="to-add-a-page-to-the-web-application"></a><span data-ttu-id="48c03-177">Web アプリケーションにページを追加するには</span><span class="sxs-lookup"><span data-stu-id="48c03-177">To add a page to the Web application</span></span>


1. <span data-ttu-id="48c03-178">閉じる、 *Default.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="48c03-178">Close the *Default.aspx* page.</span></span> <span data-ttu-id="48c03-179">これを行うには、ファイル名を表示するタブをクリックし、閉じるオプションをクリックします。</span><span class="sxs-lookup"><span data-stu-id="48c03-179">To do this, click the tab that displays the file name and then click the close option.</span></span>
2. <span data-ttu-id="48c03-180">**ソリューション エクスプ ローラー**、Web アプリケーションの名前を右クリックし (アプリケーション名は、このチュートリアルでは**BasicWebSite**)、をクリックし、**追加** - &gt;**新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="48c03-180">In **Solution Explorer**, right-click the Web application name (in this tutorial the application name is **BasicWebSite**), and then click **Add** -&gt; **New Item**.</span></span>   
<span data-ttu-id="48c03-181">**[新しい項目の追加]** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="48c03-181">The **Add New Item** dialog box is displayed.</span></span>
3. <span data-ttu-id="48c03-182">選択、 **Visual c#**  - &gt; **Web**左側のテンプレート グループ。</span><span class="sxs-lookup"><span data-stu-id="48c03-182">Select the **Visual C#** -&gt; **Web** templates group on the left.</span></span> <span data-ttu-id="48c03-183">次に、選択**Web フォーム**中央から一覧表示し、名前を*名前*します。</span><span class="sxs-lookup"><span data-stu-id="48c03-183">Then, select **Web Form** from the middle list and name it *FirstWebPage.aspx*.</span></span>   
    <span data-ttu-id="48c03-184">![新しい項目 ダイアログ ボックスを追加します。](creating-a-basic-web-forms-page/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="48c03-184">![Add New Item dialog box](creating-a-basic-web-forms-page/_static/image6.png)</span></span>
4. <span data-ttu-id="48c03-185">クリックして**追加**web ページ、プロジェクトを追加します。</span><span class="sxs-lookup"><span data-stu-id="48c03-185">Click **Add** to add the web page to your project.</span></span>  
<span data-ttu-id="48c03-186">Visual Studio では、新しいページを作成し、それを開きます。</span><span class="sxs-lookup"><span data-stu-id="48c03-186">Visual Studio creates the new page and opens it.</span></span>


### <a name="adding-html-to-the-page"></a><span data-ttu-id="48c03-187">ページに HTML を追加します。</span><span class="sxs-lookup"><span data-stu-id="48c03-187">Adding HTML to the Page</span></span>


<span data-ttu-id="48c03-188">このチュートリアルでは、ページに静的テキストを追加します。</span><span class="sxs-lookup"><span data-stu-id="48c03-188">In this part of the walkthrough, you will add some static text to the page.</span></span>

### <a name="to-add-text-to-the-page"></a><span data-ttu-id="48c03-189">テキスト ページを追加するには</span><span class="sxs-lookup"><span data-stu-id="48c03-189">To add text to the page</span></span>


1. <span data-ttu-id="48c03-190">ドキュメント ウィンドウの下部で、をクリックして、**デザイン** タブに切り替える**デザイン**ビュー。</span><span class="sxs-lookup"><span data-stu-id="48c03-190">At the bottom of the document window, click the **Design** tab to switch to **Design** view.</span></span>

    <span data-ttu-id="48c03-191">デザイン ビューでは、WYSIWYG に近い方法で、現在のページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="48c03-191">Design view displays the current page in a WYSIWYG-like way.</span></span> <span data-ttu-id="48c03-192">この時点では、必要はありません、テキストや ページで、コントロール、ページは空白で、四角形の輪郭を示す破線。</span><span class="sxs-lookup"><span data-stu-id="48c03-192">At this point, you do not have any text or controls on the page, so the page is blank except for a dashed line that outlines a rectangle.</span></span> <span data-ttu-id="48c03-193">この四角形を表す、 **div**ページ上の要素。</span><span class="sxs-lookup"><span data-stu-id="48c03-193">This rectangle represents a **div** element on the page.</span></span>
2. <span data-ttu-id="48c03-194">破線で示されている四角形の内側をクリックします。</span><span class="sxs-lookup"><span data-stu-id="48c03-194">Click inside the rectangle that is outlined by a dashed line.</span></span>
3. <span data-ttu-id="48c03-195">型**Visual Web Developer へようこそ**キーを押します**ENTER** 2 回です。</span><span class="sxs-lookup"><span data-stu-id="48c03-195">Type **Welcome to Visual Web Developer** and press **ENTER** twice.</span></span>

    <span data-ttu-id="48c03-196">次の図で入力したテキスト**デザイン**ビュー。</span><span class="sxs-lookup"><span data-stu-id="48c03-196">The following illustration shows the text you typed in **Design** view.</span></span>

    <span data-ttu-id="48c03-197">![デザイン ビューでの開始テキスト](creating-a-basic-web-forms-page/_static/image7.png "デザイン ビューでの開始テキスト")</span><span class="sxs-lookup"><span data-stu-id="48c03-197">![Welcome text in Design view](creating-a-basic-web-forms-page/_static/image7.png "Welcome text in Design view")</span></span>
4. <span data-ttu-id="48c03-198">切り替える**ソース**ビュー。</span><span class="sxs-lookup"><span data-stu-id="48c03-198">Switch to **Source** view.</span></span>

    <span data-ttu-id="48c03-199">HTML を表示できます**ソース**で入力したときに作成したビュー**デザイン**ビュー。</span><span class="sxs-lookup"><span data-stu-id="48c03-199">You can see the HTML in **Source** view that you created when you typed in **Design** view.</span></span>  
    <span data-ttu-id="48c03-200">![静的テキストを含む Web ページ](creating-a-basic-web-forms-page/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="48c03-200">![Web Page with Static Text](creating-a-basic-web-forms-page/_static/image8.png)</span></span>


### <a name="running-the-page"></a><span data-ttu-id="48c03-201">ページを実行します。</span><span class="sxs-lookup"><span data-stu-id="48c03-201">Running the Page</span></span>


<span data-ttu-id="48c03-202">ページにコントロールを追加して続行する前に、最初に実行できます。</span><span class="sxs-lookup"><span data-stu-id="48c03-202">Before you proceed by adding controls to the page, you can first run it.</span></span>

### <a name="to-run-the-page"></a><span data-ttu-id="48c03-203">ページの実行</span><span class="sxs-lookup"><span data-stu-id="48c03-203">To run the page</span></span>


1. <span data-ttu-id="48c03-204">**ソリューション エクスプ ローラー**、右クリック*名前*選択と**スタート ページとして設定**します。</span><span class="sxs-lookup"><span data-stu-id="48c03-204">In **Solution Explorer**, right-click *FirstWebPage.aspx* and select **Set as Start Page**.</span></span>
2. <span data-ttu-id="48c03-205">キーを押して**CTRL + F5**ページを実行します。</span><span class="sxs-lookup"><span data-stu-id="48c03-205">Press **CTRL+F5** to run the page.</span></span>

    <span data-ttu-id="48c03-206">ページがブラウザーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="48c03-206">The page is displayed in the browser.</span></span> <span data-ttu-id="48c03-207">作成したページには、ファイル名拡張子が *.aspx*、現在の HTML ページのように実行されます。</span><span class="sxs-lookup"><span data-stu-id="48c03-207">Although the page you created has a file-name extension of *.aspx*, it currently runs like any HTML page.</span></span>

    <span data-ttu-id="48c03-208">ブラウザーでページを表示するもでページを右クリックしてできます**ソリューション エクスプ ローラー**選択**ブラウザーで表示**します。</span><span class="sxs-lookup"><span data-stu-id="48c03-208">To display a page in the browser you can also right-click the page in **Solution Explorer** and select **View in Browser**.</span></span>
3. <span data-ttu-id="48c03-209">Web アプリケーションを停止するブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="48c03-209">Close the browser to stop the Web application.</span></span>


## <a name="adding-and-programming-controls"></a><span data-ttu-id="48c03-210">追加して、コントロールのプログラミング</span><span class="sxs-lookup"><span data-stu-id="48c03-210">Adding and Programming Controls</span></span>


<a id="sectionToggle1"></a>

<span data-ttu-id="48c03-211">これで、ページにサーバー コントロールを追加します。</span><span class="sxs-lookup"><span data-stu-id="48c03-211">You will now add server controls to the page.</span></span> <span data-ttu-id="48c03-212">ボタン、ラベル、テキスト ボックス、およびその他の使い慣れたコントロールなどのサーバー コントロールでは、Web フォーム ページの一般的なフォーム処理機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="48c03-212">Server controls, such as buttons, labels, text boxes, and other familiar controls, provide typical form-processing capabilities for your Web Forms pages.</span></span> <span data-ttu-id="48c03-213">ただし、クライアントではなく、サーバーで実行されるコードでコントロールをプログラミングできます。</span><span class="sxs-lookup"><span data-stu-id="48c03-213">However, you can program the controls with code that runs on the server, rather than the client.</span></span>

<span data-ttu-id="48c03-214">追加、[ボタン](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)コントロール、 [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx)コントロールと[ラベル](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)コントロールをページと、処理するコードを記述、 [ をクリックして](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx)イベントを[ボタン](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)コントロール。</span><span class="sxs-lookup"><span data-stu-id="48c03-214">You will add a [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) control, a [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) control, and a [Label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) control to the page and write code to handle the [Click](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) event for the [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) control.</span></span>

### <a name="to-add-controls-to-the-page"></a><span data-ttu-id="48c03-215">ページにコントロールを追加するには</span><span class="sxs-lookup"><span data-stu-id="48c03-215">To add controls to the page</span></span>


1. <span data-ttu-id="48c03-216">をクリックして、**デザイン** タブに切り替える**デザイン**ビュー。</span><span class="sxs-lookup"><span data-stu-id="48c03-216">Click the **Design** tab to switch to **Design** view.</span></span>
2. <span data-ttu-id="48c03-217">末尾にカーソルを置きます、 **Visual Web Developer へようこそ**テキストとキーを押して**ENTER**で確保するために 5 つ以上の時間、 **div**要素のボックスです。</span><span class="sxs-lookup"><span data-stu-id="48c03-217">Put the insertion point at the end of the **Welcome to Visual Web Developer** text and press **ENTER** five or more times to make some room in the **div** element box.</span></span>
3. <span data-ttu-id="48c03-218">**ツールボックス**、展開、**標準**が展開されていない場合にグループ化します。</span><span class="sxs-lookup"><span data-stu-id="48c03-218">In the **Toolbox**, expand the **Standard** group if it is not already expanded.</span></span>  
<span data-ttu-id="48c03-219">展開する必要がありますに注意してください、**ツールボックス**表示するための左側のウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="48c03-219">Note that you may need to expand the **Toolbox** window on the left to view it.</span></span>
4. <span data-ttu-id="48c03-220">ドラッグ、 [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx)ページ上に制御し、ドロップの途中で、 **div**を持つ要素のボックス**Visual Web Developer へようこそ**最初の行にします。</span><span class="sxs-lookup"><span data-stu-id="48c03-220">Drag a [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) control onto the page and drop it in the middle of the **div** element box that has **Welcome to Visual Web Developer** in the first line.</span></span>
5. <span data-ttu-id="48c03-221">ドラッグ、[ボタン](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)ページ上に制御しの右側にドロップ、 [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx)コントロール。</span><span class="sxs-lookup"><span data-stu-id="48c03-221">Drag a [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) control onto the page and drop it to the right of the [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) control.</span></span>
6. <span data-ttu-id="48c03-222">ドラッグ、[ラベル](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)ページ上に制御し、下の個別の行にドロップ、[ボタン](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)コントロール。</span><span class="sxs-lookup"><span data-stu-id="48c03-222">Drag a [Label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) control onto the page and drop it on a separate line below the [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) control.</span></span>
7. <span data-ttu-id="48c03-223">上にカーソルを配置、 [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) 、制御し、入力**名を入力します:** します。</span><span class="sxs-lookup"><span data-stu-id="48c03-223">Put the insertion point above the [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) control, and then type **Enter your name:** .</span></span>

    <span data-ttu-id="48c03-224">この静的な HTML テキストがのキャプション、 [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx)コントロール。</span><span class="sxs-lookup"><span data-stu-id="48c03-224">This static HTML text is the caption for the [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) control.</span></span> <span data-ttu-id="48c03-225">静的な HTML と同じページ上のサーバー コントロールを混在させることができます。</span><span class="sxs-lookup"><span data-stu-id="48c03-225">You can mix static HTML and server controls on the same page.</span></span> <span data-ttu-id="48c03-226">次の図は、3 つのコントロールでの表示方法を示します**デザイン**ビュー。</span><span class="sxs-lookup"><span data-stu-id="48c03-226">The following illustration shows how the three controls appear in **Design** view.</span></span>

    <span data-ttu-id="48c03-227">![デザイン ビューで 3 つのコントロール](creating-a-basic-web-forms-page/_static/image9.png "デザイン ビューで 3 つのコントロール")</span><span class="sxs-lookup"><span data-stu-id="48c03-227">![Three controls in Design view](creating-a-basic-web-forms-page/_static/image9.png "Three controls in Design view")</span></span>


### <a name="setting-control-properties"></a><span data-ttu-id="48c03-228">コントロールのプロパティの設定</span><span class="sxs-lookup"><span data-stu-id="48c03-228">Setting Control Properties</span></span>


<span data-ttu-id="48c03-229">Visual Studio では、ページ上のコントロールのプロパティを設定するさまざまな方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="48c03-229">Visual Studio offers you various ways to set the properties of controls on the page.</span></span> <span data-ttu-id="48c03-230">このチュートリアルでは、両方のプロパティを設定**デザイン**ビューと**ソース**ビュー。</span><span class="sxs-lookup"><span data-stu-id="48c03-230">In this part of the walkthrough, you will set properties in both **Design** view and **Source** view.</span></span>

### <a name="to-set-control-properties"></a><span data-ttu-id="48c03-231">コントロールのプロパティを設定するには</span><span class="sxs-lookup"><span data-stu-id="48c03-231">To set control properties</span></span>


1. <span data-ttu-id="48c03-232">最初に、表示、**プロパティ**からを選択して、windows、**ビュー**メニュー -&gt; **その他の Windows**  - &gt; **プロパティ ウィンドウ**します。</span><span class="sxs-lookup"><span data-stu-id="48c03-232">First, display the **Properties** windows by selecting from the **View** menu -&gt; **Other Windows** -&gt; **Properies Window**.</span></span> <span data-ttu-id="48c03-233">選択または**F4**を表示する、**プロパティ**ウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="48c03-233">You could alternatively select **F4** to display the **Properties** window.</span></span>
2. <span data-ttu-id="48c03-234">選択、[ボタン](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)コントロール、し、**プロパティ**ウィンドウで、設定の値**テキスト**に**表示名**します。</span><span class="sxs-lookup"><span data-stu-id="48c03-234">Select the [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) control, and then in the **Properties** window, set the value of **Text** to **Display Name**.</span></span> <span data-ttu-id="48c03-235">入力したテキストは、次の図に示すように、デザイナーで、ボタンに表示されます。</span><span class="sxs-lookup"><span data-stu-id="48c03-235">The text you entered appears on the button in the designer, as shown in the following illustration.</span></span>

    <span data-ttu-id="48c03-236">![ボタン設定テキスト](creating-a-basic-web-forms-page/_static/image10.png "設定ボタンのテキスト")</span><span class="sxs-lookup"><span data-stu-id="48c03-236">![Set Button text](creating-a-basic-web-forms-page/_static/image10.png "Set Button text")</span></span>
3. <span data-ttu-id="48c03-237">切り替える**ソース**ビュー。</span><span class="sxs-lookup"><span data-stu-id="48c03-237">Switch to **Source** view.</span></span>

    <span data-ttu-id="48c03-238">**ソース**ビューには、Visual Studio のサーバー コントロールの作成した要素を含む、ページの HTML が表示されます。</span><span class="sxs-lookup"><span data-stu-id="48c03-238">**Source** view displays the HTML for the page, including the elements that Visual Studio has created for the server controls.</span></span> <span data-ttu-id="48c03-239">コントロールは、HTML に似た構文で宣言**asp:** 属性を含めると**runat =&quot;server&quot;** します。</span><span class="sxs-lookup"><span data-stu-id="48c03-239">Controls are declared using HTML-like syntax, except that the tags use the prefix **asp:** and include the attribute **runat=&quot;server&quot;**.</span></span>

    <span data-ttu-id="48c03-240">コントロールのプロパティは、属性として宣言されます。</span><span class="sxs-lookup"><span data-stu-id="48c03-240">Control properties are declared as attributes.</span></span> <span data-ttu-id="48c03-241">たとえば、設定、[テキスト](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.text.aspx)プロパティを[ボタン](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)コントロールを手順 1 で、実際の設定、**テキスト**コントロールのマークアップ内の属性。</span><span class="sxs-lookup"><span data-stu-id="48c03-241">For example, when you set the [Text](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.text.aspx) property for the [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) control, in step 1, you were actually setting the **Text** attribute in the control's markup.</span></span>

    > [!NOTE] 
    > 
    > <span data-ttu-id="48c03-242">内のすべてのコントロールは、**フォーム**要素、属性を持つ**runat =&quot;server&quot;** します。</span><span class="sxs-lookup"><span data-stu-id="48c03-242">All the controls are inside a **form** element, which also has the attribute **runat=&quot;server&quot;**.</span></span> <span data-ttu-id="48c03-243">**Runat =&quot;server&quot;** 属性と**asp:** プレフィックスは、によって処理される ASP.NET サーバー ページが実行されるように、コントロールのタグは、コントロールをマークします。</span><span class="sxs-lookup"><span data-stu-id="48c03-243">The **runat=&quot;server&quot;** attribute and the **asp:** prefix for control tags mark the controls so that they are processed by ASP.NET on the server when the page runs.</span></span> <span data-ttu-id="48c03-244">外部コード**&lt;フォームの runat =&quot;サーバー&quot; &gt;** と**&lt;スクリプトの runat =&quot;server&quot; &gt;** 要素は、ASP.NET コードが持つ開始タグを含む要素内にある必要がありますブラウザーにそのまま送信は、 **runat =&quot;server&quot;** 属性。</span><span class="sxs-lookup"><span data-stu-id="48c03-244">Code outside of **&lt;form runat=&quot;server&quot;&gt;** and **&lt;script runat=&quot;server&quot;&gt;** elements is sent unchanged to the browser, which is why the ASP.NET code must be inside an element whose opening tag contains the **runat=&quot;server&quot;** attribute.</span></span>
4. <span data-ttu-id="48c03-245">追加のプロパティを次に、追加は、[ラベル](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)コントロール。</span><span class="sxs-lookup"><span data-stu-id="48c03-245">Next, you will add an additional property to the [Label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)  control.</span></span> <span data-ttu-id="48c03-246">配置後に、挿入ポイントを直接**Asp:label**で、 **&lt;Asp:label&gt;** タグ、およびキーを押します**space キーを押す**します。</span><span class="sxs-lookup"><span data-stu-id="48c03-246">Put the insertion point directly after **asp:Label** in the **&lt;asp:Label&gt;** tag, and then press **SPACEBAR**.</span></span>

    <span data-ttu-id="48c03-247">設定できる使用可能なプロパティの一覧を表示するドロップダウン リストが表示されます、[ラベル](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)コントロール。</span><span class="sxs-lookup"><span data-stu-id="48c03-247">A drop-down list appears that displays the list of available properties you can set for a [Label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) control.</span></span> <span data-ttu-id="48c03-248">呼ばれるこの機能により、 **IntelliSense**で役立つ**ソース**サーバー コントロール、HTML 要素、およびその他の項目の構文を使用して、ページのビュー。</span><span class="sxs-lookup"><span data-stu-id="48c03-248">This feature, referred to as **IntelliSense**, helps you in **Source** view with the syntax of server controls, HTML elements, and other items on the page.</span></span> <span data-ttu-id="48c03-249">次の図は、 **IntelliSense**のドロップダウン リスト、[ラベル](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)コントロール。</span><span class="sxs-lookup"><span data-stu-id="48c03-249">The following illustration shows the **IntelliSense** drop-down list for the [Label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) control.</span></span>

    <span data-ttu-id="48c03-250">![IntelliSense 属性](creating-a-basic-web-forms-page/_static/image11.png "IntelliSense 属性")</span><span class="sxs-lookup"><span data-stu-id="48c03-250">![IntelliSense attributes](creating-a-basic-web-forms-page/_static/image11.png "IntelliSense attributes")</span></span>
5. <span data-ttu-id="48c03-251">選択**ForeColor**し、等号 (=) を入力します。</span><span class="sxs-lookup"><span data-stu-id="48c03-251">Select **ForeColor** and then type an equal sign.</span></span>

    <span data-ttu-id="48c03-252">IntelliSense では、色の一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="48c03-252">IntelliSense displays a list of colors.</span></span>

    > [!NOTE] 
    > 
    > <span data-ttu-id="48c03-253">表示することができます、 **IntelliSense**ドロップダウン リストを押して、いつでも**CTRL + J**コードを表示するときにします。</span><span class="sxs-lookup"><span data-stu-id="48c03-253">You can display an **IntelliSense** drop-down list at any time by pressing **CTRL+J** when viewing code.</span></span>
6. <span data-ttu-id="48c03-254">色を選択、 **[ラベル](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)** コントロールのテキスト。</span><span class="sxs-lookup"><span data-stu-id="48c03-254">Select a color for the **[Label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)** control's text.</span></span> <span data-ttu-id="48c03-255">白の背景を読み取る十分に暗い色を選択することを確認します。</span><span class="sxs-lookup"><span data-stu-id="48c03-255">Make sure you select a color that is dark enough to read against a white background.</span></span>

    <span data-ttu-id="48c03-256">**ForeColor**属性には、二重引用符を含め、選択した色が完了しました。</span><span class="sxs-lookup"><span data-stu-id="48c03-256">The **ForeColor** attribute is completed with the color that you have selected, including the closing quotation mark.</span></span>


### <a name="programming-the-button-control"></a><span data-ttu-id="48c03-257">ボタン コントロールのプログラミング</span><span class="sxs-lookup"><span data-stu-id="48c03-257">Programming the Button Control</span></span>


<span data-ttu-id="48c03-258">このチュートリアルでは、名前を読み取り、ユーザーがテキスト ボックスを入力しで名前を表示するコードを記述する、[ラベル](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)コントロール。</span><span class="sxs-lookup"><span data-stu-id="48c03-258">For this walkthrough, you will write code that reads the name that the user enters into the text box and then displays the name in the [Label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) control.</span></span>

### <a name="add-a-default-button-event-handler"></a><span data-ttu-id="48c03-259">既定のボタン イベント ハンドラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="48c03-259">Add a default button event handler</span></span>


1. <span data-ttu-id="48c03-260">切り替える**デザイン**ビュー。</span><span class="sxs-lookup"><span data-stu-id="48c03-260">Switch to **Design** view.</span></span>
2. <span data-ttu-id="48c03-261">ダブルクリックして、[ボタン](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)コントロール。</span><span class="sxs-lookup"><span data-stu-id="48c03-261">Double-click the [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) control.</span></span>

    <span data-ttu-id="48c03-262">既定では、Visual Studio の分離コード ファイルに切り替わりますのスケルトン イベント ハンドラーを作成、[ボタン](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)コントロールの既定のイベントを[クリックして](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx)イベント。</span><span class="sxs-lookup"><span data-stu-id="48c03-262">By default, Visual Studio switches to a code-behind file and creates a skeleton event handler for the [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) control's default event, the [Click](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) event.</span></span> <span data-ttu-id="48c03-263">分離コード ファイルは、(c#) など、サーバー コードから、UI のマークアップ (HTML など) を分離します。</span><span class="sxs-lookup"><span data-stu-id="48c03-263">The code-behind file separates your UI markup (such as HTML) from your server code (such as C#).</span></span>   
   <span data-ttu-id="48c03-264">カーソルがこのイベント ハンドラーのコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="48c03-264">The cursor is positioned to added code for this event handler.</span></span>

    > [!NOTE] 
    > 
    > <span data-ttu-id="48c03-265">コントロールをダブルクリック**デザイン**ビューは、イベント ハンドラーを作成するいくつかの方法の 1 つにすぎません。</span><span class="sxs-lookup"><span data-stu-id="48c03-265">Double-clicking a control in **Design** view is just one of several ways you can create event handlers.</span></span>
3. <span data-ttu-id="48c03-266">内で、 **Button1\_クリックして**イベント ハンドラーで、「 **Label1**ピリオドが続く (**.**)。</span><span class="sxs-lookup"><span data-stu-id="48c03-266">Inside the **Button1\_Click** event handler, type **Label1** followed by a period (**.**).</span></span>

    <span data-ttu-id="48c03-267">後にピリオドを入力すると、 **ID**ラベルの (**Label1**)、Visual Studio の利用可能なメンバーの一覧を表示する、[ラベル](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)コントロールが、次に示すように図。</span><span class="sxs-lookup"><span data-stu-id="48c03-267">When you type the period after the **ID** of the label (**Label1**), Visual Studio displays a list of available members for the [Label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) control, as shown in the following illustration.</span></span> <span data-ttu-id="48c03-268">メンバー プロパティでは通常、メソッド、またはイベント。</span><span class="sxs-lookup"><span data-stu-id="48c03-268">A member commonly a property, method, or event.</span></span>

    <span data-ttu-id="48c03-269">![コード ビューでの IntelliSense](creating-a-basic-web-forms-page/_static/image12.png "コード ビューでの IntelliSense")</span><span class="sxs-lookup"><span data-stu-id="48c03-269">![IntelliSense in Code view](creating-a-basic-web-forms-page/_static/image12.png "IntelliSense in Code view")</span></span>
4. <span data-ttu-id="48c03-270">[完了]、**クリックして**を読み取り、次のコード例で示すように、ボタンのイベント ハンドラー。</span><span class="sxs-lookup"><span data-stu-id="48c03-270">Finish the **Click** event handler for the button so that it reads as shown in the following code example.</span></span>

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample1.cs?highlight=3)]

    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample2.vb?highlight=2)]
5. <span data-ttu-id="48c03-271">表示に戻り、**ソース**ビューを右クリックし、HTML マークアップの*名前*で、**ソリューション エクスプ ローラー**を選択して**ビューマークアップ**します。</span><span class="sxs-lookup"><span data-stu-id="48c03-271">Switch back to viewing the **Source** view of your HTML markup by right-clicking *FirstWebPage.aspx* in the **Solution Explorer** and selecting **View Markup**.</span></span>
6. <span data-ttu-id="48c03-272">スクロールして、 **&lt;Asp:button&gt;** 要素。</span><span class="sxs-lookup"><span data-stu-id="48c03-272">Scroll to the **&lt;asp:Button&gt;** element.</span></span> <span data-ttu-id="48c03-273">なお、 **&lt;Asp:button&gt;** 要素が属性を持つようになりました**onclick =&quot;Button1\_クリックして&quot;** します。</span><span class="sxs-lookup"><span data-stu-id="48c03-273">Note that the **&lt;asp:Button&gt;** element now has the attribute **onclick=&quot;Button1\_Click&quot;**.</span></span>

    <span data-ttu-id="48c03-274">この属性のバインド ボタンの[クリックして](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx)イベント前の手順でコード化されたハンドラー メソッドにします。</span><span class="sxs-lookup"><span data-stu-id="48c03-274">This attribute binds the button's [Click](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) event to the handler method you coded in the previous step.</span></span>

    <span data-ttu-id="48c03-275">イベント ハンドラー メソッドは、任意の名前を持つことができます。表示される名前は、Visual Studio によって作成された既定の名前です。</span><span class="sxs-lookup"><span data-stu-id="48c03-275">Event handler methods can have any name; the name you see is the default name created by Visual Studio.</span></span> <span data-ttu-id="48c03-276">重要な点は、名前に使用する、 **OnClick** HTML 属性では分離コードで定義されているメソッドの名前と一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="48c03-276">The important point is that the name used for the **OnClick** attribute in the HTML must match the name of a method defined in the code-behind.</span></span>


### <a name="running-the-page"></a><span data-ttu-id="48c03-277">ページを実行します。</span><span class="sxs-lookup"><span data-stu-id="48c03-277">Running the Page</span></span>


<span data-ttu-id="48c03-278">ページ上のサーバー コントロールをテストできます。</span><span class="sxs-lookup"><span data-stu-id="48c03-278">You can now test the server controls on the page.</span></span>

### <a name="to-run-the-page"></a><span data-ttu-id="48c03-279">ページの実行</span><span class="sxs-lookup"><span data-stu-id="48c03-279">To run the page</span></span>


1. <span data-ttu-id="48c03-280">キーを押して**CTRL + F5**ブラウザーでページを実行します。</span><span class="sxs-lookup"><span data-stu-id="48c03-280">Press **CTRL+F5** to run the page in the browser.</span></span> <span data-ttu-id="48c03-281">エラーが発生する場合は、上記の手順を再確認します。</span><span class="sxs-lookup"><span data-stu-id="48c03-281">If an error occurs, recheck the steps above.</span></span>
2. <span data-ttu-id="48c03-282">テキスト ボックスに名前を入力し、をクリックして、**表示名**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="48c03-282">Enter a name into the text box and click the **Display Name** button.</span></span>

    <span data-ttu-id="48c03-283">入力した名前が表示されます、[ラベル](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)コントロール。</span><span class="sxs-lookup"><span data-stu-id="48c03-283">The name you entered is displayed in the [Label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) control.</span></span> <span data-ttu-id="48c03-284">ボタンをクリックするとに、ページが Web サーバーにポストされたことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="48c03-284">Note that when you click the button, the page is posted to the Web server.</span></span> <span data-ttu-id="48c03-285">ASP.NET ページを再作成、コードを実行 (この場合、[ボタン](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)コントロールの[ をクリックして](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx)イベント ハンドラーが実行される)、し、ブラウザーに新しいページを送信します。</span><span class="sxs-lookup"><span data-stu-id="48c03-285">ASP.NET then recreates the page, runs your code (in this case, the [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) control's [Click](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) event handler runs), and then sends the new page to the browser.</span></span> <span data-ttu-id="48c03-286">ブラウザーのステータス バーを視聴する場合、ページことのラウンド トリップ Web サーバーに、ボタンをクリックするたびが確認できます。</span><span class="sxs-lookup"><span data-stu-id="48c03-286">If you watch the status bar in the browser, you can see that the page is making a round trip to the Web server each time you click the button.</span></span>
3. <span data-ttu-id="48c03-287">ブラウザーで表示 ページの右クリックして実行しているページのソース**ソースの表示**します。</span><span class="sxs-lookup"><span data-stu-id="48c03-287">In the browser, view the source of the page you are running by right-clicking on the page and selecting **View source**.</span></span>

    <span data-ttu-id="48c03-288">ページのソース コードでは、サーバー コードなし HTML を表示します。</span><span class="sxs-lookup"><span data-stu-id="48c03-288">In the page source code, you see HTML without any server code.</span></span> <span data-ttu-id="48c03-289">具体的には、表示されない、 **&lt;asp:&gt;** で作業していた要素**ソース**ビュー。</span><span class="sxs-lookup"><span data-stu-id="48c03-289">Specifically, you do not see the **&lt;asp:&gt;** elements that you were working with in **Source** view.</span></span> <span data-ttu-id="48c03-290">ページの実行時に ASP.NET がサーバー コントロールを処理し、コントロールを表す機能を実行するページに HTML 要素をレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="48c03-290">When the page runs, ASP.NET processes the server controls and renders HTML elements to the page that perform the functions that represent the control.</span></span> <span data-ttu-id="48c03-291">たとえば、 **&lt;Asp:button&gt;** コントロールが HTML としてレンダリングされる**&lt;入力の種類 =&quot;送信&quot;&gt;** 要素。</span><span class="sxs-lookup"><span data-stu-id="48c03-291">For example, the **&lt;asp:Button&gt;** control is rendered as the HTML **&lt;input type=&quot;submit&quot;&gt;** element.</span></span>
4. <span data-ttu-id="48c03-292">ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="48c03-292">Close the browser.</span></span>


## <a name="working-with-additional-controls"></a><span data-ttu-id="48c03-293">その他のコントロールの操作</span><span class="sxs-lookup"><span data-stu-id="48c03-293">Working with Additional Controls</span></span>

<a id="sectionToggle2"></a>

<span data-ttu-id="48c03-294">このチュートリアルでは、機能、[カレンダー](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)一度に 1 か月間の日付を表示するコントロール。</span><span class="sxs-lookup"><span data-stu-id="48c03-294">In this part of the walkthrough, you will work with the [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) control, which displays dates a month at a time.</span></span> <span data-ttu-id="48c03-295">[カレンダー](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)コントロールで作業しているボタン、テキスト ボックス、およびラベルよりもさらに複雑なコントロールし、サーバー コントロールのいくつか他の機能を示しています。</span><span class="sxs-lookup"><span data-stu-id="48c03-295">The [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) control is a more complex control than the button, text box, and label you have been working with and illustrates some further capabilities of server controls.</span></span>

<span data-ttu-id="48c03-296">このセクションでは追加、 [System.Web.UI.WebControls.Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)コントロールをページと書式設定します。</span><span class="sxs-lookup"><span data-stu-id="48c03-296">In this section, you will add a [System.Web.UI.WebControls.Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) control to the page and format it.</span></span>

### <a name="to-add-a-calendar-control"></a><span data-ttu-id="48c03-297">カレンダー コントロールを追加するには</span><span class="sxs-lookup"><span data-stu-id="48c03-297">To add a Calendar control</span></span>


1. <span data-ttu-id="48c03-298">Visual Studio に切り替えます**デザイン**ビュー。</span><span class="sxs-lookup"><span data-stu-id="48c03-298">In Visual Studio, switch to **Design** view.</span></span>
2. <span data-ttu-id="48c03-299">**標準**のセクション、**ツールボックス**、ドラッグ、[カレンダー](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)ページ上に制御し、下にドロップ、 **div**要素ですその他のコントロールが含まれています。</span><span class="sxs-lookup"><span data-stu-id="48c03-299">From the **Standard** section of the **Toolbox**, drag a [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) control onto the page and drop it below the **div** element that contains the other controls.</span></span>

    <span data-ttu-id="48c03-300">カレンダーのスマート タグ パネルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="48c03-300">The calendar's smart tag panel is displayed.</span></span> <span data-ttu-id="48c03-301">パネルには、選択したコントロールの最も一般的なタスクを実行しやすくコマンドが表示されます。</span><span class="sxs-lookup"><span data-stu-id="48c03-301">The panel displays commands that make it easy for you to perform the most common tasks for the selected control.</span></span> <span data-ttu-id="48c03-302">次の図は、[カレンダー](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)コントロールで表示される**デザイン**ビュー。</span><span class="sxs-lookup"><span data-stu-id="48c03-302">The following illustration shows the [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) control as rendered in **Design** view.</span></span>

    <span data-ttu-id="48c03-303">![デザイン ビューでコントロールをカレンダー](creating-a-basic-web-forms-page/_static/image13.png "カレンダーのデザイン ビューでコントロール")</span><span class="sxs-lookup"><span data-stu-id="48c03-303">![Calendar control in Design view](creating-a-basic-web-forms-page/_static/image13.png "Calendar control in Design view")</span></span>
3. <span data-ttu-id="48c03-304">スマート タグ パネルで、選択**オート フォーマット**します。</span><span class="sxs-lookup"><span data-stu-id="48c03-304">In the smart tag panel, choose **Auto Format**.</span></span>

    <span data-ttu-id="48c03-305">**オート フォーマット** ダイアログ ボックスが表示されたら、予定表の書式指定スキームを選択できます。</span><span class="sxs-lookup"><span data-stu-id="48c03-305">The **Auto Format** dialog box is displayed, which allows you to select a formatting scheme for the calendar.</span></span> <span data-ttu-id="48c03-306">次の図は、**オート フォーマット**の ダイアログ ボックス、[カレンダー](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)コントロール。</span><span class="sxs-lookup"><span data-stu-id="48c03-306">The following illustration shows the **Auto Format** dialog box for the [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) control.</span></span>

    <span data-ttu-id="48c03-307">![オート フォーマット ダイアログ ボックス (Calendar コントロール)](creating-a-basic-web-forms-page/_static/image14.png "オート フォーマット ダイアログ ボックス (Calendar コントロール)")</span><span class="sxs-lookup"><span data-stu-id="48c03-307">![Auto Format dialog box (Calendar control)](creating-a-basic-web-forms-page/_static/image14.png "Auto Format dialog box (Calendar control)")</span></span>
4. <span data-ttu-id="48c03-308">**スキームを選択**一覧で、選択**単純な**順にクリックします**OK**します。</span><span class="sxs-lookup"><span data-stu-id="48c03-308">From the **Select a scheme** list, select **Simple** and then click **OK**.</span></span>
5. <span data-ttu-id="48c03-309">切り替える**ソース**ビュー。</span><span class="sxs-lookup"><span data-stu-id="48c03-309">Switch to **Source** view.</span></span>

    <span data-ttu-id="48c03-310">確認できます、  **&lt;asp: 予定表&gt;** 要素。</span><span class="sxs-lookup"><span data-stu-id="48c03-310">You can see the **&lt;asp:Calendar&gt;** element.</span></span> <span data-ttu-id="48c03-311">この要素は、先ほど作成した単純なコントロールの要素よりも長いです。</span><span class="sxs-lookup"><span data-stu-id="48c03-311">This element is much longer than the elements for the simple controls you created earlier.</span></span> <span data-ttu-id="48c03-312">これも含まれています、サブ要素など**&lt;この&gt;**、さまざまな書式設定を表します。</span><span class="sxs-lookup"><span data-stu-id="48c03-312">It also includes subelements, such as **&lt;WeekEndDayStyle&gt;**, which represent various formatting settings.</span></span> <span data-ttu-id="48c03-313">次の図は、[カレンダー](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)制御**ソース**ビュー。</span><span class="sxs-lookup"><span data-stu-id="48c03-313">The following illustration shows the [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) control in **Source** view.</span></span> <span data-ttu-id="48c03-314">(正確なマークアップに表示される**ソース**からの図は、ビューが若干異なる場合があります)。</span><span class="sxs-lookup"><span data-stu-id="48c03-314">(The exact markup that you see in **Source** view might differ slightly from the illustration.)</span></span>

    <span data-ttu-id="48c03-315">![ソース ビュー内のコントロールをカレンダー](creating-a-basic-web-forms-page/_static/image15.png "予定表のソース ビュー内のコントロール")</span><span class="sxs-lookup"><span data-stu-id="48c03-315">![Calendar control in Source view](creating-a-basic-web-forms-page/_static/image15.png "Calendar control in Source view")</span></span>


### <a name="programming-the-calendar-control"></a><span data-ttu-id="48c03-316">予定表コントロールのプログラミング</span><span class="sxs-lookup"><span data-stu-id="48c03-316">Programming the Calendar Control</span></span>


<span data-ttu-id="48c03-317">プログラムがのこのセクションで、[カレンダー](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)現在選択されている日付を表示するコントロール。</span><span class="sxs-lookup"><span data-stu-id="48c03-317">In this section, you will program the [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) control to display the currently selected date.</span></span>

### <a name="to-program-the-calendar-control"></a><span data-ttu-id="48c03-318">予定表コントロールのプログラミング</span><span class="sxs-lookup"><span data-stu-id="48c03-318">To program the Calendar control</span></span>


1. <span data-ttu-id="48c03-319">**デザイン**ビューで、ダブルクリック、[カレンダー](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)コントロール。</span><span class="sxs-lookup"><span data-stu-id="48c03-319">In **Design** view, double-click the [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) control.</span></span>

    <span data-ttu-id="48c03-320">新しいイベント ハンドラーが作成され、という名前の分離コード ファイルに表示される*FirstWebPage.aspx.cs*します。</span><span class="sxs-lookup"><span data-stu-id="48c03-320">A new event handler is created and displayed in the code-behind file named *FirstWebPage.aspx.cs*.</span></span>
2. <span data-ttu-id="48c03-321">[完了]、 [SelectionChanged](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selectionchanged.aspx)イベント ハンドラーを次のコード。</span><span class="sxs-lookup"><span data-stu-id="48c03-321">Finish the [SelectionChanged](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selectionchanged.aspx) event handler with the following code.</span></span>

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample3.cs?highlight=3)]


    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample4.vb?highlight=2)]

    <span data-ttu-id="48c03-322">上記のコードは、ラベル コントロールのテキストを予定表コントロールの選択した日付に設定します。</span><span class="sxs-lookup"><span data-stu-id="48c03-322">The above code sets the text of the label control to the selected date of the calendar control.</span></span>


### <a name="running-the-page"></a><span data-ttu-id="48c03-323">ページを実行します。</span><span class="sxs-lookup"><span data-stu-id="48c03-323">Running the Page</span></span>


<span data-ttu-id="48c03-324">予定表をテストできます。</span><span class="sxs-lookup"><span data-stu-id="48c03-324">You can now test the calendar.</span></span>

### <a name="to-run-the-page"></a><span data-ttu-id="48c03-325">ページの実行</span><span class="sxs-lookup"><span data-stu-id="48c03-325">To run the page</span></span>


1. <span data-ttu-id="48c03-326">キーを押して**CTRL + F5**ブラウザーでページを実行します。</span><span class="sxs-lookup"><span data-stu-id="48c03-326">Press **CTRL+F5** to run the page in the browser.</span></span>
2. <span data-ttu-id="48c03-327">カレンダーの日付をクリックします。</span><span class="sxs-lookup"><span data-stu-id="48c03-327">Click a date in the calendar.</span></span>

    <span data-ttu-id="48c03-328">をクリックした日付が表示されます、[ラベル](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)コントロール。</span><span class="sxs-lookup"><span data-stu-id="48c03-328">The date you clicked is displayed in the [Label](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) control.</span></span>
3. <span data-ttu-id="48c03-329">ブラウザーでページのソース コードを表示します。</span><span class="sxs-lookup"><span data-stu-id="48c03-329">In the browser, view the source code for the page.</span></span>

    <span data-ttu-id="48c03-330">なお、[カレンダー](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)コントロールとしてページに表示されて、**テーブル**、として毎日を**td**要素。</span><span class="sxs-lookup"><span data-stu-id="48c03-330">Note that the [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) control has been rendered to the page as a **table**, with each day as a **td** element.</span></span>
4. <span data-ttu-id="48c03-331">ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="48c03-331">Close the browser.</span></span>


## <a name="next-steps"></a><span data-ttu-id="48c03-332">次の手順</span><span class="sxs-lookup"><span data-stu-id="48c03-332">Next Steps</span></span>


<a id="nextStepsToggle"></a>

<span data-ttu-id="48c03-333">このチュートリアルでは、Visual Studio のページ デザイナーの基本機能を説明しました。</span><span class="sxs-lookup"><span data-stu-id="48c03-333">This walkthrough has illustrated the basic features of the Visual Studio page designer.</span></span> <span data-ttu-id="48c03-334">作成し、Visual Studio で Web フォーム ページを編集する方法を理解したところでその他の機能を探索する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="48c03-334">Now that you understand how to create and edit a Web Forms page in Visual Studio, you might want to explore other features.</span></span> <span data-ttu-id="48c03-335">たとえば、以下を実行します。</span><span class="sxs-lookup"><span data-stu-id="48c03-335">For example, you might want to do the following:</span></span>

- <span data-ttu-id="48c03-336">ASP.NET Web フォームの詳細について次のステップ バイ ステップ チュートリアル シリーズして[ASP.NET 4.5 Web フォームと Visual Studio 2013 の概要](getting-started-with-aspnet-45-web-forms/introduction-and-overview.md)します。</span><span class="sxs-lookup"><span data-stu-id="48c03-336">Learn more about ASP.NET Web Forms by following the step-by-step tutorial series [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013](getting-started-with-aspnet-45-web-forms/introduction-and-overview.md).</span></span>
- <span data-ttu-id="48c03-337">カスケード スタイル シート (CSS) について説明します。</span><span class="sxs-lookup"><span data-stu-id="48c03-337">Learn more about Cascading style sheets (CSS).</span></span> <span data-ttu-id="48c03-338">詳細については、次を参照してください。 [CSS の概要での作業](https://msdn.microsoft.com/library/bb398931.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="48c03-338">For details, see [Working with CSS Overview](https://msdn.microsoft.com/library/bb398931.aspx).</span></span>
