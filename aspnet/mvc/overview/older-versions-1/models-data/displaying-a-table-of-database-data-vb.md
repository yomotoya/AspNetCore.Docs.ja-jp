---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
title: データベースのデータ (VB) の表を表示する |Microsoft Docs
author: microsoft
description: このチュートリアルでは、一連のデータベース レコードを表示する 2 つの方法を紹介します。 書式設定、HTML でのデータベース レコードのセットを ta の 2 つの方法を紹介しています.
ms.author: aspnetcontent
ms.date: 10/07/2008
ms.assetid: 5bb4587f-5bcd-44f5-b368-3c1709162b35
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 0b796c424cfe3fb03f3d6eddd8812438ceea2026
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830274"
---
<a name="displaying-a-table-of-database-data-vb"></a>データベースのデータ (VB) の表を表示します。
====================
によって[Microsoft](https://github.com/microsoft)

[PDF のダウンロード](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_VB.pdf)

> このチュートリアルでは、一連のデータベース レコードを表示する 2 つの方法を紹介します。 一連の HTML テーブルのデータベース レコードを書式設定の 2 つの方法を紹介します。 最初に、ビュー内で直接、データベース レコードの書式を設定する方法を紹介します。 次に、方法を利用するパーシャルのデータベース レコードを書式設定時に紹介します。


このチュートリアルの目的では、ASP.NET MVC アプリケーションでデータベースのデータの HTML テーブルを表示する方法について説明します。 まず、Visual Studio に含まれるスキャフォールディング ツールを使用して、一連のレコードを自動的に表示するビューを生成する方法について説明します。 次に、データベース レコードを書式設定時に一部のテンプレートとして使用する方法について説明します。

## <a name="create-the-model-classes"></a>モデル クラスを作成します。

ここ、映画データベース テーブルからのレコードのセットを表示します。 映画データベース テーブルには、次の列が含まれています。

<a id="0.4_table01"></a>


| **列名** | **データ型** | **Null を許容します。** |
| --- | --- | --- |
| ID | Int | False |
| Title | Nvarchar(200) | False |
| ディレクター | Nvarchar (50) | False |
| DateReleased | DateTime | False |


ASP.NET MVC アプリケーションで映画のテーブルを表すためには、モデル クラスを作成する必要があります。 このチュートリアルでは、モデル クラスを作成するのに Microsoft Entity Framework を使用します。

> [!NOTE] 
> 
> このチュートリアルでは、Microsoft Entity Framework を使用します。 ただし、LINQ to SQL、NHibernate、または ADO.NET を含む、ASP.NET MVC アプリケーションからデータベースと対話する多様なテクノロジを使用することができますを理解しておく必要です。


エンティティ データ モデル ウィザードを起動する次の手順に従います。

1. ソリューション エクスプ ローラー ウィンドウおよびメニュー オプションを選択して、モデル フォルダーを右クリックして**追加、新しい項目の**します。
2. 選択、**データ**カテゴリと選択、 **ADO.NET Entity Data Model**テンプレート。
3. データ モデルに名前を付けます*MoviesDBModel.edmx*  をクリックし、**追加**ボタンをクリックします。

[追加] ボタンをクリックした後、Entity Data Model ウィザードでは、(図 1 参照) が表示されます。 ウィザードの次の手順に従います。

1. **モデルのコンテンツの選択**手順で、**データベースから生成**オプション。
2. **データ接続の選択**ステップで、使用、 *MoviesDB.mdf*データ接続と名前*MoviesDBEntities*接続の設定。 をクリックして、**次**ボタンをクリックします。
3. **データベース オブジェクトの選択**ステップ、[テーブル] ノードを展開し、映画のテーブルを選択します。 名前空間を入力*モデル* をクリックし、**完了**ボタンをクリックします。


[![LINQ to SQL クラスを作成](displaying-a-table-of-database-data-vb/_static/image1.jpg)](displaying-a-table-of-database-data-vb/_static/image1.png)

**図 01**: LINQ to SQL クラスを作成する ([フルサイズの画像を表示する をクリックします](displaying-a-table-of-database-data-vb/_static/image2.png))。


Entity Data Model ウィザードを完了すると、Entity Data Model のデザイナーが開きます。 デザイナーは、ムービー エンティティを表示する必要があります (図 2 参照)。


[![Entity Data Model デザイナー](displaying-a-table-of-database-data-vb/_static/image2.jpg)](displaying-a-table-of-database-data-vb/_static/image3.png)

**図 02**: The Entity Data Model デザイナー ([フルサイズの画像を表示する をクリックします](displaying-a-table-of-database-data-vb/_static/image4.png))。


続行する前に 1 つの変更を行う必要があります。 という名前のモデル クラスを生成するエンティティのデータ ウィザード*映画*映画データベース テーブルを表します。 映画クラスを使用して、特定のムービーを表す、ためにクラスの名前を変更する必要があります*ムービー*の代わりに*映画*(単数形ではなく複数形)。

デザイナー画面で、クラスの名前をダブルクリックし、ムービーからムービーにクラスの名前を変更します。 この変更を加えたら、 をクリックして、**保存**ムービー クラスを生成する (フロッピー ディスクのアイコン) をクリックします。

## <a name="create-the-movies-controller"></a>ムービー コント ローラーを作成します。

あるので、データベース レコードを表す方法、映画のコレクションを返すコント ローラーを作成できます。 Visual Studio ソリューション エクスプ ローラー ウィンドウで、Controllers フォルダーを右クリックし、メニュー オプションを選択します**追加、コント ローラー** (図 3 を参照してください)。


[![コント ローラーのメニューに追加します。](displaying-a-table-of-database-data-vb/_static/image3.jpg)](displaying-a-table-of-database-data-vb/_static/image5.png)

**図 03**: [コント ローラー] メニューの追加 ([フルサイズの画像を表示する をクリックします](displaying-a-table-of-database-data-vb/_static/image6.png))。


ときに、**コント ローラーの追加**ダイアログが表示されたら、MovieController コント ローラー名を入力します (図 4 参照)。 をクリックして、**追加**新しいコント ローラーを追加するボタンをクリックします。


[![コント ローラーの追加 ダイアログ](displaying-a-table-of-database-data-vb/_static/image4.jpg)](displaying-a-table-of-database-data-vb/_static/image7.png)

**図 04**:、コント ローラーの追加 ダイアログ ([フルサイズの画像を表示する をクリックします](displaying-a-table-of-database-data-vb/_static/image8.png))。


データベースのレコードのセットを返すように、ムービー コント ローラーによって公開される Index() アクションを変更する必要があります。 コント ローラーを変更して、リスト 1 でコント ローラーのように見えます。

**1 – Controllers\MovieController.vb を一覧表示します。**

[!code-vb[Main](displaying-a-table-of-database-data-vb/samples/sample1.vb)]

リストの 1 では、MoviesDBEntities クラスを使用してを MoviesDB データベースを表します。 式*エンティティ。MovieSet.ToList()* 映画データベース テーブルからすべての映画のセットを返します。

## <a name="create-the-view"></a>ビューを作成します。

HTML テーブルの一連のデータベース レコードを表示する最も簡単な方法は、Visual Studio によって提供されるスキャフォールディングを活用するためにです。

メニュー オプションを選択して、アプリケーションをビルド**ソリューションのビルドをビルドします。** します。 開始する前に、アプリケーションをビルドする必要があります、**ビューの追加**ダイアログや、データ クラスは、ダイアログ ボックスに表示されません。

Index() アクションを右クリックし、メニュー オプションを選択**ビューの追加**(図 5 を参照してください)。


[![ビューの追加](displaying-a-table-of-database-data-vb/_static/image5.jpg)](displaying-a-table-of-database-data-vb/_static/image9.png)

**図 05**: ビューの追加 ([フルサイズの画像を表示する をクリックします](displaying-a-table-of-database-data-vb/_static/image10.png))。


**ビューの追加**ダイアログ ボックスで、チェック ボックスをラベル付け**厳密に型指定されたビューを作成**。 としてムービー クラスの選択、**データ クラスを表示**します。 選択*一覧*として、**コンテンツを表示**(図 6 参照)。 これらのオプションを選択すると、ムービーの一覧を表示する厳密に型指定されたビューが生成されます。


[![ビューの追加 ダイアログ](displaying-a-table-of-database-data-vb/_static/image6.jpg)](displaying-a-table-of-database-data-vb/_static/image11.png)

**図 06**: ビューの追加 ダイアログ ([フルサイズの画像を表示する をクリックします](displaying-a-table-of-database-data-vb/_static/image12.png))。


クリックした後、**追加**ボタン、リスト 2 でビューが自動的に生成されます。 このビューには、映画のコレクションを反復処理し、各ビデオのプロパティの表示に必要なコードが含まれています。

**2 – Views\Movie\Index.aspx を一覧表示します。**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample2.aspx)]

アプリケーションを実行するには、メニュー オプションを選択して**デバッグ、デバッグの開始** (または、F5 キー)。 アプリケーションを実行すると、Internet Explorer が起動します。 /Movie URL に移動すると、図 7 ページが表示されます。


[![映画のテーブル](displaying-a-table-of-database-data-vb/_static/image7.jpg)](displaying-a-table-of-database-data-vb/_static/image13.png)

**図 07**: 映画のテーブル ([フルサイズの画像を表示する をクリックします](displaying-a-table-of-database-data-vb/_static/image14.png))。


図 7 内のデータベース レコードのグリッドの外観について何も満足できない場合は、インデックス ビューだけ変更できます。 たとえば、変更、 *DateReleased*ヘッダーを*リリース日*インデックス ビューを変更することで。

## <a name="create-a-template-with-a-partial"></a>一部のテンプレートを作成します。

ビューは、複雑すぎるようですは、パーシャル、ビューに分割を開始することをお勧めします。 パーシャルを使用すると、ビューの理解および保守簡単です。 部分を作成します各ムービー データベース レコードを書式設定するテンプレートとして使用できます。

部分を作成する次の手順に従います。

1. Views\Movie フォルダーを右クリックし、メニュー オプションを選択**ビューの追加**します。
2. チェック ボックスをオンというラベルの付いた*部分ビュー (.ascx) を作成*です。
3. 部分的な名前を付けます*MovieTemplate*します。
4. チェック ボックスをラベル付け**厳密に型指定されたビューを作成する**します。
5. としてムービーを選択して、*データ クラスを表示*します。
6. として空の選択、*コンテンツを表示*します。
7. をクリックして、**追加**部分をプロジェクトに追加するボタンをクリックします。

次の手順を完了すると、リスト 3 のように部分的な MovieTemplate を変更します。

**3 – Views\Movie\MovieTemplate.ascx を一覧表示します。**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample3.aspx)]

リスト 3 の部分には、レコードの 1 つの行のテンプレートが含まれています。

リスト 4 変更後のインデックス ビューは、部分的な MovieTemplate を使用します。

**4 – Views\Movie\Index.aspx を一覧表示します。**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample4.aspx)]

ビュー リスト 4 は、For Each ループをムービーのすべての反復処理します。 各ムービーの MovieTemplate 部分は、ムービーを書式設定に使用されます。 RenderPartial() ヘルパー メソッドを呼び出して、MovieTemplate がレンダリングされます。

変更したのインデックス ビューは、データベース レコードのまったく同じ HTML テーブルを表示します。 ただし、ビューが大幅に簡略化されました。


RenderPartial() メソッドは、文字列を返さないため、ほとんどの他のヘルパー メソッドとは異なる。 RenderPartial() メソッドを使用して、呼び出す必要がありますので、 &lt;%html.renderpartial()&gt;の代わりに&lt;% = Html.RenderPartial() %&gt;します。


## <a name="summary"></a>まとめ

このチュートリアルの目的は、HTML テーブルの一連のデータベース レコードを表示する方法を説明することでした。 最初に、Microsoft Entity Framework を活用して、コント ローラー アクションから一連のデータベース レコードを返す方法を学習しました。 次に、Visual Studio のスキャフォールディングを使用して、項目のコレクションを自動的に表示するビューを生成する方法を学習しました。 最後に、一部を活用して、ビューを簡略化する方法を学習しました。 データベースのレコードの書式を設定することができるように、テンプレートとして、部分的なを使用する方法を学習しました。

> [!div class="step-by-step"]
> [前へ](creating-model-classes-with-linq-to-sql-vb.md)
> [次へ](performing-simple-validation-vb.md)
