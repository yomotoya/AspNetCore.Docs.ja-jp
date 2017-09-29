---
title: "ASP.NET Core でのテストの統合"
author: ardalis
description: "ASP.NET Core の統合アプリケーションのコンポーネントが正しく動作するようにテストを使用する方法。"
keywords: "ASP.NET Core、統合 Razor をテストします。"
ms.author: riande
manager: wpickett
ms.date: 09/25/2017
ms.topic: article
ms.assetid: 40d534f2-89b3-4b09-9c2c-3494bf9991c9
ms.technology: aspnet
ms.prod: asp.net-core
uid: testing/integration-testing
ms.openlocfilehash: fab1fb0e64debd8488713b3518cb3bc90182616b
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/28/2017
---
# <a name="integration-testing-in-aspnet-core"></a><span data-ttu-id="ecdc6-104">ASP.NET Core でのテストの統合</span><span class="sxs-lookup"><span data-stu-id="ecdc6-104">Integration testing in ASP.NET Core</span></span>

<span data-ttu-id="ecdc6-105">によって[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="ecdc6-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="ecdc6-106">統合テストにより、一緒にアセンブルときに、アプリケーションのコンポーネントが正しく動作します。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-106">Integration testing ensures that an application's components function correctly when assembled together.</span></span> <span data-ttu-id="ecdc6-107">ASP.NET Core サポート統合が単体テスト フレームワークと、ネットワークのオーバーヘッドが要求を処理するために使用する組み込みのテストの web ホストを使用してテストします。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-107">ASP.NET Core supports integration testing using unit test frameworks and a built-in test web host that can be used to handle requests without network overhead.</span></span>

[<span data-ttu-id="ecdc6-108">サンプル コードを表示またはダウンロードする</span><span class="sxs-lookup"><span data-stu-id="ecdc6-108">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/integration-testing/sample)

## <a name="introduction-to-integration-testing"></a><span data-ttu-id="ecdc6-109">統合テストの概要</span><span class="sxs-lookup"><span data-stu-id="ecdc6-109">Introduction to integration testing</span></span>

<span data-ttu-id="ecdc6-110">統合テストでは、アプリケーションのさまざまな部分が正しく動作する同時を確認します。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-110">Integration tests verify that different parts of an application work correctly together.</span></span> <span data-ttu-id="ecdc6-111">異なり[単体テスト](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)、統合テスト アプリケーション インフラストラクチャ上の問題、データベース、ファイル システム、ネットワーク リソース、または web 要求と応答など多くの場合します。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-111">Unlike [Unit testing](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test), integration tests frequently involve application infrastructure concerns, such as a database, file system, network resources, or web requests and responses.</span></span> <span data-ttu-id="ecdc6-112">単体テストを使用して fakes またはこれらの問題の代わりにモック オブジェクトですが、統合テストの目的は、システムがこれらのシステムで正しく動作することを確認します。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-112">Unit tests use fakes or mock objects in place of these concerns, but the purpose of integration tests is to confirm that the system works as expected with these systems.</span></span>

<span data-ttu-id="ecdc6-113">統合テストとコードの大規模なセグメントが実行されるため、インフラストラクチャの要素に依存しているために次の桁違い単体テストよりも低速である傾向があります。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-113">Integration tests, because they exercise larger segments of code and because they rely on infrastructure elements, tend to be orders of magnitude slower than unit tests.</span></span> <span data-ttu-id="ecdc6-114">したがって、ことをお勧め統合テストの数を制限するを記述すると、単体テストで同じ動作をテストする場合に特にです。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-114">Thus, it's a good idea to limit how many integration tests you write, especially if you can test the same behavior with a unit test.</span></span>

> [!NOTE]
> <span data-ttu-id="ecdc6-115">一部の動作をテストするには、単体テストまたは統合テストを使用して、場合は、単体テストを高速になりますほぼ常になるためです。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-115">If some behavior can be tested using either a unit test or an integration test, prefer the unit test, since it will be almost always be faster.</span></span> <span data-ttu-id="ecdc6-116">数十または数百の多くの異なる入力で単体テストがいくつかの最も重要なシナリオをカバーする統合テストだけがあります。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-116">You might have dozens or hundreds of unit tests with many different inputs but just a handful of integration tests covering the most important scenarios.</span></span>

<span data-ttu-id="ecdc6-117">独自のメソッド内のロジックをテストは、通常、単体テストのドメインです。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-117">Testing the logic within your own methods is usually the domain of unit tests.</span></span> <span data-ttu-id="ecdc6-118">ASP.NET Core やデータベースなど、そのフレームワーク内で、アプリケーションの動作のテストは統合をテストになるようにします。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-118">Testing how your application works within its framework, for example with ASP.NET Core, or with a database is where integration tests come into play.</span></span> <span data-ttu-id="ecdc6-119">多くの統合テストをデータベースに行を記述し、それを読み出すことがあることを確認することを受け取りません。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-119">It doesn't take too many integration tests to confirm that you're able to write a row to the database and read it back.</span></span> <span data-ttu-id="ecdc6-120">データ アクセス コードのすべての可能な順列をテストする必要はありません - のみ、アプリケーションが正常に動作している信頼することを十分にテストする必要があります。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-120">You don't need to test every possible permutation of your data access code - you only need to test enough to give you confidence that your application is working properly.</span></span>

## <a name="integration-testing-aspnet-core"></a><span data-ttu-id="ecdc6-121">統合テスト ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ecdc6-121">Integration testing ASP.NET Core</span></span>

<span data-ttu-id="ecdc6-122">実行の統合テストに設定を取得するには、テスト プロジェクトの作成を ASP.NET Core web プロジェクトへの参照を追加およびテスト ランナーをインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-122">To get set up to run integration tests, you'll need to create a test project, add a reference to your ASP.NET Core web project, and install a test runner.</span></span> <span data-ttu-id="ecdc6-123">このプロセスの説明、[単体テスト](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)ドキュメントについてより詳細な手順について、テストおよびテストおよびテスト クラスの名前付けに関する推奨事項を実行するとします。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-123">This process is described in the [Unit testing](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) documentation, along with more detailed instructions on running tests and recommendations for naming your tests and test classes.</span></span>

> [!NOTE]
> <span data-ttu-id="ecdc6-124">単体テストとさまざまなプロジェクトを使用して、統合テストで区切ります。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-124">Separate your unit tests and your integration tests using different projects.</span></span> <span data-ttu-id="ecdc6-125">これにより、により、単体テストにインフラストラクチャ上の問題を誤って導入しないことを確認し、簡単に実行するテストのセットを選択することができます。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-125">This helps ensure you don't accidentally introduce infrastructure concerns into your unit tests and lets you easily choose which set of tests to run.</span></span>

### <a name="the-test-host"></a><span data-ttu-id="ecdc6-126">テスト ホスト</span><span class="sxs-lookup"><span data-stu-id="ecdc6-126">The Test Host</span></span>

<span data-ttu-id="ecdc6-127">ASP.NET Core には、統合テスト プロジェクトに追加することができ、アプリケーションで使用される ASP.NET Core のホストに、実際の web ホストの必要としないサービス テストを要求するテスト ホストが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-127">ASP.NET Core includes a test host that can be added to integration test projects and used to host ASP.NET Core applications, serving test requests without the need for a real web host.</span></span> <span data-ttu-id="ecdc6-128">提供されたサンプルには、統合テスト プロジェクト使用するように構成されていますにはが含まれています。 [xUnit](https://xunit.github.io)とホストをテストします。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-128">The provided sample includes an integration test project which has been configured to use [xUnit](https://xunit.github.io) and the Test Host.</span></span> <span data-ttu-id="ecdc6-129">使用して、 `Microsoft.AspNetCore.TestHost` NuGet パッケージです。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-129">It uses the `Microsoft.AspNetCore.TestHost` NuGet package.</span></span>

<span data-ttu-id="ecdc6-130">1 回、`Microsoft.AspNetCore.TestHost`パッケージがプロジェクトに含まれる、作成および構成することができます、`TestServer`テストにします。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-130">Once the `Microsoft.AspNetCore.TestHost` package is included in the project, you'll be able to create and configure a `TestServer` in your tests.</span></span> <span data-ttu-id="ecdc6-131">次のテストは、サイトのルートへの要求が"Hello World!"を返すことを確認する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-131">The following test shows how to verify that a request made to the root of a site returns "Hello World!"</span></span> <span data-ttu-id="ecdc6-132">必要が正常に実行の既定値に対する Visual Studio によって作成された ASP.NET Core の空の Web テンプレート。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-132">and should run successfully against the default ASP.NET Core Empty Web template created by Visual Studio.</span></span>

[!code-csharp[Main](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebDefaultRequestShould.cs?name=snippet_WebDefault&highlight=7,16,22)]

<span data-ttu-id="ecdc6-133">このテストでは、配置 Act アサート パターンを使用しています。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-133">This test is using the Arrange-Act-Assert pattern.</span></span> <span data-ttu-id="ecdc6-134">配置手順のインスタンスを作成するコンス トラクターで行われます`TestServer`です。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-134">The Arrange step is done in the constructor, which creates an instance of `TestServer`.</span></span> <span data-ttu-id="ecdc6-135">構成`WebHostBuilder`作成に使用される、`TestHost`以外の場合は、この例では、 `Configure` (SUT) をテスト対象システムからメソッド`Startup`クラスに渡される、`WebHostBuilder`です。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-135">A configured `WebHostBuilder` will be used to create a `TestHost`; in this example, the `Configure` method from the system under test (SUT)'s `Startup` class is passed to the `WebHostBuilder`.</span></span> <span data-ttu-id="ecdc6-136">このメソッドの要求パイプラインの構成に使用する、 `TestServer` SUT サーバーの構成方法とまったく同様にします。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-136">This method will be used to configure the request pipeline of the `TestServer` identically to how the SUT server would be configured.</span></span>

<span data-ttu-id="ecdc6-137">テストの Act 部分で、要求が行われる、`TestServer`を文字列に戻す「/」パス、および応答のインスタンスは読み取りです。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-137">In the Act portion of the test, a request is made to the `TestServer` instance for the "/" path, and the response is read back into a string.</span></span> <span data-ttu-id="ecdc6-138">この文字列は、"Hello World!"の期待される文字列と比較されます。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-138">This string is compared with the expected string of "Hello World!".</span></span> <span data-ttu-id="ecdc6-139">一致した場合、テストに合格します。それ以外の場合、失敗します。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-139">If they match, the test passes; otherwise, it fails.</span></span>

<span data-ttu-id="ecdc6-140">これで、web アプリケーションを使用して素数のチェック機能が動作することを確認するいくつかの他の統合テストを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-140">Now you can add a few additional integration tests to confirm that the prime checking functionality works via the web application:</span></span>

[!code-csharp[Main](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebCheckPrimeShould.cs?name=snippet_CheckPrime)]

<span data-ttu-id="ecdc6-141">これらのテストを使用して、素数チェッカーの正確性をテストに実際にはしようとしていることではなく、web アプリケーションが期待どおりに表示を行う際に注意します。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-141">Note that you're not really trying to test the correctness of the prime number checker with these tests but rather that the web application is doing what you expect.</span></span> <span data-ttu-id="ecdc6-142">得られるように信頼度で単体テスト カバレッジが既にある`PrimeService`、ここで確認できます。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-142">You already have unit test coverage that gives you confidence in `PrimeService`, as you can see here:</span></span>

![テスト エクスプローラー](integration-testing/_static/test-explorer.png)

<span data-ttu-id="ecdc6-144">単体テストに関する詳細については、[単体テスト](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)資料です。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-144">You can learn more about the unit tests in the [Unit testing](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) article.</span></span>


### <a name="integration-testing-mvcrazor"></a><span data-ttu-id="ecdc6-145">Mvc Razor/テストの統合</span><span class="sxs-lookup"><span data-stu-id="ecdc6-145">Integration testing Mvc/Razor</span></span>

<span data-ttu-id="ecdc6-146">Razor ビューを含むテスト プロジェクトが必要`<PreserveCompilationContext>`に設定するのには true、 *.csproj*ファイル。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-146">Test projects that contain Razor views require `<PreserveCompilationContext>` be set to true in the *.csproj* file:</span></span>


```xml
    <PreserveCompilationContext>true</PreserveCompilationContext>
```

<span data-ttu-id="ecdc6-147">この要素が見つからないプロジェクトは次のようなエラーが生成されます。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-147">Projects missing this element will generate an error similar to the following:</span></span>
```
Microsoft.AspNetCore.Mvc.Razor.Compilation.CompilationFailedException: 'One or more compilation failures occurred:
ooebhccx.1bd(4,62): error CS0012: The type 'Attribute' is defined in an assembly that is not referenced. You must add a reference to assembly 'netstandard, Version=2.0.0.0, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51'.
```


## <a name="refactoring-to-use-middleware"></a><span data-ttu-id="ecdc6-148">リファクタリング ミドルウェアを使用するには</span><span class="sxs-lookup"><span data-stu-id="ecdc6-148">Refactoring to use middleware</span></span>

<span data-ttu-id="ecdc6-149">リファクタリングとは、その動作を変更することがなくそのデザインを向上させるために、アプリケーションのコードを変更するプロセスです。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-149">Refactoring is the process of changing an application's code to improve its design without changing its behavior.</span></span> <span data-ttu-id="ecdc6-150">理想的には、一連のこれらのヘルプでは前に、と変更後、システムの動作は同じことを確認するために、テストを渡すことがある場合に行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-150">It should ideally be done when there is a suite of passing tests, since these help ensure the system's behavior remains the same before and after the changes.</span></span> <span data-ttu-id="ecdc6-151">Web アプリケーションのロジック チェック素数を実装する方法を見て`Configure`メソッドを参照してください。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-151">Looking at the way in which the prime checking logic is implemented in the web application's `Configure` method, you see:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }

    app.Run(async (context) =>
    {
        if (context.Request.Path.Value.Contains("checkprime"))
        {
            int numberToCheck;
            try
            {
                numberToCheck = int.Parse(context.Request.QueryString.Value.Replace("?", ""));
                var primeService = new PrimeService();
                if (primeService.IsPrime(numberToCheck))
                {
                    await context.Response.WriteAsync($"{numberToCheck} is prime!");
                }
                else
                {
                    await context.Response.WriteAsync($"{numberToCheck} is NOT prime!");
                }
            }
            catch
            {
                await context.Response.WriteAsync("Pass in a number to check in the form /checkprime?5");
            }
        }
        else
        {
            await context.Response.WriteAsync("Hello World!");
        }
    });
}
```

<span data-ttu-id="ecdc6-152">このコードの動作ですが、方法として単純なようにこれは、ASP.NET Core アプリケーションでこのような機能を実装したいかけ離れていること。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-152">This code works, but it's far from how you would like to implement this kind of functionality in an ASP.NET Core application, even as simple as this is.</span></span> <span data-ttu-id="ecdc6-153">新機能を想像してください、`Configure`他の URL エンドポイントを追加するたびにこの量のコードを追加する必要がある場合は、メソッドをようになります。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-153">Imagine what the `Configure` method would look like if you needed to add this much code to it every time you add another URL endpoint!</span></span>

<span data-ttu-id="ecdc6-154">考慮すべき 1 つのオプションを追加する[MVC](xref:mvc/overview)素チェックを処理するアプリケーションとコント ローラーの作成にします。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-154">One option to consider is adding [MVC](xref:mvc/overview) to the application and creating a controller to handle the prime checking.</span></span> <span data-ttu-id="ecdc6-155">ただし、現在ないかどうかと仮定した場合必要があるその他の MVC 機能はすべて、ビットが overkill です。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-155">However, assuming you don't currently need any other MVC functionality, that's a bit overkill.</span></span>

<span data-ttu-id="ecdc6-156">活用すること、ただし、ASP.NET Core[ミドルウェア](xref:fundamentals/middleware)、いるいただくと、独自のクラス内のロジック チェック素数をカプセル化し、改善を実現[関心の分離](http://deviq.com/separation-of-concerns/)で、 `Configure`メソッド。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-156">You can, however, take advantage of ASP.NET Core [middleware](xref:fundamentals/middleware), which will help us encapsulate the prime checking logic in its own class and achieve better [separation of concerns](http://deviq.com/separation-of-concerns/) in the `Configure` method.</span></span>

<span data-ttu-id="ecdc6-157">ミドルウェア クラスが必要ですが、パラメーターとして指定するミドルウェアを使用してパスを許可する、`RequestDelegate`と`PrimeCheckerOptions`コンス トラクター内のインスタンス。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-157">You want to allow the path the middleware uses to be specified as a parameter, so the middleware class expects a `RequestDelegate` and a `PrimeCheckerOptions` instance in its constructor.</span></span> <span data-ttu-id="ecdc6-158">要求のパスでは、このミドルウェアは、新機能と一致しない場合、期待するように構成する単にチェーンで次のミドルウェアを呼び出すし、それ以上何もしないでください。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-158">If the path of the request doesn't match what this middleware is configured to expect, you simply call the next middleware in the chain and do nothing further.</span></span> <span data-ttu-id="ecdc6-159">実装コードの残りの部分`Configure`は、現在、`Invoke`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-159">The rest of the implementation code that was in `Configure` is now in the `Invoke` method.</span></span>

> [!NOTE]
> <span data-ttu-id="ecdc6-160">ミドルウェアが異なりますので、`PrimeService`サービスは、コンス トラクターでは、このサービスのインスタンスを要求してもします。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-160">Since the middleware depends on the `PrimeService` service, you're also requesting an instance of this service with the constructor.</span></span> <span data-ttu-id="ecdc6-161">このフレームワークは経由でこのサービスを提供[依存性の注入](xref:fundamentals/dependency-injection)に構成されているの例の場合を想定して`ConfigureServices`です。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-161">The framework will provide this service via [Dependency Injection](xref:fundamentals/dependency-injection), assuming it has been configured, for example in `ConfigureServices`.</span></span>

[!code-csharp[Main](../testing/integration-testing/sample/src/PrimeWeb/Middleware/PrimeCheckerMiddleware.cs?highlight=39-63)]

<span data-ttu-id="ecdc6-162">呼び出しがないため、このミドルウェアは、そのパスが一致するときに、要求のデリゲートのチェーン内のエンドポイントとして機能、`_next.Invoke`このミドルウェアは要求を処理します。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-162">Since this middleware acts as an endpoint in the request delegate chain when its path matches, there is no call to `_next.Invoke` when this middleware handles the request.</span></span>

<span data-ttu-id="ecdc6-163">設定され、いくつか便利な拡張メソッドをより簡単に構成するために作成、リファクタリングされたこのミドルウェア`Configure`メソッドは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-163">With this middleware in place and some helpful extension methods created to make configuring it easier, the refactored `Configure` method looks like this:</span></span>

[!code-csharp[Main](../testing/integration-testing/sample/src/PrimeWeb/Startup.cs?highlight=9&range=19-33)]

<span data-ttu-id="ecdc6-164">このリファクタリング後、web アプリケーションも動作する前とに、、統合テストにすべて合格ため確実に把握しています。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-164">Following this refactoring, you're confident that the web application still works as before, since your integration tests are all passing.</span></span>

> [!NOTE]
> <span data-ttu-id="ecdc6-165">リファクタリングを完了して、テストに合格した後、ソース管理に変更をコミットすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-165">It's a good idea to commit your changes to source control after you complete a refactoring and your tests pass.</span></span> <span data-ttu-id="ecdc6-166">場合は、テスト駆動開発を練習するとき[、赤、緑-リファクター サイクルへのコミットの追加を検討してください](https://ardalis.com/rgrc-is-the-new-red-green-refactor-for-test-first-development)です。</span><span class="sxs-lookup"><span data-stu-id="ecdc6-166">If you're practicing Test Driven Development, [consider adding Commit to your Red-Green-Refactor cycle](https://ardalis.com/rgrc-is-the-new-red-green-refactor-for-test-first-development).</span></span>

## <a name="resources"></a><span data-ttu-id="ecdc6-167">リソース</span><span class="sxs-lookup"><span data-stu-id="ecdc6-167">Resources</span></span>

* [<span data-ttu-id="ecdc6-168">単体テスト</span><span class="sxs-lookup"><span data-stu-id="ecdc6-168">Unit testing</span></span>](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [<span data-ttu-id="ecdc6-169">ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="ecdc6-169">Middleware</span></span>](xref:fundamentals/middleware)
* [<span data-ttu-id="ecdc6-170">コントローラーのテスト</span><span class="sxs-lookup"><span data-stu-id="ecdc6-170">Testing controllers</span></span>](xref:mvc/controllers/testing)
