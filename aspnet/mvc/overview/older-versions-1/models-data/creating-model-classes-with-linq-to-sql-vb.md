---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-vb
title: LINQ to SQL (VB) でモデル クラスを作成する |Microsoft ドキュメント
author: microsoft
description: このチュートリアルの目的では、ASP.NET MVC アプリケーション用のモデル クラスを作成する方法の 1 つをについて説明します。 このチュートリアルでは、c のモデルを構築する方法を学習しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: a4a25a75-d71f-4509-98b4-df72e748985a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-vb
msc.type: authoredcontent
ms.openlocfilehash: 5438838123c40d82afbda191a48878d6dca80736
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874812"
---
<a name="creating-model-classes-with-linq-to-sql-vb"></a>LINQ to SQL (VB) でモデル クラスを作成します。
====================
によって[Microsoft](https://github.com/microsoft)

[PDF をダウンロードします。](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_10_VB.pdf)

> このチュートリアルの目的では、ASP.NET MVC アプリケーション用のモデル クラスを作成する方法の 1 つをについて説明します。 このチュートリアルでは、モデル クラスを構築し、利用してマイクロソフトの LINQ to SQL データベースへのアクセスを実行する方法を学習します。


このチュートリアルの目的では、ASP.NET MVC アプリケーション用のモデル クラスを作成する方法の 1 つをについて説明します。 このチュートリアルでは、モデル クラスを構築し、利用してマイクロソフトの LINQ to SQL データベースへのアクセスを実行する方法を学習します。

このチュートリアルでは、ムービーの基本的なデータベース アプリケーションを構築します。 可能な最も高速で簡単な方法で、ムービーのデータベース アプリケーションを作成して開始します。 すべてのデータ アクセスをコント ローラーのアクションから直接実行します。

次に、リポジトリ パターンを使用する方法を説明します。 リポジトリ パターンを使用するには、少しの作業が必要です。 ただし、このパターンを採用するに対応するアプリケーションを構築できるを変更して、簡単にテストすることができます。

## <a name="what-is-a-model-class"></a>モデル クラスとは何ですか。

MVC モデルには、すべての MVC ビューまたは MVC コント ローラーに含まれないアプリケーション ロジックが含まれます。 具体的には、MVC モデルには、すべてのアプリケーションの業務とデータ アクセス ロジックが含まれています。

さまざまな異なるテクノロジを使用すると、データ アクセス ロジックを実装します。 たとえば、Microsoft の Entity Framework や NHibernate、Subsonic、ADO.NET のクラスを使用して、データ アクセス クラスをビルドすることができます。

このチュートリアルでは、クエリを実行し、データベースを更新する LINQ to SQL を使用します。 LINQ to SQL は、Microsoft SQL Server データベースとの対話の非常に簡単な方法を提供します。 ただし、こと、ASP.NET MVC フレームワークに関連付けられていない LINQ to SQL で任意の方法を理解しておく必要があります。 ASP.NET MVC は、任意のデータ アクセス テクノロジとの互換性。

## <a name="create-a-movie-database"></a>ムービーのデータベースを作成します。

このチュートリアル--モデル クラスを構築する方法を説明するために単純なムービー データベース アプリケーションお構築します。 最初の手順では、新しいデータベースを作成します。 アプリケーションを右クリックして\_メニュー オプションを選択し、ソリューション エクスプ ローラーのウィンドウで、データ フォルダー**追加]、[新しい項目の**します。 SQL Server データベースのテンプレートを選択して、MoviesDB.mdf、名前を付けますおよびをクリックして、**追加**(図 1 を参照してください) ボタンをクリックします。


[![新しい SQL Server データベースを追加します。](creating-model-classes-with-linq-to-sql-vb/_static/image2.png)](creating-model-classes-with-linq-to-sql-vb/_static/image1.png)

**図 01**: 新しい SQL Server データベースを追加する ([フルサイズのイメージを表示するをクリックして](creating-model-classes-with-linq-to-sql-vb/_static/image3.png))


新しいデータベースを作成した後は、アプリで MoviesDB.mdf ファイルをダブルクリックして、データベースを開くことができます\_データ フォルダーです。 サーバー エクスプ ローラー ウィンドウを開き、MoviesDB.mdf ファイルをダブルクリックすると (図 2 を参照してください)。


|   | サーバー エクスプ ローラー ウィンドウは Visual Web Developer を使用する場合、データベース エクスプ ローラー ウィンドウと呼ばれます。 |
|---|----------------------------------------------------------------------------------------------------|
|   |                                                                                                    |

[![サーバー エクスプ ローラー ウィンドウの使用](creating-model-classes-with-linq-to-sql-vb/_static/image5.png)](creating-model-classes-with-linq-to-sql-vb/_static/image4.png)

**図 02**: サーバー エクスプ ローラー ウィンドウの使用 ([フルサイズのイメージを表示するをクリックして](creating-model-classes-with-linq-to-sql-vb/_static/image6.png))


ムービーを表す、データベースに 1 つのテーブルを追加する必要があります。 [テーブル] フォルダーを右クリックし、メニュー オプションを選択**新しいテーブルの追加**です。 このメニュー オプションを選択すると、テーブル デザイナーを開きます (図 3 を参照してください)。


[![サーバー エクスプ ローラー ウィンドウの使用](creating-model-classes-with-linq-to-sql-vb/_static/image8.png)](creating-model-classes-with-linq-to-sql-vb/_static/image7.png)

**図 03**: テーブル デザイナー ([フルサイズのイメージを表示するをクリックして](creating-model-classes-with-linq-to-sql-vb/_static/image9.png))


このデータベース テーブルに、次の列を追加する必要があります。

| **列名** | **データ型** | **Null を許容します。** |
| --- | --- | --- |
| ID | Int | False |
| Title | Nvarchar(200) | False |
| ディレクター | Nvarchar(50) | False |

Id 列に 2 つの特別な作業を行う必要があります。 最初に、テーブル デザイナーで列を選択し、キーのアイコンをクリックすると、主キー列として Id 列をマークする必要があります。 LINQ to SQL では、挿入またはデータベースに対する更新を実行するときに、主キー列を指定する必要があります。

次に、値を はい に割り当てることで Id 列として Id 列をマークする必要があります、 **Is Identity**プロパティ (図 3 を参照してください)。 Id 列は、テーブルに新しいデータの行を追加するたびに新しい番号を自動的に割り当てられている列です。

これらの変更を加えた後は、名前 tblMovie でテーブルを保存します。 テーブルを保存するには、[保存] ボタンをクリックします。

## <a name="create-linq-to-sql-classes"></a>LINQ to SQL クラスを作成します。

MVC モデルは、LINQ を tblMovie データベース テーブルを表す SQL クラスに含まれます。 これらの LINQ to SQL クラスを作成する最も簡単な方法は、モデル フォルダーを右クリックし、選択**追加、新しい項目の**、LINQ to SQL クラス テンプレートを選択して、名前、クラス、Movie.dbml、し をクリックして、**追加**(図 4 を参照してください) ボタンをクリックします。


[![LINQ to SQL クラスの作成](creating-model-classes-with-linq-to-sql-vb/_static/image11.png)](creating-model-classes-with-linq-to-sql-vb/_static/image10.png)

**図 04**: LINQ to SQL クラスを作成する ([フルサイズのイメージを表示するをクリックして](creating-model-classes-with-linq-to-sql-vb/_static/image12.png))


ムービーの LINQ to SQL クラスを作成した後にすぐに、オブジェクト リレーショナル デザイナーが表示されます。 LINQ to SQL クラスを表す特定のデータベース テーブルを作成するには、オブジェクト リレーショナル デザイナー上にサーバー エクスプ ローラー ウィンドウからデータベース テーブルをドラッグできます。 オブジェクト リレーショナル デザイナーに tblMovie データベース テーブルを追加する必要があります (図 4 を参照してください)。


[![オブジェクト リレーショナル デザイナーの使用](creating-model-classes-with-linq-to-sql-vb/_static/image14.png)](creating-model-classes-with-linq-to-sql-vb/_static/image13.png)

**図 05**: オブジェクト リレーショナル デザイナーを使用して ([フルサイズのイメージを表示するをクリックして](creating-model-classes-with-linq-to-sql-vb/_static/image15.png))


既定では、オブジェクト リレーショナル デザイナーは、デザイナーにドラッグするデータベース テーブルとまったく同じ名前のクラスを作成します。 ただし、クラス、tblMovie を呼び出すしたくありません。 そのため、デザイナーでクラスの名前をクリックし、ムービーにクラスの名前を変更します。

最後に、をクリックして、**保存**SQL クラスを LINQ を保存する (フロッピー ディスクの画像) ボタンをクリックします。 それ以外の場合、LINQ to SQL クラスは、オブジェクト リレーショナル デザイナーによって生成されません。

## <a name="using-linq-to-sql-in-a-controller-action"></a>コント ローラーのアクションでの LINQ to SQL の使用

これが、LINQ to SQL クラス、データベースからデータを取得するのにこれらのクラスを使用できます。 このセクションでは、コント ローラーのアクション内で直接 SQL クラスを LINQ を使用する方法を学習します。 MVC ビュー内 tblMovies データベース テーブルからムービーの一覧表示します。

最初に、テンプレートを使用するクラスを変更する必要があります。 このクラスは、アプリケーションのコント ローラーのフォルダーにあります。 次の 1 のリスト内のクラス名のように、クラスを変更します。

**1 – を一覧表示します。 `Controllers\HomeController.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample1.vb)]

リスト 1 の Index() アクションでは、LINQ to SQL DataContext クラス (MovieDataContext) を使用して、MoviesDB データベースを表します。 MoveDataContext クラスは、Visual Studio オブジェクト リレーショナル デザイナーによって生成されました。

LINQ クエリはすべて、ムービーの tblMovies データベース テーブルから取得する DataContext に対して実行されます。 ムービーの一覧は、ムービーをという名前のローカル変数に割り当てられます。 最後に、ムービーの一覧は、ビュー データをビューに渡されます。

ムービーを表示するのには次にインデックス ビューを変更する必要があります。 Views\Home\ フォルダーで、インデックス ビューを参照できます。 2 を一覧表示するビューと同様に見えるように、インデックス ビューを更新します。

**2 – を一覧表示します。 `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample2.aspx)]

変更済みインデックス ビューに含まれることに注意してください、 &lt;% インポート名前空間 % @&gt;ビューの上部にあるディレクティブです。 このディレクティブは、MvcApplication1 名前空間をインポートします。 この名前空間のモデル クラス – 具体的には、ビューで、ムービー クラスを使用するために必要です。

2 のリスト ビューには、For が含まれています。 すべての ViewData.Model プロパティで表される項目を反復処理する各ループします。 各ムービーのタイトルのプロパティの値が表示されます。

ViewData.Model プロパティの値が IEnumerable にキャストされていることに注意してください。 これは、ViewData.Model の内容をループするために必要です。 別のオプションは、ここでは、厳密に型指定されたビューを作成します。 厳密に型指定されたビューを作成するときは、ビューの分離コード クラスで ViewData.Model プロパティを特定の型をキャストする必要があります。

HomeController クラスと、インデックス ビューを変更した後にアプリケーションを実行すると、空白のページが表示されます。 TblMovies データベース テーブルのムービーのレコードが存在しないため、空白のページが表示されます。

TblMovies データベース テーブルにレコードを追加するために、サーバー エクスプ ローラー ウィンドウ (Visual Web Developer でデータベース エクスプ ローラー ウィンドウ) tblMovies データベース テーブルを右クリックし、メニュー オプションを選択**テーブル データの表示**です。 (図 5 を参照してください) に表示されるグリッドを使用して、ムービー レコードを挿入することができます。


[![ムービーを挿入します。](creating-model-classes-with-linq-to-sql-vb/_static/image17.png)](creating-model-classes-with-linq-to-sql-vb/_static/image16.png)

**図 06**: 映画を挿入する ([フルサイズのイメージを表示するをクリックして](creating-model-classes-with-linq-to-sql-vb/_static/image18.png))


データベース レコードの一部を tblMovies テーブルに追加すると、アプリケーションを実行する、図 7 にページが表示されます。 箇条書きリストには、すべてのムービーのデータベース レコードが表示されます。


[![インデックス ビューでムービーを表示します。](creating-model-classes-with-linq-to-sql-vb/_static/image20.png)](creating-model-classes-with-linq-to-sql-vb/_static/image19.png)

**図 07**: インデックスが表示されたムービーを表示する ([フルサイズのイメージを表示するをクリックして](creating-model-classes-with-linq-to-sql-vb/_static/image21.png))


## <a name="using-the-repository-pattern"></a>リポジトリ パターンを使用します。

前のセクションでは、LINQ を使用して、コント ローラーのアクション内で直接 SQL クラスにします。 Index() コント ローラーのアクションから直接 MovieDataContext クラスを使用します。 問題はありません、簡単なアプリケーションの場合、これを行うとします。 ただしより複雑なアプリケーションを構築する必要がある場合、コント ローラー クラスでの LINQ to SQL で直接作業は、問題を作成します。

コント ローラー クラス内で LINQ to SQL を使用すると、データ アクセス テクノロジを後で切り替えるには困難になります。 たとえば、Microsoft LINQ to SQL を使用して、データ アクセス テクノロジとして Microsoft Entity Framework の使用に切り替えることができます。 その場合は、アプリケーション内でデータベースにアクセスするすべてのコント ローラーを書き直す必要があります。

コント ローラー クラス内で LINQ to SQL を使用しても困難になって、アプリケーションの単体テストをビルドします。 通常、単体テストを実行するときに、データベースとやり取りすることはおたくないです。 単体テストを使用して、アプリケーション ロジックと、データベース サーバーではなくをテストするには。

MVC アプリケーションを構築するために将来に適応性のある変更とをより簡単にテストすることができます、リポジトリ パターンの使用を検討する必要があります。 リポジトリ パターンを使用する場合は、すべてのデータベース アクセス ロジックを含む別のリポジトリ クラスを作成します。

リポジトリ クラスを作成するときは、すべてのリポジトリ クラスによって使用されるメソッドを表すインターフェイスを作成します。 コント ローラー内では、リポジトリではなく、インターフェイスに対して、コードを記述します。 このように、後で別のデータ アクセス テクノロジを使用してリポジトリを実装できます。

リスト 3 のインターフェイスは IMovieRepository という ListAll() という単一のメソッドを表します。

**3 – を一覧表示します。 `Models\IMovieRepository.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample3.vb)]

リスト 4 リポジトリ クラスは、IMovieRepository インターフェイスを実装します。 IMovieRepository インターフェイスで必要なメソッドに対応する ListAll() をという名前のメソッドが含まれていることを確認します。

**4 – を一覧表示します。 `Models\MovieRepository.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample4.vb)]

最後に、コード サンプル 5 MoviesController クラスは、リポジトリ パターンを使用します。 不要になった LINQ to SQL クラスを直接使用します。

**5 – を一覧表示します。 `Controllers\MoviesController.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample5.vb)]

リスト 5 で MoviesController クラスが 2 つのコンス トラクターに注意してください。 最初のコンス トラクター パラメーターなしのコンス トラクターは、アプリケーションが実行されているときに呼び出されます。 このコンス トラクターは、MovieRepository クラスのインスタンスを作成し、2 番目のコンス トラクターに渡されます。

2 番目のコンス トラクターが 1 つのパラメーター: IMovieRepository パラメーター。 このコンス トラクターは、という名前のクラス レベル フィールドにパラメーターの値を代入するだけで\_リポジトリです。

MoviesController クラスには、依存性の注入パターンと呼ばれるソフトウェアの設計パターンの利点がかかっています。 具体的には、使用されているコンス トラクターの依存関係挿入と呼ばれるものです。 詳細を読み取ることができます Martin ファウラーによって、次の記事を参照してこのパターンは。

[http://martinfowler.com/articles/injection.html](http://martinfowler.com/articles/injection.html)

実際の MovieRepository クラスではなく IMovieRepository インターフェイスと対話すべて (最初のコンス トラクター) を除く MoviesController クラスのコードのことに注意してください。 コードは、インターフェイスの具象実装ではなく抽象インターフェイスと対話します。

アプリケーションで使用されるデータ アクセス テクノロジを変更する場合を別のデータベース アクセス テクノロジを使用してクラスを持つ IMovieRepository インターフェイスだけで実装できます。 たとえば、EntityFrameworkMovieRepository クラスまたは SubSonicMovieRepository クラスを作成できます。 コント ローラー クラスは、インターフェイスに対してプログラミングされて、ため IMovieRepository の新しい実装をコント ローラー クラスに渡すことができますし、クラスは引き続き機能します。

さらに、MoviesController クラスをテストする場合、MoviesController を偽のムービー リポジトリ クラスを渡すことができます。 データベースを実際にはアクセスしませんが、IMovieRepository インターフェイスの必要なメソッドのすべてを含むクラスを含む IMovieRepository クラスを実装することができます。 このように、MoviesController クラスの単体テストを実際には、実際のデータベースにアクセスしなくてもできます。

## <a name="summary"></a>まとめ

このチュートリアルの目的を利用してマイクロソフトの LINQ to SQL MVC モデル クラスを作成する方法を示すためでした。 ここには、ASP.NET MVC アプリケーションでデータベースのデータを表示するための 2 つの戦略が調べられます。 最初に、LINQ to SQL クラスが作成し、コント ローラーのアクション内で直接クラスを使用します。 コント ローラー内の SQL クラスを LINQ を使用して、簡単にすることができ、簡単に、MVC アプリケーションでデータベースのデータを表示します。

次に、データベースのデータを表示するため、多少難しいことが確実より好パスを説明します。 リポジトリ パターンを利用し、データベース アクセス ロジックのすべてを別のリポジトリ クラスに配置します。 コント ローラーにご説明したとおりのすべての具象クラスではなくインターフェイスに対して、コード。 リポジトリ パターンの利点は、データベース アクセス テクノロジを後で簡単に変更することにより、簡単にテスト コント ローラー クラスにすることをによりします。

> [!div class="step-by-step"]
> [前へ](creating-model-classes-with-the-entity-framework-vb.md)
> [次へ](displaying-a-table-of-database-data-vb.md)
