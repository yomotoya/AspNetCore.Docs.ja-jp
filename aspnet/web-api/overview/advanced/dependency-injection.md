---
uid: web-api/overview/advanced/dependency-injection
title: ASP.NET Web API 2 の依存関係の挿入 |Microsoft ドキュメント
author: MikeWasson
description: このチュートリアルでは、ASP.NET Web API コント ローラーに依存関係を挿入する方法を示します。 ソフトウェアのバージョンがチュートリアル Web API 2 Unity アプリケーション ブロックで使用しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 7f64cc83e36c80b0ffd53edfc629557c0847b200
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036516"
---
<a name="dependency-injection-in-aspnet-web-api-2"></a><span data-ttu-id="19028-104">ASP.NET Web API 2 の依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="19028-104">Dependency Injection in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="19028-105">によって[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="19028-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="19028-106">完成したプロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="19028-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> <span data-ttu-id="19028-107">このチュートリアルでは、ASP.NET Web API コント ローラーに依存関係を挿入する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="19028-107">This tutorial shows how to inject dependencies into your ASP.NET Web API controller.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="19028-108">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="19028-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="19028-109">Web API 2</span><span class="sxs-lookup"><span data-stu-id="19028-109">Web API 2</span></span>
> - [<span data-ttu-id="19028-110">Unity アプリケーション ブロック</span><span class="sxs-lookup"><span data-stu-id="19028-110">Unity Application Block</span></span>](https://www.nuget.org/packages/Unity/)
> - <span data-ttu-id="19028-111">Entity Framework 6 (バージョン 5 でも動作)</span><span class="sxs-lookup"><span data-stu-id="19028-111">Entity Framework 6 (version 5 also works)</span></span>


## <a name="what-is-dependency-injection"></a><span data-ttu-id="19028-112">依存関係の挿入とは何ですか。</span><span class="sxs-lookup"><span data-stu-id="19028-112">What is Dependency Injection?</span></span>

<span data-ttu-id="19028-113">A*依存関係*別のオブジェクトを必要とする任意のオブジェクトします。</span><span class="sxs-lookup"><span data-stu-id="19028-113">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="19028-114">たとえばを定義する一般的なは、[リポジトリ](http://martinfowler.com/eaaCatalog/repository.html)データ アクセスを処理します。</span><span class="sxs-lookup"><span data-stu-id="19028-114">For example, it's common to define a [repository](http://martinfowler.com/eaaCatalog/repository.html) that handles data access.</span></span> <span data-ttu-id="19028-115">例を使って説明しましょう。</span><span class="sxs-lookup"><span data-stu-id="19028-115">Let's illustrate with an example.</span></span> <span data-ttu-id="19028-116">最初に、ドメイン モデルを定義します。</span><span class="sxs-lookup"><span data-stu-id="19028-116">First, we'll define a domain model:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="19028-117">Entity Framework を使用して、データベース内の項目を格納する単純なリポジトリ クラスを次に示します。</span><span class="sxs-lookup"><span data-stu-id="19028-117">Here is a simple repository class that stores items in a database, using Entity Framework.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="19028-118">ここでの GET 要求をサポートする Web API コント ローラーを定義してみましょう`Product`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="19028-118">Now let's define a Web API controller that supports GET requests for `Product` entities.</span></span> <span data-ttu-id="19028-119">(いい POST とわかりやすくするためには、他のメソッドを出力します。)最初の試行を次に示します。</span><span class="sxs-lookup"><span data-stu-id="19028-119">(I'm leaving out POST and other methods for simplicity.) Here is a first attempt:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="19028-120">コント ローラーのクラスが依存している通知`ProductRepository`、私たちは、コント ローラーを作成できるようにすることと、`ProductRepository`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="19028-120">Notice that the controller class depends on `ProductRepository`, and we are letting the controller create the `ProductRepository` instance.</span></span> <span data-ttu-id="19028-121">ただし、いくつかの理由、ハード コードの依存関係でこのようにすることは適切を勧めします。</span><span class="sxs-lookup"><span data-stu-id="19028-121">However, it's a bad idea to hard code the dependency in this way, for several reasons.</span></span>

- <span data-ttu-id="19028-122">置換する場合`ProductRepository`別の実装にする必要があります、コント ローラー クラスを変更します。</span><span class="sxs-lookup"><span data-stu-id="19028-122">If you want to replace `ProductRepository` with a different implementation, you also need to modify the controller class.</span></span>
- <span data-ttu-id="19028-123">場合、`ProductRepository`依存関係がある、この内部コント ローラーを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="19028-123">If the `ProductRepository` has dependencies, you must configure these inside the controller.</span></span> <span data-ttu-id="19028-124">複数のコント ローラーで、大規模なプロジェクトの構成コードがプロジェクトに分散します。</span><span class="sxs-lookup"><span data-stu-id="19028-124">For a large project with multiple controllers, your configuration code becomes scattered across your project.</span></span>
- <span data-ttu-id="19028-125">コント ローラーは、データベースのクエリにハードコードされているため、単体テストに困難です。</span><span class="sxs-lookup"><span data-stu-id="19028-125">It is hard to unit test, because the controller is hard-coded to query the database.</span></span> <span data-ttu-id="19028-126">単体テストでは、現在デザインでは可能では、モックまたはスタブのリポジトリを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="19028-126">For a unit test, you should use a mock or stub repository, which is not possible with the currect design.</span></span>

<span data-ttu-id="19028-127">これらの問題を解決お*送り込んで*コント ローラーにリポジトリです。</span><span class="sxs-lookup"><span data-stu-id="19028-127">We can address these problems by *injecting* the repository into the controller.</span></span> <span data-ttu-id="19028-128">まず、リファクタリング、`ProductRepository`クラス インターフェイスを。</span><span class="sxs-lookup"><span data-stu-id="19028-128">First, refactor the `ProductRepository` class into an interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="19028-129">指定し、`IProductRepository`コンス トラクターのパラメーターとして。</span><span class="sxs-lookup"><span data-stu-id="19028-129">Then provide the `IProductRepository` as a constructor parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="19028-130">この例では[コンス トラクター インジェクション](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection)です。</span><span class="sxs-lookup"><span data-stu-id="19028-130">This example uses [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="19028-131">使用することも*set アクセス操作子インジェクション*、setter メソッドまたはプロパティを依存関係を設定します。</span><span class="sxs-lookup"><span data-stu-id="19028-131">You can also use *setter injection*, where you set the dependency through a setter method or property.</span></span>

<span data-ttu-id="19028-132">アプリケーションでは、コント ローラーを直接作成しないため、問題があるようになりました。</span><span class="sxs-lookup"><span data-stu-id="19028-132">But now there is a problem, because your application doesn't create the controller directly.</span></span> <span data-ttu-id="19028-133">Web API は、要求をルーティングし、は認識しない Web API コント ローラーを作成`IProductRepository`です。</span><span class="sxs-lookup"><span data-stu-id="19028-133">Web API creates the controller when it routes the request, and Web API doesn't know anything about `IProductRepository`.</span></span> <span data-ttu-id="19028-134">これは、Web API の依存関係競合回避モジュールの出番です。</span><span class="sxs-lookup"><span data-stu-id="19028-134">This is where the Web API dependency resolver comes in.</span></span>

## <a name="the-web-api-dependency-resolver"></a><span data-ttu-id="19028-135">Web API の 依存関係競合回避モジュール</span><span class="sxs-lookup"><span data-stu-id="19028-135">The Web API Dependency Resolver</span></span>

<span data-ttu-id="19028-136">Web API の定義、 **IDependencyResolver**の依存関係を解決するためのインターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="19028-136">Web API defines the **IDependencyResolver** interface for resolving dependencies.</span></span> <span data-ttu-id="19028-137">インターフェイスの定義を次に示します。</span><span class="sxs-lookup"><span data-stu-id="19028-137">Here is the definition of the interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="19028-138">**IDependencyScope**インターフェイスが 2 つのメソッドには。</span><span class="sxs-lookup"><span data-stu-id="19028-138">The **IDependencyScope** interface has two methods:</span></span>

- <span data-ttu-id="19028-139">**GetService**型の 1 つのインスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="19028-139">**GetService** creates one instance of a type.</span></span>
- <span data-ttu-id="19028-140">**GetServices**指定した型のオブジェクトのコレクションを作成します。</span><span class="sxs-lookup"><span data-stu-id="19028-140">**GetServices** creates a collection of objects of a specified type.</span></span>

<span data-ttu-id="19028-141">**IDependencyResolver**メソッドを継承**IDependencyScope**を追加し、 **BeginScope**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="19028-141">The **IDependencyResolver** method inherits **IDependencyScope** and adds the **BeginScope** method.</span></span> <span data-ttu-id="19028-142">このチュートリアルで後ほどスコープについて説明します。</span><span class="sxs-lookup"><span data-stu-id="19028-142">I'll talk about scopes later in this tutorial.</span></span>

<span data-ttu-id="19028-143">Web API コント ローラーのインスタンスを作成すると、最初に呼び出す**IDependencyResolver.GetService**コント ローラーの種類を渡して、します。</span><span class="sxs-lookup"><span data-stu-id="19028-143">When Web API creates a controller instance, it first calls **IDependencyResolver.GetService**, passing in the controller type.</span></span> <span data-ttu-id="19028-144">この拡張フックを使用すると、すべての依存関係を解決する、コント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="19028-144">You can use this extensibility hook to create the controller, resolving any dependencies.</span></span> <span data-ttu-id="19028-145">場合**GetService**は null を返します、Web API コント ローラー クラスにパラメーターなしのコンス トラクターを検索します。</span><span class="sxs-lookup"><span data-stu-id="19028-145">If **GetService** returns null, Web API looks for a parameterless constructor on the controller class.</span></span>

## <a name="dependency-resolution-with-the-unity-container"></a><span data-ttu-id="19028-146">Unity コンテナーとの依存関係の解決</span><span class="sxs-lookup"><span data-stu-id="19028-146">Dependency Resolution with the Unity Container</span></span>

<span data-ttu-id="19028-147">完全な記述できますが**IDependencyResolver**最初、インターフェイスから実装は実際に、Web API と既存の IoC コンテナー間の仲介役として機能するように設計されています。</span><span class="sxs-lookup"><span data-stu-id="19028-147">Although you could write a complete **IDependencyResolver** implementation from scratch, the interface is really designed to act as bridge between Web API and existing IoC containers.</span></span>

<span data-ttu-id="19028-148">IoC コンテナーは、依存関係の管理を担当するソフトウェア コンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="19028-148">An IoC container is a software component that is responsible for managing dependencies.</span></span> <span data-ttu-id="19028-149">型のコンテナーを登録し、コンテナーを使用して、オブジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="19028-149">You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="19028-150">コンテナーは、依存関係を自動的に判別されます。</span><span class="sxs-lookup"><span data-stu-id="19028-150">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="19028-151">多くの IoC コンテナーを使用すると、オブジェクトの有効期間、スコープなどを制御できます。</span><span class="sxs-lookup"><span data-stu-id="19028-151">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="19028-152">"IoC"が「コントロールの反転」の略フレームワークがアプリケーション コードを呼び出す、一般的なパターンはします。</span><span class="sxs-lookup"><span data-stu-id="19028-152">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="19028-153">IoC コンテナーは、通常の制御フローの「反転」に、オブジェクトに構築します。</span><span class="sxs-lookup"><span data-stu-id="19028-153">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


<span data-ttu-id="19028-154">このチュートリアルで使用します[Unity](https://msdn.microsoft.com/library/ff647202.aspx) Microsoft Patterns から&amp;プラクティスです。</span><span class="sxs-lookup"><span data-stu-id="19028-154">For this tutorial, we'll use [Unity](https://msdn.microsoft.com/library/ff647202.aspx) from Microsoft Patterns &amp; Practices.</span></span> <span data-ttu-id="19028-155">(その他の一般的なライブラリを含む[城 Windsor](http://www.castleproject.org/)、 [Spring.Net](http://www.springframework.net/)、 [Autofac](https://code.google.com/p/autofac/)、 [Ninject](http://www.ninject.org/)、および[StructureMap](http://docs.structuremap.net/).)NuGet Package Manager を使用して、Unity をインストールすることができます。</span><span class="sxs-lookup"><span data-stu-id="19028-155">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), and [StructureMap](http://docs.structuremap.net/).) You can use NuGet Package Manager to install Unity.</span></span> <span data-ttu-id="19028-156">**ツール**選択、Visual Studio のメニュー**ライブラリ パッケージ マネージャー**選択してから、 **Package Manager Console**です。</span><span class="sxs-lookup"><span data-stu-id="19028-156">From the **Tools** menu in Visual Studio, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="19028-157">パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="19028-157">In the Package Manager Console window, type the following command:</span></span>

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

<span data-ttu-id="19028-158">実装を次に示します**IDependencyResolver** Unity コンテナーをラップします。</span><span class="sxs-lookup"><span data-stu-id="19028-158">Here is an implementation of **IDependencyResolver** that wraps a Unity container.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="19028-159">場合、 **GetService**メソッドが型を解決することはできませんが返されます**null**です。</span><span class="sxs-lookup"><span data-stu-id="19028-159">If the **GetService** method cannot resolve a type, it should return **null**.</span></span> <span data-ttu-id="19028-160">場合、 **GetServices**メソッドは、型を解決することはできません、空のコレクション オブジェクトを返す必要があります。</span><span class="sxs-lookup"><span data-stu-id="19028-160">If the **GetServices** method cannot resolve a type, it should return an empty collection object.</span></span> <span data-ttu-id="19028-161">不明な型の例外をスローしません。</span><span class="sxs-lookup"><span data-stu-id="19028-161">Don't throw exceptions for unknown types.</span></span>


## <a name="configuring-the-dependency-resolver"></a><span data-ttu-id="19028-162">依存関係競合回避モジュールを構成します。</span><span class="sxs-lookup"><span data-stu-id="19028-162">Configuring the Dependency Resolver</span></span>

<span data-ttu-id="19028-163">依存関係競合回避モジュールを設定、 **DependencyResolver**のグローバル プロパティ**HttpConfiguration**オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="19028-163">Set the dependency resolver on the **DependencyResolver** property of the global **HttpConfiguration** object.</span></span>

<span data-ttu-id="19028-164">次のコードのレジスタ、 `IProductRepository` Unity とやり取りし、作成、`UnityResolver`です。</span><span class="sxs-lookup"><span data-stu-id="19028-164">The following code registers the `IProductRepository` interface with Unity and then creates a `UnityResolver`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a><span data-ttu-id="19028-165">依存関係スコープとコント ローラーの有効期間</span><span class="sxs-lookup"><span data-stu-id="19028-165">Dependency Scope and Controller Lifetime</span></span>

<span data-ttu-id="19028-166">コント ローラーは、要求ごとに作成されます。</span><span class="sxs-lookup"><span data-stu-id="19028-166">Controllers are created per request.</span></span> <span data-ttu-id="19028-167">オブジェクトの有効期間を管理する**IDependencyResolver**の概念を使用して、*スコープ*です。</span><span class="sxs-lookup"><span data-stu-id="19028-167">To manage object lifetimes, **IDependencyResolver** uses the concept of a *scope*.</span></span>

<span data-ttu-id="19028-168">接続されている依存関係競合回避モジュール、 **HttpConfiguration**オブジェクトはグローバル スコープを持ちます。</span><span class="sxs-lookup"><span data-stu-id="19028-168">The dependency resolver attached to the **HttpConfiguration** object has global scope.</span></span> <span data-ttu-id="19028-169">Web API コント ローラーを作成すると呼び出す**BeginScope**です。</span><span class="sxs-lookup"><span data-stu-id="19028-169">When Web API creates a controller, it calls **BeginScope**.</span></span> <span data-ttu-id="19028-170">このメソッドが戻る、 **IDependencyScope**子スコープを表すです。</span><span class="sxs-lookup"><span data-stu-id="19028-170">This method returns an **IDependencyScope** that represents a child scope.</span></span>

<span data-ttu-id="19028-171">Web API を呼び出します**GetService**コント ローラーを作成する子スコープでします。</span><span class="sxs-lookup"><span data-stu-id="19028-171">Web API then calls **GetService** on the child scope to create the controller.</span></span> <span data-ttu-id="19028-172">要求が完了したら、Web API を呼び出す**Dispose**子スコープでします。</span><span class="sxs-lookup"><span data-stu-id="19028-172">When request is complete, Web API calls **Dispose** on the child scope.</span></span> <span data-ttu-id="19028-173">使用して、 **Dispose**コント ローラーの依存関係を破棄するメソッド。</span><span class="sxs-lookup"><span data-stu-id="19028-173">Use the **Dispose** method to dispose of the controller's dependencies.</span></span>

<span data-ttu-id="19028-174">どのように実装する**BeginScope** IoC コンテナーによって異なります。</span><span class="sxs-lookup"><span data-stu-id="19028-174">How you implement **BeginScope** depends on the IoC container.</span></span> <span data-ttu-id="19028-175">Unity のスコープは、子コンテナーに対応します。</span><span class="sxs-lookup"><span data-stu-id="19028-175">For Unity, scope corresponds to a child container:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

<span data-ttu-id="19028-176">ほとんどの IoC コンテナーは、対応するようなソリューションを持ちます。</span><span class="sxs-lookup"><span data-stu-id="19028-176">Most IoC containers have similar equivalents.</span></span>
