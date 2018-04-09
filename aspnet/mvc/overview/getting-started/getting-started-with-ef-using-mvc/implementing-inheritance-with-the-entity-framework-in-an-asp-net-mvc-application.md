---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: ASP.NET MVC 5 アプリケーション (11/12) で Entity Framework 6 による継承の実装 |Microsoft ドキュメント
author: tdykstra
description: Contoso 大学でサンプル web アプリケーションでは、Entity Framework 6 の Code First と Visual Studio を使用して ASP.NET MVC 5 アプリケーションを作成する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 08834147-77ec-454a-bb7a-d931d2a40dab
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 1826659626106993d4796641492c62fcbd22a1b3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="implementing-inheritance-with-the-entity-framework-6-in-an-aspnet-mvc-5-application-11-of-12"></a>ASP.NET MVC 5 アプリケーション (11/12) で Entity Framework 6 による継承の実装
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[完成したプロジェクトをダウンロード](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)または[PDF のダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso 大学でサンプル web アプリケーションでは、Entity Framework 6 の Code First と Visual Studio 2013 を使用して ASP.NET MVC 5 アプリケーションを作成する方法を示します。 チュートリアル シリーズについては、[シリーズの最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)をご覧ください。


前のチュートリアルでは、同時実行例外を処理します。 このチュートリアルでは、データ モデルで継承を実装する方法を示します。

オブジェクト指向プログラミングで使用できます[継承](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming))を容易にする[コードの再利用](http://en.wikipedia.org/wiki/Code_reuse)です。 このチュートリアルでは、`Instructor` と `Student` クラスを `Person` 基底クラスから派生するように変更します。この基底クラスはインストラクターと受講者の両方に共通な `LastName` などのプロパティを含んでいます。 どの Web ページも追加または変更しませんが、コードの一部を変更し、それらの変更はデータベースに自動的に反映されます。

## <a name="options-for-mapping-inheritance-to-database-tables"></a>継承をデータベース テーブルにマップするためのオプション

`Instructor`と`Student`内のクラス、`School`データ モデルと同じではいくつかのプロパティがあります。

![Student_and_Instructor_classes](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

`Instructor` エンティティと `Student` エンティティで共有されるプロパティの冗長なコードを削除すると仮定します。 または、インストラクターと学生のどちらから名前を取得したかに関係なく、名前をフォーマットできるサービスを記述するとします。 作成することが、`Person`プロパティ、その共有だけが含まれているクラスを基本にしてください、`Instructor`と`Student`エンティティは、次の図に示すように、その基本クラスから継承します。

![Student_and_Instructor_classes_deriving_from_Person_class](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

データベースでこの継承構造を表すことができるいくつかの方法があります。 割り当てることも、`Person`受講者と講習においてインストラクターに 1 つのテーブルの両方に関する情報を含むテーブル。 講習においてインストラクターにのみ適用できなかった一部の列 (`HireDate`)、受講者にのみ一部 (`EnrollmentDate`)、一部には、両方 (`LastName`、 `FirstName`)。 通常、する必要があります、*識別子*各行を入力するかを示す列を表します。 たとえば、識別子列にインストラクターを示す "Instructor" と受講者を示す "Student" がある場合があります。

![Table-per-hierarchy_example](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

1 つのデータベース テーブルからエンティティ継承構造を生成するには、このパターンが呼び出された*テーブルの階層あたり*(TPH) 継承します。

代わりに、継承構造と同じように見えるデータベースを作成することもできます。 たとえば name フィールドのみを持つことが、`Person`テーブルし、は独立した`Instructor`と`Student`日付フィールドを持つテーブルです。

![Table-per-type_inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

各エンティティ クラスと呼ばれるデータベース テーブルのこのパターン*型ごとにテーブル*(TPT) 継承します。

他のオプションとして、個々のテーブルにすべての非抽象型をマップすることもできます。 継承されたプロパティを含むクラスのすべてのプロパティは、対応するテーブルの列にマップされます。 このパターンは、Table-per-Concrete Class (TPC) 継承と呼ばれます。 継承を TPC を実装している場合、 `Person`、 `Student`、および`Instructor`、前に示したようにクラス、`Student`と`Instructor`テーブルはよりも長く前に、継承を実装した後にまったく同じなります。

TPC と TPH 継承パターン TPT 継承のパターンよりも、Entity Framework のパフォーマンスを向上させるが通常提供 TPT パターンが複雑な結合クエリのために使用します。

このチュートリアルでは、TPH 継承の実装方法を示します。 TPH は Entity Framework での既定の継承パターンを作成では行う必要があるすべて、`Person`クラス、変更、`Instructor`と`Student`クラスから派生する`Person`、新しいクラスを追加、 `DbContext`、作成し、移行します。 (その他の継承パターンを実装する方法については、次を参照してください[マッピング テーブルの種類ごと (TPT) 継承](https://msdn.microsoft.com/data/jj591617#2.5)と[具象型でのテーブルあたりクラス (TPC) の継承をマッピング](https://msdn.microsoft.com/data/jj591617#2.6)、MSDN の。Entity Framework ドキュメントです。)

## <a name="create-the-person-class"></a>Person クラスの作成

*モデル*フォルダー作成*Person.cs*テンプレート コードを次のコードに置き換えます。

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a>Person クラスから Student クラスと Instructor クラスを派生させる

*Instructor.cs*、派生、`Instructor`クラス、`Person`クラスし、キーと名前のフィールドを削除します。 コードは次の例のように表示されます。

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

同様に変更を*Student.cs*です。 `Student`クラスは次の例のようになります。

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="add-the-person-entity-type-to-the-model"></a>Person エンティティ型をモデルに追加します。

*SchoolContext.cs*、追加、`DbSet`プロパティを`Person`エンティティの種類。

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Table-per-Hierarchy 継承を構成するために Entity Framework に必要なのことはこれですべてです。 わかる、データベースが更新されたとき、`Person`の代わりにテーブル、`Student`と`Instructor`テーブル。

## <a name="create-and-update-a-migrations-file"></a>作成し、移行ファイルの更新

パッケージ マネージャー コンソール (PMC) では、次のコマンドを入力します。

`Add-Migration Inheritance`

実行、 `Update-Database` PMC コマンド。 コマンドは、既存のデータ移行を処理する方法を知らないにしているために、この時点で失敗します。 次のようなエラー メッセージを表示します。

> *オブジェクトを削除できませんでした ' dbo します。講師 ' FOREIGN KEY 制約で参照されているためです。*


開いている*移行\&lt; タイムスタンプ&gt;\_Inheritance.cs*と置換、`Up`メソッドを次のコード。

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

このコードは、次のデータベースの更新タスクを処理します。

- 外部キー制約と Student テーブルをポイントするインデックスを削除します。
- Instructor テーブルの名前の Person に変更し、Student データを格納するために必要な変更を加えます。

    - 受講者の null 許容 EnrollmentDate を追加します。
    - 行が、受講者かインストラクターかを示すために識別子列を追加します。
    - 受講者行には雇用日がないので HireDate を nul 許容にします。
    - 受講者をポイントする外部キーの更新に使用する一時的なフィールドを追加します。 Person テーブルに受講者をコピーするときに、新しい主キー値が表示されます。
- Student テーブルから Person テーブルにデータをコピーします。 これにより、受講者に新しい主キー値が割り当てられます。
- 受講者をポイントする外部キー値を修正します。
- 今は Person テーブルをポイントしている外部キー制約とインデックスを再作成します 

(主キーの型として整数の代わりに GUID を使用した場合は、受講者の主キー値を変更する必要はなく、これらの手順のいくつかを省略できます)。

実行、`update-database`もう一度コマンドします。

(実稼働システムでは対応に変更を加えるダウン メソッド場合に、データベースの以前のバージョンに戻るに使用しなければならなかったことです。 このチュートリアルではありませんする方法を使用してダウンします。)

> [!NOTE]
> データとスキーマの変更を移行するときにその他のエラーが発生することができます。 移行エラーが発生する場合は解決できない場合、内の接続文字列を変更することで、チュートリアルを続行することができます、 *Web.config*ファイルまたはでデータベースを削除します。 最も簡単な方法が内のデータベースの名前を変更するには、 *Web.config*ファイル。 たとえば、次の例で示すように ContosoUniversity2 にデータベース名を変更します。
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.xml?highlight=2)]
> 
> 新しいデータベースを移行するデータがないと、`update-database`コマンドがエラーなしで完了する可能性が高くなります。 データベースを削除する方法については、次を参照してください。[を Visual Studio 2012 からデータベースを削除する方法](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/)です。 チュートリアルを続行するためにこの方法を採用する場合、このチュートリアルの最後の配置手順をスキップまたは新しいサイトとデータベースを展開します。 したされてに配置する、既に同じサイトに更新プログラムを展開する場合 EF で移行を自動的に実行するときに同じエラーがありますが表示されます。 移行エラーのトラブルシューティングする場合に最適なリソースは Entity Framework のフォーラムや StackOverflow.com のいずれかです。


## <a name="testing"></a>テスト中

サイトを実行して、さまざまなページを再試行してください。 すべてが前と同じように動作します。

**サーバー エクスプ ローラーで、**展開**データ Connections\SchoolContext**し**テーブル**、されたことを確認し、**学生**と**インストラクター**テーブルに置換された、 **Person**テーブル。 展開、 **Person**テーブルとしているときに存在するために使用される列のすべて、**学生**と**インストラクター**テーブル。

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Person テーブルを右クリックし、**[テーブル データの表示]** をクリックして識別子列を表示します。

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

次の図は、新しい School データベースの構造を示しています。

![School_database_diagram](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

## <a name="deploy-to-azure"></a>Azure への配置します。

このセクションでは、省略可能な完了している必要があります**Azure へのアプリの展開**」の「[パート 3、並べ替え、フィルター、およびページング](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)このチュートリアル シリーズのです。 ローカル プロジェクトでデータベースを削除することによって解決移行エラーがある場合です。 この手順をスキップします。または、新しいサイトとデータベースを作成し、新しい環境に展開します。

1. Visual Studio でプロジェクトを右クリックして**ソリューション エクスプ ローラー**選択**発行**コンテキスト メニューからです。  
  
    ![プロジェクトのコンテキスト メニューを発行します。](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)
2. **[発行]**をクリックします。  
  
    ![publish](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)  
  
   Web アプリは、既定のブラウザーで開きます。
3. アプリケーションをテストすることを確認して動作します。

    初めてページを実行するデータベースにアクセスする場合は、Entity Framework 実行のすべての移行の`Up`データベースが現在のデータ モデルに最新の状態に必要なメソッドです。

## <a name="summary"></a>まとめ

`Person`、`Student`、および `Instructor` クラスの Table-per-Hierarchy 継承を実装しました。 このおよびその他の継承構造の詳細については、次を参照してください。 [TPT の継承パターン](https://msdn.microsoft.com/data/jj618293)と[TPH の継承パターン](https://msdn.microsoft.com/data/jj618292)msdn です。 次のチュートリアルでは、比較的高度なさまざまな Entity Framework のシナリオを処理する方法を説明します。

その他の Entity Framework リソースへのリンクは含まれて、 [ASP.NET データ アクセス - リソースのことをお勧め](../../../../whitepapers/aspnet-data-access-content-map.md)です。

> [!div class="step-by-step"]
> [前へ](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [次へ](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
