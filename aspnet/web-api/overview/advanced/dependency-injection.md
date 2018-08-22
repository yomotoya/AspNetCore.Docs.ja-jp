---
uid: web-api/overview/advanced/dependency-injection
title: ASP.NET Web API 2 の依存関係の挿入 |Microsoft Docs
author: MikeWasson
description: このチュートリアルでは、ASP.NET Web API コント ローラーに依存関係を挿入する方法を示します。 ソフトウェアのバージョンがチュートリアルの Web API 2 Unity Application Block で使用しています.
ms.author: riande
ms.date: 01/20/2014
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 41db1af79ed63ff4dd12be37e9cc76e16f1bf5e4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834333"
---
<a name="dependency-injection-in-aspnet-web-api-2"></a><span data-ttu-id="27deb-104">ASP.NET Web API 2 の依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="27deb-104">Dependency Injection in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="27deb-105">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="27deb-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="27deb-106">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="27deb-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> <span data-ttu-id="27deb-107">このチュートリアルでは、ASP.NET Web API コント ローラーに依存関係を挿入する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="27deb-107">This tutorial shows how to inject dependencies into your ASP.NET Web API controller.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="27deb-108">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="27deb-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="27deb-109">Web API 2</span><span class="sxs-lookup"><span data-stu-id="27deb-109">Web API 2</span></span>
> - [<span data-ttu-id="27deb-110">Unity Application Block</span><span class="sxs-lookup"><span data-stu-id="27deb-110">Unity Application Block</span></span>](https://www.nuget.org/packages/Unity/)
> - <span data-ttu-id="27deb-111">Entity Framework 6 の (バージョン 5 の機能も)</span><span class="sxs-lookup"><span data-stu-id="27deb-111">Entity Framework 6 (version 5 also works)</span></span>


## <a name="what-is-dependency-injection"></a><span data-ttu-id="27deb-112">依存関係の挿入とは何ですか。</span><span class="sxs-lookup"><span data-stu-id="27deb-112">What is Dependency Injection?</span></span>

<span data-ttu-id="27deb-113">"*依存関係*" とは、他のオブジェクトが必要とする任意のオブジェクトのことです。</span><span class="sxs-lookup"><span data-stu-id="27deb-113">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="27deb-114">たとえば、定義する一般的なは、[リポジトリ](http://martinfowler.com/eaaCatalog/repository.html)データ アクセスを処理します。</span><span class="sxs-lookup"><span data-stu-id="27deb-114">For example, it's common to define a [repository](http://martinfowler.com/eaaCatalog/repository.html) that handles data access.</span></span> <span data-ttu-id="27deb-115">例を使って説明しましょう。</span><span class="sxs-lookup"><span data-stu-id="27deb-115">Let's illustrate with an example.</span></span> <span data-ttu-id="27deb-116">最初に、ドメイン モデルを定義します。</span><span class="sxs-lookup"><span data-stu-id="27deb-116">First, we'll define a domain model:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="27deb-117">Entity Framework を使用して、データベース内の項目を格納する単純なリポジトリ クラスを次に示します。</span><span class="sxs-lookup"><span data-stu-id="27deb-117">Here is a simple repository class that stores items in a database, using Entity Framework.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="27deb-118">ここでの GET 要求をサポートする Web API コント ローラーを定義してみましょう`Product`エンティティ。</span><span class="sxs-lookup"><span data-stu-id="27deb-118">Now let's define a Web API controller that supports GET requests for `Product` entities.</span></span> <span data-ttu-id="27deb-119">(ままにしてあります POST とわかりやすくするためには、その他の方法です。)最初の試行を次に示します。</span><span class="sxs-lookup"><span data-stu-id="27deb-119">(I'm leaving out POST and other methods for simplicity.) Here is a first attempt:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="27deb-120">コント ローラー クラスが依存している通知`ProductRepository`を知らせ、コント ローラーを作成し、`ProductRepository`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="27deb-120">Notice that the controller class depends on `ProductRepository`, and we are letting the controller create the `ProductRepository` instance.</span></span> <span data-ttu-id="27deb-121">ただし、いくつかの理由にハード コードの依存関係で、この方法をお勧めできませんを勧めします。</span><span class="sxs-lookup"><span data-stu-id="27deb-121">However, it's a bad idea to hard code the dependency in this way, for several reasons.</span></span>

- <span data-ttu-id="27deb-122">置換する場合`ProductRepository`別の実装にもする必要があるコント ローラー クラスを変更します。</span><span class="sxs-lookup"><span data-stu-id="27deb-122">If you want to replace `ProductRepository` with a different implementation, you also need to modify the controller class.</span></span>
- <span data-ttu-id="27deb-123">場合、`ProductRepository`依存関係は、コント ローラー内でこれらを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="27deb-123">If the `ProductRepository` has dependencies, you must configure these inside the controller.</span></span> <span data-ttu-id="27deb-124">複数のコント ローラーで、大規模なプロジェクトの構成コードがプロジェクトに分散します。</span><span class="sxs-lookup"><span data-stu-id="27deb-124">For a large project with multiple controllers, your configuration code becomes scattered across your project.</span></span>
- <span data-ttu-id="27deb-125">コント ローラーが、データベースのクエリにハード コードされたため、単体テストには困難です。</span><span class="sxs-lookup"><span data-stu-id="27deb-125">It is hard to unit test, because the controller is hard-coded to query the database.</span></span> <span data-ttu-id="27deb-126">単体テストでは、現在デザインでは実行できません、モックまたはスタブのリポジトリを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="27deb-126">For a unit test, you should use a mock or stub repository, which is not possible with the currect design.</span></span>

<span data-ttu-id="27deb-127">これらの問題を取り上げます*挿入*コント ローラーにリポジトリ。</span><span class="sxs-lookup"><span data-stu-id="27deb-127">We can address these problems by *injecting* the repository into the controller.</span></span> <span data-ttu-id="27deb-128">最初に、リファクタリング、`ProductRepository`クラス、インターフェイスに。</span><span class="sxs-lookup"><span data-stu-id="27deb-128">First, refactor the `ProductRepository` class into an interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="27deb-129">すると、`IProductRepository`コンス トラクター パラメーターとして。</span><span class="sxs-lookup"><span data-stu-id="27deb-129">Then provide the `IProductRepository` as a constructor parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="27deb-130">この例では[コンス トラクターの挿入](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection)します。</span><span class="sxs-lookup"><span data-stu-id="27deb-130">This example uses [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="27deb-131">使用することも*セッターの挿入*setter メソッドまたはプロパティから依存関係を設定します。</span><span class="sxs-lookup"><span data-stu-id="27deb-131">You can also use *setter injection*, where you set the dependency through a setter method or property.</span></span>

<span data-ttu-id="27deb-132">ただし、アプリケーションは、コント ローラーを直接作成しないため、問題があるようになりました。</span><span class="sxs-lookup"><span data-stu-id="27deb-132">But now there is a problem, because your application doesn't create the controller directly.</span></span> <span data-ttu-id="27deb-133">Web API の要求をルーティングし、は認識しない Web API コント ローラーを作成します`IProductRepository`します。</span><span class="sxs-lookup"><span data-stu-id="27deb-133">Web API creates the controller when it routes the request, and Web API doesn't know anything about `IProductRepository`.</span></span> <span data-ttu-id="27deb-134">これでの Web API の依存関係競合回避モジュールの出番です。</span><span class="sxs-lookup"><span data-stu-id="27deb-134">This is where the Web API dependency resolver comes in.</span></span>

## <a name="the-web-api-dependency-resolver"></a><span data-ttu-id="27deb-135">Web API の依存関係競合回避モジュール</span><span class="sxs-lookup"><span data-stu-id="27deb-135">The Web API Dependency Resolver</span></span>

<span data-ttu-id="27deb-136">Web API の定義、 **IDependencyResolver**依存関係を解決するためのインターフェイス。</span><span class="sxs-lookup"><span data-stu-id="27deb-136">Web API defines the **IDependencyResolver** interface for resolving dependencies.</span></span> <span data-ttu-id="27deb-137">インターフェイスの定義を次に示します。</span><span class="sxs-lookup"><span data-stu-id="27deb-137">Here is the definition of the interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="27deb-138">**IDependencyScope**インターフェイスが 2 つのメソッドには。</span><span class="sxs-lookup"><span data-stu-id="27deb-138">The **IDependencyScope** interface has two methods:</span></span>

- <span data-ttu-id="27deb-139">**GetService**型の 1 つのインスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="27deb-139">**GetService** creates one instance of a type.</span></span>
- <span data-ttu-id="27deb-140">**GetServices**指定した型のオブジェクトのコレクションを作成します。</span><span class="sxs-lookup"><span data-stu-id="27deb-140">**GetServices** creates a collection of objects of a specified type.</span></span>

<span data-ttu-id="27deb-141">**IDependencyResolver**メソッド継承**IDependencyScope**を追加し、 **BeginScope**メソッド。</span><span class="sxs-lookup"><span data-stu-id="27deb-141">The **IDependencyResolver** method inherits **IDependencyScope** and adds the **BeginScope** method.</span></span> <span data-ttu-id="27deb-142">このチュートリアルの後半でスコープについて説明します。</span><span class="sxs-lookup"><span data-stu-id="27deb-142">I'll talk about scopes later in this tutorial.</span></span>

<span data-ttu-id="27deb-143">最初に呼び出す Web API コント ローラー インスタンスを作成するとき**IDependencyResolver.GetService**、コント ローラーの種類に渡します。</span><span class="sxs-lookup"><span data-stu-id="27deb-143">When Web API creates a controller instance, it first calls **IDependencyResolver.GetService**, passing in the controller type.</span></span> <span data-ttu-id="27deb-144">コント ローラーを作成するには、すべての依存関係を解決するのには、この拡張性フックを使用できます。</span><span class="sxs-lookup"><span data-stu-id="27deb-144">You can use this extensibility hook to create the controller, resolving any dependencies.</span></span> <span data-ttu-id="27deb-145">場合**GetService** null を返します、Web API コント ローラー クラスをパラメーターなしのコンス トラクターを検索します。</span><span class="sxs-lookup"><span data-stu-id="27deb-145">If **GetService** returns null, Web API looks for a parameterless constructor on the controller class.</span></span>

## <a name="dependency-resolution-with-the-unity-container"></a><span data-ttu-id="27deb-146">Unity コンテナーと依存関係の解決</span><span class="sxs-lookup"><span data-stu-id="27deb-146">Dependency Resolution with the Unity Container</span></span>

<span data-ttu-id="27deb-147">完全な記述できますが**IDependencyResolver** Web API と既存の IoC コンテナー間のブリッジとして機能する目的は、最初のインターフェイスから実装します。</span><span class="sxs-lookup"><span data-stu-id="27deb-147">Although you could write a complete **IDependencyResolver** implementation from scratch, the interface is really designed to act as bridge between Web API and existing IoC containers.</span></span>

<span data-ttu-id="27deb-148">IoC コンテナーは、依存関係の管理を担当するソフトウェア コンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="27deb-148">An IoC container is a software component that is responsible for managing dependencies.</span></span> <span data-ttu-id="27deb-149">型、コンテナーを登録し、コンテナーを使用して、オブジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="27deb-149">You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="27deb-150">コンテナーは、依存関係を自動的に検出します。</span><span class="sxs-lookup"><span data-stu-id="27deb-150">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="27deb-151">多くの IoC コンテナーでは、オブジェクトの有効期間とスコープなどを制御することもできます。</span><span class="sxs-lookup"><span data-stu-id="27deb-151">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="27deb-152">"IoC"「の制御の反転」の略フレームワークからアプリケーション コードを呼び出す、一般的なパターンは。</span><span class="sxs-lookup"><span data-stu-id="27deb-152">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="27deb-153">IoC コンテナーを構築、オブジェクト、コントロールの通常のフローの「反転」をします。</span><span class="sxs-lookup"><span data-stu-id="27deb-153">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


<span data-ttu-id="27deb-154">このチュートリアルで使用して[Unity](https://msdn.microsoft.com/library/ff647202.aspx) Microsoft Patterns から&amp;プラクティス。</span><span class="sxs-lookup"><span data-stu-id="27deb-154">For this tutorial, we'll use [Unity](https://msdn.microsoft.com/library/ff647202.aspx) from Microsoft Patterns &amp; Practices.</span></span> <span data-ttu-id="27deb-155">(その他の一般的なライブラリを含める[Castle Windsor](http://www.castleproject.org/)、 [Spring.Net](http://www.springframework.net/)、 [Autofac](https://code.google.com/p/autofac/)、 [Ninject](http://www.ninject.org/)、および[StructureMap](http://docs.structuremap.net/).)NuGet パッケージ マネージャーを使用して、Unity をインストールすることができます。</span><span class="sxs-lookup"><span data-stu-id="27deb-155">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), and [StructureMap](http://docs.structuremap.net/).) You can use NuGet Package Manager to install Unity.</span></span> <span data-ttu-id="27deb-156">**ツール** メニューの選択 Visual Studio で**ライブラリ パッケージ マネージャー**を選択し、**パッケージ マネージャー コンソール**します。</span><span class="sxs-lookup"><span data-stu-id="27deb-156">From the **Tools** menu in Visual Studio, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="27deb-157">パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="27deb-157">In the Package Manager Console window, type the following command:</span></span>

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

<span data-ttu-id="27deb-158">実装を次に示します**IDependencyResolver** Unity コンテナーをラップします。</span><span class="sxs-lookup"><span data-stu-id="27deb-158">Here is an implementation of **IDependencyResolver** that wraps a Unity container.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="27deb-159">場合、 **GetService**返すかメソッドは、型を解決することはできません、 **null**します。</span><span class="sxs-lookup"><span data-stu-id="27deb-159">If the **GetService** method cannot resolve a type, it should return **null**.</span></span> <span data-ttu-id="27deb-160">場合、 **GetServices**メソッドは、型を解決することはできません、空のコレクション オブジェクトを返す必要があります。</span><span class="sxs-lookup"><span data-stu-id="27deb-160">If the **GetServices** method cannot resolve a type, it should return an empty collection object.</span></span> <span data-ttu-id="27deb-161">不明な型の例外をスローしません。</span><span class="sxs-lookup"><span data-stu-id="27deb-161">Don't throw exceptions for unknown types.</span></span>


## <a name="configuring-the-dependency-resolver"></a><span data-ttu-id="27deb-162">依存関係競合回避モジュールを構成します。</span><span class="sxs-lookup"><span data-stu-id="27deb-162">Configuring the Dependency Resolver</span></span>

<span data-ttu-id="27deb-163">依存関係競合回避モジュールを設定、 **DependencyResolver**のグローバル プロパティ**HttpConfiguration**オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="27deb-163">Set the dependency resolver on the **DependencyResolver** property of the global **HttpConfiguration** object.</span></span>

<span data-ttu-id="27deb-164">次のコードのレジスタ、 `IProductRepository` Unity を使用したインターフェイスを作成し、`UnityResolver`します。</span><span class="sxs-lookup"><span data-stu-id="27deb-164">The following code registers the `IProductRepository` interface with Unity and then creates a `UnityResolver`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a><span data-ttu-id="27deb-165">依存関係のスコープとコント ローラーの有効期間</span><span class="sxs-lookup"><span data-stu-id="27deb-165">Dependency Scope and Controller Lifetime</span></span>

<span data-ttu-id="27deb-166">コント ローラーは、要求ごとに作成されます。</span><span class="sxs-lookup"><span data-stu-id="27deb-166">Controllers are created per request.</span></span> <span data-ttu-id="27deb-167">オブジェクトの有効期間を管理する**IDependencyResolver**の概念を使用して、*スコープ*します。</span><span class="sxs-lookup"><span data-stu-id="27deb-167">To manage object lifetimes, **IDependencyResolver** uses the concept of a *scope*.</span></span>

<span data-ttu-id="27deb-168">アタッチされている依存関係競合回避モジュール、 **HttpConfiguration**オブジェクトはグローバル スコープを持ちます。</span><span class="sxs-lookup"><span data-stu-id="27deb-168">The dependency resolver attached to the **HttpConfiguration** object has global scope.</span></span> <span data-ttu-id="27deb-169">Web API コント ローラーを作成するときに呼び出す**BeginScope**します。</span><span class="sxs-lookup"><span data-stu-id="27deb-169">When Web API creates a controller, it calls **BeginScope**.</span></span> <span data-ttu-id="27deb-170">このメソッドが戻る、 **IDependencyScope**子スコープを表します。</span><span class="sxs-lookup"><span data-stu-id="27deb-170">This method returns an **IDependencyScope** that represents a child scope.</span></span>

<span data-ttu-id="27deb-171">Web API を呼び出して**GetService**コント ローラーを作成する子スコープにします。</span><span class="sxs-lookup"><span data-stu-id="27deb-171">Web API then calls **GetService** on the child scope to create the controller.</span></span> <span data-ttu-id="27deb-172">Web API を呼び出す要求が完了したら、 **Dispose**子スコープにします。</span><span class="sxs-lookup"><span data-stu-id="27deb-172">When request is complete, Web API calls **Dispose** on the child scope.</span></span> <span data-ttu-id="27deb-173">使用して、 **Dispose**メソッドをコント ローラーの依存関係を破棄します。</span><span class="sxs-lookup"><span data-stu-id="27deb-173">Use the **Dispose** method to dispose of the controller's dependencies.</span></span>

<span data-ttu-id="27deb-174">どのように実装する**BeginScope** IoC コンテナーに依存します。</span><span class="sxs-lookup"><span data-stu-id="27deb-174">How you implement **BeginScope** depends on the IoC container.</span></span> <span data-ttu-id="27deb-175">Unity のスコープは、子コンテナーに対応します。</span><span class="sxs-lookup"><span data-stu-id="27deb-175">For Unity, scope corresponds to a child container:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

<span data-ttu-id="27deb-176">ほとんどの IoC コンテナーのような対応があります。</span><span class="sxs-lookup"><span data-stu-id="27deb-176">Most IoC containers have similar equivalents.</span></span>
