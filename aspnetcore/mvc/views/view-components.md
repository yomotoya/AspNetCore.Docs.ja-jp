---
title: ASP.NET Core のビュー コンポーネント
author: rick-anderson
description: ASP.NET Core でのビュー コンポーネントの使用方法とそれらをアプリに追加する方法を説明します。
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/view-components
ms.openlocfilehash: 2b196d8d46942604d1c85eb5f2f073661e5acb30
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278363"
---
# <a name="view-components-in-aspnet-core"></a><span data-ttu-id="ef46b-103">ASP.NET Core のビュー コンポーネント</span><span class="sxs-lookup"><span data-stu-id="ef46b-103">View components in ASP.NET Core</span></span>

<span data-ttu-id="ef46b-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ef46b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ef46b-105">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="ef46b-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="view-components"></a><span data-ttu-id="ef46b-106">ビュー コンポーネント</span><span class="sxs-lookup"><span data-stu-id="ef46b-106">View components</span></span>

<span data-ttu-id="ef46b-107">ビュー コンポーネントは部分ビューと似ていますが、はるかに強力なものです。</span><span class="sxs-lookup"><span data-stu-id="ef46b-107">View components are similar to partial views, but they're much more powerful.</span></span> <span data-ttu-id="ef46b-108">ビュー コンポーネントでは、モデル バインドを使用せず、呼び出すときに指定されたデータのみに依存します。</span><span class="sxs-lookup"><span data-stu-id="ef46b-108">View components don't use model binding, and only depend on the data provided when calling into it.</span></span> <span data-ttu-id="ef46b-109">この記事は ASP.NET Core MVC を使用して作成されましたが、ビュー コンポーネントは Razor ページでも利用できます。</span><span class="sxs-lookup"><span data-stu-id="ef46b-109">This article was written using ASP.NET Core MVC, but view components also work with Razor Pages.</span></span>

<span data-ttu-id="ef46b-110">ビュー コンポーネントの特徴は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="ef46b-110">A view component:</span></span>

* <span data-ttu-id="ef46b-111">応答全体ではなく、チャンクをレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="ef46b-111">Renders a chunk rather than a whole response.</span></span>
* <span data-ttu-id="ef46b-112">コントローラーとビューの間にあるのと同じ関心の分離とテストの容易性の利点があります。</span><span class="sxs-lookup"><span data-stu-id="ef46b-112">Includes the same separation-of-concerns and testability benefits found between a controller and view.</span></span>
* <span data-ttu-id="ef46b-113">パラメーターとビジネス ロジックを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="ef46b-113">Can have parameters and business logic.</span></span>
* <span data-ttu-id="ef46b-114">通常、レイアウト ページから呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="ef46b-114">Is typically invoked from a layout page.</span></span>

<span data-ttu-id="ef46b-115">ビュー コンポーネントは、次のような部分ビューには複雑すぎる、再利用可能なレンダリング ロジックをどこでも使用できるようにするためのものです。</span><span class="sxs-lookup"><span data-stu-id="ef46b-115">View components are intended anywhere you have reusable rendering logic that's too complex for a partial view, such as:</span></span>

* <span data-ttu-id="ef46b-116">動的なナビゲーション メニュー</span><span class="sxs-lookup"><span data-stu-id="ef46b-116">Dynamic navigation menus</span></span>
* <span data-ttu-id="ef46b-117">タグ クラウド (データベースをクエリする場所)</span><span class="sxs-lookup"><span data-stu-id="ef46b-117">Tag cloud (where it queries the database)</span></span>
* <span data-ttu-id="ef46b-118">ログイン パネル</span><span class="sxs-lookup"><span data-stu-id="ef46b-118">Login panel</span></span>
* <span data-ttu-id="ef46b-119">ショッピング カート</span><span class="sxs-lookup"><span data-stu-id="ef46b-119">Shopping cart</span></span>
* <span data-ttu-id="ef46b-120">新着情報の記事</span><span class="sxs-lookup"><span data-stu-id="ef46b-120">Recently published articles</span></span>
* <span data-ttu-id="ef46b-121">一般的なブログのサイドバーのコンテンツ</span><span class="sxs-lookup"><span data-stu-id="ef46b-121">Sidebar content on a typical blog</span></span>
* <span data-ttu-id="ef46b-122">すべてのページでレンダリングされ、ユーザーの状態のログに応じて、ログアウトまたはログインのいずれかのリンクを示すログイン パネル</span><span class="sxs-lookup"><span data-stu-id="ef46b-122">A login panel that would be rendered on every page and show either the links to log out or log in, depending on the log in state of the user</span></span>

<span data-ttu-id="ef46b-123">ビュー コンポーネントは、次の 2 つのパーツで構成されます。クラス (通常、[ViewComponent](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponent) から派生) と、クラスで返される結果 (通常はビュー) です。</span><span class="sxs-lookup"><span data-stu-id="ef46b-123">A view component consists of two parts: the class (typically derived from [ViewComponent](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponent)) and the result it returns (typically a view).</span></span> <span data-ttu-id="ef46b-124">コントローラーと同様に、ビュー コンポーネントは POCO の場合がありますが、ほとんどの開発者は `ViewComponent` から派生させて、利用できるメソッドとプロパティを活用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ef46b-124">Like controllers, a view component can be a POCO, but most developers will want to take advantage of the methods and properties available by deriving from `ViewComponent`.</span></span>

## <a name="creating-a-view-component"></a><span data-ttu-id="ef46b-125">ビューのコンポーネントを作成する</span><span class="sxs-lookup"><span data-stu-id="ef46b-125">Creating a view component</span></span>

<span data-ttu-id="ef46b-126">このセクションには、ビュー コンポーネントを作成するための高レベルの要件が含まれています。</span><span class="sxs-lookup"><span data-stu-id="ef46b-126">This section contains the high-level requirements to create a view component.</span></span> <span data-ttu-id="ef46b-127">記事の後半で、各ステップの詳細を検証し、ビュー コンポーネントを作成します。</span><span class="sxs-lookup"><span data-stu-id="ef46b-127">Later in the article, we'll examine each step in detail and create a view component.</span></span>

### <a name="the-view-component-class"></a><span data-ttu-id="ef46b-128">ビュー コンポーネント クラス</span><span class="sxs-lookup"><span data-stu-id="ef46b-128">The view component class</span></span>

<span data-ttu-id="ef46b-129">ビュー コンポーネント クラスは、次のいずれかによって作成できます。</span><span class="sxs-lookup"><span data-stu-id="ef46b-129">A view component class can be created by any of the following:</span></span>

* <span data-ttu-id="ef46b-130">*ViewComponent* から派生させる</span><span class="sxs-lookup"><span data-stu-id="ef46b-130">Deriving from *ViewComponent*</span></span>
* <span data-ttu-id="ef46b-131">`[ViewComponent]` 属性でクラスを装飾するか、`[ViewComponent]` 属性でクラスから派生させる</span><span class="sxs-lookup"><span data-stu-id="ef46b-131">Decorating a class with the `[ViewComponent]` attribute, or deriving from a class with the `[ViewComponent]` attribute</span></span>
* <span data-ttu-id="ef46b-132">名前がサフィックス *ViewComponent* で終わるクラスを作成する</span><span class="sxs-lookup"><span data-stu-id="ef46b-132">Creating a class where the name ends with the suffix *ViewComponent*</span></span>

<span data-ttu-id="ef46b-133">コントローラーと同様に、ビュー コンポーネントは、パブリック クラス、入れ子にされていないクラス、および非抽象クラスである必要があります。</span><span class="sxs-lookup"><span data-stu-id="ef46b-133">Like controllers, view components must be public, non-nested, and non-abstract classes.</span></span> <span data-ttu-id="ef46b-134">ビュー コンポーネント名は、"ViewComponent" サフィックスを除いたクラス名です。</span><span class="sxs-lookup"><span data-stu-id="ef46b-134">The view component name is the class name with the "ViewComponent" suffix removed.</span></span> <span data-ttu-id="ef46b-135">また、これは `ViewComponentAttribute.Name` プロパティを使用して、明示的に指定することもできます。</span><span class="sxs-lookup"><span data-stu-id="ef46b-135">It can also be explicitly specified using the `ViewComponentAttribute.Name` property.</span></span>

<span data-ttu-id="ef46b-136">ビュー コンポーネント クラスの特徴は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="ef46b-136">A view component class:</span></span>

* <span data-ttu-id="ef46b-137">コンストラクターの[依存性の注入](../../fundamentals/dependency-injection.md)を完全にサポートします</span><span class="sxs-lookup"><span data-stu-id="ef46b-137">Fully supports constructor [dependency injection](../../fundamentals/dependency-injection.md)</span></span>

* <span data-ttu-id="ef46b-138">コントローラーのライフサイクルに関わりません。つまり、ビュー コンポーネントで[フィルター](../controllers/filters.md)を使用できないということです</span><span class="sxs-lookup"><span data-stu-id="ef46b-138">Doesn't take part in the controller lifecycle, which means you can't use [filters](../controllers/filters.md) in a view component</span></span>

### <a name="view-component-methods"></a><span data-ttu-id="ef46b-139">ビュー コンポーネント メソッド</span><span class="sxs-lookup"><span data-stu-id="ef46b-139">View component methods</span></span>

<span data-ttu-id="ef46b-140">ビュー コンポーネントでは、`IViewComponentResult` を返す `InvokeAsync` メソッドでそのロジックを定義します。</span><span class="sxs-lookup"><span data-stu-id="ef46b-140">A view component defines its logic in an `InvokeAsync` method that returns an `IViewComponentResult`.</span></span> <span data-ttu-id="ef46b-141">パラメーターは、モデル バインドではなく、ビュー コンポーネントから直接取得します。</span><span class="sxs-lookup"><span data-stu-id="ef46b-141">Parameters come directly from invocation of the view component, not from model binding.</span></span> <span data-ttu-id="ef46b-142">ビュー コンポーネントが要求を直接処理することはありません。</span><span class="sxs-lookup"><span data-stu-id="ef46b-142">A view component never directly handles a request.</span></span> <span data-ttu-id="ef46b-143">通常、ビュー コンポーネントは、モデルを初期化し、`View` メソッドを呼び出してビューに渡します。</span><span class="sxs-lookup"><span data-stu-id="ef46b-143">Typically, a view component initializes a model and passes it to a view by calling the `View` method.</span></span> <span data-ttu-id="ef46b-144">要約すると、ビュー コンポーネント メソッドの特徴は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="ef46b-144">In summary, view component methods:</span></span>

* <span data-ttu-id="ef46b-145">`IViewComponentResult` を返す `InvokeAsync` メソッドを定義します</span><span class="sxs-lookup"><span data-stu-id="ef46b-145">Define an `InvokeAsync` method that returns an `IViewComponentResult`</span></span>
* <span data-ttu-id="ef46b-146">通常、モデルを初期化し、`ViewComponent` `View` メソッドを呼び出してビューに渡します</span><span class="sxs-lookup"><span data-stu-id="ef46b-146">Typically initializes a model and passes it to a view by calling the `ViewComponent` `View` method</span></span>
* <span data-ttu-id="ef46b-147">パラメーターは HTTP ではなく、呼び出しメソッドから取得されます。モデル バインドはありません</span><span class="sxs-lookup"><span data-stu-id="ef46b-147">Parameters come from the calling method, not HTTP, there's no model binding</span></span>
* <span data-ttu-id="ef46b-148">HTTP エンドポイントとして直接到達することはできません。自分のコード (通常はビュー内) から呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="ef46b-148">Are not reachable directly as an HTTP endpoint, they're invoked from your code (usually in a view).</span></span> <span data-ttu-id="ef46b-149">ビュー コンポーネントは要求を処理しません</span><span class="sxs-lookup"><span data-stu-id="ef46b-149">A view component never handles a request</span></span>
* <span data-ttu-id="ef46b-150">現在の HTTP 要求からの詳細ではなく、シグネチャでオーバーロードされます</span><span class="sxs-lookup"><span data-stu-id="ef46b-150">Are overloaded on the signature rather than any details from the current HTTP request</span></span>

### <a name="view-search-path"></a><span data-ttu-id="ef46b-151">ビューの検索パス</span><span class="sxs-lookup"><span data-stu-id="ef46b-151">View search path</span></span>

<span data-ttu-id="ef46b-152">ランタイムでは、次のパスでビューを検索します。</span><span class="sxs-lookup"><span data-stu-id="ef46b-152">The runtime searches for the view in the following paths:</span></span>

   * <span data-ttu-id="ef46b-153">Views/\<コントローラー名>/Components/\<ビュー コンポーネント名>/\<ビュー名></span><span class="sxs-lookup"><span data-stu-id="ef46b-153">Views/\<controller_name>/Components/\<view_component_name>/\<view_name></span></span>
   * <span data-ttu-id="ef46b-154">Views/Shared/Components/\<ビュー コンポーネント名>/\<ビュー名></span><span class="sxs-lookup"><span data-stu-id="ef46b-154">Views/Shared/Components/\<view_component_name>/\<view_name></span></span>

<span data-ttu-id="ef46b-155">ビュー コンポーネントの既定のビュー名は、*Default* です。つまり、通常、ビュー ファイルは *Default.cshtml* という名前になるということです。</span><span class="sxs-lookup"><span data-stu-id="ef46b-155">The default view name for a view component is *Default*, which means your view file will typically be named *Default.cshtml*.</span></span> <span data-ttu-id="ef46b-156">ビュー コンポーネントの結果を作成したり、`View` メソッドを呼び出したりするときに、別のビュー名を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="ef46b-156">You can specify a different view name when creating the view component result or when calling the `View` method.</span></span>

<span data-ttu-id="ef46b-157">ビュー ファイルに *Default.cshtml* という名前を付けて、*Views/Shared/Components/\<ビュー コンポーネント名>/\<ビュー名>* パスを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="ef46b-157">We recommend you name the view file *Default.cshtml* and use the *Views/Shared/Components/\<view_component_name>/\<view_name>* path.</span></span> <span data-ttu-id="ef46b-158">このサンプルで使用される `PriorityList` ビュー コンポーネントは、ビュー コンポーネント ビューに *Views/Shared/Components/PriorityList/Default.cshtml* を使用します。</span><span class="sxs-lookup"><span data-stu-id="ef46b-158">The `PriorityList` view component used in this sample uses *Views/Shared/Components/PriorityList/Default.cshtml* for the view component view.</span></span>

## <a name="invoking-a-view-component"></a><span data-ttu-id="ef46b-159">ビュー コンポーネントを呼び出す</span><span class="sxs-lookup"><span data-stu-id="ef46b-159">Invoking a view component</span></span>

<span data-ttu-id="ef46b-160">ビュー コンポーネントを使用するには、ビュー内で以下を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="ef46b-160">To use the view component, call the following inside a view:</span></span>

```cshtml
@Component.InvokeAsync("Name of view component", <anonymous type containing parameters>)
```

<span data-ttu-id="ef46b-161">パラメーターは、`InvokeAsync` メソッドに渡されます。</span><span class="sxs-lookup"><span data-stu-id="ef46b-161">The parameters will be passed to the `InvokeAsync` method.</span></span> <span data-ttu-id="ef46b-162">この記事で開発された `PriorityList` ビュー コンポーネントは、*Views/Todo/Index.cshtml* ビュー ファイルから呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="ef46b-162">The `PriorityList` view component developed in the article is invoked from the *Views/Todo/Index.cshtml* view file.</span></span> <span data-ttu-id="ef46b-163">以下では、`InvokeAsync` メソッドは、2 つのパラメーターで呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="ef46b-163">In the following, the `InvokeAsync` method is called with two parameters:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

## <a name="invoking-a-view-component-as-a-tag-helper"></a><span data-ttu-id="ef46b-164">タグ ヘルパーとしてビュー コンポーネントを呼び出す</span><span class="sxs-lookup"><span data-stu-id="ef46b-164">Invoking a view component as a Tag Helper</span></span>

<span data-ttu-id="ef46b-165">ASP.NET Core 1.1 以降の場合は、[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)としてビュー コンポーネントを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="ef46b-165">For ASP.NET Core 1.1 and higher, you can invoke a view component as a [Tag Helper](xref:mvc/views/tag-helpers/intro):</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="ef46b-166">タグ ヘルパーのパスカル ケースのクラスとメソッドのパラメーターは、それぞれ[小文字のケバブ ケース](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101)に変換されます。</span><span class="sxs-lookup"><span data-stu-id="ef46b-166">Pascal-cased class and method parameters for Tag Helpers are translated into their [lower kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="ef46b-167">ビュー コンポーネントを呼び出すタグ ヘルパーでは、`<vc></vc>` 要素を使用します。</span><span class="sxs-lookup"><span data-stu-id="ef46b-167">The Tag Helper to invoke a view component uses the `<vc></vc>` element.</span></span> <span data-ttu-id="ef46b-168">ビュー コンポーネントは、次のように指定されます。</span><span class="sxs-lookup"><span data-stu-id="ef46b-168">The view component is specified as follows:</span></span>

```cshtml
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

<span data-ttu-id="ef46b-169">注: ビュー コンポーネントをタグ ヘルパーとして使用するには、`@addTagHelper` ディレクティブを使用して、ビュー コンポーネントを含むアセンブリを登録する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ef46b-169">Note: In order to use a View Component as a Tag Helper, you must register the assembly containing the View Component using the `@addTagHelper` directive.</span></span> <span data-ttu-id="ef46b-170">たとえば、ビュー コンポーネントが "MyWebApp" と呼ばれるアセンブリにある場合は、次のディレクティブを `_ViewImports.cshtml` ファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="ef46b-170">For example, if your View Component is in an assembly called "MyWebApp", add the following directive to the `_ViewImports.cshtml` file:</span></span>

```cshtml
@addTagHelper *, MyWebApp
```

<span data-ttu-id="ef46b-171">ビュー コンポーネントを参照する任意のファイルへのタグ ヘルパーとして、ビュー コンポーネントを登録できます。</span><span class="sxs-lookup"><span data-stu-id="ef46b-171">You can register a View Component as a Tag Helper to any file that references the View Component.</span></span> <span data-ttu-id="ef46b-172">タグ ヘルパーを登録する方法の詳細については、「[Managing Tag Helper Scope](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope)」 (タグ ヘルパーのスコープの管理) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="ef46b-172">See [Managing Tag Helper Scope](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) for more information on how to register Tag Helpers.</span></span>

<span data-ttu-id="ef46b-173">このチュートリアルで使用される `InvokeAsync` メソッドは、次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="ef46b-173">The `InvokeAsync` method used in this tutorial:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="ef46b-174">タグ ヘルパーのマークアップでは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="ef46b-174">In Tag Helper markup:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="ef46b-175">上記のサンプルでは、`PriorityList` ビュー コンポーネントは `priority-list` になります。</span><span class="sxs-lookup"><span data-stu-id="ef46b-175">In the sample above, the `PriorityList` view component becomes `priority-list`.</span></span> <span data-ttu-id="ef46b-176">ビュー コンポーネントに対するパラメーターは、小文字のケバブ ケースの属性として渡されます。</span><span class="sxs-lookup"><span data-stu-id="ef46b-176">The parameters to the view component are passed as attributes in lower kebab case.</span></span>

### <a name="invoking-a-view-component-directly-from-a-controller"></a><span data-ttu-id="ef46b-177">ビュー コンポーネントをコントローラーから直接呼び出す</span><span class="sxs-lookup"><span data-stu-id="ef46b-177">Invoking a view component directly from a controller</span></span>

<span data-ttu-id="ef46b-178">通常、ビュー コンポーネントはビューから呼び出されますが、コントローラー メソッドから直接呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="ef46b-178">View components are typically invoked from a view, but you can invoke them directly from a controller method.</span></span> <span data-ttu-id="ef46b-179">ビュー コンポーネントでコントローラーなどのエンドポイントを定義しないときに、`ViewComponentResult` のコンテンツを返すコントローラー アクションを簡単に実装できます。</span><span class="sxs-lookup"><span data-stu-id="ef46b-179">While view components don't define endpoints like controllers, you can easily implement a controller action that returns the content of a `ViewComponentResult`.</span></span>

<span data-ttu-id="ef46b-180">この例では、ビュー コンポーネントは、コントローラーから直接呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="ef46b-180">In this example, the view component is called directly from the controller:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a><span data-ttu-id="ef46b-181">チュートリアル: 単純なビュー コンポーネントの作成</span><span class="sxs-lookup"><span data-stu-id="ef46b-181">Walkthrough: Creating a simple view component</span></span>

<span data-ttu-id="ef46b-182">スタート コードを[ダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample)、ビルド、およびテストします。</span><span class="sxs-lookup"><span data-stu-id="ef46b-182">[Download](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), build and test the starter code.</span></span> <span data-ttu-id="ef46b-183">これは、*[Todo]* 項目のリストを表示する `Todo` コントローラーを含む単純なプロジェクトです。</span><span class="sxs-lookup"><span data-stu-id="ef46b-183">It's a simple project with a `Todo` controller that displays a list of *Todo* items.</span></span>

![[ToDo] のリスト](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a><span data-ttu-id="ef46b-185">ViewComponent クラスの追加</span><span class="sxs-lookup"><span data-stu-id="ef46b-185">Add a ViewComponent class</span></span>

<span data-ttu-id="ef46b-186">*ViewComponents* フォルダーを作成して、次の `PriorityListViewComponent` クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="ef46b-186">Create a *ViewComponents* folder and add the following `PriorityListViewComponent` class:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

<span data-ttu-id="ef46b-187">コードに関する注意事項</span><span class="sxs-lookup"><span data-stu-id="ef46b-187">Notes on the code:</span></span>

* <span data-ttu-id="ef46b-188">ビュー コンポーネント クラスは、プロジェクト内の**任意**のフォルダーに含めることができます。</span><span class="sxs-lookup"><span data-stu-id="ef46b-188">View component classes can be contained in **any** folder in the project.</span></span>
* <span data-ttu-id="ef46b-189">クラス名 PriorityList**ViewComponent** は、サフィックス **ViewComponent** で終わるため、ビューからクラス コンポーネントを参照するときに、ランタイムでは文字列 "PriorityList" を使用します。</span><span class="sxs-lookup"><span data-stu-id="ef46b-189">Because the class name PriorityList**ViewComponent** ends with the suffix **ViewComponent**, the runtime will use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="ef46b-190">これについては、後で詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="ef46b-190">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="ef46b-191">`[ViewComponent]` 属性では、ビュー コンポーネントを参照するために使用する名前を変更できます。</span><span class="sxs-lookup"><span data-stu-id="ef46b-191">The `[ViewComponent]` attribute can change the name used to reference a view component.</span></span> <span data-ttu-id="ef46b-192">たとえば、クラスに `XYZ` という名前を付けて、`ViewComponent` 属性を適用することができます。</span><span class="sxs-lookup"><span data-stu-id="ef46b-192">For example, we could've named the class `XYZ` and applied the `ViewComponent` attribute:</span></span>

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* <span data-ttu-id="ef46b-193">上述の `[ViewComponent]` 属性は、ビュー コンポーネント セレクターに、コンポーネントに関連付けられたビューを探す場合は `PriorityList` という名前を使用し、ビューからクラス コンポーネントを参照する場合は文字列 "PriorityList" を使用するように指示します。</span><span class="sxs-lookup"><span data-stu-id="ef46b-193">The `[ViewComponent]` attribute above tells the view component selector to use the name `PriorityList` when looking for the views associated with the component, and to use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="ef46b-194">これについては、後で詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="ef46b-194">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="ef46b-195">コンポーネントでは、[依存性の注入](../../fundamentals/dependency-injection.md)を使用して、データ コンテキストを利用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="ef46b-195">The component uses [dependency injection](../../fundamentals/dependency-injection.md) to make the data context available.</span></span>
* <span data-ttu-id="ef46b-196">`InvokeAsync` ではビューから呼び出すことができるメソッドを表示し、任意の数の引数を取得できます。</span><span class="sxs-lookup"><span data-stu-id="ef46b-196">`InvokeAsync` exposes a method which can be called from a view, and it can take an arbitrary number of arguments.</span></span>
* <span data-ttu-id="ef46b-197">`InvokeAsync` メソッドでは、`isDone` と `maxPriority` パラメーターを満たす `ToDo` 項目のセットを返します。</span><span class="sxs-lookup"><span data-stu-id="ef46b-197">The `InvokeAsync` method returns the set of `ToDo` items that satisfy the `isDone` and `maxPriority` parameters.</span></span>

### <a name="create-the-view-component-razor-view"></a><span data-ttu-id="ef46b-198">ビュー コンポーネントの Razor ビューの作成</span><span class="sxs-lookup"><span data-stu-id="ef46b-198">Create the view component Razor view</span></span>

* <span data-ttu-id="ef46b-199">*Views/Shared/Components* フォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="ef46b-199">Create the *Views/Shared/Components* folder.</span></span> <span data-ttu-id="ef46b-200">このフォルダーは、*Components* という名前にする**必要があります**。</span><span class="sxs-lookup"><span data-stu-id="ef46b-200">This folder **must** be named *Components*.</span></span>

* <span data-ttu-id="ef46b-201">*Views/Shared/Components/PriorityList* フォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="ef46b-201">Create the *Views/Shared/Components/PriorityList* folder.</span></span> <span data-ttu-id="ef46b-202">このフォルダー名は、ビュー コンポーネント クラスの名前、または (規則に従い、クラス名に *ViewComponent* サフィックスを使用した場合は) サフィックスを差し引いたクラスの名前に一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ef46b-202">This folder name must match the name of the view component class, or the name of the class minus the suffix (if we followed convention and used the *ViewComponent* suffix in the class name).</span></span> <span data-ttu-id="ef46b-203">`ViewComponent` 属性を使用した場合は、クラス名は属性の指定に一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ef46b-203">If you used the `ViewComponent` attribute, the class name would need to match the attribute designation.</span></span>

* <span data-ttu-id="ef46b-204">*Views/Shared/Components/PriorityList/Default.cshtml* Razor ビューを作成します。[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="ef46b-204">Create a *Views/Shared/Components/PriorityList/Default.cshtml* Razor view: [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span></span>
    
   <span data-ttu-id="ef46b-205">Razor ビューでは、`TodoItem` のリストを取得して表示します。</span><span class="sxs-lookup"><span data-stu-id="ef46b-205">The Razor view takes a list of `TodoItem` and displays them.</span></span> <span data-ttu-id="ef46b-206">ビュー コンポーネントの `InvokeAsync` メソッドで (サンプルのように) ビューの名前を渡さない場合、*Default* が規則によってビュー名に使用されます。</span><span class="sxs-lookup"><span data-stu-id="ef46b-206">If the view component `InvokeAsync` method doesn't pass the name of the view (as in our sample), *Default* is used for the view name by convention.</span></span> <span data-ttu-id="ef46b-207">このチュートリアルの後半で、ビューの名前を渡す方法について示します。</span><span class="sxs-lookup"><span data-stu-id="ef46b-207">Later in the tutorial, I'll show you how to pass the name of the view.</span></span> <span data-ttu-id="ef46b-208">特定のコントローラーの既定のスタイルをオーバーライドするには、コントローラーに固有のビュー フォルダー (例: *Views/Todo/Components/PriorityList/Default.cshtml)* にビューを追加します。</span><span class="sxs-lookup"><span data-stu-id="ef46b-208">To override the default styling for a specific controller, add a view to the controller-specific view folder (for example *Views/Todo/Components/PriorityList/Default.cshtml)*.</span></span>
    
    <span data-ttu-id="ef46b-209">ビュー コンポーネントがコントローラーに固有の場合、コントローラーに固有のフォルダー (*Views/Todo/Components/PriorityList/Default.cshtml*) に追加できます。</span><span class="sxs-lookup"><span data-stu-id="ef46b-209">If the view component is controller-specific, you can add it to the controller-specific folder (*Views/Todo/Components/PriorityList/Default.cshtml*).</span></span>

* <span data-ttu-id="ef46b-210">優先順位リストのコンポーネントの呼び出しを含む `div` を *Views/Todo/index.cshtml* ファイルの下部に追加します。</span><span class="sxs-lookup"><span data-stu-id="ef46b-210">Add a `div` containing a call to the priority list component to the bottom of the *Views/Todo/index.cshtml* file:</span></span>

    [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFirst.cshtml?range=34-38)]

<span data-ttu-id="ef46b-211">`@await Component.InvokeAsync` マークアップは、ビュー コンポーネントを呼び出すための構文を示します。</span><span class="sxs-lookup"><span data-stu-id="ef46b-211">The markup `@await Component.InvokeAsync` shows the syntax for calling view components.</span></span> <span data-ttu-id="ef46b-212">最初の引数は、呼び出す必要があるコンポーネントの名前です。</span><span class="sxs-lookup"><span data-stu-id="ef46b-212">The first argument is the name of the component we want to invoke or call.</span></span> <span data-ttu-id="ef46b-213">後続のパラメーターは、そのコンポーネントに渡されます。</span><span class="sxs-lookup"><span data-stu-id="ef46b-213">Subsequent parameters are passed to the component.</span></span> <span data-ttu-id="ef46b-214">`InvokeAsync` では、任意の数の引数を取得できます。</span><span class="sxs-lookup"><span data-stu-id="ef46b-214">`InvokeAsync` can take an arbitrary number of arguments.</span></span>

<span data-ttu-id="ef46b-215">アプリをテストします。</span><span class="sxs-lookup"><span data-stu-id="ef46b-215">Test the app.</span></span> <span data-ttu-id="ef46b-216">次の画像は、[ToDo] リストと優先順位の項目を示します。</span><span class="sxs-lookup"><span data-stu-id="ef46b-216">The following image shows the ToDo list and the priority items:</span></span>

![[ToDo] リストと優先順位の項目](view-components/_static/pi.png)

<span data-ttu-id="ef46b-218">また、コントローラーから直接ビュー コンポーネントを呼び出すこともできます。</span><span class="sxs-lookup"><span data-stu-id="ef46b-218">You can also call the view component directly from the controller:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![IndexVC アクションからの優先順位の項目](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a><span data-ttu-id="ef46b-220">ビュー名を指定する</span><span class="sxs-lookup"><span data-stu-id="ef46b-220">Specifying a view name</span></span>

<span data-ttu-id="ef46b-221">複雑なビュー コンポーネントでは、いくつかの条件下で、既定以外のビューを指定する必要がある場合があります。</span><span class="sxs-lookup"><span data-stu-id="ef46b-221">A complex view component might need to specify a non-default view under some conditions.</span></span> <span data-ttu-id="ef46b-222">次のコードでは、`InvokeAsync` メソッドから "PVC" ビューを指定する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="ef46b-222">The following code shows how to specify the "PVC" view  from the `InvokeAsync` method.</span></span> <span data-ttu-id="ef46b-223">`PriorityListViewComponent` クラスで `InvokeAsync` メソッドを更新します。</span><span class="sxs-lookup"><span data-stu-id="ef46b-223">Update the `InvokeAsync` method in the `PriorityListViewComponent` class.</span></span>

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

<span data-ttu-id="ef46b-224">*Views/Shared/Components/PriorityList/Default.cshtml* ファイルを *Views/Shared/Components/PriorityList/PVC.cshtml* という名前のビューにコピーします。</span><span class="sxs-lookup"><span data-stu-id="ef46b-224">Copy the *Views/Shared/Components/PriorityList/Default.cshtml* file to a view named *Views/Shared/Components/PriorityList/PVC.cshtml*.</span></span> <span data-ttu-id="ef46b-225">PVC ビューが使用されていることを示すために、見出しを追加します。</span><span class="sxs-lookup"><span data-stu-id="ef46b-225">Add a heading to indicate the PVC view is being used.</span></span>

[!code-cshtml[](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

<span data-ttu-id="ef46b-226">*Views/TodoList/Index.cshtml* を更新します。</span><span class="sxs-lookup"><span data-stu-id="ef46b-226">Update *Views/TodoList/Index.cshtml*:</span></span>

<!-- Views/TodoList/Index.cshtml is never imported, so change to test tutorial -->

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="ef46b-227">アプリを実行して、PVC ビューを確認します。</span><span class="sxs-lookup"><span data-stu-id="ef46b-227">Run the app and verify PVC view.</span></span>

![優先順位のビュー コンポーネント](view-components/_static/pvc.png)

<span data-ttu-id="ef46b-229">PVC ビューがレンダリングされない場合は、4 以上の優先順位でビュー コンポーネントを呼び出していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="ef46b-229">If the PVC view isn't rendered, verify you are calling the view component with a priority of 4 or higher.</span></span>

### <a name="examine-the-view-path"></a><span data-ttu-id="ef46b-230">ビューのパスを調べる</span><span class="sxs-lookup"><span data-stu-id="ef46b-230">Examine the view path</span></span>

* <span data-ttu-id="ef46b-231">優先順位ビューが返されないように、優先順位パラメーターを 3 以下に変更します。</span><span class="sxs-lookup"><span data-stu-id="ef46b-231">Change the priority parameter to three or less so the priority view isn't returned.</span></span>
* <span data-ttu-id="ef46b-232">一時的に *Views/Todo/Components/PriorityList/Default.cshtml* の名前を *1Default.cshtml* に変更します。</span><span class="sxs-lookup"><span data-stu-id="ef46b-232">Temporarily rename the *Views/Todo/Components/PriorityList/Default.cshtml* to *1Default.cshtml*.</span></span>
* <span data-ttu-id="ef46b-233">アプリをテストすると、次のエラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ef46b-233">Test the app, you'll get the following error:</span></span>

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' wasn't found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* <span data-ttu-id="ef46b-234">*Views/Todo/Components/PriorityList/1Default.cshtml* を *Views/Shared/Components/PriorityList/Default.cshtml* にコピーします。</span><span class="sxs-lookup"><span data-stu-id="ef46b-234">Copy *Views/Todo/Components/PriorityList/1Default.cshtml* to *Views/Shared/Components/PriorityList/Default.cshtml*.</span></span>
* <span data-ttu-id="ef46b-235">*[Shared]* の [ToDo] ビュー コンポーネントにいくつかのマークアップを追加して、*[Shared]* フォルダーからのビューを示します。</span><span class="sxs-lookup"><span data-stu-id="ef46b-235">Add some markup to the *Shared* Todo view component view to indicate the view is from the *Shared* folder.</span></span>
* <span data-ttu-id="ef46b-236">**[Shared]** コンポーネント ビューをテストします。</span><span class="sxs-lookup"><span data-stu-id="ef46b-236">Test the **Shared** component view.</span></span>

![[Shared] コンポーネント ビューを含む [ToDo] 出力](view-components/_static/shared.png)

### <a name="avoiding-magic-strings"></a><span data-ttu-id="ef46b-238">マジック文字列の回避</span><span class="sxs-lookup"><span data-stu-id="ef46b-238">Avoiding magic strings</span></span>

<span data-ttu-id="ef46b-239">コンパイル時間の安全性を確保する必要がある場合は、ハードコーディングされたビュー コンポーネント名をクラス名に置き換えることができます。</span><span class="sxs-lookup"><span data-stu-id="ef46b-239">If you want compile time safety, you can replace the hard-coded view component name with the class name.</span></span> <span data-ttu-id="ef46b-240">"ViewComponent" サフィックスのないビュー コンポーネントを作成します。</span><span class="sxs-lookup"><span data-stu-id="ef46b-240">Create the view component without the "ViewComponent" suffix:</span></span>

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

<span data-ttu-id="ef46b-241">`using` ステートメントを Razor ビュー ファイルに追加して、`nameof` 演算子を使用します。</span><span class="sxs-lookup"><span data-stu-id="ef46b-241">Add a `using` statement to your Razor view file, and use the `nameof` operator:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexNameof.cshtml?range=1-6,35-)]

## <a name="additional-resources"></a><span data-ttu-id="ef46b-242">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="ef46b-242">Additional resources</span></span>

* [<span data-ttu-id="ef46b-243">ビューへの依存性の注入</span><span class="sxs-lookup"><span data-stu-id="ef46b-243">Dependency injection into views</span></span>](xref:mvc/views/dependency-injection)
