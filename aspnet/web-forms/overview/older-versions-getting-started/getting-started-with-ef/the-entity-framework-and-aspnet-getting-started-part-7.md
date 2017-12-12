---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
title: "データベースの概要 Entity Framework 4.0 最初および ASP.NET 4 Web フォームの第 7 部 |Microsoft ドキュメント"
author: tdykstra
description: "Contoso 大学でサンプル web アプリケーションでは、Entity Framework を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。 サンプル アプリケーションは、."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: f8afb245-b705-419c-8790-0b295e90d5e2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
msc.type: authoredcontent
ms.openlocfilehash: 7697763b97e36304d686c77e8cedd060d630c530
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-7"></a>データベースの概要 Entity Framework 4.0 最初および ASP.NET 4 Web フォームの第 7 部
====================
によって[Tom Dykstra](https://github.com/tdykstra)

> Contoso 大学でサンプル web アプリケーションでは、Entity Framework 4.0 および Visual Studio 2010 を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。 一連のチュートリアルについては、次を参照してください[系列内の最初のチュートリアル。](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-stored-procedures"></a>ストアド プロシージャの使用

前のチュートリアルでは、テーブルの階層あたりの継承パターンを実装します。 このチュートリアルでは、ストアド プロシージャを使用して、データベースのアクセスをより詳細に制御する方法を示します。

Entity Framework では、データベースへのアクセスのストアド プロシージャを使用するように指定することができます。 任意のエンティティ型の作成、更新、またはその型のエンティティの削除に使用するストアド プロシージャを指定できます。 データ モデルでは、エンティティのセットを取得するなどのタスクの実行に使用できるストアド プロシージャへの参照を追加できます。

データベースへのアクセスの一般的な要件は、ストアド プロシージャを使用します。 場合によっては、データベース管理者は、すべてのデータベースへのアクセスがセキュリティ上の理由のためのストアド プロシージャを介して行われる必要があります。 それ以外の場合、データベースを更新するときに、Entity Framework が使用するプロセスの一部にビジネス ロジックを構築する可能性があります。 たとえば、エンティティを削除するたびに可能性があるアーカイブ データベースにコピーします。 または、行が更新されるたびには、変更を行ったユーザーを記録するログ テーブルに行を記述することができます。 Entity Framework のエンティティを削除またはエンティティを更新するたびに呼び出されるストアド プロシージャでは、このようなタスクを実行できます。

前のチュートリアルと同様にない新しいページを作成します。 代わりに、Entity Framework をまだ作成して、ページの一部のデータベースにアクセスする方法を変更します。

このチュートリアルでは、挿入するため、データベース内のストアド プロシージャを作成します`Student`と`Instructor`エンティティです。 データ モデルを追加して、Entity Framework 必要がありますを追加するために使用を指定します`Student`と`Instructor`エンティティをデータベースにします。 取得に使用できるストアド プロシージャを作成することもあります。`Course`エンティティです。

## <a name="creating-stored-procedures-in-the-database"></a>データベースでストアド プロシージャの作成

(使用する場合、 *School.mdf*ファイル プロジェクトからこのチュートリアルをダウンロードできる、ストアド プロシージャが既に存在するため、このセクションをスキップできます)。

**サーバー エクスプ ローラー**、展開*School.mdf*を右クリックして**Stored Procedures**を選択し、**新しいストアド プロシージャの追加**です。

[![image15](the-entity-framework-and-aspnet-getting-started-part-7/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image1.png)

次の SQL ステートメントをコピーし、貼り付けるストアド プロシージャ ウィンドウで、スケルトンのストアド プロシージャを置換します。

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample1.sql)]

[![image14](the-entity-framework-and-aspnet-getting-started-part-7/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image3.png)

`Student`エンティティがある 4 つのプロパティ: `PersonID`、 `LastName`、 `FirstName`、および`EnrollmentDate`です。 データベース ID 値を自動的に生成して、ストアド プロシージャは、その他の 3 つのパラメーターを受け取ります。 ストアド プロシージャは、Entity Framework を追跡できますをメモリに保持するエンティティのバージョンように、新しい行のレコードのキーの値を返します。

保存して、ストアド プロシージャ ウィンドウを閉じます。

作成、`InsertInstructor`ストアド プロシージャを次の SQL ステートメントを使用して、同じ方法で。

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample2.sql)]

作成`Update`ストアド プロシージャの`Student`と`Instructor`エンティティもします。 (データベースは既に、`DeletePerson`ストアド プロシージャの両方で動作する`Instructor`と`Student`エンティティです)。

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample3.sql)]

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample4.sql)]

このチュートリアルではエンティティ型ごとのすべての 3 つ関数--insert、update、および delete--をマップします。 Entity Framework バージョン 4 では、1 つだけマップできますまたは、他のユーザー、1 つの例外を割り当てずにストアド プロシージャにこれらの 2 つの関数: Entity Framework は例外をスロー マップする場合、update 関数が、削除関数ではない、ときにする。エンティティを削除しようとしてください。 Entity Framework バージョン 3.5 ではありませんでしたストアド プロシージャのマッピングにこれほどの柔軟性: 3 つすべてをマップする必要がある 1 つの関数をマップする場合。

データを更新するのではなく、読み取りをストアド プロシージャを作成するには、すべて選択する 1 つを作成`Course`エンティティ、次の SQL ステートメントを使用します。

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample5.sql)]

## <a name="adding-the-stored-procedures-to-the-data-model"></a>データ モデルにストアド プロシージャを追加します。

ストアド プロシージャは、データベースで定義されているようになりましたが、これらは、Entity Framework に使用できるようにするデータ モデルに追加する必要があります。 開いている*SchoolModel.edmx*をデザイン サーフェイスを右クリックし、**データベースからモデルを更新**です。 **追加**のタブ、**データベース オブジェクトの選択** ダイアログ ボックスで、展開**Stored Procedures**、新しく作成されたストアド プロシージャを選択し、 `DeletePerson`ストアド プロシージャをクリックして**完了**です。

[![image20](the-entity-framework-and-aspnet-getting-started-part-7/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image5.png)

## <a name="mapping-the-stored-procedures"></a>ストアド プロシージャのマッピング

データ モデル デザイナー内を右クリックし、`Student`エンティティと選択**ストアド プロシージャ マッピング**です。

[![image21](the-entity-framework-and-aspnet-getting-started-part-7/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image7.png)

**マッピングの詳細**ウィンドウが表示されたら、Entity Framework は、挿入、更新、およびこの型のエンティティの削除に使用するストアド プロシージャを指定することができます。

[![image22](the-entity-framework-and-aspnet-getting-started-part-7/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image9.png)

設定、**挿入**関数**InsertStudent**です。 ウィンドウは、エンティティ プロパティにそれぞれがマップする必要があります、ストアド プロシージャのパラメーターの一覧を示します。 これらの 2 つが自動的にマップ名前が同じであるためです。 名前付きエンティティのプロパティが存在しない`FirstName`手動で選択する必要がありますので、`FirstMidName`使用可能なエンティティのプロパティを表示するドロップダウン リストからです。 (これは、名前を変更したため、`FirstName`プロパティを`FirstMidName`内の最初のチュートリアルです)。

[![image23](the-entity-framework-and-aspnet-getting-started-part-7/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image11.png)

同じ**マッピングの詳細**ウィンドウで、マップ、`Update`関数を`UpdateStudent`ストアド プロシージャ (を指定するかどうかを確認`FirstMidName`のパラメーター値として`FirstName`場合と同様、 `Insert`ストアド プロシージャ) および`Delete`関数を`DeletePerson`ストアド プロシージャです。

[![image01](the-entity-framework-and-aspnet-getting-started-part-7/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image13.png)

講習においてインストラクターにストアド プロシージャの挿入、更新、および削除をマップする同じ手順に従って、`Instructor`エンティティです。

[![image02](the-entity-framework-and-aspnet-getting-started-part-7/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image15.png)

使用するデータの更新ではなく読み取りストアド プロシージャの場合、**モデル ブラウザー**ストアド プロシージャをエンティティにマップするウィンドウから、入力を返します。 データ モデル デザイナーでデザイン画面と選択を右クリックして**モデル ブラウザー**です。 開く、 **SchoolModel.Store**ノードし、開きます、 **Stored Procedures**ノード。 右クリックし、`GetCourses`ストアド プロシージャと選択**関数インポートの追加**です。

[![image24](the-entity-framework-and-aspnet-getting-started-part-7/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image17.png)

**関数インポートの追加**ダイアログ ボックスで、**コレクションを返します**選択**エンティティ**、し、`Course`エンティティ型として返されます。 完了したら、をクリックして**OK**です。 保存して閉じます、 *.edmx*ファイル。

[![image25](the-entity-framework-and-aspnet-getting-started-part-7/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image19.png)

## <a name="using-insert-update-and-delete-stored-procedures"></a>Insert を使用して、Update、Delete ストアド プロシージャ

ストアド プロシージャを挿入するには、更新、およびデータの削除は、Entity Framework によって自動的に使用、データ モデルに追加し、それらを適切なエンティティにマップした後です。 今すぐ実行することができます、 *StudentsAdd.aspx*  ページで、新しい学生を作成するたびに、Entity Framework を使用して、`InsertStudent`ストアド プロシージャに新しい行を追加する、`Student`テーブル。

[![image03](the-entity-framework-and-aspnet-getting-started-part-7/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image21.png)

実行、 *Students.aspx*ページおよび新しい学生が一覧に表示します。

[![image04](the-entity-framework-and-aspnet-getting-started-part-7/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image23.png)

Update 関数が動作することを確認する名前を変更し、機能の削除が動作することを確認する、受講者を削除します。

[![image05](the-entity-framework-and-aspnet-getting-started-part-7/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image25.png)

## <a name="using-select-stored-procedures"></a>Select ストアド プロシージャの使用

Entity Framework は自動的に実行されないストアド プロシージャなど`GetCourses`、これらを使用することはできませんし、`EntityDataSource`コントロール。 を使用するには、コードから呼び出しです。

開く、 *InstructorsCourses.aspx.cs*ファイル。 `PopulateDropDownLists`メソッドでは、LINQ-エンティティ クエリを使用して、一覧をループし、インストラクターに割り当てられているうち、どれ割り当てが解除されるを決定できるように、すべてのコース エンティティを取得します。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample6.cs)]

これを次のコードで置き換えます。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample7.cs)]

ページを使用して今すぐ、`GetCourses`ストアド プロシージャをすべてのコースの一覧を取得します。 以前と同じように動作することを確認する ページを実行します。

(ストアド プロシージャによって取得されたエンティティのナビゲーション プロパティがありますいない自動的に設定されますに応じて、これらのエンティティに関連するデータ`ObjectContext`既定の設定。 詳細については、次を参照してください[関連オブジェクトの読み込み](https://msdn.microsoft.com/en-us/library/bb896272.aspx)、MSDN ライブラリです。)。

次のチュートリアルでは、Dynamic Data 機能を使用して、プログラムとテスト データの書式設定と検証規則に容易にできるようにする方法を学習します。 などのデータの書式指定文字列には、各 web ページ ルールとフィールドが必要かどうかを指定することではなくデータ モデルのメタデータでこのようなルールを指定することができ、すべてのページに自動的に適用します。

>[!div class="step-by-step"]
[前へ](the-entity-framework-and-aspnet-getting-started-part-6.md)
[次へ](the-entity-framework-and-aspnet-getting-started-part-8.md)
