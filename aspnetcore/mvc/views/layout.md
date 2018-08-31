---
title: ASP.NET Core でのレイアウト
author: ardalis
description: 共通レイアウトの使用方法、ディレクティブの共有方法、および ASP.NET Core アプリでビューをレンダリングする前に共通コードを実行する方法について説明します。
ms.author: riande
ms.date: 10/14/2016
uid: mvc/views/layout
ms.openlocfilehash: ad0b339572f387be8a636204015ffc361947acb8
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751692"
---
# <a name="layout-in-aspnet-core"></a><span data-ttu-id="979e4-103">ASP.NET Core でのレイアウト</span><span class="sxs-lookup"><span data-stu-id="979e4-103">Layout in ASP.NET Core</span></span>

<span data-ttu-id="979e4-104">作成者: [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="979e4-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="979e4-105">ビューは多くの場合、ビジュアルおよびプログラムの要素を共有します。</span><span class="sxs-lookup"><span data-stu-id="979e4-105">Views frequently share visual and programmatic elements.</span></span> <span data-ttu-id="979e4-106">この記事では、共通レイアウトの使用方法、ディレクティブの共有方法、および ASP.NET Core アプリでビューをレンダリングする前に共通コードを実行する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="979e4-106">In this article, you'll learn how to use common layouts, share directives, and run common code before rendering views in your ASP.NET Core app.</span></span>

## <a name="what-is-a-layout"></a><span data-ttu-id="979e4-107">レイアウトとは</span><span class="sxs-lookup"><span data-stu-id="979e4-107">What is a Layout</span></span>

<span data-ttu-id="979e4-108">ほとんどの Web アプリには、ユーザーがページ間を移動する際に一貫性のあるエクスペリエンスを提供する共通レイアウトがあります。</span><span class="sxs-lookup"><span data-stu-id="979e4-108">Most web apps have a common layout that provides the user with a consistent experience as they navigate from page to page.</span></span> <span data-ttu-id="979e4-109">通常、このレイアウトには、アプリのヘッダー、ナビゲーションまたはメニュー要素、フッターなどの共通のユーザー インターフェイス要素が含まれます。</span><span class="sxs-lookup"><span data-stu-id="979e4-109">The layout typically includes common user interface elements such as the app header, navigation or menu elements, and footer.</span></span>

![ページ レイアウトの例](layout/_static/page-layout.png)

<span data-ttu-id="979e4-111">スクリプトやスタイルシートなどの共通の HTML 構造体も、アプリ内の多くのページでよく使用されます。</span><span class="sxs-lookup"><span data-stu-id="979e4-111">Common HTML structures such as scripts and stylesheets are also frequently used by many pages within an app.</span></span> <span data-ttu-id="979e4-112">これらの共有要素をすべて *layout* ファイルで定義することで、アプリ内で使用する任意のビューで参照できるようになります。</span><span class="sxs-lookup"><span data-stu-id="979e4-112">All of these shared elements may be defined in a *layout* file, which can then be referenced by any view used within the app.</span></span> <span data-ttu-id="979e4-113">レイアウトは、[DRY (Don't Repeat Yourself) 原則](http://deviq.com/don-t-repeat-yourself/)に従って、ビュー内の重複するコードを削減します。</span><span class="sxs-lookup"><span data-stu-id="979e4-113">Layouts reduce duplicate code in views, helping them follow the [Don't Repeat Yourself (DRY) principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="979e4-114">規則により、ASP.NET Core アプリの既定のレイアウトには `_Layout.cshtml` という名前が付けられます。</span><span class="sxs-lookup"><span data-stu-id="979e4-114">By convention, the default layout for an ASP.NET Core app is named `_Layout.cshtml`.</span></span> <span data-ttu-id="979e4-115">Visual Studio ASP.NET Core MVC プロジェクト テンプレートには、このレイアウト ファイルが `Views/Shared` フォルダーに含まれています。</span><span class="sxs-lookup"><span data-stu-id="979e4-115">The Visual Studio ASP.NET Core MVC project template includes this layout file in the `Views/Shared` folder:</span></span>

![ソリューション エクスプローラーの Views フォルダー](layout/_static/web-project-views.png)

<span data-ttu-id="979e4-117">このレイアウトは、アプリのビューの最上位のテンプレートを定義します。</span><span class="sxs-lookup"><span data-stu-id="979e4-117">This layout defines a top level template for views in the app.</span></span> <span data-ttu-id="979e4-118">アプリはレイアウトを必要とせず、それぞれ異なるレイアウトを指定するさまざまなビューを使用して、複数のレイアウトを定義できます。</span><span class="sxs-lookup"><span data-stu-id="979e4-118">Apps don't require a layout, and apps can define more than one layout, with different views specifying different layouts.</span></span>

<span data-ttu-id="979e4-119">`_Layout.cshtml` の例:</span><span class="sxs-lookup"><span data-stu-id="979e4-119">An example `_Layout.cshtml`:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a><span data-ttu-id="979e4-120">レイアウトの指定</span><span class="sxs-lookup"><span data-stu-id="979e4-120">Specifying a Layout</span></span>

<span data-ttu-id="979e4-121">Razor ビューには `Layout` プロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="979e4-121">Razor views have a `Layout` property.</span></span> <span data-ttu-id="979e4-122">個々のビューは、このプロパティを設定することでレイアウトを指定します。</span><span class="sxs-lookup"><span data-stu-id="979e4-122">Individual views specify a layout by setting this property:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

<span data-ttu-id="979e4-123">指定されたレイアウトは、完全なパス (例: `/Views/Shared/_Layout.cshtml`) または部分的な名前 (例: `_Layout`) を使用できます。</span><span class="sxs-lookup"><span data-stu-id="979e4-123">The layout specified can use a full path (example: `/Views/Shared/_Layout.cshtml`) or a partial name (example: `_Layout`).</span></span> <span data-ttu-id="979e4-124">部分的な名前を指定すると、Razor ビュー エンジンが標準の検出プロセスを使用して、レイアウト ファイルを検索します。</span><span class="sxs-lookup"><span data-stu-id="979e4-124">When a partial name is provided, the Razor view engine will search for the layout file using its standard discovery process.</span></span> <span data-ttu-id="979e4-125">コントローラーに関連付けられているフォルダーが最初に検索され、次に `Shared` フォルダーが検索されます。</span><span class="sxs-lookup"><span data-stu-id="979e4-125">The controller-associated folder is searched first, followed by the `Shared` folder.</span></span> <span data-ttu-id="979e4-126">この検出プロセスは、[部分ビュー](partial.md)の検出に使用されるのと同じプロセスです。</span><span class="sxs-lookup"><span data-stu-id="979e4-126">This discovery process is identical to the one used to discover [partial views](partial.md).</span></span>

<span data-ttu-id="979e4-127">既定では、すべてのレイアウトで `RenderBody` を呼び出す必要があります。</span><span class="sxs-lookup"><span data-stu-id="979e4-127">By default, every layout must call `RenderBody`.</span></span> <span data-ttu-id="979e4-128">`RenderBody` への呼び出しが配置されると、ビューのコンテンツがレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="979e4-128">Wherever the call to `RenderBody` is placed, the contents of the view will be rendered.</span></span>

<a name="layout-sections-label"></a>

### <a name="sections"></a><span data-ttu-id="979e4-129">セクション</span><span class="sxs-lookup"><span data-stu-id="979e4-129">Sections</span></span>

<span data-ttu-id="979e4-130">レイアウトは、必要に応じて `RenderSection` を呼び出すことで、1 つ以上の*セクション*を参照することができます。</span><span class="sxs-lookup"><span data-stu-id="979e4-130">A layout can optionally reference one or more *sections*, by calling `RenderSection`.</span></span> <span data-ttu-id="979e4-131">セクションは、特定のページ要素の配置場所を整理する方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="979e4-131">Sections provide a way to organize where certain page elements should be placed.</span></span> <span data-ttu-id="979e4-132">`RenderSection` の呼び出しごとに、そのセクションを必須またはオプションにするかどうかを指定できます。</span><span class="sxs-lookup"><span data-stu-id="979e4-132">Each call to `RenderSection` can specify whether that section is required or optional.</span></span> <span data-ttu-id="979e4-133">必須のセクションが見つからない場合、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="979e4-133">If a required section isn't found, an exception will be thrown.</span></span> <span data-ttu-id="979e4-134">個々のビューは、`@section` Razor 構文を使用して、セクション内にレンダリングされるコンテンツを指定します。</span><span class="sxs-lookup"><span data-stu-id="979e4-134">Individual views specify the content to be rendered within a section using the `@section` Razor syntax.</span></span> <span data-ttu-id="979e4-135">ビューでセクションを定義する場合は、レンダリングされる必要があります (そうしないと、エラーが発生します)。</span><span class="sxs-lookup"><span data-stu-id="979e4-135">If a view defines a section, it must be rendered (or an error will occur).</span></span>

<span data-ttu-id="979e4-136">ビューでの `@section` 定義の例:</span><span class="sxs-lookup"><span data-stu-id="979e4-136">An example `@section` definition in a view:</span></span>

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

<span data-ttu-id="979e4-137">上記のコードでは、フォームを含むビューの `scripts` セクションに検証スクリプトが追加されます。</span><span class="sxs-lookup"><span data-stu-id="979e4-137">In the code above, validation scripts are added to the `scripts` section on a view that includes a form.</span></span> <span data-ttu-id="979e4-138">同じアプリケーション内の他のビューに追加のスクリプトが必要ない場合は、スクリプト セクションを定義する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="979e4-138">Other views in the same application might not require any additional scripts, and so wouldn't need to define a scripts section.</span></span>

<span data-ttu-id="979e4-139">ビューで定義されたセクションは、そのイミディエイト レイアウト ページでのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="979e4-139">Sections defined in a view are available only in its immediate layout page.</span></span> <span data-ttu-id="979e4-140">これらは、部分、ビュー コンポーネント、またはビュー システムの他の部分からは参照できません。</span><span class="sxs-lookup"><span data-stu-id="979e4-140">They cannot be referenced from partials, view components, or other parts of the view system.</span></span>

### <a name="ignoring-sections"></a><span data-ttu-id="979e4-141">セクションの無視</span><span class="sxs-lookup"><span data-stu-id="979e4-141">Ignoring sections</span></span>

<span data-ttu-id="979e4-142">既定では、コンテンツ ページの本文とすべてのセクションがレイアウト ページですべてレンダリングされる必要があります。</span><span class="sxs-lookup"><span data-stu-id="979e4-142">By default, the body and all sections in a content page must all be rendered by the layout page.</span></span> <span data-ttu-id="979e4-143">Razor ビュー エンジンは、本文と各セクションがレンダリングされているかどうかを追跡することによってこれを実行します。</span><span class="sxs-lookup"><span data-stu-id="979e4-143">The Razor view engine enforces this by tracking whether the body and each section have been rendered.</span></span>

<span data-ttu-id="979e4-144">本文またはセクションを無視するようにビュー エンジンに指示するには、`IgnoreBody` メソッドと `IgnoreSection` メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="979e4-144">To instruct the view engine to ignore the body or sections, call the `IgnoreBody` and `IgnoreSection` methods.</span></span>

<span data-ttu-id="979e4-145">Razor ページ内の本文とすべてのセクションは、レンダリングされるか無視される必要があります。</span><span class="sxs-lookup"><span data-stu-id="979e4-145">The body and every section in a Razor page must be either rendered or ignored.</span></span>

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a><span data-ttu-id="979e4-146">共有ディレクティブのインポート</span><span class="sxs-lookup"><span data-stu-id="979e4-146">Importing Shared Directives</span></span>

<span data-ttu-id="979e4-147">ビューは、Razor ディレクティブを使用して、名前空間のインポートや[依存性の注入](dependency-injection.md)など、多くのことを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="979e4-147">Views can use Razor directives to do many things, such as importing namespaces or performing [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="979e4-148">多くのビューで共有されるディレクティブは、共通の `_ViewImports.cshtml` ファイルで指定できます。</span><span class="sxs-lookup"><span data-stu-id="979e4-148">Directives shared by many views may be specified in a common `_ViewImports.cshtml` file.</span></span> <span data-ttu-id="979e4-149">`_ViewImports` ファイルは、次のディレクティブをサポートします。</span><span class="sxs-lookup"><span data-stu-id="979e4-149">The `_ViewImports` file supports the following directives:</span></span>

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

<span data-ttu-id="979e4-150">このファイルは、関数やセクションの定義などの Razor 機能をサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="979e4-150">The file doesn't support other Razor features, such as functions and section definitions.</span></span>

<span data-ttu-id="979e4-151">`_ViewImports.cshtml` ファイルのサンプル:</span><span class="sxs-lookup"><span data-stu-id="979e4-151">A sample `_ViewImports.cshtml` file:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

<span data-ttu-id="979e4-152">ASP.NET Core MVC アプリの `_ViewImports.cshtml` ファイルは、通常、`Views` フォルダーに配置されます。</span><span class="sxs-lookup"><span data-stu-id="979e4-152">The `_ViewImports.cshtml` file for an ASP.NET Core MVC app is typically placed in the `Views` folder.</span></span> <span data-ttu-id="979e4-153">`_ViewImports.cshtml` ファイルは、任意のフォルダー内に配置できますが、その場合は、そのフォルダーとそのサブフォルダー内にあるビューにのみ適用されます。</span><span class="sxs-lookup"><span data-stu-id="979e4-153">A `_ViewImports.cshtml` file can be placed within any folder, in which case it will only be applied to views within that folder and its subfolders.</span></span> <span data-ttu-id="979e4-154">`_ViewImports` ファイルは、最初にルート レベルで処理され、その後ビュー自体の場所までフォルダーごとに処理されるため、ルート レベルで指定された設定がフォルダー レベルでオーバーライドされる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="979e4-154">`_ViewImports` files are processed starting at the root level, and then for each folder leading up to the location of the view itself, so settings specified at the root level may be overridden at the folder level.</span></span>

<span data-ttu-id="979e4-155">たとえば、ルート レベルの `_ViewImports.cshtml` ファイルで `@model` と `@addTagHelper` を指定し、ビューのコントローラーに関連付けられているフォルダー内の別の `_ViewImports.cshtml` ファイルで異なる `@model` を指定し、別の `@addTagHelper` を追加すると、ビューは両方のタグ ヘルパーのアクセス権を持ち、後者の `@model` を使用するようになります。</span><span class="sxs-lookup"><span data-stu-id="979e4-155">For example, if a root level `_ViewImports.cshtml` file specifies `@model` and `@addTagHelper`, and another `_ViewImports.cshtml` file in the controller-associated folder of the view specifies a different `@model` and adds another `@addTagHelper`, the view will have access to both tag helpers and will use the latter `@model`.</span></span>

<span data-ttu-id="979e4-156">1 つのビューに対して複数の `_ViewImports.cshtml` ファイルを実行すると、`ViewImports.cshtml` ファイルに含まれるディレクティブの動作の組み合わせは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="979e4-156">If multiple `_ViewImports.cshtml` files are run for a view, combined behavior of the directives included in the `ViewImports.cshtml` files will be as follows:</span></span>

* <span data-ttu-id="979e4-157">`@addTagHelper`、`@removeTagHelper`: 順番どおりにすべて実行</span><span class="sxs-lookup"><span data-stu-id="979e4-157">`@addTagHelper`, `@removeTagHelper`: all run, in order</span></span>

* <span data-ttu-id="979e4-158">`@tagHelperPrefix`: ビューに最も近いものが、他のものをすべてをオーバーライドする</span><span class="sxs-lookup"><span data-stu-id="979e4-158">`@tagHelperPrefix`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="979e4-159">`@model`: ビューに最も近いものが、他のものをすべてをオーバーライドする</span><span class="sxs-lookup"><span data-stu-id="979e4-159">`@model`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="979e4-160">`@inherits`: ビューに最も近いものが、他のものをすべてをオーバーライドする</span><span class="sxs-lookup"><span data-stu-id="979e4-160">`@inherits`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="979e4-161">`@using`: すべてが含まれ、重複は無視される</span><span class="sxs-lookup"><span data-stu-id="979e4-161">`@using`: all are included; duplicates are ignored</span></span>

* <span data-ttu-id="979e4-162">`@inject`: プロパティごとに、ビューに最も近いものが、同じプロパティ名を持つ他のものすべてをオーバーライドする</span><span class="sxs-lookup"><span data-stu-id="979e4-162">`@inject`: for each property, the closest one to the view overrides any others with the same property name</span></span>

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a><span data-ttu-id="979e4-163">各ビューの前にコードを実行する</span><span class="sxs-lookup"><span data-stu-id="979e4-163">Running Code Before Each View</span></span>

<span data-ttu-id="979e4-164">すべてのビューの前に実行する必要があるコードがある場合は、このコードを `_ViewStart.cshtml` ファイル内に配置する必要があります。</span><span class="sxs-lookup"><span data-stu-id="979e4-164">If you have code you need to run before every view, this should be placed in the `_ViewStart.cshtml` file.</span></span> <span data-ttu-id="979e4-165">規則によって、`_ViewStart.cshtml` ファイルは `Views` フォルダー内に配置されます。</span><span class="sxs-lookup"><span data-stu-id="979e4-165">By convention, the `_ViewStart.cshtml` file is located in the `Views` folder.</span></span> <span data-ttu-id="979e4-166">`_ViewStart.cshtml` に一覧表示されているステートメントは、すべての (レイアウトでもなく、部分ビューでもない) 完全なビューより前に実行されます。</span><span class="sxs-lookup"><span data-stu-id="979e4-166">The statements listed in `_ViewStart.cshtml` are run before every full view (not layouts, and not partial views).</span></span> <span data-ttu-id="979e4-167">[ViewImports.cshtml](xref:mvc/views/layout#viewimports) と同様に、`_ViewStart.cshtml` は階層構造です。</span><span class="sxs-lookup"><span data-stu-id="979e4-167">Like [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` is hierarchical.</span></span> <span data-ttu-id="979e4-168">`_ViewStart.cshtml` ファイルがコントローラーに関連付けられているビュー フォルダーで定義されている場合、`Views` フォルダーのルートで定義されているファイル (ある場合) の後に実行されます。</span><span class="sxs-lookup"><span data-stu-id="979e4-168">If a `_ViewStart.cshtml` file is defined in the controller-associated view folder, it will be run after the one defined in the root of the `Views` folder (if any).</span></span>

<span data-ttu-id="979e4-169">`_ViewStart.cshtml` ファイルのサンプル:</span><span class="sxs-lookup"><span data-stu-id="979e4-169">A sample `_ViewStart.cshtml` file:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

<span data-ttu-id="979e4-170">上記のファイルは、すべてのビューで `_Layout.cshtml` レイアウトを使用することを指定します。</span><span class="sxs-lookup"><span data-stu-id="979e4-170">The file above specifies that all views will use the `_Layout.cshtml` layout.</span></span>

> [!NOTE]
> <span data-ttu-id="979e4-171">通常、`_ViewStart.cshtml` または `_ViewImports.cshtml` も `/Views/Shared` フォルダーには配置されません。</span><span class="sxs-lookup"><span data-stu-id="979e4-171">Neither `_ViewStart.cshtml` nor `_ViewImports.cshtml` are typically placed in the `/Views/Shared` folder.</span></span> <span data-ttu-id="979e4-172">これらのファイルのアプリ レベルのバージョンは、`/Views` フォルダーに直接配置する必要があります。</span><span class="sxs-lookup"><span data-stu-id="979e4-172">The app-level versions of these files should be placed directly in the `/Views` folder.</span></span>
