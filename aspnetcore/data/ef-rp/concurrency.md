---
title: "Razor ページの EF コアでの同時実行性、8 8"
author: rick-anderson
description: "このチュートリアルでは、複数のユーザーが同時に同じエンティティを更新するときに競合を処理する方法を示します。"
keywords: "ASP.NET Core、Entity Framework Core では、同時実行"
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/concurrency
ms.openlocfilehash: 8862c6b9a5eb7ac3b6889071e4ce9ff6f02512c9
ms.sourcegitcommit: 281f0c614543a6c3db565ea4655b70fe49b61d84
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2018
---
en-米国/

# <a name="handling-concurrency-conflicts---ef-core-with-razor-pages-8-of-8"></a>同時実行の競合の EF コア Razor ページ (8 の 8) での処理

によって[Rick Anderson](https://twitter.com/RickAndMSFT)、 [Tom Dykstra](https://github.com/tdykstra)、および[Jon P Smith](https://twitter.com/thereformedprog)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

このチュートリアルでは、複数のユーザーが同時に (一度に) エンティティを更新するときに競合を処理する方法を示します。 問題を解決できない場合に実行する場合は、ダウンロード、[この段階でのアプリを完成](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8)です。

## <a name="concurrency-conflicts"></a>同時実行の競合

同時実行の競合が発生した場合。

* ユーザーは、エンティティの編集 ページに移動します。
* 別のユーザーは、最初のユーザーの変更がデータベースに書き込まれる前に、同じエンティティを更新します。

同時実行の検出が有効でない場合、同時実行更新が発生した場合。

* 最後の更新の競合。 つまり、最後の値の更新は、データベースに保存されます。
* 現在の更新プログラムの最初の数値は失われます。

### <a name="optimistic-concurrency"></a>オプティミスティック同時実行制御

オプティミスティック同時実行制御により、同時実行の競合が発生するし対処する適切な場合、操作を行います。 たとえば、加藤さんは、部門の編集 ページにアクセスし、$350,000.00 から $0.00 に英語版の部門に予算を変更します。

![予算を 0 に変更します。](concurrency/_static/change-budget.png)

加藤さんがクリックする前に**保存**John は、同じページにアクセスし、2007 年 9 月 1 日から 2013 年 9 月 1 日に、Start Date フィールドを変更します。

![2013 を開始日を変更します。](concurrency/_static/change-date.png)

加藤さんがクリックした**保存**最初られ、ブラウザーには、インデックス ページが表示されます。 変更彼女に表示されます。

![予算を 0 に変更されました](concurrency/_static/budget-zero.png)

John が**保存**ドル 350,000.00 の予算が表示される編集ページにします。 次の動作は、同時実行の競合を処理する方法によって決まります。

オプティミスティック同時実行制御には、次のオプションが含まれています。

* プロパティをユーザーが変更の追跡および、DB に対応する列のみを更新できます。

 シナリオでは、データは失われません。 さまざまなプロパティは、2 人のユーザーによって更新されました。 次回の英語版の部門を参照するユーザーがジェーンのと John の両方の変更が表示されます。 更新には、このメソッドは、データが失われる可能性のある競合の数を減らすことができます。 このアプローチ: * 競合する変更が同じプロパティに加えられた場合、データの損失を回避することはできません。
        * は、web アプリで実際的で、通常ありません。 すべてのフェッチ値と新しい値を追跡するために重要な状態を維持することが必要です。 大量の状態を保持することと、アプリのパフォーマンスが影響することができます。
        * が、エンティティの同時実行の検出と比較して、アプリの複雑度が向上します。

* John の変更がジェーンの変更を上書きすることができます。

 次に英語版の部門を参照するユーザーが、2013 年 9 月 1 日が表示されます、フェッチされたドル 350,000.00 値。 この方法と呼ばれる、*クライアントが Wins*または*Wins の最後に*シナリオです。 (クライアントからのすべての値よりも優先、データ ストアには、新機能です。)同時実行処理のコーディングを行わない場合は自動的に行われますクライアントが優先されます。

* John の変更は、データベースで更新されているを妨害することができます。 通常、アプリは: * エラー メッセージを表示します。
        * データの現在の状態を表示します。
        *、変更を再適用するユーザーを許可します。

 これと呼ばれる、*ストア Wins*シナリオです。 (データ ストアの値よりも優先、クライアントから送信された値です。)このチュートリアルでは、ストア Wins シナリオを実装します。 このメソッドは、警告を表示されているユーザーがいない状態の変更が上書きされないことを確認します。

## <a name="handling-concurrency"></a>同時実行の処理 

プロパティとして構成されている場合、[同時実行トークン](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency):

* EF コアは、フェッチ後にプロパティが変更されていないことを確認します。 チェックが実行時に[SaveChanges](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges)または[SaveChangesAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_)と呼びます。
* フェッチ後、プロパティが変更された場合、 [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0)がスローされます。 

スローされることをサポートするために、データベースとデータ モデルを構成する必要があります`DbUpdateConcurrencyException`です。

### <a name="detecting-concurrency-conflicts-on-a-property"></a>プロパティの同時実行の競合を検出します。

使用してプロパティ レベルに同時実行の競合を検出できなかったことができます、 [ConcurrencyCheck](https://docs.microsoft.com/en-us/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0)属性。 属性は、モデルに複数のプロパティに適用できます。 詳細については、次を参照してください。[データ注釈 ConcurrencyCheck](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency#data-annotations)です。

`[ConcurrencyCheck]`属性は、このチュートリアルでは使用されません。

### <a name="detecting-concurrency-conflicts-on-a-row"></a>行を同時実行の競合を検出します。

同時実行の競合を検出するために、 [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql)列を追跡は、モデルに追加します。  `rowversion` :

* SQL Server は固有です。 他のデータベースはのような機能を備えていない可能性があります。
* データベースからフェッチされたエンティティが変更されていないことを確認に使用されます。 

DB 生成順次`rowversion`がインクリメントごとに、行番号を更新します。 `Update`または`Delete`コマンド、`Where`句には、フェッチされた値が含まれています。`rowversion`です。 場合は、更新された行が変更されました。

 * `rowversion`フェッチされた値が一致しません。
 * `Update`または`Delete`コマンドは、行を検索しないため、`Where`句が含まれていますが、フェッチされた`rowversion`です。
 * A`DbUpdateConcurrencyException`がスローされます。

EF コア、によって行が更新されていないときに、`Update`または`Delete`コマンドを同時実行例外がスローされます。

### <a name="add-a-tracking-property-to-the-department-entity"></a>追跡プロパティ、Department エンティティを追加します。

*Models/Department.cs*RowVersion をという名前の追跡のプロパティを追加します。

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

[タイムスタンプ](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute)でこの列が含まれている属性を指定、`Where`の句`Update`と`Delete`コマンド。 属性といいます`Timestamp`旧バージョンの SQL Server、SQL を使用するため`timestamp`データ型、SQL に`rowversion`型で置き換えられます。

Fluent API では、追跡プロパティも指定できます。

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

次のコードは、部門名が更新されたときに、EF コアによって生成された T-SQL ステートメントの一部を示しています。

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

上記のコードに示す強調表示されている、`WHERE`句を含む`RowVersion`です。 場合 DB`RowVersion`値が一致しません、`RowVersion`パラメーター (`@p2`)、行は更新されません。

次の強調表示されたコードは、ただ 1 つの行が更新されたことを検証する T-SQL ステートメントを示しています。

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

[@@ROWCOUNT ](https://docs.microsoft.com/en-us/sql/t-sql/functions/rowcount-transact-sql)最後のステートメントによる影響を受ける行の数を返します。 なしで行が更新されて、EF コアをスロー、`DbUpdateConcurrencyException`です。

Visual Studio の出力ウィンドウに T-SQL EF コアが生成されるを確認できます。

### <a name="update-the-db"></a>DB を更新します。

追加する、`RowVersion`プロパティが、移行を必要とする DB モデルを変更します。

プロジェクトをビルドします。 コマンド ウィンドウで、次を入力します。

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

上記のコマンド:

* 追加、*移行/{時間 stamp}_RowVersion.cs*移行ファイル。
* 更新プログラム、 *Migrations/SchoolContextModelSnapshot.cs*ファイル。 次の強調表示されたコードを追加する、更新、`BuildModel`メソッド。

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* DB の更新への移行を実行します。

<a name="scaffold"></a>
## <a name="scaffold-the-departments-model"></a>Scaffold 部門モデル

* Visual Studio を終了します。
* プロジェクト ディレクトリ (*Program.cs*、*Startup.cs*、および *.csproj* ファイルを含むディレクトリ) でコマンド ウィンドウを開きます。
* 次のコマンドを実行します。

 ```console
dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
 ```

上記のコマンド スキャフォールディング、`Department`モデル。 Visual Studio でプロジェクトを開きます。

プロジェクトをビルドします。 ビルドには、次のようなエラーが生成されます。

`1>Pages/Departments/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Department' and no extension method 'Department' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 グローバルに変更する`_context.Department`に`_context.Departments`(つまり、"s"を追加する`Department`)。 7 回の出現が検出され、更新します。

### <a name="update-the-departments-index-page"></a>部門のインデックス ページを更新します。

作成スキャフォールディング エンジン、`RowVersion`インデックス ページがそのフィールドの列を表示することはできません。 このチュートリアルの最後のバイトで、`RowVersion`同時実行制御の理解に役立つが表示されます。 最後のバイトは、一意であることは保証されません。 実際のアプリを表示しない`RowVersion`またはの最後のバイト`RowVersion`です。

インデックス ページを更新します。

* 部門では、インデックスを置き換えます。
* 含むマークアップを置き換える`RowVersion`の最後のバイトについて`RowVersion`です。
* FullName を FirstMidName を置き換えます。

次のマークアップは、更新されたページを示しています。

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a>編集ページ モデルを更新します。

更新*pages\departments\edit.cshtml.cs*を次のコード。

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

同時実行の問題を検出するために、 [OriginalValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue)で更新が、`rowVersion`フェッチ エンティティからの値。 EF コア元を含む WHERE 句を使用して SQL の UPDATE コマンドが生成されます`RowVersion`値。 更新コマンドで行に影響がない場合 (行には、元のあるありません`RowVersion`値)、`DbUpdateConcurrencyException`例外がスローされます。

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-)]

上記のコードで`Department.RowVersion`エンティティがフェッチしたときに、値は、します。 `OriginalValue`DB 内の値と`FirstOrDefaultAsync`がこのメソッドで呼び出されました。

次のコードでは、クライアントの値 (このメソッドにポストされた値)、および DB の値を取得します。

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

次のコードは、DB のある各列の値にポストされた内容と異なるためにカスタム エラー メッセージを追加`OnPostAsync`:

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

次にコード セットが強調表示されて、 `RowVersion` DB から新しい値に値を取得します。 次に、ユーザーがクリックしたとき**保存**、最後の編集 ページの表示が検出されるために発生する同時実行エラーのみです。

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

`ModelState.Remove`ために、ステートメントは必要な`ModelState`が古い`RowVersion`値。 Razor ページで、`ModelState`フィールドよりも優先、モデル プロパティの値が両方に存在する場合の値します。

## <a name="update-the-edit-page"></a>更新プログラムの編集 ページ

更新*Pages/Departments/Edit.cshtml*次のマークアップ。

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

上記のマークアップ。

* 更新プログラム、`page`からディレクティブ`@page`に`@page "{id:int}"`です。
* 非表示の行バージョンを追加します。 `RowVersion`ポスト バック値をバインドするために追加する必要があります。
* 最後のバイトを表示`RowVersion`デバッグのためにします。
* 置換`ViewData`厳密に型指定と`InstructorNameSL`です。

## <a name="test-concurrency-conflicts-with-the-edit-page"></a>テストの編集 ページで、同時実行の競合

英語版の部門の編集の 2 つのブラウザー インスタンスを開きます。

* アプリを実行し、部門を選択します。
* 右クリックし、**編集**ハイパーリンクをクリックし、英語部門**新しいタブで開く**です。
* 最初のタブをクリックして、**編集**英語部門のハイパーリンクのです。

2 つのブラウザー タブでは、同じ情報を表示します。

最初のブラウザー タブで名前を変更し、をクリックして**保存**です。

![部門編集変更後のページ 1](concurrency/_static/edit-after-change-1.png)

ブラウザーでは、変更された値と更新された rowVersion インジケーターのインデックス ページを示します。 更新された rowVersion インジケーターを注意してください。 その他 タブで 2 つ目のポストバック時に表示されます。

2 番目のブラウザー タブで、別のフィールドを変更します。

![部門編集変更した後に 2 ページ目](concurrency/_static/edit-after-change-2.png)

**[保存]**をクリックします。 DB 値と一致しないすべてのフィールドのエラー メッセージを参照してください。

![部門の編集 ページのエラー メッセージ](concurrency/_static/edit-error.png)

このブラウザー ウィンドウを Name フィールドを変更する意図しません。 コピーし、[名前] フィールドに、現在の値 (言語) を貼り付けます。 タブです。クライアント側の検証では、エラー メッセージを削除します。

![部門の編集 ページのエラー メッセージ](concurrency/_static/cv.png)

をクリックして**保存**もう一度です。 2 番目のブラウザー タブで入力した値は保存されます。 インデックス ページ内に保存された値が参照してください。

## <a name="update-the-delete-page"></a>更新プログラムの削除 ページ

次のコードで Delete ページ モデルを更新します。

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

ページの削除 は、エンティティは、フェッチ後に変更されたときに同時実行の競合を検出します。 `Department.RowVersion`エンティティがフェッチしたときに、行バージョンです。 EF コアは、SQL の DELETE コマンドを作成するときに WHERE 句が含まれます`RowVersion`です。 場合は、SQL の DELETE コマンドの結果行は 0 行で影響を受けます。

* `RowVersion` SQL の DELETE でコマンドが一致しない`RowVersion`DB にします。
* DbUpdateConcurrencyException 例外がスローされます。
* `OnGetAsync`使用が呼び出される、`concurrencyError`です。

### <a name="update-the-delete-page"></a>更新プログラムの削除 ページ

更新*Pages/Departments/Delete.cshtml*を次のコード。

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,36,51)]


上記のマークアップでは、次の変更を加えます。

* 更新プログラム、`page`からディレクティブ`@page`に`@page "{id:int}"`です。
* エラー メッセージを追加します。
* 置き換えますで FullName FirstMidName、**管理者**フィールドです。
* 変更`RowVersion`を最後のバイトを表示します。
* 非表示の行バージョンを追加します。 `RowVersion`ポスト バック値をバインドするために追加する必要があります。

### <a name="test-concurrency-conflicts-with-the-delete-page"></a>テストの削除 ページで、同時実行の競合

テストの部門を作成します。

テスト部門の削除の 2 つのブラウザー インスタンスを開きます。

* アプリを実行し、部門を選択します。
* 右クリックし、**削除**ハイパーリンクを選択してテスト部門**新しいタブで開く**です。
* クリックして、**編集**テスト部門のハイパーリンクのです。

2 つのブラウザー タブでは、同じ情報を表示します。

最初のブラウザー タブで、予算を変更し、クリックして**保存**です。

ブラウザーでは、変更された値と更新された rowVersion インジケーターのインデックス ページを示します。 更新された rowVersion インジケーターを注意してください。 その他 タブで 2 つ目のポストバック時に表示されます。

2 番目のタブから、テストの部門を削除します。同時実行エラーは、DB から現在の値で表示します。 クリックすると**削除**、エンティティを削除しない限り、 `RowVersion` updated.department が削除されたされました。

参照してください[継承](xref:data/ef-mvc/inheritance)については、データ モデルで継承する方法です。

### <a name="additional-resources"></a>その他の技術情報

* [EF Core での同時実行トークン](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency)
* [EF Core での同時実行の処理](https://docs.microsoft.com/en-us/ef/core/saving/concurrency)

>[!div class="step-by-step"]
[前へ](xref:data/ef-rp/update-related-data)
