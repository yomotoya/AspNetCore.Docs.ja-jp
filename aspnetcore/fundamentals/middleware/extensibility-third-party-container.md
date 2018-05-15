---
title: ASP.NET Core でのサードパーティ コンテナーによるミドルウェアのアクティブ化
author: guardrex
description: ASP.NET Core で、ファクトリベースのアクティブ化とサードパーティ コンテナーによる厳密に型指定されたミドルウェアを使用する方法を説明します。
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 02/02/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware/extensibility-third-party-container
ms.openlocfilehash: b6fd02d1863efe571bafe1344fb34939883e6cb5
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/02/2018
---
# <a name="middleware-activation-with-a-third-party-container-in-aspnet-core"></a><span data-ttu-id="07b91-103">ASP.NET Core でのサードパーティ コンテナーによるミドルウェアのアクティブ化</span><span class="sxs-lookup"><span data-stu-id="07b91-103">Middleware activation with a third-party container in ASP.NET Core</span></span>

<span data-ttu-id="07b91-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="07b91-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="07b91-105">この記事では、[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) と [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) を、サードパーティ コンテナーによる[ミドルウェア](xref:fundamentals/middleware/index)のアクティブ化の拡張ポイントとして使用する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="07b91-105">This article demonstrates how to use [IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) and [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) as an extensibility point for [middleware](xref:fundamentals/middleware/index) activation with a third-party container.</span></span> <span data-ttu-id="07b91-106">`IMiddlewareFactory` と `IMiddleware` の概要については、[ファクトリ ベースのミドルウェアのアクティブ化](xref:fundamentals/middleware/extensibility)に関するトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="07b91-106">For introductory information on `IMiddlewareFactory` and `IMiddleware`, see the [Factory-based middleware activation](xref:fundamentals/middleware/extensibility) topic.</span></span>

<span data-ttu-id="07b91-107">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="07b91-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="07b91-108">このサンプル アプリでは、`IMiddlewareFactory` の実装である `SimpleInjectorMiddlewareFactory` によるミドルウェアのアクティブ化を示します。</span><span class="sxs-lookup"><span data-stu-id="07b91-108">The sample app demonstrates middleware activation by an `IMiddlewareFactory` implementation, `SimpleInjectorMiddlewareFactory`.</span></span> <span data-ttu-id="07b91-109">このサンプルでは、[Simple Injector](https://simpleinjector.org) 依存関係の挿入 (DI) コンテナーを使用しています。</span><span class="sxs-lookup"><span data-stu-id="07b91-109">The sample uses the [Simple Injector](https://simpleinjector.org) dependency injection (DI) container.</span></span>

<span data-ttu-id="07b91-110">サンプルのミドルウェアの実装は、クエリ文字列パラメーターで提供された値を記録します (`key`)。</span><span class="sxs-lookup"><span data-stu-id="07b91-110">The sample's middleware implementation records the value provided by a query string parameter (`key`).</span></span> <span data-ttu-id="07b91-111">ミドルウェアは、メモリ内データベースにクエリ文字列値を記録するのに、挿入されたデータベース コンテキスト (スコープ化されたサービス) を使用します。</span><span class="sxs-lookup"><span data-stu-id="07b91-111">The middleware uses an injected database context (a scoped service) to record the query string value in an in-memory database.</span></span>

> [!NOTE]
> <span data-ttu-id="07b91-112">このサンプル アプリでは、デモンストレーション目的でのみ [Simple Injector](https://github.com/simpleinjector/SimpleInjector) を使用しています。</span><span class="sxs-lookup"><span data-stu-id="07b91-112">The sample app uses [Simple Injector](https://github.com/simpleinjector/SimpleInjector) purely for demonstration purposes.</span></span> <span data-ttu-id="07b91-113">Simple Injector の使用を推奨するものではありません。</span><span class="sxs-lookup"><span data-stu-id="07b91-113">Use of Simple Injector isn't an endorsement.</span></span> <span data-ttu-id="07b91-114">Simple Injector のドキュメントと GitHub Issues に記載されているミドルウェアのアクティブ化方法は、Simple Injector の保守担当者たちから推奨されています。</span><span class="sxs-lookup"><span data-stu-id="07b91-114">Middleware activation approaches described in the Simple Injector documentation and GitHub issues are recommended by the maintainers of Simple Injector.</span></span> <span data-ttu-id="07b91-115">詳細については、[Simple Injector のドキュメント](https://simpleinjector.readthedocs.io/en/latest/index.html)と [Simple Injector の GitHub リポジトリ](https://github.com/simpleinjector/SimpleInjector)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="07b91-115">For more information, see the [Simple Injector documentation](https://simpleinjector.readthedocs.io/en/latest/index.html) and [Simple Injector GitHub repository](https://github.com/simpleinjector/SimpleInjector).</span></span>

## <a name="imiddlewarefactory"></a><span data-ttu-id="07b91-116">IMiddlewareFactory</span><span class="sxs-lookup"><span data-stu-id="07b91-116">IMiddlewareFactory</span></span>

<span data-ttu-id="07b91-117">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) では、ミドルウェアを作成するメソッドを提供します。</span><span class="sxs-lookup"><span data-stu-id="07b91-117">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) provides methods to create middleware.</span></span>

<span data-ttu-id="07b91-118">サンプル アプリでは、`SimpleInjectorActivatedMiddleware` インスタンスを作成するミドルウェア ファクトリが実装されています。</span><span class="sxs-lookup"><span data-stu-id="07b91-118">In the sample app, a middleware factory is implemented to create an `SimpleInjectorActivatedMiddleware` instance.</span></span> <span data-ttu-id="07b91-119">このミドルウェア ファクトリでは、Simple Injector コンテナーを使用してミドルウェアを解決しています。</span><span class="sxs-lookup"><span data-stu-id="07b91-119">The middleware factory uses the Simple Injector container to resolve the middleware:</span></span>

[!code-csharp[](extensibility-third-party-container/sample/Middleware/SimpleInjectorMiddlewareFactory.cs?name=snippet1&highlight=5-8,12)]

## <a name="imiddleware"></a><span data-ttu-id="07b91-120">IMiddleware</span><span class="sxs-lookup"><span data-stu-id="07b91-120">IMiddleware</span></span>

<span data-ttu-id="07b91-121">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) では、アプリの要求パイプライン用にミドルウェアが定義されます。</span><span class="sxs-lookup"><span data-stu-id="07b91-121">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) defines middleware for the app's request pipeline.</span></span>

<span data-ttu-id="07b91-122">`IMiddlewareFactory` の実装によってアクティブ化されるミドルウェア (*Middleware/SimpleInjectorActivatedMiddleware.cs*):</span><span class="sxs-lookup"><span data-stu-id="07b91-122">Middleware activated by an `IMiddlewareFactory` implementation (*Middleware/SimpleInjectorActivatedMiddleware.cs*):</span></span>

[!code-csharp[](extensibility-third-party-container/sample/Middleware/SimpleInjectorActivatedMiddleware.cs?name=snippet1)]

<span data-ttu-id="07b91-123">ミドルウェア (*Middleware/MiddlewareExtensions.cs*) の拡張機能が作成されます。</span><span class="sxs-lookup"><span data-stu-id="07b91-123">An extension is created for the middleware (*Middleware/MiddlewareExtensions.cs*):</span></span>

[!code-csharp[](extensibility-third-party-container/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<span data-ttu-id="07b91-124">`Startup.ConfigureServices` はいくつかのタスクを実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="07b91-124">`Startup.ConfigureServices` must perform several tasks:</span></span>

* <span data-ttu-id="07b91-125">Simple Injector コンテナーを設定します。</span><span class="sxs-lookup"><span data-stu-id="07b91-125">Set up the Simple Injector container.</span></span>
* <span data-ttu-id="07b91-126">ファクトリとミドルウェアを登録します。</span><span class="sxs-lookup"><span data-stu-id="07b91-126">Register the factory and middleware.</span></span>
* <span data-ttu-id="07b91-127">Razor ページの Simple Injector コンテナーからアプリのデータベース コンテキストを利用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="07b91-127">Make the app's database context available from the Simple Injector container for a Razor Page.</span></span>

[!code-csharp[](extensibility-third-party-container/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="07b91-128">ミドルウェアは、要求を処理する `Startup.Configure` のパイプラインに登録されます。</span><span class="sxs-lookup"><span data-stu-id="07b91-128">The middleware is registered in the request processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](extensibility-third-party-container/sample/Startup.cs?name=snippet2&highlight=12)]

## <a name="additional-resources"></a><span data-ttu-id="07b91-129">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="07b91-129">Additional resources</span></span>

* [<span data-ttu-id="07b91-130">ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="07b91-130">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="07b91-131">ファクトリ ベースのミドルウェアのアクティブ化</span><span class="sxs-lookup"><span data-stu-id="07b91-131">Factory-based middleware activation</span></span>](xref:fundamentals/middleware/extensibility)
* [<span data-ttu-id="07b91-132">Simple Injector の GitHub リポジトリ</span><span class="sxs-lookup"><span data-stu-id="07b91-132">Simple Injector GitHub repository</span></span>](https://github.com/simpleinjector/SimpleInjector)
* [<span data-ttu-id="07b91-133">Simple Injector のドキュメント</span><span class="sxs-lookup"><span data-stu-id="07b91-133">Simple Injector documentation</span></span>](https://simpleinjector.readthedocs.io/en/latest/index.html)
