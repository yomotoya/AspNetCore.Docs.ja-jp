---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: エンティティ フレームワークのシナリオを高度な MVC Web アプリケーション (10 10) の |Microsoft ドキュメント
author: tdykstra
description: Contoso 大学でサンプル web アプリケーションでは、Entity Framework 5 Code First と Visual Studio を使用して ASP.NET MVC 4 アプリケーションを作成する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 64906a1d-f734-41cf-9615-ee95f8740996
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: 277503b65d9b75a9d3cc05538d5327f9367f45e0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="advanced-entity-framework-scenarios-for-an-mvc-web-application-10-of-10"></a>MVC Web アプリケーション (10 10) の高度なエンティティ フレームワークのシナリオ
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[完成したプロジェクトのダウンロード](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso 大学でサンプル web アプリケーションでは、Entity Framework 5 Code First と Visual Studio 2012 を使用して ASP.NET MVC 4 アプリケーションを作成する方法を示します。 チュートリアル シリーズについては、[シリーズの最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)をご覧ください。 一連のチュートリアルを開始するには、最初からまたは[この章のスタート プロジェクトをダウンロード](building-the-ef5-mvc4-chapter-downloads.md)し、ここから開始します。
> 
> > [!NOTE] 
> > 
> > 解決できない場合、問題が発生した場合[ダウンロード完了章](building-the-ef5-mvc4-chapter-downloads.md)の問題を再現しようとします。 一般に、コードを完成したコードを比較することによって、問題の解決策を検索できます。 一般的なエラーとそれらを解決する方法は、次を参照してください。[エラーと回避策です。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


前のチュートリアルでは、リポジトリと作業パターンの単位を実装します。 このチュートリアルでは、次のトピックについて説明します。

- 生の SQL クエリを実行します。
- No 追跡クエリを実行しています。
- クエリの調査は、データベースに送信します。
- プロキシ クラスを使用します。
- 変更の自動検出を無効にします。
- 変更を保存するときに検証を無効にします。
- [エラーと回避策](#errors)

これらのトピックのほとんどは、既に作成しているページを操作します。 一括更新を実行する生の SQL を使用するには、データベース内のすべてのコースのクレジットの数を更新する新しいページを作成します。

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

なしの追跡クエリを使用して、部門の編集 ページに新しい検証ロジックを追加します。

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

## <a name="performing-raw-sql-queries"></a>生の SQL クエリを実行します。

Entity Framework コードの最初の API には、データベースに直接 SQL コマンドを渡すことができるようにするメソッドが含まれています。 次のオプションがあります。

- エンティティ型を返すクエリに対して `DbSet.SqlQuery` メソッドを使用します。 返されたオブジェクトで想定されている型でなければなりません、`DbSet`オブジェクト、およびそれらが自動的に追跡データベースのコンテキストによって追跡を無効にする場合を除き、します。 (は、次のセクションを参照してください、`AsNoTracking`メソッドです)。
- 使用して、`Database.SqlQuery`をエンティティがない型を返すクエリ メソッド。 このメソッドを使用してエンティティ型を取得する場合でも、返されるデータはデータベース コンテキストによって追跡されません。
- 使用して、 [Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456(v=vs.103).aspx)非クエリ コマンド。

Entity Framework を使用する利点の 1 つは、データを格納する特定のメソッドにコードを過度に接近させなくてもよい点です。 SQL クエリとコマンドが生成されるため、自分でこれらを記述する必要がなくなります。 例外的なシナリオは、手動で作成した特定の SQL クエリを実行する必要がある場合とこれらのメソッドのできるようにすると、このような例外を処理します。

Web アプリケーションで SQL コマンドを実行する場合は常に、SQL インジェクション攻撃から自身のサイトを保護する対策を講じる必要があります。 これを行う 1 つの方法として、パラメーター化されたクエリを使用して、Web ページによって送信された文字列が SQL コマンドとして解釈できないことを確認します。 このチュートリアルでは、ユーザー入力をクエリに統合するときに、パラメーター化されたクエリを使用します。

### <a name="calling-a-query-that-returns-entities"></a>クエリを呼び出すには、エンティティが返されます。

場合を考えます、`GenericRepository`追加フィルターおよび並べ替え、その他の方法では、派生クラスを作成することを必要とせずの柔軟性を提供するクラス。 実現するための 1 つの方法は、SQL クエリを受け取るメソッドを追加することです。 でしたしするを指定する任意の種類のフィルター処理や並べ替えするコント ローラーなど、`Where`結合またはサブクエリに依存する句。 このセクションでは、このようなメソッドを実装する方法が表示されます。

作成、`GetWithRawSql`メソッドに次のコードを追加することによって*GenericRepository.cs*:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs)]

*CourseController.cs*から新しいメソッドを呼び出す、`Details`メソッドを次の例で示すようにします。

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

ここで使用しても、`GetByID`メソッドを使用している、`GetWithRawSql`ことを確認するメソッド、`GetWithRawSQL`メソッドの動作です。

選択が動作をクエリすることを確認する詳細ページを実行 (選択、**コース** タブし**詳細**コースの 1 つの)。

![Course_Details_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>クエリを呼び出すには、その他の種類のオブジェクトが返されます。

以前に、登録日ごとの学生数を示す About ページ用に、学生の統計グリッドを作成しました。 これを行うコード*HomeController.cs* LINQ を使用します。

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs)]

LINQ を使用するのではなく、SQL の直接このデータを取得するコードを記述するとします。 エンティティ オブジェクト以外のものを返すクエリを実行する必要があることを意味する必要がありますを使用して、`Database.SqlQuery`メソッドです。

*HomeController.cs*での LINQ ステートメントは、置換、`About`メソッドを次のコード。

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

バージョン情報 ページを実行します。 以前に行ったのと同じデータが表示されます。

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

### <a name="calling-an-update-query"></a>更新クエリを呼び出す

Contoso 大学管理者がすべてコースのクレジットの数の変更などのデータベースで一括変更を実行することができるようにするとします。 大学に多くのコースがある場合は、それらすべてをエンティティとして取得し、それらを個別に変更するのは非効率的です。 Web ページをすべてのコースのクレジットの数を変更する要素を指定することができますを実装するこのセクションの内容と SQL を実行することによって、変更を行うを`UPDATE`ステートメントです。 Web ページは次の図のようになります。

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

前のチュートリアルでの読み取りし、更新を汎用的なリポジトリを使用して`Course`内のエンティティ、`Course`コント ローラー。 この一括更新操作では、汎用的なリポジトリにない新しいリポジトリ メソッドを作成する必要があります。 そのためには作成専用`CourseRepository`から派生したクラス、`GenericRepository`クラスです。

*DAL*フォルダー作成*CourseRepository.cs*し、既存のコードを次のコードに置き換えます。

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cs)]

*UnitOfWork.cs*、変更、`Course`リポジトリの種類から`GenericRepository<Course>`に `CourseRepository:`

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.cs)]

*CourseContoller.cs*、追加、`UpdateCourseCredits`メソッド。

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

このメソッドは、両方の使用は`HttpGet`と`HttpPost`です。 ときに、 `HttpGet` `UpdateCourseCredits`メソッドを実行する、`multiplier`変数が null でないと、表示は、その前の図に示すように空のテキスト ボックスと [送信] ボタン、表示されます。

ときに、**更新**ボタンがクリックされると`HttpPost`メソッドを実行する`multiplier`テキスト ボックスに入力した値になります。 コード リポジトリを呼び出して、`UpdateCourseCredits`の数を返すメソッドの影響を受ける行、およびでその値が格納されている、`ViewBag`オブジェクト。 ビューが影響を受ける行の数を受信すると、`ViewBag`オブジェクトのテキスト ボックスの代わりにその番号が表示され、次の図に示すように、ボタンを送信します。

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

ビューを作成、 *Views\Course*更新コース クレジット ページ用のフォルダー。

![Add_View_dialog_box_for_Update_Course_Credits](https://asp.net/media/2578203/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Add_View_dialog_box_for_Update_Course_Credits.png)

*Views\Course\UpdateCourseCredits.cshtml*、テンプレート コードを次のコードに置き換えます。

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

**[Courses]\(コース\)** タブを選択してから、ブラウザーのアドレス バーで URL の末尾に "/UpdateCourseCredits" を追加して (例: `http://localhost:50205/Course/UpdateCourseCredits`)、`UpdateCourseCredits` メソッドを実行します。 テキスト ボックスに数値を入力します。

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

**[更新]**をクリックします。 影響を受けた行の数が表示されます。

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

**[リストに戻る]** をクリックして、単位数が変更されたコースの一覧を表示します。

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

生の SQL クエリの詳細については、次を参照してください。[生の SQL クエリ](https://blogs.msdn.com/b/adonet/archive/2011/02/04/using-dbcontext-in-ef-feature-ctp5-part-10-raw-sql-queries.aspx)Entity Framework チームのブログです。

## <a name="no-tracking-queries"></a>No 追跡クエリ

データベース コンテキストは、データベースの行を取得し、それらを表すエンティティ オブジェクトを作成、ときに既定では、追跡データベース内にメモリ内のエンティティが同期されているかどうか。 メモリ内のデータはキャッシュとして機能し、エンティティを更新するときに使われます。 Web アプリケーションでは、一般にコンテキスト インスタンスの存続期間は短く (要求ごとに新しいインスタンスが作成されて破棄されます)、通常、エンティティを読み取るコンテキストはエンティティが再び使われる前に破棄されるので、多くの場合、このようなキャッシュは必要ありません。

コンテキストが使用して、クエリのエンティティ オブジェクトを追跡するかどうかを指定することができます、`AsNoTracking`メソッドです。 追跡を無効にした方がよい一般的なシナリオを以下に示します。

- クエリでは、大量の追跡を無効にするとパフォーマンスを向上させる著しくするデータを取得します。
- 前のさまざまな目的は同じエンティティを取得したが、更新するためにエンティティをアタッチする場合します。 エンティティはデータベース コンテキストによって既に追跡されているため、変更するエンティティをアタッチできません。 これを防ぐ方法の 1 つを使用して、`AsNoTracking`オプションは、以前のクエリを使用します。

このセクションの内容を 2 番目のこれらのシナリオを示しています。 ビジネス ロジックを実装します。 具体的には、インストラクターは 1 つ以上の部門の管理者であることを示すビジネス ルールを適用します。

*DepartmentController.cs*から呼び出すことのできる新しいメソッドを追加、`Edit`と`Create`ない 2 つの部門に同じ管理者が持っていることを確認する方法。

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.cs)]

コードを追加、`try`のブロック、 `HttpPost` `Edit`検証エラーがない場合は、この新しいメソッドを呼び出すメソッド。 `try`ブロックが次の例のようになります。

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample11.cs?highlight=9-12)]

部門の編集 ページを実行し、インストラクターは既に別の部門の管理者ユーザーに部門の管理者を変更しようとします。 予期されるエラー メッセージを表示されます。

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

これにより実行部門の編集ページをもう一度この時刻の変更、**予算**量。 クリックすると、**保存**、エラー ページを参照してください。

![Department_Edit_page_with_object_state_manager_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

例外のエラー メッセージは"`An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key.`"これには、次の一連のイベントの原因が発生しました。

- `Edit`メソッドの呼び出し、`ValidateOneAdministratorAssignmentPerInstructor`メソッドで、管理者は、Kim Abercrombie のすべての部門を取得します。 読み取られる英語部門を原因となったとします。 編集中、部門があるために、エラーは報告されません。 この読み取り操作の結果としてただし、データベースから読み取られた英語部門 エンティティは、今すぐによって追跡されているデータベース コンテキスト。
- `Edit`メソッド設定しようとする、`Modified`英語のフラグ、MVC モデル バインダーで、department エンティティが作成されますが、失敗すると、コンテキストが既に英語部門のエンティティを追跡するためです。

この問題を解決では、コンテキストが検証クエリによって取得されるメモリ内の部門エンティティを追跡するを防止します。 欠点は、このエンティティが更新されないため、これを行うまたはメモリにキャッシュされていることからメリットを得られる方法でもう一度読み取るにはありません。

*DepartmentController.cs*で、`ValidateOneAdministratorAssignmentPerInstructor`メソッドを次に示すようには、追跡を指定しません。

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample12.cs?highlight=4)]

編集しようとするを繰り返して、**予算**部門の量。 今度は、操作が成功すると、し、修正された予算の値を表示、部門インデックス ページに期待どおりに、サイトを返します。

## <a name="examining-queries-sent-to-the-database"></a>データベースに送信されるクエリを確認します。

データベースに送信される実際の SQL クエリを確認できると役立つ場合があります。 これを行うには、デバッガーでクエリ変数を確認またはクエリの呼び出す`ToString`メソッドです。 これを試すをするには、単純なクエリを見てし、このような一括読み込み、フィルター、および並べ替えオプションを追加するように動作を確認します。

*コント ローラー/CourseController*、置換、`Index`メソッドを次のコード。

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample13.cs?highlight=3)]

今すぐにブレークポイントを設定*GenericRepository.cs*上、`return query.ToList();`と`return orderBy(query).ToList();`のステートメント、`Get`メソッドです。 プロジェクトをデバッグ モードで実行し、コース インデックス ページを選択します。 コードでは、ブレークポイントに達すると、確認、`query`変数。 SQL Server に送信されるクエリが表示されます。 単純な`Select`ステートメント。

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample14.json)]

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

クエリは、Visual Studio でのデバッグ ウィンドウに表示するには長すぎます。 クエリ全体を表示するには、変数の値をコピーしてテキスト エディターに貼り付けます。

![Copy_value_of_variable_in_debug_mode](https://asp.net/media/2578239/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Copy_value_of_variable_in_debug_mode_0902a2b1-b799-47a6-9b4b-f266c79d83c1.png)

今すぐできるように、ユーザーが特定の部署のフィルター処理できます、コース インデックス ページにドロップダウン リストを追加します。 タイトル、コースが並べ替えられるしの一括読み込みを指定します、`Department`ナビゲーション プロパティ。 *CourseController.cs*、置換、`Index`メソッドを次のコード。

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample15.cs)]

このメソッドは、ドロップダウン リストの選択した値、`SelectedDepartment`パラメーター。 何も選択すると、このパラメーターは null になります。

A`SelectList`ドロップ ダウン リスト ビューにすべての部門を含むコレクションが渡されました。 渡されるパラメーター、`SelectList`コンス トラクターは、値フィールドの名前、テキスト フィールド名、および選択した項目を指定します。

`Get`のメソッド、`Course`リポジトリ、コードを指定するフィルター式、並べ替え順、および一括読み込みの`Department`ナビゲーション プロパティ。 常に、フィルター式を返す`true`かどうか何も選択ドロップダウン リストで (つまり、`SelectedDepartment`が null です)。

*Views\Course\Index.cshtml*、開始する直前に`table`タグで、ドロップダウン リストと送信 ボタンを作成する次のコードを追加します。

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample16.cshtml)]

設定されているブレークポイントが、`GenericRepository`クラス、コース インデックス ページを実行します。 コードが、ブレークポイントをヒットの最初の 2 回に従い、ページがブラウザーに表示されないようにします。 ドロップダウン リストから、部署を選択し、クリックして**フィルター**:

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

今度は、最初のブレークポイントは、ドロップダウン リストの部門クエリのされます。 スキップして、表示、`query`変数、次回、コードのブレークポイントに達した新機能を確認するために、`Course`ようこれでクエリを実行します。 次のようなものが表示されます。

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample17.json)]

クエリを参照してください、`JOIN`読み込むクエリの`Department`データと共に、`Course`データ、およびが含まれている、`WHERE`句。

<a id="proxyclasses"></a>

## <a name="working-with-proxy-classes"></a>プロキシ クラスの扱い

Entity Framework では、エンティティのインスタンス (たとえば、クエリを実行する場合) を作成するときは、エンティティのプロキシとして機能する、動的に生成される派生型のインスタンスとしてそれを多くの場合を作成します。 このプロキシは、プロパティにアクセスする場合、自動的にアクションを実行するためのフックを挿入するエンティティの一部の仮想プロパティをオーバーライドします。 たとえば、このメカニズムはリレーションシップの遅延読み込みをサポートするために使用されます。

ほとんどの場合このプロキシの使用について注意する必要ありませんは例外があります。

- 一部のシナリオでは、Entity Framework がプロキシのインスタンスを作成しないようにすることができます。 たとえば、プロキシではないインスタンスをシリアル化する場合がありますプロキシ インスタンスをシリアル化するよりも効率的です。
- ときにインスタンス化する、エンティティ クラスを使用して、`new`演算子、プロキシ インスタンスを取得しません。 これは、遅延読み込みと自動変更の追跡などの機能を取得しないことを意味します。 これは、通常さてです。通常必要はありません、遅延読み込み時、データベースにない新しいエンティティを作成しているため、通常必要し、しない変更の追跡とエンティティを明示的にマークしている場合`Added`です。 ただし、遅延読み込みする必要は、変更の追跡が必要な場合は、インスタンスを作成できます新しいエンティティを使用してプロキシ、`Create`のメソッド、`DbSet`クラスです。
- プロキシの種類から実際のエンティティ型を取得する可能性があります。 使用することができます、`GetObjectType`のメソッド、`ObjectContext`プロキシ型のインスタンスの実際のエンティティ型を取得するクラス。

詳細については、次を参照してください。[プロキシ扱う](https://blogs.msdn.com/b/adonet/archive/2011/02/02/using-dbcontext-in-ef-feature-ctp5-part-8-working-with-proxies.aspx)Entity Framework チームのブログです。

## <a name="disabling-automatic-detection-of-changes"></a>変更の自動検出を無効にします。

Entity Framework では、エンティティの現在の値と元の値を比較して、エンティティがどのように変更されたか (およびそれによって、どの更新プログラムをデータベースに送信する必要があるか) を判断します。 元の値は、エンティティがクエリを実行したり、接続されているときに格納されます。 変更の自動検出を行うメソッドには、次のようなものがあります。

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

多数のエンティティを管理するいると、ループ内で何度もがこれらのメソッドのいずれかの呼び出すと、自動変更の検出を使用して機能をオフに一時的に大幅なパフォーマンス向上をする可能性があります、 [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled(VS.103).aspx)プロパティです。 詳細については、次を参照してください。[変更を自動的に検出する](https://blogs.msdn.com/b/adonet/archive/2011/02/06/using-dbcontext-in-ef-feature-ctp5-part-12-automatically-detecting-changes.aspx)です。

## <a name="disabling-validation-when-saving-changes"></a>変更を保存するときに検証を無効にします。

呼び出すと、`SaveChanges`メソッド、既定では、Entity Framework データを検証、すべての変更されたエンティティのすべてのプロパティで、データベースを更新する前にします。 多数のエンティティを更新したし、して既に検証した、データをこの作業は必要と保存の処理を行うことが、変更は、一時的に検証を無効にすることによって時間を短縮します。 使用して行うことができます、 [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled(VS.103).aspx)プロパティです。 詳細については、次を参照してください。[検証](https://blogs.msdn.com/b/adonet/archive/2010/12/15/ef-feature-ctp5-validation.aspx)です。

## <a name="summary"></a>まとめ

これは、この一連の ASP.NET MVC アプリケーションで Entity Framework を使用してチュートリアルを完了します。 その他の Entity Framework リソースへのリンクは含まれて、 [ASP.NET データ アクセス コンテンツ マップ](../../../../whitepapers/aspnet-data-access-content-map.md)です。

構築した後、web アプリケーションを展開する方法の詳細については、次を参照してください。 [ASP.NET 展開のコンテンツ マップ](https://msdn.microsoft.com/library/bb386521.aspx)、MSDN ライブラリです。

については、認証および承認など、MVC に関連するその他のトピックを参照してください、[推奨の MVC リソース](../../getting-started/recommended-resources-for-mvc.md)です。

<a id="acknowledgments"></a>

## <a name="acknowledgments"></a>謝辞

- Tom Dykstra はこのチュートリアルの元のバージョンを作成、Microsoft Web プラットフォームとツール コンテンツ チームのシニア プログラミング ライターです。
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) このチュートリアルを共同で作成し、5 の EF と MVC 4 の更新作業の大半をします。 Rick は、Microsoft Azure と MVC に焦点を当てたのスキルの限られたプログラミング ライターです。
- [Rowan Miller](http://www.romiller.com)および Entity Framework チームの他のメンバー コード レビューを支援して EF 5 のチュートリアルが更新されたことをお中に発生し、移行に関する多くの問題のデバッグを支援します。

## <a name="vb"></a>VB

このチュートリアルがもともと生成されるとき、おダウンロードが完了したプロジェクトの c# および VB の両方のバージョンが用意されています。 この更新プログラムでは、c# ダウンロード可能なプロジェクトの作業を開始する任意の場所が時間の制限事項と作業を行っていないを vb です。 の他の優先順位によりシリーズでは、容易にできるように各章を提供します。 これらのチュートリアルを使用して、VB プロジェクトをビルドして可能な項目を共有する他のユーザー、お知らせします。

<a id="errors"></a>

## <a name="errors-and-workarounds"></a>エラーと回避策

### <a name="cannot-createshadow-copy"></a>コピーを作成またはシャドウことはできません。

エラー メッセージ:

*できません作成、またはシャドウ コピー 'DotNetOpenAuth.OpenId' そのファイルが既に存在する場合。*

解決方法 : 

数秒待ってから、ページを更新します。

### <a name="update-database-not-recognized"></a>データベースの更新認識されません。

エラー メッセージ:

*用語 'データベースの更新' は、コマンドレット、関数、スクリプト ファイル、または操作可能なプログラムの名前としては認識されません。名のスペルを確認するかパスが含まれている場合、パスが正しいことを確認し、もう一度やり直してください。*(から、 *`Update-Database`* PMC コマンドです)。

解決方法 : 

Visual Studio を終了します。 プロジェクトを開き直すし、もう一度やり直してください。

### <a name="validation-failed"></a>検証に失敗しました

エラー メッセージ:

*1 つまたは複数のエンティティの検証に失敗しました。詳細については 'EntityValidationErrors' プロパティを参照してください。* (から、 *`Update-Database`* PMC コマンドです)。

解決方法 : 

この問題の原因の 1 つは、検証エラー時に、`Seed`メソッドが実行されます。 参照してください[シード処理、およびデバッグ Entity Framework (EF) Db](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx)のデバッグに関するヒントについては、`Seed`メソッドです。

### <a name="http-50019-error"></a>HTTP 500.19 エラー

エラー メッセージ:

*HTTP エラー 500.19 - 内部サーバー エラー、要求されたページはページの関連する構成データが正しくないために、アクセスできません。*

解決方法 : 

このエラーを取得する方法の 1 つは同じポート番号を使用してそれらの各ソリューションの複数のコピーです。 通常、この問題を解決するには、Visual Studio のすべてのインスタンスを終了し、プロジェクトで作業中の再起動します。 それでもうまくいかない場合は、ポート番号を変更してください。 プロジェクト ファイルを右クリックし、[プロパティ] をクリックします。 選択、 **Web**  タブをされているポート番号を変更、**プロジェクト Url**テキスト ボックス。

### <a name="error-locating-sql-server-instance"></a>SQL Server インスタンスの位置を特定しているときのエラー

エラー メッセージ:

*SQL Server への接続を確立中にネットワーク関連またはインスタンス固有のエラーが発生しました。サーバーが見つからないかアクセスできません。インスタンス名が正しいことと、リモート接続を許可する SQL Server が構成されていることを確認します。(プロバイダー: SQL Network Interfaces、エラー: 26 - エラーの検索のサーバー/インスタンスの指定)*

解決方法 : 

接続文字列を確認します。 データベースを手動で削除する場合は、構築文字列でデータベースの名前を変更します。

> [!div class="step-by-step"]
> [前へ](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
> [次へ](building-the-ef5-mvc4-chapter-downloads.md)
