---
title: ASP.NET MVC から ASP.NET Core MVC への移行します。
author: ardalis
description: ASP.NET Core mvc、ASP.NET MVC プロジェクトの移行を開始する方法について説明します。
ms.author: riande
ms.date: 04/06/2019
uid: migration/mvc
ms.openlocfilehash: a9e2b41b933ed04a23515564892ed1694a4ac4f8
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64893499"
---
# <a name="migrate-from-aspnet-mvc-to-aspnet-core-mvc"></a><span data-ttu-id="f060d-103">ASP.NET MVC から ASP.NET Core MVC への移行します。</span><span class="sxs-lookup"><span data-stu-id="f060d-103">Migrate from ASP.NET MVC to ASP.NET Core MVC</span></span>

<span data-ttu-id="f060d-104">によって[Rick Anderson](https://twitter.com/RickAndMSFT)、 [Daniel Roth](https://github.com/danroth27)、 [Steve Smith](https://ardalis.com/)、および[Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="f060d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="f060d-105">この記事では、ASP.NET MVC プロジェクトの移行を開始する方法を示しています。 [ASP.NET Core MVC](../mvc/overview.md)します。</span><span class="sxs-lookup"><span data-stu-id="f060d-105">This article shows how to get started migrating an ASP.NET MVC project to [ASP.NET Core MVC](../mvc/overview.md).</span></span> <span data-ttu-id="f060d-106">プロセスで ASP.NET MVC から変更されたことの多くハイライトされます。</span><span class="sxs-lookup"><span data-stu-id="f060d-106">In the process, it highlights many of the things that have changed from ASP.NET MVC.</span></span> <span data-ttu-id="f060d-107">複数手順のプロセスは、ASP.NET MVC から移行して、この記事では、初期セットアップ、基本的なコント ローラーとビュー、静的コンテンツ、およびクライアント側の依存関係について説明します。</span><span class="sxs-lookup"><span data-stu-id="f060d-107">Migrating from ASP.NET MVC is a multiple step process and this article covers the initial setup, basic controllers and views, static content, and client-side dependencies.</span></span> <span data-ttu-id="f060d-108">その他の記事では、移行の構成と多くの ASP.NET MVC プロジェクトで検出された id コードについて説明します。</span><span class="sxs-lookup"><span data-stu-id="f060d-108">Additional articles cover migrating configuration and identity code found in many ASP.NET MVC projects.</span></span>

> [!NOTE]
> <span data-ttu-id="f060d-109">サンプルのバージョン番号が最新でない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="f060d-109">The version numbers in the samples might not be current.</span></span> <span data-ttu-id="f060d-110">それに応じて、プロジェクトを更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f060d-110">You may need to update your projects accordingly.</span></span>

## <a name="create-the-starter-aspnet-mvc-project"></a><span data-ttu-id="f060d-111">Starter の ASP.NET MVC プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="f060d-111">Create the starter ASP.NET MVC project</span></span>

<span data-ttu-id="f060d-112">アップグレードを示すためには、ASP.NET MVC アプリの作成から始めます。</span><span class="sxs-lookup"><span data-stu-id="f060d-112">To demonstrate the upgrade, we'll start by creating a ASP.NET MVC app.</span></span> <span data-ttu-id="f060d-113">名前を使用して作成*WebApp1*ので、名前空間の一致、ASP.NET Core プロジェクトの次の手順で作成します。</span><span class="sxs-lookup"><span data-stu-id="f060d-113">Create it with the name *WebApp1* so the namespace matches the ASP.NET Core project we create in the next step.</span></span>

![Visual Studio の新しいプロジェクト ダイアログ ボックス](mvc/_static/new-project.png)

![新しい Web アプリケーション ダイアログ:ASP.NET テンプレート パネルで選択した MVC プロジェクト テンプレート](mvc/_static/new-project-select-mvc-template.png)

<span data-ttu-id="f060d-116">*省略可能。* ソリューションからの名前を変更*WebApp1*に*Mvc5*します。</span><span class="sxs-lookup"><span data-stu-id="f060d-116">*Optional:* Change the name of the Solution from *WebApp1* to *Mvc5*.</span></span> <span data-ttu-id="f060d-117">Visual Studio は、新しいソリューション名を表示します (*Mvc5*)、ことが容易に、次のプロジェクトからこのプロジェクトに指示します。</span><span class="sxs-lookup"><span data-stu-id="f060d-117">Visual Studio displays the new solution name (*Mvc5*), which makes it easier to tell this project from the next project.</span></span>

## <a name="create-the-aspnet-core-project"></a><span data-ttu-id="f060d-118">ASP.NET Core プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="f060d-118">Create the ASP.NET Core project</span></span>

<span data-ttu-id="f060d-119">新規作成*空*前のプロジェクトと同じ名前の ASP.NET Core web アプリ (*WebApp1*) 2 つのプロジェクトの名前空間が一致するようにします。</span><span class="sxs-lookup"><span data-stu-id="f060d-119">Create a new *empty* ASP.NET Core web app with the same name as the previous project (*WebApp1*) so the namespaces in the two projects match.</span></span> <span data-ttu-id="f060d-120">同じ名前空間を持つと、2 つのプロジェクト間でコードをコピーしやすきます。</span><span class="sxs-lookup"><span data-stu-id="f060d-120">Having the same namespace makes it easier to copy code between the two projects.</span></span> <span data-ttu-id="f060d-121">同じ名前を使用して、前のプロジェクトとは異なるディレクトリでこのプロジェクトを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f060d-121">You'll have to create this project in a different directory than the previous project to use the same name.</span></span>

![[新しいプロジェクト] ダイアログ](mvc/_static/new_core.png)

![新しい ASP.NET Web アプリケーション ダイアログ:ASP.NET Core テンプレート パネルで選択した空のプロジェクト テンプレート](mvc/_static/new-project-select-empty-aspnet5-template.png)

* <span data-ttu-id="f060d-124">*省略可能。* 新しい ASP.NET Core アプリを作成、 *Web アプリケーション*プロジェクト テンプレート。</span><span class="sxs-lookup"><span data-stu-id="f060d-124">*Optional:* Create a new ASP.NET Core app using the *Web Application* project template.</span></span> <span data-ttu-id="f060d-125">プロジェクトに名前を*WebApp1*の認証オプションを選択します**個々 のユーザー アカウント**します。</span><span class="sxs-lookup"><span data-stu-id="f060d-125">Name the project *WebApp1*, and select an authentication option of **Individual User Accounts**.</span></span> <span data-ttu-id="f060d-126">このアプリの名前を変更*FullAspNetCore*します。</span><span class="sxs-lookup"><span data-stu-id="f060d-126">Rename this app to *FullAspNetCore*.</span></span> <span data-ttu-id="f060d-127">変換には、このプロジェクトを保存する時間を作成します。</span><span class="sxs-lookup"><span data-stu-id="f060d-127">Creating this project saves you time in the conversion.</span></span> <span data-ttu-id="f060d-128">最終的な結果を参照してください。 または、変換のプロジェクトにコードをコピーするテンプレートによって生成されたコードを確認することができます。</span><span class="sxs-lookup"><span data-stu-id="f060d-128">You can look at the template-generated code to see the end result or to copy code to the conversion project.</span></span> <span data-ttu-id="f060d-129">テンプレートによって生成されたプロジェクトと比較する変換ステップを行き詰まった場合にも便利です。</span><span class="sxs-lookup"><span data-stu-id="f060d-129">It's also helpful when you get stuck on a conversion step to compare with the template-generated project.</span></span>

## <a name="configure-the-site-to-use-mvc"></a><span data-ttu-id="f060d-130">MVC を使用するサイトを構成します。</span><span class="sxs-lookup"><span data-stu-id="f060d-130">Configure the site to use MVC</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="f060d-131">.NET Core を対象とする場合、 [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)既定で参照します。</span><span class="sxs-lookup"><span data-stu-id="f060d-131">When targeting .NET Core, the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) is referenced by default.</span></span> <span data-ttu-id="f060d-132">このパッケージには、MVC アプリでよく使用されるパッケージのパッケージが含まれています。</span><span class="sxs-lookup"><span data-stu-id="f060d-132">This package contains packages commonly used packages by MVC apps.</span></span> <span data-ttu-id="f060d-133">.NET Framework を対象とする場合、プロジェクト ファイルでのパッケージ参照を個別に追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f060d-133">If targeting .NET Framework, package references must be listed individually in the project file.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <span data-ttu-id="f060d-134">.NET Core を対象とする場合、 [Microsoft.AspNetCore.All メタパッケージ](xref:fundamentals/metapackage)既定で参照します。</span><span class="sxs-lookup"><span data-stu-id="f060d-134">When targeting .NET Core, the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) is referenced by default.</span></span> <span data-ttu-id="f060d-135">このパッケージには、MVC アプリでよく使用されるパッケージのパッケージが含まれています。</span><span class="sxs-lookup"><span data-stu-id="f060d-135">This package contains packages commonly used packages by MVC apps.</span></span> <span data-ttu-id="f060d-136">.NET Framework を対象とする場合、プロジェクト ファイルでのパッケージ参照を個別に追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f060d-136">If targeting .NET Framework, package references must be listed individually in the project file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* <span data-ttu-id="f060d-137">.NET Core または .NET Framework を対象とするときに MVC アプリでよく使用されるパッケージのパッケージがプロジェクト ファイルに個別に表示されます。</span><span class="sxs-lookup"><span data-stu-id="f060d-137">When targeting .NET Core or .NET Framework, packages commonly used packages by MVC apps are listed individually in the project file.</span></span>

::: moniker-end

<span data-ttu-id="f060d-138">`Microsoft.AspNetCore.Mvc` ASP.NET Core MVC フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="f060d-138">`Microsoft.AspNetCore.Mvc` is the ASP.NET Core MVC framework.</span></span> <span data-ttu-id="f060d-139">`Microsoft.AspNetCore.StaticFiles` 静的ファイル ハンドラーです。</span><span class="sxs-lookup"><span data-stu-id="f060d-139">`Microsoft.AspNetCore.StaticFiles` is the static file handler.</span></span> <span data-ttu-id="f060d-140">ASP.NET Core のランタイムがモジュール化、および静的ファイルを処理するために明示的にオプトインする必要があります (を参照してください[静的ファイル](xref:fundamentals/static-files))。</span><span class="sxs-lookup"><span data-stu-id="f060d-140">The ASP.NET Core runtime is modular, and you must explicitly opt in to serve static files (see [Static files](xref:fundamentals/static-files)).</span></span>

* <span data-ttu-id="f060d-141">開く、 *Startup.cs*ファイルを開き、次の一致するようにコードを変更します。</span><span class="sxs-lookup"><span data-stu-id="f060d-141">Open the *Startup.cs* file and change the code to match the following:</span></span>

  [!code-csharp[](mvc/sample/Startup.cs?highlight=13,26-31)]

<span data-ttu-id="f060d-142">`UseStaticFiles`拡張メソッドが静的ファイル ハンドラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="f060d-142">The `UseStaticFiles` extension method adds the static file handler.</span></span> <span data-ttu-id="f060d-143">前述のように、ASP.NET ランタイムは、モジュール式と、静的ファイルを処理するために明示的にオプトインする必要があります。</span><span class="sxs-lookup"><span data-stu-id="f060d-143">As mentioned previously, the ASP.NET runtime is modular, and you must explicitly opt in to serve static files.</span></span> <span data-ttu-id="f060d-144">`UseMvc`拡張メソッドがルーティングを追加します。</span><span class="sxs-lookup"><span data-stu-id="f060d-144">The `UseMvc` extension method adds routing.</span></span> <span data-ttu-id="f060d-145">詳細については、次を参照してください。[アプリケーションの起動](xref:fundamentals/startup)と[ルーティング](xref:fundamentals/routing)します。</span><span class="sxs-lookup"><span data-stu-id="f060d-145">For more information, see [Application Startup](xref:fundamentals/startup) and [Routing](xref:fundamentals/routing).</span></span>

## <a name="add-a-controller-and-view"></a><span data-ttu-id="f060d-146">コント ローラーとビューを追加します。</span><span class="sxs-lookup"><span data-stu-id="f060d-146">Add a controller and view</span></span>

<span data-ttu-id="f060d-147">このセクションでは、最小限のコント ローラーと ASP.NET MVC コント ローラーのプレース ホルダーとして機能するビューとビューの次のセクションで移行されますを追加します。</span><span class="sxs-lookup"><span data-stu-id="f060d-147">In this section, you'll add a minimal controller and view to serve as placeholders for the ASP.NET MVC controller and views you'll migrate in the next section.</span></span>

* <span data-ttu-id="f060d-148">追加、*コント ローラー*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="f060d-148">Add a *Controllers* folder.</span></span>

* <span data-ttu-id="f060d-149">追加、**コント ローラー クラス**という*HomeController.cs*を*コント ローラー*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="f060d-149">Add a **Controller Class** named *HomeController.cs* to the *Controllers* folder.</span></span>

![[新しい項目の追加] ダイアログ](mvc/_static/add_mvc_ctl.png)

* <span data-ttu-id="f060d-151">追加、*ビュー*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="f060d-151">Add a *Views* folder.</span></span>

* <span data-ttu-id="f060d-152">追加、*ビュー/ホーム*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="f060d-152">Add a *Views/Home* folder.</span></span>

* <span data-ttu-id="f060d-153">追加、 **Razor ビュー**という*Index.cshtml*を*ビュー/ホーム*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="f060d-153">Add a **Razor View** named *Index.cshtml* to the *Views/Home* folder.</span></span>

![[新しい項目の追加] ダイアログ](mvc/_static/view.png)

<span data-ttu-id="f060d-155">プロジェクトの構造は、以下に示します。</span><span class="sxs-lookup"><span data-stu-id="f060d-155">The project structure is shown below:</span></span>

![WebApp1 のファイルとフォルダーを表示したソリューション エクスプ ローラー](mvc/_static/project-structure-controller-view.png)

<span data-ttu-id="f060d-157">内容を置き換える、 *Views/Home/Index.cshtml*を次のファイル。</span><span class="sxs-lookup"><span data-stu-id="f060d-157">Replace the contents of the *Views/Home/Index.cshtml* file with the following:</span></span>

```html
<h1>Hello world!</h1>
```

<span data-ttu-id="f060d-158">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="f060d-158">Run the app.</span></span>

![Microsoft Edge で開いている web アプリ](mvc/_static/hello-world.png)

<span data-ttu-id="f060d-160">参照してください[コント ローラー](xref:mvc/controllers/actions)と[ビュー](xref:mvc/views/overview)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="f060d-160">See [Controllers](xref:mvc/controllers/actions) and [Views](xref:mvc/views/overview) for more information.</span></span>

<span data-ttu-id="f060d-161">最小限の作業用 ASP.NET Core プロジェクトが作成できた、ASP.NET MVC プロジェクトから機能の移行が始めることができます。</span><span class="sxs-lookup"><span data-stu-id="f060d-161">Now that we have a minimal working ASP.NET Core project, we can start migrating functionality from the ASP.NET MVC project.</span></span> <span data-ttu-id="f060d-162">次に移動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f060d-162">We need to move the following:</span></span>

* <span data-ttu-id="f060d-163">クライアント側のコンテンツ (CSS、フォント、およびスクリプト)</span><span class="sxs-lookup"><span data-stu-id="f060d-163">client-side content (CSS, fonts, and scripts)</span></span>

* <span data-ttu-id="f060d-164">コントローラー</span><span class="sxs-lookup"><span data-stu-id="f060d-164">controllers</span></span>

* <span data-ttu-id="f060d-165">ビュー</span><span class="sxs-lookup"><span data-stu-id="f060d-165">views</span></span>

* <span data-ttu-id="f060d-166">モデル</span><span class="sxs-lookup"><span data-stu-id="f060d-166">models</span></span>

* <span data-ttu-id="f060d-167">バンドル</span><span class="sxs-lookup"><span data-stu-id="f060d-167">bundling</span></span>

* <span data-ttu-id="f060d-168">フィルター</span><span class="sxs-lookup"><span data-stu-id="f060d-168">filters</span></span>

* <span data-ttu-id="f060d-169">入力/出力のログ、Identity (これは次のチュートリアルです。)</span><span class="sxs-lookup"><span data-stu-id="f060d-169">Log in/out, Identity (This is done in the next tutorial.)</span></span>

## <a name="controllers-and-views"></a><span data-ttu-id="f060d-170">コント ローラーとビュー</span><span class="sxs-lookup"><span data-stu-id="f060d-170">Controllers and views</span></span>

* <span data-ttu-id="f060d-171">ASP.NET MVC からの各メソッドをコピー`HomeController`を新しい`HomeController`します。</span><span class="sxs-lookup"><span data-stu-id="f060d-171">Copy each of the methods from the ASP.NET MVC `HomeController` to the new `HomeController`.</span></span> <span data-ttu-id="f060d-172">ASP.NET MVC では、組み込みのテンプレートのコント ローラー アクション メソッドの戻り型は[ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); ASP.NET Core MVC、アクション メソッドの戻り値で`IActionResult`代わりにします。</span><span class="sxs-lookup"><span data-stu-id="f060d-172">Note that in ASP.NET MVC, the built-in template's controller action method return type is [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC, the action methods return `IActionResult` instead.</span></span> <span data-ttu-id="f060d-173">`ActionResult` 実装`IActionResult`では、アクション メソッドの戻り値の型を変更する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="f060d-173">`ActionResult` implements `IActionResult`, so there's no need to change the return type of your action methods.</span></span>

* <span data-ttu-id="f060d-174">コピー、 *About.cshtml*、 *Contact.cshtml*、および*Index.cshtml* Razor ビューのファイルを ASP.NET MVC プロジェクトから ASP.NET Core プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="f060d-174">Copy the *About.cshtml*, *Contact.cshtml*, and *Index.cshtml* Razor view files from the ASP.NET MVC project to the ASP.NET Core project.</span></span>

* <span data-ttu-id="f060d-175">ASP.NET Core アプリを実行し、各メソッドをテストします。</span><span class="sxs-lookup"><span data-stu-id="f060d-175">Run the ASP.NET Core app and test each method.</span></span> <span data-ttu-id="f060d-176">いない移行ファイルのレイアウトやスタイル、まだ、レンダリングされるビューには、ビュー ファイルのコンテンツのみが含まれるようにします。</span><span class="sxs-lookup"><span data-stu-id="f060d-176">We haven't migrated the layout file or styles yet, so the rendered views only contain the content in the view files.</span></span> <span data-ttu-id="f060d-177">リンクをレイアウト ファイルを生成する必要はありません、`About`と`Contact`ビュー、ブラウザーから呼び出すことがあるありますので (置換**4492**プロジェクトで使用されるポート番号を使用)。</span><span class="sxs-lookup"><span data-stu-id="f060d-177">You won't have the layout file generated links for the `About` and `Contact` views, so you'll have to invoke them from the browser (replace **4492** with the port number used in your project).</span></span>

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![連絡先のページ](mvc/_static/contact-page.png)

<span data-ttu-id="f060d-179">スタイルとメニュー項目がないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="f060d-179">Note the lack of styling and menu items.</span></span> <span data-ttu-id="f060d-180">次のセクションでこれを修正します。</span><span class="sxs-lookup"><span data-stu-id="f060d-180">We'll fix that in the next section.</span></span>

## <a name="static-content"></a><span data-ttu-id="f060d-181">静的コンテンツ</span><span class="sxs-lookup"><span data-stu-id="f060d-181">Static content</span></span>

<span data-ttu-id="f060d-182">ASP.NET MVC の以前のバージョンでは、静的コンテンツは、web プロジェクトのルートからホストされているし、サーバー側のファイルが個です。</span><span class="sxs-lookup"><span data-stu-id="f060d-182">In previous versions of ASP.NET MVC, static content was hosted from the root of the web project and was intermixed with server-side files.</span></span> <span data-ttu-id="f060d-183">ASP.NET Core で静的コンテンツがホストされている、 *wwwroot*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="f060d-183">In ASP.NET Core, static content is hosted in the *wwwroot* folder.</span></span> <span data-ttu-id="f060d-184">以前の ASP.NET MVC アプリから静的コンテンツをコピーする必要あります、 *wwwroot* ASP.NET Core プロジェクトのフォルダー。</span><span class="sxs-lookup"><span data-stu-id="f060d-184">You'll want to copy the static content from your old ASP.NET MVC app to the *wwwroot* folder in your ASP.NET Core project.</span></span> <span data-ttu-id="f060d-185">このサンプルの変換。</span><span class="sxs-lookup"><span data-stu-id="f060d-185">In this sample conversion:</span></span>

* <span data-ttu-id="f060d-186">コピー、*追加すると*に古い MVC プロジェクトのファイル、 *wwwroot* ASP.NET Core プロジェクトのフォルダー。</span><span class="sxs-lookup"><span data-stu-id="f060d-186">Copy the *favicon.ico* file from the old MVC project to the *wwwroot* folder in the ASP.NET Core project.</span></span>

<span data-ttu-id="f060d-187">ASP.NET MVC、古いプロジェクトは[ブートス トラップ](https://getbootstrap.com/)にファイルをブートス トラップのスタイルとストアの*コンテンツ*と*スクリプト*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="f060d-187">The old ASP.NET MVC project uses [Bootstrap](https://getbootstrap.com/) for its styling and stores the Bootstrap files in the *Content* and *Scripts* folders.</span></span> <span data-ttu-id="f060d-188">テンプレートで、以前の ASP.NET MVC プロジェクトを生成するには、これを参照して、レイアウト ファイルでブートス トラップ (*Views/Shared/_Layout.cshtml*)。</span><span class="sxs-lookup"><span data-stu-id="f060d-188">The template, which generated the old ASP.NET MVC project, references Bootstrap in the layout file (*Views/Shared/_Layout.cshtml*).</span></span> <span data-ttu-id="f060d-189">コピーすることで、 *bootstrap.js*と*bootstrap.css*ファイルから ASP.NET MVC プロジェクトを*wwwroot*新しいプロジェクトのフォルダー。</span><span class="sxs-lookup"><span data-stu-id="f060d-189">You could copy the *bootstrap.js* and *bootstrap.css* files from the ASP.NET MVC project to the *wwwroot* folder in the new project.</span></span> <span data-ttu-id="f060d-190">代わりに、次のセクションで Cdn を使用して、ブートス トラップのサポート (およびその他のクライアント側ライブラリ) を追加します。</span><span class="sxs-lookup"><span data-stu-id="f060d-190">Instead, we'll add support for Bootstrap (and other client-side libraries) using CDNs in the next section.</span></span>

## <a name="migrate-the-layout-file"></a><span data-ttu-id="f060d-191">レイアウト ファイルを移行します。</span><span class="sxs-lookup"><span data-stu-id="f060d-191">Migrate the layout file</span></span>

* <span data-ttu-id="f060d-192">コピー、 *_ViewStart.cshtml*ファイルから以前の ASP.NET MVC プロジェクトの*ビュー*に ASP.NET Core プロジェクトのフォルダー*ビュー*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="f060d-192">Copy the *_ViewStart.cshtml* file from the old ASP.NET MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="f060d-193">*_ViewStart.cshtml* ASP.NET Core MVC でファイルが変更されていません。</span><span class="sxs-lookup"><span data-stu-id="f060d-193">The *_ViewStart.cshtml* file has not changed in ASP.NET Core MVC.</span></span>

* <span data-ttu-id="f060d-194">作成、 *Views/shared*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="f060d-194">Create a *Views/Shared* folder.</span></span>

* <span data-ttu-id="f060d-195">*省略可能。* コピー *_ViewImports.cshtml*から、 *FullAspNetCore* MVC プロジェクトの*ビュー*に ASP.NET Core プロジェクトのフォルダー*ビュー*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="f060d-195">*Optional:* Copy *_ViewImports.cshtml* from the *FullAspNetCore* MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="f060d-196">任意の名前空間宣言を削除、 *_ViewImports.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="f060d-196">Remove any namespace declaration in the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="f060d-197">*_ViewImports.cshtml*ファイルは、すべてのビュー ファイルの名前空間を提供しに移行する[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)します。</span><span class="sxs-lookup"><span data-stu-id="f060d-197">The *_ViewImports.cshtml* file provides namespaces for all the view files and brings in [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="f060d-198">タグ ヘルパーは、新しいレイアウト ファイルで使用されます。</span><span class="sxs-lookup"><span data-stu-id="f060d-198">Tag Helpers are used in the new layout file.</span></span> <span data-ttu-id="f060d-199">*_ViewImports.cshtml*ファイルは ASP.NET Core の新機能です。</span><span class="sxs-lookup"><span data-stu-id="f060d-199">The *_ViewImports.cshtml* file is new for ASP.NET Core.</span></span>

* <span data-ttu-id="f060d-200">コピー、 *_Layout.cshtml*ファイルから以前の ASP.NET MVC プロジェクトの*Views/shared*に ASP.NET Core プロジェクトのフォルダー *Views/shared*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="f060d-200">Copy the *_Layout.cshtml* file from the old ASP.NET MVC project's *Views/Shared* folder into the ASP.NET Core project's *Views/Shared* folder.</span></span>

<span data-ttu-id="f060d-201">開いている *_Layout.cshtml*ファイルを開き、次の変更 (完成したコードは下記参照)。</span><span class="sxs-lookup"><span data-stu-id="f060d-201">Open *_Layout.cshtml* file and make the following changes (the completed code is shown below):</span></span>

* <span data-ttu-id="f060d-202">置換`@Styles.Render("~/Content/css")`で、`<link>`要素を読み込む*bootstrap.css* (下記参照)。</span><span class="sxs-lookup"><span data-stu-id="f060d-202">Replace `@Styles.Render("~/Content/css")` with a `<link>` element to load *bootstrap.css* (see below).</span></span>

* <span data-ttu-id="f060d-203">`@Scripts.Render("~/bundles/modernizr")` を削除します。</span><span class="sxs-lookup"><span data-stu-id="f060d-203">Remove `@Scripts.Render("~/bundles/modernizr")`.</span></span>

* <span data-ttu-id="f060d-204">コメント アウト、`@Html.Partial("_LoginPartial")`行 (では、行を囲む`@*...*@`)。</span><span class="sxs-lookup"><span data-stu-id="f060d-204">Comment out the `@Html.Partial("_LoginPartial")` line (surround the line with `@*...*@`).</span></span> <span data-ttu-id="f060d-205">詳細については、次を参照してください[認証の移行と ASP.NET core Id。](xref:migration/identity)</span><span class="sxs-lookup"><span data-stu-id="f060d-205">For more information see [Migrate Authentication and Identity to ASP.NET Core](xref:migration/identity)</span></span>

* <span data-ttu-id="f060d-206">置換`@Scripts.Render("~/bundles/jquery")`で、`<script>`要素 (下記参照)。</span><span class="sxs-lookup"><span data-stu-id="f060d-206">Replace `@Scripts.Render("~/bundles/jquery")` with a `<script>` element (see below).</span></span>

* <span data-ttu-id="f060d-207">置換`@Scripts.Render("~/bundles/bootstrap")`で、`<script>`要素 (下記参照)。</span><span class="sxs-lookup"><span data-stu-id="f060d-207">Replace `@Scripts.Render("~/bundles/bootstrap")` with a `<script>` element (see below).</span></span>

<span data-ttu-id="f060d-208">ブートス トラップ CSS 含める置換マークアップ:</span><span class="sxs-lookup"><span data-stu-id="f060d-208">The replacement markup for Bootstrap CSS inclusion:</span></span>

```html
<link rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"
    integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
    crossorigin="anonymous">
```

<span data-ttu-id="f060d-209">JQuery と JavaScript のブートス トラップの包含の置換マークアップ:</span><span class="sxs-lookup"><span data-stu-id="f060d-209">The replacement markup for jQuery and Bootstrap JavaScript inclusion:</span></span>

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"
    integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
```

<span data-ttu-id="f060d-210">更新された *_Layout.cshtml*ファイルを次に示します。</span><span class="sxs-lookup"><span data-stu-id="f060d-210">The updated *_Layout.cshtml* file is shown below:</span></span>

[!code-cshtml[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7-10,29,41-44)]

<span data-ttu-id="f060d-211">ブラウザーでサイトを表示します。</span><span class="sxs-lookup"><span data-stu-id="f060d-211">View the site in the browser.</span></span> <span data-ttu-id="f060d-212">それを場所に必要なスタイルを使用して正しく読み込むようになりましたする必要があります。</span><span class="sxs-lookup"><span data-stu-id="f060d-212">It should now load correctly, with the expected styles in place.</span></span>

* <span data-ttu-id="f060d-213">*省略可能。* 新しいレイアウト ファイルを使用してお試しくださいする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="f060d-213">*Optional:* You might want to try using the new layout file.</span></span> <span data-ttu-id="f060d-214">このプロジェクトからレイアウト ファイルをコピーすることができます、 *FullAspNetCore*プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="f060d-214">For this project you can copy the layout file from the *FullAspNetCore* project.</span></span> <span data-ttu-id="f060d-215">新しいレイアウト ファイルを使用して[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)あり、その他の改善。</span><span class="sxs-lookup"><span data-stu-id="f060d-215">The new layout file uses [Tag Helpers](xref:mvc/views/tag-helpers/intro) and has other improvements.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="f060d-216">バンドルと縮小を構成します。</span><span class="sxs-lookup"><span data-stu-id="f060d-216">Configure bundling and minification</span></span>

<span data-ttu-id="f060d-217">バンドルと縮小を構成する方法については、次を参照してください。[バンドルと縮小](../client-side/bundling-and-minification.md)します。</span><span class="sxs-lookup"><span data-stu-id="f060d-217">For information about how to configure bundling and minification, see [Bundling and Minification](../client-side/bundling-and-minification.md).</span></span>

## <a name="solve-http-500-errors"></a><span data-ttu-id="f060d-218">HTTP 500 エラーを解決します。</span><span class="sxs-lookup"><span data-stu-id="f060d-218">Solve HTTP 500 errors</span></span>

<span data-ttu-id="f060d-219">多くの問題、HTTP 500 エラー メッセージを引き起こす可能性のある問題の原因に関する情報を含まないがあります。</span><span class="sxs-lookup"><span data-stu-id="f060d-219">There are many problems that can cause a HTTP 500 error message that contain no information on the source of the problem.</span></span> <span data-ttu-id="f060d-220">たとえば場合、 *Views/_ViewImports.cshtml*ファイルがプロジェクトに存在しない名前空間を含む、HTTP 500 エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f060d-220">For example, if the *Views/_ViewImports.cshtml* file contains a namespace that doesn't exist in your project, you'll get a HTTP 500 error.</span></span> <span data-ttu-id="f060d-221">既定では、ASP.NET Core アプリで、`UseDeveloperExceptionPage`に拡張子を追加、`IApplicationBuilder`構成が実行されると*開発*します。</span><span class="sxs-lookup"><span data-stu-id="f060d-221">By default in ASP.NET Core apps, the `UseDeveloperExceptionPage` extension is added to the `IApplicationBuilder` and executed when the configuration is *Development*.</span></span> <span data-ttu-id="f060d-222">これは、次のコードの詳細を示します。</span><span class="sxs-lookup"><span data-stu-id="f060d-222">This is detailed in the following code:</span></span>

[!code-csharp[](mvc/sample/Startup.cs?highlight=19-22)]

<span data-ttu-id="f060d-223">ASP.NET Core は、web アプリで未処理の例外を HTTP 500 エラー応答に変換します。</span><span class="sxs-lookup"><span data-stu-id="f060d-223">ASP.NET Core converts unhandled exceptions in a web app into HTTP 500 error responses.</span></span> <span data-ttu-id="f060d-224">通常、エラーの詳細は、サーバーに関する機密情報の漏えいを防ぐためにこれらの応答に含まれません。</span><span class="sxs-lookup"><span data-stu-id="f060d-224">Normally, error details aren't included in these responses to prevent disclosure of potentially sensitive information about the server.</span></span> <span data-ttu-id="f060d-225">参照してください**開発者例外ページを使用して**で[エラー処理](../fundamentals/error-handling.md)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="f060d-225">See **Using the Developer Exception Page** in [Handle errors](../fundamentals/error-handling.md) for more information.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f060d-226">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="f060d-226">Additional resources</span></span>

* <xref:blazor/index>
* <xref:mvc/views/tag-helpers/intro>
