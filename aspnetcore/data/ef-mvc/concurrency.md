---
title: "ASP.NET Core MVC と EF Core - Concurrency - 第 8 回 (全 10 回)"
author: tdykstra
description: "このチュートリアルでは、複数のユーザーが同じエンティティを同時に更新するときの競合の処理方法について説明します。"
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/concurrency
ms.openlocfilehash: c271488d4da72ba340f3617ac20c7b6da2574c69
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/31/2018
---
# <a name="handling-concurrency-conflicts---ef-core-with-aspnet-core-mvc-tutorial-8-of-10"></a><span data-ttu-id="03c12-103">同時実行の競合処理 - EF Core と ASP.NET Core MVC チュートリアル、第 8 回 (全 10 回)</span><span class="sxs-lookup"><span data-stu-id="03c12-103">Handling concurrency conflicts - EF Core with ASP.NET Core MVC tutorial (8 of 10)</span></span>

<span data-ttu-id="03c12-104">作成者: [Tom Dykstra](https://github.com/tdykstra)、[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="03c12-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="03c12-105">Contoso University のサンプル Web アプリケーションでは、Entity Framework Core と Visual Studio を使用して ASP.NET Core MVC Web アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="03c12-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="03c12-106">チュートリアル シリーズについては、[シリーズの最初のチュートリアル](intro.md)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="03c12-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="03c12-107">先のチュートリアルでは、データを更新する方法について学習しました。</span><span class="sxs-lookup"><span data-stu-id="03c12-107">In earlier tutorials, you learned how to update data.</span></span> <span data-ttu-id="03c12-108">このチュートリアルでは、複数のユーザーが同じエンティティを同時に更新するときの競合の処理方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="03c12-108">This tutorial shows how to handle conflicts when multiple users update the same entity at the same time.</span></span>

<span data-ttu-id="03c12-109">Department エンティティを使用する Web ページを作成し、同時実行エラーを処理します。</span><span class="sxs-lookup"><span data-stu-id="03c12-109">You'll create web pages that work with the Department entity and handle concurrency errors.</span></span> <span data-ttu-id="03c12-110">次の図は Edit ページと Delete ページのものです。同時実行で競合が発生すると、メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="03c12-110">The following illustrations show the Edit and Delete pages, including some messages that are displayed if a concurrency conflict occurs.</span></span>

![Department Edit ページ](concurrency/_static/edit-error.png)

![Department Delete ページ](concurrency/_static/delete-error.png)

## <a name="concurrency-conflicts"></a><span data-ttu-id="03c12-113">同時実行の競合</span><span class="sxs-lookup"><span data-stu-id="03c12-113">Concurrency conflicts</span></span>

<span data-ttu-id="03c12-114">あるユーザーがあるエンティティのデータを編集目的で表示したとき、別のユーザーが同じエンティティのデータを最初のユーザーの変更がデータベースに書き込まれる前に更新すると、同時実行の競合が発生します。</span><span class="sxs-lookup"><span data-stu-id="03c12-114">A concurrency conflict occurs when one user displays an entity's data in order to edit it, and then another user updates the same entity's data before the first user's change is written to the database.</span></span> <span data-ttu-id="03c12-115">このような競合の検出を有効にしないと、最後にデータベースを更新したユーザーが他のユーザーの変更を上書きすることになります。</span><span class="sxs-lookup"><span data-stu-id="03c12-115">If you don't enable the detection of such conflicts, whoever updates the database last overwrites the other user's changes.</span></span> <span data-ttu-id="03c12-116">多くのアプリケーションでは、このリスクが許容されています。ユーザーや更新がわずかであれば、あるいは変更が一部上書きされても大きな問題なければ、同時実行のプログラミングにかかるコストが利点よりも重視されることがあります。</span><span class="sxs-lookup"><span data-stu-id="03c12-116">In many applications, this risk is acceptable: if there are few users, or few updates, or if isn't really critical if some changes are overwritten, the cost of programming for concurrency might outweigh the benefit.</span></span> <span data-ttu-id="03c12-117">その場合、同時実行の競合を処理するようにアプリケーションを構成する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="03c12-117">In that case, you don't have to configure the application to handle concurrency conflicts.</span></span>

### <a name="pessimistic-concurrency-locking"></a><span data-ttu-id="03c12-118">ペシミスティック同時実行制御 (ロック)</span><span class="sxs-lookup"><span data-stu-id="03c12-118">Pessimistic concurrency (locking)</span></span>

<span data-ttu-id="03c12-119">同時実行で偶発的にデータが失われる事態をアプリケーションで回避する必要があれば、その方法としてデータベース ロックがあります。</span><span class="sxs-lookup"><span data-stu-id="03c12-119">If your application does need to prevent accidental data loss in concurrency scenarios, one way to do that is to use database locks.</span></span> <span data-ttu-id="03c12-120">これはペシミスティック同時実行制御と呼ばれています。</span><span class="sxs-lookup"><span data-stu-id="03c12-120">This is called pessimistic concurrency.</span></span> <span data-ttu-id="03c12-121">たとえば、データベースから行を読む前に、読み取り専用か更新アクセスでロックを要求します。</span><span class="sxs-lookup"><span data-stu-id="03c12-121">For example, before you read a row from a database, you request a lock for read-only or for update access.</span></span> <span data-ttu-id="03c12-122">更新アクセスで行をロックすると、他のユーザーはその行を読み取り専用または更新アクセスでロックできなくなります。変更中のデータのコピーが与えられるためです。</span><span class="sxs-lookup"><span data-stu-id="03c12-122">If you lock a row for update access, no other users are allowed to lock the row either for read-only or update access, because they would get a copy of data that's in the process of being changed.</span></span> <span data-ttu-id="03c12-123">読み取り専用で行をロックすると、他のユーザーはその行を読み取り専用でロックできますが、更新アクセスではロックできません。</span><span class="sxs-lookup"><span data-stu-id="03c12-123">If you lock a row for read-only access, others can also lock it for read-only access but not for update.</span></span>

<span data-ttu-id="03c12-124">ロックの利用には短所があります。</span><span class="sxs-lookup"><span data-stu-id="03c12-124">Managing locks has disadvantages.</span></span> <span data-ttu-id="03c12-125">プログラムが複雑になります。</span><span class="sxs-lookup"><span data-stu-id="03c12-125">It can be complex to program.</span></span> <span data-ttu-id="03c12-126">相当なデータベース管理リソースが必要になります。アプリケーションの利用者数が増えると、パフォーマンス上の問題を引き起こすことがあります。</span><span class="sxs-lookup"><span data-stu-id="03c12-126">It requires significant database management resources, and it can cause performance problems as the number of users of an application increases.</span></span> <span data-ttu-id="03c12-127">そのような理由から、一部のデータベース管理システムはペシミスティック同時実行制御に対応していません。</span><span class="sxs-lookup"><span data-stu-id="03c12-127">For these reasons, not all database management systems support pessimistic concurrency.</span></span> <span data-ttu-id="03c12-128">Entity Framework Core にも組み込まれておらず、このチュートリアルでは実装方法を説明しません。</span><span class="sxs-lookup"><span data-stu-id="03c12-128">Entity Framework Core provides no built-in support for it, and this tutorial doesn't show you how to implement it.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="03c12-129">オプティミスティック同時実行制御</span><span class="sxs-lookup"><span data-stu-id="03c12-129">Optimistic Concurrency</span></span>

<span data-ttu-id="03c12-130">ペシミスティック同時実行制御の代わりとなるのがオプティミスティック同時実行制御です。</span><span class="sxs-lookup"><span data-stu-id="03c12-130">The alternative to pessimistic concurrency is optimistic concurrency.</span></span> <span data-ttu-id="03c12-131">オプティミスティック同時実行制御では、同時実行の競合の発生を許し、発生したら適切に対処します。</span><span class="sxs-lookup"><span data-stu-id="03c12-131">Optimistic concurrency means allowing concurrency conflicts to happen, and then reacting appropriately if they do.</span></span> <span data-ttu-id="03c12-132">たとえば、Jane が Department Edit ページにアクセスし、English 部署の予算を $350,000.00 から $0.00 に変更するとします。</span><span class="sxs-lookup"><span data-stu-id="03c12-132">For example, Jane visits the Department Edit page and changes the Budget amount for the English department from $350,000.00 to $0.00.</span></span>

![予算を 0 に変更する](concurrency/_static/change-budget.png)

<span data-ttu-id="03c12-134">Jane が **[保存]** をクリックする前に John が同じページにアクセスし、[開始日] フィールドを 9/1/2007 から 9/1/2013 に変更します。</span><span class="sxs-lookup"><span data-stu-id="03c12-134">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![開始日を 2013 に変更する](concurrency/_static/change-date.png)

<span data-ttu-id="03c12-136">Jane が **[保存]** を先にクリックすると、ブラウザーが Index ページに戻ったとき、Jane の変更が反映されています。</span><span class="sxs-lookup"><span data-stu-id="03c12-136">Jane clicks **Save** first and sees her change when the browser returns to the Index page.</span></span>

![予算が 0 に変更された](concurrency/_static/budget-zero.png)

<span data-ttu-id="03c12-138">次に John が Edit ページの **[保存]** をクリックします。このとき、予算は $350,000.00 と表示されています。</span><span class="sxs-lookup"><span data-stu-id="03c12-138">Then John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="03c12-139">この後の動作は、同時実行の競合の処理方法によって決定します。</span><span class="sxs-lookup"><span data-stu-id="03c12-139">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="03c12-140">次のようなオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="03c12-140">Some of the options include the following:</span></span>

* <span data-ttu-id="03c12-141">ユーザーが変更したプロパティを追跡記録し、それに該当する列だけをデータベースで更新できます。</span><span class="sxs-lookup"><span data-stu-id="03c12-141">You can keep track of which property a user has modified and update only the corresponding columns in the database.</span></span>

     <span data-ttu-id="03c12-142">例のシナリオでは、2 人のユーザーが異なるプロパティを更新したため、データは失われません。</span><span class="sxs-lookup"><span data-stu-id="03c12-142">In the example scenario, no data would be lost, because different properties were updated by the two users.</span></span> <span data-ttu-id="03c12-143">今度誰かが English 部署を閲覧すると、Jane の変更と John の変更が両方とも表示されます。開始日が 9/1/2013 で予算が 0 ドルです。</span><span class="sxs-lookup"><span data-stu-id="03c12-143">The next time someone browses the English department, they will see both Jane's and John's changes -- a start date of 9/1/2013 and a budget of zero dollars.</span></span> <span data-ttu-id="03c12-144">この更新方法では、データの損失につながる可能性がある競合の数を減らすことができますが、あるエンティティの同じプロパティに対して行われた変更が競合する場合、データの損失は避けられません。</span><span class="sxs-lookup"><span data-stu-id="03c12-144">This method of updating can reduce the number of conflicts that could result in data loss, but it can't avoid data loss if competing changes are made to the same property of an entity.</span></span> <span data-ttu-id="03c12-145">Entity Framework がこのように動作するかどうかは、更新コードの実装方法に依存します。</span><span class="sxs-lookup"><span data-stu-id="03c12-145">Whether the Entity Framework works this way depends on how you implement your update code.</span></span> <span data-ttu-id="03c12-146">これは Web アプリケーションの場合、実用的ではない場合が多いです。あるエンティティの新しい値に加え、元のプロパティ値もすべて追跡記録するため、大量のステータスを更新することになるからです。</span><span class="sxs-lookup"><span data-stu-id="03c12-146">It's often not practical in a web application, because it can require that you maintain large amounts of state in order to keep track of all original property values for an entity as well as new values.</span></span> <span data-ttu-id="03c12-147">大量のステータスを更新するとなると、サーバー リソースが必要になるか、Web ページ自体 (非表示フィールドなど) や Cookie に含める必要があるため、アプリケーションのパフォーマンスに影響が出ます。</span><span class="sxs-lookup"><span data-stu-id="03c12-147">Maintaining large amounts of state can affect application performance because it either requires server resources or must be included in the web page itself (for example, in hidden fields) or in a cookie.</span></span>

* <span data-ttu-id="03c12-148">John の変更で Jane の変更を上書きするように指定できます。</span><span class="sxs-lookup"><span data-stu-id="03c12-148">You can let John's change overwrite Jane's change.</span></span>

     <span data-ttu-id="03c12-149">今度誰かが English 部署を閲覧すると、日付は 9/1/2013 ですが、金額が $350,000.00 に戻っています。</span><span class="sxs-lookup"><span data-stu-id="03c12-149">The next time someone browses the English department, they will see 9/1/2013 and the restored $350,000.00 value.</span></span> <span data-ttu-id="03c12-150">これは *Client Wins* (クライアント側に合わせる) シナリオまたは *Last in Wins* (最終書き込み者優先) シナリオと呼ばれています。</span><span class="sxs-lookup"><span data-stu-id="03c12-150">This is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="03c12-151">(クライアントからの値がすべて、データ ストアの値より優先されます。)このセクションの冒頭でお伝えしたように、同時実行処理について何のコーディングもしない場合、これが自動的に行われます。</span><span class="sxs-lookup"><span data-stu-id="03c12-151">(All values from the client take precedence over what's in the data store.) As noted in the introduction to this section, if you don't do any coding for concurrency handling, this will happen automatically.</span></span>

* <span data-ttu-id="03c12-152">データベースで John の変更が更新されないようにできます。</span><span class="sxs-lookup"><span data-stu-id="03c12-152">You can prevent John's change from being updated in the database.</span></span>

     <span data-ttu-id="03c12-153">通常、エラー メッセージが表示され、John にデータの現在の状態が伝えられます。John は望むなら変更を再適用できます。</span><span class="sxs-lookup"><span data-stu-id="03c12-153">Typically, you would display an error message, show him the current state of the data, and allow him to reapply his changes if he still wants to make them.</span></span> <span data-ttu-id="03c12-154">これは *Store Wins* (ストア側に合わせる) シナリオと呼ばれています。</span><span class="sxs-lookup"><span data-stu-id="03c12-154">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="03c12-155">(クライアントが送信した値よりデータストアの値が優先されます。)このチュートリアルでは、Store Wins シナリオを実装します。</span><span class="sxs-lookup"><span data-stu-id="03c12-155">(The data-store values take precedence over the values submitted by the client.) You'll implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="03c12-156">この手法では、変更が上書きされるとき、それが必ずユーザーに警告されます。</span><span class="sxs-lookup"><span data-stu-id="03c12-156">This method ensures that no changes are overwritten without a user being alerted to what's happening.</span></span>

### <a name="detecting-concurrency-conflicts"></a><span data-ttu-id="03c12-157">同時実行の競合の検出</span><span class="sxs-lookup"><span data-stu-id="03c12-157">Detecting concurrency conflicts</span></span>

<span data-ttu-id="03c12-158">Entity Framework がスローする `DbConcurrencyException` 例外を処理することで競合を解決できます。</span><span class="sxs-lookup"><span data-stu-id="03c12-158">You can resolve conflicts by handling `DbConcurrencyException` exceptions that the Entity Framework throws.</span></span> <span data-ttu-id="03c12-159">このような例外がスローされるタイミングを認識する目的で、Entity Framework は競合を検出できなければなりません。</span><span class="sxs-lookup"><span data-stu-id="03c12-159">In order to know when to throw these exceptions, the Entity Framework must be able to detect conflicts.</span></span> <span data-ttu-id="03c12-160">そのため、データベースとデータ モデルを適宜構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="03c12-160">Therefore, you must configure the database and the data model appropriately.</span></span> <span data-ttu-id="03c12-161">競合検出を有効にするためのオプションには次のようなものがあります。</span><span class="sxs-lookup"><span data-stu-id="03c12-161">Some options for enabling conflict detection include the following:</span></span>

* <span data-ttu-id="03c12-162">行が変更されたタイミングを判断するトラッキング列をデータベース テーブルに追加します。</span><span class="sxs-lookup"><span data-stu-id="03c12-162">In the database table, include a tracking column that can be used to determine when a row has been changed.</span></span> <span data-ttu-id="03c12-163">その後、Entity Framework を構成し、SQL の Update または Delete コマンドの Where 句にその列を追加できます。</span><span class="sxs-lookup"><span data-stu-id="03c12-163">You can then configure the Entity Framework to include that column in the Where clause of SQL Update or Delete commands.</span></span>

     <span data-ttu-id="03c12-164">トラッキング列のデータ型は通常、`rowversion` です。</span><span class="sxs-lookup"><span data-stu-id="03c12-164">The data type of the tracking column is typically `rowversion`.</span></span> <span data-ttu-id="03c12-165">`rowversion` 値は連続番号であり、行が更新されるたびに増えます。</span><span class="sxs-lookup"><span data-stu-id="03c12-165">The `rowversion` value is a sequential number that's incremented each time the row is updated.</span></span> <span data-ttu-id="03c12-166">Update または Delete コマンドでは、Where 句にトラッキング列の元の値が含まれます (元の行バージョン)。</span><span class="sxs-lookup"><span data-stu-id="03c12-166">In an Update or Delete command, the Where clause includes the original value of the tracking column (the original row version) .</span></span> <span data-ttu-id="03c12-167">更新中の行が別のユーザーによって変更された場合、`rowversion` 列の値は元の値とは異なります。Where 句に起因し、Update または Delete ステートメントは更新する行を見つけられません。</span><span class="sxs-lookup"><span data-stu-id="03c12-167">If the row being updated has been changed by another user, the value in the `rowversion` column is different than the original value, so the Update or Delete statement can't find the row to update because of the Where clause.</span></span> <span data-ttu-id="03c12-168">Update または Delete コマンドによって行が更新されていないことを Entity Framework が確認すると (影響を受けた行の数が 0 のとき)、それを同時実行の競合として解釈します。</span><span class="sxs-lookup"><span data-stu-id="03c12-168">When the Entity Framework finds that no rows have been updated by the Update or Delete command (that is, when the number of affected rows is zero), it interprets that as a concurrency conflict.</span></span>

* <span data-ttu-id="03c12-169">Entity Framework を構成し、テーブルで Update コマンドと Delete コマンドの Where 句にすべての列の元の値を追加します。</span><span class="sxs-lookup"><span data-stu-id="03c12-169">Configure the Entity Framework to include the original values of every column in the table in the Where clause of Update and Delete commands.</span></span>

     <span data-ttu-id="03c12-170">最初のオプションと同様に、行が最初に読み取られてから行に変更があった場合、Where 句は更新する行を返さず、Entity Framework はそれを同時実行の競合として解釈します。</span><span class="sxs-lookup"><span data-stu-id="03c12-170">As in the first option, if anything in the row has changed since the row was first read, the Where clause won't return a row to update, which the Entity Framework interprets as a concurrency conflict.</span></span> <span data-ttu-id="03c12-171">データベース テーブルに列がたくさんある場合、この手法では結果的に大量の Where 句が出現し、大量のステータスを保守管理しなければならなくなります。</span><span class="sxs-lookup"><span data-stu-id="03c12-171">For database tables that have many columns, this approach can result in very large Where clauses, and can require that you maintain large amounts of state.</span></span> <span data-ttu-id="03c12-172">先に述べたように、大量のステータスを保守管理することになると、アプリケーションのパフォーマンスに影響が出ます。</span><span class="sxs-lookup"><span data-stu-id="03c12-172">As noted earlier, maintaining large amounts of state can affect application performance.</span></span> <span data-ttu-id="03c12-173">そのため、この手法は一般的には推奨されません。このチュートリアルでも利用しません。</span><span class="sxs-lookup"><span data-stu-id="03c12-173">Therefore this approach is generally not recommended, and it isn't the method used in this tutorial.</span></span>

     <span data-ttu-id="03c12-174">同時実行にこの手法を導入する場合、同時実行を追跡記録するエンティティのすべての主キーではないプロパティに `ConcurrencyCheck` 属性を追加し、印を付ける必要があります。</span><span class="sxs-lookup"><span data-stu-id="03c12-174">If you do want to implement this approach to concurrency, you have to mark all non-primary-key properties in the entity you want to track concurrency for by adding the `ConcurrencyCheck` attribute to them.</span></span> <span data-ttu-id="03c12-175">これで、Entity Framework では、Update ステートメントと Delete ステートメントの SQL Where 句にすべての列を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="03c12-175">That change enables the Entity Framework to include all columns in the SQL Where clause of Update and Delete statements.</span></span>

<span data-ttu-id="03c12-176">このチュートリアルの残りの部分では、Department エンティティに `rowversion` トラッキング プロパティを追加し、コントローラーとビューを作成し、すべてが適切に動作することをテストで確認します。</span><span class="sxs-lookup"><span data-stu-id="03c12-176">In the remainder of this tutorial you'll add a `rowversion` tracking property to the Department entity, create a controller and views, and test to verify that everything works correctly.</span></span>

## <a name="add-a-tracking-property-to-the-department-entity"></a><span data-ttu-id="03c12-177">Department エンティティにトラッキング プロパティを追加する</span><span class="sxs-lookup"><span data-stu-id="03c12-177">Add a tracking property to the Department entity</span></span>

<span data-ttu-id="03c12-178">*Models/Department.cs* で、RowVersion という名前のトラッキング プロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="03c12-178">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

<span data-ttu-id="03c12-179">`Timestamp` 属性によって、データベースに送信された Update コマンドと Delete コマンドの Where 句にこの列が追加されます。</span><span class="sxs-lookup"><span data-stu-id="03c12-179">The `Timestamp` attribute specifies that this column will be included in the Where clause of Update and Delete commands sent to the database.</span></span> <span data-ttu-id="03c12-180">前のバージョンの SQL Server では、SQL `rowversion` に取って代わられる前、SQL `timestamp` というデータ型が使用されていたため、この属性は `Timestamp` と呼ばれています。</span><span class="sxs-lookup"><span data-stu-id="03c12-180">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` replaced it.</span></span> <span data-ttu-id="03c12-181">`rowversion` の .NET 型はバイト配列です。</span><span class="sxs-lookup"><span data-stu-id="03c12-181">The .NET type for `rowversion` is a byte array.</span></span>

<span data-ttu-id="03c12-182">fluent API を利用する場合、次の例のように、`IsConcurrencyToken` メソッドを利用し (*Data/SchoolContext.cs* で)、トラッキング プロパティを指定できます。</span><span class="sxs-lookup"><span data-stu-id="03c12-182">If you prefer to use the fluent API, you can use the `IsConcurrencyToken` method (in *Data/SchoolContext.cs*) to specify the tracking property, as shown in the following example:</span></span>

```csharp
modelBuilder.Entity<Department>()
    .Property(p => p.RowVersion).IsConcurrencyToken();
```

<span data-ttu-id="03c12-183">プロパティを追加し、データベース モデルを変更したので、別の移行を行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="03c12-183">By adding a property you changed the database model, so you need to do another migration.</span></span>

<span data-ttu-id="03c12-184">変更内容を保存し、プロジェクトをビルドしてください。その後、コマンド ウィンドウに次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="03c12-184">Save your changes and build the project, and then enter the following commands in the command window:</span></span>

```console
dotnet ef migrations add RowVersion
```

```console
dotnet ef database update
```

## <a name="create-a-departments-controller-and-views"></a><span data-ttu-id="03c12-185">Departments のコントローラーとビューを作成する</span><span class="sxs-lookup"><span data-stu-id="03c12-185">Create a Departments controller and views</span></span>

<span data-ttu-id="03c12-186">先に Students、Courses、Instructors に行ったように、Departments のコントローラーとビューをスキャフォールディングします。</span><span class="sxs-lookup"><span data-stu-id="03c12-186">Scaffold a Departments controller and views as you did earlier for Students, Courses, and Instructors.</span></span>

![Department のスキャフォールディング](concurrency/_static/add-departments-controller.png)

<span data-ttu-id="03c12-188">*DepartmentsController.cs* ファイルに 4 回登場する "FirstMidName" をすべて "FullName" に変更します。それにより、部署の管理者のドロップダウン リストに講師の姓だけでなく、姓名が表示されます。</span><span class="sxs-lookup"><span data-stu-id="03c12-188">In the *DepartmentsController.cs* file, change all four occurrences of "FirstMidName" to "FullName" so that the department administrator drop-down lists will contain the full name of the instructor rather than just the last name.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_Dropdown)]

## <a name="update-the-departments-index-view"></a><span data-ttu-id="03c12-189">Departments/Index ビューを更新する</span><span class="sxs-lookup"><span data-stu-id="03c12-189">Update the Departments Index view</span></span>

<span data-ttu-id="03c12-190">スキャフォールディング エンジンによりインデックス ビューに RowVersion 列が作成されましたが、このフィールドは表示すべきではありません。</span><span class="sxs-lookup"><span data-stu-id="03c12-190">The scaffolding engine created a RowVersion column in the Index view, but that field shouldn't be displayed.</span></span>

<span data-ttu-id="03c12-191">*Views/Departments/Index.cshtml* のコードを次のコードに変更します。</span><span class="sxs-lookup"><span data-stu-id="03c12-191">Replace the code in *Views/Departments/Index.cshtml* with the following code.</span></span>

[!code-html[Main](intro/samples/cu/Views/Departments/Index.cshtml?highlight=4,7,44)]

<span data-ttu-id="03c12-192">これで見出しが "Departments" に変更され、RowVersion 列が削除され、管理者の名ではなく姓名が表示されます。</span><span class="sxs-lookup"><span data-stu-id="03c12-192">This changes the heading to "Departments", deletes the RowVersion column, and shows full name instead of first name for the administrator.</span></span>

## <a name="update-the-edit-methods-in-the-departments-controller"></a><span data-ttu-id="03c12-193">Departments コントローラーの Edit メソッドの更新</span><span class="sxs-lookup"><span data-stu-id="03c12-193">Update the Edit methods in the Departments controller</span></span>

<span data-ttu-id="03c12-194">HttpGet `Edit` メソッドと `Details` メソッドの両方に `AsNoTracking` を追加します。</span><span class="sxs-lookup"><span data-stu-id="03c12-194">In both the HttpGet `Edit` method and the `Details` method, add `AsNoTracking`.</span></span> <span data-ttu-id="03c12-195">HttpGet `Edit` メソッドに Administrator の一括読み込みを追加します。</span><span class="sxs-lookup"><span data-stu-id="03c12-195">In the HttpGet `Edit` method, add eager loading for the Administrator.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EagerLoading&highlight=2,3)]

<span data-ttu-id="03c12-196">HttpPost `Edit` メソッドの既存コードを次のコードに変更します。</span><span class="sxs-lookup"><span data-stu-id="03c12-196">Replace the existing code for the HttpPost `Edit` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EditPost)]

<span data-ttu-id="03c12-197">このコードはまず、更新する部署を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="03c12-197">The code begins by trying to read the department to be updated.</span></span> <span data-ttu-id="03c12-198">`SingleOrDefaultAsync` が null を返した場合、部署は別のユーザーが削除しています。</span><span class="sxs-lookup"><span data-stu-id="03c12-198">If the `SingleOrDefaultAsync` method returns null, the department was deleted by another user.</span></span> <span data-ttu-id="03c12-199">その場合、このコードは送信されたフォーム値を利用して部署エンティティを作成します。編集ページはエラー メッセージと共に再表示できます。</span><span class="sxs-lookup"><span data-stu-id="03c12-199">In that case the code uses the posted form values to create a department entity so that the Edit page can be redisplayed with an error message.</span></span> <span data-ttu-id="03c12-200">あるいは、部署フィールドを再表示せず、エラー メッセージのみを表示するのであれば、部署エンティティを再作成する必要はないでしょう。</span><span class="sxs-lookup"><span data-stu-id="03c12-200">As an alternative, you wouldn't have to re-create the department entity if you display only an error message without redisplaying the department fields.</span></span>

<span data-ttu-id="03c12-201">ビューの非表示フィールドに元の `RowVersion` 値が保管されます。このメソッドは `rowVersion` パラメーターでその値を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="03c12-201">The view stores the original `RowVersion` value in a hidden field, and this method receives that value in the `rowVersion` parameter.</span></span> <span data-ttu-id="03c12-202">`SaveChanges` を呼び出す前に、エンティティの `OriginalValues` コレクションにその元の `RowVersion` プロパティ値を置く必要があります。</span><span class="sxs-lookup"><span data-stu-id="03c12-202">Before you call `SaveChanges`, you have to put that original `RowVersion` property value in the `OriginalValues` collection for the entity.</span></span>

```csharp
_context.Entry(departmentToUpdate).Property("RowVersion").OriginalValue = rowVersion;
```

<span data-ttu-id="03c12-203">次に、Entity Framework で SQL UPDATE コマンドが作成されるとき、元の `RowVersion` 値が含まれる行を探す WHERE 句がそのコマンドに含まれます。</span><span class="sxs-lookup"><span data-stu-id="03c12-203">Then when the Entity Framework creates a SQL UPDATE command, that command will include a WHERE clause that looks for a row that has the original `RowVersion` value.</span></span> <span data-ttu-id="03c12-204">UPDATE コマンドの影響を受ける行がない場合 (元の `RowVersion` 値が含まれる行がない)、Entity Framework は `DbUpdateConcurrencyException` 例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="03c12-204">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value),  the Entity Framework throws a `DbUpdateConcurrencyException` exception.</span></span>

<span data-ttu-id="03c12-205">その例外の catch ブロックのコードによって、影響を受けた、例外オブジェクトの `Entries` プロパティから更新後の値が与えられた Department エンティティが取得されます。</span><span class="sxs-lookup"><span data-stu-id="03c12-205">The code in the catch block for that exception gets the affected Department entity that has the updated values from the `Entries` property on the exception object.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=164)]

<span data-ttu-id="03c12-206">`Entries` コレクションには `EntityEntry` オブジェクトが 1 つだけ与えられます。</span><span class="sxs-lookup"><span data-stu-id="03c12-206">The `Entries` collection will have just one `EntityEntry` object.</span></span>  <span data-ttu-id="03c12-207">そのオブジェクトを利用し、ユーザーが入力した新しい値と現在のデータベース値を取得できます。</span><span class="sxs-lookup"><span data-stu-id="03c12-207">You can use that object to get the new values entered by the user and the current database values.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=165-166)]

<span data-ttu-id="03c12-208">このコードにより、編集ページでユーザーが入力したものとデータベース値が異なる列ごとにカスタムのエラー メッセージが追加されます (簡潔にするため、ここでは 1 つだけフィールドを示しています)。</span><span class="sxs-lookup"><span data-stu-id="03c12-208">The code adds a custom error message for each column that has database values different from what the user entered on the Edit page (only one field is shown here for brevity).</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=174-178)]

<span data-ttu-id="03c12-209">最後になりますが、このコードで `departmentToUpdate` の `RowVersion` 値がデータベースから取得された新しい値に設定されます。</span><span class="sxs-lookup"><span data-stu-id="03c12-209">Finally, the code sets the `RowVersion` value of the `departmentToUpdate` to the new value retrieved from the database.</span></span> <span data-ttu-id="03c12-210">Edit ページが再表示されるとき、この新しい `RowVersion` 値が非表示フィールドに保存されます。今度ユーザーが **[保存]** をクリックすると、Edit ページの再表示後に発生した同時実行エラーのみが取得されます。</span><span class="sxs-lookup"><span data-stu-id="03c12-210">This new `RowVersion` value will be stored in the hidden field when the Edit page is redisplayed, and the next time the user clicks **Save**, only concurrency errors that happen since the redisplay of the Edit page will be caught.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=199-200)]

<span data-ttu-id="03c12-211">`ModelState` の `RowVersion` 値が古いため、`ModelState.Remove` ステートメントが必要になります。</span><span class="sxs-lookup"><span data-stu-id="03c12-211">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="03c12-212">このビューでは、フィールドの `ModelState` 値がモデル プロパティ値より優先されます。</span><span class="sxs-lookup"><span data-stu-id="03c12-212">In the view, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-the-department-edit-view"></a><span data-ttu-id="03c12-213">Department/Edit ビューの更新</span><span class="sxs-lookup"><span data-stu-id="03c12-213">Update the Department Edit view</span></span>

<span data-ttu-id="03c12-214">*Views/Departments/Edit.cshtml* で、次の変更を行います。</span><span class="sxs-lookup"><span data-stu-id="03c12-214">In *Views/Departments/Edit.cshtml*, make the following changes:</span></span>

* <span data-ttu-id="03c12-215">`DepartmentID` プロパティの非表示フィールドのすぐ後ろに `RowVersion` プロパティ値を保存する非表示フィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="03c12-215">Add a hidden field to save the `RowVersion` property value, immediately following the hidden field for the `DepartmentID` property.</span></span>

* <span data-ttu-id="03c12-216">ドロップダウン リストに "Select Administrator" オプションを追加します。</span><span class="sxs-lookup"><span data-stu-id="03c12-216">Add a "Select Administrator" option to the drop-down list.</span></span>

[!code-html[Main](intro/samples/cu/Views/Departments/Edit.cshtml?highlight=16,34-36)]

## <a name="test-concurrency-conflicts-in-the-edit-page"></a><span data-ttu-id="03c12-217">Edit ページ同時実行の競合をテストする</span><span class="sxs-lookup"><span data-stu-id="03c12-217">Test concurrency conflicts in the Edit page</span></span>

<span data-ttu-id="03c12-218">アプリを実行し、Departments Index ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="03c12-218">Run the app and go to the Departments Index page.</span></span> <span data-ttu-id="03c12-219">English 部署の **[編集]** ハイパーリンクを右クリックし、**[新しいタブで開く]** を選択し、English 部署の **[編集]** ハイパーリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="03c12-219">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**, then click the **Edit** hyperlink for the English department.</span></span> <span data-ttu-id="03c12-220">2 つのブラウザー タブに同じ情報が表示されます。</span><span class="sxs-lookup"><span data-stu-id="03c12-220">The two browser tabs now display the same information.</span></span>

<span data-ttu-id="03c12-221">最初のブラウザー タブでフィールドを変更し、**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="03c12-221">Change a field in the first browser tab and click **Save**.</span></span>

![変更後の Department Edit ページ 1](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="03c12-223">値が変更された Index ページがブラウザーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="03c12-223">The browser shows the Index page with the changed value.</span></span>

<span data-ttu-id="03c12-224">2 番目のブラウザー タブでフィールドを変更します。</span><span class="sxs-lookup"><span data-stu-id="03c12-224">Change a field in the second browser tab.</span></span>

![変更後の Department Edit ページ 2](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="03c12-226">**[保存]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="03c12-226">Click **Save**.</span></span> <span data-ttu-id="03c12-227">エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="03c12-227">You see an error message:</span></span>

![Department Edit ページのエラー メッセージ](concurrency/_static/edit-error.png)

<span data-ttu-id="03c12-229">**[保存]** をもう一度クリックします。</span><span class="sxs-lookup"><span data-stu-id="03c12-229">Click **Save** again.</span></span> <span data-ttu-id="03c12-230">2 番目のブラウザー タブで入力した値が保存されます。</span><span class="sxs-lookup"><span data-stu-id="03c12-230">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="03c12-231">Index ページが表示されると、保存した値を確認できます。</span><span class="sxs-lookup"><span data-stu-id="03c12-231">You see the saved values when the Index page appears.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="03c12-232">Delete ページを更新する</span><span class="sxs-lookup"><span data-stu-id="03c12-232">Update the Delete page</span></span>

<span data-ttu-id="03c12-233">Delete ページの場合、Entity Framework は、同様の方法で部署を編集している他のユーザーが起した同時実行の競合を検出します。</span><span class="sxs-lookup"><span data-stu-id="03c12-233">For the Delete page, the Entity Framework detects concurrency conflicts caused by someone else editing the department in a similar manner.</span></span> <span data-ttu-id="03c12-234">HttpGet `Delete` 値に確定ビューが表示されると、そのビューに非表示フィールドの元の `RowVersion` 値が含まれます。</span><span class="sxs-lookup"><span data-stu-id="03c12-234">When the HttpGet `Delete` method displays the confirmation view, the view includes the original `RowVersion` value in a hidden field.</span></span> <span data-ttu-id="03c12-235">その値は、ユーザーが削除を確定したときに呼び出される HttpPost `Delete` メソッドで利用されます。</span><span class="sxs-lookup"><span data-stu-id="03c12-235">That value is then available to the HttpPost `Delete` method that's called when the user confirms the deletion.</span></span> <span data-ttu-id="03c12-236">Entity Framework で SQL DELETE コマンドが作成されるとき、WHERE 句と元の `RowVersion` 値が含まれます。</span><span class="sxs-lookup"><span data-stu-id="03c12-236">When the Entity Framework creates the SQL DELETE command, it includes a WHERE clause with the original `RowVersion` value.</span></span> <span data-ttu-id="03c12-237">コマンドの結果、影響を受けた行が 0 であれば (削除確認ページの表示後に行が変更された)、同時実行例外がスローされます。確定ページをエラー メッセージと共に再表示するために、エラー フラグが true に設定された状態で HttpGet `Delete` メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="03c12-237">If the command results in zero rows affected (meaning the row was changed after the Delete confirmation page was displayed), a concurrency exception is thrown, and the HttpGet `Delete` method is called with an error flag set to true in order to redisplay the confirmation page with an error message.</span></span> <span data-ttu-id="03c12-238">別のユーザーが行を削除したため、影響を受けた行が 0 になることもあります。その場合、エラー メッセージは表示されません。</span><span class="sxs-lookup"><span data-stu-id="03c12-238">It's also possible that zero rows were affected because the row was deleted by another user, so in that case no error message is displayed.</span></span>

### <a name="update-the-delete-methods-in-the-departments-controller"></a><span data-ttu-id="03c12-239">Departments コントローラーの Delete メソッドの更新</span><span class="sxs-lookup"><span data-stu-id="03c12-239">Update the Delete methods in the Departments controller</span></span>

<span data-ttu-id="03c12-240">*DepartmentsController.cs* で、HttpGet `Delete` メソッドを次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="03c12-240">In *DepartmentsController.cs*, replace the HttpGet `Delete` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeleteGet&highlight=1,10,14-17,21-29)]

<span data-ttu-id="03c12-241">このメソッドは、同時実行エラー後にページが再表示されたかどうかを示すオプション パラメーターを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="03c12-241">The method accepts an optional parameter that indicates whether the page is being redisplayed after a concurrency error.</span></span> <span data-ttu-id="03c12-242">このフラグが true のとき、指定の部署が現存していなければ、別のユーザーによって削除されています。</span><span class="sxs-lookup"><span data-stu-id="03c12-242">If this flag is true and the department specified no longer exists, it was deleted by another user.</span></span> <span data-ttu-id="03c12-243">その場合、このコードは Index ページにリダイレクトされます。</span><span class="sxs-lookup"><span data-stu-id="03c12-243">In that case, the code redirects to the Index page.</span></span>  <span data-ttu-id="03c12-244">このフラグが true のとき、Department が存在すれば、別のユーザーが変更しています。</span><span class="sxs-lookup"><span data-stu-id="03c12-244">If this flag is true and the Department does exist, it was changed by another user.</span></span> <span data-ttu-id="03c12-245">その場合、このコードは `ViewData` を利用してビューにエラー メッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="03c12-245">In that case, the code sends an error message to the view using `ViewData`.</span></span>  

<span data-ttu-id="03c12-246">HttpPost `Delete` メソッドのコード (名前は `DeleteConfirmed`) を次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="03c12-246">Replace the code in the HttpPost `Delete` method (named `DeleteConfirmed`) with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeletePost&highlight=1,3,5-8,11-18)]

<span data-ttu-id="03c12-247">置き換えたスキャフォールディングされたコードで、このメソッドがレコード ID を 1 つだけ受け取りました。</span><span class="sxs-lookup"><span data-stu-id="03c12-247">In the scaffolded code that you just replaced, this method accepted only a record ID:</span></span>


```csharp
public async Task<IActionResult> DeleteConfirmed(int id)
```

<span data-ttu-id="03c12-248">このパラメーターをモデル バインダーによって作成された Department エンティティに変更しています。</span><span class="sxs-lookup"><span data-stu-id="03c12-248">You've changed this parameter to a Department entity instance created by the model binder.</span></span> <span data-ttu-id="03c12-249">それにより、レコード キーに加え、RowVersion プロパティ値に EF はアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="03c12-249">This gives EF access to the RowVersion property value in addition to the record key.</span></span>

```csharp
public async Task<IActionResult> Delete(Department department)
```

<span data-ttu-id="03c12-250">また、アクション メソッドの名前を `DeleteConfirmed` から `Delete` に変更しています。</span><span class="sxs-lookup"><span data-stu-id="03c12-250">You have also changed the action method name from `DeleteConfirmed` to `Delete`.</span></span> <span data-ttu-id="03c12-251">スキャフォールディングされたコードは `DeleteConfirmed` という名前を利用し、HttpPost メソッドに一意のシグネチャを与えました。</span><span class="sxs-lookup"><span data-stu-id="03c12-251">The scaffolded code used the name `DeleteConfirmed` to give the HttpPost method a unique signature.</span></span> <span data-ttu-id="03c12-252">(CLR では、さまざまなメソッド パラメーターを持つために、オーバーロードされたメソッドを必要とします。)シグネチャが一意になっているので、MVC 規則に準拠し、削除メソッドの HttpPost と HttpGet に同じ名前を利用できます。</span><span class="sxs-lookup"><span data-stu-id="03c12-252">(The CLR requires overloaded methods to have different method parameters.) Now that the signatures are unique, you can stick with the MVC convention and use the same name for the HttpPost and HttpGet delete methods.</span></span>

<span data-ttu-id="03c12-253">部署が既に削除されている場合、`AnyAsync` メソッドは false を返します。アプリケーションは Index メソッドに戻ります。</span><span class="sxs-lookup"><span data-stu-id="03c12-253">If the department is already deleted, the `AnyAsync` method returns false and the application just goes back to the Index method.</span></span>

<span data-ttu-id="03c12-254">同時実行エラーがキャッチされた場合、このコードは削除確認ページを再表示し、同時実行エラー メッセージを表示するかどうかを示すフラグを提供します。</span><span class="sxs-lookup"><span data-stu-id="03c12-254">If a concurrency error is caught, the code redisplays the Delete confirmation page and provides a flag that indicates it should display a concurrency error message.</span></span>

### <a name="update-the-delete-view"></a><span data-ttu-id="03c12-255">Delete ビューを更新する</span><span class="sxs-lookup"><span data-stu-id="03c12-255">Update the Delete view</span></span>

<span data-ttu-id="03c12-256">*Views/Departments/Delete.cshtml* で、スキャフォールディングされたコードを次のコードで置き換えます。このコードは、DepartmentID プロパティと RowVersion プロパティのエラー メッセージ フィールドと非表示フィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="03c12-256">In *Views/Departments/Delete.cshtml*, replace the scaffolded code with the following code that adds an error message field and hidden fields for the DepartmentID and RowVersion properties.</span></span> <span data-ttu-id="03c12-257">変更が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="03c12-257">The changes are highlighted.</span></span>

[!code-html[Main](intro/samples/cu/Views/Departments/Delete.cshtml?highlight=9,38,44,45,48)]

<span data-ttu-id="03c12-258">これにより、次の変更が行われます。</span><span class="sxs-lookup"><span data-stu-id="03c12-258">This makes the following changes:</span></span>

* <span data-ttu-id="03c12-259">見出しの `h2` と `h3` の間にエラー メッセージが追加されます。</span><span class="sxs-lookup"><span data-stu-id="03c12-259">Adds an error message between the `h2` and `h3` headings.</span></span>

* <span data-ttu-id="03c12-260">**[管理者]** フィールドで FirstMidName が FullName に変更されます。</span><span class="sxs-lookup"><span data-stu-id="03c12-260">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>

* <span data-ttu-id="03c12-261">RowVersion フィールドが削除されます。</span><span class="sxs-lookup"><span data-stu-id="03c12-261">Removes the RowVersion field.</span></span>

* <span data-ttu-id="03c12-262">`RowVersion` プロパティの非表示フィールドが追加されます。</span><span class="sxs-lookup"><span data-stu-id="03c12-262">Adds a hidden field for the `RowVersion` property.</span></span>

<span data-ttu-id="03c12-263">アプリを実行し、Departments Index ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="03c12-263">Run the app and go to the Departments Index page.</span></span> <span data-ttu-id="03c12-264">English 部署の **[削除]** ハイパーリンクを右クリックし、**[新しいタブで開く]** を選択します。次に最初のタブで English 部署の **[編集]** ハイパーリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="03c12-264">Right-click the **Delete** hyperlink for the English department and select **Open in new tab**, then in the first tab click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="03c12-265">最初のウィンドウで、いずれかの値を変更し、**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="03c12-265">In the first window, change one of the values, and click **Save**:</span></span>

![変更後/削除前の Department Edit ページ](concurrency/_static/edit-after-change-for-delete.png)

<span data-ttu-id="03c12-267">2 番目のタブで **[削除]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="03c12-267">In the second tab, click **Delete**.</span></span> <span data-ttu-id="03c12-268">同時実行エラー メッセージが表示されます。Department 値がデータベースの現在の内容で更新されています。</span><span class="sxs-lookup"><span data-stu-id="03c12-268">You see the concurrency error message, and the Department values are refreshed with what's currently in the database.</span></span>

![Department 削除確認ページと同時実行エラー](concurrency/_static/delete-error.png)

<span data-ttu-id="03c12-270">**[削除]** をもう一度クリックすると、Index ページにリダイレクトされます。Index ページには、部署が削除されていることが表示されます。</span><span class="sxs-lookup"><span data-stu-id="03c12-270">If you click **Delete** again, you're redirected to the Index page, which shows that the department has been deleted.</span></span>

## <a name="update-details-and-create-views"></a><span data-ttu-id="03c12-271">Details ビューと Create ビューの更新</span><span class="sxs-lookup"><span data-stu-id="03c12-271">Update Details and Create views</span></span>

<span data-ttu-id="03c12-272">Details ビューと Create ビューで、スキャフォールディングされたコードを任意でクリーンアップできます。</span><span class="sxs-lookup"><span data-stu-id="03c12-272">You can optionally clean up scaffolded code in the Details and Create views.</span></span>

<span data-ttu-id="03c12-273">RowVersion 列を削除し、管理者の姓名を表示するように *Views/Departments/Details.cshtml* のコードを置換します。</span><span class="sxs-lookup"><span data-stu-id="03c12-273">Replace the code in *Views/Departments/Details.cshtml* to delete the RowVersion column and show the full name of the Administrator.</span></span>

[!code-html[Main](intro/samples/cu/Views/Departments/Details.cshtml?highlight=35)]

<span data-ttu-id="03c12-274">ドロップダウン リストに Select オプションを追加するように *Views/Departments/Create.cshtml* のコードを置換します。</span><span class="sxs-lookup"><span data-stu-id="03c12-274">Replace the code in *Views/Departments/Create.cshtml* to add a Select option to the drop-down list.</span></span>

[!code-html[Main](intro/samples/cu/Views/Departments/Create.cshtml?highlight=32-34)]

## <a name="summary"></a><span data-ttu-id="03c12-275">まとめ</span><span class="sxs-lookup"><span data-stu-id="03c12-275">Summary</span></span>

<span data-ttu-id="03c12-276">同時実行の競合処理の入門編はこれで終わりです。</span><span class="sxs-lookup"><span data-stu-id="03c12-276">This completes the introduction to handling concurrency conflicts.</span></span> <span data-ttu-id="03c12-277">EF Core の同時実行処理の詳細については、[同時実行競合](https://docs.microsoft.com/ef/core/saving/concurrency)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="03c12-277">For more information about how to handle concurrency in EF Core, see [Concurrency conflicts](https://docs.microsoft.com/ef/core/saving/concurrency).</span></span> <span data-ttu-id="03c12-278">次のチュートリアルでは、Instructor エンティティと Student エンティティの Table-Per-Hierarchy 継承の実装方法について表示します。</span><span class="sxs-lookup"><span data-stu-id="03c12-278">The next tutorial shows how to implement table-per-hierarchy inheritance for the Instructor and Student entities.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="03c12-279">[前へ](update-related-data.md)
[次へ](inheritance.md)</span><span class="sxs-lookup"><span data-stu-id="03c12-279">[Previous](update-related-data.md)
[Next](inheritance.md)</span></span>  
