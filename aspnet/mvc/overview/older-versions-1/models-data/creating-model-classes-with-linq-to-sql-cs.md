---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-cs
title: LINQ to SQL (c#) でモデル クラスを作成する |Microsoft ドキュメント
author: microsoft
description: このチュートリアルの目的では、ASP.NET MVC アプリケーション用のモデル クラスを作成する方法の 1 つをについて説明します。 このチュートリアルでは、c のモデルを構築する方法を学習しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: f84b4a16-e8bb-49e8-87a0-1832879a3501
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-cs
msc.type: authoredcontent
ms.openlocfilehash: 9a56ceb9eab5774906ecc89ce9da570d4f691a82
ms.sourcegitcommit: 466300d32f8c33e64ee1b419a2cbffe702863cdf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/27/2018
ms.locfileid: "34555314"
---
<a name="creating-model-classes-with-linq-to-sql-c"></a>LINQ to SQL (c#) でモデル クラスを作成します。
====================
によって[Microsoft](https://github.com/microsoft)

[PDF のダウンロード](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_10_CS.pdf)

> このチュートリアルの目的では、ASP.NET MVC アプリケーション用のモデル クラスを作成する方法の 1 つをについて説明します。 このチュートリアルでは、モデル クラスを構築し、利用してマイクロソフトの LINQ to SQL データベースへのアクセスを実行する方法を学習します。


このチュートリアルの目的では、ASP.NET MVC アプリケーション用のモデル クラスを作成する方法の 1 つをについて説明します。 このチュートリアルでは、モデル クラスを構築し、利用してマイクロソフトの LINQ to SQL データベースへのアクセスを実行する方法を学びます

このチュートリアルでは、ムービーの基本的なデータベース アプリケーションを構築します。 可能な最も高速で簡単な方法で、ムービーのデータベース アプリケーションを作成して開始します。 すべてのデータ アクセスをコント ローラーのアクションから直接実行します。

次に、リポジトリ パターンを使用する方法を説明します。 リポジトリ パターンを使用するには、少しの作業が必要です。 ただし、このパターンを採用するに対応するアプリケーションを構築できるを変更して、簡単にテストすることができます。

## <a name="what-is-a-model-class"></a>モデル クラスとは何ですか。

MVC モデルには、すべての MVC ビューまたは MVC コント ローラーに含まれないアプリケーション ロジックが含まれます。 具体的には、MVC モデルには、すべてのアプリケーションの業務とデータ アクセス ロジックが含まれています。

さまざまな異なるテクノロジを使用すると、データ アクセス ロジックを実装します。 たとえば、Microsoft の Entity Framework や NHibernate、Subsonic、ADO.NET のクラスを使用して、データ アクセス クラスをビルドすることができます。

このチュートリアルでは、クエリを実行し、データベースを更新する LINQ to SQL を使用します。 LINQ to SQL は、Microsoft SQL Server データベースとの対話の非常に簡単な方法を提供します。 ただし、こと、ASP.NET MVC フレームワークに関連付けられていない LINQ to SQL で任意の方法を理解しておく必要があります。 ASP.NET MVC は、任意のデータ アクセス テクノロジとの互換性。

## <a name="create-a-movie-database"></a>ムービーのデータベースを作成します。

このチュートリアル--モデル クラスを構築する方法を説明するために単純なムービー データベース アプリケーションお構築します。 最初の手順では、新しいデータベースを作成します。 アプリケーションを右クリックして\_メニュー オプションを選択し、ソリューション エクスプ ローラーのウィンドウで、データ フォルダー**追加]、[新しい項目の**します。 選択、 **SQL Server データベース**テンプレート、MoviesDB.mdf、名前を付けるし、をクリックして、**追加**(図 1 を参照してください) ボタンをクリックします。


[![新しい SQL Server データベースを追加します。](creating-model-classes-with-linq-to-sql-cs/_static/image2.png)](creating-model-classes-with-linq-to-sql-cs/_static/image1.png)

**図 01**: 新しい SQL Server データベースを追加する ([フルサイズのイメージを表示するをクリックして](creating-model-classes-with-linq-to-sql-cs/_static/image3.png))


新しいデータベースを作成した後は、アプリで MoviesDB.mdf ファイルをダブルクリックして、データベースを開くことができます\_データ フォルダーです。 サーバー エクスプ ローラー ウィンドウを開き、MoviesDB.mdf ファイルをダブルクリックすると (図 2 を参照してください)。

サーバー エクスプ ローラー ウィンドウは Visual Web Developer を使用する場合、データベース エクスプ ローラー ウィンドウと呼ばれます。


[![サーバー エクスプ ローラー ウィンドウの使用](creating-model-classes-with-linq-to-sql-cs/_static/image5.png)](creating-model-classes-with-linq-to-sql-cs/_static/image4.png)

**図 02**: サーバー エクスプ ローラー ウィンドウの使用 ([フルサイズのイメージを表示するをクリックして](creating-model-classes-with-linq-to-sql-cs/_static/image6.png))


ムービーを表す、データベースに 1 つのテーブルを追加する必要があります。 [テーブル] フォルダーを右クリックし、メニュー オプションを選択**新しいテーブルの追加**です。 このメニュー オプションを選択すると、テーブル デザイナーを開きます (図 3 を参照してください)。


[![サーバー エクスプ ローラー ウィンドウの使用](creating-model-classes-with-linq-to-sql-cs/_static/image8.png)](creating-model-classes-with-linq-to-sql-cs/_static/image7.png)

**図 03**: テーブル デザイナー ([フルサイズのイメージを表示するをクリックして](creating-model-classes-with-linq-to-sql-cs/_static/image9.png))


このデータベース テーブルに、次の列を追加する必要があります。

| **列名** | **データ型** | **Null を許容します。** |
| --- | --- | --- |
| ID | Int | False |
| Title | Nvarchar(200) | False |
| ディレクター | Nvarchar (50) | False |

Id 列に 2 つの特別な作業を行う必要があります。 最初に、テーブル デザイナーで列を選択し、キーのアイコンをクリックすると、主キー列として Id 列をマークする必要があります。 LINQ to SQL では、挿入またはデータベースに対する更新を実行するときに、主キー列を指定する必要があります。

次に、値を はい に割り当てることで Id 列として Id 列をマークする必要があります、 **Is Identity**プロパティ (図 3 を参照してください)。 Id 列は、テーブルに新しいデータの行を追加するたびに新しい番号を自動的に割り当てられている列です。

## <a name="create-linq-to-sql-classes"></a>LINQ to SQL クラスを作成します。

MVC モデルは、LINQ を tblMovie データベース テーブルを表す SQL クラスに含まれます。 これらの LINQ to SQL クラスを作成する最も簡単な方法は、モデル フォルダーを右クリックし、選択**追加、新しい項目の**、LINQ to SQL クラス テンプレートを選択して、名前、クラス、Movie.dbml、し をクリックして、**追加**(図 4 を参照してください) ボタンをクリックします。


[![LINQ to SQL クラスの作成](creating-model-classes-with-linq-to-sql-cs/_static/image11.png)](creating-model-classes-with-linq-to-sql-cs/_static/image10.png)

**図 04**: LINQ to SQL クラスを作成する ([フルサイズのイメージを表示するをクリックして](creating-model-classes-with-linq-to-sql-cs/_static/image12.png))


ムービーの LINQ to SQL クラスを作成した後にすぐに、オブジェクト リレーショナル デザイナーが表示されます。 LINQ to SQL クラスを表す特定のデータベース テーブルを作成するには、オブジェクト リレーショナル デザイナー上にサーバー エクスプ ローラー ウィンドウからデータベース テーブルをドラッグできます。 オブジェクト リレーショナル デザイナーに tblMovie データベース テーブルを追加する必要があります (図 5 を参照してください)。


[![オブジェクト リレーショナル デザイナーの使用](creating-model-classes-with-linq-to-sql-cs/_static/image14.png)](creating-model-classes-with-linq-to-sql-cs/_static/image13.png)

**図 05**: オブジェクト リレーショナル デザイナーを使用して ([フルサイズのイメージを表示するをクリックして](creating-model-classes-with-linq-to-sql-cs/_static/image15.png))


既定では、オブジェクト リレーショナル デザイナーは、デザイナーにドラッグするデータベース テーブルとまったく同じ名前のクラスを作成します。 ただし、このクラスを呼び出せばしたくありません`tblMovie`です。 そのため、デザイナーでクラスの名前をクリックし、ムービーにクラスの名前を変更します。

最後に、をクリックして、**保存**SQL クラスを LINQ を保存する (フロッピー ディスクの画像) ボタンをクリックします。 それ以外の場合、LINQ to SQL クラスは、オブジェクト リレーショナル デザイナーによって生成されません。

## <a name="using-linq-to-sql-in-a-controller-action"></a>コント ローラーのアクションでの LINQ to SQL の使用

これが、LINQ to SQL クラス、データベースからデータを取得するのにこれらのクラスを使用できます。 このセクションでは、コント ローラーのアクション内で直接 SQL クラスを LINQ を使用する方法を学習します。 MVC ビュー内 tblMovies データベース テーブルからムービーの一覧表示します。

最初に、テンプレートを使用するクラスを変更する必要があります。 このクラスは、アプリケーションのコント ローラーのフォルダーにあります。 次の 1 のリスト内のクラス名のように、クラスを変更します。

**1 – を一覧表示します。 `Controllers\HomeController.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample1.cs)]

`Index()`リスト 1 のアクションは、LINQ to SQL DataContext クラスを使用して (、 `MovieDataContext`) を表す、`MoviesDB`データベース。 `MoveDataContext`クラスは、Visual Studio オブジェクト リレーショナル デザイナーによって生成されました。

ビデオをすべて取得する DataContext に対して LINQ クエリが実行、`tblMovies`データベース テーブルです。 ムービーの一覧がという名前のローカル変数に割り当てられている`movies`です。 最後に、ムービーの一覧は、ビュー データをビューに渡されます。

ムービーを表示するのには次にインデックス ビューを変更する必要があります。 インデックスのビューを見つけることができます、`Views\Home\`フォルダーです。 2 を一覧表示するビューと同様に見えるように、インデックス ビューを更新します。

**2 – を一覧表示します。 `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample2.aspx)]

変更済みインデックス ビューに含まれることに注意してください、`<%@ import namespace %>`ビューの上部にあるディレクティブです。 このディレクティブをインポート、`MvcApplication1.Models namespace`です。 使用するためにこの名前空間が必要、`model`具体的には、クラス、 `Movie` --ビュー内のクラスです。

2 のリスト ビューに含まれる、`foreach`によって表される項目のすべてを反復処理するループ、`ViewData.Model`プロパティです。 値、`Title`ごとに表示されるプロパティ`movie`です。

注意しての値、`ViewData.Model`プロパティにキャスト、`IEnumerable`です。 これは、内容をループするために必要な`ViewData.Model`します。 ここで、別のオプションは、厳密に型指定を作成する`view`です。 厳密に型指定を作成するときに`view`、キャストする、`ViewData.Model`プロパティをビューの分離コード クラスの特定の型。

変更した後にアプリケーションを実行する場合、`HomeController`クラスと、インデックス ビューが空白のページが発生します。 ムービーのレコードが存在しないため、空白のページが表示されます、`tblMovies`データベース テーブルです。

レコードを追加するために、`tblMovies`データベース テーブルを右クリックして、 `tblMovies` (Visual Web Developer でデータベース エクスプ ローラー ウィンドウ) のサーバー エクスプ ローラー ウィンドウでテーブルをデータベースし、テーブル データの表示 メニュー オプションを選択します。 挿入できる`movie`(図 6 を参照してください) に表示されるグリッドを使用してレコード。


[![ムービーを挿入します。](creating-model-classes-with-linq-to-sql-cs/_static/image17.png)](creating-model-classes-with-linq-to-sql-cs/_static/image16.png)

**図 06**: 映画を挿入する ([フルサイズのイメージを表示するをクリックして](creating-model-classes-with-linq-to-sql-cs/_static/image18.png))


データベース レコードの一部を追加した後、`tblMovies`アプリケーションを実行して、テーブル、図 7 にページが表示されます。 箇条書きリストには、すべてのムービーのデータベース レコードが表示されます。


[![インデックス ビューでムービーを表示します。](creating-model-classes-with-linq-to-sql-cs/_static/image20.png)](creating-model-classes-with-linq-to-sql-cs/_static/image19.png)

**図 07**: インデックスが表示されたムービーを表示する ([フルサイズのイメージを表示するをクリックして](creating-model-classes-with-linq-to-sql-cs/_static/image21.png))


## <a name="using-the-repository-pattern"></a>リポジトリ パターンを使用します。

前のセクションでは、LINQ を使用して、コント ローラーのアクション内で直接 SQL クラスにします。 使用して、`MovieDataContext`クラスから直接、`Index()`コント ローラーのアクション。 問題はありません、簡単なアプリケーションの場合、これを行うとします。 ただしより複雑なアプリケーションを構築する必要がある場合、コント ローラー クラスでの LINQ to SQL で直接作業は、問題を作成します。

コント ローラー クラス内で LINQ to SQL を使用すると、データ アクセス テクノロジを後で切り替えるには困難になります。 たとえば、Microsoft LINQ to SQL を使用して、データ アクセス テクノロジとして Microsoft Entity Framework の使用に切り替えることができます。 その場合は、アプリケーション内でデータベースにアクセスするすべてのコント ローラーを書き直す必要があります。

コント ローラー クラス内で LINQ to SQL を使用しても困難になって、アプリケーションの単体テストをビルドします。 通常、単体テストを実行するときに、データベースとやり取りすることはおたくないです。 単体テストを使用して、アプリケーション ロジックと、データベース サーバーではなくをテストするには。

MVC アプリケーションを構築するために将来に適応性のある変更とをより簡単にテストすることができます、リポジトリ パターンの使用を検討する必要があります。 リポジトリ パターンを使用する場合は、すべてのデータベース アクセス ロジックを含む別のリポジトリ クラスを作成します。

リポジトリ クラスを作成するときは、すべてのリポジトリ クラスによって使用されるメソッドを表すインターフェイスを作成します。 コント ローラー内では、リポジトリではなく、インターフェイスに対して、コードを記述します。 このように、後で別のデータ アクセス テクノロジを使用してリポジトリを実装できます。

リスト 3 のインターフェイスの名前は`IMovieRepository`という単一のメソッドを表すと`ListAll()`です。

**3 – を一覧表示します。 `Models\IMovieRepository.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample3.cs)]

リスト 4 リポジトリ クラスを実装して、`IMovieRepository`インターフェイスです。 という名前のメソッドが含まれている通知`ListAll()`に必要なメソッドに対応する、`IMovieRepository`インターフェイスです。

**4 – を一覧表示します。 `Models\MovieRepository.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample4.cs)]

最後に、`MoviesController`リスト 5 内のクラスは、リポジトリ パターンを使用します。 不要になった LINQ to SQL クラスを直接使用します。

**5 – を一覧表示します。 `Controllers\MoviesController.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample5.cs)]

注意して、`MoviesController`クラス 5 の一覧表示するのには 2 つのコンス トラクターです。 最初のコンス トラクター パラメーターなしのコンス トラクターは、アプリケーションが実行されているときに呼び出されます。 このコンス トラクターのインスタンスを作成する、`MovieRepository`クラスし、2 番目のコンス トラクターに渡されます。

2 番目のコンス トラクターが 1 つのパラメーター:`IMovieRepository`パラメーター。 このコンス トラクターは、という名前のクラス レベル フィールドにパラメーターの値を代入するだけで`_repository`です。

`MoviesController`依存性の注入パターンと呼ばれるソフトウェアの設計パターンのクラスが活用されます。 具体的には、使用されているコンス トラクターの依存関係挿入と呼ばれるものです。 詳細を読み取ることができます Martin ファウラーによって、次の記事を参照してこのパターンは。

[http://martinfowler.com/articles/injection.html](http://martinfowler.com/articles/injection.html)

注意してすべてのコードの`MoviesController`(最初のコンス トラクター) を除くクラスの対話、`IMovieRepository`インターフェイスではなく、実際`MovieRepository`クラスです。 コードは、インターフェイスの具象実装ではなく抽象インターフェイスと対話します。

アプリケーションで使用されるデータ アクセス テクノロジを変更するかどうかは、単に実装することができます、`IMovieRepository`を別のデータベース アクセス テクノロジを使用してクラスを持つインターフェイスです。 たとえば、作成した、`EntityFrameworkMovieRepository`クラスまたは`SubSonicMovieRepository`クラスです。 新しい実装を渡すことができます、コント ローラー クラスは、インターフェイスに対してプログラミングされて、ため`IMovieRepository`コント ローラーに、クラスとクラスは引き続き機能します。

さらに、テストする場合、`MoviesController`クラス、偽のムービー リポジトリ クラスを渡すことができます、`HomeController`です。 実装することができます、`IMovieRepository`しないアクセス実際には、データベースがすべての必要なメソッドを含むクラスを持つクラス、`IMovieRepository`インターフェイスです。 このように、単体テストができます、`MoviesController`実際には、実際のデータベースにアクセスしなくてもクラスです。

## <a name="summary"></a>まとめ

このチュートリアルの目的を利用してマイクロソフトの LINQ to SQL MVC モデル クラスを作成する方法を示すためでした。 ここには、ASP.NET MVC アプリケーションでデータベースのデータを表示するための 2 つの戦略が調べられます。 最初に、LINQ to SQL クラスが作成し、コント ローラーのアクション内で直接クラスを使用します。 コント ローラー内の SQL クラスを LINQ を使用して、簡単にすることができ、簡単に、MVC アプリケーションでデータベースのデータを表示します。

次に、データベースのデータを表示するため、多少難しいことが確実より好パスを説明します。 リポジトリ パターンを利用し、データベース アクセス ロジックのすべてを別のリポジトリ クラスに配置します。 コント ローラーにご説明したとおりのすべての具象クラスではなくインターフェイスに対して、コード。 リポジトリ パターンの利点は、データベース アクセス テクノロジを後で簡単に変更することにより、簡単にテスト コント ローラー クラスにすることをによりします。

> [!div class="step-by-step"]
> [前へ](creating-model-classes-with-the-entity-framework-cs.md)
> [次へ](displaying-a-table-of-database-data-cs.md)
