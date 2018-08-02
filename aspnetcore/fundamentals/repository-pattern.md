---
title: ASP.NET Core でのリポジトリ パターン
author: ardalis
description: ASP.NET Core アプリでリポジトリ アプリの設計パターンを実装する方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2018
uid: fundamentals/repository-pattern
ms.openlocfilehash: d64c3d090f62c580ebc05a6bbc2744a4508bb9bc
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342632"
---
# <a name="repository-pattern-with-aspnet-core"></a><span data-ttu-id="2a122-103">ASP.NET Core でのリポジトリ パターン</span><span class="sxs-lookup"><span data-stu-id="2a122-103">Repository pattern with ASP.NET Core</span></span>

<span data-ttu-id="2a122-104">作成者: [Steve Smith](https://ardalis.com/)、[Scott Addie](https://scottaddie.com)、[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2a122-104">By [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="2a122-105">"*リポジトリ パターン*" とは、データのアクセスをインターフェイスの抽象化の背後に分離する設計パターンです。</span><span class="sxs-lookup"><span data-stu-id="2a122-105">The *repository pattern* is a design pattern that isolates data access behind interface abstractions.</span></span> <span data-ttu-id="2a122-106">データベースへの接続やデータ ストレージ オブジェクトの操作を、インターフェイスの実装によって提供されるメソッドを使用して実行します。</span><span class="sxs-lookup"><span data-stu-id="2a122-106">Connecting to the database and manipulating data storage objects is performed through methods provided by the interface's implementation.</span></span> <span data-ttu-id="2a122-107">その結果、データベースに関する問題 (接続、コマンド、閲覧者など) に対処するためにコードを呼び出す必要が生じません。</span><span class="sxs-lookup"><span data-stu-id="2a122-107">Consequently, there's no need for calling code to deal with database concerns, such as connections, commands, and readers.</span></span>

<span data-ttu-id="2a122-108">ASP.NET Core でリポジトリ パターンを実装することの利点は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="2a122-108">Implementation of the repository pattern with ASP.NET Core has the following benefits:</span></span>

* <span data-ttu-id="2a122-109">ビジネス層とデータ アクセス層の間で相互に直接依存することがないため、アプリの構成が簡素になります。</span><span class="sxs-lookup"><span data-stu-id="2a122-109">Organization of the app is less complex with no direct interdependency between the business and data access layers.</span></span>
* <span data-ttu-id="2a122-110">データベースへのアクセス コードが 1 つまたは複数のリポジトリによって一元的に管理されるため、コードの再利用が簡単になります。</span><span class="sxs-lookup"><span data-stu-id="2a122-110">It's easier to reuse database access code because the code is centrally managed by one or more repositories.</span></span>
* <span data-ttu-id="2a122-111">データベース層から独立してビジネス ドメインを単体テストできます。</span><span class="sxs-lookup"><span data-stu-id="2a122-111">The business domain can be independently unit tested from the database layer.</span></span>

<span data-ttu-id="2a122-112">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="2a122-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="2a122-113">[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples)では、映画の登場人物の名前の一覧を初期化して表示するために、リポジトリ パターンが使用されます。</span><span class="sxs-lookup"><span data-stu-id="2a122-113">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) uses the repository pattern to initialize and display a list of movie character names.</span></span> <span data-ttu-id="2a122-114">アプリはデータ永続化のために [Entity Framework Core](/ef/core/) と `ApplicationDbContext` クラスを使用しますが、データベースのインフラストラクチャはデータにアクセスする場所からは見えません。</span><span class="sxs-lookup"><span data-stu-id="2a122-114">The app uses [Entity Framework Core](/ef/core/) and an `ApplicationDbContext` class for its data persistence, but the database infrastructure isn't apparent where the data is accessed.</span></span> <span data-ttu-id="2a122-115">データ アクセスとデータベース オブジェクトは、[リポジトリ](https://martinfowler.com/eaaCatalog/repository.html)の背後に抽象化されます。</span><span class="sxs-lookup"><span data-stu-id="2a122-115">Data access and database objects are abstracted behind a [repository](https://martinfowler.com/eaaCatalog/repository.html).</span></span>

## <a name="repository-interface"></a><span data-ttu-id="2a122-116">リポジトリ インターフェイス</span><span class="sxs-lookup"><span data-stu-id="2a122-116">Repository interface</span></span>

<span data-ttu-id="2a122-117">リポジトリ インターフェイスでは、実装のプロパティとメソッドが定義されます。</span><span class="sxs-lookup"><span data-stu-id="2a122-117">A repository interface defines the properties and methods for implementation.</span></span> <span data-ttu-id="2a122-118">サンプル アプリでは、映画の登場人物データのリポジトリ インターフェイスは `ICharacterRepository` です。</span><span class="sxs-lookup"><span data-stu-id="2a122-118">In the sample app, the repository interface for movie character data is `ICharacterRepository`.</span></span> <span data-ttu-id="2a122-119">`ICharacterRepository` では、アプリの `Character` インスタンスを操作するために必要な `ListAll` メソッドと `Add` メソッドが定義されます。</span><span class="sxs-lookup"><span data-stu-id="2a122-119">`ICharacterRepository` defines the `ListAll` and `Add` methods required to work with `Character` instances in the app:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="2a122-120">`Character` は次のように定義します。</span><span class="sxs-lookup"><span data-stu-id="2a122-120">`Character` is defined as:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

## <a name="repository-concrete-type"></a><span data-ttu-id="2a122-121">リポジトリの具象型</span><span class="sxs-lookup"><span data-stu-id="2a122-121">Repository concrete type</span></span>

<span data-ttu-id="2a122-122">インターフェイスは具象型によって実装されます。</span><span class="sxs-lookup"><span data-stu-id="2a122-122">The interface is implemented by a concrete type.</span></span> <span data-ttu-id="2a122-123">サンプル アプリでは、`CharacterRepository` でデータベース コンテキストが管理され、`ListAll` メソッドと `Add` メソッドが実装されます。</span><span class="sxs-lookup"><span data-stu-id="2a122-123">In the sample app, `CharacterRepository` manages the database context and implements the `ListAll` and `Add` methods:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

## <a name="register-the-repository-service"></a><span data-ttu-id="2a122-124">リポジトリ サービスの登録</span><span class="sxs-lookup"><span data-stu-id="2a122-124">Register the repository service</span></span>

<span data-ttu-id="2a122-125">リポジトリとデータベースのコンテキストは、`Startup.ConfigureServices` でサービス コンテナーに登録されます。</span><span class="sxs-lookup"><span data-stu-id="2a122-125">The repository and database context are registered with the service container in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="2a122-126">サンプル アプリでは、`ApplicationDbContext` は、拡張メソッド [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) を呼び出して構成します。</span><span class="sxs-lookup"><span data-stu-id="2a122-126">In the sample app, `ApplicationDbContext` is configured with the call to the extension method [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext).</span></span> <span data-ttu-id="2a122-127">`ICharacterRepository` はスコープ サービスとして登録されます。</span><span class="sxs-lookup"><span data-stu-id="2a122-127">`ICharacterRepository` is registered as a scoped service:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,18)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,12)]

::: moniker-end

## <a name="inject-an-instance-of-the-repository"></a><span data-ttu-id="2a122-128">リポジトリのインスタンスの挿入</span><span class="sxs-lookup"><span data-stu-id="2a122-128">Inject an instance of the repository</span></span>

<span data-ttu-id="2a122-129">データベースへのアクセスが必要なクラスでは、リポジトリのインスタンスがコンストラクターを介して要求され、クラスのメソッドが使用するためにプライベート フィールドに割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="2a122-129">In a class where database access is required, an instance of the repository is requested via the constructor and assigned to a private field for use by class methods.</span></span> <span data-ttu-id="2a122-130">サンプル アプリでは、次の操作を行うために `ICharacterRepository` が使用されます。</span><span class="sxs-lookup"><span data-stu-id="2a122-130">In the sample app, `ICharacterRepository` is used to:</span></span>

* <span data-ttu-id="2a122-131">登場人物が存在しない場合にデータベースにデータを入力します。</span><span class="sxs-lookup"><span data-stu-id="2a122-131">Populate the database if no characters exist.</span></span>
* <span data-ttu-id="2a122-132">表示用に登場人物のリストを取得します。</span><span class="sxs-lookup"><span data-stu-id="2a122-132">Obtain a list of the characters for display.</span></span>

<span data-ttu-id="2a122-133">呼び出し元のコードが、インターフェイスの実装 (`CharacterRepository`) とのみやり取りしていることに注目します。</span><span class="sxs-lookup"><span data-stu-id="2a122-133">Notice how the calling code only interacts with the interface's implementation, `CharacterRepository`.</span></span> <span data-ttu-id="2a122-134">呼び出し元のコードは `ApplicationDbContext` を直接使用しません。</span><span class="sxs-lookup"><span data-stu-id="2a122-134">Calling code doesn't use the `ApplicationDbContext` directly:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Pages/Index.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Controllers/HomeController.cs?name=snippet1)]

::: moniker-end

## <a name="generic-repository-interface"></a><span data-ttu-id="2a122-135">ジェネリック リポジトリ インターフェイス</span><span class="sxs-lookup"><span data-stu-id="2a122-135">Generic repository interface</span></span>

<span data-ttu-id="2a122-136">このトピックとサンプル アプリでは、リポジトリ パターンの最も簡単な実装を紹介しました。そこでは、ビジネス オブジェクトごとに 1 つのリポジトリが作成されました。</span><span class="sxs-lookup"><span data-stu-id="2a122-136">This topic and its sample app demonstrate the simplest implementation of the repository pattern, where one repository is created for each business object.</span></span> <span data-ttu-id="2a122-137">アプリが成長してオブジェクトの数が増えた場合、"*ジェネリック リポジトリ インターフェイス*" を使用することで、リポジトリ パターンを実装するために必要なコードの量を減らすことができる場合があります。</span><span class="sxs-lookup"><span data-stu-id="2a122-137">If the app grows beyond a few objects, a *generic repository interface* may reduce the amount of code required to implement the repository pattern.</span></span> <span data-ttu-id="2a122-138">詳細については、[DevIQ: リポジトリ パターン: ジェネリック リポジトリ インターフェイス](http://deviq.com/repository-pattern/)に関するページをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="2a122-138">For more information, see [DevIQ: Repository Pattern: Generic Repository Interface](http://deviq.com/repository-pattern/).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2a122-139">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="2a122-139">Additional resources</span></span>

* [<span data-ttu-id="2a122-140">DevIQ: リポジトリ パターン</span><span class="sxs-lookup"><span data-stu-id="2a122-140">DevIQ: Repository Pattern</span></span>](https://deviq.com/repository-pattern/)
* [<span data-ttu-id="2a122-141">依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="2a122-141">Dependency injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="2a122-142">ビューへの依存性の注入</span><span class="sxs-lookup"><span data-stu-id="2a122-142">Dependency injection into views</span></span>](xref:mvc/views/dependency-injection)
* [<span data-ttu-id="2a122-143">コントローラーへの依存性の注入</span><span class="sxs-lookup"><span data-stu-id="2a122-143">Dependency injection into controllers</span></span>](xref:mvc/controllers/dependency-injection)
* [<span data-ttu-id="2a122-144">要件ハンドラーでの依存性の注入</span><span class="sxs-lookup"><span data-stu-id="2a122-144">Dependency injection in requirement handlers</span></span>](xref:security/authorization/dependencyinjection)
* [<span data-ttu-id="2a122-145">制御の反転コンテナーと依存関係の挿入パターン</span><span class="sxs-lookup"><span data-stu-id="2a122-145">Inversion of Control Containers and the Dependency Injection Pattern</span></span>](https://www.martinfowler.com/articles/injection.html)
