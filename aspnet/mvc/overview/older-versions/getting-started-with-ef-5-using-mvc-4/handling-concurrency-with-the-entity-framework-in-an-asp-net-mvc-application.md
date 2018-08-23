---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: ASP.NET MVC アプリケーション (7/10) で Entity Framework での同時実行の処理 |Microsoft Docs
author: tdykstra
description: Contoso University のサンプルの web アプリケーションでは、Entity Framework 5 Code First と Visual Studio を使用して ASP.NET MVC 4 アプリケーションを作成する方法について説明しています.
ms.author: riande
ms.date: 07/30/2013
ms.assetid: b83f47c4-8521-4d0a-8644-e8f77e39733e
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 07d4d673b7bb6bad6e9d8cbacbc965a60608db2a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832827"
---
<a name="handling-concurrency-with-the-entity-framework-in-an-aspnet-mvc-application-7-of-10"></a>ASP.NET MVC アプリケーション (7/10) で Entity Framework での同時実行の処理
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[完成したプロジェクトのダウンロード](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso University のサンプルの web アプリケーションでは、Entity Framework 5 Code First と Visual Studio 2012 を使用して ASP.NET MVC 4 アプリケーションを作成する方法を示します。 チュートリアル シリーズについては、[シリーズの最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)をご覧ください。 チュートリアルのシリーズを開始するには、最初からまたは[この章のスタート プロジェクトをダウンロード](building-the-ef5-mvc4-chapter-downloads.md)し、ここから始めてください。
> 
> > [!NOTE] 
> > 
> > を解決できない問題が生じた場合[章では、完了したダウンロード](building-the-ef5-mvc4-chapter-downloads.md)の問題を再現しようとします。 問題の解決策は、完成したコードにコードを比較することによって一般的に見つかります。 一般的なエラーとその解決方法は、次を参照してください。[エラーと回避策。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


前の 2 つのチュートリアルでは、関連するデータと連携してください。 このチュートリアルでは、同時実行を処理する方法を示します。 使用する web ページを作成します、`Department`エンティティ、およびページの編集し、削除を`Department`エンティティは、同時実行エラーを処理します。 次の図は、同時実行の競合が発生した場合に表示される一部のメッセージを含む、インデックスと Delete ページを示しています。

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="concurrency-conflicts"></a>同時実行の競合

あるユーザーがあるエンティティのデータを編集目的で表示したとき、別のユーザーが同じエンティティのデータを最初のユーザーの変更がデータベースに書き込まれる前に更新すると、同時実行の競合が発生します。 このような競合の検出を有効にしないと、最後にデータベースを更新したユーザーが他のユーザーの変更を上書きすることになります。 多くのアプリケーションでは、このリスクが許容されています。ユーザーや更新がわずかであれば、あるいは変更が一部上書きされても大きな問題なければ、同時実行のプログラミングにかかるコストが利点よりも重視されることがあります。 その場合、同時実行の競合を処理するようにアプリケーションを構成する必要はありません。

### <a name="pessimistic-concurrency-locking"></a>ペシミスティック同時実行制御 (ロック)

同時実行で偶発的にデータが失われる事態をアプリケーションで回避する必要があれば、その方法としてデータベース ロックがあります。 これは呼び出されます*ペシミスティック同時実行制御*します。 たとえば、データベースから行を読む前に、読み取り専用か更新アクセスでロックを要求します。 更新アクセスで行をロックすると、他のユーザーはその行を読み取り専用または更新アクセスでロックできなくなります。変更中のデータのコピーが与えられるためです。 読み取り専用で行をロックすると、他のユーザーはその行を読み取り専用でロックできますが、更新アクセスではロックできません。

ロックの利用には短所があります。 プログラムが複雑になります。 必要な重大なデータベース管理のリソースとアプリケーションのユーザーの数とパフォーマンスの問題が生じる増加 (つまり、適切にスケールされない)。 そのような理由から、一部のデータベース管理システムはペシミスティック同時実行制御に対応していません。 Entity Framework は、組み込みのサポートしていませんし、チュートリアルのこの方法は説明しませんが実装するためにします。

### <a name="optimistic-concurrency"></a>オプティミスティック同時実行制御

ペシミスティック同時実行制御の代わりに、*オプティミスティック同時実行制御*します。 オプティミスティック同時実行制御では、同時実行の競合の発生を許し、発生したら適切に対処します。 たとえば、John が部門の編集 ページで変更を実行、**予算**$350,000.00 から $0.00 に English 部署の量。

![Changing_English_dept_budget_to_100000](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

John が前に**保存**、Jane が、同じページの追加と変更を実行、 **Start Date**フィールドを 2007 年 9 月 1 日から 8/8/2013 にします。

![Changing_English_dept_start_date_to_1999](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

John が**保存**最初と認識し Jane、インデックス ページに、ブラウザーが返されるときに、変更をクリックした**保存**します。 この後の動作は、同時実行の競合の処理方法によって決定します。 次のようなオプションがあります。

- ユーザーが変更したプロパティを追跡記録し、それに該当する列だけをデータベースで更新できます。 例のシナリオでは、2 人のユーザーが異なるプロパティを更新したため、データは失われません。 次回の誰かが English 部署を閲覧、John のと Jane の変更が表示されます-2013 年 8 月 8 の開始日および予算が 0 ドルです。

    この更新方法では、データの損失につながる可能性がある競合の数を減らすことができますが、あるエンティティの同じプロパティに対して行われた変更が競合する場合、データの損失は避けられません。 Entity Framework がこのように動作するかどうかは、更新コードの実装方法に依存します。 これは Web アプリケーションの場合、実用的ではない場合が多いです。あるエンティティの新しい値に加え、元のプロパティ値もすべて追跡記録するため、大量のステータスを更新することになるからです。 大量の状態を維持するサーバーのリソースが必要ですか (たとえば、非表示フィールド) で、web ページ自体に含める必要があるため、アプリケーションのパフォーマンスに影響ことができます。
- Jane の変更の John の変更を上書きすることができます。 次回の誰かが English 部署を閲覧、2013 年 8 月 8 が表示されますが、復元された $350,000.00 の値。 これは *Client Wins* (クライアント側に合わせる) シナリオまたは *Last in Wins* (最終書き込み者優先) シナリオと呼ばれています。 (クライアントの値よりも優先データ ストアの新機能です。)このセクションの冒頭でお伝えしたように、同時実行処理について何のコーディングもしない場合、これが自動的に行われます。
- Jane の変更は、データベースに更新されないようにできます。 通常、エラー メッセージが表示は、データの現在の状態を表示させます、彼女できるようにする必要がある場合は、その変更を再適用できるようにと。 これは *Store Wins* (ストア側に合わせる) シナリオと呼ばれています。 (クライアントが送信した値よりデータストアの値が優先されます。)このチュートリアルでは、Store Wins シナリオを実装します。 この手法では、変更が上書きされるとき、それが必ずユーザーに警告されます。

### <a name="detecting-concurrency-conflicts"></a>同時実行競合の検出

処理することによって競合を解決する[OptimisticConcurrencyException](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) Entity Framework がスローする例外。 このような例外がスローされるタイミングを認識する目的で、Entity Framework は競合を検出できなければなりません。 そのため、データベースとデータ モデルを適宜構成する必要があります。 競合検出を有効にするためのオプションには次のようなものがあります。

- 行が変更されたタイミングを判断するトラッキング列をデータベース テーブルに追加します。 その列を含めるに Entity Framework を構成することができますし、 `Where` sql 句`Update`または`Delete`コマンド。

    追跡列のデータ型は、通常[rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)します。 [Rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)値は、行が更新されるたびにインクリメントが番号が付加されます。 `Update`または`Delete`コマンド、`Where`句にトラッキング列 (最初の行バージョン) の元の値が含まれています。 別のユーザーの値によって更新される行が変更された場合、`rowversion`列は元の値と異なるため、`Update`または`Delete`ステートメントのために更新する行を見つけることができません、`Where`句。 Entity Framework に、行が更新されていないことが見つかると、`Update`または`Delete`(つまり、影響を受けた行の数が 0 である場合) をコマンドを同時実行の競合として解釈します。
- Entity Framework 内のテーブルにすべての列の元の値を含めるを構成、`Where`の句`Update`と`Delete`コマンド。

    最初のオプションで、行が最初に読み取らため、行のすべてのものが変更された場合のように、`Where`句しない行を返すを更新する同時実行の競合として解釈し、Entity Framework。 多くの列を含むデータベース テーブルでは、このアプローチにより、非常に大きな`Where`句、および大量の状態を維持することを要求することができます。 前述のように、大量の状態を保持することと、サーバーのリソースが必要ですか、web ページ自体に含める必要があるためにはアプリケーションのパフォーマンスが影響ことができます。 そのためこの方法は推奨できませんし、このチュートリアルで使用する方法はありません。

    すべての非主キー プロパティの同時実行制御を追加することで追跡するエンティティをマークする必要がある同時実行するには、このアプローチを実装する場合、 [ConcurrencyCheck](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx)属性をします。 Entity Framework、SQL にすべての列を含めることにより`WHERE`の句`UPDATE`ステートメント。

このチュートリアルの残りの部分では追加します、 [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)プロパティを追跡、`Department`エンティティが、コント ローラーとビューを作成して、すべてが正常に動作することを確認します。

## <a name="add-an-optimistic-concurrency-property-to-the-department-entity"></a>オプティミスティック同時実行制御プロパティ Department エンティティを追加します。

*Models\Department.cs*、という名前の追跡プロパティを追加`RowVersion`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=18-19)]

[タイムスタンプ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx)属性は、この列に含まれることを指定します、`Where`の句`Update`と`Delete`データベースに送信されたコマンド。 属性がという名前[タイムスタンプ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx)以前のバージョンの SQL Server、SQL を使用するため、[タイムスタンプ](https://msdn.microsoft.com/library/ms182776(v=SQL.90).aspx)データ型、SQL に[rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)に置き換えられます。 用の .Net 型`rowversion`バイト配列です。 Fluent API を使用する場合は、使用、 [IsConcurrencyToken](https://msdn.microsoft.com/library/gg679501(v=VS.103).aspx)次の例に示すように、トラッキング プロパティを指定します。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

プロパティを追加し、データベース モデルを変更したので、別の移行を行う必要があります。 パッケージ マネージャー コンソール (PMC) で、次のコマンドを入力します。

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="create-a-department-controller"></a>部門のコント ローラーを作成します。

作成、`Department`コント ローラーとビューの次の設定を使用して他のコント ローラーを実行したのと同様。

![Add_Controller_dialog_box_for_Department_controller](https://asp.net/media/2578041/Windows-Live-Writer_Handling-C.NET-MVC-Application-7-of-10h1_AFDC_Add_Controller_dialog_box_for_Department_controller_d1d9c788-f970-4d6a-9f5a-1eddc84330b7.png)

*Controllers\DepartmentController.cs*、追加、`using`ステートメント。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

"LastName"を"FullName"このファイル (4 回) 内にあるすべての変更できるように、部門管理者のドロップダウン リストが格納されます姓だけではなく、インストラクターの完全名。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

既存のコードを置き換える、 `HttpPost` `Edit`メソッドを次のコード。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

ビューは、元を格納する`RowVersion`非表示フィールドの値。 モデル バインダーを作成する場合、`department`インスタンス、そのオブジェクトには、元に`RowVersion`プロパティの値と他のプロパティを編集 ページで、ユーザーが入力したとおりの新しい値。 SQL を作成と、Entity Framework`UPDATE`コマンド、コマンドが含まれる、`WHERE`元を含む行を検索する句`RowVersion`値。

行に影響がない場合、`UPDATE`コマンド (行には、元のあるありません`RowVersion`値)、Entity Framework がスローされます、`DbUpdateConcurrencyException`例外、およびコードでは、`catch`ブロックを取得、影響を受ける`Department`例外からのエンティティオブジェクト。 このエンティティは、データベースから読み取られた値と、ユーザーが入力した新しい値の両方があります。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

次に、コードの編集 ページで、ユーザーが入力した異なるデータベース値を持つ各列のカスタム エラー メッセージを追加します。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

長いエラー メッセージは、変更点と、それについての対処方法について説明します。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

コードの最後に、設定、`RowVersion`の値、`Department`データベースからオブジェクトを新しい値を取得します。 Edit ページが再表示されるとき、この新しい `RowVersion` 値が非表示フィールドに保存されます。今度ユーザーが **[保存]** をクリックすると、Edit ページの再表示後に発生した同時実行エラーのみが取得されます。

*Views\Department\Edit.cshtml*、保存する非表示フィールドを追加、`RowVersion`用の隠しフィールドのすぐ後、プロパティの値、`DepartmentID`プロパティ。

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml?highlight=17)]

*Views\Department\Index.cshtml*、既存のコード行へのリンクを左に移動し、表示するページのタイトルと列見出しを変更するには、次のコードに置き換えます`FullName`の代わりに`LastName`で**管理者**列。

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]

## <a name="testing-optimistic-concurrency-handling"></a>オプティミスティック同時実行処理のテスト

サイトを実行し、をクリックして**部門**:

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

右クリックして、**編集**ハイパーリンクを選択して Kim Abercrombie**新しいタブで開く**順にクリックして、**編集**Kim Abercrombie のハイパーリンクです。 2 つのウィンドウでは、同じ情報を表示します。

![Department_Edit_page_before_changes](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

最初のブラウザー ウィンドウ内のフィールドを変更し、クリックして**保存**します。

![Department_Edit_page_1_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

値が変更された Index ページがブラウザーに表示されます。

![Departments_Index_page_after_first_budget_edit](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

2 番目のブラウザー ウィンドウで、フィールドを変更し、クリックして**保存**します。

![Department_Edit_page_2_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

クリックして**保存**2 つ目のブラウザー ウィンドウ。 エラー メッセージが表示されます。

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

**[保存]** をもう一度クリックします。 2 番目のブラウザーで入力した値は、最初のブラウザーで変更するデータの元の値と共に保存されます。 Index ページが表示されると、保存した値を確認できます。

![Department_Index_page_with_change_from_second_browser](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Delete ページの更新

Delete ページの場合、Entity Framework は、同様の方法で部署を編集している他のユーザーが起した同時実行の競合を検出します。 ときに、 `HttpGet` `Delete`メソッドが確定ビューが表示されます、ビュー、元に含まれる`RowVersion`非表示フィールドの値。 値が使用できる、そのこと、 `HttpPost` `Delete`ユーザーが、削除するときに呼び出されるメソッド。 Entity Framework で SQL を作成するときに`DELETE`コマンドが含まれています、`WHERE`句と元`RowVersion`値。 行が 0 で、コマンドの結果に (つまり、削除の確認ページが表示された後、行が変更された) が影響を受ける場合は、同時実行例外がスローされます、および`HttpGet Delete`エラー フラグを設定するメソッドが呼び出された`true`再表示するには、エラー メッセージの確認 ページ。 別のエラー メッセージが表示される場合は、行が別のユーザーによって削除されたため、0 行が影響を受けたこともできます。

*DepartmentController.cs*、置換、 `HttpGet` `Delete`メソッドを次のコード。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

このメソッドは、同時実行エラー後にページが再表示されたかどうかを示すオプション パラメーターを受け取ります。 このフラグが場合`true`、ビューを使用して、エラー メッセージを送信、`ViewBag`プロパティ。

コードに置き換えます、 `HttpPost` `Delete`メソッド (名前付き`DeleteConfirmed`) を次のコード。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

置き換えたスキャフォールディングされたコードで、このメソッドがレコード ID を 1 つだけ受け取りました。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

このパラメーターを変更して、`Department`モデル バインダーによって作成されたエンティティ インスタンス。 これによりへのアクセス、`RowVersion`レコード キーに加え、プロパティの値。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

また、アクション メソッドの名前を `DeleteConfirmed` から `Delete` に変更しています。 という名前のスキャフォールディングされたコード、 `HttpPost` `Delete`メソッド`DeleteConfirmed`提供する、`HttpPost`メソッドに一意のシグネチャ。 (CLR では、さまざまなメソッド パラメーターを持つために、オーバーロードされたメソッドを必要とします。)シグネチャが一意ではこれでは、MVC 規則に従うし、に同じ名前を使用、`HttpPost`と`HttpGet`メソッドを削除します。

同時実行エラーがキャッチされた場合、このコードは削除確認ページを再表示し、同時実行エラー メッセージを表示するかどうかを示すフラグを提供します。

*Views\Department\Delete.cshtml*置換を書式設定の一部は、次のコードのスキャフォールディングされたコードが変更され、エラー メッセージ フィールドを追加します。 変更が強調表示されます。

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml?highlight=9,37,40,45-46)]

このコードは、間にエラー メッセージを追加、`h2`と`h3`見出し。

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

置き換える`LastName`で`FullName`で、`Administrator`フィールド。

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

最後に、非表示フィールドを追加、`DepartmentID`と`RowVersion`プロパティの後、`Html.BeginForm`ステートメント。

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Departments Index ページを実行します。 右クリックして、**削除**ハイパーリンクをクリックし、English 部署**新しいウィンドウで開く**し、最初のウィンドウでをクリックして、**編集**英語版のハイパーリンク部門。

最初のウィンドウで、値のいずれかを変更し、をクリックして**保存**:

![Department_Edit_page_after_change_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

インデックス ページが変更を確認します。

![Departments_Index_page_after_budget_edit_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)

2 番目のウィンドウで次のようにクリックします。**削除**します。

![Department_Delete_confirmation_page_before_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

同時実行エラー メッセージが表示されます。Department 値がデータベースの現在の内容で更新されています。

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

**[削除]** をもう一度クリックすると、Index ページにリダイレクトされます。Index ページには、部署が削除されていることが表示されます。

## <a name="summary"></a>まとめ

同時実行の競合処理の入門編はこれで終わりです。 同時実行のさまざまなシナリオを処理する他の方法については、次を参照してください。[オプティミスティック同時実行パターン](https://blogs.msdn.com/b/adonet/archive/2011/02/03/using-dbcontext-in-ef-feature-ctp5-part-9-optimistic-concurrency-patterns.aspx)と[プロパティの値を操作](https://blogs.msdn.com/b/adonet/archive/2011/01/30/using-dbcontext-in-ef-feature-ctp5-part-5-working-with-property-values.aspx)Entity Framework チームのブログ。 次のチュートリアル用の table-per-hierarchy 継承を実装する方法を示しています、`Instructor`と`Student`エンティティ。

その他の Entity Framework リソースへのリンクが記載されて、 [ASP.NET データ アクセス コンテンツ マップ](../../../../whitepapers/aspnet-data-access-content-map.md)します。

> [!div class="step-by-step"]
> [前へ](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [次へ](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
