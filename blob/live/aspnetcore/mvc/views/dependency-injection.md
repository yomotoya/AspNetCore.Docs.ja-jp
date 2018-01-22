---
title: "ビューに依存関係の挿入"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/dependency-injection
ms.openlocfilehash: cade61b1ebdb2b845b07117384475638c0227f7f
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="dependency-injection-into-views"></a><span data-ttu-id="3b0e6-102">ビューに依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="3b0e6-102">Dependency injection into views</span></span>

<span data-ttu-id="3b0e6-103">によって[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="3b0e6-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="3b0e6-104">ASP.NET Core をサポートしている[依存性の注入](xref:fundamentals/dependency-injection)の表示にします。</span><span class="sxs-lookup"><span data-stu-id="3b0e6-104">ASP.NET Core supports [dependency injection](xref:fundamentals/dependency-injection) into views.</span></span> <span data-ttu-id="3b0e6-105">これは、ローカライズや要素の表示を設定するためだけに必要なデータなどのビューに固有のサービスに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="3b0e6-105">This can be useful for view-specific services, such as localization or data required only for populating view elements.</span></span> <span data-ttu-id="3b0e6-106">管理しようとする必要があります[関心の分離](http://deviq.com/separation-of-concerns/)コント ローラーとビューの間です。</span><span class="sxs-lookup"><span data-stu-id="3b0e6-106">You should try to maintain [separation of concerns](http://deviq.com/separation-of-concerns/) between your controllers and views.</span></span> <span data-ttu-id="3b0e6-107">表示するビューは、ユーザー データのほとんどは、コント ローラーからに渡されます。</span><span class="sxs-lookup"><span data-stu-id="3b0e6-107">Most of the data your views display should be passed in from the controller.</span></span>

<span data-ttu-id="3b0e6-108">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="3b0e6-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="a-simple-example"></a><span data-ttu-id="3b0e6-109">単純な例</span><span class="sxs-lookup"><span data-stu-id="3b0e6-109">A Simple Example</span></span>

<span data-ttu-id="3b0e6-110">使用してビューに、サービスを挿入することができます、`@inject`ディレクティブです。</span><span class="sxs-lookup"><span data-stu-id="3b0e6-110">You can inject a service into a view using the `@inject` directive.</span></span> <span data-ttu-id="3b0e6-111">考えることができます`@inject`プロパティを表示します を追加して、DI を使用してプロパティを設定するとします。</span><span class="sxs-lookup"><span data-stu-id="3b0e6-111">You can think of `@inject` as adding a property to your view, and populating the property using DI.</span></span>

<span data-ttu-id="3b0e6-112">構文`@inject`:`@inject <type> <name>`</span><span class="sxs-lookup"><span data-stu-id="3b0e6-112">The syntax for `@inject`: `@inject <type> <name>`</span></span>

<span data-ttu-id="3b0e6-113">例`@inject`の動作。</span><span class="sxs-lookup"><span data-stu-id="3b0e6-113">An example of `@inject` in action:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]

<span data-ttu-id="3b0e6-114">このビューの一覧を表示`ToDoItem`全体的な統計を示す概要と共にのインスタンス。</span><span class="sxs-lookup"><span data-stu-id="3b0e6-114">This view displays a list of `ToDoItem` instances, along with a summary showing overall statistics.</span></span> <span data-ttu-id="3b0e6-115">概要が表示されます、挿入されたから`StatisticsService`です。</span><span class="sxs-lookup"><span data-stu-id="3b0e6-115">The summary is populated from the injected `StatisticsService`.</span></span> <span data-ttu-id="3b0e6-116">このサービスに登録されてで依存性の注入`ConfigureServices`で*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3b0e6-116">This service is registered for dependency injection in `ConfigureServices` in *Startup.cs*:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]

<span data-ttu-id="3b0e6-117">`StatisticsService`のセットに対して計算をいくつかを実行`ToDoItem`リポジトリを使用してアクセスするインスタンス。</span><span class="sxs-lookup"><span data-stu-id="3b0e6-117">The `StatisticsService` performs some calculations on the set of `ToDoItem` instances, which it accesses via a repository:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,26)]

<span data-ttu-id="3b0e6-118">サンプル リポジトリでは、メモリ内コレクションを使用します。</span><span class="sxs-lookup"><span data-stu-id="3b0e6-118">The sample repository uses an in-memory collection.</span></span> <span data-ttu-id="3b0e6-119">上記の実装が、リモートでアクセスされる大規模なデータ セットは (これはすべてメモリ内のデータの動作) は推奨されません。</span><span class="sxs-lookup"><span data-stu-id="3b0e6-119">The implementation shown above (which operates on all of the data in memory) is not recommended for large, remotely accessed data sets.</span></span>

<span data-ttu-id="3b0e6-120">このサンプルには、ビューにバインドされたモデルとビューに挿入される、サービスからデータが表示されます。</span><span class="sxs-lookup"><span data-stu-id="3b0e6-120">The sample displays data from the model bound to the view and the service injected into the view:</span></span>

![表示、アイテムの合計を一覧表示するには、項目、平均の優先度、およびその優先度レベルと完了を示すブール値を使用してタスクの一覧を完了します。](dependency-injection/_static/screenshot.png)

## <a name="populating-lookup-data"></a><span data-ttu-id="3b0e6-122">参照データを設定します。</span><span class="sxs-lookup"><span data-stu-id="3b0e6-122">Populating Lookup Data</span></span>

<span data-ttu-id="3b0e6-123">ビューの挿入をドロップダウン リストなどの UI 要素のオプションを設定することができます。</span><span class="sxs-lookup"><span data-stu-id="3b0e6-123">View injection can be useful to populate options in UI elements, such as dropdown lists.</span></span> <span data-ttu-id="3b0e6-124">性別、状態、およびその他の設定を指定するためのオプションを含むユーザー プロファイルのフォームを検討してください。</span><span class="sxs-lookup"><span data-stu-id="3b0e6-124">Consider a user profile form that includes options for specifying gender, state, and other preferences.</span></span> <span data-ttu-id="3b0e6-125">これらのオプションのセットの各データ アクセス サービスを要求し、モデルを作成し、コント ローラーが必要になります、標準の MVC アプローチを使用して、このようなフォームを表示または`ViewBag`をバインドするオプションのセットごとにします。</span><span class="sxs-lookup"><span data-stu-id="3b0e6-125">Rendering such a form using a standard MVC approach would require the controller to request data access services for each of these sets of options, and then populate a model or `ViewBag` with each set of options to be bound.</span></span>

<span data-ttu-id="3b0e6-126">その他の方法では、オプションを取得するビューに直接サービスを挿入します。</span><span class="sxs-lookup"><span data-stu-id="3b0e6-126">An alternative approach injects services directly into the view to obtain the options.</span></span> <span data-ttu-id="3b0e6-127">これには、このビューの要素の構築ロジックをビュー自体に移動し、コント ローラーで必要なコードの量が最小限に抑えます。</span><span class="sxs-lookup"><span data-stu-id="3b0e6-127">This minimizes the amount of code required by the controller, moving this view element construction logic into the view itself.</span></span> <span data-ttu-id="3b0e6-128">プロファイルの編集フォームに表示するコント ローラーのアクションは、のみ、フォームにプロファイル インスタンスを渡す必要があります。</span><span class="sxs-lookup"><span data-stu-id="3b0e6-128">The controller action to display a profile editing form only needs to pass the form the profile instance:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]

<span data-ttu-id="3b0e6-129">HTML フォーム使用してこれらの設定を更新するにはには、プロパティの 3 つのドロップダウン リストが含まれています。</span><span class="sxs-lookup"><span data-stu-id="3b0e6-129">The HTML form used to update these preferences includes dropdown lists for three of the properties:</span></span>

![名前、性別、状態、および好きな色のエントリを許可するフォームで [プロファイル] ビューを更新します。](dependency-injection/_static/updateprofile.png)

<span data-ttu-id="3b0e6-131">ビューに挿入された、サービスによっては、これらのリストが設定されます。</span><span class="sxs-lookup"><span data-stu-id="3b0e6-131">These lists are populated by a service that has been injected into the view:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]

<span data-ttu-id="3b0e6-132">`ProfileOptionsService`このフォームに必要なデータだけを提供するよう設計された UI レベル サービスには。</span><span class="sxs-lookup"><span data-stu-id="3b0e6-132">The `ProfileOptionsService` is a UI-level service designed to provide just the data needed for this form:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]

>[!TIP]
> <span data-ttu-id="3b0e6-133">忘れずに登録されている種類の依存性の注入を通じて要求が、`ConfigureServices`メソッド*Startup.cs*です。</span><span class="sxs-lookup"><span data-stu-id="3b0e6-133">Don't forget to register types you will request through dependency injection in the  `ConfigureServices` method in *Startup.cs*.</span></span>

## <a name="overriding-services"></a><span data-ttu-id="3b0e6-134">サービスをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="3b0e6-134">Overriding Services</span></span>

<span data-ttu-id="3b0e6-135">新しいサービスを提供するだけでなくこの手法をページ上の前に挿入したサービスを上書きする使用もできます。</span><span class="sxs-lookup"><span data-stu-id="3b0e6-135">In addition to injecting new services, this technique can also be used to override previously injected services on a page.</span></span> <span data-ttu-id="3b0e6-136">次の図は、最初の例で使用されるページのすべての利用可能なフィールドを示します。</span><span class="sxs-lookup"><span data-stu-id="3b0e6-136">The figure below shows all of the fields available on the page used in the first example:</span></span>

![Intellisense のコンテキスト メニューで、型指定された @ 記号を Html、コンポーネント、StatsService、および Url のフィールドを一覧表示します。](dependency-injection/_static/razor-fields.png)

<span data-ttu-id="3b0e6-138">既定のフィールドを含めることがわかります`Html`、 `Component`、および`Url`(だけでなく`StatsService`挿入お)。</span><span class="sxs-lookup"><span data-stu-id="3b0e6-138">As you can see, the default fields include `Html`, `Component`, and `Url` (as well as the `StatsService` that we injected).</span></span> <span data-ttu-id="3b0e6-139">インスタンスに、独自の既定の HTML ヘルパーを置き換えたい、でした簡単に行うを使用して`@inject`:</span><span class="sxs-lookup"><span data-stu-id="3b0e6-139">If for instance you wanted to replace the default HTML Helpers with your own, you could easily do so using `@inject`:</span></span>

[!code-html[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]

<span data-ttu-id="3b0e6-140">既存のサービスを拡張する場合からの継承、または独自の既存の実装の折り返しの中に単にこの手法を使用できます。</span><span class="sxs-lookup"><span data-stu-id="3b0e6-140">If you want to extend existing services, you can simply use this technique while inheriting from or wrapping the existing implementation with your own.</span></span>

## <a name="see-also"></a><span data-ttu-id="3b0e6-141">参照</span><span class="sxs-lookup"><span data-stu-id="3b0e6-141">See Also</span></span>

* <span data-ttu-id="3b0e6-142">Simon Timms ブログ:[ビュー内に参照データの取得](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)</span><span class="sxs-lookup"><span data-stu-id="3b0e6-142">Simon Timms Blog: [Getting Lookup Data Into Your View](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)</span></span>
