---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: ASP.NET MVC アプリケーション (10 の 8) で、Entity Framework による継承の実装 |Microsoft Docs
author: tdykstra
description: Contoso University のサンプルの web アプリケーションでは、Entity Framework 5 Code First と Visual Studio を使用して ASP.NET MVC 4 アプリケーションを作成する方法について説明しています.
ms.author: riande
ms.date: 07/30/2013
ms.assetid: a5c3feff-5335-4cdd-a97d-f7a8785c2494
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 10ee0be62a1d601e323afc423e9022bed56f4f33
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827313"
---
<a name="implementing-inheritance-with-the-entity-framework-in-an-aspnet-mvc-application-8-of-10"></a>ASP.NET MVC アプリケーション (10 の 8) で、Entity Framework による継承の実装
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[完成したプロジェクトのダウンロード](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso University のサンプルの web アプリケーションでは、Entity Framework 5 Code First と Visual Studio 2012 を使用して ASP.NET MVC 4 アプリケーションを作成する方法を示します。 チュートリアル シリーズについては、[シリーズの最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)をご覧ください。 チュートリアルのシリーズを開始するには、最初からまたは[この章のスタート プロジェクトをダウンロード](building-the-ef5-mvc4-chapter-downloads.md)し、ここから始めてください。
> 
> > [!NOTE] 
> > 
> > を解決できない問題が生じた場合[章では、完了したダウンロード](building-the-ef5-mvc4-chapter-downloads.md)の問題を再現しようとします。 問題の解決策は、完成したコードにコードを比較することによって一般的に見つかります。 一般的なエラーとその解決方法は、次を参照してください。[エラーと回避策。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


前のチュートリアルでは、同時実行例外を処理します。 このチュートリアルでは、データ モデルで継承を実装する方法を示します。

オブジェクト指向プログラミングでは、冗長なコードを排除するのに継承を使用することができます。 このチュートリアルでは、`Instructor` と `Student` クラスを `Person` 基底クラスから派生するように変更します。この基底クラスはインストラクターと受講者の両方に共通な `LastName` などのプロパティを含んでいます。 どの Web ページも追加または変更しませんが、コードの一部を変更し、それらの変更はデータベースに自動的に反映されます。

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>テーブルの型ごとの継承とテーブルは階層ごと

オブジェクト指向プログラミングでの関連クラスを使用しやすく継承を使用することができます。 たとえば、`Instructor`と`Student`クラス、`School`データ モデルを共有コードが冗長になるいくつかのプロパティ。

![Student_and_Instructor_classes](https://asp.net/media/2578113/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_e7a32f99-8bc4-48ce-aeaf-216a18071a8b.png)

`Instructor` エンティティと `Student` エンティティで共有されるプロパティの冗長なコードを削除すると仮定します。 作成することが、`Person`基本クラスがそれらの共有プロパティだけが含まれているようにして、`Instructor`と`Student`エンティティは、次の図に示すように、その基本クラスから継承します。

![Student_and_Instructor_classes_deriving_from_Person_class](https://asp.net/media/2578119/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_deriving_from_Person_class_671d708c-cbb8-454a-a8f8-c2d99439acd9.png)

データベースでこの継承構造を表すことができるいくつかの方法があります。 割り当てることも、`Person`受講者とインストラクターによる 1 つのテーブルの両方に関する情報が含まれるテーブル。 インストラクターのみに適用の一部の列 (`HireDate`)、受講者のみに (`EnrollmentDate`)、一部には、両方 (`LastName`、 `FirstName`)。 通常があります、*識別子*各行を入力するかを示す列を表します。 たとえば、識別子列にインストラクターを示す "Instructor" と受講者を示す "Student" がある場合があります。

![テーブル-hierarchy_example ごと](https://asp.net/media/2578125/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-hierarchy_example_244067cd-b451-4e9b-9595-793b9afca505.png)

単一のデータベース テーブルからエンティティの継承構造を生成するには、このパターンと呼ばれます *- table-per-hierarchy* (TPH) 継承します。

代わりに、継承構造と同じように見えるデータベースを作成することもできます。 など名前フィールドのみがある可能性があります、`Person`テーブルいて個別`Instructor`と`Student`日付フィールドを持つテーブル。

![Table-per-type_inheritance](https://asp.net/media/2578131/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-type_inheritance.png)

各エンティティ クラスと呼ばれる、データベース テーブルを作成するこのパターン*table-per-type* (TPT) 継承します。

TPH 継承パターンは、TPT パターンが複雑な結合クエリのため一般にパフォーマンスを向上させると、TPT 継承パターンよりも、Entity Framework で提供します。 このチュートリアルでは、TPH 継承の実装方法を示します。 次の手順を実行することによってを実行してみましょう。

- 作成、`Person`クラスし、変更、`Instructor`と`Student`クラスから派生する`Person`します。
- データベースへのモデルのマッピング コードをデータベース コンテキスト クラスに追加します。
- 変更`InstructorID`と`StudentID`参照をプロジェクト全体にわたって`PersonID`します。

## <a name="creating-the-person-class"></a>Person クラスを作成します。

 注: これらのクラスを使用するコント ローラーを更新するまでは、次のクラスを作成した後、プロジェクトをコンパイルできません。 

*モデル*フォルダー作成*Person.cs*テンプレート コードを次のコードに置き換えます。

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

*Instructor.cs*、派生、`Instructor`クラスから、`Person`クラスし、キーと名前のフィールドを削除します。 コードは次の例のように表示されます。

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

同様に変更する*Student.cs*します。 `Student`クラスは次の例のようになります。

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="adding-the-person-entity-type-to-the-model"></a>Person エンティティ型をモデルに追加します。

*SchoolContext.cs*、追加、`DbSet`プロパティを`Person`エンティティの種類。

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Table-per-Hierarchy 継承を構成するために Entity Framework に必要なのことはこれですべてです。 わかる、データベースが再作成すると、`Person`テーブルの代わりに、`Student`と`Instructor`テーブル。

## <a name="changing-instructorid-and-studentid-to-personid"></a>PersonID に InstructorID と学生 Id を変更します。

*SchoolContext.cs*、インストラクター コース マッピング ステートメントで次のように変更します`MapRightKey("InstructorID")`に`MapRightKey("PersonID")`:。

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=4)]

この変更が必要です。多対多の結合テーブルで InstructorID 列の名前を変更するだけです。 InstructorID と名前のままの場合、アプリケーションが正しく機能もします。 ここでは、完成した*SchoolContext.cs*:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=15,24)]

次を変更する必要があります`InstructorID`に`PersonID`と`StudentID`に`PersonID`プロジェクト全体にわたって***を除く***内の移行のタイムスタンプ付きのファイルで、*移行*フォルダー。 そのために検索して、変更する必要のあるファイルのみを開くし、開いているファイルのグローバルな変更を実行します。 唯一のファイル、*移行*フォルダーを変更する必要がありますが*migrations \configuration.cs します。*

1. > [!IMPORTANT]
   > Visual Studio で開いているすべてのファイルを閉じることから始めます。
2. をクリックして**検索し置換--すべてのファイルを検索する**で、**編集**メニューのおよびプロジェクトが含まれているすべてのファイルを検索`InstructorID`します。  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. 内の各ファイルを開き、**検索結果**ウィンドウ***を除く***、&lt;タイムスタンプ&gt;*\_.cs* で移行ファイル*移行*フォルダー、ファイルごとに 1 つの線をダブルクリックします。  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)
4. 開く、**ファイル内の置換**ダイアログと変更**検索**に**すべての開いているドキュメント**。
5. 使用して、**ファイル内の置換**すべて変更するためのダイアログ`InstructorID`に `PersonID.`  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)
6. プロジェクトが含まれているすべてのファイルを検索`StudentID`します。
7. 内の各ファイルを開き、**検索結果**ウィンドウ***を除く***、&lt;タイムスタンプ&gt;*\_\*.cs*移行ファイル*移行*フォルダー、ファイルごとに 1 つの線をダブルクリックします。  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
8. 開く、**ファイル内の置換**ダイアログと変更**検索**に**すべての開いているドキュメント**。
9. 使用して、**ファイル内の置換**すべて変更するためのダイアログ`StudentID`に`PersonID`します。   
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
10. プロジェクトをビルドします。

(これを示す注を*欠点*の`classnameID`主キーの名前付けパターン。 場合は、クラス名をプレフィックスとしてせず主キーの ID をという名前が*ありません*名前を変更する必要があるとようになりました)。

## <a name="create-and-update-a-migrations-file"></a>作成し、移行ファイルを更新

パッケージ マネージャー コンソール (PMC) では、次のコマンドを入力します。

`Add-Migration Inheritance`

実行、 `Update-Database` PMC でコマンド。 移行が処理する方法を認識しない既存のデータがあるので、コマンドはこの時点で失敗します。 次のエラーが発生しました。

*ALTER TABLE ステートメントと競合して外部キー制約"FK\_dbo します。部門\_dbo します。Person\_PersonID"。競合が発生したデータベース"ContosoUniversity"テーブル"dbo します。Person"です。 列 'PersonID'。*

開いている*移行\&lt; タイムスタンプ&gt;\_Inheritance.cs*と置換、`Up`メソッドを次のコード。

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=25,29-40)]

実行、`update-database`コマンドを再実行します。

> [!NOTE]
> データとスキーマの変更を移行するときにその他のエラーが発生することになります。 移行エラーが発生する場合は解決できない場合、接続文字列を変更することで、チュートリアルを続けることができます、 *Web.config*ファイルまたはデータベースを削除します。 データベースの名前を変更する最も簡単なアプローチとしては、 *Web.config*ファイル。 CU へ、データベース名を変更するなど、\_の次の例に示すようにテストします。
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.xml?highlight=1-2)]
> 
> 新しいデータベースを移行するデータがないと、`update-database`コマンドがエラーなしで完了する可能性が高くなります。 データベースを削除する方法の詳細については、次を参照してください。 [Visual Studio 2012 からデータベースを削除する方法](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/)します。 チュートリアルを続行するにはこの方法を実行する場合、配置サイトの移行を自動的に実行時に同じエラーが発生があるために、このチュートリアルの最後に配置手順をスキップします。 移行エラーのトラブルシューティングを行う場合は、最適なリソースは、Entity Framework のフォーラムまたは StackOverflow.com のいずれか。


## <a name="testing"></a>テスト

サイトを実行し、さまざまなページをお試しください。 すべてが前と同じように動作します。

**サーバー エクスプ ローラー**展開**SchoolContext**し**テーブル**、ことを確認して、**学生**と**インストラクター**テーブルに置換された、 **Person**テーブル。 展開、 **Person**テーブルとするすべてに使用されていた列があるを参照してください、**学生**と**インストラクター**テーブル。

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Person テーブルを右クリックし、**[テーブル データの表示]** をクリックして識別子列を表示します。

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

次の図は、新しい School データベースの構造を示しています。

![School_database_diagram](https://asp.net/media/2578143/Windows-Live-Writer_58f5a93579b2_CC7B_School_database_diagram_6350a801-7199-413f-bbac-4a2009ed19d7.png)

## <a name="summary"></a>まとめ

Table-per-hierarchy 継承が実装されているようになりました、 `Person`、 `Student`、および`Instructor`クラス。 これと他の継承構造の詳細については、次を参照してください。[継承のマッピング方法](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx)Morteza Manavi のブログ。 次のチュートリアルでは、リポジトリと unit of work パターンを実装するためにいくつかの点を確認します。

その他の Entity Framework リソースへのリンクが記載されて、 [ASP.NET データ アクセス コンテンツ マップ](../../../../whitepapers/aspnet-data-access-content-map.md)します。

> [!div class="step-by-step"]
> [前へ](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [次へ](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
