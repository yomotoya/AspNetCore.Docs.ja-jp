---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-cs
title: LINQ to SQL (c#) でモデル クラスを作成 |Microsoft Docs
author: microsoft
description: このチュートリアルの目的では、1 つの ASP.NET MVC アプリケーションのモデル クラスを作成する方法について説明します。 このチュートリアルでは、c のモデルを構築する方法について説明します.
ms.author: riande
ms.date: 10/07/2008
ms.assetid: f84b4a16-e8bb-49e8-87a0-1832879a3501
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-cs
msc.type: authoredcontent
ms.openlocfilehash: c76e92ebfab1db162151ea5753888a60a4d54e59
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832397"
---
<a name="creating-model-classes-with-linq-to-sql-c"></a>LINQ to SQL (c#) でモデル クラスを作成
====================
によって[Microsoft](https://github.com/microsoft)

[PDF のダウンロード](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_10_CS.pdf)

> このチュートリアルの目的では、1 つの ASP.NET MVC アプリケーションのモデル クラスを作成する方法について説明します。 このチュートリアルでは、モデル クラスを構築し、活用して Microsoft LINQ to SQL データベースへのアクセスを実行する方法について説明します。


このチュートリアルの目的では、1 つの ASP.NET MVC アプリケーションのモデル クラスを作成する方法について説明します。 このチュートリアルでは、モデル クラスを構築し、活用して Microsoft LINQ to SQL データベースへのアクセスを実行する方法がについて説明します。

このチュートリアルでは、基本的な映画データベース アプリケーションを構築します。 まず、可能な最も高速かつ最も簡単な方法で、映画データベース アプリケーションを作成します。 私たちは、コント ローラー アクションから直接、データ アクセスのすべてを実行します。

次に、リポジトリ パターンを使用する方法について説明します。 リポジトリ パターンを使用して、もう少し作業が必要です。 ただし、このパターンを採用することの利点に対応できるアプリケーションを構築できるを変更して、簡単にテストできます。

## <a name="what-is-a-model-class"></a>モデル クラスとは何ですか。

MVC モデルには、MVC ビューまたは MVC コント ローラーに含まれていないアプリケーション ロジックのすべてが含まれます。 具体的には、MVC モデルには、すべてのアプリケーションのビジネスおよびデータ アクセス ロジックが含まれています。

データ アクセス ロジックを実装するのに多様なテクノロジを使用できます。 たとえば、Microsoft Entity Framework、NHibernate、Subsonic、または ADO.NET のクラスを使用して、データ アクセス クラスをビルドすることができます。

このチュートリアルでは、クエリを実行し、データベースを更新する LINQ to SQL を使用します。 LINQ to SQL は、Microsoft SQL Server データベースとの対話の非常に簡単なメソッドを提供します。 ただし、ASP.NET MVC フレームワークが何らかの方法で関連付け linq to SQL されませんを理解しておく必要です。 ASP.NET MVC では、任意のデータ アクセス テクノロジと互換性が。

## <a name="create-a-movie-database"></a>ムービー データベースを作成します。

-このチュートリアルではモデル クラスを作成する方法を説明するためにアプリケーションの作成簡単なムービー データベース。 最初の手順では、新しいデータベースを作成します。 アプリを右クリックして\_メニュー オプションを選択して、ソリューション エクスプ ローラー ウィンドウで、データ フォルダー**追加]、[新しい項目の**します。 選択、 **SQL Server データベース**テンプレート、MoviesDB.mdf、名前を付け、**追加**(図 1 参照) ボタンをクリックします。


[![新しい SQL Server データベースを追加します。](creating-model-classes-with-linq-to-sql-cs/_static/image2.png)](creating-model-classes-with-linq-to-sql-cs/_static/image1.png)

**図 01**: 新しい SQL Server データベースの追加 ([フルサイズの画像を表示する をクリックします](creating-model-classes-with-linq-to-sql-cs/_static/image3.png))。


新しいデータベースを作成した後、アプリで MoviesDB.mdf ファイルをダブルクリックして、データベースを開くことができます\_データ フォルダー。 MoviesDB.mdf ファイルをダブルクリックすると、サーバー エクスプ ローラー ウィンドウを開きます (図 2 参照)。

サーバー エクスプ ローラー ウィンドウは Visual Web Developer を使用する場合、データベース エクスプ ローラー ウィンドウと呼ばれます。


[![サーバー エクスプ ローラー ウィンドウの使用](creating-model-classes-with-linq-to-sql-cs/_static/image5.png)](creating-model-classes-with-linq-to-sql-cs/_static/image4.png)

**図 02**: サーバー エクスプ ローラー ウィンドウを使用して ([フルサイズの画像を表示する をクリックします](creating-model-classes-with-linq-to-sql-cs/_static/image6.png))。


1 つのテーブルをムービーを表すデータベースに追加する必要があります。 [テーブル] フォルダーを右クリックし、メニュー オプションを選択**新しいテーブルの追加**します。 このメニュー オプションを選択すると、テーブル デザイナーを開きます (図 3 を参照してください)。


[![サーバー エクスプ ローラー ウィンドウの使用](creating-model-classes-with-linq-to-sql-cs/_static/image8.png)](creating-model-classes-with-linq-to-sql-cs/_static/image7.png)

**図 03**: テーブル デザイナー ([フルサイズの画像を表示する をクリックします](creating-model-classes-with-linq-to-sql-cs/_static/image9.png))。


このデータベース テーブルに、次の列を追加する必要があります。

| **列名** | **データ型** | **Null を許容します。** |
| --- | --- | --- |
| ID | Int | False |
| Title | Nvarchar(200) | False |
| ディレクター | nvarchar (50) | False |

Id 列に 2 つの特別な作業を行う必要があります。 最初に、テーブル デザイナーで列を選択し、キーのアイコンをクリックして、主キー列として Id 列をマークする必要があります。 LINQ to SQL では、挿入またはデータベースに対する更新を実行するときに、主キー列を指定する必要があります。

次に、値を [はい] に割り当てることで、Id 列として Id 列をマークする必要があります、 **Id**プロパティ (図 3 を参照してください)。 Id 列では、テーブルに新しいデータの行を追加するたびに新しい番号を自動的に割り当てられている列です。

## <a name="create-linq-to-sql-classes"></a>LINQ to SQL クラスを作成します。

MVC モデルは、LINQ を tblMovie データベースのテーブルを表す SQL クラスに含まれます。 これらの LINQ to SQL クラスを作成する最も簡単な方法では、Models フォルダーを右クリックして、**追加、新しい項目の**、LINQ to SQL クラス テンプレートの選択、Movie.dbml、名前のクラスを提供および をクリックして、**追加**(図 4 参照) ボタンをクリックします。


[![LINQ to SQL クラスを作成](creating-model-classes-with-linq-to-sql-cs/_static/image11.png)](creating-model-classes-with-linq-to-sql-cs/_static/image10.png)

**図 04**: LINQ to SQL クラスを作成する ([フルサイズの画像を表示する をクリックします](creating-model-classes-with-linq-to-sql-cs/_static/image12.png))。


ムービーの LINQ to SQL クラスを作成した後にすぐに、オブジェクト リレーショナル デザイナーが表示されます。 LINQ to SQL クラスを表す特定のデータベース テーブルを作成するには、オブジェクト リレーショナル デザイナー、サーバー エクスプ ローラー ウィンドウからデータベース テーブルをドラッグすることができます。 オブジェクト リレーショナル デザイナーに tblMovie データベース テーブルを追加する必要があります (図 5 を参照してください)。


[![オブジェクト リレーショナル デザイナーの使用](creating-model-classes-with-linq-to-sql-cs/_static/image14.png)](creating-model-classes-with-linq-to-sql-cs/_static/image13.png)

**図 05**: Object Relational Designer を使用して ([フルサイズの画像を表示する をクリックします](creating-model-classes-with-linq-to-sql-cs/_static/image15.png))。


既定では、オブジェクト リレーショナル デザイナーは、デザイナーにドラッグするデータベース テーブルとまったく同じ名前のクラスを作成します。 ただし、このクラスを呼び出すたく`tblMovie`します。 そのため、デザイナーでクラスの名前をクリックし、ムービーにクラスの名前を変更します。

最後に、をクリックして、**保存**(フロッピー ディスクの画像) をクリックすると、LINQ to SQL クラスに保存します。 それ以外の場合、LINQ to SQL クラスは、オブジェクト リレーショナル デザイナーによって生成されません。

## <a name="using-linq-to-sql-in-a-controller-action"></a>コント ローラーのアクションで LINQ to SQL の使用

あるので、LINQ to SQL クラス、データベースからデータを取得するのにこれらのクラスを使用できます。 このセクションでは、コント ローラー アクション内で直接 SQL クラスを LINQ を使用する方法について説明します。 MVC ビューで tblMovies データベース テーブルからのムービーの一覧表示します。

最初に、HomeController クラスを変更する必要があります。 このクラスは、アプリケーションのコント ローラーのフォルダーにあります。 次のリスト 1 で、クラスのようにクラスを変更します。

**1 – を一覧表示します。 `Controllers\HomeController.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample1.cs)]

`Index()`リスト 1 でのアクションは、LINQ to SQL DataContext クラスを使用 (、 `MovieDataContext`) を表す、`MoviesDB`データベース。 `MoveDataContext`クラスは、Visual Studio オブジェクト リレーショナル デザイナーによって生成されました。

ビデオをすべて取得する DataContext に対して LINQ クエリが実行される、`tblMovies`データベース テーブル。 ムービーのリストがという名前のローカル変数に割り当てられている`movies`します。 最後に、ムービーの一覧は、ビュー データをビューに渡されます。

ビデオを表示するには次に、インデックス ビューを変更する必要があります。 インデックスのビューを見つけることができます、`Views\Home\`フォルダー。 リスト 2 で、ビューのように見えるように、インデックス ビューを更新します。

**2 – を一覧表示します。 `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample2.aspx)]

変更したのインデックス ビューが含まれていますが、`<%@ import namespace %>`ビューの上部にあるディレクティブ。 このディレクティブをインポート、`MvcApplication1.Models namespace`します。 この名前空間を使用するために必要、 `model` – 具体的には、クラス、`Movie`ビュー クラス。

リスト 2 でビューが含まれています、`foreach`によって表される項目のすべてを反復処理するループ、`ViewData.Model`プロパティ。 値、`Title`ごとに表示されるプロパティ`movie`します。

注意の値、`ViewData.Model`プロパティにキャスト、`IEnumerable`します。 これは、内容をループ処理するために必要な`ViewData.Model`します。 もう 1 つのオプションをここでは、厳密に型指定を作成する`view`します。 厳密に型を作成するときに`view`、キャストする、`ViewData.Model`プロパティをビューの分離コード クラスで特定の型。

変更した後、アプリケーションを実行する場合、`HomeController`クラスと、インデックス ビューが空白のページが表示されます。 ムービーのレコードが存在しないために、空白のページが表示されます、`tblMovies`データベース テーブル。

レコードを追加するには、`tblMovies`データベース テーブルを右クリックして、`tblMovies`サーバー エクスプ ローラー ウィンドウ (Visual Web Developer でのデータベース エクスプ ローラー ウィンドウ) でテーブルをデータベースし、テーブル データの表示 メニュー オプションを選択します。 挿入できます`movie`(図 6 参照) に表示されるグリッドを使用してレコード。


[![映画を挿入します。](creating-model-classes-with-linq-to-sql-cs/_static/image17.png)](creating-model-classes-with-linq-to-sql-cs/_static/image16.png)

**図 06**: 映画の挿入 ([フルサイズの画像を表示する をクリックします](creating-model-classes-with-linq-to-sql-cs/_static/image18.png))。


データベースのいくつかのレコードを追加した後、`tblMovies`アプリケーションを実行して、テーブル、図 7 ページが表示されます。 箇条書きリストには、すべてのムービーのデータベース レコードが表示されます。


[![インデックスが表示されたムービーを表示します。](creating-model-classes-with-linq-to-sql-cs/_static/image20.png)](creating-model-classes-with-linq-to-sql-cs/_static/image19.png)

**図 07**: インデックスが表示されたムービーを表示する ([フルサイズの画像を表示する をクリックします](creating-model-classes-with-linq-to-sql-cs/_static/image21.png))。


## <a name="using-the-repository-pattern"></a>リポジトリ パターンを使用します。

前のセクションでは、コント ローラー アクション内で直接 SQL クラスを LINQ を使用しました。 使用して、`MovieDataContext`クラスから直接、`Index()`コント ローラーのアクション。 これを行うと、単純なアプリケーションの場合に問題があります。 ただし、コント ローラー クラスでの LINQ to SQL を直接操作より複雑なアプリケーションを構築する必要がある場合、問題を作成します。

コント ローラー クラス内で LINQ to SQL を使用すると、データ アクセス テクノロジの今後のスイッチを困難になります。 たとえば、Microsoft LINQ to SQL を使用して、データ アクセス テクノロジとして Microsoft Entity Framework を使用するからに切り替えることがあります。 その場合は、アプリケーション内のデータベースにアクセスするすべてのコント ローラーを書き直す必要があります。

コント ローラー クラス内で LINQ to SQL を使用しても困難になります、アプリケーションの単体テストをビルドします。 通常、単体テストを実行するときに、データベースと対話しない操作を行います。 単体テストを使用して、アプリケーション ロジックと、データベース サーバーではなくをテストするには。

将来を適応性のある変更を MVC アプリケーションを作成するをより簡単にテストできるリポジトリ パターンの使用を検討する必要があります。 リポジトリ パターンを使用する場合は、すべてのデータベース アクセス ロジックを含む別のリポジトリ クラスを作成します。

リポジトリ クラスを作成するときに、すべてのリポジトリ クラスで使用されるメソッドを表すインターフェイスを作成します。 コント ローラー内では、リポジトリではなくインターフェイスに対して、コードを記述します。 これにより、今後のさまざまなデータ アクセス テクノロジを使用してリポジトリを実装できます。

リスト 3 のインターフェイスの名前は`IMovieRepository`という単一のメソッドを表します`ListAll()`します。

**3 – を一覧表示します。 `Models\IMovieRepository.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample3.cs)]

リスト 4 リポジトリ クラスを実装して、`IMovieRepository`インターフェイス。 という名前のメソッドが含まれている通知`ListAll()`で必要なメソッドに対応する、`IMovieRepository`インターフェイス。

**4 – を一覧表示します。 `Models\MovieRepository.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample4.cs)]

最後に、`MoviesController`リスト 5 クラスは、リポジトリ パターンを使用します。 不要になった LINQ to SQL クラスを直接使用します。

**5 – を一覧表示します。 `Controllers\MoviesController.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample5.cs)]

なお、`MoviesController`リスト 5 でクラスが 2 つのコンス トラクター。 アプリケーションが実行されている最初のコンス トラクター パラメーターなしのコンス トラクターが呼び出されます。 このコンス トラクターのインスタンスを作成する、`MovieRepository`クラスを 2 番目のコンス トラクターに渡します。

2 番目のコンス トラクターが 1 つのパラメーター:`IMovieRepository`パラメーター。 このコンス トラクターは、という名前のクラスレベル フィールドにパラメーターの値を代入するだけ`_repository`です。

`MoviesController`クラスは依存関係の注入パターンと呼ばれるソフトウェアの設計パターンの利用ができます。 具体的には、コンス トラクターの依存関係挿入と呼ばれるものを使用しています。 詳細については、Martin Fowler が次の記事を参照して、このパターン。

[http://martinfowler.com/articles/injection.html](http://martinfowler.com/articles/injection.html)

注意してすべてのコードで、 `MoviesController` (最初のコンス トラクター) を除くクラスの対話、`IMovieRepository`インターフェイスではなく、実際`MovieRepository`クラス。 コードは、インターフェイスの具象実装ではなく抽象インターフェイスと対話します。

データ アクセス テクノロジを使って、アプリケーションを変更するかどうかは、単純に実装することができます、`IMovieRepository`別のデータベース アクセス テクノロジを使用するクラスとのインターフェイス。 たとえば、作成する、`EntityFrameworkMovieRepository`クラスまたは`SubSonicMovieRepository`クラス。 コント ローラー クラスがインターフェイスに対してプログラムを作成するための新しい実装を渡すことができます`IMovieRepository`コント ローラーにクラスとクラスは継続して動作します。

さらに、テストする場合、`MoviesController`クラスに偽のムービーのリポジトリ クラスを渡すことができます、`HomeController`します。 実装することができます、`IMovieRepository`クラスは実際にはアクセス、データベースがすべての必要なメソッドを含むクラスを使用して、`IMovieRepository`インターフェイス。 これにより、単体テストができます、`MoviesController`実際には、実際のデータベースにアクセスしなくてもクラス。

## <a name="summary"></a>まとめ

このチュートリアルの目的を活用して Microsoft LINQ to SQL、MVC モデル クラスを作成する方法を示すためでした。 ASP.NET MVC アプリケーションでデータベースのデータを表示するための 2 つの戦略を調べる。 最初に、LINQ to SQL クラスが作成し、コント ローラー アクション内で直接クラスを使用します。 コント ローラー内の SQL クラスを LINQ を使用して簡単にすることができ、MVC アプリケーションでデータベースのデータを簡単に表示します。

次に、データベースのデータを表示するため少し困難ですが、間違いなくより好パスを説明します。 リポジトリ パターンの利点をすべて、データベース アクセス ロジックの別のリポジトリ クラスに配置します。 コント ローラーの具象クラスではなくインターフェイスに対して、コードをすべて作成しました。 リポジトリ パターンの利点は、今後のデータベース アクセス テクノロジを簡単に変更することによりを簡単に、コント ローラー クラスをテストすることによりです。

> [!div class="step-by-step"]
> [前へ](creating-model-classes-with-the-entity-framework-cs.md)
> [次へ](displaying-a-table-of-database-data-cs.md)
