---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: "ASP.NET MVC アプリケーション (10 の 7) で Entity Framework での同時実行の処理 |Microsoft ドキュメント"
author: tdykstra
description: "Contoso 大学でサンプル web アプリケーションでは、Entity Framework 5 Code First と Visual Studio を使用して ASP.NET MVC 4 アプリケーションを作成する方法について説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: b83f47c4-8521-4d0a-8644-e8f77e39733e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: b072134043ceda809bfeca98447a132ed407b323
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="handling-concurrency-with-the-entity-framework-in-an-aspnet-mvc-application-7-of-10"></a>ASP.NET MVC アプリケーション (10 の 7) で Entity Framework での同時実行の処理
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[完成したプロジェクトをダウンロードします。](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso 大学でサンプル web アプリケーションでは、Entity Framework 5 Code First と Visual Studio 2012 を使用して ASP.NET MVC 4 アプリケーションを作成する方法を示します。 一連のチュートリアルについては、次を参照してください。[系列内の最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)です。 一連のチュートリアルを開始するには、最初からまたは[この章のスタート プロジェクトをダウンロード](building-the-ef5-mvc4-chapter-downloads.md)し、ここから開始します。
> 
> > [!NOTE] 
> > 
> > 解決できない場合、問題が発生した場合[ダウンロード完了章](building-the-ef5-mvc4-chapter-downloads.md)の問題を再現しようとします。 一般に、コードを完成したコードを比較することによって、問題の解決策を検索できます。 一般的なエラーとそれらを解決する方法は、次を参照してください。[エラーと回避策です。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


前の 2 つのチュートリアルでは、関連するデータを使用していた。 このチュートリアルでは、同時実行を処理する方法を示します。 処理する web ページを作成、`Department`エンティティ、およびページの編集し、削除を`Department`エンティティは同時実行エラーを処理します。 次の図は、同時実行の競合が発生した場合に表示される一部のメッセージなどのインデックスと削除のページを表示します。

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="concurrency-conflicts"></a>同時実行の競合

1 人のユーザーが、編集するために、エンティティのデータを表示し、別のユーザーが最初のユーザーの変更がデータベースに書き込まれる前に、同じエンティティのデータを更新し、同時実行の競合が発生します。 このような競合の検出を有効にしない場合、データベースを更新したユーザーは、他のユーザーの変更を最後上書きされます。 多くのアプリケーションでこのようなリスクが許容可能な: 数名のユーザー、またはいくつかの更新プログラムがある場合、または場合いない本当に重要な場合は、いくつかの変更が上書きされると、同時実行のプログラミングのコストがのメリットを上回る可能性があります。 その場合は、同時実行の競合を処理するアプリケーションを構成する必要はありません。

### <a name="pessimistic-concurrency-locking"></a>ペシミスティック同時実行制御 (ロック)

場合は、アプリケーションは同時実行シナリオの偶発的なデータ損失を防ぐため、これを行うにはデータベース ロックを使用することができます。 これと呼ばれる*ペシミスティック同時実行制御*です。 たとえば、データベースから行を参照する前にするロックを要求の読み取り専用または更新アクセスします。 場合更新アクセス権の行をロックすると、他のユーザーは許可されませんのいずれかの行をロックする読み取り専用かが変更されているプロセスでデータのコピーを取得するために、アクセスを更新します。 場合は読み取り専用アクセスの行をロックすると、他のユーザーもロックできます読み取り専用アクセス用のではなく更新します。

ロックの管理、短所があります。 プログラムに複雑なことができます。 必要な管理リソースは大量のデータベース、およびアプリケーションのユーザーの数とパフォーマンスの問題を引き起こす可能性が増加 (つまり、適切にスケールされない)。 これらの理由は、すべてのデータベース管理システムは、ペシミスティック同時実行をサポートします。 Entity Framework には、組み込みのサポートはありません、このチュートリアルは示してそれを実装する方法。

### <a name="optimistic-concurrency"></a>オプティミスティック同時実行制御

ペシミスティック同時実行する代わりに、*オプティミスティック同時実行制御*です。 オプティミスティック同時実行制御は、許可する同時実行の競合が発生する場合は適切に対処を意味します。 John が部門の編集 ページで変更を実行するなど、**予算**$0.00 ドル 350,000.00 から英語部門の量。

![Changing_English_dept_budget_to_100000](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

John が前に**保存**、加藤さんは、同じページと変更を実行、 **Start Date** 2013 年 8 月 8 日から 2007 年 9 月 1 日フィールドです。

![Changing_English_dept_start_date_to_1999](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

John が**保存**first と認識し、加藤さん、インデックス ページにブラウザーが返されるときに、変更をクリックした**保存**です。 次の動作は、同時実行の競合を処理する方法によって決まります。 オプションの一部を以下に示します。

- プロパティをユーザーが変更を追跡し、データベースに対応する列のみを更新できます。 例のシナリオで失われるデータなし、2 人のユーザーによって更新されたプロパティが異なるためです。 次に英語版の部門を参照する他のユーザーの John と Jane の両方の変更が表示されます: 2013 年 8 月 8 日の開始日と 0 個のドルの予算します。

    更新には、このメソッドは、データが失われる可能性のある競合の数を減らすことができますが、エンティティの同じプロパティに競合する変更が加えられた場合は、データ損失を防ぐことことはできません。 この方法を Entity Framework で動作するかどうかは、更新プログラムのコードを実装する方法によって異なります。 必要のエンティティの元のプロパティ値をすべてと新しい値を追跡するためには、大量の状態を維持することができるため、web アプリケーションで現実的では、ほとんどの場合。 大量の状態を保持することと、サーバー リソースが必要ですか、web ページ自体 (たとえば、非表示フィールド) 内に含める必要があるためにアプリケーションのパフォーマンスが影響することができます。
- John の変更が上書きジェーンの変更することができます。 次に英語版の部門を参照するユーザーが、2013 年 8 月 8 日が表示されますと復元のドル 350,000.00 値。 これと呼ばれる、*クライアントが Wins*または*Wins の最後に*シナリオです。 (クライアントの値よりも優先、データ ストアには、新機能です。)同時実行処理のコーディングそうしない場合は、このセクションの概要で説明したよう、これは自動的に発生します。
- ジェーンの変更は、データベースで更新されているを妨害することができます。 通常、エラー メッセージが表示、データの現在の状態を表示するユーザー、および彼女ようにする必要がある場合は、自分が加えた変更を再適用できるようにはします。 これと呼ばれる、*ストア Wins*シナリオです。 (データ ストアの値よりも優先、クライアントから送信された値です。)このチュートリアルでは、ストアの Wins のシナリオを実装します。 このメソッドは、何が起こっているかに警告を表示されているユーザーがいない状態の変更が上書きされないことを確認します。

### <a name="detecting-concurrency-conflicts"></a>同時実行の競合を検出します。

競合を解決するには処理することにより[OptimisticConcurrencyException](https://msdn.microsoft.com/en-us/library/system.data.optimisticconcurrencyexception.aspx) Entity Framework がスローする例外。 これらの例外をスローするタイミングを知るために、Entity Framework は、競合を検出できる必要があります。 そのため、データベースとデータ モデルを適切に構成する必要があります。 競合の検出を有効にするためのいくつかのオプションを以下に示します。

- データベース テーブルを特定の行が変更されたときに使用できる追跡列を含めます。 その列を含めるに Entity Framework を構成することができますし、 `Where` SQL の句`Update`または`Delete`コマンド。

    追跡列のデータ型は、通常[rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx)です。 [Rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx)値は、行が更新されるたびにインクリメントが連続番号です。 `Update`または`Delete`コマンド、`Where`句には、追跡列 (元の行バージョン) の元の値が含まれています。 更新された行が他のユーザーの値が変更されている場合、`rowversion`列は、元の値と異なるため、`Update`または`Delete`ステートメントがあるため更新する行を見つけることができません、`Where`句。 Entity Framework が、行が更新されていないことを見つけたとき、`Update`または`Delete`コマンドの (つまり、影響を受けた行の数が 0 である場合)、同時実行の競合として解釈します。
- テーブルのすべての列の元の値を含める Entity Framework の構成、`Where`の句`Update`と`Delete`コマンド。

    最初のオプションの場合は、行の任意のものが変更されたため、行が読み取られた最初のように、`Where`句しない行が返されます、更新するには、Entity Framework は同時実行の競合として解釈します。 多くの列を持つデータベース テーブルでは、このアプローチにつながる非常に大きな`Where`句、大量の状態を維持することが必要とします。 前述のように、大量の状態を維持することができますパフォーマンスに影響アプリケーション サーバーのリソースが必要ですか web ページ自体に含める必要があるためです。 そのためこの方法は推奨できませんし、このチュートリアルで使用する方法はありません。

    追加することでの同時実行制御を追跡するエンティティのすべての非主キー プロパティをマークする必要がある同時実行するには、このアプローチを実装する場合、 [ConcurrencyCheck](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx)属性をします。 変更により、SQL にすべての列を含める Entity Framework`WHERE`の句`UPDATE`ステートメントです。

このチュートリアルの残りの部分では追加、 [rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx)追跡するプロパティ、`Department`エンティティ、コント ローラーとビューを作成し、すべてが正常に動作することを確認します。

## <a name="add-an-optimistic-concurrency-property-to-the-department-entity"></a>オプティミスティック同時実行プロパティを Department エンティティに追加します。

*Models\Department.cs*、という名前の追跡のプロパティを追加`RowVersion`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=18-19)]

[タイムスタンプ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.timestampattribute.aspx)属性は、この列に含まれることを指定します、`Where`の句`Update`と`Delete`データベースに送信されたコマンド。 属性といいます[タイムスタンプ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.timestampattribute.aspx)旧バージョンの SQL Server、SQL を使用するため[タイムスタンプ](https://msdn.microsoft.com/en-us/library/ms182776(v=SQL.90).aspx)データ型に、SQL [rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx)に置き換えられます。 .Net 型を`rowversion`バイト配列。 Fluent API を使用する場合は、使用、 [IsConcurrencyToken](https://msdn.microsoft.com/en-us/library/gg679501(v=VS.103).aspx)メソッドを次の例で示すように、追跡プロパティを指定します。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

プロパティを追加することで変更した場合、データベース モデル別の移行を実行する必要があります。 パッケージ マネージャー コンソール (PMC) では、次のコマンドを入力します。

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="create-a-department-controller"></a>部門のコント ローラーを作成します。

作成、`Department`コント ローラーとビューで実行したその他のコント ローラーで、次の設定を使用して同じ方法。

![Add_Controller_dialog_box_for_Department_controller](https://asp.net/media/2578041/Windows-Live-Writer_Handling-C.NET-MVC-Application-7-of-10h1_AFDC_Add_Controller_dialog_box_for_Department_controller_d1d9c788-f970-4d6a-9f5a-1eddc84330b7.png)

*Controllers\DepartmentController.cs*、追加、`using`ステートメント。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

"LastName""FullName"にこのファイル (4 つのオカレンス) 内にあるすべての変更ようにが部門の管理者のドロップダウン リストに含まれる最後の名前だけではなく、インストラクターの完全な名前です。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

既存のコードを置き換える、 `HttpPost` `Edit`メソッドを次のコード。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

ビューは、元を格納する`RowVersion`隠しフィールドの値。 モデル バインダーを作成する場合、`department`インスタンス、そのオブジェクトは、元は`RowVersion`プロパティの値と、他のプロパティの編集 ページで、ユーザーが入力された新しい値。 ときに、Entity Framework は、SQL`UPDATE`コマンド、コマンドが含まれる、`WHERE`句が元のある行を検索する`RowVersion`値。

によって行に影響がない場合、`UPDATE`コマンド (行には、元のあるありません`RowVersion`値)、Entity Framework がスローされます、`DbUpdateConcurrencyException`例外、および内のコード、`catch`ブロックを取得、影響を受ける`Department`例外からのエンティティオブジェクト。 このエンティティは、データベースから読み取られた値と、ユーザーが入力した新しい値の両方には。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

次に、コードでは、カスタム エラー メッセージをデータベース内の編集 ページで入力したユーザーから別の値を持つ各列を追加します。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

長いエラー メッセージには、何が起こりについての対処方法について説明します。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

コードの最後に、設定、`RowVersion`の値、`Department`データベースから新しい値にオブジェクトを取得します。 この新しい`RowVersion`ページが表示され、編集および次の時間をユーザーがクリックしたときに非表示のフィールドに保存する値**保存**の編集 ページの再表示が検出されるために発生する同時実行エラーのみです。

*Views\Department\Edit.cshtml*、保存する隠しフィールドを追加、`RowVersion`プロパティの値、用の隠しフィールドの直後、`DepartmentID`プロパティ。

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml?highlight=17)]

*Views\Department\Index.cshtml*、行へのリンクを左に移動し、表示するページのタイトルと列見出しを変更するには、次のコードで、既存のコードを置き換えます`FullName`の代わりに`LastName`で**管理者**列。

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]

## <a name="testing-optimistic-concurrency-handling"></a>オプティミスティック同時実行処理のテスト

サイトを実行し、クリックして**部門**:

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

右クリックして、**編集**ハイパーリンクを選択して Kim Abercrombie **[新規] タブで開く**順にクリックして、**編集**Kim Abercrombie のハイパーリンクです。 2 つのウィンドウでは、同じ情報を表示します。

![Department_Edit_page_before_changes](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

最初のブラウザー ウィンドウ内のフィールドを変更し、クリックして**保存**です。

![Department_Edit_page_1_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

ブラウザーでは、変更された値を持つインデックス ページを示します。

![Departments_Index_page_after_first_budget_edit](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

2 番目のブラウザーのウィンドウで、フィールドを変更し、クリックして**保存**です。

![Department_Edit_page_2_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

をクリックして**保存**2 つ目のブラウザー ウィンドウです。 エラー メッセージを参照してください。

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

をクリックして**保存**もう一度です。 2 番目のブラウザーで入力した値は、最初のブラウザーで変更するデータの元の値と共に保存されます。 保存された値が表示されるは、インデックス ページが表示されたときです。

![Department_Index_page_with_change_from_second_browser](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>ページの削除 を更新

削除 ページは、Entity Framework は、それ以外の場合と同様の方法で、部門の編集のユーザーによる同時実行の競合を検出します。 ときに、 `HttpGet` `Delete`メソッドは、確認のビューを表示、ビューに含まれる元`RowVersion`隠しフィールドの値。 値が、使用できること、 `HttpPost` `Delete`ユーザー削除を確認するときに呼び出されるメソッド。 Entity Framework が、SQL を作成するときに`DELETE`コマンドが含まれている、`WHERE`を元の句`RowVersion`値。 0 個の行で、コマンドの結果 (つまり、削除の確認 ページが表示された後に、行が変更された)、影響を受ける場合は、同時実行例外がスローされ、および`HttpGet Delete`エラー フラグを設定するメソッドが呼び出された`true`再表示するために、エラー メッセージを確認 ページ。 その場合、別のエラー メッセージが表示されるように、別のユーザーによって行が削除されたため 0 行が影響を受けたこともできます。

*DepartmentController.cs*、置換、 `HttpGet` `Delete`メソッドを次のコード。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

このメソッドは、同時実行エラーの後、ページが表示されされているかどうかを示す省略可能なパラメーターを受け取ります。 このフラグは場合`true`を使用してビューに、エラー メッセージを送信、`ViewBag`プロパティです。

コードで置き換え、 `HttpPost` `Delete`メソッド (名前付き`DeleteConfirmed`) を次のコード。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

交換したスキャフォールディングのコードでは、このメソッドは、レコード ID のみを使用できます。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

このパラメーターを変更する、`Department`モデル バインダーによって作成されたエンティティのインスタンス。 これにより、利用できる、`RowVersion`だけでなく、レコードのキー プロパティの値。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

アクション メソッドの名前も変更した`DeleteConfirmed`に`Delete`です。 名前付きスキャフォールディング コード、 `HttpPost` `Delete`メソッド`DeleteConfirmed`を与える、`HttpPost`メソッド固有の署名。 (CLR には、別のメソッドのパラメーターを持つオーバー ロードされたメソッドが必要があります)。署名が一意ではこれでは、MVC 規則のオプションを使用と同じ名前を使用、`HttpPost`と`HttpGet`メソッドを削除します。

同時実行エラーが検出された場合、コードは削除の確認 ページを再表示し、それを示すフラグは同時実行エラー メッセージを表示する必要がありますを提供します。

*Views\Department\Delete.cshtml*置換、スキャフォールディング コードを書式設定の一部は、次のコードが変更され、エラー メッセージ フィールドを追加します。 変更が強調表示されます。

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml?highlight=9,37,40,45-46)]

このコードの間でエラー メッセージを追加する、`h2`と`h3`見出し。

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

置き換えられます`LastName`で`FullName`で、`Administrator`フィールド。

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

最後に、非表示フィールドを追加、`DepartmentID`と`RowVersion`後プロパティ、`Html.BeginForm`ステートメント。

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

部門のインデックス ページを実行します。 右クリックして、**削除**ハイパーリンクをクリックし、英語版の部門**新規ウィンドウで開いている**[最初のウィンドウ] をクリックして、**編集**英語のハイパーリンク部門。

最初のウィンドウで、値のいずれかを変更し、クリックして**保存**:

![Department_Edit_page_after_change_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

インデックス ページは、変更を確認します。

![Departments_Index_page_after_budget_edit_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)

2 番目のウィンドウで **削除**です。

![Department_Delete_confirmation_page_before_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

同時実行エラー メッセージが表示および部門値が現在のデータベースにあるもので更新します。

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

クリックすると**削除**もう一度、部門が削除されたことを示しています。 インデックスのページにリダイレクトしています。

## <a name="summary"></a>概要

これは、同時実行の競合の処理の概要を完了します。 同時実行のさまざまなシナリオを処理する方法については、次を参照してください。[オプティミスティック同時実行パターン](https://blogs.msdn.com/b/adonet/archive/2011/02/03/using-dbcontext-in-ef-feature-ctp5-part-9-optimistic-concurrency-patterns.aspx)と[プロパティ値を持つ作業](https://blogs.msdn.com/b/adonet/archive/2011/01/30/using-dbcontext-in-ef-feature-ctp5-part-5-working-with-property-values.aspx)Entity Framework チームのブログです。 次のチュートリアルでは、テーブルの階層あたり継承を実装する方法、`Instructor`と`Student`エンティティです。

その他の Entity Framework リソースへのリンクは含まれて、 [ASP.NET データ アクセス コンテンツ マップ](../../../../whitepapers/aspnet-data-access-content-map.md)です。

>[!div class="step-by-step"]
[前へ](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[次へ](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
