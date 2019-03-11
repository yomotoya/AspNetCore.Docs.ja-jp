---
uid: web-forms/overview/getting-started/code-editing-in-web-forms-pages
title: Visual Studio 2013 で ASP.NET Web フォーム コードの編集 |Microsoft Docs
author: Erikre
description: ''
ms.author: riande
ms.date: 03/03/2014
ms.assetid: 5344b74e-b888-479a-92bc-601a33bd61a2
msc.legacyurl: /web-forms/overview/getting-started/code-editing-in-web-forms-pages
msc.type: authoredcontent
ms.openlocfilehash: 670f81ca1ef9923575cb2fee1747f06f426963d8
ms.sourcegitcommit: f43f430a166a7ec137fcad12ded0372747227498
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/17/2018
ms.locfileid: "49391220"
---
<a name="code-editing-aspnet-web-forms-in-visual-studio-2013"></a><span data-ttu-id="c429c-102">Visual Studio 2013 でのコード編集の ASP.NET Web フォームします。</span><span class="sxs-lookup"><span data-stu-id="c429c-102">Code Editing ASP.NET Web Forms in Visual Studio 2013</span></span>
====================
<span data-ttu-id="c429c-103">によって[Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="c429c-103">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="c429c-104">多くの ASP.NET Web フォーム ページでは、Visual Basic、C#、または別の言語でコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="c429c-104">In many ASP.NET Web Form pages, you write code in Visual Basic, C#, or another language.</span></span> <span data-ttu-id="c429c-105">Visual Studio でコード エディターでは、エラーを回避することができ、コードをすばやく記述できます。</span><span class="sxs-lookup"><span data-stu-id="c429c-105">The code editor in Visual Studio can help you write code quickly while helping you avoid errors.</span></span> <span data-ttu-id="c429c-106">さらに、エディターには、行う必要がある作業量を減らすために再利用可能なコードを作成するための方法が用意されています。</span><span class="sxs-lookup"><span data-stu-id="c429c-106">In addition, the editor provides ways for you to create reusable code to help reduce the amount of work you need to do.</span></span>

<span data-ttu-id="c429c-107">このチュートリアルでは、Visual Studio コード エディターのさまざまな機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="c429c-107">This walkthrough illustrates various features of the Visual Studio code editor.</span></span>

<span data-ttu-id="c429c-108">このチュートリアルでは、次の作業を行う方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="c429c-108">During this walkthrough, you will learn how to:</span></span>

- <span data-ttu-id="c429c-109">インライン コード エラーを修正します。</span><span class="sxs-lookup"><span data-stu-id="c429c-109">Correct inline coding errors.</span></span>
- <span data-ttu-id="c429c-110">リファクタリングし、コードの名前を変更します。</span><span class="sxs-lookup"><span data-stu-id="c429c-110">Refactor and rename code.</span></span>
- <span data-ttu-id="c429c-111">変数およびオブジェクトの名前を変更します。</span><span class="sxs-lookup"><span data-stu-id="c429c-111">Rename variables and objects.</span></span>
- <span data-ttu-id="c429c-112">コード スニペットを挿入します。</span><span class="sxs-lookup"><span data-stu-id="c429c-112">Insert code snippets.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c429c-113">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="c429c-113">Prerequisites</span></span>


<span data-ttu-id="c429c-114">このチュートリアルを完了するための要件は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="c429c-114">In order to complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="c429c-115">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs)または[Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)します。</span><span class="sxs-lookup"><span data-stu-id="c429c-115">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) or [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span> <span data-ttu-id="c429c-116">.NET Framework は、自動的にインストールされます。</span><span class="sxs-lookup"><span data-stu-id="c429c-116">The .NET Framework is installed automatically.</span></span> 

    > [!NOTE] 
    > 
    > <span data-ttu-id="c429c-117">Microsoft Visual Studio 2013 および Microsoft Visual Studio Express 2013 for Web は多くの場合、呼びます Visual Studio とこのチュートリアル シリーズです。</span><span class="sxs-lookup"><span data-stu-id="c429c-117">Microsoft Visual Studio 2013 and Microsoft Visual Studio Express 2013 for Web will often be referred to as Visual Studio throughout this tutorial series.</span></span>  
    >   
    > <span data-ttu-id="c429c-118">Visual Studio を使用している場合は、このチュートリアルの前提条件が選択されている、 **Web 開発**設定のコレクションを初めて Visual Studio を起動します。</span><span class="sxs-lookup"><span data-stu-id="c429c-118">If you are using Visual Studio, this walkthrough assumes that you selected the **Web Development** collection of settings the first time that you started Visual Studio.</span></span> <span data-ttu-id="c429c-119">詳細については、次を参照してください。[方法: Web 開発環境の設定の選択](https://msdn.microsoft.com/library/ff521558.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="c429c-119">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>

## <a name="creating-a-web-application-project-and-a-page"></a><span data-ttu-id="c429c-120">Web アプリケーション プロジェクトと、ページの作成</span><span class="sxs-lookup"><span data-stu-id="c429c-120">Creating a Web application project and a Page</span></span>

<a id="sectionToggle0"></a>

<span data-ttu-id="c429c-121">チュートリアルのこの部分では、Web アプリケーション プロジェクトを作成し、新しいページを追加します。</span><span class="sxs-lookup"><span data-stu-id="c429c-121">In this part of the walkthrough, you will create a Web application project and add a new page to it.</span></span>

### <a name="to-create-a-web-application-project"></a><span data-ttu-id="c429c-122">Web アプリケーション プロジェクトを作成するには</span><span class="sxs-lookup"><span data-stu-id="c429c-122">To create a Web application project</span></span>

1. <span data-ttu-id="c429c-123">Microsoft Visual Studio を開きます。</span><span class="sxs-lookup"><span data-stu-id="c429c-123">Open Microsoft Visual Studio.</span></span>
2. <span data-ttu-id="c429c-124">**[ファイル]** メニューの **[新しいプロジェクト]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="c429c-124">On the **File** menu, select **New Project**.</span></span>  
    <span data-ttu-id="c429c-125">![[ファイル] メニュー](code-editing-in-web-forms-pages/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c429c-125">![File Menu](code-editing-in-web-forms-pages/_static/image1.png)</span></span>

    <span data-ttu-id="c429c-126">**[新しいプロジェクト]** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="c429c-126">The **New Project** dialog box appears.</span></span>
3. <span data-ttu-id="c429c-127">選択、**テンプレート** - &gt; **Visual C#**  - &gt; **Web**左側のテンプレート グループ。</span><span class="sxs-lookup"><span data-stu-id="c429c-127">Select the **Templates** -&gt; **Visual C#** -&gt; **Web** templates group on the left.</span></span>
4. <span data-ttu-id="c429c-128">選択、 **ASP.NET Web アプリケーション**中央の列のテンプレート。</span><span class="sxs-lookup"><span data-stu-id="c429c-128">Choose the **ASP.NET Web Application** template in the center column.</span></span>
5. <span data-ttu-id="c429c-129">プロジェクトに名前を***BasicWebApp***  をクリックし、 **OK**ボタン。</span><span class="sxs-lookup"><span data-stu-id="c429c-129">Name your project ***BasicWebApp*** and click the **OK** button.</span></span>   
<span data-ttu-id="c429c-130">![新しいプロジェクト ダイアログ ボックス](code-editing-in-web-forms-pages/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="c429c-130">![New Project dialog box](code-editing-in-web-forms-pages/_static/image2.png)</span></span>
6. <span data-ttu-id="c429c-131">次に、選択、 **Web フォーム**テンプレートをクリックして、 **OK**プロジェクトを作成するボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="c429c-131">Next, select the **Web Forms** template and click the **OK** button to create the project.</span></span>  
<span data-ttu-id="c429c-132">![新しい ASP.NET プロジェクト ダイアログ ボックス](code-editing-in-web-forms-pages/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="c429c-132">![New ASP.NET Project dialog box](code-editing-in-web-forms-pages/_static/image3.png)</span></span>  

    <span data-ttu-id="c429c-133">Visual Studio では、Web フォーム テンプレートに基づく事前構築済みの機能を含む新しいプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="c429c-133">Visual Studio creates a new project that includes prebuilt functionality based on the Web Forms template.</span></span>


## <a name="creating-a-new-aspnet-web-forms-page"></a><span data-ttu-id="c429c-134">新しい ASP.NET Web フォーム ページの作成</span><span class="sxs-lookup"><span data-stu-id="c429c-134">Creating a new ASP.NET Web Forms Page</span></span>


<span data-ttu-id="c429c-135">新しい Web フォーム アプリケーションを作成する場合、 **ASP.NET Web アプリケーション**プロジェクト テンプレートを Visual Studio の ASP.NET ページ (Web フォーム ページ) という名前を追加します*Default.aspx*他のいくつかのファイルだけでなく、フォルダーを選択します。</span><span class="sxs-lookup"><span data-stu-id="c429c-135">When you create a new Web Forms application using the **ASP.NET Web Application** project template, Visual Studio adds an ASP.NET page (Web Forms page) named *Default.aspx*, as well as several other files and folders.</span></span> <span data-ttu-id="c429c-136">使用することができます、 *Default.aspx*ページ、Web アプリケーションのホーム ページとして。</span><span class="sxs-lookup"><span data-stu-id="c429c-136">You can use the *Default.aspx* page as the home page for your Web application.</span></span> <span data-ttu-id="c429c-137">ただし、このチュートリアルでは作成し、新しいページを使用します。</span><span class="sxs-lookup"><span data-stu-id="c429c-137">However, for this walkthrough, you will create and work with a new page.</span></span>

### <a name="to-add-a-page-to-the-web-application"></a><span data-ttu-id="c429c-138">Web アプリケーションにページを追加するには</span><span class="sxs-lookup"><span data-stu-id="c429c-138">To add a page to the Web application</span></span>


1. <span data-ttu-id="c429c-139">**ソリューション エクスプ ローラー**、Web アプリケーションの名前を右クリックし (アプリケーション名は、このチュートリアルでは**BasicWebSite**)、をクリックし、**追加** - &gt;**新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="c429c-139">In **Solution Explorer**, right-click the Web application name (in this tutorial the application name is **BasicWebSite**), and then click **Add** -&gt; **New Item**.</span></span>   
<span data-ttu-id="c429c-140">**[新しい項目の追加]** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="c429c-140">The **Add New Item** dialog box is displayed.</span></span>
2. <span data-ttu-id="c429c-141">選択、 **Visual C#**  - &gt; **Web**左側のテンプレート グループ。</span><span class="sxs-lookup"><span data-stu-id="c429c-141">Select the **Visual C#** -&gt; **Web** templates group on the left.</span></span> <span data-ttu-id="c429c-142">次に、選択**Web フォーム**中央から一覧表示し、名前を*名前*します。</span><span class="sxs-lookup"><span data-stu-id="c429c-142">Then, select **Web Form** from the middle list and name it *FirstWebPage.aspx*.</span></span>   
    <span data-ttu-id="c429c-143">![新しい項目 ダイアログ ボックスを追加します。](code-editing-in-web-forms-pages/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="c429c-143">![Add New Item dialog box](code-editing-in-web-forms-pages/_static/image4.png)</span></span>
3. <span data-ttu-id="c429c-144">クリックして**追加**をプロジェクトに Web フォーム ページを追加します。</span><span class="sxs-lookup"><span data-stu-id="c429c-144">Click **Add** to add the Web Forms page to your project.</span></span>  
 <span data-ttu-id="c429c-145">Visual Studio では、新しいページを作成し、それを開きます。</span><span class="sxs-lookup"><span data-stu-id="c429c-145">Visual Studio creates the new page and opens it.</span></span>
4. <span data-ttu-id="c429c-146">次に、この新しいページを既定のスタートアップ ページとして設定します。</span><span class="sxs-lookup"><span data-stu-id="c429c-146">Next, set this new page as the default startup page.</span></span> <span data-ttu-id="c429c-147">**ソリューション エクスプ ローラー**、という名前の新しいページを右クリックして*名前*選択**スタート ページとして設定**します。</span><span class="sxs-lookup"><span data-stu-id="c429c-147">In **Solution Explorer**, right-click the new page named *FirstWebPage.aspx* and select **Set As Start Page**.</span></span> <span data-ttu-id="c429c-148">次に、進行状況をテストするには、このアプリケーションを実行したとき、ブラウザーでこの新しいページに自動的に表示されます。</span><span class="sxs-lookup"><span data-stu-id="c429c-148">The next time you run this application to test our progress, you will automatically see this new page in the browser.</span></span>


## <a name="correcting-inline-coding-errors"></a><span data-ttu-id="c429c-149">インライン コード エラーを修正します。</span><span class="sxs-lookup"><span data-stu-id="c429c-149">Correcting Inline Coding Errors</span></span>


<span data-ttu-id="c429c-150">Visual Studio でコード エディターを使用して、コードを記述すると、コード エディターに、エラーを修正するのに役立ちますエラーを加えた場合、エラーを回避するのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="c429c-150">The code editor in Visual Studio helps you to avoid errors as you write code, and if you have made an error, the code editor helps you to correct the error.</span></span> <span data-ttu-id="c429c-151">このチュートリアルでは、コード エディターでのエラー修正機能を示す行を記述します。</span><span class="sxs-lookup"><span data-stu-id="c429c-151">In this part of the walkthrough, you will write a line of code that illustrate the error correction features in the editor.</span></span>

### <a name="to-correct-simple-coding-errors-in-visual-studio"></a><span data-ttu-id="c429c-152">Visual Studio での単純なコーディング エラーを修正するには</span><span class="sxs-lookup"><span data-stu-id="c429c-152">To correct simple coding errors in Visual Studio</span></span>


1. <span data-ttu-id="c429c-153">**デザイン**ビューで、空白のページのハンドラーを作成するをダブルクリック、**ロード**ページのイベント。</span><span class="sxs-lookup"><span data-stu-id="c429c-153">In **Design** view, double-click the blank page to create a handler for the **Load** event for the page.</span></span>   
   <span data-ttu-id="c429c-154">コードを記述する場所としてのみ、イベント ハンドラーを使用しています。</span><span class="sxs-lookup"><span data-stu-id="c429c-154">You are using the event handler only as a place to write some code.</span></span>
2. <span data-ttu-id="c429c-155">ハンドラーの内部は、エラー メッセージとキーを押して、次の行を入力**ENTER**:</span><span class="sxs-lookup"><span data-stu-id="c429c-155">Inside the handler, type the following line that contains an error and press **ENTER**:</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample1.cs)]

   <span data-ttu-id="c429c-156">キーを押すと **」と入力**、コード エディターが配置される緑と赤の下線 (呼び出す機会が多い&quot;波線&quot;行) の問題があるコードの領域。</span><span class="sxs-lookup"><span data-stu-id="c429c-156">When you press **ENTER**, the code editor places green and red underlines (commonly call &quot;squiggly&quot; lines) under areas of the code that have issues.</span></span> <span data-ttu-id="c429c-157">緑色の下線では、警告を示します。</span><span class="sxs-lookup"><span data-stu-id="c429c-157">A green underline indicates a warning.</span></span> <span data-ttu-id="c429c-158">赤い下線では、エラーを修正する必要がありますを示します。</span><span class="sxs-lookup"><span data-stu-id="c429c-158">A red underline indicates an error that you must fix.</span></span> 

    <span data-ttu-id="c429c-159">マウス ポインターを置く`myStr`に関する警告を示すツールヒントを確認します。</span><span class="sxs-lookup"><span data-stu-id="c429c-159">Hold the mouse pointer over `myStr` to see a tooltip that tells you about the warning.</span></span> <span data-ttu-id="c429c-160">また、エラー メッセージを表示する赤い下線の上にマウス ポインターを保持します。</span><span class="sxs-lookup"><span data-stu-id="c429c-160">Also, hold your mouse pointer over the red underline to see the error message.</span></span>

    <span data-ttu-id="c429c-161">次の図は、下線を使用してコードを示します。</span><span class="sxs-lookup"><span data-stu-id="c429c-161">The following image shows the code with the underlines.</span></span>

    <span data-ttu-id="c429c-162">![デザイン ビューでの開始テキスト](code-editing-in-web-forms-pages/_static/image5.png "デザイン ビューでの開始テキスト")</span><span class="sxs-lookup"><span data-stu-id="c429c-162">![Welcome text in Design view](code-editing-in-web-forms-pages/_static/image5.png "Welcome text in Design view")</span></span>  
   <span data-ttu-id="c429c-163">セミコロンを追加することで、エラーを修正する必要があります`;`行の末尾にします。</span><span class="sxs-lookup"><span data-stu-id="c429c-163">The error must be fixed by adding a semicolon `;` to the end of the line.</span></span> <span data-ttu-id="c429c-164">警告通知するだけでを使用していない、`myStr`まだ変数。</span><span class="sxs-lookup"><span data-stu-id="c429c-164">The warning simply notifies you that you haven't used the `myStr` variable yet.</span></span>  

    > [!NOTE] 
    > 
    > <span data-ttu-id="c429c-165">Visual Studio での設定を選択して書式設定、現在のコードを表示する**ツール** - &gt; **オプション** - &gt; **フォントと色**します。</span><span class="sxs-lookup"><span data-stu-id="c429c-165">You view your current code formatting settings in Visual Studio by selecting **Tools** -&gt; **Options** -&gt; **Fonts and Colors**.</span></span>


## <a name="refactoring-and-renaming"></a><span data-ttu-id="c429c-166">リファクタリングと名前を変更します。</span><span class="sxs-lookup"><span data-stu-id="c429c-166">Refactoring and Renaming</span></span>

<span data-ttu-id="c429c-167">リファクタリングとは、ソフトウェアの方法を理解し、その機能を維持しながら、維持するために容易にできるように、コードを再構築を含むです。</span><span class="sxs-lookup"><span data-stu-id="c429c-167">Refactoring is a software methodology that involves restructuring your code to make it easier to understand and to maintain, while preserving its functionality.</span></span> <span data-ttu-id="c429c-168">簡単な例は、データベースからデータを取得するためのイベント ハンドラーにコードを記述する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="c429c-168">A simple example might be that you write code in an event handler to get data from a database.</span></span> <span data-ttu-id="c429c-169">ページを開発するとき、さまざまなハンドラーからデータにアクセスする必要があるがわかります。</span><span class="sxs-lookup"><span data-stu-id="c429c-169">As you develop your page, you discover that you need to access the data from several different handlers.</span></span> <span data-ttu-id="c429c-170">そのため、ページのコードをリファクタリングするには、ページでデータ アクセス メソッドを作成し、ハンドラーのメソッドの呼び出しを挿入します。</span><span class="sxs-lookup"><span data-stu-id="c429c-170">Therefore, you refactor the page's code by creating a data-access method in the page and inserting calls to the method in the handlers.</span></span>

<span data-ttu-id="c429c-171">コード エディターには、リファクタリングのさまざまなタスクを実行するためにツールが含まれています。</span><span class="sxs-lookup"><span data-stu-id="c429c-171">The code editor includes tools to help you perform various refactoring tasks.</span></span> <span data-ttu-id="c429c-172">このチュートリアルでは、2 つのリファクタリング手法に精通する機能: 変数の名前を変更して、メソッドを抽出します。</span><span class="sxs-lookup"><span data-stu-id="c429c-172">In this walkthrough, you will work with two refactoring techniques: renaming variables and extracting methods.</span></span> <span data-ttu-id="c429c-173">その他のリファクタリング オプションには、フィールドをカプセル化する、メソッドのパラメーターにローカル変数の昇格およびメソッドのパラメーターの管理が含まれます。</span><span class="sxs-lookup"><span data-stu-id="c429c-173">Other refactoring options include encapsulating fields, promoting local variables to method parameters, and managing method parameters.</span></span> <span data-ttu-id="c429c-174">これらのリファクタリング オプションの可用性は、コード内の場所に依存します。</span><span class="sxs-lookup"><span data-stu-id="c429c-174">The availability of these refactoring options depends on the location in the code.</span></span>

### <a name="refactoring-code"></a><span data-ttu-id="c429c-175">コードのリファクタリング</span><span class="sxs-lookup"><span data-stu-id="c429c-175">Refactoring Code</span></span>

<span data-ttu-id="c429c-176">リファクタリングの一般的なシナリオは、(extract) を作成するメソッドなどの別のメンバー内にあるコードからのメソッド。</span><span class="sxs-lookup"><span data-stu-id="c429c-176">A common refactoring scenario is to create (extract) a method from code that is inside another member, such as a method.</span></span> <span data-ttu-id="c429c-177">これにより、元のメンバーのサイズを縮小し、抽出されたコードを再利用可能な。</span><span class="sxs-lookup"><span data-stu-id="c429c-177">This reduces the size of the original member and makes the extracted code reusable.</span></span>

<span data-ttu-id="c429c-178">チュートリアルのこの部分では、単純なコードを記述し、そこからメソッドを抽出します。</span><span class="sxs-lookup"><span data-stu-id="c429c-178">In this part of the walkthrough, you will write some simple code, and then extract a method from it.</span></span> <span data-ttu-id="c429c-179">リファクタリングはサポートされている C# の場合は、ため、プログラミング言語として C# を使用するページを作成します。</span><span class="sxs-lookup"><span data-stu-id="c429c-179">Refactoring is supported for C#, so you will create a page that uses C# as its programming language.</span></span>

### <a name="to-extract-a-method-in-a-c-page"></a><span data-ttu-id="c429c-180">C# のページのメソッドを抽出するには</span><span class="sxs-lookup"><span data-stu-id="c429c-180">To extract a method in a C# page</span></span>

1. <span data-ttu-id="c429c-181">切り替える**デザイン**ビュー。</span><span class="sxs-lookup"><span data-stu-id="c429c-181">Switch to **Design** view.</span></span>
2. <span data-ttu-id="c429c-182">**ツールボックス**から、**標準** タブで、ドラッグ、[ボタン](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)をページにコントロール。</span><span class="sxs-lookup"><span data-stu-id="c429c-182">In the **Toolbox**, from the **Standard** tab, drag a [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) control onto the page.</span></span>
3. <span data-ttu-id="c429c-183">ダブルクリックして、**ボタン**のハンドラーを作成するコントロールをその[ をクリックして](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx)イベント、し、次の強調表示されたコードを追加。</span><span class="sxs-lookup"><span data-stu-id="c429c-183">Double-click the **Button** control to create a handler for its [Click](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) event, and then add the following highlighted code:</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample2.cs?highlight=3-16)]

   <span data-ttu-id="c429c-184">このコードを作成、 **ArrayList**オブジェクト、ループを使用して値を持つロードおよび別のループを使用して、内容を表示、 **ArrayList**オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="c429c-184">The code creates an **ArrayList** object, uses a loop to load it with values, and then uses another loop to display the contents of the **ArrayList** object.</span></span>
4. <span data-ttu-id="c429c-185">キーを押して**CTRL + F5**ページを実行し、**ボタン**次の出力が表示されるかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="c429c-185">Press **CTRL+F5** to run the page, and then click the **button** to make sure that you see the following output:</span></span>   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample3.html)]
5. <span data-ttu-id="c429c-186">コード エディターに戻り、イベント ハンドラーで、次の行を選択します。</span><span class="sxs-lookup"><span data-stu-id="c429c-186">Return to the code editor, and then select the following lines in the event handler.</span></span>   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample4.html)]
6. <span data-ttu-id="c429c-187">選択範囲を右クリックし、をクリックして**リファクタリング**を選び、**メソッドの抽出**します。</span><span class="sxs-lookup"><span data-stu-id="c429c-187">Right-click the selection, click **Refactor**, and then choose **Extract Method**.</span></span> 

    <span data-ttu-id="c429c-188">**メソッドの抽出** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="c429c-188">The **Extract Method** dialog box appears.</span></span>
7. <span data-ttu-id="c429c-189">**新しいメソッドの名前**ボックスに「 **DisplayArray**、順にクリックします**OK**します。</span><span class="sxs-lookup"><span data-stu-id="c429c-189">In the **New Method Name** box, type **DisplayArray**, and then click **OK**.</span></span> 

    <span data-ttu-id="c429c-190">コード エディターがという名前の新しいメソッドを作成`DisplayArray`で新しいメソッドの呼び出しを配置し、 **をクリックして**ループが元のハンドラー。</span><span class="sxs-lookup"><span data-stu-id="c429c-190">The code editor creates a new method named `DisplayArray`, and puts a call to the new method in the **Click** handler where the loop was originally.</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample5.cs?highlight=12)]
8. <span data-ttu-id="c429c-191">キーを押して**CTRL + F5**ページを再度実行し、をクリックして、**ボタン**します。</span><span class="sxs-lookup"><span data-stu-id="c429c-191">Press **CTRL+F5** to run the page again, and click the **button**.</span></span>

    <span data-ttu-id="c429c-192">前に、と同様、同じページに機能します。</span><span class="sxs-lookup"><span data-stu-id="c429c-192">The page functions the same as it did before.</span></span> <span data-ttu-id="c429c-193">`DisplayArray`メソッドが任意の場所からの呼び出しにできるようになりましたページ クラスにします。</span><span class="sxs-lookup"><span data-stu-id="c429c-193">The `DisplayArray` method can now be call from anywhere in the page class.</span></span>

## <a name="renaming-variables"></a><span data-ttu-id="c429c-194">変数の名前を変更します。</span><span class="sxs-lookup"><span data-stu-id="c429c-194">Renaming Variables</span></span>

<span data-ttu-id="c429c-195">オブジェクトと変数を使用するときに、コードで既に参照されている後に名前を変更する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="c429c-195">When you work with variables, as well as objects, you might want to rename them after they are already referenced in your code.</span></span> <span data-ttu-id="c429c-196">ただし、変数およびオブジェクトの名前を変更すると、参照のいずれかの名前の変更がない場合、コードが発生することができます。</span><span class="sxs-lookup"><span data-stu-id="c429c-196">However, renaming variables and objects can cause the code to break if you miss renaming one of the references.</span></span> <span data-ttu-id="c429c-197">そのため、リファクタリングを使用する名前の変更を実行できます。</span><span class="sxs-lookup"><span data-stu-id="c429c-197">Therefore, you can use refactoring to perform the renaming.</span></span>

### <a name="to-use-refactoring-to-rename-a-variable"></a><span data-ttu-id="c429c-198">リファクタリングを使用して、変数の名前を変更するには</span><span class="sxs-lookup"><span data-stu-id="c429c-198">To use refactoring to rename a variable</span></span>


1. <span data-ttu-id="c429c-199">\*\* をクリックして\*\*イベント ハンドラーでは、次の行を見つけます。</span><span class="sxs-lookup"><span data-stu-id="c429c-199">In the **Click** event handler, locate the following line:</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample6.cs)]
2. <span data-ttu-id="c429c-200">変数名を右クリックして`alist`、選択**リファクタリング**を選び、**の名前を変更**します。</span><span class="sxs-lookup"><span data-stu-id="c429c-200">Right-click the variable name `alist`, choose **Refactor**, and then choose **Rename**.</span></span>

    <span data-ttu-id="c429c-201">**の名前を変更** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="c429c-201">The **Rename** dialog box appears.</span></span>
3. <span data-ttu-id="c429c-202">**新しい名前**ボックスに「 **ArrayList1**を確認して、**参照の変更のプレビュー**チェック ボックスが選択されていますいます。</span><span class="sxs-lookup"><span data-stu-id="c429c-202">In the **New name** box, type **ArrayList1** and make sure the **Preview reference changes** checkbox has been selected.</span></span> <span data-ttu-id="c429c-203">次に、 **[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="c429c-203">Then click **OK**.</span></span>

    <span data-ttu-id="c429c-204">**変更のプレビュー**  ダイアログ ボックスが表示され、すべての参照の名前を変更する変数を含むツリーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="c429c-204">The **Preview Changes** dialog box appears, and displays a tree that contains all references to the variable that you are renaming.</span></span>
4. <span data-ttu-id="c429c-205">クリックして**適用**を閉じる、**変更のプレビュー**  ダイアログ ボックス。</span><span class="sxs-lookup"><span data-stu-id="c429c-205">Click **Apply** to close the **Preview Changes** dialog box.</span></span>

    <span data-ttu-id="c429c-206">選択したインスタンスを指している変数の名前が変更されます。</span><span class="sxs-lookup"><span data-stu-id="c429c-206">The variables that refer specifically to the instance that you selected are renamed.</span></span> <span data-ttu-id="c429c-207">ただし、注意を変数`alist`次の行では、名前は変更されません。</span><span class="sxs-lookup"><span data-stu-id="c429c-207">Note, however, that the variable `alist` in the following line is not renamed.</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample7.cs)]

    <span data-ttu-id="c429c-208">変数`alist`この行の名前は変更されず、変数と同じ値を表していないため、`alist`名前を変更しました。</span><span class="sxs-lookup"><span data-stu-id="c429c-208">The variable `alist` in this line is not renamed because it does not represent the same value as the variable `alist` that you renamed.</span></span> <span data-ttu-id="c429c-209">変数`alist`で、`DisplayArray`宣言は、そのメソッドのローカル変数。</span><span class="sxs-lookup"><span data-stu-id="c429c-209">The variable `alist` in the `DisplayArray` declaration is a local variable for that method.</span></span> <span data-ttu-id="c429c-210">これは、リファクタリングを使用して、変数の名前を変更するが、エディターで単純に検索および置換操作を実行するよりも異なることを示しています操作している変数のセマンティクスの知識がある変数を名前変更をリファクタリングします。</span><span class="sxs-lookup"><span data-stu-id="c429c-210">This illustrates that using refactoring to rename variables is different than simply performing a find-and-replace action in the editor; refactoring renames variables with knowledge of the semantics of the variable that it is working with.</span></span>


## <a name="inserting-snippets"></a><span data-ttu-id="c429c-211">スニペットの挿入</span><span class="sxs-lookup"><span data-stu-id="c429c-211">Inserting Snippets</span></span>

<span data-ttu-id="c429c-212">Web フォームの開発者が頻繁に実行する必要がある多くのコーディング作業があるため、コード エディターは、スニペット、または作成済みのコード ブロックのライブラリを提供します。</span><span class="sxs-lookup"><span data-stu-id="c429c-212">Because there are many coding tasks that Web Forms developers frequently need to perform, the code editor provides a library of snippets, or blocks of prewritten code.</span></span> <span data-ttu-id="c429c-213">ページには、これらのスニペットを挿入できます。</span><span class="sxs-lookup"><span data-stu-id="c429c-213">You can insert these snippets into your page.</span></span>

<span data-ttu-id="c429c-214">Visual Studio で使用する各言語には、コード スニペットを挿入する方法にはわずかな違いがあります。</span><span class="sxs-lookup"><span data-stu-id="c429c-214">Each language that you use in Visual Studio has slight differences in the way you insert code snippets.</span></span> <span data-ttu-id="c429c-215">スニペットの挿入については、次を参照してください。 [Visual Basic の IntelliSense コード スニペット](https://msdn.microsoft.com/library/18yz4be4.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="c429c-215">For information about inserting snippets, see [Visual Basic IntelliSense Code Snippets](https://msdn.microsoft.com/library/18yz4be4.aspx).</span></span> <span data-ttu-id="c429c-216">Visual C# スニペットの挿入については、次を参照してください。 [Visual C# コード スニペット](https://msdn.microsoft.com/library/z41h7fat.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="c429c-216">For information about inserting snippets in Visual C#, see [Visual C# Code Snippets](https://msdn.microsoft.com/library/z41h7fat.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c429c-217">次の手順</span><span class="sxs-lookup"><span data-stu-id="c429c-217">Next Steps</span></span>

<span data-ttu-id="c429c-218">このチュートリアルでは、コードのエラーを修正、コードのリファクタリング、変数の名前を変更およびコードにコード スニペットの挿入の Visual Studio 2010 のコード エディターの基本的な機能を説明しました。</span><span class="sxs-lookup"><span data-stu-id="c429c-218">This walkthrough has illustrated the basic features of the Visual Studio 2010 code editor for correcting errors in your code, refactoring code, renaming variables, and inserting code snippets into your code.</span></span> <span data-ttu-id="c429c-219">エディターで追加の機能には、アプリケーションの開発を高速かつ簡単を利用します。</span><span class="sxs-lookup"><span data-stu-id="c429c-219">Additional features in the editor can make application development fast and easy.</span></span> <span data-ttu-id="c429c-220">たとえば、次の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="c429c-220">For example, you might want to:</span></span>

- <span data-ttu-id="c429c-221">IntelliSense オプションの変更、コード スニペットを管理、コード スニペットをオンラインで検索などの IntelliSense の機能についてよりについて説明します。</span><span class="sxs-lookup"><span data-stu-id="c429c-221">Learn more about the features of IntelliSense, such as modifying IntelliSense options, managing code snippets, and searching for code snippets online.</span></span> <span data-ttu-id="c429c-222">詳細については、「[IntelliSense の使用](https://msdn.microsoft.com/library/hcw1s69b.aspx)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="c429c-222">For more information, see [Using IntelliSense](https://msdn.microsoft.com/library/hcw1s69b.aspx).</span></span>
- <span data-ttu-id="c429c-223">独自のコード スニペットを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="c429c-223">Learn how to create your own code snippets.</span></span> <span data-ttu-id="c429c-224">詳細については、次を参照してください[の作成と IntelliSense コード スニペットの使用。](https://msdn.microsoft.com/library/ms165392.aspx)</span><span class="sxs-lookup"><span data-stu-id="c429c-224">For more information, see [Creating and Using IntelliSense Code Snippets](https://msdn.microsoft.com/library/ms165392.aspx)</span></span>
- <span data-ttu-id="c429c-225">IntelliSense コード スニペットでは、スニペットのカスタマイズやトラブルシューティングなどの Visual Basic 固有の機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="c429c-225">Learn more about the Visual Basic-specific features of IntelliSense code snippets, such as customizing the snippets and troubleshooting.</span></span> <span data-ttu-id="c429c-226">詳細については、次を参照してください[Visual Basic の IntelliSense コード スニペット。](https://msdn.microsoft.com/library/18yz4be4.aspx)</span><span class="sxs-lookup"><span data-stu-id="c429c-226">For more information, see [Visual Basic IntelliSense Code Snippets](https://msdn.microsoft.com/library/18yz4be4.aspx)</span></span>
- <span data-ttu-id="c429c-227">詳細については、C# は、リファクタリングとコード スニペットなど、IntelliSense の特定の機能。</span><span class="sxs-lookup"><span data-stu-id="c429c-227">Learn more about the C#-specific features of IntelliSense, such as refactoring and code snippets.</span></span> <span data-ttu-id="c429c-228">詳細については、次を参照してください。 [Visual C# IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="c429c-228">For more information, see [Visual C# IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx).</span></span>
