---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: ASP.NET MVC アプリケーション (10 の 8) で Entity Framework による継承の実装 |Microsoft ドキュメント
author: tdykstra
description: Contoso 大学でサンプル web アプリケーションでは、Entity Framework 5 Code First と Visual Studio を使用して ASP.NET MVC 4 アプリケーションを作成する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: a5c3feff-5335-4cdd-a97d-f7a8785c2494
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: ee088f841bdb68f4806b0b62be7d379b9eab9f8c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874006"
---
<a name="implementing-inheritance-with-the-entity-framework-in-an-aspnet-mvc-application-8-of-10"></a>ASP.NET MVC アプリケーション (10 の 8) で Entity Framework による継承の実装
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[完成したプロジェクトのダウンロード](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso 大学でサンプル web アプリケーションでは、Entity Framework 5 Code First と Visual Studio 2012 を使用して ASP.NET MVC 4 アプリケーションを作成する方法を示します。 チュートリアル シリーズについては、[シリーズの最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)をご覧ください。 一連のチュートリアルを開始するには、最初からまたは[この章のスタート プロジェクトをダウンロード](building-the-ef5-mvc4-chapter-downloads.md)し、ここから開始します。
> 
> > [!NOTE] 
> > 
> > 解決できない場合、問題が発生した場合[ダウンロード完了章](building-the-ef5-mvc4-chapter-downloads.md)の問題を再現しようとします。 一般に、コードを完成したコードを比較することによって、問題の解決策を検索できます。 一般的なエラーとそれらを解決する方法は、次を参照してください。[エラーと回避策です。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


前のチュートリアルでは、同時実行例外を処理します。 このチュートリアルでは、データ モデルで継承を実装する方法を示します。

オブジェクト指向プログラミングでは、冗長なコードを除去するのに継承を使用できます。 このチュートリアルでは、`Instructor` と `Student` クラスを `Person` 基底クラスから派生するように変更します。この基底クラスはインストラクターと受講者の両方に共通な `LastName` などのプロパティを含んでいます。 どの Web ページも追加または変更しませんが、コードの一部を変更し、それらの変更はデータベースに自動的に反映されます。

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>テーブルの種類ごとの継承ではなくテーブルは階層ごと

オブジェクト指向プログラミングでは、関連するクラスを使用しやすく継承を使用できます。 たとえば、`Instructor`と`Student`内のクラス、`School`データ モデルは冗長なコードでこの結果、複数のプロパティを共有します。

![Student_and_Instructor_classes](https://asp.net/media/2578113/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_e7a32f99-8bc4-48ce-aeaf-216a18071a8b.png)

`Instructor` エンティティと `Student` エンティティで共有されるプロパティの冗長なコードを削除すると仮定します。 作成することが、`Person`プロパティ、その共有だけが含まれているクラスを基本にしてください、`Instructor`と`Student`エンティティは、次の図に示すように、その基本クラスから継承します。

![Student_and_Instructor_classes_deriving_from_Person_class](https://asp.net/media/2578119/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_deriving_from_Person_class_671d708c-cbb8-454a-a8f8-c2d99439acd9.png)

データベースでこの継承構造を表すことができるいくつかの方法があります。 割り当てることも、`Person`受講者と講習においてインストラクターに 1 つのテーブルの両方に関する情報を含むテーブル。 講習においてインストラクターにのみ適用できなかった一部の列 (`HireDate`)、受講者にのみ一部 (`EnrollmentDate`)、一部には、両方 (`LastName`、 `FirstName`)。 通常、する必要があります、*識別子*各行を入力するかを示す列を表します。 たとえば、識別子列にインストラクターを示す "Instructor" と受講者を示す "Student" がある場合があります。

![Table-per-hierarchy_example](https://asp.net/media/2578125/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-hierarchy_example_244067cd-b451-4e9b-9595-793b9afca505.png)

1 つのデータベース テーブルからエンティティ継承構造を生成するには、このパターンが呼び出された*テーブルの階層あたり*(TPH) 継承します。

代わりに、継承構造と同じように見えるデータベースを作成することもできます。 たとえば name フィールドのみを持つことが、`Person`テーブルし、は独立した`Instructor`と`Student`日付フィールドを持つテーブルです。

![Table-per-type_inheritance](https://asp.net/media/2578131/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-type_inheritance.png)

各エンティティ クラスと呼ばれるデータベース テーブルのこのパターン*型ごとにテーブル*(TPT) 継承します。

一般に TPH 継承パターンでは TPT パターンが複雑な結合クエリのためにパフォーマンスを向上させる TPT 継承のパターンよりも、Entity Framework でに配信しています。 このチュートリアルでは、TPH 継承の実装方法を示します。 次の手順を実行することによってを実行してみましょう。

- 作成、`Person`クラスし、変更、`Instructor`と`Student`クラスから派生する`Person`です。
- データベース コンテキスト クラスには、データベースへのモデルのマッピング コードを追加します。
- 変更`InstructorID`と`StudentID`プロジェクト全体にわたる参照`PersonID`です。

## <a name="creating-the-person-class"></a>ユーザー クラスを作成します。

 注: これらのクラスを使用するコント ローラーを更新するまでは、次のクラスを作成した後、プロジェクトをコンパイルできません。 

*モデル*フォルダー作成*Person.cs*テンプレート コードを次のコードに置き換えます。

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

*Instructor.cs*、派生、`Instructor`クラス、`Person`クラスし、キーと名前のフィールドを削除します。 コードは次の例のように表示されます。

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

同様に変更を*Student.cs*です。 `Student`クラスは次の例のようになります。

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="adding-the-person-entity-type-to-the-model"></a>Person エンティティ型をモデルに追加します。

*SchoolContext.cs*、追加、`DbSet`プロパティを`Person`エンティティの種類。

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Table-per-Hierarchy 継承を構成するために Entity Framework に必要なのことはこれですべてです。 わかる、データベースが再作成された場合、`Person`の代わりにテーブル、`Student`と`Instructor`テーブル。

## <a name="changing-instructorid-and-studentid-to-personid"></a>PersonID に InstructorID と学生 Id を変更します。

*SchoolContext.cs*、ステートメントでは、インストラクター コース マッピング変更`MapRightKey("InstructorID")`に`MapRightKey("PersonID")`:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=4)]

この変更は必要はありません。多対多の結合テーブル内の InstructorID 列の名前を変更するだけです。 InstructorID と名前のままの場合、アプリケーションが正しく機能もします。 ここでは、完成した*SchoolContext.cs*:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=15,24)]

次を変更する必要があります`InstructorID`に`PersonID`と`StudentID`に`PersonID`プロジェクト全体にわたって***を除く***内の移行のタイムスタンプ付きのファイルで、*移行*フォルダーです。 これを行うにするを検索し、開きますを変更する必要のあるファイルのみファイルが開かれてでグローバル変更を実行します。 唯一のファイル、*移行*フォルダーを変更する必要がありますが*Migrations\Configuration.cs です。*

1. > [!IMPORTANT]
   > Visual Studio で開いているすべてのファイルを終了して作業を開始します。
2. をクリックして**検索し置換--すべてのファイルを検索する**で、**編集**メニューのおよびを含む、プロジェクト内のすべてのファイルを検索`InstructorID`です。  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. 内の各ファイルを開き、**検索結果**ウィンドウ***を除く***、&lt;タイムスタンプ&gt;*\_.cs* で移行ファイル*移行*フォルダー、ファイルごとに 1 つの線をダブルクリックします。  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)
4. 開く、**ファイル内の置換**ダイアログと変更**ファイルの場所**に**すべての開いているドキュメント**です。
5. 使用して、**ファイル内の置換**すべて変更するためのダイアログ`InstructorID`に `PersonID.`  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)
6. プロジェクトが含まれているすべてのファイルを検索`StudentID`です。
7. 内の各ファイルを開き、**検索結果**ウィンドウ***を除く***、&lt;タイムスタンプ&gt;*\_\*.cs*移行ファイル*移行*フォルダー、ファイルごとに 1 つの線をダブルクリックします。  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
8. 開く、**ファイル内の置換**ダイアログと変更**ファイルの場所**に**すべての開いているドキュメント**です。
9. 使用して、**ファイル内の置換**すべて変更するためのダイアログ`StudentID`に`PersonID`です。   
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
10. プロジェクトをビルドします。

(これを示す注、*欠点*の`classnameID`主キーの名前付けパターン。 クラスの名前を付けることがなく主キー ID の名前を付けていた場合*ありません*名前を変更する必要があるようになりました)。

## <a name="create-and-update-a-migrations-file"></a>作成し、移行ファイルの更新

パッケージ マネージャー コンソール (PMC) では、次のコマンドを入力します。

`Add-Migration Inheritance`

実行、 `Update-Database` PMC コマンド。 コマンドは、既存のデータ移行を処理する方法を知らないにしているために、この時点で失敗します。 次のエラーが発生します。

*ALTER TABLE ステートメントは、FOREIGN KEY 制約と競合して"FK\_dbo します。部門\_dbo します。Person\_PersonID"です。競合が発生したデータベース"ContosoUniversity"テーブル"dbo します。人"、列 'PersonID' です。*

開いている*移行\&lt; タイムスタンプ&gt;\_Inheritance.cs*と置換、`Up`メソッドを次のコード。

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=25,29-40)]

実行、`update-database`もう一度コマンドします。

> [!NOTE]
> データとスキーマの変更を移行するときにその他のエラーが発生することができます。 移行エラーが発生する場合は解決できない場合、内の接続文字列を変更することで、チュートリアルを続行することができます、 *Web.config*ファイルまたはデータベースを削除します。 最も簡単な方法が内のデータベースの名前を変更するには、 *Web.config*ファイル。 たとえば、CU にデータベース名を変更\_の次の例に示すようにテストします。
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.xml?highlight=1-2)]
> 
> 新しいデータベースを移行するデータがないと、`update-database`コマンドがエラーなしで完了する可能性が高くなります。 データベースを削除する方法については、次を参照してください。[を Visual Studio 2012 からデータベースを削除する方法](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/)です。 チュートリアルを続行するためにこの方法を採用する場合は、移行を自動的に実行時に同じエラーが発生は、配置サイトから、このチュートリアルの最後の配置手順をスキップします。 移行エラーのトラブルシューティングする場合に最適なリソースは Entity Framework のフォーラムや StackOverflow.com のいずれかです。


## <a name="testing"></a>テスト中

サイトを実行して、さまざまなページを再試行してください。 すべてが前と同じように動作します。

**サーバー エクスプ ローラーで、** 展開**SchoolContext**し**テーブル**、されたことを確認し、**学生**と**インストラクター**テーブルに置換された、 **Person**テーブル。 展開、 **Person**テーブルとしているときに存在するために使用される列のすべて、**学生**と**インストラクター**テーブル。

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Person テーブルを右クリックし、**[テーブル データの表示]** をクリックして識別子列を表示します。

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

次の図は、新しい School データベースの構造を示しています。

![School_database_diagram](https://asp.net/media/2578143/Windows-Live-Writer_58f5a93579b2_CC7B_School_database_diagram_6350a801-7199-413f-bbac-4a2009ed19d7.png)

## <a name="summary"></a>まとめ

テーブルの階層あたりの継承が実装されたようになりました、 `Person`、 `Student`、および`Instructor`クラスです。 このおよびその他の継承構造の詳細については、次を参照してください。[継承のマッピング方法](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx)Morteza Manavi ブログ。 次のチュートリアルでは、リポジトリと作業パターンの単位を実装するいくつかの方法が表示されます。

その他の Entity Framework リソースへのリンクは含まれて、 [ASP.NET データ アクセス コンテンツ マップ](../../../../whitepapers/aspnet-data-access-content-map.md)です。

> [!div class="step-by-step"]
> [前へ](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [次へ](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
