---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
title: Entity Framework 4.0 Database でまず getting Started と ASP.NET 4 Web フォーム - パート 5 |Microsoft Docs
author: tdykstra
description: Contoso University のサンプルの web アプリケーションでは、Entity Framework を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。 サンプル アプリケーションは、.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 24ad4379-3fb2-44dc-ba59-85fe0ffcb2ae
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
msc.type: authoredcontent
ms.openlocfilehash: 0d0ae8385af96cf5e5951ac85f5cf6ae3c201a4d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366677"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-5"></a>Entity Framework 4.0 Database でまず getting Started と ASP.NET 4 Web フォーム - パート 5
====================
によって[Tom Dykstra](https://github.com/tdykstra)

> Contoso University のサンプルの web アプリケーションでは、Entity Framework 4.0 と Visual Studio 2010 を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。 チュートリアル シリーズについては、次を参照してください[シリーズの最初のチュートリアル。](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="working-with-related-data-continued"></a>続き関連データを扱う

使用を開始し、前のチュートリアルで、`EntityDataSource`関連データを操作するコントロール。 階層の複数のレベルを表示し、ナビゲーション プロパティのデータを編集します。 このチュートリアルで追加してリレーションシップを削除して、既存のエンティティの関係がある新しいエンティティを追加することで、関連するデータを操作する進みます。

学科に割り当てられているコースを追加するページを作成します。 各部署が既に存在して、新しいコースを作成するときに同時が関係を確立して、既存の部門間。

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image1.png)

(選択した 2 つのエンティティ間のリレーションシップの追加) コースにインストラクターを割り当てることで、多対多リレーションシップで動作するページを作成することもありますまたはコースの講師を削除する (2 つのエンティティ間のリレーションシップを削除します。オンにします。 データベース内に追加される新しい行の結果、インストラクターとコースの間のリレーションシップの追加、`CourseInstructor`関連テーブル; からの行を削除するリレーションシップは、削除、`CourseInstructor`関連テーブル。 ただし、これを行う Entity Framework で参照することがなくナビゲーション プロパティを設定して、`CourseInstructor`明示的にテーブルです。

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image3.png)

## <a name="adding-an-entity-with-a-relationship-to-an-existing-entity"></a>既存のエンティティにリレーションシップを持つエンティティを追加します。

という名前の新しい web ページを作成する*CoursesAdd.aspx*を使用して、 *Site.Master*マスター ページ、および次のマークアップを追加、`Content`という名前のコントロール`Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample1.aspx)]

このマークアップを作成、`EntityDataSource`を挿入できるようにして、ハンドラーを指定する、コースの選択コントロール、`Inserting`イベント。 ハンドラーの更新に使用する、`Department`ナビゲーション プロパティの新しい`Course`エンティティを作成します。

マークアップを作成も、`DetailsView`新規追加に使用するコントロール`Course`エンティティ。 マークアップにバインドされたフィールドを使用して`Course`エンティティのプロパティ。 入力する必要がある、`CourseID`これがシステムによって生成された ID フィールドではないため、その値します。 代わりに、コースの作成時に手動で指定する必要がありますコース番号になります。

テンプレート フィールドを使用する、`Department`ナビゲーション プロパティとナビゲーション プロパティを使用できないため`BoundField`コントロール。 [テンプレート] フィールドでは、部門を選択するドロップダウン リストを提供します。 ドロップダウン リストにバインドされた、`Departments`エンティティ セットを使用して`Eval`なく`Bind`、もう一度ため、それらを更新するにはナビゲーション プロパティを直接バインドすることはできません。 ハンドラーを指定する、`DropDownList`コントロールの`Init`イベントを更新するコードで使用するためのコントロールへの参照を保存するため、`DepartmentID`外部キーです。

*CoursesAdd.aspx.cs*への参照を保持するためにクラス フィールドを追加、部分クラス宣言の直後後、`DepartmentsDropDownList`コントロール。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample2.cs)]

ハンドラーを追加、`DepartmentsDropDownList`コントロールの`Init`イベントのため、コントロールへの参照を格納することです。 これにより、ユーザーが入力した値を取得し、更新に使用できます、`DepartmentID`の値、`Course`エンティティ。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample3.cs)]

ハンドラーを追加、`DetailsView`コントロールの`Inserting`イベント。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample4.cs)]

ユーザーがクリックすると`Insert`、`Inserting`新しいレコードが挿入される前に、イベントが発生します。 ハンドラーのコードを取得、`DepartmentID`から、`DropDownList`を制御し、使用される値を設定するため、`DepartmentID`のプロパティ、`Course`エンティティ。

Entity Framework が処理するには、このコースを追加する、`Courses`ナビゲーション プロパティ、関連付けられている`Department`エンティティ。 部署をさらに追加、`Department`のナビゲーション プロパティ、`Course`エンティティ。

ページを実行します。

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image5.png)

ID、タイトル、クレジットの数値を入力し、部署を選択します をクリックして**挿入**します。

実行、 *Courses.aspx*ページ、および新しいコースを表示する同じ部門を選択します。

[![Image03](the-entity-framework-and-aspnet-getting-started-part-5/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image7.png)

## <a name="working-with-many-to-many-relationships"></a>多対多リレーションシップの使用

間のリレーションシップ、`Courses`エンティティ セットと`People`エンティティ セットは、多対多リレーションシップ。 A`Course`エンティティという名前のナビゲーション プロパティには`People`0、1、またはその関連する詳細を格納できる`Person`エンティティ (そのコースを教えるに割り当てられている講師を表す)。 および`Person`エンティティという名前のナビゲーション プロパティには`Courses`0、1、またはその関連する詳細を含めることができます`Course`エンティティ (コースを表すについて解説するそのインストラクターが割り当てられます)。 1 人の講師は、複数のコースを学ぶことし、複数の講師が 1 つのコースを担当する可能性があります。 チュートリアルのこのセクションで追加し、間のリレーションシップを削除`Person`と`Course`エンティティ関連エンティティのナビゲーション プロパティを更新します。

という名前の新しい web ページを作成する*InstructorsCourses.aspx*を使用して、 *Site.Master*マスター ページ、および次のマークアップを追加、`Content`という名前のコントロール`Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample5.aspx)]

このマークアップを作成、 `EntityDataSource` 、名前を取得するコントロールと`PersonID`の`Person`インストラクター エンティティ。 A`DropDrownList`コントロールにバインドする、`EntityDataSource`コントロール。 `DropDownList`コントロールのハンドラーを指定する、`DataBound`イベント。 このハンドラーのコースを表示するデータ バインド、2 つのドロップダウン リストを使用します。

マークアップでは、選択したインストラクターにコースの割り当てに使用するコントロールの次のグループも作成されます。

- A`DropDownList`を割り当てるコースを選択するためのコントロール。 選択したインストラクターに現在割り当てられていないコースでは、このコントロールが表示されます。
- A`Button`割り当てを開始するコントロール。
- A`Label`割り当てが失敗した場合、エラー メッセージを表示するコントロール。

最後に、マークアップでは、選択したインストラクターのコースを削除するために使用するコントロールのグループも作成します。

*InstructorsCourses.aspx.cs*を使用して、追加ステートメント。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample6.cs)]

コースを表示する 2 つのドロップダウン リストを設定するためのメソッドを追加します。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample7.cs)]

このコードからすべてのコースを取得する、`Courses`エンティティが設定されからコースを取得、`Courses`のナビゲーション プロパティ、`Person`選択したインストラクター エンティティ。 コースは、そのインストラクターに割り当てられているかを決定し、ドロップダウン リストを適宜設定します。

ハンドラーを追加、`Assign`ボタンの`Click`イベント。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample8.cs)]

このコードを取得、`Person`選択されたインストラクターのエンティティを取得、`Course`選択したコースのエンティティを選択したコースを追加します、`Courses`のインストラクターのナビゲーション プロパティ`Person`エンティティ。 データベースに変更を保存し、結果をすぐに確認できるように、ドロップダウン リストを再作成します。

ハンドラーを追加、`Remove`ボタンの`Click`イベント。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample9.cs)]

このコードを取得、`Person`選択されたインストラクターのエンティティを取得、`Course`選択したコースのエンティティから選択したコースを削除し、`Person`エンティティの`Courses`ナビゲーション プロパティ。 データベースに変更を保存し、結果をすぐに確認できるように、ドロップダウン リストを再作成します。

コードを追加して、`Page_Load`ことを確認して、エラー メッセージは、メソッドはレポートと用のハンドラーを追加するとエラーがない場合に表示されない、`DataBound`と`SelectedIndexChanged`コースのドロップダウン リストを設定するインストラクターのドロップダウン リストのイベント。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample10.cs)]

ページを実行します。

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image9.png)

インストラクターを選択します。 <strong>コースを割り当てる</strong>、インストラクターが指導するコースを表示するドロップダウン リスト、<strong>コースを削除</strong>ドロップダウン リストに既に割り当てられている、インストラクター コースを表示します。 <strong>コースを割り当てる</strong>セクション、コースを選択し、クリックして<strong>割り当てる</strong>します。 コースに移動、<strong>コースを削除</strong>ドロップダウン リスト。 コースを選択、<strong>コースを削除</strong>セクションし、をクリックして<strong>削除</strong><em>します。</em> コースに移動、<strong>コースを割り当てる</strong>ドロップダウン リスト。

関連データを操作する方法がいくつか見てきました。 次のチュートリアルでは、アプリケーションの保守容易性を向上させるために、データ モデルで継承を使用する方法を学習します。

> [!div class="step-by-step"]
> [前へ](the-entity-framework-and-aspnet-getting-started-part-4.md)
> [次へ](the-entity-framework-and-aspnet-getting-started-part-6.md)
