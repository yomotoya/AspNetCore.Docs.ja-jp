---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
title: Entity Framework 4.0 Database でまず getting Started と ASP.NET 4 Web フォームの第 7 部 |Microsoft Docs
author: tdykstra
description: Contoso University のサンプルの web アプリケーションでは、Entity Framework を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。 サンプル アプリケーションは、.
ms.author: riande
ms.date: 12/03/2010
ms.assetid: f8afb245-b705-419c-8790-0b295e90d5e2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
msc.type: authoredcontent
ms.openlocfilehash: 48092838ac0b00137ae0a4e37391c31883afcd09
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838053"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-7"></a>Entity Framework 4.0 Database でまず getting Started と ASP.NET 4 Web フォーム - パート 7
====================
によって[Tom Dykstra](https://github.com/tdykstra)

> Contoso University のサンプルの web アプリケーションでは、Entity Framework 4.0 と Visual Studio 2010 を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。 チュートリアル シリーズについては、次を参照してください[シリーズの最初のチュートリアル。](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-stored-procedures"></a>ストアド プロシージャの使用

前のチュートリアルでは、table-per-hierarchy 継承パターンを実装しました。 このチュートリアルでは、ストアド プロシージャを使用して、データベースのアクセスをより詳細に制御する方法を説明します。

Entity Framework では、データベースへのアクセスのストアド プロシージャを使用することを指定することができます。 任意のエンティティ型の作成、更新、またはその型のエンティティを削除するのに使用するストアド プロシージャを指定できます。 データ モデルのエンティティのセットの取得などのタスクを実行するのに使用できるストアド プロシージャへの参照を追加できます。

データベースへのアクセスの一般的な要件は、ストアド プロシージャを使用します。 場合によっては、データベース管理者は、セキュリティ上の理由からストアド プロシージャを介してすべてのデータベース アクセスが行われる必要があります。 それ以外の場合、Entity Framework がデータベースを更新する際に使用するプロセスの一部にビジネス ロジックをビルドする場合があります。 たとえば、エンティティを削除するたびに、アーカイブ データベースにコピーする可能性があります。 または、変更を行ったユーザーを記録するログ記録テーブルに行を記述する場合、行が更新されるたびにします。 Entity Framework のエンティティを削除またはエンティティを更新するたびに呼び出されるストアド プロシージャでは、このようなタスクを実行できます。

前のチュートリアルでは、ようにしない新しいページを作成します。 代わりに、Entity Framework を既に作成してページの一部のデータベースにアクセスする方法を変更します。

このチュートリアルでは、挿入するため、データベース内のストアド プロシージャを作成します`Student`と`Instructor`エンティティ。 データ モデルに追加され、エンティティ フレームワークが追加するために使用するかを指定します`Student`と`Instructor`エンティティをデータベースにします。 取得に使用できるストアド プロシージャを作成することもあります。`Course`エンティティ。

## <a name="creating-stored-procedures-in-the-database"></a>データベースのストアド プロシージャの作成

(使用する場合、 *School.mdf*ファイル、ストアド プロシージャが既に存在するため、このチュートリアルでダウンロード可能なプロジェクトからこのセクションをスキップすることができます)。

**サーバー エクスプ ローラー**、展開*School.mdf*を右クリックして**ストアド プロシージャ**、選択と**新しいストアド プロシージャの追加**します。

[![image15](the-entity-framework-and-aspnet-getting-started-part-7/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image1.png)

次の SQL ステートメントをコピーし、そのウィンドウに貼り付けストアド プロシージャ、スケルトンのストアド プロシージャを置き換えます。

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample1.sql)]

[![image14](the-entity-framework-and-aspnet-getting-started-part-7/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image3.png)

`Student` エンティティがある 4 つのプロパティ: `PersonID`、 `LastName`、 `FirstName`、および`EnrollmentDate`します。 データベース ID 値を自動的に生成して、ストアド プロシージャは、その他の 3 つのパラメーターを受け取ります。 ストアド プロシージャは、Entity Framework の追跡できるをメモリ内に保持するエンティティのバージョンにするために、新しい行のレコードのキーの値を返します。

保存して、ストアド プロシージャ ウィンドウを閉じます。

作成、`InsertInstructor`ストアド プロシージャを次の SQL ステートメントを使用して、同じ方法で。

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample2.sql)]

作成`Update`ストアド プロシージャの`Student`と`Instructor`エンティティもします。 (データベースは既に、`DeletePerson`ストアド プロシージャの両方で動作する`Instructor`と`Student`エンティティです)。

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample3.sql)]

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample4.sql)]

このチュートリアルでは、各エンティティ型のすべての 3 つ関数 - insert、update、および delete--をマップします。 Entity Framework バージョン 4 では、1 つだけマップできますまたは、他のユーザー、1 つの例外を割り当てずにストアド プロシージャに関数のこれら 2 つ: Entity Framework で例外がスローされますが、更新関数、not delete 関数をマップした場合とする。エンティティを削除しようとしてください。 ストアド プロシージャのマッピングでこれほどの柔軟性がありません、Entity Framework version 3.5 で: 3 つすべてをマップする必要があった場合は 1 つの関数をマップします。

データを更新するのではなく、読み取りをストアド プロシージャを作成するには、すべてを選択する 1 つを作成`Course`エンティティは、次の SQL ステートメントを使用します。

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample5.sql)]

## <a name="adding-the-stored-procedures-to-the-data-model"></a>データ モデルにストアド プロシージャを追加します。

ストアド プロシージャは、データベースで定義されているようになりましたが、これらは、Entity Framework を使用できるようにするデータ モデルに追加する必要があります。 開いている*SchoolModel.edmx*をデザイン画面を右クリックし、**データベースからモデルを更新**します。 **追加**のタブ、**データベース オブジェクトの選択** ダイアログ ボックスで、展開**ストアド プロシージャ**、新しく作成されたストアド プロシージャを選択し、 `DeletePerson`ストアド プロシージャ、およびクリックして**完了**します。

[![image20](the-entity-framework-and-aspnet-getting-started-part-7/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image5.png)

## <a name="mapping-the-stored-procedures"></a>ストアド プロシージャのマッピング

データ モデル デザイナーで右クリックし、`Student`エンティティと選択**ストアド プロシージャ マッピング**します。

[![image21](the-entity-framework-and-aspnet-getting-started-part-7/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image7.png)

**マッピングの詳細**ウィンドウが表示されたら、Entity Framework が挿入、更新、および、この型のエンティティを削除するのに使用するストアド プロシージャを指定することができます。

[![image22](the-entity-framework-and-aspnet-getting-started-part-7/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image9.png)

設定、**挿入**関数を**InsertStudent**します。 ウィンドウには、ストアド プロシージャのパラメーターを使用して、それぞれのエンティティ プロパティにマップする必要がありますの一覧が表示されます。 名前が同じであるためには、自動的にマップのこれら 2 つです。 という名前のエンティティ プロパティはありません`FirstName`で手動で選択する必要があるため、`FirstMidName`使用可能なエンティティのプロパティを表示するドロップダウン リストから。 (これは、名前を変更したため、`FirstName`プロパティを`FirstMidName`最初のチュートリアルです)。

[![image23](the-entity-framework-and-aspnet-getting-started-part-7/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image11.png)

同じ**マッピングの詳細**ウィンドウで、マップ、`Update`関数を`UpdateStudent`ストアド プロシージャ (を指定するかどうかを確認`FirstMidName`のパラメーター値として`FirstName`の場合と同様、 `Insert`ストアド プロシージャ) および`Delete`関数を`DeletePerson`ストアド プロシージャ。

[![image01](the-entity-framework-and-aspnet-getting-started-part-7/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image13.png)

インストラクターにストアド プロシージャを挿入、更新、および削除をマップするのと同じ手順に従って、`Instructor`エンティティ。

[![image02](the-entity-framework-and-aspnet-getting-started-part-7/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image15.png)

使用するデータの更新ではなく読み取りなストアド プロシージャの場合、**モデル ブラウザー**ストアド プロシージャをエンティティにマップするウィンドウに入力を返します。 データ モデル デザイナーでデザイン サーフェスと選択を右クリックして**モデル ブラウザー**します。 開く、 **SchoolModel.Store**ノードと順に開いて、 **Stored Procedures**ノード。 右クリックし、`GetCourses`ストアド プロシージャと選択**関数インポートの追加**します。

[![image24](the-entity-framework-and-aspnet-getting-started-part-7/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image17.png)

**関数インポートの追加**ダイアログ ボックスで、**コレクションの返します**選択**エンティティ**、し、`Course`エンティティ型として返されます。 完了したら、クリックして**OK**します。 保存して閉じます、 *.edmx*ファイル。

[![image25](the-entity-framework-and-aspnet-getting-started-part-7/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image19.png)

## <a name="using-insert-update-and-delete-stored-procedures"></a>Insert を使用して、更新、およびストアド プロシージャの削除

ストアド プロシージャを挿入するには、更新、および、データの削除は、Entity Framework によって自動的に使用、データ モデルに追加し、それらを適切なエンティティにマップした後。 今すぐ実行することができます、 *StudentsAdd.aspx*  ページで、新しい学生を作成するたびに、Entity Framework を使用して、`InsertStudent`ストアド プロシージャに新しい行を追加する、`Student`テーブル。

[![image03](the-entity-framework-and-aspnet-getting-started-part-7/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image21.png)

実行、 *Students.aspx*ページと、新しい学生が、一覧に表示されます。

[![image04](the-entity-framework-and-aspnet-getting-started-part-7/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image23.png)

Update 関数が動作することを確認する名前を変更し、受講者の機能の削除の動作を確認するを削除します。

[![image05](the-entity-framework-and-aspnet-getting-started-part-7/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image25.png)

## <a name="using-select-stored-procedures"></a>Select ストアド プロシージャの使用

Entity Framework は自動的に実行されないストアド プロシージャなど`GetCourses`でそれらを使用することはできませんし、`EntityDataSource`コントロール。 を使用するには、そのコードから呼び出します。

開く、 *InstructorsCourses.aspx.cs*ファイル。 `PopulateDropDownLists`メソッドでは、LINQ to Entities クエリを使用して、一覧をループしてに、インストラクターが割り当てられているものと、割り当てが解除されるかを判断できるようにすべての course エンティティを取得します。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample6.cs)]

これは、次のコードで置き換えます。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample7.cs)]

では、ページ、`GetCourses`ストアド プロシージャをすべてのコースの一覧を取得します。 前に、と同様に動作することを確認するページを実行します。

(ストアド プロシージャで取得したエンティティのナビゲーション プロパティが自動的が設定されませんに応じて、これらのエンティティに関連するデータ`ObjectContext`既定の設定。 詳細については、次を参照してください[関連オブジェクトの読み込み](https://msdn.microsoft.com/library/bb896272.aspx)、MSDN ライブラリです。)。

次のチュートリアルでは、Dynamic Data 機能を使用して、プログラムとテスト データの書式設定と検証規則に容易にできるようにする方法を学習します。 などのデータの書式指定文字列には、各 web ページ ルールとフィールドが必要かどうかを指定することではなくデータ モデルのメタデータでこのようなルールを指定することができ、すべてのページに自動的に適用します。

> [!div class="step-by-step"]
> [前へ](the-entity-framework-and-aspnet-getting-started-part-6.md)
> [次へ](the-entity-framework-and-aspnet-getting-started-part-8.md)
