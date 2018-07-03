---
uid: mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-vb
title: '繰り返し #6 – テスト駆動開発 (VB) を使用して、|Microsoft Docs'
author: microsoft
description: この 6 番目のイテレーションでは、アプリケーションに新しい機能を追加には、まず単体テストの記述、単体テストに対してコードを記述します。 このイテレーションにしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: e1fd226f-3f8e-4575-a179-5c75b240333d
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-vb
msc.type: authoredcontent
ms.openlocfilehash: 3b55bb689a5a6dfdb07b02a7d595244f886f0fd2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377247"
---
<a name="iteration-6--use-test-driven-development-vb"></a><span data-ttu-id="818f9-104">繰り返し #6 – テスト駆動開発 (VB) を使用します。</span><span class="sxs-lookup"><span data-stu-id="818f9-104">Iteration #6 – Use test-driven development (VB)</span></span>
====================
<span data-ttu-id="818f9-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="818f9-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="818f9-106">コードをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="818f9-106">Download Code</span></span>](iteration-6-use-test-driven-development-vb/_static/contactmanager_6_vb1.zip)

> <span data-ttu-id="818f9-107">この 6 番目のイテレーションでは、アプリケーションに新しい機能を追加には、まず単体テストの記述、単体テストに対してコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="818f9-107">In this sixth iteration, we add new functionality to our application by writing unit tests first and writing code against the unit tests.</span></span> <span data-ttu-id="818f9-108">このイテレーションは、連絡先グループを追加します。</span><span class="sxs-lookup"><span data-stu-id="818f9-108">In this iteration, we add contact groups.</span></span>


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a><span data-ttu-id="818f9-109">ASP.NET MVC 連絡先管理アプリケーション (VB) の構築</span><span class="sxs-lookup"><span data-stu-id="818f9-109">Building a Contact Management ASP.NET MVC Application (VB)</span></span>
  

<span data-ttu-id="818f9-110">このチュートリアル シリーズでは、開始から終了に全体連絡先管理アプリケーションを構築します。</span><span class="sxs-lookup"><span data-stu-id="818f9-110">In this series of tutorials, we build an entire Contact Management application from start to finish.</span></span> <span data-ttu-id="818f9-111">Contact Manager アプリケーションは、ユーザーの一覧については店舗連絡先情報の名前、電話番号、電子メール アドレスにするようにことができます。</span><span class="sxs-lookup"><span data-stu-id="818f9-111">The Contact Manager application enables you to store contact information - names, phone numbers and email addresses - for a list of people.</span></span>

<span data-ttu-id="818f9-112">複数のイテレーションにおける、アプリケーションを構築します。</span><span class="sxs-lookup"><span data-stu-id="818f9-112">We build the application over multiple iterations.</span></span> <span data-ttu-id="818f9-113">反復処理ごとに、アプリケーション徐々 に向上します。</span><span class="sxs-lookup"><span data-stu-id="818f9-113">With each iteration, we gradually improve the application.</span></span> <span data-ttu-id="818f9-114">この複数のイテレーションのアプローチの目的は、各変更の理由を理解するためです。</span><span class="sxs-lookup"><span data-stu-id="818f9-114">The goal of this multiple iteration approach is to enable you to understand the reason for each change.</span></span>

- <span data-ttu-id="818f9-115">繰り返し #1 - は、アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="818f9-115">Iteration #1 - Create the application.</span></span> <span data-ttu-id="818f9-116">最初のイテレーションを作成、連絡先マネージャー最も簡単な方法で考えられるします。</span><span class="sxs-lookup"><span data-stu-id="818f9-116">In the first iteration, we create the Contact Manager in the simplest way possible.</span></span> <span data-ttu-id="818f9-117">基本的なデータベース操作のサポートを追加します: 作成、読み取り、更新、および削除 (CRUD)。</span><span class="sxs-lookup"><span data-stu-id="818f9-117">We add support for basic database operations: Create, Read, Update, and Delete (CRUD).</span></span>

- <span data-ttu-id="818f9-118">繰り返し #2 - は、素敵に見えるアプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="818f9-118">Iteration #2 - Make the application look nice.</span></span> <span data-ttu-id="818f9-119">このイテレーションで、アプリケーションの見た目を向上させる、既定の ASP.NET MVC ビュー マスター ページを変更し、カスケード スタイル シート。</span><span class="sxs-lookup"><span data-stu-id="818f9-119">In this iteration, we improve the appearance of the application by modifying the default ASP.NET MVC view master page and cascading style sheet.</span></span>

- <span data-ttu-id="818f9-120">繰り返し #3 - フォーム検証を追加します。</span><span class="sxs-lookup"><span data-stu-id="818f9-120">Iteration #3 - Add form validation.</span></span> <span data-ttu-id="818f9-121">3 番目のイテレーションでは、基本的なフォーム検証を追加します。</span><span class="sxs-lookup"><span data-stu-id="818f9-121">In the third iteration, we add basic form validation.</span></span> <span data-ttu-id="818f9-122">ユーザーは、必要なフォームのフィールドを完了しなくても、フォームを送信できないようにようにします。</span><span class="sxs-lookup"><span data-stu-id="818f9-122">We prevent people from submitting a form without completing required form fields.</span></span> <span data-ttu-id="818f9-123">また、電子メール アドレスと電話番号を検証しました。</span><span class="sxs-lookup"><span data-stu-id="818f9-123">We also validate email addresses and phone numbers.</span></span>

- <span data-ttu-id="818f9-124">繰り返し #4 - は、アプリケーションを疎結合を作成します。</span><span class="sxs-lookup"><span data-stu-id="818f9-124">Iteration #4 - Make the application loosely coupled.</span></span> <span data-ttu-id="818f9-125">この 3 番目のイテレーションで、保守し、Contact Manager アプリケーションの変更を容易にできるようにするソフトウェア設計パターンをいくつかの利点を実行します。</span><span class="sxs-lookup"><span data-stu-id="818f9-125">In this third iteration, we take advantage of several software design patterns to make it easier to maintain and modify the Contact Manager application.</span></span> <span data-ttu-id="818f9-126">たとえば、Repository パターンと依存関係の注入パターンを使用するようにアプリケーションをリファクタリングします。</span><span class="sxs-lookup"><span data-stu-id="818f9-126">For example, we refactor our application to use the Repository pattern and the Dependency Injection pattern.</span></span>

- <span data-ttu-id="818f9-127">繰り返し #5 - 単体テストを作成します。</span><span class="sxs-lookup"><span data-stu-id="818f9-127">Iteration #5 - Create unit tests.</span></span> <span data-ttu-id="818f9-128">5 番目のイテレーションでアプリケーションと簡単に維持単体テストを追加して変更します。</span><span class="sxs-lookup"><span data-stu-id="818f9-128">In the fifth iteration, we make our application easier to maintain and modify by adding unit tests.</span></span> <span data-ttu-id="818f9-129">データ モデル クラスの模擬テストを実行し、コント ローラーと検証ロジックの単体テストをビルドします。</span><span class="sxs-lookup"><span data-stu-id="818f9-129">We mock our data model classes and build unit tests for our controllers and validation logic.</span></span>

- <span data-ttu-id="818f9-130">繰り返し #6 - は、テスト駆動開発を使用します。</span><span class="sxs-lookup"><span data-stu-id="818f9-130">Iteration #6 - Use test-driven development.</span></span> <span data-ttu-id="818f9-131">この 6 番目のイテレーションでは、アプリケーションに新しい機能を追加には、まず単体テストの記述、単体テストに対してコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="818f9-131">In this sixth iteration, we add new functionality to our application by writing unit tests first and writing code against the unit tests.</span></span> <span data-ttu-id="818f9-132">このイテレーションは、連絡先グループを追加します。</span><span class="sxs-lookup"><span data-stu-id="818f9-132">In this iteration, we add contact groups.</span></span>

- <span data-ttu-id="818f9-133">繰り返し #7 - Ajax 機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="818f9-133">Iteration #7 - Add Ajax functionality.</span></span> <span data-ttu-id="818f9-134">7 番目のイテレーションで改良、応答性と、アプリケーションのパフォーマンスの Ajax のサポートを追加します。</span><span class="sxs-lookup"><span data-stu-id="818f9-134">In the seventh iteration, we improve the responsiveness and performance of our application by adding support for Ajax.</span></span>

## <a name="this-iteration"></a><span data-ttu-id="818f9-135">このイテレーション</span><span class="sxs-lookup"><span data-stu-id="818f9-135">This Iteration</span></span>

<span data-ttu-id="818f9-136">Contact Manager アプリケーションの前のイテレーションでは、コードの安全策を提供する単体テストを作成しました。</span><span class="sxs-lookup"><span data-stu-id="818f9-136">In the previous iteration of the Contact Manager application, we created unit tests to provide a safety net for our code.</span></span> <span data-ttu-id="818f9-137">単体テストを作成するための意図は、コードを変更に柔軟に対応することでした。</span><span class="sxs-lookup"><span data-stu-id="818f9-137">The motivation for creating the unit tests was to make our code more resilient to change.</span></span> <span data-ttu-id="818f9-138">インプレース単体テストでさいわいにも、コードを変更をすぐに既存の機能が崩れているかどうかを把握できます。</span><span class="sxs-lookup"><span data-stu-id="818f9-138">With unit tests in place, we can happily make any change to our code and immediately know whether we have broken existing functionality.</span></span>

<span data-ttu-id="818f9-139">このイテレーションでは、まったく別の目的で単体テストを使用します。</span><span class="sxs-lookup"><span data-stu-id="818f9-139">In this iteration, we use unit tests for an entirely different purpose.</span></span> <span data-ttu-id="818f9-140">このイテレーションでと呼ばれるアプリケーション設計理念の一部として単体テストを使用します*テスト駆動開発*します。</span><span class="sxs-lookup"><span data-stu-id="818f9-140">In this iteration, we use unit tests as part of an application design philosophy called *test-driven development*.</span></span> <span data-ttu-id="818f9-141">テスト駆動開発を練習するときは、まずテストを記述し、テストに対してコードを書き込みます。</span><span class="sxs-lookup"><span data-stu-id="818f9-141">When you practice test-driven development, you write tests first and then write code against the tests.</span></span>

<span data-ttu-id="818f9-142">正確には、テスト駆動開発を実践している場合があるコードを作成するときに完了する 3 つの手順 (赤/緑/リファクタリング)。</span><span class="sxs-lookup"><span data-stu-id="818f9-142">More precisely, when practicing test-driven development, there are three steps that you complete when creating code (Red/ Green/Refactor):</span></span>

1. <span data-ttu-id="818f9-143">(赤) に失敗した単体テストを記述します。</span><span class="sxs-lookup"><span data-stu-id="818f9-143">Write a unit test that fails (Red)</span></span>
2. <span data-ttu-id="818f9-144">単体テスト (緑) に合格するコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="818f9-144">Write code that passes the unit test (Green)</span></span>
3. <span data-ttu-id="818f9-145">(リファクタリング)、コードをリファクタリングします。</span><span class="sxs-lookup"><span data-stu-id="818f9-145">Refactor your code (Refactor)</span></span>

<span data-ttu-id="818f9-146">最初に、単体テストを記述します。</span><span class="sxs-lookup"><span data-stu-id="818f9-146">First, you write the unit test.</span></span> <span data-ttu-id="818f9-147">単体テストでは、動作する方法、コードが期待どおりの意図したものを表現する必要があります。</span><span class="sxs-lookup"><span data-stu-id="818f9-147">The unit test should express your intention for how you expect your code to behave.</span></span> <span data-ttu-id="818f9-148">最初に、単体テストを作成するときに、単体テストは失敗します。</span><span class="sxs-lookup"><span data-stu-id="818f9-148">When you first create the unit test, the unit test should fail.</span></span> <span data-ttu-id="818f9-149">テストに合格するアプリケーション コードを記述していないために、テストが失敗する必要があります。</span><span class="sxs-lookup"><span data-stu-id="818f9-149">The test should fail because you have not yet written any application code that satisfies the test.</span></span>

<span data-ttu-id="818f9-150">次に、単体テストが成功するためのに十分なコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="818f9-150">Next, you write just enough code in order for the unit test to pass.</span></span> <span data-ttu-id="818f9-151">目標は、laziest、sloppiest 可能な最速の方法でコードを記述することです。</span><span class="sxs-lookup"><span data-stu-id="818f9-151">The goal is to write the code in the laziest, sloppiest and fastest possible way.</span></span> <span data-ttu-id="818f9-152">アプリケーションのアーキテクチャについて考える時間がかからなく必要があります。</span><span class="sxs-lookup"><span data-stu-id="818f9-152">You should not waste time thinking about the architecture of your application.</span></span> <span data-ttu-id="818f9-153">代わりに、単体テストで表現するという意図を満たすために必要なコード量が最小限の作成に集中する必要があります。</span><span class="sxs-lookup"><span data-stu-id="818f9-153">Instead, you should focus on writing the minimal amount of code necessary to satisfy the intention expressed by the unit test.</span></span>

<span data-ttu-id="818f9-154">最後に、十分なコードを記述した後は、前に戻るし、アプリケーションの全体的なアーキテクチャを検討してください。</span><span class="sxs-lookup"><span data-stu-id="818f9-154">Finally, after you have written enough code, you can step back and consider the overall architecture of your application.</span></span> <span data-ttu-id="818f9-155">(リファクタリング) を書き直す場合、この手順でソフトウェアの設計を活用して、コード パターン - リポジトリ パターン - など、コードが保守しやすくなります。</span><span class="sxs-lookup"><span data-stu-id="818f9-155">In this step, you rewrite (refactor) your code by taking advantage of software design patterns -- such as the repository pattern -- so that your code is more maintainable.</span></span> <span data-ttu-id="818f9-156">この手順では、コードは、コードが単体テストでカバーされるため、安心書き直すことができます。</span><span class="sxs-lookup"><span data-stu-id="818f9-156">You can fearlessly rewrite your code in this step because your code is covered by unit tests.</span></span>

<span data-ttu-id="818f9-157">テスト駆動開発を実践に起因する多くの利点があります。</span><span class="sxs-lookup"><span data-stu-id="818f9-157">There are many benefits that result from practicing test-driven development.</span></span> <span data-ttu-id="818f9-158">まず、テスト駆動開発を強制的に実際に書き込まれる必要のあるコードに専念すること。</span><span class="sxs-lookup"><span data-stu-id="818f9-158">First, test-driven development forces you to focus on code that actually needs to be written.</span></span> <span data-ttu-id="818f9-159">常に特定のテストに合格するための十分なコード記述に注目している、ためから、雑草を歩き回っていると、膨大な量の利用しないコードを記述できません。</span><span class="sxs-lookup"><span data-stu-id="818f9-159">Because you are constantly focused on just writing enough code to pass a particular test, you are prevented from wandering into the weeds and writing massive amounts of code that you will never use.</span></span>

<span data-ttu-id="818f9-160">第 2 に、「最初のテスト」の設計方法論は、コードの使用方法の観点からコードを記述することを強制します。</span><span class="sxs-lookup"><span data-stu-id="818f9-160">Second, a "test first" design methodology forces you to write code from the perspective of how your code will be used.</span></span> <span data-ttu-id="818f9-161">つまり、テスト駆動開発を実践するには、ときに常に、テストを作成するユーザーの観点から。</span><span class="sxs-lookup"><span data-stu-id="818f9-161">In other words, when practicing test-driven development, you are constantly writing your tests from a user perspective.</span></span> <span data-ttu-id="818f9-162">そのため、テスト駆動開発が簡潔でわかりやすくし Api にされることができます。</span><span class="sxs-lookup"><span data-stu-id="818f9-162">Therefore, test-driven development can result in cleaner and more understandable APIs.</span></span>

<span data-ttu-id="818f9-163">最後に、テスト駆動開発は、通常、アプリケーションの作成のプロセスの一部として単体テストを作成することを強制します。</span><span class="sxs-lookup"><span data-stu-id="818f9-163">Finally, test-driven development forces you to write unit tests as part of the normal process of writing an application.</span></span> <span data-ttu-id="818f9-164">プロジェクトの期限が近づくとテストは通常、ウィンドウが最初に行うことです。</span><span class="sxs-lookup"><span data-stu-id="818f9-164">As a project deadline approaches, testing is typically the first thing that goes out the window.</span></span> <span data-ttu-id="818f9-165">テスト駆動開発、実践しているときにその一方がテスト駆動開発は、アプリケーションをビルド プロセスに単体テストを中央ため単体テストの記述に関する好する可能性が高くなります。</span><span class="sxs-lookup"><span data-stu-id="818f9-165">When practicing test-driven development, on the other hand, you are more likely to be virtuous about writing unit tests because test-driven development makes unit tests central to the process of building an application.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="818f9-166">Michael Feathers の書籍を読むことを勧めはテスト駆動開発の詳細については、**レガシー コードを効率的に作業**します。</span><span class="sxs-lookup"><span data-stu-id="818f9-166">To learn more about test-driven development, I recommend that you read Michael Feathers book **Working Effectively with Legacy Code**.</span></span>


<span data-ttu-id="818f9-167">このイテレーションでは、Contact Manager アプリケーションに新しい機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="818f9-167">In this iteration, we add a new feature to our Contact Manager application.</span></span> <span data-ttu-id="818f9-168">連絡先グループのサポートを追加します。</span><span class="sxs-lookup"><span data-stu-id="818f9-168">We add support for Contact Groups.</span></span> <span data-ttu-id="818f9-169">ビジネスなどのカテゴリに連絡先を整理する連絡先グループと友人のグループを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="818f9-169">You can use Contact Groups to organize your contacts into categories such as Business and Friend groups.</span></span>

<span data-ttu-id="818f9-170">この新しい機能のテスト駆動開発のプロセスに従って、アプリケーションに追加します。</span><span class="sxs-lookup"><span data-stu-id="818f9-170">We'll add this new functionality to our application by following a process of test-driven development.</span></span> <span data-ttu-id="818f9-171">最初に、単体テストを記述し、すべてのこれらのテストに対して、コードを記述します。</span><span class="sxs-lookup"><span data-stu-id="818f9-171">We'll write our unit tests first and we'll write all of our code against these tests.</span></span>

## <a name="what-gets-tested"></a><span data-ttu-id="818f9-172">どのようなテストを取得します</span><span class="sxs-lookup"><span data-stu-id="818f9-172">What Gets Tested</span></span>

<span data-ttu-id="818f9-173">前のイテレーションで説明したように通常データ アクセス ロジックの単体テストを記述したりしないロジックを表示します。</span><span class="sxs-lookup"><span data-stu-id="818f9-173">As we discussed in the previous iteration, you typically do not write unit tests for data access logic or view logic.</span></span> <span data-ttu-id="818f9-174">比較的低速の操作は、データベースにアクセスするため、データ アクセス ロジックの t 書き込み単体テストはありません。</span><span class="sxs-lookup"><span data-stu-id="818f9-174">You don t write unit tests for data access logic because accessing a database is a relatively slow operation.</span></span> <span data-ttu-id="818f9-175">比較的低速の操作には、web サーバーを開始して、ビューにアクセスする必要があるために、ビュー ロジックの t 書き込み単体テストがありません。</span><span class="sxs-lookup"><span data-stu-id="818f9-175">You don t write unit tests for view logic because accessing a view requires spinning up a web server which is a relatively slow operation.</span></span> <span data-ttu-id="818f9-176">T べきでは、テスト実行できる何度も高速な場合を除き、単体テストを記述します。</span><span class="sxs-lookup"><span data-stu-id="818f9-176">You shouldn t write a unit test unless the test can be executed over and over again very fast</span></span>

<span data-ttu-id="818f9-177">テスト駆動開発は、単体テストによって主導別、ので注目最初にコント ローラーとビジネス ロジックを記述します。</span><span class="sxs-lookup"><span data-stu-id="818f9-177">Because test-driven development is driven by unit tests, we focus initially on writing controller and business logic.</span></span> <span data-ttu-id="818f9-178">私たちは、データベースまたはビューを変更しないでください。</span><span class="sxs-lookup"><span data-stu-id="818f9-178">We avoid touching the database or views.</span></span> <span data-ttu-id="818f9-179">T 獲得したデータベースを変更したり、このチュートリアルの最後までビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="818f9-179">We won t modify the database or create our views until the very end of this tutorial.</span></span> <span data-ttu-id="818f9-180">まず何をテストすることができます。</span><span class="sxs-lookup"><span data-stu-id="818f9-180">We start with what can be tested.</span></span>

## <a name="creating-user-stories"></a><span data-ttu-id="818f9-181">ユーザー ストーリーの作成</span><span class="sxs-lookup"><span data-stu-id="818f9-181">Creating User Stories</span></span>

<span data-ttu-id="818f9-182">テスト駆動開発を実践するにはとき、に、テストを記述することで常にで開始します。</span><span class="sxs-lookup"><span data-stu-id="818f9-182">When practicing test-driven development, you always start by writing a test.</span></span> <span data-ttu-id="818f9-183">これは、直後に疑問が出てきます。 最初に記述するには、どのようなテストを決定するでしょうか。</span><span class="sxs-lookup"><span data-stu-id="818f9-183">This immediately raises the question: How do you decide what test to write first?</span></span> <span data-ttu-id="818f9-184">この質問のセットを書き込む必要があります[*ユーザー ストーリー*](http://en.wikipedia.org/wiki/User_stories)します。</span><span class="sxs-lookup"><span data-stu-id="818f9-184">To answer this question, you should write a set of [*user stories*](http://en.wikipedia.org/wiki/User_stories).</span></span>

<span data-ttu-id="818f9-185">ユーザー ストーリーは、ソフトウェア要件 (通常は 1 つの文) を簡単に説明します。</span><span class="sxs-lookup"><span data-stu-id="818f9-185">A user story is a very brief (usually one sentence) description of a software requirement.</span></span> <span data-ttu-id="818f9-186">ユーザーの観点から書き込まれた要件の非技術的な説明が必要です。</span><span class="sxs-lookup"><span data-stu-id="818f9-186">It should be a non-technical description of a requirement written from a user perspective.</span></span>

<span data-ttu-id="818f9-187">ここでは、新しい連絡先グループ機能に必要な機能について説明しているユーザー ストーリーのセット。</span><span class="sxs-lookup"><span data-stu-id="818f9-187">Here s the set of user stories that describe the features required by the new Contact Group functionality:</span></span>

1. <span data-ttu-id="818f9-188">ユーザーは、連絡先グループの一覧を表示できます。</span><span class="sxs-lookup"><span data-stu-id="818f9-188">User can view a list of contact groups.</span></span>
2. <span data-ttu-id="818f9-189">ユーザーは、新しい連絡先グループを作成できます。</span><span class="sxs-lookup"><span data-stu-id="818f9-189">User can create a new contact group.</span></span>
3. <span data-ttu-id="818f9-190">ユーザーは、既存の連絡先グループを削除できます。</span><span class="sxs-lookup"><span data-stu-id="818f9-190">User can delete an existing contact group.</span></span>
4. <span data-ttu-id="818f9-191">ユーザーは、新しい連絡先を作成するときに、連絡先グループを選択できます。</span><span class="sxs-lookup"><span data-stu-id="818f9-191">User can select a contact group when creating a new contact.</span></span>
5. <span data-ttu-id="818f9-192">ユーザーは、既存の連絡先を編集するときに、連絡先グループを選択できます。</span><span class="sxs-lookup"><span data-stu-id="818f9-192">User can select a contact group when editing an existing contact.</span></span>
6. <span data-ttu-id="818f9-193">連絡先グループの一覧は、インデックス ビューに表示されます。</span><span class="sxs-lookup"><span data-stu-id="818f9-193">A list of contact groups is displayed in the Index view.</span></span>
7. <span data-ttu-id="818f9-194">ユーザーは、連絡先グループをクリックすると、該当する連絡先の一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="818f9-194">When a user clicks a contact group, a list of matching contacts is displayed.</span></span>

<span data-ttu-id="818f9-195">このユーザー ストーリーの一覧が顧客によって完全に理解できることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="818f9-195">Notice that this list of user stories is completely understandable by a customer.</span></span> <span data-ttu-id="818f9-196">技術的な実装の詳細の記述は含まれません。</span><span class="sxs-lookup"><span data-stu-id="818f9-196">There is no mention of technical implementation details.</span></span>

<span data-ttu-id="818f9-197">アプリケーションのビルド プロセス中に、一連のユーザー ストーリーはより洗練されたになります。</span><span class="sxs-lookup"><span data-stu-id="818f9-197">While in the process of building your application, the set of user stories can become more refined.</span></span> <span data-ttu-id="818f9-198">ユーザー ストーリーは、複数のストーリー (要件) に壊れることがあります。</span><span class="sxs-lookup"><span data-stu-id="818f9-198">You might break a user story into multiple stories (requirements).</span></span> <span data-ttu-id="818f9-199">たとえば、新しい連絡先グループを作成するが検証が含まれることをことがあります。</span><span class="sxs-lookup"><span data-stu-id="818f9-199">For example, you might decide that creating a new contact group should involve validation.</span></span> <span data-ttu-id="818f9-200">名前のない連絡先グループを送信すると、検証エラーを返す必要があります。</span><span class="sxs-lookup"><span data-stu-id="818f9-200">Submitting a contact group without a name should return a validation error.</span></span>

<span data-ttu-id="818f9-201">ユーザー ストーリーの一覧を作成した後は、最初の単体テストを記述する準備が完了したら。</span><span class="sxs-lookup"><span data-stu-id="818f9-201">After you create a list of user stories, you are ready to write your first unit test.</span></span> <span data-ttu-id="818f9-202">連絡先グループの一覧を表示するための単体テストの作成から始めます。</span><span class="sxs-lookup"><span data-stu-id="818f9-202">We'll start by creating a unit test for viewing the list of contact groups.</span></span>

## <a name="listing-contact-groups"></a><span data-ttu-id="818f9-203">連絡先グループを一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="818f9-203">Listing Contact Groups</span></span>

<span data-ttu-id="818f9-204">この最初のユーザー ストーリーは、ユーザーが連絡先グループの一覧を表示できることです。</span><span class="sxs-lookup"><span data-stu-id="818f9-204">Our first user story is that a user should be able to view a list of contact groups.</span></span> <span data-ttu-id="818f9-205">テストをこのストーリーを表現する必要があります。</span><span class="sxs-lookup"><span data-stu-id="818f9-205">We need to express this story with a test.</span></span>

<span data-ttu-id="818f9-206">ContactManager.Tests プロジェクトで、Controllers フォルダーを右クリックして新しい単体テストの作成を選択すると**追加]、[新しいテスト**を選択して、**単体テスト**テンプレート (図 1 参照)。</span><span class="sxs-lookup"><span data-stu-id="818f9-206">Create a new unit test by right-clicking the Controllers folder in the ContactManager.Tests project, selecting **Add, New Test**, and selecting the **Unit Test** template (see Figure 1).</span></span> <span data-ttu-id="818f9-207">名前の新しい単位が GroupControllerTest.vb をテストし、をクリックして、 **OK**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="818f9-207">Name the new unit test GroupControllerTest.vb and click the **OK** button.</span></span>


<span data-ttu-id="818f9-208">[![GroupControllerTest 単体テストを追加します。](iteration-6-use-test-driven-development-vb/_static/image1.jpg)](iteration-6-use-test-driven-development-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="818f9-208">[![Adding the GroupControllerTest unit test](iteration-6-use-test-driven-development-vb/_static/image1.jpg)](iteration-6-use-test-driven-development-vb/_static/image1.png)</span></span>

<span data-ttu-id="818f9-209">**図 01**: GroupControllerTest 単体テストを追加する ([フルサイズの画像を表示する をクリックします](iteration-6-use-test-driven-development-vb/_static/image2.png))。</span><span class="sxs-lookup"><span data-stu-id="818f9-209">**Figure 01**: Adding the GroupControllerTest unit test([Click to view full-size image](iteration-6-use-test-driven-development-vb/_static/image2.png))</span></span>


<span data-ttu-id="818f9-210">リスト 1 で最初の単体テストが含まれています。</span><span class="sxs-lookup"><span data-stu-id="818f9-210">Our first unit test is contained in Listing 1.</span></span> <span data-ttu-id="818f9-211">このテストでは、グループのコント ローラーの Index() メソッドがグループのセットを返しますを確認します。</span><span class="sxs-lookup"><span data-stu-id="818f9-211">This test verifies that the Index() method of the Group controller returns a set of Groups.</span></span> <span data-ttu-id="818f9-212">テストは、グループのコレクションがビュー内のデータで返されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="818f9-212">The test verifies that a collection of Groups is returned in view data.</span></span>

<span data-ttu-id="818f9-213">**1 - Controllers\GroupControllerTest.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="818f9-213">**Listing 1 - Controllers\GroupControllerTest.vb**</span></span>

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample1.vb)]

<span data-ttu-id="818f9-214">Visual Studio ではリスト 1 で最初に、コードを入力すると、大量の赤の波線が表示されます。</span><span class="sxs-lookup"><span data-stu-id="818f9-214">When you first type the code in Listing 1 in Visual Studio, you'll get a lot of red squiggly lines.</span></span> <span data-ttu-id="818f9-215">GroupController またはグループのクラスを作成していません。</span><span class="sxs-lookup"><span data-stu-id="818f9-215">We have not created the GroupController or Group classes.</span></span>

<span data-ttu-id="818f9-216">この時点では、t もビルドできる t ように、アプリケーションは、最初の単体テストを実行します。</span><span class="sxs-lookup"><span data-stu-id="818f9-216">At this point, we can t even build our application so we can t execute our first unit test.</span></span> <span data-ttu-id="818f9-217">お勧めします。</span><span class="sxs-lookup"><span data-stu-id="818f9-217">That s good.</span></span> <span data-ttu-id="818f9-218">失敗したテストとしてをカウントします。</span><span class="sxs-lookup"><span data-stu-id="818f9-218">That counts as a failing test.</span></span> <span data-ttu-id="818f9-219">そのため、ここでアプリケーション コードの記述を開始するアクセス許可があります。</span><span class="sxs-lookup"><span data-stu-id="818f9-219">Therefore, we now have permission to start writing application code.</span></span> <span data-ttu-id="818f9-220">このテストを実行するための十分なコードを記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="818f9-220">We need to write enough code to execute our test.</span></span>

<span data-ttu-id="818f9-221">リスト 2 でのグループのコント ローラー クラスには、単体テストに合格するために必要なコードの最低限が含まれています。</span><span class="sxs-lookup"><span data-stu-id="818f9-221">The Group controller class in Listing 2 contains the bare minimum of code required to pass the unit test.</span></span> <span data-ttu-id="818f9-222">Index() アクションは、グループ (グループ クラスは、リスト 3 で定義されます) の静的にコード化された一覧を返します。</span><span class="sxs-lookup"><span data-stu-id="818f9-222">The Index() action returns a statically coded list of Groups (the Group class is defined in Listing 3).</span></span>

<span data-ttu-id="818f9-223">**2 - Controllers\GroupController.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="818f9-223">**Listing 2 - Controllers\GroupController.vb**</span></span>

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample2.vb)]

<span data-ttu-id="818f9-224">**3 - Models\Group.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="818f9-224">**Listing 3 - Models\Group.vb**</span></span>

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample3.vb)]

<span data-ttu-id="818f9-225">最初の単体テストが正常に完了後に GroupController とグループのクラスをプロジェクトを追加します (図 2 参照)。</span><span class="sxs-lookup"><span data-stu-id="818f9-225">After we add the GroupController and Group classes to our project, our first unit test completes successfully (see Figure 2).</span></span> <span data-ttu-id="818f9-226">テストに合格するために必要な最低限の作業を行っています。</span><span class="sxs-lookup"><span data-stu-id="818f9-226">We have done the minimum work required to pass the test.</span></span> <span data-ttu-id="818f9-227">時間を記念することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="818f9-227">It is time to celebrate.</span></span>


<span data-ttu-id="818f9-228">[![Success!](iteration-6-use-test-driven-development-vb/_static/image2.jpg)](iteration-6-use-test-driven-development-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="818f9-228">[![Success!](iteration-6-use-test-driven-development-vb/_static/image2.jpg)](iteration-6-use-test-driven-development-vb/_static/image3.png)</span></span>

<span data-ttu-id="818f9-229">**図 02**: 成功! ([フルサイズの画像を表示する をクリックします](iteration-6-use-test-driven-development-vb/_static/image4.png))。</span><span class="sxs-lookup"><span data-stu-id="818f9-229">**Figure 02**: Success!([Click to view full-size image](iteration-6-use-test-driven-development-vb/_static/image4.png))</span></span>


## <a name="creating-contact-groups"></a><span data-ttu-id="818f9-230">連絡先グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="818f9-230">Creating Contact Groups</span></span>

<span data-ttu-id="818f9-231">これで、2 つ目のユーザー ストーリー進むことができます。</span><span class="sxs-lookup"><span data-stu-id="818f9-231">Now we can move on to the second user story.</span></span> <span data-ttu-id="818f9-232">新しい連絡先グループを作成できる必要があります。</span><span class="sxs-lookup"><span data-stu-id="818f9-232">We need to be able to create new contact groups.</span></span> <span data-ttu-id="818f9-233">テストでこの意図を表現する必要があります。</span><span class="sxs-lookup"><span data-stu-id="818f9-233">We need to express this intention with a test.</span></span>

<span data-ttu-id="818f9-234">リスト 4 のテストは、新しいグループを持つメソッドは、Index() メソッドによって返されるグループの一覧に、グループを追加します。 Create() を呼び出すことを確認します。</span><span class="sxs-lookup"><span data-stu-id="818f9-234">The test in Listing 4 verifies that calling the Create() method with a new Group adds the Group to the list of Groups returned by the Index() method.</span></span> <span data-ttu-id="818f9-235">つまり、新しいグループを作成した場合、必要のある Index() メソッドによって返されるグループの一覧から戻り、新しいグループを取得できません。</span><span class="sxs-lookup"><span data-stu-id="818f9-235">In other words, if I create a new group then I should be able to get the new Group back from the list of Groups returned by the Index() method.</span></span>

<span data-ttu-id="818f9-236">**4 - Controllers\GroupControllerTest.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="818f9-236">**Listing 4 - Controllers\GroupControllerTest.vb**</span></span>

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample4.vb)]

<span data-ttu-id="818f9-237">リスト 4 のテストでは、グループのコント ローラーで新しい連絡先グループ Create() メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="818f9-237">The test in Listing 4 calls the Group controller Create() method with a new contact Group.</span></span> <span data-ttu-id="818f9-238">次に、テストでは、データの表示で新しいグループを返すグループ コント ローラー Index() メソッドを呼び出すことを確認します。</span><span class="sxs-lookup"><span data-stu-id="818f9-238">Next, the test verifies that calling the Group controller Index() method returns the new Group in view data.</span></span>

<span data-ttu-id="818f9-239">リスト 5 で変更されたグループ コント ローラーには、必要な最小限新しいテストに合格するために必要な変更にはが含まれています。</span><span class="sxs-lookup"><span data-stu-id="818f9-239">The modified Group controller in Listing 5 contains the bare minimum of changes required to pass the new test.</span></span>

<span data-ttu-id="818f9-240">**5 - Controllers\GroupController.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="818f9-240">**Listing 5 - Controllers\GroupController.vb**</span></span>

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample5.vb)]

## <a name="the-group-controller-in-listing-5-has-a-new-create-action-this-action-adds-a-group-to-a-collection-of-groups-notice-that-the-index-action-has-been-modified-to-return-the-contents-of-the-collection-of-groups"></a><span data-ttu-id="818f9-241">リスト 5 でグループのコント ローラーでは、新しい Create() アクションを持ちます。</span><span class="sxs-lookup"><span data-stu-id="818f9-241">The Group controller in Listing 5 has a new Create() action.</span></span> <span data-ttu-id="818f9-242">このアクションでは、グループのコレクションにグループを追加します。</span><span class="sxs-lookup"><span data-stu-id="818f9-242">This action adds a Group to a collection of Groups.</span></span> <span data-ttu-id="818f9-243">グループのコレクションのコンテンツを返す Index() アクションが変更されたことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="818f9-243">Notice that the Index() action has been modified to return the contents of the collection of Groups.</span></span>

<span data-ttu-id="818f9-244">もう一度に、最低限の単体テストに合格するために必要な作業を実行しました。</span><span class="sxs-lookup"><span data-stu-id="818f9-244">Once again, we have performed the bare minimum amount of work required to pass the unit test.</span></span> <span data-ttu-id="818f9-245">グループのコント ローラーにこれらの変更を行った後、単位のすべてのパスをテストします。</span><span class="sxs-lookup"><span data-stu-id="818f9-245">After we make these changes to the Group controller, all of our unit tests pass.</span></span>

## <a name="adding-validation"></a><span data-ttu-id="818f9-246">検証の追加</span><span class="sxs-lookup"><span data-stu-id="818f9-246">Adding Validation</span></span>

<span data-ttu-id="818f9-247">この要件は、ユーザー ストーリーで明示的に報告されていません。</span><span class="sxs-lookup"><span data-stu-id="818f9-247">This requirement was not stated explicitly in the user story.</span></span> <span data-ttu-id="818f9-248">ただし、グループに名前があることを要求する妥当なは。</span><span class="sxs-lookup"><span data-stu-id="818f9-248">However, it is reasonable to require that a Group have a name.</span></span> <span data-ttu-id="818f9-249">それ以外の場合、連絡先グループに編成しない非常に便利になります。</span><span class="sxs-lookup"><span data-stu-id="818f9-249">Otherwise, organizing contacts into Groups would not be very useful.</span></span>

<span data-ttu-id="818f9-250">6 を一覧表示するには、この意図を表す新しいテストが含まれています。</span><span class="sxs-lookup"><span data-stu-id="818f9-250">Listing 6 contains a new test that expresses this intention.</span></span> <span data-ttu-id="818f9-251">このテストでは、モデルの状態の検証エラー メッセージに名前の結果を指定せずにグループを作成しようとすることを確認します。</span><span class="sxs-lookup"><span data-stu-id="818f9-251">This test verifies that attempting to create a Group without supplying a name results in a validation error message in model state.</span></span>

<span data-ttu-id="818f9-252">**6 - Controllers\GroupControllerTest.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="818f9-252">**Listing 6 - Controllers\GroupControllerTest.vb**</span></span>

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample6.vb)]

<span data-ttu-id="818f9-253">このテストを満たすために、グループのクラス (7 のリストを参照) に Name プロパティを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="818f9-253">In order to satisfy this test, we need to add a Name property to our Group class (see Listing 7).</span></span> <span data-ttu-id="818f9-254">さらに、グループ コント ローラー s Create() アクション (8 のリストを参照) をわずかな検証ロジックを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="818f9-254">Furthermore, we need to add a tiny bit of validation logic to our Group controller s Create() action (see Listing 8).</span></span>

<span data-ttu-id="818f9-255">**7 - Models\Group.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="818f9-255">**Listing 7 - Models\Group.vb**</span></span>

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample7.vb)]

<span data-ttu-id="818f9-256">**8 - Controllers\GroupController.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="818f9-256">**Listing 8 - Controllers\GroupController.vb**</span></span>

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample8.vb)]

<span data-ttu-id="818f9-257">今すぐ Create() のアクション グループのコント ローラーにはロジック検証とデータベースの両方にはが含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="818f9-257">Notice that the Group controller Create() action now contains both validation and database logic.</span></span> <span data-ttu-id="818f9-258">現時点では、グループのコント ローラーによって使用されるデータベースは、nothing よりもメモリ内コレクションで構成されます。</span><span class="sxs-lookup"><span data-stu-id="818f9-258">Currently, the database used by the Group controller consists of nothing more than an in-memory collection.</span></span>

## <a name="time-to-refactor"></a><span data-ttu-id="818f9-259">リファクタリングする時間</span><span class="sxs-lookup"><span data-stu-id="818f9-259">Time to Refactor</span></span>

<span data-ttu-id="818f9-260">赤/緑/リファクタリングの 3 番目の手順では、リファクタリング部分です。</span><span class="sxs-lookup"><span data-stu-id="818f9-260">The third step in Red/Green/Refactor is the Refactor part.</span></span> <span data-ttu-id="818f9-261">この時点では、コードからステップのデザインを向上させるために、アプリケーションのリファクタリングを考慮する必要があります。</span><span class="sxs-lookup"><span data-stu-id="818f9-261">At this point, we need to step back from our code and consider how we can refactor our application to improve its design.</span></span> <span data-ttu-id="818f9-262">リファクタリング ステージは、位置真剣に考えてソフトウェア設計の原則とパターンを実装する最善の方法の段階です。</span><span class="sxs-lookup"><span data-stu-id="818f9-262">The Refactor stage is the stage at which we think hard about the best way of implementing software design principles and patterns.</span></span>

<span data-ttu-id="818f9-263">自由に、コードの設計を向上させるために選択した任意の方法でコードを変更しています。</span><span class="sxs-lookup"><span data-stu-id="818f9-263">We are free to modify our code in any way that we choose to improve the design of the code.</span></span> <span data-ttu-id="818f9-264">既存の機能を損なうを妨げている単体テストのセーフティ ネットがあります。</span><span class="sxs-lookup"><span data-stu-id="818f9-264">We have a safety net of unit tests that prevent us from breaking existing functionality.</span></span>

<span data-ttu-id="818f9-265">現在のところ、グループ コント ローラーは、優れたソフトウェア設計の観点から混乱します。</span><span class="sxs-lookup"><span data-stu-id="818f9-265">Right now, our Group controller is a mess from the perspective of good software design.</span></span> <span data-ttu-id="818f9-266">グループのコント ローラーには、検証とデータ アクセス コードのすぎなく混乱が含まれています。</span><span class="sxs-lookup"><span data-stu-id="818f9-266">The Group controller contains a tangled mess of validation and data access code.</span></span> <span data-ttu-id="818f9-267">単一責任の原則に違反しないように、さまざまなクラスをこれらの問題を分離する必要があります。</span><span class="sxs-lookup"><span data-stu-id="818f9-267">To avoid violating the Single Responsibility Principle, we need to separate these concerns into different classes.</span></span>

<span data-ttu-id="818f9-268">リファクタリングされたグループのコント ローラー クラスは、9 の一覧に含まれます。</span><span class="sxs-lookup"><span data-stu-id="818f9-268">Our refactored Group controller class is contained in Listing 9.</span></span> <span data-ttu-id="818f9-269">ContactManager サービス層を使用するには、コント ローラーを変更されています。</span><span class="sxs-lookup"><span data-stu-id="818f9-269">The controller has been modified to use the ContactManager service layer.</span></span> <span data-ttu-id="818f9-270">これは、同じサービス層にお問い合わせくださいコント ローラーを使用します。</span><span class="sxs-lookup"><span data-stu-id="818f9-270">This is the same service layer that we use with the Contact controller.</span></span>

<span data-ttu-id="818f9-271">10 を一覧表示するには、検証、一覧表示、およびグループの作成をサポートするために、ContactManager サービス層に追加された新しいメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="818f9-271">Listing 10 contains the new methods added to the ContactManager service layer to support validating, listing, and creating groups.</span></span> <span data-ttu-id="818f9-272">新しいメソッドを含める IContactManagerService インターフェイスが更新されました。</span><span class="sxs-lookup"><span data-stu-id="818f9-272">The IContactManagerService interface was updated to include the new methods.</span></span>

<span data-ttu-id="818f9-273">11 を一覧表示するには、IContactManagerRepository インターフェイスを実装する新しい FakeContactManagerRepository クラスが含まれています。</span><span class="sxs-lookup"><span data-stu-id="818f9-273">Listing 11 contains a new FakeContactManagerRepository class that implements the IContactManagerRepository interface.</span></span> <span data-ttu-id="818f9-274">を IContactManagerRepository インターフェイスも実装する EntityContactManagerRepository クラスとは異なり、新しい FakeContactManagerRepository クラスは、データベースと通信しません。</span><span class="sxs-lookup"><span data-stu-id="818f9-274">Unlike the EntityContactManagerRepository class that also implements the IContactManagerRepository interface, our new FakeContactManagerRepository class does not communicate with the database.</span></span> <span data-ttu-id="818f9-275">FakeContactManagerRepository クラスは、データベースのプロキシとしてのメモリ内コレクションを使用します。</span><span class="sxs-lookup"><span data-stu-id="818f9-275">The FakeContactManagerRepository class uses an in-memory collection as a proxy for the database.</span></span> <span data-ttu-id="818f9-276">このクラス、単体テストで偽リポジトリ層として使用します。</span><span class="sxs-lookup"><span data-stu-id="818f9-276">We'll use this class in our unit tests as a fake repository layer.</span></span>

<span data-ttu-id="818f9-277">**9 - Controllers\GroupController.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="818f9-277">**Listing 9 - Controllers\GroupController.vb**</span></span>

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample9.vb)]

<span data-ttu-id="818f9-278">**10 - Controllers\ContactManagerService.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="818f9-278">**Listing 10 - Controllers\ContactManagerService.vb**</span></span>

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample10.vb)]

<span data-ttu-id="818f9-279">**11 - Controllers\FakeContactManagerRepository.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="818f9-279">**Listing 11 - Controllers\FakeContactManagerRepository.vb**</span></span>

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample11.vb)]


<span data-ttu-id="818f9-280">使用して EntityContactManagerRepository クラスで CreateGroup() と ListGroups() メソッドを実装するインターフェイスが必要です IContactManagerRepository を変更します。</span><span class="sxs-lookup"><span data-stu-id="818f9-280">Modifying the IContactManagerRepository interface requires use to implement the CreateGroup() and ListGroups() methods in the EntityContactManagerRepository class.</span></span> <span data-ttu-id="818f9-281">これを行う laziest で時間のかからない方法は、次のようにスタブ メソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="818f9-281">The laziest and fastest way to do this is to add stub methods that look like this:</span></span>

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample12.vb)]


<span data-ttu-id="818f9-282">最後に、これらの変更をアプリケーションの設計には、いくつかの単体テストを変更することが必要です。</span><span class="sxs-lookup"><span data-stu-id="818f9-282">Finally, these changes to the design of our application require us to make some modifications to our unit tests.</span></span> <span data-ttu-id="818f9-283">これで、単体テストを実行するときに、FakeContactManagerRepository を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="818f9-283">We now need to use the FakeContactManagerRepository when performing the unit tests.</span></span> <span data-ttu-id="818f9-284">更新された GroupControllerTest クラスは、12 の一覧に含まれます。</span><span class="sxs-lookup"><span data-stu-id="818f9-284">The updated GroupControllerTest class is contained in Listing 12.</span></span>

<span data-ttu-id="818f9-285">**12 - Controllers\GroupControllerTest.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="818f9-285">**Listing 12 - Controllers\GroupControllerTest.vb**</span></span>

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample13.vb)]

<span data-ttu-id="818f9-286">これらのすべての操作を行った後の変更をもう一度、すべての単体テスト成功の。</span><span class="sxs-lookup"><span data-stu-id="818f9-286">After we make all of these changes, once again, all of our unit tests pass.</span></span> <span data-ttu-id="818f9-287">赤/緑/リファクタリングのサイクル全体が完了しました。</span><span class="sxs-lookup"><span data-stu-id="818f9-287">We have completed the entire cycle of Red/Green/Refactor.</span></span> <span data-ttu-id="818f9-288">最初の 2 つのユーザー ストーリーを実装しました。</span><span class="sxs-lookup"><span data-stu-id="818f9-288">We have implemented the first two user stories.</span></span> <span data-ttu-id="818f9-289">単体テストをサポートしているユーザー ストーリーで表される要件があるようになりました。</span><span class="sxs-lookup"><span data-stu-id="818f9-289">We now have supporting unit tests for the requirements expressed in the user stories.</span></span> <span data-ttu-id="818f9-290">ユーザー ストーリーの残りの部分を実装するには、同じ赤/緑/リファクタリングのサイクルを繰り返す必要があります。</span><span class="sxs-lookup"><span data-stu-id="818f9-290">Implementing the remainder of the user stories involves repeating the same cycle of Red/Green/Refactor.</span></span>

## <a name="modifying-our-database"></a><span data-ttu-id="818f9-291">データベースの変更</span><span class="sxs-lookup"><span data-stu-id="818f9-291">Modifying our Database</span></span>

<span data-ttu-id="818f9-292">残念ながら、すべての単体テストによって表される要件を満たすことも、作業は行われません。</span><span class="sxs-lookup"><span data-stu-id="818f9-292">Unfortunately, even though we have satisfied all of the requirements expressed by our unit tests, our work is not done.</span></span> <span data-ttu-id="818f9-293">このデータベースを変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="818f9-293">We still need to modify our database.</span></span>

<span data-ttu-id="818f9-294">新しいグループのデータベース テーブルを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="818f9-294">We need to create a new Group database table.</span></span> <span data-ttu-id="818f9-295">この場合は、以下の手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="818f9-295">Follow these steps:</span></span>

1. <span data-ttu-id="818f9-296">サーバー エクスプ ローラー ウィンドウで テーブル フォルダーを右クリックし、メニュー オプションを選択**新しいテーブルの追加**します。</span><span class="sxs-lookup"><span data-stu-id="818f9-296">In the Server Explorer window, right-click the Tables folder and select the menu option **Add New Table**.</span></span>
2. <span data-ttu-id="818f9-297">次のテーブル デザイナーで説明する 2 つの列を入力します。</span><span class="sxs-lookup"><span data-stu-id="818f9-297">Enter the two columns described below in the Table Designer.</span></span>
3. <span data-ttu-id="818f9-298">主キーと Id 列として Id 列をマークします。</span><span class="sxs-lookup"><span data-stu-id="818f9-298">Mark the Id column as a primary key and Identity column.</span></span>
4. <span data-ttu-id="818f9-299">名前のグループで、新しいテーブルを保存するには、フロッピー ディスクのアイコンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="818f9-299">Save the new table with the name Groups by clicking the icon of the floppy.</span></span>

<a id="0.12_table01"></a>


| <span data-ttu-id="818f9-300">**列名**</span><span class="sxs-lookup"><span data-stu-id="818f9-300">**Column Name**</span></span> | <span data-ttu-id="818f9-301">**データ型**</span><span class="sxs-lookup"><span data-stu-id="818f9-301">**Data Type**</span></span> | <span data-ttu-id="818f9-302">**Null を許容します。**</span><span class="sxs-lookup"><span data-stu-id="818f9-302">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="818f9-303">ID</span><span class="sxs-lookup"><span data-stu-id="818f9-303">Id</span></span> | <span data-ttu-id="818f9-304">int</span><span class="sxs-lookup"><span data-stu-id="818f9-304">int</span></span> | <span data-ttu-id="818f9-305">False</span><span class="sxs-lookup"><span data-stu-id="818f9-305">False</span></span> |
| <span data-ttu-id="818f9-306">name</span><span class="sxs-lookup"><span data-stu-id="818f9-306">Name</span></span> | <span data-ttu-id="818f9-307">nvarchar (50)</span><span class="sxs-lookup"><span data-stu-id="818f9-307">nvarchar(50)</span></span> | <span data-ttu-id="818f9-308">False</span><span class="sxs-lookup"><span data-stu-id="818f9-308">False</span></span> |


<span data-ttu-id="818f9-309">次に、Contacts テーブルからすべてのデータを削除する必要があります (それ以外の場合、獲得した連絡先およびグループのテーブル間のリレーションシップを作成することはできません)。</span><span class="sxs-lookup"><span data-stu-id="818f9-309">Next, we need to delete all of the data from the Contacts table (otherwise, we won t be able to create a relationship between the Contacts and Groups tables).</span></span> <span data-ttu-id="818f9-310">この場合は、以下の手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="818f9-310">Follow these steps:</span></span>

1. <span data-ttu-id="818f9-311">Contacts テーブルを右クリックし、メニュー オプションを選択**テーブル データの表示**します。</span><span class="sxs-lookup"><span data-stu-id="818f9-311">Right-click the Contacts table and select the menu option **Show Table Data**.</span></span>
2. <span data-ttu-id="818f9-312">すべての行を削除します。</span><span class="sxs-lookup"><span data-stu-id="818f9-312">Delete all of the rows.</span></span>

<span data-ttu-id="818f9-313">次に、グループのデータベース テーブルと既存の連絡先データベース テーブルの間のリレーションシップを定義する必要があります。</span><span class="sxs-lookup"><span data-stu-id="818f9-313">Next, we need to define a relationship between the Groups database table and the existing Contacts database table.</span></span> <span data-ttu-id="818f9-314">この場合は、以下の手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="818f9-314">Follow these steps:</span></span>

1. <span data-ttu-id="818f9-315">テーブル デザイナーを開き、サーバー エクスプ ローラー ウィンドウで、Contacts テーブルをダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="818f9-315">Double-click the Contacts table in the Server Explorer window to open the Table Designer.</span></span>
2. <span data-ttu-id="818f9-316">GroupId 連絡先テーブルには、新しい整数型の列を追加します。</span><span class="sxs-lookup"><span data-stu-id="818f9-316">Add a new integer column to the Contacts table named GroupId.</span></span>
3. <span data-ttu-id="818f9-317">外部キーのリレーションシップ ダイアログを開きますリレーションシップ ボタンをクリックします (図 3 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="818f9-317">Click the Relationship button to open the Foreign Key Relationships dialog (see Figure 3).</span></span>
4. <span data-ttu-id="818f9-318">[追加] ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="818f9-318">Click the Add button.</span></span>
5. <span data-ttu-id="818f9-319">テーブルと列の指定 ボタンの横に表示される省略記号ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="818f9-319">Click the ellipsis button that appears next to the Table and Columns Specification button.</span></span>
6. <span data-ttu-id="818f9-320">テーブルと列 ダイアログで、主キー テーブルに主キー列として Id とグループを選択します。</span><span class="sxs-lookup"><span data-stu-id="818f9-320">In the Tables and Columns dialog, select Groups as the primary key table and Id as the primary key column.</span></span> <span data-ttu-id="818f9-321">外部キー テーブルと外部キー列として GroupId として連絡先を選択します (図 4 参照)。</span><span class="sxs-lookup"><span data-stu-id="818f9-321">Select Contacts as the foreign key table and GroupId as the foreign key column (see Figure 4).</span></span> <span data-ttu-id="818f9-322">[Ok] ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="818f9-322">Click the OK button.</span></span>
7. <span data-ttu-id="818f9-323">**INSERT および UPDATE の指定**、値を選択して**Cascade**の**ルールの削除**します。</span><span class="sxs-lookup"><span data-stu-id="818f9-323">Under **INSERT and UPDATE Specification**, select the value **Cascade** for **Delete Rule**.</span></span>
8. <span data-ttu-id="818f9-324">外部キーのリレーションシップ ダイアログを閉じる閉じる ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="818f9-324">Click the Close button to close the Foreign Key Relationships dialog.</span></span>
9. <span data-ttu-id="818f9-325">Contacts テーブルに対する変更を保存する [保存] ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="818f9-325">Click the Save button to save the changes to the Contacts table.</span></span>


<span data-ttu-id="818f9-326">[![データベース テーブルのリレーションシップを作成します。](iteration-6-use-test-driven-development-vb/_static/image3.jpg)](iteration-6-use-test-driven-development-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="818f9-326">[![Creating a database table relationship](iteration-6-use-test-driven-development-vb/_static/image3.jpg)](iteration-6-use-test-driven-development-vb/_static/image5.png)</span></span>

<span data-ttu-id="818f9-327">**図 03**: データベースのテーブル リレーションシップの作成 ([フルサイズの画像を表示する をクリックします](iteration-6-use-test-driven-development-vb/_static/image6.png))。</span><span class="sxs-lookup"><span data-stu-id="818f9-327">**Figure 03**: Creating a database table relationship ([Click to view full-size image](iteration-6-use-test-driven-development-vb/_static/image6.png))</span></span>


<span data-ttu-id="818f9-328">[![テーブルのリレーションシップを指定します。](iteration-6-use-test-driven-development-vb/_static/image4.jpg)](iteration-6-use-test-driven-development-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="818f9-328">[![Specifying table relationships](iteration-6-use-test-driven-development-vb/_static/image4.jpg)](iteration-6-use-test-driven-development-vb/_static/image7.png)</span></span>

<span data-ttu-id="818f9-329">**図 04**: テーブルのリレーションシップを指定する ([フルサイズの画像を表示する をクリックします](iteration-6-use-test-driven-development-vb/_static/image8.png))。</span><span class="sxs-lookup"><span data-stu-id="818f9-329">**Figure 04**: Specifying table relationships([Click to view full-size image](iteration-6-use-test-driven-development-vb/_static/image8.png))</span></span>


### <a name="updating-our-data-model"></a><span data-ttu-id="818f9-330">このデータ モデルを更新します。</span><span class="sxs-lookup"><span data-stu-id="818f9-330">Updating our Data Model</span></span>

<span data-ttu-id="818f9-331">次に、新しいデータベース テーブルを表すため、データ モデルを更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="818f9-331">Next, we need to update our data model to represent the new database table.</span></span> <span data-ttu-id="818f9-332">この場合は、以下の手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="818f9-332">Follow these steps:</span></span>

1. <span data-ttu-id="818f9-333">エンティティ デザイナーを開く、Models フォルダーに ContactManagerModel.edmx ファイルをダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="818f9-333">Double-click the ContactManagerModel.edmx file in the Models folder to open the Entity Designer.</span></span>
2. <span data-ttu-id="818f9-334">デザイナー画面を右クリックし、メニュー オプションを選択**データベースからモデルを更新**します。</span><span class="sxs-lookup"><span data-stu-id="818f9-334">Right-click the Designer surface and select the menu option **Update Model from Database**.</span></span>
3. <span data-ttu-id="818f9-335">更新ウィザードでは、選択、グループのテーブルし、完了 をクリックします。 は、(図 5 を参照してください) ボタンします。</span><span class="sxs-lookup"><span data-stu-id="818f9-335">In the Update Wizard, select the Groups table and click the Finish button (see Figure 5).</span></span>
4. <span data-ttu-id="818f9-336">グループ エンティティを右クリックし、メニュー オプションを選択**の名前を変更**します。</span><span class="sxs-lookup"><span data-stu-id="818f9-336">Right-click the Groups entity and select the menu option **Rename**.</span></span> <span data-ttu-id="818f9-337">名前を変更、*グループ*エンティティ*グループ*(単数形)。</span><span class="sxs-lookup"><span data-stu-id="818f9-337">Change the name of the *Groups* entity to *Group* (singular).</span></span>
5. <span data-ttu-id="818f9-338">Contact エンティティの下部に表示されるグループ ナビゲーション プロパティを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="818f9-338">Right-click the Groups navigation property that appears at the bottom of the Contact entity.</span></span> <span data-ttu-id="818f9-339">名前を変更、*グループ*ナビゲーション プロパティを*グループ*(単数形)。</span><span class="sxs-lookup"><span data-stu-id="818f9-339">Change the name of the *Groups* navigation property to *Group* (singular).</span></span>


<span data-ttu-id="818f9-340">[![データベースから Entity Framework モデルを更新しています](iteration-6-use-test-driven-development-vb/_static/image5.jpg)](iteration-6-use-test-driven-development-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="818f9-340">[![Updating an Entity Framework model from the database](iteration-6-use-test-driven-development-vb/_static/image5.jpg)](iteration-6-use-test-driven-development-vb/_static/image9.png)</span></span>

<span data-ttu-id="818f9-341">**図 05**: データベースから Entity Framework モデルを更新しています ([フルサイズの画像を表示する をクリックします](iteration-6-use-test-driven-development-vb/_static/image10.png))。</span><span class="sxs-lookup"><span data-stu-id="818f9-341">**Figure 05**: Updating an Entity Framework model from the database([Click to view full-size image](iteration-6-use-test-driven-development-vb/_static/image10.png))</span></span>


<span data-ttu-id="818f9-342">次の手順を完了すると、データ モデルは、連絡先とグループの両方のテーブルを表します。</span><span class="sxs-lookup"><span data-stu-id="818f9-342">After you complete these steps, your data model will represent both the Contacts and Groups tables.</span></span> <span data-ttu-id="818f9-343">エンティティ デザイナーは、両方のエンティティを表示する必要があります (図 6 参照)。</span><span class="sxs-lookup"><span data-stu-id="818f9-343">The Entity Designer should show both entities (see Figure 6).</span></span>


<span data-ttu-id="818f9-344">[![エンティティ デザイナーのグループと連絡先を表示します。](iteration-6-use-test-driven-development-vb/_static/image6.jpg)](iteration-6-use-test-driven-development-vb/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="818f9-344">[![Entity Designer displaying Group and Contact](iteration-6-use-test-driven-development-vb/_static/image6.jpg)](iteration-6-use-test-driven-development-vb/_static/image11.png)</span></span>

<span data-ttu-id="818f9-345">**図 06**: グループと連絡先を表示するエンティティ デザイナー ([フルサイズの画像を表示する をクリックします](iteration-6-use-test-driven-development-vb/_static/image12.png))。</span><span class="sxs-lookup"><span data-stu-id="818f9-345">**Figure 06**: Entity Designer displaying Group and Contact([Click to view full-size image](iteration-6-use-test-driven-development-vb/_static/image12.png))</span></span>


### <a name="creating-our-repository-classes"></a><span data-ttu-id="818f9-346">リポジトリ クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="818f9-346">Creating our Repository Classes</span></span>

<span data-ttu-id="818f9-347">次に、リポジトリ クラスを実装する必要があります。</span><span class="sxs-lookup"><span data-stu-id="818f9-347">Next, we need to implement our repository class.</span></span> <span data-ttu-id="818f9-348">このイテレーションの過程で、単体テストを満たすためにコードの書き込み中に IContactManagerRepository インターフェイスにいくつかの新しいメソッドをしました。</span><span class="sxs-lookup"><span data-stu-id="818f9-348">Over the course of this iteration, we added several new methods to the IContactManagerRepository interface while writing code to satisfy our unit tests.</span></span> <span data-ttu-id="818f9-349">IContactManagerRepository インターフェイスの最終バージョンは、14 の一覧に含まれます。</span><span class="sxs-lookup"><span data-stu-id="818f9-349">The final version of the IContactManagerRepository interface is contained in Listing 14.</span></span>

<span data-ttu-id="818f9-350">**14 - Models\IContactManagerRepository.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="818f9-350">**Listing 14 - Models\IContactManagerRepository.vb**</span></span>

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample14.vb)]

<span data-ttu-id="818f9-351">私たち haven t が実際には、実際の EntityContactManagerRepository クラスで連絡先グループの操作に関連するメソッドのいずれかを実装します。</span><span class="sxs-lookup"><span data-stu-id="818f9-351">We haven t actually implemented any of the methods related to working with contact groups in our real EntityContactManagerRepository class.</span></span> <span data-ttu-id="818f9-352">現時点では、EntityContactManagerRepository クラスでは、各 IContactManagerRepository インターフェイスに表示されている連絡先グループ メソッド スタブのメソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="818f9-352">Currently, the EntityContactManagerRepository class has stub methods for each of the contact group methods listed in the IContactManagerRepository interface.</span></span> <span data-ttu-id="818f9-353">たとえば、現在 ListGroups() メソッドが次のようになります。</span><span class="sxs-lookup"><span data-stu-id="818f9-353">For example, the ListGroups() method currently looks like this:</span></span>

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample15.vb)]

<span data-ttu-id="818f9-354">スタブ メソッドをアプリケーションをコンパイルして、単体テストに合格するようになりました。</span><span class="sxs-lookup"><span data-stu-id="818f9-354">The stub methods enabled us to compile our application and pass the unit tests.</span></span> <span data-ttu-id="818f9-355">ただし、いよいよを実際にこれらのメソッドを実装します。</span><span class="sxs-lookup"><span data-stu-id="818f9-355">However, now it is time to actually implement these methods.</span></span> <span data-ttu-id="818f9-356">EntityContactManagerRepository クラスの最終バージョンは、13 の一覧に含まれます。</span><span class="sxs-lookup"><span data-stu-id="818f9-356">The final version of the EntityContactManagerRepository class is contained in Listing 13.</span></span>

<span data-ttu-id="818f9-357">**13 - Models\EntityContactManagerRepository.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="818f9-357">**Listing 13 - Models\EntityContactManagerRepository.vb**</span></span>

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample16.vb)]

### <a name="creating-the-views"></a><span data-ttu-id="818f9-358">ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="818f9-358">Creating the Views</span></span>

<span data-ttu-id="818f9-359">既定の ASP.NET ビュー エンジンを使用する場合の ASP.NET MVC アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="818f9-359">ASP.NET MVC application when you use the default ASP.NET view engine.</span></span> <span data-ttu-id="818f9-360">そのため、don t は、特定の単体テストへの応答でビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="818f9-360">So, you don t create views in response to a particular unit test.</span></span> <span data-ttu-id="818f9-361">ただし、ため、アプリケーションが役に立たないビューである場合はなるべく t の作成と Contact Manager アプリケーションに含まれるビューを変更せずにこのイテレーションを完了します。</span><span class="sxs-lookup"><span data-stu-id="818f9-361">However, because an application would be useless without views, we can t complete this iteration without creating and modifying the views contained in the Contact Manager application.</span></span>

<span data-ttu-id="818f9-362">ビューを作成する次新しい連絡先グループを管理するため (図 7 を参照してください) 必要があります。</span><span class="sxs-lookup"><span data-stu-id="818f9-362">We need to create the following new views for managing contact groups (see Figure 7):</span></span>

- <span data-ttu-id="818f9-363">Views\Group\Index.aspx - 連絡先グループの一覧を表示</span><span class="sxs-lookup"><span data-stu-id="818f9-363">Views\Group\Index.aspx - Displays list of contact groups</span></span>
- <span data-ttu-id="818f9-364">Views\Group\Delete.aspx - 連絡先グループの削除確認フォームを表示します</span><span class="sxs-lookup"><span data-stu-id="818f9-364">Views\Group\Delete.aspx - Displays confirmation form for deleting a contact group</span></span>


<span data-ttu-id="818f9-365">[![グループのインデックス ビュー](iteration-6-use-test-driven-development-vb/_static/image7.jpg)](iteration-6-use-test-driven-development-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="818f9-365">[![The Group Index view](iteration-6-use-test-driven-development-vb/_static/image7.jpg)](iteration-6-use-test-driven-development-vb/_static/image13.png)</span></span>

<span data-ttu-id="818f9-366">**図 07**: グループ インデックス ビュー ([フルサイズの画像を表示する をクリックします](iteration-6-use-test-driven-development-vb/_static/image14.png))。</span><span class="sxs-lookup"><span data-stu-id="818f9-366">**Figure 07**: The Group Index view([Click to view full-size image](iteration-6-use-test-driven-development-vb/_static/image14.png))</span></span>


<span data-ttu-id="818f9-367">連絡先グループが含まれるように、次の既存のビューを変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="818f9-367">We need to modify the following existing views so that they include contact groups:</span></span>

- <span data-ttu-id="818f9-368">Views\Home\Create.aspx</span><span class="sxs-lookup"><span data-stu-id="818f9-368">Views\Home\Create.aspx</span></span>
- <span data-ttu-id="818f9-369">Views\Home\Edit.aspx</span><span class="sxs-lookup"><span data-stu-id="818f9-369">Views\Home\Edit.aspx</span></span>
- <span data-ttu-id="818f9-370">Views\Home\Index.aspx</span><span class="sxs-lookup"><span data-stu-id="818f9-370">Views\Home\Index.aspx</span></span>

<span data-ttu-id="818f9-371">このチュートリアルに付属する Visual Studio アプリケーションを調べることで変更されたビューを表示できます。</span><span class="sxs-lookup"><span data-stu-id="818f9-371">You can see the modified views by looking at the Visual Studio application that accompanies this tutorial.</span></span> <span data-ttu-id="818f9-372">たとえば、図 8 は、連絡先のインデックス ビューを示しています。</span><span class="sxs-lookup"><span data-stu-id="818f9-372">For example, Figure 8 illustrates the Contact Index view.</span></span>


<span data-ttu-id="818f9-373">[![連絡先のインデックス ビュー](iteration-6-use-test-driven-development-vb/_static/image8.jpg)](iteration-6-use-test-driven-development-vb/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="818f9-373">[![The Contact Index view](iteration-6-use-test-driven-development-vb/_static/image8.jpg)](iteration-6-use-test-driven-development-vb/_static/image15.png)</span></span>

<span data-ttu-id="818f9-374">**図 08**: 連絡先インデックス ビュー ([フルサイズの画像を表示する をクリックします](iteration-6-use-test-driven-development-vb/_static/image16.png))。</span><span class="sxs-lookup"><span data-stu-id="818f9-374">**Figure 08**: The Contact Index view([Click to view full-size image](iteration-6-use-test-driven-development-vb/_static/image16.png))</span></span>


## <a name="summary"></a><span data-ttu-id="818f9-375">まとめ</span><span class="sxs-lookup"><span data-stu-id="818f9-375">Summary</span></span>

<span data-ttu-id="818f9-376">このイテレーションでテスト駆動開発アプリケーションの設計方法に従って、Contact Manager アプリケーションに新しい機能をしました。</span><span class="sxs-lookup"><span data-stu-id="818f9-376">In this iteration, we added new functionality to our Contact Manager application by following a test-driven development application design methodology.</span></span> <span data-ttu-id="818f9-377">ユーザー ストーリーのセットを作成して開始したとします。</span><span class="sxs-lookup"><span data-stu-id="818f9-377">We started by creating a set of user stories.</span></span> <span data-ttu-id="818f9-378">ユーザー ストーリーによって表される要件に対応する一連の単体テストを作成しました。</span><span class="sxs-lookup"><span data-stu-id="818f9-378">We created a set of unit tests that corresponds to the requirements expressed by the user stories.</span></span> <span data-ttu-id="818f9-379">最後に、単体テストによって表される要件を満たすのに十分なコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="818f9-379">Finally, we wrote just enough code to satisfy the requirements expressed by the unit tests.</span></span>

<span data-ttu-id="818f9-380">単体テストによって表される要件を満たすのに十分なコードを記述完了すると、データベースとビューが更新されます。</span><span class="sxs-lookup"><span data-stu-id="818f9-380">After we finished writing enough code to satisfy the requirements expressed by the unit tests, we updated our database and views.</span></span> <span data-ttu-id="818f9-381">データベースに新しいグループのテーブルを追加し、Entity Framework データ モデルを更新します。</span><span class="sxs-lookup"><span data-stu-id="818f9-381">We added a new Groups table to our database and updated our Entity Framework Data Model.</span></span> <span data-ttu-id="818f9-382">私たちも作成され、一連のビューを変更します。</span><span class="sxs-lookup"><span data-stu-id="818f9-382">We also created and modified a set of views.</span></span>

<span data-ttu-id="818f9-383">次のイテレーション - 最後のイテレーション - は、Ajax を利用するようにアプリケーションを書き直します。</span><span class="sxs-lookup"><span data-stu-id="818f9-383">In the next iteration -- the final iteration -- we rewrite our application to take advantage of Ajax.</span></span> <span data-ttu-id="818f9-384">Ajax を利用して、応答性と Contact Manager アプリケーションのパフォーマンスを改善します。</span><span class="sxs-lookup"><span data-stu-id="818f9-384">By taking advantage of Ajax, we'll improve the responsiveness and performance of the Contact Manager application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="818f9-385">[前へ](iteration-5-create-unit-tests-vb.md)
> [次へ](iteration-7-add-ajax-functionality-vb.md)</span><span class="sxs-lookup"><span data-stu-id="818f9-385">[Previous](iteration-5-create-unit-tests-vb.md)
[Next](iteration-7-add-ajax-functionality-vb.md)</span></span>
