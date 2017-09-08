---
title: "ASP.NET MVC を持つコアを EF Core - 詳細 - 10 10"
author: tdykstra
description: "このチュートリアルでは、Entity Framework のコアを使用する ASP.NET web アプリケーションの開発、基本機能を越えて移動するときに認識する便利ないくつかのトピックを紹介します。"
keywords: "ASP.NET Core、Entity Framework Core、生の sql を調べる sql、リポジトリ パターン、単位の作業パターンでは、自動変更の検出、既存のデータベース"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 92a2986a-d005-4ff6-9559-6657fd466bb7
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/advanced
ms.openlocfilehash: d1c6ece833672af3ef2003510ef96c4ff0d63fbf
ms.sourcegitcommit: 418e6aa4ab79474ecc4d0a6af573a3759b113fe4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/05/2017
---
# <a name="advanced-topics---ef-core-with-aspnet-core-mvc-tutorial-10-of-10"></a>高度なトピックの EF コア ASP.NET Core MVC のチュートリアル (10 10 の)

によって[Tom Dykstra](https://github.com/tdykstra)と[Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso 大学でサンプル web アプリケーションでは、Entity Framework のコアと Visual Studio を使用して ASP.NET Core MVC web アプリケーションを作成する方法を示します。 一連のチュートリアルについては、次を参照してください。[系列内の最初のチュートリアル](intro.md)です。

前のチュートリアルでは、テーブルの階層あたりの継承を実装します。 このチュートリアルでは、Entity Framework のコアを使用する ASP.NET Core web アプリケーションの開発、基本機能を越えて移動するときに認識する便利ないくつかのトピックを紹介します。

## <a name="raw-sql-queries"></a>生の SQL クエリ

Entity Framework を使用する利点の 1 つは、コードのデータを格納する特定のメソッドを過度に結び付ける済む点です。 これは、SQL クエリとコマンドの生成も自分で記述することから解放することで実行します。 手動で作成した特定の SQL クエリを実行する必要がある場合、例外的なシナリオがあります。 このようなシナリオは、Entity Framework コードの最初の API には、使用すると、データベースに直接 SQL コマンドを渡すメソッドが含まれます。 EF Core 1.0 では、次のオプションがあります。

* 使用して、`DbSet.FromSql`をエンティティ型を返すクエリ メソッド。 返されたオブジェクトで想定されている型でなければなりません、`DbSet`オブジェクト、およびそれらが自動的に追跡データベースのコンテキストによってしない限りする[追跡をオフにする](crud.md#no-tracking-queries)です。

* 使用して、`Database.ExecuteSqlCommand`非クエリ コマンド。

エンティティがない型を返すクエリを実行する必要がある場合は、EF によって提供されるデータベース接続で ADO.NET を使用できます。 返されるデータは、エンティティの種類を取得するこのメソッドを使用する場合でもデータベース コンテキストによって追跡されていません。

常に true web アプリケーションで SQL コマンドを実行するときに、としては、SQL インジェクション攻撃からサイトを保護する対策を講じる必要があります。 行うには 1 つの方法では、パラメーター化クエリを使用して、SQL コマンドとして web ページによって送信される文字列を解釈できないことを確認します。 このチュートリアルでは、ユーザー入力をクエリに統合するとき、パラメーター化クエリを使用します。

## <a name="call-a-query-that-returns-entities"></a>エンティティを返すクエリを呼び出す

`DbSet<TEntity>`クラス型のエンティティを返すクエリの実行に使用できるメソッドを提供`TEntity`です。 内のコードを変更するしくみを表示する、`Details`部門コント ローラーのメソッドです。

*DepartmentsController.cs*で、`Details`メソッド、部門を取得するコードを置き換える、`FromSql`メソッドの呼び出し、次の強調表示されたコードに示すように。

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_RawSQL&highlight=8,9,10,13)]

新しいコードが正しく動作することを確認するには、次のように選択します。、**部門**タブし、**詳細**部門のいずれか。

![部門の詳細](advanced/_static/department-details.png)

## <a name="call-a-query-that-returns-other-types"></a>その他の型を返すクエリを呼び出す

以前登録日付ごとの生徒の数が映っているバージョン情報 ページの受講者統計グリッドを作成します。 学生のエンティティ セットからデータを取得した (`_context.Students`) の一覧に結果を射影する LINQ を使用して`EnrollmentDateGroup`モデル オブジェクトを表示します。 自体ではなく LINQ を使用して、SQL を記述するとします。 SQL クエリを実行する必要があることを行うを返すエンティティ オブジェクト以外のものです。 EF Core 1.0 では、これを行うには ADO.NET コードを記述し、EF からデータベース接続を取得できます。

*HomeController.cs*、置換、`About`メソッドを次のコード。

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseRawSQL&highlight=3-32)]

使用して、追加のステートメント。

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings2)]

バージョン情報 ページを実行します。 以前と同じ、同じデータが表示されます。

![ページについて](advanced/_static/about.png)

## <a name="call-an-update-query"></a>更新クエリを呼び出す

Contoso 大学管理者がすべてのコースのクレジットの数の変更などのデータベースでグローバルに変更を実行するとします。 かかる大学のコースの数が多い場合は、できなくなるそれらすべてのエンティティとして取得し、それらを個別に変更が効率的です。 ユーザーが、すべてのコースのクレジットの数を変更する係数を指定できるようにする web ページを実装するこのセクションの内容と SQL UPDATE ステートメントを実行することによって変更されます。 Web ページは、次の図のようになります。

![コースのクレジットのページを更新します。](advanced/_static/update-credits.png)

*CoursesContoller.cs*、HttpGet および HttpPost UpdateCourseCredits メソッドを追加します。

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdateGet)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdatePost)]

何も返されない、コント ローラーは、HttpGet 要求を処理するときに`ViewData["RowsAffected"]`、前の図に示すように空のテキスト ボックスと、[送信] ボタンをビューが表示されます。

ときに、**更新**ボタンがクリックされた、HttpPost メソッドを呼び出して、乗数値を保持して、テキスト ボックスに入力します。 コードの更新を記録するコースのビューに影響を受けた行の数を返します SQL を実行し、`ViewData`です。 ビューを取得すると、`RowsAffected`値、更新された行の数を表示します。

**ソリューション エクスプ ローラー**を右クリックし、*ビュー/コース*フォルダー、およびクリック**追加 > 新しい項目の**します。

**新しい項目の追加**ダイアログ ボックスで、をクリックして**ASP.NET** **インストール**左側のウィンドウでをクリックして**MVC ビュー ページ**、ビューの名前と*UpdateCourseCredits.cshtml*です。

*Views/Courses/UpdateCourseCredits.cshtml*、テンプレート コードを次のコードに置き換えます。

[!code-html[Main](intro/samples/cu/Views/Courses/UpdateCourseCredits.cshtml)]

実行、`UpdateCourseCredits`メソッドを選択して、**コース** タブで、追加し、"/UpdateCourseCredits"ブラウザーのアドレス バーの URL の末尾に (たとえば: `http://localhost:5813/Courses/UpdateCourseCredits`)。 テキスト ボックスに数値を入力します。

![コースのクレジットのページを更新します。](advanced/_static/update-credits.png)

**[更新]**をクリックします。 影響を受ける行の数が参照してください。

![更新コースのクレジットが影響を受ける行をページします。](advanced/_static/update-credits-rows-affected.png)

をクリックして**一覧に戻る**コースのクレジットの改訂された数との一覧を表示します。

常に有効なデータの結果を更新すると実稼働コードで確実に注意してください。 ここで示す簡略化されたコードでは、5 より大きい数値で発生するのに十分なクレジットの数を乗算可能性があります。 (、`Credits`プロパティには、`[Range(0, 5)]`属性です)。更新クエリが機能しますが、無効なデータのクレジットの数は 5 個以下であると、システムの他の部分で予期しない結果が発生する可能性があります。

生の SQL クエリの詳細については、次を参照してください。[生の SQL クエリ](https://docs.microsoft.com/ef/core/querying/raw-sql)です。

## <a name="examine-sql-sent-to-the-database"></a>データベースに送信する SQL を確認します。

データベースに送信される実際の SQL クエリを表示できるようにすると役立つ場合があります。 ASP.NET Core の組み込みのログ記録機能を SQL クエリと更新プログラムを含むログ ファイルを書き込む EF コアに自動的に使用します。 このセクションでは、SQL ログの例をいくつかが表示されます。

開いている*StudentsController.cs*し、、`Details`メソッドにブレークポイントを設定する、`if (student == null)`ステートメントです。

デバッグ モードでアプリケーションを実行し、受講者についての詳細 ページに移動します。

移動して、**出力**デバッグを表示するウィンドウは次の出力、およびクエリを参照してください。

```
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (56ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT TOP(2) [s].[ID], [s].[Discriminator], [s].[FirstName], [s].[LastName], [s].[EnrollmentDate]
FROM [Person] AS [s]
WHERE ([s].[Discriminator] = N'Student') AND ([s].[ID] = @__id_0)
ORDER BY [s].[ID]
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (122ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT [s.Enrollments].[EnrollmentID], [s.Enrollments].[CourseID], [s.Enrollments].[Grade], [s.Enrollments].[StudentID], [e.Course].[CourseID], [e.Course].[Credits], [e.Course].[DepartmentID], [e.Course].[Title]
FROM [Enrollment] AS [s.Enrollments]
INNER JOIN [Course] AS [e.Course] ON [s.Enrollments].[CourseID] = [e.Course].[CourseID]
INNER JOIN (
    SELECT TOP(1) [s0].[ID]
    FROM [Person] AS [s0]
    WHERE ([s0].[Discriminator] = N'Student') AND ([s0].[ID] = @__id_0)
    ORDER BY [s0].[ID]
) AS [t] ON [s.Enrollments].[StudentID] = [t].[ID]
ORDER BY [t].[ID]
```

ここに何かされる驚くかもしれませんがわかります: SQL が最大 2 つの行を選択します (`TOP(2)`)、Person テーブルからです。 `SingleOrDefaultAsync`メソッドは、サーバー上の 1 行に解決しません。 その理由を次に示します。

* クエリでは、複数の行を返すと、メソッドは null を返します。
* クエリが複数の行を返すかどうかを判断するのには EF が少なくとも 2 を返すかどうかを確認する必要です。

デバッグ モードを使用して、出力を取得するログ記録のブレークポイントで停止がないことに注意してください、**出力**ウィンドウです。 だけを容易に出力を確認するポイントでログ記録を停止することをお勧めします。 かどうかしないように、ログ記録を続行しようとしているに関心があるパーツの検索に戻るまでスクロールします。

## <a name="repository-and-unit-of-work-patterns"></a>リポジトリと作業パターンの単位

多くの開発者は、Entity Framework で動作するコードをラップするラッパーとしてリポジトリと作業パターンの単位を実装するコードを記述します。 これらのパターンは、データ アクセス層とアプリケーションのビジネス ロジック層の間で抽象化レイヤーを作成するためのものです。 これらのパターンを実装して、変更によって、データ ストアにアプリケーションを分離できます、自動化された単体テストまたはテスト駆動開発 (TDD) に容易にすることができます。 ただし、これらのパターンを実装する追加のコードの記述は常にいくつかの理由、EF を使用するアプリケーションに最適な選択肢。

* EF コンテキスト クラス自体は、データ ストア固有のコードからコードを分離します。

* EF コンテキスト クラスは、データベースの作業単位クラス更新 EF を使用してを実行することで動作できます。

* EF には、リポジトリのコードを記述することがなく TDD を実装するための機能が含まれています。

リポジトリと作業パターンの単位を実装する方法については、次を参照してください。[このチュートリアル シリーズの Entity Framework 5 バージョン](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application)します。

Entity Framework Core では、テストに使用できるメモリ内のデータベース プロバイダーを実装します。 詳細については、次を参照してください。 [InMemory でテスト](https://docs.microsoft.com/ef/core/miscellaneous/testing/in-memory)です。

## <a name="automatic-change-detection"></a>自動の変更の検出

Entity Framework では、元の値を持つエンティティの現在の値を比較することによってエンティティがどのように変更されたか (およびしたがって、更新プログラムがデータベースに送信する必要があります) を決定します。 元の値は、エンティティがクエリを実行したり、接続されているときに格納されます。 自動の変更の検出が発生する方法の一部を次に示します。

* DbContext.SaveChanges

* DbContext.Entry

* ChangeTracker.Entries

多数のエンティティを管理するいると、ループ内で何度もがこれらのメソッドのいずれかの呼び出すと、自動変更の検出を使用して機能をオフに一時的に大幅なパフォーマンス向上をする可能性があります、`ChangeTracker.AutoDetectChangesEnabled`プロパティです。 例:

```csharp
_context.ChangeTracker.AutoDetectChangesEnabled = false;
```

## <a name="entity-framework-core-source-code-and-development-plans"></a>Entity Framework Core のソース コードと開発計画

Entity Framework Core のソース コードは[https://github.com/aspnet/EntityFramework](https://github.com/aspnet/EntityFramework)です。 ソース コードだけでなくすることができます夜間ビルドを取得、懸案事項の管理、機能仕様、設計ミーティングのメモ[将来開発のためのロードマップ](https://github.com/aspnet/EntityFramework/wiki/Roadmap)などです。 バグ、ファイルを作成でき、EF ソース コードに独自の機能強化に協力することができます。

ソース コードは開いているが、Entity Framework のコアが完全にマイクロソフト製品サポートします。 Microsoft Entity Framework チームは、コントロール コントリビューションの受け入れを保持し、各リリースの品質を保証するすべてのコード変更をテストします。

## <a name="reverse-engineer-from-existing-database"></a>既存のデータベースをリバース エンジニア リング

既存のデータベースからのエンティティ クラスを含むデータ モデルをリバース エンジニア リングを使用して、 [scaffold dbcontext](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext)コマンド。 参照してください、[入門チュートリアル](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db)です。

<a id="dynamic-linq"></a>
## <a name="use-dynamic-linq-to-simplify-sort-selection-code"></a>動的な LINQ 並べ替え選択コードを簡略化を使用してください。

[このシリーズの第 3 のチュートリアル](sort-filter-page.md)で列名をハード コーディングでの LINQ のコードを記述する方法を示しています、`switch`ステートメントです。 2 つの列から選択するでは、これは正常に機能しますが、コードの詳細な取得でした多数の列がある場合。 問題を解決するために使用することができます、`EF.Property`を文字列として、プロパティの名前を指定します。 このアプローチには、置換、`Index`メソッドで、`StudentsController`を次のコード。

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DynamicLinq)]

## <a name="next-steps"></a>次のステップ

これは、この一連の ASP.NET MVC アプリケーションで Entity Framework のコアを使用してチュートリアルを完了します。

EF Core の詳細については、次を参照してください。、 [Entity Framework の主要なマニュアル](https://docs.microsoft.com/ef/core)です。 本も利用できます:[アクションで Entity Framework Core](https://www.manning.com/books/entity-framework-core-in-action)です。

作成した後、web アプリケーションを展開する方法については、次を参照してください。[発行および配置](../../publishing/index.md)です。

については、認証および承認など、ASP.NET Core MVC に関連するその他のトピックを参照してください、 [ASP.NET Core ドキュメント](https://docs.microsoft.com/aspnet/core/)です。

## <a name="acknowledgments"></a>受信確認

Tom Dykstra と Rick Anderson (twitter @RickAndMSFT) このチュートリアルを記述します。 Rowan Miller、Diego ヴェガおよび Entity Framework チームの他のメンバー コード レビューを支援し、チュートリアルのコードを記述おされたときに発生した問題のデバッグに役に立ちます。

## <a name="common-errors"></a>一般的なエラー  

### <a name="contosouniversitydll-used-by-another-process"></a>別のプロセスによって使用される ContosoUniversity.dll

エラー メッセージ:

> 開くことができません '... bin\Debug\netcoreapp1.0\ContosoUniversity.dll' 書き込み--用 '、プロセスがファイルにアクセスできません'... \bin\Debug\netcoreapp1.0\ContosoUniversity.dll' 別のプロセスによって使用されているためです。

解決方法 : 

IIS express サイトを停止します。 Windows のシステム トレイに IIS Express しそのアイコンを右クリックし、Contoso 大学サイトを選択してを見つけてクリックを参照してください**サイトの停止**です。

### <a name="migration-scaffolded-with-no-code-in-up-and-down-methods"></a>コードを使用せずにメソッドを上下スキャフォールディングされた移行

考えられる原因:

EF CLI コマンドしない自動的に閉じてコード ファイルを保存します。 未保存の変更を実行すると場合、`migrations add`コマンド、EF、変更は検出されません。

解決方法 : 

実行、`migrations remove`コマンドをコードの変更を保存して再実行、`migrations add`コマンド。

### <a name="errors-while-running-database-update"></a>実行中のデータベースの更新中にエラー

既存のデータを含むデータベースでスキーマ変更を行うときにその他のエラーが発生することができます。 移行エラーを解決できない場合は、いずれか、接続文字列にデータベース名を変更したり、データベースを削除できます。 新しいデータベースを移行するデータが存在しないと、データベースの更新コマンドがエラーなしで完了する可能性が高くなります。

データベースの名前を変更する最も簡単な方法は、*される appsettings.json*です。 次に実行したとき`database update`、新しいデータベースは作成されます。

SSOX でデータベースを削除するデータベースを右クリックして**削除**、し、**データベースの削除**ダイアログ ボックスの [**既存の接続を閉じる** ]をクリック**[Ok]**です。

CLI を使用してデータベースを削除するには、実行、 `database drop` CLI コマンド。

```console
dotnet ef database drop
```

### <a name="error-locating-sql-server-instance"></a>SQL Server インスタンスの検索エラー

エラー メッセージ:

> SQL Server への接続を確立中にネットワーク関連またはインスタンス固有のエラーが発生しました。 サーバーが見つからないかアクセスできません。 インスタンス名が正しいこと、および SQL Server がリモート接続を許可するように構成されていることを確認してください。 (プロバイダー: SQL Network Interfaces、エラー: 26 - エラーの検索のサーバー/インスタンスの指定)

解決方法 : 

接続文字列を確認します。 データベース ファイルを手動で削除する場合は、新しいデータベースを最初からやり直す構築文字列でデータベースの名前を変更します。

>[!div class="step-by-step"]
[前へ](inheritance.md)
