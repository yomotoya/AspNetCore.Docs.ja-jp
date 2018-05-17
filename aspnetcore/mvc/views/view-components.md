---
title: ASP.NET Core のビュー コンポーネント
author: rick-anderson
description: ASP.NET Core でのビュー コンポーネントの使用方法とそれらをアプリに追加する方法を説明します。
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-components
ms.openlocfilehash: a3614024c7f776e4502bc049180ae1c965e31db4
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/03/2018
---
# <a name="view-components-in-aspnet-core"></a><span data-ttu-id="768d2-103">ASP.NET Core のビュー コンポーネント</span><span class="sxs-lookup"><span data-stu-id="768d2-103">View components in ASP.NET Core</span></span>

<span data-ttu-id="768d2-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="768d2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="768d2-105">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="768d2-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="introducing-view-components"></a><span data-ttu-id="768d2-106">ビュー コンポーネントの概要</span><span class="sxs-lookup"><span data-stu-id="768d2-106">Introducing view components</span></span>

<span data-ttu-id="768d2-107">ASP.NET Core MVC を初めてお使いの場合、ビュー コンポーネントは部分ビューと似ていますが、はるかに強力なものです。</span><span class="sxs-lookup"><span data-stu-id="768d2-107">New to ASP.NET Core MVC, view components are similar to partial views, but they're much more powerful.</span></span> <span data-ttu-id="768d2-108">ビュー コンポーネントでは、モデル バインドを使用せず、呼び出すときに指定されたデータのみに依存します。</span><span class="sxs-lookup"><span data-stu-id="768d2-108">View components don't use model binding, and only depend on the data provided when calling into it.</span></span> <span data-ttu-id="768d2-109">ビュー コンポーネントの特徴は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="768d2-109">A view component:</span></span>

* <span data-ttu-id="768d2-110">応答全体ではなく、チャンクをレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="768d2-110">Renders a chunk rather than a whole response.</span></span>
* <span data-ttu-id="768d2-111">コントローラーとビューの間にあるのと同じ関心の分離とテストの容易性の利点があります。</span><span class="sxs-lookup"><span data-stu-id="768d2-111">Includes the same separation-of-concerns and testability benefits found between a controller and view.</span></span>
* <span data-ttu-id="768d2-112">パラメーターとビジネス ロジックを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="768d2-112">Can have parameters and business logic.</span></span>
* <span data-ttu-id="768d2-113">通常、レイアウト ページから呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="768d2-113">Is typically invoked from a layout page.</span></span>

<span data-ttu-id="768d2-114">ビュー コンポーネントは、次のような部分ビューには複雑すぎる、再利用可能なレンダリング ロジックをどこでも使用できるようにするためのものです。</span><span class="sxs-lookup"><span data-stu-id="768d2-114">View components are intended anywhere you have reusable rendering logic that's too complex for a partial view, such as:</span></span>

* <span data-ttu-id="768d2-115">動的なナビゲーション メニュー</span><span class="sxs-lookup"><span data-stu-id="768d2-115">Dynamic navigation menus</span></span>
* <span data-ttu-id="768d2-116">タグ クラウド (データベースをクエリする場所)</span><span class="sxs-lookup"><span data-stu-id="768d2-116">Tag cloud (where it queries the database)</span></span>
* <span data-ttu-id="768d2-117">ログイン パネル</span><span class="sxs-lookup"><span data-stu-id="768d2-117">Login panel</span></span>
* <span data-ttu-id="768d2-118">ショッピング カート</span><span class="sxs-lookup"><span data-stu-id="768d2-118">Shopping cart</span></span>
* <span data-ttu-id="768d2-119">新着情報の記事</span><span class="sxs-lookup"><span data-stu-id="768d2-119">Recently published articles</span></span>
* <span data-ttu-id="768d2-120">一般的なブログのサイドバーのコンテンツ</span><span class="sxs-lookup"><span data-stu-id="768d2-120">Sidebar content on a typical blog</span></span>
* <span data-ttu-id="768d2-121">すべてのページでレンダリングされ、ユーザーの状態のログに応じて、ログアウトまたはログインのいずれかのリンクを示すログイン パネル</span><span class="sxs-lookup"><span data-stu-id="768d2-121">A login panel that would be rendered on every page and show either the links to log out or log in, depending on the log in state of the user</span></span>

<span data-ttu-id="768d2-122">ビュー コンポーネントは、次の 2 つのパーツで構成されます。クラス (通常、[ViewComponent](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponent) から派生) と、クラスで返される結果 (通常はビュー) です。</span><span class="sxs-lookup"><span data-stu-id="768d2-122">A view component consists of two parts: the class (typically derived from [ViewComponent](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponent)) and the result it returns (typically a view).</span></span> <span data-ttu-id="768d2-123">コントローラーと同様に、ビュー コンポーネントは POCO の場合がありますが、ほとんどの開発者は `ViewComponent` から派生させて、利用できるメソッドとプロパティを活用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="768d2-123">Like controllers, a view component can be a POCO, but most developers will want to take advantage of the methods and properties available by deriving from `ViewComponent`.</span></span>

## <a name="creating-a-view-component"></a><span data-ttu-id="768d2-124">ビューのコンポーネントを作成する</span><span class="sxs-lookup"><span data-stu-id="768d2-124">Creating a view component</span></span>

<span data-ttu-id="768d2-125">このセクションには、ビュー コンポーネントを作成するための高レベルの要件が含まれています。</span><span class="sxs-lookup"><span data-stu-id="768d2-125">This section contains the high-level requirements to create a view component.</span></span> <span data-ttu-id="768d2-126">記事の後半で、各ステップの詳細を検証し、ビュー コンポーネントを作成します。</span><span class="sxs-lookup"><span data-stu-id="768d2-126">Later in the article, we'll examine each step in detail and create a view component.</span></span>

### <a name="the-view-component-class"></a><span data-ttu-id="768d2-127">ビュー コンポーネント クラス</span><span class="sxs-lookup"><span data-stu-id="768d2-127">The view component class</span></span>

<span data-ttu-id="768d2-128">ビュー コンポーネント クラスは、次のいずれかによって作成できます。</span><span class="sxs-lookup"><span data-stu-id="768d2-128">A view component class can be created by any of the following:</span></span>

* <span data-ttu-id="768d2-129">*ViewComponent* から派生させる</span><span class="sxs-lookup"><span data-stu-id="768d2-129">Deriving from *ViewComponent*</span></span>
* <span data-ttu-id="768d2-130">`[ViewComponent]` 属性でクラスを装飾するか、`[ViewComponent]` 属性でクラスから派生させる</span><span class="sxs-lookup"><span data-stu-id="768d2-130">Decorating a class with the `[ViewComponent]` attribute, or deriving from a class with the `[ViewComponent]` attribute</span></span>
* <span data-ttu-id="768d2-131">名前がサフィックス *ViewComponent* で終わるクラスを作成する</span><span class="sxs-lookup"><span data-stu-id="768d2-131">Creating a class where the name ends with the suffix *ViewComponent*</span></span>

<span data-ttu-id="768d2-132">コントローラーと同様に、ビュー コンポーネントは、パブリック クラス、入れ子にされていないクラス、および非抽象クラスである必要があります。</span><span class="sxs-lookup"><span data-stu-id="768d2-132">Like controllers, view components must be public, non-nested, and non-abstract classes.</span></span> <span data-ttu-id="768d2-133">ビュー コンポーネント名は、"ViewComponent" サフィックスを除いたクラス名です。</span><span class="sxs-lookup"><span data-stu-id="768d2-133">The view component name is the class name with the "ViewComponent" suffix removed.</span></span> <span data-ttu-id="768d2-134">また、これは `ViewComponentAttribute.Name` プロパティを使用して、明示的に指定することもできます。</span><span class="sxs-lookup"><span data-stu-id="768d2-134">It can also be explicitly specified using the `ViewComponentAttribute.Name` property.</span></span>

<span data-ttu-id="768d2-135">ビュー コンポーネント クラスの特徴は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="768d2-135">A view component class:</span></span>

* <span data-ttu-id="768d2-136">コンストラクターの[依存性の注入](../../fundamentals/dependency-injection.md)を完全にサポートします</span><span class="sxs-lookup"><span data-stu-id="768d2-136">Fully supports constructor [dependency injection](../../fundamentals/dependency-injection.md)</span></span>

* <span data-ttu-id="768d2-137">コントローラーのライフサイクルに関わりません。つまり、ビュー コンポーネントで[フィルター](../controllers/filters.md)を使用できないということです</span><span class="sxs-lookup"><span data-stu-id="768d2-137">Doesn't take part in the controller lifecycle, which means you can't use [filters](../controllers/filters.md) in a view component</span></span>

### <a name="view-component-methods"></a><span data-ttu-id="768d2-138">ビュー コンポーネント メソッド</span><span class="sxs-lookup"><span data-stu-id="768d2-138">View component methods</span></span>

<span data-ttu-id="768d2-139">ビュー コンポーネントでは、`IViewComponentResult` を返す `InvokeAsync` メソッドでそのロジックを定義します。</span><span class="sxs-lookup"><span data-stu-id="768d2-139">A view component defines its logic in an `InvokeAsync` method that returns an `IViewComponentResult`.</span></span> <span data-ttu-id="768d2-140">パラメーターは、モデル バインドではなく、ビュー コンポーネントから直接取得します。</span><span class="sxs-lookup"><span data-stu-id="768d2-140">Parameters come directly from invocation of the view component, not from model binding.</span></span> <span data-ttu-id="768d2-141">ビュー コンポーネントが要求を直接処理することはありません。</span><span class="sxs-lookup"><span data-stu-id="768d2-141">A view component never directly handles a request.</span></span> <span data-ttu-id="768d2-142">通常、ビュー コンポーネントは、モデルを初期化し、`View` メソッドを呼び出してビューに渡します。</span><span class="sxs-lookup"><span data-stu-id="768d2-142">Typically, a view component initializes a model and passes it to a view by calling the `View` method.</span></span> <span data-ttu-id="768d2-143">要約すると、ビュー コンポーネント メソッドの特徴は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="768d2-143">In summary, view component methods:</span></span>

* <span data-ttu-id="768d2-144">`IViewComponentResult` を返す `InvokeAsync` メソッドを定義します</span><span class="sxs-lookup"><span data-stu-id="768d2-144">Define an `InvokeAsync` method that returns an `IViewComponentResult`</span></span>
* <span data-ttu-id="768d2-145">通常、モデルを初期化し、`ViewComponent` `View` メソッドを呼び出してビューに渡します</span><span class="sxs-lookup"><span data-stu-id="768d2-145">Typically initializes a model and passes it to a view by calling the `ViewComponent` `View` method</span></span>
* <span data-ttu-id="768d2-146">パラメーターは HTTP ではなく、呼び出しメソッドから取得されます。モデル バインドはありません</span><span class="sxs-lookup"><span data-stu-id="768d2-146">Parameters come from the calling method, not HTTP, there's no model binding</span></span>
* <span data-ttu-id="768d2-147">HTTP エンドポイントとして直接到達することはできません。自分のコード (通常はビュー内) から呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="768d2-147">Are not reachable directly as an HTTP endpoint, they're invoked from your code (usually in a view).</span></span> <span data-ttu-id="768d2-148">ビュー コンポーネントは要求を処理しません</span><span class="sxs-lookup"><span data-stu-id="768d2-148">A view component never handles a request</span></span>
* <span data-ttu-id="768d2-149">現在の HTTP 要求からの詳細ではなく、シグネチャでオーバーロードされます</span><span class="sxs-lookup"><span data-stu-id="768d2-149">Are overloaded on the signature rather than any details from the current HTTP request</span></span>

### <a name="view-search-path"></a><span data-ttu-id="768d2-150">ビューの検索パス</span><span class="sxs-lookup"><span data-stu-id="768d2-150">View search path</span></span>

<span data-ttu-id="768d2-151">ランタイムでは、次のパスでビューを検索します。</span><span class="sxs-lookup"><span data-stu-id="768d2-151">The runtime searches for the view in the following paths:</span></span>

   * <span data-ttu-id="768d2-152">Views/\<コントローラー名>/Components/\<ビュー コンポーネント名>/\<ビュー名></span><span class="sxs-lookup"><span data-stu-id="768d2-152">Views/\<controller_name>/Components/\<view_component_name>/\<view_name></span></span>
   * <span data-ttu-id="768d2-153">Views/Shared/Components/\<ビュー コンポーネント名>/\<ビュー名></span><span class="sxs-lookup"><span data-stu-id="768d2-153">Views/Shared/Components/\<view_component_name>/\<view_name></span></span>

<span data-ttu-id="768d2-154">ビュー コンポーネントの既定のビュー名は、*Default* です。つまり、通常、ビュー ファイルは *Default.cshtml* という名前になるということです。</span><span class="sxs-lookup"><span data-stu-id="768d2-154">The default view name for a view component is *Default*, which means your view file will typically be named *Default.cshtml*.</span></span> <span data-ttu-id="768d2-155">ビュー コンポーネントの結果を作成したり、`View` メソッドを呼び出したりするときに、別のビュー名を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="768d2-155">You can specify a different view name when creating the view component result or when calling the `View` method.</span></span>

<span data-ttu-id="768d2-156">ビュー ファイルに *Default.cshtml* という名前を付けて、*Views/Shared/Components/\<ビュー コンポーネント名>/\<ビュー名>* パスを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="768d2-156">We recommend you name the view file *Default.cshtml* and use the *Views/Shared/Components/\<view_component_name>/\<view_name>* path.</span></span> <span data-ttu-id="768d2-157">このサンプルで使用される `PriorityList` ビュー コンポーネントは、ビュー コンポーネント ビューに *Views/Shared/Components/PriorityList/Default.cshtml* を使用します。</span><span class="sxs-lookup"><span data-stu-id="768d2-157">The `PriorityList` view component used in this sample uses *Views/Shared/Components/PriorityList/Default.cshtml* for the view component view.</span></span>

## <a name="invoking-a-view-component"></a><span data-ttu-id="768d2-158">ビュー コンポーネントを呼び出す</span><span class="sxs-lookup"><span data-stu-id="768d2-158">Invoking a view component</span></span>

<span data-ttu-id="768d2-159">ビュー コンポーネントを使用するには、ビュー内で以下を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="768d2-159">To use the view component, call the following inside a view:</span></span>

```cshtml
@Component.InvokeAsync("Name of view component", <anonymous type containing parameters>)
```

<span data-ttu-id="768d2-160">パラメーターは、`InvokeAsync` メソッドに渡されます。</span><span class="sxs-lookup"><span data-stu-id="768d2-160">The parameters will be passed to the `InvokeAsync` method.</span></span> <span data-ttu-id="768d2-161">この記事で開発された `PriorityList` ビュー コンポーネントは、*Views/Todo/Index.cshtml* ビュー ファイルから呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="768d2-161">The `PriorityList` view component developed in the article is invoked from the *Views/Todo/Index.cshtml* view file.</span></span> <span data-ttu-id="768d2-162">以下では、`InvokeAsync` メソッドは、2 つのパラメーターで呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="768d2-162">In the following, the `InvokeAsync` method is called with two parameters:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

## <a name="invoking-a-view-component-as-a-tag-helper"></a><span data-ttu-id="768d2-163">タグ ヘルパーとしてビュー コンポーネントを呼び出す</span><span class="sxs-lookup"><span data-stu-id="768d2-163">Invoking a view component as a Tag Helper</span></span>

<span data-ttu-id="768d2-164">ASP.NET Core 1.1 以降の場合は、[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)としてビュー コンポーネントを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="768d2-164">For ASP.NET Core 1.1 and higher, you can invoke a view component as a [Tag Helper](xref:mvc/views/tag-helpers/intro):</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="768d2-165">タグ ヘルパーのパスカル ケースのクラスとメソッドのパラメーターは、それぞれ[小文字のケバブ ケース](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101)に変換されます。</span><span class="sxs-lookup"><span data-stu-id="768d2-165">Pascal-cased class and method parameters for Tag Helpers are translated into their [lower kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="768d2-166">ビュー コンポーネントを呼び出すタグ ヘルパーでは、`<vc></vc>` 要素を使用します。</span><span class="sxs-lookup"><span data-stu-id="768d2-166">The Tag Helper to invoke a view component uses the `<vc></vc>` element.</span></span> <span data-ttu-id="768d2-167">ビュー コンポーネントは、次のように指定されます。</span><span class="sxs-lookup"><span data-stu-id="768d2-167">The view component is specified as follows:</span></span>

```cshtml
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

<span data-ttu-id="768d2-168">注: ビュー コンポーネントをタグ ヘルパーとして使用するには、`@addTagHelper` ディレクティブを使用して、ビュー コンポーネントを含むアセンブリを登録する必要があります。</span><span class="sxs-lookup"><span data-stu-id="768d2-168">Note: In order to use a View Component as a Tag Helper, you must register the assembly containing the View Component using the `@addTagHelper` directive.</span></span> <span data-ttu-id="768d2-169">たとえば、ビュー コンポーネントが "MyWebApp" と呼ばれるアセンブリにある場合は、次のディレクティブを `_ViewImports.cshtml` ファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="768d2-169">For example, if your View Component is in an assembly called "MyWebApp", add the following directive to the `_ViewImports.cshtml` file:</span></span>

```cshtml
@addTagHelper *, MyWebApp
```

<span data-ttu-id="768d2-170">ビュー コンポーネントを参照する任意のファイルへのタグ ヘルパーとして、ビュー コンポーネントを登録できます。</span><span class="sxs-lookup"><span data-stu-id="768d2-170">You can register a View Component as a Tag Helper to any file that references the View Component.</span></span> <span data-ttu-id="768d2-171">タグ ヘルパーを登録する方法の詳細については、「[Managing Tag Helper Scope](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope)」 (タグ ヘルパーのスコープの管理) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="768d2-171">See [Managing Tag Helper Scope](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) for more information on how to register Tag Helpers.</span></span>

<span data-ttu-id="768d2-172">このチュートリアルで使用される `InvokeAsync` メソッドは、次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="768d2-172">The `InvokeAsync` method used in this tutorial:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="768d2-173">タグ ヘルパーのマークアップでは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="768d2-173">In Tag Helper markup:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="768d2-174">上記のサンプルでは、`PriorityList` ビュー コンポーネントは `priority-list` になります。</span><span class="sxs-lookup"><span data-stu-id="768d2-174">In the sample above, the `PriorityList` view component becomes `priority-list`.</span></span> <span data-ttu-id="768d2-175">ビュー コンポーネントに対するパラメーターは、小文字のケバブ ケースの属性として渡されます。</span><span class="sxs-lookup"><span data-stu-id="768d2-175">The parameters to the view component are passed as attributes in lower kebab case.</span></span>

### <a name="invoking-a-view-component-directly-from-a-controller"></a><span data-ttu-id="768d2-176">ビュー コンポーネントをコントローラーから直接呼び出す</span><span class="sxs-lookup"><span data-stu-id="768d2-176">Invoking a view component directly from a controller</span></span>

<span data-ttu-id="768d2-177">通常、ビュー コンポーネントはビューから呼び出されますが、コントローラー メソッドから直接呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="768d2-177">View components are typically invoked from a view, but you can invoke them directly from a controller method.</span></span> <span data-ttu-id="768d2-178">ビュー コンポーネントでコントローラーなどのエンドポイントを定義しないときに、`ViewComponentResult` のコンテンツを返すコントローラー アクションを簡単に実装できます。</span><span class="sxs-lookup"><span data-stu-id="768d2-178">While view components don't define endpoints like controllers, you can easily implement a controller action that returns the content of a `ViewComponentResult`.</span></span>

<span data-ttu-id="768d2-179">この例では、ビュー コンポーネントは、コントローラーから直接呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="768d2-179">In this example, the view component is called directly from the controller:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a><span data-ttu-id="768d2-180">チュートリアル: 単純なビュー コンポーネントの作成</span><span class="sxs-lookup"><span data-stu-id="768d2-180">Walkthrough: Creating a simple view component</span></span>

<span data-ttu-id="768d2-181">スタート コードを[ダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample)、ビルド、およびテストします。</span><span class="sxs-lookup"><span data-stu-id="768d2-181">[Download](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), build and test the starter code.</span></span> <span data-ttu-id="768d2-182">これは、*[Todo]* 項目のリストを表示する `Todo` コントローラーを含む単純なプロジェクトです。</span><span class="sxs-lookup"><span data-stu-id="768d2-182">It's a simple project with a `Todo` controller that displays a list of *Todo* items.</span></span>

![[ToDo] のリスト](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a><span data-ttu-id="768d2-184">ViewComponent クラスの追加</span><span class="sxs-lookup"><span data-stu-id="768d2-184">Add a ViewComponent class</span></span>

<span data-ttu-id="768d2-185">*ViewComponents* フォルダーを作成して、次の `PriorityListViewComponent` クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="768d2-185">Create a *ViewComponents* folder and add the following `PriorityListViewComponent` class:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

<span data-ttu-id="768d2-186">コードに関する注意事項</span><span class="sxs-lookup"><span data-stu-id="768d2-186">Notes on the code:</span></span>

* <span data-ttu-id="768d2-187">ビュー コンポーネント クラスは、プロジェクト内の**任意**のフォルダーに含めることができます。</span><span class="sxs-lookup"><span data-stu-id="768d2-187">View component classes can be contained in **any** folder in the project.</span></span>
* <span data-ttu-id="768d2-188">クラス名 PriorityList**ViewComponent** は、サフィックス **ViewComponent** で終わるため、ビューからクラス コンポーネントを参照するときに、ランタイムでは文字列 "PriorityList" を使用します。</span><span class="sxs-lookup"><span data-stu-id="768d2-188">Because the class name PriorityList**ViewComponent** ends with the suffix **ViewComponent**, the runtime will use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="768d2-189">これについては、後で詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="768d2-189">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="768d2-190">`[ViewComponent]` 属性では、ビュー コンポーネントを参照するために使用する名前を変更できます。</span><span class="sxs-lookup"><span data-stu-id="768d2-190">The `[ViewComponent]` attribute can change the name used to reference a view component.</span></span> <span data-ttu-id="768d2-191">たとえば、クラスに `XYZ` という名前を付けて、`ViewComponent` 属性を適用することができます。</span><span class="sxs-lookup"><span data-stu-id="768d2-191">For example, we could've named the class `XYZ` and applied the `ViewComponent` attribute:</span></span>

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* <span data-ttu-id="768d2-192">上述の `[ViewComponent]` 属性は、ビュー コンポーネント セレクターに、コンポーネントに関連付けられたビューを探す場合は `PriorityList` という名前を使用し、ビューからクラス コンポーネントを参照する場合は文字列 "PriorityList" を使用するように指示します。</span><span class="sxs-lookup"><span data-stu-id="768d2-192">The `[ViewComponent]` attribute above tells the view component selector to use the name `PriorityList` when looking for the views associated with the component, and to use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="768d2-193">これについては、後で詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="768d2-193">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="768d2-194">コンポーネントでは、[依存性の注入](../../fundamentals/dependency-injection.md)を使用して、データ コンテキストを利用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="768d2-194">The component uses [dependency injection](../../fundamentals/dependency-injection.md) to make the data context available.</span></span>
* <span data-ttu-id="768d2-195">`InvokeAsync` ではビューから呼び出すことができるメソッドを表示し、任意の数の引数を取得できます。</span><span class="sxs-lookup"><span data-stu-id="768d2-195">`InvokeAsync` exposes a method which can be called from a view, and it can take an arbitrary number of arguments.</span></span>
* <span data-ttu-id="768d2-196">`InvokeAsync` メソッドでは、`isDone` と `maxPriority` パラメーターを満たす `ToDo` 項目のセットを返します。</span><span class="sxs-lookup"><span data-stu-id="768d2-196">The `InvokeAsync` method returns the set of `ToDo` items that satisfy the `isDone` and `maxPriority` parameters.</span></span>

### <a name="create-the-view-component-razor-view"></a><span data-ttu-id="768d2-197">ビュー コンポーネントの Razor ビューの作成</span><span class="sxs-lookup"><span data-stu-id="768d2-197">Create the view component Razor view</span></span>

* <span data-ttu-id="768d2-198">*Views/Shared/Components* フォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="768d2-198">Create the *Views/Shared/Components* folder.</span></span> <span data-ttu-id="768d2-199">このフォルダーは、*Components* という名前にする**必要があります**。</span><span class="sxs-lookup"><span data-stu-id="768d2-199">This folder **must** be named *Components*.</span></span>

* <span data-ttu-id="768d2-200">*Views/Shared/Components/PriorityList* フォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="768d2-200">Create the *Views/Shared/Components/PriorityList* folder.</span></span> <span data-ttu-id="768d2-201">このフォルダー名は、ビュー コンポーネント クラスの名前、または (規則に従い、クラス名に *ViewComponent* サフィックスを使用した場合は) サフィックスを差し引いたクラスの名前に一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="768d2-201">This folder name must match the name of the view component class, or the name of the class minus the suffix (if we followed convention and used the *ViewComponent* suffix in the class name).</span></span> <span data-ttu-id="768d2-202">`ViewComponent` 属性を使用した場合は、クラス名は属性の指定に一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="768d2-202">If you used the `ViewComponent` attribute, the class name would need to match the attribute designation.</span></span>

* <span data-ttu-id="768d2-203">*Views/Shared/Components/PriorityList/Default.cshtml* Razor ビューを作成します。[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="768d2-203">Create a *Views/Shared/Components/PriorityList/Default.cshtml* Razor view: [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span></span>
    
   <span data-ttu-id="768d2-204">Razor ビューでは、`TodoItem` のリストを取得して表示します。</span><span class="sxs-lookup"><span data-stu-id="768d2-204">The Razor view takes a list of `TodoItem` and displays them.</span></span> <span data-ttu-id="768d2-205">ビュー コンポーネントの `InvokeAsync` メソッドで (サンプルのように) ビューの名前を渡さない場合、*Default* が規則によってビュー名に使用されます。</span><span class="sxs-lookup"><span data-stu-id="768d2-205">If the view component `InvokeAsync` method doesn't pass the name of the view (as in our sample), *Default* is used for the view name by convention.</span></span> <span data-ttu-id="768d2-206">このチュートリアルの後半で、ビューの名前を渡す方法について示します。</span><span class="sxs-lookup"><span data-stu-id="768d2-206">Later in the tutorial, I'll show you how to pass the name of the view.</span></span> <span data-ttu-id="768d2-207">特定のコントローラーの既定のスタイルをオーバーライドするには、コントローラーに固有のビュー フォルダー (例: *Views/Todo/Components/PriorityList/Default.cshtml)* にビューを追加します。</span><span class="sxs-lookup"><span data-stu-id="768d2-207">To override the default styling for a specific controller, add a view to the controller-specific view folder (for example *Views/Todo/Components/PriorityList/Default.cshtml)*.</span></span>
    
    <span data-ttu-id="768d2-208">ビュー コンポーネントがコントローラーに固有の場合、コントローラーに固有のフォルダー (*Views/Todo/Components/PriorityList/Default.cshtml*) に追加できます。</span><span class="sxs-lookup"><span data-stu-id="768d2-208">If the view component is controller-specific, you can add it to the controller-specific folder (*Views/Todo/Components/PriorityList/Default.cshtml*).</span></span>

* <span data-ttu-id="768d2-209">優先順位リストのコンポーネントの呼び出しを含む `div` を *Views/Todo/index.cshtml* ファイルの下部に追加します。</span><span class="sxs-lookup"><span data-stu-id="768d2-209">Add a `div` containing a call to the priority list component to the bottom of the *Views/Todo/index.cshtml* file:</span></span>

    [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFirst.cshtml?range=34-38)]

<span data-ttu-id="768d2-210">`@await Component.InvokeAsync` マークアップは、ビュー コンポーネントを呼び出すための構文を示します。</span><span class="sxs-lookup"><span data-stu-id="768d2-210">The markup `@await Component.InvokeAsync` shows the syntax for calling view components.</span></span> <span data-ttu-id="768d2-211">最初の引数は、呼び出す必要があるコンポーネントの名前です。</span><span class="sxs-lookup"><span data-stu-id="768d2-211">The first argument is the name of the component we want to invoke or call.</span></span> <span data-ttu-id="768d2-212">後続のパラメーターは、そのコンポーネントに渡されます。</span><span class="sxs-lookup"><span data-stu-id="768d2-212">Subsequent parameters are passed to the component.</span></span> <span data-ttu-id="768d2-213">`InvokeAsync` では、任意の数の引数を取得できます。</span><span class="sxs-lookup"><span data-stu-id="768d2-213">`InvokeAsync` can take an arbitrary number of arguments.</span></span>

<span data-ttu-id="768d2-214">アプリをテストします。</span><span class="sxs-lookup"><span data-stu-id="768d2-214">Test the app.</span></span> <span data-ttu-id="768d2-215">次の画像は、[ToDo] リストと優先順位の項目を示します。</span><span class="sxs-lookup"><span data-stu-id="768d2-215">The following image shows the ToDo list and the priority items:</span></span>

![[ToDo] リストと優先順位の項目](view-components/_static/pi.png)

<span data-ttu-id="768d2-217">また、コントローラーから直接ビュー コンポーネントを呼び出すこともできます。</span><span class="sxs-lookup"><span data-stu-id="768d2-217">You can also call the view component directly from the controller:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![IndexVC アクションからの優先順位の項目](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a><span data-ttu-id="768d2-219">ビュー名を指定する</span><span class="sxs-lookup"><span data-stu-id="768d2-219">Specifying a view name</span></span>

<span data-ttu-id="768d2-220">複雑なビュー コンポーネントでは、いくつかの条件下で、既定以外のビューを指定する必要がある場合があります。</span><span class="sxs-lookup"><span data-stu-id="768d2-220">A complex view component might need to specify a non-default view under some conditions.</span></span> <span data-ttu-id="768d2-221">次のコードでは、`InvokeAsync` メソッドから "PVC" ビューを指定する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="768d2-221">The following code shows how to specify the "PVC" view  from the `InvokeAsync` method.</span></span> <span data-ttu-id="768d2-222">`PriorityListViewComponent` クラスで `InvokeAsync` メソッドを更新します。</span><span class="sxs-lookup"><span data-stu-id="768d2-222">Update the `InvokeAsync` method in the `PriorityListViewComponent` class.</span></span>

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

<span data-ttu-id="768d2-223">*Views/Shared/Components/PriorityList/Default.cshtml* ファイルを *Views/Shared/Components/PriorityList/PVC.cshtml* という名前のビューにコピーします。</span><span class="sxs-lookup"><span data-stu-id="768d2-223">Copy the *Views/Shared/Components/PriorityList/Default.cshtml* file to a view named *Views/Shared/Components/PriorityList/PVC.cshtml*.</span></span> <span data-ttu-id="768d2-224">PVC ビューが使用されていることを示すために、見出しを追加します。</span><span class="sxs-lookup"><span data-stu-id="768d2-224">Add a heading to indicate the PVC view is being used.</span></span>

[!code-cshtml[](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

<span data-ttu-id="768d2-225">*Views/TodoList/Index.cshtml* を更新します。</span><span class="sxs-lookup"><span data-stu-id="768d2-225">Update *Views/TodoList/Index.cshtml*:</span></span>

<!-- Views/TodoList/Index.cshtml is never imported, so change to test tutorial -->

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="768d2-226">アプリを実行して、PVC ビューを確認します。</span><span class="sxs-lookup"><span data-stu-id="768d2-226">Run the app and verify PVC view.</span></span>

![優先順位のビュー コンポーネント](view-components/_static/pvc.png)

<span data-ttu-id="768d2-228">PVC ビューがレンダリングされない場合は、4 以上の優先順位でビュー コンポーネントを呼び出していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="768d2-228">If the PVC view isn't rendered, verify you are calling the view component with a priority of 4 or higher.</span></span>

### <a name="examine-the-view-path"></a><span data-ttu-id="768d2-229">ビューのパスを調べる</span><span class="sxs-lookup"><span data-stu-id="768d2-229">Examine the view path</span></span>

* <span data-ttu-id="768d2-230">優先順位ビューが返されないように、優先順位パラメーターを 3 以下に変更します。</span><span class="sxs-lookup"><span data-stu-id="768d2-230">Change the priority parameter to three or less so the priority view isn't returned.</span></span>
* <span data-ttu-id="768d2-231">一時的に *Views/Todo/Components/PriorityList/Default.cshtml* の名前を *1Default.cshtml* に変更します。</span><span class="sxs-lookup"><span data-stu-id="768d2-231">Temporarily rename the *Views/Todo/Components/PriorityList/Default.cshtml* to *1Default.cshtml*.</span></span>
* <span data-ttu-id="768d2-232">アプリをテストすると、次のエラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="768d2-232">Test the app, you'll get the following error:</span></span>

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' wasn't found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* <span data-ttu-id="768d2-233">*Views/Todo/Components/PriorityList/1Default.cshtml* を *Views/Shared/Components/PriorityList/Default.cshtml* にコピーします。</span><span class="sxs-lookup"><span data-stu-id="768d2-233">Copy *Views/Todo/Components/PriorityList/1Default.cshtml* to *Views/Shared/Components/PriorityList/Default.cshtml*.</span></span>
* <span data-ttu-id="768d2-234">*[Shared]* の [ToDo] ビュー コンポーネントにいくつかのマークアップを追加して、*[Shared]* フォルダーからのビューを示します。</span><span class="sxs-lookup"><span data-stu-id="768d2-234">Add some markup to the *Shared* Todo view component view to indicate the view is from the *Shared* folder.</span></span>
* <span data-ttu-id="768d2-235">**[Shared]** コンポーネント ビューをテストします。</span><span class="sxs-lookup"><span data-stu-id="768d2-235">Test the **Shared** component view.</span></span>

![[Shared] コンポーネント ビューを含む [ToDo] 出力](view-components/_static/shared.png)

### <a name="avoiding-magic-strings"></a><span data-ttu-id="768d2-237">マジック文字列の回避</span><span class="sxs-lookup"><span data-stu-id="768d2-237">Avoiding magic strings</span></span>

<span data-ttu-id="768d2-238">コンパイル時間の安全性を確保する必要がある場合は、ハードコーディングされたビュー コンポーネント名をクラス名に置き換えることができます。</span><span class="sxs-lookup"><span data-stu-id="768d2-238">If you want compile time safety, you can replace the hard-coded view component name with the class name.</span></span> <span data-ttu-id="768d2-239">"ViewComponent" サフィックスのないビュー コンポーネントを作成します。</span><span class="sxs-lookup"><span data-stu-id="768d2-239">Create the view component without the "ViewComponent" suffix:</span></span>

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

<span data-ttu-id="768d2-240">`using` ステートメントを Razor ビュー ファイルに追加して、`nameof` 演算子を使用します。</span><span class="sxs-lookup"><span data-stu-id="768d2-240">Add a `using` statement to your Razor view file, and use the `nameof` operator:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexNameof.cshtml?range=1-6,35-)]

## <a name="additional-resources"></a><span data-ttu-id="768d2-241">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="768d2-241">Additional resources</span></span>

* [<span data-ttu-id="768d2-242">ビューへの依存性の注入</span><span class="sxs-lookup"><span data-stu-id="768d2-242">Dependency injection into views</span></span>](xref:mvc/views/dependency-injection)
