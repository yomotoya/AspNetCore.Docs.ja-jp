---
uid: mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb
title: "ASP.NET MVC (VB) で 15 分以内にムービー データベース アプリケーションを作成 |Microsoft ドキュメント"
author: StephenWalther
description: "Stephen Walther は、データベースによる ASP.NET MVC アプリケーション全体が開始されてから完了するを構築します。 このチュートリアルでは新しい t のあるユーザーのための優れた紹介しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: e4ba9786-734c-4eb3-91bb-089793325d0d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb
msc.type: authoredcontent
ms.openlocfilehash: b87a69df24a410161dfaf055519eb6137fa76c06
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="create-a-movie-database-application-in-15-minutes-with-aspnet-mvc-vb"></a>ASP.NET MVC (VB) で 15 分以内にムービー データベース アプリケーションを作成します。
====================
によって[Stephen Walther](https://github.com/StephenWalther)

[コードをダウンロードします。](http://download.microsoft.com/download/7/2/8/728F8794-E59A-4D18-9A56-7AD2DB05BD9D/MovieApp_VB.zip)

> Stephen Walther は、データベースによる ASP.NET MVC アプリケーション全体が開始されてから完了するを構築します。 このチュートリアルは、新しい ASP.NET MVC フレームワークにし、ASP.NET MVC アプリケーションを構築するプロセスを把握したいユーザーの導入にも最適です。


このチュートリアルの目的は、「は、」の感を与える、ASP.NET MVC アプリケーションをビルドします。 このチュートリアルでは、ASP.NET MVC アプリケーション全体が開始されてから完了する構築をすれば稼動です。 どのようにすることができますを一覧表示、作成、およびデータベース レコードを編集について説明する簡単なデータベースに基づくアプリケーションをビルドする方法を示します。

アプリケーションを構築するプロセスを簡略化は、Visual Studio 2008 のスキャフォールディング機能の利点をみましょう。 Visual Studio によって、最初のコードと、コント ローラー、モデル、およびビューのコンテンツの生成をお知らせします。

Active Server Pages または ASP.NET を使用する場合、しはず ASP.NET MVC 非常になじみです。 ASP.NET MVC ビューは、Active Server Pages アプリケーションのページとよく似ています。 また、従来の ASP.NET Web フォーム アプリケーションと同じように ASP.NET MVC での言語と .NET framework が提供するクラスの豊富なセットへのフル アクセス。

自分の望みは、このチュートリアルがどのようにの ASP.NET MVC アプリケーションの構築のエクスペリエンスと同様と、Active Server Pages または ASP.NET Web フォーム アプリケーションのビルドのエクスペリエンスとは異なる意味します。

## <a name="overview-of-the-movie-database-application"></a>ムービーのデータベース アプリケーションの概要

目標は簡単であるため、非常に単純なムービー データベース アプリケーションはビルドします。 簡単なムービー データベース アプリケーションでは次の 3 つの作業を行うことを許可します。

1. ムービーのデータベース レコードのセットを一覧表示します。
2. 新しいムービーのデータベース レコードを作成します。
3. ムービーの既存のデータベース レコードを編集します。

もう一度、するために複雑にならない、利点は、アプリケーションの構築に必要な ASP.NET MVC フレームワークの機能の最小数の移動します。 たとえば、おされませんする活用テスト駆動型開発。

アプリケーションを作成するのには、それぞれ、次の手順を完了する必要があります。

1. ASP.NET MVC Web アプリケーション プロジェクトを作成します。
2. データベースを作成します。
3. データベース モデルを作成します。
4. ASP.NET MVC コント ローラーを作成します。
5. ASP.NET MVC ビューを作成します。

## <a name="preliminaries"></a>準備

Visual Studio 2008 または Visual Web Developer 2008 Express ASP.NET MVC アプリケーションを構築する必要があります。 また、ASP.NET MVC フレームワークをダウンロードする必要があります。

Visual Studio 2008 を所有していない場合は、この web サイトから、Visual Studio 2008 の 90 日間試用版をダウンロードできます。

[https://msdn.microsoft.com/vs2008/products/cc268305.aspx](https://msdn.microsoft.com/vs2008/products/cc268305.aspx)

または、作成できます ASP.NET MVC アプリケーションを Visual Web Developer Express 2008 で。 Visual Web Developer Express を使用すると、Service Pack 1 がインストールされているが必要です。 Visual Web Developer 2008 Express with Service Pack 1 は、この web サイトからダウンロードできます。

[https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang=en](https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang=en)

Visual Studio 2008 または Visual Web Developer 2008 のいずれかをインストールした後は、ASP.NET MVC フレームワークをインストールする必要があります。 ASP.NET MVC フレームワークは、次の web サイトからダウンロードできます。

[https://www.asp.net/mvc/](../../../index.md)

> [!NOTE] 
> 
> ASP.NET フレームワークと ASP.NET MVC フレームワークを個別にダウンロードする代わりに、Web Platform Installer の利点を実行できます。 Web Platform Installer は、アプリケーションを簡単にインストールされているアプリケーションを管理することができますが、コンピューターです。
> 
> [https://www.microsoft.com/web/gallery/Install.aspx](https://www.microsoft.com/web/gallery/Install.aspx)


## <a name="creating-an-aspnet-mvc-web-application-project"></a>ASP.NET MVC Web アプリケーション プロジェクトを作成します。

Visual Studio 2008 で新しい ASP.NET MVC Web アプリケーション プロジェクトを作成して開始しましょう。 メニュー オプションを選択**ファイル、新しいプロジェクト**図 1 に新しいプロジェクト ダイアログ ボックスが表示されます。 プログラミング言語と Visual Basic を選択し、ASP.NET MVC Web アプリケーション プロジェクト テンプレートを選択します。 プロジェクト MovieApp 名前を付け、[ok] ボタンをクリックします。


[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image1.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image1.png)

**図 01**: [新しいプロジェクト] ダイアログ ボックス ([フルサイズのイメージを表示するをクリックして](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image2.png))


新しいプロジェクト ダイアログの上部にあるドロップダウン リストから .NET Framework 3.5 を選択するか、ASP.NET MVC Web アプリケーション プロジェクト テンプレートが表示されないことを確認してください。


新しい MVC Web アプリケーション プロジェクトを作成するときに Visual Studio では、個別の単体テスト プロジェクトを作成するように求められます。 図 2 では、ダイアログが表示されます。 おされませんするテストの作成このチュートリアルでは時間の制約により、はい、必要がありますだと考えてについて少しまま) を選択、**いいえ**オプションをクリックして、 **ok**ボタンをクリックします。

> [!NOTE] 
> 
> Visual Web Developer は、テスト プロジェクトをサポートしていません。


[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image2.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image3.png)

**図 02**:、単体テスト プロジェクトの作成 ダイアログ ([フルサイズのイメージを表示するをクリックして](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image4.png))


ASP.NET MVC アプリケーションが標準的な一連のフォルダー: モデル、ビュー、およびコント ローラーのフォルダーです。 この標準的な一連のソリューション エクスプ ローラー ウィンドウでフォルダーを表示できます。 ファイルを追加するそれぞれのモデル、ビュー、およびコント ローラーのフォルダーに、ムービーのデータベース アプリケーションを構築するために必要になります。

Visual Studio で新しい MVC アプリケーションを作成するときに、サンプル アプリケーションを取得します。 最初から開始したいのでこのサンプル アプリケーションのコンテンツを削除する必要があります。 次のファイルと、次のフォルダーを削除する必要があります。

- Controllers\HomeController.vb
- Views\Home

## <a name="creating-the-database"></a>データベースの作成

ムービー データベース レコードを保持するためにデータベースを作成する必要があります。 幸運にも、Visual Studio には、SQL Server Express という名前のフリー データベースが含まれています。 データベースを作成する手順に従います。

1. アプリケーションを右クリックして\_メニュー オプションを選択し、ソリューション エクスプ ローラーのウィンドウで、データ フォルダー**追加]、[新しい項目の**します。
2. 選択、**データ**カテゴリを選択、 **SQL Server データベース**テンプレート (図 3 を参照してください)。
3. 新しいデータベースの名前を付けます*MoviesDB.mdf*  をクリックし、**追加**ボタンをクリックします。

アプリにある MoviesDB.mdf ファイルをダブルクリックして、データベースに接続することができます、データベースを作成した後\_データ フォルダーです。 サーバー エクスプ ローラー ウィンドウを開きます。 この MoviesDB.mdf ファイルをダブルクリックします。

> [!NOTE] 
> 
> サーバー エクスプ ローラー ウィンドウには、Visual Web Developer の場合、データベース エクスプ ローラー ウィンドウがという名前です。


[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image3.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image5.png)

**図 03**: Microsoft SQL Server データベースの作成 ([フルサイズのイメージを表示するをクリックして](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image6.png))


次に、新しいデータベース テーブルを作成する必要があります。 サーバー エクスプ ローラー ウィンドウ内でテーブルのフォルダーを右クリックし、メニュー オプションを選択**新しいテーブルの追加**です。 このメニュー オプションを選択すると、データベースのテーブル デザイナーが開きます。 次のデータベース列を作成します。

<a id="0.2_table01"></a>


| **列名** | **データ型** | **Null を許容します。** |
| --- | --- | --- |
| ID | Int | False |
| Title | Nvarchar(100) | False |
| ディレクター | Nvarchar(100) | False |
| DateReleased | DateTime | False |


最初の列、Id 列には、次の 2 つの特殊なプロパティがあります。 最初に、Id 列を主キー列としてマークする必要があります。 Id 列を選択するをクリックして、**主キーの設定**(キーのようなアイコンでは) ボタンをクリックします。 次に、Id 列、Id 列としてマークする必要があります。 列のプロパティ ウィンドウで Identity の指定 セクションまで下にスクロールし、それを展開します。 変更、 **Is Identity**プロパティ値を**はい**です。 完了したら、表に、図 4 のようになります。


[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image4.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image7.png)

**図 04**:、映画データベース テーブル ([フルサイズのイメージを表示するをクリックして](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image8.png))


最後の手順では、新しいテーブルを保存します。 [保存] ボタン (フロッピー ディスクのアイコン) をクリックし、新しいテーブル名の映画を提供します。

テーブルの作成が完了したら、ムービー レコードの一部をテーブルに追加します。 サーバー エクスプ ローラー ウィンドウで、ムービーのテーブルを右クリックし、メニュー オプションを選択**テーブル データの表示**です。 (図 5 を参照してください)、お気に入りの映画の一覧を入力します。


[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image5.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image9.png)

**図 05**: ムービー レコードの入力 ([フルサイズのイメージを表示するをクリックして](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image10.png))


## <a name="creating-the-model"></a>モデルの作成

次に、データベースを表すクラスのセットを作成する必要があります。 データベース モデルを作成する必要があります。 このデータベース モデルのクラスを自動的に生成する Microsoft Entity Framework を活用をみましょう。

> [!NOTE] 
> 
> ASP.NET MVC フレームワークは、Microsoft の Entity Framework に関連付けられていません。 さまざまなオブジェクト リレーショナル マッピングを活用して、データベース モデル クラスを作成できます (または/M) LINQ to SQL、Subsonic、および NHibernate などのツールです。


Entity Data Model ウィザードを起動する次の手順に従います。

1. ソリューション エクスプ ローラー ウィンドウのオプションを選択し、メニューの モデル フォルダーを右クリックして**追加、新しい項目の**します。
2. 選択、**データ**カテゴリを選択、 **ADO.NET エンティティ データ モデル**テンプレート。
3. データ モデルに名前を付ける*MoviesDBModel.edmx*  をクリックし、**追加**ボタンをクリックします。

Entity Data Model ウィザードでは、[追加] ボタンをクリックした後、(図 6 を参照) が表示されます。 ウィザードを完了する手順に従います。

1. **モデルのコンテンツ**手順、select、**データベースから生成**オプション。
2. **データ接続の選択**ステップを使用して、 *MoviesDB.mdf*データ接続と名前*MoviesDBEntities*接続の設定。 クリックして、**次**ボタンをクリックします。
3. **データベース オブジェクトの選択**ステップ、[テーブル] ノードを展開し、映画のテーブルを選択します。 名前空間を入力*MovieApp.Models*  をクリックし、**完了**ボタンをクリックします。


[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image6.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image11.png)

**図 06**: Entity Data Model ウィザードでデータベース モデルの生成 ([フルサイズのイメージを表示するをクリックして](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image12.png))


Entity Data Model ウィザードを完了すると、Entity Data Model デザイナーが開きます。 映画データベース テーブルをデザイナーに表示する必要があります (図 7 を参照してください)。


[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image7.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image13.png)

**図 07**: The Entity Data Model Designer ([フルサイズのイメージを表示するをクリックして](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image14.png))


続行する前に、1 つの変更を行う必要があります。 エンティティ データ ウィザードでは、ムービーのデータベース テーブルを表すモデル クラス映画をという名前を生成します。 映画クラスを使用して、特定のムービーを表す、ためにクラスの名前を変更する必要があります*ムービー*の代わりに*映画*(単数形ではなく複数形)。

デザイナー画面にクラスの名前をダブルクリックし、ムービーからムービーにクラスの名前を変更します。 この変更を加えたらをクリックして、**保存**ムービー クラスを生成する (フロッピー ディスクのアイコン) ボタンをクリックします。

## <a name="creating-the-aspnet-mvc-controller"></a>ASP.NET MVC コント ローラーを作成します。

次の手順では、ASP.NET MVC コント ローラーを作成します。 コント ローラーは、ユーザーが、ASP.NET MVC アプリケーションと対話する方法を制御します。

この場合は、以下の手順に従ってください。

1. ソリューション エクスプ ローラー ウィンドウで、Controllers フォルダーを右クリックし、メニュー オプションを選択**追加、コント ローラー**です。
2. コント ローラーの追加 ダイアログ ボックスで、名前を入力してください。 *HomeController*  チェック ボックスを確認および**アクション メソッドの作成、更新、および詳細シナリオを追加**(図 8 を参照してください)。
3. をクリックして、**追加**をプロジェクトに、新しいコント ローラーを追加するボタンをクリックします。

次の手順を完了すると、リスト 1 のコント ローラーが作成されます。 インデックス、詳細については、作成、という名前のメソッドが含まれていることを確認および編集します。 次のセクションでは、作業にこれらのメソッドを取得するために必要なコードを追加します。


[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image8.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image15.png)

**図 08**: 新しい ASP.NET MVC コント ローラーの追加 ([フルサイズのイメージを表示するをクリックして](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image16.png))


**1 – Controllers\HomeController.vb を一覧表示します。**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample1.vb)]

## <a name="listing-database-records"></a>データベース レコードの一覧

Home コント ローラーの Index() メソッドは、ASP.NET MVC アプリケーションの既定のメソッドです。 ASP.NET MVC アプリケーションを実行すると、Index() メソッドが呼び出される最初のコント ローラー メソッドを使用します。

Index() メソッドを使用して、ムービーのデータベース テーブルからレコードの一覧を表示します。 データベースの利用 Index() メソッドを使用して、ムービーのデータベース レコードを取得する前に作成したモデル クラス移動します。

という名前の新しいプライベート フィールドが含まれているように 2 の一覧でテンプレートを使用するクラスを変更しました\_db します。 MoviesDBEntities クラスは、データベース モデルを表し、このクラスを使用して、データベースと通信します。

また、Index() メソッドを一覧表示する 2 を変更しました。 Index() メソッドでは、MoviesDBEntities クラスを使用して、映画データベース テーブルからすべてのムービーのレコードを取得します。 式 *\_db します。MovieSet.ToList()*映画データベース テーブルからすべてのムービーのレコードの一覧を返します。

ムービーの一覧は、ビューに渡されます。 データの表示と、ビューに渡さ View() メソッドに渡されたものを取得します。

**2 – Controllers/HomeController.vb (修飾されているインデックス メソッド) を一覧表示します。**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample2.vb)]

Index() メソッドでは、インデックスをという名前のビューを返します。 ムービー データベース レコードの一覧を表示するには、このビューを作成する必要があります。 この場合は、以下の手順に従ってください。


プロジェクトをビルドする必要があります (メニュー オプションを選択**ビルド、ソリューションのビルド**) 開く前に、**ビューの追加**ダイアログまたはクラスを持たないに表示されます、**データ クラスを表示**ドロップダウン リスト。


1. コード エディターで Index() メソッドを右クリックし、メニュー オプションを選択**ビューの追加**(図 9 を参照してください)。
2. ビューの追加 ダイアログ ボックスで確認するチェック ボックス ラベルの付いた**厳密に型指定されたビューを作成する**がオンになっています。
3. **コンテンツを表示** ドロップダウン リストで、値を選択*リスト*です。
4. **データ クラスを表示** ドロップダウン リストで、値を選択*MovieApp.Movie*です。
5. 新しいを作成する追加のボタンをクリックして (図 10 参照) を表示します。

次の手順を完了すると、Index.aspx をという名前の新しいビューは、views \home フォルダーに追加されます。 3 の一覧表示するのには、インデックス ビューの内容が含まれています。


[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image9.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image17.png)

**図 09**: コント ローラーのアクションからビューの追加 ([フルサイズのイメージを表示するをクリックして](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image18.png))


[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image10.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image19.png)

**図 10**: ビューの追加 ダイアログで、新しいビューを作成する ([フルサイズのイメージを表示するをクリックして](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image20.png))


[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample3.aspx)]

インデックス ビューには、すべての HTML テーブル内のムービーのデータベース テーブルからムービー レコードが表示されます。 ビューに含まれる For ViewData.Model プロパティで表される各ムービーを反復処理する各ループします。 F5 キーを押すことによって、アプリケーションを実行すると、図 11 では、web ページが表示されます。


[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image11.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image21.png)

**図 11**:、インデックス ビュー ([フルサイズのイメージを表示するをクリックして](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image22.png))


## <a name="creating-new-database-records"></a>新しいデータベース レコードを作成します。

前のセクションで作成したインデックス ビューには、新しいデータベース レコードを作成するためのリンクが含まれています。 では早速しロジックを実装して、新しいムービーのデータベース レコードを作成するために必要なビューを作成します。

Home コント ローラーには、Create() という名前の 2 つのメソッドが含まれています。 最初の Create() メソッドにパラメーターがありません。 この Create() メソッドのオーバー ロードを使用して、新しいムービーのデータベース レコードを作成するための HTML フォームを表示します。

2 番目の Create() メソッドには、FormCollection パラメーターがあります。 この Create() メソッドのオーバー ロードは、新しいムービーを作成するための HTML フォームがサーバーにポストされたときに呼び出されます。 この 2 つ目の Create() メソッドが AcceptVerbs 属性、メソッドが HTTP Post 操作を実行しない限り、呼び出されることを防ぎますを持つことに注意してください。

この 2 つ目の Create() メソッドは、更新されたテンプレートを使用するクラスを一覧表示する 4 で変更されています。 新しいバージョンの Create() メソッドは、ムービー パラメーターを確定し、映画データベース テーブルに新しいムービーを挿入するためのロジックが含まれています。

> [!NOTE] 
> 
> Bind 属性に注意してください。 HTML フォームからムービー Id プロパティを更新したくありません、ためこのプロパティを明示的に除外する必要があります。


**4 – Controllers\HomeController.vb (修飾されている作成メソッド) を一覧表示します。**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample4.vb)]

Visual Studio では、新しいムービーのデータベースを作成するためのフォームを作成する簡単 (図 12 を参照してください) を記録します。 この場合は、以下の手順に従ってください。

1. コード エディターで Create() メソッドを右クリックし、メニュー オプションを選択**ビューの追加**です。
2. チェック ボックスがラベルを確認してください**厳密に型指定されたビューを作成**がオンになっています。
3. **コンテンツを表示** ドロップダウン リストで、値を選択*作成*です。
4. **データ クラスを表示** ドロップダウン リストで、値を選択*MovieApp.Movie*です。
5. クリックして、**追加**クリックすると、新しいビューを作成します。


[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image12.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image23.png)

**図 12**: 作成ビューの追加 ([フルサイズのイメージを表示するをクリックして](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image24.png))


Visual Studio は、自動的に 5 を一覧表示するビューを生成します。 このビューには、各ムービー クラスのプロパティに対応するフィールドを含む、HTML フォームが含まれています。

**Listing 5 – Views\Home\Create.aspx**

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample5.aspx)]

> [!NOTE] 
> 
> ビューの追加 ダイアログで生成された HTML フォームには、Id フォーム フィールドが生成されます。 Id 列は Id 列であるため、このフォームのフィールドは不要し、安全に削除することができます。


作成ビューを追加した後は、データベースに新しいムービーのレコードを追加できます。 F5 キーを押してアプリケーションを実行し、図 13 で実行されたフォームを新規作成 をクリックします。 完了し、フォームを送信すると、新しいムービーのデータベース レコードが作成されます。

フォーム検証を自動的に取得することに注意してください。 無視すると、ムービーのリリース日付を入力するか、無効なリリース日を入力する、フォームが再表示し、リリース日 フィールドが強調表示されます。


[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image13.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image25.png)

**図 13**: 新しいムービーのデータベース レコードを作成する ([フルサイズのイメージを表示するをクリックして](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image26.png))


## <a name="editing-existing-database-records"></a>既存のデータベース レコードの編集

前のセクションで、一覧表示し、新しいデータベース レコードを作成する方法を説明します。 この最後のセクションでは、既存のデータベース レコードを編集する方法について説明します。

最初に、編集フォームを作成する必要があります。 Visual Studio はご利用の米国編集用のフォームを自動的に生成されるため、この手順は簡単です。 Visual Studio コード エディターで HomeController.vb クラスを開き、これらの手順に従います。

1. コード エディターで Edit() メソッドを右クリックし、メニュー オプションを選択**ビューの追加**(図 14 を参照してください)。
2. チェック ボックス ラベルの付いた**厳密に型指定されたビューを作成する**です。
3. **コンテンツを表示** ドロップダウン リストで、値を選択*編集*です。
4. **データ クラスを表示** ドロップダウン リストで、値を選択*MovieApp.Movie*です。
5. クリックして、**追加**クリックすると、新しいビューを作成します。

Views \home フォルダーに Edit.aspx をという名前の新しいビューを追加するこれらの手順を完了します。 このビューには、ムービーのレコードを編集するため、HTML フォームが含まれています。


[![[新しいプロジェクト] ダイアログ ボックス](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image14.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image27.png)

**図 14**: エディット ビューの追加 ([フルサイズのイメージを表示するをクリックして](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image28.png))


> [!NOTE] 
> 
> 編集ビューには、ムービーの Id プロパティに対応する HTML フォーム フィールドが含まれています。 Id プロパティの値を編集しているユーザーを含めない場合は、あるために、このフォームのフィールドを削除する必要があります。


最後に、データベース レコードの編集をサポートするように、Home コント ローラーを変更する必要があります。 更新後のテンプレートを使用するクラスは、6 の一覧に含まれます。

**6 – Controllers\HomeController.vb (編集メソッド) を一覧表示します。**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample6.vb)]

リストの 6 Edit() メソッドの両方のオーバー ロードする追加のロジックを追加しました。 最初の Edit() メソッドは、メソッドに渡される Id パラメーターに対応するムービー データベース レコードを返します。 2 番目のオーバー ロードは、データベースのムービーのレコードに更新プログラムを実行します。

オリジナルのムービーを取得し、データベース内の既存のムービーを更新する、ApplyPropertyChanges() を呼び出す必要がありますに注意してください。

## <a name="summary"></a>まとめ

このチュートリアルの目的は、ASP.NET MVC アプリケーションの構築のエクスペリエンスの感を与えるでした。 ASP.NET MVC web アプリケーションのビルドが、Active Server Pages または ASP.NET アプリケーションの構築のエクスペリエンスとよく似ていますを検出することを願っています。

このチュートリアルでは、ASP.NET MVC フレームワークの最も基本的な機能だけを検査します。 今後のチュートリアルでは、ここをさらに深くコント ローラー、コント ローラーのアクション、ビュー、データの表示、および HTML ヘルパーなどのトピックです。

>[!div class="step-by-step"]
[前へ](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs.md)
