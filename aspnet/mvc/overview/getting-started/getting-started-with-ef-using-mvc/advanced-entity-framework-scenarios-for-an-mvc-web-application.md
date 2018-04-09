---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: Entity Framework 6 のシナリオを高度な MVC 5 Web アプリケーション (12/12) の |Microsoft ドキュメント
author: tdykstra
description: Contoso 大学でサンプル web アプリケーションでは、Entity Framework 6 の Code First と Visual Studio を使用して ASP.NET MVC 5 アプリケーションを作成する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/08/2014
ms.topic: article
ms.assetid: f35a9b0c-49ef-4cde-b06d-19d1543feb0b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: 50987b30a49173605e7aeb8eb65ff1fe5d5e5753
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="advanced-entity-framework-6-scenarios-for-an-mvc-5-web-application-12-of-12"></a>MVC 5 Web アプリケーション (12/12) の高度な Entity Framework 6 のシナリオ
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[完成したプロジェクトをダウンロード](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)または[PDF のダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso 大学でサンプル web アプリケーションでは、Entity Framework 6 の Code First と Visual Studio 2013 を使用して ASP.NET MVC 5 アプリケーションを作成する方法を示します。 チュートリアル シリーズについては、[シリーズの最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)をご覧ください。


前のチュートリアルでは、テーブルの階層あたりの継承を実装します。 このチュートリアルでは、Entity Framework Code First を使用する ASP.NET web アプリケーションの開発の基礎を超えたときに認識する便利ないくつかのトピックについて紹介します。 詳細な手順を進めていきますコードと、次のトピックの Visual Studio を使用します。

- [生の SQL クエリを実行します。](#rawsql)
- [No の追跡クエリを実行します。](#notracking)
- [データベースに送信する SQL の確認](#sql)

このチュートリアルでは、簡単な概要については次の詳細については、リソースをリンクでいくつかのトピックが導入されています。

- [リポジトリと作業パターンの単位](#repo)
- [プロキシ クラス](#proxies)
- [自動の変更の検出](#changedetection)
- [自動検証](#validation)
- [Visual Studio の EF ツール](#tools)
- [Entity Framework のソース コード](#source)

このチュートリアルでは、次のセクションも含まれます。

- [まとめ](#summary)
- [受信確認](#acknowledgments)
- [VB に関する注意事項](#vb)
- [一般的なエラーと解決方法またはそれらの回避策](#errors)

これらのトピックのほとんどは、既に作成しているページを操作します。 一括更新を実行する生の SQL を使用するには、データベース内のすべてのコースのクレジットの数を更新する新しいページを作成します。

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

<a id="rawsql"></a>
## <a name="performing-raw-sql-queries"></a>生の SQL クエリを実行します。

Entity Framework コードの最初の API には、データベースに直接 SQL コマンドを渡すことができるようにするメソッドが含まれています。 次のオプションがあります。

- 使用して、 [DbSet.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.dbset.sqlquery.aspx)をエンティティ型を返すクエリ メソッド。 返されたオブジェクトで想定されている型でなければなりません、`DbSet`オブジェクト、およびそれらが自動的に追跡データベースのコンテキストによって追跡を無効にする場合を除き、します。 (は、次のセクションを参照してください、 [AsNoTracking](https://msdn.microsoft.com/library/system.data.entity.dbextensions.asnotracking.aspx)メソッドです)。
- 使用して、 [Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery.aspx)をエンティティがない型を返すクエリ メソッド。 このメソッドを使用してエンティティ型を取得する場合でも、返されるデータはデータベース コンテキストによって追跡されません。
- 使用して、 [Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456.aspx)非クエリ コマンド。

Entity Framework を使用する利点の 1 つは、データを格納する特定のメソッドにコードを過度に接近させなくてもよい点です。 SQL クエリとコマンドが生成されるため、自分でこれらを記述する必要がなくなります。 例外的なシナリオは、手動で作成した特定の SQL クエリを実行する必要がある場合とこれらのメソッドのできるようにすると、このような例外を処理します。

Web アプリケーションで SQL コマンドを実行する場合は常に、SQL インジェクション攻撃から自身のサイトを保護する対策を講じる必要があります。 これを行う 1 つの方法として、パラメーター化されたクエリを使用して、Web ページによって送信された文字列が SQL コマンドとして解釈できないことを確認します。 このチュートリアルでは、ユーザー入力をクエリに統合するときに、パラメーター化されたクエリを使用します。

### <a name="calling-a-query-that-returns-entities"></a>クエリを呼び出すには、エンティティが返されます。

[DbSet&lt;TEntity&gt; ](https://msdn.microsoft.com/library/gg696460.aspx)クラス型のエンティティを返すクエリの実行に使用できるメソッドを提供`TEntity`です。 内のコードを変更するしくみを表示する、`Details`のメソッド、`Department`コント ローラー。

*DepartmentController.cs*で、`Details`メソッド、置換、`db.Departments.FindAsync`メソッド呼び出し、`db.Departments.SqlQuery`メソッドの呼び出し、次の強調表示されたコードに示すように。

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs?highlight=8-14)]

新しいコードが正しく動作することを確認するには、**[Departments]\(部門\)** タブを選択し、いずれかの部門の **[Details]\(詳細\)** を選択します。

![部門の詳細](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>クエリを呼び出すには、その他の種類のオブジェクトが返されます。

以前に、登録日ごとの学生数を示す About ページ用に、学生の統計グリッドを作成しました。 これを行うコード*HomeController.cs* LINQ を使用します。

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

LINQ を使用するのではなく、SQL の直接このデータを取得するコードを記述するとします。 エンティティ オブジェクト以外のものを返すクエリを実行する必要があることを意味する必要がありますを使用して、 [Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery(v=VS.103).aspx)メソッドです。

*HomeController.cs*での LINQ ステートメントは、置換、 `About` SQL ステートメントには、次の強調表示されたコードに示すように、メソッド。

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs?highlight=3-18)]

バージョン情報 ページを実行します。 以前に行ったのと同じデータが表示されます。

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-an-update-query"></a>更新クエリを呼び出す

Contoso 大学管理者がすべてコースのクレジットの数の変更などのデータベースで一括変更を実行することができるようにするとします。 大学に多くのコースがある場合は、それらすべてをエンティティとして取得し、それらを個別に変更するのは非効率的です。 ユーザーが、すべてのコースのクレジットの数を変更する係数を指定できるようにする web ページを実装するこのセクションの内容と SQL を実行することによって、変更を加えるを`UPDATE`ステートメントです。 Web ページは次の図のようになります。

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

*CourseContoller.cs*、追加`UpdateCourseCredits`メソッド`HttpGet`と`HttpPost`:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

コント ローラーを処理すると、`HttpGet`要求、何も返されないで、`ViewBag.RowsAffected`変数、およびビューが表示されます、空のテキスト ボックスと [送信] ボタン、前の図に示すようにします。

ときに、**更新**ボタンをクリックして、`HttpPost`メソッドを呼び出すと`multiplier`テキスト ボックスに入力した値を持ちます。 コードの更新を記録するコースのビューに影響を受けた行の数を返します SQL を実行し、`ViewBag.RowsAffected`変数。 ビューは、その値を取得、変数がテキスト ボックスの代わりに更新された行の数を表示され、次の図に示すように、ボタンを送信します。

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

*CourseController.cs*のいずれかを右クリックし、`UpdateCourseCredits`メソッド、およびクリック**ビューの追加**です。

![Add_View_dialog_box_for_Update_Course_Credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

*Views\Course\UpdateCourseCredits.cshtml*、テンプレート コードを次のコードに置き換えます。

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cshtml)]

**[Courses]\(コース\)** タブを選択してから、ブラウザーのアドレス バーで URL の末尾に "/UpdateCourseCredits" を追加して (例: `http://localhost:50205/Course/UpdateCourseCredits`)、`UpdateCourseCredits` メソッドを実行します。 テキスト ボックスに数値を入力します。

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

**[更新]**をクリックします。 影響を受けた行の数が表示されます。

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

**[リストに戻る]** をクリックして、単位数が変更されたコースの一覧を表示します。

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

生の SQL クエリの詳細については、次を参照してください。[生の SQL クエリ](https://msdn.microsoft.com/data/jj592907)msdn です。

<a id="notracking"></a>
## <a name="no-tracking-queries"></a>No 追跡クエリ

データベース コンテキストは、テーブルの行を取得してそれらを表すエンティティ オブジェクトを作成するとき、既定では、メモリ内のエンティティがデータベース内のデータと同期されているかどうかを追跡します。 メモリ内のデータはキャッシュとして機能し、エンティティを更新するときに使われます。 Web アプリケーションでは、一般にコンテキスト インスタンスの存続期間は短く (要求ごとに新しいインスタンスが作成されて破棄されます)、通常、エンティティを読み取るコンテキストはエンティティが再び使われる前に破棄されるので、多くの場合、このようなキャッシュは必要ありません。

使用してメモリ内のエンティティ オブジェクトの追跡を無効にすることができます、 [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx)メソッドです。 追跡を無効にした方がよい一般的なシナリオを以下に示します。

- クエリでは、大量の追跡を無効にするとパフォーマンスを向上させる著しくするデータを取得します。
- 前のさまざまな目的は同じエンティティを取得したが、更新するためにエンティティをアタッチする場合します。 エンティティはデータベース コンテキストによって既に追跡されているため、変更するエンティティをアタッチできません。 この状況に対処する方法の 1 つを使用して、`AsNoTracking`オプションは、以前のクエリを使用します。

使用する方法を示す例については、 [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx)メソッドを参照してください[このチュートリアルの以前のバージョン](../../older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application.md)します。 チュートリアルのこのバージョンは、必要があるために、メソッドでは、編集、モデル バインダーに作成されたエンティティでの Modified フラグを設定しない`AsNoTracking`です。

<a id="sql"></a>
## <a name="examining-sql-sent-to-the-database"></a>データベースに送信する SQL の確認

データベースに送信される実際の SQL クエリを確認できると役立つ場合があります。 以前のチュートリアルでは、インターセプターのコードで操作する方法を説明今すぐをインターセプター コードを書かずに行うにはいくつかの方法が表示されます。 これを試すをするには、単純なクエリを見てし、このような一括読み込み、フィルター、および並べ替えオプションを追加するように動作を確認します。

*コント ローラー/CourseController*、置換、`Index`メソッドを一時的に一括読み込みを停止するのには、次のコード。

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

今すぐにブレークポイントを設定、`return`ステートメント (その行にカーソルを f9 キーを押します)。 F5 キーを押して、プロジェクトをデバッグ モードで実行し、コース インデックス ページを選択します。 コードでは、ブレークポイントに達すると、確認、`sql`変数。 SQL Server に送信されるクエリが表示されます。 単純な`Select`ステートメントです。

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.json)]

拡大鏡クラス内のクエリを表示する をクリックして、**テキスト ビジュアライザー**です。

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

今すぐできるように、ユーザーが特定の部署のフィルター処理できます、コース インデックス ページにドロップダウン リストを追加します。 タイトル、コースが並べ替えられるしの一括読み込みを指定します、`Department`ナビゲーション プロパティ。

*CourseController.cs*、置換、`Index`メソッドを次のコード。

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

ブレークポイントの復元、`return`ステートメントです。

このメソッドは、ドロップダウン リストの選択した値、`SelectedDepartment`パラメーター。 何も選択すると、このパラメーターは null になります。

A`SelectList`ドロップ ダウン リスト ビューにすべての部門を含むコレクションが渡されました。 渡されるパラメーター、`SelectList`コンス トラクターは、値フィールドの名前、テキスト フィールド名、および選択した項目を指定します。

`Get`のメソッド、`Course`リポジトリ、コードを指定するフィルター式、並べ替え順、および一括読み込みの`Department`ナビゲーション プロパティ。 常に、フィルター式を返す`true`かどうか何も選択ドロップダウン リストで (つまり、`SelectedDepartment`が null です)。

*Views\Course\Index.cshtml*、開始する直前に`table`タグで、ドロップダウン リストと送信 ボタンを作成する次のコードを追加します。

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

まだブレークポイントを設定、コース インデックス ページを実行します。 コードが、ブレークポイントをヒットする最初の時間に従い、ページがブラウザーに表示されないようにします。 ドロップダウン リストから、部署を選択し、クリックして**フィルター**:

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

今度は、最初のブレークポイントは、ドロップダウン リストの部門クエリのされます。 スキップして、表示、`query`変数、次回、コードのブレークポイントに達した新機能を確認するために、`Course`ようこれでクエリを実行します。 次のようなものが表示されます。

[!code-sql[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.sql)]

クエリを参照してください、`JOIN`読み込むクエリの`Department`データと共に、`Course`データ、およびが含まれている、`WHERE`句。

削除、`var sql = courses.ToString()`行です。

<a id="repo"></a>

## <a name="repository-and-unit-of-work-patterns"></a>Repository パターンと Unit of Work パターン

多くの開発者は、Entity Framework で動作するコードをラップするラッパーとして、Repository パターンと Unit of Work パターンを実装するためのコードを記述します。 これらのパターンは、アプリケーションのデータ アクセス層とビジネス ロジック層の間に抽象化レイヤーを作成するためのものです。 これらのパターンを実装すると、データ ストアの変更からアプリケーションを隔離でき、自動化された単体テストやテスト駆動開発 (TDD) を円滑化できます。 ただし、これらのパターンを実装する追加のコードの記述は常にいくつかの理由、EF を使用するアプリケーションに最適な選択肢。

- EF コンテキスト クラス自体が、コードをデータ ストア固有のコードから隔離します。
- EF コンテキスト クラスは、EF を使用して行っているデータベースの更新の unit-of-work クラスとして動作できます。
- Entity Framework 6 で導入された機能をやすくリポジトリ コードを記述することがなく TDD を実装します。

リポジトリと作業パターンの単位を実装する方法の詳細については、次を参照してください。[このチュートリアル シリーズの Entity Framework 5 バージョン](../../older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)します。 Entity Framework 6 で TDD を実装する方法については、次のリソースを参照してください。

- [EF6 実現するしくみついて Mocking DbSets より簡単に](http://thedatafarm.com/data-access/how-ef6-enables-mocking-dbsets-more-easily/)
- [モック フレームワークとテスト](https://msdn.microsoft.com/data/dn314429)
- [独自のテスト代替でテストします。](https://msdn.microsoft.com/data/dn314431)

<a id="proxies"></a>
## <a name="proxy-classes"></a>プロキシ クラス

Entity Framework では、エンティティのインスタンス (たとえば、クエリを実行する場合) を作成するときは、エンティティのプロキシとして機能する、動的に生成される派生型のインスタンスとしてそれを多くの場合を作成します。 たとえば、次の 2 つのデバッガー イメージを参照してください。 最初の図で表示される、`student`変数が、予期された`Student`エンティティのインスタンスを作成した後すぐを入力します。 2 番目のイメージで EF 学生エンティティをデータベースから読み取りに使用した後、プロキシ クラスが参照してください。

![プロキシ クラスの前に](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

![プロキシ クラスの後](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

このプロキシ クラスは、プロパティにアクセスする場合、自動的にアクションを実行するためのフックを挿入するエンティティの一部の仮想プロパティをオーバーライドします。 このメカニズムが使用される 1 つの関数は、遅延読み込みです。

ほとんどの場合このプロキシの使用について注意する必要ありませんは例外があります。

- 一部のシナリオでは、Entity Framework がプロキシのインスタンスを作成しないようにすることができます。 たとえば、エンティティをシリアル化しているときに通常するプロキシ クラスではありません、POCO クラスです。 シリアル化の問題を回避する方法の 1 つが、エンティティ オブジェクトの代わりにデータ転送オブジェクト (Dto) をシリアル化のように、 [Entity Framework と Web API を使用して](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-1.md)チュートリアルです。 別の方法は、する[プロキシの作成を無効にする](https://msdn.microsoft.com/data/jj592886.aspx)です。
- ときにインスタンス化する、エンティティ クラスを使用して、`new`演算子、プロキシ インスタンスを取得しません。 これは、遅延読み込みと自動変更の追跡などの機能を取得しないことを意味します。 これは、通常さてです。通常必要はありません、遅延読み込み時、データベースにない新しいエンティティを作成しているため、通常必要し、しない変更の追跡とエンティティを明示的にマークしている場合`Added`です。 ただし、遅延読み込みする必要は、変更の追跡が必要な場合は、インスタンスを作成できます新しいエンティティを使用してプロキシ、[作成](https://msdn.microsoft.com/library/gg679504.aspx)のメソッド、`DbSet`クラスです。
- プロキシの種類から実際のエンティティ型を取得する可能性があります。 使用することができます、 [GetObjectType](https://msdn.microsoft.com/library/system.data.objects.objectcontext.getobjecttype.aspx)のメソッド、`ObjectContext`プロキシ型のインスタンスの実際のエンティティ型を取得するクラス。

詳細については、次を参照してください。[プロキシ扱う](https://msdn.microsoft.com/data/JJ592886.aspx)msdn です。

<a id="changedetection"></a>
## <a name="automatic-change-detection"></a>変更の自動検出

Entity Framework では、エンティティの現在の値と元の値を比較して、エンティティがどのように変更されたか (およびそれによって、どの更新プログラムをデータベースに送信する必要があるか) を判断します。 元の値は、エンティティが照会されるかアタッチされるときに格納されます。 変更の自動検出を行うメソッドには、次のようなものがあります。

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

多数のエンティティを管理するいると、ループ内で何度もがこれらのメソッドのいずれかの呼び出すと、自動変更の検出を使用して機能をオフに一時的に大幅なパフォーマンス向上をする可能性があります、 [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled.aspx)プロパティです。 詳細については、次を参照してください。[変更を自動的に検出する](https://msdn.microsoft.com/data/jj556205)msdn です。

<a id="validation"></a>
## <a name="automatic-validation"></a>自動検証

呼び出すと、`SaveChanges`メソッド、既定では、Entity Framework データを検証、すべての変更されたエンティティのすべてのプロパティで、データベースを更新する前にします。 多数のエンティティを更新したし、して既に検証した、データをこの作業は必要と保存の処理を行うことが、変更は、一時的に検証を無効にすることによって時間を短縮します。 使用して行うことができます、 [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled.aspx)プロパティです。 詳細については、次を参照してください。[検証](https://msdn.microsoft.com/data/gg193959)msdn です。

<a id="tools"></a>
## <a name="entity-framework-power-tools"></a>Entity Framework のパワー ツール

[Entity Framework Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d)データ モデル図を作成するために使用した Visual Studio アドインがこれらのチュートリアルで表示します。 ツールには、Code First でデータベースを使用できるように、既存のデータベース内のテーブルに基づくエンティティ クラスを生成するなどその他の関数も実行できます。 ツールをインストールすると、コンテキスト メニューに追加のオプションが表示されます。 たとえば、右クリックすると、コンテキスト クラスを**ソリューション エクスプ ローラー**図を生成するオプションを取得します。 Code First を使用している場合、ダイアグラムで、データ モデルを変更することはできませんが、理解しやすいようにことを行うことができます。

![コンテキスト メニューで EF](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image14.png)

![EF ダイアグラム](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image15.png)

<a id="source"></a>
## <a name="entity-framework-source-code"></a>Entity Framework のソース コード

Entity Framework 6 のソース コードは[GitHub](https://github.com/aspnet/EntityFramework6)です。 バグ、ファイルを作成でき、EF ソース コードに独自の機能強化に協力することができます。

ソース コードは開いているが、Entity Framework が Microsoft の製品としてサポートされます。 Microsoft Entity Framework チームは、各リリースの品質を保証するため、受け入れる投稿を管理し、すべてのコード変更をテストしています。

<a id="summary"></a>
## <a name="summary"></a>まとめ

これは、この一連の ASP.NET MVC アプリケーションで Entity Framework を使用してチュートリアルを完了します。 Entity Framework を使用してデータを操作する方法の詳細については、次を参照してください。、 [MSDN のドキュメント ページを EF](https://msdn.microsoft.com/data/ee712907)と[ASP.NET データ アクセス - リソースのことをお勧め](../../../../whitepapers/aspnet-data-access-content-map.md)です。

構築した後、web アプリケーションを展開する方法の詳細については、次を参照してください。 [ASP.NET Web 展開 - リソースのことをお勧め](../../../../whitepapers/aspnet-web-deployment-content-map.md)、MSDN ライブラリです。

については、認証および承認など、MVC に関連するその他のトピックを参照してください、 [ASP.NET MVC のリソースをお勧め](../recommended-resources-for-mvc.md)です。

<a id="acknowledgments"></a>
## <a name="acknowledgments"></a>謝辞

- Tom Dykstra このチュートリアルの元のバージョンを作成するには、併置 EF 5 更新プログラムを作成および EF 6 更新プログラムを記述します。 Tom は、Microsoft Web プラットフォームとツール コンテンツ チームのシニア プログラミング ライターです。
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) 5 の EF と MVC 4 のチュートリアルを更新して作業の大半と併置 EF 6 更新プログラムを作成します。 Rick は、Microsoft Azure と MVC に焦点を当てたのスキルの限られたプログラミング ライターです。
- [Rowan Miller](http://www.romiller.com)および Entity Framework チームの他のメンバー コード レビューを支援して EF 5 および 6 の EF のチュートリアルが更新されたことをお中に発生し、移行に関する多くの問題のデバッグを支援します。

<a id="vb"></a>
## <a name="vb"></a>VB

EF 4.1 は、このチュートリアルは生成された最初と、おダウンロードが完了したプロジェクトの c# および VB の両方のバージョンが用意されています。 時間の制限とその他の優先順位によりお作業を行っていないをこのバージョンのです。 これらのチュートリアルを使用して、VB プロジェクトをビルドして可能な項目を共有する他のユーザー、お知らせします。

<a id="errors"></a>
## <a name="common-errors-and-solutions-or-workarounds-for-them"></a>一般的なエラーと解決方法またはそれらの回避策

### <a name="cannot-createshadow-copy"></a>コピーを作成またはシャドウことはできません。

エラー メッセージ:

> コピーを作成またはシャドウできません '&lt;filename&gt;' そのファイルが既に存在する場合。


ソリューション

数秒待ってから、ページを更新します。

### <a name="update-database-not-recognized"></a>データベースの更新認識されません。

エラー メッセージ (から、 `Update-Database` PMC コマンド)。

> 用語 'データベースの更新' は、コマンドレット、関数、スクリプト ファイル、または操作可能なプログラムの名前としては認識されません。 名のスペルを確認するかパスが含まれている場合、パスが正しいことを確認し、もう一度やり直してください。


ソリューション

Visual Studio を終了します。 プロジェクトを開き直すし、もう一度やり直してください。

### <a name="validation-failed"></a>検証に失敗しました

エラー メッセージ (から、 `Update-Database` PMC コマンド)。

> 1 つまたは複数のエンティティの検証に失敗しました。 詳細については 'EntityValidationErrors' プロパティを参照してください。


ソリューション

この問題の原因の 1 つは、検証エラー時に、`Seed`メソッドが実行されます。 参照してください[シード処理、およびデバッグ Entity Framework (EF) Db](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx)のデバッグに関するヒントについては、`Seed`メソッドです。

### <a name="http-50019-error"></a>HTTP 500.19 エラー

エラー メッセージ:

> HTTP エラー 500.19 - 内部サーバー エラー  
> ページの関連する構成データが正しくないため、要求されたページにアクセスできません。


ソリューション

このエラーを取得する方法の 1 つは同じポート番号を使用してそれらの各ソリューションの複数のコピーです。 通常、この問題を解決には、Visual Studio のすべてのインスタンスを終了しで作業するプロジェクトを再起動します。 それでもうまくいかない場合は、ポート番号を変更してください。 プロジェクト ファイルを右クリックし、[プロパティ] をクリックします。 選択、 **Web**  タブをされているポート番号を変更、**プロジェクト Url**テキスト ボックス。

### <a name="error-locating-sql-server-instance"></a>SQL Server インスタンスの位置を特定しているときのエラー

エラー メッセージ:

> SQL Server への接続を確立しているときに、ネットワーク関連またはインスタンス固有のエラーが発生しました。 サーバーが見つからないかアクセスできません。 インスタンス名が正しいこと、および SQL Server がリモート接続を許可するように構成されていることを確認してください。 (プロバイダー: SQL ネットワーク インターフェイス、エラー: 26 - 指定されたサーバーまたはインスタンスの位置を特定しているときにエラーが発生しました)。


ソリューション

接続文字列を確認します。 データベースを手動で削除する場合は、構築文字列でデータベースの名前を変更します。

> [!div class="step-by-step"]
> [前へ](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
