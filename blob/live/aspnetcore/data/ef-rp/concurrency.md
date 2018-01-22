---
title: "Razor ページの EF コアでの同時実行性、8 8"
author: rick-anderson
description: "このチュートリアルでは、複数のユーザーが同時に同じエンティティを更新するときに競合を処理する方法を示します。"
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/concurrency
ms.openlocfilehash: a980669d49d332d7ef2ff5a18c73e9b269281287
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
<span data-ttu-id="e97f3-103">en-米国/</span><span class="sxs-lookup"><span data-stu-id="e97f3-103">en-us/</span></span>

# <a name="handling-concurrency-conflicts---ef-core-with-razor-pages-8-of-8"></a><span data-ttu-id="e97f3-104">同時実行の競合の EF コア Razor ページ (8 の 8) での処理</span><span class="sxs-lookup"><span data-stu-id="e97f3-104">Handling concurrency conflicts - EF Core with Razor Pages (8 of 8)</span></span>

<span data-ttu-id="e97f3-105">によって[Rick Anderson](https://twitter.com/RickAndMSFT)、 [Tom Dykstra](https://github.com/tdykstra)、および[Jon P Smith](https://twitter.com/thereformedprog)</span><span class="sxs-lookup"><span data-stu-id="e97f3-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), and  [Jon P Smith](https://twitter.com/thereformedprog)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="e97f3-106">このチュートリアルでは、複数のユーザーが同時に (一度に) エンティティを更新するときに競合を処理する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-106">This tutorial shows how to handle conflicts when multiple users update an entity concurrently (at the same time).</span></span> <span data-ttu-id="e97f3-107">問題を解決できない場合に実行する場合は、ダウンロード、[この段階でのアプリを完成](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8)です。</span><span class="sxs-lookup"><span data-stu-id="e97f3-107">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).</span></span>

## <a name="concurrency-conflicts"></a><span data-ttu-id="e97f3-108">同時実行の競合</span><span class="sxs-lookup"><span data-stu-id="e97f3-108">Concurrency conflicts</span></span>

<span data-ttu-id="e97f3-109">同時実行の競合が発生した場合。</span><span class="sxs-lookup"><span data-stu-id="e97f3-109">A concurrency conflict occurs when:</span></span>

* <span data-ttu-id="e97f3-110">ユーザーは、エンティティの編集 ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-110">A user navigates to the edit page for an entity.</span></span>
* <span data-ttu-id="e97f3-111">別のユーザーは、最初のユーザーの変更がデータベースに書き込まれる前に、同じエンティティを更新します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-111">Another user updates the same entity before the first user's change is written to the DB.</span></span>

<span data-ttu-id="e97f3-112">同時実行の検出が有効でない場合、同時実行更新が発生した場合。</span><span class="sxs-lookup"><span data-stu-id="e97f3-112">If concurrency detection is not enabled, when concurrent updates occur:</span></span>

* <span data-ttu-id="e97f3-113">最後の更新の競合。</span><span class="sxs-lookup"><span data-stu-id="e97f3-113">The last update wins.</span></span> <span data-ttu-id="e97f3-114">つまり、最後の値の更新は、データベースに保存されます。</span><span class="sxs-lookup"><span data-stu-id="e97f3-114">That is, the last update values are saved to the DB.</span></span>
* <span data-ttu-id="e97f3-115">現在の更新プログラムの最初の数値は失われます。</span><span class="sxs-lookup"><span data-stu-id="e97f3-115">The first of the current updates are lost.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="e97f3-116">オプティミスティック同時実行制御</span><span class="sxs-lookup"><span data-stu-id="e97f3-116">Optimistic concurrency</span></span>

<span data-ttu-id="e97f3-117">オプティミスティック同時実行制御により、同時実行の競合が発生するし対処する適切な場合、操作を行います。</span><span class="sxs-lookup"><span data-stu-id="e97f3-117">Optimistic concurrency allows concurrency conflicts to happen, and then reacts appropriately when they do.</span></span> <span data-ttu-id="e97f3-118">たとえば、加藤さんは、部門の編集 ページにアクセスし、$350,000.00 から $0.00 に英語版の部門に予算を変更します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-118">For example, Jane visits the Department edit page and changes the budget for the English department from $350,000.00 to $0.00.</span></span>

![予算を 0 に変更します。](concurrency/_static/change-budget.png)

<span data-ttu-id="e97f3-120">加藤さんがクリックする前に**保存**John は、同じページにアクセスし、2007 年 9 月 1 日から 2013 年 9 月 1 日に、Start Date フィールドを変更します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-120">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![2013 を開始日を変更します。](concurrency/_static/change-date.png)

<span data-ttu-id="e97f3-122">加藤さんがクリックした**保存**最初られ、ブラウザーには、インデックス ページが表示されます。 変更彼女に表示されます。</span><span class="sxs-lookup"><span data-stu-id="e97f3-122">Jane clicks **Save** first and sees her change when the browser displays the Index page.</span></span>

![予算を 0 に変更されました](concurrency/_static/budget-zero.png)

<span data-ttu-id="e97f3-124">John が**保存**ドル 350,000.00 の予算が表示される編集ページにします。</span><span class="sxs-lookup"><span data-stu-id="e97f3-124">John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="e97f3-125">次の動作は、同時実行の競合を処理する方法によって決まります。</span><span class="sxs-lookup"><span data-stu-id="e97f3-125">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="e97f3-126">オプティミスティック同時実行制御には、次のオプションが含まれています。</span><span class="sxs-lookup"><span data-stu-id="e97f3-126">Optimistic concurrency includes the following options:</span></span>

* <span data-ttu-id="e97f3-127">プロパティをユーザーが変更の追跡および、DB に対応する列のみを更新できます。</span><span class="sxs-lookup"><span data-stu-id="e97f3-127">You can keep track of which property a user has modified and update only the corresponding columns in the DB.</span></span>

 <span data-ttu-id="e97f3-128">シナリオでは、データは失われません。</span><span class="sxs-lookup"><span data-stu-id="e97f3-128">In the scenario, no data would be lost.</span></span> <span data-ttu-id="e97f3-129">さまざまなプロパティは、2 人のユーザーによって更新されました。</span><span class="sxs-lookup"><span data-stu-id="e97f3-129">Different properties were updated by the two users.</span></span> <span data-ttu-id="e97f3-130">次回の英語版の部門を参照するユーザーがジェーンのと John の両方の変更が表示されます。</span><span class="sxs-lookup"><span data-stu-id="e97f3-130">The next time someone browses the English department, they'll see both Jane's and John's changes.</span></span> <span data-ttu-id="e97f3-131">更新には、このメソッドは、データが失われる可能性のある競合の数を減らすことができます。</span><span class="sxs-lookup"><span data-stu-id="e97f3-131">This method of updating can reduce the number of conflicts that could result in data loss.</span></span> <span data-ttu-id="e97f3-132">このアプローチ: \* 競合する変更が同じプロパティに加えられた場合、データの損失を回避することはできません。</span><span class="sxs-lookup"><span data-stu-id="e97f3-132">This approach: \* Can't avoid data loss if competing changes are made to the same property.</span></span>
        <span data-ttu-id="e97f3-133">\* は、web アプリで実際的で、通常ありません。</span><span class="sxs-lookup"><span data-stu-id="e97f3-133">\* Is generally not practical in a web app.</span></span> <span data-ttu-id="e97f3-134">すべてのフェッチ値と新しい値を追跡するために重要な状態を維持することが必要です。</span><span class="sxs-lookup"><span data-stu-id="e97f3-134">It requires maintaining significant state in order to keep track of all fetched values and new values.</span></span> <span data-ttu-id="e97f3-135">大量の状態を保持することと、アプリのパフォーマンスが影響することができます。</span><span class="sxs-lookup"><span data-stu-id="e97f3-135">Maintaining large amounts of state can affect app performance.</span></span>
        <span data-ttu-id="e97f3-136">\* が、エンティティの同時実行の検出と比較して、アプリの複雑度が向上します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-136">\* Can increase app complexity compared to concurrency detection on an entity.</span></span>

* <span data-ttu-id="e97f3-137">John の変更がジェーンの変更を上書きすることができます。</span><span class="sxs-lookup"><span data-stu-id="e97f3-137">You can let John's change overwrite Jane's change.</span></span>

 <span data-ttu-id="e97f3-138">次に英語版の部門を参照するユーザーが、2013 年 9 月 1 日が表示されます、フェッチされたドル 350,000.00 値。</span><span class="sxs-lookup"><span data-stu-id="e97f3-138">The next time someone browses the English department, they'll see 9/1/2013 and the fetched $350,000.00 value.</span></span> <span data-ttu-id="e97f3-139">この方法と呼ばれる、*クライアントが Wins*または*Wins の最後に*シナリオです。</span><span class="sxs-lookup"><span data-stu-id="e97f3-139">This approach is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="e97f3-140">(クライアントからのすべての値よりも優先、データ ストアには、新機能です。)同時実行処理のコーディングを行わない場合は自動的に行われますクライアントが優先されます。</span><span class="sxs-lookup"><span data-stu-id="e97f3-140">(All values from the client take precedence over what's in the data store.) If you don't do any coding for concurrency handling, Client Wins happens automatically.</span></span>

* <span data-ttu-id="e97f3-141">John の変更は、データベースで更新されているを妨害することができます。</span><span class="sxs-lookup"><span data-stu-id="e97f3-141">You can prevent John's change from being updated in the DB.</span></span> <span data-ttu-id="e97f3-142">通常、アプリは: \* エラー メッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-142">Typically, the app would: \* Display an error message.</span></span>
        <span data-ttu-id="e97f3-143">\* データの現在の状態を表示します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-143">\* Show the current state of the data.</span></span>
        <span data-ttu-id="e97f3-144">\*、変更を再適用するユーザーを許可します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-144">\* Allow the user to reapply the changes.</span></span>

 <span data-ttu-id="e97f3-145">これと呼ばれる、*ストア Wins*シナリオです。</span><span class="sxs-lookup"><span data-stu-id="e97f3-145">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="e97f3-146">(データ ストアの値よりも優先、クライアントから送信された値です。)このチュートリアルでは、ストア Wins シナリオを実装します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-146">(The data-store values take precedence over the values submitted by the client.) You implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="e97f3-147">このメソッドは、警告を表示されているユーザーがいない状態の変更が上書きされないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-147">This method ensures that no changes are overwritten without a user being alerted.</span></span>

## <a name="handling-concurrency"></a><span data-ttu-id="e97f3-148">同時実行の処理</span><span class="sxs-lookup"><span data-stu-id="e97f3-148">Handling concurrency</span></span> 

<span data-ttu-id="e97f3-149">プロパティとして構成されている場合、[同時実行トークン](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency):</span><span class="sxs-lookup"><span data-stu-id="e97f3-149">When a property is configured as a [concurrency token](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency):</span></span>

* <span data-ttu-id="e97f3-150">EF コアは、フェッチ後にプロパティが変更されていないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-150">EF Core verifies that property has not been modified after it was fetched.</span></span> <span data-ttu-id="e97f3-151">チェックが実行時に[SaveChanges](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges)または[SaveChangesAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_)と呼びます。</span><span class="sxs-lookup"><span data-stu-id="e97f3-151">The check occurs when [SaveChanges](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) or [SaveChangesAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) is called.</span></span>
* <span data-ttu-id="e97f3-152">フェッチ後、プロパティが変更された場合、 [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0)がスローされます。</span><span class="sxs-lookup"><span data-stu-id="e97f3-152">If the property has been changed after it was fetched, a [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) is thrown.</span></span> 

<span data-ttu-id="e97f3-153">スローされることをサポートするために、データベースとデータ モデルを構成する必要があります`DbUpdateConcurrencyException`です。</span><span class="sxs-lookup"><span data-stu-id="e97f3-153">The DB and data model must be configured to support throwing `DbUpdateConcurrencyException`.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-property"></a><span data-ttu-id="e97f3-154">プロパティの同時実行の競合を検出します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-154">Detecting concurrency conflicts on a property</span></span>

<span data-ttu-id="e97f3-155">使用してプロパティ レベルに同時実行の競合を検出できなかったことができます、 [ConcurrencyCheck](https://docs.microsoft.com/en-us/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0)属性。</span><span class="sxs-lookup"><span data-stu-id="e97f3-155">Concurrency conflicts can be detected at the property level with the [ConcurrencyCheck](https://docs.microsoft.com/en-us/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) attribute.</span></span> <span data-ttu-id="e97f3-156">属性は、モデルに複数のプロパティに適用できます。</span><span class="sxs-lookup"><span data-stu-id="e97f3-156">The attribute can be applied to multiple properties on the model.</span></span> <span data-ttu-id="e97f3-157">詳細については、次を参照してください。[データ注釈 ConcurrencyCheck](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency#data-annotations)です。</span><span class="sxs-lookup"><span data-stu-id="e97f3-157">For more information, see [Data Annotations-ConcurrencyCheck](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency#data-annotations).</span></span>

<span data-ttu-id="e97f3-158">`[ConcurrencyCheck]`属性は、このチュートリアルでは使用されません。</span><span class="sxs-lookup"><span data-stu-id="e97f3-158">The `[ConcurrencyCheck]` attribute is not used in this tutorial.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-row"></a><span data-ttu-id="e97f3-159">行を同時実行の競合を検出します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-159">Detecting concurrency conflicts on a row</span></span>

<span data-ttu-id="e97f3-160">同時実行の競合を検出するために、 [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql)列を追跡は、モデルに追加します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-160">To detect concurrency conflicts, a [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) tracking column is added to the model.</span></span>  <span data-ttu-id="e97f3-161">`rowversion` :</span><span class="sxs-lookup"><span data-stu-id="e97f3-161">`rowversion` :</span></span>

* <span data-ttu-id="e97f3-162">SQL Server は固有です。</span><span class="sxs-lookup"><span data-stu-id="e97f3-162">Is SQL Server specific.</span></span> <span data-ttu-id="e97f3-163">他のデータベースはのような機能を備えていない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="e97f3-163">Other databases may not provide a similar feature.</span></span>
* <span data-ttu-id="e97f3-164">データベースからフェッチされたエンティティが変更されていないことを確認に使用されます。</span><span class="sxs-lookup"><span data-stu-id="e97f3-164">Is used to determine that an entity has not been changed since it was fetched from the DB.</span></span> 

<span data-ttu-id="e97f3-165">DB 生成順次`rowversion`がインクリメントごとに、行番号を更新します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-165">The DB generates a sequential `rowversion` number that's incremented each time the row is updated.</span></span> <span data-ttu-id="e97f3-166">`Update`または`Delete`コマンド、`Where`句には、フェッチされた値が含まれています。`rowversion`です。</span><span class="sxs-lookup"><span data-stu-id="e97f3-166">In an `Update` or `Delete` command, the `Where` clause includes the fetched value of `rowversion`.</span></span> <span data-ttu-id="e97f3-167">場合は、更新された行が変更されました。</span><span class="sxs-lookup"><span data-stu-id="e97f3-167">If the row being updated has changed:</span></span>

 * <span data-ttu-id="e97f3-168">`rowversion`フェッチされた値が一致しません。</span><span class="sxs-lookup"><span data-stu-id="e97f3-168">`rowversion` doesn't match the fetched value.</span></span>
 * <span data-ttu-id="e97f3-169">`Update`または`Delete`コマンドは、行を検索しないため、`Where`句が含まれていますが、フェッチされた`rowversion`です。</span><span class="sxs-lookup"><span data-stu-id="e97f3-169">The `Update` or `Delete` commands don't find a row because the `Where` clause includes the fetched `rowversion`.</span></span>
 * <span data-ttu-id="e97f3-170">A`DbUpdateConcurrencyException`がスローされます。</span><span class="sxs-lookup"><span data-stu-id="e97f3-170">A `DbUpdateConcurrencyException` is thrown.</span></span>

<span data-ttu-id="e97f3-171">EF コア、によって行が更新されていないときに、`Update`または`Delete`コマンドを同時実行例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="e97f3-171">In EF Core, when no rows have been updated by an `Update` or `Delete` command, a concurrency exception is thrown.</span></span>

### <a name="add-a-tracking-property-to-the-department-entity"></a><span data-ttu-id="e97f3-172">追跡プロパティ、Department エンティティを追加します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-172">Add a tracking property to the Department entity</span></span>

<span data-ttu-id="e97f3-173">*Models/Department.cs*RowVersion をという名前の追跡のプロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-173">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

<span data-ttu-id="e97f3-174">[タイムスタンプ](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute)でこの列が含まれている属性を指定、`Where`の句`Update`と`Delete`コマンド。</span><span class="sxs-lookup"><span data-stu-id="e97f3-174">The [Timestamp](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute) attribute specifies that this column is included in the `Where` clause of `Update` and `Delete` commands.</span></span> <span data-ttu-id="e97f3-175">属性といいます`Timestamp`旧バージョンの SQL Server、SQL を使用するため`timestamp`データ型、SQL に`rowversion`型で置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="e97f3-175">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` type replaced it.</span></span>

<span data-ttu-id="e97f3-176">Fluent API では、追跡プロパティも指定できます。</span><span class="sxs-lookup"><span data-stu-id="e97f3-176">The fluent API can also specify the tracking property:</span></span>

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

<span data-ttu-id="e97f3-177">次のコードは、部門名が更新されたときに、EF コアによって生成された T-SQL ステートメントの一部を示しています。</span><span class="sxs-lookup"><span data-stu-id="e97f3-177">The following code shows a portion of the T-SQL generated by EF Core when the Department name is updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

<span data-ttu-id="e97f3-178">上記のコードに示す強調表示されている、`WHERE`句を含む`RowVersion`です。</span><span class="sxs-lookup"><span data-stu-id="e97f3-178">The preceding highlighted code shows the `WHERE` clause containing `RowVersion`.</span></span> <span data-ttu-id="e97f3-179">場合 DB`RowVersion`値が一致しません、`RowVersion`パラメーター (`@p2`)、行は更新されません。</span><span class="sxs-lookup"><span data-stu-id="e97f3-179">If the DB `RowVersion` doesn't equal the `RowVersion` parameter (`@p2`), no rows are updated.</span></span>

<span data-ttu-id="e97f3-180">次の強調表示されたコードは、ただ 1 つの行が更新されたことを検証する T-SQL ステートメントを示しています。</span><span class="sxs-lookup"><span data-stu-id="e97f3-180">The following highlighted code shows the T-SQL that verifies exactly one row was updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

<span data-ttu-id="e97f3-181">[@@ROWCOUNT ](https://docs.microsoft.com/en-us/sql/t-sql/functions/rowcount-transact-sql)最後のステートメントによる影響を受ける行の数を返します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-181">[@@ROWCOUNT](https://docs.microsoft.com/en-us/sql/t-sql/functions/rowcount-transact-sql) returns the number of rows affected by the last statement.</span></span> <span data-ttu-id="e97f3-182">なしで行が更新されて、EF コアをスロー、`DbUpdateConcurrencyException`です。</span><span class="sxs-lookup"><span data-stu-id="e97f3-182">In no rows are updated, EF Core throws a `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="e97f3-183">Visual Studio の出力ウィンドウに T-SQL EF コアが生成されるを確認できます。</span><span class="sxs-lookup"><span data-stu-id="e97f3-183">You can see the T-SQL EF Core generates in the output window of Visual Studio.</span></span>

### <a name="update-the-db"></a><span data-ttu-id="e97f3-184">DB を更新します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-184">Update the DB</span></span>

<span data-ttu-id="e97f3-185">追加する、`RowVersion`プロパティが、移行を必要とする DB モデルを変更します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-185">Adding the `RowVersion` property changes the DB model, which requires a migration.</span></span>

<span data-ttu-id="e97f3-186">プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="e97f3-186">Build the project.</span></span> <span data-ttu-id="e97f3-187">コマンド ウィンドウで、次を入力します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-187">Enter the following in a command window:</span></span>

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

<span data-ttu-id="e97f3-188">上記のコマンド:</span><span class="sxs-lookup"><span data-stu-id="e97f3-188">The preceding commands:</span></span>

* <span data-ttu-id="e97f3-189">追加、*移行/{時間 stamp}_RowVersion.cs*移行ファイル。</span><span class="sxs-lookup"><span data-stu-id="e97f3-189">Adds the *Migrations/{time stamp}_RowVersion.cs* migration file.</span></span>
* <span data-ttu-id="e97f3-190">更新プログラム、 *Migrations/SchoolContextModelSnapshot.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="e97f3-190">Updates the *Migrations/SchoolContextModelSnapshot.cs* file.</span></span> <span data-ttu-id="e97f3-191">次の強調表示されたコードを追加する、更新、`BuildModel`メソッド。</span><span class="sxs-lookup"><span data-stu-id="e97f3-191">The update adds the following highlighted code to the `BuildModel` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* <span data-ttu-id="e97f3-192">DB の更新への移行を実行します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-192">Runs migrations to update the DB.</span></span>

<a name="scaffold"></a>
## <a name="scaffold-the-departments-model"></a><span data-ttu-id="e97f3-193">Scaffold 部門モデル</span><span class="sxs-lookup"><span data-stu-id="e97f3-193">Scaffold the Departments model</span></span>

* <span data-ttu-id="e97f3-194">Visual Studio を終了します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-194">Exit Visual Studio.</span></span>
* <span data-ttu-id="e97f3-195">プロジェクト ディレクトリ (*Program.cs*、*Startup.cs*、および *.csproj* ファイルを含むディレクトリ) でコマンド ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="e97f3-195">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="e97f3-196">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-196">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
 ```

<span data-ttu-id="e97f3-197">上記のコマンド スキャフォールディング、`Department`モデル。</span><span class="sxs-lookup"><span data-stu-id="e97f3-197">The preceding command scaffolds the `Department` model.</span></span> <span data-ttu-id="e97f3-198">Visual Studio でプロジェクトを開きます。</span><span class="sxs-lookup"><span data-stu-id="e97f3-198">Open the project in Visual Studio.</span></span>

<span data-ttu-id="e97f3-199">プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="e97f3-199">Build the project.</span></span> <span data-ttu-id="e97f3-200">ビルドには、次のようなエラーが生成されます。</span><span class="sxs-lookup"><span data-stu-id="e97f3-200">The build generates errors like the following:</span></span>

`1>Pages/Departments/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Department' and no extension method 'Department' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="e97f3-201">グローバルに変更する`_context.Department`に`_context.Departments`(つまり、"s"を追加する`Department`)。</span><span class="sxs-lookup"><span data-stu-id="e97f3-201">Globally change `_context.Department` to `_context.Departments` (that is, add an "s" to `Department`).</span></span> <span data-ttu-id="e97f3-202">7 回の出現が検出され、更新します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-202">7 occurrences are found and updated.</span></span>

### <a name="update-the-departments-index-page"></a><span data-ttu-id="e97f3-203">部門のインデックス ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-203">Update the Departments Index page</span></span>

<span data-ttu-id="e97f3-204">作成スキャフォールディング エンジン、`RowVersion`インデックス ページがそのフィールドの列を表示することはできません。</span><span class="sxs-lookup"><span data-stu-id="e97f3-204">The scaffolding engine created a `RowVersion` column for the Index page, but that field shouldn't be displayed.</span></span> <span data-ttu-id="e97f3-205">このチュートリアルの最後のバイトで、`RowVersion`同時実行制御の理解に役立つが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e97f3-205">In this tutorial, the last byte of the `RowVersion` is displayed to help understand concurrency.</span></span> <span data-ttu-id="e97f3-206">最後のバイトは、一意であることは保証されません。</span><span class="sxs-lookup"><span data-stu-id="e97f3-206">The last byte is not guaranteed to be unique.</span></span> <span data-ttu-id="e97f3-207">実際のアプリを表示しない`RowVersion`またはの最後のバイト`RowVersion`です。</span><span class="sxs-lookup"><span data-stu-id="e97f3-207">A real app wouldn't display `RowVersion` or the last byte of `RowVersion`.</span></span>

<span data-ttu-id="e97f3-208">インデックス ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-208">Update the Index page:</span></span>

* <span data-ttu-id="e97f3-209">部門では、インデックスを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="e97f3-209">Replace Index with Departments.</span></span>
* <span data-ttu-id="e97f3-210">含むマークアップを置き換える`RowVersion`の最後のバイトについて`RowVersion`です。</span><span class="sxs-lookup"><span data-stu-id="e97f3-210">Replace the markup containing `RowVersion` with the last byte of `RowVersion`.</span></span>
* <span data-ttu-id="e97f3-211">FullName を FirstMidName を置き換えます。</span><span class="sxs-lookup"><span data-stu-id="e97f3-211">Replace FirstMidName with FullName.</span></span>

<span data-ttu-id="e97f3-212">次のマークアップは、更新されたページを示しています。</span><span class="sxs-lookup"><span data-stu-id="e97f3-212">The following markup shows the updated page:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a><span data-ttu-id="e97f3-213">編集ページ モデルを更新します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-213">Update the Edit page model</span></span>

<span data-ttu-id="e97f3-214">更新*pages\departments\edit.cshtml.cs*を次のコード。</span><span class="sxs-lookup"><span data-stu-id="e97f3-214">Update *pages\departments\edit.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="e97f3-215">同時実行の問題を検出するために、 [OriginalValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue)で更新が、`rowVersion`フェッチ エンティティからの値。</span><span class="sxs-lookup"><span data-stu-id="e97f3-215">To detect a concurrency issue, the [OriginalValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) is updated with the `rowVersion` value from the entity it was fetched.</span></span> <span data-ttu-id="e97f3-216">EF コア元を含む WHERE 句を使用して SQL の UPDATE コマンドが生成されます`RowVersion`値。</span><span class="sxs-lookup"><span data-stu-id="e97f3-216">EF Core generates a SQL UPDATE command with a WHERE clause containing the original `RowVersion` value.</span></span> <span data-ttu-id="e97f3-217">更新コマンドで行に影響がない場合 (行には、元のあるありません`RowVersion`値)、`DbUpdateConcurrencyException`例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="e97f3-217">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value), a `DbUpdateConcurrencyException` exception is thrown.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-)]

<span data-ttu-id="e97f3-218">上記のコードで`Department.RowVersion`エンティティがフェッチしたときに、値は、します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-218">In the preceding code, `Department.RowVersion` is the value when the entity was fetched.</span></span> <span data-ttu-id="e97f3-219">`OriginalValue`DB 内の値と`FirstOrDefaultAsync`がこのメソッドで呼び出されました。</span><span class="sxs-lookup"><span data-stu-id="e97f3-219">`OriginalValue` is the value in the DB when `FirstOrDefaultAsync` was called in this method.</span></span>

<span data-ttu-id="e97f3-220">次のコードでは、クライアントの値 (このメソッドにポストされた値)、および DB の値を取得します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-220">The following code gets the client values (the values posted to this method) and the DB values:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

<span data-ttu-id="e97f3-221">次のコードは、DB のある各列の値にポストされた内容と異なるためにカスタム エラー メッセージを追加`OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="e97f3-221">The follwing code adds a custom error message for each column that has DB values different from what was posted to `OnPostAsync`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

<span data-ttu-id="e97f3-222">次にコード セットが強調表示されて、 `RowVersion` DB から新しい値に値を取得します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-222">The following highlighted code sets the `RowVersion` value to the new value retrieved from the DB.</span></span> <span data-ttu-id="e97f3-223">次に、ユーザーがクリックしたとき**保存**、最後の編集 ページの表示が検出されるために発生する同時実行エラーのみです。</span><span class="sxs-lookup"><span data-stu-id="e97f3-223">The next time the user clicks **Save**, only concurrency errors that happen since the last display of the Edit page will be caught.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

<span data-ttu-id="e97f3-224">`ModelState.Remove`ために、ステートメントは必要な`ModelState`が古い`RowVersion`値。</span><span class="sxs-lookup"><span data-stu-id="e97f3-224">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="e97f3-225">Razor ページで、`ModelState`フィールドよりも優先、モデル プロパティの値が両方に存在する場合の値します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-225">In the Razor Page, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="e97f3-226">更新プログラムの編集 ページ</span><span class="sxs-lookup"><span data-stu-id="e97f3-226">Update the Edit page</span></span>

<span data-ttu-id="e97f3-227">更新*Pages/Departments/Edit.cshtml*次のマークアップ。</span><span class="sxs-lookup"><span data-stu-id="e97f3-227">Update *Pages/Departments/Edit.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

<span data-ttu-id="e97f3-228">上記のマークアップ。</span><span class="sxs-lookup"><span data-stu-id="e97f3-228">The preceding markup:</span></span>

* <span data-ttu-id="e97f3-229">更新プログラム、`page`からディレクティブ`@page`に`@page "{id:int}"`です。</span><span class="sxs-lookup"><span data-stu-id="e97f3-229">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="e97f3-230">非表示の行バージョンを追加します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-230">Adds a hidden row version.</span></span> <span data-ttu-id="e97f3-231">`RowVersion`ポスト バック値をバインドするために追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e97f3-231">`RowVersion` must be added so post back binds the value.</span></span>
* <span data-ttu-id="e97f3-232">最後のバイトを表示`RowVersion`デバッグのためにします。</span><span class="sxs-lookup"><span data-stu-id="e97f3-232">Displays the last byte of `RowVersion` for debugging purposes.</span></span>
* <span data-ttu-id="e97f3-233">置換`ViewData`厳密に型指定と`InstructorNameSL`です。</span><span class="sxs-lookup"><span data-stu-id="e97f3-233">Replaces `ViewData` with the strongly-typed `InstructorNameSL`.</span></span>

## <a name="test-concurrency-conflicts-with-the-edit-page"></a><span data-ttu-id="e97f3-234">テストの編集 ページで、同時実行の競合</span><span class="sxs-lookup"><span data-stu-id="e97f3-234">Test concurrency conflicts with the Edit page</span></span>

<span data-ttu-id="e97f3-235">英語版の部門の編集の 2 つのブラウザー インスタンスを開きます。</span><span class="sxs-lookup"><span data-stu-id="e97f3-235">Open two browsers instances of Edit on the English department:</span></span>

* <span data-ttu-id="e97f3-236">アプリを実行し、部門を選択します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-236">Run the app and select Departments.</span></span>
* <span data-ttu-id="e97f3-237">右クリックし、**編集**ハイパーリンクをクリックし、英語部門**新しいタブで開く**です。</span><span class="sxs-lookup"><span data-stu-id="e97f3-237">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**.</span></span>
* <span data-ttu-id="e97f3-238">最初のタブをクリックして、**編集**英語部門のハイパーリンクのです。</span><span class="sxs-lookup"><span data-stu-id="e97f3-238">In the first tab, click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="e97f3-239">2 つのブラウザー タブでは、同じ情報を表示します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-239">The two browser tabs display the same information.</span></span>

<span data-ttu-id="e97f3-240">最初のブラウザー タブで名前を変更し、をクリックして**保存**です。</span><span class="sxs-lookup"><span data-stu-id="e97f3-240">Change the name in the first browser tab and click **Save**.</span></span>

![部門編集変更後のページ 1](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="e97f3-242">ブラウザーでは、変更された値と更新された rowVersion インジケーターのインデックス ページを示します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-242">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="e97f3-243">更新された rowVersion インジケーターを注意してください。 その他 タブで 2 つ目のポストバック時に表示されます。</span><span class="sxs-lookup"><span data-stu-id="e97f3-243">Note the updated rowVersion indicator, it is displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="e97f3-244">2 番目のブラウザー タブで、別のフィールドを変更します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-244">Change a different field in the second browser tab.</span></span>

![部門編集変更した後に 2 ページ目](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="e97f3-246">**[保存]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e97f3-246">Click **Save**.</span></span> <span data-ttu-id="e97f3-247">DB 値と一致しないすべてのフィールドのエラー メッセージを参照してください。</span><span class="sxs-lookup"><span data-stu-id="e97f3-247">You see error messages for all fields that don't match the DB values:</span></span>

![部門の編集 ページのエラー メッセージ](concurrency/_static/edit-error.png)

<span data-ttu-id="e97f3-249">このブラウザー ウィンドウを Name フィールドを変更する意図しません。</span><span class="sxs-lookup"><span data-stu-id="e97f3-249">This browser window did not intend to change the Name field.</span></span> <span data-ttu-id="e97f3-250">コピーし、[名前] フィールドに、現在の値 (言語) を貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="e97f3-250">Copy and paste the current value (Languages) into the Name field.</span></span> <span data-ttu-id="e97f3-251">タブです。クライアント側の検証では、エラー メッセージを削除します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-251">Tab out. Client-side validation removes the error message.</span></span>

![部門の編集 ページのエラー メッセージ](concurrency/_static/cv.png)

<span data-ttu-id="e97f3-253">をクリックして**保存**もう一度です。</span><span class="sxs-lookup"><span data-stu-id="e97f3-253">Click **Save** again.</span></span> <span data-ttu-id="e97f3-254">2 番目のブラウザー タブで入力した値は保存されます。</span><span class="sxs-lookup"><span data-stu-id="e97f3-254">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="e97f3-255">インデックス ページ内に保存された値が参照してください。</span><span class="sxs-lookup"><span data-stu-id="e97f3-255">You see the saved values in the Index page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="e97f3-256">更新プログラムの削除 ページ</span><span class="sxs-lookup"><span data-stu-id="e97f3-256">Update the Delete page</span></span>

<span data-ttu-id="e97f3-257">次のコードで Delete ページ モデルを更新します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-257">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

<span data-ttu-id="e97f3-258">ページの削除 は、エンティティは、フェッチ後に変更されたときに同時実行の競合を検出します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-258">The Delete page detects concurrency conflicts when the entity has changed after it was fetched.</span></span> <span data-ttu-id="e97f3-259">`Department.RowVersion`エンティティがフェッチしたときに、行バージョンです。</span><span class="sxs-lookup"><span data-stu-id="e97f3-259">`Department.RowVersion` is the row version when the entity was fetched.</span></span> <span data-ttu-id="e97f3-260">EF コアは、SQL の DELETE コマンドを作成するときに WHERE 句が含まれます`RowVersion`です。</span><span class="sxs-lookup"><span data-stu-id="e97f3-260">When EF Core creates the SQL DELETE command, it includes a WHERE clause with `RowVersion`.</span></span> <span data-ttu-id="e97f3-261">場合は、SQL の DELETE コマンドの結果行は 0 行で影響を受けます。</span><span class="sxs-lookup"><span data-stu-id="e97f3-261">If the SQL DELETE command results in zero rows affected:</span></span>

* <span data-ttu-id="e97f3-262">`RowVersion` SQL の DELETE でコマンドが一致しない`RowVersion`DB にします。</span><span class="sxs-lookup"><span data-stu-id="e97f3-262">The `RowVersion` in the SQL DELETE command doesn't match `RowVersion` in the DB.</span></span>
* <span data-ttu-id="e97f3-263">DbUpdateConcurrencyException 例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="e97f3-263">A DbUpdateConcurrencyException exception is thrown.</span></span>
* <span data-ttu-id="e97f3-264">`OnGetAsync`使用が呼び出される、`concurrencyError`です。</span><span class="sxs-lookup"><span data-stu-id="e97f3-264">`OnGetAsync` is called with the `concurrencyError`.</span></span>

### <a name="update-the-delete-page"></a><span data-ttu-id="e97f3-265">更新プログラムの削除 ページ</span><span class="sxs-lookup"><span data-stu-id="e97f3-265">Update the Delete page</span></span>

<span data-ttu-id="e97f3-266">更新*Pages/Departments/Delete.cshtml*を次のコード。</span><span class="sxs-lookup"><span data-stu-id="e97f3-266">Update *Pages/Departments/Delete.cshtml* with the following code:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,36,51)]


<span data-ttu-id="e97f3-267">上記のマークアップでは、次の変更を加えます。</span><span class="sxs-lookup"><span data-stu-id="e97f3-267">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="e97f3-268">更新プログラム、`page`からディレクティブ`@page`に`@page "{id:int}"`です。</span><span class="sxs-lookup"><span data-stu-id="e97f3-268">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="e97f3-269">エラー メッセージを追加します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-269">Adds an error message.</span></span>
* <span data-ttu-id="e97f3-270">置き換えますで FullName FirstMidName、**管理者**フィールドです。</span><span class="sxs-lookup"><span data-stu-id="e97f3-270">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>
* <span data-ttu-id="e97f3-271">変更`RowVersion`を最後のバイトを表示します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-271">Changes `RowVersion` to display the last byte.</span></span>
* <span data-ttu-id="e97f3-272">非表示の行バージョンを追加します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-272">Adds a hidden row version.</span></span> <span data-ttu-id="e97f3-273">`RowVersion`ポスト バック値をバインドするために追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e97f3-273">`RowVersion` must be added so post back binds the value.</span></span>

### <a name="test-concurrency-conflicts-with-the-delete-page"></a><span data-ttu-id="e97f3-274">テストの削除 ページで、同時実行の競合</span><span class="sxs-lookup"><span data-stu-id="e97f3-274">Test concurrency conflicts with the Delete page</span></span>

<span data-ttu-id="e97f3-275">テストの部門を作成します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-275">Create a test department.</span></span>

<span data-ttu-id="e97f3-276">テスト部門の削除の 2 つのブラウザー インスタンスを開きます。</span><span class="sxs-lookup"><span data-stu-id="e97f3-276">Open two browsers instances of Delete on the test department:</span></span>

* <span data-ttu-id="e97f3-277">アプリを実行し、部門を選択します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-277">Run the app and select Departments.</span></span>
* <span data-ttu-id="e97f3-278">右クリックし、**削除**ハイパーリンクを選択してテスト部門**新しいタブで開く**です。</span><span class="sxs-lookup"><span data-stu-id="e97f3-278">Right-click the **Delete** hyperlink for the test department and select **Open in new tab**.</span></span>
* <span data-ttu-id="e97f3-279">クリックして、**編集**テスト部門のハイパーリンクのです。</span><span class="sxs-lookup"><span data-stu-id="e97f3-279">Click the **Edit** hyperlink for the test department.</span></span>

<span data-ttu-id="e97f3-280">2 つのブラウザー タブでは、同じ情報を表示します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-280">The two browser tabs display the same information.</span></span>

<span data-ttu-id="e97f3-281">最初のブラウザー タブで、予算を変更し、クリックして**保存**です。</span><span class="sxs-lookup"><span data-stu-id="e97f3-281">Change the budget in the first browser tab and click **Save**.</span></span>

<span data-ttu-id="e97f3-282">ブラウザーでは、変更された値と更新された rowVersion インジケーターのインデックス ページを示します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-282">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="e97f3-283">更新された rowVersion インジケーターを注意してください。 その他 タブで 2 つ目のポストバック時に表示されます。</span><span class="sxs-lookup"><span data-stu-id="e97f3-283">Note the updated rowVersion indicator, it is displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="e97f3-284">2 番目のタブから、テストの部門を削除します。同時実行エラーは、DB から現在の値で表示します。</span><span class="sxs-lookup"><span data-stu-id="e97f3-284">Delete the test department from the second tab. A concurrency error is display with the current values from the DB.</span></span> <span data-ttu-id="e97f3-285">クリックすると**削除**、エンティティを削除しない限り、 `RowVersion` updated.department が削除されたされました。</span><span class="sxs-lookup"><span data-stu-id="e97f3-285">Clicking **Delete** deletes the entity, unless `RowVersion` has been updated.department has been deleted.</span></span>

<span data-ttu-id="e97f3-286">参照してください[継承](xref:data/ef-mvc/inheritance)データ モデルを継承する方法にします。</span><span class="sxs-lookup"><span data-stu-id="e97f3-286">See [Inheritance](xref:data/ef-mvc/inheritance) on how to inherit a data model.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="e97f3-287">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="e97f3-287">Additional resources</span></span>

* [<span data-ttu-id="e97f3-288">EF Core での同時実行トークン</span><span class="sxs-lookup"><span data-stu-id="e97f3-288">Concurrency Tokens in EF Core</span></span>](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency)
* [<span data-ttu-id="e97f3-289">EF Core での同時実行の処理</span><span class="sxs-lookup"><span data-stu-id="e97f3-289">Handling concurrency in EF Core</span></span>](https://docs.microsoft.com/en-us/ef/core/saving/concurrency)

>[!div class="step-by-step"]
[<span data-ttu-id="e97f3-290">前へ</span><span class="sxs-lookup"><span data-stu-id="e97f3-290">Previous</span></span>](xref:data/ef-rp/update-related-data)
