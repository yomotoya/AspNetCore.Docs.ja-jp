---
title: "ASP.NET MVC を持つコアを EF Core での同時実行性 - 8 10"
author: tdykstra
description: "このチュートリアルでは、複数のユーザーが同時に同じエンティティを更新するときに競合を処理する方法を示します。"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/concurrency
ms.openlocfilehash: eee84fe0fbec6ed772342d09931986994903906a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
# <a name="handling-concurrency-conflicts---ef-core-with-aspnet-core-mvc-tutorial-8-of-10"></a>同時実行の競合の ASP.NET Core MVC のチュートリアル (10 の 8) に EF コアの処理

によって[Tom Dykstra](https://github.com/tdykstra)と[Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso 大学でサンプル web アプリケーションでは、Entity Framework のコアと Visual Studio を使用して ASP.NET Core MVC web アプリケーションを作成する方法を示します。 一連のチュートリアルについては、次を参照してください。[系列内の最初のチュートリアル](intro.md)です。

前のチュートリアルでは、データを更新する方法について学習しました。 このチュートリアルでは、複数のユーザーが同時に同じエンティティを更新するときに競合を処理する方法を示します。

使用、Department エンティティと同時実行エラーを処理する web ページを作成します。 次の図は、同時実行の競合が発生した場合に表示される一部のメッセージなどの編集および削除のページを表示します。

![部門の編集 ページ](concurrency/_static/edit-error.png)

![部門の削除 ページ](concurrency/_static/delete-error.png)

## <a name="concurrency-conflicts"></a>同時実行の競合

1 人のユーザーが、編集するために、エンティティのデータを表示し、別のユーザーが最初のユーザーの変更がデータベースに書き込まれる前に、同じエンティティのデータを更新し、同時実行の競合が発生します。 このような競合の検出を有効にしない場合、データベースを更新したユーザーは、他のユーザーの変更を最後上書きされます。 多くのアプリケーションでこのようなリスクが許容可能な: 数名のユーザー、またはいくつかの更新プログラムがある場合、または場合いない本当に重要な場合は、いくつかの変更が上書きされると、同時実行のプログラミングのコストがのメリットを上回る可能性があります。 その場合は、同時実行の競合を処理するアプリケーションを構成する必要はありません。

### <a name="pessimistic-concurrency-locking"></a>ペシミスティック同時実行制御 (ロック)

場合は、アプリケーションは同時実行シナリオの偶発的なデータ損失を防ぐため、これを行うにはデータベース ロックを使用することができます。 これには、ペシミスティック同時実行制御は呼び出されます。 たとえば、データベースから行を参照する前にするロックを要求の読み取り専用または更新アクセスします。 場合更新アクセス権の行をロックすると、他のユーザーは許可されませんのいずれかの行をロックする読み取り専用かが変更されているプロセスでデータのコピーを取得するために、アクセスを更新します。 場合は読み取り専用アクセスの行をロックすると、他のユーザーもロックできます読み取り専用アクセス用のではなく更新します。

ロックの管理、短所があります。 プログラムに複雑なことができます。 必要な管理リソースは大量のデータベース、およびアプリケーションのユーザーの数とパフォーマンスの問題を引き起こす可能性が増加します。 これらの理由は、すべてのデータベース管理システムは、ペシミスティック同時実行をサポートします。 Entity Framework のコアには、組み込みのサポートはありません、このチュートリアルは示してそれを実装する方法。

### <a name="optimistic-concurrency"></a>オプティミスティック同時実行制御

ペシミスティック同時実行制御を使用しない場合は、オプティミスティック同時実行制御です。 オプティミスティック同時実行制御は、許可する同時実行の競合が発生する場合は適切に対処を意味します。 たとえば、加藤さんは、部門の編集 ページにアクセスし、英語版の部門に予算額をドル 350,000.00 から $0.00 に変更します。

![予算を 0 に変更します。](concurrency/_static/change-budget.png)

加藤さんがクリックする前に**保存**John は、同じページにアクセスし、2007 年 9 月 1 日から 2013 年 9 月 1 日に、Start Date フィールドを変更します。

![2013 を開始日を変更します。](concurrency/_static/change-date.png)

加藤さんがクリックした**保存**最初られ、インデックス ページに戻ると、ブラウザーを変更するユーザーに表示されます。

![予算を 0 に変更されました](concurrency/_static/budget-zero.png)

John がその**保存**ドル 350,000.00 の予算が表示される編集ページにします。 次の動作は、同時実行の競合を処理する方法によって決まります。

オプションの一部を以下に示します。

* プロパティをユーザーが変更を追跡し、データベースに対応する列のみを更新できます。

     例のシナリオで失われるデータなし、2 人のユーザーによって更新されたプロパティが異なるためです。 次回の英語版の部門を参照するユーザーがジェーンのと John の両方の変更 - 2013 年 9 月 1 日の開始日と 0 個のドルのコストの予算が表示されます。 更新には、このメソッドは、データが失われる可能性のある競合の数を減らすことができますが、エンティティの同じプロパティに競合する変更が加えられた場合は、データ損失を防ぐことことはできません。 この方法を Entity Framework で動作するかどうかは、更新プログラムのコードを実装する方法によって異なります。 必要のエンティティの元のプロパティ値をすべてと新しい値を追跡するためには、大量の状態を維持することができるため、web アプリケーションで現実的では、ほとんどの場合。 サーバーのリソースが必要ですか、web ページ自体 (たとえば、非表示フィールド) 内に含める必要があるために、大量の状態を保持することと、アプリケーションのパフォーマンスが影響することができますか、クッキーにします。

* John の変更がジェーンの変更を上書きすることができます。

     次に英語版の部門を参照するユーザーが、2013 年 9 月 1 日が表示されますと復元のドル 350,000.00 値。 これと呼ばれる、*クライアントが Wins*または*Wins の最後に*シナリオです。 (クライアントからのすべての値よりも優先、データ ストアには、新機能です。)同時実行処理のコーディングそうしない場合は、このセクションの概要で説明したよう、これは自動的に発生します。

* John の変更は、データベースで更新されているを妨害することができます。

     通常は、エラー メッセージが表示、データの現在の状態を表示すること、および彼ようにする必要がある場合は、自分の変更を適用することを許可するがします。 これと呼ばれる、*ストア Wins*シナリオです。 (データ ストアの値よりも優先、クライアントから送信された値です。)このチュートリアルでは、ストアの Wins のシナリオを実装します。 このメソッドは、何が起こっているかに警告を表示されているユーザーがいない状態の変更が上書きされないことを確認します。

### <a name="detecting-concurrency-conflicts"></a>同時実行の競合を検出します。

競合を解決するには処理することにより`DbConcurrencyException`Entity Framework がスローする例外。 これらの例外をスローするタイミングを知るために、Entity Framework は、競合を検出できる必要があります。 そのため、データベースとデータ モデルを適切に構成する必要があります。 競合の検出を有効にするためのいくつかのオプションを以下に示します。

* データベース テーブルを特定の行が変更されたときに使用できる追跡列を含めます。 Where 句でその列を含めるように、Entity Framework を構成することができますし、SQL の Update または Delete コマンドの句。

     追跡列のデータ型は、通常`rowversion`です。 `rowversion`値は、行が更新されるたびにインクリメントが連続番号です。 Update または Delete コマンドで、Where 句には、追跡列 (元の行バージョン) の元の値が含まれています。 更新された行が他のユーザーの値が変更されている場合、 `rowversion` Update または Delete ステートメントは Where により更新する行を見つけることはできませんので、列は、元の値とは異なる句。 行が更新または削除によって更新されていない Entity Framework 検索コマンドの (つまり、影響を受けた行の数が 0 である場合)、時として同時実行の競合を解釈します。

* Where 句でテーブルのすべての列の元の値に Entity Framework を構成する Update および Delete コマンドの句。

     最初のオプションの場合は、行の任意のものが変更されたため、行が読み取られた最初のように Where 句が返されません、行を更新するには、Entity Framework は同時実行の競合として解釈します。 多くの列を持つデータベース テーブルでは、このアプローチされる可能性が非常に大規模な Where 句、大量の状態を維持することが必要とします。 前に述べたようには、アプリケーションのパフォーマンスに影響大量の状態を維持することができます。 そのためこの方法は、通常、推奨されませんし、このチュートリアルで使用する方法はありません。

     追加することでの同時実行制御を追跡するエンティティのすべての非主キー プロパティをマークする必要がある同時実行するには、このアプローチを実装する場合、`ConcurrencyCheck`属性をします。 その変更には、Update および Delete ステートメントの SQL の Where 句にすべての列を含める Entity Framework が有効になります。

このチュートリアルの残りの部分では追加、 `rowversion` Department エンティティを追跡するプロパティ、コント ローラーとビューを作成し、すべてが正常に動作することを確認します。

## <a name="add-a-tracking-property-to-the-department-entity"></a>追跡プロパティ、Department エンティティを追加します。

*Models/Department.cs*RowVersion をという名前の追跡のプロパティを追加します。

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

`Timestamp`属性は、この列が含まれることを指定します、Where 句で Update および Delete コマンドの句は、データベースに送信します。 属性といいます`Timestamp`旧バージョンの SQL Server、SQL を使用するため`timestamp`データ型に、SQL`rowversion`に置き換えられます。 .NET 型を`rowversion`バイト配列。

Fluent API を使用する場合は、使用、`IsConcurrencyToken`メソッド (で*Data/SchoolContext.cs*) プロパティを指定する、追跡、次の例で示すようにします。

```csharp
modelBuilder.Entity<Department>()
    .Property(p => p.RowVersion).IsConcurrencyToken();
```

プロパティを追加することで変更した場合、データベース モデル別の移行を実行する必要があります。

変更を保存し、プロジェクトをビルドし、コマンド ウィンドウで、次のコマンドを入力します。

```console
dotnet ef migrations add RowVersion
```

```console
dotnet ef database update
```

## <a name="create-a-departments-controller-and-views"></a>部門のコント ローラーとビューを作成します。

学生、コース、および講習においてインストラクター以前実行した方法では、部門のコント ローラーとビューをスキャフォールディングです。

![Scaffold 部門](concurrency/_static/add-departments-controller.png)

*DepartmentsController.cs*ファイルは、部門の管理者のドロップダウン リストは、インストラクターの完全な名前ではなく最後の名前だけが含まれるように、"FullName""FirstMidName"の 4 つすべての項目を変更します。

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_Dropdown)]

## <a name="update-the-departments-index-view"></a>部門インデックス ビューを更新します。

スキャフォールディング エンジンでは、インデックス ビューで RowVersion 列が作成された日時が、そのフィールドを表示することはできません。

コードに置き換えます*Views/Departments/Index.cshtml*を次のコード。

[!code-html[Main](intro/samples/cu/Views/Departments/Index.cshtml?highlight=4,7,44)]

RowVersion 列を削除、および管理者の名の代わりに完全名を示しますこの「部門」に見出しを変更します。

## <a name="update-the-edit-methods-in-the-departments-controller"></a>更新プログラムの部門コント ローラーの編集方法

両方の HttpGet で`Edit`メソッドおよび`Details`メソッド、追加`AsNoTracking`です。 HttpGet`Edit`メソッド、一括読み込みの管理者を追加します。

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EagerLoading&highlight=2,3)]

HttpPost の既存のコードを置き換えます`Edit`メソッドを次のコード。

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_EditPost)]

コードを更新する部門を読み取ろうとして開始します。 場合、 `SingleOrDefaultAsync` null が返されます、部門別のユーザーが削除されました。 その場合は、コードは、エラー メッセージでの編集 ページを再表示できるように、department エンティティを作成する、ポストされたフォーム値を使用します。 代わりは、部門のフィールドの再表示せず、エラー メッセージのみを表示する場合に、department エンティティを再作成する必要はありません。

ビュー、元の格納`RowVersion`の値の非表示フィールドの場合は、このメソッド内でその値を受信する、`rowVersion`パラメーター。 呼び出す前に`SaveChanges`、元の配置を`RowVersion`プロパティ値を`OriginalValues`エンティティのコレクション。

```csharp
_context.Entry(departmentToUpdate).Property("RowVersion").OriginalValue = rowVersion;
```

そのコマンドが元のある行を検索する WHERE 句は、Entity Framework では、SQL の UPDATE コマンドを作成するとき、`RowVersion`値。 更新コマンドで行に影響がない場合 (行には、元のあるありません`RowVersion`値)、Entity Framework をスロー、`DbUpdateConcurrencyException`例外。

その例外のキャッチ ブロック内のコード エンティティを取得します影響を受ける部署をから更新された値を持つ、`Entries`例外オブジェクトのプロパティです。

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=164)]

`Entries`コレクションでは、1 つがあります`EntityEntry`オブジェクト。  そのオブジェクトを使用すると、ユーザーが入力した新しい値と現在のデータベースの値を取得します。

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=165-166)]

コードでは、データベースの値を持つ異なるページからユーザーが、編集時に入力した各列のカスタム エラー メッセージを追加する (1 つだけのフィールドは、次に示す簡略化のため)。

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=174-178)]

コードの最後に、設定、`RowVersion`の値、`departmentToUpdate`新しい値をデータベースから取得します。 この新しい`RowVersion`ページが表示され、編集および次の時間をユーザーがクリックしたときに非表示のフィールドに保存する値**保存**の編集 ページの再表示が検出されるために発生する同時実行エラーのみです。

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?range=199-200)]

`ModelState.Remove`ために、ステートメントは必要な`ModelState`が古い`RowVersion`値。 ビューで、`ModelState`フィールドよりも優先、モデル プロパティの値が両方に存在する場合の値します。

## <a name="update-the-department-edit-view"></a>更新プログラム ビューの部門の編集

*Views/Departments/Edit.cshtml*、次の変更します。

* 保存する隠しフィールドを追加、`RowVersion`プロパティの値、用の隠しフィールドの直後、`DepartmentID`プロパティです。

* 「管理者の選択」オプションをドロップダウン リストに追加します。

[!code-html[Main](intro/samples/cu/Views/Departments/Edit.cshtml?highlight=16,34-36)]

## <a name="test-concurrency-conflicts-in-the-edit-page"></a>[編集] ページでの同時実行の競合をテストします。

アプリを実行して、部門インデックス ページに移動します。 右クリックし、**編集**ハイパーリンクをクリックし、英語部門**新しいタブで開く**、をクリックして、**編集**英語部門のハイパーリンクのです。 2 つのブラウザー タブでは、同じ情報が表示されます。

最初のブラウザー タブ内のフィールドを変更し、クリックして**保存**です。

![部門編集変更後のページ 1](concurrency/_static/edit-after-change-1.png)

ブラウザーでは、変更された値を持つインデックス ページを示します。

2 番目のブラウザー タブ内のフィールドを変更します。

![部門編集変更した後に 2 ページ目](concurrency/_static/edit-after-change-2.png)

**[保存]**をクリックします。 エラー メッセージを参照してください。

![部門の編集 ページのエラー メッセージ](concurrency/_static/edit-error.png)

をクリックして**保存**もう一度です。 2 番目のブラウザー タブで入力した値は保存されます。 保存された値が表示されるは、インデックス ページが表示されたときです。

## <a name="update-the-delete-page"></a>更新プログラムの削除 ページ

削除 ページは、Entity Framework は、それ以外の場合と同様の方法で、部門の編集のユーザーによる同時実行の競合を検出します。 ときに、HttpGet`Delete`メソッドは、確認のビューを表示、ビューに含まれる元`RowVersion`隠しフィールドの値。 値が、HttpPost を使用できますが、そのこと`Delete`ユーザー削除を確認するときに呼び出されるメソッド。 Entity Framework では、SQL の DELETE コマンドを作成するとき、元の WHERE 句が含まれます`RowVersion`値。 0 個の行で、コマンドの結果 (つまり、削除の確認 ページが表示された後に、行が変更された)、影響を受ける場合は、同時実行例外がスローされ、および、HttpGet`Delete`メソッドは、エラー フラグを再表示するためには、true に設定して、エラー メッセージを確認 ページ。 その場合のエラー メッセージが表示されないように、別のユーザーによって行が削除されたため 0 行が影響を受けたこともできます。

### <a name="update-the-delete-methods-in-the-departments-controller"></a>更新プログラムの部門コント ローラーの削除方法

*DepartmentsController.cs*、置換、HttpGet`Delete`メソッドを次のコード。

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeleteGet&highlight=1,10,14-17,21-29)]

このメソッドは、同時実行エラーの後、ページが表示されされているかどうかを示す省略可能なパラメーターを受け取ります。 このフラグが true、不要になったに指定された部門が存在する場合は、別のユーザーによって削除されました。 その場合は、コードは、インデックス ページにリダイレクトします。  このフラグが true であり、部門が存在して場合、は、別のユーザーによって変更されました。 ビューを使用するコードがエラー メッセージを送信する場合は、`ViewData`です。  

コード、HttpPost で置き換え`Delete`メソッド (名前付き`DeleteConfirmed`) を次のコード。

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_DeletePost&highlight=1,3,5-8,11-18)]

交換したスキャフォールディングのコードでは、このメソッドは、レコード ID のみを使用できます。


```csharp
public async Task<IActionResult> DeleteConfirmed(int id)
```

このパラメーターは、モデル バインダーによって作成された部門エンティティ インスタンスに変更しました。 これにより、レコードのキーだけでなく RowVersion プロパティの値に EF アクセスします。

```csharp
public async Task<IActionResult> Delete(Department department)
```

アクション メソッドの名前も変更した`DeleteConfirmed`に`Delete`です。 スキャフォールディングのコードには、名前が使用されて`DeleteConfirmed`HttpPost メソッドに一意のシグネチャを指定します。 (CLR には、別のメソッドのパラメーターを持つオーバー ロードされたメソッドが必要があります)。これで、署名が一意では MVC 規則のオプションを使用し、同じ名前、HttpPost、HttpGet delete メソッドを使用できます。

場合は、部門は既に削除されて、`AnyAsync`メソッドは false を返し、アプリケーションだけに戻るインデックス メソッドです。

同時実行エラーが検出された場合、コードは削除の確認 ページを再表示し、それを示すフラグは同時実行エラー メッセージを表示する必要がありますを提供します。

### <a name="update-the-delete-view"></a>削除ビューを更新します。

*Views/Departments/Delete.cshtml*、スキャフォールディングのコードをエラー メッセージ フィールドと DepartmentID および RowVersion のプロパティの非表示フィールドを追加する次のコードに置き換えます。 変更が強調表示されます。

[!code-html[Main](intro/samples/cu/Views/Departments/Delete.cshtml?highlight=9,38,44,45,48)]

これは、次の変更を加えます。

* 間でエラー メッセージを追加、`h2`と`h3`見出し。

* 置き換えますで FullName FirstMidName、**管理者**フィールドです。

* RowVersion フィールドを削除します。

* 隠しフィールドを追加、`RowVersion`プロパティです。

アプリを実行して、部門インデックス ページに移動します。 右クリックし、**削除**ハイパーリンクをクリックし、英語部門**新しいタブで開く**、最初のタブをクリックして、**編集**英語部門のハイパーリンクのです。

最初のウィンドウで、値のいずれかを変更し、クリックして**保存**:

![削除する前に変更した後に部門の編集 ページ](concurrency/_static/edit-after-change-for-delete.png)

2 番目のタブをクリックして**削除**です。 同時実行エラー メッセージが表示および部門値が現在のデータベースにあるもので更新します。

![同時実行エラーのため、部門削除の確認 ページ](concurrency/_static/delete-error.png)

クリックすると**削除**もう一度、部門が削除されたことを示しています。 インデックスのページにリダイレクトしています。

## <a name="update-details-and-create-views"></a>更新プログラムの詳細と、ビューの作成

必要に応じて詳細にスキャフォールディング コードをクリーンアップしてビューを作成できます。

コードに置き換えます*Views/Departments/Details.cshtml* RowVersion 列を削除し、管理者の完全な名前を表示します。

[!code-html[Main](intro/samples/cu/Views/Departments/Details.cshtml?highlight=35)]

コードに置き換えます*Views/Departments/Create.cshtml*選択オプションをドロップダウン リストに追加します。

[!code-html[Main](intro/samples/cu/Views/Departments/Create.cshtml?highlight=32-34)]

## <a name="summary"></a>まとめ

これは、同時実行の競合の処理の概要を完了します。 EF Core での同時実行を処理する方法の詳細については、次を参照してください。[同時実行の競合](https://docs.microsoft.com/ef/core/saving/concurrency)です。 次のチュートリアルでは、インストラクターと学生エンティティのテーブルの階層あたりの継承を実装する方法を示します。

>[!div class="step-by-step"]
[前へ](update-related-data.md)
[次へ](inheritance.md)  
