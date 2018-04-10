---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-cs
title: Entity Framework (c#) でモデル クラスを作成する |Microsoft ドキュメント
author: microsoft
description: このチュートリアルでは、Microsoft の Entity Framework での ASP.NET MVC を使用する方法を学習します。 ウィザードを使用して、エンティティを ADO.NET エンティティ Da を作成する方法を説明するとしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 61644169-e8b1-45dd-bf96-9c2301b69879
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-cs
msc.type: authoredcontent
ms.openlocfilehash: b0a79da580f14d5ae6bcfaaa00d3900234dc662e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="creating-model-classes-with-the-entity-framework-c"></a>Entity Framework (c#) でモデル クラスを作成します。
====================
によって[Microsoft](https://github.com/microsoft)

> このチュートリアルでは、Microsoft の Entity Framework での ASP.NET MVC を使用する方法を学習します。 ウィザードを使用して、エンティティを ADO.NET エンティティ データ モデルを作成する方法を学びます。 このチュートリアルの過程で、ここを選択、挿入、更新、および Entity Framework を使用してデータベースのデータを削除する方法について説明する web アプリケーションを構築します。


このチュートリアルの目的では、ASP.NET MVC アプリケーションを構築するときに、Microsoft の Entity Framework を使用してデータ アクセス クラスを作成する方法について説明します。 このチュートリアルは、Microsoft の Entity Framework の以前の知識を負いません。 このチュートリアルの目的は、Entity Framework を使用して、選択、挿入、更新、およびデータベースのレコードを削除する方法を理解します。

Microsoft の Entity Framework は、オブジェクト リレーショナル マッピング (O/RM) ツールをデータベースからデータ アクセス層を自動的に生成することができます。 Entity Framework では、データ アクセス クラスを手動で構築の面倒な作業を回避することができます。

ASP.NET MVC で Microsoft Entity Framework を使用する方法がわかるようにするためにここには、簡単なサンプル アプリケーションを構築します。 ムービーのデータベース アプリケーションを表示し、ムービーのデータベース レコードを編集することができますを作成します。

このチュートリアルでは、Visual Studio 2008 または Visual Web Developer 2008 Service Pack 1 があることを前提としています。 Entity Framework を使用するために Service Pack 1 を必要とします。 Visual Studio 2008 Service Pack 1 または Service Pack 1 の Visual Web Developer は、次のアドレスからダウンロードできます。

> [https://www.asp.net/downloads/](https://www.asp.net/downloads)


> [!NOTE] 
> 
> ASP.NET MVC と Microsoft Entity Framework の不可欠な接続がありません。 ASP.NET MVC で使用できるエンティティ フレームワークのいくつかの選択肢があります。 たとえば、SQL、NHibernate または SubSonic Microsoft LINQ などその他の O/RM ツールを使用して MVC モデル クラスをビルドすることができます。


## <a name="creating-the-movie-sample-database"></a>ムービーのサンプル データベースの作成

ムービーのデータベース アプリケーションでは、ムービーをという名前を次の列を含むデータベース テーブルを使用します。

| 列名 | データの種類 | Null 値を許可しますか。 | プライマリ キーとは |
| --- | --- | --- | --- |
| ID | int | False | True |
| Title | nvarchar(100) | False | False |
| ディレクター | nvarchar(100) | False | False |

このテーブルは、次の手順で ASP.NET MVC プロジェクトに追加できます。

1. アプリケーションを右クリックして\_メニュー オプションを選択し、ソリューション エクスプ ローラーのウィンドウで、データ フォルダー**追加]、[新しい項目。**
2. **新しい項目の追加**ダイアログ ボックスで、 **SQL Server データベース**名前 MoviesDB.mdf、データベースを指定し、クリックして、**追加**ボタンをクリックします。
3. サーバー エクスプ ローラー/データベース エクスプ ローラー ウィンドウを開いて MoviesDB.mdf ファイルをダブルクリックします。
4. MoviesDB.mdf データベース接続を展開し、[テーブル] フォルダーを右クリックしてメニュー オプションを選択**新しいテーブルの追加**です。
5. テーブル デザイナーでは、Id、タイトル、およびディレクター列を追加します。
6. をクリックして、**保存**ボタン (フロッピー ディスクのアイコンがある) 名前ムービーと共に、新しいテーブルを保存します。

映画のデータベース テーブルを作成した後は、テーブルにサンプル データを追加する必要があります。 映画テーブルを右クリックし、メニュー オプションを選択**テーブル データの表示**です。 表示されたグリッドには、偽のムービー データを入力できます。

## <a name="creating-the-adonet-entity-data-model"></a>ADO.NET エンティティ データ モデルを作成します。

Entity Framework を使用するのには、Entity Data Model を作成する必要があります。 Visual Studio の活用*Entity Data Model ウィザード*エンティティ データ モデルをデータベースから自動的に生成します。

この場合は、以下の手順に従ってください。

1. ソリューション エクスプ ローラー ウィンドウで モデル フォルダーを右クリックし、メニュー オプションを選択**追加、新しい項目の**します。
2. **新しい項目の追加**ダイアログ ボックスで、データ カテゴリを選択 (図 1 を参照してください)。
3. 選択、 **ADO.NET エンティティ データ モデル**テンプレート、名前、Entity Data Model、MoviesDBModel.edmx、しをクリックして、**追加**ボタンをクリックします。 クリックすると、**追加**ボタンは、データ モデル ウィザードを起動します。
4. **[モデルのコンテンツ**ステップで、選択、**データベースから生成**オプションをクリックして、 **[次へ]** (図 2 を参照してください)] ボタンをクリックします。
5. **データ接続の選択**ステップ、MoviesDB.mdf データベース接続を選択し、接続の設定が MoviesDBEntities の名前を指定しをクリックしてエンティティを入力、**次**(図 3 を参照してください) ボタンをクリックします。
6. **データベース オブジェクトの選択**ステップ、ムービーのデータベース テーブルを選択してをクリックして、**完了**(図 4 を参照してください) ボタンをクリックします。

次の手順を完了すると、ADO.NET Entity Data Model デザイナー (エンティティ デザイナー) が開きます。

**図 1 – 新しいエンティティ データ モデルを作成します。**

![clip_image002](creating-model-classes-with-the-entity-framework-cs/_static/image1.jpg)

**図 2 – モデル内容のステップを選択**

![clip_image004](creating-model-classes-with-the-entity-framework-cs/_static/image2.jpg)

**図 3 – データ接続の選択**

![clip_image006](creating-model-classes-with-the-entity-framework-cs/_static/image3.jpg)

**図 4-データベース オブジェクトの選択**

![clip_image008](creating-model-classes-with-the-entity-framework-cs/_static/image4.jpg)

#### <a name="modifying-the-adonet-entity-data-model"></a>ADO.NET Entity Data Model を変更します。

エンティティ デザイナーを利用して、モデルを変更するには Entity Data Model を作成した後 (図 5 を参照してください)。 ソリューション エクスプ ローラー ウィンドウ内で、Models フォルダーに含まれている MoviesDBModel.edmx ファイルをダブルクリックして、いつでも、エンティティ デザイナーを開くことができます。

**図 5 – に ADO.NET Entity Data Model デザイナー**

![clip_image010](creating-model-classes-with-the-entity-framework-cs/_static/image5.jpg)

たとえば、エンティティ デザイナーを使用して、エンティティ モデルのデータ ウィザードで生成されるクラスの名前を変更することができます。 ウィザードでは、ムービーをという名前の新しいデータ アクセス クラスを作成します。 つまり、ウィザードは、非常に同じ名前のデータベース テーブル クラスできました。 おをこのクラスを使用して、ムービーの特定のインスタンスを表す、ためムービーからクラスをムービーにお名前を変更する必要があります。

エンティティ クラスの名前を変更するには、エンティティ デザイナーでクラス名をダブルクリックし、新しい名前を入力することができます (図 6 を参照してください)。 また、エンティティ デザイナーでエンティティを選択した後、[プロパティ] ウィンドウ内のエンティティの名前を変更できます。

**図 6 – エンティティ名を変更します。**

![clip_image012](creating-model-classes-with-the-entity-framework-cs/_static/image6.jpg)

[保存] ボタン (フロッピー ディスクのアイコン) をクリックして、変更を行った後、エンティティ データ モデルを保存してください。 バック グラウンドでは、エンティティ デザイナーは、一連の c# クラスを生成します。 これらのクラスを表示するには、ソリューション エクスプ ローラー ウィンドウから MoviesDBModel.Designer.cs ファイルを開きます。


次に、エンティティ デザイナーを使用するときに、変更が上書きされますので、Designer.cs ファイル内のコードを変更しないでください。 Designer.cs ファイルで定義されたエンティティ クラスの機能を拡張するかどうかは、作成することができます*部分クラス*で個別のファイルです。


#### <a name="selecting-database-records-with-the-entity-framework"></a>Entity Framework でのデータベース レコードを選択します。

ムービーのレコードの一覧を表示するページを作成することで、ムービーのデータベース アプリケーションの構築を開始しましょう。 1 の一覧で、Home コント ローラーは、Index() をという名前のアクションを公開します。 Index() アクションに、ムービーのデータベース テーブルからのムービー レコードはすべて、Entity Framework を活用してに返します。

**1 – controllers \homecontroller.cs を一覧表示します。**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample1.cs)]

リスト 1 のコント ローラーに、コンス トラクターが含まれることに注意してください。 コンス トラクターは、という名前のクラス レベル フィールド\_db します。 \_Db フィールドは、Microsoft の Entity Framework によって生成されるデータベース エンティティを表します。 \_Db フィールドがエンティティ デザイナーで生成された MoviesDBEntities クラスのインスタンス。


TheMoviesDBEntities クラスを Home コント ローラーを使用するのには、MovieEntityApp.Models 名前空間をインポートする必要があります (*MVCProjectName*です。モデル)。


\_Db フィールドは、ムービーのデータベース テーブルからレコードを取得する Index() 操作内で使用します。 式\_db します。MovieSet は、ムービーのデータベース テーブルのすべてのレコードを表します。 ToList() メソッドを使用して、ムービーのセットに映画オブジェクトのジェネリック コレクション変換 (リスト&lt;ムービー&gt;)。

ムービーのレコードは、LINQ to Entities を利用して取得されます。 LINQ を使用するリスト 1 の Index() アクション*メソッド構文*データベース レコードのセットを取得します。 LINQ を使用することができる場合は、*クエリの構文*代わりにします。 次の 2 つのステートメントでは、同じ操作を行います。

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample2.cs)]

どの LINQ 構文を使用 – メソッド構文またはクエリの構文: 最も直観的な表示されています。 2 つの方法のパフォーマンスに違いはありません: 唯一の違いはスタイル。

2 を一覧表示するビューを使用すると、ムービーのレコードを表示します。

**Listing 2 – Views\Home\Index.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample3.aspx)]

2 のリスト ビューに含まれる、 **foreach**ループをムービーの各レコードを反復処理し、ムービー レコードのタイトルおよびディレクター プロパティの値を表示します。 各レコードの横にある編集および削除のリンクが表示されることに注意してください。 ビューの下部にさらに、ムービーの追加リンクが表示されます (図 7 を参照してください)。

**図 7 –、インデックス ビュー**

![clip_image014](creating-model-classes-with-the-entity-framework-cs/_static/image7.jpg)

インデックス ビューは、*ビューに型指定された*です。 インデックス ビューに含まれる、 &lt;% ページ % @&gt;でディレクティブ、 *Inherits*ムービー オブジェクトの厳密に型のジェネリック リスト コレクションにモデルのプロパティをキャストする属性 (リスト&lt;ムービー)。

## <a name="inserting-database-records-with-the-entity-framework"></a>Entity Framework でのデータベース レコードを挿入します。

Entity Framework を使用すると、データベース テーブルに新しいレコードを挿入しやすきます。 3 を一覧表示するには、ムービーのデータベース テーブルに新しいレコードを挿入する際、ホーム コント ローラー クラスに追加された 2 つの新しいアクションが含まれます。

**3 – controllers \homecontroller.cs (メソッドの追加) を一覧表示します。**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample4.cs)]

Add() の最初のアクションは、単にビューを返します。 ビューには、新しいムービーのデータベースを追加するためのフォームが含まれています (図 8 を参照してください) を記録します。 フォームを送信するときに、2 番目の Add() アクションが呼び出されます。

2 番目の Add() アクションが AcceptVerbs 属性で修飾されたことに注意してください。 HTTP POST 操作を実行する場合にのみ、この操作を呼び出すことができます。 つまり、この操作は、HTML フォームを送信する場合にのみ呼び出すことができます。

2 番目の Add() アクションは、ASP.NET MVC TryUpdateModel() メソッドのヘルプを Entity Framework ムービー クラスの新しいインスタンスを作成します。 TryUpdateModel() メソッドは Add() メソッドに渡される FormCollection でフィールドを受け取りし、ムービー クラスに HTML フォーム フィールドの値を割り当てます。


Entity Framework を使用する場合は、TryUpdateModel または UpdateModel メソッドを使用してエンティティ クラスのプロパティを更新する場合、"空白"プロパティの一覧を指定してください。


次に、Add() アクションでは、いくつかの簡単なフォーム検証を実行します。 アクションは、タイトルとディレクターの両方のプロパティに値があることを確認します。 検証エラーがある場合、検証エラー メッセージは ModelState に追加されます。

検証エラーがない場合、新しいムービーのレコードは、Entity Framework を利用して、ムービー データベース テーブルに追加されます。 新しいレコードは、次の 2 つの行のコードで、データベースに追加されます。

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample5.cs)]

コードの最初の行は、Entity Framework によって追跡されている映画のセットに映画の新しいエンティティを追加します。 コードの 2 行目では、基になるデータベースに追跡されている映画にどのような変更が加えられたを保存します。

**図 8-追加ビュー**

![clip_image016](creating-model-classes-with-the-entity-framework-cs/_static/image8.jpg)

#### <a name="updating-database-records-with-the-entity-framework"></a>Entity Framework でのデータベース レコードの更新

新しいデータベース レコードを挿入するだけに従うと、アプローチとして、Entity Framework とデータベース レコードを編集するほぼ同じアプローチを行うことができます。 4 を一覧表示するには、次の 2 つの新しいコント ローラー アクション Edit() をという名前が含まれます。 最初の Edit() アクションでは、ムービーのレコードを編集するため、HTML フォームを返します。 2 番目の Edit() アクションは、データベースを更新しようとします。

**4 – controllers \homecontroller.cs (編集メソッド) を一覧表示します。**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample6.cs)]

2 番目の Edit() アクションは、編集されている映画の Id に一致するデータベースからムービー レコードを取得することによって開始されます。 エンティティ ステートメントに次の LINQ では、特定の Id と一致する最初のデータベース レコードを取得します。

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample7.cs)]

次に、TryUpdateModel() メソッドを使用して、ムービーのエンティティのプロパティには HTML フォームのフィールドの値を割り当てます。 更新する正確なプロパティを指定する、ホワイト リストを提供することを確認します。

次に、いくつかの単純な検証を実行して、ムービー タイトルとディレクターの両方のプロパティに値があることを確認します。 いずれかのプロパティの値がない場合は、ModelState に検証エラー メッセージを追加し、ModelState.IsValid 値 false が返されます。

最後に、検証エラーがない場合は、基になる映画データベース テーブルが更新で変更された SaveChanges() メソッドを呼び出すことによってです。

データベース レコードを編集するときは、データベースの更新を実行するコント ローラーのアクションを編集中のレコードの Id を渡す必要があります。 それ以外の場合、コント ローラーのアクションでは、基になるデータベースで更新するためにどのレコードがわかりません。 5 の一覧に含まれている、編集ビューには、編集中のデータベース レコードの Id を表す隠しフォーム フィールドが含まれています。

**Listing 5 – Views\Home\Edit.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample8.aspx)]

## <a name="deleting-database-records-with-the-entity-framework"></a>Entity Framework でのデータベース レコードを削除します。

このチュートリアルで取り組む必要があります、最終的なデータベースの操作は、データベース レコードを削除しています。 リスト 6 でコント ローラーのアクションを使用すると、特定のデータベース レコードを削除します。

**Listing 6 -- \Controllers\HomeController.cs (Delete action)**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample9.cs)]

Delete() 操作は、アクションに渡される Id と一致するエンティティ、ムービーを最初に取得します。 次に、ムービーは SaveChanges() メソッドを続けて DeleteObject() メソッドを呼び出すことによって、データベースから削除されます。 最後に、インデックス ビューに戻る、ユーザーがリダイレクトされます。

## <a name="summary"></a>まとめ

このチュートリアルの目的は、ASP.NET MVC および Microsoft Entity Framework を活用して、データベースに基づく web アプリケーションを構築する方法を示すためにしました。 使用すると、選択、挿入、更新、アプリケーションを構築およびデータベースのレコードを削除する方法を学習しました。

最初に、Entity Data Model ウィザードを使用して、Visual Studio 内からのエンティティ データ モデルを生成する方法について説明します。 次に、LINQ to Entities を使用して、データベース テーブルから特定のデータベース レコード セットを取得する方法を説明します。 最後に、Entity Framework を使用して挿入、更新、およびデータベースのレコードを削除しました。

> [!div class="step-by-step"]
> [次へ](creating-model-classes-with-linq-to-sql-cs.md)
