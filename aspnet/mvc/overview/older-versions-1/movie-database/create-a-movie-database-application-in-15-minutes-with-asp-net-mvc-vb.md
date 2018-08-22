---
uid: mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb
title: ASP.NET MVC (VB) で 15 分以内に映画データベース アプリケーションを作成 |Microsoft Docs
author: StephenWalther
description: Stephen Walther データベース駆動 ASP.NET MVC アプリケーション全体の開始から完了するを構築します。 このチュートリアルでは、新しい t している人たちの導入として優れています.
ms.author: riande
ms.date: 01/27/2009
ms.assetid: e4ba9786-734c-4eb3-91bb-089793325d0d
msc.legacyurl: /mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb
msc.type: authoredcontent
ms.openlocfilehash: f0a060bffc2e45f54d03571b6609a30876202e32
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823852"
---
<a name="create-a-movie-database-application-in-15-minutes-with-aspnet-mvc-vb"></a>ASP.NET MVC (VB) で 15 分以内に映画データベース アプリケーションを作成します。
====================
によって[Stephen Walther](https://github.com/StephenWalther)

[コードをダウンロードします。](http://download.microsoft.com/download/7/2/8/728F8794-E59A-4D18-9A56-7AD2DB05BD9D/MovieApp_VB.zip)

> Stephen Walther データベース駆動 ASP.NET MVC アプリケーション全体の開始から完了するを構築します。 このチュートリアルは、ASP.NET MVC アプリケーションを構築するためのプロセスを把握したいは、ASP.NET MVC フレームワークに初めて、およびユーザーの導入として優れています。


このチュートリアルの目的は、「は、」の感覚をつかむことに、ASP.NET MVC アプリケーションを構築します。 このチュートリアルでは爆発開始から終了する全体 ASP.NET MVC アプリケーションを構築します。 私は、どのようにすることができますを一覧表示、作成、およびデータベース レコードを編集を示す単純なデータベース駆動型アプリケーションを構築する方法を示します。

アプリケーションを構築するプロセスを簡略化するのには、Visual Studio 2008 のスキャフォールディング機能の利点をみましょう。 最初のコードと、コント ローラー、モデル、およびビューのコンテンツを生成する Visual Studio をお知らせします。

Active Server Pages または ASP.NET を使用する場合、しする必要があります ASP.NET MVC とてもなじみやすいものです。 ASP.NET MVC ビューは、Active Server Pages アプリケーションのページとよく似ています。 また、従来の ASP.NET Web フォーム アプリケーションと同じように ASP.NET MVC を使うと、言語と .NET framework によって提供されるクラスの豊富なセットへのフル アクセス。

思いますが、このチュートリアルを把握するのと同様と異なる場合、Active Server Pages または ASP.NET Web フォーム アプリケーションの開発エクスペリエンスの両方の ASP.NET MVC アプリケーションを構築するためのエクスペリエンスです。

## <a name="overview-of-the-movie-database-application"></a>映画データベース アプリケーションの概要

私たちの目標では、簡潔であるために、非常に単純な映画データベース アプリケーションを作成します。 簡単なムービー データベース アプリケーションでは次の 3 つの作業を行うことを許可します。

1. ムービー データベース レコードのセットを一覧表示します。
2. 新しいムービー データベース レコードを作成します。
3. 既存のムービー データベース レコードを編集します。

ここでも、単純化するため、アプリケーションのビルドに必要な ASP.NET MVC framework の機能の最小数を活用移動します。 たとえば、私たちはありませんの恩恵をテスト駆動型開発。

アプリケーションを作成するためには、それぞれ、次の手順を完了する必要があります。

1. ASP.NET MVC Web アプリケーション プロジェクトを作成します。
2. データベースの作成
3. データベース モデルを作成します。
4. ASP.NET MVC コント ローラーを作成します。
5. ASP.NET MVC ビューを作成します。

## <a name="preliminaries"></a>準備作業

Visual Studio 2008 または Visual Web Developer 2008 Express、ASP.NET MVC アプリケーションを構築する必要があります。 また、ASP.NET MVC framework をダウンロードする必要があります。

Visual Studio 2008 を所有していない場合は、この web サイトから、Visual Studio 2008 の 90 日間試用版をダウンロードできます。

[https://msdn.microsoft.com/vs2008/products/cc268305.aspx](https://msdn.microsoft.com/vs2008/products/cc268305.aspx)

または、する ASP.NET MVC アプリケーションを作成できます Visual Web Developer Express 2008。 Visual Web Developer Express を使用すると、Service Pack 1 がインストールされているが必要です。 Visual Web Developer 2008 Express with Service Pack 1 は、この web サイトからダウンロードできます。

[https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&ampdisplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang=en)

Visual Studio 2008 または Visual Web Developer 2008 をインストールした後は、ASP.NET MVC framework をインストールする必要があります。 ASP.NET MVC フレームワークは、次の web サイトからダウンロードできます。

[https://www.asp.net/mvc/](../../../index.md)

> [!NOTE] 
> 
> ASP.NET framework と ASP.NET MVC フレームワークを個別にダウンロードする代わりに、Web Platform Installer の利点を実行できます。 Web Platform Installer は、アプリケーションを簡単にインストールされているアプリケーションを管理することができますが、コンピューターです。
> 
> [https://www.microsoft.com/web/gallery/Install.aspx](https://www.microsoft.com/web/gallery/Install.aspx)


## <a name="creating-an-aspnet-mvc-web-application-project"></a>ASP.NET MVC Web アプリケーション プロジェクトを作成します。

Visual Studio 2008 で新しい ASP.NET MVC Web アプリケーション プロジェクトを作成してみましょう。 メニュー オプションを選択**ファイル、新しいプロジェクト**図 1 新しいプロジェクト ダイアログ ボックスが表示されます。 プログラミング言語と Visual Basic を選択し、ASP.NET MVC Web アプリケーション プロジェクト テンプレートを選択します。 プロジェクト MovieApp の名前を付け、[ok] ボタンをクリックします。


[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image1.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image1.png)

**図 01**: [新しいプロジェクト] ダイアログ ボックス ([フルサイズの画像を表示する をクリックします](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image2.png))。


必ず、新しいプロジェクト ダイアログの上部にあるドロップダウン リストから .NET Framework 3.5 を選択するか、ASP.NET MVC Web アプリケーション プロジェクト テンプレートは表示されません。


新しい MVC Web アプリケーション プロジェクトを作成するたびに Visual Studio では、個別の単体テスト プロジェクトを作成するように求められます。 図 2 ダイアログが表示されます。 いますしないテストは作成このチュートリアルでは時間的制約のためであり、そうする必要がありますだと少しこの) を選択、**いいえ**オプションを選択し、をクリックして、 **OK**ボタン。

> [!NOTE] 
> 
> Visual Web Developer では、テスト プロジェクトはサポートされません。


[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image2.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image3.png)

**図 02**:、単体テスト プロジェクトの作成 ダイアログ ([フルサイズの画像を表示する をクリックします](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image4.png))。


ASP.NET MVC アプリケーションが標準的な一連のフォルダー: モデル、ビュー、およびコント ローラーのフォルダー。 この標準的な一連のソリューション エクスプ ローラー ウィンドウでフォルダーを確認できます。 私たちは、映画データベース アプリケーションをビルドするにはそれぞれのモデル、ビュー、およびコント ローラーのフォルダーにファイルを追加する必要があります。

Visual Studio で新しい MVC アプリケーションを作成するときに、サンプル アプリケーションを取得します。 最初から開始するため、このサンプル アプリケーションのコンテンツを削除する必要があります。 次のファイルと、次のフォルダーを削除する必要があります。

- Controllers\HomeController.vb
- Views\Home

## <a name="creating-the-database"></a>データベースの作成

ムービー データベース レコードを保持するためにデータベースを作成する必要があります。 さいわい、Visual Studio には、SQL Server Express という名前のフリー データベースが含まれています。 データベースを作成する次の手順に従います。

1. アプリを右クリックして\_メニュー オプションを選択して、ソリューション エクスプ ローラー ウィンドウで、データ フォルダー**追加]、[新しい項目の**します。
2. 選択、**データ**カテゴリと選択、 **SQL Server データベース**テンプレート (図 3 を参照してください)。
3. 新しいデータベースの名前*MoviesDB.mdf*  をクリックし、**追加**ボタンをクリックします。

アプリにある MoviesDB.mdf ファイルをダブルクリックして、データベースに接続することができます、データベースを作成した後\_データ フォルダー。 MoviesDB.mdf ファイルをダブルクリックすると、サーバー エクスプ ローラー ウィンドウが開きます。

> [!NOTE] 
> 
> サーバー エクスプ ローラー ウィンドウには、Visual Web Developer の場合、データベース エクスプ ローラー ウィンドウがという名前です。


[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image3.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image5.png)

**図 03**: Microsoft SQL Server データベースを作成する ([フルサイズの画像を表示する をクリックします](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image6.png))。


次に、新しいデータベースのテーブルを作成する必要があります。 サーバー エクスプ ローラー ウィンドウ内でテーブル フォルダーを右クリックし、メニュー オプションを選択**新しいテーブルの追加**します。 このメニュー オプションを選択すると、データベースのテーブル デザイナーが開きます。 次のデータベース列を作成します。

<a id="0.2_table01"></a>


| **列名** | **データ型** | **Null を許容します。** |
| --- | --- | --- |
| ID | Int | False |
| Title | nvarchar (100) | False |
| ディレクター | nvarchar (100) | False |
| DateReleased | DateTime | False |


最初の列、Id 列には、2 つの特殊なプロパティがあります。 最初に、Id 列を主キー列としてマークする必要があります。 Id 列を選択すると、クリックして、**主キーの設定**(キーのようなアイコンは) ボタンをクリックします。 次に、Id 列が Id 列としてマークする必要があります。 列のプロパティ ウィンドウでは、Identity の指定 セクションまでスクロールしを展開します。 変更、 **Id**プロパティ値を**はい**します。 完了したら、図 4 ようテーブルになります。


[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image4.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image7.png)

**図 04**:、映画データベース テーブル ([フルサイズの画像を表示する をクリックします](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image8.png))。


最後の手順では、新しいテーブルを保存します。 保存ボタン (フロッピー ディスクのアイコン) をクリックし、新しいテーブル名の映画を提供します。

テーブルの作成が完了したら、ムービー レコードの一部をテーブルに追加します。 サーバー エクスプ ローラー ウィンドウで映画のテーブルを右クリックし、メニュー オプションを選択**テーブル データの表示**します。 (図 5 参照)、お気に入りの映画の一覧を入力します。


[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image5.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image9.png)

**図 05**: ムービー レコードの入力 ([フルサイズの画像を表示する をクリックします](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image10.png))。


## <a name="creating-the-model"></a>モデルを作成します。

次に、データベースを表すクラスのセットを作成する必要があります。 データベース モデルを作成する必要があります。 移動、データベース モデルのクラスを自動的に生成する Microsoft Entity Framework を活用します。

> [!NOTE] 
> 
> ASP.NET MVC フレームワークは、Microsoft Entity Framework には関連付けられません。 さまざまなオブジェクト リレーショナル マッピングを活用して、データベース モデル クラスを作成できます (または/M) LINQ to SQL、Subsonic、および NHibernate などのツール。


エンティティ データ モデル ウィザードを起動する次の手順に従います。

1. ソリューション エクスプ ローラー ウィンドウおよびメニュー オプションを選択して、モデル フォルダーを右クリックして**追加、新しい項目の**します。
2. 選択、**データ**カテゴリと選択、 **ADO.NET Entity Data Model**テンプレート。
3. データ モデルに名前を付けます*MoviesDBModel.edmx*  をクリックし、**追加**ボタンをクリックします。

[追加] ボタンをクリックした後、Entity Data Model ウィザードでは、(図 6 参照) が表示されます。 ウィザードの次の手順に従います。

1. **モデルのコンテンツの選択**手順で、**データベースから生成**オプション。
2. **データ接続の選択**ステップで、使用、 *MoviesDB.mdf*データ接続と名前*MoviesDBEntities*接続の設定。 をクリックして、**次**ボタンをクリックします。
3. **データベース オブジェクトの選択**ステップ、[テーブル] ノードを展開し、映画のテーブルを選択します。 名前空間を入力*MovieApp.Models*  をクリックし、**完了**ボタンをクリックします。


[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image6.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image11.png)

**図 06**: Entity Data Model ウィザードでデータベース モデルを生成する ([フルサイズの画像を表示する をクリックします](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image12.png))。


Entity Data Model ウィザードを完了すると、Entity Data Model のデザイナーが開きます。 デザイナーは、映画データベース テーブルを表示する必要があります (図 7 を参照してください)。


[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image7.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image13.png)

**図 07**: The Entity Data Model デザイナー ([フルサイズの画像を表示する をクリックします](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image14.png))。


続行する前に 1 つの変更を行う必要があります。 エンティティのデータ ウィザードでは、映画データベース テーブルを表すモデル クラスがムービーをという名前を生成します。 映画クラスを使用して、特定のムービーを表す、ためにクラスの名前を変更する必要があります*ムービー*の代わりに*映画*(単数形ではなく複数形)。

デザイナー画面で、クラスの名前をダブルクリックし、ムービーからムービーにクラスの名前を変更します。 この変更を加えたら、 をクリックして、**保存**ムービー クラスを生成する (フロッピー ディスクのアイコン) をクリックします。

## <a name="creating-the-aspnet-mvc-controller"></a>ASP.NET MVC コント ローラーの作成

次の手順では、ASP.NET MVC コント ローラーを作成します。 コント ローラーは、ユーザーが、ASP.NET MVC アプリケーションと対話する方法を制御する責任を負います。

この場合は、以下の手順に従ってください。

1. ソリューション エクスプ ローラー ウィンドウで、Controllers フォルダーを右クリックし、メニュー オプションを選択**追加、コント ローラー**します。
2. コント ローラーの追加 ダイアログ ボックスで、名前を入力します。 *HomeController*というラベルの付いたチェック ボックスをオンおよび**アクション メソッドの作成、更新、および詳細のシナリオを追加**(図 8 参照)。
3. をクリックして、**追加**をプロジェクトに新しいコント ローラーを追加するボタンをクリックします。

次の手順を完了すると、リスト 1 で、コント ローラーが作成されます。 インデックスの詳細は、作成、という名前のメソッドが含まれていることを確認および編集します。 次のセクションを使用するこれらのメソッドを取得するために必要なコードを追加します。


[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image8.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image15.png)

**図 08**: 新しい ASP.NET MVC コント ローラーの追加 ([フルサイズの画像を表示する をクリックします](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image16.png))。


**1 – Controllers\HomeController.vb を一覧表示します。**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample1.vb)]

## <a name="listing-database-records"></a>データベース レコードの一覧

Home コント ローラーの Index() メソッドは、ASP.NET MVC アプリケーションの既定の方法です。 ASP.NET MVC アプリケーションを実行すると、Index() メソッドが呼び出される最初のコント ローラー メソッドを使用します。

Index() メソッドを使用して、映画データベース テーブルからレコードの一覧を表示します。 データベースの利用 Index() メソッドを使用して、ムービーのデータベース レコードを取得する前に作成したモデル クラス移動します。

リスト 2 で HomeController クラスを変更しましたという名前の新しいプライベート フィールドが含まれるように\_db します。 MoviesDBEntities クラスは、データベース モデルを表し、このクラスを使用して、データベースと通信します。

リスト 2 で Index() メソッドも変更しました。 Index() メソッドでは、MoviesDBEntities クラスを使用して、映画データベース テーブルからすべてのムービーのレコードを取得します。 式 *\_db します。MovieSet.ToList()* 映画データベース テーブルからすべてのムービーのレコードの一覧を返します。

ムービーの一覧は、ビューに渡されます。 View() メソッドに渡されるものは、ビュー データとして、ビューに渡されます。

**2 – Controllers/HomeController.vb (変更された Index メソッド) を一覧表示します。**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample2.vb)]

Index() メソッドは、インデックスをという名前のビューを返します。 ムービー データベース レコードの一覧を表示するには、このビューを作成する必要があります。 この場合は、以下の手順に従ってください。


プロジェクトをビルドする必要があります (メニュー オプションを選択**ソリューションのビルドをビルドします**) 開く前に、**ビューの追加**ダイアログまたはクラスを持たないに表示されます、**データ クラスを表示**。ドロップダウン リスト。


1. コード エディターで Index() メソッドを右クリックし、メニュー オプションを選択**ビューの追加**(図 9 参照)。
2. ビューの追加 ダイアログ ボックスで、チェック ボックスをオンにというラベルが付いたことを確認します**厳密に型指定されたビューを作成する**がチェックされます。
3. **コンテンツを表示**ドロップダウン リストで、値を選択*一覧*します。
4. **データ クラスを表示**ドロップダウン リストで、値を選択して*MovieApp.Movie*します。
5. 新しいを作成する [追加] ボタンをクリックします (図 10 参照) を表示します。

次の手順を完了すると、Index.aspx をという名前の新しいビューは views \home フォルダーに追加されます。 リスト 3 では、インデックス ビューの内容が含まれます。


[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image9.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image17.png)

**図 09**: コント ローラー アクションからビューの追加 ([フルサイズの画像を表示する をクリックします](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image18.png))。


[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image10.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image19.png)

**図 10**: ビューの追加 ダイアログで新しいビューを作成する ([フルサイズの画像を表示する をクリックします](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image20.png))。


[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample3.aspx)]

インデックス ビューには、すべての HTML テーブル内の映画データベース テーブルからムービー レコードが表示されます。 ビューには、For が含まれています。 各 ViewData.Model プロパティで表される各ムービーを反復処理するループします。 F5 キーを押すことによって、アプリケーションを実行すると、図 11 では、web ページが表示されます。


[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image11.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image21.png)

**図 11**: The Index ビュー ([フルサイズの画像を表示する をクリックします](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image22.png))。


## <a name="creating-new-database-records"></a>新しいデータベース レコードを作成します。

前のセクションで作成したインデックス ビューには、新しいデータベース レコードを作成するためのリンクが含まれています。 みましょうロジックを実装および新しいムービーのデータベース レコードを作成するために必要なビューを作成します。

Home コント ローラーには、Create() という名前の 2 つのメソッドが含まれています。 最初の Create() メソッドにパラメーターがありません。 Create() メソッドのこのオーバー ロードを使用して、新しいムービーのデータベース レコードを作成するための HTML フォームを表示できます。

2 番目の Create() メソッドでは、FormCollection パラメーターがあります。 Create() メソッドのこのオーバー ロードは、新しいムービーを作成するための HTML フォームがサーバーに投稿されたときに呼び出されます。 この 2 つ目の Create() メソッドが AcceptVerbs 属性、メソッドが HTTP Post 操作を実行しない限り、呼び出されていることを防ぎますを持つことに注意してください。

この 2 つ目の Create() メソッドは、リスト 4 の更新された HomeController クラスに変更されています。 Create() メソッドの新しいバージョンは、ムービー パラメーターを受け取り、映画データベース テーブルに新しいビデオを挿入するためのロジックが含まれています。

> [!NOTE] 
> 
> Bind 属性に注意してください。 HTML フォームからムービー Id プロパティを更新するため、このプロパティを明示的に除外する必要があります。


**4 – Controllers\HomeController.vb (変更の作成方法) を一覧表示します。**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample4.vb)]

Visual Studio で簡単に新しいムービー データベースを作成するためのフォームを作成できます (図 12 を参照してください) を記録します。 この場合は、以下の手順に従ってください。

1. コード エディターで Create() メソッドを右クリックし、メニュー オプションを選択**ビューの追加**します。
2. チェック ボックスをオンにというラベルが付いたことを確認します。**厳密に型指定されたビューを作成する**がチェックされます。
3. **コンテンツを表示**ドロップダウン リストで、値を選択*作成*です。
4. **データ クラスを表示**ドロップダウン リストで、値を選択して*MovieApp.Movie*します。
5. をクリックして、**追加**新しいビューを作成するボタンをクリックします。


[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image12.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image23.png)

**図 12**: 作成ビューの追加 ([フルサイズの画像を表示する をクリックします](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image24.png))。


Visual Studio は、自動的にリスト 5 で、ビューを生成します。 このビューには、各ムービー クラスのプロパティに対応するフィールドを含む HTML フォームが含まれています。

**Listing 5 – Views\Home\Create.aspx**

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample5.aspx)]

> [!NOTE] 
> 
> ビューの追加 ダイアログで作成した HTML 形式では、フォームの Id フィールドを生成します。 Id 列が Id 列であるため、このフォームのフィールドは不要し、安全に削除することができます。


作成ビューを追加した後は、データベースに新しいムービーのレコードを追加できます。 F5 キーを押してアプリケーションを実行し、図 13 でフォームを新規作成 をクリックします。 完了し、フォームを送信する場合、新しいムービーのデータベース レコードが作成されます。

フォームの検証を自動的に取得することに注意してください。 、映画のリリース日を入力を怠ると、または無効なリリース日を入力する、、フォームが再表示し、リリース日フィールドが強調表示されます。


[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image13.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image25.png)

**図 13**: 新しいムービーのデータベース レコードを作成する ([フルサイズの画像を表示する をクリックします](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image26.png))。


## <a name="editing-existing-database-records"></a>既存のデータベース レコードの編集

前のセクションでは、一覧表示し、新しいデータベース レコードを作成する方法を説明しました。 この最後のセクションでは、既存のデータベース レコードを編集する方法について説明します。

最初に、編集フォームを生成する必要があります。 Visual Studio は私たちにとって編集フォームを自動的に生成されるため、この手順は簡単です。 Visual Studio コード エディターで HomeController.vb クラスを開き、次の手順に従います。

1. コード エディターで Edit() メソッドを右クリックし、メニュー オプションを選択**ビューの追加**(図 14 を参照してください)。
2. チェック ボックスをラベル付け**厳密に型指定されたビューを作成する**します。
3. **コンテンツを表示**ドロップダウン リストで、値を選択*編集*します。
4. **データ クラスを表示**ドロップダウン リストで、値を選択して*MovieApp.Movie*します。
5. をクリックして、**追加**新しいビューを作成するボタンをクリックします。

次の手順を完了するには、views \home フォルダーに Edit.aspx をという名前の新しいビューが追加されます。 このビューには、ムービーのレコードを編集するための HTML フォームが含まれています。


[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image14.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image27.png)

**図 14**: 編集ビューの追加 ([フルサイズの画像を表示する をクリックします](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image28.png))。


> [!NOTE] 
> 
> 編集ビューには、映画 Id プロパティに対応する HTML フォーム フィールドが含まれています。 Id プロパティの値を編集しているユーザーをしたくないため、このフォームのフィールドを削除する必要があります。


最後に、データベース レコードの編集をサポートするように、Home コント ローラーを変更する必要があります。 更新された HomeController クラスは、6 の一覧に含まれます。

**6 – Controllers\HomeController.vb (Edit メソッド) を一覧表示します。**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample6.vb)]

リストの 6 で追加のロジック Edit() メソッドの両方のオーバー ロードを追加しました。 最初の Edit() メソッドは、メソッドに渡される Id パラメーターに対応するムービー データベース レコードを返します。 2 番目のオーバー ロードは、データベースのムービーのレコードに更新プログラムを実行します。

元のビデオを取得して呼び出して ApplyPropertyChanges()、データベース内の既存のムービーを更新する必要がありますに注意してください。

## <a name="summary"></a>まとめ

このチュートリアルの目的は、ASP.NET MVC アプリケーションを構築するためのエクスペリエンスの感覚をつかむことにしました。 ASP.NET MVC web アプリケーションを構築するが、Active Server Pages または ASP.NET アプリケーションの開発エクスペリエンスのとよく似ていますが検出されたと思います。

このチュートリアルでは、ASP.NET MVC フレームワークの最も基本的な機能のみを調査します。 今後のチュートリアルでは、私たちをさらに深くコント ローラー、コント ローラー アクション、ビュー、データの表示、および HTML ヘルパーなどのトピックです。

> [!div class="step-by-step"]
> [前へ](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs.md)
