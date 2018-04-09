---
uid: mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-cs
title: イテレーション 6-テスト駆動開発 (c#) を使用して |Microsoft ドキュメント
author: microsoft
description: この 6 番目のイテレーションでは、アプリケーションに新しい機能を追加おには、まず単体テストを記述し、単体テストに対してコードを記述します。 このイテレーションにしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: 013c3c26-7dc3-41d1-8064-f233c86008b5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-cs
msc.type: authoredcontent
ms.openlocfilehash: 94502625f66d3eb08a24b8f2a369bf456a3367b1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="iteration-6--use-test-driven-development-c"></a><span data-ttu-id="647bb-104">イテレーション 6-テスト駆動開発 (c#) を使用します。</span><span class="sxs-lookup"><span data-stu-id="647bb-104">Iteration #6 – Use test-driven development (C#)</span></span>
====================
<span data-ttu-id="647bb-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="647bb-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="647bb-106">コードをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="647bb-106">Download Code</span></span>](iteration-6-use-test-driven-development-cs/_static/contactmanager_6_cs1.zip)

> <span data-ttu-id="647bb-107">この 6 番目のイテレーションでは、アプリケーションに新しい機能を追加おには、まず単体テストを記述し、単体テストに対してコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="647bb-107">In this sixth iteration, we add new functionality to our application by writing unit tests first and writing code against the unit tests.</span></span> <span data-ttu-id="647bb-108">このイテレーションは、連絡先グループを追加します。</span><span class="sxs-lookup"><span data-stu-id="647bb-108">In this iteration, we add contact groups.</span></span>


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a><span data-ttu-id="647bb-109">連絡先管理 ASP.NET MVC アプリケーション (c#) の構築</span><span class="sxs-lookup"><span data-stu-id="647bb-109">Building a Contact Management ASP.NET MVC Application (C#)</span></span>
  

<span data-ttu-id="647bb-110">この一連のチュートリアルは、連絡先管理アプリケーション全体が開始されてから完了するを構築します。</span><span class="sxs-lookup"><span data-stu-id="647bb-110">In this series of tutorials, we build an entire Contact Management application from start to finish.</span></span> <span data-ttu-id="647bb-111">お問い合わせのマネージャー アプリケーションでは、人のユーザーの一覧については使用すると、連絡先情報の名前、電話番号、電子メール アドレスを格納できます。</span><span class="sxs-lookup"><span data-stu-id="647bb-111">The Contact Manager application enables you to store contact information - names, phone numbers and email addresses - for a list of people.</span></span>

<span data-ttu-id="647bb-112">私たちは、複数のイテレーションにおける、アプリケーションを構築します。</span><span class="sxs-lookup"><span data-stu-id="647bb-112">We build the application over multiple iterations.</span></span> <span data-ttu-id="647bb-113">各イテレーションで、アプリケーション、徐々 に向上します。</span><span class="sxs-lookup"><span data-stu-id="647bb-113">With each iteration, we gradually improve the application.</span></span> <span data-ttu-id="647bb-114">この複数のイテレーション アプローチの目的は、各変更の理由を理解するためです。</span><span class="sxs-lookup"><span data-stu-id="647bb-114">The goal of this multiple iteration approach is to enable you to understand the reason for each change.</span></span>

- <span data-ttu-id="647bb-115">イテレーション 1 には、アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="647bb-115">Iteration #1 - Create the application.</span></span> <span data-ttu-id="647bb-116">最初のイテレーションでお連絡先のマネージャー最も簡単な方法で可能なを作成します。</span><span class="sxs-lookup"><span data-stu-id="647bb-116">In the first iteration, we create the Contact Manager in the simplest way possible.</span></span> <span data-ttu-id="647bb-117">基本的なデータベース操作のサポートを追加します: 作成、読み取り、更新、および削除 (CRUD)。</span><span class="sxs-lookup"><span data-stu-id="647bb-117">We add support for basic database operations: Create, Read, Update, and Delete (CRUD).</span></span>

- <span data-ttu-id="647bb-118">イテレーション 2 では、素敵に見えるアプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="647bb-118">Iteration #2 - Make the application look nice.</span></span> <span data-ttu-id="647bb-119">このイテレーションで、アプリケーションの外観を向上させる、既定の ASP.NET MVC ビュー マスター ページを変更し、カスケード スタイル シート。</span><span class="sxs-lookup"><span data-stu-id="647bb-119">In this iteration, we improve the appearance of the application by modifying the default ASP.NET MVC view master page and cascading style sheet.</span></span>

- <span data-ttu-id="647bb-120">イテレーション 3 - フォーム検証を追加します。</span><span class="sxs-lookup"><span data-stu-id="647bb-120">Iteration #3 - Add form validation.</span></span> <span data-ttu-id="647bb-121">3 番目のイテレーションは、基本フォーム検証を追加します。</span><span class="sxs-lookup"><span data-stu-id="647bb-121">In the third iteration, we add basic form validation.</span></span> <span data-ttu-id="647bb-122">おは人が必要なフォームのフィールドを完了しなくても、フォームを送信することを防ぐ。</span><span class="sxs-lookup"><span data-stu-id="647bb-122">We prevent people from submitting a form without completing required form fields.</span></span> <span data-ttu-id="647bb-123">私たちも電子メール アドレスと電話番号を検証します。</span><span class="sxs-lookup"><span data-stu-id="647bb-123">We also validate email addresses and phone numbers.</span></span>

- <span data-ttu-id="647bb-124">4: イテレーションは、疎結合アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="647bb-124">Iteration #4 - Make the application loosely coupled.</span></span> <span data-ttu-id="647bb-125">この 3 番目のイテレーションで利用の保守し、連絡先のマネージャー アプリケーションの変更を容易にできるようにするソフトウェア設計パターンをいくつかのです。</span><span class="sxs-lookup"><span data-stu-id="647bb-125">In this third iteration, we take advantage of several software design patterns to make it easier to maintain and modify the Contact Manager application.</span></span> <span data-ttu-id="647bb-126">たとえば、リポジトリ パターンと依存関係の挿入のパターンを使用するようにアプリケーションをリファクターします。</span><span class="sxs-lookup"><span data-stu-id="647bb-126">For example, we refactor our application to use the Repository pattern and the Dependency Injection pattern.</span></span>

- <span data-ttu-id="647bb-127">イテレーション #5 - 単体テストを作成します。</span><span class="sxs-lookup"><span data-stu-id="647bb-127">Iteration #5 - Create unit tests.</span></span> <span data-ttu-id="647bb-128">5 番目のイテレーションでおやすく、アプリケーションを維持し、単体テストを追加して変更できます。</span><span class="sxs-lookup"><span data-stu-id="647bb-128">In the fifth iteration, we make our application easier to maintain and modify by adding unit tests.</span></span> <span data-ttu-id="647bb-129">データ モデル クラスを模擬表示し、コント ローラーと検証ロジックの単体テストをビルドします。</span><span class="sxs-lookup"><span data-stu-id="647bb-129">We mock our data model classes and build unit tests for our controllers and validation logic.</span></span>

- <span data-ttu-id="647bb-130">イテレーション 6 - テスト駆動開発を使用します。</span><span class="sxs-lookup"><span data-stu-id="647bb-130">Iteration #6 - Use test-driven development.</span></span> <span data-ttu-id="647bb-131">この 6 番目のイテレーションでは、アプリケーションに新しい機能を追加おには、まず単体テストを記述し、単体テストに対してコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="647bb-131">In this sixth iteration, we add new functionality to our application by writing unit tests first and writing code against the unit tests.</span></span> <span data-ttu-id="647bb-132">このイテレーションは、連絡先グループを追加します。</span><span class="sxs-lookup"><span data-stu-id="647bb-132">In this iteration, we add contact groups.</span></span>

- <span data-ttu-id="647bb-133">イテレーション #7 - Ajax 機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="647bb-133">Iteration #7 - Add Ajax functionality.</span></span> <span data-ttu-id="647bb-134">7 番目のイテレーションでお、応答性およびパフォーマンスの向上、アプリケーションの Ajax のサポートを追加することで。</span><span class="sxs-lookup"><span data-stu-id="647bb-134">In the seventh iteration, we improve the responsiveness and performance of our application by adding support for Ajax.</span></span>

## <a name="this-iteration"></a><span data-ttu-id="647bb-135">このイテレーション</span><span class="sxs-lookup"><span data-stu-id="647bb-135">This Iteration</span></span>

<span data-ttu-id="647bb-136">連絡先のマネージャー アプリケーションの前回のイテレーションでは、コードの安全策を提供する単体テストを作成しました。</span><span class="sxs-lookup"><span data-stu-id="647bb-136">In the previous iteration of the Contact Manager application, we created unit tests to provide a safety net for our code.</span></span> <span data-ttu-id="647bb-137">単体テストを作成する動機は、コードを変更する回復力を高めることでした。</span><span class="sxs-lookup"><span data-stu-id="647bb-137">The motivation for creating the unit tests was to make our code more resilient to change.</span></span> <span data-ttu-id="647bb-138">単体テストの場所に、問題なく、コードを変更したり既存の機能が崩れているかどうかがすぐにわかることがことができます。</span><span class="sxs-lookup"><span data-stu-id="647bb-138">With unit tests in place, we can happily make any change to our code and immediately know whether we have broken existing functionality.</span></span>

<span data-ttu-id="647bb-139">このイテレーションで、単体テストを使用して、まったく異なる目的。</span><span class="sxs-lookup"><span data-stu-id="647bb-139">In this iteration, we use unit tests for an entirely different purpose.</span></span> <span data-ttu-id="647bb-140">このイテレーションは使用して単体テストと呼ばれる、アプリケーション デザインの理念の一部として*テスト駆動開発*です。</span><span class="sxs-lookup"><span data-stu-id="647bb-140">In this iteration, we use unit tests as part of an application design philosophy called *test-driven development*.</span></span> <span data-ttu-id="647bb-141">テスト駆動開発を練習するときは、まずテストを記述し、テストに対してコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="647bb-141">When you practice test-driven development, you write tests first and then write code against the tests.</span></span>

<span data-ttu-id="647bb-142">具体的には、テスト駆動開発を来た場合があるコードを作成するときに完了する 3 つの手順 (赤/緑/リファクター)。</span><span class="sxs-lookup"><span data-stu-id="647bb-142">More precisely, when practicing test-driven development, there are three steps that you complete when creating code (Red/ Green/Refactor):</span></span>

1. <span data-ttu-id="647bb-143">(赤) に失敗した単体テストを作成します。</span><span class="sxs-lookup"><span data-stu-id="647bb-143">Write a unit test that fails (Red)</span></span>
2. <span data-ttu-id="647bb-144">単体テスト (緑) に合格するコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="647bb-144">Write code that passes the unit test (Green)</span></span>
3. <span data-ttu-id="647bb-145">(リファクター) にコードをリファクタリングします。</span><span class="sxs-lookup"><span data-stu-id="647bb-145">Refactor your code (Refactor)</span></span>

<span data-ttu-id="647bb-146">最初に、単体テストを記述します。</span><span class="sxs-lookup"><span data-stu-id="647bb-146">First, you write the unit test.</span></span> <span data-ttu-id="647bb-147">単体テストでは、方法、コードが期待どおりに動作するという意図を表現があります。</span><span class="sxs-lookup"><span data-stu-id="647bb-147">The unit test should express your intention for how you expect your code to behave.</span></span> <span data-ttu-id="647bb-148">最初に、単体テストを作成するときは、単体テストは失敗します。</span><span class="sxs-lookup"><span data-stu-id="647bb-148">When you first create the unit test, the unit test should fail.</span></span> <span data-ttu-id="647bb-149">テストに合格するアプリケーション コードを記述していないために、テストは失敗する必要があります。</span><span class="sxs-lookup"><span data-stu-id="647bb-149">The test should fail because you have not yet written any application code that satisfies the test.</span></span>

<span data-ttu-id="647bb-150">次に、単体テストを成功させるためにのに十分なコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="647bb-150">Next, you write just enough code in order for the unit test to pass.</span></span> <span data-ttu-id="647bb-151">目標は、laziest、sloppiest 可能な最速の方法でコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="647bb-151">The goal is to write the code in the laziest, sloppiest and fastest possible way.</span></span> <span data-ttu-id="647bb-152">時間を考えることに、アプリケーションのアーキテクチャの概要を浪費する必要があります。</span><span class="sxs-lookup"><span data-stu-id="647bb-152">You should not waste time thinking about the architecture of your application.</span></span> <span data-ttu-id="647bb-153">代わりに、最小限の単体テストによって表される目的を満たすために必要なコードの記述に集中する必要があります。</span><span class="sxs-lookup"><span data-stu-id="647bb-153">Instead, you should focus on writing the minimal amount of code necessary to satisfy the intention expressed by the unit test.</span></span>

<span data-ttu-id="647bb-154">最後に、十分なコードを記述した後は、もう一度し、アプリケーションの全体的なアーキテクチャを検討してください。</span><span class="sxs-lookup"><span data-stu-id="647bb-154">Finally, after you have written enough code, you can step back and consider the overall architecture of your application.</span></span> <span data-ttu-id="647bb-155">この手順では、(リファクター) を書き換えてソフトウェア設計を活用して、コード パターン----リポジトリ パターンなど、コードがより優れたようにします。</span><span class="sxs-lookup"><span data-stu-id="647bb-155">In this step, you rewrite (refactor) your code by taking advantage of software design patterns -- such as the repository pattern -- so that your code is more maintainable.</span></span> <span data-ttu-id="647bb-156">コードは、単体テストでカバーされるために fearlessly、この手順では、コードを書き直すことができます。</span><span class="sxs-lookup"><span data-stu-id="647bb-156">You can fearlessly rewrite your code in this step because your code is covered by unit tests.</span></span>

<span data-ttu-id="647bb-157">テスト駆動開発の実践に起因する多くの利点があります。</span><span class="sxs-lookup"><span data-stu-id="647bb-157">There are many benefits that result from practicing test-driven development.</span></span> <span data-ttu-id="647bb-158">まず、テスト駆動開発強制的に実際に書き込まれる必要のあるコードに重点をします。</span><span class="sxs-lookup"><span data-stu-id="647bb-158">First, test-driven development forces you to focus on code that actually needs to be written.</span></span> <span data-ttu-id="647bb-159">常に記述された特定のテストに合格するための十分なコードに注目している、ため、雑草にいると、膨大な量の使用していない場合はコードを記述できません。</span><span class="sxs-lookup"><span data-stu-id="647bb-159">Because you are constantly focused on just writing enough code to pass a particular test, you are prevented from wandering into the weeds and writing massive amounts of code that you will never use.</span></span>

<span data-ttu-id="647bb-160">次に、"test 最初"の設計方法論強制的にコードを記述するコードの使用方法の観点からします。</span><span class="sxs-lookup"><span data-stu-id="647bb-160">Second, a "test first" design methodology forces you to write code from the perspective of how your code will be used.</span></span> <span data-ttu-id="647bb-161">つまり、テスト駆動開発を来たときに常に、テストを作成するユーザーの観点からです。</span><span class="sxs-lookup"><span data-stu-id="647bb-161">In other words, when practicing test-driven development, you are constantly writing your tests from a user perspective.</span></span> <span data-ttu-id="647bb-162">そのため、テスト駆動開発と、わかりやすく Api があります。</span><span class="sxs-lookup"><span data-stu-id="647bb-162">Therefore, test-driven development can result in cleaner and more understandable APIs.</span></span>

<span data-ttu-id="647bb-163">最後に、テスト駆動開発を強制的を通常のアプリケーションを作成するプロセスの一環として単体テストを記述します。</span><span class="sxs-lookup"><span data-stu-id="647bb-163">Finally, test-driven development forces you to write unit tests as part of the normal process of writing an application.</span></span> <span data-ttu-id="647bb-164">プロジェクトの期限に近づくにつれて、テストは通常、最初に行う、ウィンドウがなくなることです。</span><span class="sxs-lookup"><span data-stu-id="647bb-164">As a project deadline approaches, testing is typically the first thing that goes out the window.</span></span> <span data-ttu-id="647bb-165">テスト駆動開発を来たときにその一方がテスト駆動開発では、単体テスト サーバーの全体のアプリケーションのビルド プロセスに単体テストの記述に関する好である可能性が高くなります。</span><span class="sxs-lookup"><span data-stu-id="647bb-165">When practicing test-driven development, on the other hand, you are more likely to be virtuous about writing unit tests because test-driven development makes unit tests central to the process of building an application.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="647bb-166">テスト駆動開発の詳細については、ことをお勧め Michael の受信可能方向 book お読みになる**レガシ コードを効率的に作業**です。</span><span class="sxs-lookup"><span data-stu-id="647bb-166">To learn more about test-driven development, I recommend that you read Michael Feathers book **Working Effectively with Legacy Code**.</span></span>


<span data-ttu-id="647bb-167">このイテレーションでは、新しい機能を追加お、連絡先のマネージャー アプリケーションにします。</span><span class="sxs-lookup"><span data-stu-id="647bb-167">In this iteration, we add a new feature to our Contact Manager application.</span></span> <span data-ttu-id="647bb-168">連絡先グループのサポートを追加します。</span><span class="sxs-lookup"><span data-stu-id="647bb-168">We add support for Contact Groups.</span></span> <span data-ttu-id="647bb-169">ビジネスなどのカテゴリに連絡先を整理する連絡先グループと、友人グループを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="647bb-169">You can use Contact Groups to organize your contacts into categories such as Business and Friend groups.</span></span>

<span data-ttu-id="647bb-170">テスト駆動開発のプロセスに従うことによってこの新機能をアプリケーションに追加します。</span><span class="sxs-lookup"><span data-stu-id="647bb-170">We'll add this new functionality to our application by following a process of test-driven development.</span></span> <span data-ttu-id="647bb-171">最初の単体テストを記述し、すべてのこれらのテストに対して、コードを記述します。</span><span class="sxs-lookup"><span data-stu-id="647bb-171">We'll write our unit tests first and we'll write all of our code against these tests.</span></span>

## <a name="what-gets-tested"></a><span data-ttu-id="647bb-172">テストのものを取得</span><span class="sxs-lookup"><span data-stu-id="647bb-172">What Gets Tested</span></span>

<span data-ttu-id="647bb-173">前回のイテレーションで説明したよう通常データ アクセス ロジックの単体テストを記述したりしないロジックを表示します。</span><span class="sxs-lookup"><span data-stu-id="647bb-173">As we discussed in the previous iteration, you typically do not write unit tests for data access logic or view logic.</span></span> <span data-ttu-id="647bb-174">比較的低速の操作は、データベースにアクセスするため、データ アクセス ロジックの t 書き込み単体テストがありません。</span><span class="sxs-lookup"><span data-stu-id="647bb-174">You don t write unit tests for data access logic because accessing a database is a relatively slow operation.</span></span> <span data-ttu-id="647bb-175">比較的低速の操作では、web サーバーをスピンアップ ビューにアクセスする必要があるために、ビュー ロジックの t 書き込み単体テストがありません。</span><span class="sxs-lookup"><span data-stu-id="647bb-175">You don t write unit tests for view logic because accessing a view requires spinning up a web server which is a relatively slow operation.</span></span> <span data-ttu-id="647bb-176">T べきでは、テスト実行できる何度も非常に高速しない限り、単体テストを記述します。</span><span class="sxs-lookup"><span data-stu-id="647bb-176">You shouldn t write a unit test unless the test can be executed over and over again very fast</span></span>

<span data-ttu-id="647bb-177">テスト駆動開発は単体テストによって促進されて、ためお重点最初にコント ローラーとビジネス ロジックを記述します。</span><span class="sxs-lookup"><span data-stu-id="647bb-177">Because test-driven development is driven by unit tests, we focus initially on writing controller and business logic.</span></span> <span data-ttu-id="647bb-178">データベースまたはビューを変更することは避けてください。</span><span class="sxs-lookup"><span data-stu-id="647bb-178">We avoid touching the database or views.</span></span> <span data-ttu-id="647bb-179">T 獲得したデータベースを変更したり、このチュートリアルの最後まで、ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="647bb-179">We won t modify the database or create our views until the very end of this tutorial.</span></span> <span data-ttu-id="647bb-180">新機能をテストできるように開始します。</span><span class="sxs-lookup"><span data-stu-id="647bb-180">We start with what can be tested.</span></span>

## <a name="creating-user-stories"></a><span data-ttu-id="647bb-181">ユーザー ストーリーを作成します。</span><span class="sxs-lookup"><span data-stu-id="647bb-181">Creating User Stories</span></span>

<span data-ttu-id="647bb-182">テスト駆動開発を来たときに常にまずテストを作成します。</span><span class="sxs-lookup"><span data-stu-id="647bb-182">When practicing test-driven development, you always start by writing a test.</span></span> <span data-ttu-id="647bb-183">これは、直後に疑問が出てきます: 最初に記述するには、どのようなテストを決定するか。</span><span class="sxs-lookup"><span data-stu-id="647bb-183">This immediately raises the question: How do you decide what test to write first?</span></span> <span data-ttu-id="647bb-184">この質問に答えるのセットを記述する必要があります[**ユーザー ストーリー**](http://en.wikipedia.org/wiki/User_stories)です。</span><span class="sxs-lookup"><span data-stu-id="647bb-184">To answer this question, you should write a set of [**user stories**](http://en.wikipedia.org/wiki/User_stories).</span></span>

<span data-ttu-id="647bb-185">ユーザー ストーリーは、ソフトウェアの要件の非常に短く (通常 1 文) 説明です。</span><span class="sxs-lookup"><span data-stu-id="647bb-185">A user story is a very brief (usually one sentence) description of a software requirement.</span></span> <span data-ttu-id="647bb-186">ユーザーの観点から書き込まれた要件の技術的説明になります。</span><span class="sxs-lookup"><span data-stu-id="647bb-186">It should be a non-technical description of a requirement written from a user perspective.</span></span>

<span data-ttu-id="647bb-187">ここでは、新しい連絡先グループ機能で必要な機能について説明しているユーザー ストーリーのセット。</span><span class="sxs-lookup"><span data-stu-id="647bb-187">Here s the set of user stories that describe the features required by the new Contact Group functionality:</span></span>

1. <span data-ttu-id="647bb-188">ユーザーは、連絡先グループの一覧を表示できます。</span><span class="sxs-lookup"><span data-stu-id="647bb-188">User can view a list of contact groups.</span></span>
2. <span data-ttu-id="647bb-189">ユーザーは、新しい連絡先グループを作成できます。</span><span class="sxs-lookup"><span data-stu-id="647bb-189">User can create a new contact group.</span></span>
3. <span data-ttu-id="647bb-190">ユーザーは、既存の連絡先グループを削除できます。</span><span class="sxs-lookup"><span data-stu-id="647bb-190">User can delete an existing contact group.</span></span>
4. <span data-ttu-id="647bb-191">新しい連絡先を作成するときに、ユーザーは連絡先グループを選択できます。</span><span class="sxs-lookup"><span data-stu-id="647bb-191">User can select a contact group when creating a new contact.</span></span>
5. <span data-ttu-id="647bb-192">既存の連絡先を編集する場合、ユーザーは連絡先グループを選択できます。</span><span class="sxs-lookup"><span data-stu-id="647bb-192">User can select a contact group when editing an existing contact.</span></span>
6. <span data-ttu-id="647bb-193">連絡先グループの一覧は、インデックス ビューに表示されます。</span><span class="sxs-lookup"><span data-stu-id="647bb-193">A list of contact groups is displayed in the Index view.</span></span>
7. <span data-ttu-id="647bb-194">ユーザーは、連絡先グループをクリックすると、該当する連絡先の一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="647bb-194">When a user clicks a contact group, a list of matching contacts is displayed.</span></span>

<span data-ttu-id="647bb-195">このユーザー ストーリーの一覧が、顧客が完全に理解できるものであることを確認します。</span><span class="sxs-lookup"><span data-stu-id="647bb-195">Notice that this list of user stories is completely understandable by a customer.</span></span> <span data-ttu-id="647bb-196">技術的な実装詳細の記述は含まれません。</span><span class="sxs-lookup"><span data-stu-id="647bb-196">There is no mention of technical implementation details.</span></span>

<span data-ttu-id="647bb-197">過程でアプリケーションをビルドして、一連のユーザー ストーリーがより洗練されたになることができます。</span><span class="sxs-lookup"><span data-stu-id="647bb-197">While in the process of building your application, the set of user stories can become more refined.</span></span> <span data-ttu-id="647bb-198">複数のストーリー (要件など) をユーザー ストーリーを中断する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="647bb-198">You might break a user story into multiple stories (requirements).</span></span> <span data-ttu-id="647bb-199">たとえば、新しい連絡先グループを作成する必要があります含まれることの検証を決定する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="647bb-199">For example, you might decide that creating a new contact group should involve validation.</span></span> <span data-ttu-id="647bb-200">名前のない連絡先グループを送信すると、検証エラーを返す必要があります。</span><span class="sxs-lookup"><span data-stu-id="647bb-200">Submitting a contact group without a name should return a validation error.</span></span>

<span data-ttu-id="647bb-201">ユーザー ストーリーの一覧を作成した後は、最初の単体テストを記述する準備ができたらです。</span><span class="sxs-lookup"><span data-stu-id="647bb-201">After you create a list of user stories, you are ready to write your first unit test.</span></span> <span data-ttu-id="647bb-202">連絡先グループの一覧を表示するための単体テストを作成することで始めます。</span><span class="sxs-lookup"><span data-stu-id="647bb-202">We'll start by creating a unit test for viewing the list of contact groups.</span></span>

## <a name="listing-contact-groups"></a><span data-ttu-id="647bb-203">連絡先グループを一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="647bb-203">Listing Contact Groups</span></span>

<span data-ttu-id="647bb-204">この最初のユーザー ストーリーは、ユーザーが連絡先グループの一覧を表示できることです。</span><span class="sxs-lookup"><span data-stu-id="647bb-204">Our first user story is that a user should be able to view a list of contact groups.</span></span> <span data-ttu-id="647bb-205">このストーリーをテストして express が必要です。</span><span class="sxs-lookup"><span data-stu-id="647bb-205">We need to express this story with a test.</span></span>

<span data-ttu-id="647bb-206">ContactManager.Tests プロジェクトで、Controllers フォルダーを右クリックして新しい単体テストの作成を選択すると**追加]、[新しいテスト**を選択して、**単体テスト**テンプレート (図 1 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="647bb-206">Create a new unit test by right-clicking the Controllers folder in the ContactManager.Tests project, selecting **Add, New Test**, and selecting the **Unit Test** template (see Figure 1).</span></span> <span data-ttu-id="647bb-207">名前の新しい単位が GroupControllerTest.cs をテストし、をクリックして、 **OK**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="647bb-207">Name the new unit test GroupControllerTest.cs and click the **OK** button.</span></span>


<span data-ttu-id="647bb-208">[![GroupControllerTest 単体テストを追加します。](iteration-6-use-test-driven-development-cs/_static/image1.jpg)](iteration-6-use-test-driven-development-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="647bb-208">[![Adding the GroupControllerTest unit test](iteration-6-use-test-driven-development-cs/_static/image1.jpg)](iteration-6-use-test-driven-development-cs/_static/image1.png)</span></span>

<span data-ttu-id="647bb-209">**図 01**: GroupControllerTest 単体テストを追加する ([フルサイズのイメージを表示するをクリックして](iteration-6-use-test-driven-development-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="647bb-209">**Figure 01**: Adding the GroupControllerTest unit test([Click to view full-size image](iteration-6-use-test-driven-development-cs/_static/image2.png))</span></span>


<span data-ttu-id="647bb-210">最初の単体テストは、1 のリストに含まれます。</span><span class="sxs-lookup"><span data-stu-id="647bb-210">Our first unit test is contained in Listing 1.</span></span> <span data-ttu-id="647bb-211">このテストでは、グループ コント ローラーの Index() メソッドがグループのセットを返すことを確認します。</span><span class="sxs-lookup"><span data-stu-id="647bb-211">This test verifies that the Index() method of the Group controller returns a set of Groups.</span></span> <span data-ttu-id="647bb-212">テストでは、グループのコレクションがビュー内のデータで返されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="647bb-212">The test verifies that a collection of Groups is returned in view data.</span></span>

<span data-ttu-id="647bb-213">**1 - Controllers\GroupControllerTest.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="647bb-213">**Listing 1 - Controllers\GroupControllerTest.cs**</span></span>

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample1.cs)]

<span data-ttu-id="647bb-214">最初に、Visual Studio でのリストの 1 のコードを入力すると、ときに多数の赤い波線が表示されます。</span><span class="sxs-lookup"><span data-stu-id="647bb-214">When you first type the code in Listing 1 in Visual Studio, you'll get a lot of red squiggly lines.</span></span> <span data-ttu-id="647bb-215">GroupController またはグループのクラスを作成していません。</span><span class="sxs-lookup"><span data-stu-id="647bb-215">We have not created the GroupController or Group classes.</span></span>

<span data-ttu-id="647bb-216">この時点では、t でもビルドことができないので、アプリケーションは、最初の単体テストを実行します。</span><span class="sxs-lookup"><span data-stu-id="647bb-216">At this point, we can t even build our application so we can t execute our first unit test.</span></span> <span data-ttu-id="647bb-217">お勧めします。</span><span class="sxs-lookup"><span data-stu-id="647bb-217">That s good.</span></span> <span data-ttu-id="647bb-218">失敗したテストとしてをカウントします。</span><span class="sxs-lookup"><span data-stu-id="647bb-218">That counts as a failing test.</span></span> <span data-ttu-id="647bb-219">そのため、お今すぐアクセス許可を持つをアプリケーション コードの記述を開始します。</span><span class="sxs-lookup"><span data-stu-id="647bb-219">Therefore, we now have permission to start writing application code.</span></span> <span data-ttu-id="647bb-220">このテストを実行するための十分なコードを記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="647bb-220">We need to write enough code to execute our test.</span></span>

<span data-ttu-id="647bb-221">リスト 2 でグループのコント ローラー クラスには、単体テストに合格するために必要なコードの最低限が含まれています。</span><span class="sxs-lookup"><span data-stu-id="647bb-221">The Group controller class in Listing 2 contains the bare minimum of code required to pass the unit test.</span></span> <span data-ttu-id="647bb-222">Index() アクションでは、グループ (グループ クラスは一覧表示する 3 で定義) の静的にコード化された一覧を返します。</span><span class="sxs-lookup"><span data-stu-id="647bb-222">The Index() action returns a statically coded list of Groups (the Group class is defined in Listing 3).</span></span>

<span data-ttu-id="647bb-223">**2 - Controllers\GroupController.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="647bb-223">**Listing 2 - Controllers\GroupController.cs**</span></span>

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample2.cs)]

<span data-ttu-id="647bb-224">**3 - Models\Group.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="647bb-224">**Listing 3 - Models\Group.cs**</span></span>

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample3.cs)]

<span data-ttu-id="647bb-225">プロジェクトに GroupController およびグループのクラスを追加お後、最初の単体テストが正常に完了 (図 2 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="647bb-225">After we add the GroupController and Group classes to our project, our first unit test completes successfully (see Figure 2).</span></span> <span data-ttu-id="647bb-226">テストに合格するために必要な最小限の作業を行っています。</span><span class="sxs-lookup"><span data-stu-id="647bb-226">We have done the minimum work required to pass the test.</span></span> <span data-ttu-id="647bb-227">時間を記念することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="647bb-227">It is time to celebrate.</span></span>


<span data-ttu-id="647bb-228">[![正常にされました。](iteration-6-use-test-driven-development-cs/_static/image2.jpg)](iteration-6-use-test-driven-development-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="647bb-228">[![Success!](iteration-6-use-test-driven-development-cs/_static/image2.jpg)](iteration-6-use-test-driven-development-cs/_static/image3.png)</span></span>

<span data-ttu-id="647bb-229">**図 02**: 成功! ([フルサイズのイメージを表示するをクリックして](iteration-6-use-test-driven-development-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="647bb-229">**Figure 02**: Success!([Click to view full-size image](iteration-6-use-test-driven-development-cs/_static/image4.png))</span></span>


## <a name="creating-contact-groups"></a><span data-ttu-id="647bb-230">連絡先グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="647bb-230">Creating Contact Groups</span></span>

<span data-ttu-id="647bb-231">これで、2 番目のユーザー ストーリーに移動できます。</span><span class="sxs-lookup"><span data-stu-id="647bb-231">Now we can move on to the second user story.</span></span> <span data-ttu-id="647bb-232">新しい連絡先グループを作成できるようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="647bb-232">We need to be able to create new contact groups.</span></span> <span data-ttu-id="647bb-233">テストでこの目的を表現する必要があります。</span><span class="sxs-lookup"><span data-stu-id="647bb-233">We need to express this intention with a test.</span></span>

<span data-ttu-id="647bb-234">リスト 4 内のテストでは、新しいグループを持つメソッドが Index() メソッドによって返されるグループの一覧に対してグループを追加 Create() を呼び出しているを確認します。</span><span class="sxs-lookup"><span data-stu-id="647bb-234">The test in Listing 4 verifies that calling the Create() method with a new Group adds the Group to the list of Groups returned by the Index() method.</span></span> <span data-ttu-id="647bb-235">つまり、新しいグループを作成する場合、すべき Index() メソッドによって返されるグループの一覧から戻り、新しいグループを取得できません。</span><span class="sxs-lookup"><span data-stu-id="647bb-235">In other words, if I create a new group then I should be able to get the new Group back from the list of Groups returned by the Index() method.</span></span>

<span data-ttu-id="647bb-236">**4 - Controllers\GroupControllerTest.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="647bb-236">**Listing 4 - Controllers\GroupControllerTest.cs**</span></span>

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample4.cs)]

<span data-ttu-id="647bb-237">リスト 4 内のテストでは、グループ コント ローラーの新しい連絡先グループの Create() メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="647bb-237">The test in Listing 4 calls the Group controller Create() method with a new contact Group.</span></span> <span data-ttu-id="647bb-238">次に、テストでは、データの表示で新しいグループを返すグループ コント ローラー Index() メソッドを呼び出すことを確認します。</span><span class="sxs-lookup"><span data-stu-id="647bb-238">Next, the test verifies that calling the Group controller Index() method returns the new Group in view data.</span></span>

<span data-ttu-id="647bb-239">変更されたグループ内のコント ローラー 5 の一覧表示するには、新しいテストに合格するために必要な変更の最低限が含まれています。</span><span class="sxs-lookup"><span data-stu-id="647bb-239">The modified Group controller in Listing 5 contains the bare minimum of changes required to pass the new test.</span></span>

<span data-ttu-id="647bb-240">**5 - Controllers\GroupController.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="647bb-240">**Listing 5 - Controllers\GroupController.cs**</span></span>

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample5.cs)]

## <a name="the-group-controller-in-listing-5-has-a-new-create-action-this-action-adds-a-group-to-a-collection-of-groups-notice-that-the-index-action-has-been-modified-to-return-the-contents-of-the-collection-of-groups"></a><span data-ttu-id="647bb-241">コード サンプル 5 グループ コント ローラーは、新しい Create() アクションを持ちます。</span><span class="sxs-lookup"><span data-stu-id="647bb-241">The Group controller in Listing 5 has a new Create() action.</span></span> <span data-ttu-id="647bb-242">この操作は、グループのコレクションにグループを追加します。</span><span class="sxs-lookup"><span data-stu-id="647bb-242">This action adds a Group to a collection of Groups.</span></span> <span data-ttu-id="647bb-243">グループのコレクションのコンテンツを返す Index() アクションが変更されたことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="647bb-243">Notice that the Index() action has been modified to return the contents of the collection of Groups.</span></span>

<span data-ttu-id="647bb-244">もう一度おベア最小限の単体テストに合格するために必要な作業を実行しています。</span><span class="sxs-lookup"><span data-stu-id="647bb-244">Once again, we have performed the bare minimum amount of work required to pass the unit test.</span></span> <span data-ttu-id="647bb-245">グループ コント ローラーにこれらの変更は後で、すべてのユニットのテストに成功します。</span><span class="sxs-lookup"><span data-stu-id="647bb-245">After we make these changes to the Group controller, all of our unit tests pass.</span></span>

## <a name="adding-validation"></a><span data-ttu-id="647bb-246">検証の追加</span><span class="sxs-lookup"><span data-stu-id="647bb-246">Adding Validation</span></span>

<span data-ttu-id="647bb-247">この要件は、ユーザー ストーリーで明示的に報告されていません。</span><span class="sxs-lookup"><span data-stu-id="647bb-247">This requirement was not stated explicitly in the user story.</span></span> <span data-ttu-id="647bb-248">ただし、グループが名前を付けることを要求するように適切なは。</span><span class="sxs-lookup"><span data-stu-id="647bb-248">However, it is reasonable to require that a Group have a name.</span></span> <span data-ttu-id="647bb-249">それ以外の場合、アドレス帳をグループに編成できません非常に便利です。</span><span class="sxs-lookup"><span data-stu-id="647bb-249">Otherwise, organizing contacts into Groups would not be very useful.</span></span>

<span data-ttu-id="647bb-250">6 を一覧表示するには、この目的を表す新しいテストが含まれます。</span><span class="sxs-lookup"><span data-stu-id="647bb-250">Listing 6 contains a new test that expresses this intention.</span></span> <span data-ttu-id="647bb-251">このテストでは、モデルの状態の検証エラー メッセージに名前の結果を指定せず、グループを作成しようとしています。 ことを確認します。</span><span class="sxs-lookup"><span data-stu-id="647bb-251">This test verifies that attempting to create a Group without supplying a name results in a validation error message in model state.</span></span>

<span data-ttu-id="647bb-252">**6 - Controllers\GroupControllerTest.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="647bb-252">**Listing 6 - Controllers\GroupControllerTest.cs**</span></span>

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample6.cs)]

<span data-ttu-id="647bb-253">このテストを満たすために、グループのクラス (7 のリストを参照) に、Name プロパティを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="647bb-253">In order to satisfy this test, we need to add a Name property to our Group class (see Listing 7).</span></span> <span data-ttu-id="647bb-254">さらに、このグループのコント ローラー s Create() アクション (8 のリストを参照) にわずかな検証ロジックを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="647bb-254">Furthermore, we need to add a tiny bit of validation logic to our Group controller s Create() action (see Listing 8).</span></span>

<span data-ttu-id="647bb-255">**7 - Models\Group.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="647bb-255">**Listing 7 - Models\Group.cs**</span></span>

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample7.cs)]

<span data-ttu-id="647bb-256">**8 - Controllers\GroupController.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="647bb-256">**Listing 8 - Controllers\GroupController.cs**</span></span>

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample8.cs)]

<span data-ttu-id="647bb-257">グループのコント ローラー Create() 処理をすぐに検証とデータベースの両方のロジックが含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="647bb-257">Notice that the Group controller Create() action now contains both validation and database logic.</span></span> <span data-ttu-id="647bb-258">現時点では、グループのコント ローラーによって使用されるデータベースは、メモリ内コレクションよりも詳細 nothing で構成されます。</span><span class="sxs-lookup"><span data-stu-id="647bb-258">Currently, the database used by the Group controller consists of nothing more than an in-memory collection.</span></span>

## <a name="time-to-refactor"></a><span data-ttu-id="647bb-259">リファクタリングする時間</span><span class="sxs-lookup"><span data-stu-id="647bb-259">Time to Refactor</span></span>

<span data-ttu-id="647bb-260">赤/緑/リファクタリングの 3 番目の手順では、リファクタリングの部分です。</span><span class="sxs-lookup"><span data-stu-id="647bb-260">The third step in Red/Green/Refactor is the Refactor part.</span></span> <span data-ttu-id="647bb-261">この時点では、そのデザインを向上させるためにアプリケーションをリファクターお方法を検討してください、コードからステップする必要があります。</span><span class="sxs-lookup"><span data-stu-id="647bb-261">At this point, we need to step back from our code and consider how we can refactor our application to improve its design.</span></span> <span data-ttu-id="647bb-262">リファクタリングを行うステージは、ステージを考えてハード ソフトウェア設計の原則、およびパターンを実装する最適な方法です。</span><span class="sxs-lookup"><span data-stu-id="647bb-262">The Refactor stage is the stage at which we think hard about the best way of implementing software design principles and patterns.</span></span>

<span data-ttu-id="647bb-263">コードの設計を向上させるために選択する任意の方法でコードを変更するために解放います。</span><span class="sxs-lookup"><span data-stu-id="647bb-263">We are free to modify our code in any way that we choose to improve the design of the code.</span></span> <span data-ttu-id="647bb-264">既存の機能を損なうを妨げている単体テストの安全策があります。</span><span class="sxs-lookup"><span data-stu-id="647bb-264">We have a safety net of unit tests that prevent us from breaking existing functionality.</span></span>

<span data-ttu-id="647bb-265">現在、当社グループ コント ローラーは優れたソフトウェア設計の観点から乱雑です。</span><span class="sxs-lookup"><span data-stu-id="647bb-265">Right now, our Group controller is a mess from the perspective of good software design.</span></span> <span data-ttu-id="647bb-266">グループのコント ローラーには、絡み合った乱雑な検証とデータ アクセス コードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="647bb-266">The Group controller contains a tangled mess of validation and data access code.</span></span> <span data-ttu-id="647bb-267">単一の責任の原則に違反するを回避するのには、これらの問題をさまざまなクラスに分割する必要があります。</span><span class="sxs-lookup"><span data-stu-id="647bb-267">To avoid violating the Single Responsibility Principle, we need to separate these concerns into different classes.</span></span>

<span data-ttu-id="647bb-268">リファクタリングされたグループ コント ローラー クラスは、9 の一覧に含まれます。</span><span class="sxs-lookup"><span data-stu-id="647bb-268">Our refactored Group controller class is contained in Listing 9.</span></span> <span data-ttu-id="647bb-269">ContactManager サービス層を使用するには、コント ローラーを変更されています。</span><span class="sxs-lookup"><span data-stu-id="647bb-269">The controller has been modified to use the ContactManager service layer.</span></span> <span data-ttu-id="647bb-270">これは、連絡先コント ローラーを使用して、同じサービス レイヤーです。</span><span class="sxs-lookup"><span data-stu-id="647bb-270">This is the same service layer that we use with the Contact controller.</span></span>

<span data-ttu-id="647bb-271">10 を一覧表示するには、検証、一覧表示、およびグループの作成をサポートするために ContactManager サービス層に追加された新しいメソッドが含まれます。</span><span class="sxs-lookup"><span data-stu-id="647bb-271">Listing 10 contains the new methods added to the ContactManager service layer to support validating, listing, and creating groups.</span></span> <span data-ttu-id="647bb-272">新しいメソッドを含める IContactManagerService インターフェイスが更新されました。</span><span class="sxs-lookup"><span data-stu-id="647bb-272">The IContactManagerService interface was updated to include the new methods.</span></span>

<span data-ttu-id="647bb-273">11 を一覧表示するには、IContactManagerRepository インターフェイスを実装する新しい FakeContactManagerRepository クラスが含まれます。</span><span class="sxs-lookup"><span data-stu-id="647bb-273">Listing 11 contains a new FakeContactManagerRepository class that implements the IContactManagerRepository interface.</span></span> <span data-ttu-id="647bb-274">インターフェイスを実装する、IContactManagerRepository EntityContactManagerRepository クラスとは異なり、新しい FakeContactManagerRepository クラスは、データベースと通信しません。</span><span class="sxs-lookup"><span data-stu-id="647bb-274">Unlike the EntityContactManagerRepository class that also implements the IContactManagerRepository interface, our new FakeContactManagerRepository class does not communicate with the database.</span></span> <span data-ttu-id="647bb-275">FakeContactManagerRepository クラスは、データベースのプロキシとして、メモリ内コレクションを使用します。</span><span class="sxs-lookup"><span data-stu-id="647bb-275">The FakeContactManagerRepository class uses an in-memory collection as a proxy for the database.</span></span> <span data-ttu-id="647bb-276">このクラスの単体テストで偽リポジトリ レイヤーとして使用されます。</span><span class="sxs-lookup"><span data-stu-id="647bb-276">We'll use this class in our unit tests as a fake repository layer.</span></span>

<span data-ttu-id="647bb-277">**9 - Controllers\GroupController.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="647bb-277">**Listing 9 - Controllers\GroupController.cs**</span></span>

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample9.cs)]

<span data-ttu-id="647bb-278">**10 - Controllers\ContactManagerService.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="647bb-278">**Listing 10 - Controllers\ContactManagerService.cs**</span></span>

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample10.cs)]

<span data-ttu-id="647bb-279">**11 - Controllers\FakeContactManagerRepository.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="647bb-279">**Listing 11 - Controllers\FakeContactManagerRepository.cs**</span></span>

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample11.cs)]

<span data-ttu-id="647bb-280">使用して EntityContactManagerRepository クラスで、CreateGroup() および ListGroups() メソッドを実装するインターフェイスが必要です IContactManagerRepository を変更します。</span><span class="sxs-lookup"><span data-stu-id="647bb-280">Modifying the IContactManagerRepository interface requires use to implement the CreateGroup() and ListGroups() methods in the EntityContactManagerRepository class.</span></span> <span data-ttu-id="647bb-281">これを行う laziest と最も高速な方法は、次のようにスタブ メソッドを追加してです。</span><span class="sxs-lookup"><span data-stu-id="647bb-281">The laziest and fastest way to do this is to add stub methods that look like this:</span></span>   

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample12.cs)]


<span data-ttu-id="647bb-282">最後に、これらの変更をアプリケーションの設計では、単体テストに変更を加えることが必要です。</span><span class="sxs-lookup"><span data-stu-id="647bb-282">Finally, these changes to the design of our application require us to make some modifications to our unit tests.</span></span> <span data-ttu-id="647bb-283">これで、単体テストを実行するときに、FakeContactManagerRepository を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="647bb-283">We now need to use the FakeContactManagerRepository when performing the unit tests.</span></span> <span data-ttu-id="647bb-284">更新後の GroupControllerTest クラスは、12 の一覧に含まれます。</span><span class="sxs-lookup"><span data-stu-id="647bb-284">The updated GroupControllerTest class is contained in Listing 12.</span></span>

<span data-ttu-id="647bb-285">**12 - Controllers\GroupControllerTest.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="647bb-285">**Listing 12 - Controllers\GroupControllerTest.cs**</span></span>

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample13.cs)]

<span data-ttu-id="647bb-286">これらすべてを作成した後の変更をもう一度すべての単体テストの成功のです。</span><span class="sxs-lookup"><span data-stu-id="647bb-286">After we make all of these changes, once again, all of our unit tests pass.</span></span> <span data-ttu-id="647bb-287">赤/緑/リファクタリングのサイクル全体が完了しました。</span><span class="sxs-lookup"><span data-stu-id="647bb-287">We have completed the entire cycle of Red/Green/Refactor.</span></span> <span data-ttu-id="647bb-288">最初の 2 つのユーザー ストーリーが実装されました。</span><span class="sxs-lookup"><span data-stu-id="647bb-288">We have implemented the first two user stories.</span></span> <span data-ttu-id="647bb-289">ユーザー ストーリーで示される要件の単体テストをサポートするがあるようになりました。</span><span class="sxs-lookup"><span data-stu-id="647bb-289">We now have supporting unit tests for the requirements expressed in the user stories.</span></span> <span data-ttu-id="647bb-290">ユーザー ストーリーの残りの部分を実装するには、赤/緑/リファクタリングの同じサイクルを繰り返す必要があります。</span><span class="sxs-lookup"><span data-stu-id="647bb-290">Implementing the remainder of the user stories involves repeating the same cycle of Red/Green/Refactor.</span></span>

## <a name="modifying-our-database"></a><span data-ttu-id="647bb-291">データベースを変更します。</span><span class="sxs-lookup"><span data-stu-id="647bb-291">Modifying our Database</span></span>

<span data-ttu-id="647bb-292">残念ながら、すべての単体テストで示される要件を満たすお、場合でも、作業は行われません。</span><span class="sxs-lookup"><span data-stu-id="647bb-292">Unfortunately, even though we have satisfied all of the requirements expressed by our unit tests, our work is not done.</span></span> <span data-ttu-id="647bb-293">このデータベースを変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="647bb-293">We still need to modify our database.</span></span>

<span data-ttu-id="647bb-294">新しいグループのデータベース テーブルを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="647bb-294">We need to create a new Group database table.</span></span> <span data-ttu-id="647bb-295">この場合は、以下の手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="647bb-295">Follow these steps:</span></span>

1. <span data-ttu-id="647bb-296">サーバー エクスプ ローラー ウィンドウで テーブル フォルダーを右クリックし、メニュー オプションを選択**新しいテーブルの追加**です。</span><span class="sxs-lookup"><span data-stu-id="647bb-296">In the Server Explorer window, right-click the Tables folder and select the menu option **Add New Table**.</span></span>
2. <span data-ttu-id="647bb-297">次のテーブル デザイナーで説明する 2 つの列を入力します。</span><span class="sxs-lookup"><span data-stu-id="647bb-297">Enter the two columns described below in the Table Designer.</span></span>
3. <span data-ttu-id="647bb-298">Primary key 制約と Identity 列として Id 列をマークします。</span><span class="sxs-lookup"><span data-stu-id="647bb-298">Mark the Id column as a primary key and Identity column.</span></span>
4. <span data-ttu-id="647bb-299">名前のグループを含む新しいテーブルを保存するには、フロッピー ディスクのアイコンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="647bb-299">Save the new table with the name Groups by clicking the icon of the floppy.</span></span>

<a id="0.11_table01"></a>


| <span data-ttu-id="647bb-300">**列名**</span><span class="sxs-lookup"><span data-stu-id="647bb-300">**Column Name**</span></span> | <span data-ttu-id="647bb-301">**データ型**</span><span class="sxs-lookup"><span data-stu-id="647bb-301">**Data Type**</span></span> | <span data-ttu-id="647bb-302">**Null を許容します。**</span><span class="sxs-lookup"><span data-stu-id="647bb-302">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="647bb-303">ID</span><span class="sxs-lookup"><span data-stu-id="647bb-303">Id</span></span> | <span data-ttu-id="647bb-304">int</span><span class="sxs-lookup"><span data-stu-id="647bb-304">int</span></span> | <span data-ttu-id="647bb-305">False</span><span class="sxs-lookup"><span data-stu-id="647bb-305">False</span></span> |
| <span data-ttu-id="647bb-306">名前</span><span class="sxs-lookup"><span data-stu-id="647bb-306">Name</span></span> | <span data-ttu-id="647bb-307">nvarchar(50)</span><span class="sxs-lookup"><span data-stu-id="647bb-307">nvarchar(50)</span></span> | <span data-ttu-id="647bb-308">False</span><span class="sxs-lookup"><span data-stu-id="647bb-308">False</span></span> |


<span data-ttu-id="647bb-309">次に、Contacts テーブルからすべてのデータを削除する必要があります (獲得した場合は、連絡先、およびグループのテーブル間のリレーションシップを作成することはできない)。</span><span class="sxs-lookup"><span data-stu-id="647bb-309">Next, we need to delete all of the data from the Contacts table (otherwise, we won t be able to create a relationship between the Contacts and Groups tables).</span></span> <span data-ttu-id="647bb-310">この場合は、以下の手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="647bb-310">Follow these steps:</span></span>

1. <span data-ttu-id="647bb-311">Contacts テーブルを右クリックし、メニュー オプションを選択**テーブル データの表示**です。</span><span class="sxs-lookup"><span data-stu-id="647bb-311">Right-click the Contacts table and select the menu option **Show Table Data**.</span></span>
2. <span data-ttu-id="647bb-312">すべての行を削除します。</span><span class="sxs-lookup"><span data-stu-id="647bb-312">Delete all of the rows.</span></span>

<span data-ttu-id="647bb-313">次に、グループのデータベース テーブルと既存の連絡先データベース テーブル間のリレーションシップを定義する必要があります。</span><span class="sxs-lookup"><span data-stu-id="647bb-313">Next, we need to define a relationship between the Groups database table and the existing Contacts database table.</span></span> <span data-ttu-id="647bb-314">この場合は、以下の手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="647bb-314">Follow these steps:</span></span>

1. <span data-ttu-id="647bb-315">テーブル デザイナーを開き、サーバー エクスプ ローラー ウィンドウで、Contacts テーブルをダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="647bb-315">Double-click the Contacts table in the Server Explorer window to open the Table Designer.</span></span>
2. <span data-ttu-id="647bb-316">GroupId をという名前の連絡先のテーブルに整数型の新しい列を追加します。</span><span class="sxs-lookup"><span data-stu-id="647bb-316">Add a new integer column to the Contacts table named GroupId.</span></span>
3. <span data-ttu-id="647bb-317">Foreign Key Relationships ダイアログを開いてリレーションシップ ボタンをクリックして (図 3 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="647bb-317">Click the Relationship button to open the Foreign Key Relationships dialog (see Figure 3).</span></span>
4. <span data-ttu-id="647bb-318">[追加] ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="647bb-318">Click the Add button.</span></span>
5. <span data-ttu-id="647bb-319">テーブルと列の指定 ボタンの横に表示される省略記号ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="647bb-319">Click the ellipsis button that appears next to the Table and Columns Specification button.</span></span>
6. <span data-ttu-id="647bb-320">テーブルと列 ダイアログ ボックスでは、主キー テーブルと、主キー列として Id としてグループを選択します。</span><span class="sxs-lookup"><span data-stu-id="647bb-320">In the Tables and Columns dialog, select Groups as the primary key table and Id as the primary key column.</span></span> <span data-ttu-id="647bb-321">外部キー テーブルと外部キー列として GroupId として連絡先を選択 (図 4 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="647bb-321">Select Contacts as the foreign key table and GroupId as the foreign key column (see Figure 4).</span></span> <span data-ttu-id="647bb-322">[Ok] ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="647bb-322">Click the OK button.</span></span>
7. <span data-ttu-id="647bb-323">**INSERT および UPDATE の指定**、値を選択して**Cascade**の**ルールの削除**です。</span><span class="sxs-lookup"><span data-stu-id="647bb-323">Under **INSERT and UPDATE Specification**, select the value **Cascade** for **Delete Rule**.</span></span>
8. <span data-ttu-id="647bb-324">Foreign Key Relationships ダイアログを閉じる閉じるボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="647bb-324">Click the Close button to close the Foreign Key Relationships dialog.</span></span>
9. <span data-ttu-id="647bb-325">変更を保存、Contacts テーブルに保存 ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="647bb-325">Click the Save button to save the changes to the Contacts table.</span></span>


<span data-ttu-id="647bb-326">[![データベース テーブルのリレーションシップを作成します。](iteration-6-use-test-driven-development-cs/_static/image3.jpg)](iteration-6-use-test-driven-development-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="647bb-326">[![Creating a database table relationship](iteration-6-use-test-driven-development-cs/_static/image3.jpg)](iteration-6-use-test-driven-development-cs/_static/image5.png)</span></span>

<span data-ttu-id="647bb-327">**図 03**: データベースのテーブル リレーションシップの作成 ([フルサイズのイメージを表示するをクリックして](iteration-6-use-test-driven-development-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="647bb-327">**Figure 03**: Creating a database table relationship ([Click to view full-size image](iteration-6-use-test-driven-development-cs/_static/image6.png))</span></span>


<span data-ttu-id="647bb-328">[![テーブル間のリレーションシップを指定します。](iteration-6-use-test-driven-development-cs/_static/image4.jpg)](iteration-6-use-test-driven-development-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="647bb-328">[![Specifying table relationships](iteration-6-use-test-driven-development-cs/_static/image4.jpg)](iteration-6-use-test-driven-development-cs/_static/image7.png)</span></span>

<span data-ttu-id="647bb-329">**図 04**: テーブルのリレーションシップを指定する ([フルサイズのイメージを表示するをクリックして](iteration-6-use-test-driven-development-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="647bb-329">**Figure 04**: Specifying table relationships([Click to view full-size image](iteration-6-use-test-driven-development-cs/_static/image8.png))</span></span>


### <a name="updating-our-data-model"></a><span data-ttu-id="647bb-330">このデータ モデルを更新します。</span><span class="sxs-lookup"><span data-stu-id="647bb-330">Updating our Data Model</span></span>

<span data-ttu-id="647bb-331">次に、新しいデータベース テーブルを表すため、データ モデルを更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="647bb-331">Next, we need to update our data model to represent the new database table.</span></span> <span data-ttu-id="647bb-332">この場合は、以下の手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="647bb-332">Follow these steps:</span></span>

1. <span data-ttu-id="647bb-333">モデル フォルダーを開くには、Entity Designer で ContactManagerModel.edmx ファイルをダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="647bb-333">Double-click the ContactManagerModel.edmx file in the Models folder to open the Entity Designer.</span></span>
2. <span data-ttu-id="647bb-334">デザイナー画面を右クリックし、メニュー オプションを選択**データベースからモデルを更新**です。</span><span class="sxs-lookup"><span data-stu-id="647bb-334">Right-click the Designer surface and select the menu option **Update Model from Database**.</span></span>
3. <span data-ttu-id="647bb-335">更新ウィザードでは、テーブルが表示され、完了 をクリックして、グループの選択は、(図 5 を参照してください) ボタンします。</span><span class="sxs-lookup"><span data-stu-id="647bb-335">In the Update Wizard, select the Groups table and click the Finish button (see Figure 5).</span></span>
4. <span data-ttu-id="647bb-336">グループ エンティティを右クリックし、メニュー オプションを選択**の名前を変更**です。</span><span class="sxs-lookup"><span data-stu-id="647bb-336">Right-click the Groups entity and select the menu option **Rename**.</span></span> <span data-ttu-id="647bb-337">名前を変更、*グループ*エンティティを*グループ*(単数形)。</span><span class="sxs-lookup"><span data-stu-id="647bb-337">Change the name of the *Groups* entity to *Group* (singular).</span></span>
5. <span data-ttu-id="647bb-338">Contact エンティティの下部に表示されるグループ ナビゲーション プロパティを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="647bb-338">Right-click the Groups navigation property that appears at the bottom of the Contact entity.</span></span> <span data-ttu-id="647bb-339">名前を変更、*グループ*へのナビゲーション プロパティ*グループ*(単数形)。</span><span class="sxs-lookup"><span data-stu-id="647bb-339">Change the name of the *Groups* navigation property to *Group* (singular).</span></span>


<span data-ttu-id="647bb-340">[![データベースから、Entity Framework モデルを更新します。](iteration-6-use-test-driven-development-cs/_static/image5.jpg)](iteration-6-use-test-driven-development-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="647bb-340">[![Updating an Entity Framework model from the database](iteration-6-use-test-driven-development-cs/_static/image5.jpg)](iteration-6-use-test-driven-development-cs/_static/image9.png)</span></span>

<span data-ttu-id="647bb-341">**図 05**: データベースから、Entity Framework モデルを更新する ([フルサイズのイメージを表示するをクリックして](iteration-6-use-test-driven-development-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="647bb-341">**Figure 05**: Updating an Entity Framework model from the database([Click to view full-size image](iteration-6-use-test-driven-development-cs/_static/image10.png))</span></span>


<span data-ttu-id="647bb-342">次の手順を完了すると、データ モデルは、連絡先とグループの両方のテーブルを表します。</span><span class="sxs-lookup"><span data-stu-id="647bb-342">After you complete these steps, your data model will represent both the Contacts and Groups tables.</span></span> <span data-ttu-id="647bb-343">エンティティ デザイナーは、両方のエンティティを表示する必要があります (図 6 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="647bb-343">The Entity Designer should show both entities (see Figure 6).</span></span>


<span data-ttu-id="647bb-344">[![エンティティ デザイナーのグループおよび連絡先を表示します。](iteration-6-use-test-driven-development-cs/_static/image6.jpg)](iteration-6-use-test-driven-development-cs/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="647bb-344">[![Entity Designer displaying Group and Contact](iteration-6-use-test-driven-development-cs/_static/image6.jpg)](iteration-6-use-test-driven-development-cs/_static/image11.png)</span></span>

<span data-ttu-id="647bb-345">**図 06**: グループおよび連絡先を表示するエンティティ デザイナー ([フルサイズのイメージを表示するをクリックして](iteration-6-use-test-driven-development-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="647bb-345">**Figure 06**: Entity Designer displaying Group and Contact([Click to view full-size image](iteration-6-use-test-driven-development-cs/_static/image12.png))</span></span>


### <a name="creating-our-repository-classes"></a><span data-ttu-id="647bb-346">このリポジトリ クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="647bb-346">Creating our Repository Classes</span></span>

<span data-ttu-id="647bb-347">次に、このリポジトリ クラスを実装する必要があります。</span><span class="sxs-lookup"><span data-stu-id="647bb-347">Next, we need to implement our repository class.</span></span> <span data-ttu-id="647bb-348">このイテレーションの過程で、いくつかの新しいメソッドに追加 IContactManagerRepository インターフェイス単体テストを満たすためにコードを書き込み中にします。</span><span class="sxs-lookup"><span data-stu-id="647bb-348">Over the course of this iteration, we added several new methods to the IContactManagerRepository interface while writing code to satisfy our unit tests.</span></span> <span data-ttu-id="647bb-349">IContactManagerRepository インターフェイスの最終バージョンは、14 の一覧に含まれます。</span><span class="sxs-lookup"><span data-stu-id="647bb-349">The final version of the IContactManagerRepository interface is contained in Listing 14.</span></span>

<span data-ttu-id="647bb-350">**14 - Models\IContactManagerRepository.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="647bb-350">**Listing 14 - Models\IContactManagerRepository.cs**</span></span>

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample14.cs)]

<span data-ttu-id="647bb-351">おいない t が実際に連絡先グループの操作に関連するメソッドのいずれかを実装します。</span><span class="sxs-lookup"><span data-stu-id="647bb-351">We haven t actually implemented any of the methods related to working with contact groups.</span></span> <span data-ttu-id="647bb-352">現時点では、EntityContactManagerRepository クラスでは、各 IContactManagerRepository インターフェイスで示されている連絡先グループ メソッドのスタブのメソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="647bb-352">Currently, the EntityContactManagerRepository class has stub methods for each of the contact group methods listed in the IContactManagerRepository interface.</span></span> <span data-ttu-id="647bb-353">たとえば、ListGroups() メソッドは、次のように現在なります。</span><span class="sxs-lookup"><span data-stu-id="647bb-353">For example, the ListGroups() method currently looks like this:</span></span>

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample15.cs)]

<span data-ttu-id="647bb-354">スタブ メソッドをアプリケーションをコンパイルし、単体テストに合格するようになりました。</span><span class="sxs-lookup"><span data-stu-id="647bb-354">The stub methods enabled us to compile our application and pass the unit tests.</span></span> <span data-ttu-id="647bb-355">ただし、ここには実際にはこれらのメソッドを実装するします。</span><span class="sxs-lookup"><span data-stu-id="647bb-355">However, now it is time to actually implement these methods.</span></span> <span data-ttu-id="647bb-356">EntityContactManagerRepository クラスの最終バージョンは、13 の一覧に含まれます。</span><span class="sxs-lookup"><span data-stu-id="647bb-356">The final version of the EntityContactManagerRepository class is contained in Listing 13.</span></span>

<span data-ttu-id="647bb-357">**13 - Models\EntityContactManagerRepository.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="647bb-357">**Listing 13 - Models\EntityContactManagerRepository.cs**</span></span>

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample16.cs)]

### <a name="creating-the-views"></a><span data-ttu-id="647bb-358">ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="647bb-358">Creating the Views</span></span>

<span data-ttu-id="647bb-359">ASP.NET MVC アプリケーションは、既定の ASP.NET ビュー エンジンを使用するとします。</span><span class="sxs-lookup"><span data-stu-id="647bb-359">ASP.NET MVC application when you use the default ASP.NET view engine.</span></span> <span data-ttu-id="647bb-360">そのためが表示されないは、特定の単体テストへの応答にビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="647bb-360">So, you don t create views in response to a particular unit test.</span></span> <span data-ttu-id="647bb-361">ただし、アプリケーションがあるためビュー役に立たない、ことが t の作成と、連絡先のマネージャー アプリケーションに含まれるビューを変更せずにこのイテレーションを完了します。</span><span class="sxs-lookup"><span data-stu-id="647bb-361">However, because an application would be useless without views, we can t complete this iteration without creating and modifying the views contained in the Contact Manager application.</span></span>

<span data-ttu-id="647bb-362">次の新しいビュー連絡先グループを管理するため (図 7 を参照してください) を作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="647bb-362">We need to create the following new views for managing contact groups (see Figure 7):</span></span>

- <span data-ttu-id="647bb-363">Views\Group\Index.aspx - 連絡先グループの一覧を表示</span><span class="sxs-lookup"><span data-stu-id="647bb-363">Views\Group\Index.aspx - Displays list of contact groups</span></span>
- <span data-ttu-id="647bb-364">Views\Group\Delete.aspx - 連絡先グループを削除するため確認フォームを表示</span><span class="sxs-lookup"><span data-stu-id="647bb-364">Views\Group\Delete.aspx - Displays confirmation form for deleting a contact group</span></span>


<span data-ttu-id="647bb-365">[![グループ インデックス ビュー](iteration-6-use-test-driven-development-cs/_static/image7.jpg)](iteration-6-use-test-driven-development-cs/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="647bb-365">[![The Group Index view](iteration-6-use-test-driven-development-cs/_static/image7.jpg)](iteration-6-use-test-driven-development-cs/_static/image13.png)</span></span>

<span data-ttu-id="647bb-366">**図 07**: グループのインデックス ビュー ([フルサイズのイメージを表示するをクリックして](iteration-6-use-test-driven-development-cs/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="647bb-366">**Figure 07**: The Group Index view([Click to view full-size image](iteration-6-use-test-driven-development-cs/_static/image14.png))</span></span>


<span data-ttu-id="647bb-367">連絡先グループが含まれるように、次の既存のビューを変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="647bb-367">We need to modify the following existing views so that they include contact groups:</span></span>

- <span data-ttu-id="647bb-368">Views\Home\Create.aspx</span><span class="sxs-lookup"><span data-stu-id="647bb-368">Views\Home\Create.aspx</span></span>
- <span data-ttu-id="647bb-369">Views\Home\Edit.aspx</span><span class="sxs-lookup"><span data-stu-id="647bb-369">Views\Home\Edit.aspx</span></span>
- <span data-ttu-id="647bb-370">Views\Home\Index.aspx</span><span class="sxs-lookup"><span data-stu-id="647bb-370">Views\Home\Index.aspx</span></span>

<span data-ttu-id="647bb-371">このチュートリアルに付属している Visual Studio アプリケーションを確認して、変更されたビューを表示できます。</span><span class="sxs-lookup"><span data-stu-id="647bb-371">You can see the modified views by looking at the Visual Studio application that accompanies this tutorial.</span></span> <span data-ttu-id="647bb-372">たとえば、図 8 は、連絡先インデックス ビューを示しています。</span><span class="sxs-lookup"><span data-stu-id="647bb-372">For example, Figure 8 illustrates the Contact Index view.</span></span>


<span data-ttu-id="647bb-373">[![連絡先のインデックス ビュー](iteration-6-use-test-driven-development-cs/_static/image8.jpg)](iteration-6-use-test-driven-development-cs/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="647bb-373">[![The Contact Index view](iteration-6-use-test-driven-development-cs/_static/image8.jpg)](iteration-6-use-test-driven-development-cs/_static/image15.png)</span></span>

<span data-ttu-id="647bb-374">**図 08**:、連絡先インデックス ビュー ([フルサイズのイメージを表示するをクリックして](iteration-6-use-test-driven-development-cs/_static/image16.png))</span><span class="sxs-lookup"><span data-stu-id="647bb-374">**Figure 08**: The Contact Index view([Click to view full-size image](iteration-6-use-test-driven-development-cs/_static/image16.png))</span></span>


## <a name="summary"></a><span data-ttu-id="647bb-375">まとめ</span><span class="sxs-lookup"><span data-stu-id="647bb-375">Summary</span></span>

<span data-ttu-id="647bb-376">このイテレーションが追加されました新機能、連絡先のマネージャー アプリケーションをテスト駆動開発アプリケーションの設計方法に従って。</span><span class="sxs-lookup"><span data-stu-id="647bb-376">In this iteration, we added new functionality to our Contact Manager application by following a test-driven development application design methodology.</span></span> <span data-ttu-id="647bb-377">ユーザー ストーリーのセットを作成して開始します。</span><span class="sxs-lookup"><span data-stu-id="647bb-377">We started by creating a set of user stories.</span></span> <span data-ttu-id="647bb-378">ユーザー ストーリーに示される要件に対応する一連の単体テストを作成しました。</span><span class="sxs-lookup"><span data-stu-id="647bb-378">We created a set of unit tests that corresponds to the requirements expressed by the user stories.</span></span> <span data-ttu-id="647bb-379">最後に、単体テストで示される要件を満たすのに十分なコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="647bb-379">Finally, we wrote just enough code to satisfy the requirements expressed by the unit tests.</span></span>

<span data-ttu-id="647bb-380">単体テストで示される要件を満たすために十分なコードを記述して完了すると、データベースとビューに更新されました。</span><span class="sxs-lookup"><span data-stu-id="647bb-380">After we finished writing enough code to satisfy the requirements expressed by the unit tests, we updated our database and views.</span></span> <span data-ttu-id="647bb-381">データベースに新しいグループ テーブルを追加し、Entity Framework データ モデルを更新します。</span><span class="sxs-lookup"><span data-stu-id="647bb-381">We added a new Groups table to our database and updated our Entity Framework Data Model.</span></span> <span data-ttu-id="647bb-382">おも作成し、一連のビューを変更します。</span><span class="sxs-lookup"><span data-stu-id="647bb-382">We also created and modified a set of views.</span></span>

<span data-ttu-id="647bb-383">次のイテレーション--最後の反復処理--では、Ajax を活用するようにアプリケーションを書き直します。</span><span class="sxs-lookup"><span data-stu-id="647bb-383">In the next iteration -- the final iteration -- we rewrite our application to take advantage of Ajax.</span></span> <span data-ttu-id="647bb-384">Ajax を利用して、応答性と連絡先のマネージャー アプリケーションのパフォーマンスを改善します。</span><span class="sxs-lookup"><span data-stu-id="647bb-384">By taking advantage of Ajax, we'll improve the responsiveness and performance of the Contact Manager application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="647bb-385">[前へ](iteration-5-create-unit-tests-cs.md)
> [次へ](iteration-7-add-ajax-functionality-cs.md)</span><span class="sxs-lookup"><span data-stu-id="647bb-385">[Previous](iteration-5-create-unit-tests-cs.md)
[Next](iteration-7-add-ajax-functionality-cs.md)</span></span>
