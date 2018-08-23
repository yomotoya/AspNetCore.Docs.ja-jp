---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: ASP.NET MVC 4 依存関係の挿入 |Microsoft Docs
author: rick-anderson
description: '注: このハンズオン ラボでは、ASP.NET MVC と ASP.NET MVC 4 のフィルターの基本的な知識がある前提としています。 場合 rec する前に、ASP.NET MVC 4 のフィルターに使用していない.'
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 3f9222c7b485f552da91f4875c882db7e03cdd0a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831345"
---
# <a name="aspnet-mvc-4-dependency-injection"></a><span data-ttu-id="8df15-104">ASP.NET MVC 4 依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="8df15-104">ASP.NET MVC 4 Dependency Injection</span></span>

<span data-ttu-id="8df15-105">によって[Web キャンプ チーム](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="8df15-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="8df15-106">Web のキャンプ トレーニング キットをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="8df15-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="8df15-107">このハンズオン ラボは、の基本的な知識があることを前提**ASP.NET MVC**と**ASP.NET MVC 4 をフィルター処理**します。</span><span class="sxs-lookup"><span data-stu-id="8df15-107">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC** and **ASP.NET MVC 4 filters**.</span></span> <span data-ttu-id="8df15-108">使用していない場合**ASP.NET MVC 4 をフィルター処理**前を経由することを勧めします。 **ASP.NET MVC のカスタム アクション フィルター**ハンズオン ラボ。</span><span class="sxs-lookup"><span data-stu-id="8df15-108">If you have not used **ASP.NET MVC 4 filters** before, we recommend you to go over **ASP.NET MVC Custom Action Filters** Hands-on Lab.</span></span>

> [!NOTE]
> <span data-ttu-id="8df15-109">すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[Microsoft の Web/WebCampTrainingKit リリース](https://aka.ms/webcamps-training-kit)します。</span><span class="sxs-lookup"><span data-stu-id="8df15-109">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="8df15-110">このラボに固有のプロジェクトは、「 [ASP.NET MVC 4 依存関係の注入](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection)します。</span><span class="sxs-lookup"><span data-stu-id="8df15-110">The project specific to this lab is available at [ASP.NET MVC 4 Dependency Injection](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).</span></span>

<span data-ttu-id="8df15-111">**オブジェクト指向プログラミング**パラダイムでは、各オブジェクトが連携コラボレーション モデルでの共同作成者とコンシューマーがある場合。</span><span class="sxs-lookup"><span data-stu-id="8df15-111">In **Object Oriented Programming** paradigm, objects work together in a collaboration model where there are contributors and consumers.</span></span> <span data-ttu-id="8df15-112">当然ながら、この通信モデルは、オブジェクトと複雑さが増すと管理が困難になるコンポーネント間の依存関係を生成します。</span><span class="sxs-lookup"><span data-stu-id="8df15-112">Naturally, this communication model generates dependencies between objects and components, becoming difficult to manage when complexity increases.</span></span>

<span data-ttu-id="8df15-113">![クラスの依存関係と、モデルの複雑さ](aspnet-mvc-4-dependency-injection/_static/image1.png "クラスの依存関係と、モデルの複雑さ")</span><span class="sxs-lookup"><span data-stu-id="8df15-113">![Class dependencies and model complexity](aspnet-mvc-4-dependency-injection/_static/image1.png "Class dependencies and model complexity")</span></span>

<span data-ttu-id="8df15-114">*クラスの依存関係とモデルの複雑さ*</span><span class="sxs-lookup"><span data-stu-id="8df15-114">*Class dependencies and model complexity*</span></span>

<span data-ttu-id="8df15-115">おそらく耳にしたが、**ファクトリ パターン**およびインターフェイスとクライアント オブジェクトはサービスの場所を担当多くの場合、サービスを使用する実装の間の分離。</span><span class="sxs-lookup"><span data-stu-id="8df15-115">You have probably heard about the **Factory Pattern** and the separation between the interface and the implementation using services, where the client objects are often responsible for service location.</span></span>

<span data-ttu-id="8df15-116">依存関係の注入パターンは、制御の反転の特定の実装です。</span><span class="sxs-lookup"><span data-stu-id="8df15-116">The Dependency Injection pattern is a particular implementation of Inversion of Control.</span></span> <span data-ttu-id="8df15-117">**反転 (IoC) コントロールの**オブジェクトが依存しているが作業を行う他のオブジェクトを作成しないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="8df15-117">**Inversion of Control (IoC)** means that objects do not create other objects on which they rely to do their work.</span></span> <span data-ttu-id="8df15-118">代わりに、これらは、外部ソース (たとえば、xml 構成ファイル) から必要なオブジェクトを取得します。</span><span class="sxs-lookup"><span data-stu-id="8df15-118">Instead, they get the objects that they need from an outside source (for example, an xml configuration file).</span></span>

<span data-ttu-id="8df15-119">**依存関係挿入 (DI)** つまりこれは、オブジェクトの介入なしは、通常、コンポーネントによって実行フレームワークをコンス トラクターのパラメーターを渡すし、プロパティを設定します。</span><span class="sxs-lookup"><span data-stu-id="8df15-119">**Dependency Injection (DI)** means that this is done without the object intervention, usually by a framework component that passes constructor parameters and set properties.</span></span>

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a><span data-ttu-id="8df15-120">依存関係の注入 (DI) デザイン パターン</span><span class="sxs-lookup"><span data-stu-id="8df15-120">The Dependency Injection (DI) Design Pattern</span></span>

<span data-ttu-id="8df15-121">大まかに言えば、依存関係の注入の目的は、するクライアント クラス (例: *、ゴルファー*) ものをインターフェイスを満たす必要があります (例: *IClub*)。</span><span class="sxs-lookup"><span data-stu-id="8df15-121">At a high level, the goal of Dependency Injection is that a client class (e.g. *the golfer*) needs something that satisfies an interface (e.g. *IClub*).</span></span> <span data-ttu-id="8df15-122">具象型では関係ありません (例: *WoodClub、IronClub、WedgeClub*または*PutterClub*)、それを処理する他のユーザーが必要 (例: 適切な*キャディ*)。</span><span class="sxs-lookup"><span data-stu-id="8df15-122">It doesn't care what the concrete type is (e.g. *WoodClub, IronClub, WedgeClub* or *PutterClub*), it wants someone else to handle that (e.g. a good *caddy*).</span></span> <span data-ttu-id="8df15-123">ASP.NET MVC では、依存関係競合回避モジュールは依存関係のロジックを別の場所を登録することを許可することができます (例: コンテナーまたは*クラブのバッグ*)。</span><span class="sxs-lookup"><span data-stu-id="8df15-123">The Dependency Resolver in ASP.NET MVC can allow you to register your dependency logic somewhere else (e.g. a container or a *bag of clubs*).</span></span>

<span data-ttu-id="8df15-124">![依存関係の注入ダイアグラム](aspnet-mvc-4-dependency-injection/_static/image2.png "依存関係の挿入の図")</span><span class="sxs-lookup"><span data-stu-id="8df15-124">![Dependency Injection diagram](aspnet-mvc-4-dependency-injection/_static/image2.png "Dependency Injection illustration")</span></span>

<span data-ttu-id="8df15-125">*依存関係の挿入 - ゴルフとの類似性*</span><span class="sxs-lookup"><span data-stu-id="8df15-125">*Dependency Injection - Golf analogy*</span></span>

<span data-ttu-id="8df15-126">依存関係の注入パターンと制御の反転を使用する利点は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="8df15-126">The advantages of using Dependency Injection pattern and Inversion of Control are the following:</span></span>

- <span data-ttu-id="8df15-127">クラス結合度を削減します。</span><span class="sxs-lookup"><span data-stu-id="8df15-127">Reduces class coupling</span></span>
- <span data-ttu-id="8df15-128">コードの再利用が増加します。</span><span class="sxs-lookup"><span data-stu-id="8df15-128">Increases code reusing</span></span>
- <span data-ttu-id="8df15-129">コードの保守性を向上します。</span><span class="sxs-lookup"><span data-stu-id="8df15-129">Improves code maintainability</span></span>
- <span data-ttu-id="8df15-130">アプリケーションのテストが向上します。</span><span class="sxs-lookup"><span data-stu-id="8df15-130">Improves application testing</span></span>

> [!NOTE]
> <span data-ttu-id="8df15-131">依存関係の挿入は抽象ファクトリ デザイン パターンと比較場合がありますが、両方のアプローチのわずかな違いがあります。</span><span class="sxs-lookup"><span data-stu-id="8df15-131">Dependency Injection is sometimes compared with Abstract Factory Design Pattern, but there is a slight difference between both approaches.</span></span> <span data-ttu-id="8df15-132">DI には、フレームワーク、ファクトリと、登録済みサービスを呼び出すことによって依存関係を解決するために、背後にある作業があります。</span><span class="sxs-lookup"><span data-stu-id="8df15-132">DI has a Framework working behind to solve dependencies by calling the factories and the registered services.</span></span>


<span data-ttu-id="8df15-133">依存関係の注入パターンを理解したところで ASP.NET MVC 4 を適用する方法をこの演習全体を通じて学習がします。</span><span class="sxs-lookup"><span data-stu-id="8df15-133">Now that you understand the Dependency Injection Pattern, you will learn throughout this lab how to apply it in ASP.NET MVC 4.</span></span> <span data-ttu-id="8df15-134">依存関係挿入を使用して開始、**コント ローラー**にデータベース アクセス サービスが含まれます。</span><span class="sxs-lookup"><span data-stu-id="8df15-134">You will start using Dependency Injection in the **Controllers** to include a database access service.</span></span> <span data-ttu-id="8df15-135">次に、依存関係の挿入が適用されますが、**ビュー**サービスを使用して、情報を表示します。</span><span class="sxs-lookup"><span data-stu-id="8df15-135">Next, you will apply Dependency Injection to the **Views** to consume a service and show information.</span></span> <span data-ttu-id="8df15-136">最後に、ソリューション内のカスタム アクション フィルターを挿入する ASP.NET MVC 4 のフィルターに、DI を拡張します。</span><span class="sxs-lookup"><span data-stu-id="8df15-136">Finally, you will extend the DI to ASP.NET MVC 4 Filters, injecting a custom action filter in the solution.</span></span>

<span data-ttu-id="8df15-137">このハンズオン ラボでは、学習する方法。</span><span class="sxs-lookup"><span data-stu-id="8df15-137">In this Hands-on Lab, you will learn how to:</span></span>

- <span data-ttu-id="8df15-138">NuGet パッケージを使用して依存関係の注入に Unity を使用した ASP.NET MVC 4 を統合します。</span><span class="sxs-lookup"><span data-stu-id="8df15-138">Integrate ASP.NET MVC 4 with Unity for Dependency Injection using NuGet Packages</span></span>
- <span data-ttu-id="8df15-139">ASP.NET MVC のコント ローラー内の依存関係の挿入を使用して、</span><span class="sxs-lookup"><span data-stu-id="8df15-139">Use Dependency Injection inside an ASP.NET MVC Controller</span></span>
- <span data-ttu-id="8df15-140">ASP.NET MVC ビュー内の依存関係の挿入を使用して、</span><span class="sxs-lookup"><span data-stu-id="8df15-140">Use Dependency Injection inside an ASP.NET MVC View</span></span>
- <span data-ttu-id="8df15-141">ASP.NET MVC のアクション フィルター内の依存関係の挿入を使用して、</span><span class="sxs-lookup"><span data-stu-id="8df15-141">Use Dependency Injection inside an ASP.NET MVC Action Filter</span></span>

> [!NOTE]
> <span data-ttu-id="8df15-142">このラボは、依存関係の解決の Unity.Mvc3 NuGet パッケージを使用して、ASP.NET MVC 4 で動作する任意の依存関係挿入フレームワークを調整することができます。</span><span class="sxs-lookup"><span data-stu-id="8df15-142">This Lab is using Unity.Mvc3 NuGet Package for dependency resolution, but it is possible to adapt any Dependency Injection Framework to work with ASP.NET MVC 4.</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="8df15-143">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="8df15-143">Prerequisites</span></span>

<span data-ttu-id="8df15-144">このラボを完成させるのには、次の項目が必要です。</span><span class="sxs-lookup"><span data-stu-id="8df15-144">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="8df15-145">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)または上位 (読み取り[付録 A](#AppendixA)をインストールする方法について)。</span><span class="sxs-lookup"><span data-stu-id="8df15-145">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="8df15-146">セットアップ</span><span class="sxs-lookup"><span data-stu-id="8df15-146">Setup</span></span>

<span data-ttu-id="8df15-147">**コード スニペットをインストールします。**</span><span class="sxs-lookup"><span data-stu-id="8df15-147">**Installing Code Snippets**</span></span>

<span data-ttu-id="8df15-148">便宜上、このラボに管理するコードの多くは、Visual Studio コード スニペットとして利用できます。</span><span class="sxs-lookup"><span data-stu-id="8df15-148">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="8df15-149">実行コード スニペットをインストールする **.\Source\Setup\CodeSnippets.vsi**ファイル。</span><span class="sxs-lookup"><span data-stu-id="8df15-149">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="8df15-150">このドキュメントの付録を参照することができます、Visual Studio のコード スニペットとその使用方法を学習するに慣れていない場合&quot;[付録 b: を使用してコード スニペット](#AppendixB)&quot;します。</span><span class="sxs-lookup"><span data-stu-id="8df15-150">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="8df15-151">演習</span><span class="sxs-lookup"><span data-stu-id="8df15-151">Exercises</span></span>

<span data-ttu-id="8df15-152">次の演習では、このハンズオン ラボから成ります。</span><span class="sxs-lookup"><span data-stu-id="8df15-152">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="8df15-153">手順 1: コント ローラーを挿入します。</span><span class="sxs-lookup"><span data-stu-id="8df15-153">Exercise 1: Injecting a Controller</span></span>](#Exercise1)
2. [<span data-ttu-id="8df15-154">手順 2: ビューを挿入します。</span><span class="sxs-lookup"><span data-stu-id="8df15-154">Exercise 2: Injecting a View</span></span>](#Exercise2)
3. [<span data-ttu-id="8df15-155">手順 3: フィルターを挿入します。</span><span class="sxs-lookup"><span data-stu-id="8df15-155">Exercise 3: Injecting Filters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="8df15-156">各演習が用意されており、**エンド**演習を完了した後に取得する必要があります、結果として得られるソリューションに含まれているフォルダー。</span><span class="sxs-lookup"><span data-stu-id="8df15-156">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="8df15-157">作業、演習を通じて追加のヘルプが必要な場合は、このソリューションをガイドとして使用できます。</span><span class="sxs-lookup"><span data-stu-id="8df15-157">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="8df15-158">この演習の所要時間を推定: **30 分**します。</span><span class="sxs-lookup"><span data-stu-id="8df15-158">Estimated time to complete this lab: **30 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a><span data-ttu-id="8df15-159">手順 1: コント ローラーを挿入します。</span><span class="sxs-lookup"><span data-stu-id="8df15-159">Exercise 1: Injecting a Controller</span></span>

<span data-ttu-id="8df15-160">この演習では、Unity は NuGet パッケージを使用して統合することにより、ASP.NET MVC のコント ローラーで依存関係の挿入を使用する方法を学びます。</span><span class="sxs-lookup"><span data-stu-id="8df15-160">In this exercise, you will learn how to use Dependency Injection in ASP.NET MVC Controllers by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="8df15-161">そのため、サービスをデータ アクセス ロジックを分離する、MvcMusicStore コント ローラーに含まれます。</span><span class="sxs-lookup"><span data-stu-id="8df15-161">For that reason, you will include services into your MvcMusicStore controllers to separate the logic from the data access.</span></span> <span data-ttu-id="8df15-162">サービスはコント ローラーのコンス トラクターは、のヘルプで依存関係の挿入を使用して、解決に新しい依存関係を作成**Unity**します。</span><span class="sxs-lookup"><span data-stu-id="8df15-162">The services will create a new dependency in the controller constructor, which will be resolved using Dependency Injection with the help of **Unity**.</span></span>

<span data-ttu-id="8df15-163">このアプローチでは、小さい結合された、アプリケーションはより柔軟で簡単に管理およびテストを生成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="8df15-163">This approach will show you how to generate less coupled applications, which are more flexible and easier to maintain and test.</span></span> <span data-ttu-id="8df15-164">Unity を使用した ASP.NET MVC を統合する方法も学習します。</span><span class="sxs-lookup"><span data-stu-id="8df15-164">You will also learn how to integrate ASP.NET MVC with Unity.</span></span>

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a><span data-ttu-id="8df15-165">StoreManager サービスについて</span><span class="sxs-lookup"><span data-stu-id="8df15-165">About StoreManager Service</span></span>

<span data-ttu-id="8df15-166">今すぐ begin ソリューションで提供されている MVC Music Store にはという名前のデータ ストアのコント ローラーを管理するサービスが含まれています**StoreService**します。</span><span class="sxs-lookup"><span data-stu-id="8df15-166">The MVC Music Store provided in the begin solution now includes a service that manages the Store Controller data named **StoreService**.</span></span> <span data-ttu-id="8df15-167">次のとおり、ストア サービスの実装が表示されます。</span><span class="sxs-lookup"><span data-stu-id="8df15-167">Below you will find the Store Service implementation.</span></span> <span data-ttu-id="8df15-168">すべてのメソッドがモデルのエンティティを返すことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="8df15-168">Note that all the methods return Model entities.</span></span>

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

<span data-ttu-id="8df15-169">**StoreController**ソリューションのようになりましたの使用を開始から**StoreService**します。</span><span class="sxs-lookup"><span data-stu-id="8df15-169">**StoreController** from the begin solution now consumes **StoreService**.</span></span> <span data-ttu-id="8df15-170">すべてのデータの参照が削除された**StoreController**、および使用する任意のメソッドを変更することがなく、現在のデータ アクセス プロバイダーを変更する実行可能なようになりました**StoreService**します。</span><span class="sxs-lookup"><span data-stu-id="8df15-170">All the data references were removed from **StoreController**, and now possible to modify the current data access provider without changing any method that consumes **StoreService**.</span></span>

<span data-ttu-id="8df15-171">その下に表示されます、 **StoreController**実装との依存関係を持つ**StoreService**クラスのコンス トラクター内。</span><span class="sxs-lookup"><span data-stu-id="8df15-171">You will find below that the **StoreController** implementation has a dependency with **StoreService** inside the class constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="8df15-172">この演習で導入された依存関係に関連する**Inversion of Control** (IoC)。</span><span class="sxs-lookup"><span data-stu-id="8df15-172">The dependency introduced in this exercise is related to **Inversion of Control** (IoC).</span></span>
> 
> <span data-ttu-id="8df15-173">**StoreController**クラスのコンス トラクターは、 **IStoreService**型のパラメーターは、クラス内からのサービス呼び出しを実行するために不可欠です。</span><span class="sxs-lookup"><span data-stu-id="8df15-173">The **StoreController** class constructor receives an **IStoreService** type parameter, which is essential to perform service calls from inside the class.</span></span> <span data-ttu-id="8df15-174">ただし、 **StoreController**を ASP.NET MVC を使用する任意のコント ローラーを持つ必要があります (パラメーターなし) での既定コンス トラクターを実装していません。</span><span class="sxs-lookup"><span data-stu-id="8df15-174">However, **StoreController** does not implement the default constructor (with no parameters) that any controller must have to work with ASP.NET MVC.</span></span>
> 
> <span data-ttu-id="8df15-175">依存関係を解決するには、コント ローラーを抽象ファクトリ (指定した型の任意のオブジェクトを返すクラス) で作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8df15-175">To resolve the dependency, the controller has to be created by an abstract factory (a class that returns any object of the specified type).</span></span>


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="8df15-176">クラスは、パラメーターなしのコンス トラクターが宣言されていないため、サービス オブジェクトを送信することがなく、StoreController を作成するとき、エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="8df15-176">You will get an error when the class tries to create the StoreController without sending the service object, as there is no parameterless constructor declared.</span></span>


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a><span data-ttu-id="8df15-177">タスク 1 - アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="8df15-177">Task 1 - Running the Application</span></span>

<span data-ttu-id="8df15-178">このタスクで、データ アクセスでは、アプリケーション ロジックから分離されるストア コント ローラーに、サービスを含む開始アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="8df15-178">In this task, you will run the Begin application, which includes the service into the Store Controller that separates the data access from the application logic.</span></span>

<span data-ttu-id="8df15-179">アプリケーションを実行するときにコント ローラー サービスが既定でのパラメーターとして渡されないと、例外が表示されます。</span><span class="sxs-lookup"><span data-stu-id="8df15-179">When running the application, you will receive an exception, as the controller service is not passed as a parameter by default:</span></span>

1. <span data-ttu-id="8df15-180">開く、**開始**ソリューション**Source\Ex01 挿入 Controller\Begin**します。</span><span class="sxs-lookup"><span data-stu-id="8df15-180">Open the **Begin** solution located in **Source\Ex01-Injecting Controller\Begin**.</span></span>

   1. <span data-ttu-id="8df15-181">いくつか不足している NuGet パッケージをダウンロードする必要がありますが続行する前にします。</span><span class="sxs-lookup"><span data-stu-id="8df15-181">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="8df15-182">これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。</span><span class="sxs-lookup"><span data-stu-id="8df15-182">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="8df15-183">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。</span><span class="sxs-lookup"><span data-stu-id="8df15-183">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="8df15-184">最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。</span><span class="sxs-lookup"><span data-stu-id="8df15-184">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="8df15-185">NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="8df15-185">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="8df15-186">With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="8df15-186">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="8df15-187">これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8df15-187">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="8df15-188">キーを押して**ctrl キーを押しながら f5 キーを押して**デバッグなしでアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="8df15-188">Press **Ctrl + F5** to run the application without debugging.</span></span> <span data-ttu-id="8df15-189">エラー メッセージが表示されます&quot;**パラメーターなしのコンス トラクターはこのオブジェクトに対して定義されている**&quot;:</span><span class="sxs-lookup"><span data-stu-id="8df15-189">You will get the error message &quot;**No parameterless constructor defined for this object**&quot;:</span></span>

    <span data-ttu-id="8df15-190">![ASP.NET MVC の開始のアプリケーションの実行中にエラー](aspnet-mvc-4-dependency-injection/_static/image3.png "ASP.NET MVC の開始のアプリケーションの実行中にエラー")</span><span class="sxs-lookup"><span data-stu-id="8df15-190">![Error while running ASP.NET MVC Begin Application](aspnet-mvc-4-dependency-injection/_static/image3.png "Error while running ASP.NET MVC Begin Application")</span></span>

    <span data-ttu-id="8df15-191">*ASP.NET MVC の開始のアプリケーションの実行中にエラー*</span><span class="sxs-lookup"><span data-stu-id="8df15-191">*Error while running ASP.NET MVC Begin Application*</span></span>
3. <span data-ttu-id="8df15-192">ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="8df15-192">Close the browser.</span></span>

<span data-ttu-id="8df15-193">次の手順では、このコント ローラーが必要な依存関係を挿入する音楽ストア ソリューションの機能します。</span><span class="sxs-lookup"><span data-stu-id="8df15-193">In the following steps you will work on the Music Store Solution to inject the dependency this controller needs.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a><span data-ttu-id="8df15-194">タスク 2 - MvcMusicStore ソリューションに Unity を含む</span><span class="sxs-lookup"><span data-stu-id="8df15-194">Task 2 - Including Unity into MvcMusicStore Solution</span></span>

<span data-ttu-id="8df15-195">このタスクで含める**Unity.Mvc3** NuGet パッケージをソリューションにします。</span><span class="sxs-lookup"><span data-stu-id="8df15-195">In this task, you will include **Unity.Mvc3** NuGet Package to the solution.</span></span>

> [!NOTE]
> <span data-ttu-id="8df15-196">Unity.Mvc3 パッケージは、ASP.NET MVC 3 では、用に設計されましたが、ASP.NET MVC 4 と完全な互換性です。</span><span class="sxs-lookup"><span data-stu-id="8df15-196">Unity.Mvc3 package was designed for ASP.NET MVC 3, but it is fully compatible with ASP.NET MVC 4.</span></span>
> 
> <span data-ttu-id="8df15-197">Unity では、インスタンス オプション サポートで軽量で拡張可能な依存関係挿入コンテナーにし、型のインターセプトします。</span><span class="sxs-lookup"><span data-stu-id="8df15-197">Unity is a lightweight, extensible dependency injection container with optional support for instance and type interception.</span></span> <span data-ttu-id="8df15-198">あらゆる種類の .NET アプリケーションで使用するための汎用コンテナーになります。</span><span class="sxs-lookup"><span data-stu-id="8df15-198">It is a general-purpose container for use in any type of .NET application.</span></span> <span data-ttu-id="8df15-199">などの依存関係の注入メカニズムで見つかったすべての一般的な機能を提供します。 オブジェクトの作成、コンテナーにコンポーネントの構成を遅らせることで、ランタイムと柔軟性の依存関係を指定することによって要件を抽象化します。</span><span class="sxs-lookup"><span data-stu-id="8df15-199">It provides all the common features found in dependency injection mechanisms including: object creation, abstraction of requirements by specifying dependencies at runtime and flexibility, by deferring the component configuration to the container.</span></span>


1. <span data-ttu-id="8df15-200">インストール**Unity.Mvc3**で NuGet パッケージ、 **MvcMusicStore**プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="8df15-200">Install **Unity.Mvc3** NuGet Package in the **MvcMusicStore** project.</span></span> <span data-ttu-id="8df15-201">これを行うには、開く、**パッケージ マネージャー コンソール**から**ビュー** | **その他の Windows**します。</span><span class="sxs-lookup"><span data-stu-id="8df15-201">To do this, open the **Package Manager Console** from **View** | **Other Windows**.</span></span>
2. <span data-ttu-id="8df15-202">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="8df15-202">Run the following command.</span></span>

    <span data-ttu-id="8df15-203">PMC</span><span class="sxs-lookup"><span data-stu-id="8df15-203">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    <span data-ttu-id="8df15-204">![Unity.Mvc3 NuGet パッケージをインストールする](aspnet-mvc-4-dependency-injection/_static/image4.png "Unity.Mvc3 NuGet パッケージをインストールします。")</span><span class="sxs-lookup"><span data-stu-id="8df15-204">![Installing Unity.Mvc3 NuGet Package](aspnet-mvc-4-dependency-injection/_static/image4.png "Installing Unity.Mvc3 NuGet Package")</span></span>

    <span data-ttu-id="8df15-205">*Unity.Mvc3 NuGet パッケージをインストールします。*</span><span class="sxs-lookup"><span data-stu-id="8df15-205">*Installing Unity.Mvc3 NuGet Package*</span></span>
3. <span data-ttu-id="8df15-206">1 回、 **Unity.Mvc3**パッケージがインストールされている、探索ファイルとフォルダーの Unity の構成を簡略化するために自動的に追加します。</span><span class="sxs-lookup"><span data-stu-id="8df15-206">Once the **Unity.Mvc3** package is installed, explore the files and folders it automatically adds in order to simplify Unity configuration.</span></span>

    <span data-ttu-id="8df15-207">![Unity.Mvc3 パッケージがインストールされている](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 パッケージがインストールされています。")</span><span class="sxs-lookup"><span data-stu-id="8df15-207">![Unity.Mvc3 package installed](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 package installed")</span></span>

    <span data-ttu-id="8df15-208">*Unity.Mvc3 パッケージがインストールされています。*</span><span class="sxs-lookup"><span data-stu-id="8df15-208">*Unity.Mvc3 package installed*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a><span data-ttu-id="8df15-209">タスク 3 - Global.asax.cs のアプリケーションを登録する Unity\_開始</span><span class="sxs-lookup"><span data-stu-id="8df15-209">Task 3 - Registering Unity in Global.asax.cs Application\_Start</span></span>

<span data-ttu-id="8df15-210">このタスクでは更新、**アプリケーション\_開始**メソッドにある**Global.asax.cs** Unity ブートス トラップ初期化子を呼び出すし、次に、ブートス トラップ ファイルの登録を更新するにはサービスと依存関係の挿入に使用するコント ローラー。</span><span class="sxs-lookup"><span data-stu-id="8df15-210">In this task, you will update the **Application\_Start** method located in **Global.asax.cs** to call the Unity Bootstrapper initializer and then, update the Bootstrapper file registering the Service and Controller you will use for Dependency Injection.</span></span>

1. <span data-ttu-id="8df15-211">ここで、これは、Unity コンテナーを初期化するファイル ブートス トラップと依存関係競合回避モジュールをフックするされます。</span><span class="sxs-lookup"><span data-stu-id="8df15-211">Now, you will hook up the Bootstrapper which is the file that initializes the Unity container and Dependency Resolver.</span></span> <span data-ttu-id="8df15-212">これを行うには、開く**Global.asax.cs**内で強調表示されている次のコードを追加し、**アプリケーション\_開始**メソッド。</span><span class="sxs-lookup"><span data-stu-id="8df15-212">To do this, open **Global.asax.cs** and add the following highlighted code within the **Application\_Start** method.</span></span>

    <span data-ttu-id="8df15-213">(コード スニペット - *ASP.NET 依存関係の注入ラボ - Ex01 - 初期化 Unity*)</span><span class="sxs-lookup"><span data-stu-id="8df15-213">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Initialize Unity*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. <span data-ttu-id="8df15-214">開いている**Bootstrapper.cs**ファイル。</span><span class="sxs-lookup"><span data-stu-id="8df15-214">Open **Bootstrapper.cs** file.</span></span>
3. <span data-ttu-id="8df15-215">次の名前空間を含める: **MvcMusicStore.Services**と**MusicStore.Controllers**します。</span><span class="sxs-lookup"><span data-stu-id="8df15-215">Include the following namespaces: **MvcMusicStore.Services** and **MusicStore.Controllers**.</span></span>

    <span data-ttu-id="8df15-216">(コード スニペット - *ASP.NET 依存関係の注入ラボ - Ex01 - ブートス トラップ名前空間を追加する*)</span><span class="sxs-lookup"><span data-stu-id="8df15-216">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. <span data-ttu-id="8df15-217">置換**BuildUnityContainer**メソッドの内容をストアのコント ローラーとストア サービスを登録する次のコードにします。</span><span class="sxs-lookup"><span data-stu-id="8df15-217">Replace **BuildUnityContainer** method's content with the following code that registers Store Controller and Store Service.</span></span>

    <span data-ttu-id="8df15-218">(コード スニペット - *ASP.NET 依存関係の注入ラボ - Ex01 - 登録ストア コント ローラーとサービス*)</span><span class="sxs-lookup"><span data-stu-id="8df15-218">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Register Store Controller and Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="8df15-219">タスク 4 - アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="8df15-219">Task 4 - Running the Application</span></span>

<span data-ttu-id="8df15-220">このタスクで Unity を含めた後にロードするようになりましたことを確認するアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="8df15-220">In this task, you will run the application to verify that it can now be loaded after including Unity.</span></span>

1. <span data-ttu-id="8df15-221">キーを押して**F5**アプリケーションを実行する、アプリケーションはすべてのエラー メッセージを表示することがなく読み込むようになりました必要があります。</span><span class="sxs-lookup"><span data-stu-id="8df15-221">Press **F5** to run the application, the application should now load without showing any error message.</span></span>

    <span data-ttu-id="8df15-222">![アプリケーションを実行して、依存関係の挿入](aspnet-mvc-4-dependency-injection/_static/image6.png "アプリケーションを実行して、依存関係の挿入")</span><span class="sxs-lookup"><span data-stu-id="8df15-222">![Running Application with Dependency Injection](aspnet-mvc-4-dependency-injection/_static/image6.png "Running Application with Dependency Injection")</span></span>

    <span data-ttu-id="8df15-223">*実行中のアプリケーション依存関係の挿入*</span><span class="sxs-lookup"><span data-stu-id="8df15-223">*Running Application with Dependency Injection*</span></span>
2. <span data-ttu-id="8df15-224">参照する **/格納**します。</span><span class="sxs-lookup"><span data-stu-id="8df15-224">Browse to **/Store**.</span></span> <span data-ttu-id="8df15-225">これを呼び出す**StoreController**を使用して今すぐ作成されます**Unity**します。</span><span class="sxs-lookup"><span data-stu-id="8df15-225">This will invoke **StoreController**, which is now created using **Unity**.</span></span>

    <span data-ttu-id="8df15-226">![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")</span><span class="sxs-lookup"><span data-stu-id="8df15-226">![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")</span></span>

    <span data-ttu-id="8df15-227">*MVC Music Store*</span><span class="sxs-lookup"><span data-stu-id="8df15-227">*MVC Music Store*</span></span>
3. <span data-ttu-id="8df15-228">ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="8df15-228">Close the browser.</span></span>

<span data-ttu-id="8df15-229">次の演習では、ASP.NET MVC ビューとアクション フィルター内で使用する依存関係の挿入のスコープを拡張する方法を学びます。</span><span class="sxs-lookup"><span data-stu-id="8df15-229">In the following exercises you will learn how to extend the Dependency Injection scope to use it inside ASP.NET MVC Views and Action Filters.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a><span data-ttu-id="8df15-230">手順 2: ビューを挿入します。</span><span class="sxs-lookup"><span data-stu-id="8df15-230">Exercise 2: Injecting a View</span></span>

<span data-ttu-id="8df15-231">この演習では、Unity の統合を ASP.NET MVC 4 の新機能を含むビューの依存関係の挿入を使用する方法を学びます。</span><span class="sxs-lookup"><span data-stu-id="8df15-231">In this exercise, you will learn how to use Dependency Injection in a view with the new features of ASP.NET MVC 4 for Unity integration.</span></span> <span data-ttu-id="8df15-232">このため、ストア ビューの参照、これは、メッセージと、次の図に記載内にカスタム サービスを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="8df15-232">In order to do that, you will call a custom service inside the Store Browse View, which will show a message and an image below.</span></span>

<span data-ttu-id="8df15-233">次に、プロジェクトを Unity に統合し、依存関係を挿入するカスタム依存関係競合回避モジュールを作成します。</span><span class="sxs-lookup"><span data-stu-id="8df15-233">Then, you will integrate the project with Unity and create a custom dependency resolver to inject the dependencies.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a><span data-ttu-id="8df15-234">タスク 1 - サービスを使用するビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="8df15-234">Task 1 - Creating a View that Consumes a Service</span></span>

<span data-ttu-id="8df15-235">このタスクでは、新しい依存関係を生成するサービスの呼び出しを実行するビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="8df15-235">In this task, you will create a view that performs a service call to generate a new dependency.</span></span> <span data-ttu-id="8df15-236">このソリューションに含まれる単純なメッセージング サービスで、サービスが構成されます。</span><span class="sxs-lookup"><span data-stu-id="8df15-236">The service consists in a simple messaging service included in this solution.</span></span>

1. <span data-ttu-id="8df15-237">開く、**開始**ソリューション、 **Source\Ex02 挿入 View\Begin**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="8df15-237">Open the **Begin** solution located in the **Source\Ex02-Injecting View\Begin** folder.</span></span> <span data-ttu-id="8df15-238">使用を続ける可能性がありますそれ以外の場合、**エンド**ソリューションは、前の演習を完了して取得します。</span><span class="sxs-lookup"><span data-stu-id="8df15-238">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="8df15-239">指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。</span><span class="sxs-lookup"><span data-stu-id="8df15-239">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="8df15-240">これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。</span><span class="sxs-lookup"><span data-stu-id="8df15-240">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="8df15-241">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。</span><span class="sxs-lookup"><span data-stu-id="8df15-241">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="8df15-242">最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。</span><span class="sxs-lookup"><span data-stu-id="8df15-242">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="8df15-243">NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="8df15-243">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="8df15-244">With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="8df15-244">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="8df15-245">これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8df15-245">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="8df15-246">詳細については、この記事を参照してください: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages)します。</span><span class="sxs-lookup"><span data-stu-id="8df15-246">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="8df15-247">含める、 **MessageService.cs**と**IMessageService.cs**クラスに格納、 **\Assets をソース**フォルダー **/サービス**。</span><span class="sxs-lookup"><span data-stu-id="8df15-247">Include the **MessageService.cs** and the **IMessageService.cs** classes located in the **Source \Assets** folder in **/Services**.</span></span> <span data-ttu-id="8df15-248">これを行うには、右クリックして**サービス**フォルダーと選択**既存項目の追加**します。</span><span class="sxs-lookup"><span data-stu-id="8df15-248">To do this, right-click **Services** folder and select **Add Existing Item**.</span></span> <span data-ttu-id="8df15-249">ファイルの場所を参照し、それらが含まれます。</span><span class="sxs-lookup"><span data-stu-id="8df15-249">Browse to the files' location and include them.</span></span>

    <span data-ttu-id="8df15-250">![メッセージのサービスとサービスのインターフェイスを追加する](aspnet-mvc-4-dependency-injection/_static/image8.png "メッセージ サービスとサービスのインターフェイスを追加します。")</span><span class="sxs-lookup"><span data-stu-id="8df15-250">![Adding Message Service and Service Interface](aspnet-mvc-4-dependency-injection/_static/image8.png "Adding Message Service and Service Interface")</span></span>

    <span data-ttu-id="8df15-251">*サービス インターフェイスと追加のメッセージ サービス*</span><span class="sxs-lookup"><span data-stu-id="8df15-251">*Adding Message Service and Service Interface*</span></span>

    > [!NOTE]
    > <span data-ttu-id="8df15-252">**IMessageService**インターフェイスによって実装される 2 つのプロパティを定義する、 **MessageService**クラス。</span><span class="sxs-lookup"><span data-stu-id="8df15-252">The **IMessageService** interface defines two properties implemented by the **MessageService** class.</span></span> <span data-ttu-id="8df15-253">これらのプロパティ -**メッセージ**と**ImageUrl**-表示するには、メッセージと、イメージの URL を格納します。</span><span class="sxs-lookup"><span data-stu-id="8df15-253">These properties -**Message** and **ImageUrl**- store the message and the URL of the image to be displayed.</span></span>
3. <span data-ttu-id="8df15-254">フォルダーを作成**ページ/** プロジェクトのルート フォルダー、および既存のクラスを追加し、 **MyBasePage.cs**から**Source\Assets**します。</span><span class="sxs-lookup"><span data-stu-id="8df15-254">Create the folder **/Pages** in the project's root folder, and then add the existing class **MyBasePage.cs** from **Source\Assets**.</span></span> <span data-ttu-id="8df15-255">継承する基本ページには、次の構造があります。</span><span class="sxs-lookup"><span data-stu-id="8df15-255">The base page you will inherit from has the following structure.</span></span>

    <span data-ttu-id="8df15-256">![Pages フォルダー](aspnet-mvc-4-dependency-injection/_static/image9.png "Pages フォルダー")</span><span class="sxs-lookup"><span data-stu-id="8df15-256">![Pages folder](aspnet-mvc-4-dependency-injection/_static/image9.png "Pages folder")</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. <span data-ttu-id="8df15-257">開いている**Browse.cshtml**から表示 **/ビュー/ストア**フォルダーから継承し**MyBasePage.cs**します。</span><span class="sxs-lookup"><span data-stu-id="8df15-257">Open **Browse.cshtml** view from **/Views/Store** folder, and make it inherit from **MyBasePage.cs**.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. <span data-ttu-id="8df15-258">**参照**ビューで、呼び出しを追加して**MessageService**イメージと、サービスによって取得したメッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="8df15-258">In the **Browse** view, add a call to **MessageService** to display an image and a message retrieved by the service.</span></span>
   <span data-ttu-id="8df15-259">(C#)</span><span class="sxs-lookup"><span data-stu-id="8df15-259">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a><span data-ttu-id="8df15-260">タスク 2 - カスタム依存関係競合回避モジュールと、カスタム ビュー ページ アクティベーターを含む</span><span class="sxs-lookup"><span data-stu-id="8df15-260">Task 2 - Including a Custom Dependency Resolver and a Custom View Page Activator</span></span>

<span data-ttu-id="8df15-261">前のタスクでは、その中のサービス呼び出しを実行して、ビュー内の新しい依存関係を挿入します。</span><span class="sxs-lookup"><span data-stu-id="8df15-261">In the previous task, you injected a new dependency inside a view to perform a service call inside it.</span></span> <span data-ttu-id="8df15-262">ここで、ASP.NET MVC 依存関係の注入インターフェイスを実装することでその依存関係を解決する**IViewPageActivator**と**IDependencyResolver**します。</span><span class="sxs-lookup"><span data-stu-id="8df15-262">Now, you will resolve that dependency by implementing the ASP.NET MVC Dependency Injection interfaces **IViewPageActivator** and **IDependencyResolver**.</span></span> <span data-ttu-id="8df15-263">実装、ソリューションに含めるは**IDependencyResolver** Unity を使用してサービスの取得を処理するされます。</span><span class="sxs-lookup"><span data-stu-id="8df15-263">You will include in the solution an implementation of **IDependencyResolver** that will deal with the service retrieval by using Unity.</span></span> <span data-ttu-id="8df15-264">別のカスタム実装が含まれますが、 **IViewPageActivator**インターフェイス ビューの作成を解決するためです。</span><span class="sxs-lookup"><span data-stu-id="8df15-264">Then, you will include another custom implementation of **IViewPageActivator** interface that will solve the creation of the views.</span></span>

> [!NOTE]
> <span data-ttu-id="8df15-265">ASP.NET MVC 3 では、以降の依存関係の挿入の実装にサービスを登録するインターフェイスが簡略化されます。</span><span class="sxs-lookup"><span data-stu-id="8df15-265">Since ASP.NET MVC 3, the implementation for Dependency Injection had simplified the interfaces to register services.</span></span> <span data-ttu-id="8df15-266">**IDependencyResolver**と**IViewPageActivator**依存関係の挿入の ASP.NET MVC 3 の機能の一部であります。</span><span class="sxs-lookup"><span data-stu-id="8df15-266">**IDependencyResolver** and **IViewPageActivator** are part of ASP.NET MVC 3 features for Dependency Injection.</span></span>
> 
> <span data-ttu-id="8df15-267">**-IDependencyResolver**インターフェイスには、前の IMvcServiceLocator が置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="8df15-267">**- IDependencyResolver** interface replaces the previous IMvcServiceLocator.</span></span> <span data-ttu-id="8df15-268">IDependencyResolver を実装には、サービスまたはサービスのコレクションのインスタンスを返す必要があります。</span><span class="sxs-lookup"><span data-stu-id="8df15-268">Implementers of IDependencyResolver must return an instance of the service or a service collection.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> <span data-ttu-id="8df15-269">**-IViewPageActivator**インターフェイスには、ページの表示に依存関係の挿入を使用してインスタンス化きめの細かい制御が用意されています。</span><span class="sxs-lookup"><span data-stu-id="8df15-269">**- IViewPageActivator** interface provides more fine-grained control over how view pages are instantiated via dependency injection.</span></span> <span data-ttu-id="8df15-270">実装するクラス**IViewPageActivator**インターフェイスは、コンテキスト情報を使用してビューのインスタンスを作成できます。</span><span class="sxs-lookup"><span data-stu-id="8df15-270">The classes that implement **IViewPageActivator** interface can create view instances using context information.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. <span data-ttu-id="8df15-271">作成、/**ファクトリ**プロジェクトのルート フォルダー内のフォルダー。</span><span class="sxs-lookup"><span data-stu-id="8df15-271">Create the /**Factories** folder in the project's root folder.</span></span>
2. <span data-ttu-id="8df15-272">含める**CustomViewPageActivator.cs**からソリューションに**ソース/資産/** に**ファクトリ**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="8df15-272">Include **CustomViewPageActivator.cs** to your solution from **/Sources/Assets/** to **Factories** folder.</span></span> <span data-ttu-id="8df15-273">そのためには右クリックし、 **/Factories**フォルダーで、**追加 |既存項目の**選び**CustomViewPageActivator.cs**します。</span><span class="sxs-lookup"><span data-stu-id="8df15-273">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **CustomViewPageActivator.cs**.</span></span> <span data-ttu-id="8df15-274">このクラスは、実装、 **IViewPageActivator** Unity コンテナーを保持するインターフェイス。</span><span class="sxs-lookup"><span data-stu-id="8df15-274">This class implements the **IViewPageActivator** interface to hold the Unity Container.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > <span data-ttu-id="8df15-275">**CustomViewPageActivator** Unity コンテナーを使用して、ビューの作成の管理を担当します。</span><span class="sxs-lookup"><span data-stu-id="8df15-275">**CustomViewPageActivator** is responsible for managing the creation of a view by using a Unity container.</span></span>
3. <span data-ttu-id="8df15-276">含める**UnityDependencyResolver.cs**ファイルから **/ソース/資産**に **/Factories**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="8df15-276">Include **UnityDependencyResolver.cs** file from **/Sources/Assets** to **/Factories** folder.</span></span> <span data-ttu-id="8df15-277">そのためには右クリックし、 **/Factories**フォルダーで、**追加 |既存項目の**選び**UnityDependencyResolver.cs**ファイル。</span><span class="sxs-lookup"><span data-stu-id="8df15-277">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **UnityDependencyResolver.cs** file.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > <span data-ttu-id="8df15-278">**UnityDependencyResolver**クラスは、Unity のカスタム DependencyResolver します。</span><span class="sxs-lookup"><span data-stu-id="8df15-278">**UnityDependencyResolver** class is a custom DependencyResolver for Unity.</span></span> <span data-ttu-id="8df15-279">Unity コンテナー内でサービスが見つからない場合は、ベースの競合回避モジュールが invocated します。</span><span class="sxs-lookup"><span data-stu-id="8df15-279">When a service cannot be found inside the Unity container, the base resolver is invocated.</span></span>

<span data-ttu-id="8df15-280">次の実習で両方の実装は、サービスと、ビューの場所を知っているモデルに登録されます。</span><span class="sxs-lookup"><span data-stu-id="8df15-280">In the following task both implementations will be registered to let the model know the location of the services and the views.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a><span data-ttu-id="8df15-281">タスク 3 - Unity コンテナー内の依存関係の挿入に登録します。</span><span class="sxs-lookup"><span data-stu-id="8df15-281">Task 3 - Registering for Dependency Injection within Unity container</span></span>

<span data-ttu-id="8df15-282">このタスクでは、依存関係の挿入操作を作成する前、すべての操作を格納します。</span><span class="sxs-lookup"><span data-stu-id="8df15-282">In this task, you will put all the previous things together to make Dependency Injection work.</span></span>

<span data-ttu-id="8df15-283">ここまで、ソリューションは、次の要素があります。</span><span class="sxs-lookup"><span data-stu-id="8df15-283">Up to now your solution has the following elements:</span></span>

- <span data-ttu-id="8df15-284">A**参照**ビューから継承する**MyBaseClass**消費**MessageService**します。</span><span class="sxs-lookup"><span data-stu-id="8df15-284">A **Browse** View that inherits from **MyBaseClass** and consumes **MessageService**.</span></span>
- <span data-ttu-id="8df15-285">中間クラス -**MyBaseClass**-依存関係の挿入サービス インターフェイスの宣言を持ちます。</span><span class="sxs-lookup"><span data-stu-id="8df15-285">An intermediate class -**MyBaseClass**- that has dependency injection declared for the service interface.</span></span>
- <span data-ttu-id="8df15-286">サービス - **MessageService** - とそのインターフェイス**IMessageService**します。</span><span class="sxs-lookup"><span data-stu-id="8df15-286">A service - **MessageService** - and its interface **IMessageService**.</span></span>
- <span data-ttu-id="8df15-287">Unity - 使用するカスタム依存関係リゾルバー **UnityDependencyResolver** -サービスの取得を処理します。</span><span class="sxs-lookup"><span data-stu-id="8df15-287">A custom dependency resolver for Unity - **UnityDependencyResolver** - that deals with service retrieval.</span></span>
- <span data-ttu-id="8df15-288">ビュー ページ アクティベーター - **CustomViewPageActivator** -ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="8df15-288">A View Page activator - **CustomViewPageActivator** - that creates the page.</span></span>

<span data-ttu-id="8df15-289">挿入する**参照**ビュー、今すぐ登録するカスタム依存関係競合回避モジュール Unity コンテナーにします。</span><span class="sxs-lookup"><span data-stu-id="8df15-289">To inject **Browse** View, you will now register the custom dependency resolver in the Unity container.</span></span>

1. <span data-ttu-id="8df15-290">開いている**Bootstrapper.cs**ファイル。</span><span class="sxs-lookup"><span data-stu-id="8df15-290">Open **Bootstrapper.cs** file.</span></span>
2. <span data-ttu-id="8df15-291">インスタンスを登録**MessageService**サービスを初期化するために Unity コンテナーに。</span><span class="sxs-lookup"><span data-stu-id="8df15-291">Register an instance of **MessageService** into the Unity container to initialize the service:</span></span>

    <span data-ttu-id="8df15-292">(コード スニペット - *ASP.NET 依存関係注入ラボ - Ex02 - 登録メッセージ サービス*)</span><span class="sxs-lookup"><span data-stu-id="8df15-292">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register Message Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. <span data-ttu-id="8df15-293">参照を追加**MvcMusicStore.Factories**名前空間。</span><span class="sxs-lookup"><span data-stu-id="8df15-293">Add a reference to **MvcMusicStore.Factories** namespace.</span></span>

    <span data-ttu-id="8df15-294">(コード スニペット - *ASP.NET 依存関係の注入ラボ - Ex02 - ファクトリ Namespace*)</span><span class="sxs-lookup"><span data-stu-id="8df15-294">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Factories Namespace*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. <span data-ttu-id="8df15-295">登録**CustomViewPageActivator**として Unity コンテナーにビュー ページ アクティベーター。</span><span class="sxs-lookup"><span data-stu-id="8df15-295">Register **CustomViewPageActivator** as a View Page activator into the Unity container:</span></span>

    <span data-ttu-id="8df15-296">(コード スニペット - *ASP.NET 依存関係の注入ラボ - Ex02 - 登録 CustomViewPageActivator*)</span><span class="sxs-lookup"><span data-stu-id="8df15-296">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register CustomViewPageActivator*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. <span data-ttu-id="8df15-297">インスタンスでの ASP.NET MVC 4 の既定の依存関係競合回避モジュールを置き換えます**UnityDependencyResolver**します。</span><span class="sxs-lookup"><span data-stu-id="8df15-297">Replace ASP.NET MVC 4 default dependency resolver with an instance of **UnityDependencyResolver**.</span></span> <span data-ttu-id="8df15-298">これを行うには、置き換える**Initialise**メソッドの内容を次のコードに。</span><span class="sxs-lookup"><span data-stu-id="8df15-298">To do this, replace **Initialise** method content with the following code:</span></span>

    <span data-ttu-id="8df15-299">(コード スニペット - *ASP.NET 依存関係の挿入ラボ - Ex02 - 更新プログラムの依存関係リゾルバー*)</span><span class="sxs-lookup"><span data-stu-id="8df15-299">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Update Dependency Resolver*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > <span data-ttu-id="8df15-300">ASP.NET MVC には、既定の依存関係競合回避モジュールのクラスが用意されています。</span><span class="sxs-lookup"><span data-stu-id="8df15-300">ASP.NET MVC provides a default dependency resolver class.</span></span> <span data-ttu-id="8df15-301">Unity に対して作成した 1 つとして、カスタム依存関係競合回避モジュールを操作するには、この競合回避モジュールを交換する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8df15-301">To work with custom dependency resolvers as the one we have created for unity, this resolver has to be replaced.</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="8df15-302">タスク 4 - アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="8df15-302">Task 4 - Running the Application</span></span>

<span data-ttu-id="8df15-303">このタスクでストア ブラウザーが、サービスを利用して、取得したメッセージのイメージを示していますを確認するアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="8df15-303">In this task, you will run the application to verify that the Store Browser consumes the service and shows the image and the message retrieved:</span></span>

1. <span data-ttu-id="8df15-304">**F5** キーを押してアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="8df15-304">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="8df15-305">をクリックして**Rock**を参照し、[ジャンル] メニュー内方法、 **MessageService**ビューに挿入され、ウェルカム メッセージと、イメージが読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="8df15-305">Click **Rock** within the Genres Menu and see how the **MessageService** was injected to the view and loaded the welcome message and the image.</span></span> <span data-ttu-id="8df15-306">この例で入力は&quot; **Rock**&quot;:</span><span class="sxs-lookup"><span data-stu-id="8df15-306">In this example, we are entering to &quot;**Rock**&quot;:</span></span>

    <span data-ttu-id="8df15-307">![MVC Music Store - ビュー挿入](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - ビューの挿入")</span><span class="sxs-lookup"><span data-stu-id="8df15-307">![MVC Music Store - View Injection](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - View Injection")</span></span>

    <span data-ttu-id="8df15-308">*MVC Music Store - ビューの挿入*</span><span class="sxs-lookup"><span data-stu-id="8df15-308">*MVC Music Store - View Injection*</span></span>
3. <span data-ttu-id="8df15-309">ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="8df15-309">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a><span data-ttu-id="8df15-310">手順 3: アクション フィルターを挿入します。</span><span class="sxs-lookup"><span data-stu-id="8df15-310">Exercise 3: Injecting Action Filters</span></span>

<span data-ttu-id="8df15-311">前のハンズオン ラボで**カスタム アクション フィルター**フィルターのカスタマイズと挿入を使用してきた。</span><span class="sxs-lookup"><span data-stu-id="8df15-311">In the previous Hands-On lab **Custom Action Filters** you have worked with filters customization and injection.</span></span> <span data-ttu-id="8df15-312">この演習では、依存関係の注入に Unity コンテナーを使用してフィルターを挿入する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="8df15-312">In this exercise, you will learn how to inject filters with Dependency Injection by using the Unity container.</span></span> <span data-ttu-id="8df15-313">そのためにするソリューションに追加されます、ミュージック ストア サイトのアクティビティをトレースするカスタム アクション フィルター。</span><span class="sxs-lookup"><span data-stu-id="8df15-313">To do that, you will add to the Music Store solution a custom action filter that will trace the activity of the site.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a><span data-ttu-id="8df15-314">タスク 1 - ソリューションで追跡フィルターを含む</span><span class="sxs-lookup"><span data-stu-id="8df15-314">Task 1 - Including the Tracking Filter in the Solution</span></span>

<span data-ttu-id="8df15-315">このタスクでに含める音楽ストア イベントをトレースするカスタム アクション フィルター。</span><span class="sxs-lookup"><span data-stu-id="8df15-315">In this task, you will include in the Music Store a custom action filter to trace events.</span></span> <span data-ttu-id="8df15-316">概念を既に前の実習で扱うカスタム アクション フィルターとして&quot;カスタム アクション フィルター&quot;、このラボでの Assets フォルダーからフィルター クラスを含めるだけですし、し、Unity のフィルター プロバイダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="8df15-316">As custom action filter concepts are already treated in the previous Lab &quot;Custom Action Filters&quot;, you will just include the filter class from the Assets folder of this lab, and then create a Filter Provider for Unity:</span></span>

1. <span data-ttu-id="8df15-317">開く、**開始**ソリューション、 **Source\Ex03 - 挿入アクション Filter\Begin**フォルダー。</span><span class="sxs-lookup"><span data-stu-id="8df15-317">Open the **Begin** solution located in the **Source\Ex03 - Injecting Action Filter\Begin** folder.</span></span> <span data-ttu-id="8df15-318">使用を続ける可能性がありますそれ以外の場合、**エンド**ソリューションは、前の演習を完了して取得します。</span><span class="sxs-lookup"><span data-stu-id="8df15-318">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="8df15-319">指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。</span><span class="sxs-lookup"><span data-stu-id="8df15-319">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="8df15-320">これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。</span><span class="sxs-lookup"><span data-stu-id="8df15-320">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="8df15-321">**NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。</span><span class="sxs-lookup"><span data-stu-id="8df15-321">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="8df15-322">最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。</span><span class="sxs-lookup"><span data-stu-id="8df15-322">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="8df15-323">NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。</span><span class="sxs-lookup"><span data-stu-id="8df15-323">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="8df15-324">With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="8df15-324">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="8df15-325">これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8df15-325">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="8df15-326">詳細については、この記事を参照してください: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages)します。</span><span class="sxs-lookup"><span data-stu-id="8df15-326">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="8df15-327">含める**TraceActionFilter.cs**ファイルから **/ソース/資産**に**フィルター/** フォルダー。</span><span class="sxs-lookup"><span data-stu-id="8df15-327">Include **TraceActionFilter.cs** file from **/Sources/Assets** to **/Filters** folder.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > <span data-ttu-id="8df15-328">このカスタム アクション フィルターは、ASP.NET のトレースを実行します。</span><span class="sxs-lookup"><span data-stu-id="8df15-328">This custom action filter performs ASP.NET tracing.</span></span> <span data-ttu-id="8df15-329">チェックすることができます&quot;ASP.NET MVC 4 ローカルおよびアクション フィルターが動的&quot;ラボ他の参照。</span><span class="sxs-lookup"><span data-stu-id="8df15-329">You can check &quot;ASP.NET MVC 4 local and Dynamic Action Filters&quot; Lab for more reference.</span></span>
3. <span data-ttu-id="8df15-330">空のクラスを追加**FilterProvider.cs**フォルダー内のプロジェクトに  **/フィルター処理します。**</span><span class="sxs-lookup"><span data-stu-id="8df15-330">Add the empty class **FilterProvider.cs** to the project in the folder **/Filters.**</span></span>
4. <span data-ttu-id="8df15-331">追加、 **System.Web.Mvc**と**Microsoft.Practices.Unity**内の名前空間**FilterProvider.cs**します。</span><span class="sxs-lookup"><span data-stu-id="8df15-331">Add the **System.Web.Mvc** and **Microsoft.Practices.Unity** namespaces in **FilterProvider.cs**.</span></span>

    <span data-ttu-id="8df15-332">(コード スニペット - *ASP.NET 依存関係のインジェクション ラボ - Ex03 - フィルター プロバイダーが名前空間を追加する*)</span><span class="sxs-lookup"><span data-stu-id="8df15-332">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. <span data-ttu-id="8df15-333">クラスから継承するように**IFilterProvider**インターフェイス。</span><span class="sxs-lookup"><span data-stu-id="8df15-333">Make the class inherit from **IFilterProvider** Interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. <span data-ttu-id="8df15-334">追加、 **IUnityContainer**プロパティ、 **FilterProvider**クラス、し、コンテナーを割り当てるクラスのコンス トラクターを作成します。</span><span class="sxs-lookup"><span data-stu-id="8df15-334">Add a **IUnityContainer** property in the **FilterProvider** class, and then create a class constructor to assign the container.</span></span>

    <span data-ttu-id="8df15-335">(コード スニペット - *ASP.NET 依存関係の注入ラボ - Ex03 - フィルター プロバイダー コンス トラクター*)</span><span class="sxs-lookup"><span data-stu-id="8df15-335">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Constructor*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="8df15-336">フィルター プロバイダーのクラス コンス トラクターを作成しない、**新しい**内のオブジェクトします。</span><span class="sxs-lookup"><span data-stu-id="8df15-336">The filter provider class constructor is not creating a **new** object inside.</span></span> <span data-ttu-id="8df15-337">コンテナーは、パラメーターとして渡され、依存関係は、Unity によって解決されます。</span><span class="sxs-lookup"><span data-stu-id="8df15-337">The container is passed as a parameter, and the dependency is solved by Unity.</span></span>
7. <span data-ttu-id="8df15-338">**FilterProvider**クラス、メソッドを実装**GetFilters**から**IFilterProvider**インターフェイス。</span><span class="sxs-lookup"><span data-stu-id="8df15-338">In the **FilterProvider** class, implement the method **GetFilters** from **IFilterProvider** interface.</span></span>

    <span data-ttu-id="8df15-339">(コード スニペット - *ASP.NET 依存関係の注入ラボ - Ex03 - フィルター プロバイダー GetFilters*)</span><span class="sxs-lookup"><span data-stu-id="8df15-339">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider GetFilters*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a><span data-ttu-id="8df15-340">タスク 2 - 登録して、フィルターを有効にします。</span><span class="sxs-lookup"><span data-stu-id="8df15-340">Task 2 - Registering and Enabling the Filter</span></span>

<span data-ttu-id="8df15-341">このタスクでは、サイトの追跡を有効になります。</span><span class="sxs-lookup"><span data-stu-id="8df15-341">In this task, you will enable site tracking.</span></span> <span data-ttu-id="8df15-342">フィルターを登録するには、 **Bootstrapper.cs BuildUnityContainer**トレースを開始するメソッド。</span><span class="sxs-lookup"><span data-stu-id="8df15-342">To do that, you will register the filter in **Bootstrapper.cs BuildUnityContainer** method to start tracing:</span></span>

1. <span data-ttu-id="8df15-343">開いている**Web.config** System.Web グループで有効にするトレースの追跡とプロジェクトのルートにあります。</span><span class="sxs-lookup"><span data-stu-id="8df15-343">Open **Web.config** located in the project root and enable trace tracking at System.Web group.</span></span>

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. <span data-ttu-id="8df15-344">開いている**Bootstrapper.cs**プロジェクト ルートにあります。</span><span class="sxs-lookup"><span data-stu-id="8df15-344">Open **Bootstrapper.cs** at project root.</span></span>
3. <span data-ttu-id="8df15-345">参照を追加、 **MvcMusicStore.Filters**名前空間。</span><span class="sxs-lookup"><span data-stu-id="8df15-345">Add a reference to the **MvcMusicStore.Filters** namespace.</span></span>

    <span data-ttu-id="8df15-346">(コード スニペット - *ASP.NET 依存関係の注入ラボ - Ex03 - ブートス トラップ名前空間を追加する*)</span><span class="sxs-lookup"><span data-stu-id="8df15-346">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. <span data-ttu-id="8df15-347">選択、 **BuildUnityContainer**メソッドと Unity コンテナーでのフィルターを登録します。</span><span class="sxs-lookup"><span data-stu-id="8df15-347">Select the **BuildUnityContainer** method and register the filter in the Unity Container.</span></span> <span data-ttu-id="8df15-348">アクション フィルターとフィルター プロバイダーを登録する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8df15-348">You will have to register the filter provider as well as the action filter.</span></span>

    <span data-ttu-id="8df15-349">(コード スニペット - *ASP.NET 依存関係の注入ラボ - Ex03 - 登録 FilterProvider と ActionFilter*)</span><span class="sxs-lookup"><span data-stu-id="8df15-349">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Register FilterProvider and ActionFilter*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="8df15-350">タスク 3 - アプリケーションの実行</span><span class="sxs-lookup"><span data-stu-id="8df15-350">Task 3 - Running the Application</span></span>

<span data-ttu-id="8df15-351">このタスクでは、アプリケーションを実行し、カスタム アクション フィルターが、アクティビティをトレースしているテストします。</span><span class="sxs-lookup"><span data-stu-id="8df15-351">In this task, you will run the application and test that the custom action filter is tracing the activity:</span></span>

1. <span data-ttu-id="8df15-352">**F5** キーを押してアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="8df15-352">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="8df15-353">クリックして**Rock**ジャンル メニュー内で。</span><span class="sxs-lookup"><span data-stu-id="8df15-353">Click **Rock** within the Genres Menu.</span></span> <span data-ttu-id="8df15-354">する場合は、ジャンルの詳細を参照できます。</span><span class="sxs-lookup"><span data-stu-id="8df15-354">You can browse to more genres if you want to.</span></span>

    <span data-ttu-id="8df15-355">![ミュージック ストア](aspnet-mvc-4-dependency-injection/_static/image11.png "ミュージック ストア")</span><span class="sxs-lookup"><span data-stu-id="8df15-355">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span></span>

    <span data-ttu-id="8df15-356">*Music Store*</span><span class="sxs-lookup"><span data-stu-id="8df15-356">*Music Store*</span></span>
3. <span data-ttu-id="8df15-357">参照する **/Trace.axd** ] ページの [クリックして、アプリケーション トレースを表示する**詳細を表示する**します。</span><span class="sxs-lookup"><span data-stu-id="8df15-357">Browse to **/Trace.axd** to see the Application Trace page, and then click **View Details**.</span></span>

    <span data-ttu-id="8df15-358">![アプリケーション トレース ログ](aspnet-mvc-4-dependency-injection/_static/image12.png "アプリケーション トレース ログ")</span><span class="sxs-lookup"><span data-stu-id="8df15-358">![Application Trace Log](aspnet-mvc-4-dependency-injection/_static/image12.png "Application Trace Log")</span></span>

    <span data-ttu-id="8df15-359">*アプリケーション トレース ログ*</span><span class="sxs-lookup"><span data-stu-id="8df15-359">*Application Trace Log*</span></span>

    <span data-ttu-id="8df15-360">![アプリケーション トレース - 要求の詳細](aspnet-mvc-4-dependency-injection/_static/image13.png "アプリケーション トレース - 要求の詳細")</span><span class="sxs-lookup"><span data-stu-id="8df15-360">![Application Trace - Request Details](aspnet-mvc-4-dependency-injection/_static/image13.png "Application Trace - Request Details")</span></span>

    <span data-ttu-id="8df15-361">*アプリケーション トレース - 要求の詳細*</span><span class="sxs-lookup"><span data-stu-id="8df15-361">*Application Trace - Request Details*</span></span>
4. <span data-ttu-id="8df15-362">ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="8df15-362">Close the browser.</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="8df15-363">まとめ</span><span class="sxs-lookup"><span data-stu-id="8df15-363">Summary</span></span>

<span data-ttu-id="8df15-364">このハンズオン ラボは、Unity は NuGet パッケージを使用して統合することにより、ASP.NET MVC 4 で依存関係の挿入を使用する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="8df15-364">By completing this Hands-On Lab you have learned how to use Dependency Injection in ASP.NET MVC 4 by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="8df15-365">これを実現するには、コント ローラー、ビュー、およびアクション フィルター内で依存関係の挿入を使用しています。</span><span class="sxs-lookup"><span data-stu-id="8df15-365">To achieve that, you have used Dependency Injection inside controllers, views and action filters.</span></span>

<span data-ttu-id="8df15-366">次の概念がについて説明します。</span><span class="sxs-lookup"><span data-stu-id="8df15-366">The following concepts were covered:</span></span>

- <span data-ttu-id="8df15-367">ASP.NET MVC 4 依存関係の注入機能</span><span class="sxs-lookup"><span data-stu-id="8df15-367">ASP.NET MVC 4 Dependency Injection features</span></span>
- <span data-ttu-id="8df15-368">Unity.Mvc3 NuGet パッケージを使用して unity の統合</span><span class="sxs-lookup"><span data-stu-id="8df15-368">Unity integration using Unity.Mvc3 NuGet Package</span></span>
- <span data-ttu-id="8df15-369">コント ローラーで依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="8df15-369">Dependency Injection in Controllers</span></span>
- <span data-ttu-id="8df15-370">ビューの依存関係挿入</span><span class="sxs-lookup"><span data-stu-id="8df15-370">Dependency Injection in Views</span></span>
- <span data-ttu-id="8df15-371">アクション フィルターの依存関係の挿入</span><span class="sxs-lookup"><span data-stu-id="8df15-371">Dependency injection of Action Filters</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="8df15-372">付録 a: Installing Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="8df15-372">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="8df15-373">インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、 **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="8df15-373">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="8df15-374">次の手順をインストールするために必要な手順をガイドします。 *Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*します。</span><span class="sxs-lookup"><span data-stu-id="8df15-374">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="8df15-375">移動して[ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)します。</span><span class="sxs-lookup"><span data-stu-id="8df15-375">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="8df15-376">または、既に Web Platform Installer をインストールした場合を開くことも、製品を検索して、 &quot; <em>Visual Studio Express 2012 for Web と Windows Azure SDK</em>&quot;します。</span><span class="sxs-lookup"><span data-stu-id="8df15-376">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="8df15-377">をクリックして**を今すぐインストール**します。</span><span class="sxs-lookup"><span data-stu-id="8df15-377">Click on **Install Now**.</span></span> <span data-ttu-id="8df15-378">ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="8df15-378">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="8df15-379">1 回**Web Platform Installer**を開くと、クリックして**インストール**セットアップを開始します。</span><span class="sxs-lookup"><span data-stu-id="8df15-379">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="8df15-380">![Visual Studio Express のインストール](aspnet-mvc-4-dependency-injection/_static/image14.png "Visual Studio Express のインストール")</span><span class="sxs-lookup"><span data-stu-id="8df15-380">![Install Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="8df15-381">*Visual Studio Express をインストールします。*</span><span class="sxs-lookup"><span data-stu-id="8df15-381">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="8df15-382">すべての製品のライセンスと使用条件を読み、クリックして**同意**を続行します。</span><span class="sxs-lookup"><span data-stu-id="8df15-382">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![ライセンス条項に同意](aspnet-mvc-4-dependency-injection/_static/image15.png)

    <span data-ttu-id="8df15-384">*ライセンス条項に同意*</span><span class="sxs-lookup"><span data-stu-id="8df15-384">*Accepting the license terms*</span></span>
5. <span data-ttu-id="8df15-385">ダウンロードとインストール プロセスが完了するまで待機します。</span><span class="sxs-lookup"><span data-stu-id="8df15-385">Wait until the downloading and installation process completes.</span></span>

    ![インストールの進行状況](aspnet-mvc-4-dependency-injection/_static/image16.png)

    <span data-ttu-id="8df15-387">*インストールの進行状況*</span><span class="sxs-lookup"><span data-stu-id="8df15-387">*Installation progress*</span></span>
6. <span data-ttu-id="8df15-388">インストールが完了したら、クリックして**完了**します。</span><span class="sxs-lookup"><span data-stu-id="8df15-388">When the installation completes, click **Finish**.</span></span>

    ![インストールが完了しました](aspnet-mvc-4-dependency-injection/_static/image17.png)

    <span data-ttu-id="8df15-390">*インストールが完了しました*</span><span class="sxs-lookup"><span data-stu-id="8df15-390">*Installation completed*</span></span>
7. <span data-ttu-id="8df15-391">クリックして**終了**Web Platform Installer を閉じます。</span><span class="sxs-lookup"><span data-stu-id="8df15-391">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="8df15-392">Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、 をクリックし、 **VS Express for Web**並べて表示します。</span><span class="sxs-lookup"><span data-stu-id="8df15-392">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web のタイル](aspnet-mvc-4-dependency-injection/_static/image18.png)

    <span data-ttu-id="8df15-394">*VS Express for Web のタイル*</span><span class="sxs-lookup"><span data-stu-id="8df15-394">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="8df15-395">付録 b: コード スニペットの使用</span><span class="sxs-lookup"><span data-stu-id="8df15-395">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="8df15-396">コードのスニペットでは、指先ひとつで必要なすべてのコードがあります。</span><span class="sxs-lookup"><span data-stu-id="8df15-396">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="8df15-397">ラボ ドキュメントがわかりますだけをいつ使用できる、次の図に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="8df15-397">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="8df15-398">![Visual Studio コード スニペットを使用して、プロジェクトにコードを挿入する](aspnet-mvc-4-dependency-injection/_static/image19.png "プロジェクトにコードを挿入するコード スニペットを Visual Studio の使用")</span><span class="sxs-lookup"><span data-stu-id="8df15-398">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-dependency-injection/_static/image19.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="8df15-399">*Visual Studio コード スニペットを使用して、プロジェクトにコードを挿入するには*</span><span class="sxs-lookup"><span data-stu-id="8df15-399">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="8df15-400">***キーボード (c# のみ) を使用するコード スニペットを追加するには***</span><span class="sxs-lookup"><span data-stu-id="8df15-400">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="8df15-401">コードを挿入するには、カーソルを置きます。</span><span class="sxs-lookup"><span data-stu-id="8df15-401">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="8df15-402">スニペットの名前 (なし、スペースやハイフン) の入力を開始します。</span><span class="sxs-lookup"><span data-stu-id="8df15-402">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="8df15-403">スニペットの名前に一致する IntelliSense の表示を確認します。</span><span class="sxs-lookup"><span data-stu-id="8df15-403">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="8df15-404">適切なスニペットを選択します (または全体のスニペットの名前が選択されるまで」と入力してください)。</span><span class="sxs-lookup"><span data-stu-id="8df15-404">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="8df15-405">カーソル位置にスニペットを挿入するには、2 回、Tab キーを押します。</span><span class="sxs-lookup"><span data-stu-id="8df15-405">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="8df15-406">![スニペットの名前の入力を開始](aspnet-mvc-4-dependency-injection/_static/image20.png "スニペット名の入力を開始")</span><span class="sxs-lookup"><span data-stu-id="8df15-406">![Start typing the snippet name](aspnet-mvc-4-dependency-injection/_static/image20.png "Start typing the snippet name")</span></span>

<span data-ttu-id="8df15-407">*スニペットの名前の入力を開始します。*</span><span class="sxs-lookup"><span data-stu-id="8df15-407">*Start typing the snippet name*</span></span>

<span data-ttu-id="8df15-408">![強調表示されているスニペットを選択して Tab キーを押して](aspnet-mvc-4-dependency-injection/_static/image21.png "キーを押してタブが強調表示されているスニペットを選択するには")</span><span class="sxs-lookup"><span data-stu-id="8df15-408">![Press Tab to select the highlighted snippet](aspnet-mvc-4-dependency-injection/_static/image21.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="8df15-409">*Tab キーを押して、強調表示されているスニペットを選択します*</span><span class="sxs-lookup"><span data-stu-id="8df15-409">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="8df15-410">![キーを押して タブで再度とスニペットが展開](aspnet-mvc-4-dependency-injection/_static/image22.png "キーを押して タブで再度とスニペットが展開されます")</span><span class="sxs-lookup"><span data-stu-id="8df15-410">![Press Tab again and the snippet will expand](aspnet-mvc-4-dependency-injection/_static/image22.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="8df15-411">*キーを押して タブで再度とスニペットが展開されます。*</span><span class="sxs-lookup"><span data-stu-id="8df15-411">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="8df15-412">***(C#、Visual Basic および XML) にマウスを使用するコード スニペットを追加する***1。</span><span class="sxs-lookup"><span data-stu-id="8df15-412">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="8df15-413">コード スニペットを挿入するを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="8df15-413">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="8df15-414">選択**スニペットの挿入**続けて**マイ コード スニペット**します。</span><span class="sxs-lookup"><span data-stu-id="8df15-414">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="8df15-415">クリックして、一覧から関連するスニペットを選択します。</span><span class="sxs-lookup"><span data-stu-id="8df15-415">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="8df15-416">![コード スニペットを挿入し、スニペットの挿入を選択する場所を右クリックして](aspnet-mvc-4-dependency-injection/_static/image23.png "コード スニペットを挿入し、スニペットの挿入を選択する場所を右クリック")</span><span class="sxs-lookup"><span data-stu-id="8df15-416">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-dependency-injection/_static/image23.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="8df15-417">*コード スニペットを挿入して、スニペットの挿入先の選択します。*</span><span class="sxs-lookup"><span data-stu-id="8df15-417">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="8df15-418">![クリックして、一覧から関連するスニペットを選択](aspnet-mvc-4-dependency-injection/_static/image24.png "クリックして、一覧から関連するスニペットを選択")</span><span class="sxs-lookup"><span data-stu-id="8df15-418">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-dependency-injection/_static/image24.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="8df15-419">*クリックして、一覧から関連するスニペットを選択します。*</span><span class="sxs-lookup"><span data-stu-id="8df15-419">*Pick the relevant snippet from the list, by clicking on it*</span></span>
