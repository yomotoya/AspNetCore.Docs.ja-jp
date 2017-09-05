---
title: "ASP.NET MVC を持つコアを EF Core での同時実行性 - 8 10"
author: tdykstra
description: "このチュートリアルでは、複数のユーザーが同時に同じエンティティを更新するときに競合を処理する方法を示します。"
keywords: "ASP.NET Core、Entity Framework Core では、同時実行"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 15e79e15-bda5-441d-80c7-8032a2628605
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/concurrency
ms.openlocfilehash: 4c402aee195d6614733be71c9c422e33553ad646
ms.sourcegitcommit: 4e84d8bf5f404bb77f3d41665cf7e7374fc39142
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/05/2017
---
# <a name="handling-concurrency-conflicts---ef-core-with-aspnet-core-mvc-tutorial-8-of-10"></a><span data-ttu-id="d264f-104">同時実行の競合の ASP.NET Core MVC のチュートリアル (10 の 8) に EF コアの処理</span><span class="sxs-lookup"><span data-stu-id="d264f-104">Handling concurrency conflicts - EF Core with ASP.NET Core MVC tutorial (8 of 10)</span></span>

<span data-ttu-id="d264f-105">によって[Tom Dykstra](https://github.com/tdykstra)と[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d264f-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d264f-106">Contoso 大学でサンプル web アプリケーションでは、Entity Framework のコアと Visual Studio を使用して ASP.NET Core MVC web アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="d264f-106">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="d264f-107">一連のチュートリアルについては、次を参照してください。[系列内の最初のチュートリアル](intro.md)です。</span><span class="sxs-lookup"><span data-stu-id="d264f-107">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="d264f-108">前のチュートリアルでは、データを更新する方法について学習しました。</span><span class="sxs-lookup"><span data-stu-id="d264f-108">In earlier tutorials, you learned how to update data.</span></span> <span data-ttu-id="d264f-109">このチュートリアルでは、複数のユーザーが同時に同じエンティティを更新するときに競合を処理する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="d264f-109">This tutorial shows how to handle conflicts when multiple users update the same entity at the same time.</span></span>

<span data-ttu-id="d264f-110">使用、Department エンティティと同時実行エラーを処理する web ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="d264f-110">You'll create web pages that work with the Department entity and handle concurrency errors.</span></span> <span data-ttu-id="d264f-111">次の図は、同時実行の競合が発生した場合に表示される一部のメッセージなどの編集および削除のページを表示します。</span><span class="sxs-lookup"><span data-stu-id="d264f-111">The following illustrations show the Edit and Delete pages, including some messages that are displayed if a concurrency conflict occurs.</span></span>

![部門の編集 ページ](concurrency/_static/edit-error.png)

![部門の削除 ページ](concurrency/_static/delete-error.png)

## <a name="concurrency-conflicts"></a><span data-ttu-id="d264f-114">同時実行の競合</span><span class="sxs-lookup"><span data-stu-id="d264f-114">Concurrency conflicts</span></span>

<span data-ttu-id="d264f-115">1 人のユーザーが、編集するために、エンティティのデータを表示し、別のユーザーが最初のユーザーの変更がデータベースに書き込まれる前に、同じエンティティのデータを更新し、同時実行の競合が発生します。</span><span class="sxs-lookup"><span data-stu-id="d264f-115">A concurrency conflict occurs when one user displays an entity's data in order to edit it, and then another user updates the same entity's data before the first user's change is written to the database.</span></span> <span data-ttu-id="d264f-116">このような競合の検出を有効にしない場合、データベースを更新したユーザーは、他のユーザーの変更を最後上書きされます。</span><span class="sxs-lookup"><span data-stu-id="d264f-116">If you don't enable the detection of such conflicts, whoever updates the database last overwrites the other user's changes.</span></span> <span data-ttu-id="d264f-117">多くのアプリケーションでこのようなリスクが許容可能な: 数名のユーザー、またはいくつかの更新プログラムがある場合、または場合いない本当に重要な場合は、いくつかの変更が上書きされると、同時実行のプログラミングのコストがのメリットを上回る可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d264f-117">In many applications, this risk is acceptable: if there are few users, or few updates, or if isn't really critical if some changes are overwritten, the cost of programming for concurrency might outweigh the benefit.</span></span> <span data-ttu-id="d264f-118">その場合は、同時実行の競合を処理するアプリケーションを構成する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="d264f-118">In that case, you don't have to configure the application to handle concurrency conflicts.</span></span>

### <a name="pessimistic-concurrency-locking"></a><span data-ttu-id="d264f-119">ペシミスティック同時実行制御 (ロック)</span><span class="sxs-lookup"><span data-stu-id="d264f-119">Pessimistic concurrency (locking)</span></span>

<span data-ttu-id="d264f-120">場合は、アプリケーションは同時実行シナリオの偶発的なデータ損失を防ぐため、これを行うにはデータベース ロックを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="d264f-120">If your application does need to prevent accidental data loss in concurrency scenarios, one way to do that is to use database locks.</span></span> <span data-ttu-id="d264f-121">これには、ペシミスティック同時実行制御は呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="d264f-121">This is called pessimistic concurrency.</span></span> <span data-ttu-id="d264f-122">たとえば、データベースから行を参照する前にするロックを要求の読み取り専用または更新アクセスします。</span><span class="sxs-lookup"><span data-stu-id="d264f-122">For example, before you read a row from a database, you request a lock for read-only or for update access.</span></span> <span data-ttu-id="d264f-123">場合更新アクセス権の行をロックすると、他のユーザーは許可されませんのいずれかの行をロックする読み取り専用かが変更されているプロセスでデータのコピーを取得するために、アクセスを更新します。</span><span class="sxs-lookup"><span data-stu-id="d264f-123">If you lock a row for update access, no other users are allowed to lock the row either for read-only or update access, because they would get a copy of data that's in the process of being changed.</span></span> <span data-ttu-id="d264f-124">場合は読み取り専用アクセスの行をロックすると、他のユーザーもロックできます読み取り専用アクセス用のではなく更新します。</span><span class="sxs-lookup"><span data-stu-id="d264f-124">If you lock a row for read-only access, others can also lock it for read-only access but not for update.</span></span>

<span data-ttu-id="d264f-125">ロックの管理、短所があります。</span><span class="sxs-lookup"><span data-stu-id="d264f-125">Managing locks has disadvantages.</span></span> <span data-ttu-id="d264f-126">プログラムに複雑なことができます。</span><span class="sxs-lookup"><span data-stu-id="d264f-126">It can be complex to program.</span></span> <span data-ttu-id="d264f-127">必要な管理リソースは大量のデータベース、およびアプリケーションのユーザーの数とパフォーマンスの問題を引き起こす可能性が増加します。</span><span class="sxs-lookup"><span data-stu-id="d264f-127">It requires significant database management resources, and it can cause performance problems as the number of users of an application increases.</span></span> <span data-ttu-id="d264f-128">これらの理由は、すべてのデータベース管理システムは、ペシミスティック同時実行をサポートします。</span><span class="sxs-lookup"><span data-stu-id="d264f-128">For these reasons, not all database management systems support pessimistic concurrency.</span></span> <span data-ttu-id="d264f-129">Entity Framework のコアには、組み込みのサポートはありません、このチュートリアルは示してそれを実装する方法。</span><span class="sxs-lookup"><span data-stu-id="d264f-129">Entity Framework Core provides no built-in support for it, and this tutorial doesn't show you how to implement it.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="d264f-130">オプティミスティック同時実行制御</span><span class="sxs-lookup"><span data-stu-id="d264f-130">Optimistic Concurrency</span></span>

<span data-ttu-id="d264f-131">ペシミスティック同時実行制御を使用しない場合は、オプティミスティック同時実行制御です。</span><span class="sxs-lookup"><span data-stu-id="d264f-131">The alternative to pessimistic concurrency is optimistic concurrency.</span></span> <span data-ttu-id="d264f-132">オプティミスティック同時実行制御は、許可する同時実行の競合が発生する場合は適切に対処を意味します。</span><span class="sxs-lookup"><span data-stu-id="d264f-132">Optimistic concurrency means allowing concurrency conflicts to happen, and then reacting appropriately if they do.</span></span> <span data-ttu-id="d264f-133">たとえば、加藤さんは、部門の編集 ページにアクセスし、英語版の部門に予算額をドル 350,000.00 から $0.00 に変更します。</span><span class="sxs-lookup"><span data-stu-id="d264f-133">For example, Jane visits the Department Edit page and changes the Budget amount for the English department from $350,000.00 to $0.00.</span></span>

![予算を 0 に変更します。](concurrency/_static/change-budget.png)

<span data-ttu-id="d264f-135">加藤さんがクリックする前に**保存**John は、同じページにアクセスし、2007 年 9 月 1 日から 2013 年 9 月 1 日に、Start Date フィールドを変更します。</span><span class="sxs-lookup"><span data-stu-id="d264f-135">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![2013 を開始日を変更します。](concurrency/_static/change-date.png)

<span data-ttu-id="d264f-137">加藤さんがクリックした**保存**最初られ、インデックス ページに戻ると、ブラウザーを変更するユーザーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="d264f-137">Jane clicks **Save** first and sees her change when the browser returns to the Index page.</span></span>

![予算を 0 に変更されました](concurrency/_static/budget-zero.png)

<span data-ttu-id="d264f-139">John がその**保存**ドル 350,000.00 の予算が表示される編集ページにします。</span><span class="sxs-lookup"><span data-stu-id="d264f-139">Then John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="d264f-140">次の動作は、同時実行の競合を処理する方法によって決まります。</span><span class="sxs-lookup"><span data-stu-id="d264f-140">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="d264f-141">オプションの一部を以下に示します。</span><span class="sxs-lookup"><span data-stu-id="d264f-141">Some of the options include the following:</span></span>

* <span data-ttu-id="d264f-142">プロパティをユーザーが変更を追跡し、データベースに対応する列のみを更新できます。</span><span class="sxs-lookup"><span data-stu-id="d264f-142">You can keep track of which property a user has modified and update only the corresponding columns in the database.</span></span>

     <span data-ttu-id="d264f-143">例のシナリオで失われるデータなし、2 人のユーザーによって更新されたプロパティが異なるためです。</span><span class="sxs-lookup"><span data-stu-id="d264f-143">In the example scenario, no data would be lost, because different properties were updated by the two users.</span></span> <span data-ttu-id="d264f-144">次回の英語版の部門を参照するユーザーがジェーンのと John の両方の変更 - 2013 年 9 月 1 日の開始日と 0 個のドルのコストの予算が表示されます。</span><span class="sxs-lookup"><span data-stu-id="d264f-144">The next time someone browses the English department, they'll see both Jane's and John's changes -- a start date of 9/1/2013 and a budget of zero dollars.</span></span> <span data-ttu-id="d264f-145">更新には、このメソッドは、データが失われる可能性のある競合の数を減らすことができますが、エンティティの同じプロパティに競合する変更が加えられた場合は、データ損失を防ぐことことはできません。</span><span class="sxs-lookup"><span data-stu-id="d264f-145">This method of updating can reduce the number of conflicts that could result in data loss, but it can't avoid data loss if competing changes are made to the same property of an entity.</span></span> <span data-ttu-id="d264f-146">この方法を Entity Framework で動作するかどうかは、更新プログラムのコードを実装する方法によって異なります。</span><span class="sxs-lookup"><span data-stu-id="d264f-146">Whether the Entity Framework works this way depends on how you implement your update code.</span></span> <span data-ttu-id="d264f-147">必要のエンティティの元のプロパティ値をすべてと新しい値を追跡するためには、大量の状態を維持することができるため、web アプリケーションで現実的では、ほとんどの場合。</span><span class="sxs-lookup"><span data-stu-id="d264f-147">It's often not practical in a web application, because it can require that you maintain large amounts of state in order to keep track of all original property values for an entity as well as new values.</span></span> <span data-ttu-id="d264f-148">サーバーのリソースが必要ですか、web ページ自体 (たとえば、非表示フィールド) 内に含める必要があるために、大量の状態を保持することと、アプリケーションのパフォーマンスが影響することができますか、クッキーにします。</span><span class="sxs-lookup"><span data-stu-id="d264f-148">Maintaining large amounts of state can affect application performance because it either requires server resources or must be included in the web page itself (for example, in hidden fields) or in a cookie.</span></span>

* <span data-ttu-id="d264f-149">John の変更がジェーンの変更を上書きすることができます。</span><span class="sxs-lookup"><span data-stu-id="d264f-149">You can let John's change overwrite Jane's change.</span></span>

     <span data-ttu-id="d264f-150">次に英語版の部門を参照するユーザーが、2013 年 9 月 1 日が表示されますと復元のドル 350,000.00 値。</span><span class="sxs-lookup"><span data-stu-id="d264f-150">The next time someone browses the English department, they'll see 9/1/2013 and the restored $350,000.00 value.</span></span> <span data-ttu-id="d264f-151">これと呼ばれる、*クライアントが Wins*または*Wins の最後に*シナリオです。</span><span class="sxs-lookup"><span data-stu-id="d264f-151">This is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="d264f-152">(クライアントからのすべての値よりも優先、データ ストアには、新機能です。)同時実行処理のコーディングそうしない場合は、このセクションの概要で説明したよう、これは自動的に発生します。</span><span class="sxs-lookup"><span data-stu-id="d264f-152">(All values from the client take precedence over what's in the data store.) As noted in the introduction to this section, if you don't do any coding for concurrency handling, this will happen automatically.</span></span>

* <span data-ttu-id="d264f-153">John の変更は、データベースで更新されているを妨害することができます。</span><span class="sxs-lookup"><span data-stu-id="d264f-153">You can prevent John's change from being updated in the database.</span></span>

     <span data-ttu-id="d264f-154">通常は、エラー メッセージが表示、データの現在の状態を表示すること、および彼ようにする必要がある場合は、自分の変更を適用することを許可するがします。</span><span class="sxs-lookup"><span data-stu-id="d264f-154">Typically, you would display an error message, show him the current state of the data, and allow him to reapply his changes if he still wants to make them.</span></span> <span data-ttu-id="d264f-155">これと呼ばれる、*ストア Wins*シナリオです。</span><span class="sxs-lookup"><span data-stu-id="d264f-155">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="d264f-156">(データ ストアの値よりも優先、クライアントから送信された値です。)このチュートリアルでは、ストアの Wins のシナリオを実装します。</span><span class="sxs-lookup"><span data-stu-id="d264f-156">(The data-store values take precedence over the values submitted by the client.) You'll implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="d264f-157">このメソッドは、何が起こっているかに警告を表示されているユーザーがいない状態の変更が上書きされないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="d264f-157">This method ensures that no changes are overwritten without a user being alerted to what's happening.</span></span>

### <a name="detecting-concurrency-conflicts"></a><span data-ttu-id="d264f-158">同時実行の競合を検出します。</span><span class="sxs-lookup"><span data-stu-id="d264f-158">Detecting concurrency conflicts</span></span>

<span data-ttu-id="d264f-159">競合を解決するには処理することにより`DbConcurrencyException`Entity Framework がスローする例外。</span><span class="sxs-lookup"><span data-stu-id="d264f-159">You can resolve conflicts by handling `DbConcurrencyException` exceptions that the Entity Framework throws.</span></span> <span data-ttu-id="d264f-160">これらの例外をスローするタイミングを知るために、Entity Framework は、競合を検出できる必要があります。</span><span class="sxs-lookup"><span data-stu-id="d264f-160">In order to know when to throw these exceptions, the Entity Framework must be able to detect conflicts.</span></span> <span data-ttu-id="d264f-161">そのため、データベースとデータ モデルを適切に構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d264f-161">Therefore, you must configure the database and the data model appropriately.</span></span> <span data-ttu-id="d264f-162">競合の検出を有効にするためのいくつかのオプションを以下に示します。</span><span class="sxs-lookup"><span data-stu-id="d264f-162">Some options for enabling conflict detection include the following:</span></span>

* <span data-ttu-id="d264f-163">データベース テーブルを特定の行が変更されたときに使用できる追跡列を含めます。</span><span class="sxs-lookup"><span data-stu-id="d264f-163">In the database table, include a tracking column that can be used to determine when a row has been changed.</span></span> <span data-ttu-id="d264f-164">Where 句でその列を含めるように、Entity Framework を構成することができますし、SQL の Update または Delete コマンドの句。</span><span class="sxs-lookup"><span data-stu-id="d264f-164">You can then configure the Entity Framework to include that column in the Where clause of SQL Update or Delete commands.</span></span>

     <span data-ttu-id="d264f-165">追跡列のデータ型は、通常`rowversion`です。</span><span class="sxs-lookup"><span data-stu-id="d264f-165">The data type of the tracking column is typically `rowversion`.</span></span> <span data-ttu-id="d264f-166">`rowversion`値は、行が更新されるたびにインクリメントが連続番号です。</span><span class="sxs-lookup"><span data-stu-id="d264f-166">The `rowversion` value is a sequential number that's incremented each time the row is updated.</span></span> <span data-ttu-id="d264f-167">Update または Delete コマンドで、Where 句には、追跡列 (元の行バージョン) の元の値が含まれています。</span><span class="sxs-lookup"><span data-stu-id="d264f-167">In an Update or Delete command, the Where clause includes the original value of the tracking column (the original row version) .</span></span> <span data-ttu-id="d264f-168">更新された行が他のユーザーの値が変更されている場合、 `rowversion` Update または Delete ステートメントは Where により更新する行を見つけることはできませんので、列は、元の値とは異なる句。</span><span class="sxs-lookup"><span data-stu-id="d264f-168">If the row being updated has been changed by another user, the value in the `rowversion` column is different than the original value, so the Update or Delete statement can't find the row to update because of the Where clause.</span></span> <span data-ttu-id="d264f-169">行が更新または削除によって更新されていない Entity Framework 検索コマンドの (つまり、影響を受けた行の数が 0 である場合)、時として同時実行の競合を解釈します。</span><span class="sxs-lookup"><span data-stu-id="d264f-169">When the Entity Framework finds that no rows have been updated by the Update or Delete command (that is, when the number of affected rows is zero), it interprets that as a concurrency conflict.</span></span>

* <span data-ttu-id="d264f-170">Where 句でテーブルのすべての列の元の値に Entity Framework を構成する Update および Delete コマンドの句。</span><span class="sxs-lookup"><span data-stu-id="d264f-170">Configure the Entity Framework to include the original values of every column in the table in the Where clause of Update and Delete commands.</span></span>

     <span data-ttu-id="d264f-171">最初のオプションの場合は、行の任意のものが変更されたため、行が読み取られた最初のように Where 句が返されません、行を更新するには、Entity Framework は同時実行の競合として解釈します。</span><span class="sxs-lookup"><span data-stu-id="d264f-171">As in the first option, if anything in the row has changed since the row was first read, the Where clause won't return a row to update, which the Entity Framework interprets as a concurrency conflict.</span></span> <span data-ttu-id="d264f-172">多くの列を持つデータベース テーブルでは、このアプローチされる可能性が非常に大規模な Where 句、大量の状態を維持することが必要とします。</span><span class="sxs-lookup"><span data-stu-id="d264f-172">For database tables that have many columns, this approach can result in very large Where clauses, and can require that you maintain large amounts of state.</span></span> <span data-ttu-id="d264f-173">前に述べたようには、アプリケーションのパフォーマンスに影響大量の状態を維持することができます。</span><span class="sxs-lookup"><span data-stu-id="d264f-173">As noted earlier, maintaining large amounts of state can affect application performance.</span></span> <span data-ttu-id="d264f-174">そのためこの方法は、通常、推奨されませんし、このチュートリアルで使用する方法はありません。</span><span class="sxs-lookup"><span data-stu-id="d264f-174">Therefore this approach is generally not recommended, and it isn't the method used in this tutorial.</span></span>

     <span data-ttu-id="d264f-175">追加することでの同時実行制御を追跡するエンティティのすべての非主キー プロパティをマークする必要がある同時実行するには、このアプローチを実装する場合、`ConcurrencyCheck`属性をします。</span><span class="sxs-lookup"><span data-stu-id="d264f-175">If you do want to implement this approach to concurrency, you have to mark all non-primary-key properties in the entity you want to track concurrency for by adding the `ConcurrencyCheck` attribute to them.</span></span> <span data-ttu-id="d264f-176">その変更には、Update および Delete ステートメントの SQL の Where 句にすべての列を含める Entity Framework が有効になります。</span><span class="sxs-lookup"><span data-stu-id="d264f-176">That change enables the Entity Framework to include all columns in the SQL Where clause of Update and Delete statements.</span></span>

<span data-ttu-id="d264f-177">このチュートリアルの残りの部分では追加、 `rowversion` Department エンティティを追跡するプロパティ、コント ローラーとビューを作成し、すべてが正常に動作することを確認します。</span><span class="sxs-lookup"><span data-stu-id="d264f-177">In the remainder of this tutorial you'll add a `rowversion` tracking property to the Department entity, create a controller and views, and test to verify that everything works correctly.</span></span>

## <a name="add-a-tracking-property-to-the-department-entity"></a><span data-ttu-id="d264f-178">追跡プロパティ、Department エンティティを追加します。</span><span class="sxs-lookup"><span data-stu-id="d264f-178">Add a tracking property to the Department entity</span></span>

<span data-ttu-id="d264f-179">*Models/Department.cs*RowVersion をという名前の追跡のプロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="d264f-179">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

<span data-ttu-id="d264f-180">[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]</span><span class="sxs-lookup"><span data-stu-id="d264f-180">[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]</span></span>

<span data-ttu-id="d264f-181">`Timestamp`属性は、この列が含まれることを指定します、Where 句で Update および Delete コマンドの句は、データベースに送信します。</span><span class="sxs-lookup"><span data-stu-id="d264f-181">The `Timestamp` attribute specifies that this column will be included in the Where clause of Update and Delete commands sent to the database.</span></span> <span data-ttu-id="d264f-182">属性といいます`Timestamp`旧バージョンの SQL Server、SQL を使用するため`timestamp`データ型に、SQL`rowversion`に置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="d264f-182">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` replaced it.</span></span> <span data-ttu-id="d264f-183">.NET 型を`rowversion`バイト配列。</span><span class="sxs-lookup"><span data-stu-id="d264f-183">The .NET type for `rowversion` is a byte array.</span></span>

<span data-ttu-id="d264f-184">Fluent API を使用する場合は、使用、`IsConcurrencyToken`メソッド (で*Data/SchoolContext.cs*) プロパティを指定する、追跡、次の例で示すようにします。</span><span class="sxs-lookup"><span data-stu-id="d264f-184">If you prefer to use the fluent API, you can use the `IsConcurrencyToken` method (in *Data/SchoolContext.cs*) to specify the tracking property, as shown in the following example:</span></span>

```csharp
modelBuilder.Entity<Department>()
    .Property(p => p.RowVersion).IsConcurrencyToken();
```

<span data-ttu-id="d264f-185">プロパティを追加することで変更した場合、データベース モデル別の移行を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d264f-185">By adding a property you changed the database model, so you need to do another migration.</span></span>

<span data-ttu-id="d264f-186">変更を保存し、プロジェクトをビルドし、コマンド ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="d264f-186">Save your changes and build the project, and then enter the following commands in the command window:</span></span>

```console
dotnet ef migrations add RowVersion
```

```console
dotnet ef database update
```

## <a name="create-a-departments-controller-and-views"></a><span data-ttu-id="d264f-187">部門のコント ローラーとビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="d264f-187">Create a Departments controller and views</span></span>

<span data-ttu-id="d264f-188">学生、コース、および講習においてインストラクター以前実行した方法では、部門のコント ローラーとビューをスキャフォールディングです。</span><span class="sxs-lookup"><span data-stu-id="d264f-188">Scaffold a Departments controller and views as you did earlier for Students, Courses, and Instructors.</span></span>

![Scaffold 部門](concurrency/_static/add-departments-controller.png)

<span data-ttu-id="d264f-190">*DepartmentsController.cs*ファイルは、部門の管理者のドロップダウン リストは、インストラクターの完全な名前ではなく最後の名前だけが含まれるように、"FullName""FirstMidName"の 4 つすべての項目を変更します。</span><span class="sxs-lookup"><span data-stu-id="d264f-190">In the *DepartmentsController.cs* file, change all four occurrences of "FirstMidName" to "FullName" so that the department administrator drop-down lists will contain the full name of the instructor rather than just the last name.</span></span>

<span data-ttu-id="d264f-191">[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_Dropdown)]</span><span class="sxs-lookup"><span data-stu-id="d264f-191">[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_Dropdown)]</span></span>

## <a name="update-the-departments-index-view"></a><span data-ttu-id="d264f-192">部門インデックス ビューを更新します。</span><span class="sxs-lookup"><span data-stu-id="d264f-192">Update the Departments Index view</span></span>

<span data-ttu-id="d264f-193">スキャフォールディング エンジンでは、インデックス ビューで RowVersion 列が作成された日時が、そのフィールドを表示することはできません。</span><span class="sxs-lookup"><span data-stu-id="d264f-193">The scaffolding engine created a RowVersion column in the Index view, but that field shouldn't be displayed.</span></span>

<span data-ttu-id="d264f-194">コードに置き換えます*Views/Departments/Index.cshtml*を次のコード。</span><span class="sxs-lookup"><span data-stu-id="d264f-194">Replace the code in *Views/Departments/Index.cshtml* with the following code.</span></span>

<span data-ttu-id="d264f-195">[!code-html[Main](intro/samples/cu/Views/Departments/Index.cshtml?highlight=4,7,44)]</span><span class="sxs-lookup"><span data-stu-id="d264f-195">[!code-html[Main](intro/samples/cu/Views/Departments/Index.cshtml?highlight=4,7,44)]</span></span>

<span data-ttu-id="d264f-196">RowVersion 列を削除、および管理者の名の代わりに完全名を示しますこの「部門」に見出しを変更します。</span><span class="sxs-lookup"><span data-stu-id="d264f-196">This changes the heading to "Departments", deletes the RowVersion column, and shows full name instead of first name for the administrator.</span></span>

## <a name="update-the-edit-methods-in-the-departments-controller"></a><span data-ttu-id="d264f-197">更新プログラムの部門コント ローラーの編集方法</span><span class="sxs-lookup"><span data-stu-id="d264f-197">Update the Edit methods in the Departments controller</span></span>

<span data-ttu-id="d264f-198">両方の HttpGet で`Edit`メソッドおよび`Details`メソッド、追加`AsNoTracking`です。</span><span class="sxs-lookup"><span data-stu-id="d264f-198">In both the HttpGet `Edit` method and the `Details` method, add `AsNoTracking`.</span></span> <span data-ttu-id="d264f-199">HttpGet`Edit`メソッド、一括読み込みの管理者を追加します。</span><span class="sxs-lookup"><span data-stu-id="d264f-199">In the HttpGet `Edit` method, add eager loading for the Administrator.</span></span>

<span data-ttu-id="d264f-200">[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EagerLoading&highlight=2,3)]</span><span class="sxs-lookup"><span data-stu-id="d264f-200">[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EagerLoading&highlight=2,3)]</span></span>

<span data-ttu-id="d264f-201">HttpPost の既存のコードを置き換えます`Edit`メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="d264f-201">Replace the existing code for the HttpPost `Edit` method with the following code:</span></span>

<span data-ttu-id="d264f-202">[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EditPost)]</span><span class="sxs-lookup"><span data-stu-id="d264f-202">[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EditPost)]</span></span>

<span data-ttu-id="d264f-203">コードを更新する部門を読み取ろうとして開始します。</span><span class="sxs-lookup"><span data-stu-id="d264f-203">The code begins by trying to read the department to be updated.</span></span> <span data-ttu-id="d264f-204">場合、 `SingleOrDefaultAsync` null が返されます、部門別のユーザーが削除されました。</span><span class="sxs-lookup"><span data-stu-id="d264f-204">If the `SingleOrDefaultAsync` method returns null, the department was deleted by another user.</span></span> <span data-ttu-id="d264f-205">その場合は、コードは、エラー メッセージでの編集 ページを再表示できるように、department エンティティを作成する、ポストされたフォーム値を使用します。</span><span class="sxs-lookup"><span data-stu-id="d264f-205">In that case the code uses the posted form values to create a department entity so that the Edit page can be redisplayed with an error message.</span></span> <span data-ttu-id="d264f-206">代わりは、部門のフィールドの再表示せず、エラー メッセージのみを表示する場合に、department エンティティを再作成する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="d264f-206">As an alternative, you wouldn't have to re-create the department entity if you display only an error message without redisplaying the department fields.</span></span>

<span data-ttu-id="d264f-207">ビュー、元の格納`RowVersion`の値の非表示フィールドの場合は、このメソッド内でその値を受信する、`rowVersion`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="d264f-207">The view stores the original `RowVersion` value in a hidden field, and this method receives that value in the `rowVersion` parameter.</span></span> <span data-ttu-id="d264f-208">呼び出す前に`SaveChanges`、元の配置を`RowVersion`プロパティ値を`OriginalValues`エンティティのコレクション。</span><span class="sxs-lookup"><span data-stu-id="d264f-208">Before you call `SaveChanges`, you have to put that original `RowVersion` property value in the `OriginalValues` collection for the entity.</span></span>

```csharp
_context.Entry(departmentToUpdate).Property("RowVersion").OriginalValue = rowVersion;
```

<span data-ttu-id="d264f-209">そのコマンドが元のある行を検索する WHERE 句は、Entity Framework では、SQL の UPDATE コマンドを作成するとき、`RowVersion`値。</span><span class="sxs-lookup"><span data-stu-id="d264f-209">Then when the Entity Framework creates a SQL UPDATE command, that command will include a WHERE clause that looks for a row that has the original `RowVersion` value.</span></span> <span data-ttu-id="d264f-210">更新コマンドで行に影響がない場合 (行には、元のあるありません`RowVersion`値)、Entity Framework をスロー、`DbUpdateConcurrencyException`例外。</span><span class="sxs-lookup"><span data-stu-id="d264f-210">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value),  the Entity Framework throws a `DbUpdateConcurrencyException` exception.</span></span>

<span data-ttu-id="d264f-211">その例外のキャッチ ブロック内のコード エンティティを取得します影響を受ける部署をから更新された値を持つ、`Entries`例外オブジェクトのプロパティです。</span><span class="sxs-lookup"><span data-stu-id="d264f-211">The code in the catch block for that exception gets the affected Department entity that has the updated values from the `Entries` property on the exception object.</span></span>

<span data-ttu-id="d264f-212">[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=164)]</span><span class="sxs-lookup"><span data-stu-id="d264f-212">[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=164)]</span></span>

<span data-ttu-id="d264f-213">`Entries`コレクションでは、1 つがあります`EntityEntry`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="d264f-213">The `Entries` collection will have just one `EntityEntry` object.</span></span>  <span data-ttu-id="d264f-214">そのオブジェクトを使用すると、ユーザーが入力した新しい値と現在のデータベースの値を取得します。</span><span class="sxs-lookup"><span data-stu-id="d264f-214">You can use that object to get the new values entered by the user and the current database values.</span></span>

<span data-ttu-id="d264f-215">[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=165-166)]</span><span class="sxs-lookup"><span data-stu-id="d264f-215">[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=165-166)]</span></span>

<span data-ttu-id="d264f-216">コードでは、データベースの値を持つ異なるページからユーザーが、編集時に入力した各列のカスタム エラー メッセージを追加する (1 つだけのフィールドは、次に示す簡略化のため)。</span><span class="sxs-lookup"><span data-stu-id="d264f-216">The code adds a custom error message for each column that has database values different from what the user entered on the Edit page (only one field is shown here for brevity).</span></span>

<span data-ttu-id="d264f-217">[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=174-178)]</span><span class="sxs-lookup"><span data-stu-id="d264f-217">[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=174-178)]</span></span>

<span data-ttu-id="d264f-218">コードの最後に、設定、`RowVersion`の値、`departmentToUpdate`新しい値をデータベースから取得します。</span><span class="sxs-lookup"><span data-stu-id="d264f-218">Finally, the code sets the `RowVersion` value of the `departmentToUpdate` to the new value retrieved from the database.</span></span> <span data-ttu-id="d264f-219">この新しい`RowVersion`ページが表示され、編集および次の時間をユーザーがクリックしたときに非表示のフィールドに保存する値**保存**の編集 ページの再表示が検出されるために発生する同時実行エラーのみです。</span><span class="sxs-lookup"><span data-stu-id="d264f-219">This new `RowVersion` value will be stored in the hidden field when the Edit page is redisplayed, and the next time the user clicks **Save**, only concurrency errors that happen since the redisplay of the Edit page will be caught.</span></span>

<span data-ttu-id="d264f-220">[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=199-200)]</span><span class="sxs-lookup"><span data-stu-id="d264f-220">[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=199-200)]</span></span>

<span data-ttu-id="d264f-221">`ModelState.Remove`ために、ステートメントは必要な`ModelState`が古い`RowVersion`値。</span><span class="sxs-lookup"><span data-stu-id="d264f-221">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="d264f-222">ビューで、`ModelState`フィールドよりも優先、モデル プロパティの値が両方に存在する場合の値します。</span><span class="sxs-lookup"><span data-stu-id="d264f-222">In the view, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-the-department-edit-view"></a><span data-ttu-id="d264f-223">更新プログラム ビューの部門の編集</span><span class="sxs-lookup"><span data-stu-id="d264f-223">Update the Department Edit view</span></span>

<span data-ttu-id="d264f-224">*Views/Departments/Edit.cshtml*、次の変更します。</span><span class="sxs-lookup"><span data-stu-id="d264f-224">In *Views/Departments/Edit.cshtml*, make the following changes:</span></span>

* <span data-ttu-id="d264f-225">削除、`<div>`がのスキャフォールディングされました要素、`RowVersion`フィールドです。</span><span class="sxs-lookup"><span data-stu-id="d264f-225">Remove the `<div>` element that was scaffolded for the `RowVersion` field.</span></span>

* <span data-ttu-id="d264f-226">保存する隠しフィールドを追加、`RowVersion`プロパティの値、用の隠しフィールドの直後、`DepartmentID`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="d264f-226">Add a hidden field to save the `RowVersion` property value, immediately following the hidden field for the `DepartmentID` property.</span></span>

* <span data-ttu-id="d264f-227">「管理者の選択」オプションをドロップダウン リストに追加します。</span><span class="sxs-lookup"><span data-stu-id="d264f-227">Add a "Select Administrator" option to the drop-down list.</span></span>

<span data-ttu-id="d264f-228">[!code-html[Main](intro/samples/cu/Views/Departments/Edit.cshtml?highlight=16,34-36)]</span><span class="sxs-lookup"><span data-stu-id="d264f-228">[!code-html[Main](intro/samples/cu/Views/Departments/Edit.cshtml?highlight=16,34-36)]</span></span>

## <a name="test-concurrency-conflicts-in-the-edit-page"></a><span data-ttu-id="d264f-229">[編集] ページでの同時実行の競合をテストします。</span><span class="sxs-lookup"><span data-stu-id="d264f-229">Test concurrency conflicts in the Edit page</span></span>

<span data-ttu-id="d264f-230">サイトを実行し、[部門インデックス ページに移動するのには、部門] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d264f-230">Run the site and click Departments to go to the Departments Index page.</span></span>

<span data-ttu-id="d264f-231">右クリックし、**編集**ハイパーリンクをクリックし、英語部門**新しいタブで開く**、をクリックして、**編集**英語部門のハイパーリンクのです。</span><span class="sxs-lookup"><span data-stu-id="d264f-231">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**, then click the **Edit** hyperlink for the English department.</span></span> <span data-ttu-id="d264f-232">2 つのブラウザー タブでは、同じ情報が表示されます。</span><span class="sxs-lookup"><span data-stu-id="d264f-232">The two browser tabs now display the same information.</span></span>

<span data-ttu-id="d264f-233">最初のブラウザー タブ内のフィールドを変更し、クリックして**保存**です。</span><span class="sxs-lookup"><span data-stu-id="d264f-233">Change a field in the first browser tab and click **Save**.</span></span>

![部門編集変更後のページ 1](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="d264f-235">ブラウザーでは、変更された値を持つインデックス ページを示します。</span><span class="sxs-lookup"><span data-stu-id="d264f-235">The browser shows the Index page with the changed value.</span></span>

<span data-ttu-id="d264f-236">2 番目のブラウザー タブ内のフィールドを変更します。</span><span class="sxs-lookup"><span data-stu-id="d264f-236">Change a field in the second browser tab.</span></span>

![部門編集変更した後に 2 ページ目](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="d264f-238">**[保存]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d264f-238">Click **Save**.</span></span> <span data-ttu-id="d264f-239">エラー メッセージを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d264f-239">You see an error message:</span></span>

![部門の編集 ページのエラー メッセージ](concurrency/_static/edit-error.png)

<span data-ttu-id="d264f-241">をクリックして**保存**もう一度です。</span><span class="sxs-lookup"><span data-stu-id="d264f-241">Click **Save** again.</span></span> <span data-ttu-id="d264f-242">2 番目のブラウザー タブで入力した値は保存されます。</span><span class="sxs-lookup"><span data-stu-id="d264f-242">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="d264f-243">保存された値が表示されるは、インデックス ページが表示されたときです。</span><span class="sxs-lookup"><span data-stu-id="d264f-243">You see the saved values when the Index page appears.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="d264f-244">更新プログラムの削除 ページ</span><span class="sxs-lookup"><span data-stu-id="d264f-244">Update the Delete page</span></span>

<span data-ttu-id="d264f-245">削除 ページは、Entity Framework は、それ以外の場合と同様の方法で、部門の編集のユーザーによる同時実行の競合を検出します。</span><span class="sxs-lookup"><span data-stu-id="d264f-245">For the Delete page, the Entity Framework detects concurrency conflicts caused by someone else editing the department in a similar manner.</span></span> <span data-ttu-id="d264f-246">ときに、HttpGet`Delete`メソッドは、確認のビューを表示、ビューに含まれる元`RowVersion`隠しフィールドの値。</span><span class="sxs-lookup"><span data-stu-id="d264f-246">When the HttpGet `Delete` method displays the confirmation view, the view includes the original `RowVersion` value in a hidden field.</span></span> <span data-ttu-id="d264f-247">値が、HttpPost を使用できますが、そのこと`Delete`ユーザー削除を確認するときに呼び出されるメソッド。</span><span class="sxs-lookup"><span data-stu-id="d264f-247">That value is then available to the HttpPost `Delete` method that's called when the user confirms the deletion.</span></span> <span data-ttu-id="d264f-248">Entity Framework では、SQL の DELETE コマンドを作成するとき、元の WHERE 句が含まれます`RowVersion`値。</span><span class="sxs-lookup"><span data-stu-id="d264f-248">When the Entity Framework creates the SQL DELETE command, it includes a WHERE clause with the original `RowVersion` value.</span></span> <span data-ttu-id="d264f-249">0 個の行で、コマンドの結果 (つまり、削除の確認 ページが表示された後に、行が変更された)、影響を受ける場合は、同時実行例外がスローされ、および、HttpGet`Delete`メソッドは、エラー フラグを再表示するためには、true に設定して、エラー メッセージを確認 ページ。</span><span class="sxs-lookup"><span data-stu-id="d264f-249">If the command results in zero rows affected (meaning the row was changed after the Delete confirmation page was displayed), a concurrency exception is thrown, and the HttpGet `Delete` method is called with an error flag set to true in order to redisplay the confirmation page with an error message.</span></span> <span data-ttu-id="d264f-250">その場合のエラー メッセージが表示されないように、別のユーザーによって行が削除されたため 0 行が影響を受けたこともできます。</span><span class="sxs-lookup"><span data-stu-id="d264f-250">It's also possible that zero rows were affected because the row was deleted by another user, so in that case no error message is displayed.</span></span>

### <a name="update-the-delete-methods-in-the-departments-controller"></a><span data-ttu-id="d264f-251">更新プログラムの部門コント ローラーの削除方法</span><span class="sxs-lookup"><span data-stu-id="d264f-251">Update the Delete methods in the Departments controller</span></span>

<span data-ttu-id="d264f-252">*DepartmentsController.cs*、置換、HttpGet`Delete`メソッドを次のコード。</span><span class="sxs-lookup"><span data-stu-id="d264f-252">In *DepartmentsController.cs*, replace the HttpGet `Delete` method with the following code:</span></span>

<span data-ttu-id="d264f-253">[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeleteGet&highlight=1,10,14-17,21-29)]</span><span class="sxs-lookup"><span data-stu-id="d264f-253">[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeleteGet&highlight=1,10,14-17,21-29)]</span></span>

<span data-ttu-id="d264f-254">このメソッドは、同時実行エラーの後、ページが表示されされているかどうかを示す省略可能なパラメーターを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="d264f-254">The method accepts an optional parameter that indicates whether the page is being redisplayed after a concurrency error.</span></span> <span data-ttu-id="d264f-255">このフラグが true、不要になったに指定された部門が存在する場合は、別のユーザーによって削除されました。</span><span class="sxs-lookup"><span data-stu-id="d264f-255">If this flag is true and the department specified no longer exists, it was deleted by another user.</span></span> <span data-ttu-id="d264f-256">その場合は、コードは、インデックス ページにリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="d264f-256">In that case, the code redirects to the Index page.</span></span>  <span data-ttu-id="d264f-257">このフラグが true であり、部門が存在して場合、は、別のユーザーによって変更されました。</span><span class="sxs-lookup"><span data-stu-id="d264f-257">If this flag is true and the Department does exist, it was changed by another user.</span></span> <span data-ttu-id="d264f-258">ビューを使用するコードがエラー メッセージを送信する場合は、`ViewData`です。</span><span class="sxs-lookup"><span data-stu-id="d264f-258">In that case, the code sends an error message to the view using `ViewData`.</span></span>  

<span data-ttu-id="d264f-259">コード、HttpPost で置き換え`Delete`メソッド (名前付き`DeleteConfirmed`) を次のコード。</span><span class="sxs-lookup"><span data-stu-id="d264f-259">Replace the code in the HttpPost `Delete` method (named `DeleteConfirmed`) with the following code:</span></span>

<span data-ttu-id="d264f-260">[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeletePost&highlight=1,3,5-8,11-18)]</span><span class="sxs-lookup"><span data-stu-id="d264f-260">[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeletePost&highlight=1,3,5-8,11-18)]</span></span>

<span data-ttu-id="d264f-261">交換したスキャフォールディングのコードでは、このメソッドは、レコード ID のみを使用できます。</span><span class="sxs-lookup"><span data-stu-id="d264f-261">In the scaffolded code that you just replaced, this method accepted only a record ID:</span></span>


```csharp
public async Task<IActionResult> DeleteConfirmed(int id)
```

<span data-ttu-id="d264f-262">このパラメーターは、モデル バインダーによって作成された部門エンティティ インスタンスに変更しました。</span><span class="sxs-lookup"><span data-stu-id="d264f-262">You've changed this parameter to a Department entity instance created by the model binder.</span></span> <span data-ttu-id="d264f-263">これにより、レコードのキーだけでなく RowVersion プロパティの値に EF アクセスします。</span><span class="sxs-lookup"><span data-stu-id="d264f-263">This gives EF access to the RowVersion property value in addition to the record key.</span></span>

```csharp
public async Task<IActionResult> Delete(Department department)
```

<span data-ttu-id="d264f-264">アクション メソッドの名前も変更した`DeleteConfirmed`に`Delete`です。</span><span class="sxs-lookup"><span data-stu-id="d264f-264">You have also changed the action method name from `DeleteConfirmed` to `Delete`.</span></span> <span data-ttu-id="d264f-265">スキャフォールディングのコードには、名前が使用されて`DeleteConfirmed`HttpPost メソッドに一意のシグネチャを指定します。</span><span class="sxs-lookup"><span data-stu-id="d264f-265">The scaffolded code used the name `DeleteConfirmed` to give the HttpPost method a unique signature.</span></span> <span data-ttu-id="d264f-266">(CLR には、別のメソッドのパラメーターを持つオーバー ロードされたメソッドが必要があります)。これで、署名が一意では MVC 規則のオプションを使用し、同じ名前、HttpPost、HttpGet delete メソッドを使用できます。</span><span class="sxs-lookup"><span data-stu-id="d264f-266">(The CLR requires overloaded methods to have different method parameters.) Now that the signatures are unique, you can stick with the MVC convention and use the same name for the HttpPost and HttpGet delete methods.</span></span>

<span data-ttu-id="d264f-267">場合は、部門は既に削除されて、`AnyAsync`メソッドは false を返し、アプリケーションだけに戻るインデックス メソッドです。</span><span class="sxs-lookup"><span data-stu-id="d264f-267">If the department is already deleted, the `AnyAsync` method returns false and the application just goes back to the Index method.</span></span>

<span data-ttu-id="d264f-268">同時実行エラーが検出された場合、コードは削除の確認 ページを再表示し、それを示すフラグは同時実行エラー メッセージを表示する必要がありますを提供します。</span><span class="sxs-lookup"><span data-stu-id="d264f-268">If a concurrency error is caught, the code redisplays the Delete confirmation page and provides a flag that indicates it should display a concurrency error message.</span></span>

### <a name="update-the-delete-view"></a><span data-ttu-id="d264f-269">削除ビューを更新します。</span><span class="sxs-lookup"><span data-stu-id="d264f-269">Update the Delete view</span></span>

<span data-ttu-id="d264f-270">*Views/Departments/Delete.cshtml*、スキャフォールディングのコードをエラー メッセージ フィールドと DepartmentID および RowVersion のプロパティの非表示フィールドを追加する次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="d264f-270">In *Views/Departments/Delete.cshtml*, replace the scaffolded code with the following code that adds an error message field and hidden fields for the DepartmentID and RowVersion properties.</span></span> <span data-ttu-id="d264f-271">変更が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="d264f-271">The changes are highlighted.</span></span>

<span data-ttu-id="d264f-272">[!code-html[Main](intro/samples/cu/Views/Departments/Delete.cshtml?highlight=9,38,44,45,48)]</span><span class="sxs-lookup"><span data-stu-id="d264f-272">[!code-html[Main](intro/samples/cu/Views/Departments/Delete.cshtml?highlight=9,38,44,45,48)]</span></span>

<span data-ttu-id="d264f-273">これは、次の変更を加えます。</span><span class="sxs-lookup"><span data-stu-id="d264f-273">This makes the following changes:</span></span>

* <span data-ttu-id="d264f-274">間でエラー メッセージを追加、`h2`と`h3`見出し。</span><span class="sxs-lookup"><span data-stu-id="d264f-274">Adds an error message between the `h2` and `h3` headings.</span></span>

* <span data-ttu-id="d264f-275">LastName を置き換えますで FullName、**管理者**フィールドです。</span><span class="sxs-lookup"><span data-stu-id="d264f-275">Replaces LastName with FullName in the **Administrator** field.</span></span>

* <span data-ttu-id="d264f-276">RowVersion フィールドを削除します。</span><span class="sxs-lookup"><span data-stu-id="d264f-276">Removes the RowVersion field.</span></span>

* <span data-ttu-id="d264f-277">隠しフィールドを追加、`RowVersion`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="d264f-277">Adds a hidden field for the `RowVersion` property.</span></span>

<span data-ttu-id="d264f-278">部門のインデックス ページを実行します。</span><span class="sxs-lookup"><span data-stu-id="d264f-278">Run the Departments Index page.</span></span> <span data-ttu-id="d264f-279">右クリックして、**削除**ハイパーリンクをクリックし、英語部門**新しいタブで開く**、最初のタブをクリックして、**編集**英語部門のハイパーリンクのです。</span><span class="sxs-lookup"><span data-stu-id="d264f-279">Right click the **Delete** hyperlink for the English department and select **Open in new tab**, then in the first tab click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="d264f-280">最初のウィンドウで、値のいずれかを変更し、クリックして**保存**:</span><span class="sxs-lookup"><span data-stu-id="d264f-280">In the first window, change one of the values, and click **Save**:</span></span>

![削除する前に変更した後に部門の編集 ページ](concurrency/_static/edit-after-change-for-delete.png)

<span data-ttu-id="d264f-282">2 番目のタブをクリックして**削除**です。</span><span class="sxs-lookup"><span data-stu-id="d264f-282">In the second tab, click **Delete**.</span></span> <span data-ttu-id="d264f-283">同時実行エラー メッセージが表示および部門値が現在のデータベースにあるもので更新します。</span><span class="sxs-lookup"><span data-stu-id="d264f-283">You see the concurrency error message, and the Department values are refreshed with what's currently in the database.</span></span>

![同時実行エラーのため、部門削除の確認 ページ](concurrency/_static/delete-error.png)

<span data-ttu-id="d264f-285">クリックすると**削除**もう一度、部門が削除されたことを示しています。 インデックスのページにリダイレクトしています。</span><span class="sxs-lookup"><span data-stu-id="d264f-285">If you click **Delete** again, you're redirected to the Index page, which shows that the department has been deleted.</span></span>

## <a name="update-details-and-create-views"></a><span data-ttu-id="d264f-286">更新プログラムの詳細と、ビューの作成</span><span class="sxs-lookup"><span data-stu-id="d264f-286">Update Details and Create views</span></span>

<span data-ttu-id="d264f-287">必要に応じて詳細にスキャフォールディング コードをクリーンアップしてビューを作成できます。</span><span class="sxs-lookup"><span data-stu-id="d264f-287">You can optionally clean up scaffolded code in the Details and Create views.</span></span>

<span data-ttu-id="d264f-288">コードに置き換えます*Views/Departments/Details.cshtml* RowVersion 列を削除し、管理者の完全な名前を表示します。</span><span class="sxs-lookup"><span data-stu-id="d264f-288">Replace the code in *Views/Departments/Details.cshtml* to delete the RowVersion column and show the full name of the Administrator.</span></span>

<span data-ttu-id="d264f-289">[!code-html[Main](intro/samples/cu/Views/Departments/Details.cshtml?highlight=35)]</span><span class="sxs-lookup"><span data-stu-id="d264f-289">[!code-html[Main](intro/samples/cu/Views/Departments/Details.cshtml?highlight=35)]</span></span>

<span data-ttu-id="d264f-290">コードに置き換えます*Views/Departments/Create.cshtml*選択オプションをドロップダウン リストに追加します。</span><span class="sxs-lookup"><span data-stu-id="d264f-290">Replace the code in *Views/Departments/Create.cshtml* to add a Select option to the drop-down list.</span></span>

<span data-ttu-id="d264f-291">[!code-html[Main](intro/samples/cu/Views/Departments/Create.cshtml?highlight=32-34)]</span><span class="sxs-lookup"><span data-stu-id="d264f-291">[!code-html[Main](intro/samples/cu/Views/Departments/Create.cshtml?highlight=32-34)]</span></span>

## <a name="summary"></a><span data-ttu-id="d264f-292">概要</span><span class="sxs-lookup"><span data-stu-id="d264f-292">Summary</span></span>

<span data-ttu-id="d264f-293">これは、同時実行の競合の処理の概要を完了します。</span><span class="sxs-lookup"><span data-stu-id="d264f-293">This completes the introduction to handling concurrency conflicts.</span></span> <span data-ttu-id="d264f-294">EF Core での同時実行を処理する方法の詳細については、次を参照してください。[同時実行の競合](https://docs.microsoft.com/ef/core/saving/concurrency)です。</span><span class="sxs-lookup"><span data-stu-id="d264f-294">For more information about how to handle concurrency in EF Core, see [Concurrency conflicts](https://docs.microsoft.com/ef/core/saving/concurrency).</span></span> <span data-ttu-id="d264f-295">次のチュートリアルでは、インストラクターと学生エンティティのテーブルの階層あたりの継承を実装する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="d264f-295">The next tutorial shows how to implement table-per-hierarchy inheritance for the Instructor and Student entities.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="d264f-296">[前へ](update-related-data.md)
[次へ](inheritance.md)</span><span class="sxs-lookup"><span data-stu-id="d264f-296">[Previous](update-related-data.md)
[Next](inheritance.md)</span></span>  
