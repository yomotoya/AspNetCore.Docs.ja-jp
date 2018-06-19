---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: 新しい ASP.NET MVC プロジェクトを作成 |Microsoft ドキュメント
author: microsoft
description: 手順 1 では、基本的な NerdDinner アプリケーション構造を導入する方法を示します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: d15ca67f0ddd8db6842bc5112996ae2dee433536
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869258"
---
<a name="create-a-new-aspnet-mvc-project"></a><span data-ttu-id="21835-103">新しい ASP.NET MVC プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="21835-103">Create a New ASP.NET MVC Project</span></span>
====================
<span data-ttu-id="21835-104">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="21835-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="21835-105">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="21835-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="21835-106">これは、無料のステップ 1 ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク-にする方法を小規模の構築が完了すると、ASP.NET MVC 1 を使用して web アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="21835-106">This is step 1 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="21835-107">手順 1 では、基本的な NerdDinner アプリケーション構造を導入する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="21835-107">Step 1 shows you how to put the basic NerdDinner application structure in place.</span></span>
> 
> <span data-ttu-id="21835-108">ASP.NET MVC 3 を使用している場合ことをお勧めする、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="21835-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-1-file-gtnew-project"></a><span data-ttu-id="21835-109">NerdDinner 手順 1: ファイル-&gt;新しいプロジェクト</span><span class="sxs-lookup"><span data-stu-id="21835-109">NerdDinner Step 1: File-&gt;New Project</span></span>

<span data-ttu-id="21835-110">まず NerdDinner アプリケーションを選択すると、**ファイル -&gt;新しいプロジェクト**メニュー項目内で Visual Studio 2008 または、空き Visual Web Developer 2008 Express のいずれか。</span><span class="sxs-lookup"><span data-stu-id="21835-110">We'll begin our NerdDinner application by selecting the **File-&gt;New Project** menu item within either Visual Studio 2008 or the free Visual Web Developer 2008 Express.</span></span>

<span data-ttu-id="21835-111">これは、"新しいプロジェクト ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="21835-111">This will bring up the "New Project" dialog.</span></span> <span data-ttu-id="21835-112">新しい ASP.NET MVC アプリケーションを作成するには、ダイアログ ボックスの左側にある"Web"ノードを選択し、右側の「ASP.NET MVC Web アプリケーション」のプロジェクト テンプレートを選択しておします。</span><span class="sxs-lookup"><span data-stu-id="21835-112">To create a new ASP.NET MVC application, we'll select the "Web" node on the left-hand side of the dialog and then choose the "ASP.NET MVC Web Application" project template on the right:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image1.png)

<span data-ttu-id="21835-113">*重要: ダウンロードし、ASP.NET MVC で、新しいプロジェクト ダイアログに表示されませんが、それ以外の場合をインストールしたことを確認してください。V2 を使用することができます、 [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)まだインストールしていない場合 (ASP.NET MVC は内で使用できる、"Web Platform の&gt;フレームワークおよびランタイム"セクション)。*</span><span class="sxs-lookup"><span data-stu-id="21835-113">*Important: Make sure you've downloaded and installed ASP.NET MVC - otherwise it won't show up in the New Project dialog. You can use V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) if you haven't installed it yet (ASP.NET MVC is available within the "Web Platform-&gt;Frameworks and Runtimes" section).*</span></span>

<span data-ttu-id="21835-114">新しいプロジェクトの"NerdDinner"を作成し、それを作成する"ok"ボタンをクリックするという名前です。</span><span class="sxs-lookup"><span data-stu-id="21835-114">We'll name the new project we are going to create "NerdDinner" and then click the "ok" button to create it.</span></span>

<span data-ttu-id="21835-115">[Ok] をクリックしたとき、Visual Studio は、必要に応じて、新しいアプリケーションの単体テスト プロジェクトを作成することを要求する追加のダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="21835-115">When we click "ok" Visual Studio will bring up an additional dialog that prompts us to optionally create a unit test project for the new application as well.</span></span> <span data-ttu-id="21835-116">アプリケーションの動作と機能を検証する自動テストを作成することにより、この単体テスト プロジェクト (で取り上げるもの方法このチュートリアルで後ほど to do)。</span><span class="sxs-lookup"><span data-stu-id="21835-116">This unit test project enables us to create automated tests that verify the functionality and behavior of our application (something we'll cover how to-do later in this tutorial).</span></span>

![](create-a-new-aspnet-mvc-project/_static/image2.png)

<span data-ttu-id="21835-117">上記のダイアログ ボックスで、「テスト フレームワーク」ドロップダウンには、すべて使用可能な ASP.NET MVC 単体テスト プロジェクト テンプレートがコンピューターにインストールが格納されます。</span><span class="sxs-lookup"><span data-stu-id="21835-117">The "Test framework" dropdown in the above dialog is populated with all available ASP.NET MVC unit test project templates installed on the machine.</span></span> <span data-ttu-id="21835-118">バージョンは、NUnit、XUnit、MBUnit、ダウンロードできます。</span><span class="sxs-lookup"><span data-stu-id="21835-118">Versions can be downloaded for NUnit, MBUnit, and XUnit.</span></span> <span data-ttu-id="21835-119">組み込みの Visual Studio 単体テスト フレームワークもサポートされます。</span><span class="sxs-lookup"><span data-stu-id="21835-119">The built-in Visual Studio Unit Test framework is also supported.</span></span>

<span data-ttu-id="21835-120">*注意: Visual Studio の単体テスト フレームワークが Visual Studio 2008 Professional と以降のバージョンで使用可能ではのみです。VS 2008 Standard Edition または Visual Web Developer 2008 Express を使用している場合をダウンロードして、ASP.NET MVC のこのダイアログ ボックスに表示されるためには、NUnit、MBUnit、XUnit 拡張機能をインストールする必要があります。インストールされているすべてのテスト フレームワークがない場合は、ダイアログ ボックスは表示されません。*</span><span class="sxs-lookup"><span data-stu-id="21835-120">*Note: The Visual Studio Unit Test Framework is only available with Visual Studio 2008 Professional and higher versions. If you are using VS 2008 Standard Edition or Visual Web Developer 2008 Express you will need to download and install the NUnit, MBUnit or XUnit extensions for ASP.NET MVC in order for this dialog to be shown. The dialog will not display if there aren't any test frameworks installed.*</span></span>

<span data-ttu-id="21835-121">おを名前を使用して、既定値"NerdDinner.Tests"を作成するテスト プロジェクトのオプションを使用「Visual Studio 単体テスト」フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="21835-121">We'll use the default "NerdDinner.Tests" name for the test project we create, and use the "Visual Studio Unit Test" framework option.</span></span> <span data-ttu-id="21835-122">クリックしたとき、"ok"ボタン Visual Studio で作成されますソリューションご利用の米国内の web アプリケーション用と、単体テストのための 2 つのプロジェクト。</span><span class="sxs-lookup"><span data-stu-id="21835-122">When we click the "ok" button Visual Studio will create a solution for us with two projects in it - one for our web application and one for our unit tests:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a><span data-ttu-id="21835-123">NerdDinner ディレクトリ構造の確認</span><span class="sxs-lookup"><span data-stu-id="21835-123">Examining the NerdDinner directory structure</span></span>

<span data-ttu-id="21835-124">Visual Studio で新しい ASP.NET MVC アプリケーションを作成するときに自動的に、プロジェクトにいくつかのファイルとディレクトリを追加します。</span><span class="sxs-lookup"><span data-stu-id="21835-124">When you create a new ASP.NET MVC application with Visual Studio, it automatically adds a number of files and directories to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image4.png)

<span data-ttu-id="21835-125">既定では ASP.NET MVC プロジェクトでは、最上位 6 つのディレクトリがあります。</span><span class="sxs-lookup"><span data-stu-id="21835-125">ASP.NET MVC projects by default have six top-level directories:</span></span>

| <span data-ttu-id="21835-126">**ディレクトリ**</span><span class="sxs-lookup"><span data-stu-id="21835-126">**Directory**</span></span> | <span data-ttu-id="21835-127">**目的**</span><span class="sxs-lookup"><span data-stu-id="21835-127">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="21835-128">**/コント ローラー**</span><span class="sxs-lookup"><span data-stu-id="21835-128">**/Controllers**</span></span> | <span data-ttu-id="21835-129">URL 要求を処理するコント ローラー クラスを配置します。</span><span class="sxs-lookup"><span data-stu-id="21835-129">Where you put Controller classes that handle URL requests</span></span> |
| <span data-ttu-id="21835-130">**/Models**</span><span class="sxs-lookup"><span data-stu-id="21835-130">**/Models**</span></span> | <span data-ttu-id="21835-131">データの操作を表すクラスを配置します。</span><span class="sxs-lookup"><span data-stu-id="21835-131">Where you put classes that represent and manipulate data</span></span> |
| <span data-ttu-id="21835-132">**/Views**</span><span class="sxs-lookup"><span data-stu-id="21835-132">**/Views**</span></span> | <span data-ttu-id="21835-133">出力のレンダリングを担当する UI テンプレート ファイルを配置します。</span><span class="sxs-lookup"><span data-stu-id="21835-133">Where you put UI template files that are responsible for rendering output</span></span> |
| <span data-ttu-id="21835-134">**/Scripts**</span><span class="sxs-lookup"><span data-stu-id="21835-134">**/Scripts**</span></span> | <span data-ttu-id="21835-135">JavaScript ライブラリのファイルとスクリプト (.js) を配置します。</span><span class="sxs-lookup"><span data-stu-id="21835-135">Where you put JavaScript library files and scripts (.js)</span></span> |
| <span data-ttu-id="21835-136">**/Content**</span><span class="sxs-lookup"><span data-stu-id="21835-136">**/Content**</span></span> | <span data-ttu-id="21835-137">CSS、画像ファイル、およびその他の非動的/非 JavaScript コンテンツを配置します。</span><span class="sxs-lookup"><span data-stu-id="21835-137">Where you put CSS and image files, and other non-dynamic/non-JavaScript content</span></span> |
| <span data-ttu-id="21835-138">**/App\_Data**</span><span class="sxs-lookup"><span data-stu-id="21835-138">**/App\_Data**</span></span> | <span data-ttu-id="21835-139">データ ファイルを保存する場合は、読み取り/書き込みします。</span><span class="sxs-lookup"><span data-stu-id="21835-139">Where you store data files you want to read/write.</span></span> |

<span data-ttu-id="21835-140">ASP.NET MVC には、この構造体は不要です。</span><span class="sxs-lookup"><span data-stu-id="21835-140">ASP.NET MVC does not require this structure.</span></span> <span data-ttu-id="21835-141">実際には、大規模なアプリケーションで作業する開発者は通常のパーティション、アプリケーションをより管理しやすいに複数のプロジェクト (例: データ モデル クラスは多くの場合、別のクラス ライブラリ プロジェクトで、web アプリケーションから移動) します。</span><span class="sxs-lookup"><span data-stu-id="21835-141">In fact, developers working on large applications will typically partition the application up across multiple projects to make it more manageable (for example: data model classes often go in a separate class library project from the web application).</span></span> <span data-ttu-id="21835-142">ただし、既定のプロジェクト構造は、とらえてアプリケーションをクリーンして使用できる便利な既定のディレクトリ規約を提供しています。</span><span class="sxs-lookup"><span data-stu-id="21835-142">The default project structure, however, does provide a nice default directory convention that we can use to keep our application concerns clean.</span></span>

<span data-ttu-id="21835-143">/Controllers ディレクトリを展開してときに Visual Studio が、プロジェクトに既定で – HomeController および AccountController – 2 つのコント ローラー クラスを追加するを検索します。</span><span class="sxs-lookup"><span data-stu-id="21835-143">When we expand the /Controllers directory we'll find that Visual Studio added two controller classes – HomeController and AccountController – by default to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image5.png)

<span data-ttu-id="21835-144">/Views ディレクトリを展開して、ときに 3 つのサブ ディレクトリ –/Home、/Account/Shared – とそれらに含まれるファイルがプロジェクトに既定では追加もいくつかのテンプレートを検索します。</span><span class="sxs-lookup"><span data-stu-id="21835-144">When we expand the /Views directory, we'll find three sub-directories – /Home, /Account and /Shared – as well as several template files within them were also added to the project by default:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image6.png)

<span data-ttu-id="21835-145">お展開と/Content と/Scripts ディレクトリ、サイトで、すべての HTML のスタイル設定に使用される Site.css ファイルだけでなく JavaScript ライブラリが ASP.NET AJAX と jQuery を有効にすることができるアプリケーション内でサポートを検索します。</span><span class="sxs-lookup"><span data-stu-id="21835-145">When we expand the /Content and /Scripts directories, we'll find a Site.css file that is used to style all HTML on the site, as well as JavaScript libraries that can enable ASP.NET AJAX and jQuery support within the application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image7.png)

<span data-ttu-id="21835-146">NerdDinner.Tests プロジェクトを展開している場合、コント ローラー クラスの単体テストを含む 2 つのクラスを検索します。</span><span class="sxs-lookup"><span data-stu-id="21835-146">When we expand the NerdDinner.Tests project we'll find two classes that contain unit tests for our controller classes:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image8.png)

<span data-ttu-id="21835-147">Visual Studio によって追加されたこれらの既定のファイルでは、us 基本的な構造を持つ作業アプリケーションのホーム ページで、に関するページ、アカウントのログインとログアウト/登録ページ、および未処理のエラー ページ (すべてワイヤード (有線) し、すぐ作業) に完了しませんでした。</span><span class="sxs-lookup"><span data-stu-id="21835-147">These default files added by Visual Studio provide us with a basic structure for a working application - complete with home page, about page, account login/logout/registration pages, and an unhandled error page (all wired-up and working out of the box).</span></span>

### <a name="running-the-nerddinner-application"></a><span data-ttu-id="21835-148">NerdDinner アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="21835-148">Running the NerdDinner Application</span></span>

<span data-ttu-id="21835-149">いずれかを選択してプロジェクトを実行できる、**デバッグ -&gt;デバッグの開始**または**デバッグ -&gt;デバッグなしで開始**メニュー項目。</span><span class="sxs-lookup"><span data-stu-id="21835-149">We can run the project by choosing either the **Debug-&gt;Start Debugging** or **Debug-&gt;Start Without Debugging** menu items:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image9.png)

<span data-ttu-id="21835-150">これでは、組み込み ASP.NET Web サーバー、Visual Studio に付属しているを起動して、アプリケーションの実行します。</span><span class="sxs-lookup"><span data-stu-id="21835-150">This will launch the built-in ASP.NET Web-server that comes with Visual Studio, and run our application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image10.png)

<span data-ttu-id="21835-151">新しいプロジェクトのホーム ページを次に示します (URL:「/」) 実行されるとき。</span><span class="sxs-lookup"><span data-stu-id="21835-151">Below is the home page for our new project (URL: "/") when it runs:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image11.png)

<span data-ttu-id="21835-152">"About" タブをクリックすると、に関するページ (URL:「ホーム/についての/」)。</span><span class="sxs-lookup"><span data-stu-id="21835-152">Clicking the "About" tab displays an about page (URL: "/Home/About"):</span></span>

![](create-a-new-aspnet-mvc-project/_static/image12.png)

<span data-ttu-id="21835-153">右上の [ログオン] リンクをクリックして、ログイン ページに移動 (URL:「/アカウント/ログオン」)</span><span class="sxs-lookup"><span data-stu-id="21835-153">Clicking the "Log On" link on the top-right takes us to a Login page (URL: "/Account/LogOn")</span></span>

![](create-a-new-aspnet-mvc-project/_static/image13.png)

<span data-ttu-id="21835-154">かどうかレジスタ」リンクをクリックしてログイン アカウントがありません (URL:「/アカウント/レジスタ」) を作成します。</span><span class="sxs-lookup"><span data-stu-id="21835-154">If we don't have a login account we can click the register link (URL: "/Account/Register") to create one:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image14.png)

<span data-ttu-id="21835-155">上記のホームを実装するコードとログアウトを登録/、新しいプロジェクトを作成した際、既定では機能が追加されました。</span><span class="sxs-lookup"><span data-stu-id="21835-155">The code to implement the above home, about, and logout/ register functionality was added by default when we created our new project.</span></span> <span data-ttu-id="21835-156">そのアプリケーションの開始点として使用されます。</span><span class="sxs-lookup"><span data-stu-id="21835-156">We'll use it as the starting point of our application.</span></span>

### <a name="testing-the-nerddinner-application"></a><span data-ttu-id="21835-157">NerdDinner アプリケーションのテスト</span><span class="sxs-lookup"><span data-stu-id="21835-157">Testing the NerdDinner Application</span></span>

<span data-ttu-id="21835-158">Professional Edition またはより新しいバージョンの Visual Studio 2008 を使用して場合プロジェクトをテストするテストの Visual Studio 内で IDE サポート組み込み単位を使用できます。</span><span class="sxs-lookup"><span data-stu-id="21835-158">If we are using the Professional Edition or higher version of Visual Studio 2008, we can use the built-in unit testing IDE support within Visual Studio to test the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image15.png)

<span data-ttu-id="21835-159">上記のオプションのいずれかを選択する、IDE 内で"テスト結果 ウィンドウを開くし、組み込みの機能をカバーする新しいプロジェクトに含まれている 27 単体テストの成功/失敗ステータスをお聞かせ。</span><span class="sxs-lookup"><span data-stu-id="21835-159">Choosing one of the above options will open the "Test Results" pane within the IDE and provide us with pass/fail status on the 27 unit tests included in our new project that cover the built-in functionality:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image16.png)

<span data-ttu-id="21835-160">このチュートリアルで後ほどおを自動テストについて詳しく説明し、実装のアプリケーションの機能に対応する追加の単体テストを追加します。</span><span class="sxs-lookup"><span data-stu-id="21835-160">Later in this tutorial we'll talk more about automated testing and add additional unit tests that cover the application functionality we implement.</span></span>

### <a name="next-step"></a><span data-ttu-id="21835-161">次の手順</span><span class="sxs-lookup"><span data-stu-id="21835-161">Next Step</span></span>

<span data-ttu-id="21835-162">場所にアプリケーションの基本構造を取得しました。</span><span class="sxs-lookup"><span data-stu-id="21835-162">We've now got a basic application structure in place.</span></span> <span data-ttu-id="21835-163">これにしましょう[、アプリケーションのデータを格納するデータベースを作成する](create-a-database.md)です。</span><span class="sxs-lookup"><span data-stu-id="21835-163">Let's now [create a database to store our application data](create-a-database.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="21835-164">[前へ](introducing-the-nerddinner-tutorial.md)
> [次へ](create-a-database.md)</span><span class="sxs-lookup"><span data-stu-id="21835-164">[Previous](introducing-the-nerddinner-tutorial.md)
[Next](create-a-database.md)</span></span>
