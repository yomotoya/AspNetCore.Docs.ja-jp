---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
title: ASP.NET MVC アプリケーション (9/10) で、リポジトリと Unit of Work パターンを実装する |Microsoft Docs
author: tdykstra
description: Contoso University のサンプルの web アプリケーションでは、Entity Framework 5 Code First と Visual Studio を使用して ASP.NET MVC 4 アプリケーションを作成する方法について説明しています.
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 44761193-04ba-4990-9f90-145d3c10a716
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 20f82744582faddaffdc5b4785bf208ecf0955a7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828782"
---
<a name="implementing-the-repository-and-unit-of-work-patterns-in-an-aspnet-mvc-application-9-of-10"></a>ASP.NET MVC アプリケーション (9/10) で、リポジトリと Unit of Work パターンを実装します。
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[完成したプロジェクトのダウンロード](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso University のサンプルの web アプリケーションでは、Entity Framework 5 Code First と Visual Studio 2012 を使用して ASP.NET MVC 4 アプリケーションを作成する方法を示します。 チュートリアル シリーズについては、[シリーズの最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)をご覧ください。 チュートリアルのシリーズを開始するには、最初からまたは[この章のスタート プロジェクトをダウンロード](building-the-ef5-mvc4-chapter-downloads.md)し、ここから始めてください。
> 
> > [!NOTE] 
> > 
> > を解決できない問題が生じた場合[章では、完了したダウンロード](building-the-ef5-mvc4-chapter-downloads.md)の問題を再現しようとします。 問題の解決策は、完成したコードにコードを比較することによって一般的に見つかります。 一般的なエラーとその解決方法は、次を参照してください。[エラーと回避策。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


前のチュートリアルでの冗長なコードを削減する継承を使用して、`Student`と`Instructor`エンティティ クラスです。 このチュートリアルでは、CRUD 操作のリポジトリと unit of work パターンを使用するいくつかの点を確認します。 このいずれかで、前のチュートリアルでは、ように、コードの新しいページを作成するのではなく、既に作成したページの動作を変更します。

## <a name="the-repository-and-unit-of-work-patterns"></a>リポジトリと Unit of Work パターン

リポジトリと unit of work パターンは、データ アクセス層とアプリケーションのビジネス ロジック層の間の抽象化レイヤーを作成する対象としています。 これらのパターンを実装すると、データ ストアの変更からアプリケーションを隔離でき、自動化された単体テストやテスト駆動開発 (TDD) を円滑化できます。

このチュートリアルでは、各エンティティ型のリポジトリ クラスを実装します。 `Student`エンティティ型が、リポジトリ インターフェイスと、リポジトリ クラスを作成します。 コント ローラーで、リポジトリをインスタンス化するときは、コント ローラーはリポジトリ インターフェイスを実装する任意のオブジェクトへの参照をそのまま使用できるように、インターフェイスを使用します。 Web サーバー、コント ローラーを実行すると、Entity Framework で動作するリポジトリを受け取ります。 コント ローラーを単体テスト クラスの下で実行すると、メモリ内コレクションなど、テストを簡単に操作できる方法で格納されているデータで動作するリポジトリを受け取ります。

チュートリアルの後半では、複数のリポジトリと単位の作業のクラスを使用します、`Course`と`Department`エンティティ型、`Course`コント ローラー。 クラスの作業の単位は、それらすべてによって共有される 1 つのデータベース コンテキスト クラスを作成して複数のリポジトリの作業を調整します。 自動化された単体テストを実行できる場合は、作成し、これらのクラスの場合と同様にインターフェイスを使用して、`Student`リポジトリ。 ただし、チュートリアルを簡潔にするを作成し、インターフェイスせずにこれらのクラスを使用します。

次の図は、コント ローラーとまったく使用しないリポジトリまたは unit of work パターンと比較するコンテキスト クラス間の関係を理解する 1 つの方法を示します。

![Repository_pattern_diagram](https://asp.net/media/2578149/Windows-Live-Writer_8c4963ba1fa3_CE3B_Repository_pattern_diagram_1df790d3-bdf2-4c11-9098-946ddd9cd884.png)

このチュートリアル シリーズでは、単体テストを作成しません。 概要については TDD リポジトリ パターンを使用して MVC アプリケーションで、次を参照してください。[チュートリアル: ASP.NET MVC で TDD を使用して](https://msdn.microsoft.com/library/ff847525.aspx)します。 リポジトリ パターンの詳細については、次のリソースを参照してください。

- [リポジトリ パターン](https://msdn.microsoft.com/library/ff649690.aspx)msdn です。
- [Entity Framework 4.0 でリポジトリと Unit of Work パターンを使用して](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)Entity Framework チームのブログ。
- [アジャイルの Entity Framework 4 リポジトリ](http://thedatafarm.com/blog/data-access/agile-entity-framework-4-repository-part-1-model-and-poco-classes/)Julie Lerman のブログ投稿のシリーズです。
- [ビルド概要の HTML5 と jQuery アプリケーションでアカウント](https://weblogs.asp.net/dwahlin/archive/2011/08/15/building-the-account-at-a-glance-html5-jquery-application.aspx)Dan Wahlin のブログ。

> [!NOTE]
> リポジトリと unit of work パターンを実装する方法はたくさんあります。 クラスの作業の単位の有無は、リポジトリ クラスを使用することができます。 すべてのエンティティ型、または種類ごとに 1 つ 1 つのリポジトリを実装することができます。 種類ごとに 1 つを実装する場合は、別のクラス、ジェネリック基底クラスと派生クラスまたは抽象基本クラスおよび派生クラスを使用できます。 リポジトリでビジネス ロジックを含むまたはデータ アクセス ロジックに制限できます。 使用して、データベース コンテキスト クラスに抽象化レイヤーを構築することもできます[IDbSet](https://msdn.microsoft.com/library/gg679233(v=vs.103).aspx)の代わりにインターフェイスがあります[DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx)種類、エンティティ セット。 このチュートリアルで示すように抽象化レイヤーを実装する方法は、いないすべてのシナリオおよび環境の推奨事項を考慮するための 1 つのオプションです。


## <a name="creating-the-student-repository-class"></a>学生のリポジトリ クラスを作成します。

*DAL*フォルダー、という名前のクラス ファイルを作成する*IStudentRepository.cs*既存のコードを次のコードに置き換えます。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample1.cs)]

このコードは 2 つの読み取りメソッドを含む、CRUD メソッドの標準的なセットを宣言します-1 つをすべて返す`Student`エンティティ、および 1 つを検索する 1 つ`Student`ID によってエンティティ

*DAL*フォルダー、という名前のクラス ファイルを作成する*StudentRepository.cs*ファイル。 実装する次のコードで、既存のコードを置き換える、`IStudentRepository`インターフェイス。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample2.cs)]

データベース コンテキストはクラスの変数で定義され、コンス トラクターは、コンテキストのインスタンスを渡すには、呼び出し元のオブジェクトが必要ですが。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample3.cs)]

リポジトリで、新しいコンテキストをインスタンス化する可能性がありますが、1 つのコント ローラーで複数のリポジトリを使用した場合の各は別のコンテキストでします。 後で複数のリポジトリを使用します、`Course`コント ローラー、わかる方法クラスの作業の単位がすべてのリポジトリが、同じコンテキストを使用することを確認することができます。

リポジトリを実装して[IDisposable](https://msdn.microsoft.com/library/system.idisposable.aspx)しコント ローラーは、前述のように、CRUD メソッドは、前述と同じ方法でデータベースのコンテキストに呼び出しを行う、データベース コンテキストを破棄します。

## <a name="change-the-student-controller-to-use-the-repository"></a>リポジトリを使用する学生コント ローラーを変更します。

*StudentController.cs*クラスの現在のコードを次のコードに置き換えます。 変更が強調表示されます。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=13-18,44,75,77,102-103,120,137-138,159,172-174,186)]

コント ローラーは今すぐを実装するオブジェクトのクラス変数を宣言、`IStudentRepository`コンテキスト クラスではなくインターフェイス。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample5.cs)]

既定の (パラメーターなしの) コンス トラクターが新しいコンテキスト インスタンスを作成し、省略可能なコンス トラクターにより、呼び出し元コンテキスト インスタンスを渡します。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample6.cs)]

(使用していた場合*依存関係の注入*DI、DI ソフトウェアは、適切なリポジトリ オブジェクトは常に指定することを確認するため、既定のコンス トラクターを必要はありませんか)。

CRUD のメソッドでコンテキストの代わりに、リポジトリと呼ばれるようになりました。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample7.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample8.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample9.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample10.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample11.cs)]

および`Dispose`メソッドは、コンテキストではなく、リポジトリを破棄するようになりました。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample12.cs)]

サイトを実行し、をクリックして、**学生**タブ。

![Students_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image1.png)

ページは、外観し、リポジトリを使用するコードを変更して、その他の学生のページも同じ動作前に、と同じように動作します。 ただし、方法で重要な違いがある、`Index`コント ローラーのメソッドはフィルター処理と並べ替え。 このメソッドの元のバージョンには、次のコードが含まれています。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample13.cs?highlight=1)]

更新された`Index`メソッドには、次のコードが含まれています。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=1)]

強調表示されたコードのみが変更されました。

コードの元のバージョンで`students`として型指定された、`IQueryable`オブジェクト。 などのメソッドを使用してコレクションに変換されるまで、データベースにクエリが送信されない`ToList`、student モデルが、インデックス ビューにアクセスするまで発生しません。 `Where`上の元のコードでメソッドになります、`WHERE`データベースに送信される SQL クエリ内の句。 さらに、選択したエンティティのみがデータベースによって返されることを意味します。 変更した結果として、ただし、`context.Students`に`studentRepository.GetStudents()`、`students`後、このステートメントは、変数、`IEnumerable`で、データベース内のすべての学生を含むコレクション。 適用した結果、`Where`メソッドには、web サーバーとデータベースではなく、メモリ内処理を実行するようになりました。 大量のデータを返すクエリでは、これはできない効率的です。

> [!TIP]
> 
> **Vs の IQueryable。IEnumerable**
> 
> 実装した後、リポジトリ、次のように値を入力する場合でも、**検索**ボックス SQL Server に送信されるクエリでは、検索条件は含まれないため、すべての学生行が返されます。
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image2.png)
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample15.sql)]
> 
> このクエリはリポジトリ検索条件の詳細について知ることがなく、クエリを実行するためには、学生データのすべてを返します。 プロセスは、並べ替え、検索条件を適用すること、およびメモリ内で行われます (この場合のみに 3 つの行を表示) ページング、データのサブセットを選択するときに後で、`ToPagedList`メソッドが、`IEnumerable`コレクション。
> 
> (前に、リポジトリを実装する) コードの以前のバージョンで、クエリは送信されませんまでデータベースに、検索条件を適用した後ときに`ToPagedList`で呼び出される、`IQueryable`オブジェクト。
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image3.png)
> 
> ToPagedList が呼び出された場合、`IQueryable`オブジェクト、SQL Server に送信されるクエリは、検索文字列を指定し、検索条件を満たす行のみが返された結果として、およびフィルター処理する必要はありませんが、メモリ内で実行します。
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample16.sql)]
> 
> (次のチュートリアルでは、SQL Server に送信されるクエリを調査する方法を説明します)。


次のセクションでは、データベースでこの作業を行う必要がありますを指定するためのリポジトリ メソッドを実装する方法を示します。

コント ローラーと Entity Framework データベース コンテキストの間の抽象化レイヤーが作成されました。 実装する単体テスト プロジェクトで別のリポジトリ クラスを作成することが自動化された単体テストでこのアプリケーションを実行する場合は、 `IStudentRepository`*します。* データを読み書きするコンテキストを呼び出す代わりにこのモック リポジトリ クラスは、コント ローラーの機能をテストするには、インメモリ コレクションを操作する可能性があります。

## <a name="implement-a-generic-repository-and-a-unit-of-work-class"></a>汎用リポジトリと単位作業クラスを実装します。

多くの冗長なコードは、結果でした各エンティティ型のリポジトリ クラスを作成したり、部分的な更新プログラムになる可能性があります。 たとえば、同じトランザクションの一部として 2 つの異なるエンティティ タイプを更新する必要があるとします。 それぞれには、別のデータベース コンテキストのインスタンスが使用されている場合が成功した 1 つと、もう一方が失敗する可能性があります。 冗長なコードを最小限に抑える方法の 1 つは汎用的なリポジトリでは、および確認する方法を使用することのすべてのリポジトリと同じデータベース コンテキストを使用して (およびしたがってすべての更新プログラムを調整) はクラスの作業の単位を使用します。

チュートリアルのこのセクションで作成します、`GenericRepository`クラスと`UnitOfWork`クラス、およびそれらを使用して、`Course`両方にアクセスするコント ローラー、 `Department` 、`Course`エンティティ セット。 前述のようをチュートリアルのこの部分をシンプルにするは作成して、これらのクラスのインターフェイス。 使用して TDD を容易にする場合は通常実装するインターフェイスを持つ、同じ方法が、`Student`リポジトリ。

### <a name="create-a-generic-repository"></a>汎用的なリポジトリを作成します。

*DAL*フォルダー作成*GenericRepository.cs*既存のコードを次のコードに置き換えます。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample17.cs)]

データベース コンテキストとエンティティ セットのリポジトリがインスタンス化される、クラス変数が宣言されます。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample18.cs)]

コンス トラクターは、データベース コンテキスト インスタンスを受け取り、エンティティ セットの変数を初期化します。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample19.cs)]

`Get`メソッドでは、ラムダ式を使用して、フィルター条件と列で、結果の順序を指定するには、呼び出し元のコードを許可して、文字列パラメーターにより、一括読み込みでナビゲーション プロパティのコンマ区切りのリストは、呼び出し元。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample20.cs)]

コード`Expression<Func<TEntity, bool>> filter`呼び出し元がラムダ式に基づくを提供することを意味、`TEntity`型、およびこの式はブール値を返します。 リポジトリがのインスタンス化される場合など、`Student`エンティティ型の場合は、呼び出し元のメソッドのコードを指定`student => student.LastName == "Smith`&quot;の`filter`パラメーター。

コード`Func<IQueryable<TEntity>, IOrderedQueryable<TEntity>> orderBy`呼び出し元がラムダ式を渡すことも意味します。 式への入力は、ここでは、`IQueryable`オブジェクト、`TEntity`型。 式は、順序付けられたバージョンを返します`IQueryable`オブジェクト。 リポジトリがのインスタンス化される場合など、`Student`エンティティ型の場合は、呼び出し元のメソッドのコードを指定`q => q.OrderBy(s => s.LastName)`の`orderBy`パラメーター。

コードでは、`Get`メソッドを作成、`IQueryable`オブジェクトし、1 つを使用する必要がある場合、フィルター式を適用します。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample21.cs)]

次に、コンマ区切りの一覧を解析した後、一括読み込みの式が適用されます。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample22.cs)]

最後に、適用、`orderBy`式のいずれかを使用する必要がある場合の結果を返しますそれ以外の場合、順序なしのクエリから結果を返します。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample23.cs)]

呼び出すと、`Get`メソッド、フィルター処理とでの並べ替えを実行できます、`IEnumerable`これらの関数のパラメーターを提供するのではなく、メソッドによって返されるコレクション。 ただし、web サーバー上のメモリで並べ替えおよびフィルター処理を行うことが。 これらのパラメーターを使用すると、web サーバーではなく、データベースでの作業が実行されたことを確認します。 特定のエンティティ型の派生クラスの作成を追加する特殊な代替策は`Get`メソッドなど`GetStudentsInNameOrder`または`GetStudentsByName`します。 ただし、複雑なアプリケーションの場合は、これにより、多数のような派生クラスと特殊なメソッド、維持するためにより多くの作業である可能性があります。

内のコード、 `GetByID`、 `Insert`、および`Update`メソッドは、非ジェネリック リポジトリでの表示内容に似ています。 (一括読み込みパラメーターを提供することはありません、`GetByID`のシグネチャで一括読み込みを行うことはできませんので、`Find`メソッド)。

2 つのオーバー ロードが用意されて、`Delete`メソッド。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample24.cs)]

削除するエンティティの ID のみによりこれらのいずれかを渡すし、エンティティ インスタンスは、1 つ。 見たよう、[同時実行の処理](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)同時実行処理する必要があるは、チュートリアル、`Delete`エンティティ インスタンスを受け取るメソッドには、追跡プロパティの元の値が含まれています。

この汎用リポジトリでは、通常の CRUD 要求を処理します。 特定のエンティティ型により複雑なフィルター処理や、順序付けなどの特別な要件がある場合は、派生クラスであり、その型の追加のメソッドを作成できます。

## <a name="creating-the-unit-of-work-class"></a>クラスの作業の単位を作成します。

クラスの作業の単位は 1 つの目的を果たします。 ように複数のリポジトリを使用すると、1 つのデータベース コンテキストを共有します。 これにより、作業単位が完了すると呼び出すことができます、`SaveChanges`コンテキストのインスタンスに対してメソッドし関連するすべての変更内容が調整することを保証します。 クラス必要なものはすべて、`Save`メソッドと各リポジトリのプロパティ。 リポジトリの各プロパティは、他のリポジトリ インスタンスとして同じデータベース コンテキスト インスタンスを使用してインスタンス化されているリポジトリ インスタンスを返します。

*DAL*フォルダー、という名前のクラス ファイルを作成する*UnitOfWork.cs*テンプレート コードを次のコードに置き換えます。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample25.cs)]

コードは、データベース コンテキストと各リポジトリ クラス変数を作成します。 `context`変数、新しいコンテキストをインスタンス化されます。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample26.cs)]

リポジトリの各プロパティは、リポジトリが既に存在するかどうかを確認します。 それ以外の場合は、コンテキスト インスタンスを渡して、リポジトリがインスタンス化します。 その結果、すべてのリポジトリは、同じコンテキスト インスタンスを共有します。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample27.cs)]

`Save`メソッド呼び出し`SaveChanges`データベース コンテキストでします。

などの任意のクラスの変数では、データベース コンテキストをインスタンス化するクラス、`UnitOfWork`クラスが実装する`IDisposable`とコンテキストを破棄します。

### <a name="changing-the-course-controller-to-use-the-unitofwork-class-and-repositories"></a>リポジトリおよび UnitOfWork クラスを使用するコースのコント ローラーを変更します。

現在のあるコードに置き換えます*CourseController.cs*を次のコード。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample28.cs?highlight=15,20,22,31,54-55,70,85-86,101-102,122-124,130)]

このコードは追加のクラスの変数、`UnitOfWork`クラス。 (ここで、インターフェイスを使用していた場合、は、ここで変数を初期化するでしょう以外の場合と同様にコンス トラクターの 2 つのパターンを実装するは代わりに、、`Student`リポジトリです。)。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample29.cs)]

クラスの残りの部分で、データベース コンテキストに対するすべての参照は置き換えられます、適切なリポジトリへの参照を使用して`UnitOfWork`リポジトリにアクセスするプロパティ。 `Dispose`メソッドを破棄します、`UnitOfWork`インスタンス。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample30.cs)]

サイトを実行し、をクリックして、**コース**タブ。

![Courses_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image4.png)

ページは、外観し、変更内容は、先ほどし、他のページのコースと同じ動作も同じように動作します。

## <a name="summary"></a>まとめ

リポジトリと unit of work パターンの両方が実装されました。 汎用リポジトリ内のメソッド パラメーターとしてラムダ式を使用しています。 これらの式を使用する方法について、`IQueryable`オブジェクトを参照してください[IQueryable(T) インターフェイス (System.Linq)](https://msdn.microsoft.com/library/bb351562.aspx) MSDN ライブラリ。 次のチュートリアルの一部を処理する方法について説明しますには、シナリオが高度な。

その他の Entity Framework リソースへのリンクが記載されて、 [ASP.NET データ アクセス コンテンツ マップ](../../../../whitepapers/aspnet-data-access-content-map.md)します。

> [!div class="step-by-step"]
> [前へ](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [次へ](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
