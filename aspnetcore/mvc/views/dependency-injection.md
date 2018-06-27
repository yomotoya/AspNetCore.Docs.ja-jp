---
title: ASP.NET Core でのビューへの依存関係の挿入
author: ardalis
description: ASP.NET Core で MVC ビューへの依存関係の挿入をサポートする方法について説明します。
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/dependency-injection
ms.openlocfilehash: d33f0253fc7c1329e8bab400ace4c4ce8d10d792
ms.sourcegitcommit: 4e3497bda0c3e5011ffba3717eb61a1d46c61c15
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/14/2018
ms.locfileid: "35613061"
---
# <a name="dependency-injection-into-views-in-aspnet-core"></a><span data-ttu-id="bb958-103">ASP.NET Core でのビューへの依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="bb958-103">Dependency injection into views in ASP.NET Core</span></span>

<span data-ttu-id="bb958-104">作成者: [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="bb958-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="bb958-105">ASP.NET Core では、ビューへの[依存関係の挿入](xref:fundamentals/dependency-injection)をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="bb958-105">ASP.NET Core supports [dependency injection](xref:fundamentals/dependency-injection) into views.</span></span> <span data-ttu-id="bb958-106">この機能は、ビュー要素を作成するためだけに必要なローカライズやデータなど、ビュー固有のサービスに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="bb958-106">This can be useful for view-specific services, such as localization or data required only for populating view elements.</span></span> <span data-ttu-id="bb958-107">コントローラーとビューの間で[関心の分離](http://deviq.com/separation-of-concerns/)を保持する必要があります。</span><span class="sxs-lookup"><span data-stu-id="bb958-107">You should try to maintain [separation of concerns](http://deviq.com/separation-of-concerns/) between your controllers and views.</span></span> <span data-ttu-id="bb958-108">ビューに表示されるデータのほとんどは、コントローラーから渡されます。</span><span class="sxs-lookup"><span data-stu-id="bb958-108">Most of the data your views display should be passed in from the controller.</span></span>

<span data-ttu-id="bb958-109">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="bb958-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="a-simple-example"></a><span data-ttu-id="bb958-110">簡単な例</span><span class="sxs-lookup"><span data-stu-id="bb958-110">A Simple Example</span></span>

<span data-ttu-id="bb958-111">`@inject` ディレクティブを使用して、サービスをビューに挿入することができます。</span><span class="sxs-lookup"><span data-stu-id="bb958-111">You can inject a service into a view using the `@inject` directive.</span></span> <span data-ttu-id="bb958-112">`@inject` は、ビューにプロパティを追加したり、DI を使用してプロパティを作成したりすると考えることができます。</span><span class="sxs-lookup"><span data-stu-id="bb958-112">You can think of `@inject` as adding a property to your view, and populating the property using DI.</span></span>

<span data-ttu-id="bb958-113">`@inject`: `@inject <type> <name>` の構文</span><span class="sxs-lookup"><span data-stu-id="bb958-113">The syntax for `@inject`: `@inject <type> <name>`</span></span>

<span data-ttu-id="bb958-114">アクションの `@inject` の例:</span><span class="sxs-lookup"><span data-stu-id="bb958-114">An example of `@inject` in action:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]

<span data-ttu-id="bb958-115">このビューでは、全体的な統計を示す概要と共に、`ToDoItem` インスタンスのリストが表示されます。</span><span class="sxs-lookup"><span data-stu-id="bb958-115">This view displays a list of `ToDoItem` instances, along with a summary showing overall statistics.</span></span> <span data-ttu-id="bb958-116">概要は、挿入された `StatisticsService` から作成されます。</span><span class="sxs-lookup"><span data-stu-id="bb958-116">The summary is populated from the injected `StatisticsService`.</span></span> <span data-ttu-id="bb958-117">このサービスは、*Startup.cs* 内の `ConfigureServices` の依存関係の挿入に登録されます。</span><span class="sxs-lookup"><span data-stu-id="bb958-117">This service is registered for dependency injection in `ConfigureServices` in *Startup.cs*:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]

<span data-ttu-id="bb958-118">`StatisticsService` では、`ToDoItem` インスタンスのセットでいくつかの計算を実行します。これには、次のリポジトリを使用してアクセスします。</span><span class="sxs-lookup"><span data-stu-id="bb958-118">The `StatisticsService` performs some calculations on the set of `ToDoItem` instances, which it accesses via a repository:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,25)]

<span data-ttu-id="bb958-119">サンプルのリポジトリでは、メモリ内のコレクションを使用します。</span><span class="sxs-lookup"><span data-stu-id="bb958-119">The sample repository uses an in-memory collection.</span></span> <span data-ttu-id="bb958-120">上記の (メモリ内のすべてのデータを操作する) 実装は、大規模なリモートでアクセスされるデータ セットにはお勧めしません。</span><span class="sxs-lookup"><span data-stu-id="bb958-120">The implementation shown above (which operates on all of the data in memory) isn't recommended for large, remotely accessed data sets.</span></span>

<span data-ttu-id="bb958-121">サンプルには、ビューとビューに挿入されたサービスにバインドされたモデルからのデータが表示されています。</span><span class="sxs-lookup"><span data-stu-id="bb958-121">The sample displays data from the model bound to the view and the service injected into the view:</span></span>

![合計項目数、完了した項目数、優先度の平均、および優先度と完了を示すブール値を含むタスクのリストを一覧する To Do ビュー。](dependency-injection/_static/screenshot.png)

## <a name="populating-lookup-data"></a><span data-ttu-id="bb958-123">ルックアップ データの作成</span><span class="sxs-lookup"><span data-stu-id="bb958-123">Populating Lookup Data</span></span>

<span data-ttu-id="bb958-124">ビューの挿入は、ドロップダウン リストなど、UI 要素のオプションの作成に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="bb958-124">View injection can be useful to populate options in UI elements, such as dropdown lists.</span></span> <span data-ttu-id="bb958-125">性別、状態、およびその他の基本設定を指定するオプションを含む、ユーザー プロファイル フォームを検討します。</span><span class="sxs-lookup"><span data-stu-id="bb958-125">Consider a user profile form that includes options for specifying gender, state, and other preferences.</span></span> <span data-ttu-id="bb958-126">標準の MVC アプローチを使用するフォームのようなレンダリングには、これらのオプションのセットごとにデータ アクセス サービスを要求し、バインドするオプションの各セットを含むモデルまたは `ViewBag` を作成するために、コントローラーが必要です。</span><span class="sxs-lookup"><span data-stu-id="bb958-126">Rendering such a form using a standard MVC approach would require the controller to request data access services for each of these sets of options, and then populate a model or `ViewBag` with each set of options to be bound.</span></span>

<span data-ttu-id="bb958-127">別のアプローチでは、オプションを取得するために、ビューに直接サービスを挿入します。</span><span class="sxs-lookup"><span data-stu-id="bb958-127">An alternative approach injects services directly into the view to obtain the options.</span></span> <span data-ttu-id="bb958-128">これにより、このビュー要素の構築ロジックをビュー自体に移動して、コントローラーから要求されるコードの量を最小限にします。</span><span class="sxs-lookup"><span data-stu-id="bb958-128">This minimizes the amount of code required by the controller, moving this view element construction logic into the view itself.</span></span> <span data-ttu-id="bb958-129">プロファイルの編集フォームを表示するコントローラー アクションは、フォームにプロファイル インスタンスを渡すためにのみ必要です。</span><span class="sxs-lookup"><span data-stu-id="bb958-129">The controller action to display a profile editing form only needs to pass the form the profile instance:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]

<span data-ttu-id="bb958-130">これらの基本設定を更新するために使用される HTML フォームには、3 つのプロパティのドロップダウン リストが含まれます。</span><span class="sxs-lookup"><span data-stu-id="bb958-130">The HTML form used to update these preferences includes dropdown lists for three of the properties:</span></span>

![名前、性別、状態、好きな色の入力を許可するフォームを含む、[Update Profile] (プロファイルの更新) ビュー。](dependency-injection/_static/updateprofile.png)

<span data-ttu-id="bb958-132">これらのリストは、ビューに挿入されたサービスによって作成されます。</span><span class="sxs-lookup"><span data-stu-id="bb958-132">These lists are populated by a service that has been injected into the view:</span></span>

[!code-cshtml[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]

<span data-ttu-id="bb958-133">`ProfileOptionsService` は、このフォームに必要なデータだけを提供する UI レベルのサービスです。</span><span class="sxs-lookup"><span data-stu-id="bb958-133">The `ProfileOptionsService` is a UI-level service designed to provide just the data needed for this form:</span></span>

[!code-csharp[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]

> [!IMPORTANT]
> <span data-ttu-id="bb958-134">`Startup.ConfigureServices` での依存関係の挿入を介して要求する型を登録するのを忘れないでください。</span><span class="sxs-lookup"><span data-stu-id="bb958-134">Don't forget to register types you request through dependency injection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="bb958-135">サービス プロバイダーは [GetRequiredService](/dotnet/api/microsoft.extensions.dependencyinjection.serviceproviderserviceextensions.getrequiredservice) を介して内部的にクエリが実行されるため、未登録の型は実行時に例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="bb958-135">An unregistered type throws an exception at runtime because the service provider is internally queried via [GetRequiredService](/dotnet/api/microsoft.extensions.dependencyinjection.serviceproviderserviceextensions.getrequiredservice).</span></span>

## <a name="overriding-services"></a><span data-ttu-id="bb958-136">サービスのオーバーライド</span><span class="sxs-lookup"><span data-stu-id="bb958-136">Overriding Services</span></span>

<span data-ttu-id="bb958-137">新しいサービスを挿入するだけでなく、この技術はページ上の以前に挿入されたサービスをオーバーライドするためにも使用できます。</span><span class="sxs-lookup"><span data-stu-id="bb958-137">In addition to injecting new services, this technique can also be used to override previously injected services on a page.</span></span> <span data-ttu-id="bb958-138">以下の図は、最初の例で使用されたページで利用できるすべてのフィールドを示しています。</span><span class="sxs-lookup"><span data-stu-id="bb958-138">The figure below shows all of the fields available on the page used in the first example:</span></span>

![@ 記号を入力して、Html、Component、StatsService、Url フィールドが一覧された IntelliSense コンテキスト メニュー](dependency-injection/_static/razor-fields.png)

<span data-ttu-id="bb958-140">ご覧のように、既定のフィールドには、`Html`、`Component`、`Url` (さらに、挿入した `StatsService`) が含まれます。</span><span class="sxs-lookup"><span data-stu-id="bb958-140">As you can see, the default fields include `Html`, `Component`, and `Url` (as well as the `StatsService` that we injected).</span></span> <span data-ttu-id="bb958-141">たとえば、既定の HTML ヘルパーを自分のヘルパーに置き換える場合は、`@inject` を使用して簡単に置き換えることができます。</span><span class="sxs-lookup"><span data-stu-id="bb958-141">If for instance you wanted to replace the default HTML Helpers with your own, you could easily do so using `@inject`:</span></span>

[!code-cshtml[](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]

<span data-ttu-id="bb958-142">既存のサービスを拡張する場合、独自に既存の実装から継承したり、ラップしたりするときに、この技術を使用できます。</span><span class="sxs-lookup"><span data-stu-id="bb958-142">If you want to extend existing services, you can simply use this technique while inheriting from or wrapping the existing implementation with your own.</span></span>

## <a name="see-also"></a><span data-ttu-id="bb958-143">参照</span><span class="sxs-lookup"><span data-stu-id="bb958-143">See Also</span></span>

* <span data-ttu-id="bb958-144">Simon Timms のブログ: [ルックアップ データをビューに取り込む](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)</span><span class="sxs-lookup"><span data-stu-id="bb958-144">Simon Timms Blog: [Getting Lookup Data Into Your View](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)</span></span>
