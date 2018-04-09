---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
title: データ アクセス層を作成 |Microsoft ドキュメント
author: Erikre
description: このチュートリアルの系列では、お用 ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 0bbf7a6e-d7eb-4091-91e4-fff892777f32
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
msc.type: authoredcontent
ms.openlocfilehash: 671d1bbf661dfb3e56c6ccd67ce0d383990918d6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="create-the-data-access-layer"></a>データ アクセス層を作成します。
====================
によって[Erik Reitan](https://github.com/Erikre)

[Wingtip Toys のサンプル プロジェクト (c#) をダウンロード](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)または[電子書籍 (PDF) のダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> このチュートリアルの系列では、Web 用 ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明します。 Visual Studio 2013[プロジェクトと c# ソース コード](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)はこのチュートリアルのシリーズに付随する使用できます。


このチュートリアルでは、作成、アクセス、および ASP.NET Web フォームと Entity Framework Code First を使用してデータベースからデータを確認する方法について説明します。 このチュートリアルでは、前のチュートリアルでは、「プロジェクトを作成する」を上に構築され、Wingtip Toy ストア チュートリアル シリーズの一部です。 内にあるデータ アクセス クラスのグループが作成した、このチュートリアルを完了したときに、*モデル*プロジェクトのフォルダーです。

## <a name="what-youll-learn"></a>学習する内容。

- データ モデルを作成する方法。
- 初期化し、データベースのシード方法。
- 更新、およびデータベースをサポートするアプリケーションを構成する方法。

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>これらは、このチュートリアルで導入された機能です。

- Entity Framework Code First
- LocalDB
- データの注釈

## <a name="creating-the-data-models"></a>データ モデルを作成します。

[Entity Framework](https://msdn.microsoft.com/data/aa937723)オブジェクト リレーショナル マッピング (ORM) フレームワークがします。 オブジェクトを記述する必要は通常のデータ アクセス コードの大部分を排除することと、リレーショナル データを操作できます。 Entity Framework を使用して、使用してクエリを発行する[LINQ](https://msdn.microsoft.com/library/bb397926.aspx)、取得し、厳密に型指定されたオブジェクトとしてデータを操作します。 LINQ は、クエリを実行して、データを更新するためのパターンを提供します。 Entity Framework を使用するに焦点を当てたデータ アクセスの基本のではなく、アプリケーションの残りの部分を作成するのに集中することができます。 データを使用して、ナビゲーションおよび製品のクエリを作成する方法表示は、このチュートリアル シリーズの後半。

Entity Framework と呼ばれる開発パラダイムをサポートしている*Code First*です。 最初のコードではクラスを使用して、データ モデルを定義することができます。 クラスは、コンストラクトの他の型、メソッドおよびイベント変数をグループ化して、独自のカスタム型を作成することができます。 クラスを既存のデータベースにマップしたり、それらを使用してデータベースを生成できます。 このチュートリアルでは、データ モデル クラスを記述してデータ モデルを作成します。 次に、これらの新しいクラスから実行時にデータベースを作成する Entity Framework を使用するからお知らせします。

Web フォーム アプリケーションのデータ モデルを定義するエンティティ クラスを作成することで、開始されます。 その後エンティティ クラスを管理し、データベースへのデータ アクセスを提供するコンテキスト クラスが作成されます。 データベースの設定に使用する初期化子クラスも作成します。

### <a name="entity-framework-and-references"></a>Entity Framework と参照

既定では、Entity Framework が含まれる新規に作成するときに**ASP.NET Web アプリケーション**を使用して、 **Web フォーム**テンプレート。 Entity Framework のインストール、アンインストールすると、して NuGet パッケージとして更新できます。

この NuGet パッケージには、次が含まれています。**ランタイム**プロジェクト内のアセンブリ。

- EntityFramework.dll: Entity Framework で使用されるすべての一般的なランタイム コード
- EntityFramework.SqlServer.dll: Entity Framework 用 Microsoft SQL Server プロバイダー

### <a name="entity-classes"></a>エンティティ クラス

クラスを作成することをデータのスキーマを定義するのには、エンティティ クラスと呼ばれます。 初めて使用するデータベースの設計する場合は、エンティティ クラスと考える、データベースのテーブルの定義。 クラス内の各プロパティは、データベースのテーブルで列を指定します。 これらのクラスは、オブジェクト指向のコードと、データベースのリレーショナル テーブル構造の間で軽量のオブジェクト リレーショナル インターフェイスを提供します。

このチュートリアルでは、まず製品および分類のスキーマを表す単純なエンティティ クラスを追加することによりします。 製品クラスは、各製品の定義が格納されます。 各 product クラスのメンバーの名前になります`ProductID`、 `ProductName`、 `Description`、 `ImagePath`、 `UnitPrice`、 `CategoryID`、および`Category`です。 カテゴリのクラスは、製品に属する、車、ボート、平面など各カテゴリの定義が格納されます。 各カテゴリのクラスのメンバーの名前になります`CategoryID`、 `CategoryName`、 `Description`、および`Products`です。 各製品カテゴリのいずれかに属します。 これらのエンティティ クラスは、既存のプロジェクトに追加されます*モデル*フォルダーです。

1. **ソリューション エクスプ ローラー**を右クリックし、*モデル*クリックしてフォルダー**追加** - &gt; **新しい項目の**します。 

    ![データ アクセス レイヤーでの新しいメニュー項目の作成します。](create_the_data_access_layer/_static/image1.png)

   **[新しい項目の追加]** ダイアログ ボックスが表示されます。
2. **Visual c#**から、**インストール**左側のウィンドウ、**コード**です。 

    ![データ アクセス レイヤーでの新しいメニュー項目の作成します。](create_the_data_access_layer/_static/image2.png)
3. 選択**クラス**中央のペインからこの新しいクラスの名前と*Product.cs*です。
4. **[追加]** をクリックします。  
   新しいクラス ファイルがエディターで表示されます。
5. 既定のコードを次のコードに置き換えます。   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample1.cs)]
6. ただし、名前、新しいクラスを手順 1. ~ 4. を繰り返して、別のクラスを作成する*Category.cs*し、既定のコードを次のコードに置き換えます。  

    [!code-csharp[Main](create_the_data_access_layer/samples/sample2.cs)]

既に触れましたが、`Category`クラスを表しますが、アプリケーションは、製品の種類に販売するために設計されています (など<a id="a"> </a>&quot;車&quot;、&quot;ボート&quot;、 &quot;ロケット&quot;など)、および`Product`クラスは、データベース内の個々 の製品 (toys) を表します。 各インスタンス、`Product`オブジェクトは、リレーショナル データベース テーブル内の行に対応しているし、Product クラスの各プロパティは、リレーショナル データベース テーブル内の列にマップされます。 このチュートリアルの後半では、データベースに含まれる製品データを確認します。

### <a name="data-annotations"></a>データの注釈

お気付きクラスの特定のメンバーがあるなど、メンバーに関する詳細を指定する属性`[ScaffoldColumn(false)]`です。 これらは*データ注釈*です。 データの注釈属性は、書式を指定する方法には、モデル化、データベースの作成時に指定して、そのメンバーのユーザー入力を検証する方法を記述できます。

### <a name="context-class"></a>Context クラス

データ アクセスのため、クラスの使用を開始するには、コンテキスト クラスを定義する必要があります。 コンテキスト クラスは、エンティティ クラスを管理前述のように、(など、`Product`クラスおよび`Category`クラス) し、データベースへのデータ アクセスを提供します。

このプロシージャは、新しい c# するコンテキスト クラスを追加、*モデル*フォルダーです。

1. 右クリックし、*モデル*クリックしてフォルダー**追加** - &gt; **新しい項目の**します。   
   **[新しい項目の追加]** ダイアログ ボックスが表示されます。
2. 選択**クラス**に中央のペインから名前を付けます*ProductContext.cs*  をクリック**追加**です。
3. 次のコードでクラスに含まれる既定のコードに置き換えます。   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample3.cs)]

このコードを追加、`System.Data.Entity`名前空間をクエリする機能を含む、Entity Framework のすべてのコア機能にアクセスできるため、挿入、更新、および厳密に型指定されたオブジェクトを使用してデータを削除します。

`ProductContext`クラスを表します。 Entity Framework 製品のデータベース コンテキストのフェッチ、保存、および更新を処理する`Product`クラスのインスタンス、データベースにします。 `ProductContext`から派生したクラス、`DbContext`基本 Entity Framework によって提供されるクラスです。

### <a name="initializer-class"></a>初期化子のクラス

最初のデータベース コンテキストを使用するときに初期化するためにいくつかのカスタム ロジックを実行する必要があります。 これにより、製品と分類をすぐに表示できるように、データベースに追加するデータをシードが許可されます。

この手順で、新しい c# 初期化子のクラスを追加、*モデル*フォルダーです。

1. 新しいインスタンスを作成`Class`で、*モデル*フォルダーし名前を付けます*ProductDatabaseInitializer.cs*です。
2. 次のコードでクラスに含まれる既定のコードに置き換えます。   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample4.cs)]

データベースが作成され、初期化時に、上記のコードからご覧、`Seed`プロパティがオーバーライドされ、設定します。 ときに、`Seed`プロパティが設定されて、カテゴリおよび製品からの値が、データベースの作成に使用されます。 データベースが作成された後に、上記のコードを変更することによってシードのデータを更新しようとする場合に、Web アプリケーションを実行するときに、すべての更新が表示されません。 その理由は、上記のコードの実装を使用して、`DropCreateDatabaseIfModelChanges`シード データをリセットする前に、モデル (スキーマ) が変更されたかどうかを認識するクラス。 変更が作成されていない場合、`Category`と`Product`シード データ エンティティ クラスは、データベースが再初期化されません。

> [!NOTE] 
> 
> アプリケーションを実行するたびに再作成するデータベースの場合は、使用する可能性があります、`DropCreateDatabaseAlways`クラスの代わりに、`DropCreateDatabaseIfModelChanges`クラスです。 ただしこのチュートリアルの系列を使用して、`DropCreateDatabaseIfModelChanges`クラスです。


この時点でこのチュートリアルでは、*モデル*4 つの新しいクラスとクラスの 1 つの既定のフォルダー。

![データ アクセス層のモデル フォルダーを作成します。](create_the_data_access_layer/_static/image3.png)

### <a name="configuring-the-application-to-use-the-data-model"></a>データ モデルを使用してアプリケーションを構成します。

これで、データを表すクラスを作成した、クラスを使用してアプリケーションを構成する必要があります。 *Global.asax*ファイル、モデルを初期化するコードを追加します。 *Web.config*新しいデータ クラスで表されるデータの格納に使用するデータベースの新機能をアプリケーションに通知する情報を追加するファイル。 *Global.asax*ファイルを使用して、アプリケーションのイベントやメソッドを処理します。 *Web.config*ファイルでは、ASP.NET web アプリケーションの構成を制御することができます。

#### <a name="updating-the-globalasax-file"></a>Global.asax ファイルの更新

更新は、アプリケーションの起動時にデータ モデルを初期化、`Application_Start`ハンドラー、 *Global.asax.cs*ファイル。

> [!NOTE] 
> 
> ソリューション エクスプ ローラーで、いずれかを選択できます、 *Global.asax*ファイルまたは*Global.asax.cs*を編集するファイル、 *Global.asax.cs*ファイル。


1. 黄色で強調表示されている次のコードを追加、`Application_Start`メソッドで、 *Global.asax.cs*ファイル。   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample5.cs?highlight=9-10,22-23)]

> [!NOTE] 
> 
> お使いのブラウザーは、ブラウザーでこのチュートリアルの系列を表示するときに、黄色で強調表示されているコードを表示する HTML5 に対応する必要があります。


アプリケーションの起動時に、上記のコードに示すように、アプリケーションでは、データを初めて中に実行される初期化子にアクセスを指定します。 2 つの追加の名前空間のアクセスに必要な`Database`オブジェクトおよび`ProductDatabaseInitializer`オブジェクト。

 Web.Config ファイルを変更します。 

Entity Framework Code First は生成、データベースの既定の場所にデータベースには、シード データが入力されたときに、独自の接続情報をアプリケーションに追加するは、データベースの場所を制御します。 アプリケーションの接続文字列を使用してこのデータベース接続を指定する*Web.config*プロジェクトのルートにあるファイルです。 新しい接続文字列を追加すると、データベースの場所を指示することができます (*wingtiptoys.mdf*) アプリケーションのデータ ディレクトリに作成する (*アプリ\_データ*)、既定ではなく場所です。 この変更を行った使用すると、検索し、このチュートリアルの後半で、データベース ファイルを検査できます。

1. **ソリューション エクスプ ローラー**を検索して開く、 *Web.config*ファイル。
2. 黄色で強調表示されている接続文字列を追加、`<connectionStrings>`のセクションで、 *Web.config*ファイルの次のようにします。  

    [!code-xml[Main](create_the_data_access_layer/samples/sample6.xml?highlight=4-7)]

実行時に、アプリケーションが最初に、接続文字列によって指定された場所にデータベースが構築されます。 アプリケーションを実行する前にみましょう最初にビルドすることです。

## <a name="building-the-application"></a>アプリケーションのビルド

すべてのクラスと、Web アプリケーションへの変更が動作することを確認するには、アプリケーションをビルドする必要があります。

1. **デバッグ**メニューの **ビルド WingtipToys**です。  
 **出力**ウィンドウが表示され、すべてうまく表示、*が成功した*メッセージ。  

    ![データ アクセス レイヤーでは、出力ウィンドウを作成します。](create_the_data_access_layer/_static/image4.png)

エラーが発生した場合は、上記の手順を再度確認します。 内の情報、**出力**ウィンドウを示し、どのファイルに問題があるファイルの変更が必要な場所です。 この情報を使用すると、上記の手順のどの部分を確認して、プロジェクトで修正する必要がありますを決定できます。

## <a name="summary"></a>まとめ

系列のこのチュートリアルではするが、データ モデルの作成し、同様を初期化し、データベースのシードに使用されるコードを追加します。 アプリケーションを実行すると、データ モデルを使用してアプリケーションを構成しても。

チュートリアルでは、[次へ]、UI を更新、追加のナビゲーションしてデータベースからデータを取得します。 これにより、このチュートリアルで作成したエンティティ クラスに基づいて自動的に作成するデータベースが発生します。

## <a name="additional-resources"></a>その他のリソース

[Entity Framework の概要](https://msdn.microsoft.com/library/bb399567.aspx)   
[ADO.NET Entity Framework にビギナーズ ガイド](https://msdn.microsoft.com/data/ee712907)   
[Code First の開発と Entity Framework](http://www.msteched.com/2010/Europe/DEV212) (ビデオ)   
[コード最初のリレーションシップ Fluent API](https://msdn.microsoft.com/data/hh134698)   
[コードの最初のデータの注釈](https://msdn.microsoft.com/data/gg193958)  
[Entity Framework 用の生産性の向上](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0)

> [!div class="step-by-step"]
> [前へ](create-the-project.md)
> [次へ](ui_and_navigation.md)
