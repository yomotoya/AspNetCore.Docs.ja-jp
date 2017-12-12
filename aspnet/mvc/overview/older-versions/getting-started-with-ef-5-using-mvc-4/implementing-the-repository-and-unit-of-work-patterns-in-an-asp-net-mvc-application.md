---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
title: "ASP.NET MVC アプリケーション (9 10 の) リポジトリおよび作業パターンの単位を実装する |Microsoft ドキュメント"
author: tdykstra
description: "Contoso 大学でサンプル web アプリケーションでは、Entity Framework 5 Code First と Visual Studio を使用して ASP.NET MVC 4 アプリケーションを作成する方法について説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 44761193-04ba-4990-9f90-145d3c10a716
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: c920dc8defe18b6f27d122c2cd1a6c6ffdaad608
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="implementing-the-repository-and-unit-of-work-patterns-in-an-aspnet-mvc-application-9-of-10"></a>ASP.NET MVC アプリケーション (9 10 の) リポジトリおよび作業パターンの単位を実装します。
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[完成したプロジェクトをダウンロードします。](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso 大学でサンプル web アプリケーションでは、Entity Framework 5 Code First と Visual Studio 2012 を使用して ASP.NET MVC 4 アプリケーションを作成する方法を示します。 一連のチュートリアルについては、次を参照してください。[系列内の最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)です。 一連のチュートリアルを開始するには、最初からまたは[この章のスタート プロジェクトをダウンロード](building-the-ef5-mvc4-chapter-downloads.md)し、ここから開始します。
> 
> > [!NOTE] 
> > 
> > 解決できない場合、問題が発生した場合[ダウンロード完了章](building-the-ef5-mvc4-chapter-downloads.md)の問題を再現しようとします。 一般に、コードを完成したコードを比較することによって、問題の解決策を検索できます。 一般的なエラーとそれらを解決する方法は、次を参照してください。[エラーと回避策です。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


前のチュートリアルでの冗長なコードを削減する継承を使用して、`Student`と`Instructor`エンティティ クラスです。 このチュートリアルでの CRUD 操作のリポジトリと作業パターンの単位を使用するいくつかの方法が表示されます。 このいずれかで、前のチュートリアルと同様に、コードが新しいページを作成するのではなく、既に作成したページと機能の仕方を変更します。

## <a name="the-repository-and-unit-of-work-patterns"></a>リポジトリと作業パターンの単位

リポジトリと作業パターンの単位は、データ アクセス層とアプリケーションのビジネス ロジック層の間で抽象化レイヤーを作成します。 これらのパターンを実装して、変更によって、データ ストアにアプリケーションを分離できます、自動化された単体テストまたはテスト駆動開発 (TDD) に容易にすることができます。

このチュートリアルでは各エンティティ タイプのリポジトリ クラスを実装します。 `Student`リポジトリ インターフェイスおよびリポジトリ クラスを作成するエンティティ型。 コント ローラーでリポジトリをインスタンス化するときに、コント ローラーがリポジトリ インターフェイスを実装する任意のオブジェクトへの参照を受け入れるように、インターフェイスを使用します。 Web サーバーの下で、コント ローラーを実行すると、Entity Framework と連携する、リポジトリを受け取ります。 単体テスト クラスの下で、コント ローラーが実行されると、テストでは、メモリ内コレクションなどの簡単に操作できるように格納されているデータと連携する、リポジトリを受け取ります。

このチュートリアルで後ほど使用する複数のリポジトリとの作業のクラスの単体、`Course`と`Department`エンティティ型、`Course`コント ローラー。 クラスの作業の単位は、それらすべてによって共有される 1 つのデータベース コンテキスト クラスを作成することで複数のリポジトリの作業を調整します。 自動化された単体テストを実行できる場合は、作成し、これらのクラスが、同じ方法でインターフェイスを使用して、`Student`リポジトリです。 ただし、チュートリアルを簡潔にするを作成し、インターフェイスせずにこれらのクラスを使用します。

次の図は、コント ローラーとまったく使用しないリポジトリまたはパターンの作業の単位と比較してコンテキスト クラス間の関係を理解する 1 つの方法を示します。

![Repository_pattern_diagram](https://asp.net/media/2578149/Windows-Live-Writer_8c4963ba1fa3_CE3B_Repository_pattern_diagram_1df790d3-bdf2-4c11-9098-946ddd9cd884.png)

このチュートリアルの一連の単体テストを作成できません。 概要については TDD リポジトリ パターンを使用して MVC アプリケーションで、次を参照してください。[チュートリアル: ASP.NET MVC で TDD を使用して](https://msdn.microsoft.com/en-us/library/ff847525.aspx)です。 リポジトリ パターンの詳細については、次のリソースを参照してください。

- [リポジトリ パターン](https://msdn.microsoft.com/en-us/library/ff649690.aspx)msdn です。
- [Entity Framework 4.0 でリポジトリと作業単位のパターンを使用して](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)Entity Framework チームのブログです。
- [アジャイルの Entity Framework 4 リポジトリ](http://thedatafarm.com/blog/data-access/agile-entity-framework-4-repository-part-1-model-and-poco-classes/)Julie Lerman のブログの投稿の系列です。
- [概要の HTML5 と jQuery アプリケーションでアカウントを構築](https://weblogs.asp.net/dwahlin/archive/2011/08/15/building-the-account-at-a-glance-html5-jquery-application.aspx)Dan Wahlin ブログ。

> [!NOTE]
> リポジトリと作業パターンの単位を実装する方法はたくさんあります。 クラスの作業の単位の有無は、リポジトリ クラスを使用することができます。 すべてのエンティティ型、または種類ごとに 1 つの 1 つのリポジトリを実装することができます。 種類ごとに 1 つを実装する場合は、別のクラス、ジェネリックの基底クラスと派生クラス、または抽象基本クラスおよび派生クラスを使用することができます。 リポジトリにビジネス ロジックを含めるしたり、データ アクセス ロジックに制限できます。 使用して、データベース コンテキスト クラスに抽象化レイヤーを作成することも[した IDbSet](https://msdn.microsoft.com/en-us/library/gg679233(v=vs.103).aspx)の代わりにあるインターフェイス[DbSet](https://msdn.microsoft.com/en-us/library/system.data.entity.dbset(v=vs.103).aspx)エンティティ セットの種類。 このチュートリアルで示すように抽象化レイヤーを実装する方法は、1 つのオプションを使用する、すべてのシナリオおよび環境の推奨設定されませんです。


## <a name="creating-the-student-repository-class"></a>学生リポジトリ クラスを作成します。

*DAL*フォルダー、という名前のクラス ファイルを作成する*IStudentRepository.cs*し、既存のコードを次のコードに置き換えます。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample1.cs)]

このコードは、2 つの読み取りメソッドを含む、CRUD メソッドの標準的なセットを宣言 — すべてを返す`Student`エンティティ、および 1 つを検索する 1 つ`Student`ID によってエンティティ

*DAL*フォルダー、という名前のクラス ファイルを作成する*StudentRepository.cs*ファイル。 実装する次のコードで、既存のコードを置き換えます、`IStudentRepository`インターフェイス。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample2.cs)]

クラスの変数で、データベース コンテキストが定義されているし、コンス トラクターは、コンテキストのインスタンスを渡すには、呼び出し元のオブジェクトが必要です。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample3.cs)]

リポジトリに新しいコンテキストをインスタンス化する可能性がありますが、し、1 つのコント ローラー内の複数のリポジトリを使用した各しまいますは別のコンテキストでします。 内の複数のリポジトリを使用してを後で、`Course`コント ローラー、およびする作業クラスの単体ようにする方法のすべてのリポジトリが、同じコンテキストを使用することが表示されます。

リポジトリを実装する[IDisposable](https://msdn.microsoft.com/en-us/library/system.idisposable.aspx)コント ローラーは、前述のように、CRUD メソッドでは、データベース コンテキストに呼び出しを行う先ほど見たのと同じ方法では、データベース コンテキストを破棄します。

## <a name="change-the-student-controller-to-use-the-repository"></a>リポジトリを使用する、受講者コント ローラーの変更

*StudentController.cs*クラスの現在のコードを次のコードに置き換えます。 変更が強調表示されます。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=13-18,44,75,77,102-103,120,137-138,159,172-174,186)]

コント ローラーは今すぐを実装するオブジェクトのクラス変数を宣言、`IStudentRepository`コンテキスト クラスの代わりにインターフェイス。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample5.cs)]

既定 (パラメーターなし) コンス トラクターが、新しいコンテキストのインスタンスを作成し、省略可能なコンス トラクターでは、呼び出し元のコンテキストのインスタンスを渡します。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample6.cs)]

(を使用していた場合*依存性の注入*、または DI、DI ソフトウェアは、適切なリポジトリ オブジェクト名は常に提供されることを確認してくださいため既定のコンス トラクターを必要はありません。)。

CRUD メソッドでコンテキストの代わりに、リポジトリと呼ばれるようになりました。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample7.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample8.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample9.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample10.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample11.cs)]

および`Dispose`メソッドでは、コンテキストの代わりにリポジトリを破棄します。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample12.cs)]

サイトを実行し、クリックして、**受講者**タブです。

![Students_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image1.png)

ページは、検索し、同様に、リポジトリを使用するコードを変更して、他のページの受講者も同様に動作する前に、同じように動作します。 ただし、方法で重要な違いがある、`Index`コント ローラーのメソッドはフィルター処理と順序付けします。 このメソッドの元のバージョンには、次のコードが含まれています。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample13.cs?highlight=1)]

更新された`Index`メソッドには、次のコードが含まれています。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=1)]

強調表示されたコードだけが変更されました。

コードの元のバージョンで`students`として型指定されて、`IQueryable`オブジェクト。 などのメソッドを使用してコレクションに変換するまで、クエリがデータベースに送信されていない`ToList`、インデックス ビューにアクセスする、受講者モデルまで発生しません。 `Where`上の元のコードでメソッドになります、`WHERE`データベースに送信される SQL クエリ内の句。 さらに、選択したエンティティだけがデータベースによって返されることを意味します。 変更した結果として、ただし、`context.Students`に`studentRepository.GetStudents()`、`students`後、このステートメントは、変数、`IEnumerable`データベース内のすべての受講者を含むコレクション。 適用する際の最終結果、`Where`メソッドは同じですが、およびデータベースではなく、web サーバー上のメモリ内で作業を実行するようになりました。 大量のデータを返すクエリでこのできます効率的です。

> [!TIP] 
> 
> **Vs の IQueryable。IEnumerable**
> 
> 実装した後、リポジトリ、ここで示すように値を入力する場合でも、**検索**ボックス SQL Server に送信されるクエリが、検索条件が含まれていないので、すべての学生行を返します。
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image2.png)
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample15.sql)]
> 
> このクエリは、リポジトリの検索条件に関するがわからなくても、クエリを実行するためのすべての学生データを返します。 プロセスの並べ替え、検索条件を適用すること、およびページング (ここでは 3 個の行が表示される) がメモリ内で実行のデータのサブセットを選択するときに後で、`ToPagedList`でメソッドが呼び出さ、`IEnumerable`コレクション。
> 
> (リポジトリを実装して) する前に、コードの以前のバージョンで、クエリがデータベースに送信されません、まで、検索条件を適用した後と`ToPagedList`で呼び出されると、`IQueryable`オブジェクト。
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image3.png)
> 
> ToPagedList が呼び出されたとき、`IQueryable`オブジェクト、SQL Server に送信されるクエリ、検索文字列を指定し、その結果を検索条件を満たす行だけが返されます、およびフィルター処理する必要はありませんメモリ内で実行します。
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample16.sql)]
> 
> (以下のチュートリアルは、SQL Server に送信されるクエリを確認する方法を説明します)。


次のセクションでは、データベースでこの作業を行う必要がありますを指定できるリポジトリ メソッドを実装する方法を示します。

コント ローラーと、Entity Framework データベースのコンテキストの抽象化レイヤーが作成されました。 実装する単体テスト プロジェクトでリポジトリの代替クラスを作成する自動化された単体テストでこのアプリケーションを実行する場合は、でした`IStudentRepository`*です。* データを読み書きするコンテキストを呼び出す代わりにこのモック リポジトリ クラスは、コント ローラーの機能をテストするためにメモリ内コレクションを操作する可能性があります。

## <a name="implement-a-generic-repository-and-a-unit-of-work-class"></a>汎用的なリポジトリと作業クラスの単位を実装します。

多くの冗長なコードは、各エンティティ型に対してリポジトリ クラスを作成することがあり、部分的な更新プログラムを可能性があります。 たとえば、同じトランザクションの一部として 2 つの別のエンティティ型を更新する必要があるとします。 それぞれには、別のデータベース コンテキストのインスタンスが使用されている場合は、成功いずれかの可能性があります、もう一方が失敗します。 重複するコードを最小限に抑える方法の 1 つは、汎用的なリポジトリとことを確認する方法の 1 つを使用するすべてのリポジトリが同じデータベース コンテキストを使用して (およびためすべての更新プログラムを調整) は作業クラスの単位を使用します。

チュートリアルのこのセクションで作成、`GenericRepository`クラスおよび`UnitOfWork`クラス、およびそれらを使用して、`Course`両方にアクセスするコント ローラー、`Department`と`Course`エンティティ セット。 前述のように、チュートリアルのこの部分をシンプルに作成するには、これらのクラス インターフェイス TDD を容易にするために使用された場合は、通常実装するそれらのインターフェイスで実行したのと同様ですが、`Student`リポジトリです。

### <a name="create-a-generic-repository"></a>汎用的なリポジトリを作成します。

*DAL*フォルダー作成*GenericRepository.cs*し、既存のコードを次のコードに置き換えます。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample17.cs)]

クラスの変数は、データベースのコンテキストとのリポジトリがインスタンス化されるエンティティ セットに対して宣言されます。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample18.cs)]

コンス トラクターは、データベース コンテキストのインスタンスを受け取り、エンティティ セットの変数を初期化します。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample19.cs)]

`Get`メソッドでは、ラムダ式を使用して、フィルター条件と、結果の並べ替えに列を指定する呼び出し元のコードを許可して、文字列パラメーターにより、呼び出し元は、一括読み込みのナビゲーション プロパティのコンマ区切りの一覧を提供します。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample20.cs)]

コード`Expression<Func<TEntity, bool>> filter`、呼び出し元は、ラムダ式に基づいて、なります、`TEntity`型、およびこの式はブール値を返します。 リポジトリがのインスタンス化される場合など、`Student`エンティティ型、呼び出し元のメソッドのコードを指定`student => student.LastName == "Smith`&quot;の`filter`パラメーター。

コード`Func<IQueryable<TEntity>, IOrderedQueryable<TEntity>> orderBy`も呼び出し元がラムダ式を渡すことを意味します。 この場合、式への入力、`IQueryable`オブジェクトに対して、`TEntity`型です。 式は順序付けられたバージョンを返します`IQueryable`オブジェクト。 リポジトリがのインスタンス化される場合など、`Student`エンティティ型、呼び出し元のメソッドのコードを指定`q => q.OrderBy(s => s.LastName)`の`orderBy`パラメーター。

内のコード、`Get`メソッドを作成、`IQueryable`オブジェクトを 1 つを使用する必要がある場合、フィルター式を適用します。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample21.cs)]

次に、コンマ区切りの一覧を解析した後は、一括読み込みの式が適用されます。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample22.cs)]

最後に、適用される、`orderBy`式のいずれかを使用する必要がある場合です。 その結果を返しますそれ以外の場合、順序付けられていないクエリから結果を返します。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample23.cs)]

呼び出すと、`Get`メソッド、適用するにはフィルター処理と並べ替え、`IEnumerable`これらの関数のパラメーターを提供する代わりに、メソッドによって返されるコレクション。 Web サーバー上のメモリに並べ替えと作業をフィルター処理を完了できるとします。 これらのパラメーターを使用すると、web サーバーではなく、データベースでの処理を実行することを確認します。 代わりに、特定のエンティティ型の派生クラスを作成し、特殊な追加`Get`メソッドなど`GetStudentsInNameOrder`または`GetStudentsByName`です。 ただし、複雑なアプリケーションでは、これが生じることが非常に多数のような派生クラスと特殊なメソッドより多くの作業を維持する可能性があります。

内のコード、 `GetByID`、 `Insert`、および`Update`メソッドは、非ジェネリック リポジトリで学習した内容に似ています。 (の一括読み込みパラメーターを提供することは、`GetByID`署名で一括読み込みを行うことはできませんので、`Find`メソッドです)。

2 つのオーバー ロードに提供される、`Delete`メソッド。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample24.cs)]

削除するエンティティの ID のみによりこれらのいずれかを渡すし、エンティティ インスタンスを受け取り、1 つです。 説明したとおり、[同時実行の処理](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)同時処理する必要があるは、チュートリアル、`Delete`をエンティティのインスタンスを受け取るメソッドには、追跡プロパティの元の値が含まれています。

この汎用的なリポジトリでは、一般的な CRUD 要件を処理します。 特定のエンティティ型により複雑なフィルター処理や、順序付けなどの特別な要件がある場合は、その種類の追加方法を持つ派生クラスを作成できます。

## <a name="creating-the-unit-of-work-class"></a>クラスの作業の単位を作成します。

クラスの作業の単位は 1 つの目的を果たします。 ように複数のリポジトリを使用すると、1 つのデータベース コンテキストを共有します。 こうすれば、作業の単位が完了すると呼び出すことができます、`SaveChanges`メソッド コンテキストのインスタンスを関連するすべての変更が連携することが確実に実行するとします。 クラスの必要なものはすべて、`Save`メソッドおよび各リポジトリのプロパティです。 リポジトリの各プロパティは、他のリポジトリ インスタンスと同じデータベース コンテキストのインスタンスを使用してインスタンス化されているリポジトリのインスタンスを返します。

*DAL*フォルダー、という名前のクラス ファイルを作成する*UnitOfWork.cs*テンプレート コードを次のコードに置き換えます。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample25.cs)]

コードでは、データベースのコンテキストと各リポジトリ用のクラス変数を作成します。 `context`変数、新しいコンテキストがインスタンス化します。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample26.cs)]

リポジトリの各プロパティは、リポジトリが既に存在するかどうかを確認します。 それ以外の場合は、コンテキストのインスタンスを渡して、リポジトリをインスタンス化します。 その結果、すべてのリポジトリは、同じコンテキスト インスタンスを共有します。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample27.cs)]

`Save`メソッド呼び出し`SaveChanges`データベース コンテキストでします。

クラスの変数で、データベース コンテキストをインスタンス化するすべてのクラスと同様に、`UnitOfWork`クラスが実装する`IDisposable`コンテキストを破棄します。

### <a name="changing-the-course-controller-to-use-the-unitofwork-class-and-repositories"></a>UnitOfWork クラスとリポジトリを使用するコース コント ローラーを変更します。

現在のあるコードを置き換える*CourseController.cs*を次のコード。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample28.cs?highlight=15,20,22,31,54-55,70,85-86,101-102,122-124,130)]

このコードがクラスに変数を追加、`UnitOfWork`クラスです。 (ここで、インターフェイスを使用していた場合、は、ここに変数を初期化する考えないから以外場合と同様に 2 つのコンス トラクターのパターンを実装するが代わりの場合は、`Student`リポジトリです)。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample29.cs)]

クラスの残りの部分で、データベース コンテキストに対するすべての参照に置換された、適切なリポジトリへの参照を使用して`UnitOfWork`リポジトリにアクセスするプロパティです。 `Dispose`メソッドの破棄、`UnitOfWork`インスタンス。

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample30.cs)]

サイトを実行し、クリックして、**コース**タブです。

![Courses_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image4.png)

ページは、検索し、変更内容は、以前と同じし、他のページのコースはまた、同じ動作と、同様に機能します。

## <a name="summary"></a>概要

リポジトリと作業パターンの単位の両方を実装しているようになりました。 ジェネリックのリポジトリ内のメソッド パラメーターとしてラムダ式を使用しています。 これらの式を使用する方法の詳細についての`IQueryable`オブジェクトを参照してください[IQueryable(T) インターフェイス (System.Linq)](https://msdn.microsoft.com/en-us/library/bb351562.aspx) MSDN ライブラリです。 次のチュートリアルの一部を処理する方法を学習には、シナリオが高度な。

その他の Entity Framework リソースへのリンクは含まれて、 [ASP.NET データ アクセス コンテンツ マップ](../../../../whitepapers/aspnet-data-access-content-map.md)です。

>[!div class="step-by-step"]
[前へ](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[次へ](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
