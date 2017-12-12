---
title: "レイアウト"
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 29f12d1f-9734-48bd-bf1a-cee53a8ab700
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/layout
ms.openlocfilehash: 064621d8756b007c5b8859111bf3a03a0d7dda81
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="layout"></a><span data-ttu-id="3d57d-103">レイアウト</span><span class="sxs-lookup"><span data-stu-id="3d57d-103">Layout</span></span>

<span data-ttu-id="3d57d-104">によって[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="3d57d-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="3d57d-105">ビューは、ビジュアル化およびプログラムの要素を頻繁に共有します。</span><span class="sxs-lookup"><span data-stu-id="3d57d-105">Views frequently share visual and programmatic elements.</span></span> <span data-ttu-id="3d57d-106">この記事では、一般的なレイアウトを使用して、共有するディレクティブ、および ASP.NET アプリのビューのレンダリングの前に共通のコードを実行する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="3d57d-106">In this article, you'll learn how to use common layouts, share directives, and run common code before rendering views in your ASP.NET app.</span></span>

## <a name="what-is-a-layout"></a><span data-ttu-id="3d57d-107">レイアウトします。</span><span class="sxs-lookup"><span data-stu-id="3d57d-107">What is a Layout</span></span>

<span data-ttu-id="3d57d-108">ほとんどの web アプリでは、ページ間を移動するのに合わせて一貫した使用環境をユーザーに提供する一般的なレイアウトがあります。</span><span class="sxs-lookup"><span data-stu-id="3d57d-108">Most web apps have a common layout that provides the user with a consistent experience as they navigate from page to page.</span></span> <span data-ttu-id="3d57d-109">レイアウトには、通常、アプリのヘッダー、ナビゲーションまたはメニュー要素、およびフッターなどの一般的なユーザー インターフェイス要素が含まれます。</span><span class="sxs-lookup"><span data-stu-id="3d57d-109">The layout typically includes common user interface elements such as the app header, navigation or menu elements, and footer.</span></span>

![ページ レイアウトの例](layout/_static/page-layout.png)

<span data-ttu-id="3d57d-111">スクリプトとスタイル シートなどの共通の HTML 構造体は、アプリ内の多数のページでも頻繁に使用されます。</span><span class="sxs-lookup"><span data-stu-id="3d57d-111">Common HTML structures such as scripts and stylesheets are also frequently used by many pages within an app.</span></span> <span data-ttu-id="3d57d-112">これらの共有要素のすべての環境で定義されている可能性があります、*レイアウト*ファイルで、アプリ内で使用される任意のビューで参照できます。</span><span class="sxs-lookup"><span data-stu-id="3d57d-112">All of these shared elements may be defined in a *layout* file, which can then be referenced by any view used within the app.</span></span> <span data-ttu-id="3d57d-113">レイアウト削減重複コード ビュー、支援をフォロー、[しない繰り返します自分で (ドライ) 原則](http://deviq.com/don-t-repeat-yourself/)です。</span><span class="sxs-lookup"><span data-stu-id="3d57d-113">Layouts reduce duplicate code in views, helping them follow the [Don't Repeat Yourself (DRY) principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="3d57d-114">慣例により、ASP.NET アプリケーションの既定のレイアウトが名前付き`_Layout.cshtml`します。</span><span class="sxs-lookup"><span data-stu-id="3d57d-114">By convention, the default layout for an ASP.NET app is named `_Layout.cshtml`.</span></span> <span data-ttu-id="3d57d-115">Visual Studio ASP.NET Core の MVC プロジェクト テンプレートには、このレイアウト ファイルが含まれています、`Views/Shared`フォルダー。</span><span class="sxs-lookup"><span data-stu-id="3d57d-115">The Visual Studio ASP.NET Core MVC project template includes this layout file in the `Views/Shared` folder:</span></span>

![ソリューション エクスプ ローラー ビュー フォルダー](layout/_static/web-project-views.png)

<span data-ttu-id="3d57d-117">このレイアウトは、アプリでのビューの最上位レベルのテンプレートを定義します。</span><span class="sxs-lookup"><span data-stu-id="3d57d-117">This layout defines a top level template for views in the app.</span></span> <span data-ttu-id="3d57d-118">アプリには、レイアウトは不要、アプリは、さまざまなレイアウトを指定するさまざまなビューで複数のレイアウトを定義できます。</span><span class="sxs-lookup"><span data-stu-id="3d57d-118">Apps do not require a layout, and apps can define more than one layout, with different views specifying different layouts.</span></span>

<span data-ttu-id="3d57d-119">たとえば`_Layout.cshtml`:</span><span class="sxs-lookup"><span data-stu-id="3d57d-119">An example `_Layout.cshtml`:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a><span data-ttu-id="3d57d-120">レイアウトの指定</span><span class="sxs-lookup"><span data-stu-id="3d57d-120">Specifying a Layout</span></span>

<span data-ttu-id="3d57d-121">Razor ビューが、`Layout`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="3d57d-121">Razor views have a `Layout` property.</span></span> <span data-ttu-id="3d57d-122">個々 のビューは、このプロパティを設定して、レイアウトを指定します。</span><span class="sxs-lookup"><span data-stu-id="3d57d-122">Individual views specify a layout by setting this property:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

<span data-ttu-id="3d57d-123">指定したレイアウトは、完全なパスを使用できます (例: `/Views/Shared/_Layout.cshtml`) または部分的な名前 (例: `_Layout`)。</span><span class="sxs-lookup"><span data-stu-id="3d57d-123">The layout specified can use a full path (example: `/Views/Shared/_Layout.cshtml`) or a partial name (example: `_Layout`).</span></span> <span data-ttu-id="3d57d-124">部分的な名前を指定すると、Razor ビュー エンジンは、標準の探索プロセスを使用して、レイアウト ファイルで検索します。</span><span class="sxs-lookup"><span data-stu-id="3d57d-124">When a partial name is provided, the Razor view engine will search for the layout file using its standard discovery process.</span></span> <span data-ttu-id="3d57d-125">コント ローラーに関連付けられているフォルダーが最初に検索後に、`Shared`フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="3d57d-125">The controller-associated folder is searched first, followed by the `Shared` folder.</span></span> <span data-ttu-id="3d57d-126">この検出プロセスが検出に使用したものと同じ[部分ビュー](partial.md)です。</span><span class="sxs-lookup"><span data-stu-id="3d57d-126">This discovery process is identical to the one used to discover [partial views](partial.md).</span></span>

<span data-ttu-id="3d57d-127">既定では、すべてのレイアウトを呼び出す必要があります`RenderBody`です。</span><span class="sxs-lookup"><span data-stu-id="3d57d-127">By default, every layout must call `RenderBody`.</span></span> <span data-ttu-id="3d57d-128">任意の場所への呼び出し`RenderBody`が配置されると、ビューの内容が表示されます。</span><span class="sxs-lookup"><span data-stu-id="3d57d-128">Wherever the call to `RenderBody` is placed, the contents of the view will be rendered.</span></span>

<a name="layout-sections-label"></a>

### <a name="sections"></a><span data-ttu-id="3d57d-129">セクション</span><span class="sxs-lookup"><span data-stu-id="3d57d-129">Sections</span></span>

<span data-ttu-id="3d57d-130">レイアウトは、必要に応じて 1 つまたは複数を参照できます*セクション*、呼び出すことによって`RenderSection`です。</span><span class="sxs-lookup"><span data-stu-id="3d57d-130">A layout can optionally reference one or more *sections*, by calling `RenderSection`.</span></span> <span data-ttu-id="3d57d-131">セクションでは、特定のページ要素の配置場所を整理することを示します。</span><span class="sxs-lookup"><span data-stu-id="3d57d-131">Sections provide a way to organize where certain page elements should be placed.</span></span> <span data-ttu-id="3d57d-132">各呼び出し`RenderSection`そのセクションが必須またはオプションかどうかを指定できます。</span><span class="sxs-lookup"><span data-stu-id="3d57d-132">Each call to `RenderSection` can specify whether that section is required or optional.</span></span> <span data-ttu-id="3d57d-133">必要なセクションが見つからない場合、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="3d57d-133">If a required section is not found, an exception will be thrown.</span></span> <span data-ttu-id="3d57d-134">個々 のビューを使用して、セクション内に表示するコンテンツを指定する、 `@section` Razor 構文です。</span><span class="sxs-lookup"><span data-stu-id="3d57d-134">Individual views specify the content to be rendered within a section using the `@section` Razor syntax.</span></span> <span data-ttu-id="3d57d-135">ビューには、セクションが定義されている場合、表示する必要があります (または、エラーが発生)。</span><span class="sxs-lookup"><span data-stu-id="3d57d-135">If a view defines a section, it must be rendered (or an error will occur).</span></span>

<span data-ttu-id="3d57d-136">たとえば`@section`ビューで定義します。</span><span class="sxs-lookup"><span data-stu-id="3d57d-136">An example `@section` definition in a view:</span></span>

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

<span data-ttu-id="3d57d-137">上記のコードに検証スクリプトを追加、`scripts`セクションのフォームを含むビュー。</span><span class="sxs-lookup"><span data-stu-id="3d57d-137">In the code above, validation scripts are added to the `scripts` section on a view that includes a form.</span></span> <span data-ttu-id="3d57d-138">同じアプリケーション内の他のビューは、追加のスクリプトは必要ありませんがおよびためスクリプトのセクションを定義する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="3d57d-138">Other views in the same application might not require any additional scripts, and so wouldn't need to define a scripts section.</span></span>

<span data-ttu-id="3d57d-139">ビューで定義されたセクションは、イミディ エイト レイアウト ページでのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="3d57d-139">Sections defined in a view are available only in its immediate layout page.</span></span> <span data-ttu-id="3d57d-140">パーシャル、コンポーネントの表示、またはシステムの表示の他の部分からは参照できません。</span><span class="sxs-lookup"><span data-stu-id="3d57d-140">They cannot be referenced from partials, view components, or other parts of the view system.</span></span>

### <a name="ignoring-sections"></a><span data-ttu-id="3d57d-141">セクションでは無視されます。</span><span class="sxs-lookup"><span data-stu-id="3d57d-141">Ignoring sections</span></span>

<span data-ttu-id="3d57d-142">既定では、本体およびコンテンツ ページのすべてのセクション必要がありますすべてによってレンダリングされるページをレイアウトします。</span><span class="sxs-lookup"><span data-stu-id="3d57d-142">By default, the body and all sections in a content page must all be rendered by the layout page.</span></span> <span data-ttu-id="3d57d-143">Razor ビュー エンジンは、本文と各セクションで表示されているかどうかを追跡することによってこれを適用します。</span><span class="sxs-lookup"><span data-stu-id="3d57d-143">The Razor view engine enforces this by tracking whether the body and each section have been rendered.</span></span>

<span data-ttu-id="3d57d-144">セクションでは、本文を無視する、ビュー エンジンを指示する、`IgnoreBody`と`IgnoreSection`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="3d57d-144">To instruct the view engine to ignore the body or sections, call the `IgnoreBody` and `IgnoreSection` methods.</span></span>

<span data-ttu-id="3d57d-145">本文と Razor ページで、すべてのセクション必要があるレンダリングまたは無視します。</span><span class="sxs-lookup"><span data-stu-id="3d57d-145">The body and every section in a Razor page must be either rendered or ignored.</span></span>

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a><span data-ttu-id="3d57d-146">共有ディレクティブのインポート</span><span class="sxs-lookup"><span data-stu-id="3d57d-146">Importing Shared Directives</span></span>

<span data-ttu-id="3d57d-147">ビューは、Razor ディレクティブを使用して名前空間をインポートまたはを実行するなど、多くの作業を行う[依存性の注入](dependency-injection.md)です。</span><span class="sxs-lookup"><span data-stu-id="3d57d-147">Views can use Razor directives to do many things, such as importing namespaces or performing [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="3d57d-148">共通の多くのビューで共有されるディレクティブを指定することがあります`_ViewImports.cshtml`ファイル。</span><span class="sxs-lookup"><span data-stu-id="3d57d-148">Directives shared by many views may be specified in a common `_ViewImports.cshtml` file.</span></span> <span data-ttu-id="3d57d-149">`_ViewImports`ファイルは、次のディレクティブをサポートします。</span><span class="sxs-lookup"><span data-stu-id="3d57d-149">The `_ViewImports` file supports the following directives:</span></span>

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

<span data-ttu-id="3d57d-150">ファイルは、関数とセクションの定義などの Razor 機能をサポートしません。</span><span class="sxs-lookup"><span data-stu-id="3d57d-150">The file does not support other Razor features, such as functions and section definitions.</span></span>

<span data-ttu-id="3d57d-151">サンプル`_ViewImports.cshtml`ファイル。</span><span class="sxs-lookup"><span data-stu-id="3d57d-151">A sample `_ViewImports.cshtml` file:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

<span data-ttu-id="3d57d-152">`_ViewImports.cshtml` ASP.NET Core MVC アプリは、一般に配置のファイル、`Views`フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="3d57d-152">The `_ViewImports.cshtml` file for an ASP.NET Core MVC app is typically placed in the `Views` folder.</span></span> <span data-ttu-id="3d57d-153">A`_ViewImports.cshtml`場合にのみ適用されますが、そのフォルダーとそのサブフォルダー内ビューを任意のフォルダー内でファイルを配置することができます。</span><span class="sxs-lookup"><span data-stu-id="3d57d-153">A `_ViewImports.cshtml` file can be placed within any folder, in which case it will only be applied to views within that folder and its subfolders.</span></span> <span data-ttu-id="3d57d-154">`_ViewImports`ファイルのルート レベルでは、起動処理がおり、しに至るまでの各フォルダーについて自体には、ビューの場所のため、ルート レベルの設定を指定オーバーライド フォルダー レベルでします。</span><span class="sxs-lookup"><span data-stu-id="3d57d-154">`_ViewImports` files are processed starting at the root level, and then for each folder leading up to the location of the view itself, so settings specified at the root level may be overridden at the folder level.</span></span>

<span data-ttu-id="3d57d-155">たとえば、ルート レベル`_ViewImports.cshtml`ファイルを指定`@model`と`@addTagHelper`、別および`_ViewImports.cshtml`ビューのコント ローラーに関連付けられているフォルダー内のファイルが、さまざまなを指定します`@model`し、別の追加`@addTagHelper`、ビュー両方のタグ ヘルパーにはアクセスおよび後者を使用して、`@model`です。</span><span class="sxs-lookup"><span data-stu-id="3d57d-155">For example, if a root level `_ViewImports.cshtml` file specifies `@model` and `@addTagHelper`, and another `_ViewImports.cshtml` file in the controller-associated folder of the view specifies a different `@model` and adds another `@addTagHelper`, the view will have access to both tag helpers and will use the latter `@model`.</span></span>

<span data-ttu-id="3d57d-156">複数`_ViewImports.cshtml`ファイル ビューの実行に含まれるディレクティブの動作を組み合わせて、`ViewImports.cshtml`ファイルに次のようになります。</span><span class="sxs-lookup"><span data-stu-id="3d57d-156">If multiple `_ViewImports.cshtml` files are run for a view, combined behavior of the directives included in the `ViewImports.cshtml` files will be as follows:</span></span>

* <span data-ttu-id="3d57d-157">`@addTagHelper`、 `@removeTagHelper`: 順序で、すべての実行</span><span class="sxs-lookup"><span data-stu-id="3d57d-157">`@addTagHelper`, `@removeTagHelper`: all run, in order</span></span>

* <span data-ttu-id="3d57d-158">`@tagHelperPrefix`: ビューに最も近いものでは、その他のものよりも優先</span><span class="sxs-lookup"><span data-stu-id="3d57d-158">`@tagHelperPrefix`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="3d57d-159">`@model`: ビューに最も近いものでは、その他のものよりも優先</span><span class="sxs-lookup"><span data-stu-id="3d57d-159">`@model`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="3d57d-160">`@inherits`: ビューに最も近いものでは、その他のものよりも優先</span><span class="sxs-lookup"><span data-stu-id="3d57d-160">`@inherits`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="3d57d-161">`@using`: すべてが含まれます。重複部分は無視されます。</span><span class="sxs-lookup"><span data-stu-id="3d57d-161">`@using`: all are included; duplicates are ignored</span></span>

* <span data-ttu-id="3d57d-162">`@inject`: 各プロパティのビューに最も近いものよりも優先、それ以外の、同じプロパティ名</span><span class="sxs-lookup"><span data-stu-id="3d57d-162">`@inject`: for each property, the closest one to the view overrides any others with the same property name</span></span>

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a><span data-ttu-id="3d57d-163">各ビューの前にコードを実行しています。</span><span class="sxs-lookup"><span data-stu-id="3d57d-163">Running Code Before Each View</span></span>

<span data-ttu-id="3d57d-164">すべてのビューの前に実行する必要があります、これは、コードがある場合、`_ViewStart.cshtml`ファイル。</span><span class="sxs-lookup"><span data-stu-id="3d57d-164">If you have code you need to run before every view, this should be placed in the `_ViewStart.cshtml` file.</span></span> <span data-ttu-id="3d57d-165">慣例により、`_ViewStart.cshtml`ファイルにある、`Views`フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="3d57d-165">By convention, the `_ViewStart.cshtml` file is located in the `Views` folder.</span></span> <span data-ttu-id="3d57d-166">表示されているステートメント`_ViewStart.cshtml`すべて完全のビュー (いないレイアウト、およびいない部分ビュー) の前に実行されます。</span><span class="sxs-lookup"><span data-stu-id="3d57d-166">The statements listed in `_ViewStart.cshtml` are run before every full view (not layouts, and not partial views).</span></span> <span data-ttu-id="3d57d-167">同様に[ViewImports.cshtml](xref:mvc/views/layout#viewimports)、`_ViewStart.cshtml`は階層構造です。</span><span class="sxs-lookup"><span data-stu-id="3d57d-167">Like [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` is hierarchical.</span></span> <span data-ttu-id="3d57d-168">場合、`_ViewStart.cshtml`コント ローラーに関連付けられている表示フォルダーで定義されたファイルは、これは、1 つのルートで定義されている後に実行されます、`Views`フォルダー (存在する場合)。</span><span class="sxs-lookup"><span data-stu-id="3d57d-168">If a `_ViewStart.cshtml` file is defined in the controller-associated view folder, it will be run after the one defined in the root of the `Views` folder (if any).</span></span>

<span data-ttu-id="3d57d-169">サンプル`_ViewStart.cshtml`ファイル。</span><span class="sxs-lookup"><span data-stu-id="3d57d-169">A sample `_ViewStart.cshtml` file:</span></span>

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

<span data-ttu-id="3d57d-170">上記のファイルは、すべてのビューが使用することを指定します、`_Layout.cshtml`レイアウトです。</span><span class="sxs-lookup"><span data-stu-id="3d57d-170">The file above specifies that all views will use the `_Layout.cshtml` layout.</span></span>

> [!NOTE]
> <span data-ttu-id="3d57d-171">どちらも`_ViewStart.cshtml`も`_ViewImports.cshtml`では、通常、`/Views/Shared`フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="3d57d-171">Neither `_ViewStart.cshtml` nor `_ViewImports.cshtml` are typically placed in the `/Views/Shared` folder.</span></span> <span data-ttu-id="3d57d-172">これらのファイルのアプリケーション レベルのバージョンに直接配置する必要があります、`/Views`フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="3d57d-172">The app-level versions of these files should be placed directly in the `/Views` folder.</span></span>
