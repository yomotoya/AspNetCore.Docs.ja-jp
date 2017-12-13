---
title: "ASP.NET Core MVC の概要"
author: ardalis
description: "ASP.NET Core MVC web アプリケーションを構築するための豊富なフレームワークおよび方法モデル ビュー コント ローラーを使用して Api デザイン パターンについて説明します。"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 89af38d1-52e0-4db7-b791-dbce909b0714
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/overview
ms.openlocfilehash: 2492b6aa4602dbbf3b9cd3dca00d40690c640cab
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="overview-of-aspnet-core-mvc"></a><span data-ttu-id="a6959-104">ASP.NET Core MVC の概要</span><span class="sxs-lookup"><span data-stu-id="a6959-104">Overview of ASP.NET Core MVC</span></span>

<span data-ttu-id="a6959-105">によって[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="a6959-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="a6959-106">ASP.NET Core MVC は、web アプリを構築するための豊富なフレームワークと、モデル ビュー コント ローラーを使用して Api デザイン パターンです。</span><span class="sxs-lookup"><span data-stu-id="a6959-106">ASP.NET Core MVC is a rich framework for building web apps and APIs using the Model-View-Controller design pattern.</span></span>

## <a name="what-is-the-mvc-pattern"></a><span data-ttu-id="a6959-107">MVC パターンとは何ですか。</span><span class="sxs-lookup"><span data-stu-id="a6959-107">What is the MVC pattern?</span></span>

<span data-ttu-id="a6959-108">モデル ビュー コント ローラー (MVC) アーキテクチャ パターンでは、アプリケーション コンポーネントの次の 3 つの主要グループに: モデル、ビュー、およびコント ローラー。</span><span class="sxs-lookup"><span data-stu-id="a6959-108">The Model-View-Controller (MVC) architectural pattern separates an application into three main groups of components: Models, Views, and Controllers.</span></span> <span data-ttu-id="a6959-109">このパターンを実現するために役立つ[関心の分離](http://deviq.com/separation-of-concerns/)です。</span><span class="sxs-lookup"><span data-stu-id="a6959-109">This pattern helps to achieve [separation of concerns](http://deviq.com/separation-of-concerns/).</span></span> <span data-ttu-id="a6959-110">このパターンを使用して、ユーザーの要求は、担当するユーザーの操作の実行やクエリの結果を取得するモデルはコント ローラーにルーティングされます。</span><span class="sxs-lookup"><span data-stu-id="a6959-110">Using this pattern, user requests are routed to a Controller which is responsible for working with the Model to perform user actions and/or retrieve results of queries.</span></span> <span data-ttu-id="a6959-111">コント ローラーは、ユーザーに表示するビューを選択し、必要なすべてのモデル データを提供します。</span><span class="sxs-lookup"><span data-stu-id="a6959-111">The Controller chooses the View to display to the user, and provides it with any Model data it requires.</span></span>

<span data-ttu-id="a6959-112">次の図は、次の 3 つの主要なコンポーネントと、どれが、他のユーザーを参照します。</span><span class="sxs-lookup"><span data-stu-id="a6959-112">The following diagram shows the three main components and which ones reference the others:</span></span>

![MVC パターン](overview/_static/mvc.png)

<span data-ttu-id="a6959-114">この概念の責任のでは、コード、デバッグ、およびテスト (モデル、ビュー、またはコント ローラー) が持っているもの 1 つのジョブを簡単になっているために、複雑さの観点からアプリケーションを拡張できます (およびに従って、[単一責任の原則。](http://deviq.com/single-responsibility-principle/)).</span><span class="sxs-lookup"><span data-stu-id="a6959-114">This delineation of responsibilities helps you scale the application in terms of complexity because it’s easier to code, debug, and test something (model, view, or controller) that has a single job (and follows the [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/)).</span></span> <span data-ttu-id="a6959-115">更新プログラム、テスト、および分散これら 3 つの領域の 2 つ以上の依存関係のあるコードをデバッグすることが難しくなります。</span><span class="sxs-lookup"><span data-stu-id="a6959-115">It's more difficult to update, test, and debug code that has dependencies spread across two or more of these three areas.</span></span> <span data-ttu-id="a6959-116">たとえば、ユーザー インターフェイス ロジックは、ビジネス ロジックよりも頻繁に変更する傾向があります。</span><span class="sxs-lookup"><span data-stu-id="a6959-116">For example, user interface logic tends to change more frequently than business logic.</span></span> <span data-ttu-id="a6959-117">プレゼンテーション コードとビジネス ロジックを 1 つのオブジェクトに結合し場合、は、ユーザー インターフェイスを変更するたびに、ビジネス ロジックを格納するオブジェクトの変更が必要です。</span><span class="sxs-lookup"><span data-stu-id="a6959-117">If presentation code and business logic are combined in a single object, you have to modify an object containing business logic every time you change the user interface.</span></span> <span data-ttu-id="a6959-118">これは、エラーが発生して、すべてのビジネス ロジックの再テストごとの最小ユーザー インターフェイスを変更した後を必要とする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a6959-118">This is likely to introduce errors and require the retesting of all business logic after every minimal user interface change.</span></span>

> [!NOTE]
> <span data-ttu-id="a6959-119">ビューとコント ローラーの両方は、モデルによって異なります。</span><span class="sxs-lookup"><span data-stu-id="a6959-119">Both the view and the controller depend on the model.</span></span> <span data-ttu-id="a6959-120">ただし、モデルは、ビューでも、コント ローラーに依存します。</span><span class="sxs-lookup"><span data-stu-id="a6959-120">However, the model depends on neither the view nor the controller.</span></span> <span data-ttu-id="a6959-121">これは、分離の主要な利点の 1 つです。</span><span class="sxs-lookup"><span data-stu-id="a6959-121">This is one of the key benefits of the separation.</span></span> <span data-ttu-id="a6959-122">この分離により、モデルをビルドおよびテストできる、ビジュアル化に依存しません。</span><span class="sxs-lookup"><span data-stu-id="a6959-122">This separation allows the model to be built and tested independent of the visual presentation.</span></span>

### <a name="model-responsibilities"></a><span data-ttu-id="a6959-123">モデルの責任</span><span class="sxs-lookup"><span data-stu-id="a6959-123">Model Responsibilities</span></span>

<span data-ttu-id="a6959-124">MVC アプリケーション内のモデルでは、アプリケーションやビジネス ロジックによって実行される操作の状態を表します。</span><span class="sxs-lookup"><span data-stu-id="a6959-124">The Model in an MVC application represents the state of the application and any business logic or operations that should be performed by it.</span></span> <span data-ttu-id="a6959-125">モデルでは、アプリケーションの状態を保存するための実装ロジックとビジネス ロジックをカプセル化する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a6959-125">Business logic should be encapsulated in the model, along with any implementation logic for persisting the state of the application.</span></span> <span data-ttu-id="a6959-126">厳密に型指定されたビューはされる通常具体的には、データを含むように設計 ViewModel 型を使用してそのビューに表示するにはコント ローラーが作成され、モデルからこれらの ViewModel インスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="a6959-126">Strongly-typed views will typically use ViewModel types specifically designed to contain the data to display on that view; the controller will create and populate these ViewModel instances from the model.</span></span>

> [!NOTE]
> <span data-ttu-id="a6959-127">MVC アーキテクチャ パターンを使用するアプリでモデルを整理する方法があります。</span><span class="sxs-lookup"><span data-stu-id="a6959-127">There are many ways to organize the model in an app that uses the MVC architectural pattern.</span></span> <span data-ttu-id="a6959-128">詳細については、いくつか[さまざまな種類の種類のモデルの](http://deviq.com/kinds-of-models/)します。</span><span class="sxs-lookup"><span data-stu-id="a6959-128">Learn more about some [different kinds of model types](http://deviq.com/kinds-of-models/).</span></span>

### <a name="view-responsibilities"></a><span data-ttu-id="a6959-129">ビューの責任</span><span class="sxs-lookup"><span data-stu-id="a6959-129">View Responsibilities</span></span>

<span data-ttu-id="a6959-130">ビューは、ユーザー インターフェイスからコンテンツの表示を担当します。</span><span class="sxs-lookup"><span data-stu-id="a6959-130">Views are responsible for presenting content through the user interface.</span></span> <span data-ttu-id="a6959-131">使用する、 [Razor ビュー エンジン](#razor-view-engine)HTML マークアップに .NET コードを埋め込む。</span><span class="sxs-lookup"><span data-stu-id="a6959-131">They use the [Razor view engine](#razor-view-engine) to embed .NET code in HTML markup.</span></span> <span data-ttu-id="a6959-132">ビューの中で最低限のロジックが存在する必要があり、それらで任意のロジックは、コンテンツを表示することに関連する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a6959-132">There should be minimal logic within views, and any logic in them should relate to presenting content.</span></span> <span data-ttu-id="a6959-133">複雑なモデルからデータを表示するためにファイルのビューに大量のロジックを実行する必要を検索する場合は、使用を検討して、[ビュー コンポーネント](views/view-components.md)ViewModel、または表示を簡略化のテンプレートの表示。</span><span class="sxs-lookup"><span data-stu-id="a6959-133">If you find the need to perform a great deal of logic in view files in order to display data from a complex model, consider using a [View Component](views/view-components.md), ViewModel, or view template to simplify the view.</span></span>

### <a name="controller-responsibilities"></a><span data-ttu-id="a6959-134">コント ローラーの機能</span><span class="sxs-lookup"><span data-stu-id="a6959-134">Controller Responsibilities</span></span>

<span data-ttu-id="a6959-135">コント ローラーは、ユーザーとの対話を処理し、モデルでは、作業、最終的にレンダリングするビューを選択するコンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="a6959-135">Controllers are the components that handle user interaction, work with the model, and ultimately select a view to render.</span></span> <span data-ttu-id="a6959-136">MVC アプリケーションでは、ビューのみ情報が表示されます。コント ローラーは、処理し、ユーザー入力との対話に応答します。</span><span class="sxs-lookup"><span data-stu-id="a6959-136">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span> <span data-ttu-id="a6959-137">MVC パターンでは、コント ローラー、初期のエントリ ポイントは、どのモデルの種類を使用して表示するには、どのビューの選択を担当する (そのため、名前の制御、アプリが特定の要求に応答する方法)。</span><span class="sxs-lookup"><span data-stu-id="a6959-137">In the MVC pattern, the controller is the initial entry point, and is responsible for selecting which model types to work with and which view to render (hence its name - it controls how the app responds to a given request).</span></span>

> [!NOTE]
> <span data-ttu-id="a6959-138">多数の責任でコント ローラーが過度に複雑ないない必要があります。</span><span class="sxs-lookup"><span data-stu-id="a6959-138">Controllers should not be overly complicated by too many responsibilities.</span></span> <span data-ttu-id="a6959-139">コント ローラーのロジックが過度に複雑になることを防止するには、[単一責任の原則](http://deviq.com/single-responsibility-principle/)プッシュ ビジネス ロジック、コント ローラー アウトし、ドメイン モデルにします。</span><span class="sxs-lookup"><span data-stu-id="a6959-139">To keep controller logic from becoming overly complex, use the [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/) to push business logic out of the controller and into the domain model.</span></span>

>[!TIP]
> <span data-ttu-id="a6959-140">コント ローラーのアクションは、同じ種類のアクションを頻繁に実行する場合は、行うことができる、[繰り返さない自分で原則](http://deviq.com/don-t-repeat-yourself/)にこれらの一般的なアクションを移動することによって[フィルター](#filters)です。</span><span class="sxs-lookup"><span data-stu-id="a6959-140">If you find that your controller actions frequently perform the same kinds of actions, you can follow the [Don't Repeat Yourself principle](http://deviq.com/don-t-repeat-yourself/) by moving these common actions into [filters](#filters).</span></span>

## <a name="what-is-aspnet-core-mvc"></a><span data-ttu-id="a6959-141">ASP.NET Core MVC は、新機能</span><span class="sxs-lookup"><span data-stu-id="a6959-141">What is ASP.NET Core MVC</span></span>

<span data-ttu-id="a6959-142">ASP.NET Core MVC フレームワークは、ライトウェイト、オープン ソース、ASP.NET Core で使用するために最適化された高テスト可能なプレゼンテーション フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="a6959-142">The ASP.NET Core MVC framework is a lightweight, open source, highly testable presentation framework optimized for use with ASP.NET Core.</span></span>

<span data-ttu-id="a6959-143">ASP.NET Core MVC は、問題を明確に分離できるようにする動的な web サイトを構築するパターンに基づく方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="a6959-143">ASP.NET Core MVC provides a patterns-based way to build dynamic websites that enables a clean separation of concerns.</span></span> <span data-ttu-id="a6959-144">マークアップを完全に制御を規格をサポートしては最新の web 標準を使用します。</span><span class="sxs-lookup"><span data-stu-id="a6959-144">It gives you full control over markup, supports TDD-friendly development and uses the latest web standards.</span></span>

## <a name="features"></a><span data-ttu-id="a6959-145">フィーチャー</span><span class="sxs-lookup"><span data-stu-id="a6959-145">Features</span></span>

<span data-ttu-id="a6959-146">ASP.NET Core MVC、次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="a6959-146">ASP.NET Core MVC includes the following:</span></span>

* [<span data-ttu-id="a6959-147">ルーティング</span><span class="sxs-lookup"><span data-stu-id="a6959-147">Routing</span></span>](#routing)
* [<span data-ttu-id="a6959-148">モデル バインディング</span><span class="sxs-lookup"><span data-stu-id="a6959-148">Model binding</span></span>](#model-binding)
* [<span data-ttu-id="a6959-149">モデル検証</span><span class="sxs-lookup"><span data-stu-id="a6959-149">Model validation</span></span>](#model-validation)
* [<span data-ttu-id="a6959-150">依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="a6959-150">Dependency injection</span></span>](../fundamentals/dependency-injection.md)
* [<span data-ttu-id="a6959-151">フィルター</span><span class="sxs-lookup"><span data-stu-id="a6959-151">Filters</span></span>](#filters)
* [<span data-ttu-id="a6959-152">領域</span><span class="sxs-lookup"><span data-stu-id="a6959-152">Areas</span></span>](#areas)
* [<span data-ttu-id="a6959-153">Web Api</span><span class="sxs-lookup"><span data-stu-id="a6959-153">Web APIs</span></span>](#web-apis)
* [<span data-ttu-id="a6959-154">テストの容易性</span><span class="sxs-lookup"><span data-stu-id="a6959-154">Testability</span></span>](#testability)
* [<span data-ttu-id="a6959-155">Razor ビュー エンジン</span><span class="sxs-lookup"><span data-stu-id="a6959-155">Razor view engine</span></span>](#razor-view-engine)
* [<span data-ttu-id="a6959-156">厳密に型指定されたビュー</span><span class="sxs-lookup"><span data-stu-id="a6959-156">Strongly typed views</span></span>](#strongly-typed-views)
* [<span data-ttu-id="a6959-157">タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="a6959-157">Tag Helpers</span></span>](#tag-helpers)
* [<span data-ttu-id="a6959-158">コンポーネントの表示</span><span class="sxs-lookup"><span data-stu-id="a6959-158">View Components</span></span>](#view-components)

### <a name="routing"></a><span data-ttu-id="a6959-159">ルーティング</span><span class="sxs-lookup"><span data-stu-id="a6959-159">Routing</span></span>

<span data-ttu-id="a6959-160">ASP.NET Core MVC がの上に構築された[ASP.NET Core のルーティング](../fundamentals/routing.md)、できる強力な URL マッピング コンポーネントと、わかりやすく検索可能な Url を持つアプリケーションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="a6959-160">ASP.NET Core MVC is built on top of [ASP.NET Core's routing](../fundamentals/routing.md), a powerful URL-mapping component that lets you build applications that have comprehensible and searchable URLs.</span></span> <span data-ttu-id="a6959-161">これにより、web サーバー上のファイルの整理方法に関係なく、リンクの生成と検索エンジン最適化 (SEO) にとっても、アプリケーションの URL の名前付けパターンを定義することができます。</span><span class="sxs-lookup"><span data-stu-id="a6959-161">This enables you to define your application's URL naming patterns that work well for search engine optimization (SEO) and for link generation, without regard for how the files on your web server are organized.</span></span> <span data-ttu-id="a6959-162">ルート値の制約、既定値および省略可能な値をサポートする便利なルート テンプレートの構文を使用して、ルートを定義することができます。</span><span class="sxs-lookup"><span data-stu-id="a6959-162">You can define your routes using a convenient route template syntax that supports route value constraints, defaults and optional values.</span></span>

<span data-ttu-id="a6959-163">*ルーティング規約に基づく*形式の URL をグローバルに定義することにより、アプリケーションを受け入れるし、特定のコント ローラー上の特定のアクション メソッドにマップそれらのすべてがどのように書式設定します。</span><span class="sxs-lookup"><span data-stu-id="a6959-163">*Convention-based routing* enables you to globally define the URL formats that your application accepts and how each of those formats maps to a specific action method on given controller.</span></span> <span data-ttu-id="a6959-164">着信要求を受信すると、ルーティング エンジンは、URL を解析および定義済みの URL の形式の 1 つに一致する、関連付けられているコント ローラーのアクション メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="a6959-164">When an incoming request is received, the routing engine parses the URL and matches it to one of the defined URL formats, and then calls the associated controller's action method.</span></span>

```csharp
routes.MapRoute(name: "Default", template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="a6959-165">*属性のルーティング*コント ローラーとアクションは、アプリケーションのルートを定義する属性で修飾することによって、ルーティング情報を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="a6959-165">*Attribute routing* enables you to specify routing information by decorating your controllers and actions with attributes that define your application's routes.</span></span> <span data-ttu-id="a6959-166">これは、コント ローラーと関連付けられているアクションの横にある、route 定義を配置していることを意味します。</span><span class="sxs-lookup"><span data-stu-id="a6959-166">This means that your route definitions are placed next to the controller and action with which they're associated.</span></span>

```csharp
[Route("api/[controller]")]
public class ProductsController : Controller
{
  [HttpGet("{id}")]
  public IActionResult GetProduct(int id)
  {
    ...
  }
}
```

### <a name="model-binding"></a><span data-ttu-id="a6959-167">モデル バインディング</span><span class="sxs-lookup"><span data-stu-id="a6959-167">Model binding</span></span>

<span data-ttu-id="a6959-168">ASP.NET Core MVC[モデル バインディング](models/model-binding.md)クライアント要求データ (フォームの値、ルート データ、クエリ文字列パラメーター、HTTP ヘッダー)、コント ローラーを処理できるオブジェクトを変換します。</span><span class="sxs-lookup"><span data-stu-id="a6959-168">ASP.NET Core MVC [model binding](models/model-binding.md) converts client request data  (form values, route data, query string parameters, HTTP headers) into objects that the controller can handle.</span></span> <span data-ttu-id="a6959-169">その結果、コント ローラーのロジックを受信した要求データを見つけ出しの作業を行うにはありません。単に、そのアクション メソッドにパラメーターとデータを持ちます。</span><span class="sxs-lookup"><span data-stu-id="a6959-169">As a result, your controller logic doesn't have to do the work of figuring out the incoming request data; it simply has the data as parameters to its action methods.</span></span>

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null) { ... }
   ```

### <a name="model-validation"></a><span data-ttu-id="a6959-170">モデルの検証</span><span class="sxs-lookup"><span data-stu-id="a6959-170">Model validation</span></span>

<span data-ttu-id="a6959-171">ASP.NET Core MVC をサポートしている[検証](models/validation.md)データ注釈検証属性を持つモデル オブジェクトを修飾します。</span><span class="sxs-lookup"><span data-stu-id="a6959-171">ASP.NET Core MVC supports [validation](models/validation.md) by decorating your model object with data annotation validation attributes.</span></span> <span data-ttu-id="a6959-172">検証属性の値は、サーバーに送信される前に、クライアント側で確認されますだけでなくコント ローラー アクションの前に、サーバー上と呼びます。</span><span class="sxs-lookup"><span data-stu-id="a6959-172">The validation attributes are checked on the client side before values are posted to the server, as well as on the server before the controller action is called.</span></span>

```csharp
using System.ComponentModel.DataAnnotations;
public class LoginViewModel
{
    [Required]
    [EmailAddress]
    public string Email { get; set; }

    [Required]
    [DataType(DataType.Password)]
    public string Password { get; set; }

    [Display(Name = "Remember me?")]
    public bool RememberMe { get; set; }
}
```

<span data-ttu-id="a6959-173">コント ローラーのアクション:</span><span class="sxs-lookup"><span data-stu-id="a6959-173">A controller action:</span></span>

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null)
{
    if (ModelState.IsValid)
    {
      // work with the model
    }
    // If we got this far, something failed, redisplay form
    return View(model);
}
```

<span data-ttu-id="a6959-174">フレームワークでは、クライアントとサーバーの両方の要求データの検証を処理します。</span><span class="sxs-lookup"><span data-stu-id="a6959-174">The framework will handle validating request data both on the client and on the server.</span></span> <span data-ttu-id="a6959-175">モデルの種類に対して指定された検証ロジックは控えめな注釈としてレンダリングされるビューに追加され、と共にブラウザーに適用される[jQuery 検証](https://jqueryvalidation.org/)です。</span><span class="sxs-lookup"><span data-stu-id="a6959-175">Validation logic specified on model types is added to the rendered views as unobtrusive annotations and is enforced in the browser with [jQuery Validation](https://jqueryvalidation.org/).</span></span>

### <a name="dependency-injection"></a><span data-ttu-id="a6959-176">依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="a6959-176">Dependency injection</span></span>

<span data-ttu-id="a6959-177">ASP.NET Core はの組み込みサポート[依存性の注入 (DI)](../fundamentals/dependency-injection.md)です。</span><span class="sxs-lookup"><span data-stu-id="a6959-177">ASP.NET Core has built-in support for [dependency injection (DI)](../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="a6959-178">MVC では ASP.NET Core、[コント ローラー](controllers/dependency-injection.md)に従うように、コンス トラクターからサービスを必要な要求、[明示的な依存関係の原則](http://deviq.com/explicit-dependencies-principle/)です。</span><span class="sxs-lookup"><span data-stu-id="a6959-178">In ASP.NET Core MVC, [controllers](controllers/dependency-injection.md) can request needed services through their constructors, allowing them to follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="a6959-179">アプリケーションで使用できるも[ファイル ビュー内の依存性の注入](views/dependency-injection.md)を使用して、`@inject`ディレクティブ。</span><span class="sxs-lookup"><span data-stu-id="a6959-179">Your app can also use [dependency injection in view files](views/dependency-injection.md), using the `@inject` directive:</span></span>

```cshtml
@inject SomeService ServiceName
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ServiceName.GetTitle</title>
</head>
<body>
    <h1>@ServiceName.GetTitle</h1>
</body>
</html>
```

### <a name="filters"></a><span data-ttu-id="a6959-180">フィルター</span><span class="sxs-lookup"><span data-stu-id="a6959-180">Filters</span></span>

<span data-ttu-id="a6959-181">[フィルター](controllers/filters.md)例外処理や承認などの横断的関心事をカプセル化する開発者を支援します。</span><span class="sxs-lookup"><span data-stu-id="a6959-181">[Filters](controllers/filters.md) help developers encapsulate cross-cutting concerns, like exception handling or authorization.</span></span> <span data-ttu-id="a6959-182">フィルターは、アクション メソッドの実行中のカスタムの前処理および後処理ロジックを有効にして、特定の要求の実行パイプライン内の特定の時点で実行するように構成できます。</span><span class="sxs-lookup"><span data-stu-id="a6959-182">Filters enable running custom pre- and post-processing logic for action methods, and can be configured to run at certain points within the execution pipeline for a given request.</span></span> <span data-ttu-id="a6959-183">フィルターは属性としてアクションまたはコント ローラーに適用できます (または、グローバルに実行することができます)。</span><span class="sxs-lookup"><span data-stu-id="a6959-183">Filters can be applied to controllers or actions as attributes (or can be run globally).</span></span> <span data-ttu-id="a6959-184">いくつかのフィルター (など`Authorize`) フレームワークに含まれています。</span><span class="sxs-lookup"><span data-stu-id="a6959-184">Several filters (such as `Authorize`) are included in the framework.</span></span>


```csharp
[Authorize]
   public class AccountController : Controller
   {
```

### <a name="areas"></a><span data-ttu-id="a6959-185">区分</span><span class="sxs-lookup"><span data-stu-id="a6959-185">Areas</span></span>

<span data-ttu-id="a6959-186">[領域](controllers/areas.md)大規模な ASP.NET Core MVC Web アプリケーションを小さい機能グループに分割する方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="a6959-186">[Areas](controllers/areas.md) provide a way to partition a large ASP.NET Core MVC Web app into smaller functional groupings.</span></span> <span data-ttu-id="a6959-187">領域は、事実上、アプリケーション内部 MVC 構造です。</span><span class="sxs-lookup"><span data-stu-id="a6959-187">An area is effectively an MVC structure inside an application.</span></span> <span data-ttu-id="a6959-188">MVC プロジェクトでは、モデル、コント ローラー、およびビューなどの論理コンポーネントが、他のフォルダーに保持され、MVC では、名前付け規則を使用して、これらのコンポーネント間のリレーションシップを作成します。</span><span class="sxs-lookup"><span data-stu-id="a6959-188">In an MVC project, logical components like Model, Controller, and View are kept in different folders, and MVC uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="a6959-189">大規模なアプリの機能の個別の高レベル領域に、アプリを分割すると便利な場合があります。</span><span class="sxs-lookup"><span data-stu-id="a6959-189">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="a6959-190">たとえば、電子商取引を使用したアプリなど、チェック アウト、請求、および検索などの複数の事業単位です。各事業ユニットは、独自の論理コンポーネント ビュー、コント ローラー、およびモデルがあります。</span><span class="sxs-lookup"><span data-stu-id="a6959-190">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search etc. Each of these units have their own logical component views, controllers, and models.</span></span>

### <a name="web-apis"></a><span data-ttu-id="a6959-191">Web API</span><span class="sxs-lookup"><span data-stu-id="a6959-191">Web APIs</span></span>

<span data-ttu-id="a6959-192">Web サイトを構築するための優れたプラットフォームだけでなくは、ASP.NET Core MVC は、Web Api を構築するための優れたサポートがします。</span><span class="sxs-lookup"><span data-stu-id="a6959-192">In addition to being a great platform for building web sites, ASP.NET Core MVC has great support for building Web APIs.</span></span> <span data-ttu-id="a6959-193">さまざまなブラウザーやモバイル デバイスを含む、クライアントに到達可能なサービスをビルドすることができます。</span><span class="sxs-lookup"><span data-stu-id="a6959-193">You can build services that can reach a broad range of clients including browsers and mobile devices.</span></span>

<span data-ttu-id="a6959-194">フレームワークの組み込みサポートを備えた HTTP コンテンツ ネゴシエーションをサポートしています[データの書式設定](models/formatting.md)JSON または XML として。</span><span class="sxs-lookup"><span data-stu-id="a6959-194">The framework includes support for HTTP content-negotiation with built-in support for [formatting data](models/formatting.md) as JSON or XML.</span></span> <span data-ttu-id="a6959-195">書き込む[カスタム フォーマッタ](advanced/custom-formatters.md)ユーザー独自の形式のサポートを追加します。</span><span class="sxs-lookup"><span data-stu-id="a6959-195">Write [custom formatters](advanced/custom-formatters.md) to add support for your own formats.</span></span>

<span data-ttu-id="a6959-196">Hypermedia のサポートを有効にするのにには、リンクの生成を使用します。</span><span class="sxs-lookup"><span data-stu-id="a6959-196">Use link generation to enable support for hypermedia.</span></span> <span data-ttu-id="a6959-197">サポートを容易にできる[クロス オリジン リソース共有 (CORS)](http://www.w3.org/TR/cors/)に Web Api は、複数の Web アプリケーションで共有できるようにします。</span><span class="sxs-lookup"><span data-stu-id="a6959-197">Easily enable support for [cross-origin resource sharing (CORS)](http://www.w3.org/TR/cors/) so that your Web APIs can be shared across multiple Web applications.</span></span>

### <a name="testability"></a><span data-ttu-id="a6959-198">テストの容易性</span><span class="sxs-lookup"><span data-stu-id="a6959-198">Testability</span></span>

<span data-ttu-id="a6959-199">インターフェイスと依存関係の挿入のフレームワークの使用に適してを単体テスト、行い、フレームワークには、構成する (Entity Framework の考えます、InMemory プロバイダー) のような機能が含まれています[統合テスト](../testing/integration-testing.md)クイック。簡単にもします。</span><span class="sxs-lookup"><span data-stu-id="a6959-199">The framework's use of interfaces and dependency injection make it well-suited to unit testing, and the framework includes features (like a TestHost and InMemory provider for Entity Framework) that make [integration testing](../testing/integration-testing.md) quick and easy as well.</span></span> <span data-ttu-id="a6959-200">詳細については[テスト コント ローラー ロジック](controllers/testing.md)です。</span><span class="sxs-lookup"><span data-stu-id="a6959-200">Learn more about [testing controller logic](controllers/testing.md).</span></span>

### <a name="razor-view-engine"></a><span data-ttu-id="a6959-201">Razor ビュー エンジン</span><span class="sxs-lookup"><span data-stu-id="a6959-201">Razor view engine</span></span>

<span data-ttu-id="a6959-202">[ASP.NET Core MVC ビュー](views/overview.md)を使用して、 [Razor ビュー エンジン](views/razor.md)ビューをレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="a6959-202">[ASP.NET Core MVC views](views/overview.md) use the [Razor view engine](views/razor.md) to render views.</span></span> <span data-ttu-id="a6959-203">Razor は、埋め込みの c# コードを使用して、ビューを定義するためのコンパクトな表現力が豊かおよび滑らかなテンプレート マークアップ言語です。</span><span class="sxs-lookup"><span data-stu-id="a6959-203">Razor is a compact, expressive and fluid template markup language for defining views using embedded C# code.</span></span> <span data-ttu-id="a6959-204">Razor を使用して、サーバー上の web コンテンツを動的に生成します。</span><span class="sxs-lookup"><span data-stu-id="a6959-204">Razor is used to dynamically generate web content on the server.</span></span> <span data-ttu-id="a6959-205">サーバー コード クライアント側のコンテンツおよびコードを混在しているクリーンにことができます。</span><span class="sxs-lookup"><span data-stu-id="a6959-205">You can cleanly mix server code with client side content and code.</span></span>

```text
<ul>
  @for (int i = 0; i < 5; i++) {
    <li>List item @i</li>
  }
</ul>
```

<span data-ttu-id="a6959-206">定義することができます、Razor ビュー エンジンを使用して[レイアウト](views/layout.md)、[部分ビュー](views/partial.md)と置き換え可能なセクションです。</span><span class="sxs-lookup"><span data-stu-id="a6959-206">Using the Razor view engine you can define [layouts](views/layout.md), [partial views](views/partial.md) and replaceable sections.</span></span>

### <a name="strongly-typed-views"></a><span data-ttu-id="a6959-207">厳密に型指定されたビュー</span><span class="sxs-lookup"><span data-stu-id="a6959-207">Strongly typed views</span></span>

<span data-ttu-id="a6959-208">MVC の razor ビューできる厳密に型指定、モデルに基づく。</span><span class="sxs-lookup"><span data-stu-id="a6959-208">Razor views in MVC can be strongly typed based on your model.</span></span> <span data-ttu-id="a6959-209">コント ローラーは、型チェックと IntelliSense をサポートして、ビューを有効にするビューを厳密に型指定されたモデルを渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="a6959-209">Controllers can pass a strongly typed model to views enabling your views to have type checking and IntelliSense support.</span></span>

<span data-ttu-id="a6959-210">たとえば、次のビューが型のモデルを定義`IEnumerable<Product>`:</span><span class="sxs-lookup"><span data-stu-id="a6959-210">For example, the following view defines a model of type `IEnumerable<Product>`:</span></span>

```cshtml
@model IEnumerable<Product>
<ul>
    @foreach (Product p in Model)
    {
        <li>@p.Name</li>
    }
</ul>
```

### <a name="tag-helpers"></a><span data-ttu-id="a6959-211">タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="a6959-211">Tag Helpers</span></span>

<span data-ttu-id="a6959-212">[タグ ヘルパー](views/tag-helpers/intro.md)を作成して、Razor ファイルでの HTML 要素のレンダリングに参加させるサーバー側コードを有効にします。</span><span class="sxs-lookup"><span data-stu-id="a6959-212">[Tag Helpers](views/tag-helpers/intro.md) enable server side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="a6959-213">カスタム タグを定義するタグ ヘルパーを使用することができます (たとえば、 `<environment>`) または既存のタグの動作を変更する (たとえば、 `<label>`)。</span><span class="sxs-lookup"><span data-stu-id="a6959-213">You can use tag helpers to define custom tags (for example, `<environment>`) or to modify the behavior of existing tags (for example, `<label>`).</span></span> <span data-ttu-id="a6959-214">タグ ヘルパーは、要素名とその属性に基づいて特定の要素にバインドします。</span><span class="sxs-lookup"><span data-stu-id="a6959-214">Tag Helpers bind to specific elements based on the element name and its attributes.</span></span> <span data-ttu-id="a6959-215">HTML 編集エクスペリエンスを維持したまま、サーバー側のレンダリングの利点を提供します。</span><span class="sxs-lookup"><span data-stu-id="a6959-215">They provide the benefits of server-side rendering while still preserving an HTML editing experience.</span></span>

<span data-ttu-id="a6959-216">これは、フォーム、リンク、読み込みの資産および詳細 - およびさらに高いで利用できるパブリックの GitHub リポジトリとして NuGet パッケージの作成などの一般的なタスクの多くの組み込みタグ ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="a6959-216">There are many built-in Tag Helpers for common tasks - such as creating forms, links, loading assets and more - and even more available in public GitHub repositories and as NuGet packages.</span></span> <span data-ttu-id="a6959-217">タグ ヘルパーは、C# の場合は、作成し、要素名、属性名、または親タグに基づく HTML 要素を対象にします。</span><span class="sxs-lookup"><span data-stu-id="a6959-217">Tag Helpers are authored in C#, and they target HTML elements based on element name, attribute name, or parent tag.</span></span> <span data-ttu-id="a6959-218">たとえば、あらかじめ登録された LinkTagHelper をへのリンクの作成に使用することができます、`Login`のアクション、 `AccountsController`:</span><span class="sxs-lookup"><span data-stu-id="a6959-218">For example, the built-in LinkTagHelper can be used to create a link to the `Login` action of the `AccountsController`:</span></span>

```cshtml
<p>
    Thank you for confirming your email.
    Please <a asp-controller="Account" asp-action="Login">Click here to Log in</a>.
</p>
```

<span data-ttu-id="a6959-219">`EnvironmentTagHelper`開発、ステージング、運用環境など、ランタイム環境に基づきます (たとえば、生または縮小された) ビュー内の別のスクリプトを含めるために使用できます。</span><span class="sxs-lookup"><span data-stu-id="a6959-219">The `EnvironmentTagHelper` can be used to include different scripts in your views (for example, raw or minified) based on the runtime environment, such as Development, Staging, or Production:</span></span>

```cshtml
<environment names="Development">
    <script src="~/lib/jquery/dist/jquery.js"></script>
</environment>
<environment names="Staging,Production">
    <script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.1.4.min.js"
            asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
            asp-fallback-test="window.jQuery">
    </script>
</environment>
```

<span data-ttu-id="a6959-220">タグ ヘルパーは、HTML に適した開発エクスペリエンスと HTML および Razor マークアップを作成するための豊富な IntelliSense 環境を提供します。</span><span class="sxs-lookup"><span data-stu-id="a6959-220">Tag Helpers provide an HTML-friendly development experience and a rich IntelliSense environment for creating HTML and Razor markup.</span></span> <span data-ttu-id="a6959-221">組み込みタグ ヘルパーのほとんどは、既存の HTML 要素をターゲットし、要素のサーバー側の属性を指定します。</span><span class="sxs-lookup"><span data-stu-id="a6959-221">Most of the built-in Tag Helpers target existing HTML elements and provide server-side attributes for the element.</span></span>

### <a name="view-components"></a><span data-ttu-id="a6959-222">コンポーネントの表示</span><span class="sxs-lookup"><span data-stu-id="a6959-222">View Components</span></span>

<span data-ttu-id="a6959-223">[コンポーネントの表示](views/view-components.md)レンダリング ロジックをパッケージ化し、アプリケーション全体で再利用することを許可します。</span><span class="sxs-lookup"><span data-stu-id="a6959-223">[View Components](views/view-components.md) allow you to package rendering logic and reuse it throughout the application.</span></span> <span data-ttu-id="a6959-224">似て[部分ビュー](views/partial.md)が関連付けられているロジックを使用します。</span><span class="sxs-lookup"><span data-stu-id="a6959-224">They're similar to [partial views](views/partial.md), but with associated logic.</span></span>
