---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
title: "データベースの概要 Entity Framework 4.0 最初および ASP.NET 4 Web フォーム - パート 5 |Microsoft ドキュメント"
author: tdykstra
description: "Contoso 大学でサンプル web アプリケーションでは、Entity Framework を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。 サンプル アプリケーションは、."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 24ad4379-3fb2-44dc-ba59-85fe0ffcb2ae
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
msc.type: authoredcontent
ms.openlocfilehash: 7200899d54585cd09e0a648e3aaaf839db2649e0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-5"></a>データベースの概要 Entity Framework 4.0 最初および ASP.NET 4 Web フォーム - パート 5
====================
によって[Tom Dykstra](https://github.com/tdykstra)

> Contoso 大学でサンプル web アプリケーションでは、Entity Framework 4.0 および Visual Studio 2010 を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。 一連のチュートリアルについては、次を参照してください[系列内の最初のチュートリアル。](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="working-with-related-data-continued"></a>関連するデータを扱う続き

前のチュートリアルで使用を開始し、`EntityDataSource`関連データを操作するコントロール。 複数レベルの階層を表示し、ナビゲーション プロパティでデータを編集します。 このチュートリアルで追加してリレーションシップを削除して、既存のエンティティの関係がある新しいエンティティを追加することで、関連するデータを操作に進みます。

部門に割り当てられているコースを追加するページを作成します。 部門が既に存在して、新しいコースを作成するときに、同時にあります関係を確立すると、既存の部門とします。

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image1.png)

(選択した 2 つのエンティティ間のリレーションシップの追加) コースをインストラクターを割り当てることによって、多対多リレーションシップで動作するページを作成することもありますまたはコースの、インストラクターを削除する (2 つのエンティティ間のリレーションシップを削除します。オンにします。 データベース内に追加される新しい行の結果、インストラクターとコースの間のリレーションシップの追加、`CourseInstructor`関連テーブル以外の場合は、リレーションシップから行を削除する削除、`CourseInstructor`関連テーブル。 ただし、これを行う、Entity framework を参照しなくても、ナビゲーション プロパティの設定によって、`CourseInstructor`明示的にテーブルです。

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image3.png)

## <a name="adding-an-entity-with-a-relationship-to-an-existing-entity"></a>既存のエンティティへのリレーションシップを持つエンティティの追加

という名前の新しい web ページを作成する*CoursesAdd.aspx*を使用して、 *Site.Master*マスター ページ、および次のマークアップを追加、`Content`という名前のコントロール`Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample1.aspx)]

このマークアップを作成、`EntityDataSource`コースを選択するの挿入を有効にするを指定するコントロールのハンドラーを`Inserting`イベント。 ハンドラーは、更新に使用する、`Department`ナビゲーション プロパティの新しい`Course`エンティティを作成します。

またマークアップを作成、`DetailsView`新規追加に使用するコントロール`Course`エンティティです。 マークアップのバインドされたフィールドを使用して`Course`エンティティのプロパティです。 入力する必要がある、`CourseID`値のため、これはシステムによって生成された ID フィールドではありません。 代わりに、これは、コースの作成時に手動で指定する必要がありますをコース番号です。

テンプレートのフィールドを使用する、`Department`ナビゲーション プロパティとナビゲーション プロパティを使用できないため`BoundField`コントロール。 [テンプレート] フィールドでは、部門を選択するドロップダウン リストを提供します。 ドロップダウン リストにバインドされて、`Departments`エンティティ セットを使用して`Eval`なく`Bind`、もう一度ため、それらを更新するためにナビゲーション プロパティを直接バインドすることはできません。 ハンドラーを指定する、`DropDownList`コントロールの`Init`イベントを更新するコードによって使用される、コントロールへの参照を保存すること、`DepartmentID`外部キーです。

*CoursesAdd.aspx.cs*部分クラス宣言の直後にへの参照を保持するためにクラス フィールドの追加、`DepartmentsDropDownList`コントロール。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample2.cs)]

ハンドラーを追加、`DepartmentsDropDownList`コントロールの`Init`イベント コントロールへの参照を保存することです。 これにより、ユーザーが入力した値を取得および更新に使用できます、`DepartmentID`の値、`Course`エンティティです。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample3.cs)]

ハンドラーを追加、`DetailsView`コントロールの`Inserting`イベント。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample4.cs)]

ユーザーがクリックしたとき`Insert`、`Inserting`イベントは、新しいレコードが挿入される前に発生します。 ハンドラーのコードを取得、`DepartmentID`から、`DropDownList`制御に使用される値の設定を使用して、`DepartmentID`のプロパティ、`Course`エンティティです。

Entity Framework に対処するには、このコースを追加する、`Courses`関連付けられているナビゲーション プロパティ`Department`エンティティです。 部門にも追加、`Department`のナビゲーション プロパティ、`Course`エンティティです。

ページを実行します。

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image5.png)

ID、タイトル、クレジットの数を入力してください、部門を選択し、をクリックして**挿入**です。

実行、 *Courses.aspx*ページ、および新しいコースを表示する同じ部門を選択します。

[![Image03](the-entity-framework-and-aspnet-getting-started-part-5/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image7.png)

## <a name="working-with-many-to-many-relationships"></a>多対多リレーションシップの使用

間のリレーションシップ、`Courses`エンティティ セットと`People`エンティティ セットは、多対多のリレーションシップ。 A`Course`エンティティがという名前のナビゲーション プロパティ`People`0、1、またはその関連する詳細を持つことができる`Person`エンティティ (そのコースを教えるに割り当てられている講習においてインストラクターを表す) です。 および`Person`という名前のナビゲーション プロパティがエンティティにが`Courses`0、1、またはその関連する詳細を含めることができます`Course`エンティティ (コースの指導を求めましたその講師が割り当てられていることを表す) です。 講師が 1 つは、複数のコースを教えることがあり、複数講習においてインストラクターによって 1 つのコースを教える可能性があります。 チュートリアルのこのセクションに追加して間のリレーションシップを削除する`Person`と`Course`関連エンティティのナビゲーション プロパティを更新することでエンティティです。

という名前の新しい web ページを作成する*InstructorsCourses.aspx*を使用して、 *Site.Master*マスター ページ、および次のマークアップを追加、`Content`という名前のコントロール`Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample5.aspx)]

このマークアップを作成、`EntityDataSource`名前を取得するコントロールと`PersonID`の`Person`講習においてインストラクター エンティティです。 A`DropDrownList`コントロールにバインドする、`EntityDataSource`コントロール。 `DropDownList`コントロールのハンドラーを指定する、`DataBound`イベント。 このハンドラーのコースを表示するデータ バインド、2 つのドロップダウン リストを使用します。

また、マークアップを選択した講師コースに割り当てるために使用するコントロールの次のグループが作成されます。

- A`DropDownList`に割り当てるコースを選択するためのコントロールです。 このコントロールは、選択したインストラクターに現在割り当てられていないコースで入力されます。
- A`Button`割り当てを開始するコントロール。
- A`Label`コントロールに割り当てが失敗した場合、エラー メッセージが表示されます。

最後に、マークアップでは、選択した講師からコースを削除するのに使用するコントロールのグループも作成します。

*InstructorsCourses.aspx.cs*を使用して、追加のステートメント。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample6.cs)]

コースを表示する 2 つのドロップダウン リストを設定するためのメソッドを追加します。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample7.cs)]

このコードの取得からすべてのコース、`Courses`セットされからコースを取得するエンティティ、`Courses`のナビゲーション プロパティ、`Person`講師が選択されているエンティティ。 そのインストラクターに割り当てられているどのコースを決定し、それに応じてドロップダウン リストを設定します。

ハンドラーを追加、`Assign`ボタンの`Click`イベント。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample8.cs)]

このコードを取得、`Person`講師が、選択したエンティティを取得、`Course`選択のコースのエンティティを選択したコースを追加し、`Courses`インストラクターのナビゲーション プロパティ`Person`エンティティです。 データベースに変更を保存し、結果はすぐに表示されることができますので、ドロップダウン リストを再作成します。

ハンドラーを追加、`Remove`ボタンの`Click`イベント。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample9.cs)]

このコードを取得、`Person`講師が、選択したエンティティを取得、`Course`選択のコースのエンティティから選択したコースを削除し、`Person`エンティティの`Courses`ナビゲーション プロパティ。 データベースに変更を保存し、結果はすぐに表示されることができますので、ドロップダウン リストを再作成します。

コードを追加して、`Page_Load`ことを確認して、エラー メッセージは、メソッドはレポートには、しのハンドラーを追加するとエラーがない場合に表示されない、`DataBound`と`SelectedIndexChanged`コースのドロップダウン リストを設定するインストラクター ドロップダウン リストのイベント。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample10.cs)]

ページを実行します。

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image9.png)

あるインストラクターを選択します。 **コースを割り当てる**インストラクターしない講義では、コースがドロップダウン リストに表示されます、**コースを削除する**ドロップダウン リストには、インストラクターに既に割り当てられているコースが表示されます。 **コースを割り当てる**セクションでコースを選択し、をクリックして**割り当てる**です。 コースに移動、**コースを削除する**ドロップダウン リスト。 コースを選択、**コースを削除する**セクションし、をクリックして**削除***です。* コースに移動、**コースを割り当てる**ドロップダウン リスト。

関連するデータを操作する方法がいくつか見てきました。 次のチュートリアルでは、アプリケーションの保守容易性を向上させるために、データ モデルでの継承を使用する方法を学習します。

>[!div class="step-by-step"]
[前へ](the-entity-framework-and-aspnet-getting-started-part-4.md)
[次へ](the-entity-framework-and-aspnet-getting-started-part-6.md)
