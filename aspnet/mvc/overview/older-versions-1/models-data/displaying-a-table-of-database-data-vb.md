---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
title: データベースのデータ (VB) のテーブルを表示する |Microsoft ドキュメント
author: microsoft
description: このチュートリアルでは、一連のデータベース レコードを表示する 2 つの方法を説明します。 書式の HTML 内のデータベース レコードのセット ta の 2 つの方法を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: 5bb4587f-5bcd-44f5-b368-3c1709162b35
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
msc.type: authoredcontent
ms.openlocfilehash: dd871520f98aaae2d7b33d83b1646eb9eee88821
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872274"
---
<a name="displaying-a-table-of-database-data-vb"></a>データベースのデータ (VB) のテーブルを表示します。
====================
によって[Microsoft](https://github.com/microsoft)

[PDF をダウンロードします。](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_VB.pdf)

> このチュートリアルでは、一連のデータベース レコードを表示する 2 つの方法を説明します。 HTML テーブル内のデータベース レコードのセットを書式設定の 2 つの方法を説明します。 最初に、ビュー内で直接、データベース レコードの書式を設定する方法をお見せします。 次に、デモが利用できるかパーシャルのデータベース レコードを書式設定する場合。


このチュートリアルの目的では、ASP.NET MVC アプリケーションで HTML テーブル データベースのデータを表示する方法について説明します。 最初に、ツールを使用して、スキャフォールディング Visual Studio に含まれる一連のレコードを自動的に表示するビューを生成する方法を説明します。 次に、データベース レコードをフォーマットするときに、部分的なをテンプレートとして使用する方法を説明します。

## <a name="create-the-model-classes"></a>モデルのクラスを作成します。

映画のデータベース テーブルからレコードのセットを表示しようとしています。 映画のデータベース テーブルには、次の列が含まれています。

<a id="0.4_table01"></a>


| **列名** | **データ型** | **Null を許容します。** |
| --- | --- | --- |
| ID | Int | False |
| Title | Nvarchar(200) | False |
| ディレクター | NVarchar(50) | False |
| DateReleased | DateTime | False |


ASP.NET MVC アプリケーションでは、ムービーのテーブルを表すためには、モデル クラスを作成する必要があります。 このチュートリアルでは、Microsoft の Entity Framework を使用して、モデル クラスを作成します。

> [!NOTE] 
> 
> このチュートリアルでは、Microsoft の Entity Framework を使用します。 ただし、LINQ to SQL、NHibernate、または ADO.NET を含む ASP.NET MVC アプリケーションからデータベースとやり取りするさまざまな異なるテクノロジを使用することができますを理解しておく必要があります。


Entity Data Model ウィザードを起動する次の手順に従います。

1. ソリューション エクスプ ローラー ウィンドウのオプションを選択し、メニューの モデル フォルダーを右クリックして**追加、新しい項目の**します。
2. 選択、**データ**カテゴリを選択、 **ADO.NET エンティティ データ モデル**テンプレート。
3. データ モデルに名前を付ける*MoviesDBModel.edmx*  をクリックし、**追加**ボタンをクリックします。

Entity Data Model ウィザードでは、[追加] ボタンをクリックした後、(図 1 を参照) が表示されます。 ウィザードを完了する手順に従います。

1. **モデルのコンテンツ**手順、select、**データベースから生成**オプション。
2. **データ接続の選択**ステップを使用して、 *MoviesDB.mdf*データ接続と名前*MoviesDBEntities*接続の設定。 クリックして、**次**ボタンをクリックします。
3. **データベース オブジェクトの選択**ステップ、[テーブル] ノードを展開し、映画のテーブルを選択します。 名前空間を入力*モデル* をクリックし、**完了**ボタンをクリックします。


[![LINQ to SQL クラスの作成](displaying-a-table-of-database-data-vb/_static/image1.jpg)](displaying-a-table-of-database-data-vb/_static/image1.png)

**図 01**: LINQ to SQL クラスを作成する ([フルサイズのイメージを表示するをクリックして](displaying-a-table-of-database-data-vb/_static/image2.png))


Entity Data Model ウィザードを完了すると、Entity Data Model デザイナーが開きます。 デザイナーは、映画エンティティを表示する必要があります (図 2 を参照してください)。


[![Entity Data Model Designer](displaying-a-table-of-database-data-vb/_static/image2.jpg)](displaying-a-table-of-database-data-vb/_static/image3.png)

**図 02**: The Entity Data Model Designer ([フルサイズのイメージを表示するをクリックして](displaying-a-table-of-database-data-vb/_static/image4.png))


続行する前に、1 つの変更を行う必要があります。 エンティティ データ ウィザードという名前のモデル クラスを生成する*映画*映画データベース テーブルを表すです。 映画クラスを使用して、特定のムービーを表す、ためにクラスの名前を変更する必要があります*ムービー*の代わりに*映画*(単数形ではなく複数形)。

デザイナー画面にクラスの名前をダブルクリックし、ムービーからムービーにクラスの名前を変更します。 この変更を加えたらをクリックして、**保存**ムービー クラスを生成する (フロッピー ディスクのアイコン) ボタンをクリックします。

## <a name="create-the-movies-controller"></a>ビデオ コント ローラーを作成します。

データベース レコードを表す方法がある、ムービーのコレクションを取得するコント ローラーを作成できます。 Visual Studio ソリューション エクスプ ローラー ウィンドウ内でコント ローラーのフォルダーを右クリックし、メニュー オプションを選択**追加、コント ローラー** (図 3 を参照してください)。


[![コント ローラーのメニューに追加します。](displaying-a-table-of-database-data-vb/_static/image3.jpg)](displaying-a-table-of-database-data-vb/_static/image5.png)

**図 03**: 追加のコント ローラー メニュー ([フルサイズのイメージを表示するをクリックして](displaying-a-table-of-database-data-vb/_static/image6.png))


ときに、**コント ローラーの追加**MovieController コント ローラー名を入力してください ダイアログが表示されます (図 4 を参照してください)。 クリックして、**追加**を新しいコント ローラーを追加するボタンをクリックします。


[![コント ローラーの追加 ダイアログ](displaying-a-table-of-database-data-vb/_static/image4.jpg)](displaying-a-table-of-database-data-vb/_static/image7.png)

**図 04**:「コント ローラーの追加 ダイアログ ([フルサイズのイメージを表示するには、をクリックして](displaying-a-table-of-database-data-vb/_static/image8.png))


データベース レコードのセットを返すようにビデオ コント ローラーによって公開される Index() アクションを変更する必要があります。 コント ローラーを変更して、リスト 1 のコント ローラーのようになります。

**1 – Controllers\MovieController.vb を一覧表示します。**

[!code-vb[Main](displaying-a-table-of-database-data-vb/samples/sample1.vb)]

1 の一覧表示するのには、MoviesDBEntities クラスを使用してを MoviesDB データベースを表します。 式*エンティティです。MovieSet.ToList()* 映画データベース テーブルからすべてのムービーのセットを返します。

## <a name="create-the-view"></a>ビューを作成します。

HTML テーブルにデータベース レコードのセットを表示する最も簡単な方法は、Visual Studio によって提供されるスキャフォールディングを活用するためにです。

メニュー オプションを選択して、アプリケーションをビルド**ビルド、ソリューションのビルド**です。 開始する前に、アプリケーションをビルドする必要があります、**ビューの追加**ダイアログや、データ クラスは、ダイアログ ボックスに表示されません。

Index() アクションを右クリックし、メニュー オプションを選択**ビューの追加**(図 5 を参照してください)。


[![ビューの追加](displaying-a-table-of-database-data-vb/_static/image5.jpg)](displaying-a-table-of-database-data-vb/_static/image9.png)

**図 05**: ビューの追加 ([フルサイズのイメージを表示するをクリックして](displaying-a-table-of-database-data-vb/_static/image10.png))


**ビューの追加**ダイアログ ボックスで、チェック ボックス ラベルの付いた**厳密に型指定されたビューを作成**です。 としてムービー クラスを選択して、**データ クラスを表示**です。 選択*リスト*として、**コンテンツを表示**(図 6 を参照してください)。 これらのオプションを選択すると、ムービーの一覧を表示する厳密に型指定されたビューが生成されます。


[![ビューの追加 ダイアログ](displaying-a-table-of-database-data-vb/_static/image6.jpg)](displaying-a-table-of-database-data-vb/_static/image11.png)

**図 06**: ビューの追加 ダイアログ ([フルサイズのイメージを表示するをクリックして](displaying-a-table-of-database-data-vb/_static/image12.png))


クリックした後、**追加**ボタン、リスト 2 でビューが自動的に生成されます。 このビューには、映画のコレクションを反復処理し、各ムービーのプロパティを表示するためのコードが含まれています。

**2 – Views\Movie\Index.aspx を一覧表示します。**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample2.aspx)]

メニュー オプションを選択してアプリケーションを実行することができます**デバッグ、デバッグの開始** (または F5 キーを押す) します。 アプリケーションを実行すると、Internet Explorer が起動します。 /Movie URL に移動すると、図 7 にページが表示されます。


[![ムービーのテーブル](displaying-a-table-of-database-data-vb/_static/image7.jpg)](displaying-a-table-of-database-data-vb/_static/image13.png)

**図 07**: 映画のテーブル ([フルサイズのイメージを表示するをクリックして](displaying-a-table-of-database-data-vb/_static/image14.png))


図 7 にデータベース レコードのグリッドの外観について何もたくない場合、インデックス ビューだけで変更できます。 たとえば、変更することができます、 *DateReleased*ヘッダーを*リリース日*インデックス ビューを変更することによりします。

## <a name="create-a-template-with-a-partial"></a>一部のテンプレートを作成します。

ビューが複雑すぎるを取得するときは、パーシャルにビューを分割を開始することをお勧めします。 パーシャルを使用して容易ビューは、ユーザーに理解および保守します。 ここで作成、部分的なのムービーのデータベース レコードのそれぞれの書式を設定するテンプレートとして使用することができます。

部分を作成する手順に従います。

1. Views\Movie フォルダーを右クリックし、メニュー オプションを選択**ビューの追加**です。
2. チェック ボックス ラベルの付いた*部分ビュー (.ascx) を作成する*です。
3. 部分的な名前を付けます*MovieTemplate*です。
4. チェック ボックス ラベルの付いた**厳密に型指定されたビューを作成する**です。
5. ビデオとしてを選択して、*データ クラスを表示*です。
6. として空の選択、*コンテンツを表示*です。
7. クリックして、**追加**部分的なプロジェクトを追加する、ボタンをクリックします。

次の手順を完了すると、3 の一覧表示するように部分的な MovieTemplate を変更します。

**3 – Views\Movie\MovieTemplate.ascx を一覧表示します。**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample3.aspx)]

リスト 3 の部分には、レコードの単一行のテンプレートが含まれています。

インデックス ビューを一覧表示する 4 では、MovieTemplate 部分を使用します。

**Listing 4 – Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample4.aspx)]

ビュー一覧表示する 4 で、には For が含まれています、ムービーのすべてを反復処理する各ループします。 各ムービー MovieTemplate 部分を使用して、ムービーを書式設定をします。 RenderPartial() ヘルパー メソッドを呼び出すことによって、MovieTemplate がレンダリングされます。

変更したのインデックス ビューは、データベース レコードのまったく同じ HTML テーブルを表示します。 ただし、ビューを大幅に簡略化ができます。


RenderPartial() メソッドではほとんどの他のヘルパー メソッドとは異なる文字列を返すことはできません。 そのため、呼び出す必要があります RenderPartial() メソッドを使用して、 &lt;%html.renderpartial()&gt;の代わりに&lt;% = Html.RenderPartial() %&gt;です。


## <a name="summary"></a>まとめ

このチュートリアルの目的は、HTML テーブルの一連のデータベース レコードを表示する方法を説明しました。 最初に、コント ローラーのアクションから Microsoft Entity Framework を活用して、データベース レコードのセットを返す方法を学習します。 次に、Visual Studio のスキャフォールディングを使用して項目のコレクションを自動的に表示するビューを生成する方法を学習します。 最後に、一部を利用して、ビューを簡略化する方法を学習します。 データベースの各レコードの書式を設定することができるように、テンプレートとして、部分的なを使用する方法を学習しました。

> [!div class="step-by-step"]
> [前へ](creating-model-classes-with-linq-to-sql-vb.md)
> [次へ](performing-simple-validation-vb.md)
