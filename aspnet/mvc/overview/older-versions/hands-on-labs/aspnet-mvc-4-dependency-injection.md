---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: "ASP.NET MVC 4 の依存関係の挿入 |Microsoft ドキュメント"
author: rick-anderson
description: "注: このハンズオン ラボでは、ASP.NET MVC と ASP.NET MVC 4 のフィルターの基本的な知識がある前提としています。 場合は使用していないする前に、ASP.NET MVC 4 フィルター推奨しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 48a7d7fdb670aebb72450fc4eb12a364ef595c53
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-mvc-4-dependency-injection"></a><span data-ttu-id="dc6cb-104">ASP.NET MVC 4 の依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="dc6cb-104">ASP.NET MVC 4 Dependency Injection</span></span>
====================
<span data-ttu-id="dc6cb-105">によって[Web キャンプ チーム](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="dc6cb-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> [!NOTE]
> <span data-ttu-id="dc6cb-106">このハンズオン ラボは、の基本的な知識がある前提としています。 **ASP.NET MVC**と**ASP.NET MVC 4 フィルター**です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-106">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC** and **ASP.NET MVC 4 filters**.</span></span> <span data-ttu-id="dc6cb-107">使用していない場合**ASP.NET MVC 4 フィルター**経由で移動する前をお勧め**ASP.NET MVC のカスタム アクション フィルター**ハンズオン ラボ。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-107">If you have not used **ASP.NET MVC 4 filters** before, we recommend you to go over **ASP.NET MVC Custom Action Filters** Hands-on Lab.</span></span>
> 
> <span data-ttu-id="dc6cb-108">すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843)です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-108">All sample code and snippets are included in the Web Camps Training Kit, available at [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).</span></span>


<span data-ttu-id="dc6cb-109">**オブジェクト指向プログラミング**パラダイム、オブジェクトが連携コラボレーション モデルでの寄稿者およびコンシューマーがある場所です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-109">In **Object Oriented Programming** paradigm, objects work together in a collaboration model where there are contributors and consumers.</span></span> <span data-ttu-id="dc6cb-110">必然的に、この通信モデルは、オブジェクトおよび複雑さが増すときに、管理するが困難になるコンポーネント間の依存関係を生成します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-110">Naturally, this communication model generates dependencies between objects and components, becoming difficult to manage when complexity increases.</span></span>

<span data-ttu-id="dc6cb-111">![クラスの依存関係と複雑さをモデル化](aspnet-mvc-4-dependency-injection/_static/image1.png "クラスの依存関係と複雑さをモデル化")</span><span class="sxs-lookup"><span data-stu-id="dc6cb-111">![Class dependencies and model complexity](aspnet-mvc-4-dependency-injection/_static/image1.png "Class dependencies and model complexity")</span></span>

<span data-ttu-id="dc6cb-112">*クラスの依存関係とモデルの複雑さ*</span><span class="sxs-lookup"><span data-stu-id="dc6cb-112">*Class dependencies and model complexity*</span></span>

<span data-ttu-id="dc6cb-113">知らされた可能性があります、**ファクトリ パターン**とインターフェイスと、クライアント オブジェクトがある多くの場合、サービスの場所を担当するサービスを使用する実装の間の分離。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-113">You have probably heard about the **Factory Pattern** and the separation between the interface and the implementation using services, where the client objects are often responsible for service location.</span></span>

<span data-ttu-id="dc6cb-114">依存関係の挿入パターンは、制御の反転の特定の実装です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-114">The Dependency Injection pattern is a particular implementation of Inversion of Control.</span></span> <span data-ttu-id="dc6cb-115">**反転 (IoC) のコントロールの**オブジェクトが依存している仕事をするその他のオブジェクトを作成しないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-115">**Inversion of Control (IoC)** means that objects do not create other objects on which they rely to do their work.</span></span> <span data-ttu-id="dc6cb-116">代わりに、外部ソース (たとえば、xml 構成ファイル) に必要なオブジェクトを取得します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-116">Instead, they get the objects that they need from an outside source (for example, an xml configuration file).</span></span>

<span data-ttu-id="dc6cb-117">**依存関係の挿入 (DI)**つまりこれは通常はコンス トラクターのパラメーターを渡す framework コンポーネントによって、オブジェクトの介入なし行われます、プロパティを設定します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-117">**Dependency Injection (DI)** means that this is done without the object intervention, usually by a framework component that passes constructor parameters and set properties.</span></span>

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a><span data-ttu-id="dc6cb-118">依存関係挿入 (DI) デザイン パターン</span><span class="sxs-lookup"><span data-stu-id="dc6cb-118">The Dependency Injection (DI) Design Pattern</span></span>

<span data-ttu-id="dc6cb-119">大まかに言えば、依存関係の挿入の目的は、するクライアント クラス (例: *、ゴルファー*) インターフェイスを満たしているものが必要 (例: *IClub*)。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-119">At a high level, the goal of Dependency Injection is that a client class (e.g. *the golfer*) needs something that satisfies an interface (e.g. *IClub*).</span></span> <span data-ttu-id="dc6cb-120">具象型を気に (例: *WoodClub、IronClub、WedgeClub*または*PutterClub*)、他のユーザーを処理する必要がある (適切な例:*キャディ*)。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-120">It doesn't care what the concrete type is (e.g. *WoodClub, IronClub, WedgeClub* or *PutterClub*), it wants someone else to handle that (e.g. a good *caddy*).</span></span> <span data-ttu-id="dc6cb-121">ASP.NET MVC で依存関係競合回避モジュールは依存関係のロジックを別の場所を登録することを許可することができます (例: コンテナーまたは*クラブのバッグ*)。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-121">The Dependency Resolver in ASP.NET MVC can allow you to register your dependency logic somewhere else (e.g. a container or a *bag of clubs*).</span></span>

<span data-ttu-id="dc6cb-122">![依存関係の挿入ダイアグラム](aspnet-mvc-4-dependency-injection/_static/image2.png "依存関係の挿入の図")</span><span class="sxs-lookup"><span data-stu-id="dc6cb-122">![Dependency Injection diagram](aspnet-mvc-4-dependency-injection/_static/image2.png "Dependency Injection illustration")</span></span>

<span data-ttu-id="dc6cb-123">*依存関係の挿入のゴルフとの類似性*</span><span class="sxs-lookup"><span data-stu-id="dc6cb-123">*Dependency Injection - Golf analogy*</span></span>

<span data-ttu-id="dc6cb-124">次に示します依存性の注入パターンと制御の反転を使用する利点があります。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-124">The advantages of using Dependency Injection pattern and Inversion of Control are the following:</span></span>

- <span data-ttu-id="dc6cb-125">クラス結合度が減少します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-125">Reduces class coupling</span></span>
- <span data-ttu-id="dc6cb-126">コードの再利用を向上します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-126">Increases code reusing</span></span>
- <span data-ttu-id="dc6cb-127">コードの保守性を向上します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-127">Improves code maintainability</span></span>
- <span data-ttu-id="dc6cb-128">アプリケーションのテストが向上します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-128">Improves application testing</span></span>

> [!NOTE]
> <span data-ttu-id="dc6cb-129">依存関係の挿入が抽象のファクトリ デザイン パターンと比較場合もありますが、両方のアプローチのわずかな違いがあります。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-129">Dependency Injection is sometimes compared with Abstract Factory Design Pattern, but there is a slight difference between both approaches.</span></span> <span data-ttu-id="dc6cb-130">DI では、背後にあるが、ファクトリと、登録済みサービスを呼び出すことによって依存関係の解決に取り組んでフレームワークがあります。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-130">DI has a Framework working behind to solve dependencies by calling the factories and the registered services.</span></span>


<span data-ttu-id="dc6cb-131">依存関係挿入パターンを理解する方法について取り上げますこの実習で ASP.NET MVC 4 で適用します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-131">Now that you understand the Dependency Injection Pattern, you will learn throughout this lab how to apply it in ASP.NET MVC 4.</span></span> <span data-ttu-id="dc6cb-132">依存関係の挿入での使用を開始するが、**コント ローラー**データベース アクセス サービスを含めることです。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-132">You will start using Dependency Injection in the **Controllers** to include a database access service.</span></span> <span data-ttu-id="dc6cb-133">依存関係の挿入を適用する次に、**ビュー**サービスを使用すると、情報を表示します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-133">Next, you will apply Dependency Injection to the **Views** to consume a service and show information.</span></span> <span data-ttu-id="dc6cb-134">最後に、ソリューション内のカスタム アクション フィルターを提供する ASP.NET MVC 4 フィルターに、DI が拡張されます。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-134">Finally, you will extend the DI to ASP.NET MVC 4 Filters, injecting a custom action filter in the solution.</span></span>

<span data-ttu-id="dc6cb-135">このハンズオン ラボでは学習する方法。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-135">In this Hands-on Lab, you will learn how to:</span></span>

- <span data-ttu-id="dc6cb-136">依存関係の挿入が NuGet パッケージを使用するために Unity と ASP.NET MVC 4 を統合します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-136">Integrate ASP.NET MVC 4 with Unity for Dependency Injection using NuGet Packages</span></span>
- <span data-ttu-id="dc6cb-137">ASP.NET MVC のコント ローラー内の依存関係の挿入を使用します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-137">Use Dependency Injection inside an ASP.NET MVC Controller</span></span>
- <span data-ttu-id="dc6cb-138">ASP.NET MVC ビュー内の依存関係の挿入を使用します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-138">Use Dependency Injection inside an ASP.NET MVC View</span></span>
- <span data-ttu-id="dc6cb-139">ASP.NET MVC アクション フィルター内の依存関係の挿入を使用します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-139">Use Dependency Injection inside an ASP.NET MVC Action Filter</span></span>

> [!NOTE]
> <span data-ttu-id="dc6cb-140">このラボが Unity.Mvc3 NuGet パッケージを使用して、依存関係の解決ですが、ASP.NET MVC 4 を使用する任意の依存関係挿入フレームワークを適用すること。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-140">This Lab is using Unity.Mvc3 NuGet Package for dependency resolution, but it is possible to adapt any Dependency Injection Framework to work with ASP.NET MVC 4.</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="dc6cb-141">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="dc6cb-141">Prerequisites</span></span>

<span data-ttu-id="dc6cb-142">このラボを完成させるのには、次の項目が必要です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-142">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="dc6cb-143">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)または上位 (読み取り[付録 A](#AppendixA)をインストールする方法について)。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-143">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="dc6cb-144">セットアップ</span><span class="sxs-lookup"><span data-stu-id="dc6cb-144">Setup</span></span>

<span data-ttu-id="dc6cb-145">**コード スニペットをインストールします。**</span><span class="sxs-lookup"><span data-stu-id="dc6cb-145">**Installing Code Snippets**</span></span>

<span data-ttu-id="dc6cb-146">便宜上、このラボに沿ったを管理するコードの多くは、Visual Studio のコード スニペットとして利用できます。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-146">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="dc6cb-147">実行のコード スニペットをインストールする**.\Source\Setup\CodeSnippets.vsi**ファイル。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-147">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="dc6cb-148">このドキュメントの付録を参照することができます、Visual Studio のコード スニペットとその使用方法を学習するに慣れていない場合&quot;[付録 b: を使用してコード スニペット](#AppendixB)&quot;です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-148">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="dc6cb-149">演習</span><span class="sxs-lookup"><span data-stu-id="dc6cb-149">Exercises</span></span>

<span data-ttu-id="dc6cb-150">次の演習では、このハンズオン ラボから成ります。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-150">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="dc6cb-151">手順 1: コント ローラーの挿入</span><span class="sxs-lookup"><span data-stu-id="dc6cb-151">Exercise 1: Injecting a Controller</span></span>](#Exercise1)
2. [<span data-ttu-id="dc6cb-152">手順 2: ビューを挿入します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-152">Exercise 2: Injecting a View</span></span>](#Exercise2)
3. [<span data-ttu-id="dc6cb-153">手順 3: フィルターを提供します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-153">Exercise 3: Injecting Filters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="dc6cb-154">各演習が組み込まれた、**終了**演習を完了した後に取得する必要があります、結果として得られるソリューションを含むフォルダー。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-154">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="dc6cb-155">演習では操作のヘルプを参照する必要がある場合は、このソリューションをガイドとして使用できます。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-155">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="dc6cb-156">この演習を完了する時間を推定: **30 分**です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-156">Estimated time to complete this lab: **30 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a><span data-ttu-id="dc6cb-157">手順 1: コント ローラーの挿入</span><span class="sxs-lookup"><span data-stu-id="dc6cb-157">Exercise 1: Injecting a Controller</span></span>

<span data-ttu-id="dc6cb-158">この演習では、NuGet パッケージを使用する Unity の統合により、ASP.NET MVC のコント ローラーで依存関係の挿入を使用する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-158">In this exercise, you will learn how to use Dependency Injection in ASP.NET MVC Controllers by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="dc6cb-159">そのため、サービスをデータ アクセス ロジックを区別するための MvcMusicStore コント ローラーに含まれます。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-159">For that reason, you will include services into your MvcMusicStore controllers to separate the logic from the data access.</span></span> <span data-ttu-id="dc6cb-160">サービスは、コント ローラーのコンス トラクターでのヘルプで依存関係の挿入を使用して決定される新しい依存関係を作成**Unity**です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-160">The services will create a new dependency in the controller constructor, which will be resolved using Dependency Injection with the help of **Unity**.</span></span>

<span data-ttu-id="dc6cb-161">この方法では、小さい結合であるアプリケーションより柔軟かつ容易に維持し、テストを生成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-161">This approach will show you how to generate less coupled applications, which are more flexible and easier to maintain and test.</span></span> <span data-ttu-id="dc6cb-162">ASP.NET MVC を Unity に統合する方法も学習します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-162">You will also learn how to integrate ASP.NET MVC with Unity.</span></span>

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a><span data-ttu-id="dc6cb-163">StoreManager サービスについて</span><span class="sxs-lookup"><span data-stu-id="dc6cb-163">About StoreManager Service</span></span>

<span data-ttu-id="dc6cb-164">今すぐ開始ソリューションで提供されている MVC 音楽ストアには、という名前のストアのコント ローラー データを管理するサービスが含まれています。 **StoreService**です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-164">The MVC Music Store provided in the begin solution now includes a service that manages the Store Controller data named **StoreService**.</span></span> <span data-ttu-id="dc6cb-165">次に示す、ストア サービスの実装が表示されます。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-165">Below you will find the Store Service implementation.</span></span> <span data-ttu-id="dc6cb-166">すべてのメソッドがモデルのエンティティを返すことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-166">Note that all the methods return Model entities.</span></span>

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

<span data-ttu-id="dc6cb-167">**StoreController**ソリューション、begin から消費今すぐ**StoreService**です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-167">**StoreController** from the begin solution now consumes **StoreService**.</span></span> <span data-ttu-id="dc6cb-168">すべてのデータの参照が削除された**StoreController**を使用する任意の方法を変更することがなく、現在のデータ アクセス プロバイダーを変更するようになりましたことも、 **StoreService**です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-168">All the data references were removed from **StoreController**, and now possible to modify the current data access provider without changing any method that consumes **StoreService**.</span></span>

<span data-ttu-id="dc6cb-169">次に表示されます、 **StoreController**実装との依存関係を持つ**StoreService**クラスのコンス トラクター内です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-169">You will find below that the **StoreController** implementation has a dependency with **StoreService** inside the class constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="dc6cb-170">この演習で導入された依存関係に関連する**制御の反転**(IoC)。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-170">The dependency introduced in this exercise is related to **Inversion of Control** (IoC).</span></span>
> 
> <span data-ttu-id="dc6cb-171">**StoreController**クラスのコンス トラクターを受け取る、 **IStoreService**型のパラメーターは、クラス内からサービスの呼び出しを実行することは不可欠です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-171">The **StoreController** class constructor receives an **IStoreService** type parameter, which is essential to perform service calls from inside the class.</span></span> <span data-ttu-id="dc6cb-172">ただし、 **StoreController**は任意のコント ローラーが必要な ASP.NET MVC を使用する (パラメーターなし) での既定コンス トラクターを実装していません。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-172">However, **StoreController** does not implement the default constructor (with no parameters) that any controller must have to work with ASP.NET MVC.</span></span>
> 
> <span data-ttu-id="dc6cb-173">依存関係を解決するには、コント ローラーは、抽象ファクトリ (を指定した型の任意のオブジェクトを返すクラス) によって作成されるがします。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-173">To resolve the dependency, the controller has to be created by an abstract factory (a class that returns any object of the specified type).</span></span>


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="dc6cb-174">クラスが宣言されているパラメーターなしのコンス トラクターが存在しないために、サービス オブジェクトを送信せず、StoreController を作成しようとするときにエラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-174">You will get an error when the class tries to create the StoreController without sending the service object, as there is no parameterless constructor declared.</span></span>


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a><span data-ttu-id="dc6cb-175">タスク 1 - アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-175">Task 1 - Running the Application</span></span>

<span data-ttu-id="dc6cb-176">このタスクには、サービスを含む、アプリケーション ロジックからデータ アクセスを分離するストア コント ローラーに Begin アプリケーションが実行されます。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-176">In this task, you will run the Begin application, which includes the service into the Store Controller that separates the data access from the application logic.</span></span>

<span data-ttu-id="dc6cb-177">アプリケーションを実行するときに、コント ローラー サービスが既定でのパラメーターとして渡されないと、例外が表示されます。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-177">When running the application, you will receive an exception, as the controller service is not passed as a parameter by default:</span></span>

1. <span data-ttu-id="dc6cb-178">開く、**開始**にソリューションがある**Source\Ex01 送り込んで Controller\Begin**です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-178">Open the **Begin** solution located in **Source\Ex01-Injecting Controller\Begin**.</span></span>

    1. <span data-ttu-id="dc6cb-179">いくつか不足している NuGet パッケージをダウンロードする必要がありますが続行前にします。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-179">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="dc6cb-180">これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-180">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="dc6cb-181">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-181">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="dc6cb-182">最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-182">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="dc6cb-183">NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-183">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="dc6cb-184">NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-184">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="dc6cb-185">これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-185">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="dc6cb-186">キーを押して**ctrl キーを押しながら f5 キーを押して**デバッグを使わないアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-186">Press **Ctrl + F5** to run the application without debugging.</span></span> <span data-ttu-id="dc6cb-187">エラー メッセージが表示されます&quot;**パラメーターなしのコンス トラクターはこのオブジェクトに対して定義されている**&quot;:</span><span class="sxs-lookup"><span data-stu-id="dc6cb-187">You will get the error message &quot;**No parameterless constructor defined for this object**&quot;:</span></span>

    <span data-ttu-id="dc6cb-188">![ASP.NET MVC 開始アプリケーションの実行中にエラー](aspnet-mvc-4-dependency-injection/_static/image3.png "ASP.NET MVC 開始アプリケーションの実行中にエラー")</span><span class="sxs-lookup"><span data-stu-id="dc6cb-188">![Error while running ASP.NET MVC Begin Application](aspnet-mvc-4-dependency-injection/_static/image3.png "Error while running ASP.NET MVC Begin Application")</span></span>

    <span data-ttu-id="dc6cb-189">*ASP.NET MVC 開始アプリケーションの実行中にエラー*</span><span class="sxs-lookup"><span data-stu-id="dc6cb-189">*Error while running ASP.NET MVC Begin Application*</span></span>
3. <span data-ttu-id="dc6cb-190">ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-190">Close the browser.</span></span>

<span data-ttu-id="dc6cb-191">次の手順では、このコント ローラーが必要な依存関係を挿入する音楽ストア ソリューションで動作します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-191">In the following steps you will work on the Music Store Solution to inject the dependency this controller needs.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a><span data-ttu-id="dc6cb-192">タスク 2 - MvcMusicStore ソリューションに Unity を含む</span><span class="sxs-lookup"><span data-stu-id="dc6cb-192">Task 2 - Including Unity into MvcMusicStore Solution</span></span>

<span data-ttu-id="dc6cb-193">このタスクでに含めます**Unity.Mvc3** NuGet パッケージをソリューションにします。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-193">In this task, you will include **Unity.Mvc3** NuGet Package to the solution.</span></span>

> [!NOTE]
> <span data-ttu-id="dc6cb-194">ASP.NET MVC 3 の Unity.Mvc3 パッケージの設計ですが、ASP.NET MVC 4 と完全な互換性。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-194">Unity.Mvc3 package was designed for ASP.NET MVC 3, but it is fully compatible with ASP.NET MVC 4.</span></span>
> 
> <span data-ttu-id="dc6cb-195">Unity では、インスタンスのサポートが省略可能な軽量で拡張性のある依存性の注入コンテナーは、し、インターセプションを入力します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-195">Unity is a lightweight, extensible dependency injection container with optional support for instance and type interception.</span></span> <span data-ttu-id="dc6cb-196">これは、あらゆる種類の .NET アプリケーションで使用する汎用的なコンテナーです。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-196">It is a general-purpose container for use in any type of .NET application.</span></span> <span data-ttu-id="dc6cb-197">などの依存関係の挿入メカニズムで見つかったすべての一般的な機能を提供します。 オブジェクトの作成、コンテナーにコンポーネントの構成を延期すると、ランタイムと、柔軟性の依存関係を指定することによって要件の抽象化します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-197">It provides all the common features found in dependency injection mechanisms including: object creation, abstraction of requirements by specifying dependencies at runtime and flexibility, by deferring the component configuration to the container.</span></span>


1. <span data-ttu-id="dc6cb-198">インストール**Unity.Mvc3**での NuGet パッケージ、 **MvcMusicStore**プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-198">Install **Unity.Mvc3** NuGet Package in the **MvcMusicStore** project.</span></span> <span data-ttu-id="dc6cb-199">これを行うには、開く、 **Package Manager Console**から**ビュー** | **その他のウィンドウ**します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-199">To do this, open the **Package Manager Console** from **View** | **Other Windows**.</span></span>
2. <span data-ttu-id="dc6cb-200">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-200">Run the following command.</span></span>

    <span data-ttu-id="dc6cb-201">PMC</span><span class="sxs-lookup"><span data-stu-id="dc6cb-201">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    <span data-ttu-id="dc6cb-202">![Unity.Mvc3 NuGet パッケージをインストールする](aspnet-mvc-4-dependency-injection/_static/image4.png "Unity.Mvc3 NuGet パッケージをインストールします。")</span><span class="sxs-lookup"><span data-stu-id="dc6cb-202">![Installing Unity.Mvc3 NuGet Package](aspnet-mvc-4-dependency-injection/_static/image4.png "Installing Unity.Mvc3 NuGet Package")</span></span>

    <span data-ttu-id="dc6cb-203">*Unity.Mvc3 NuGet パッケージをインストールします。*</span><span class="sxs-lookup"><span data-stu-id="dc6cb-203">*Installing Unity.Mvc3 NuGet Package*</span></span>
3. <span data-ttu-id="dc6cb-204">1 回、 **Unity.Mvc3**パッケージがインストールされている、ファイルおよび Unity の構成を簡略化するために自動的に追加フォルダーを表示します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-204">Once the **Unity.Mvc3** package is installed, explore the files and folders it automatically adds in order to simplify Unity configuration.</span></span>

    <span data-ttu-id="dc6cb-205">![インストールされている Unity.Mvc3 パッケージ](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 パッケージがインストールされています。")</span><span class="sxs-lookup"><span data-stu-id="dc6cb-205">![Unity.Mvc3 package installed](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 package installed")</span></span>

    <span data-ttu-id="dc6cb-206">*Unity.Mvc3 パッケージがインストールされています。*</span><span class="sxs-lookup"><span data-stu-id="dc6cb-206">*Unity.Mvc3 package installed*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a><span data-ttu-id="dc6cb-207">タスク 3 - Global.asax.cs アプリケーションで登録する Unity\_開始</span><span class="sxs-lookup"><span data-stu-id="dc6cb-207">Task 3 - Registering Unity in Global.asax.cs Application\_Start</span></span>

<span data-ttu-id="dc6cb-208">このタスクを更新、**アプリケーション\_開始**にメソッドがある**Global.asax.cs** Unity ブートス トラップ初期化子を呼び出すし、次に、ブートス トラップ ファイルの登録を更新するにはサービスとコント ローラーは、依存関係の挿入に使用されます。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-208">In this task, you will update the **Application\_Start** method located in **Global.asax.cs** to call the Unity Bootstrapper initializer and then, update the Bootstrapper file registering the Service and Controller you will use for Dependency Injection.</span></span>

1. <span data-ttu-id="dc6cb-209">ここで、これは、Unity のコンテナーを初期化するファイル ブートス トラップと依存関係競合回避モジュールをフックするされます。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-209">Now, you will hook up the Bootstrapper which is the file that initializes the Unity container and Dependency Resolver.</span></span> <span data-ttu-id="dc6cb-210">これを行うには、開く**Global.asax.cs**内で強調表示されている次のコードを追加し、**アプリケーション\_開始**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-210">To do this, open **Global.asax.cs** and add the following highlighted code within the **Application\_Start** method.</span></span>

    <span data-ttu-id="dc6cb-211">(コード スニペットの*ASP.NET 依存関係の挿入ラボ - Ex01 - 初期化 Unity*)</span><span class="sxs-lookup"><span data-stu-id="dc6cb-211">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Initialize Unity*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. <span data-ttu-id="dc6cb-212">開いている**Bootstrapper.cs**ファイル。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-212">Open **Bootstrapper.cs** file.</span></span>
3. <span data-ttu-id="dc6cb-213">次の名前空間を含める: **MvcMusicStore.Services**と**MusicStore.Controllers**です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-213">Include the following namespaces: **MvcMusicStore.Services** and **MusicStore.Controllers**.</span></span>

    <span data-ttu-id="dc6cb-214">(コード スニペットの*ASP.NET 依存関係の挿入ラボ - Ex01 - ブートス トラップの名前空間を追加する*)</span><span class="sxs-lookup"><span data-stu-id="dc6cb-214">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. <span data-ttu-id="dc6cb-215">置き換える**BuildUnityContainer**メソッドのコンテンツ ストア コント ローラーとストア サービスを登録する次のコードにします。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-215">Replace **BuildUnityContainer** method's content with the following code that registers Store Controller and Store Service.</span></span>

    <span data-ttu-id="dc6cb-216">(コード スニペットの*ASP.NET 依存関係の挿入ラボ - Ex01 - 登録ストア コント ローラーとサービス*)</span><span class="sxs-lookup"><span data-stu-id="dc6cb-216">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Register Store Controller and Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="dc6cb-217">タスク 4 - アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-217">Task 4 - Running the Application</span></span>

<span data-ttu-id="dc6cb-218">このタスクを確認するようになりました読み込むことができます Unity を含めた後でアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-218">In this task, you will run the application to verify that it can now be loaded after including Unity.</span></span>

1. <span data-ttu-id="dc6cb-219">キーを押して**f5 キーを押して**アプリケーションを実行する、アプリケーションは、エラー メッセージを表示することがなく読み込むようになりました必要があります。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-219">Press **F5** to run the application, the application should now load without showing any error message.</span></span>

    <span data-ttu-id="dc6cb-220">![依存関係の挿入でアプリケーションを実行している](aspnet-mvc-4-dependency-injection/_static/image6.png "アプリケーション依存関係の挿入を実行")</span><span class="sxs-lookup"><span data-stu-id="dc6cb-220">![Running Application with Dependency Injection](aspnet-mvc-4-dependency-injection/_static/image6.png "Running Application with Dependency Injection")</span></span>

    <span data-ttu-id="dc6cb-221">*依存関係の挿入を実行中のアプリケーション*</span><span class="sxs-lookup"><span data-stu-id="dc6cb-221">*Running Application with Dependency Injection*</span></span>
2. <span data-ttu-id="dc6cb-222">参照**/格納**です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-222">Browse to **/Store**.</span></span> <span data-ttu-id="dc6cb-223">これを呼び出す**StoreController**を使用してこれは今すぐ作成**Unity**です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-223">This will invoke **StoreController**, which is now created using **Unity**.</span></span>

    <span data-ttu-id="dc6cb-224">![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC 音楽ストア")</span><span class="sxs-lookup"><span data-stu-id="dc6cb-224">![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")</span></span>

    <span data-ttu-id="dc6cb-225">*MVC 音楽ストア*</span><span class="sxs-lookup"><span data-stu-id="dc6cb-225">*MVC Music Store*</span></span>
3. <span data-ttu-id="dc6cb-226">ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-226">Close the browser.</span></span>

<span data-ttu-id="dc6cb-227">次の演習では、ASP.NET MVC ビューとアクション フィルター内で使用する依存関係の挿入のスコープを拡張する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-227">In the following exercises you will learn how to extend the Dependency Injection scope to use it inside ASP.NET MVC Views and Action Filters.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a><span data-ttu-id="dc6cb-228">手順 2: ビューを挿入します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-228">Exercise 2: Injecting a View</span></span>

<span data-ttu-id="dc6cb-229">この演習では、Unity の統合を ASP.NET MVC 4 の新機能を含むビューの依存関係の挿入を使用する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-229">In this exercise, you will learn how to use Dependency Injection in a view with the new features of ASP.NET MVC 4 for Unity integration.</span></span> <span data-ttu-id="dc6cb-230">これを実行するためには、内部、ストア ビューの参照をメッセージと下の画像を表示するカスタム サービスを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-230">In order to do that, you will call a custom service inside the Store Browse View, which will show a message and an image below.</span></span>

<span data-ttu-id="dc6cb-231">次に、unity プロジェクトを統合し、依存関係を挿入するカスタムの依存関係リゾルバーを作成します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-231">Then, you will integrate the project with Unity and create a custom dependency resolver to inject the dependencies.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a><span data-ttu-id="dc6cb-232">タスク 1 - サービスを使用するビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-232">Task 1 - Creating a View that Consumes a Service</span></span>

<span data-ttu-id="dc6cb-233">このタスクでは、新しい依存関係を生成するサービスの呼び出しを実行するビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-233">In this task, you will create a view that performs a service call to generate a new dependency.</span></span> <span data-ttu-id="dc6cb-234">このソリューションに含まれる単純なメッセージング サービスで、サービスが構成されます。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-234">The service consists in a simple messaging service included in this solution.</span></span>

1. <span data-ttu-id="dc6cb-235">開く、**開始**にソリューションがある、 **Source\Ex02 送り込んで View\Begin**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-235">Open the **Begin** solution located in the **Source\Ex02-Injecting View\Begin** folder.</span></span> <span data-ttu-id="dc6cb-236">それ以外の場合、作業を続行できますを使用して、**終了**ソリューションは、前の手順を完了して取得します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-236">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="dc6cb-237">指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-237">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="dc6cb-238">これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-238">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="dc6cb-239">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-239">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="dc6cb-240">最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-240">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="dc6cb-241">NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-241">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="dc6cb-242">NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-242">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="dc6cb-243">これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-243">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
    > 
    > <span data-ttu-id="dc6cb-244">詳細については、この記事を参照してください: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages)です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-244">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="dc6cb-245">含める、 **MessageService.cs**と**IMessageService.cs**クラスに格納、 **\Assets をソース**フォルダーに**/サービス**です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-245">Include the **MessageService.cs** and the **IMessageService.cs** classes located in the **Source \Assets** folder in **/Services**.</span></span> <span data-ttu-id="dc6cb-246">これを行うを右クリックして**Services**フォルダーと選択**既存項目の追加**です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-246">To do this, right-click **Services** folder and select **Add Existing Item**.</span></span> <span data-ttu-id="dc6cb-247">ファイルの場所を参照し、それらを含めます。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-247">Browse to the files' location and include them.</span></span>

    <span data-ttu-id="dc6cb-248">![メッセージのサービスとサービスのインターフェイスを追加する](aspnet-mvc-4-dependency-injection/_static/image8.png "メッセージ サービスとサービスのインターフェイスを追加します。")</span><span class="sxs-lookup"><span data-stu-id="dc6cb-248">![Adding Message Service and Service Interface](aspnet-mvc-4-dependency-injection/_static/image8.png "Adding Message Service and Service Interface")</span></span>

    <span data-ttu-id="dc6cb-249">*サービス インターフェイスと追加のメッセージ サービス*</span><span class="sxs-lookup"><span data-stu-id="dc6cb-249">*Adding Message Service and Service Interface*</span></span>

    > [!NOTE]
    > <span data-ttu-id="dc6cb-250">**IMessageService**インターフェイスによって実装される 2 つのプロパティを定義する、 **MessageService**クラスです。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-250">The **IMessageService** interface defines two properties implemented by the **MessageService** class.</span></span> <span data-ttu-id="dc6cb-251">これらのプロパティ -**メッセージ**と**ImageUrl**-を表示するには、メッセージとイメージの URL を格納します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-251">These properties -**Message** and **ImageUrl**- store the message and the URL of the image to be displayed.</span></span>
3. <span data-ttu-id="dc6cb-252">フォルダーを作成して**ページ/**プロジェクトのルート フォルダー、および既存のクラスを追加**MyBasePage.cs**から**Source\Assets**です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-252">Create the folder **/Pages** in the project's root folder, and then add the existing class **MyBasePage.cs** from **Source\Assets**.</span></span> <span data-ttu-id="dc6cb-253">継承する基本ページには、次のような構造があります。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-253">The base page you will inherit from has the following structure.</span></span>

    <span data-ttu-id="dc6cb-254">![ページ フォルダー](aspnet-mvc-4-dependency-injection/_static/image9.png "ページ フォルダー")</span><span class="sxs-lookup"><span data-stu-id="dc6cb-254">![Pages folder](aspnet-mvc-4-dependency-injection/_static/image9.png "Pages folder")</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. <span data-ttu-id="dc6cb-255">開いている**Browse.cshtml**から表示**/ビュー/ストア**フォルダーから継承して**MyBasePage.cs**です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-255">Open **Browse.cshtml** view from **/Views/Store** folder, and make it inherit from **MyBasePage.cs**.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. <span data-ttu-id="dc6cb-256">**参照**ビューで、呼び出しを追加して**MessageService**イメージと、サービスによって取得したメッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-256">In the **Browse** view, add a call to **MessageService** to display an image and a message retrieved by the service.</span></span>
<span data-ttu-id="dc6cb-257">(C#)</span><span class="sxs-lookup"><span data-stu-id="dc6cb-257">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a><span data-ttu-id="dc6cb-258">タスク 2 - カスタム依存関係競合回避モジュールとカスタム ビュー ページ アクティベーターを含む</span><span class="sxs-lookup"><span data-stu-id="dc6cb-258">Task 2 - Including a Custom Dependency Resolver and a Custom View Page Activator</span></span>

<span data-ttu-id="dc6cb-259">前のタスクでは、その中のサービス呼び出しを実行するためのビュー内の新しい依存関係を挿入します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-259">In the previous task, you injected a new dependency inside a view to perform a service call inside it.</span></span> <span data-ttu-id="dc6cb-260">ASP.NET MVC 依存性の注入インターフェイスを実装する依存関係を解決するようになりました、 **IViewPageActivator**と**IDependencyResolver**です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-260">Now, you will resolve that dependency by implementing the ASP.NET MVC Dependency Injection interfaces **IViewPageActivator** and **IDependencyResolver**.</span></span> <span data-ttu-id="dc6cb-261">実装ソリューションに含めるは**IDependencyResolver** Unity を使用して、サービスの取得を扱うことができます。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-261">You will include in the solution an implementation of **IDependencyResolver** that will deal with the service retrieval by using Unity.</span></span> <span data-ttu-id="dc6cb-262">次の別のカスタム実装を含めますが**IViewPageActivator**インターフェイス ビューの作成を解決するためです。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-262">Then, you will include another custom implementation of **IViewPageActivator** interface that will solve the creation of the views.</span></span>

> [!NOTE]
> <span data-ttu-id="dc6cb-263">ASP.NET MVC 3 から依存関係の挿入の実装にサービスを登録するインターフェイスが簡略化されます。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-263">Since ASP.NET MVC 3, the implementation for Dependency Injection had simplified the interfaces to register services.</span></span> <span data-ttu-id="dc6cb-264">**IDependencyResolver**と**IViewPageActivator**依存関係の挿入の ASP.NET MVC 3 の機能の一部であります。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-264">**IDependencyResolver** and **IViewPageActivator** are part of ASP.NET MVC 3 features for Dependency Injection.</span></span>
> 
> <span data-ttu-id="dc6cb-265">**-IDependencyResolver**インターフェイスには、以前の IMvcServiceLocator が置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-265">**- IDependencyResolver** interface replaces the previous IMvcServiceLocator.</span></span> <span data-ttu-id="dc6cb-266">IDependencyResolver の実装では、サービスまたはサービスのコレクションのインスタンスを返す必要があります。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-266">Implementers of IDependencyResolver must return an instance of the service or a service collection.</span></span>
>
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> <span data-ttu-id="dc6cb-267">**-IViewPageActivator**インターフェイスには、依存関係の挿入を使用してページの表示がインスタンス化する方法より詳細に制御が用意されています。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-267">**- IViewPageActivator** interface provides more fine-grained control over how view pages are instantiated via dependency injection.</span></span> <span data-ttu-id="dc6cb-268">実装するクラス**IViewPageActivator**インターフェイスは、コンテキスト情報を使用してビューのインスタンスを作成できます。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-268">The classes that implement **IViewPageActivator** interface can create view instances using context information.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. <span data-ttu-id="dc6cb-269">作成、/**ファクトリ**プロジェクトのルート フォルダー内のフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-269">Create the /**Factories** folder in the project's root folder.</span></span>
2. <span data-ttu-id="dc6cb-270">含める**CustomViewPageActivator.cs**からソリューションに**ソース資産//**に**ファクトリ**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-270">Include **CustomViewPageActivator.cs** to your solution from **/Sources/Assets/** to **Factories** folder.</span></span> <span data-ttu-id="dc6cb-271">右クリックし、 **/Factories**フォルダーを選択**追加 |既存の項目**し、 **CustomViewPageActivator.cs**です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-271">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **CustomViewPageActivator.cs**.</span></span> <span data-ttu-id="dc6cb-272">このクラスは、実装、 **IViewPageActivator** Unity コンテナーを保持するインターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-272">This class implements the **IViewPageActivator** interface to hold the Unity Container.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > <span data-ttu-id="dc6cb-273">**CustomViewPageActivator** Unity コンテナーを使用して、ビューの作成の管理を担当します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-273">**CustomViewPageActivator** is responsible for managing the creation of a view by using a Unity container.</span></span>
3. <span data-ttu-id="dc6cb-274">含める**UnityDependencyResolver.cs**ファイルから**ソース/資産**に**/Factories**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-274">Include **UnityDependencyResolver.cs** file from **/Sources/Assets** to **/Factories** folder.</span></span> <span data-ttu-id="dc6cb-275">右クリックし、 **/Factories**フォルダーを選択**追加 |既存の項目**し、 **UnityDependencyResolver.cs**ファイル。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-275">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **UnityDependencyResolver.cs** file.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > <span data-ttu-id="dc6cb-276">**UnityDependencyResolver**クラスは、Unity のカスタム DependencyResolver です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-276">**UnityDependencyResolver** class is a custom DependencyResolver for Unity.</span></span> <span data-ttu-id="dc6cb-277">Unity コンテナー内のサービスが見つからない場合は、ベースの競合回避モジュールが invocated です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-277">When a service cannot be found inside the Unity container, the base resolver is invocated.</span></span>

<span data-ttu-id="dc6cb-278">次の実習で両方の実装は、サービスと、ビューの場所を知っているモデルに登録されます。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-278">In the following task both implementations will be registered to let the model know the location of the services and the views.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a><span data-ttu-id="dc6cb-279">タスク 3 - Unity コンテナー内の依存関係の挿入の登録</span><span class="sxs-lookup"><span data-stu-id="dc6cb-279">Task 3 - Registering for Dependency Injection within Unity container</span></span>

<span data-ttu-id="dc6cb-280">このタスクでは、依存関係の挿入操作にまとめる前、すべての処理を格納します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-280">In this task, you will put all the previous things together to make Dependency Injection work.</span></span>

<span data-ttu-id="dc6cb-281">これまでは、ソリューションは、次の要素に。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-281">Up to now your solution has the following elements:</span></span>

- <span data-ttu-id="dc6cb-282">A**参照**ビューから継承する**MyBaseClass**消費**MessageService**です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-282">A **Browse** View that inherits from **MyBaseClass** and consumes **MessageService**.</span></span>
- <span data-ttu-id="dc6cb-283">中間クラス -**MyBaseClass**-依存関係の挿入、サービス インターフェイスの宣言を含むです。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-283">An intermediate class -**MyBaseClass**- that has dependency injection declared for the service interface.</span></span>
- <span data-ttu-id="dc6cb-284">サービス - **MessageService** - とそのインターフェイス**IMessageService**です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-284">A service - **MessageService** - and its interface **IMessageService**.</span></span>
- <span data-ttu-id="dc6cb-285">Unity でのカスタムの依存関係リゾルバー **UnityDependencyResolver** -サービスの取得を処理します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-285">A custom dependency resolver for Unity - **UnityDependencyResolver** - that deals with service retrieval.</span></span>
- <span data-ttu-id="dc6cb-286">ビュー ページ アクティベーター - **CustomViewPageActivator**のページを作成します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-286">A View Page activator - **CustomViewPageActivator** - that creates the page.</span></span>

<span data-ttu-id="dc6cb-287">挿入する**参照**ビュー、今すぐ登録する、カスタムの依存関係競合回避モジュール Unity コンテナーにします。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-287">To inject **Browse** View, you will now register the custom dependency resolver in the Unity container.</span></span>

1. <span data-ttu-id="dc6cb-288">開いている**Bootstrapper.cs**ファイル。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-288">Open **Bootstrapper.cs** file.</span></span>
2. <span data-ttu-id="dc6cb-289">インスタンスを登録**MessageService**サービスを初期化するために Unity コンテナーに。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-289">Register an instance of **MessageService** into the Unity container to initialize the service:</span></span>

    <span data-ttu-id="dc6cb-290">(コード スニペットの*ASP.NET 依存関係の挿入ラボ - Ex02 - 登録メッセージ サービス*)</span><span class="sxs-lookup"><span data-stu-id="dc6cb-290">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register Message Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. <span data-ttu-id="dc6cb-291">参照を追加**MvcMusicStore.Factories**名前空間。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-291">Add a reference to **MvcMusicStore.Factories** namespace.</span></span>

    <span data-ttu-id="dc6cb-292">(コード スニペットの*ASP.NET 依存関係の挿入ラボ - Ex02 - ファクトリ Namespace*)</span><span class="sxs-lookup"><span data-stu-id="dc6cb-292">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Factories Namespace*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. <span data-ttu-id="dc6cb-293">登録**CustomViewPageActivator**として Unity コンテナーにビュー ページ アクティベーター。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-293">Register **CustomViewPageActivator** as a View Page activator into the Unity container:</span></span>

    <span data-ttu-id="dc6cb-294">(コード スニペットの*ASP.NET 依存関係の挿入ラボ - Ex02 - 登録 CustomViewPageActivator*)</span><span class="sxs-lookup"><span data-stu-id="dc6cb-294">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register CustomViewPageActivator*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. <span data-ttu-id="dc6cb-295">インスタンスと ASP.NET MVC 4 の既定の依存関係競合回避モジュールを交換して**UnityDependencyResolver**です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-295">Replace ASP.NET MVC 4 default dependency resolver with an instance of **UnityDependencyResolver**.</span></span> <span data-ttu-id="dc6cb-296">これを行うには、置換**Initialise**メソッドを次のコード コンテンツ。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-296">To do this, replace **Initialise** method content with the following code:</span></span>

    <span data-ttu-id="dc6cb-297">(コード スニペットの*ASP.NET の依存関係インジェクション ラボ - Ex02 - 更新プログラムの依存関係競合回避モジュール*)</span><span class="sxs-lookup"><span data-stu-id="dc6cb-297">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Update Dependency Resolver*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > <span data-ttu-id="dc6cb-298">ASP.NET MVC には、既定の依存関係競合回避モジュールのクラスが用意されています。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-298">ASP.NET MVC provides a default dependency resolver class.</span></span> <span data-ttu-id="dc6cb-299">Unity に対して作成したものと、カスタムの依存関係競合回避モジュールを操作するには、この競合回避モジュールは、置換するがします。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-299">To work with custom dependency resolvers as the one we have created for unity, this resolver has to be replaced.</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="dc6cb-300">タスク 4 - アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-300">Task 4 - Running the Application</span></span>

<span data-ttu-id="dc6cb-301">このタスクでは、ことを確認ストア ブラウザー サービスを使用し、イメージと取得したメッセージを示しています。 アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-301">In this task, you will run the application to verify that the Store Browser consumes the service and shows the image and the message retrieved:</span></span>

1. <span data-ttu-id="dc6cb-302">**F5** キーを押してアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-302">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="dc6cb-303">をクリックして**ロック**を参照し、ジャンル メニュー内でどのように**MessageService**ビューに挿入された、ようこそメッセージと、イメージを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-303">Click **Rock** within the Genres Menu and see how the **MessageService** was injected to the view and loaded the welcome message and the image.</span></span> <span data-ttu-id="dc6cb-304">この例では入力する&quot;**ロック**&quot;:</span><span class="sxs-lookup"><span data-stu-id="dc6cb-304">In this example, we are entering to &quot;**Rock**&quot;:</span></span>

    <span data-ttu-id="dc6cb-305">![MVC 音楽ストア - ビュー インジェクション](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC 音楽ストア - ビュー インジェクション")</span><span class="sxs-lookup"><span data-stu-id="dc6cb-305">![MVC Music Store - View Injection](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - View Injection")</span></span>

    <span data-ttu-id="dc6cb-306">*MVC 音楽ストア - ビュー インジェクション*</span><span class="sxs-lookup"><span data-stu-id="dc6cb-306">*MVC Music Store - View Injection*</span></span>
3. <span data-ttu-id="dc6cb-307">ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-307">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a><span data-ttu-id="dc6cb-308">手順 3: 挿入アクション フィルター</span><span class="sxs-lookup"><span data-stu-id="dc6cb-308">Exercise 3: Injecting Action Filters</span></span>

<span data-ttu-id="dc6cb-309">前の実習で**カスタム アクション フィルター**フィルター カスタマイズおよびインジェクションで作業しました。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-309">In the previous Hands-On lab **Custom Action Filters** you have worked with filters customization and injection.</span></span> <span data-ttu-id="dc6cb-310">この演習では、依存関係の挿入と Unity のコンテナーを使用してフィルターを挿入する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-310">In this exercise, you will learn how to inject filters with Dependency Injection by using the Unity container.</span></span> <span data-ttu-id="dc6cb-311">実行するにはソリューションに追加する、音楽ストア、サイトの利用状況を追跡するカスタム アクション フィルター。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-311">To do that, you will add to the Music Store solution a custom action filter that will trace the activity of the site.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a><span data-ttu-id="dc6cb-312">タスク 1: ソリューション内の追跡フィルターを含む</span><span class="sxs-lookup"><span data-stu-id="dc6cb-312">Task 1 - Including the Tracking Filter in the Solution</span></span>

<span data-ttu-id="dc6cb-313">このタスクでは含めます音楽ストアにイベントをトレースするカスタム アクション フィルター。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-313">In this task, you will include in the Music Store a custom action filter to trace events.</span></span> <span data-ttu-id="dc6cb-314">カスタム アクション フィルターと概念は、既に前の実習で扱わ&quot;カスタム アクション フィルター&quot;、だけこのラボの Assets フォルダーからフィルター クラスを含めるされ、Unity のフィルター プロバイダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-314">As custom action filter concepts are already treated in the previous Lab &quot;Custom Action Filters&quot;, you will just include the filter class from the Assets folder of this lab, and then create a Filter Provider for Unity:</span></span>

1. <span data-ttu-id="dc6cb-315">開く、**開始**にソリューションがある、 **Source\Ex03 - 挿入アクション Filter\Begin**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-315">Open the **Begin** solution located in the **Source\Ex03 - Injecting Action Filter\Begin** folder.</span></span> <span data-ttu-id="dc6cb-316">それ以外の場合、作業を続行できますを使用して、**終了**ソリューションは、前の手順を完了して取得します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-316">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="dc6cb-317">指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-317">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="dc6cb-318">これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-318">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="dc6cb-319">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-319">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="dc6cb-320">最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-320">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="dc6cb-321">NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-321">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="dc6cb-322">NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-322">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="dc6cb-323">これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-323">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
    > 
    > <span data-ttu-id="dc6cb-324">詳細については、この記事を参照してください: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages)です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-324">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="dc6cb-325">含める**TraceActionFilter.cs**ファイルから**ソース/資産**に**フィルター/**フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-325">Include **TraceActionFilter.cs** file from **/Sources/Assets** to **/Filters** folder.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > <span data-ttu-id="dc6cb-326">このカスタム アクション フィルターでは、ASP.NET のトレースを実行します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-326">This custom action filter performs ASP.NET tracing.</span></span> <span data-ttu-id="dc6cb-327">チェックすることができます&quot;ASP.NET MVC 4 ローカルおよび動的アクション フィルター&quot;詳細の参照用のラボ環境。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-327">You can check &quot;ASP.NET MVC 4 local and Dynamic Action Filters&quot; Lab for more reference.</span></span>
3. <span data-ttu-id="dc6cb-328">空のクラスを追加**FilterProvider.cs**フォルダー内のプロジェクトに  **/フィルター処理します。**</span><span class="sxs-lookup"><span data-stu-id="dc6cb-328">Add the empty class **FilterProvider.cs** to the project in the folder **/Filters.**</span></span>
4. <span data-ttu-id="dc6cb-329">追加、 **System.Web.Mvc**と**Microsoft.Practices.Unity**内の名前空間**FilterProvider.cs**です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-329">Add the **System.Web.Mvc** and **Microsoft.Practices.Unity** namespaces in **FilterProvider.cs**.</span></span>

    <span data-ttu-id="dc6cb-330">(コード スニペットの*ASP.NET 依存関係インジェクション ラボ - Ex03 のフィルター プロバイダーの名前空間を追加する*)</span><span class="sxs-lookup"><span data-stu-id="dc6cb-330">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. <span data-ttu-id="dc6cb-331">クラスから継承するように**IFilterProvider**インターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-331">Make the class inherit from **IFilterProvider** Interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. <span data-ttu-id="dc6cb-332">追加、 **IUnityContainer**プロパティに、 **FilterProvider**クラス、およびコンテナーを割り当てるクラスのコンス トラクターを作成します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-332">Add a **IUnityContainer** property in the **FilterProvider** class, and then create a class constructor to assign the container.</span></span>

    <span data-ttu-id="dc6cb-333">(コード スニペットの*ASP.NET 依存関係の挿入ラボ - Ex03 のフィルター プロバイダー コンス トラクター*)</span><span class="sxs-lookup"><span data-stu-id="dc6cb-333">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Constructor*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="dc6cb-334">フィルター プロバイダーのクラス コンス トラクターを作成しない、**新しい**の内部オブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-334">The filter provider class constructor is not creating a **new** object inside.</span></span> <span data-ttu-id="dc6cb-335">コンテナーは、パラメーターとして渡され、Unity で依存関係を解決します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-335">The container is passed as a parameter, and the dependency is solved by Unity.</span></span>
7. <span data-ttu-id="dc6cb-336">**FilterProvider**クラス、メソッドを実装して**GetFilters**から**IFilterProvider**インターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-336">In the **FilterProvider** class, implement the method **GetFilters** from **IFilterProvider** interface.</span></span>

    <span data-ttu-id="dc6cb-337">(コード スニペットの*ASP.NET 依存関係の挿入ラボ - Ex03 のフィルター プロバイダー GetFilters*)</span><span class="sxs-lookup"><span data-stu-id="dc6cb-337">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider GetFilters*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a><span data-ttu-id="dc6cb-338">タスク 2 - 登録して、フィルターを有効にします。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-338">Task 2 - Registering and Enabling the Filter</span></span>

<span data-ttu-id="dc6cb-339">このタスクでは、サイトの追跡を有効にします。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-339">In this task, you will enable site tracking.</span></span> <span data-ttu-id="dc6cb-340">内のフィルターを登録するには、 **Bootstrapper.cs BuildUnityContainer**トレースを開始するメソッド。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-340">To do that, you will register the filter in **Bootstrapper.cs BuildUnityContainer** method to start tracing:</span></span>

1. <span data-ttu-id="dc6cb-341">開いている**Web.config** System.Web グループで有効にするトレースの追跡およびプロジェクトのルートに存在します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-341">Open **Web.config** located in the project root and enable trace tracking at System.Web group.</span></span>

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. <span data-ttu-id="dc6cb-342">開いている**Bootstrapper.cs**プロジェクトのルートにします。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-342">Open **Bootstrapper.cs** at project root.</span></span>
3. <span data-ttu-id="dc6cb-343">参照を追加、 **MvcMusicStore.Filters**名前空間。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-343">Add a reference to the **MvcMusicStore.Filters** namespace.</span></span>

    <span data-ttu-id="dc6cb-344">(コード スニペットの*ASP.NET 依存関係の挿入ラボ - Ex03 - ブートス トラップの名前空間を追加する*)</span><span class="sxs-lookup"><span data-stu-id="dc6cb-344">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. <span data-ttu-id="dc6cb-345">選択、 **BuildUnityContainer**メソッドと、Unity コンテナー内のフィルターを登録します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-345">Select the **BuildUnityContainer** method and register the filter in the Unity Container.</span></span> <span data-ttu-id="dc6cb-346">アクション フィルターと同様に、フィルター プロバイダーを登録する必要があります。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-346">You will have to register the filter provider as well as the action filter.</span></span>

    <span data-ttu-id="dc6cb-347">(コード スニペットの*ASP.NET 依存関係の挿入ラボ - Ex03 - 登録 FilterProvider と ActionFilter*)</span><span class="sxs-lookup"><span data-stu-id="dc6cb-347">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Register FilterProvider and ActionFilter*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="dc6cb-348">タスク 3 - アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-348">Task 3 - Running the Application</span></span>

<span data-ttu-id="dc6cb-349">このタスクでは、アプリケーションを実行し、カスタム アクション フィルターが、アクティビティをトレースしているテストします。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-349">In this task, you will run the application and test that the custom action filter is tracing the activity:</span></span>

1. <span data-ttu-id="dc6cb-350">**F5** キーを押してアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-350">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="dc6cb-351">をクリックして**ロック**ジャンル メニュー内で。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-351">Click **Rock** within the Genres Menu.</span></span> <span data-ttu-id="dc6cb-352">する場合、ジャンルの詳細を参照することができます。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-352">You can browse to more genres if you want to.</span></span>

    <span data-ttu-id="dc6cb-353">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "音楽ストア")</span><span class="sxs-lookup"><span data-stu-id="dc6cb-353">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span></span>

    <span data-ttu-id="dc6cb-354">*Music Store*</span><span class="sxs-lookup"><span data-stu-id="dc6cb-354">*Music Store*</span></span>
3. <span data-ttu-id="dc6cb-355">参照**/Trace.axd**  ページで、クリックして、アプリケーション トレースを表示する**詳細を表示する**です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-355">Browse to **/Trace.axd** to see the Application Trace page, and then click **View Details**.</span></span>

    <span data-ttu-id="dc6cb-356">![アプリケーションのトレース ログ](aspnet-mvc-4-dependency-injection/_static/image12.png "アプリケーションのトレース ログ")</span><span class="sxs-lookup"><span data-stu-id="dc6cb-356">![Application Trace Log](aspnet-mvc-4-dependency-injection/_static/image12.png "Application Trace Log")</span></span>

    <span data-ttu-id="dc6cb-357">*アプリケーションのトレース ログ*</span><span class="sxs-lookup"><span data-stu-id="dc6cb-357">*Application Trace Log*</span></span>

    <span data-ttu-id="dc6cb-358">![アプリケーション トレース - 要求の詳細](aspnet-mvc-4-dependency-injection/_static/image13.png "アプリケーションのトレースの要求の詳細")</span><span class="sxs-lookup"><span data-stu-id="dc6cb-358">![Application Trace - Request Details](aspnet-mvc-4-dependency-injection/_static/image13.png "Application Trace - Request Details")</span></span>

    <span data-ttu-id="dc6cb-359">*アプリケーション トレース - 要求の詳細*</span><span class="sxs-lookup"><span data-stu-id="dc6cb-359">*Application Trace - Request Details*</span></span>
4. <span data-ttu-id="dc6cb-360">ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-360">Close the browser.</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="dc6cb-361">まとめ</span><span class="sxs-lookup"><span data-stu-id="dc6cb-361">Summary</span></span>

<span data-ttu-id="dc6cb-362">このハンズオン ラボでは、NuGet パッケージを使用する Unity の統合により、ASP.NET MVC 4 で依存関係の挿入を使用する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-362">By completing this Hands-On Lab you have learned how to use Dependency Injection in ASP.NET MVC 4 by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="dc6cb-363">実現するためには、コント ローラー、ビュー、およびアクション フィルターの内部依存関係の挿入を使用しています。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-363">To achieve that, you have used Dependency Injection inside controllers, views and action filters.</span></span>

<span data-ttu-id="dc6cb-364">次の概念がカバーされました。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-364">The following concepts were covered:</span></span>

- <span data-ttu-id="dc6cb-365">ASP.NET MVC 4 の依存関係の挿入機能</span><span class="sxs-lookup"><span data-stu-id="dc6cb-365">ASP.NET MVC 4 Dependency Injection features</span></span>
- <span data-ttu-id="dc6cb-366">Unity.Mvc3 NuGet パッケージを使用する unity の統合</span><span class="sxs-lookup"><span data-stu-id="dc6cb-366">Unity integration using Unity.Mvc3 NuGet Package</span></span>
- <span data-ttu-id="dc6cb-367">コント ローラーで依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="dc6cb-367">Dependency Injection in Controllers</span></span>
- <span data-ttu-id="dc6cb-368">ビューの依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="dc6cb-368">Dependency Injection in Views</span></span>
- <span data-ttu-id="dc6cb-369">アクション フィルターの依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="dc6cb-369">Dependency injection of Action Filters</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="dc6cb-370">付録 a: をインストールする Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="dc6cb-370">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="dc6cb-371">インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、  **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="dc6cb-371">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="dc6cb-372">次の手順を案内するインストールに必要な手順*Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-372">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="dc6cb-373">移動して[ [https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-373">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="dc6cb-374">または、既に Web Platform Installer をインストールした場合、開くことができ、製品を検索&quot; *Visual Studio Express 2012 for Web と Windows Azure SDK*&quot;です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-374">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="dc6cb-375">をクリックして**を今すぐインストール**です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-375">Click on **Install Now**.</span></span> <span data-ttu-id="dc6cb-376">ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-376">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="dc6cb-377">1 回**Web Platform Installer**が開いて、をクリックして**インストール**セットアップを開始します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-377">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="dc6cb-378">![Visual Studio Express インストール](aspnet-mvc-4-dependency-injection/_static/image14.png "Visual Studio Express のインストール")</span><span class="sxs-lookup"><span data-stu-id="dc6cb-378">![Install Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="dc6cb-379">*Visual Studio Express をインストールします。*</span><span class="sxs-lookup"><span data-stu-id="dc6cb-379">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="dc6cb-380">すべての製品のライセンスと条項を読み、クリックして**同意**を続行します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-380">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![ライセンス条項に同意](aspnet-mvc-4-dependency-injection/_static/image15.png)

    <span data-ttu-id="dc6cb-382">*ライセンス条項に同意*</span><span class="sxs-lookup"><span data-stu-id="dc6cb-382">*Accepting the license terms*</span></span>
5. <span data-ttu-id="dc6cb-383">ダウンロードとインストール プロセスが完了するまで待機します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-383">Wait until the downloading and installation process completes.</span></span>

    ![インストールの進行状況](aspnet-mvc-4-dependency-injection/_static/image16.png)

    <span data-ttu-id="dc6cb-385">*インストールの進行状況*</span><span class="sxs-lookup"><span data-stu-id="dc6cb-385">*Installation progress*</span></span>
6. <span data-ttu-id="dc6cb-386">インストールが完了したらをクリックして**完了**です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-386">When the installation completes, click **Finish**.</span></span>

    ![インストールが完了しました](aspnet-mvc-4-dependency-injection/_static/image17.png)

    <span data-ttu-id="dc6cb-388">*インストールが完了しました*</span><span class="sxs-lookup"><span data-stu-id="dc6cb-388">*Installation completed*</span></span>
7. <span data-ttu-id="dc6cb-389">をクリックして**終了**Web Platform Installer を閉じます。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-389">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="dc6cb-390">Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、順にクリックして、 **VS Express for Web**並べて表示します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-390">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express Web タイルを](aspnet-mvc-4-dependency-injection/_static/image18.png)

    <span data-ttu-id="dc6cb-392">*VS Express Web タイルを*</span><span class="sxs-lookup"><span data-stu-id="dc6cb-392">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="dc6cb-393">付録 b: コード スニペットの使用</span><span class="sxs-lookup"><span data-stu-id="dc6cb-393">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="dc6cb-394">コード スニペットでは、すぐに利用できる必要があるすべてのコードがあります。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-394">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="dc6cb-395">ラボ ドキュメントがわかります正確に使用する場合が、次の図に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-395">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="dc6cb-396">![Visual Studio のコード スニペットを使用して、プロジェクトにコードを挿入する](aspnet-mvc-4-dependency-injection/_static/image19.png "プロジェクトにコードを挿入する Visual Studio を使ってコード スニペット")</span><span class="sxs-lookup"><span data-stu-id="dc6cb-396">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-dependency-injection/_static/image19.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="dc6cb-397">*Visual Studio のコード スニペットを使用して、プロジェクトにコードを挿入するには*</span><span class="sxs-lookup"><span data-stu-id="dc6cb-397">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="dc6cb-398">***キーボード (C# の場合のみ) を使用してコード スニペットを追加するには***</span><span class="sxs-lookup"><span data-stu-id="dc6cb-398">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="dc6cb-399">コードを挿入するには、カーソルを置きます。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-399">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="dc6cb-400">(なし、スペースやハイフン) スニペット名を入力してを起動します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-400">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="dc6cb-401">スニペットの名前に一致する IntelliSense 表示を確認します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-401">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="dc6cb-402">正しいスニペットを選択 (または、全体のスニペットの名前を選択するまで」と入力してください)。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-402">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="dc6cb-403">カーソル位置にスニペットを挿入するには、2 回、Tab キーを押します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-403">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="dc6cb-404">![スニペット名を入力する開始](aspnet-mvc-4-dependency-injection/_static/image20.png "スニペット名の入力を開始")</span><span class="sxs-lookup"><span data-stu-id="dc6cb-404">![Start typing the snippet name](aspnet-mvc-4-dependency-injection/_static/image20.png "Start typing the snippet name")</span></span>

<span data-ttu-id="dc6cb-405">*スニペット名の入力を開始します。*</span><span class="sxs-lookup"><span data-stu-id="dc6cb-405">*Start typing the snippet name*</span></span>

<span data-ttu-id="dc6cb-406">![Tab キーを押して、強調表示されているスニペットを選択する](aspnet-mvc-4-dependency-injection/_static/image21.png "強調表示されているスニペットを選択するキーを押してタブ")</span><span class="sxs-lookup"><span data-stu-id="dc6cb-406">![Press Tab to select the highlighted snippet](aspnet-mvc-4-dependency-injection/_static/image21.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="dc6cb-407">*Tab キーを押して、強調表示されているスニペットを選択するには*</span><span class="sxs-lookup"><span data-stu-id="dc6cb-407">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="dc6cb-408">![キーを押して タブで再度と、スニペットが展開](aspnet-mvc-4-dependency-injection/_static/image22.png "キーを押して タブで再度と、スニペットが展開されます")</span><span class="sxs-lookup"><span data-stu-id="dc6cb-408">![Press Tab again and the snippet will expand](aspnet-mvc-4-dependency-injection/_static/image22.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="dc6cb-409">*キーを押して タブで再度と、スニペットが展開されます。*</span><span class="sxs-lookup"><span data-stu-id="dc6cb-409">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="dc6cb-410">***(C#、Visual Basic および XML) にマウスを使用してコード スニペットを追加する***1 です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-410">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="dc6cb-411">コード スニペットを挿入する場所を右クリックします。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-411">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="dc6cb-412">選択**スニペットの挿入**続く**マイ コード スニペット**です。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-412">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="dc6cb-413">クリックして一覧から、関連するスニペットを選択します。</span><span class="sxs-lookup"><span data-stu-id="dc6cb-413">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="dc6cb-414">![コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリックして](aspnet-mvc-4-dependency-injection/_static/image23.png "コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリック")</span><span class="sxs-lookup"><span data-stu-id="dc6cb-414">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-dependency-injection/_static/image23.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="dc6cb-415">*コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリックします。*</span><span class="sxs-lookup"><span data-stu-id="dc6cb-415">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="dc6cb-416">![クリックして一覧から、関連するスニペットを選択](aspnet-mvc-4-dependency-injection/_static/image24.png "クリックして一覧から、関連するスニペットを選択")</span><span class="sxs-lookup"><span data-stu-id="dc6cb-416">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-dependency-injection/_static/image24.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="dc6cb-417">*クリックして一覧から、関連するスニペットを選択します。*</span><span class="sxs-lookup"><span data-stu-id="dc6cb-417">*Pick the relevant snippet from the list, by clicking on it*</span></span>
