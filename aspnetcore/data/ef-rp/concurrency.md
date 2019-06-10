---
title: ASP.NET Core の Razor ページと EF Core - コンカレンシー - 8/8
author: rick-anderson
description: このチュートリアルでは、複数のユーザーが同じエンティティを同時に更新するときの競合の処理方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2019
uid: data/ef-rp/concurrency
ms.openlocfilehash: 8430f8e720870a7b541655ea8bcfe2f67c942bb3
ms.sourcegitcommit: c5339594101d30b189f61761275b7d310e80d18a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/02/2019
ms.locfileid: "66458427"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---concurrency---8-of-8"></a><span data-ttu-id="803df-103">ASP.NET Core の Razor ページと EF Core - コンカレンシー - 8/8</span><span class="sxs-lookup"><span data-stu-id="803df-103">Razor Pages with EF Core in ASP.NET Core - Concurrency - 8 of 8</span></span>

<span data-ttu-id="803df-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)、[Tom Dykstra](https://github.com/tdykstra)、[Jon P Smith](https://twitter.com/thereformedprog)</span><span class="sxs-lookup"><span data-stu-id="803df-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), and [Jon P Smith](https://twitter.com/thereformedprog)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="803df-105">このチュートリアルでは、複数のユーザーがエンティティをコンカレントに更新するときの競合の処理方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="803df-105">This tutorial shows how to handle conflicts when multiple users update an entity concurrently (at the same time).</span></span> <span data-ttu-id="803df-106">解決できない問題が発生した場合は、[完成したアプリをダウンロードまたは表示](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)してください。</span><span class="sxs-lookup"><span data-stu-id="803df-106">If you run into problems you can't solve, [download or view the completed app.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span></span> <span data-ttu-id="803df-107">[ダウンロードの方法はこちらをご覧ください。](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="803df-107">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

## <a name="concurrency-conflicts"></a><span data-ttu-id="803df-108">コンカレンシーの競合</span><span class="sxs-lookup"><span data-stu-id="803df-108">Concurrency conflicts</span></span>

<span data-ttu-id="803df-109">コンカレンシーの競合は、次のような場合に発生します。</span><span class="sxs-lookup"><span data-stu-id="803df-109">A concurrency conflict occurs when:</span></span>

* <span data-ttu-id="803df-110">ユーザーがエンティティの Edit ページに移動した場合。</span><span class="sxs-lookup"><span data-stu-id="803df-110">A user navigates to the edit page for an entity.</span></span>
* <span data-ttu-id="803df-111">最初のユーザーの変更がデータベースに書き込まれる前に、別のユーザーが同じエンティティを更新した場合。</span><span class="sxs-lookup"><span data-stu-id="803df-111">Another user updates the same entity before the first user's change is written to the DB.</span></span>

<span data-ttu-id="803df-112">コンカレンシーの検出が無効のとき、コンカレント更新が発生すると、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="803df-112">If concurrency detection isn't enabled, when concurrent updates occur:</span></span>

* <span data-ttu-id="803df-113">最後の更新が有効になります。</span><span class="sxs-lookup"><span data-stu-id="803df-113">The last update wins.</span></span> <span data-ttu-id="803df-114">つまり、最後に更新された値がデータベースに保存されます。</span><span class="sxs-lookup"><span data-stu-id="803df-114">That is, the last update values are saved to the DB.</span></span>
* <span data-ttu-id="803df-115">現在の更新の最初のものは失われます。</span><span class="sxs-lookup"><span data-stu-id="803df-115">The first of the current updates are lost.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="803df-116">オプティミスティック コンカレンシー</span><span class="sxs-lookup"><span data-stu-id="803df-116">Optimistic concurrency</span></span>

<span data-ttu-id="803df-117">オプティミスティック コンカレンシーでは、コンカレンシーの競合の発生が許可され、それが発生した場合に適切に対処されます。</span><span class="sxs-lookup"><span data-stu-id="803df-117">Optimistic concurrency allows concurrency conflicts to happen, and then reacts appropriately when they do.</span></span> <span data-ttu-id="803df-118">たとえば、Jane が Department Edit ページにアクセスし、English 部署の予算を $350,000.00 から $0.00 に変更するとします。</span><span class="sxs-lookup"><span data-stu-id="803df-118">For example, Jane visits the Department edit page and changes the budget for the English department from $350,000.00 to $0.00.</span></span>

![予算を 0 に変更する](concurrency/_static/change-budget.png)

<span data-ttu-id="803df-120">Jane が **[保存]** をクリックする前に John が同じページにアクセスし、[開始日] フィールドを 9/1/2007 から 9/1/2013 に変更します。</span><span class="sxs-lookup"><span data-stu-id="803df-120">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![開始日を 2013 に変更する](concurrency/_static/change-date.png)

<span data-ttu-id="803df-122">Jane が **[保存]** を先にクリックすると、ブラウザーの Index ページには、Jane の変更が反映されています。</span><span class="sxs-lookup"><span data-stu-id="803df-122">Jane clicks **Save** first and sees her change when the browser displays the Index page.</span></span>

![予算が 0 に変更された](concurrency/_static/budget-zero.png)

<span data-ttu-id="803df-124">John が Edit ページの **[保存]** をクリックします。このとき、予算は $350,000.00 と表示されています。</span><span class="sxs-lookup"><span data-stu-id="803df-124">John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="803df-125">この後の動作は、コンカレンシーの競合の処理方法によって決定します。</span><span class="sxs-lookup"><span data-stu-id="803df-125">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="803df-126">オプティミスティック コンカレンシーには、次のオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="803df-126">Optimistic concurrency includes the following options:</span></span>

* <span data-ttu-id="803df-127">ユーザーが変更したプロパティを追跡記録し、それに該当する列だけをデータベースで更新します。</span><span class="sxs-lookup"><span data-stu-id="803df-127">You can keep track of which property a user has modified and update only the corresponding columns in the DB.</span></span>

  <span data-ttu-id="803df-128">このシナリオでは、データは失われません。</span><span class="sxs-lookup"><span data-stu-id="803df-128">In the scenario, no data would be lost.</span></span> <span data-ttu-id="803df-129">2 人のユーザーが別のプロパティを更新しました。</span><span class="sxs-lookup"><span data-stu-id="803df-129">Different properties were updated by the two users.</span></span> <span data-ttu-id="803df-130">今度誰かが English 部署を閲覧すると、Jane の変更と John の変更が両方とも表示されます。</span><span class="sxs-lookup"><span data-stu-id="803df-130">The next time someone browses the English department, they will see both Jane's and John's changes.</span></span> <span data-ttu-id="803df-131">この更新方法では、データが失われる原因となる競合の数を減らすことができます。</span><span class="sxs-lookup"><span data-stu-id="803df-131">This method of updating can reduce the number of conflicts that could result in data loss.</span></span> <span data-ttu-id="803df-132">この方法の特徴は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="803df-132">This approach:</span></span>
 
  * <span data-ttu-id="803df-133">競合する変更が同じプロパティに加えられた場合、データの損失を回避することはできません。</span><span class="sxs-lookup"><span data-stu-id="803df-133">Can't avoid data loss if competing changes are made to the same property.</span></span>
  * <span data-ttu-id="803df-134">Web アプリでは、一般的に実用的ではありません。</span><span class="sxs-lookup"><span data-stu-id="803df-134">Is generally not practical in a web app.</span></span> <span data-ttu-id="803df-135">フェッチされたすべての値と新しい値を追跡するために、かなりのステータスを維持することが必要になります。</span><span class="sxs-lookup"><span data-stu-id="803df-135">It requires maintaining significant state in order to keep track of all fetched values and new values.</span></span> <span data-ttu-id="803df-136">大量のステータスを保守管理することになると、アプリケーションのパフォーマンスに影響が出ます。</span><span class="sxs-lookup"><span data-stu-id="803df-136">Maintaining large amounts of state can affect app performance.</span></span>
  * <span data-ttu-id="803df-137">エンティティでのコンカレンシーの検出と比較して、アプリは複雑になります。</span><span class="sxs-lookup"><span data-stu-id="803df-137">Can increase app complexity compared to concurrency detection on an entity.</span></span>

* <span data-ttu-id="803df-138">John の変更で Jane の変更を上書きするように指定できます。</span><span class="sxs-lookup"><span data-stu-id="803df-138">You can let John's change overwrite Jane's change.</span></span>

  <span data-ttu-id="803df-139">今度誰かが English 部署を閲覧すると、9/1/2013 の日付と、フェッチされた $350,000.00 の値が表示されます。</span><span class="sxs-lookup"><span data-stu-id="803df-139">The next time someone browses the English department, they will see 9/1/2013 and the fetched $350,000.00 value.</span></span> <span data-ttu-id="803df-140">このアプローチは *Client Wins* (クライアント側に合わせる) シナリオまたは *Last in Wins* (最終書き込み者優先) シナリオと呼ばれています。</span><span class="sxs-lookup"><span data-stu-id="803df-140">This approach is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="803df-141">(クライアントからの値がすべて、データ ストアの値より優先されます。)コンカレンシー処理に対してコーディングを行わない場合、自動的にクライアント側に合わせられます。</span><span class="sxs-lookup"><span data-stu-id="803df-141">(All values from the client take precedence over what's in the data store.) If you don't do any coding for concurrency handling, Client Wins happens automatically.</span></span>

* <span data-ttu-id="803df-142">データベースで John の変更が更新されないようにできます。</span><span class="sxs-lookup"><span data-stu-id="803df-142">You can prevent John's change from being updated in the DB.</span></span> <span data-ttu-id="803df-143">通常、アプリの動作は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="803df-143">Typically, the app would:</span></span>

  * <span data-ttu-id="803df-144">エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="803df-144">Display an error message.</span></span>
  * <span data-ttu-id="803df-145">データの現在のステータスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="803df-145">Show the current state of the data.</span></span>
  * <span data-ttu-id="803df-146">ユーザーが変更を再適用できるようになります。</span><span class="sxs-lookup"><span data-stu-id="803df-146">Allow the user to reapply the changes.</span></span>

  <span data-ttu-id="803df-147">これは *Store Wins* (ストア側に合わせる) シナリオと呼ばれています。</span><span class="sxs-lookup"><span data-stu-id="803df-147">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="803df-148">(クライアントが送信した値よりデータストアの値が優先されます。)このチュートリアルでは、Store Wins シナリオを実装します。</span><span class="sxs-lookup"><span data-stu-id="803df-148">(The data-store values take precedence over the values submitted by the client.) You implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="803df-149">この手法では、変更が上書きされるとき、それが必ずユーザーに警告されます。</span><span class="sxs-lookup"><span data-stu-id="803df-149">This method ensures that no changes are overwritten without a user being alerted.</span></span>

## <a name="handling-concurrency"></a><span data-ttu-id="803df-150">コンカレンシーの処理</span><span class="sxs-lookup"><span data-stu-id="803df-150">Handling concurrency</span></span> 

<span data-ttu-id="803df-151">プロパティが[コンカレンシー トークン](/ef/core/modeling/concurrency)として構成されている場合、次が実行されます。</span><span class="sxs-lookup"><span data-stu-id="803df-151">When a property is configured as a [concurrency token](/ef/core/modeling/concurrency):</span></span>

* <span data-ttu-id="803df-152">EF Core によって、フェッチ後にプロパティが変更されていないことが確認されます。</span><span class="sxs-lookup"><span data-stu-id="803df-152">EF Core verifies that property has not been modified after it was fetched.</span></span> <span data-ttu-id="803df-153">このチェックは、[SaveChanges](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) または [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) が呼び出されたときに発生します。</span><span class="sxs-lookup"><span data-stu-id="803df-153">The check occurs when [SaveChanges](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) or [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) is called.</span></span>
* <span data-ttu-id="803df-154">フェッチ後にプロパティが変更されていると、[DbUpdateConcurrencyException](/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) がスローされます。</span><span class="sxs-lookup"><span data-stu-id="803df-154">If the property has been changed after it was fetched, a [DbUpdateConcurrencyException](/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) is thrown.</span></span> 

<span data-ttu-id="803df-155">データベースとデータ モデルは、`DbUpdateConcurrencyException` のスローをサポートするように構成される必要があります。</span><span class="sxs-lookup"><span data-stu-id="803df-155">The DB and data model must be configured to support throwing `DbUpdateConcurrencyException`.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-property"></a><span data-ttu-id="803df-156">プロパティのコンカレンシーの競合の検出</span><span class="sxs-lookup"><span data-stu-id="803df-156">Detecting concurrency conflicts on a property</span></span>

<span data-ttu-id="803df-157">コンカレンシーの競合は、[ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) 属性を使用し、プロパティ レベルで検出できます。</span><span class="sxs-lookup"><span data-stu-id="803df-157">Concurrency conflicts can be detected at the property level with the [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) attribute.</span></span> <span data-ttu-id="803df-158">この属性は、モデルの複数のプロパティに適用できます。</span><span class="sxs-lookup"><span data-stu-id="803df-158">The attribute can be applied to multiple properties on the model.</span></span> <span data-ttu-id="803df-159">詳細については、「[データの注釈 - ConcurrencyCheck](/ef/core/modeling/concurrency#data-annotations)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="803df-159">For more information, see [Data Annotations-ConcurrencyCheck](/ef/core/modeling/concurrency#data-annotations).</span></span>

<span data-ttu-id="803df-160">このチュートリアルでは、`[ConcurrencyCheck]` 属性は使用しません。</span><span class="sxs-lookup"><span data-stu-id="803df-160">The `[ConcurrencyCheck]` attribute isn't used in this tutorial.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-row"></a><span data-ttu-id="803df-161">行のコンカレンシーの競合の検出</span><span class="sxs-lookup"><span data-stu-id="803df-161">Detecting concurrency conflicts on a row</span></span>

<span data-ttu-id="803df-162">コンカレンシーの競合を検出するために、モデルに [rowversion](/sql/t-sql/data-types/rowversion-transact-sql) 追跡列が追加されました。</span><span class="sxs-lookup"><span data-stu-id="803df-162">To detect concurrency conflicts, a [rowversion](/sql/t-sql/data-types/rowversion-transact-sql) tracking column is added to the model.</span></span>  <span data-ttu-id="803df-163">`rowversion` は、次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="803df-163">`rowversion` :</span></span>

* <span data-ttu-id="803df-164">SQL Server 専用です。</span><span class="sxs-lookup"><span data-stu-id="803df-164">Is SQL Server specific.</span></span> <span data-ttu-id="803df-165">他のデータベースには、似たような機能がない場合があります。</span><span class="sxs-lookup"><span data-stu-id="803df-165">Other databases may not provide a similar feature.</span></span>
* <span data-ttu-id="803df-166">データベースからフェッチされた以降、エンティティに変更がないことを決定するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="803df-166">Is used to determine that an entity has not been changed since it was fetched from the DB.</span></span> 

<span data-ttu-id="803df-167">データベースによって、行が更新されるたびに増える、連続する `rowversion` 番号値が生成されます。</span><span class="sxs-lookup"><span data-stu-id="803df-167">The DB generates a sequential `rowversion` number that's incremented each time the row is updated.</span></span> <span data-ttu-id="803df-168">`Update` または `Delete` コマンドの `Where` 句には、フェッチされた `rowversion` の値が含まれます。</span><span class="sxs-lookup"><span data-stu-id="803df-168">In an `Update` or `Delete` command, the `Where` clause includes the fetched value of `rowversion`.</span></span> <span data-ttu-id="803df-169">更新された行が変更された場合、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="803df-169">If the row being updated has changed:</span></span>

* <span data-ttu-id="803df-170">`rowversion` がフェッチされた値と一致しなくなります。</span><span class="sxs-lookup"><span data-stu-id="803df-170">`rowversion` doesn't match the fetched value.</span></span>
* <span data-ttu-id="803df-171">`Where` 句にフェッチされた `rowversion` が含まれるので、`Update` または `Delete` コマンドでは行が検索されません。</span><span class="sxs-lookup"><span data-stu-id="803df-171">The `Update` or `Delete` commands don't find a row because the `Where` clause includes the fetched `rowversion`.</span></span>
* <span data-ttu-id="803df-172">`DbUpdateConcurrencyException` がスローされます。</span><span class="sxs-lookup"><span data-stu-id="803df-172">A `DbUpdateConcurrencyException` is thrown.</span></span>

<span data-ttu-id="803df-173">EF Core では、`Update` または `Delete` コマンドで行が更新されない場合、コンカレンシー競合がスローされます。</span><span class="sxs-lookup"><span data-stu-id="803df-173">In EF Core, when no rows have been updated by an `Update` or `Delete` command, a concurrency exception is thrown.</span></span>

### <a name="add-a-tracking-property-to-the-department-entity"></a><span data-ttu-id="803df-174">Department エンティティにトラッキング プロパティを追加する</span><span class="sxs-lookup"><span data-stu-id="803df-174">Add a tracking property to the Department entity</span></span>

<span data-ttu-id="803df-175">*Models/Department.cs* で、RowVersion という名前のトラッキング プロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="803df-175">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

<span data-ttu-id="803df-176">[Timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) 属性では、この列は、`Update` および `Delete` コマンドの `Where` 句に含まれることを指定します。</span><span class="sxs-lookup"><span data-stu-id="803df-176">The [Timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) attribute specifies that this column is included in the `Where` clause of `Update` and `Delete` commands.</span></span> <span data-ttu-id="803df-177">前のバージョンの SQL Server では、SQL `rowversion` 型に取って代わられる前、SQL `timestamp` というデータ型が使用されていたため、この属性は `Timestamp` と呼ばれています。</span><span class="sxs-lookup"><span data-stu-id="803df-177">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` type replaced it.</span></span>

<span data-ttu-id="803df-178">Fluent API でも、トラッキング プロパティを指定できます。</span><span class="sxs-lookup"><span data-stu-id="803df-178">The fluent API can also specify the tracking property:</span></span>

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

<span data-ttu-id="803df-179">次のコードは、Department 名が更新されたときに、EF Core によって生成された T-SQL ステートメントの一部です。</span><span class="sxs-lookup"><span data-stu-id="803df-179">The following code shows a portion of the T-SQL generated by EF Core when the Department name is updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

<span data-ttu-id="803df-180">上の強調表示されたコードには、`RowVersion` を含む `WHERE` 句があります。</span><span class="sxs-lookup"><span data-stu-id="803df-180">The preceding highlighted code shows the `WHERE` clause containing `RowVersion`.</span></span> <span data-ttu-id="803df-181">データベースの `RowVersion` が `RowVersion` パラメーター (`@p2`) と一致しない場合、行は更新されません。</span><span class="sxs-lookup"><span data-stu-id="803df-181">If the DB `RowVersion` doesn't equal the `RowVersion` parameter (`@p2`), no rows are updated.</span></span>

<span data-ttu-id="803df-182">次の強調表示されたコードは、1 つの行のみが更新されたことを検証する T-SQL です。</span><span class="sxs-lookup"><span data-stu-id="803df-182">The following highlighted code shows the T-SQL that verifies exactly one row was updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

<span data-ttu-id="803df-183">[@@ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) では、最後のステートメントの影響を受けた行数を返します。</span><span class="sxs-lookup"><span data-stu-id="803df-183">[@@ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) returns the number of rows affected by the last statement.</span></span> <span data-ttu-id="803df-184">更新された行がない場合、EF Core は `DbUpdateConcurrencyException` をスローします。</span><span class="sxs-lookup"><span data-stu-id="803df-184">In no rows are updated, EF Core throws a `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="803df-185">Visual Studio の出力ウィンドウでは、EF Core が生成する T-SQL を確認できます。</span><span class="sxs-lookup"><span data-stu-id="803df-185">You can see the T-SQL EF Core generates in the output window of Visual Studio.</span></span>

### <a name="update-the-db"></a><span data-ttu-id="803df-186">データベースの更新</span><span class="sxs-lookup"><span data-stu-id="803df-186">Update the DB</span></span>

<span data-ttu-id="803df-187">`RowVersion` プロパティを追加すると、移行を必要とするデータベース モデルが変更されます。</span><span class="sxs-lookup"><span data-stu-id="803df-187">Adding the `RowVersion` property changes the DB model, which requires a migration.</span></span>

<span data-ttu-id="803df-188">プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="803df-188">Build the project.</span></span> <span data-ttu-id="803df-189">コマンド ウィンドウに次を入力します。</span><span class="sxs-lookup"><span data-stu-id="803df-189">Enter the following in a command window:</span></span>

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

<span data-ttu-id="803df-190">上のコマンドでは以下の操作が行われます。</span><span class="sxs-lookup"><span data-stu-id="803df-190">The preceding commands:</span></span>

* <span data-ttu-id="803df-191">*Migrations/{time stamp}_RowVersion.cs* 移行ファイルが追加されます。</span><span class="sxs-lookup"><span data-stu-id="803df-191">Adds the *Migrations/{time stamp}_RowVersion.cs* migration file.</span></span>
* <span data-ttu-id="803df-192">*Migrations/SchoolContextModelSnapshot.cs* ファイルが更新されます。</span><span class="sxs-lookup"><span data-stu-id="803df-192">Updates the *Migrations/SchoolContextModelSnapshot.cs* file.</span></span> <span data-ttu-id="803df-193">更新により、次の強調表示されたコードが `BuildModel` メソッドに追加されます。</span><span class="sxs-lookup"><span data-stu-id="803df-193">The update adds the following highlighted code to the `BuildModel` method:</span></span>

  [!code-csharp[](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* <span data-ttu-id="803df-194">データベースを更新するために移行が実行されます。</span><span class="sxs-lookup"><span data-stu-id="803df-194">Runs migrations to update the DB.</span></span>

<a name="scaffold"></a>

## <a name="scaffold-the-departments-model"></a><span data-ttu-id="803df-195">部署モデルのスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="803df-195">Scaffold the Departments model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="803df-196">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="803df-196">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="803df-197">「[Student モデルをスキャホールディングする](xref:data/ef-rp/intro#scaffold-the-student-model)」の手順に従い、モデル クラスの `Department` を使用します。</span><span class="sxs-lookup"><span data-stu-id="803df-197">Follow the instructions in [Scaffold the student model](xref:data/ef-rp/intro#scaffold-the-student-model) and use `Department` for the model class.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="803df-198">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="803df-198">.NET Core CLI</span></span>](#tab/netcore-cli)

 <span data-ttu-id="803df-199">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="803df-199">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
  ```

---

<span data-ttu-id="803df-200">上記のコマンドは、`Department` モデルをスキャフォールディングします。</span><span class="sxs-lookup"><span data-stu-id="803df-200">The preceding command scaffolds the `Department` model.</span></span> <span data-ttu-id="803df-201">Visual Studio でプロジェクトを開きます。</span><span class="sxs-lookup"><span data-stu-id="803df-201">Open the project in Visual Studio.</span></span>

<span data-ttu-id="803df-202">プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="803df-202">Build the project.</span></span>

### <a name="update-the-departments-index-page"></a><span data-ttu-id="803df-203">Departments Index ページを更新する</span><span class="sxs-lookup"><span data-stu-id="803df-203">Update the Departments Index page</span></span>

<span data-ttu-id="803df-204">スキャフォールディング エンジンにより Index ページに `RowVersion` 列が作成されましたが、このフィールドは表示すべきではありません。</span><span class="sxs-lookup"><span data-stu-id="803df-204">The scaffolding engine created a `RowVersion` column for the Index page, but that field shouldn't be displayed.</span></span> <span data-ttu-id="803df-205">このチュートリアルでは、コンカレンシーを理解するために、`RowVersion` の最後のバイトが表示されています。</span><span class="sxs-lookup"><span data-stu-id="803df-205">In this tutorial, the last byte of the `RowVersion` is displayed to help understand concurrency.</span></span> <span data-ttu-id="803df-206">最後のバイトは、一意であるとは限りません。</span><span class="sxs-lookup"><span data-stu-id="803df-206">The last byte isn't guaranteed to be unique.</span></span> <span data-ttu-id="803df-207">実際のアプリでは、`RowVersion` や `RowVersion` の最後のバイトは表示されません。</span><span class="sxs-lookup"><span data-stu-id="803df-207">A real app wouldn't display `RowVersion` or the last byte of `RowVersion`.</span></span>

<span data-ttu-id="803df-208">Index ページを更新するために、次を実行します。</span><span class="sxs-lookup"><span data-stu-id="803df-208">Update the Index page:</span></span>

* <span data-ttu-id="803df-209">Department で Index を置き換えます。</span><span class="sxs-lookup"><span data-stu-id="803df-209">Replace Index with Departments.</span></span>
* <span data-ttu-id="803df-210">`RowVersion` の最後のバイトで、`RowVersion` を含むマークアップを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="803df-210">Replace the markup containing `RowVersion` with the last byte of `RowVersion`.</span></span>
* <span data-ttu-id="803df-211">FirstMidName を FullName で置き換えます。</span><span class="sxs-lookup"><span data-stu-id="803df-211">Replace FirstMidName with FullName.</span></span>

<span data-ttu-id="803df-212">次のマークアップは、更新されたページを示しています。</span><span class="sxs-lookup"><span data-stu-id="803df-212">The following markup shows the updated page:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a><span data-ttu-id="803df-213">Edit ページ モデルの更新</span><span class="sxs-lookup"><span data-stu-id="803df-213">Update the Edit page model</span></span>

<span data-ttu-id="803df-214">次のコードで *Pages\Departments\Edit.cshtml.cs* を更新します。</span><span class="sxs-lookup"><span data-stu-id="803df-214">Update *Pages\Departments\Edit.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="803df-215">コンカレンシーの問題を検出するために、フェッチされたエンティティの [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) が `rowVersion` 値で更新されます。</span><span class="sxs-lookup"><span data-stu-id="803df-215">To detect a concurrency issue, the [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) is updated with the `rowVersion` value from the entity it was fetched.</span></span> <span data-ttu-id="803df-216">EF Core では、WHERE 句に元の `RowVersion` 値が含まれる、SQL の UPDATE コマンドが生成されます。</span><span class="sxs-lookup"><span data-stu-id="803df-216">EF Core generates a SQL UPDATE command with a WHERE clause containing the original `RowVersion` value.</span></span> <span data-ttu-id="803df-217">UPDATE コマンドの影響を受ける行がない場合 (元の `RowVersion` 値が含まれる行がない)、`DbUpdateConcurrencyException` 例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="803df-217">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value), a `DbUpdateConcurrencyException` exception is thrown.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-999)]

<span data-ttu-id="803df-218">上のコードでは、エンティティがフェッチされたときの値は、`Department.RowVersion` です。</span><span class="sxs-lookup"><span data-stu-id="803df-218">In the preceding code, `Department.RowVersion` is the value when the entity was fetched.</span></span> <span data-ttu-id="803df-219">このメソッドで `FirstOrDefaultAsync` が呼び出されましたときのデータベース内の値は、`OriginalValue` です。</span><span class="sxs-lookup"><span data-stu-id="803df-219">`OriginalValue` is the value in the DB when `FirstOrDefaultAsync` was called in this method.</span></span>

<span data-ttu-id="803df-220">次のコードによって、クライアントの値 (このメソッドにポストされた値) とデータベースの値が取得されます。</span><span class="sxs-lookup"><span data-stu-id="803df-220">The following code gets the client values (the values posted to this method) and the DB values:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

<span data-ttu-id="803df-221">次のコードによって、`OnPostAsync` にポストされたのとは異なるデータベースの値がある各列に、カスタム エラー メッセージが追加されます。</span><span class="sxs-lookup"><span data-stu-id="803df-221">The following code adds a custom error message for each column that has DB values different from what was posted to `OnPostAsync`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

<span data-ttu-id="803df-222">次の強調表示されたコードによって、データベースから取得された新しい値に `RowVersion` 値が設定されます。</span><span class="sxs-lookup"><span data-stu-id="803df-222">The following highlighted code sets the `RowVersion` value to the new value retrieved from the DB.</span></span> <span data-ttu-id="803df-223">次にユーザーが **[保存]** をクリックしたとき、Edit ページが最後に表示されたときのコンカレンシー エラーのみが検出されます。</span><span class="sxs-lookup"><span data-stu-id="803df-223">The next time the user clicks **Save**, only concurrency errors that happen since the last display of the Edit page will be caught.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

<span data-ttu-id="803df-224">`ModelState` の `RowVersion` 値が古いため、`ModelState.Remove` ステートメントが必要になります。</span><span class="sxs-lookup"><span data-stu-id="803df-224">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="803df-225">Razor ページでは、フィールドの `ModelState` 値がモデル プロパティ値より優先されます。</span><span class="sxs-lookup"><span data-stu-id="803df-225">In the Razor Page, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="803df-226">[編集] ページを更新する</span><span class="sxs-lookup"><span data-stu-id="803df-226">Update the Edit page</span></span>

<span data-ttu-id="803df-227">次のマークアップを使用して *Pages/Departments/Edit.cshtml* を更新します。</span><span class="sxs-lookup"><span data-stu-id="803df-227">Update *Pages/Departments/Edit.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

<span data-ttu-id="803df-228">上のマークアップでは以下の操作が行われます。</span><span class="sxs-lookup"><span data-stu-id="803df-228">The preceding markup:</span></span>

* <span data-ttu-id="803df-229">`page` ディレクティブを `@page` から `@page "{id:int}"` に更新します。</span><span class="sxs-lookup"><span data-stu-id="803df-229">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="803df-230">非表示の行バージョンが追加されます。</span><span class="sxs-lookup"><span data-stu-id="803df-230">Adds a hidden row version.</span></span> <span data-ttu-id="803df-231">ポストバックが値をバインドするように、`RowVersion` を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="803df-231">`RowVersion` must be added so post back binds the value.</span></span>
* <span data-ttu-id="803df-232">デバッグのために、`RowVersion` の最後のバイトが表示されます。</span><span class="sxs-lookup"><span data-stu-id="803df-232">Displays the last byte of `RowVersion` for debugging purposes.</span></span>
* <span data-ttu-id="803df-233">`ViewData` を厳密に型指定された `InstructorNameSL` と置換します。</span><span class="sxs-lookup"><span data-stu-id="803df-233">Replaces `ViewData` with the strongly-typed `InstructorNameSL`.</span></span>

## <a name="test-concurrency-conflicts-with-the-edit-page"></a><span data-ttu-id="803df-234">Edit ページでのコンカレンシーの競合のテスト</span><span class="sxs-lookup"><span data-stu-id="803df-234">Test concurrency conflicts with the Edit page</span></span>

<span data-ttu-id="803df-235">English 部署の 2 つの Edit のブラウザー インスタンスを開きます。</span><span class="sxs-lookup"><span data-stu-id="803df-235">Open two browsers instances of Edit on the English department:</span></span>

* <span data-ttu-id="803df-236">アプリを実行し、部署を選択します。</span><span class="sxs-lookup"><span data-stu-id="803df-236">Run the app and select Departments.</span></span>
* <span data-ttu-id="803df-237">English 部署の **[編集]** ハイパーリンクを右クリックし、 **[新しいタブで開く]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="803df-237">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**.</span></span>
* <span data-ttu-id="803df-238">最初のタブの English 部署の **[編集]** ハイパーリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="803df-238">In the first tab, click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="803df-239">2 つのブラウザー タブに同じ情報が表示されます。</span><span class="sxs-lookup"><span data-stu-id="803df-239">The two browser tabs display the same information.</span></span>

<span data-ttu-id="803df-240">最初のブラウザー タブの名前を変更し、 **[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="803df-240">Change the name in the first browser tab and click **Save**.</span></span>

![変更後の Department Edit ページ 1](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="803df-242">Index ページが、値が変更され、rowVersion インジケーターが更新され、ブラウザーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="803df-242">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="803df-243">更新された rowVersion インジケーターに注意してください。これは、他方のタブの 2 番目のポストバックに表示されています。</span><span class="sxs-lookup"><span data-stu-id="803df-243">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="803df-244">2 番目のブラウザー タブで別のフィールドを変更します。</span><span class="sxs-lookup"><span data-stu-id="803df-244">Change a different field in the second browser tab.</span></span>

![変更後の Department Edit ページ 2](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="803df-246">**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="803df-246">Click **Save**.</span></span> <span data-ttu-id="803df-247">データベースの値と一致しないすべてのフィールドに、エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="803df-247">You see error messages for all fields that don't match the DB values:</span></span>

![Department Edit ページのエラー メッセージ](concurrency/_static/edit-error.png)

<span data-ttu-id="803df-249">このブラウザー ウィンドウでは、Name フィールドの変更は意図されていませんでした。</span><span class="sxs-lookup"><span data-stu-id="803df-249">This browser window didn't intend to change the Name field.</span></span> <span data-ttu-id="803df-250">Name フィールドに、現在の値 (言語) をコピーして貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="803df-250">Copy and paste the current value (Languages) into the Name field.</span></span> <span data-ttu-id="803df-251">タブを終了します。クライアント側の検証によって、エラー メッセージが削除されます。</span><span class="sxs-lookup"><span data-stu-id="803df-251">Tab out. Client-side validation removes the error message.</span></span>

![Department Edit ページのエラー メッセージ](concurrency/_static/cv.png)

<span data-ttu-id="803df-253">**[保存]** をもう一度クリックします。</span><span class="sxs-lookup"><span data-stu-id="803df-253">Click **Save** again.</span></span> <span data-ttu-id="803df-254">2 番目のブラウザー タブに入力した値が保存されます。</span><span class="sxs-lookup"><span data-stu-id="803df-254">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="803df-255">Index ページで、保存した値が表示されます。</span><span class="sxs-lookup"><span data-stu-id="803df-255">You see the saved values in the Index page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="803df-256">[削除] ページを更新する</span><span class="sxs-lookup"><span data-stu-id="803df-256">Update the Delete page</span></span>

<span data-ttu-id="803df-257">次のコードを使用して、[削除] ページ モデルを更新します。</span><span class="sxs-lookup"><span data-stu-id="803df-257">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

<span data-ttu-id="803df-258">フェッチ後にエンティティに変更があった場合、Delete ページによってコンカレンシーの競合が検出されます。</span><span class="sxs-lookup"><span data-stu-id="803df-258">The Delete page detects concurrency conflicts when the entity has changed after it was fetched.</span></span> <span data-ttu-id="803df-259">`Department.RowVersion` は、エンティティがフェッチされたときの行バージョンです。</span><span class="sxs-lookup"><span data-stu-id="803df-259">`Department.RowVersion` is the row version when the entity was fetched.</span></span> <span data-ttu-id="803df-260">EF Core が SQL DELETE コマンドを作成するとき、それには `RowVersion` 句が含まれる WHERE 句が含まれます。</span><span class="sxs-lookup"><span data-stu-id="803df-260">When EF Core creates the SQL DELETE command, it includes a WHERE clause with `RowVersion`.</span></span> <span data-ttu-id="803df-261">SQL DELETE コマンドで影響を受ける行がゼロの場合、次が発生します。</span><span class="sxs-lookup"><span data-stu-id="803df-261">If the SQL DELETE command results in zero rows affected:</span></span>

* <span data-ttu-id="803df-262">SQL DELETE コマンドの `RowVersion` がデータベースの `RowVersion` と一致しません。</span><span class="sxs-lookup"><span data-stu-id="803df-262">The `RowVersion` in the SQL DELETE command doesn't match `RowVersion` in the DB.</span></span>
* <span data-ttu-id="803df-263">DbUpdateConcurrencyException 例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="803df-263">A DbUpdateConcurrencyException exception is thrown.</span></span>
* <span data-ttu-id="803df-264">`OnGetAsync` が `concurrencyError` と共に呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="803df-264">`OnGetAsync` is called with the `concurrencyError`.</span></span>

### <a name="update-the-delete-page"></a><span data-ttu-id="803df-265">[削除] ページを更新する</span><span class="sxs-lookup"><span data-stu-id="803df-265">Update the Delete page</span></span>

<span data-ttu-id="803df-266">次のコードを使用して、*Pages/Departments/Delete.cshtml* を更新します。</span><span class="sxs-lookup"><span data-stu-id="803df-266">Update *Pages/Departments/Delete.cshtml* with the following code:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,39,51)]

<span data-ttu-id="803df-267">上記のマークアップは、次の変更を加えます。</span><span class="sxs-lookup"><span data-stu-id="803df-267">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="803df-268">`page` ディレクティブを `@page` から `@page "{id:int}"` に更新します。</span><span class="sxs-lookup"><span data-stu-id="803df-268">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="803df-269">エラー メッセージを追加します。</span><span class="sxs-lookup"><span data-stu-id="803df-269">Adds an error message.</span></span>
* <span data-ttu-id="803df-270">**[管理者]** フィールドで FirstMidName が FullName に変更されます。</span><span class="sxs-lookup"><span data-stu-id="803df-270">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>
* <span data-ttu-id="803df-271">最後のバイトを表示するよう、`RowVersion` を変更します。</span><span class="sxs-lookup"><span data-stu-id="803df-271">Changes `RowVersion` to display the last byte.</span></span>
* <span data-ttu-id="803df-272">非表示の行バージョンが追加されます。</span><span class="sxs-lookup"><span data-stu-id="803df-272">Adds a hidden row version.</span></span> <span data-ttu-id="803df-273">ポストバックが値をバインドするように、`RowVersion` を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="803df-273">`RowVersion` must be added so post back binds the value.</span></span>

### <a name="test-concurrency-conflicts-with-the-delete-page"></a><span data-ttu-id="803df-274">Delete ページでのコンカレンシーの競合のテスト</span><span class="sxs-lookup"><span data-stu-id="803df-274">Test concurrency conflicts with the Delete page</span></span>

<span data-ttu-id="803df-275">テスト部署を作成します。</span><span class="sxs-lookup"><span data-stu-id="803df-275">Create a test department.</span></span>

<span data-ttu-id="803df-276">テスト部署の 2 つの Delete のブラウザー インスタンスを開きます。</span><span class="sxs-lookup"><span data-stu-id="803df-276">Open two browsers instances of Delete on the test department:</span></span>

* <span data-ttu-id="803df-277">アプリを実行し、部署を選択します。</span><span class="sxs-lookup"><span data-stu-id="803df-277">Run the app and select Departments.</span></span>
* <span data-ttu-id="803df-278">テスト部署の **[削除]** ハイパーリンクを右クリックし、 **[新しいタブで開く]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="803df-278">Right-click the **Delete** hyperlink for the test department and select **Open in new tab**.</span></span>
* <span data-ttu-id="803df-279">テスト部署の **[編集]** ハイパーリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="803df-279">Click the **Edit** hyperlink for the test department.</span></span>

<span data-ttu-id="803df-280">2 つのブラウザー タブに同じ情報が表示されます。</span><span class="sxs-lookup"><span data-stu-id="803df-280">The two browser tabs display the same information.</span></span>

<span data-ttu-id="803df-281">最初のブラウザー タブで予算を変更し、 **[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="803df-281">Change the budget in the first browser tab and click **Save**.</span></span>

<span data-ttu-id="803df-282">Index ページが、値が変更され、rowVersion インジケーターが更新され、ブラウザーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="803df-282">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="803df-283">更新された rowVersion インジケーターに注意してください。これは、他方のタブの 2 番目のポストバックに表示されています。</span><span class="sxs-lookup"><span data-stu-id="803df-283">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="803df-284">2 番目のタブから、テスト部署を削除します。データベースからの現在の値で、コンカレンシー エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="803df-284">Delete the test department from the second tab. A concurrency error is display with the current values from the DB.</span></span> <span data-ttu-id="803df-285">**[削除]** をクリックすると、`RowVersion` が更新され、部署が削除されていない限り、エンティティは削除されます。</span><span class="sxs-lookup"><span data-stu-id="803df-285">Clicking **Delete** deletes the entity, unless `RowVersion` has been updated.department has been deleted.</span></span>

<span data-ttu-id="803df-286">データ モデルを継承する方法については、「[継承](xref:data/ef-mvc/inheritance)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="803df-286">See [Inheritance](xref:data/ef-mvc/inheritance) on how to inherit a data model.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="803df-287">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="803df-287">Additional resources</span></span>

* [<span data-ttu-id="803df-288">コンカレンシー トークン</span><span class="sxs-lookup"><span data-stu-id="803df-288">Concurrency Tokens in EF Core</span></span>](/ef/core/modeling/concurrency)
* [<span data-ttu-id="803df-289">EF Core のコンカレンシーの処理</span><span class="sxs-lookup"><span data-stu-id="803df-289">Handle concurrency in EF Core</span></span>](/ef/core/saving/concurrency)
* [<span data-ttu-id="803df-290">このチュートリアルの YouTube バージョン (コンカレンシーの競合の処理)</span><span class="sxs-lookup"><span data-stu-id="803df-290">YouTube version of this tutorial(Handling Concurrency Conflicts)</span></span>](https://youtu.be/EosxHTFgYps)
* [<span data-ttu-id="803df-291">このチュートリアルの YouTube バージョン (パート 2)</span><span class="sxs-lookup"><span data-stu-id="803df-291">YouTube version of this tutorial(Part 2)</span></span>](https://www.youtube.com/watch?v=kcxERLnaGO0)
* [<span data-ttu-id="803df-292">このチュートリアルの YouTube バージョン (パート 3)</span><span class="sxs-lookup"><span data-stu-id="803df-292">YouTube version of this tutorial(Part 3)</span></span>](https://www.youtube.com/watch?v=d4RbpfvELRs)

> [!div class="step-by-step"]
> [<span data-ttu-id="803df-293">前へ</span><span class="sxs-lookup"><span data-stu-id="803df-293">Previous</span></span>](xref:data/ef-rp/update-related-data)
