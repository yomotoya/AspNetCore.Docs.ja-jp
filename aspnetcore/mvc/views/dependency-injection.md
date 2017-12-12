---
title: "ビューに依存関係の挿入"
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 80fb9e43-e4db-4af2-b2a8-e1364a712f69
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/dependency-injection
ms.openlocfilehash: 4586f50bc663b7269914dfff28b61342e3991a48
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="dependency-injection-into-views"></a><span data-ttu-id="4218b-103">ビューに依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="4218b-103">Dependency injection into views</span></span>

<span data-ttu-id="4218b-104">によって[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="4218b-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="4218b-105">ASP.NET Core をサポートしている[依存性の注入](xref:fundamentals/dependency-injection)の表示にします。</span><span class="sxs-lookup"><span data-stu-id="4218b-105">ASP.NET Core supports [dependency injection](xref:fundamentals/dependency-injection) into views.</span></span> <span data-ttu-id="4218b-106">これは、ローカライズや要素の表示を設定するためだけに必要なデータなどのビューに固有のサービスに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="4218b-106">This can be useful for view-specific services, such as localization or data required only for populating view elements.</span></span> <span data-ttu-id="4218b-107">管理しようとする必要があります[関心の分離](http://deviq.com/separation-of-concerns/)コント ローラーとビューの間です。</span><span class="sxs-lookup"><span data-stu-id="4218b-107">You should try to maintain [separation of concerns](http://deviq.com/separation-of-concerns/) between your controllers and views.</span></span> <span data-ttu-id="4218b-108">表示するビューは、ユーザー データのほとんどは、コント ローラーからに渡されます。</span><span class="sxs-lookup"><span data-stu-id="4218b-108">Most of the data your views display should be passed in from the controller.</span></span>

<span data-ttu-id="4218b-109">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="4218b-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="a-simple-example"></a><span data-ttu-id="4218b-110">単純な例</span><span class="sxs-lookup"><span data-stu-id="4218b-110">A Simple Example</span></span>

<span data-ttu-id="4218b-111">使用してビューに、サービスを挿入することができます、`@inject`ディレクティブです。</span><span class="sxs-lookup"><span data-stu-id="4218b-111">You can inject a service into a view using the `@inject` directive.</span></span> <span data-ttu-id="4218b-112">考えることができます`@inject`プロパティを表示します を追加して、DI を使用してプロパティを設定するとします。</span><span class="sxs-lookup"><span data-stu-id="4218b-112">You can think of `@inject` as adding a property to your view, and populating the property using DI.</span></span>

<span data-ttu-id="4218b-113">構文`@inject`:`@inject <type> <name>`</span><span class="sxs-lookup"><span data-stu-id="4218b-113">The syntax for `@inject`: `@inject <type> <name>`</span></span>

<span data-ttu-id="4218b-114">例`@inject`の動作。</span><span class="sxs-lookup"><span data-stu-id="4218b-114">An example of `@inject` in action:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]

<span data-ttu-id="4218b-115">このビューの一覧を表示`ToDoItem`全体的な統計を示す概要と共にのインスタンス。</span><span class="sxs-lookup"><span data-stu-id="4218b-115">This view displays a list of `ToDoItem` instances, along with a summary showing overall statistics.</span></span> <span data-ttu-id="4218b-116">概要が表示されます、挿入されたから`StatisticsService`です。</span><span class="sxs-lookup"><span data-stu-id="4218b-116">The summary is populated from the injected `StatisticsService`.</span></span> <span data-ttu-id="4218b-117">このサービスに登録されてで依存性の注入`ConfigureServices`で*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="4218b-117">This service is registered for dependency injection in `ConfigureServices` in *Startup.cs*:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]

<span data-ttu-id="4218b-118">`StatisticsService`のセットに対して計算をいくつかを実行`ToDoItem`リポジトリを使用してアクセスするインスタンス。</span><span class="sxs-lookup"><span data-stu-id="4218b-118">The `StatisticsService` performs some calculations on the set of `ToDoItem` instances, which it accesses via a repository:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,26)]

<span data-ttu-id="4218b-119">サンプル リポジトリでは、メモリ内コレクションを使用します。</span><span class="sxs-lookup"><span data-stu-id="4218b-119">The sample repository uses an in-memory collection.</span></span> <span data-ttu-id="4218b-120">上記の実装が、リモートでアクセスされる大規模なデータ セットは (これはすべてメモリ内のデータの動作) は推奨されません。</span><span class="sxs-lookup"><span data-stu-id="4218b-120">The implementation shown above (which operates on all of the data in memory) is not recommended for large, remotely accessed data sets.</span></span>

<span data-ttu-id="4218b-121">このサンプルには、ビューにバインドされたモデルとビューに挿入される、サービスからデータが表示されます。</span><span class="sxs-lookup"><span data-stu-id="4218b-121">The sample displays data from the model bound to the view and the service injected into the view:</span></span>

![表示、アイテムの合計を一覧表示するには、項目、平均の優先度、およびその優先度レベルと完了を示すブール値を使用してタスクの一覧を完了します。](dependency-injection/_static/screenshot.png)

## <a name="populating-lookup-data"></a><span data-ttu-id="4218b-123">参照データを設定します。</span><span class="sxs-lookup"><span data-stu-id="4218b-123">Populating Lookup Data</span></span>

<span data-ttu-id="4218b-124">ビューの挿入をドロップダウン リストなどの UI 要素のオプションを設定することができます。</span><span class="sxs-lookup"><span data-stu-id="4218b-124">View injection can be useful to populate options in UI elements, such as dropdown lists.</span></span> <span data-ttu-id="4218b-125">性別、状態、およびその他の設定を指定するためのオプションを含むユーザー プロファイルのフォームを検討してください。</span><span class="sxs-lookup"><span data-stu-id="4218b-125">Consider a user profile form that includes options for specifying gender, state, and other preferences.</span></span> <span data-ttu-id="4218b-126">これらのオプションのセットの各データ アクセス サービスを要求し、モデルを作成し、コント ローラーが必要になります、標準の MVC アプローチを使用して、このようなフォームを表示または`ViewBag`をバインドするオプションのセットごとにします。</span><span class="sxs-lookup"><span data-stu-id="4218b-126">Rendering such a form using a standard MVC approach would require the controller to request data access services for each of these sets of options, and then populate a model or `ViewBag` with each set of options to be bound.</span></span>

<span data-ttu-id="4218b-127">その他の方法では、オプションを取得するビューに直接サービスを挿入します。</span><span class="sxs-lookup"><span data-stu-id="4218b-127">An alternative approach injects services directly into the view to obtain the options.</span></span> <span data-ttu-id="4218b-128">これには、このビューの要素の構築ロジックをビュー自体に移動し、コント ローラーで必要なコードの量が最小限に抑えます。</span><span class="sxs-lookup"><span data-stu-id="4218b-128">This minimizes the amount of code required by the controller, moving this view element construction logic into the view itself.</span></span> <span data-ttu-id="4218b-129">プロファイルの編集フォームに表示するコント ローラーのアクションは、のみ、フォームにプロファイル インスタンスを渡す必要があります。</span><span class="sxs-lookup"><span data-stu-id="4218b-129">The controller action to display a profile editing form only needs to pass the form the profile instance:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]

<span data-ttu-id="4218b-130">HTML フォーム使用してこれらの設定を更新するにはには、プロパティの 3 つのドロップダウン リストが含まれています。</span><span class="sxs-lookup"><span data-stu-id="4218b-130">The HTML form used to update these preferences includes dropdown lists for three of the properties:</span></span>

![名前、性別、状態、および好きな色のエントリを許可するフォームで [プロファイル] ビューを更新します。](dependency-injection/_static/updateprofile.png)

<span data-ttu-id="4218b-132">ビューに挿入された、サービスによっては、これらのリストが設定されます。</span><span class="sxs-lookup"><span data-stu-id="4218b-132">These lists are populated by a service that has been injected into the view:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]

<span data-ttu-id="4218b-133">`ProfileOptionsService`このフォームに必要なデータだけを提供するよう設計された UI レベル サービスには。</span><span class="sxs-lookup"><span data-stu-id="4218b-133">The `ProfileOptionsService` is a UI-level service designed to provide just the data needed for this form:</span></span>

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]

>[!TIP]
> <span data-ttu-id="4218b-134">忘れずに登録されている種類の依存性の注入を通じて要求が、`ConfigureServices`メソッド*Startup.cs*です。</span><span class="sxs-lookup"><span data-stu-id="4218b-134">Don't forget to register types you will request through dependency injection in the  `ConfigureServices` method in *Startup.cs*.</span></span>

## <a name="overriding-services"></a><span data-ttu-id="4218b-135">サービスをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="4218b-135">Overriding Services</span></span>

<span data-ttu-id="4218b-136">新しいサービスを提供するだけでなくこの手法をページ上の前に挿入したサービスを上書きする使用もできます。</span><span class="sxs-lookup"><span data-stu-id="4218b-136">In addition to injecting new services, this technique can also be used to override previously injected services on a page.</span></span> <span data-ttu-id="4218b-137">次の図は、最初の例で使用されるページのすべての利用可能なフィールドを示します。</span><span class="sxs-lookup"><span data-stu-id="4218b-137">The figure below shows all of the fields available on the page used in the first example:</span></span>

![Intellisense のコンテキスト メニューで、型指定された @ 記号を Html、コンポーネント、StatsService、および Url のフィールドを一覧表示します。](dependency-injection/_static/razor-fields.png)

<span data-ttu-id="4218b-139">既定のフィールドを含めることがわかります`Html`、 `Component`、および`Url`(だけでなく`StatsService`挿入お)。</span><span class="sxs-lookup"><span data-stu-id="4218b-139">As you can see, the default fields include `Html`, `Component`, and `Url` (as well as the `StatsService` that we injected).</span></span> <span data-ttu-id="4218b-140">インスタンスに、独自の既定の HTML ヘルパーを置き換えたい、でした簡単に行うを使用して`@inject`:</span><span class="sxs-lookup"><span data-stu-id="4218b-140">If for instance you wanted to replace the default HTML Helpers with your own, you could easily do so using `@inject`:</span></span>

[!code-html[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]

<span data-ttu-id="4218b-141">既存のサービスを拡張する場合からの継承、または独自の既存の実装の折り返しの中に単にこの手法を使用できます。</span><span class="sxs-lookup"><span data-stu-id="4218b-141">If you want to extend existing services, you can simply use this technique while inheriting from or wrapping the existing implementation with your own.</span></span>

## <a name="see-also"></a><span data-ttu-id="4218b-142">関連項目</span><span class="sxs-lookup"><span data-stu-id="4218b-142">See Also</span></span>

* <span data-ttu-id="4218b-143">Simon Timms ブログ:[ビュー内に参照データの取得](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)</span><span class="sxs-lookup"><span data-stu-id="4218b-143">Simon Timms Blog: [Getting Lookup Data Into Your View](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)</span></span>
