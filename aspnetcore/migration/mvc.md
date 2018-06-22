---
title: ASP.NET MVC から ASP.NET Core MVC への移行します。
author: ardalis
description: ASP.NET Core MVC に ASP.NET MVC プロジェクトを移行する作業を開始する方法を説明します。
ms.author: riande
ms.date: 03/07/2017
uid: migration/mvc
ms.openlocfilehash: 6fecc820177ca5033cfd00d632904a950c1b29bb
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274776"
---
# <a name="migrate-from-aspnet-mvc-to-aspnet-core-mvc"></a><span data-ttu-id="ed427-103">ASP.NET MVC から ASP.NET Core MVC への移行します。</span><span class="sxs-lookup"><span data-stu-id="ed427-103">Migrate from ASP.NET MVC to ASP.NET Core MVC</span></span>

<span data-ttu-id="ed427-104">によって[Rick Anderson](https://twitter.com/RickAndMSFT)、 [Daniel Roth](https://github.com/danroth27)、 [Steve Smith](https://ardalis.com/)、および[Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="ed427-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="ed427-105">この記事に ASP.NET MVC プロジェクトを移行する作業を開始する方法を示しています。 [ASP.NET Core MVC](../mvc/overview.md)です。</span><span class="sxs-lookup"><span data-stu-id="ed427-105">This article shows how to get started migrating an ASP.NET MVC project to [ASP.NET Core MVC](../mvc/overview.md).</span></span> <span data-ttu-id="ed427-106">プロセスでは、ASP.NET MVC から変更されたことの多くを強調表示します。</span><span class="sxs-lookup"><span data-stu-id="ed427-106">In the process, it highlights many of the things that have changed from ASP.NET MVC.</span></span> <span data-ttu-id="ed427-107">複数段階のプロセスは、ASP.NET MVC を移行して、この記事は、初期セットアップ、基本的なコント ローラーとビュー、静的なコンテンツ、およびクライアント側の依存関係について説明します。</span><span class="sxs-lookup"><span data-stu-id="ed427-107">Migrating from ASP.NET MVC is a multiple step process and this article covers the initial setup, basic controllers and views, static content, and client-side dependencies.</span></span> <span data-ttu-id="ed427-108">その他の記事では、構成を移行して多くの ASP.NET MVC プロジェクトで見つかった id コードについて説明します。</span><span class="sxs-lookup"><span data-stu-id="ed427-108">Additional articles cover migrating configuration and identity code found in many ASP.NET MVC projects.</span></span>

> [!NOTE]
> <span data-ttu-id="ed427-109">サンプルのバージョン番号が最新でない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="ed427-109">The version numbers in the samples might not be current.</span></span> <span data-ttu-id="ed427-110">したがって、プロジェクトを更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ed427-110">You may need to update your projects accordingly.</span></span>

## <a name="create-the-starter-aspnet-mvc-project"></a><span data-ttu-id="ed427-111">スターター ASP.NET MVC プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="ed427-111">Create the starter ASP.NET MVC project</span></span>

<span data-ttu-id="ed427-112">アップグレードを示すためには、ASP.NET MVC アプリケーションを作成することで始めます。</span><span class="sxs-lookup"><span data-stu-id="ed427-112">To demonstrate the upgrade, we'll start by creating a ASP.NET MVC app.</span></span> <span data-ttu-id="ed427-113">名前を使用して作成*WebApp1*ので、名前空間を作成する、次の手順に ASP.NET Core プロジェクトに一致します。</span><span class="sxs-lookup"><span data-stu-id="ed427-113">Create it with the name *WebApp1* so the namespace matches the ASP.NET Core project we create in the next step.</span></span>

![Visual Studio の新しいプロジェクト ダイアログ ボックス](mvc/_static/new-project.png)

![新しい Web アプリケーション ダイアログ: MVC プロジェクト テンプレートがテンプレートの ASP.NET のパネルで選択されています。](mvc/_static/new-project-select-mvc-template.png)

<span data-ttu-id="ed427-116">*省略可能:* からソリューションの名前を変更*WebApp1*に*Mvc5*です。</span><span class="sxs-lookup"><span data-stu-id="ed427-116">*Optional:* Change the name of the Solution from *WebApp1* to *Mvc5*.</span></span> <span data-ttu-id="ed427-117">Visual Studio には、新しいソリューション名が表示されます (*Mvc5*)、簡単に次のプロジェクトからこのプロジェクトに指示します。</span><span class="sxs-lookup"><span data-stu-id="ed427-117">Visual Studio displays the new solution name (*Mvc5*), which makes it easier to tell this project from the next project.</span></span>

## <a name="create-the-aspnet-core-project"></a><span data-ttu-id="ed427-118">ASP.NET Core プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="ed427-118">Create the ASP.NET Core project</span></span>

<span data-ttu-id="ed427-119">新規作成*空*前のプロジェクトと同じ名前での ASP.NET Core web アプリ (*WebApp1*) ため、2 つのプロジェクトの名前空間と一致します。</span><span class="sxs-lookup"><span data-stu-id="ed427-119">Create a new *empty* ASP.NET Core web app with the same name as the previous project (*WebApp1*) so the namespaces in the two projects match.</span></span> <span data-ttu-id="ed427-120">同じ名前空間を持つと、2 つのプロジェクト間でコードをコピーしやすきます。</span><span class="sxs-lookup"><span data-stu-id="ed427-120">Having the same namespace makes it easier to copy code between the two projects.</span></span> <span data-ttu-id="ed427-121">同じ名前を使用する前のプロジェクトとは異なるディレクトリでこのプロジェクトを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ed427-121">You'll have to create this project in a different directory than the previous project to use the same name.</span></span>

![[新しいプロジェクト] ダイアログ](mvc/_static/new_core.png)

![新しい ASP.NET Web アプリケーション ダイアログ ボックス: ASP.NET Core テンプレート パネルで選択した空のプロジェクト テンプレート](mvc/_static/new-project-select-empty-aspnet5-template.png)

* <span data-ttu-id="ed427-124">*省略可能:* 新しい ASP.NET Core アプリケーションを使用して、作成、 *Web アプリケーション*プロジェクト テンプレート。</span><span class="sxs-lookup"><span data-stu-id="ed427-124">*Optional:* Create a new ASP.NET Core app using the *Web Application* project template.</span></span> <span data-ttu-id="ed427-125">プロジェクトに名前を*WebApp1*、認証オプションを選択して**個々 のユーザー アカウント**です。</span><span class="sxs-lookup"><span data-stu-id="ed427-125">Name the project *WebApp1*, and select an authentication option of **Individual User Accounts**.</span></span> <span data-ttu-id="ed427-126">このアプリの名前を変更*FullAspNetCore*です。</span><span class="sxs-lookup"><span data-stu-id="ed427-126">Rename this app to *FullAspNetCore*.</span></span> <span data-ttu-id="ed427-127">変換では、このプロジェクト、時間の節約を作成します。</span><span class="sxs-lookup"><span data-stu-id="ed427-127">Creating this project saves you time in the conversion.</span></span> <span data-ttu-id="ed427-128">最終的な結果を表示する、または変換プロジェクトにコードをコピーするテンプレートによって生成されたコードを調べることができます。</span><span class="sxs-lookup"><span data-stu-id="ed427-128">You can look at the template-generated code to see the end result or to copy code to the conversion project.</span></span> <span data-ttu-id="ed427-129">テンプレートによって生成されたプロジェクトと比較するへの変換ステップに行き詰まった場合にも便利です。</span><span class="sxs-lookup"><span data-stu-id="ed427-129">It's also helpful when you get stuck on a conversion step to compare with the template-generated project.</span></span>

## <a name="configure-the-site-to-use-mvc"></a><span data-ttu-id="ed427-130">MVC を使用するサイトを構成します。</span><span class="sxs-lookup"><span data-stu-id="ed427-130">Configure the site to use MVC</span></span>

* <span data-ttu-id="ed427-131">.NET Core をターゲットとすると、ASP.NET Core metapackage と呼ばれる、プロジェクトに追加`Microsoft.AspNetCore.All`既定です。</span><span class="sxs-lookup"><span data-stu-id="ed427-131">When targeting .NET Core, the ASP.NET Core metapackage is added to the project, called `Microsoft.AspNetCore.All` by default.</span></span> <span data-ttu-id="ed427-132">このパッケージを含むようにパッケージ`Microsoft.AspNetCore.Mvc`と`Microsoft.AspNetCore.StaticFiles`です。</span><span class="sxs-lookup"><span data-stu-id="ed427-132">This package contains packages like `Microsoft.AspNetCore.Mvc` and `Microsoft.AspNetCore.StaticFiles`.</span></span> <span data-ttu-id="ed427-133">.NET Framework を対象にする場合、パッケージの参照は \*.csproj ファイルに個別に一覧表示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ed427-133">If targeting .NET Framework, package references need to be listed individually in the \*.csproj file.</span></span>

<span data-ttu-id="ed427-134">`Microsoft.AspNetCore.Mvc` ASP.NET Core MVC フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="ed427-134">`Microsoft.AspNetCore.Mvc` is the ASP.NET Core MVC framework.</span></span> <span data-ttu-id="ed427-135">`Microsoft.AspNetCore.StaticFiles` 静的ファイル ハンドラーです。</span><span class="sxs-lookup"><span data-stu-id="ed427-135">`Microsoft.AspNetCore.StaticFiles` is the static file handler.</span></span> <span data-ttu-id="ed427-136">ASP.NET Core ランタイムがモジュール、および静的なファイルが機能するように明示的にオプトインする必要があります (を参照してください[静的ファイル](xref:fundamentals/static-files))。</span><span class="sxs-lookup"><span data-stu-id="ed427-136">The ASP.NET Core runtime is modular, and you must explicitly opt in to serve static files (see [Static files](xref:fundamentals/static-files)).</span></span>

* <span data-ttu-id="ed427-137">開く、 *Startup.cs*ファイルを開き、次の一致するようにコードを変更します。</span><span class="sxs-lookup"><span data-stu-id="ed427-137">Open the *Startup.cs* file and change the code to match the following:</span></span>

  [!code-csharp[](mvc/sample/Startup.cs?highlight=13,26-31)]

<span data-ttu-id="ed427-138">`UseStaticFiles`拡張メソッドが静的ファイル ハンドラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="ed427-138">The `UseStaticFiles` extension method adds the static file handler.</span></span> <span data-ttu-id="ed427-139">前述のように、ASP.NET ランタイムは、モジュール、および静的なファイルが機能するように明示的にオプトインする必要があります。</span><span class="sxs-lookup"><span data-stu-id="ed427-139">As mentioned previously, the ASP.NET runtime is modular, and you must explicitly opt in to serve static files.</span></span> <span data-ttu-id="ed427-140">`UseMvc`ルーティング拡張メソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="ed427-140">The `UseMvc` extension method adds routing.</span></span> <span data-ttu-id="ed427-141">詳細については、次を参照してください。[アプリケーションの起動](xref:fundamentals/startup)と[ルーティング](xref:fundamentals/routing)です。</span><span class="sxs-lookup"><span data-stu-id="ed427-141">For more information, see [Application Startup](xref:fundamentals/startup) and [Routing](xref:fundamentals/routing).</span></span>

## <a name="add-a-controller-and-view"></a><span data-ttu-id="ed427-142">コント ローラーとビューを追加します。</span><span class="sxs-lookup"><span data-stu-id="ed427-142">Add a controller and view</span></span>

<span data-ttu-id="ed427-143">このセクションでは、最小限のコント ローラーと ASP.NET MVC コント ローラーのプレース ホルダーとして機能するようにビューとビューの次のセクションで移行しますを追加します。</span><span class="sxs-lookup"><span data-stu-id="ed427-143">In this section, you'll add a minimal controller and view to serve as placeholders for the ASP.NET MVC controller and views you'll migrate in the next section.</span></span>

* <span data-ttu-id="ed427-144">追加、*コント ローラー*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="ed427-144">Add a *Controllers* folder.</span></span>

* <span data-ttu-id="ed427-145">追加、**コント ローラー クラス**という*HomeController.cs*を*コント ローラー*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="ed427-145">Add a **Controller Class** named *HomeController.cs* to the *Controllers* folder.</span></span>

![[新しい項目の追加] ダイアログ](mvc/_static/add_mvc_ctl.png)

* <span data-ttu-id="ed427-147">追加、*ビュー*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="ed427-147">Add a *Views* folder.</span></span>

* <span data-ttu-id="ed427-148">追加、*ビュー/ホーム*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="ed427-148">Add a *Views/Home* folder.</span></span>

* <span data-ttu-id="ed427-149">追加、 **Razor ビュー**という*Index.cshtml*を*ビュー/ホーム*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="ed427-149">Add a **Razor View** named *Index.cshtml* to the *Views/Home* folder.</span></span>

![[新しい項目の追加] ダイアログ](mvc/_static/view.png)

<span data-ttu-id="ed427-151">プロジェクトの構造を次に示します。</span><span class="sxs-lookup"><span data-stu-id="ed427-151">The project structure is shown below:</span></span>

![ソリューション エクスプ ローラー WebApp1 のファイルとフォルダーの表示](mvc/_static/project-structure-controller-view.png)

<span data-ttu-id="ed427-153">内容を置き換える、 *Views/Home/Index.cshtml*を次のファイル。</span><span class="sxs-lookup"><span data-stu-id="ed427-153">Replace the contents of the *Views/Home/Index.cshtml* file with the following:</span></span>

```html
<h1>Hello world!</h1>
```

<span data-ttu-id="ed427-154">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="ed427-154">Run the app.</span></span>

![Web アプリを Microsoft Edge で開いた状態](mvc/_static/hello-world.png)

<span data-ttu-id="ed427-156">参照してください[コント ローラー](xref:mvc/controllers/actions)と[ビュー](xref:mvc/views/overview)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="ed427-156">See [Controllers](xref:mvc/controllers/actions) and [Views](xref:mvc/views/overview) for more information.</span></span>

<span data-ttu-id="ed427-157">最小限の作業用 ASP.NET Core プロジェクトができました、ASP.NET MVC プロジェクトから機能の移行が始めることができます。</span><span class="sxs-lookup"><span data-stu-id="ed427-157">Now that we have a minimal working ASP.NET Core project, we can start migrating functionality from the ASP.NET MVC project.</span></span> <span data-ttu-id="ed427-158">次に移動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ed427-158">We need to move the following:</span></span>

* <span data-ttu-id="ed427-159">クライアント側のコンテンツ (CSS、フォント、およびスクリプト)</span><span class="sxs-lookup"><span data-stu-id="ed427-159">client-side content (CSS, fonts, and scripts)</span></span>

* <span data-ttu-id="ed427-160">コントローラー</span><span class="sxs-lookup"><span data-stu-id="ed427-160">controllers</span></span>

* <span data-ttu-id="ed427-161">ビュー</span><span class="sxs-lookup"><span data-stu-id="ed427-161">views</span></span>

* <span data-ttu-id="ed427-162">モデル</span><span class="sxs-lookup"><span data-stu-id="ed427-162">models</span></span>

* <span data-ttu-id="ed427-163">バンドル化</span><span class="sxs-lookup"><span data-stu-id="ed427-163">bundling</span></span>

* <span data-ttu-id="ed427-164">フィルター</span><span class="sxs-lookup"><span data-stu-id="ed427-164">filters</span></span>

* <span data-ttu-id="ed427-165">ログの/out、Id (これは、次のチュートリアルでします。)</span><span class="sxs-lookup"><span data-stu-id="ed427-165">Log in/out, Identity (This is done in the next tutorial.)</span></span>

## <a name="controllers-and-views"></a><span data-ttu-id="ed427-166">コント ローラーとビュー</span><span class="sxs-lookup"><span data-stu-id="ed427-166">Controllers and views</span></span>

* <span data-ttu-id="ed427-167">ASP.NET MVC のコピーの各メソッド`HomeController`を新しい`HomeController`です。</span><span class="sxs-lookup"><span data-stu-id="ed427-167">Copy each of the methods from the ASP.NET MVC `HomeController` to the new `HomeController`.</span></span> <span data-ttu-id="ed427-168">ASP.NET MVC では、組み込みのテンプレートのコント ローラー アクション メソッドの戻り値型は[ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx)以外の場合は ASP.NET Core MVC、アクション メソッドの戻り値の`IActionResult`代わりにします。</span><span class="sxs-lookup"><span data-stu-id="ed427-168">Note that in ASP.NET MVC, the built-in template's controller action method return type is [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC, the action methods return `IActionResult` instead.</span></span> <span data-ttu-id="ed427-169">`ActionResult` 実装する`IActionResult`ので、アクション メソッドの戻り値の型を変更する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="ed427-169">`ActionResult` implements `IActionResult`, so there's no need to change the return type of your action methods.</span></span>

* <span data-ttu-id="ed427-170">コピー、 *About.cshtml*、 *Contact.cshtml*、および*Index.cshtml* Razor ビューのファイルを ASP.NET MVC プロジェクトから ASP.NET Core プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="ed427-170">Copy the *About.cshtml*, *Contact.cshtml*, and *Index.cshtml* Razor view files from the ASP.NET MVC project to the ASP.NET Core project.</span></span>

* <span data-ttu-id="ed427-171">ASP.NET Core アプリケーションを実行し、各メソッドをテストします。</span><span class="sxs-lookup"><span data-stu-id="ed427-171">Run the ASP.NET Core app and test each method.</span></span> <span data-ttu-id="ed427-172">いない移行ファイルのレイアウトやスタイルまだ、レンダリングされるビューには、ファイルの表示内のコンテンツのみが含まれるようにします。</span><span class="sxs-lookup"><span data-stu-id="ed427-172">We haven't migrated the layout file or styles yet, so the rendered views only contain the content in the view files.</span></span> <span data-ttu-id="ed427-173">レイアウトに生成されたファイルへのリンクする必要はありません、`About`と`Contact`ビュー、ブラウザーからそれらを起動する必要がありますので (交換**4492**プロジェクトで使用されるポート番号を持つ)。</span><span class="sxs-lookup"><span data-stu-id="ed427-173">You won't have the layout file generated links for the `About` and `Contact` views, so you'll have to invoke them from the browser (replace **4492** with the port number used in your project).</span></span>

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![連絡先のページ](mvc/_static/contact-page.png)

<span data-ttu-id="ed427-175">スタイル設定およびメニュー項目がないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="ed427-175">Note the lack of styling and menu items.</span></span> <span data-ttu-id="ed427-176">次のセクションでこれを修正します。</span><span class="sxs-lookup"><span data-stu-id="ed427-176">We'll fix that in the next section.</span></span>

## <a name="static-content"></a><span data-ttu-id="ed427-177">静的コンテンツ</span><span class="sxs-lookup"><span data-stu-id="ed427-177">Static content</span></span>

<span data-ttu-id="ed427-178">ASP.NET MVC の以前のバージョンでは、静的コンテンツは、web プロジェクトのルートからホストされているし、サーバー側のファイルが混合されました。</span><span class="sxs-lookup"><span data-stu-id="ed427-178">In previous versions of ASP.NET MVC, static content was hosted from the root of the web project and was intermixed with server-side files.</span></span> <span data-ttu-id="ed427-179">ASP.NET Core での静的なコンテンツをホスト、 *wwwroot*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="ed427-179">In ASP.NET Core, static content is hosted in the *wwwroot* folder.</span></span> <span data-ttu-id="ed427-180">古い ASP.NET MVC のアプリから静的なコンテンツをコピーしておく、 *wwwroot* ASP.NET Core プロジェクトのフォルダーにします。</span><span class="sxs-lookup"><span data-stu-id="ed427-180">You'll want to copy the static content from your old ASP.NET MVC app to the *wwwroot* folder in your ASP.NET Core project.</span></span> <span data-ttu-id="ed427-181">このサンプルの変換。</span><span class="sxs-lookup"><span data-stu-id="ed427-181">In this sample conversion:</span></span>

* <span data-ttu-id="ed427-182">コピー、 *favicon.ico*に古い MVC プロジェクトのファイル、 *wwwroot* ASP.NET Core プロジェクト フォルダーにします。</span><span class="sxs-lookup"><span data-stu-id="ed427-182">Copy the *favicon.ico* file from the old MVC project to the *wwwroot* folder in the ASP.NET Core project.</span></span>

<span data-ttu-id="ed427-183">古い ASP.NET MVC プロジェクトの使用[ブートス トラップ](https://getbootstrap.com/)にファイルをブートス トラップのスタイルとストアの*コンテンツ*と*スクリプト*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="ed427-183">The old ASP.NET MVC project uses [Bootstrap](https://getbootstrap.com/) for its styling and stores the Bootstrap files in the *Content* and *Scripts* folders.</span></span> <span data-ttu-id="ed427-184">以前の ASP.NET MVC プロジェクトを生成するには、テンプレートを参照して、レイアウト ファイル ブートス トラップ (*Views/Shared/_Layout.cshtml*)。</span><span class="sxs-lookup"><span data-stu-id="ed427-184">The template, which generated the old ASP.NET MVC project, references Bootstrap in the layout file (*Views/Shared/_Layout.cshtml*).</span></span> <span data-ttu-id="ed427-185">コピーすることで、 *bootstrap.js*と*bootstrap.css*ファイルから ASP.NET MVC プロジェクトを*wwwroot*新しいプロジェクトのフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="ed427-185">You could copy the *bootstrap.js* and *bootstrap.css* files from the ASP.NET MVC project to the *wwwroot* folder in the new project.</span></span> <span data-ttu-id="ed427-186">代わりに、次のセクションで Cdn を使用してブートス トラップのサポート (およびその他のクライアント側ライブラリ) を追加します。</span><span class="sxs-lookup"><span data-stu-id="ed427-186">Instead, we'll add support for Bootstrap (and other client-side libraries) using CDNs in the next section.</span></span>

## <a name="migrate-the-layout-file"></a><span data-ttu-id="ed427-187">レイアウト ファイルを移行します。</span><span class="sxs-lookup"><span data-stu-id="ed427-187">Migrate the layout file</span></span>

* <span data-ttu-id="ed427-188">コピー、*は _viewstart.vbhtml*古い ASP.NET MVC プロジェクトのファイルの*ビュー* ASP.NET Core プロジェクトのフォルダー*ビュー*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="ed427-188">Copy the *_ViewStart.cshtml* file from the old ASP.NET MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="ed427-189">*は _viewstart.vbhtml* ASP.NET Core MVC でファイルが変更されていません。</span><span class="sxs-lookup"><span data-stu-id="ed427-189">The *_ViewStart.cshtml* file has not changed in ASP.NET Core MVC.</span></span>

* <span data-ttu-id="ed427-190">作成、 *Views/shared*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="ed427-190">Create a *Views/Shared* folder.</span></span>

* <span data-ttu-id="ed427-191">*省略可能:* コピー *_ViewImports.cshtml*から、 *FullAspNetCore* MVC プロジェクトの*ビュー* ASP.NET Core プロジェクトのフォルダー *ビュー*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="ed427-191">*Optional:* Copy *_ViewImports.cshtml* from the *FullAspNetCore* MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="ed427-192">任意の名前空間宣言を削除、 *_ViewImports.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="ed427-192">Remove any namespace declaration in the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="ed427-193">*_ViewImports.cshtml*ファイル ビューのすべてのファイルの名前空間を提供しでによってもたらさ[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)です。</span><span class="sxs-lookup"><span data-stu-id="ed427-193">The *_ViewImports.cshtml* file provides namespaces for all the view files and brings in [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="ed427-194">タグ ヘルパーは、新しいレイアウト ファイルで使用されます。</span><span class="sxs-lookup"><span data-stu-id="ed427-194">Tag Helpers are used in the new layout file.</span></span> <span data-ttu-id="ed427-195">*_ViewImports.cshtml*ファイルが ASP.NET Core の新機能です。</span><span class="sxs-lookup"><span data-stu-id="ed427-195">The *_ViewImports.cshtml* file is new for ASP.NET Core.</span></span>

* <span data-ttu-id="ed427-196">コピー、 *_Layout.cshtml*古い ASP.NET MVC プロジェクトのファイルの*Views/shared* ASP.NET Core プロジェクトのフォルダー *Views/shared*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="ed427-196">Copy the *_Layout.cshtml* file from the old ASP.NET MVC project's *Views/Shared* folder into the ASP.NET Core project's *Views/Shared* folder.</span></span>

<span data-ttu-id="ed427-197">開いている *_Layout.cshtml*ファイルし、次の変更 (完了したコードは、以下のとおりです)。</span><span class="sxs-lookup"><span data-stu-id="ed427-197">Open *_Layout.cshtml* file and make the following changes (the completed code is shown below):</span></span>

* <span data-ttu-id="ed427-198">置き換える`@Styles.Render("~/Content/css")`で、`<link>`読み込み要素*bootstrap.css* (下記参照)。</span><span class="sxs-lookup"><span data-stu-id="ed427-198">Replace `@Styles.Render("~/Content/css")` with a `<link>` element to load *bootstrap.css* (see below).</span></span>

* <span data-ttu-id="ed427-199">`@Scripts.Render("~/bundles/modernizr")` を削除します。</span><span class="sxs-lookup"><span data-stu-id="ed427-199">Remove `@Scripts.Render("~/bundles/modernizr")`.</span></span>

* <span data-ttu-id="ed427-200">コメント アウト、`@Html.Partial("_LoginPartial")`行 (では、行を囲む`@*...*@`)。</span><span class="sxs-lookup"><span data-stu-id="ed427-200">Comment out the `@Html.Partial("_LoginPartial")` line (surround the line with `@*...*@`).</span></span> <span data-ttu-id="ed427-201">今後のチュートリアルでを返します。</span><span class="sxs-lookup"><span data-stu-id="ed427-201">We'll return to it in a future tutorial.</span></span>

* <span data-ttu-id="ed427-202">置き換える`@Scripts.Render("~/bundles/jquery")`で、`<script>`要素 (下記参照)。</span><span class="sxs-lookup"><span data-stu-id="ed427-202">Replace `@Scripts.Render("~/bundles/jquery")` with a `<script>` element (see below).</span></span>

* <span data-ttu-id="ed427-203">置き換える`@Scripts.Render("~/bundles/bootstrap")`で、`<script>`要素 (下記参照)。</span><span class="sxs-lookup"><span data-stu-id="ed427-203">Replace `@Scripts.Render("~/bundles/bootstrap")` with a `<script>` element (see below).</span></span>

<span data-ttu-id="ed427-204">ブートス トラップ CSS 含める置換マークアップ:</span><span class="sxs-lookup"><span data-stu-id="ed427-204">The replacement markup for Bootstrap CSS inclusion:</span></span>

```html
<link rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"
    integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
    crossorigin="anonymous">
```

<span data-ttu-id="ed427-205">交換用のマークアップ jQuery とブートス トラップの JavaScript の包含を:</span><span class="sxs-lookup"><span data-stu-id="ed427-205">The replacement markup for jQuery and Bootstrap JavaScript inclusion:</span></span>

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"
    integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
```

<span data-ttu-id="ed427-206">更新された *_Layout.cshtml*ファイルを次に示します。</span><span class="sxs-lookup"><span data-stu-id="ed427-206">The updated *_Layout.cshtml* file is shown below:</span></span>

[!code-cshtml[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7-10,29,41-44)]

<span data-ttu-id="ed427-207">ブラウザーで、サイトを表示します。</span><span class="sxs-lookup"><span data-stu-id="ed427-207">View the site in the browser.</span></span> <span data-ttu-id="ed427-208">これを場所に必要なスタイルを使用して正しく読み込むようになりました必要があります。</span><span class="sxs-lookup"><span data-stu-id="ed427-208">It should now load correctly, with the expected styles in place.</span></span>

* <span data-ttu-id="ed427-209">*省略可能:* 新しいレイアウト ファイルを使用して再試行することができます。</span><span class="sxs-lookup"><span data-stu-id="ed427-209">*Optional:* You might want to try using the new layout file.</span></span> <span data-ttu-id="ed427-210">このプロジェクトからレイアウト ファイルをコピーすることができます、 *FullAspNetCore*プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="ed427-210">For this project you can copy the layout file from the *FullAspNetCore* project.</span></span> <span data-ttu-id="ed427-211">新しいレイアウト ファイルを使用して[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)あり、その他の改善。</span><span class="sxs-lookup"><span data-stu-id="ed427-211">The new layout file uses [Tag Helpers](xref:mvc/views/tag-helpers/intro) and has other improvements.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="ed427-212">バンドルと縮小を構成します。</span><span class="sxs-lookup"><span data-stu-id="ed427-212">Configure bundling and minification</span></span>

<span data-ttu-id="ed427-213">バンドルと縮小を構成する方法については、次を参照してください。 [Bundling and Minification](../client-side/bundling-and-minification.md)です。</span><span class="sxs-lookup"><span data-stu-id="ed427-213">For information about how to configure bundling and minification, see [Bundling and Minification](../client-side/bundling-and-minification.md).</span></span>

## <a name="solve-http-500-errors"></a><span data-ttu-id="ed427-214">HTTP 500 エラーを解決します。</span><span class="sxs-lookup"><span data-stu-id="ed427-214">Solve HTTP 500 errors</span></span>

<span data-ttu-id="ed427-215">多くの問題 HTTP 500 エラー メッセージを引き起こす可能性のある問題の原因に関する情報が含まれていないことがあります。</span><span class="sxs-lookup"><span data-stu-id="ed427-215">There are many problems that can cause a HTTP 500 error message that contain no information on the source of the problem.</span></span> <span data-ttu-id="ed427-216">たとえば場合、 *Views/_ViewImports.cshtml*ファイルがプロジェクトに存在しない名前空間を含む、HTTP 500 エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ed427-216">For example, if the *Views/_ViewImports.cshtml* file contains a namespace that doesn't exist in your project, you'll get a HTTP 500 error.</span></span> <span data-ttu-id="ed427-217">ASP.NET Core アプリの場合は、既定では、`UseDeveloperExceptionPage`に拡張子を追加、`IApplicationBuilder`構成時に実行および*開発*です。</span><span class="sxs-lookup"><span data-stu-id="ed427-217">By default in ASP.NET Core apps, the `UseDeveloperExceptionPage` extension is added to the `IApplicationBuilder` and executed when the configuration is *Development*.</span></span> <span data-ttu-id="ed427-218">これは、次のコードの詳細を示します。</span><span class="sxs-lookup"><span data-stu-id="ed427-218">This is detailed in the following code:</span></span>

[!code-csharp[](mvc/sample/Startup.cs?highlight=19-22)]

<span data-ttu-id="ed427-219">ASP.NET Core は、web アプリで未処理の例外を HTTP 500 エラー応答に変換します。</span><span class="sxs-lookup"><span data-stu-id="ed427-219">ASP.NET Core converts unhandled exceptions in a web app into HTTP 500 error responses.</span></span> <span data-ttu-id="ed427-220">通常、エラーの詳細は、サーバーの機密性の高い情報の漏えいを防ぐためにこれらの応答に含まれません。</span><span class="sxs-lookup"><span data-stu-id="ed427-220">Normally, error details aren't included in these responses to prevent disclosure of potentially sensitive information about the server.</span></span> <span data-ttu-id="ed427-221">参照してください**開発者例外ページを使用して**で[エラーの処理](../fundamentals/error-handling.md)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="ed427-221">See **Using the Developer Exception Page** in [Handle errors](../fundamentals/error-handling.md) for more information.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ed427-222">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="ed427-222">Additional resources</span></span>

* [<span data-ttu-id="ed427-223">クライアント側の開発</span><span class="sxs-lookup"><span data-stu-id="ed427-223">Client-side development</span></span>](xref:client-side/index)
* [<span data-ttu-id="ed427-224">タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="ed427-224">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
