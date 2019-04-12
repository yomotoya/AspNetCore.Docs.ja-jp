---
title: ルーティング razor コンポーネント
author: guardrex
description: アプリや NavLink コンポーネントの詳細については、要求をルーティングする方法について説明します。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: razor-components/routing
ms.openlocfilehash: ef82fa7e0d571979a43fd8ce712bf4f22c06f9a7
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/10/2019
ms.locfileid: "59515675"
---
# <a name="razor-components-routing"></a><span data-ttu-id="d0210-103">ルーティング razor コンポーネント</span><span class="sxs-lookup"><span data-stu-id="d0210-103">Razor Components routing</span></span>

<span data-ttu-id="d0210-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d0210-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="d0210-105">アプリや NavLink コンポーネントの詳細については、要求をルーティングする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="d0210-105">Learn how to route requests in apps and about the NavLink component.</span></span>

## <a name="aspnet-core-endpoint-routing-integration"></a><span data-ttu-id="d0210-106">ASP.NET Core エンドポイントのルーティングの統合</span><span class="sxs-lookup"><span data-stu-id="d0210-106">ASP.NET Core endpoint routing integration</span></span>

<span data-ttu-id="d0210-107">Razor のコンポーネントが統合[ASP.NET Core ルーティング](xref:fundamentals/routing)します。</span><span class="sxs-lookup"><span data-stu-id="d0210-107">Razor Components are integrated into [ASP.NET Core routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="d0210-108">ASP.NET Core アプリが対話型の Razor コンポーネントでの着信接続を受け入れるように構成されている`MapComponentHub<TComponent>`で`Startup.Configure`します。</span><span class="sxs-lookup"><span data-stu-id="d0210-108">An ASP.NET Core app is configured to accept incoming connections for interactive Razor Components with `MapComponentHub<TComponent>` in `Startup.Configure`.</span></span> `MapComponentHub` <span data-ttu-id="d0210-109">指定するルート コンポーネント`App`セレクターに一致する DOM 要素内にレンダリングする必要があります`app`:</span><span class="sxs-lookup"><span data-stu-id="d0210-109">specifies that the root component `App` should be rendered within a DOM element matching the selector `app`:</span></span>

```csharp
app.UseRouting(routes =>
{
    routes.MapRazorPages();
    routes.MapComponentHub<App>("app");
});
```

## <a name="route-templates"></a><span data-ttu-id="d0210-110">ルート テンプレート</span><span class="sxs-lookup"><span data-stu-id="d0210-110">Route templates</span></span>

<span data-ttu-id="d0210-111">`<Router>`ルーティング、コンポーネントを使用して、ルート テンプレートは、アクセス可能な各コンポーネントに提供されます。</span><span class="sxs-lookup"><span data-stu-id="d0210-111">The `<Router>` component enables routing, and a route template is provided to each accessible component.</span></span> <span data-ttu-id="d0210-112">`<Router>`コンポーネントに表示されます、 *Components/App.razor*ファイル。</span><span class="sxs-lookup"><span data-stu-id="d0210-112">The `<Router>` component appears in the *Components/App.razor* file:</span></span>

```cshtml
<Router AppAssembly="typeof(Program).Assembly" />
```

<span data-ttu-id="d0210-113">ときに、 *.razor*または *.cshtml*ファイルと、`@page`ディレクティブをコンパイルすると、生成されたクラスが指定された、<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>ルート テンプレートを指定します。</span><span class="sxs-lookup"><span data-stu-id="d0210-113">When a *.razor* or *.cshtml* file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="d0210-114">ルーターが持つクラスでコンポーネントが、実行時に、`RouteAttribute`どのコンポーネントが要求された URL に一致するルート テンプレートを表示します。</span><span class="sxs-lookup"><span data-stu-id="d0210-114">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="d0210-115">コンポーネントには、複数のルート テンプレートを適用できます。</span><span class="sxs-lookup"><span data-stu-id="d0210-115">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="d0210-116">次のコンポーネントがの要求に応答`/BlazorRoute`と`/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="d0210-116">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?name=snippet_BlazorRoute)]

`<Router>` <span data-ttu-id="d0210-117">では、レンダリング時に要求されたルートのフォールバック コンポーネントの設定は、解決はありません。</span><span class="sxs-lookup"><span data-stu-id="d0210-117">supports setting a fallback component for rendering when a requested route isn't resolved.</span></span> <span data-ttu-id="d0210-118">設定をこのオプトイン シナリオを有効にする、`FallbackComponent`フォールバック コンポーネント クラスの型のパラメーター。</span><span class="sxs-lookup"><span data-stu-id="d0210-118">Enable this opt-in scenario by setting the `FallbackComponent` parameter to the type of the fallback component class.</span></span>

<span data-ttu-id="d0210-119">次の例で定義されているコンポーネントを設定する*Pages/MyFallbackRazorComponent.cshtml*フォールバック コンポーネントとして、 `<Router>`:</span><span class="sxs-lookup"><span data-stu-id="d0210-119">The following example sets a component defined in *Pages/MyFallbackRazorComponent.cshtml* as the fallback component for a `<Router>`:</span></span>

```cshtml
<Router ... FallbackComponent="typeof(Pages.MyFallbackRazorComponent)" />
```

> [!IMPORTANT]
> <span data-ttu-id="d0210-120">ルートを正しく生成するアプリを含める必要があります、`<base>`タグにその*wwwroot/index.html*で指定したアプリ ベース パスとファイル、`href`属性 (`<base href="/">`)。</span><span class="sxs-lookup"><span data-stu-id="d0210-120">To generate routes properly, the app must include a `<base>` tag in its *wwwroot/index.html* file with the app base path specified in the `href` attribute (`<base href="/">`).</span></span> <span data-ttu-id="d0210-121">詳細については、「 <xref:host-and-deploy/razor-components-blazor/blazor#app-base-path> 」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d0210-121">For more information, see <xref:host-and-deploy/razor-components-blazor/blazor#app-base-path>.</span></span>

## <a name="route-parameters"></a><span data-ttu-id="d0210-122">ルート パラメーター</span><span class="sxs-lookup"><span data-stu-id="d0210-122">Route parameters</span></span>

<span data-ttu-id="d0210-123">ルーターは、同じ名前 (大文字と小文字は区別されません) の対応するコンポーネントのパラメーターを設定するのにルート パラメーターを使用します。</span><span class="sxs-lookup"><span data-stu-id="d0210-123">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive):</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?name=snippet_RouteParameter&highlight=2,7-8)]

<span data-ttu-id="d0210-124">省略可能なパラメーターは、まだサポートされていませんので、2 つ`@page`ディレクティブは、上記の例に適用されます。</span><span class="sxs-lookup"><span data-stu-id="d0210-124">Optional parameters aren't supported yet, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="d0210-125">最初のパラメーターを指定せず、コンポーネントへの移動を許可します。</span><span class="sxs-lookup"><span data-stu-id="d0210-125">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="d0210-126">2 番目の`@page`ディレクティブは、`{text}`ルート パラメーターと値を割り当てます、`Text`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="d0210-126">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="d0210-127">ルート制約</span><span class="sxs-lookup"><span data-stu-id="d0210-127">Route constraints</span></span>

<span data-ttu-id="d0210-128">ルート制約では、コンポーネントへのルート セグメントで一致する型を強制します。</span><span class="sxs-lookup"><span data-stu-id="d0210-128">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="d0210-129">次の例では、ルート ユーザー コンポーネントにのみ一致する場合。</span><span class="sxs-lookup"><span data-stu-id="d0210-129">In the following example, the route to the Users component only matches if:</span></span>

* <span data-ttu-id="d0210-130">`Id`ルート セグメントでは、要求 URL に存在します。</span><span class="sxs-lookup"><span data-stu-id="d0210-130">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="d0210-131">`Id`セグメントは、整数 (`int`)。</span><span class="sxs-lookup"><span data-stu-id="d0210-131">The `Id` segment is an integer (`int`).</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.cshtml?highlight=1)]

<span data-ttu-id="d0210-132">次の表に示すようにルート制約は使用できます。</span><span class="sxs-lookup"><span data-stu-id="d0210-132">The route constraints shown in the following table are available.</span></span> <span data-ttu-id="d0210-133">インバリアント カルチャに一致するルート制約では、詳細については、表の下には、警告を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d0210-133">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="d0210-134">制約</span><span class="sxs-lookup"><span data-stu-id="d0210-134">Constraint</span></span> | <span data-ttu-id="d0210-135">例</span><span class="sxs-lookup"><span data-stu-id="d0210-135">Example</span></span>           | <span data-ttu-id="d0210-136">一致の例</span><span class="sxs-lookup"><span data-stu-id="d0210-136">Example Matches</span></span>                                                                  | <span data-ttu-id="d0210-137">インバリアント</span><span class="sxs-lookup"><span data-stu-id="d0210-137">Invariant</span></span><br><span data-ttu-id="d0210-138">カルチャ</span><span class="sxs-lookup"><span data-stu-id="d0210-138">culture</span></span><br><span data-ttu-id="d0210-139">一致</span><span class="sxs-lookup"><span data-stu-id="d0210-139">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | `true`<span data-ttu-id="d0210-140">,</span><span class="sxs-lookup"><span data-stu-id="d0210-140">,</span></span> `FALSE`                                                                  | <span data-ttu-id="d0210-141">いいえ</span><span class="sxs-lookup"><span data-stu-id="d0210-141">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | `2016-12-31`<span data-ttu-id="d0210-142">,</span><span class="sxs-lookup"><span data-stu-id="d0210-142">,</span></span> `2016-12-31 7:32pm`                                                | <span data-ttu-id="d0210-143">[はい]</span><span class="sxs-lookup"><span data-stu-id="d0210-143">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | `49.99`<span data-ttu-id="d0210-144">,</span><span class="sxs-lookup"><span data-stu-id="d0210-144">,</span></span> `-1,000.01`                                                             | <span data-ttu-id="d0210-145">[はい]</span><span class="sxs-lookup"><span data-stu-id="d0210-145">Yes</span></span>                              |
| `double`   | `{weight:double}` | `1.234`<span data-ttu-id="d0210-146">,</span><span class="sxs-lookup"><span data-stu-id="d0210-146">,</span></span> `-1,001.01e8`                                                           | <span data-ttu-id="d0210-147">[はい]</span><span class="sxs-lookup"><span data-stu-id="d0210-147">Yes</span></span>                              |
| `float`    | `{weight:float}`  | `1.234`<span data-ttu-id="d0210-148">,</span><span class="sxs-lookup"><span data-stu-id="d0210-148">,</span></span> `-1,001.01e8`                                                           | <span data-ttu-id="d0210-149">はい</span><span class="sxs-lookup"><span data-stu-id="d0210-149">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | `CD2C1638-1638-72D5-1638-DEADBEEF1638`<span data-ttu-id="d0210-150">,</span><span class="sxs-lookup"><span data-stu-id="d0210-150">,</span></span> `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | <span data-ttu-id="d0210-151">いいえ</span><span class="sxs-lookup"><span data-stu-id="d0210-151">No</span></span>                               |
| `int`      | `{id:int}`        | `123456789`<span data-ttu-id="d0210-152">,</span><span class="sxs-lookup"><span data-stu-id="d0210-152">,</span></span> `-123456789`                                                        | <span data-ttu-id="d0210-153">はい</span><span class="sxs-lookup"><span data-stu-id="d0210-153">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | `123456789`<span data-ttu-id="d0210-154">,</span><span class="sxs-lookup"><span data-stu-id="d0210-154">,</span></span> `-123456789`                                                        | <span data-ttu-id="d0210-155">[はい]</span><span class="sxs-lookup"><span data-stu-id="d0210-155">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="d0210-156">URL の妥当性を検証し、CLR 型 (`int` や `DateTime` など) に変換されるルート制約では、常にインバリアント カルチャが使用されます。</span><span class="sxs-lookup"><span data-stu-id="d0210-156">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="d0210-157">これらの制約では、URL がローカライズ不可であることが前提となります。</span><span class="sxs-lookup"><span data-stu-id="d0210-157">These constraints assume that the URL is non-localizable.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="d0210-158">NavLink コンポーネント</span><span class="sxs-lookup"><span data-stu-id="d0210-158">NavLink component</span></span>

<span data-ttu-id="d0210-159">HTML の代わりに NavLink コンポーネントを使用して`<a>`ナビゲーション リンクを作成するときの要素。</span><span class="sxs-lookup"><span data-stu-id="d0210-159">Use a NavLink component in place of HTML `<a>` elements when creating navigation links.</span></span> <span data-ttu-id="d0210-160">NavLink コンポーネントと同様に動作を`<a>`切り替えます点を除いて、要素、 `active` CSS クラスかどうかに基づいて、`href`現在の URL と一致します。</span><span class="sxs-lookup"><span data-stu-id="d0210-160">A NavLink component behaves like an `<a>` element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="d0210-161">`active`クラスは、どのページが表示されるナビゲーション リンクの間でのアクティブなページを理解します。</span><span class="sxs-lookup"><span data-stu-id="d0210-161">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="d0210-162">次の NavMenu コンポーネントを作成、[ブートス トラップ](https://getbootstrap.com/docs/)NavLink コンポーネントを使用する方法については、ナビゲーション バー。</span><span class="sxs-lookup"><span data-stu-id="d0210-162">The following NavMenu component creates a [Bootstrap](https://getbootstrap.com/docs/) navigation bar that demonstrates how to use NavLink components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Shared/NavMenu.cshtml?name=snippet_NavLinks&highlight=4-6,9-11)]

<span data-ttu-id="d0210-163">2 つ`NavLinkMatch`オプション。</span><span class="sxs-lookup"><span data-stu-id="d0210-163">There are two `NavLinkMatch` options:</span></span>

* `NavLinkMatch.All` <span data-ttu-id="d0210-164">&ndash; 全体の現在の URL と一致するときに、NavLink をアクティブながあることを指定します。</span><span class="sxs-lookup"><span data-stu-id="d0210-164">&ndash; Specifies that the NavLink should be active when it matches the entire current URL.</span></span>
* `NavLinkMatch.Prefix` <span data-ttu-id="d0210-165">&ndash; 現在の URL のプレフィックスと一致するときに、NavLink をアクティブながあることを指定します。</span><span class="sxs-lookup"><span data-stu-id="d0210-165">&ndash; Specifies that the NavLink should be active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="d0210-166">前の例では、ホーム NavLink (`href=""`) すべての Url と一致して、常に受信、 `active` CSS クラス。</span><span class="sxs-lookup"><span data-stu-id="d0210-166">In the preceding example, the Home NavLink (`href=""`) matches all URLs and always receives the `active` CSS class.</span></span> <span data-ttu-id="d0210-167">2 番目の NavLink のみを受信、 `active` Blazor ルート コンポーネントにアクセスするユーザー クラス (`href="BlazorRoute"`)。</span><span class="sxs-lookup"><span data-stu-id="d0210-167">The second NavLink only receives the `active` class when the user visits the Blazor Route component (`href="BlazorRoute"`).</span></span>
