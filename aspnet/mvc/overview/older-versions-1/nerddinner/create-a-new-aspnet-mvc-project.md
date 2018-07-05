---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: 新しい ASP.NET MVC プロジェクトの作成 |Microsoft Docs
author: microsoft
description: 手順 1 では、NerdDinner アプリケーションの基本的な構造を適切に配置する方法を示します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: 9f5e0b3f82d113fc72ab4002ec8d06ad8444dceb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374277"
---
<a name="create-a-new-aspnet-mvc-project"></a><span data-ttu-id="823ec-103">新しい ASP.NET MVC プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="823ec-103">Create a New ASP.NET MVC Project</span></span>
====================
<span data-ttu-id="823ec-104">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="823ec-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="823ec-105">PDF のダウンロード</span><span class="sxs-lookup"><span data-stu-id="823ec-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="823ec-106">これは、無料の手順 1 ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク スルーの小さなをビルドしても、ASP.NET MVC 1 を使用して web アプリケーションを実行する方法。</span><span class="sxs-lookup"><span data-stu-id="823ec-106">This is step 1 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="823ec-107">手順 1 では、NerdDinner アプリケーションの基本的な構造を適切に配置する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="823ec-107">Step 1 shows you how to put the basic NerdDinner application structure in place.</span></span>
> 
> <span data-ttu-id="823ec-108">次のことをお勧め ASP.NET MVC 3 を使用している場合、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアル。</span><span class="sxs-lookup"><span data-stu-id="823ec-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-1-file-gtnew-project"></a><span data-ttu-id="823ec-109">NerdDinner 手順 1: ファイル-&gt;新しいプロジェクト</span><span class="sxs-lookup"><span data-stu-id="823ec-109">NerdDinner Step 1: File-&gt;New Project</span></span>

<span data-ttu-id="823ec-110">まず、NerdDinner アプリケーションを選択すると、**ファイル -&gt;新しいプロジェクト**内で Visual Studio 2008 または、無料の Visual Web Developer 2008 Express のいずれかのメニュー項目。</span><span class="sxs-lookup"><span data-stu-id="823ec-110">We'll begin our NerdDinner application by selecting the **File-&gt;New Project** menu item within either Visual Studio 2008 or the free Visual Web Developer 2008 Express.</span></span>

<span data-ttu-id="823ec-111">[新しいプロジェクト] ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="823ec-111">This will bring up the "New Project" dialog.</span></span> <span data-ttu-id="823ec-112">新しい ASP.NET MVC アプリケーションを作成するには、ダイアログの左側にある"Web"ノードを選択し、右側の「ASP.NET MVC Web アプリケーション」プロジェクト テンプレートを選択しされます。</span><span class="sxs-lookup"><span data-stu-id="823ec-112">To create a new ASP.NET MVC application, we'll select the "Web" node on the left-hand side of the dialog and then choose the "ASP.NET MVC Web Application" project template on the right:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image1.png)

<span data-ttu-id="823ec-113">*重要: ダウンロードして、ASP.NET MVC で、新しいプロジェクト ダイアログに表示されませんが、それ以外の場合にインストールされていることを確認してください。V2 を使用することができます、 [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)まだインストールしていない場合 (ASP.NET MVC は内で使用できる、"Web プラットフォーム-&gt;フレームワークおよびランタイム"セクション)。*</span><span class="sxs-lookup"><span data-stu-id="823ec-113">*Important: Make sure you've downloaded and installed ASP.NET MVC - otherwise it won't show up in the New Project dialog. You can use V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) if you haven't installed it yet (ASP.NET MVC is available within the "Web Platform-&gt;Frameworks and Runtimes" section).*</span></span>

<span data-ttu-id="823ec-114">"NerdDinner"を作成し、クリックして [ok] ボタンを作成する新しいプロジェクトという名前です。</span><span class="sxs-lookup"><span data-stu-id="823ec-114">We'll name the new project we are going to create "NerdDinner" and then click the "ok" button to create it.</span></span>

<span data-ttu-id="823ec-115">[Ok] をクリックしたとき、Visual Studio は、必要に応じて、新しいアプリケーションの単体テスト プロジェクトを作成することを求める追加のダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="823ec-115">When we click "ok" Visual Studio will bring up an additional dialog that prompts us to optionally create a unit test project for the new application as well.</span></span> <span data-ttu-id="823ec-116">機能と、アプリケーションの動作を検証する自動テストを作成することにより、この単体テスト プロジェクト (何かについて説明する方法このチュートリアルの後半で to do)。</span><span class="sxs-lookup"><span data-stu-id="823ec-116">This unit test project enables us to create automated tests that verify the functionality and behavior of our application (something we'll cover how to-do later in this tutorial).</span></span>

![](create-a-new-aspnet-mvc-project/_static/image2.png)

<span data-ttu-id="823ec-117">上記のダイアログ ボックスで、「テスト フレームワーク」ドロップダウン リストには、すべて使用可能な ASP.NET MVC 単体テスト プロジェクト テンプレートがコンピューターにインストールが設定されます。</span><span class="sxs-lookup"><span data-stu-id="823ec-117">The "Test framework" dropdown in the above dialog is populated with all available ASP.NET MVC unit test project templates installed on the machine.</span></span> <span data-ttu-id="823ec-118">NUnit や MBUnit、XUnit のバージョンをダウンロードできます。</span><span class="sxs-lookup"><span data-stu-id="823ec-118">Versions can be downloaded for NUnit, MBUnit, and XUnit.</span></span> <span data-ttu-id="823ec-119">組み込みの Visual Studio 単体テスト フレームワークもサポートされます。</span><span class="sxs-lookup"><span data-stu-id="823ec-119">The built-in Visual Studio Unit Test framework is also supported.</span></span>

<span data-ttu-id="823ec-120">*注: Visual Studio の単体テスト フレームワークは、Visual Studio 2008 Professional および以降のバージョンで使用できるのみです。VS 2008 Standard Edition または Visual Web Developer 2008 Express を使用している場合は、ダウンロードしてこのダイアログ ボックスに表示するために、ASP.NET MVC の NUnit や MBUnit XUnit 拡張機能をインストールする必要があります。インストールされているすべてのテスト フレームワークがない場合は、ダイアログは表示されません。*</span><span class="sxs-lookup"><span data-stu-id="823ec-120">*Note: The Visual Studio Unit Test Framework is only available with Visual Studio 2008 Professional and higher versions. If you are using VS 2008 Standard Edition or Visual Web Developer 2008 Express you will need to download and install the NUnit, MBUnit or XUnit extensions for ASP.NET MVC in order for this dialog to be shown. The dialog will not display if there aren't any test frameworks installed.*</span></span>

<span data-ttu-id="823ec-121">作成するテスト プロジェクトの既定の"NerdDinner.Tests"名前を使用して、「Visual Studio 単体テスト」フレームワーク オプションを使用しておします。</span><span class="sxs-lookup"><span data-stu-id="823ec-121">We'll use the default "NerdDinner.Tests" name for the test project we create, and use the "Visual Studio Unit Test" framework option.</span></span> <span data-ttu-id="823ec-122">クリックしたとき、[ok] ボタンの Visual Studio はソリューションを単体テストのため、web アプリケーションのいずれかとその中の 2 つのプロジェクトの作成します。</span><span class="sxs-lookup"><span data-stu-id="823ec-122">When we click the "ok" button Visual Studio will create a solution for us with two projects in it - one for our web application and one for our unit tests:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a><span data-ttu-id="823ec-123">NerdDinner ディレクトリ構造の確認</span><span class="sxs-lookup"><span data-stu-id="823ec-123">Examining the NerdDinner directory structure</span></span>

<span data-ttu-id="823ec-124">Visual Studio で新しい ASP.NET MVC アプリケーションを作成するときに自動的にさまざまなファイルとディレクトリをプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="823ec-124">When you create a new ASP.NET MVC application with Visual Studio, it automatically adds a number of files and directories to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image4.png)

<span data-ttu-id="823ec-125">既定では、ASP.NET MVC プロジェクトでは、6 つの最上位ディレクトリがあります。</span><span class="sxs-lookup"><span data-stu-id="823ec-125">ASP.NET MVC projects by default have six top-level directories:</span></span>

| <span data-ttu-id="823ec-126">**ディレクトリ**</span><span class="sxs-lookup"><span data-stu-id="823ec-126">**Directory**</span></span> | <span data-ttu-id="823ec-127">**目的**</span><span class="sxs-lookup"><span data-stu-id="823ec-127">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="823ec-128">**/コント ローラー**</span><span class="sxs-lookup"><span data-stu-id="823ec-128">**/Controllers**</span></span> | <span data-ttu-id="823ec-129">URL 要求を処理するコント ローラー クラスを配置します。</span><span class="sxs-lookup"><span data-stu-id="823ec-129">Where you put Controller classes that handle URL requests</span></span> |
| <span data-ttu-id="823ec-130">**/モデル**</span><span class="sxs-lookup"><span data-stu-id="823ec-130">**/Models**</span></span> | <span data-ttu-id="823ec-131">データの操作を表すクラスを配置します。</span><span class="sxs-lookup"><span data-stu-id="823ec-131">Where you put classes that represent and manipulate data</span></span> |
| <span data-ttu-id="823ec-132">**/Views**</span><span class="sxs-lookup"><span data-stu-id="823ec-132">**/Views**</span></span> | <span data-ttu-id="823ec-133">出力のレンダリングを担当する UI テンプレート ファイルを配置します。</span><span class="sxs-lookup"><span data-stu-id="823ec-133">Where you put UI template files that are responsible for rendering output</span></span> |
| <span data-ttu-id="823ec-134">**/Scripts**</span><span class="sxs-lookup"><span data-stu-id="823ec-134">**/Scripts**</span></span> | <span data-ttu-id="823ec-135">JavaScript ライブラリのファイルとスクリプト (.js) を配置します。</span><span class="sxs-lookup"><span data-stu-id="823ec-135">Where you put JavaScript library files and scripts (.js)</span></span> |
| <span data-ttu-id="823ec-136">**/Content**</span><span class="sxs-lookup"><span data-stu-id="823ec-136">**/Content**</span></span> | <span data-ttu-id="823ec-137">CSS とイメージ ファイル、およびその他の非動的または非 JavaScript コンテンツを配置します。</span><span class="sxs-lookup"><span data-stu-id="823ec-137">Where you put CSS and image files, and other non-dynamic/non-JavaScript content</span></span> |
| <span data-ttu-id="823ec-138">**/アプリ\_データ**</span><span class="sxs-lookup"><span data-stu-id="823ec-138">**/App\_Data**</span></span> | <span data-ttu-id="823ec-139">データ ファイルの格納先は、読み取り/書き込みします。</span><span class="sxs-lookup"><span data-stu-id="823ec-139">Where you store data files you want to read/write.</span></span> |

<span data-ttu-id="823ec-140">ASP.NET MVC では、この構造体は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="823ec-140">ASP.NET MVC does not require this structure.</span></span> <span data-ttu-id="823ec-141">実際には、大規模なアプリケーションで作業する開発者は通常、アプリケーションの間でパーティション分割より管理しやすいように複数のプロジェクト (例: データ モデル クラスは多くの場合、別のクラス ライブラリ プロジェクトで web アプリケーションから移動) します。</span><span class="sxs-lookup"><span data-stu-id="823ec-141">In fact, developers working on large applications will typically partition the application up across multiple projects to make it more manageable (for example: data model classes often go in a separate class library project from the web application).</span></span> <span data-ttu-id="823ec-142">ただし、既定のプロジェクト構造は、アプリケーションの問題をクリーンに維持するために使用できる便利な既定のディレクトリ規則を提供しています。</span><span class="sxs-lookup"><span data-stu-id="823ec-142">The default project structure, however, does provide a nice default directory convention that we can use to keep our application concerns clean.</span></span>

<span data-ttu-id="823ec-143">/Controllers ディレクトリを展開している場合は、Visual Studio が、プロジェクトに既定で – HomeController と AccountController – 2 つのコント ローラー クラスを追加する分かります。</span><span class="sxs-lookup"><span data-stu-id="823ec-143">When we expand the /Controllers directory we'll find that Visual Studio added two controller classes – HomeController and AccountController – by default to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image5.png)

<span data-ttu-id="823ec-144">/Views ディレクトリを展開しましたと – 内のファイルがプロジェクトに既定で追加もいくつかのテンプレートに加え、/Home、/Account および/Shared – 3 つのサブ ディレクトリを検索します。</span><span class="sxs-lookup"><span data-stu-id="823ec-144">When we expand the /Views directory, we'll find three sub-directories – /Home, /Account and /Shared – as well as several template files within them were also added to the project by default:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image6.png)

<span data-ttu-id="823ec-145">/Content と/Scripts ディレクトリを展開しますと、アプリケーション内でをサポートできるように ASP.NET AJAX と jQuery JavaScript ライブラリと同様に、サイトのすべての HTML のスタイル設定に使用される Site.css ファイルを検索します。</span><span class="sxs-lookup"><span data-stu-id="823ec-145">When we expand the /Content and /Scripts directories, we'll find a Site.css file that is used to style all HTML on the site, as well as JavaScript libraries that can enable ASP.NET AJAX and jQuery support within the application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image7.png)

<span data-ttu-id="823ec-146">NerdDinner.Tests プロジェクトを展開している場合、コント ローラー クラスの単体テストが含まれている 2 つのクラスを検索します。</span><span class="sxs-lookup"><span data-stu-id="823ec-146">When we expand the NerdDinner.Tests project we'll find two classes that contain unit tests for our controller classes:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image8.png)

<span data-ttu-id="823ec-147">Visual Studio によって追加されたこれらの既定のファイルの基本的な構造での作業アプリケーションのホーム ページで、ページ、アカウントのログイン/ログアウト/登録ページ (すべてワイヤード (有線) アップ動作し、ボックス外) のハンドルされないエラー ページについての完全な提供します。</span><span class="sxs-lookup"><span data-stu-id="823ec-147">These default files added by Visual Studio provide us with a basic structure for a working application - complete with home page, about page, account login/logout/registration pages, and an unhandled error page (all wired-up and working out of the box).</span></span>

### <a name="running-the-nerddinner-application"></a><span data-ttu-id="823ec-148">NerdDinner アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="823ec-148">Running the NerdDinner Application</span></span>

<span data-ttu-id="823ec-149">いずれかを選択してプロジェクトを実行できる、**デバッグ -&gt;デバッグの開始**または**デバッグ -&gt;デバッグなしで開始**メニュー項目。</span><span class="sxs-lookup"><span data-stu-id="823ec-149">We can run the project by choosing either the **Debug-&gt;Start Debugging** or **Debug-&gt;Start Without Debugging** menu items:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image9.png)

<span data-ttu-id="823ec-150">組み込み ASP.NET Web サーバー、Visual Studio に付属する起動され、アプリケーションの実行します。</span><span class="sxs-lookup"><span data-stu-id="823ec-150">This will launch the built-in ASP.NET Web-server that comes with Visual Studio, and run our application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image10.png)

<span data-ttu-id="823ec-151">新しいプロジェクトのホーム ページを以下に示します (URL:「/」) 実行されるとき。</span><span class="sxs-lookup"><span data-stu-id="823ec-151">Below is the home page for our new project (URL: "/") when it runs:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image11.png)

<span data-ttu-id="823ec-152">"About" タブをクリックすると、に関するページ (URL:「ホーム/についての/」)。</span><span class="sxs-lookup"><span data-stu-id="823ec-152">Clicking the "About" tab displays an about page (URL: "/Home/About"):</span></span>

![](create-a-new-aspnet-mvc-project/_static/image12.png)

<span data-ttu-id="823ec-153">右上の [ログオン] リンクをクリックすると、ログイン ページに移動 (URL:「アカウント/ログオン」)</span><span class="sxs-lookup"><span data-stu-id="823ec-153">Clicking the "Log On" link on the top-right takes us to a Login page (URL: "/Account/LogOn")</span></span>

![](create-a-new-aspnet-mvc-project/_static/image13.png)

<span data-ttu-id="823ec-154">かどうかログイン アカウントが登録リンクをクリックすることはありません (URL:「アカウント/登録」) を作成します。</span><span class="sxs-lookup"><span data-stu-id="823ec-154">If we don't have a login account we can click the register link (URL: "/Account/Register") to create one:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image14.png)

<span data-ttu-id="823ec-155">について、上記のホームを実装するコードとログアウト/を登録、新しいプロジェクトを作成したときに、既定では機能が追加されました。</span><span class="sxs-lookup"><span data-stu-id="823ec-155">The code to implement the above home, about, and logout/ register functionality was added by default when we created our new project.</span></span> <span data-ttu-id="823ec-156">そのアプリケーションの開始点として使用します。</span><span class="sxs-lookup"><span data-stu-id="823ec-156">We'll use it as the starting point of our application.</span></span>

### <a name="testing-the-nerddinner-application"></a><span data-ttu-id="823ec-157">NerdDinner アプリケーションのテスト</span><span class="sxs-lookup"><span data-stu-id="823ec-157">Testing the NerdDinner Application</span></span>

<span data-ttu-id="823ec-158">Professional Edition または Visual Studio 2008 の新しいバージョンを使用している私たちは場合、プロジェクトをテストする組み込みの単体テストの Visual studio IDE のサポートを使用できます。</span><span class="sxs-lookup"><span data-stu-id="823ec-158">If we are using the Professional Edition or higher version of Visual Studio 2008, we can use the built-in unit testing IDE support within Visual Studio to test the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image15.png)

<span data-ttu-id="823ec-159">上記のオプションのいずれかを選択する、IDE 内にある [テスト結果] ウィンドウを開くし、組み込みの機能をカバーする、新しいプロジェクトに含まれる 27 単体テストの合格/不合格の状態を提供します。</span><span class="sxs-lookup"><span data-stu-id="823ec-159">Choosing one of the above options will open the "Test Results" pane within the IDE and provide us with pass/fail status on the 27 unit tests included in our new project that cover the built-in functionality:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image16.png)

<span data-ttu-id="823ec-160">このチュートリアルの後半で自動テストについては説明がされアプリケーションの機能の実装に対応する追加の単体テストを追加します。</span><span class="sxs-lookup"><span data-stu-id="823ec-160">Later in this tutorial we'll talk more about automated testing and add additional unit tests that cover the application functionality we implement.</span></span>

### <a name="next-step"></a><span data-ttu-id="823ec-161">次の手順</span><span class="sxs-lookup"><span data-stu-id="823ec-161">Next Step</span></span>

<span data-ttu-id="823ec-162">場所に基本的なアプリケーションの構造体を取得しました。</span><span class="sxs-lookup"><span data-stu-id="823ec-162">We've now got a basic application structure in place.</span></span> <span data-ttu-id="823ec-163">これにしましょう[、アプリケーションのデータを格納するデータベースを作成](create-a-database.md)です。</span><span class="sxs-lookup"><span data-stu-id="823ec-163">Let's now [create a database to store our application data](create-a-database.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="823ec-164">[前へ](introducing-the-nerddinner-tutorial.md)
> [次へ](create-a-database.md)</span><span class="sxs-lookup"><span data-stu-id="823ec-164">[Previous](introducing-the-nerddinner-tutorial.md)
[Next](create-a-database.md)</span></span>
