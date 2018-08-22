---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: MVC Web アプリケーション (10/10) 用の Entity Framework シナリオの詳細 |Microsoft Docs
author: tdykstra
description: Contoso University のサンプルの web アプリケーションでは、Entity Framework 5 Code First と Visual Studio を使用して ASP.NET MVC 4 アプリケーションを作成する方法について説明しています.
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 64906a1d-f734-41cf-9615-ee95f8740996
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: 5fa6ff439790f70cd60d0985a791df26d69c7107
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826596"
---
<a name="advanced-entity-framework-scenarios-for-an-mvc-web-application-10-of-10"></a>MVC Web アプリケーション (10/10) 用の高度な Entity Framework シナリオ
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[完成したプロジェクトのダウンロード](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso University のサンプルの web アプリケーションでは、Entity Framework 5 Code First と Visual Studio 2012 を使用して ASP.NET MVC 4 アプリケーションを作成する方法を示します。 チュートリアル シリーズについては、[シリーズの最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)をご覧ください。 チュートリアルのシリーズを開始するには、最初からまたは[この章のスタート プロジェクトをダウンロード](building-the-ef5-mvc4-chapter-downloads.md)し、ここから始めてください。
> 
> > [!NOTE] 
> > 
> > を解決できない問題が生じた場合[章では、完了したダウンロード](building-the-ef5-mvc4-chapter-downloads.md)の問題を再現しようとします。 問題の解決策は、完成したコードにコードを比較することによって一般的に見つかります。 一般的なエラーとその解決方法は、次を参照してください。[エラーと回避策。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


前のチュートリアルでは、リポジトリと unit of work パターンを実装します。 このチュートリアルでは、次のトピックについて説明します。

- 生 SQL クエリを実行しています。
- 追跡なしのクエリを実行しています。
- クエリの調査は、データベースに送信します。
- プロキシ クラスを使用します。
- 変更の自動検出を無効にします。
- 変更を保存するときに検証を無効にします。
- [エラーと回避策](#errors)

これらのトピックのほとんどは、既に作成したページを操作します。 生 SQL を使用して一括更新プログラムを実行するには、データベース内のすべてのコースの単位数を更新する新しいページを作成します。

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

追跡なしのクエリを使用して、Department Edit ページに新しい検証ロジックを追加します。

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

## <a name="performing-raw-sql-queries"></a>生 SQL クエリを実行します。

Entity Framework Code First API をデータベースに直接 SQL コマンドを渡すことができるようにするメソッドが含まれています。 次のオプションがあります。

- エンティティ型を返すクエリに対して `DbSet.SqlQuery` メソッドを使用します。 によって予期される型で返されたオブジェクトが必要があります、`DbSet`オブジェクト、およびそれらが自動的に追跡データベース コンテキストによって追跡をオフにする場合を除き、します。 (に関する次のセクションを参照してください、`AsNoTracking`メソッドです)。
- 使用して、`Database.SqlQuery`エンティティではない型を返すクエリ メソッド。 このメソッドを使用してエンティティ型を取得する場合でも、返されるデータはデータベース コンテキストによって追跡されません。
- 使用して、 [Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456(v=vs.103).aspx)非クエリ コマンド。

Entity Framework を使用する利点の 1 つは、データを格納する特定のメソッドにコードを過度に接近させなくてもよい点です。 SQL クエリとコマンドが生成されるため、自分でこれらを記述する必要がなくなります。 手動で作成した特定の SQL クエリを実行する必要がある場合に例外的なシナリオがあるし、これらのメソッドにより、このような例外を処理することが可能です。

Web アプリケーションで SQL コマンドを実行する場合は常に、SQL インジェクション攻撃から自身のサイトを保護する対策を講じる必要があります。 これを行う 1 つの方法として、パラメーター化されたクエリを使用して、Web ページによって送信された文字列が SQL コマンドとして解釈できないことを確認します。 このチュートリアルでは、ユーザー入力をクエリに統合するときに、パラメーター化されたクエリを使用します。

### <a name="calling-a-query-that-returns-entities"></a>クエリを呼び出すには、エンティティが返されます。

`GenericRepository`追加のフィルター処理および他のメソッドは、派生クラスを作成することを必要とせずに柔軟性を並べ替えクラス。 これを実現する方法の 1 つは、SQL クエリを受け取るメソッドを追加することです。 あらゆる種類のフィルター処理または並べ替えなどのコント ローラーなを指定できます、`Where`結合やサブクエリに依存する句。 このセクションでは、このようなメソッドを実装する方法を確認します。

作成、`GetWithRawSql`メソッドに次のコードを追加することで*GenericRepository.cs*:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs)]

*CourseController.cs*から新しいメソッドを呼び出し、`Details`メソッドを次の例に示すようにします。

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

ここで使用しても、`GetByID`メソッドを使用している、`GetWithRawSql`ことを確認するメソッド、`GetWithRawSQL`方法でも使用できます。

Select クエリのしくみを確認する詳細ページを実行 (選択、**コース** タブをクリックし**詳細**の 1 つのコース)。

![Course_Details_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>クエリを呼び出すには、その他の種類のオブジェクトが返されます。

以前に、登録日ごとの学生数を示す About ページ用に、学生の統計グリッドを作成しました。 コードでは、これを*HomeController.cs* LINQ を使用します。

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs)]

LINQ を使用するのではなく、SQL で直接このデータを取得するコードを記述するとします。 エンティティ オブジェクト以外のものを返すクエリを実行する必要があることには、ことを意味する必要があるを使用して、`Database.SqlQuery`メソッド。

*HomeController.cs*での LINQ ステートメントを置き換える、`About`メソッドを次のコード。

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

[About] ページを実行します。 以前に行ったのと同じデータが表示されます。

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

### <a name="calling-an-update-query"></a>更新クエリを呼び出す

Contoso University の管理者は、すべてのコースの単位数を変更するなど、データベースで一括変更を実行することができるようにするとします。 大学に多くのコースがある場合は、それらすべてをエンティティとして取得し、それらを個別に変更するのは非効率的です。 このセクションでは、すべてのコースの単位数を変更する係数を指定するユーザーを許可する web ページを実装し、SQL を実行することによって、変更を行います`UPDATE`ステートメント。 Web ページは次の図のようになります。

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

前のチュートリアルでの読み取りし、更新を汎用的なリポジトリを使用して`Course`内のエンティティ、`Course`コント ローラー。 この一括更新操作では、汎用的なリポジトリに含まれていない新しいリポジトリ メソッドを作成する必要があります。 そのために作成します専用`CourseRepository`から派生したクラス、`GenericRepository`クラス。

*DAL*フォルダー作成*CourseRepository.cs*既存のコードを次のコードに置き換えます。

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cs)]

*UnitOfWork.cs*、変更、`Course`リポジトリの種類から`GenericRepository<Course>`に `CourseRepository:`

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.cs)]

*CourseContoller.cs*、追加、`UpdateCourseCredits`メソッド。

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

このメソッドは、両方の使用は`HttpGet`と`HttpPost`します。 ときに、 `HttpGet` `UpdateCourseCredits`メソッドを実行、`multiplier`変数が null にして、ビューは空のテキスト ボックスと送信ボタンの場合は、前の図に示すように、表示されます。

ときに、**更新**ボタンがクリックされると、`HttpPost`メソッドを実行`multiplier`テキスト ボックスに入力した値になります。 コードを呼び出してリポジトリ`UpdateCourseCredits`の数を返すメソッドには、行が影響を受けるしでその値が格納されている、`ViewBag`オブジェクト。 ビューが影響を受ける行の数を受信すると、`ViewBag`オブジェクト、テキスト ボックスではなく、その数を表示し、次の図に示すように、ボタンを送信します。

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

ビューを作成、 *Views\Course*更新 Course Credits ページのフォルダー。

![Add_View_dialog_box_for_Update_Course_Credits](https://asp.net/media/2578203/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Add_View_dialog_box_for_Update_Course_Credits.png)

*Views\Course\UpdateCourseCredits.cshtml*、テンプレート コードを次のコードに置き換えます。

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

**[Courses]\(コース\)** タブを選択してから、ブラウザーのアドレス バーで URL の末尾に "/UpdateCourseCredits" を追加して (例: `http://localhost:50205/Course/UpdateCourseCredits`)、`UpdateCourseCredits` メソッドを実行します。 テキスト ボックスに数値を入力します。

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

**[更新]** をクリックします。 影響を受けた行の数が表示されます。

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

**[リストに戻る]** をクリックして、単位数が変更されたコースの一覧を表示します。

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

生 SQL クエリの詳細については、次を参照してください。[生 SQL クエリ](https://blogs.msdn.com/b/adonet/archive/2011/02/04/using-dbcontext-in-ef-feature-ctp5-part-10-raw-sql-queries.aspx)Entity Framework チームのブログ。

## <a name="no-tracking-queries"></a>追跡なしのクエリ

データベース コンテキストでは、データベースの行を取得し、それらを表すエンティティ オブジェクトを作成、ときに既定では、追跡データベースをメモリ内のエンティティが同期されているかどうか。 メモリ内のデータはキャッシュとして機能し、エンティティを更新するときに使われます。 Web アプリケーションでは、一般にコンテキスト インスタンスの存続期間は短く (要求ごとに新しいインスタンスが作成されて破棄されます)、通常、エンティティを読み取るコンテキストはエンティティが再び使われる前に破棄されるので、多くの場合、このようなキャッシュは必要ありません。

コンテキストが使用してクエリのエンティティ オブジェクトを追跡するかどうかを指定することができます、`AsNoTracking`メソッド。 追跡を無効にした方がよい一般的なシナリオを以下に示します。

- クエリでは、このような膨大な量の追跡を無効にするとパフォーマンスを向上させる著しくするデータを取得します。
- それを更新するには、エンティティをアタッチしたいが、異なる目的で以前に同じエンティティを取得します。 エンティティはデータベース コンテキストによって既に追跡されているため、変更するエンティティをアタッチできません。 これを防ぐ方法の 1 つが使用するには、`AsNoTracking`オプションは、以前のクエリを使用します。

このセクションでは、2 番目のこれらのシナリオを示しています。 ビジネス ロジックを実装します。 具体的には、講師は 1 つ以上の部門の管理者であることを示すビジネス ルールを適用します。

*DepartmentController.cs*から呼び出すことのできる新しいメソッドを追加、`Edit`と`Create`メソッドを 2 つの部門がありませんが、同じ管理者であること。

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.cs)]

内のコードを追加、`try`のブロック、 `HttpPost` `Edit`検証エラーがない場合、この新しいメソッドを呼び出すメソッド。 `try`ブロックでは、次の例のようになります。

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample11.cs?highlight=9-12)]

Department Edit ページを実行し、インストラクターは既に別の部門の管理者ユーザーに部門の管理者を変更しようとします。 予期されるエラー メッセージを表示されます。

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

これにより実行 Department Edit ページをもう一度この時刻の変更、**予算**量。 クリックすると**保存**、エラー ページを参照してください。

![Department_Edit_page_with_object_state_manager_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

例外のエラー メッセージは"`An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key.`"これにより次の一連のイベントが発生しました。

- `Edit`メソッドの呼び出し、`ValidateOneAdministratorAssignmentPerInstructor`メソッドで、管理者として Kim Abercrombie のすべての部門を取得します。 読み取る English 部署を原因となったとします。 編集中、部門があるため、エラーは報告されません。 この読み取り操作では、結果としてただし、データベースから読み取られた English 部署のエンティティは今すぐコンテキストによって追跡されて、データベース。
- `Edit`メソッドを設定しようとする、`Modified`英語版のフラグ、MVC モデル バインダーによって作成された department エンティティがコンテキストが既に、English 部署のエンティティを追跡するために失敗します。

この問題を 1 つのソリューションでは、検証クエリによって取得されたメモリ内の部署エンティティの追跡からコンテキストを保持します。 このエンティティを更新しないため、これを行うか、メモリにキャッシュされていることからメリットを得られる方法でもう一度読み取る欠点はありません。

*DepartmentController.cs*の`ValidateOneAdministratorAssignmentPerInstructor`メソッドを次に示すようには、追跡、指定しません。

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample12.cs?highlight=4)]

編集しようとするを繰り返して、**予算**部門の量。 この時間、操作が成功すると修正された予算の値を表示、Departments Index ページに期待どおりに、サイトが返されます。

## <a name="examining-queries-sent-to-the-database"></a>データベースに送信されるクエリの調査

データベースに送信される実際の SQL クエリを確認できると役立つ場合があります。 これを行うには、デバッガーでのクエリ変数を確認またはクエリの呼び出す`ToString`メソッド。 これを試すには、単純なクエリを見てをこのような一括読み込み、フィルター処理、および並べ替えオプションを追加するときにどう見ています。

*コント ローラー/CourseController*、置換、`Index`メソッドを次のコード。

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample13.cs?highlight=3)]

ブレークポイントを設定するようになりました*GenericRepository.cs*上、`return query.ToList();`と`return orderBy(query).ToList();`のステートメント、`Get`メソッド。 プロジェクトをデバッグ モードで実行し、コースのインデックス ページを選択します。 コードでは、ブレークポイントに達すると、確認、`query`変数。 SQL Server に送信されるクエリが表示されます。 単純な`Select`ステートメント。

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample14.json)]

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

クエリは、Visual Studio でのデバッグ ウィンドウに表示するには長すぎます。 クエリ全体を表示するは、変数の値をコピーし、テキスト エディターに貼り付けます。

![Copy_value_of_variable_in_debug_mode](https://asp.net/media/2578239/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Copy_value_of_variable_in_debug_mode_0902a2b1-b799-47a6-9b4b-f266c79d83c1.png)

今すぐできるように、ユーザーは、特定の部署のフィルター処理できます、コースのインデックス ページにドロップダウン リストを追加します。 コース タイトルで並べ替えられるし、一括読み込みを指定します、`Department`ナビゲーション プロパティ。 *CourseController.cs*、置換、`Index`メソッドを次のコード。

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample15.cs)]

メソッドのドロップダウン リストの選択した値を受け取る、`SelectedDepartment`パラメーター。 何も選択されている場合、このパラメーターは null になります。

A`SelectList`ドロップダウン リストのすべての部門を含むコレクションが、ビューに渡されます。 渡されるパラメーター、`SelectList`コンス トラクターは、値フィールドの名前、テキスト フィールド名、および選択した項目を指定します。

`Get`のメソッド、`Course`リポジトリ、コードは、フィルター式、並べ替え順序、およびの一括読み込みを指定、`Department`ナビゲーション プロパティ。 フィルター式は常に返します`true`かどうかは、ドロップダウン リストで何も選択が (つまり、`SelectedDepartment`が null)。

*Views\Course\Index.cshtml*、開始する直前に`table`タグは、ドロップダウン リストと送信ボタンを作成する次のコードを追加します。

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample16.cshtml)]

まだ設定されたブレークポイントと、`GenericRepository`クラス、コースのインデックス ページを実行します。 ページがブラウザーに表示されるよう、コードが、ブレークポイントをヒットする最初の 2 回の手順を続行します。 ドロップダウン リストから、部門を選択し、クリックして**フィルター**:

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

この時間、最初のブレークポイントは、ドロップダウン リストの部門のクエリになります。 スキップして、表示、`query`変数次に、コード、ブレークポイントに到達内容を表示するには、`Course`クエリのようになりますようになりました。 次のようなものが表示されます。

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample17.json)]

クエリを参照してください、`JOIN`読み込みクエリ`Department`データと共に、`Course`データ、および含まれている、`WHERE`句。

<a id="proxyclasses"></a>

## <a name="working-with-proxy-classes"></a>プロキシ クラスの使用

Entity Framework では、(たとえば、クエリを実行するときに) エンティティ インスタンスを作成するときは、動的に生成されたエンティティのプロキシとして機能する派生型のインスタンスとしてそのを多くの場合、作成します。 このプロキシは、プロパティにアクセスするときにアクションを自動的に実行するためのフックを挿入するエンティティの一部の仮想プロパティをオーバーライドします。 たとえば、このメカニズムは、リレーションシップの遅延読み込みをサポートに使用されます。

ほとんどの場合、このプロキシの使用を意識する必要はありませんが、例外があります。

- 一部のシナリオでは、Entity Framework がプロキシのインスタンスを作成するを防ぐためにする場合があります。 たとえば、プロキシではないインスタンスをシリアル化がプロキシ インスタンスをシリアル化するよりも効率的あります。
- インスタンス化すると、エンティティ クラスを使用して、`new`オペレーターは、プロキシ インスタンスを取得しません。 これは、遅延読み込みと自動変更の追跡などの機能を取得しないことを意味します。 これは通常では;一般的に不要な lazy loading、データベースにない新しいエンティティを作成するためと、変更の追跡としてエンティティを明示的にマークしている場合は通常不要`Added`します。 ただし、遅延読み込みする必要は、変更の追跡が必要な場合は、インスタンスを作成できます新しいエンティティを使用してプロキシを持つ、`Create`のメソッド、`DbSet`クラス。
- プロキシ型から実際のエンティティ型を取得する可能性があります。 使用することができます、`GetObjectType`のメソッド、`ObjectContext`プロキシ型のインスタンスの実際のエンティティ型を取得するクラス。

詳細については、次を参照してください。 [Proxies の操作](https://blogs.msdn.com/b/adonet/archive/2011/02/02/using-dbcontext-in-ef-feature-ctp5-part-8-working-with-proxies.aspx)Entity Framework チームのブログ。

## <a name="disabling-automatic-detection-of-changes"></a>変更の自動検出を無効にします。

Entity Framework では、エンティティの現在の値と元の値を比較して、エンティティがどのように変更されたか (およびそれによって、どの更新プログラムをデータベースに送信する必要があるか) を判断します。 エンティティのクエリを実行またはアタッチしたときに、元の値が格納されます。 変更の自動検出を行うメソッドには、次のようなものがあります。

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

多数のエンティティを管理するいると、ループ内で何度もがこれらのメソッドのいずれかの呼び出すと、一時的に変更の自動検出を使用して無効にすることで大幅なパフォーマンス向上を取得可能性があります、 [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled(VS.103).aspx)プロパティ。 詳細については、次を参照してください。[変更を自動的に検出](https://blogs.msdn.com/b/adonet/archive/2011/02/06/using-dbcontext-in-ef-feature-ctp5-part-12-automatically-detecting-changes.aspx)します。

## <a name="disabling-validation-when-saving-changes"></a>変更を保存するときに検証を無効にします。

呼び出すと、`SaveChanges`メソッド、既定では、Entity Framework データを検証、変更されたすべてのエンティティのすべてのプロパティで、データベースを更新する前にします。 多数のエンティティを更新したら、既に検証したデータは、この作業は必要ありませんし保存するプロセスを行うことができます、変更は一時的に検証を無効にすることで時間が。 使用して行うことができます、 [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled(VS.103).aspx)プロパティ。 詳細については、次を参照してください。[検証](https://blogs.msdn.com/b/adonet/archive/2010/12/15/ef-feature-ctp5-validation.aspx)です。

## <a name="summary"></a>まとめ

これは、この一連の ASP.NET MVC アプリケーションで Entity Framework の使用に関するチュートリアルを完了します。 その他の Entity Framework リソースへのリンクが記載されて、 [ASP.NET データ アクセス コンテンツ マップ](../../../../whitepapers/aspnet-data-access-content-map.md)します。

これを構築した後、web アプリケーションをデプロイする方法の詳細については、次を参照してください。 [ASP.NET 配置コンテンツ マップ](https://msdn.microsoft.com/library/bb386521.aspx)、MSDN ライブラリ。

については、認証および承認など、MVC に関連するその他のトピックを参照してください、 [MVC 推奨リソース](../../getting-started/recommended-resources-for-mvc.md)します。

<a id="acknowledgments"></a>

## <a name="acknowledgments"></a>謝辞

- Tom Dykstra はこのチュートリアルの元のバージョンを記述した、Microsoft Web プラットフォームとツール コンテンツ チームのシニア プログラミング ライターです。
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) このチュートリアルを共同執筆し、EF 5 と MVC 4 の更新作業のほとんどをでした。 Rick は、Azure と MVC に注目して Microsoft のシニア プログラミング ライターです。
- [Rowan Miller](http://www.romiller.com)と Entity Framework チームの他のメンバーは、コード レビューを支援し、EF 5 のチュートリアルを更新していた私たちの中に発生した移行に関する多くの問題のデバッグに協力します。

## <a name="vb"></a>VB

チュートリアルが生成された最初のダウンロードが完了したプロジェクトの c# および VB の両方のバージョンが用意されていますが。 この更新により、時間の制限と実行していないを VB 向けの優先順位が、シリーズでは、任意の場所を開始するが容易に章ごとに c# ダウンロード可能なプロジェクトを提供しています これらのチュートリアルを使用した VB プロジェクトをビルドして可能な項目を他のユーザーと共有、ご連絡ください。

<a id="errors"></a>

## <a name="errors-and-workarounds"></a>エラーと回避策

### <a name="cannot-createshadow-copy"></a>コピーを作成/シャドウことはできません。

エラー メッセージ:

*できませんを作成/シャドウ コピー 'DotNetOpenAuth.OpenId' そのファイルが既に存在する場合。*

解決方法 : 

数秒待ってからページを更新します。

### <a name="update-database-not-recognized"></a>データベースの更新認識されません。

エラー メッセージ:

*'データベースの更新' という用語はコマンドレット、関数、スクリプト ファイル、または操作可能なプログラムの名前として認識されません。名前のスペルを確認するか、パスが正しいことを確認し、もう一度お試しパスが含まれている場合。*(から、 *`Update-Database`* PMC でコマンド)。

解決方法 : 

Visual Studio を終了します。 プロジェクトを再度開いてもう一度やり直してください。

### <a name="validation-failed"></a>検証に失敗しました

エラー メッセージ:

*1 つまたは複数のエンティティの検証に失敗しました。詳細については、'EntityValidationErrors' プロパティを参照してください。* (から、 *`Update-Database`* PMC でコマンド)。

解決方法 : 

この問題の原因の 1 つは、検証エラー時に、`Seed`メソッドを実行します。 参照してください[シード処理中、およびデバッグ Entity Framework (EF) Db](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx)デバッグに関するヒント、`Seed`メソッド。

### <a name="http-50019-error"></a>HTTP 500.19 エラー

エラー メッセージ:

*HTTP エラー 500.19 - 内部サーバー エラー、要求されたページは、ページの関連する構成データが無効であるために、アクセスできません。*

解決方法 : 

このエラーを取得する方法の 1 つは、同じポート番号を使用してそれらの各ソリューションの複数のコピーです。 通常には、Visual Studio のすべてのインスタンスを終了し、プロジェクトで作業を再起動するこの問題を解決できます。 うまく行かない場合は、ポート番号を変更してみてください。 プロジェクト ファイルを右クリックし、し、[プロパティ] をクリックします。 選択、 **Web**タブをされているポート番号を変更し、**プロジェクト Url**テキスト ボックス。

### <a name="error-locating-sql-server-instance"></a>SQL Server インスタンスの位置を特定しているときのエラー

エラー メッセージ:

*SQL Server への接続を確立中にネットワーク関連またはインスタンス固有のエラーが発生しました。サーバーが見つからないかアクセスできません。インスタンス名が正しいことと、リモート接続を許可する SQL Server が構成されていることを確認します。(プロバイダー: SQL Network Interfaces、エラー: 26 - エラーの検索のサーバー/インスタンスの指定)*

解決方法 : 

接続文字列を確認します。 データベースを手動で削除した場合は、構築文字列でデータベースの名前を変更します。

> [!div class="step-by-step"]
> [前へ](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
> [次へ](building-the-ef5-mvc4-chapter-downloads.md)
