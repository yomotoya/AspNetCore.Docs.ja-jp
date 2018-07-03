---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-vb
title: Entity Framework (VB) でモデル クラスを作成 |Microsoft Docs
author: microsoft
description: このチュートリアルでは、Microsoft Entity Framework で ASP.NET MVC を使用する方法について説明します。 エンティティ ウィザードを使用して、ADO.NET エンティティ データを作成する方法を学習します.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: ff8322c9-12f3-4e24-aba6-a38046b9bb0d
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-vb
msc.type: authoredcontent
ms.openlocfilehash: efb8d7206cba2fd5d8db1817d57a4e2813cb5ace
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377189"
---
<a name="creating-model-classes-with-the-entity-framework-vb"></a>Entity Framework (VB) でモデル クラスを作成
====================
によって[Microsoft](https://github.com/microsoft)

> このチュートリアルでは、Microsoft Entity Framework で ASP.NET MVC を使用する方法について説明します。 エンティティ ウィザードを使用して、ADO.NET Entity Data Model を作成する方法について説明します。 このチュートリアルの過程で、選択、挿入、更新、および Entity Framework を使用してデータベースのデータを削除する方法について説明する web アプリケーションを構築します。


このチュートリアルの目的では、ASP.NET MVC アプリケーションを構築するときに、Microsoft Entity Framework を使用してデータ アクセス クラスを作成する方法について説明します。 このチュートリアルは、Microsoft Entity Framework の以前の知識を負いません。 このチュートリアルの目的は、Entity Framework を使用して、選択、挿入、更新、およびデータベースのレコードを削除する方法を理解します。

Microsoft Entity Framework は、オブジェクト リレーショナル マッピング (O/RM) ツール、データベースからデータ アクセス層を自動的に生成することができます。 Entity Framework では、手動で、データ アクセス クラスを構築する面倒な作業を回避することができます。

> [!NOTE] 
> 
> ASP.NET MVC と Entity Framework の Microsoft 必須の接続がありません。 ASP.NET MVC で使用できる Entity Framework のいくつかの選択肢があります。 たとえば、Microsoft LINQ to SQL や NHibernate など、SubSonic などその他の O/RM ツールを使用して、MVC のモデル クラスをビルドすることができます。


ASP.NET MVC を使用した Microsoft Entity Framework を使用する方法について説明するには、簡単なサンプル アプリケーションをビルドします。 表示し、映画データベース レコードを編集することができます、映画データベース アプリケーションを作成します。

このチュートリアルでは、Visual Studio 2008 または Service Pack 1 の Visual Web Developer 2008 があることを前提としています。 Entity Framework を使用するために Service Pack 1 を必要があります。 Visual Studio 2008 Service Pack 1 または Service Pack 1 の Visual Web Developer は、次のアドレスからダウンロードできます。

> [https://www.asp.net/downloads/](https://www.asp.net/downloads)


## <a name="creating-the-movie-sample-database"></a>ムービーのサンプル データベースの作成

映画データベース アプリケーションでは、次の列を含むデータベース テーブルのムービーをという名前を使用します。

| 列名 | データの種類 | Null 値を許可しますか。 | プライマリ キーとは |
| --- | --- | --- | --- |
| ID | int | False | True |
| Title | nvarchar(100) | False | False |
| ディレクター | nvarchar(100) | False | False |

このテーブルは、次の手順に従って、ASP.NET MVC プロジェクトに追加できます。

1. アプリを右クリックして\_メニュー オプションを選択して、ソリューション エクスプ ローラー ウィンドウで、データ フォルダー**追加]、[新しい項目。**
2. **新しい項目の追加**ダイアログ ボックスで、 **SQL Server データベース**MoviesDB.mdf、名、データベースを提供し、クリックして、**追加**ボタンをクリックします。
3. サーバー エクスプ ローラー/データベース エクスプ ローラー ウィンドウを開く MoviesDB.mdf ファイルをダブルクリックします。
4. MoviesDB.mdf データベース接続を展開し、[テーブル] フォルダーを右クリックしてメニュー オプションを選択**新しいテーブルの追加**します。
5. テーブル デザイナーでは、Id、タイトル、およびディレクター列を追加します。
6. をクリックして、**保存**ボタン (フロッピー ディスクのアイコンがある) 映画名と、新しいテーブルを保存します。

映画データベースのテーブルを作成した後は、テーブルにサンプル データを追加する必要があります。 映画のテーブルを右クリックし、メニュー オプションを選択**テーブル データの表示**します。 表示されたグリッドには、偽の映画のデータを入力できます。

## <a name="creating-the-adonet-entity-data-model"></a>ADO.NET Entity Data Model を作成します。

Entity Framework を使用するには、Entity Data Model を作成する必要があります。 Visual Studio の利用*Entity Data Model ウィザード*データベースからエンティティ データ モデルを自動的に生成します。

この場合は、以下の手順に従ってください。

1. ソリューション エクスプ ローラー ウィンドウで、Models フォルダーを右クリックし、メニュー オプションを選択**追加、新しい項目の**します。
2. **新しい項目の追加**ダイアログ ボックスで、データ カテゴリを選択します (図 1 参照)。
3. 選択、 **ADO.NET Entity Data Model**テンプレート、名 MoviesDBModel.edmx、エンティティ データ モデルを提供し、**追加**ボタンをクリックします。 クリックすると、**追加**ボタンは、データ モデル ウィザードを起動します。
4. **モデルのコンテンツの選択**ステップで、選択、**をデータベースから生成**オプションを選択し、をクリックして、**次**ボタン (図 2 参照)。
5. **データ接続の選択**ステップ、MoviesDB.mdf データベース接続を選択し、エンティティ接続設定 MoviesDBEntities、という名前を入力してください、**次**ボタン (図 3 参照)。
6. **データベース オブジェクトの選択**ステップ ムービー データベースのテーブルを選択して、クリックして、**完了**(図 4 参照) ボタンをクリックします。

次の手順を完了すると、ADO.NET Entity Data Model デザイナー (エンティティ デザイナー) を開きます。

**図 1 – 新しい Entity Data Model を作成します。**

![clip_image002](creating-model-classes-with-the-entity-framework-vb/_static/image1.jpg)

**図 2 – モデル内容のステップを選択**

![clip_image004](creating-model-classes-with-the-entity-framework-vb/_static/image2.jpg)

**図 3: データ接続の選択**

![clip_image006](creating-model-classes-with-the-entity-framework-vb/_static/image3.jpg)

**図 4-データベース オブジェクトの選択**

![clip_image008](creating-model-classes-with-the-entity-framework-vb/_static/image4.jpg)

## <a name="modifying-the-adonet-entity-data-model"></a>ADO.NET エンティティ データ モデルの変更

Entity Data Model を作成した後は、エンティティ デザイナーを利用して、モデルを変更できます (図 5 を参照してください)。 ソリューション エクスプ ローラー ウィンドウ内で、Models フォルダーに含まれている MoviesDBModel.edmx ファイルをダブルクリックして、いつでも、エンティティ デザイナーを開くことができます。

**5 –、ADO.NET Entity Data Model デザイナーを図します。**

![clip_image010](creating-model-classes-with-the-entity-framework-vb/_static/image5.jpg)

たとえば、エンティティ デザイナーを使用して、エンティティ モデルのデータ ウィザードで生成するクラスの名前を変更することができます。 ウィザードでは、映画をという名前の新しいデータ アクセス クラスを作成します。 つまり、ウィザードは、データベース テーブルとまったく同じ名前のクラスしました。 使用するのでこのクラスを特定のムービー インスタンスを表す、ムービーに映画からクラスの名前する必要があります。

エンティティ クラスの名前を変更する場合は、エンティティ デザイナーでクラス名をダブルクリックして、新しい名前を入力します (図 6 参照)。 また、エンティティ デザイナーでエンティティを選択した後、[プロパティ] ウィンドウ内のエンティティの名前を変更できます。

**図 6 – エンティティの名前を変更します。**

![clip_image012](creating-model-classes-with-the-entity-framework-vb/_static/image6.jpg)

保存ボタン (フロッピー ディスクのアイコン) をクリックして変更を行った後に、エンティティ データ モデルを保存してください。 バック グラウンドでは、エンティティ デザイナーは、Visual Basic .NET クラスのセットを生成します。 ソリューション エクスプ ローラー ウィンドウから MoviesDBModel.Designer.vb ファイルを開くことで、これらのクラスを表示できます。


次に、エンティティ デザイナーを使用するときに、変更が上書きされますので、.designer.vb ファイル内のコードを変更しないでください。 .Designer.vb ファイルで定義されたエンティティ クラスの機能を拡張するかどうかは、作成することができます*部分クラス*で個別のファイル。


#### <a name="selecting-database-records-with-the-entity-framework"></a>Entity Framework でのデータベース レコードを選択します。

ムービーのレコードの一覧を表示するページを作成し、映画データベース アプリケーションを構築してみましょう。 リスト 1 で、Home コント ローラーは、Index() という名前のアクションを公開します。 Index() アクションに、ムービーのデータベース テーブルからすべてのムービー レコード、Entity Framework を活用してに返します。

**1 – Controllers\HomeController.vb を一覧表示します。**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample1.vb)]

リスト 1 で、コント ローラーがコンス トラクターが含まれることに注意してください。 コンス トラクターによって初期化という名前のクラスレベル フィールド\_db します。 \_Db フィールドは、Microsoft Entity Framework によって生成されたデータベースのエンティティを表します。 \_Db フィールドは、エンティティ デザイナーによって生成された MoviesDBEntities クラスのインスタンスです。

\_Db フィールドは、映画データベース テーブルからレコードを取得する Index() 操作内で使用します。 式\_db します。MovieSet では、映画データベース テーブルからすべてのレコードを表します。 ToList() メソッドは、Movie オブジェクトのジェネリック コレクションに映画のセットを変換するために使用します。 一覧 (のムービー)。

LINQ to Entities を利用して、ムービーのレコードが取得されます。 リスト 1 で Index() アクションは LINQ を使用して*メソッド構文*データベース レコードのセットを取得します。 LINQ を使用することができる場合は、*クエリ構文*代わりにします。 次の 2 つのステートメントでは、まったく同じことを行います。

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample2.vb)]

どの LINQ 構文を使用 – メソッド構文またはクエリ構文 – 最も直感的に表示されています。 2 つのアプローチの間のパフォーマンスに違いはありません – 唯一の違いはスタイル。

リスト 2 でビューを使用すると、ムービーのレコードを表示します。

**2 – Views\Home\Index.aspx を一覧表示します。**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample3.aspx)]

リスト 2 でビューが含まれています、**各**ループをムービーの各レコードを反復処理し、ムービーのレコードのタイトルとディレクター プロパティの値が表示されます。 各レコードの横にある、編集、削除のリンクが表示されることに注意してください。 ビューの下部にあるさらに、ムービーの追加リンクが表示されます (図 7 を参照してください)。

**図 7 –、インデックス ビュー**

![clip_image014](creating-model-classes-with-the-entity-framework-vb/_static/image7.jpg)

インデックス ビューは、*ビュー型指定された*します。 インデックス ビューでは、&lt;ページ % @ %&gt;ディレクティブの Inherits 属性が含まれます。 Inherits 属性は、厳密に型指定されたジェネリック リストのコレクションにムービー オブジェクト – 一覧 (のムービー) ViewData.Model プロパティをキャストします。

## <a name="inserting-database-records-with-the-entity-framework"></a>Entity Framework でのデータベース レコードを挿入します。

Entity Framework を使用すると、データベース テーブルに新しいレコードを挿入しやすきます。 3 を一覧表示するには、ムービーのデータベース テーブルに新しいレコードを挿入するために使用できる、ホーム コント ローラー クラスに追加された 2 つの新しいアクションが含まれています。

**3 – Controllers\HomeController.vb (メソッドの追加) を一覧表示します。**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample4.vb)]

Add() の最初のアクションでは、ビューを単純に返します。 ビューには、新しいムービー データベースを追加するフォームが含まれます (図 8 参照) を記録します。 フォームを送信するときに、2 番目の Add() アクションが呼び出されます。

2 番目の Add() アクションは、AcceptVerbs 属性で修飾されていることに注意してください。 HTTP POST 操作を実行する場合にのみ、この操作を呼び出すことができます。 つまり、この操作は、HTML フォームを投稿する場合にのみ呼び出すことができます。

2 番目の Add() アクションは、ASP.NET MVC TryUpdateModel() メソッドのヘルプで、Entity Framework Movie クラスの新しいインスタンスを作成します。 TryUpdateModel() メソッドは、Add() メソッドに渡される FormCollection 内のフィールドを受け取り、Movie クラスに HTML フォーム フィールドの値を割り当てます。


Entity Framework を使用する場合は、tryupdatemodel に渡します"または"UpdateModel メソッドを使用して、エンティティ クラスのプロパティを更新する場合、プロパティの「ホワイト リスト」を指定してください。


次に、Add() アクションは、いくつかの簡単なフォーム検証を実行します。 アクションは、タイトルとディレクターの両方のプロパティに値があることを確認します。 検証エラーがある場合、検証エラー メッセージは、ModelState に追加されます。

検証エラーがない場合、新しいムービーのレコードは、Entity Framework を利用して、映画データベース テーブルに追加されます。 新しいレコードは、次の 2 つのコード行をデータベースに追加されます。

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample5.vb)]

コードの最初の行では、Entity Framework によって追跡されているムービーのセットに新しいムービー エンティティを追加します。 基になるデータベースに追跡されている映画を自由に変更を加え、2 つ目のコード行を保存します。

**図 8-追加のビュー**

![clip_image016](creating-model-classes-with-the-entity-framework-vb/_static/image8.jpg)

## <a name="updating-database-records-with-the-entity-framework"></a>Entity Framework でのデータベース レコードを更新しています

新しいデータベース レコードを挿入するだけに準拠した方法として、Entity Framework でのデータベース レコードを編集するほぼ同じ方法に従うことができます。 4 を一覧表示するには、Edit() という名前の 2 つの新しいコント ローラー アクションが含まれています。 Edit() の最初のアクションは、ムービーのレコードを編集するための HTML フォームを返します。 2 番目の Edit() アクションは、データベースを更新しようとします。

**4 – Controllers\HomeController.vb (Edit メソッド) を一覧表示します。**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample6.vb)]

2 番目の Edit() アクションは、ムービーの編集中の Id と一致するデータベースからムービー レコードを取得することによって開始します。 エンティティ ステートメントに次の LINQ では、特定の Id と一致する最初のデータベース レコードを取得します。

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample7.vb)]

次に、ムービー エンティティのプロパティには HTML フォームのフィールドの値を割り当てる TryUpdateModel() メソッドを使用します。 更新する正確なプロパティを指定するホワイト リストが提供されることに注意してください。

次に、単純な検証を実行して、ムービーのタイトルとディレクターの両方のプロパティに値があることを確認します。 プロパティのいずれかがない場合、値、ModelState に検証エラー メッセージを追加し、ModelState.IsValid 値 false を返します。

最後に、検証エラーがない場合、基になる映画データベース テーブルで更新されます変更 SaveChanges() メソッドを呼び出すことによって。

データベースのレコードを編集するときは、データベースの更新を実行するコント ローラー アクションを編集中のレコードの Id を渡す必要があります。 それ以外の場合、コント ローラー アクションでは、基になるデータベースを更新するレコードはわかりません。 リストに 5 に含まれている、編集ビューには、編集中のデータベース レコードの Id を表す隠しフォーム フィールドが含まれています。

**Listing 5 – Views\Home\Edit.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample8.aspx)]

## <a name="deleting-database-records-with-the-entity-framework"></a>Entity Framework でのデータベース レコードを削除します。

このチュートリアルに取り組む必要があります、最終的なデータベース操作では、データベース レコードを削除しています。 リスト 6 でコント ローラー アクションを使用すると、特定のデータベース レコードを削除します。

**Listing 6 -- \Controllers\HomeController.vb (Delete action)**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample9.vb)]

Delete() アクションは、まず、Id と一致するエンティティは、アクションに渡されるムービーを取得します。 次に、ムービーは、SaveChanges() メソッドを続けて DeleteObject() メソッドを呼び出すことによって、データベースから削除されます。 最後に、ユーザーはインデックス ビューにリダイレクトされます。

## <a name="summary"></a>まとめ

このチュートリアルの目的は、ASP.NET MVC と Microsoft Entity Framework を活用して、データベース駆動型 web アプリケーションをビルドする方法をデモしました。 使用すると、選択、挿入、更新、アプリケーションを構築し、データベース レコードを削除する方法を学習しました。

まず、Visual Studio 内からのエンティティ データ モデルを生成する、Entity Data Model ウィザードを使用する方法について説明します。 次に、LINQ to Entities を使用して、データベース テーブルから一連のデータベース レコードを取得する方法について説明します。 最後に、Entity Framework を使用して挿入、更新、およびデータベースのレコードを削除しました。

> [!div class="step-by-step"]
> [前へ](validation-with-the-data-annotation-validators-cs.md)
> [次へ](creating-model-classes-with-linq-to-sql-vb.md)
