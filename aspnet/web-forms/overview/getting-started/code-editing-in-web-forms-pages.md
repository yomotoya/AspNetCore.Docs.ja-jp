---
uid: web-forms/overview/getting-started/code-editing-in-web-forms-pages
title: コードは、Visual Studio 2013 での ASP.NET Web フォームを編集 |Microsoft ドキュメント
author: Erikre
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/03/2014
ms.topic: article
ms.assetid: 5344b74e-b888-479a-92bc-601a33bd61a2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/code-editing-in-web-forms-pages
msc.type: authoredcontent
ms.openlocfilehash: 79b10df04432490d6338dadb8f7ddd36192beb3e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="code-editing-aspnet-web-forms-in-visual-studio-2013"></a><span data-ttu-id="00fd2-102">Visual Studio 2013 でのコードの編集の ASP.NET Web フォームします。</span><span class="sxs-lookup"><span data-stu-id="00fd2-102">Code Editing ASP.NET Web Forms in Visual Studio 2013</span></span>
====================
<span data-ttu-id="00fd2-103">によって[Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="00fd2-103">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="00fd2-104">多くの ASP.NET Web フォーム ページには、Visual Basic、C# の場合、または別の言語でコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="00fd2-104">In many ASP.NET Web Form pages, you write code in Visual Basic, C#, or another language.</span></span> <span data-ttu-id="00fd2-105">Visual Studio でコード エディターには、エラーを回避することができ、コードをすばやく記述するのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="00fd2-105">The code editor in Visual Studio can help you write code quickly while helping you avoid errors.</span></span> <span data-ttu-id="00fd2-106">さらに、エディターは、行う必要がある作業量を減らすために再利用可能なコードを作成するための方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="00fd2-106">In addition, the editor provides ways for you to create reusable code to help reduce the amount of work you need to do.</span></span>

<span data-ttu-id="00fd2-107">このチュートリアルでは、Visual Studio code エディターのさまざまな機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="00fd2-107">This walkthrough illustrates various features of the Visual Studio code editor.</span></span>

<span data-ttu-id="00fd2-108">このチュートリアルでは、次の作業を行う方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="00fd2-108">During this walkthrough, you will learn how to:</span></span>

- <span data-ttu-id="00fd2-109">インラインのコーディング エラーを修正します。</span><span class="sxs-lookup"><span data-stu-id="00fd2-109">Correct inline coding errors.</span></span>
- <span data-ttu-id="00fd2-110">リファクタリングし、コードの名前を変更します。</span><span class="sxs-lookup"><span data-stu-id="00fd2-110">Refactor and rename code.</span></span>
- <span data-ttu-id="00fd2-111">変数およびオブジェクトの名前を変更します。</span><span class="sxs-lookup"><span data-stu-id="00fd2-111">Rename variables and objects.</span></span>
- <span data-ttu-id="00fd2-112">コード スニペットを挿入します。</span><span class="sxs-lookup"><span data-stu-id="00fd2-112">Insert code snippets.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="00fd2-113">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="00fd2-113">Prerequisites</span></span>


<span data-ttu-id="00fd2-114">このチュートリアルを完了するための要件は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="00fd2-114">In order to complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="00fd2-115">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs)または[Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)です。</span><span class="sxs-lookup"><span data-stu-id="00fd2-115">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) or [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span> <span data-ttu-id="00fd2-116">.NET Framework は、自動的にインストールされます。</span><span class="sxs-lookup"><span data-stu-id="00fd2-116">The .NET Framework is installed automatically.</span></span> 

    > [!NOTE] 
    > 
    > <span data-ttu-id="00fd2-117">Microsoft Visual Studio 2013 および Microsoft Visual Studio Express 2013 for Web が多くの場合、参照されます Visual Studio としてこのチュートリアルの系列全体です。</span><span class="sxs-lookup"><span data-stu-id="00fd2-117">Microsoft Visual Studio 2013 and Microsoft Visual Studio Express 2013 for Web will often be referred to as Visual Studio throughout this tutorial series.</span></span>  
    >   
    > <span data-ttu-id="00fd2-118">Visual Studio を使用している場合は、このチュートリアルの前提条件が選択されている、 **Web 開発**設定のコレクションを初めて Visual Studio を起動します。</span><span class="sxs-lookup"><span data-stu-id="00fd2-118">If you are using Visual Studio, this walkthrough assumes that you selected the **Web Development** collection of settings the first time that you started Visual Studio.</span></span> <span data-ttu-id="00fd2-119">詳細については、次を参照してください。[する方法: Web 開発環境設定の選択](https://msdn.microsoft.com/library/ff521558.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="00fd2-119">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>

  <span data-ttu-id="00fd2-120">Visual Studio と ASP.NET の概要については、次を参照してください。 [Visual Studio 2013 での基本的な ASP.NET 4.5 Web フォーム ページの作成](creating-a-basic-web-forms-page.md)です。</span><span class="sxs-lookup"><span data-stu-id="00fd2-120">For an introduction to Visual Studio and ASP.NET, see [Creating a basic ASP.NET 4.5 Web Forms page in Visual Studio 2013](creating-a-basic-web-forms-page.md).</span></span>   
 

## <a name="creating-a-web-application-project-and-a-page"></a><span data-ttu-id="00fd2-121">Web アプリケーション プロジェクトと、ページの作成</span><span class="sxs-lookup"><span data-stu-id="00fd2-121">Creating a Web application project and a Page</span></span>

<a id="sectionToggle0"></a>

<span data-ttu-id="00fd2-122">チュートリアルのこの部分では、Web アプリケーション プロジェクトを作成し、新しいページを追加します。</span><span class="sxs-lookup"><span data-stu-id="00fd2-122">In this part of the walkthrough, you will create a Web application project and add a new page to it.</span></span>

### <a name="to-create-a-web-application-project"></a><span data-ttu-id="00fd2-123">Web アプリケーション プロジェクトを作成するには</span><span class="sxs-lookup"><span data-stu-id="00fd2-123">To create a Web application project</span></span>

1. <span data-ttu-id="00fd2-124">Microsoft Visual Studio を開きます。</span><span class="sxs-lookup"><span data-stu-id="00fd2-124">Open Microsoft Visual Studio.</span></span>
2. <span data-ttu-id="00fd2-125">**[ファイル]** メニューの **[新しいプロジェクト]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="00fd2-125">On the **File** menu, select **New Project**.</span></span>  
    <span data-ttu-id="00fd2-126">![[ファイル] メニュー](code-editing-in-web-forms-pages/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="00fd2-126">![File Menu](code-editing-in-web-forms-pages/_static/image1.png)</span></span>

    <span data-ttu-id="00fd2-127">**[新しいプロジェクト]** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="00fd2-127">The **New Project** dialog box appears.</span></span>
3. <span data-ttu-id="00fd2-128">選択、**テンプレート** - &gt; **Visual c#**  - &gt; **Web**左側のテンプレートのグループです。</span><span class="sxs-lookup"><span data-stu-id="00fd2-128">Select the **Templates** -&gt; **Visual C#** -&gt; **Web** templates group on the left.</span></span>
4. <span data-ttu-id="00fd2-129">選択、 **ASP.NET Web アプリケーション**中央の列内のテンプレートです。</span><span class="sxs-lookup"><span data-stu-id="00fd2-129">Choose the **ASP.NET Web Application** template in the center column.</span></span>
5. <span data-ttu-id="00fd2-130">プロジェクトの名前を付けます***BasicWebApp***  をクリックし、 **ok**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="00fd2-130">Name your project ***BasicWebApp*** and click the **OK** button.</span></span>   
<span data-ttu-id="00fd2-131">![[新しいプロジェクト] ダイアログ ボックス](code-editing-in-web-forms-pages/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="00fd2-131">![New Project dialog box](code-editing-in-web-forms-pages/_static/image2.png)</span></span>
6. <span data-ttu-id="00fd2-132">次に、選択、 **Web フォーム**テンプレートをクリック、 **OK**プロジェクトを作成するボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="00fd2-132">Next, select the **Web Forms** template and click the **OK** button to create the project.</span></span>  
<span data-ttu-id="00fd2-133">![新しい ASP.NET プロジェクト ダイアログ ボックス](code-editing-in-web-forms-pages/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="00fd2-133">![New ASP.NET Project dialog box](code-editing-in-web-forms-pages/_static/image3.png)</span></span>  

    <span data-ttu-id="00fd2-134">Visual Studio では、Web フォーム テンプレートに基づいて作成済みの機能を含む新しいプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="00fd2-134">Visual Studio creates a new project that includes prebuilt functionality based on the Web Forms template.</span></span>


## <a name="creating-a-new-aspnet-web-forms-page"></a><span data-ttu-id="00fd2-135">新しい ASP.NET Web フォーム ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="00fd2-135">Creating a new ASP.NET Web Forms Page</span></span>


<span data-ttu-id="00fd2-136">新しい Web フォーム アプリケーションを使用して、作成する場合、 **ASP.NET Web アプリケーション**プロジェクト テンプレートを Visual Studio が、ASP.NET (Web フォーム ページ) という名前のページを追加する*Default.aspx*とその他のいくつかのファイル、およびフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="00fd2-136">When you create a new Web Forms application using the **ASP.NET Web Application** project template, Visual Studio adds an ASP.NET page (Web Forms page) named *Default.aspx*, as well as several other files and folders.</span></span> <span data-ttu-id="00fd2-137">使用することができます、 *Default.aspx*ページ、Web アプリケーションのホーム ページとして。</span><span class="sxs-lookup"><span data-stu-id="00fd2-137">You can use the *Default.aspx* page as the home page for your Web application.</span></span> <span data-ttu-id="00fd2-138">ただし、このチュートリアルで作成して新しいページを使用します。</span><span class="sxs-lookup"><span data-stu-id="00fd2-138">However, for this walkthrough, you will create and work with a new page.</span></span>

### <a name="to-add-a-page-to-the-web-application"></a><span data-ttu-id="00fd2-139">Web アプリケーション ページを追加するには</span><span class="sxs-lookup"><span data-stu-id="00fd2-139">To add a page to the Web application</span></span>


1. <span data-ttu-id="00fd2-140">**ソリューション エクスプ ローラー**、Web アプリケーションの名前を右クリックし (アプリケーション名は、このチュートリアルでは**BasicWebSite**)、をクリックして**追加** - &gt;**新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="00fd2-140">In **Solution Explorer**, right-click the Web application name (in this tutorial the application name is **BasicWebSite**), and then click **Add** -&gt; **New Item**.</span></span>   
<span data-ttu-id="00fd2-141">**[新しい項目の追加]** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="00fd2-141">The **Add New Item** dialog box is displayed.</span></span>
2. <span data-ttu-id="00fd2-142">選択、 **Visual c#**  - &gt; **Web**左側のテンプレートのグループです。</span><span class="sxs-lookup"><span data-stu-id="00fd2-142">Select the **Visual C#** -&gt; **Web** templates group on the left.</span></span> <span data-ttu-id="00fd2-143">次に、選択**Web フォーム**中央から一覧表示し、名前を付けます*名前*です。</span><span class="sxs-lookup"><span data-stu-id="00fd2-143">Then, select **Web Form** from the middle list and name it *FirstWebPage.aspx*.</span></span>   
    <span data-ttu-id="00fd2-144">![新しい項目の追加 ダイアログ ボックスを追加します。](code-editing-in-web-forms-pages/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="00fd2-144">![Add New Item dialog box](code-editing-in-web-forms-pages/_static/image4.png)</span></span>
3. <span data-ttu-id="00fd2-145">をクリックして**追加**をプロジェクトに Web フォーム ページを追加します。</span><span class="sxs-lookup"><span data-stu-id="00fd2-145">Click **Add** to add the Web Forms page to your project.</span></span>  
 <span data-ttu-id="00fd2-146">Visual Studio では、新しいページを作成し、それを開きます。</span><span class="sxs-lookup"><span data-stu-id="00fd2-146">Visual Studio creates the new page and opens it.</span></span>
4. <span data-ttu-id="00fd2-147">次に、この新しいページを既定のスタートアップ ページとして設定します。</span><span class="sxs-lookup"><span data-stu-id="00fd2-147">Next, set this new page as the default startup page.</span></span> <span data-ttu-id="00fd2-148">**ソリューション エクスプ ローラー**、という名前の新しいページを右クリックして*名前*選択**スタート ページとして設定**です。</span><span class="sxs-lookup"><span data-stu-id="00fd2-148">In **Solution Explorer**, right-click the new page named *FirstWebPage.aspx* and select **Set As Start Page**.</span></span> <span data-ttu-id="00fd2-149">次に、作業の進行状況をテストするには、このアプリケーションを実行したとき、ブラウザーでこの新しいページが自動的に表示されます。</span><span class="sxs-lookup"><span data-stu-id="00fd2-149">The next time you run this application to test our progress, you will automatically see this new page in the browser.</span></span>


## <a name="correcting-inline-coding-errors"></a><span data-ttu-id="00fd2-150">インラインのコーディング エラーを修正します。</span><span class="sxs-lookup"><span data-stu-id="00fd2-150">Correcting Inline Coding Errors</span></span>


<span data-ttu-id="00fd2-151">Visual Studio でコード エディターでは、コードを記述すると、コード エディターがエラーを修正することができます、エラーを行った場合、エラーを回避するのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="00fd2-151">The code editor in Visual Studio helps you to avoid errors as you write code, and if you have made an error, the code editor helps you to correct the error.</span></span> <span data-ttu-id="00fd2-152">このチュートリアルでは、コード エディターでエラー補正機能を示す行を記述します。</span><span class="sxs-lookup"><span data-stu-id="00fd2-152">In this part of the walkthrough, you will write a line of code that illustrate the error correction features in the editor.</span></span>

### <a name="to-correct-simple-coding-errors-in-visual-studio"></a><span data-ttu-id="00fd2-153">Visual Studio での単純なコーディング エラーを修正するには</span><span class="sxs-lookup"><span data-stu-id="00fd2-153">To correct simple coding errors in Visual Studio</span></span>


1. <span data-ttu-id="00fd2-154">**デザイン**ビューで、空白のページのハンドラーを作成する をダブルクリック、**ロード**ページのイベントです。</span><span class="sxs-lookup"><span data-stu-id="00fd2-154">In **Design** view, double-click the blank page to create a handler for the **Load** event for the page.</span></span>   
   <span data-ttu-id="00fd2-155">一部のコードを記述するのに場所としてのみ、イベント ハンドラーを使用しています。</span><span class="sxs-lookup"><span data-stu-id="00fd2-155">You are using the event handler only as a place to write some code.</span></span>
2. <span data-ttu-id="00fd2-156">ハンドラーの内部エラーおよびキーを押してを含む次の行を入力**ENTER**:</span><span class="sxs-lookup"><span data-stu-id="00fd2-156">Inside the handler, type the following line that contains an error and press **ENTER**:</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample1.cs)]

   <span data-ttu-id="00fd2-157">押すと**ENTER**、コード エディターが緑色と赤色の下線を配置 (よくを呼び出す&quot;波&quot;行) に問題があるコードの領域の下。</span><span class="sxs-lookup"><span data-stu-id="00fd2-157">When you press **ENTER**, the code editor places green and red underlines (commonly call &quot;squiggly&quot; lines) under areas of the code that have issues.</span></span> <span data-ttu-id="00fd2-158">緑色の下線は、警告を示します。</span><span class="sxs-lookup"><span data-stu-id="00fd2-158">A green underline indicates a warning.</span></span> <span data-ttu-id="00fd2-159">赤い下線では、解決する必要がありますのあるエラーを示します。</span><span class="sxs-lookup"><span data-stu-id="00fd2-159">A red underline indicates an error that you must fix.</span></span> 

    <span data-ttu-id="00fd2-160">上にマウス ポインターを置く`myStr`警告に関する説明が表示されるツールヒントを確認します。</span><span class="sxs-lookup"><span data-stu-id="00fd2-160">Hold the mouse pointer over `myStr` to see a tooltip that tells you about the warning.</span></span> <span data-ttu-id="00fd2-161">また、エラー メッセージを表示する赤い下線の上にマウス ポインターを保持します。</span><span class="sxs-lookup"><span data-stu-id="00fd2-161">Also, hold your mouse pointer over the red underline to see the error message.</span></span>

    <span data-ttu-id="00fd2-162">次の図は、下線でコードを示します。</span><span class="sxs-lookup"><span data-stu-id="00fd2-162">The following image shows the code with the underlines.</span></span>

    <span data-ttu-id="00fd2-163">![デザイン ビューでの開始テキスト](code-editing-in-web-forms-pages/_static/image5.png "デザイン ビューでの開始テキスト")</span><span class="sxs-lookup"><span data-stu-id="00fd2-163">![Welcome text in Design view](code-editing-in-web-forms-pages/_static/image5.png "Welcome text in Design view")</span></span>  
   <span data-ttu-id="00fd2-164">セミコロンを追加することで、エラーを修正する必要があります`;`行の末尾にします。</span><span class="sxs-lookup"><span data-stu-id="00fd2-164">The error must be fixed by adding a semicolon `;` to the end of the line.</span></span> <span data-ttu-id="00fd2-165">警告通知するだけで、使用していないこと、`myStr`まだ変数。</span><span class="sxs-lookup"><span data-stu-id="00fd2-165">The warning simply notifies you that you haven't used the `myStr` variable yet.</span></span>  

    > [!NOTE] 
    > 
    > <span data-ttu-id="00fd2-166">Visual Studio での設定を書式設定を選択して、現在のコードを表示する**ツール** - &gt; **オプション** - &gt; **フォントと色**です。</span><span class="sxs-lookup"><span data-stu-id="00fd2-166">You view your current code formatting settings in Visual Studio by selecting **Tools** -&gt; **Options** -&gt; **Fonts and Colors**.</span></span>


## <a name="refactoring-and-renaming"></a><span data-ttu-id="00fd2-167">リファクタリングと 名前を変更します。</span><span class="sxs-lookup"><span data-stu-id="00fd2-167">Refactoring and Renaming</span></span>

<span data-ttu-id="00fd2-168">リファクタリングとは、ソフトウェアの手法を理解し、その機能を維持しながら、管理を容易にできるように、コードの再構成を含むです。</span><span class="sxs-lookup"><span data-stu-id="00fd2-168">Refactoring is a software methodology that involves restructuring your code to make it easier to understand and to maintain, while preserving its functionality.</span></span> <span data-ttu-id="00fd2-169">データベースからデータを取得するイベント ハンドラーでコードを記述するには、単純な例があります。</span><span class="sxs-lookup"><span data-stu-id="00fd2-169">A simple example might be that you write code in an event handler to get data from a database.</span></span> <span data-ttu-id="00fd2-170">ページを開発するいくつかの異なるハンドラーからのデータにアクセスする必要があることを検出します。</span><span class="sxs-lookup"><span data-stu-id="00fd2-170">As you develop your page, you discover that you need to access the data from several different handlers.</span></span> <span data-ttu-id="00fd2-171">そのため、ページのコードをリファクタリングして、ページで、データ アクセス方法を作成し、ハンドラーで、メソッドの呼び出しを挿入します。</span><span class="sxs-lookup"><span data-stu-id="00fd2-171">Therefore, you refactor the page's code by creating a data-access method in the page and inserting calls to the method in the handlers.</span></span>

<span data-ttu-id="00fd2-172">コード エディターには、リファクタリングのさまざまなタスクを実行するのに役立つツールが含まれています。</span><span class="sxs-lookup"><span data-stu-id="00fd2-172">The code editor includes tools to help you perform various refactoring tasks.</span></span> <span data-ttu-id="00fd2-173">このチュートリアルでは、2 つのリファクタリング方法を操作します。 変数の名前を変更すると、メソッドを抽出します。</span><span class="sxs-lookup"><span data-stu-id="00fd2-173">In this walkthrough, you will work with two refactoring techniques: renaming variables and extracting methods.</span></span> <span data-ttu-id="00fd2-174">その他のリファクタリング オプションには、フィールドをカプセル化する、メソッド パラメーターにローカル変数の昇格とメソッドのパラメーターの管理が含まれます。</span><span class="sxs-lookup"><span data-stu-id="00fd2-174">Other refactoring options include encapsulating fields, promoting local variables to method parameters, and managing method parameters.</span></span> <span data-ttu-id="00fd2-175">これらのリファクタリング オプションの可用性は、コード内の場所によって異なります。</span><span class="sxs-lookup"><span data-stu-id="00fd2-175">The availability of these refactoring options depends on the location in the code.</span></span>

### <a name="refactoring-code"></a><span data-ttu-id="00fd2-176">コードのリファクタリング</span><span class="sxs-lookup"><span data-stu-id="00fd2-176">Refactoring Code</span></span>

<span data-ttu-id="00fd2-177">リファクタリング一般的 (抽出) を作成するのには、メソッドなどの別のメンバー内にあるコードからメソッドです。</span><span class="sxs-lookup"><span data-stu-id="00fd2-177">A common refactoring scenario is to create (extract) a method from code that is inside another member, such as a method.</span></span> <span data-ttu-id="00fd2-178">これにより、元のメンバーのサイズを縮小し、抽出したコードを再利用可能な。</span><span class="sxs-lookup"><span data-stu-id="00fd2-178">This reduces the size of the original member and makes the extracted code reusable.</span></span>

<span data-ttu-id="00fd2-179">チュートリアルのこの部分では、単純なコードを記述し、そこからメソッドを抽出します。</span><span class="sxs-lookup"><span data-stu-id="00fd2-179">In this part of the walkthrough, you will write some simple code, and then extract a method from it.</span></span> <span data-ttu-id="00fd2-180">リファクタリングはサポートされて c# の場合は、プログラミング言語として c# を使用するページを作成するようにします。</span><span class="sxs-lookup"><span data-stu-id="00fd2-180">Refactoring is supported for C#, so you will create a page that uses C# as its programming language.</span></span>

### <a name="to-extract-a-method-in-a-c-page"></a><span data-ttu-id="00fd2-181">C# のページ内のメソッドを抽出するには</span><span class="sxs-lookup"><span data-stu-id="00fd2-181">To extract a method in a C# page</span></span>

1. <span data-ttu-id="00fd2-182">切り替える**デザイン**ビュー。</span><span class="sxs-lookup"><span data-stu-id="00fd2-182">Switch to **Design** view.</span></span>
2. <span data-ttu-id="00fd2-183">**ツールボックス**から、**標準** タブで、ドラッグ、[ボタン](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)コントロールをページにします。</span><span class="sxs-lookup"><span data-stu-id="00fd2-183">In the **Toolbox**, from the **Standard** tab, drag a [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) control onto the page.</span></span>
3. <span data-ttu-id="00fd2-184">ダブルクリックして、**ボタン**のハンドラーを作成するコントロールをその[ をクリックして](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx)イベント、し、次の強調表示されたコードを追加。</span><span class="sxs-lookup"><span data-stu-id="00fd2-184">Double-click the **Button** control to create a handler for its [Click](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) event, and then add the following highlighted code:</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample2.cs?highlight=3-16)]

   <span data-ttu-id="00fd2-185">このコードを作成、 **ArrayList**オブジェクト、ループを使用して値を持つロードおよび別のループを使用して、内容を表示、 **ArrayList**オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="00fd2-185">The code creates an **ArrayList** object, uses a loop to load it with values, and then uses another loop to display the contents of the **ArrayList** object.</span></span>
4. <span data-ttu-id="00fd2-186">キーを押して**ctrl キーを押しながら f5 キーを押して** ページを実行し、をクリックする、**ボタン**次の出力が表示されるかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="00fd2-186">Press **CTRL+F5** to run the page, and then click the **button** to make sure that you see the following output:</span></span>   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample3.html)]
5. <span data-ttu-id="00fd2-187">コード エディターに戻るし、イベント ハンドラーで、次の行を選択します。</span><span class="sxs-lookup"><span data-stu-id="00fd2-187">Return to the code editor, and then select the following lines in the event handler.</span></span>   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample4.html)]
6. <span data-ttu-id="00fd2-188">選択範囲を右クリックし、をクリックして**リファクター**を選択し**メソッドの抽出**です。</span><span class="sxs-lookup"><span data-stu-id="00fd2-188">Right-click the selection, click **Refactor**, and then choose **Extract Method**.</span></span> 

    <span data-ttu-id="00fd2-189">**メソッドの抽出** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="00fd2-189">The **Extract Method** dialog box appears.</span></span>
7. <span data-ttu-id="00fd2-190">**メソッド名を新しい**ボックスに、入力**DisplayArray**、クリックして**[ok]**です。</span><span class="sxs-lookup"><span data-stu-id="00fd2-190">In the **New Method Name** box, type **DisplayArray**, and then click **OK**.</span></span> 

    <span data-ttu-id="00fd2-191">コード エディターがという名前の新しいメソッドを作成`DisplayArray`で新しいメソッドの呼び出しを配置し、 **をクリックして**ループが元のハンドラー。</span><span class="sxs-lookup"><span data-stu-id="00fd2-191">The code editor creates a new method named `DisplayArray`, and puts a call to the new method in the **Click** handler where the loop was originally.</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample5.cs?highlight=12)]
8. <span data-ttu-id="00fd2-192">キーを押して**ctrl キーを押しながら f5 キーを押して**ページを再度実行し、をクリックする、**ボタン**です。</span><span class="sxs-lookup"><span data-stu-id="00fd2-192">Press **CTRL+F5** to run the page again, and click the **button**.</span></span>

    <span data-ttu-id="00fd2-193">ページは以前と同じように、同様に機能します。</span><span class="sxs-lookup"><span data-stu-id="00fd2-193">The page functions the same as it did before.</span></span> <span data-ttu-id="00fd2-194">`DisplayArray`メソッドが任意の場所からの呼び出しにできるようになりましたページ クラスでします。</span><span class="sxs-lookup"><span data-stu-id="00fd2-194">The `DisplayArray` method can now be call from anywhere in the page class.</span></span>

## <a name="renaming-variables"></a><span data-ttu-id="00fd2-195">変数の名前を変更します。</span><span class="sxs-lookup"><span data-stu-id="00fd2-195">Renaming Variables</span></span>

<span data-ttu-id="00fd2-196">オブジェクトと同様に、変数を使用する場合は、コードで既に参照されている後に名前を変更することができます。</span><span class="sxs-lookup"><span data-stu-id="00fd2-196">When you work with variables, as well as objects, you might want to rename them after they are already referenced in your code.</span></span> <span data-ttu-id="00fd2-197">ただし、変数およびオブジェクトの名前を変更すると、参照のいずれかの名前の変更がない場合、コードが発生することができます。</span><span class="sxs-lookup"><span data-stu-id="00fd2-197">However, renaming variables and objects can cause the code to break if you miss renaming one of the references.</span></span> <span data-ttu-id="00fd2-198">したがって、リファクタリングを使用名前の変更を実行することができます。</span><span class="sxs-lookup"><span data-stu-id="00fd2-198">Therefore, you can use refactoring to perform the renaming.</span></span>

### <a name="to-use-refactoring-to-rename-a-variable"></a><span data-ttu-id="00fd2-199">使用してリファクタリングする変数名の変更</span><span class="sxs-lookup"><span data-stu-id="00fd2-199">To use refactoring to rename a variable</span></span>


1. <span data-ttu-id="00fd2-200">** をクリックして**イベント ハンドラー、次の行を探します。</span><span class="sxs-lookup"><span data-stu-id="00fd2-200">In the **Click** event handler, locate the following line:</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample6.cs)]
2. <span data-ttu-id="00fd2-201">変数名を右クリックして`alist`、選択**リファクター**を選択し**の名前を変更**です。</span><span class="sxs-lookup"><span data-stu-id="00fd2-201">Right-click the variable name `alist`, choose **Refactor**, and then choose **Rename**.</span></span>

    <span data-ttu-id="00fd2-202">**の名前を変更** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="00fd2-202">The **Rename** dialog box appears.</span></span>
3. <span data-ttu-id="00fd2-203">**新しい名前**ボックスに、入力**ArrayList1**ことを確認し、**参照の変更のプレビュー**  チェック ボックスが選択されています。</span><span class="sxs-lookup"><span data-stu-id="00fd2-203">In the **New name** box, type **ArrayList1** and make sure the **Preview reference changes** checkbox has been selected.</span></span> <span data-ttu-id="00fd2-204">次に、 **[OK]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="00fd2-204">Then click **OK**.</span></span>

    <span data-ttu-id="00fd2-205">**変更のプレビュー**  ダイアログ ボックスが表示され、名前を変更する変数へのすべての参照を含むツリーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="00fd2-205">The **Preview Changes** dialog box appears, and displays a tree that contains all references to the variable that you are renaming.</span></span>
4. <span data-ttu-id="00fd2-206">をクリックして**適用**を閉じる、**変更のプレビュー**  ダイアログ ボックス。</span><span class="sxs-lookup"><span data-stu-id="00fd2-206">Click **Apply** to close the **Preview Changes** dialog box.</span></span>

    <span data-ttu-id="00fd2-207">選択したインスタンスを明示的に参照する変数の名前が変更されます。</span><span class="sxs-lookup"><span data-stu-id="00fd2-207">The variables that refer specifically to the instance that you selected are renamed.</span></span> <span data-ttu-id="00fd2-208">ただしを変数`alist`次の行では、名前は変更されません。</span><span class="sxs-lookup"><span data-stu-id="00fd2-208">Note, however, that the variable `alist` in the following line is not renamed.</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample7.cs)]

    <span data-ttu-id="00fd2-209">変数`alist`この行の名前は変更されず、変数と同じ値を表していないため`alist`名前を変更しました。</span><span class="sxs-lookup"><span data-stu-id="00fd2-209">The variable `alist` in this line is not renamed because it does not represent the same value as the variable `alist` that you renamed.</span></span> <span data-ttu-id="00fd2-210">変数`alist`で、`DisplayArray`宣言は、そのメソッドのローカル変数。</span><span class="sxs-lookup"><span data-stu-id="00fd2-210">The variable `alist` in the `DisplayArray` declaration is a local variable for that method.</span></span> <span data-ttu-id="00fd2-211">これは、リファクタリングを使用して、変数の名前を変更するが、エディターで検索と置換操作を実行するとは異なることは示しています。に対して操作して、変数のセマンティクスの知識を持つ変数を名前変更をリファクタリングします。</span><span class="sxs-lookup"><span data-stu-id="00fd2-211">This illustrates that using refactoring to rename variables is different than simply performing a find-and-replace action in the editor; refactoring renames variables with knowledge of the semantics of the variable that it is working with.</span></span>


## <a name="inserting-snippets"></a><span data-ttu-id="00fd2-212">スニペットの挿入</span><span class="sxs-lookup"><span data-stu-id="00fd2-212">Inserting Snippets</span></span>

<span data-ttu-id="00fd2-213">Web フォーム開発者は頻繁に実行する必要がある多くのコーディング作業があるため、コード エディターは、スニペットの場合、または作成済みのコード ブロックのライブラリを提供します。</span><span class="sxs-lookup"><span data-stu-id="00fd2-213">Because there are many coding tasks that Web Forms developers frequently need to perform, the code editor provides a library of snippets, or blocks of prewritten code.</span></span> <span data-ttu-id="00fd2-214">ページには、これらのスニペットを挿入できます。</span><span class="sxs-lookup"><span data-stu-id="00fd2-214">You can insert these snippets into your page.</span></span>

<span data-ttu-id="00fd2-215">Visual Studio で使用する各言語には、コード スニペットを挿入する方法にわずかな違いがあります。</span><span class="sxs-lookup"><span data-stu-id="00fd2-215">Each language that you use in Visual Studio has slight differences in the way you insert code snippets.</span></span> <span data-ttu-id="00fd2-216">スニペットを挿入する方法については、次を参照してください。 [Visual Basic の IntelliSense コード スニペット](https://msdn.microsoft.com/library/18yz4be4.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="00fd2-216">For information about inserting snippets, see [Visual Basic IntelliSense Code Snippets](https://msdn.microsoft.com/library/18yz4be4.aspx).</span></span> <span data-ttu-id="00fd2-217">スニペットを挿入するには Visual C# の場合については、次を参照してください。 [Visual c# のコード スニペット](https://msdn.microsoft.com/library/z41h7fat.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="00fd2-217">For information about inserting snippets in Visual C#, see [Visual C# Code Snippets](https://msdn.microsoft.com/library/z41h7fat.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="00fd2-218">次の手順</span><span class="sxs-lookup"><span data-stu-id="00fd2-218">Next Steps</span></span>

<span data-ttu-id="00fd2-219">このチュートリアルでは、コード内のエラーを修正する、コードのリファクタリング、変数の名前を変更する、コードにコード スニペットを挿入するの Visual Studio 2010 のコード エディターの基本的な機能を説明しました。</span><span class="sxs-lookup"><span data-stu-id="00fd2-219">This walkthrough has illustrated the basic features of the Visual Studio 2010 code editor for correcting errors in your code, refactoring code, renaming variables, and inserting code snippets into your code.</span></span> <span data-ttu-id="00fd2-220">エディターで追加された機能は、アプリケーションの開発を高速で簡単になります。</span><span class="sxs-lookup"><span data-stu-id="00fd2-220">Additional features in the editor can make application development fast and easy.</span></span> <span data-ttu-id="00fd2-221">たとえば、次の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="00fd2-221">For example, you might want to:</span></span>

- <span data-ttu-id="00fd2-222">IntelliSense オプションの変更、コード スニペットの管理、およびコード スニペットをオンラインで検索などの IntelliSense の機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="00fd2-222">Learn more about the features of IntelliSense, such as modifying IntelliSense options, managing code snippets, and searching for code snippets online.</span></span> <span data-ttu-id="00fd2-223">詳細については、「[IntelliSense の使用](https://msdn.microsoft.com/library/hcw1s69b.aspx)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="00fd2-223">For more information, see [Using IntelliSense](https://msdn.microsoft.com/library/hcw1s69b.aspx).</span></span>
- <span data-ttu-id="00fd2-224">独自のコード スニペットを作成する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="00fd2-224">Learn how to create your own code snippets.</span></span> <span data-ttu-id="00fd2-225">詳細については、次を参照してください[の作成と使用の IntelliSense コード スニペット。](https://msdn.microsoft.com/library/ms165392.aspx)</span><span class="sxs-lookup"><span data-stu-id="00fd2-225">For more information, see [Creating and Using IntelliSense Code Snippets](https://msdn.microsoft.com/library/ms165392.aspx)</span></span>
- <span data-ttu-id="00fd2-226">IntelliSense コード スニペットでは、スニペットをカスタマイズして、トラブルシューティングなどの Visual Basic 固有の機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="00fd2-226">Learn more about the Visual Basic-specific features of IntelliSense code snippets, such as customizing the snippets and troubleshooting.</span></span> <span data-ttu-id="00fd2-227">詳細については、次を参照してください[Visual Basic の IntelliSense コード スニペット。](https://msdn.microsoft.com/library/18yz4be4.aspx)</span><span class="sxs-lookup"><span data-stu-id="00fd2-227">For more information, see [Visual Basic IntelliSense Code Snippets](https://msdn.microsoft.com/library/18yz4be4.aspx)</span></span>
- <span data-ttu-id="00fd2-228">詳細については、c# のリファクタリングとコード スニペットなど、IntelliSense の特定の機能です。</span><span class="sxs-lookup"><span data-stu-id="00fd2-228">Learn more about the C#-specific features of IntelliSense, such as refactoring and code snippets.</span></span> <span data-ttu-id="00fd2-229">詳細については、次を参照してください。 [Visual c# IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="00fd2-229">For more information, see [Visual C# IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx).</span></span>
