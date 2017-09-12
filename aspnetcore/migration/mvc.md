---
title: "ASP.NET MVC から ASP.NET Core MVC への移行"
author: ardalis
description: 
keywords: "ASP.NET Core、MVC での移行"
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: article
ms.assetid: 3155cc9e-d0c9-424b-886c-35c0ec6f9f4e
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/mvc
ms.openlocfilehash: 2bd689626e867e0ea82fbebdf92447a6029aa35b
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="migrating-from-aspnet-mvc-to-aspnet-core-mvc"></a><span data-ttu-id="b53a7-103">ASP.NET MVC から ASP.NET Core MVC への移行</span><span class="sxs-lookup"><span data-stu-id="b53a7-103">Migrating From ASP.NET MVC to ASP.NET Core MVC</span></span>

<span data-ttu-id="b53a7-104">によって[Rick Anderson](https://twitter.com/RickAndMSFT)、 [Daniel Roth](https://github.com/danroth27)、 [Steve Smith](https://ardalis.com/)、および[Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="b53a7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="b53a7-105">この記事に ASP.NET MVC プロジェクトを移行する作業を開始する方法を示しています。 [ASP.NET Core MVC](../mvc/overview.md)です。</span><span class="sxs-lookup"><span data-stu-id="b53a7-105">This article shows how to get started migrating an ASP.NET MVC project to [ASP.NET Core MVC](../mvc/overview.md).</span></span> <span data-ttu-id="b53a7-106">プロセスでは、ASP.NET MVC から変更されたことの多くを強調表示します。</span><span class="sxs-lookup"><span data-stu-id="b53a7-106">In the process, it highlights many of the things that have changed from ASP.NET MVC.</span></span> <span data-ttu-id="b53a7-107">複数段階のプロセスは、ASP.NET MVC を移行して、この記事は、初期セットアップ、基本的なコント ローラーとビュー、静的なコンテンツ、およびクライアント側の依存関係について説明します。</span><span class="sxs-lookup"><span data-stu-id="b53a7-107">Migrating from ASP.NET MVC is a multiple step process and this article covers the initial setup, basic controllers and views, static content, and client-side dependencies.</span></span> <span data-ttu-id="b53a7-108">その他の記事では、構成を移行して多くの ASP.NET MVC プロジェクトで見つかった id コードについて説明します。</span><span class="sxs-lookup"><span data-stu-id="b53a7-108">Additional articles cover migrating configuration and identity code found in many ASP.NET MVC projects.</span></span>

> [!NOTE]
> <span data-ttu-id="b53a7-109">サンプルのバージョン番号が最新でない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="b53a7-109">The version numbers in the samples might not be current.</span></span> <span data-ttu-id="b53a7-110">したがって、プロジェクトを更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b53a7-110">You may need to update your projects accordingly.</span></span>

## <a name="create-the-starter-aspnet-mvc-project"></a><span data-ttu-id="b53a7-111">スターター ASP.NET MVC プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="b53a7-111">Create the starter ASP.NET MVC project</span></span>

<span data-ttu-id="b53a7-112">アップグレードを示すためには、ASP.NET MVC アプリケーションを作成することで始めます。</span><span class="sxs-lookup"><span data-stu-id="b53a7-112">To demonstrate the upgrade, we'll start by creating a ASP.NET MVC app.</span></span> <span data-ttu-id="b53a7-113">名前を使用して作成*WebApp1* ASP.NET Core プロジェクトを作成すると、次の手順には、名前空間に一致するようにします。</span><span class="sxs-lookup"><span data-stu-id="b53a7-113">Create it with the name *WebApp1* so the namespace will match the ASP.NET Core project we create in the next step.</span></span>

![Visual Studio の新しいプロジェクト ダイアログ ボックス](mvc/_static/new-project.png)

![新しい Web アプリケーション ダイアログ: MVC プロジェクト テンプレートがテンプレートの ASP.NET のパネルで選択されています。](mvc/_static/new-project-select-mvc-template.png)

<span data-ttu-id="b53a7-116">*省略可能:*からソリューションの名前を変更*WebApp1*に*Mvc5*です。</span><span class="sxs-lookup"><span data-stu-id="b53a7-116">*Optional:* Change the name of the Solution from *WebApp1* to *Mvc5*.</span></span> <span data-ttu-id="b53a7-117">Visual Studio 新しいソリューション名が表示されます (*Mvc5*)、するやすく次回のプロジェクトからこのプロジェクトに指示します。</span><span class="sxs-lookup"><span data-stu-id="b53a7-117">Visual Studio will display the new solution name (*Mvc5*), which will make it easier to tell this project from the next project.</span></span>

## <a name="create-the-aspnet-core-project"></a><span data-ttu-id="b53a7-118">ASP.NET Core プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="b53a7-118">Create the ASP.NET Core project</span></span>

<span data-ttu-id="b53a7-119">新規作成*空*前のプロジェクトと同じ名前での ASP.NET Core web アプリ (*WebApp1*) ため、2 つのプロジェクトの名前空間と一致します。</span><span class="sxs-lookup"><span data-stu-id="b53a7-119">Create a new *empty* ASP.NET Core web app with the same name as the previous project (*WebApp1*) so the namespaces in the two projects match.</span></span> <span data-ttu-id="b53a7-120">同じ名前空間を持つと、2 つのプロジェクト間でコードをコピーしやすきます。</span><span class="sxs-lookup"><span data-stu-id="b53a7-120">Having the same namespace makes it easier to copy code between the two projects.</span></span> <span data-ttu-id="b53a7-121">同じ名前を使用する前のプロジェクトとは異なるディレクトリでこのプロジェクトを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b53a7-121">You'll have to create this project in a different directory than the previous project to use the same name.</span></span>

![[新しいプロジェクト] ダイアログ](mvc/_static/new_core.png)

![新しい ASP.NET Web アプリケーション ダイアログ ボックス: ASP.NET Core テンプレート パネルで選択した空のプロジェクト テンプレート](mvc/_static/new-project-select-empty-aspnet5-template.png)

* <span data-ttu-id="b53a7-124">*省略可能:*新しい ASP.NET Core アプリケーションを使用して、作成、 *Web アプリケーション*プロジェクト テンプレート。</span><span class="sxs-lookup"><span data-stu-id="b53a7-124">*Optional:* Create a new ASP.NET Core app using the *Web Application* project template.</span></span> <span data-ttu-id="b53a7-125">プロジェクトに名前を*WebApp1*、認証オプションを選択して**個々 のユーザー アカウント**です。</span><span class="sxs-lookup"><span data-stu-id="b53a7-125">Name the project *WebApp1*, and select an authentication option of **Individual User Accounts**.</span></span> <span data-ttu-id="b53a7-126">このアプリの名前を変更*FullAspNetCore*です。</span><span class="sxs-lookup"><span data-stu-id="b53a7-126">Rename this app to *FullAspNetCore*.</span></span> <span data-ttu-id="b53a7-127">変換では、このプロジェクトは、時間を節約を作成します。</span><span class="sxs-lookup"><span data-stu-id="b53a7-127">Creating this project will save you time in the conversion.</span></span> <span data-ttu-id="b53a7-128">最終的な結果を表示する、または変換プロジェクトにコードをコピーするテンプレートによって生成されたコードを調べることができます。</span><span class="sxs-lookup"><span data-stu-id="b53a7-128">You can look at the template-generated code to see the end result or to copy code to the conversion project.</span></span> <span data-ttu-id="b53a7-129">テンプレートによって生成されたプロジェクトと比較するへの変換ステップに行き詰まった場合にも便利です。</span><span class="sxs-lookup"><span data-stu-id="b53a7-129">It's also helpful when you get stuck on a conversion step to compare with the template-generated project.</span></span>

## <a name="configure-the-site-to-use-mvc"></a><span data-ttu-id="b53a7-130">MVC を使用するサイトを構成します。</span><span class="sxs-lookup"><span data-stu-id="b53a7-130">Configure the site to use MVC</span></span>

* <span data-ttu-id="b53a7-131">インストール、`Microsoft.AspNetCore.Mvc`と`Microsoft.AspNetCore.StaticFiles`NuGet パッケージの管理。</span><span class="sxs-lookup"><span data-stu-id="b53a7-131">Install the `Microsoft.AspNetCore.Mvc` and `Microsoft.AspNetCore.StaticFiles` NuGet packages.</span></span>

  <span data-ttu-id="b53a7-132">`Microsoft.AspNetCore.Mvc`ASP.NET Core MVC フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="b53a7-132">`Microsoft.AspNetCore.Mvc` is the ASP.NET Core MVC framework.</span></span> <span data-ttu-id="b53a7-133">`Microsoft.AspNetCore.StaticFiles`静的ファイル ハンドラーです。</span><span class="sxs-lookup"><span data-stu-id="b53a7-133">`Microsoft.AspNetCore.StaticFiles` is the static file handler.</span></span> <span data-ttu-id="b53a7-134">ASP.NET ランタイムがモジュール、および静的なファイルが機能するように明示的にオプトインする必要があります (を参照してください[静的ファイルで作業](../fundamentals/static-files.md))。</span><span class="sxs-lookup"><span data-stu-id="b53a7-134">The ASP.NET runtime is modular, and you must explicitly opt in to serve static files (see [Working with Static Files](../fundamentals/static-files.md)).</span></span>

* <span data-ttu-id="b53a7-135">開く、 *.csproj*ファイル (でプロジェクトを右クリックして**ソリューション エクスプ ローラー**を選択し、**編集 WebApp1.csproj**) を追加し、`PrepareForPublish`ターゲット。</span><span class="sxs-lookup"><span data-stu-id="b53a7-135">Open the *.csproj* file (right-click the project in **Solution Explorer** and select **Edit WebApp1.csproj**) and add a `PrepareForPublish` target:</span></span>

  <span data-ttu-id="b53a7-136">[!code-xml[Main](mvc/sample/WebApp1.csproj?range=21-23)]</span><span class="sxs-lookup"><span data-stu-id="b53a7-136">[!code-xml[Main](mvc/sample/WebApp1.csproj?range=21-23)]</span></span>

  <span data-ttu-id="b53a7-137">`PrepareForPublish`ターゲットは、Bower を使用してクライアント側のライブラリを取得するために必要です。</span><span class="sxs-lookup"><span data-stu-id="b53a7-137">The `PrepareForPublish` target is needed for acquiring client-side libraries via Bower.</span></span> <span data-ttu-id="b53a7-138">後でについてを説明します。</span><span class="sxs-lookup"><span data-stu-id="b53a7-138">We'll talk about that later.</span></span>

* <span data-ttu-id="b53a7-139">開く、 *Startup.cs*ファイルを開き、次の一致するようにコードを変更します。</span><span class="sxs-lookup"><span data-stu-id="b53a7-139">Open the *Startup.cs* file and change the code to match the following:</span></span>

  <span data-ttu-id="b53a7-140">[!code-csharp[Main](mvc/sample/Startup.cs?highlight=14,27-34)]</span><span class="sxs-lookup"><span data-stu-id="b53a7-140">[!code-csharp[Main](mvc/sample/Startup.cs?highlight=14,27-34)]</span></span>

  <span data-ttu-id="b53a7-141">`UseStaticFiles`拡張メソッドが静的ファイル ハンドラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="b53a7-141">The `UseStaticFiles` extension method adds the static file handler.</span></span> <span data-ttu-id="b53a7-142">前述のように、ASP.NET ランタイムは、モジュール、および静的なファイルが機能するように明示的にオプトインする必要があります。</span><span class="sxs-lookup"><span data-stu-id="b53a7-142">As mentioned previously, the ASP.NET runtime is modular, and you must explicitly opt in to serve static files.</span></span> <span data-ttu-id="b53a7-143">`UseMvc`ルーティング拡張メソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="b53a7-143">The `UseMvc` extension method adds routing.</span></span> <span data-ttu-id="b53a7-144">詳細については、次を参照してください。[アプリケーションの起動](../fundamentals/startup.md)と[ルーティング](../fundamentals/routing.md)です。</span><span class="sxs-lookup"><span data-stu-id="b53a7-144">For more information, see [Application Startup](../fundamentals/startup.md) and [Routing](../fundamentals/routing.md).</span></span>

## <a name="add-a-controller-and-view"></a><span data-ttu-id="b53a7-145">コント ローラーとビューを追加します。</span><span class="sxs-lookup"><span data-stu-id="b53a7-145">Add a controller and view</span></span>

<span data-ttu-id="b53a7-146">このセクションでは、最小限のコント ローラーと ASP.NET MVC コント ローラーのプレース ホルダーとして機能するようにビューとビューの次のセクションで移行しますを追加します。</span><span class="sxs-lookup"><span data-stu-id="b53a7-146">In this section, you'll add a minimal controller and view to serve as placeholders for the ASP.NET MVC controller and views you'll migrate in the next section.</span></span>

* <span data-ttu-id="b53a7-147">追加、*コント ローラー*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="b53a7-147">Add a *Controllers* folder.</span></span>

* <span data-ttu-id="b53a7-148">追加、 **MVC コント ローラー クラス**名前を持つ*HomeController.cs*を*コント ローラー*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="b53a7-148">Add an **MVC controller class** with the name *HomeController.cs* to the *Controllers* folder.</span></span>

![新しい項目 ダイアログ ボックスを追加します。](mvc/_static/add_mvc_ctl.png)

* <span data-ttu-id="b53a7-150">追加、*ビュー*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="b53a7-150">Add a *Views* folder.</span></span>

* <span data-ttu-id="b53a7-151">追加、*ビュー/ホーム*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="b53a7-151">Add a *Views/Home* folder.</span></span>

* <span data-ttu-id="b53a7-152">追加、 *Index.cshtml* MVC ビュー ページを*ビュー/ホーム*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="b53a7-152">Add an *Index.cshtml* MVC view page to the *Views/Home* folder.</span></span>

![新しい項目 ダイアログ ボックスを追加します。](mvc/_static/view.png)

<span data-ttu-id="b53a7-154">プロジェクトの構造を次に示します。</span><span class="sxs-lookup"><span data-stu-id="b53a7-154">The project structure is shown below:</span></span>

![ソリューション エクスプ ローラー WebApp1 のファイルとフォルダーの表示](mvc/_static/project-structure-controller-view.png)

<span data-ttu-id="b53a7-156">内容を置き換える、 *Views/Home/Index.cshtml*を次のファイル。</span><span class="sxs-lookup"><span data-stu-id="b53a7-156">Replace the contents of the *Views/Home/Index.cshtml* file with the following:</span></span>

```html
<h1>Hello world!</h1>
```

<span data-ttu-id="b53a7-157">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="b53a7-157">Run the app.</span></span>

![Microsoft Edge で開いている web アプリケーション](mvc/_static/hello-world.png)

<span data-ttu-id="b53a7-159">参照してください[コント ローラー](../mvc/controllers/index.md)と[ビュー](../mvc/views/index.md)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="b53a7-159">See [Controllers](../mvc/controllers/index.md) and [Views](../mvc/views/index.md) for more information.</span></span>

<span data-ttu-id="b53a7-160">最小限の作業用 ASP.NET Core プロジェクトができました、ASP.NET MVC プロジェクトから機能の移行が始めることができます。</span><span class="sxs-lookup"><span data-stu-id="b53a7-160">Now that we have a minimal working ASP.NET Core project, we can start migrating functionality from the ASP.NET MVC project.</span></span> <span data-ttu-id="b53a7-161">次に移動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b53a7-161">We will need to move the following:</span></span>

* <span data-ttu-id="b53a7-162">クライアント側のコンテンツ (CSS、フォント、およびスクリプト)</span><span class="sxs-lookup"><span data-stu-id="b53a7-162">client-side content (CSS, fonts, and scripts)</span></span>

* <span data-ttu-id="b53a7-163">コントローラー</span><span class="sxs-lookup"><span data-stu-id="b53a7-163">controllers</span></span>

* <span data-ttu-id="b53a7-164">ビュー</span><span class="sxs-lookup"><span data-stu-id="b53a7-164">views</span></span>

* <span data-ttu-id="b53a7-165">モデル</span><span class="sxs-lookup"><span data-stu-id="b53a7-165">models</span></span>

* <span data-ttu-id="b53a7-166">バンドル化</span><span class="sxs-lookup"><span data-stu-id="b53a7-166">bundling</span></span>

* <span data-ttu-id="b53a7-167">フィルター</span><span class="sxs-lookup"><span data-stu-id="b53a7-167">filters</span></span>

* <span data-ttu-id="b53a7-168">ログの/out、id (はこれで次のチュートリアルです。)</span><span class="sxs-lookup"><span data-stu-id="b53a7-168">Log in/out, identity (This will be done in the next tutorial.)</span></span>

## <a name="controllers-and-views"></a><span data-ttu-id="b53a7-169">コント ローラーとビュー</span><span class="sxs-lookup"><span data-stu-id="b53a7-169">Controllers and views</span></span>

* <span data-ttu-id="b53a7-170">ASP.NET MVC のコピーの各メソッド`HomeController`を新しい`HomeController`です。</span><span class="sxs-lookup"><span data-stu-id="b53a7-170">Copy each of the methods from the ASP.NET MVC `HomeController` to the new `HomeController`.</span></span> <span data-ttu-id="b53a7-171">ASP.NET MVC では、組み込みのテンプレートのコント ローラー アクション メソッドの戻り値型は[ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx)以外の場合は ASP.NET Core MVC、アクション メソッドの戻り値の`IActionResult`代わりにします。</span><span class="sxs-lookup"><span data-stu-id="b53a7-171">Note that in ASP.NET MVC, the built-in template's controller action method return type is [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC, the action methods return `IActionResult` instead.</span></span> <span data-ttu-id="b53a7-172">`ActionResult`実装する`IActionResult`ので、アクション メソッドの戻り値の型を変更する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="b53a7-172">`ActionResult` implements `IActionResult`, so there is no need to change the return type of your action methods.</span></span>

* <span data-ttu-id="b53a7-173">コピー、 *About.cshtml*、 *Contact.cshtml*、および*Index.cshtml* Razor ビューのファイルを ASP.NET MVC プロジェクトから ASP.NET Core プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="b53a7-173">Copy the *About.cshtml*, *Contact.cshtml*, and *Index.cshtml* Razor view files from the ASP.NET MVC project to the ASP.NET Core project.</span></span>

* <span data-ttu-id="b53a7-174">ASP.NET Core アプリケーションを実行し、各メソッドをテストします。</span><span class="sxs-lookup"><span data-stu-id="b53a7-174">Run the ASP.NET Core app and test each method.</span></span> <span data-ttu-id="b53a7-175">いない移行ファイルのレイアウトやスタイルまだ、レンダリングされるビューはビュー ファイル内のコンテンツのみを含めるようにします。</span><span class="sxs-lookup"><span data-stu-id="b53a7-175">We haven't migrated the layout file or styles yet, so the rendered views will only contain the content in the view files.</span></span> <span data-ttu-id="b53a7-176">レイアウトに生成されたファイルへのリンクする必要はありません、`About`と`Contact`ビュー、ブラウザーからそれらを起動する必要がありますので (交換**4492**プロジェクトで使用されるポート番号を持つ)。</span><span class="sxs-lookup"><span data-stu-id="b53a7-176">You won't have the layout file generated links for the `About` and `Contact` views, so you'll have to invoke them from the browser (replace **4492** with the port number used in your project).</span></span>

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![連絡先のページ](mvc/_static/contact-page.png)

<span data-ttu-id="b53a7-178">スタイル設定およびメニュー項目がないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="b53a7-178">Note the lack of styling and menu items.</span></span> <span data-ttu-id="b53a7-179">解決する次のセクションでします。</span><span class="sxs-lookup"><span data-stu-id="b53a7-179">We'll fix that in the next section.</span></span>

## <a name="static-content"></a><span data-ttu-id="b53a7-180">静的コンテンツ</span><span class="sxs-lookup"><span data-stu-id="b53a7-180">Static content</span></span>

<span data-ttu-id="b53a7-181">ASP.NET MVC の以前のバージョンでは、静的コンテンツは、web プロジェクトのルートからホストされているし、サーバー側のファイルが混合されました。</span><span class="sxs-lookup"><span data-stu-id="b53a7-181">In previous versions of  ASP.NET MVC, static content was hosted from the root of the web project and was intermixed with server-side files.</span></span> <span data-ttu-id="b53a7-182">ASP.NET Core での静的なコンテンツをホスト、 *wwwroot*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="b53a7-182">In ASP.NET Core, static content is hosted in the *wwwroot* folder.</span></span> <span data-ttu-id="b53a7-183">古い ASP.NET MVC のアプリから静的なコンテンツをコピーしておく、 *wwwroot* ASP.NET Core プロジェクトのフォルダーにします。</span><span class="sxs-lookup"><span data-stu-id="b53a7-183">You'll want to copy the static content from your old ASP.NET MVC app to the *wwwroot* folder in your ASP.NET Core project.</span></span> <span data-ttu-id="b53a7-184">このサンプルの変換。</span><span class="sxs-lookup"><span data-stu-id="b53a7-184">In this sample conversion:</span></span>

* <span data-ttu-id="b53a7-185">コピー、 *favicon.ico*に古い MVC プロジェクトのファイル、 *wwwroot* ASP.NET Core プロジェクト フォルダーにします。</span><span class="sxs-lookup"><span data-stu-id="b53a7-185">Copy the *favicon.ico* file from the old MVC project to the *wwwroot* folder in the ASP.NET Core project.</span></span>

<span data-ttu-id="b53a7-186">古い ASP.NET MVC プロジェクトの使用[ブートス トラップ](http://getbootstrap.com/)にファイルをブートス トラップのスタイルとストアの*コンテンツ*と*スクリプト*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="b53a7-186">The old ASP.NET MVC project uses [Bootstrap](http://getbootstrap.com/) for its styling and stores the Bootstrap files in the *Content* and *Scripts* folders.</span></span> <span data-ttu-id="b53a7-187">以前の ASP.NET MVC プロジェクトを生成するには、テンプレートを参照して、レイアウト ファイル ブートス トラップ (*Views/Shared/_Layout.cshtml*)。</span><span class="sxs-lookup"><span data-stu-id="b53a7-187">The template, which generated the old ASP.NET MVC project, references Bootstrap in the layout file (*Views/Shared/_Layout.cshtml*).</span></span> <span data-ttu-id="b53a7-188">コピーすることで、 *bootstrap.js*と*bootstrap.css*ファイルから ASP.NET MVC プロジェクトを*wwwroot*フォルダーに、新しいプロジェクトが、その方法を使用しない、ASP.NET Core でのクライアント側の依存関係を管理するための強化メカニズムです。</span><span class="sxs-lookup"><span data-stu-id="b53a7-188">You could copy the *bootstrap.js* and *bootstrap.css* files from the ASP.NET MVC project to the *wwwroot* folder in the new project, but that approach doesn't use the improved mechanism for managing client-side dependencies in ASP.NET Core.</span></span>

<span data-ttu-id="b53a7-189">新しいプロジェクトを使用してブートス トラップ (およびその他のクライアント側ライブラリ) サポートを追加します[Bower](https://bower.io/):</span><span class="sxs-lookup"><span data-stu-id="b53a7-189">In the new project, we'll add support for Bootstrap (and other client-side libraries) using [Bower](https://bower.io/):</span></span>

* <span data-ttu-id="b53a7-190">追加、 [Bower](https://bower.io/)という名前の構成ファイル*bower.json*プロジェクトのルートに (、プロジェクトを右クリックし、**追加 > 新しい項目の追加 > Bower 構成ファイル**)。</span><span class="sxs-lookup"><span data-stu-id="b53a7-190">Add a [Bower](https://bower.io/) configuration file named *bower.json* to the project root (Right-click on the project, and then **Add > New Item > Bower Configuration File**).</span></span> <span data-ttu-id="b53a7-191">追加[ブートス トラップ](http://getbootstrap.com/)と[jQuery](https://jquery.com/)ファイル (以下の強調表示された行を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="b53a7-191">Add [Bootstrap](http://getbootstrap.com/) and [jQuery](https://jquery.com/) to the file (see the highlighted lines below).</span></span>

  <span data-ttu-id="b53a7-192">[!code-json[Main](mvc/sample/bower.json?highlight=5-6)]</span><span class="sxs-lookup"><span data-stu-id="b53a7-192">[!code-json[Main](mvc/sample/bower.json?highlight=5-6)]</span></span>

<span data-ttu-id="b53a7-193">ファイルを保存するときに Bower は自動的にダウンロードする依存関係、 *wwwroot/lib*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="b53a7-193">Upon saving the file, Bower will automatically download the dependencies to the *wwwroot/lib* folder.</span></span> <span data-ttu-id="b53a7-194">使用することができます、**ソリューション エクスプ ローラーの検索**資産のパスを検索するボックス。</span><span class="sxs-lookup"><span data-stu-id="b53a7-194">You can use the **Search Solution Explorer** box to find the path of the assets:</span></span>

![ソリューション エクスプ ローラー検索結果に示される jquery 資産](mvc/_static/search.png)

<span data-ttu-id="b53a7-196">参照してください[Bower でクライアント側のパッケージを管理](../client-side/bower.md)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="b53a7-196">See [Manage Client-Side Packages with Bower](../client-side/bower.md) for more information.</span></span>

<a name=migrate-layout-file></a>

## <a name="migrate-the-layout-file"></a><span data-ttu-id="b53a7-197">レイアウト ファイルを移行します。</span><span class="sxs-lookup"><span data-stu-id="b53a7-197">Migrate the layout file</span></span>

* <span data-ttu-id="b53a7-198">コピー、*は _viewstart.vbhtml*古い ASP.NET MVC プロジェクトのファイルの*ビュー* ASP.NET Core プロジェクトのフォルダー*ビュー*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="b53a7-198">Copy the *_ViewStart.cshtml* file from the old ASP.NET MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="b53a7-199">*は _viewstart.vbhtml* ASP.NET Core MVC でファイルが変更されていません。</span><span class="sxs-lookup"><span data-stu-id="b53a7-199">The *_ViewStart.cshtml* file has not changed in ASP.NET Core MVC.</span></span>

* <span data-ttu-id="b53a7-200">作成、 *Views/shared*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="b53a7-200">Create a *Views/Shared* folder.</span></span>

* <span data-ttu-id="b53a7-201">*省略可能:*コピー *_ViewImports.cshtml*から、 *FullAspNetCore* MVC プロジェクトの*ビュー*フォルダーに、ASP.NET Core プロジェクトの*ビュー*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="b53a7-201">*Optional:* Copy *_ViewImports.cshtml* from the *FullAspNetCore* MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="b53a7-202">任意の名前空間宣言を削除、 *_ViewImports.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="b53a7-202">Remove any namespace declaration in the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="b53a7-203">*_ViewImports.cshtml*ファイル ビューのすべてのファイルの名前空間を提供しでによってもたらさ[タグ ヘルパー](../mvc/views/tag-helpers/index.md)です。</span><span class="sxs-lookup"><span data-stu-id="b53a7-203">The *_ViewImports.cshtml* file provides namespaces for all the view files and brings in [Tag Helpers](../mvc/views/tag-helpers/index.md).</span></span> <span data-ttu-id="b53a7-204">タグ ヘルパーは、新しいレイアウト ファイルで使用されます。</span><span class="sxs-lookup"><span data-stu-id="b53a7-204">Tag Helpers are used in the new layout file.</span></span> <span data-ttu-id="b53a7-205">*_ViewImports.cshtml*ファイルが ASP.NET Core の新機能です。</span><span class="sxs-lookup"><span data-stu-id="b53a7-205">The *_ViewImports.cshtml* file is new for ASP.NET Core.</span></span>

* <span data-ttu-id="b53a7-206">コピー、 *_Layout.cshtml*古い ASP.NET MVC プロジェクトのファイルの*Views/shared* ASP.NET Core プロジェクトのフォルダー *Views/shared*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="b53a7-206">Copy the *_Layout.cshtml* file from the old ASP.NET MVC project's *Views/Shared* folder into the ASP.NET Core project's *Views/Shared* folder.</span></span>

<span data-ttu-id="b53a7-207">開いている*_Layout.cshtml*ファイルし、次の変更 (完了したコードは、以下のとおりです)。</span><span class="sxs-lookup"><span data-stu-id="b53a7-207">Open *_Layout.cshtml* file and make the following changes (the completed code is shown below):</span></span>

   * <span data-ttu-id="b53a7-208">置き換える`@Styles.Render("~/Content/css")`で、`<link>`読み込み要素*bootstrap.css* (下記参照)。</span><span class="sxs-lookup"><span data-stu-id="b53a7-208">Replace `@Styles.Render("~/Content/css")` with a `<link>` element to load *bootstrap.css* (see below).</span></span>

   * <span data-ttu-id="b53a7-209">削除`@Scripts.Render("~/bundles/modernizr")`です。</span><span class="sxs-lookup"><span data-stu-id="b53a7-209">Remove `@Scripts.Render("~/bundles/modernizr")`.</span></span>

   * <span data-ttu-id="b53a7-210">コメント アウト、`@Html.Partial("_LoginPartial")`行 (では、行を囲む`@*...*@`)。</span><span class="sxs-lookup"><span data-stu-id="b53a7-210">Comment out the `@Html.Partial("_LoginPartial")` line (surround the line with `@*...*@`).</span></span> <span data-ttu-id="b53a7-211">今後のチュートリアルでを返します。</span><span class="sxs-lookup"><span data-stu-id="b53a7-211">We'll return to it in a future tutorial.</span></span>

   * <span data-ttu-id="b53a7-212">置き換える`@Scripts.Render("~/bundles/jquery")`で、`<script>`要素 (下記参照)。</span><span class="sxs-lookup"><span data-stu-id="b53a7-212">Replace `@Scripts.Render("~/bundles/jquery")` with a `<script>` element (see below).</span></span>

   * <span data-ttu-id="b53a7-213">置き換える`@Scripts.Render("~/bundles/bootstrap")`で、`<script>`要素 (下記参照).</span><span class="sxs-lookup"><span data-stu-id="b53a7-213">Replace `@Scripts.Render("~/bundles/bootstrap")` with a `<script>` element (see below)..</span></span>

<span data-ttu-id="b53a7-214">置換する CSS リンク:</span><span class="sxs-lookup"><span data-stu-id="b53a7-214">The replacement CSS link:</span></span>

```html
<link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.css" />
```

<span data-ttu-id="b53a7-215">交換用のスクリプト タグ:</span><span class="sxs-lookup"><span data-stu-id="b53a7-215">The replacement script tags:</span></span>

```html
<script src="~/lib/jquery/dist/jquery.js"></script>
<script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
```

<span data-ttu-id="b53a7-216">更新された*_Layout.cshtml*ファイルを次に示します。</span><span class="sxs-lookup"><span data-stu-id="b53a7-216">The updated *_Layout.cshtml* file is shown below:</span></span>

<span data-ttu-id="b53a7-217">[!code-html[Main](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7,27,39-40)]</span><span class="sxs-lookup"><span data-stu-id="b53a7-217">[!code-html[Main](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7,27,39-40)]</span></span>

<span data-ttu-id="b53a7-218">ブラウザーで、サイトを表示します。</span><span class="sxs-lookup"><span data-stu-id="b53a7-218">View the site in the browser.</span></span> <span data-ttu-id="b53a7-219">これを場所に必要なスタイルを使用して正しく読み込むようになりました必要があります。</span><span class="sxs-lookup"><span data-stu-id="b53a7-219">It should now load correctly, with the expected styles in place.</span></span>

* <span data-ttu-id="b53a7-220">*省略可能:*新しいレイアウト ファイルを使用して再試行することができます。</span><span class="sxs-lookup"><span data-stu-id="b53a7-220">*Optional:* You might want to try using the new layout file.</span></span> <span data-ttu-id="b53a7-221">このプロジェクトからレイアウト ファイルをコピーすることができます、 *FullAspNetCore*プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="b53a7-221">For this project you can copy the layout file from the *FullAspNetCore* project.</span></span> <span data-ttu-id="b53a7-222">新しいレイアウト ファイルを使用して[タグ ヘルパー](../mvc/views/tag-helpers/index.md)あり、その他の改善。</span><span class="sxs-lookup"><span data-stu-id="b53a7-222">The new layout file uses [Tag Helpers](../mvc/views/tag-helpers/index.md) and has other improvements.</span></span>

## <a name="configure-bundling--minification"></a><span data-ttu-id="b53a7-223">バンドルの構成 (& a) の縮小</span><span class="sxs-lookup"><span data-stu-id="b53a7-223">Configure Bundling & Minification</span></span>

<span data-ttu-id="b53a7-224">バンドルと縮小を構成する方法については、次を参照してください。 [Bundling and Minification](../client-side/bundling-and-minification.md)です。</span><span class="sxs-lookup"><span data-stu-id="b53a7-224">For information about how to configure bundling and minification, see [Bundling and Minification](../client-side/bundling-and-minification.md).</span></span>

## <a name="solving-http-500-errors"></a><span data-ttu-id="b53a7-225">HTTP 500 のエラーを解決します。</span><span class="sxs-lookup"><span data-stu-id="b53a7-225">Solving HTTP 500 errors</span></span>

<span data-ttu-id="b53a7-226">多くの問題 HTTP 500 エラー メッセージを引き起こす可能性のある問題の原因に関する情報が含まれていないことがあります。</span><span class="sxs-lookup"><span data-stu-id="b53a7-226">There are many problems that can cause a HTTP 500 error message that contain no information on the source of the problem.</span></span> <span data-ttu-id="b53a7-227">たとえば場合、 *Views/_ViewImports.cshtml*ファイルがプロジェクトに存在しない名前空間を含む、HTTP 500 エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b53a7-227">For example, if the *Views/_ViewImports.cshtml* file contains a namespace that doesn't exist in your project, you'll get a HTTP 500 error.</span></span> <span data-ttu-id="b53a7-228">詳細なエラー メッセージを取得するには、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="b53a7-228">To get a detailed error message, add the following code:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    if (env.IsDevelopment())
    {
         app.UseDeveloperExceptionPage();
    }

    app.UseStaticFiles();

    app.UseMvc(routes =>
    {
        routes.MapRoute(
            name: "default",
            template: "{controller=Home}/{action=Index}/{id?}");
    });
}
```

<span data-ttu-id="b53a7-229">参照してください**開発者例外ページを使用して**で[Error Handling](../fundamentals/error-handling.md)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="b53a7-229">See **Using the Developer Exception Page** in [Error Handling](../fundamentals/error-handling.md) for more information.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b53a7-230">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="b53a7-230">Additional Resources</span></span>

* [<span data-ttu-id="b53a7-231">クライアント側の開発</span><span class="sxs-lookup"><span data-stu-id="b53a7-231">Client-Side Development</span></span>](../client-side/index.md)

* [<span data-ttu-id="b53a7-232">タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="b53a7-232">Tag Helpers</span></span>](../mvc/views/tag-helpers/index.md)
