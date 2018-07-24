---
title: ASP.NET Core の Razor ページと EF Core - 同時実行 - 8/8
author: rick-anderson
description: このチュートリアルでは、複数のユーザーが同じエンティティを同時に更新するときの競合の処理方法について説明します。
ms.author: riande
ms.date: 11/15/2017
uid: data/ef-rp/concurrency
ms.openlocfilehash: ff9e52df63f9c9f47ee659a68beb28b773a114a1
ms.sourcegitcommit: a3675f9704e4e73ecc7cbbbf016a13d2a5c4d725
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/23/2018
ms.locfileid: "39202693"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---concurrency---8-of-8"></a>ASP.NET Core の Razor ページと EF Core - 同時実行 - 8/8

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)、[Tom Dykstra](https://github.com/tdykstra)、[Jon P Smith](https://twitter.com/thereformedprog)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

このチュートリアルでは、複数のユーザーがエンティティを同時に更新するときの競合の処理方法について説明します。 解決できない問題が発生した場合は、[このステージの完成したアプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8)をダウンロードしてください。

## <a name="concurrency-conflicts"></a>同時実行の競合

同時実行の競合は、次のような場合に発生します。

* ユーザーがエンティティの Edit ページに移動した場合。
* 最初のユーザーの変更がデータベースに書き込まれる前に、別のユーザーが同じエンティティを更新した場合。

同時実行の検出が無効のとき、同時更新が発生すると、次のようになります。

* 最後の更新が有効になります。 つまり、最後に更新された値がデータベースに保存されます。
* 現在の更新の最初のものは失われます。

### <a name="optimistic-concurrency"></a>オプティミスティック同時実行制御

オプティミスティック同時実行制御では、同時実行競合の発生が許可され、それが発生した場合に適切に対処されます。 たとえば、Jane が Department Edit ページにアクセスし、English 部署の予算を $350,000.00 から $0.00 に変更するとします。

![予算を 0 に変更する](concurrency/_static/change-budget.png)

Jane が **[保存]** をクリックする前に John が同じページにアクセスし、[開始日] フィールドを 9/1/2007 から 9/1/2013 に変更します。

![開始日を 2013 に変更する](concurrency/_static/change-date.png)

Jane が **[保存]** を先にクリックすると、ブラウザーの Index ページには、Jane の変更が反映されています。

![予算が 0 に変更された](concurrency/_static/budget-zero.png)

John が Edit ページの **[保存]** をクリックします。このとき、予算は $350,000.00 と表示されています。 この後の動作は、同時実行競合の処理方法によって決定されます。

オプティミスティック同時実行制御には、次のオプションがあります。

* ユーザーが変更したプロパティを追跡記録し、それに該当する列だけをデータベースで更新します。

  このシナリオでは、データは失われません。 2 人のユーザーが別のプロパティを更新しました。 今度誰かが English 部署を閲覧すると、Jane の変更と John の変更が両方とも表示されます。 この更新方法では、データが失われる原因となる競合の数を減らすことができます。 この方法の特徴は次のとおりです。
 
  * 競合する変更が同じプロパティに加えられた場合、データの損失を回避することはできません。
  * Web アプリでは、一般的に実用的ではありません。 フェッチされたすべての値と新しい値を追跡するために、かなりのステータスを維持することが必要になります。 大量のステータスを保守管理することになると、アプリケーションのパフォーマンスに影響が出ます。
  * エンティティでの同時実行の検出と比較して、アプリは複雑になります。

* John の変更で Jane の変更を上書きするように指定できます。

  今度誰かが English 部署を閲覧すると、9/1/2013 の日付と、フェッチされた $350,000.00 の値が表示されます。 このアプローチは *Client Wins* (クライアント側に合わせる) シナリオまたは *Last in Wins* (最終書き込み者優先) シナリオと呼ばれています。 (クライアントからの値がすべて、データ ストアの値より優先されます。)同時実行処理に対してコーディングを行わない場合、自動的にクライアント側に合わせられます。

* データベースで John の変更が更新されないようにできます。 通常、アプリの動作は次のようになります。

  * エラー メッセージが表示されます。
  * データの現在のステータスが表示されます。
  * ユーザーが変更を再適用できるようになります。

  これは *Store Wins* (ストア側に合わせる) シナリオと呼ばれています。 (クライアントが送信した値よりデータストアの値が優先されます。)このチュートリアルでは、Store Wins シナリオを実装します。 この手法では、変更が上書きされるとき、それが必ずユーザーに警告されます。

## <a name="handling-concurrency"></a>同時実行の処理 

プロパティが[同時実行トークン](https://docs.microsoft.com/ef/core/modeling/concurrency)として構成されている場合、次が実行されます。

* EF Core によって、フェッチ後にプロパティが変更されていないことが確認されます。 このチェックは、[SaveChanges](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) または [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) が呼び出されたときに発生します。
* フェッチ後にプロパティが変更されていると、[DbUpdateConcurrencyException](/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) がスローされます。 

データベースとデータ モデルは、`DbUpdateConcurrencyException` のスローをサポートするように構成される必要があります。

### <a name="detecting-concurrency-conflicts-on-a-property"></a>プロパティの同時実行競合の検出

同時実行の競合は、[ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) 属性を使用し、プロパティ レベルで検出できます。 この属性は、モデルの複数のプロパティに適用できます。 詳細については、「[データの注釈 - ConcurrencyCheck](/ef/core/modeling/concurrency#data-annotations)」を参照してください。

このチュートリアルでは、`[ConcurrencyCheck]` 属性は使用しません。

### <a name="detecting-concurrency-conflicts-on-a-row"></a>行の同時実行競合の検出

同時実行競合を検出するために、モデルに [rowversion](/sql/t-sql/data-types/rowversion-transact-sql) 追跡列が追加されました。  `rowversion` は、次のとおりです。

* SQL Server 専用です。 他のデータベースには、似たような機能がない場合があります。
* データベースからフェッチされた以降、エンティティに変更がないことを決定するために使用されます。 

データベースによって、行が更新されるたびに増える、連続する `rowversion` 番号値が生成されます。 `Update` または `Delete` コマンドの `Where` 句には、フェッチされた `rowversion` の値が含まれます。 更新された行が変更された場合、次のようになります。

 * `rowversion` がフェッチされた値と一致しなくなります。
 * `Where` 句にフェッチされた `rowversion` が含まれるので、`Update` または `Delete` コマンドでは行が検索されません。
 * `DbUpdateConcurrencyException` がスローされます。

EF Core では、`Update` または `Delete` コマンドで行が更新されない場合、同時実行競合がスローされます。

### <a name="add-a-tracking-property-to-the-department-entity"></a>Department エンティティにトラッキング プロパティを追加する

*Models/Department.cs* で、RowVersion という名前のトラッキング プロパティを追加します。

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

[Timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) 属性では、この列は、`Update` および `Delete` コマンドの `Where` 句に含まれることを指定します。 前のバージョンの SQL Server では、SQL `rowversion` 型に取って代わられる前、SQL `timestamp` というデータ型が使用されていたため、この属性は `Timestamp` と呼ばれています。

Fluent API でも、トラッキング プロパティを指定できます。

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

次のコードは、Department 名が更新されたときに、EF Core によって生成された T-SQL ステートメントの一部です。

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

上の強調表示されたコードには、`RowVersion` を含む `WHERE` 句があります。 データベースの `RowVersion` が `RowVersion` パラメーター (`@p2`) と一致しない場合、行は更新されません。

次の強調表示されたコードは、1 つの行のみが更新されたことを検証する T-SQL です。

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

[@@ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) では、最後のステートメントの影響を受けた行数を返します。 更新された行がない場合、EF Core は `DbUpdateConcurrencyException` をスローします。

Visual Studio の出力ウィンドウでは、EF Core が生成する T-SQL を確認できます。

### <a name="update-the-db"></a>データベースの更新

`RowVersion` プロパティを追加すると、移行を必要とするデータベース モデルが変更されます。

プロジェクトをビルドします。 コマンド ウィンドウに次を入力します。

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

上のコマンドでは以下の操作が行われます。

* *Migrations/{time stamp}_RowVersion.cs* 移行ファイルが追加されます。
* *Migrations/SchoolContextModelSnapshot.cs* ファイルが更新されます。 更新により、次の強調表示されたコードが `BuildModel` メソッドに追加されます。

  [!code-csharp[](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* データベースを更新するために移行が実行されます。

<a name="scaffold"></a>
## <a name="scaffold-the-departments-model"></a>部署モデルのスキャフォールディング

* Visual Studio を終了します。
* プロジェクト ディレクトリ (*Program.cs*、*Startup.cs*、および *.csproj* ファイルを含むディレクトリ) でコマンド ウィンドウを開きます。
* 次のコマンドを実行します。

  ```console
  dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
  ```

上記のコマンドは、`Department` モデルをスキャフォールディングします。 Visual Studio でプロジェクトを開きます。

プロジェクトをビルドします。 ビルドにより、次のようなエラーが生成されます。

`1>Pages/Departments/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Department' and no extension method 'Department' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 `_context.Department` を `_context.Departments` にグローバルに変更します (つまり、"s" を `Department` に追加します)。 7 回の出現が見つかり、更新されます。

### <a name="update-the-departments-index-page"></a>Departments Index ページを更新する

スキャフォールディング エンジンにより Index ページに `RowVersion` 列が作成されましたが、このフィールドは表示すべきではありません。 このチュートリアルでは、同時実行を理解するために、`RowVersion` の最後のバイトが表示されています。 最後のバイトは、一意であるとは限りません。 実際のアプリでは、`RowVersion` や `RowVersion` の最後のバイトは表示されません。

Index ページを更新するために、次を実行します。

* Department で Index を置き換えます。
* `RowVersion` の最後のバイトで、`RowVersion` を含むマークアップを置き換えます。
* FirstMidName を FullName で置き換えます。

次のマークアップは、更新されたページを示しています。

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a>Edit ページ モデルの更新

次のコードを使用して、*pages\departments\edit.cshtml.cs* を更新します。

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

同時実行の問題を検出するために、フェッチされたエンティティの [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) が `rowVersion` 値で更新されます。 EF Core では、WHERE 句に元の `RowVersion` 値が含まれる、SQL の UPDATE コマンドが生成されます。 UPDATE コマンドの影響を受ける行がない場合 (元の `RowVersion` 値が含まれる行がない)、`DbUpdateConcurrencyException` 例外がスローされます。

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-999)]

上のコードでは、エンティティがフェッチされたときの値は、`Department.RowVersion` です。 このメソッドで `FirstOrDefaultAsync` が呼び出されましたときのデータベース内の値は、`OriginalValue` です。

次のコードによって、クライアントの値 (このメソッドにポストされた値) とデータベースの値が取得されます。

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

次のコードによって、`OnPostAsync` にポストされたのとは異なるデータベース値がある各列に、カスタム エラー メッセージが追加されます。

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

次の強調表示されたコードによって、データベースから取得された新しい値に `RowVersion` 値が設定されます。 次にユーザーが **[保存]** をクリックしたとき、Edit ページが最後に表示されたときの同時実行エラーのみが検出されます。

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

`ModelState` の `RowVersion` 値が古いため、`ModelState.Remove` ステートメントが必要になります。 Razor ページでは、フィールドの `ModelState` 値がモデル プロパティ値より優先されます。

## <a name="update-the-edit-page"></a>[編集] ページを更新する

次のマークアップを使用して *Pages/Departments/Edit.cshtml* を更新します。

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

上のマークアップでは以下の操作が行われます。

* `page` ディレクティブを `@page` から `@page "{id:int}"` に更新します。
* 非表示の行バージョンが追加されます。 ポストバックが値をバインドするように、`RowVersion` を追加する必要があります。
* デバッグのために、`RowVersion` の最後のバイトが表示されます。
* `ViewData` を厳密に型指定された `InstructorNameSL` と置換します。

## <a name="test-concurrency-conflicts-with-the-edit-page"></a>Edit ページでの同時実行競合のテスト

English 部署の 2 つの Edit のブラウザー インスタンスを開きます。

* アプリを実行し、部署を選択します。
* English 部署の **[編集]** ハイパーリンクを右クリックし、**[新しいタブで開く]** を選択します。
* 最初のタブの English 部署の **[編集]** ハイパーリンクをクリックします。

2 つのブラウザー タブに同じ情報が表示されます。

最初のブラウザー タブの名前を変更し、**[保存]** をクリックします。

![変更後の Department Edit ページ 1](concurrency/_static/edit-after-change-1.png)

Index ページが、値が変更され、rowVersion インジケーターが更新され、ブラウザーに表示されます。 更新された rowVersion インジケーターに注意してください。これは、他方のタブの 2 番目のポストバックに表示されています。

2 番目のブラウザー タブで別のフィールドを変更します。

![変更後の Department Edit ページ 2](concurrency/_static/edit-after-change-2.png)

**[保存]** をクリックします。 データベースの値と一致しないすべてのフィールドに、エラー メッセージが表示されます。

![Department Edit ページのエラー メッセージ](concurrency/_static/edit-error.png)

このブラウザー ウィンドウでは、Name フィールドの変更は意図されていませんでした。 Name フィールドに、現在の値 (言語) をコピーして貼り付けます。 タブを終了します。クライアント側の検証によって、エラー メッセージが削除されます。

![Department Edit ページのエラー メッセージ](concurrency/_static/cv.png)

**[保存]** をもう一度クリックします。 2 番目のブラウザー タブに入力した値が保存されます。 Index ページで、保存した値が表示されます。

## <a name="update-the-delete-page"></a>[削除] ページを更新する

次のコードを使用して、[削除] ページ モデルを更新します。

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

フェッチ後にエンティティに変更があった場合、Delete ページによって同時実行の競合が検出されます。 `Department.RowVersion` は、エンティティがフェッチされたときの行バージョンです。 EF Core が SQL DELETE コマンドを作成するとき、それには `RowVersion` 句が含まれる WHERE 句が含まれます。 SQL DELETE コマンドで影響を受ける行がゼロの場合、次が発生します。

* SQL DELETE コマンドの `RowVersion` がデータベースの `RowVersion` と一致しません。
* DbUpdateConcurrencyException 例外がスローされます。
* `OnGetAsync` が `concurrencyError` と共に呼び出されます。

### <a name="update-the-delete-page"></a>[削除] ページを更新する

次のコードを使用して、*Pages/Departments/Delete.cshtml* を更新します。

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,36,51)]


上記のマークアップは、次の変更を加えます。

* `page` ディレクティブを `@page` から `@page "{id:int}"` に更新します。
* エラー メッセージを追加します。
* **[管理者]** フィールドで FirstMidName が FullName に変更されます。
* 最後のバイトを表示するよう、`RowVersion` を変更します。
* 非表示の行バージョンが追加されます。 ポストバックが値をバインドするように、`RowVersion` を追加する必要があります。

### <a name="test-concurrency-conflicts-with-the-delete-page"></a>Delete ページでの同時実行競合のテスト

テスト部署を作成します。

テスト部署の 2 つの Delete のブラウザー インスタンスを開きます。

* アプリを実行し、部署を選択します。
* テスト部署の **[削除]** ハイパーリンクを右クリックし、**[新しいタブで開く]** を選択します。
* テスト部署の **[編集]** ハイパーリンクをクリックします。

2 つのブラウザー タブに同じ情報が表示されます。

最初のブラウザー タブで予算を変更し、**[保存]** をクリックします。

Index ページが、値が変更され、rowVersion インジケーターが更新され、ブラウザーに表示されます。 更新された rowVersion インジケーターに注意してください。これは、他方のタブの 2 番目のポストバックに表示されています。

2 番目のタブから、テスト部署を削除します。データベースからの現在の値で、同時実行エラーが表示されます。 **[削除]** をクリックすると、`RowVersion` が更新され、部署が削除されていない限り、エンティティは削除されます。

データ モデルを継承する方法については、「[継承](xref:data/ef-mvc/inheritance)」を参照してください。

### <a name="additional-resources"></a>その他の技術情報

* [同時実行制御トークン](/ef/core/modeling/concurrency)
* [EF Core の同時実行の処理](/ef/core/saving/concurrency)

> [!div class="step-by-step"]
> [前へ](xref:data/ef-rp/update-related-data)
