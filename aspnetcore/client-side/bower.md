---
title: "ASP.NET Core での Bower を使用"
author: rick-anderson
description: "Bower でクライアント側のパッケージを管理します。"
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/bower
ms.openlocfilehash: 0205eb34ac7f8b10720b0aa3a19bbdc3a74b545b
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="manage-client-side-packages-with-bower-in-aspnet-core"></a><span data-ttu-id="8b92a-103">ASP.NET Core での Bower でクライアント側のパッケージを管理します。</span><span class="sxs-lookup"><span data-stu-id="8b92a-103">Manage client-side packages with Bower in ASP.NET Core</span></span>

<span data-ttu-id="8b92a-104">によって[Rick Anderson](https://twitter.com/RickAndMSFT)、[ノエル ライス](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)、および[Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="8b92a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Noel Rice](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/), and [Scott Addie](https://scottaddie.com)</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="8b92a-105">Bower を保持したまま、別のソリューションを使用してそれらを勧めします。</span><span class="sxs-lookup"><span data-stu-id="8b92a-105">While Bower is maintained, they recommend using a different solution.</span></span> <span data-ttu-id="8b92a-106">Webpack で yarn を 1 つの一般的な代替手段は、[移行手順](https://bower.io/blog/2017/how-to-migrate-away-from-bower/)を利用できます。</span><span class="sxs-lookup"><span data-stu-id="8b92a-106">Yarn with Webpack is one popular alternative for which [migration instructions](https://bower.io/blog/2017/how-to-migrate-away-from-bower/) are available.</span></span>

<span data-ttu-id="8b92a-107">[Bower](https://bower.io/) 「web のパッケージ マネージャー」自体を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="8b92a-107">[Bower](https://bower.io/) calls itself "A package manager for the web".</span></span> <span data-ttu-id="8b92a-108">.NET エコシステム内では、NuGet の静的なコンテンツ ファイルを配信できないことによってまま void が挿入されます。</span><span class="sxs-lookup"><span data-stu-id="8b92a-108">Within the .NET ecosystem, it fills the void left by NuGet's inability to deliver static content files.</span></span> <span data-ttu-id="8b92a-109">ASP.NET Core プロジェクトでは、これらの静的ファイルはのようにクライアント側のライブラリに固有[jQuery](http://jquery.com/)と[ブートス トラップ](http://getbootstrap.com/)です。</span><span class="sxs-lookup"><span data-stu-id="8b92a-109">For ASP.NET Core projects, these static files are inherent to client-side libraries like [jQuery](http://jquery.com/) and [Bootstrap](http://getbootstrap.com/).</span></span> <span data-ttu-id="8b92a-110">.NET ライブラリを使用することも[NuGet](https://www.nuget.org/)パッケージ マネージャーです。</span><span class="sxs-lookup"><span data-stu-id="8b92a-110">For .NET libraries, you still use [NuGet](https://www.nuget.org/) package manager.</span></span>

<span data-ttu-id="8b92a-111">クライアント側の設定の ASP.NET Core プロジェクト テンプレートで作成した新しいプロジェクトはビルド プロセスです。</span><span class="sxs-lookup"><span data-stu-id="8b92a-111">New projects created with the ASP.NET Core project templates set up the client-side build process.</span></span> <span data-ttu-id="8b92a-112">[jQuery](http://jquery.com/)と[ブートス トラップ](http://getbootstrap.com/)がインストールされている、Bower がサポートされているとします。</span><span class="sxs-lookup"><span data-stu-id="8b92a-112">[jQuery](http://jquery.com/) and [Bootstrap](http://getbootstrap.com/) are installed, and Bower is supported.</span></span>

<span data-ttu-id="8b92a-113">クライアント側のパッケージに記載されて、 *bower.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="8b92a-113">Client-side packages are listed in the *bower.json* file.</span></span> <span data-ttu-id="8b92a-114">ASP.NET Core プロジェクト テンプレートを構成*bower.json* jQuery、jQuery 検証、およびブートス トラップを使用します。</span><span class="sxs-lookup"><span data-stu-id="8b92a-114">The ASP.NET Core project templates configures *bower.json* with jQuery, jQuery validation, and Bootstrap.</span></span>

<span data-ttu-id="8b92a-115">このチュートリアルではサポートを追加します[フォント優れた](http://fontawesome.io)です。</span><span class="sxs-lookup"><span data-stu-id="8b92a-115">In this tutorial, we'll add support for [Font Awesome](http://fontawesome.io).</span></span> <span data-ttu-id="8b92a-116">と共にインストールできる bower パッケージ、 **Bower パッケージの管理**UI または手動で、 *bower.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="8b92a-116">Bower packages can be installed with the **Manage Bower Packages** UI or manually in the *bower.json* file.</span></span>

### <a name="installation-via-manage-bower-packages-ui"></a><span data-ttu-id="8b92a-117">Bower パッケージの管理 UI を使用してインストール</span><span class="sxs-lookup"><span data-stu-id="8b92a-117">Installation via Manage Bower Packages UI</span></span>

* <span data-ttu-id="8b92a-118">新しい ASP.NET Core Web アプリを作成、 **ASP.NET Core Web アプリケーション (.NET Core)**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="8b92a-118">Create a new ASP.NET Core Web app with the **ASP.NET Core Web Application (.NET Core)** template.</span></span> <span data-ttu-id="8b92a-119">選択**Web アプリケーション**と**認証なし**です。</span><span class="sxs-lookup"><span data-stu-id="8b92a-119">Select **Web Application** and **No Authentication**.</span></span>

* <span data-ttu-id="8b92a-120">ソリューション エクスプ ローラーでプロジェクトを右クリックし  **Bower パッケージの管理**(また、メイン メニューから**プロジェクト** > **Bowerパッケージの管理**).</span><span class="sxs-lookup"><span data-stu-id="8b92a-120">Right-click the project in Solution Explorer and select **Manage Bower Packages** (alternatively from the main menu, **Project** > **Manage Bower Packages**).</span></span>

* <span data-ttu-id="8b92a-121">**Bower:\<プロジェクト名\>**ウィンドウで、「参照」 タブをクリックし、入力することで、パッケージ の一覧をフィルター`font-awesome`検索ボックスに。</span><span class="sxs-lookup"><span data-stu-id="8b92a-121">In the **Bower: \<project name\>** window, click the "Browse" tab, and then filter the packages list by entering `font-awesome` in the search box:</span></span>

 ![bower パッケージを管理します。](bower/_static/manage-bower-packages.png)

* <span data-ttu-id="8b92a-123">いることを確認します"に変更を保存*bower.json*"チェック ボックスをオンします。</span><span class="sxs-lookup"><span data-stu-id="8b92a-123">Confirm that the "Save changes to *bower.json*" checkbox is checked.</span></span> <span data-ttu-id="8b92a-124">ドロップダウン リストからバージョンを選択し、**インストール**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="8b92a-124">Select a version from the drop-down list and click the **Install** button.</span></span> <span data-ttu-id="8b92a-125">**出力**ウィンドウは、インストールの詳細を表示します。</span><span class="sxs-lookup"><span data-stu-id="8b92a-125">The **Output** window shows the installation details.</span></span>

### <a name="manual-installation-in-bowerjson"></a><span data-ttu-id="8b92a-126">Bower.json に手動のインストール</span><span class="sxs-lookup"><span data-stu-id="8b92a-126">Manual installation in bower.json</span></span>

<span data-ttu-id="8b92a-127">開く、 *bower.json*ファイルおよび依存関係に「フォント優れた」を追加します。</span><span class="sxs-lookup"><span data-stu-id="8b92a-127">Open the *bower.json* file and add "font-awesome" to the dependencies.</span></span> <span data-ttu-id="8b92a-128">IntelliSense は、利用可能なパッケージを示しています。</span><span class="sxs-lookup"><span data-stu-id="8b92a-128">IntelliSense shows the available packages.</span></span> <span data-ttu-id="8b92a-129">パッケージを選択すると、使用可能なバージョンが表示されます。</span><span class="sxs-lookup"><span data-stu-id="8b92a-129">When a package is selected, the available versions are displayed.</span></span> <span data-ttu-id="8b92a-130">次の図は、古いし、表示される内容とも一致しません。</span><span class="sxs-lookup"><span data-stu-id="8b92a-130">The images below are older and won't match what you see.</span></span>

![Bower パッケージ エクスプ ローラーの IntelliSense](bower/_static/add-package.png)

![bower バージョン IntelliSense](bower/_static/version-intelliSense.png)

<span data-ttu-id="8b92a-133">使用を bower[セマンティック バージョニング](http://semver.org/)の依存関係を整理します。</span><span class="sxs-lookup"><span data-stu-id="8b92a-133">Bower uses [semantic versioning](http://semver.org/) to organize dependencies.</span></span> <span data-ttu-id="8b92a-134">セマンティック バージョニング、SemVer とも呼ばれる、番号付けスキームでパッケージを識別する\<メジャー >.\<マイナー >。\<パッチ >。</span><span class="sxs-lookup"><span data-stu-id="8b92a-134">Semantic versioning, also known as SemVer, identifies packages with the numbering scheme \<major>.\<minor>.\<patch>.</span></span> <span data-ttu-id="8b92a-135">IntelliSense は、いくつかの一般的な選択肢のみを表示することによって、セマンティック バージョニングを簡略化します。</span><span class="sxs-lookup"><span data-stu-id="8b92a-135">IntelliSense simplifies semantic versioning by showing only a few common choices.</span></span> <span data-ttu-id="8b92a-136">IntelliSense の一覧 (上記の例では 4.6.3) の最上位の項目は、パッケージの最新の安定バージョンと見なされます。</span><span class="sxs-lookup"><span data-stu-id="8b92a-136">The top item in the IntelliSense list (4.6.3 in the example above) is considered the latest stable version of the package.</span></span> <span data-ttu-id="8b92a-137">キャレット (^) 記号が最新のメジャー バージョンと一致し、ティルダ (~) には、最新のマイナー バージョンが一致します。</span><span class="sxs-lookup"><span data-stu-id="8b92a-137">The caret (^) symbol matches the most recent major version and the tilde (~) matches the most recent minor version.</span></span>

<span data-ttu-id="8b92a-138">保存、 *bower.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="8b92a-138">Save the *bower.json* file.</span></span> <span data-ttu-id="8b92a-139">Visual Studio は、監視、 *bower.json*ファイルの変更。</span><span class="sxs-lookup"><span data-stu-id="8b92a-139">Visual Studio watches the *bower.json* file for changes.</span></span> <span data-ttu-id="8b92a-140">保存時に、 *bower インストール*コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="8b92a-140">Upon saving, the *bower install* command is executed.</span></span> <span data-ttu-id="8b92a-141">出力ウィンドウの表示**npm/Bower**正確なコマンドが実行を表示します。</span><span class="sxs-lookup"><span data-stu-id="8b92a-141">See the Output window's **Bower/npm** view for the exact command executed.</span></span>

<span data-ttu-id="8b92a-142">開く、 *.bowerrc*下にあるファイル*bower.json*です。</span><span class="sxs-lookup"><span data-stu-id="8b92a-142">Open the *.bowerrc* file under *bower.json*.</span></span> <span data-ttu-id="8b92a-143">`directory`プロパティに設定されている*wwwroot/lib*パッケージ資産をインストールする場所 Bower を示します。</span><span class="sxs-lookup"><span data-stu-id="8b92a-143">The `directory` property is set to *wwwroot/lib* which indicates the location Bower will install the package assets.</span></span>

```json
{
 "directory": "wwwroot/lib"
}
```

<span data-ttu-id="8b92a-144">ソリューション エクスプ ローラーで、検索ボックスを使用して、見つけてフォント優れたパッケージを表示することができます。</span><span class="sxs-lookup"><span data-stu-id="8b92a-144">You can use the search box in Solution Explorer to find and display the font-awesome package.</span></span>

<span data-ttu-id="8b92a-145">開く、 *\shared\_Layout.cshtml*ファイルし、環境にフォント優れた CSS ファイルを追加[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)の`Development`します。</span><span class="sxs-lookup"><span data-stu-id="8b92a-145">Open the *Views\Shared\_Layout.cshtml* file and add the font-awesome CSS file to the environment [Tag Helper](xref:mvc/views/tag-helpers/intro) for `Development`.</span></span> <span data-ttu-id="8b92a-146">ソリューション エクスプ ローラーからドラッグ アンド ドロップ*フォント awesome.css*内、`<environment names="Development">`要素。</span><span class="sxs-lookup"><span data-stu-id="8b92a-146">From Solution Explorer, drag and drop *font-awesome.css* inside the `<environment names="Development">` element.</span></span>

[!code-html[Main](bower/sample/_Layout.cshtml?highlight=4&range=9-13)]

<span data-ttu-id="8b92a-147">運用アプリにするには追加*フォント awesome.min.css* 、環境タグのヘルパーを`Staging,Production`です。</span><span class="sxs-lookup"><span data-stu-id="8b92a-147">In a production app you would add *font-awesome.min.css* to the environment tag helper for `Staging,Production`.</span></span>

<span data-ttu-id="8b92a-148">内容を置き換える、 *Views\Home\About.cshtml*次のマークアップ Razor ファイル。</span><span class="sxs-lookup"><span data-stu-id="8b92a-148">Replace the contents of the *Views\Home\About.cshtml* Razor file with the following markup:</span></span>

[!code-html[Main](bower/sample/About.cshtml)]

<span data-ttu-id="8b92a-149">アプリを実行し、フォント優れたパッケージの動作の確認 [バージョン情報] ビューに移動します。</span><span class="sxs-lookup"><span data-stu-id="8b92a-149">Run the app and navigate to the About view to verify the font-awesome package works.</span></span>

## <a name="exploring-the-client-side-build-process"></a><span data-ttu-id="8b92a-150">クライアント側のビルド プロセスの検証</span><span class="sxs-lookup"><span data-stu-id="8b92a-150">Exploring the client-side build process</span></span>

<span data-ttu-id="8b92a-151">ほとんどの ASP.NET Core プロジェクト テンプレートは、Bower を使用してように既に構成済みです。</span><span class="sxs-lookup"><span data-stu-id="8b92a-151">Most ASP.NET Core project templates are already configured to use Bower.</span></span> <span data-ttu-id="8b92a-152">この次のチュートリアルでは、空の ASP.NET Core プロジェクトから開始して、プロジェクトで Bower を使用する方法について理解を取得できるように、手動で各部分を追加します。</span><span class="sxs-lookup"><span data-stu-id="8b92a-152">This next walkthrough starts with an empty ASP.NET Core project and adds each piece manually, so you can get a feel for how Bower is used in a project.</span></span> <span data-ttu-id="8b92a-153">プロジェクトの構造と、ランタイムが出力されている各構成の変更が行われたとおりに動作を確認できます。</span><span class="sxs-lookup"><span data-stu-id="8b92a-153">You can see what happens to the project structure and the runtime output as each configuration change is made.</span></span>

<span data-ttu-id="8b92a-154">Bower でクライアント側のビルド プロセスを使用する一般的な手順は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="8b92a-154">The general steps to use the client-side build process with Bower are:</span></span>

* <span data-ttu-id="8b92a-155">プロジェクトで使用されるパッケージを定義します。</span><span class="sxs-lookup"><span data-stu-id="8b92a-155">Define packages used in your project.</span></span> <!-- once defined, you don't need to download them, VS does -->
* <span data-ttu-id="8b92a-156">Web ページからのパッケージを参照します。</span><span class="sxs-lookup"><span data-stu-id="8b92a-156">Reference packages from your web pages.</span></span>

### <a name="define-packages"></a><span data-ttu-id="8b92a-157">パッケージを定義します。</span><span class="sxs-lookup"><span data-stu-id="8b92a-157">Define packages</span></span>

<span data-ttu-id="8b92a-158">パッケージを一覧表示した後、 *bower.json*ファイル、Visual Studio がダウンロードされます。</span><span class="sxs-lookup"><span data-stu-id="8b92a-158">Once you list packages in the *bower.json* file, Visual Studio will download them.</span></span> <span data-ttu-id="8b92a-159">次の例で Bower を使用して jQuery とブートス トラップを読み込む、 *wwwroot*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="8b92a-159">The following example uses Bower to load jQuery and Bootstrap to the *wwwroot* folder.</span></span>

* <span data-ttu-id="8b92a-160">新しい ASP.NET Core Web アプリを作成、 **ASP.NET Core Web アプリケーション (.NET Core)**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="8b92a-160">Create a new ASP.NET Core Web app with the **ASP.NET Core Web Application (.NET Core)** template.</span></span> <span data-ttu-id="8b92a-161">選択、**空**プロジェクト テンプレートとクリック**OK**です。</span><span class="sxs-lookup"><span data-stu-id="8b92a-161">Select the **Empty** project template and click **OK**.</span></span>

* <span data-ttu-id="8b92a-162">ソリューション エクスプ ローラーでプロジェクトを右クリックして >**新しい項目の追加**選択**Bower 構成ファイル**です。</span><span class="sxs-lookup"><span data-stu-id="8b92a-162">In Solution Explorer, right-click the project > **Add New Item** and select **Bower Configuration File**.</span></span> <span data-ttu-id="8b92a-163">注: A *.bowerrc*ファイルも追加します。</span><span class="sxs-lookup"><span data-stu-id="8b92a-163">Note: A *.bowerrc* file is also added.</span></span>

* <span data-ttu-id="8b92a-164">開いている*bower.json*、jquery を追加しをブートス トラップ、`dependencies`セクションです。</span><span class="sxs-lookup"><span data-stu-id="8b92a-164">Open *bower.json*, and add jquery and bootstrap to the `dependencies` section.</span></span> <span data-ttu-id="8b92a-165">その結果、 *bower.json*ファイルは次の例のようになります。</span><span class="sxs-lookup"><span data-stu-id="8b92a-165">The resulting *bower.json* file will look like the following example.</span></span> <span data-ttu-id="8b92a-166">バージョンは、時間の経過と共にが変更され、下の画像と一致しない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="8b92a-166">The versions will change over time and may not match the image below.</span></span>

[!code-json[Main](bower/sample/bower.json?highlight=5,6)]

* <span data-ttu-id="8b92a-167">保存、 *bower.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="8b92a-167">Save the *bower.json* file.</span></span>

 <span data-ttu-id="8b92a-168">プロジェクトに含まれることを確認、*ブートス トラップ*と*jQuery*ディレクトリ*wwwroot/lib*です。</span><span class="sxs-lookup"><span data-stu-id="8b92a-168">Verify the project includes the *bootstrap* and *jQuery* directories in *wwwroot/lib*.</span></span> <span data-ttu-id="8b92a-169">Bower 使用して、 *.bowerrc*ファイル内の資産をインストールする*wwwroot/lib*です。</span><span class="sxs-lookup"><span data-stu-id="8b92a-169">Bower uses the *.bowerrc* file to install the assets in *wwwroot/lib*.</span></span>

 <span data-ttu-id="8b92a-170">注:「Bower パッケージの管理」の UI は、手動ファイルを編集する代わりにを提供します。</span><span class="sxs-lookup"><span data-stu-id="8b92a-170">Note: The "Manage Bower Packages" UI provides an alternative to manual file editing.</span></span>

### <a name="enable-static-files"></a><span data-ttu-id="8b92a-171">静的なファイルを有効にします。</span><span class="sxs-lookup"><span data-stu-id="8b92a-171">Enable static files</span></span>

* <span data-ttu-id="8b92a-172">追加、 `Microsoft.AspNetCore.StaticFiles` NuGet パッケージをプロジェクトにします。</span><span class="sxs-lookup"><span data-stu-id="8b92a-172">Add the `Microsoft.AspNetCore.StaticFiles` NuGet package to the project.</span></span>
* <span data-ttu-id="8b92a-173">静的ファイルで配信されることを有効にする、[静的ファイル ミドルウェア](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions)です。</span><span class="sxs-lookup"><span data-stu-id="8b92a-173">Enable static files to be served with the [Static file middleware](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions).</span></span> <span data-ttu-id="8b92a-174">呼び出しを追加して[UseStaticFiles](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions)を`Configure`メソッドの`Startup`します。</span><span class="sxs-lookup"><span data-stu-id="8b92a-174">Add a call to [UseStaticFiles](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions) to the `Configure` method of `Startup`.</span></span>

[!code-csharp[Main](bower/sample/Startup.cs?highlight=9)]

### <a name="reference-packages"></a><span data-ttu-id="8b92a-175">参照パッケージ</span><span class="sxs-lookup"><span data-stu-id="8b92a-175">Reference packages</span></span>

<span data-ttu-id="8b92a-176">このセクションでは、配置したパッケージにアクセスできることを確認する HTML ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="8b92a-176">In this section, you will create an HTML page to verify it can access the deployed packages.</span></span>

* <span data-ttu-id="8b92a-177">という名前の新しい HTML ページを追加*Index.html*を*wwwroot*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="8b92a-177">Add a new HTML page named *Index.html* to the *wwwroot* folder.</span></span> <span data-ttu-id="8b92a-178">注: HTML ファイルを追加する必要があります、 *wwwroot*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="8b92a-178">Note: You must add the HTML file to the *wwwroot* folder.</span></span> <span data-ttu-id="8b92a-179">既定では、外部の静的なコンテンツを処理できない*wwwroot*です。</span><span class="sxs-lookup"><span data-stu-id="8b92a-179">By default, static content cannot be served outside *wwwroot*.</span></span> <span data-ttu-id="8b92a-180">参照してください[静的ファイルで作業](xref:fundamentals/static-files)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="8b92a-180">See [Working with static files](xref:fundamentals/static-files) for more information.</span></span>

 <span data-ttu-id="8b92a-181">内容を置き換える*Index.html*次のマークアップ。</span><span class="sxs-lookup"><span data-stu-id="8b92a-181">Replace the contents of *Index.html* with the following markup:</span></span>

[!code-html[Main](bower/sample/Index.html)]

* <span data-ttu-id="8b92a-182">アプリを実行してに移動`http://localhost:<port>/Index.html`です。</span><span class="sxs-lookup"><span data-stu-id="8b92a-182">Run the app and navigate to `http://localhost:<port>/Index.html`.</span></span> <span data-ttu-id="8b92a-183">代わりに、 *Index.html*開くと、キーを押して`Ctrl+Shift+W`です。</span><span class="sxs-lookup"><span data-stu-id="8b92a-183">Alternatively, with *Index.html* opened, press `Ctrl+Shift+W`.</span></span> <span data-ttu-id="8b92a-184">Jumbotron スタイルが適用される、jQuery コード応答、ボタンがクリックされたときに、ブートス トラップのボタンが状態を変更することを確認します。</span><span class="sxs-lookup"><span data-stu-id="8b92a-184">Verify that the jumbotron styling is applied, the jQuery code responds when the button is clicked, and that the Bootstrap button changes state.</span></span>

 ![適用される jumbotron スタイル](bower/_static/jumbotron.png)
