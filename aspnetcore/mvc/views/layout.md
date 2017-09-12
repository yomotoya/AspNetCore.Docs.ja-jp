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
ms.openlocfilehash: 25aa5fc730d9076fdcf9d29cb5d9dfa75a246a1a
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="layout"></a><span data-ttu-id="b2873-103">レイアウト</span><span class="sxs-lookup"><span data-stu-id="b2873-103">Layout</span></span>

<span data-ttu-id="b2873-104">によって[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="b2873-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="b2873-105">ビューは、ビジュアル化およびプログラムの要素を頻繁に共有します。</span><span class="sxs-lookup"><span data-stu-id="b2873-105">Views frequently share visual and programmatic elements.</span></span> <span data-ttu-id="b2873-106">この記事では、一般的なレイアウトを使用して、共有するディレクティブ、および ASP.NET アプリのビューのレンダリングの前に共通のコードを実行する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="b2873-106">In this article, you'll learn how to use common layouts, share directives, and run common code before rendering views in your ASP.NET app.</span></span>

## <a name="what-is-a-layout"></a><span data-ttu-id="b2873-107">レイアウトします。</span><span class="sxs-lookup"><span data-stu-id="b2873-107">What is a Layout</span></span>

<span data-ttu-id="b2873-108">ほとんどの web アプリでは、ページ間を移動するのに合わせて一貫した使用環境をユーザーに提供する一般的なレイアウトがあります。</span><span class="sxs-lookup"><span data-stu-id="b2873-108">Most web apps have a common layout that provides the user with a consistent experience as they navigate from page to page.</span></span> <span data-ttu-id="b2873-109">レイアウトには、通常、アプリのヘッダー、ナビゲーションまたはメニュー要素、およびフッターなどの一般的なユーザー インターフェイス要素が含まれます。</span><span class="sxs-lookup"><span data-stu-id="b2873-109">The layout typically includes common user interface elements such as the app header, navigation or menu elements, and footer.</span></span>

![ページ レイアウトの例](layout/_static/page-layout.png)

<span data-ttu-id="b2873-111">スクリプトとスタイル シートなどの共通の HTML 構造体は、アプリ内の多数のページでも頻繁に使用されます。</span><span class="sxs-lookup"><span data-stu-id="b2873-111">Common HTML structures such as scripts and stylesheets are also frequently used by many pages within an app.</span></span> <span data-ttu-id="b2873-112">これらの共有要素のすべての環境で定義されている可能性があります、*レイアウト*ファイルで、アプリ内で使用される任意のビューで参照できます。</span><span class="sxs-lookup"><span data-stu-id="b2873-112">All of these shared elements may be defined in a *layout* file, which can then be referenced by any view used within the app.</span></span> <span data-ttu-id="b2873-113">レイアウト削減重複コード ビュー、支援をフォロー、[しない繰り返します自分で (ドライ) 原則](http://deviq.com/don-t-repeat-yourself/)です。</span><span class="sxs-lookup"><span data-stu-id="b2873-113">Layouts reduce duplicate code in views, helping them follow the [Don't Repeat Yourself (DRY) principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="b2873-114">慣例により、ASP.NET アプリケーションの既定のレイアウトが名前付き`_Layout.cshtml`します。</span><span class="sxs-lookup"><span data-stu-id="b2873-114">By convention, the default layout for an ASP.NET app is named `_Layout.cshtml`.</span></span> <span data-ttu-id="b2873-115">Visual Studio ASP.NET Core の MVC プロジェクト テンプレートには、このレイアウト ファイルが含まれています、`Views/Shared`フォルダー。</span><span class="sxs-lookup"><span data-stu-id="b2873-115">The Visual Studio ASP.NET Core MVC project template includes this layout file in the `Views/Shared` folder:</span></span>

![ソリューション エクスプ ローラー ビュー フォルダー](layout/_static/web-project-views.png)

<span data-ttu-id="b2873-117">このレイアウトは、アプリでのビューの最上位レベルのテンプレートを定義します。</span><span class="sxs-lookup"><span data-stu-id="b2873-117">This layout defines a top level template for views in the app.</span></span> <span data-ttu-id="b2873-118">アプリには、レイアウトは不要、アプリは、さまざまなレイアウトを指定するさまざまなビューで複数のレイアウトを定義できます。</span><span class="sxs-lookup"><span data-stu-id="b2873-118">Apps do not require a layout, and apps can define more than one layout, with different views specifying different layouts.</span></span>

<span data-ttu-id="b2873-119">たとえば`_Layout.cshtml`:</span><span class="sxs-lookup"><span data-stu-id="b2873-119">An example `_Layout.cshtml`:</span></span>

<span data-ttu-id="b2873-120">[!code-html[Main](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]</span><span class="sxs-lookup"><span data-stu-id="b2873-120">[!code-html[Main](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]</span></span>

## <a name="specifying-a-layout"></a><span data-ttu-id="b2873-121">レイアウトの指定</span><span class="sxs-lookup"><span data-stu-id="b2873-121">Specifying a Layout</span></span>

<span data-ttu-id="b2873-122">Razor ビューが、`Layout`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="b2873-122">Razor views have a `Layout` property.</span></span> <span data-ttu-id="b2873-123">個々 のビューは、このプロパティを設定して、レイアウトを指定します。</span><span class="sxs-lookup"><span data-stu-id="b2873-123">Individual views specify a layout by setting this property:</span></span>

<span data-ttu-id="b2873-124">[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="b2873-124">[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]</span></span>

<span data-ttu-id="b2873-125">指定したレイアウトは、完全なパスを使用できます (例: `/Views/Shared/_Layout.cshtml`) または部分的な名前 (例: `_Layout`)。</span><span class="sxs-lookup"><span data-stu-id="b2873-125">The layout specified can use a full path (example: `/Views/Shared/_Layout.cshtml`) or a partial name (example: `_Layout`).</span></span> <span data-ttu-id="b2873-126">部分的な名前を指定すると、Razor ビュー エンジンは、標準の探索プロセスを使用して、レイアウト ファイルで検索します。</span><span class="sxs-lookup"><span data-stu-id="b2873-126">When a partial name is provided, the Razor view engine will search for the layout file using its standard discovery process.</span></span> <span data-ttu-id="b2873-127">コント ローラーに関連付けられているフォルダーが最初に検索後に、`Shared`フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="b2873-127">The controller-associated folder is searched first, followed by the `Shared` folder.</span></span> <span data-ttu-id="b2873-128">この検出プロセスが検出に使用したものと同じ[部分ビュー](partial.md)です。</span><span class="sxs-lookup"><span data-stu-id="b2873-128">This discovery process is identical to the one used to discover [partial views](partial.md).</span></span>

<span data-ttu-id="b2873-129">既定では、すべてのレイアウトを呼び出す必要があります`RenderBody`です。</span><span class="sxs-lookup"><span data-stu-id="b2873-129">By default, every layout must call `RenderBody`.</span></span> <span data-ttu-id="b2873-130">任意の場所への呼び出し`RenderBody`が配置されると、ビューの内容が表示されます。</span><span class="sxs-lookup"><span data-stu-id="b2873-130">Wherever the call to `RenderBody` is placed, the contents of the view will be rendered.</span></span>

<a name=layout-sections-label></a>

### <a name="sections"></a><span data-ttu-id="b2873-131">セクション</span><span class="sxs-lookup"><span data-stu-id="b2873-131">Sections</span></span>

<span data-ttu-id="b2873-132">レイアウトは、必要に応じて 1 つまたは複数を参照できます*セクション*、呼び出すことによって`RenderSection`です。</span><span class="sxs-lookup"><span data-stu-id="b2873-132">A layout can optionally reference one or more *sections*, by calling `RenderSection`.</span></span> <span data-ttu-id="b2873-133">セクションでは、特定のページ要素の配置場所を整理することを示します。</span><span class="sxs-lookup"><span data-stu-id="b2873-133">Sections provide a way to organize where certain page elements should be placed.</span></span> <span data-ttu-id="b2873-134">各呼び出し`RenderSection`そのセクションが必須またはオプションかどうかを指定できます。</span><span class="sxs-lookup"><span data-stu-id="b2873-134">Each call to `RenderSection` can specify whether that section is required or optional.</span></span> <span data-ttu-id="b2873-135">必要なセクションが見つからない場合、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="b2873-135">If a required section is not found, an exception will be thrown.</span></span> <span data-ttu-id="b2873-136">個々 のビューを使用して、セクション内に表示するコンテンツを指定する、 `@section` Razor 構文です。</span><span class="sxs-lookup"><span data-stu-id="b2873-136">Individual views specify the content to be rendered within a section using the `@section` Razor syntax.</span></span> <span data-ttu-id="b2873-137">ビューには、セクションが定義されている場合、表示する必要があります (または、エラーが発生)。</span><span class="sxs-lookup"><span data-stu-id="b2873-137">If a view defines a section, it must be rendered (or an error will occur).</span></span>

<span data-ttu-id="b2873-138">たとえば`@section`ビューで定義します。</span><span class="sxs-lookup"><span data-stu-id="b2873-138">An example `@section` definition in a view:</span></span>

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

<span data-ttu-id="b2873-139">上記のコードに検証スクリプトを追加、`scripts`セクションのフォームを含むビュー。</span><span class="sxs-lookup"><span data-stu-id="b2873-139">In the code above, validation scripts are added to the `scripts` section on a view that includes a form.</span></span> <span data-ttu-id="b2873-140">同じアプリケーション内の他のビューは、追加のスクリプトは必要ありませんがおよびためスクリプトのセクションを定義する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="b2873-140">Other views in the same application might not require any additional scripts, and so wouldn't need to define a scripts section.</span></span>

<span data-ttu-id="b2873-141">ビューで定義されたセクションは、イミディ エイト レイアウト ページでのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="b2873-141">Sections defined in a view are available only in its immediate layout page.</span></span> <span data-ttu-id="b2873-142">パーシャル、コンポーネントの表示、またはシステムの表示の他の部分からは参照できません。</span><span class="sxs-lookup"><span data-stu-id="b2873-142">They cannot be referenced from partials, view components, or other parts of the view system.</span></span>

### <a name="ignoring-sections"></a><span data-ttu-id="b2873-143">セクションでは無視されます。</span><span class="sxs-lookup"><span data-stu-id="b2873-143">Ignoring sections</span></span>

<span data-ttu-id="b2873-144">既定では、本体およびコンテンツ ページのすべてのセクション必要がありますすべてによってレンダリングされるページをレイアウトします。</span><span class="sxs-lookup"><span data-stu-id="b2873-144">By default, the body and all sections in a content page must all be rendered by the layout page.</span></span> <span data-ttu-id="b2873-145">Razor ビュー エンジンは、本文と各セクションで表示されているかどうかを追跡することによってこれを適用します。</span><span class="sxs-lookup"><span data-stu-id="b2873-145">The Razor view engine enforces this by tracking whether the body and each section have been rendered.</span></span>

<span data-ttu-id="b2873-146">セクションでは、本文を無視する、ビュー エンジンを指示する、`IgnoreBody`と`IgnoreSection`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="b2873-146">To instruct the view engine to ignore the body or sections, call the `IgnoreBody` and `IgnoreSection` methods.</span></span>

<span data-ttu-id="b2873-147">本文と Razor ページで、すべてのセクション必要があるレンダリングまたは無視します。</span><span class="sxs-lookup"><span data-stu-id="b2873-147">The body and every section in a Razor page must be either rendered or ignored.</span></span>

<a name=viewimports></a>

## <a name="importing-shared-directives"></a><span data-ttu-id="b2873-148">共有ディレクティブのインポート</span><span class="sxs-lookup"><span data-stu-id="b2873-148">Importing Shared Directives</span></span>

<span data-ttu-id="b2873-149">ビューは、Razor ディレクティブを使用して名前空間をインポートまたはを実行するなど、多くの作業を行う[依存性の注入](dependency-injection.md)です。</span><span class="sxs-lookup"><span data-stu-id="b2873-149">Views can use Razor directives to do many things, such as importing namespaces or performing [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="b2873-150">共通の多くのビューで共有されるディレクティブを指定することがあります`_ViewImports.cshtml`ファイル。</span><span class="sxs-lookup"><span data-stu-id="b2873-150">Directives shared by many views may be specified in a common `_ViewImports.cshtml` file.</span></span> <span data-ttu-id="b2873-151">`_ViewImports`ファイルは、次のディレクティブをサポートします。</span><span class="sxs-lookup"><span data-stu-id="b2873-151">The `_ViewImports` file supports the following directives:</span></span>

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

<span data-ttu-id="b2873-152">ファイルは、関数とセクションの定義などの Razor 機能をサポートしません。</span><span class="sxs-lookup"><span data-stu-id="b2873-152">The file does not support other Razor features, such as functions and section definitions.</span></span>

<span data-ttu-id="b2873-153">サンプル`_ViewImports.cshtml`ファイル。</span><span class="sxs-lookup"><span data-stu-id="b2873-153">A sample `_ViewImports.cshtml` file:</span></span>

<span data-ttu-id="b2873-154">[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="b2873-154">[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]</span></span>

<span data-ttu-id="b2873-155">`_ViewImports.cshtml` ASP.NET Core MVC アプリは、一般に配置のファイル、`Views`フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="b2873-155">The `_ViewImports.cshtml` file for an ASP.NET Core MVC app is typically placed in the `Views` folder.</span></span> <span data-ttu-id="b2873-156">A`_ViewImports.cshtml`場合にのみ適用されますが、そのフォルダーとそのサブフォルダー内ビューを任意のフォルダー内でファイルを配置することができます。</span><span class="sxs-lookup"><span data-stu-id="b2873-156">A `_ViewImports.cshtml` file can be placed within any folder, in which case it will only be applied to views within that folder and its subfolders.</span></span> <span data-ttu-id="b2873-157">`_ViewImports`ファイルのルート レベルでは、起動処理がおり、しに至るまでの各フォルダーについて自体には、ビューの場所のため、ルート レベルの設定を指定オーバーライド フォルダー レベルでします。</span><span class="sxs-lookup"><span data-stu-id="b2873-157">`_ViewImports` files are processed starting at the root level, and then for each folder leading up to the location of the view itself, so settings specified at the root level may be overridden at the folder level.</span></span>

<span data-ttu-id="b2873-158">たとえば、ルート レベル`_ViewImports.cshtml`ファイルを指定`@model`と`@addTagHelper`、別および`_ViewImports.cshtml`ビューのコント ローラーに関連付けられているフォルダー内のファイルが、さまざまなを指定します`@model`し、別の追加`@addTagHelper`、ビュー両方のタグ ヘルパーにはアクセスおよび後者を使用して、`@model`です。</span><span class="sxs-lookup"><span data-stu-id="b2873-158">For example, if a root level `_ViewImports.cshtml` file specifies `@model` and `@addTagHelper`, and another `_ViewImports.cshtml` file in the controller-associated folder of the view specifies a different `@model` and adds another `@addTagHelper`, the view will have access to both tag helpers and will use the latter `@model`.</span></span>

<span data-ttu-id="b2873-159">複数`_ViewImports.cshtml`ファイル ビューの実行に含まれるディレクティブの動作を組み合わせて、`ViewImports.cshtml`ファイルに次のようになります。</span><span class="sxs-lookup"><span data-stu-id="b2873-159">If multiple `_ViewImports.cshtml` files are run for a view, combined behavior of the directives included in the `ViewImports.cshtml` files will be as follows:</span></span>

* <span data-ttu-id="b2873-160">`@addTagHelper`、 `@removeTagHelper`: 順序で、すべての実行</span><span class="sxs-lookup"><span data-stu-id="b2873-160">`@addTagHelper`, `@removeTagHelper`: all run, in order</span></span>

* <span data-ttu-id="b2873-161">`@tagHelperPrefix`: ビューに最も近いものでは、その他のものよりも優先</span><span class="sxs-lookup"><span data-stu-id="b2873-161">`@tagHelperPrefix`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="b2873-162">`@model`: ビューに最も近いものでは、その他のものよりも優先</span><span class="sxs-lookup"><span data-stu-id="b2873-162">`@model`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="b2873-163">`@inherits`: ビューに最も近いものでは、その他のものよりも優先</span><span class="sxs-lookup"><span data-stu-id="b2873-163">`@inherits`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="b2873-164">`@using`: すべてが含まれます。重複部分は無視されます。</span><span class="sxs-lookup"><span data-stu-id="b2873-164">`@using`: all are included; duplicates are ignored</span></span>

* <span data-ttu-id="b2873-165">`@inject`: 各プロパティのビューに最も近いものよりも優先、それ以外の、同じプロパティ名</span><span class="sxs-lookup"><span data-stu-id="b2873-165">`@inject`: for each property, the closest one to the view overrides any others with the same property name</span></span>

<a name=viewstart></a>

## <a name="running-code-before-each-view"></a><span data-ttu-id="b2873-166">各ビューの前にコードを実行しています。</span><span class="sxs-lookup"><span data-stu-id="b2873-166">Running Code Before Each View</span></span>

<span data-ttu-id="b2873-167">すべてのビューの前に実行する必要があります、これは、コードがある場合、`_ViewStart.cshtml`ファイル。</span><span class="sxs-lookup"><span data-stu-id="b2873-167">If you have code you need to run before every view, this should be placed in the `_ViewStart.cshtml` file.</span></span> <span data-ttu-id="b2873-168">慣例により、`_ViewStart.cshtml`ファイルにある、`Views`フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="b2873-168">By convention, the `_ViewStart.cshtml` file is located in the `Views` folder.</span></span> <span data-ttu-id="b2873-169">表示されているステートメント`_ViewStart.cshtml`すべて完全のビュー (いないレイアウト、およびいない部分ビュー) の前に実行されます。</span><span class="sxs-lookup"><span data-stu-id="b2873-169">The statements listed in `_ViewStart.cshtml` are run before every full view (not layouts, and not partial views).</span></span> <span data-ttu-id="b2873-170">同様に[ViewImports.cshtml](xref:mvc/views/layout#viewimports)、`_ViewStart.cshtml`は階層構造です。</span><span class="sxs-lookup"><span data-stu-id="b2873-170">Like [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` is hierarchical.</span></span> <span data-ttu-id="b2873-171">場合、`_ViewStart.cshtml`コント ローラーに関連付けられている表示フォルダーで定義されたファイルは、これは、1 つのルートで定義されている後に実行されます、`Views`フォルダー (存在する場合)。</span><span class="sxs-lookup"><span data-stu-id="b2873-171">If a `_ViewStart.cshtml` file is defined in the controller-associated view folder, it will be run after the one defined in the root of the `Views` folder (if any).</span></span>

<span data-ttu-id="b2873-172">サンプル`_ViewStart.cshtml`ファイル。</span><span class="sxs-lookup"><span data-stu-id="b2873-172">A sample `_ViewStart.cshtml` file:</span></span>

<span data-ttu-id="b2873-173">[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="b2873-173">[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]</span></span>

<span data-ttu-id="b2873-174">上記のファイルは、すべてのビューが使用することを指定します、`_Layout.cshtml`レイアウトです。</span><span class="sxs-lookup"><span data-stu-id="b2873-174">The file above specifies that all views will use the `_Layout.cshtml` layout.</span></span>

> [!NOTE]
> <span data-ttu-id="b2873-175">どちらも`_ViewStart.cshtml`も`_ViewImports.cshtml`では、通常、`/Views/Shared`フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="b2873-175">Neither `_ViewStart.cshtml` nor `_ViewImports.cshtml` are typically placed in the `/Views/Shared` folder.</span></span> <span data-ttu-id="b2873-176">これらのファイルのアプリケーション レベルのバージョンに直接配置する必要があります、`/Views`フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="b2873-176">The app-level versions of these files should be placed directly in the `/Views` folder.</span></span>
