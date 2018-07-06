---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
title: データ アクセス層の作成 |Microsoft Docs
author: Erikre
description: このチュートリアル シリーズでは、私たちの ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
ms.date: 09/08/2014
ms.assetid: 0bbf7a6e-d7eb-4091-91e4-fff892777f32
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
msc.type: authoredcontent
ms.openlocfilehash: b462444396d0dcb3fcaf8ec93170c049cd1ebaef
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819904"
---
<a name="create-the-data-access-layer"></a>データ アクセス層を作成します。
====================
によって[Erik Reitan](https://github.com/Erikre)

[Wingtip Toys のサンプル プロジェクト (c#) をダウンロード](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)または[電子書籍 (PDF) をダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> このチュートリアル シリーズでは、Web 用 ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明します。 Visual Studio 2013[プロジェクトと c# ソース コード](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)このチュートリアル シリーズをと共に使用できます。


このチュートリアルでは、作成、アクセス、および ASP.NET Web フォームと Entity Framework Code First を使用してデータベースからデータを確認する方法について説明します。 このチュートリアルでは、「プロジェクトを作成する」前のチュートリアルに基づいており、Wingtip 玩具店のチュートリアル シリーズのパートです。 内にあるデータ アクセス クラスのグループが作成したが、このチュートリアルを完了すると、*モデル*プロジェクトのフォルダー。

## <a name="what-youll-learn"></a>学習内容。

- データ モデルを作成する方法。
- 初期化し、データベースをシードする方法。
- 更新、およびデータベースをサポートするアプリケーションを構成する方法。

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>このチュートリアルで導入された機能を次に示します。

- エンティティ フレームワーク コード ファースト
- LocalDB
- データの注釈

## <a name="creating-the-data-models"></a>データ モデルを作成します。

[Entity Framework](https://msdn.microsoft.com/data/aa937723)はオブジェクト リレーショナル マッピング (ORM) フレームワークです。 通常記述する必要のあるデータ アクセス コードの大半を排除することをオブジェクトとしてのリレーショナル データを操作できます。 Entity Framework を使用して、使用してクエリを発行する[LINQ](https://msdn.microsoft.com/library/bb397926.aspx)、取得し、厳密に型指定されたオブジェクトとしてデータを操作します。 LINQ は、クエリを実行して、データを更新するためのパターンを提供します。 Entity Framework を使用してアクセスの基本データに注目するのではなく、アプリケーションの残りの部分を作成に集中することができます。 このチュートリアルのシリーズの後半では、データを使用して、ナビゲーションおよび製品のクエリを作成する方法を紹介します。

Entity Framework と呼ばれる開発パラダイムをサポートしている*Code First*します。 最初のコードではクラスを使用して、データ モデルを定義することができます。 クラスは、コンス トラクターを他の型、メソッドおよびイベントの変数をまとめてグループ化して独自のカスタム型を作成することができます。 クラスを既存のデータベースにマップしたり、それらを使用してデータベースを生成できます。 このチュートリアルでは、データ モデル クラスを記述することで、データ モデルを作成します。 次に、Entity Framework のこれらの新しいクラスからその場でデータベースを作成できるようにします。

Web フォーム アプリケーションのデータ モデルを定義するエンティティ クラスを作成して開始されます。 エンティティ クラスを管理し、データベースへのデータ アクセスを提供するコンテキスト クラスを作成します。 また、データベースの設定を使用する初期化子クラスを作成します。

### <a name="entity-framework-and-references"></a>Entity Framework と参照

既定では、Entity Framework が含まれる新しいを作成するときに**ASP.NET Web アプリケーション**を使用して、 **Web フォーム**テンプレート。 Entity Framework のインストール、アンインストール、および NuGet パッケージとして更新します。

この NuGet パッケージには、次が含まれています。**ランタイム**プロジェクト内のアセンブリ。

- EntityFramework.dll – Entity Framework で使用されるすべての一般的なランタイム コード
- EntityFramework.SqlServer.dll – Entity Framework 用 Microsoft SQL Server プロバイダー

### <a name="entity-classes"></a>エンティティ クラス

データのスキーマの定義を作成するクラスには、エンティティ クラスが呼び出されます。 データベースの設計に慣れていない場合は、データベースのテーブルの定義とエンティティ クラスを考えます。 クラス内の各プロパティには、データベースのテーブルの列を指定します。 これらのクラスは、オブジェクト指向のコードとデータベースのリレーショナル テーブル構造の軽量のオブジェクト リレーショナル インターフェイスを提供します。

このチュートリアルでは、製品およびカテゴリのスキーマを表す単純なエンティティ クラスを追加することで開始します。 製品クラスは、各製品の定義が格納されます。 各 product クラスのメンバーの名前になります`ProductID`、 `ProductName`、 `Description`、 `ImagePath`、 `UnitPrice`、 `CategoryID`、および`Category`します。 カテゴリのクラスは、自動車、ボート、平面など、製品が属することができる各カテゴリの定義が格納されます。 各カテゴリのクラスのメンバーの名前になります`CategoryID`、 `CategoryName`、 `Description`、および`Products`します。 各製品カテゴリのいずれかに属します。 これらのエンティティ クラスは、プロジェクトの既存に追加されます*モデル*フォルダー。

1. **ソリューション エクスプ ローラー**を右クリックし、*モデル*クリックしてフォルダー**追加** - &gt; **新しい項目の**します。 

    ![データ アクセス層 - 新しい項目 メニューの作成します。](create_the_data_access_layer/_static/image1.png)

   **[新しい項目の追加]** ダイアログ ボックスが表示されます。
2. **Visual c#** から、**インストール済み**、左側のペイン**コード**します。 

    ![データ アクセス層 - 新しい項目 メニューの作成します。](create_the_data_access_layer/_static/image2.png)
3. 選択**クラス**中央のペインからこの新しいクラスの名前と*Product.cs*します。
4. **[追加]** をクリックします。  
   新しいクラス ファイルがエディターで表示されます。
5. 既定のコードを次のコードに置き換えます。   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample1.cs)]
6. ただし、名前、新しいクラスを手順 1. ~ 4. を繰り返して、別のクラスを作成する*Category.cs*既定のコードを次のコードに置き換えます。  

    [!code-csharp[Main](create_the_data_access_layer/samples/sample2.cs)]

既に触れましたが、`Category`クラスを表しますが、アプリケーションは、製品の種類は、販売するために設計されています (など<a id="a"> </a>&quot;自動車&quot;、&quot;ボート&quot;、 &quot;ロケット&quot;など)、および`Product`クラスは、データベース内の個々 の製品 (toys) を表します。 各インスタンスを`Product`オブジェクトは、リレーショナル データベース テーブル内の行に対応しているし、Product クラスの各プロパティは、リレーショナル データベース テーブル内の列にマップされます。 このチュートリアルの後半では、データベース内に含まれる製品データを確認します。

### <a name="data-annotations"></a>データの注釈

お気付きかもしれませんクラスの特定のメンバーがあるなど、メンバーに関する詳細を指定する属性`[ScaffoldColumn(false)]`します。 これらは*データ注釈*します。 データ注釈属性は、書式を指定して、データベースが作成されるときのモデル方法を指定する、そのメンバーのユーザー入力を検証する方法を記述できます。

### <a name="context-class"></a>Context クラス

データ アクセス クラスの使用を開始するには、コンテキスト クラスを定義する必要があります。 コンテキスト クラスがエンティティ クラスを管理する前述のように、(など、`Product`クラスおよび`Category`クラス) し、データベースへのデータ アクセスを提供します。

この手順で、新しい c# コンテキスト クラスを追加、*モデル*フォルダー。

1. 右クリックし、*モデル*クリックしてフォルダー**追加** - &gt; **新しい項目の**します。   
   **[新しい項目の追加]** ダイアログ ボックスが表示されます。
2. 選択**クラス**中央のペインから名前を付けます*ProductContext.cs*  をクリック**追加**します。
3. 次のコードでクラスに含まれている既定のコードに置き換えます。   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample3.cs)]

このコードを追加、`System.Data.Entity`名前空間をクエリする機能を含む、Entity Framework のすべてのコア機能にアクセスするように挿入、更新、および厳密に型指定されたオブジェクトを使用してデータを削除します。

`ProductContext`クラスは、フェッチ、格納、および更新を処理する Entity Framework 製品データベース コンテキストを表します。`Product`クラス、データベース内のインスタンス。 `ProductContext`クラスから派生、`DbContext`基本クラスを Entity Framework によって提供されます。

### <a name="initializer-class"></a>初期化子クラス

コンテキストが使用されるデータベース最初の時間を初期化するためにいくつかのカスタム ロジックを実行する必要があります。 これで、製品と分類をすぐに表示できるように、データベースに追加するデータをシードします。

この手順で、新しい c# 初期化子クラスを追加、*モデル*フォルダー。

1. 別の作成`Class`で、*モデル*フォルダーと名前を付けます*ProductDatabaseInitializer.cs*します。
2. 次のコードでクラスに含まれている既定のコードに置き換えます。   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample4.cs)]

データベースが作成され、初期化時に、上記のコードからご覧のとおり、`Seed`プロパティがオーバーライドされ、設定します。 ときに、`Seed`プロパティが設定されて、カテゴリおよび製品からの値は、データベースの作成に使用されます。 データベースが作成された後に、上記のコードを変更することで、登録されたデータを更新しようとした場合、Web アプリケーションを実行すると、更新プログラムは表示されません。 理由は、上記のコードの実装を使用して、`DropCreateDatabaseIfModelChanges`シード データをリセットする前に、モデル (スキーマ) が変更されたかどうかを認識するクラス。 変更されていない場合、`Category`と`Product`エンティティ クラス、データベースがシード データを再初期化されません。

> [!NOTE] 
> 
> アプリケーションを実行するたびに再作成するデータベースの場合は、使用する可能性があります、`DropCreateDatabaseAlways`クラスの代わりに、`DropCreateDatabaseIfModelChanges`クラス。 ただしこのチュートリアル シリーズで使用して、`DropCreateDatabaseIfModelChanges`クラス。


この時点で、このチュートリアルでは必要があります、*モデル*4 つの新しいクラスと 1 つの既定のクラスを使ってフォルダー。

![データ アクセス層の Models フォルダーを作成します。](create_the_data_access_layer/_static/image3.png)

### <a name="configuring-the-application-to-use-the-data-model"></a>データ モデルを使用するアプリケーションを構成します。

データを表すクラスを作成するので、クラスを使用してアプリケーションを構成する必要があります。 *Global.asax*ファイルはモデルを初期化するコードを追加します。 *Web.config*ファイルが新しいデータ クラスによって表されるデータの格納を使用してどのようなデータベースをアプリケーションに通知する情報を追加します。 *Global.asax*ファイルは、アプリケーションのイベントやメソッドを処理するために使用できます。 *Web.config*ファイルでは、ASP.NET web アプリケーションの構成を制御できます。

#### <a name="updating-the-globalasax-file"></a>Global.asax ファイルの更新

アプリケーションの起動時にデータ モデルを初期化するが更新されます、`Application_Start`ハンドラーで、 *Global.asax.cs*ファイル。

> [!NOTE] 
> 
> ソリューション エクスプ ローラーで、いずれかを選択できます、 *Global.asax*ファイルまたは*Global.asax.cs*ファイルを編集する、 *Global.asax.cs*ファイル。


1. 黄色で強調表示されている次のコードを追加、`Application_Start`メソッドで、 *Global.asax.cs*ファイル。   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample5.cs?highlight=9-10,22-23)]

> [!NOTE] 
> 
> ブラウザーでは、ブラウザーでこのチュートリアル シリーズを表示するときに、黄色で強調表示されているコードを表示する HTML5 をサポートする必要があります。


アプリケーションの起動時に、上記のコードに示すように、アプリケーションでは、データの最初の期間中に実行する初期化子のアクセスを指定します。 2 つの追加の名前空間のアクセスに必要な`Database`オブジェクトと`ProductDatabaseInitializer`オブジェクト。

 Web.Config ファイルを変更します。 

Entity Framework Code First は生成、データベースの既定の場所にシード データをデータベースの内容が表示されたら、アプリケーションへの接続情報を追加できます、データベースの場所を制御します。 アプリケーションの接続文字列を使用してこのデータベース接続を指定する*Web.config*プロジェクトのルートにあるファイル。 新しい接続文字列を追加すると、データベースの場所を指示することができます (*wingtiptoys.mdf*) アプリケーションのデータ ディレクトリでビルドされる (*アプリ\_データ*)、既定ではなく場所。 この変更を行った使用すると、検索して、このチュートリアルの後半で、データベース ファイルを検査できます。

1. **ソリューション エクスプ ローラー**を検索して開く、 *Web.config*ファイル。
2. 黄色で強調表示されている接続文字列を追加、`<connectionStrings>`のセクション、 *Web.config*ファイルの次のようにします。  

    [!code-xml[Main](create_the_data_access_layer/samples/sample6.xml?highlight=4-7)]

アプリケーションが実行される、最初には、接続文字列によって指定された場所にデータベースを作成します。 アプリケーションを実行する前にみましょう最初にビルド。

## <a name="building-the-application"></a>アプリケーションのビルド

クラスと、Web アプリケーションへの変更がすべて動作することを確認するには、アプリケーションを構築する必要があります。

1. **デバッグ**メニューの **ビルド WingtipToys**します。  
 **出力**ウィンドウが表示され、すべてうまくいけば、表示、*が成功した*メッセージ。  

    ![データ アクセス層 - Windows の出力を作成します。](create_the_data_access_layer/_static/image4.png)

エラーが発生した場合は、上記の手順を再度確認します。 内の情報、**出力**ウィンドウは、どのファイルに問題があるし、ファイルの変更が必要な場合。 この情報は、上記の手順のどの部分を確認して、プロジェクトで修正する必要がありますを決定するを有効になります。

## <a name="summary"></a>まとめ

シリーズのこのチュートリアルではするが、データ モデルの作成だけでなく、初期化し、データベースのシードに使用されるコードを追加します。 アプリケーションの実行時に、データ モデルを使用するアプリケーションを構成することもします。

次のチュートリアルで UI を更新、ナビゲーションを追加し、データベースからデータを取得します。 これにより、このチュートリアルで作成したエンティティ クラスに基づいて自動的に作成されているデータベースが発生します。

## <a name="additional-resources"></a>その他のリソース

[Entity Framework の概要](https://msdn.microsoft.com/library/bb399567.aspx)   
[ADO.NET Entity Framework の初心者向けガイド](https://msdn.microsoft.com/data/ee712907)   
[Entity Framework を使用した最初の開発をコード](http://www.msteched.com/2010/Europe/DEV212)(ビデオ)   
[リレーションシップ code First Fluent API](https://msdn.microsoft.com/data/hh134698)   
[Code First のデータ注釈](https://msdn.microsoft.com/data/gg193958)  
[Entity Framework の生産性の向上](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0)

> [!div class="step-by-step"]
> [前へ](create-the-project.md)
> [次へ](ui_and_navigation.md)
