---
title: "コンポーネントの表示"
author: rick-anderson
description: "コンポーネントの表示は再利用可能なレンダリング ロジックがある任意の場所ものです。"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-components
ms.openlocfilehash: 65074ca02a1365db278d348d4e024121a6eb4634
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
# <a name="view-components"></a><span data-ttu-id="4e4d1-103">コンポーネントの表示</span><span class="sxs-lookup"><span data-stu-id="4e4d1-103">View components</span></span>

<span data-ttu-id="4e4d1-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4e4d1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4e4d1-105">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="introducing-view-components"></a><span data-ttu-id="4e4d1-106">コンポーネントの表示の概要</span><span class="sxs-lookup"><span data-stu-id="4e4d1-106">Introducing view components</span></span>

<span data-ttu-id="4e4d1-107">新しい ASP.NET Core mvc コンポーネントの表示に似ています部分ビューのこれらははるかに強力なものです。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-107">New to ASP.NET Core MVC, view components are similar to partial views, but they're much more powerful.</span></span> <span data-ttu-id="4e4d1-108">コンポーネントの表示はしない、モデル バインディングを使用し、呼び出し時に指定したデータにのみ依存します。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-108">View components don’t use model binding, and only depend on the data you provide when calling into it.</span></span> <span data-ttu-id="4e4d1-109">ビュー コンポーネント:</span><span class="sxs-lookup"><span data-stu-id="4e4d1-109">A view component:</span></span>

* <span data-ttu-id="4e4d1-110">応答の全体ではなく、チャンクを表示します。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-110">Renders a chunk rather than a whole response</span></span>
* <span data-ttu-id="4e4d1-111">同じ問題の分離やテストの容易性の利点がありますが、コント ローラーとビューの間にあります。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-111">Includes the same separation-of-concerns and testability benefits found between a controller and view</span></span>
* <span data-ttu-id="4e4d1-112">パラメーターとビジネス ロジックを持つことができます。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-112">Can have parameters and business logic</span></span>
* <span data-ttu-id="4e4d1-113">通常、レイアウト ページから呼び出されます</span><span class="sxs-lookup"><span data-stu-id="4e4d1-113">Is typically invoked from a layout page</span></span>

<span data-ttu-id="4e4d1-114">コンポーネントの表示は、任意の場所などの複雑すぎるため、部分ビューが再利用可能なレンダリング ロジックがある場合。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-114">View components are intended anywhere you have reusable rendering logic that's too complex for a partial view, such as:</span></span>

* <span data-ttu-id="4e4d1-115">動的なナビゲーション メニュー</span><span class="sxs-lookup"><span data-stu-id="4e4d1-115">Dynamic navigation menus</span></span>
* <span data-ttu-id="4e4d1-116">タグのクラウド (データベースのクエリを)</span><span class="sxs-lookup"><span data-stu-id="4e4d1-116">Tag cloud (where it queries the database)</span></span>
* <span data-ttu-id="4e4d1-117">ログイン パネル</span><span class="sxs-lookup"><span data-stu-id="4e4d1-117">Login panel</span></span>
* <span data-ttu-id="4e4d1-118">ショッピング カート</span><span class="sxs-lookup"><span data-stu-id="4e4d1-118">Shopping cart</span></span>
* <span data-ttu-id="4e4d1-119">パブリッシュされたアーティクル最近</span><span class="sxs-lookup"><span data-stu-id="4e4d1-119">Recently published articles</span></span>
* <span data-ttu-id="4e4d1-120">一般的なブログでサイドバーの内容</span><span class="sxs-lookup"><span data-stu-id="4e4d1-120">Sidebar content on a typical blog</span></span>
* <span data-ttu-id="4e4d1-121">すべてのページにレンダリングされますをログアウトするかによっては、ユーザーの状態にあるログ、ログインに、いずれかのリンクを表示するログイン パネル</span><span class="sxs-lookup"><span data-stu-id="4e4d1-121">A login panel that would be rendered on every page and show either the links to log out or log in, depending on the log in state of the user</span></span>

<span data-ttu-id="4e4d1-122">ビュー コンポーネントは、2 つの部分で構成されています: クラス (から派生した通常[ViewComponent](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponent)) であり、結果 (通常はビュー) を返します。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-122">A view component consists of two parts: the class (typically derived from [ViewComponent](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponent)) and the result it returns (typically a view).</span></span> <span data-ttu-id="4e4d1-123">コント ローラーと同様に、ビュー コンポーネントが、POCO をすることができますもほとんどの開発者は活用するために使用可能なプロパティとメソッドから派生することによって`ViewComponent`です。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-123">Like controllers, a view component can be a POCO, but most developers will want to take advantage of the methods and properties available by deriving from `ViewComponent`.</span></span>

## <a name="creating-a-view-component"></a><span data-ttu-id="4e4d1-124">ビュー コンポーネントを作成します。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-124">Creating a view component</span></span>

<span data-ttu-id="4e4d1-125">このセクションには、ビュー コンポーネントを作成する高レベルの要件が含まれています。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-125">This section contains the high-level requirements to create a view component.</span></span> <span data-ttu-id="4e4d1-126">記事の後半でおうまく各手順の詳細を確認し、ビュー コンポーネントを作成します。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-126">Later in the article, we'll examine each step in detail and create a view component.</span></span>

### <a name="the-view-component-class"></a><span data-ttu-id="4e4d1-127">ビューのコンポーネント クラス</span><span class="sxs-lookup"><span data-stu-id="4e4d1-127">The view component class</span></span>

<span data-ttu-id="4e4d1-128">次のいずれかのビュー コンポーネント クラスを作成できます。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-128">A view component class can be created by any of the following:</span></span>

* <span data-ttu-id="4e4d1-129">派生する*ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="4e4d1-129">Deriving from *ViewComponent*</span></span>
* <span data-ttu-id="4e4d1-130">持つクラスを装飾、`[ViewComponent]`属性、またはクラスから派生する、`[ViewComponent]`属性</span><span class="sxs-lookup"><span data-stu-id="4e4d1-130">Decorating a class with the `[ViewComponent]` attribute, or deriving from a class with the `[ViewComponent]` attribute</span></span>
* <span data-ttu-id="4e4d1-131">名前が、サフィックスで終わる、クラスを作成する*ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="4e4d1-131">Creating a class where the name ends with the suffix *ViewComponent*</span></span>

<span data-ttu-id="4e4d1-132">コント ローラーのようにビュー コンポーネントは、パブリックの入れ子にされないインデックスおよび非抽象クラスをする必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-132">Like controllers, view components must be public, non-nested, and non-abstract classes.</span></span> <span data-ttu-id="4e4d1-133">ビューのコンポーネント名は、削除"ViewComponent"サフィックスを持つクラスの名前です。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-133">The view component name is the class name with the "ViewComponent" suffix removed.</span></span> <span data-ttu-id="4e4d1-134">明示的に指定できますを使用して、`ViewComponentAttribute.Name`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-134">It can also be explicitly specified using the `ViewComponentAttribute.Name` property.</span></span>

<span data-ttu-id="4e4d1-135">ビューのコンポーネント クラス:</span><span class="sxs-lookup"><span data-stu-id="4e4d1-135">A view component class:</span></span>

* <span data-ttu-id="4e4d1-136">コンス トラクターを完全にサポート[依存関係の挿入](../../fundamentals/dependency-injection.md)</span><span class="sxs-lookup"><span data-stu-id="4e4d1-136">Fully supports constructor [dependency injection](../../fundamentals/dependency-injection.md)</span></span>

* <span data-ttu-id="4e4d1-137">コント ローラーのライフ サイクルは、使用できないことを意味の一部を受け取りません[フィルター](../controllers/filters.md)ビュー コンポーネント</span><span class="sxs-lookup"><span data-stu-id="4e4d1-137">Doesn't take part in the controller lifecycle, which means you can't use [filters](../controllers/filters.md) in a view component</span></span>

### <a name="view-component-methods"></a><span data-ttu-id="4e4d1-138">ビュー コンポーネント メソッド</span><span class="sxs-lookup"><span data-stu-id="4e4d1-138">View component methods</span></span>

<span data-ttu-id="4e4d1-139">表示コンポーネントにそのロジックを定義する、`InvokeAsync`を返すメソッド、`IViewComponentResult`です。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-139">A view component defines its logic in an `InvokeAsync` method that returns an `IViewComponentResult`.</span></span> <span data-ttu-id="4e4d1-140">パラメーターは、モデル バインドからではなく、ビューのコンポーネントの呼び出しから直接取得します。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-140">Parameters come directly from invocation of the view component, not from model binding.</span></span> <span data-ttu-id="4e4d1-141">ビュー コンポーネントしない直接要求が処理されます。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-141">A view component never directly handles a request.</span></span> <span data-ttu-id="4e4d1-142">通常、ビュー コンポーネントは、モデルを初期化し、呼び出すことによって、ビューに渡されます、`View`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-142">Typically, a view component initializes a model and passes it to a view by calling the `View` method.</span></span> <span data-ttu-id="4e4d1-143">要約すると、コンポーネントのメソッドを参照してください。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-143">In summary, view component methods:</span></span>

* <span data-ttu-id="4e4d1-144">定義、`InvokeAsync`を返すメソッドを`IViewComponentResult`</span><span class="sxs-lookup"><span data-stu-id="4e4d1-144">Define an `InvokeAsync` method that returns an `IViewComponentResult`</span></span>
* <span data-ttu-id="4e4d1-145">モデルの初期化を呼び出すことによって、ビューに渡します通常、 `ViewComponent` `View`メソッド。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-145">Typically initializes a model and passes it to a view by calling the `ViewComponent` `View` method</span></span>
* <span data-ttu-id="4e4d1-146">パラメーターを取得する呼び出し元のメソッドでは、HTTP ではないから、モデルのバインドがありません。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-146">Parameters come from the calling method, not HTTP, there's no model binding</span></span>
* <span data-ttu-id="4e4d1-147">HTTP エンドポイントとして直接到達可能ではない、して呼び出すことが (通常は、ビュー) で、コードからです。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-147">Are not reachable directly as an HTTP endpoint, they're invoked from your code (usually in a view).</span></span> <span data-ttu-id="4e4d1-148">ビューのコンポーネントが、要求を処理しません。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-148">A view component never handles a request</span></span>
* <span data-ttu-id="4e4d1-149">詳細を現在の HTTP 要求ではなく、シグネチャではオーバー ロードします。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-149">Are overloaded on the signature rather than any details from the current HTTP request</span></span>

### <a name="view-search-path"></a><span data-ttu-id="4e4d1-150">ビューの検索パス</span><span class="sxs-lookup"><span data-stu-id="4e4d1-150">View search path</span></span>

<span data-ttu-id="4e4d1-151">ランタイムは、次のパスにビューを検索します。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-151">The runtime searches for the view in the following paths:</span></span>

   * <span data-ttu-id="4e4d1-152">ビュー/\<controller_name >/Components/\<view_component_name >/\<view_name ></span><span class="sxs-lookup"><span data-stu-id="4e4d1-152">Views/\<controller_name>/Components/\<view_component_name>/\<view_name></span></span>
   * <span data-ttu-id="4e4d1-153">Views/Shared/Components/\<view_component_name>/\<view_name></span><span class="sxs-lookup"><span data-stu-id="4e4d1-153">Views/Shared/Components/\<view_component_name>/\<view_name></span></span>

<span data-ttu-id="4e4d1-154">ビューのコンポーネントの既定のビュー名は*既定*、つまり、ビューは、ファイルは通常の名前*Default.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-154">The default view name for a view component is *Default*, which means your view file will typically be named *Default.cshtml*.</span></span> <span data-ttu-id="4e4d1-155">別のビュー名を指定するには、ビューのコンポーネントの結果を作成するときに、または呼び出すときに、`View`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-155">You can specify a different view name when creating the view component result or when calling the `View` method.</span></span>

<span data-ttu-id="4e4d1-156">ファイルの表示の名前を付けることをお勧め*Default.cshtml*を使用して、*コンポーネント/ビュー/共有/\<view_component_name >/\<view_name >*パス。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-156">We recommend you name the view file *Default.cshtml* and use the *Views/Shared/Components/\<view_component_name>/\<view_name>* path.</span></span> <span data-ttu-id="4e4d1-157">`PriorityList`このサンプルで使用されるビュー コンポーネントを使用して*Views/Shared/Components/PriorityList/Default.cshtml*ビュー コンポーネントをします。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-157">The `PriorityList` view component used in this sample uses *Views/Shared/Components/PriorityList/Default.cshtml* for the view component view.</span></span>

## <a name="invoking-a-view-component"></a><span data-ttu-id="4e4d1-158">ビュー コンポーネントを呼び出す</span><span class="sxs-lookup"><span data-stu-id="4e4d1-158">Invoking a view component</span></span>

<span data-ttu-id="4e4d1-159">ビュー コンポーネントを使用して、次を呼び出して、ビュー内。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-159">To use the view component, call the following inside a view:</span></span>

```cshtml
@Component.InvokeAsync("Name of view component", <anonymous type containing parameters>)
```

<span data-ttu-id="4e4d1-160">パラメーターに渡される、`InvokeAsync`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-160">The parameters will be passed to the `InvokeAsync` method.</span></span> <span data-ttu-id="4e4d1-161">`PriorityList`アーティクルで開発されたビュー コンポーネントが呼び出されて、 *Views/Todo/Index.cshtml*ファイルを表示します。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-161">The `PriorityList` view component developed in the article is invoked from the *Views/Todo/Index.cshtml* view file.</span></span> <span data-ttu-id="4e4d1-162">次の場合、`InvokeAsync`メソッドが呼び出された 2 つのパラメーター。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-162">In the following, the `InvokeAsync` method is called with two parameters:</span></span>

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

## <a name="invoking-a-view-component-as-a-tag-helper"></a><span data-ttu-id="4e4d1-163">としてタグ ヘルパーのビュー コンポーネントを呼び出す</span><span class="sxs-lookup"><span data-stu-id="4e4d1-163">Invoking a view component as a Tag Helper</span></span>

<span data-ttu-id="4e4d1-164">ASP.NET Core 1.1 以降、としてビュー コンポーネントを呼び出すことができます、[タグ ヘルパー](xref:mvc/views/tag-helpers/intro):</span><span class="sxs-lookup"><span data-stu-id="4e4d1-164">For ASP.NET Core 1.1 and higher, you can invoke a view component as a [Tag Helper](xref:mvc/views/tag-helpers/intro):</span></span>

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="4e4d1-165">タグ ヘルパーのクラスとメソッドを pascal でパラメーターに変換された、 [kebab ケースを下げる](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101)です。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-165">Pascal-cased class and method parameters for Tag Helpers are translated into their [lower kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="4e4d1-166">ビュー コンポーネントを呼び出すタグ ヘルパーを使用して、`<vc></vc>`要素。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-166">The Tag Helper to invoke a view component uses the `<vc></vc>` element.</span></span> <span data-ttu-id="4e4d1-167">ビューのコンポーネントの指定は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-167">The view component is specified as follows:</span></span>

```cshtml
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

<span data-ttu-id="4e4d1-168">注: タグ ヘルパーとしてビュー コンポーネントを使用するために必要がありますアセンブリを登録するを使用して、ビュー コンポーネントを含む、`@addTagHelper`ディレクティブです。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-168">Note: In order to use a View Component as a Tag Helper, you must register the assembly containing the View Component using the `@addTagHelper` directive.</span></span> <span data-ttu-id="4e4d1-169">たとえば、ビュー コンポーネントが"MyWebApp"と呼ばれるアセンブリ内にある場合は、ディレクティブを追加、次に、`_ViewImports.cshtml`ファイル。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-169">For example, if your View Component is in an assembly called "MyWebApp", add the following directive to the `_ViewImports.cshtml` file:</span></span>

```cshtml
@addTagHelper *, MyWebApp
```

<span data-ttu-id="4e4d1-170">ビュー コンポーネントを参照するすべてのファイルにタグ ヘルパーとしてビュー コンポーネントを登録することができます。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-170">You can register a View Component as a Tag Helper to any file that references the View Component.</span></span> <span data-ttu-id="4e4d1-171">参照してください[タグ ヘルパーのスコープを管理する](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope)タグ ヘルパーを登録する方法の詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-171">See [Managing Tag Helper Scope](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) for more information on how to register Tag Helpers.</span></span>

<span data-ttu-id="4e4d1-172">`InvokeAsync`このチュートリアルで使用されるメソッド。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-172">The `InvokeAsync` method used in this tutorial:</span></span>

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="4e4d1-173">タグ ヘルパーのマークアップ。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-173">In Tag Helper markup:</span></span>

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="4e4d1-174">上記のサンプルでは、`PriorityList`表示コンポーネントになります`priority-list`です。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-174">In the sample above, the `PriorityList` view component becomes `priority-list`.</span></span> <span data-ttu-id="4e4d1-175">表示コンポーネントにパラメーターは、小文字 kebab 属性として渡されます。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-175">The parameters to the view component are passed as attributes in lower kebab case.</span></span>

### <a name="invoking-a-view-component-directly-from-a-controller"></a><span data-ttu-id="4e4d1-176">コント ローラーから直接ビュー コンポーネントを呼び出す</span><span class="sxs-lookup"><span data-stu-id="4e4d1-176">Invoking a view component directly from a controller</span></span>

<span data-ttu-id="4e4d1-177">コンポーネントの表示は通常、ビューから呼び出されますが、それらをコント ローラー メソッドから直接呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-177">View components are typically invoked from a view, but you can invoke them directly from a controller method.</span></span> <span data-ttu-id="4e4d1-178">コンポーネントの表示がコント ローラーなどのエンドポイントを定義していないときに、コント ローラーのアクションの内容を表すオブジェクトを簡単に実装できます、`ViewComponentResult`です。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-178">While view components don't define endpoints like controllers, you can easily implement a controller action that returns the content of a `ViewComponentResult`.</span></span>

<span data-ttu-id="4e4d1-179">この例では、表示コンポーネントは、コント ローラーから直接呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-179">In this example, the view component is called directly from the controller:</span></span>

[!code-csharp[Main](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a><span data-ttu-id="4e4d1-180">チュートリアル: 単純なビュー コンポーネントの作成</span><span class="sxs-lookup"><span data-stu-id="4e4d1-180">Walkthrough: Creating a simple view component</span></span>

<span data-ttu-id="4e4d1-181">[ダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample)、ビルド、およびスタート コードをテストします。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-181">[Download](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), build and test the starter code.</span></span> <span data-ttu-id="4e4d1-182">単純なプロジェクトでは、`Todo`の一覧を表示するコント ローラー *Todo*項目。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-182">It's a simple project with a `Todo` controller that displays a list of *Todo* items.</span></span>

![Todo などの一覧](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a><span data-ttu-id="4e4d1-184">ViewComponent クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-184">Add a ViewComponent class</span></span>

<span data-ttu-id="4e4d1-185">作成、 *ViewComponents*フォルダーに次の追加と`PriorityListViewComponent`クラス。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-185">Create a *ViewComponents* folder and add the following `PriorityListViewComponent` class:</span></span>

[!code-csharp[Main](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

<span data-ttu-id="4e4d1-186">コードに関する注意事項</span><span class="sxs-lookup"><span data-stu-id="4e4d1-186">Notes on the code:</span></span>

* <span data-ttu-id="4e4d1-187">ビュー クラスをコンポーネントに含まれていることができます**任意**プロジェクト内のフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-187">View component classes can be contained in **any** folder in the project.</span></span>
* <span data-ttu-id="4e4d1-188">クラスの名前を PriorityList のため**ViewComponent**サフィックスで終わる**ViewComponent**、ランタイムから見ると、クラスのコンポーネントを参照するときに文字列"PriorityList"が使用されます。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-188">Because the class name PriorityList**ViewComponent** ends with the suffix **ViewComponent**, the runtime will use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="4e4d1-189">について説明をさらに詳しく後述します。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-189">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="4e4d1-190">`[ViewComponent]`属性は、ビュー コンポーネントを参照するための名前を変更できます。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-190">The `[ViewComponent]` attribute can change the name used to reference a view component.</span></span> <span data-ttu-id="4e4d1-191">たとえば、でしたしたという名前を付けてクラス`XYZ`と適用、`ViewComponent`属性。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-191">For example, we could've named the class `XYZ` and applied the `ViewComponent` attribute:</span></span>

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* <span data-ttu-id="4e4d1-192">`[ViewComponent]`上の属性名を使用するビューの コンポーネントの選択を指示する`PriorityList`に関連付けられているコンポーネントでは、ビューから、クラスのコンポーネントを参照しているときに、文字列"PriorityList"を使用するビューを探すときにします。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-192">The `[ViewComponent]` attribute above tells the view component selector to use the name `PriorityList` when looking for the views associated with the component, and to use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="4e4d1-193">について説明をさらに詳しく後述します。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-193">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="4e4d1-194">コンポーネントを使用して[依存性の注入](../../fundamentals/dependency-injection.md)データ コンテキストを使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-194">The component uses [dependency injection](../../fundamentals/dependency-injection.md) to make the data context available.</span></span>
* <span data-ttu-id="4e4d1-195">`InvokeAsync`公開をビューから呼び出すことができるメソッドは、任意の数の引数を受け取ることができます。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-195">`InvokeAsync` exposes a method which can be called from a view, and it can take an arbitrary number of arguments.</span></span>
* <span data-ttu-id="4e4d1-196">`InvokeAsync`メソッドのセットを返します`ToDo`を満たす項目、`isDone`と`maxPriority`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-196">The `InvokeAsync` method returns the set of `ToDo` items that satisfy the `isDone` and `maxPriority` parameters.</span></span>

### <a name="create-the-view-component-razor-view"></a><span data-ttu-id="4e4d1-197">ビューのコンポーネントの Razor ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-197">Create the view component Razor view</span></span>

* <span data-ttu-id="4e4d1-198">作成、*コンポーネント/ビュー/共有*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-198">Create the *Views/Shared/Components* folder.</span></span> <span data-ttu-id="4e4d1-199">このフォルダー**必要があります**という名前を付ける*コンポーネント*です。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-199">This folder **must** be named *Components*.</span></span>

* <span data-ttu-id="4e4d1-200">作成、*ビュー、共有、コンポーネント/PriorityList*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-200">Create the *Views/Shared/Components/PriorityList* folder.</span></span> <span data-ttu-id="4e4d1-201">このフォルダー名がビュー コンポーネント クラスの名前またはサフィックス マイナス クラスの名前に一致する必要があります (規約の後に使用すると、 *ViewComponent*クラス名にサフィックス)。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-201">This folder name must match the name of the view component class, or the name of the class minus the suffix (if we followed convention and used the *ViewComponent* suffix in the class name).</span></span> <span data-ttu-id="4e4d1-202">使用した場合、`ViewComponent`属性、クラス名は、属性の指定に一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-202">If you used the `ViewComponent` attribute, the class name would need to match the attribute designation.</span></span>

* <span data-ttu-id="4e4d1-203">作成、 *Views/Shared/Components/PriorityList/Default.cshtml* Razor ビュー。[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="4e4d1-203">Create a *Views/Shared/Components/PriorityList/Default.cshtml* Razor view: [!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span></span>
    
   <span data-ttu-id="4e4d1-204">Razor ビューの一覧を受け取る`TodoItem`し、それらを表示します。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-204">The Razor view takes a list of `TodoItem` and displays them.</span></span> <span data-ttu-id="4e4d1-205">場合のビュー コンポーネント`InvokeAsync`メソッド (サンプルのように、)、ビューの名前に合格しなかった*既定*規則によって、ビュー名に使用します。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-205">If the view component `InvokeAsync` method doesn't pass the name of the view (as in our sample), *Default* is used for the view name by convention.</span></span> <span data-ttu-id="4e4d1-206">、このチュートリアルで後ほど説明するビューの名前を渡す方法です。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-206">Later in the tutorial, I'll show you how to pass the name of the view.</span></span> <span data-ttu-id="4e4d1-207">特定のコント ローラーの既定のスタイルを上書きするには、コント ローラーに固有のビュー フォルダーにビューを追加 (たとえば*Views/Todo/Components/PriorityList/Default.cshtml)*です。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-207">To override the default styling for a specific controller, add a view to the controller-specific view folder (for example *Views/Todo/Components/PriorityList/Default.cshtml)*.</span></span>
    
    <span data-ttu-id="4e4d1-208">ビューのコンポーネントがコント ローラーに固有である場合は、コント ローラー固有のフォルダーに追加できます (*Views/Todo/Components/PriorityList/Default.cshtml*)。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-208">If the view component is controller-specific, you can add it to the controller-specific folder (*Views/Todo/Components/PriorityList/Default.cshtml*).</span></span>

* <span data-ttu-id="4e4d1-209">追加、`div`の最下位に優先度リスト コンポーネントへの呼び出しを含む、 *Views/Todo/index.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-209">Add a `div` containing a call to the priority list component to the bottom of the *Views/Todo/index.cshtml* file:</span></span>

    [!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFirst.cshtml?range=34-38)]

<span data-ttu-id="4e4d1-210">マークアップ`@await Component.InvokeAsync`ビュー コンポーネントを呼び出すための構文を示しています。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-210">The markup `@await Component.InvokeAsync` shows the syntax for calling view components.</span></span> <span data-ttu-id="4e4d1-211">最初の引数は、呼び出しを呼び出したりコンポーネントの名前です。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-211">The first argument is the name of the component we want to invoke or call.</span></span> <span data-ttu-id="4e4d1-212">後続のパラメーターは、コンポーネントに渡されます。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-212">Subsequent parameters are passed to the component.</span></span> <span data-ttu-id="4e4d1-213">`InvokeAsync`任意の数の引数を受け取ることができます。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-213">`InvokeAsync` can take an arbitrary number of arguments.</span></span>

<span data-ttu-id="4e4d1-214">アプリをテストします。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-214">Test the app.</span></span> <span data-ttu-id="4e4d1-215">次の図は、ToDo リストと優先度の項目を示します。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-215">The following image shows the ToDo list and the priority items:</span></span>

![todo リストと優先順位の項目](view-components/_static/pi.png)

<span data-ttu-id="4e4d1-217">コント ローラーから直接のビュー コンポーネントを呼び出すこともできます。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-217">You can also call the view component directly from the controller:</span></span>

[!code-csharp[Main](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![IndexVC アクションからの優先度のアイテム](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a><span data-ttu-id="4e4d1-219">ビュー名を指定します。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-219">Specifying a view name</span></span>

<span data-ttu-id="4e4d1-220">ビューの複雑なコンポーネントは、いくつかの条件下で、既定ではないビューを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-220">A complex view component might need to specify a non-default view under some conditions.</span></span> <span data-ttu-id="4e4d1-221">次のコードから"PVC"ビューを指定する方法を示しています、`InvokeAsync`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-221">The following code shows how to specify the "PVC" view  from the `InvokeAsync` method.</span></span> <span data-ttu-id="4e4d1-222">更新プログラム、`InvokeAsync`メソッドで、`PriorityListViewComponent`クラスです。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-222">Update the `InvokeAsync` method in the `PriorityListViewComponent` class.</span></span>

[!code-csharp[Main](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

<span data-ttu-id="4e4d1-223">コピー、 *Views/Shared/Components/PriorityList/Default.cshtml*ファイルという名前のビューを*Views/Shared/Components/PriorityList/PVC.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-223">Copy the *Views/Shared/Components/PriorityList/Default.cshtml* file to a view named *Views/Shared/Components/PriorityList/PVC.cshtml*.</span></span> <span data-ttu-id="4e4d1-224">PVC ビューが使用されているを示すために見出しを追加します。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-224">Add a heading to indicate the PVC view is being used.</span></span>

[!code-cshtml[Main](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

<span data-ttu-id="4e4d1-225">更新*Views/TodoList/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="4e4d1-225">Update *Views/TodoList/Index.cshtml*:</span></span>

<!-- Views/TodoList/Index.cshtml is never imported, so change to test tutorial -->

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="4e4d1-226">アプリを実行して、PVC ビューを確認します。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-226">Run the app and verify PVC view.</span></span>

![優先順位のビュー コンポーネント](view-components/_static/pvc.png)

<span data-ttu-id="4e4d1-228">PVC ビューが表示されていない場合は、優先度が 4 以上のビュー コンポーネントを呼び出していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-228">If the PVC view isn't rendered, verify you are calling the view component with a priority of 4 or higher.</span></span>

### <a name="examine-the-view-path"></a><span data-ttu-id="4e4d1-229">ビューのパスを調べる</span><span class="sxs-lookup"><span data-stu-id="4e4d1-229">Examine the view path</span></span>

* <span data-ttu-id="4e4d1-230">Priority ビューが返されません。 ようには以下に 3 つの優先順位 パラメーターを変更します。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-230">Change the priority parameter to three or less so the priority view isn't returned.</span></span>
* <span data-ttu-id="4e4d1-231">名前を一時的に、 *Views/Todo/Components/PriorityList/Default.cshtml*に*1Default.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-231">Temporarily rename the *Views/Todo/Components/PriorityList/Default.cshtml* to *1Default.cshtml*.</span></span>
* <span data-ttu-id="4e4d1-232">アプリのテストは、次のエラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-232">Test the app, you'll get the following error:</span></span>

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' wasn't found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* <span data-ttu-id="4e4d1-233">コピー *Views/Todo/Components/PriorityList/1Default.cshtml*に*Views/Shared/Components/PriorityList/Default.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-233">Copy *Views/Todo/Components/PriorityList/1Default.cshtml* to *Views/Shared/Components/PriorityList/Default.cshtml*.</span></span>
* <span data-ttu-id="4e4d1-234">一部のマークアップを追加、 *Shared* Todo ビュー コンポーネントを表示するビューは、 *Shared*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-234">Add some markup to the *Shared* Todo view component view to indicate the view is from the *Shared* folder.</span></span>
* <span data-ttu-id="4e4d1-235">テスト、 **Shared**コンポーネント ビュー。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-235">Test the **Shared** component view.</span></span>

![共有コンポーネントの表示と ToDo 出力](view-components/_static/shared.png)

### <a name="avoiding-magic-strings"></a><span data-ttu-id="4e4d1-237">マジック文字列の回避</span><span class="sxs-lookup"><span data-stu-id="4e4d1-237">Avoiding magic strings</span></span>

<span data-ttu-id="4e4d1-238">時間の安全性をコンパイルする場合は、クラス名に、ハード コーディングされたビュー コンポーネント名を置き換えることができます。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-238">If you want compile time safety, you can replace the hard-coded view component name with the class name.</span></span> <span data-ttu-id="4e4d1-239">"ViewComponent"サフィックスなしのビュー コンポーネントを作成します。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-239">Create the view component without the "ViewComponent" suffix:</span></span>

[!code-csharp[Main](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

<span data-ttu-id="4e4d1-240">追加、 `using` 、Razor にステートメントがファイルを表示し、使用、`nameof`演算子。</span><span class="sxs-lookup"><span data-stu-id="4e4d1-240">Add a `using` statement to your Razor view file, and use the `nameof` operator:</span></span>

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexNameof.cshtml?range=1-6,33-)]

## <a name="additional-resources"></a><span data-ttu-id="4e4d1-241">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="4e4d1-241">Additional Resources</span></span>

* [<span data-ttu-id="4e4d1-242">ビューへの依存性の注入</span><span class="sxs-lookup"><span data-stu-id="4e4d1-242">Dependency injection into views</span></span>](dependency-injection.md)
