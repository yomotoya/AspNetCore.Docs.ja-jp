---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
title: データベースの概要 Entity Framework 4.0 最初および ASP.NET 4 Web フォーム - パート 4 |Microsoft ドキュメント
author: tdykstra
description: Contoso 大学でサンプル web アプリケーションでは、Entity Framework を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。 サンプル アプリケーションは、.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: ceb9e60f-957c-4d25-9331-cc527de96a33
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
msc.type: authoredcontent
ms.openlocfilehash: 6bea5f4faeb0a9c11a406a7e4e87c4929eda6a18
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30886870"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-4"></a>データベースの概要 Entity Framework 4.0 最初および ASP.NET 4 Web フォーム - パート 4
====================
によって[Tom Dykstra](https://github.com/tdykstra)

> Contoso 大学でサンプル web アプリケーションでは、Entity Framework 4.0 および Visual Studio 2010 を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。 一連のチュートリアルについては、次を参照してください[系列内の最初のチュートリアル。](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="working-with-related-data"></a>関連するデータの使用

前のチュートリアルでは使用して、`EntityDataSource`フィルター、並べ替え、およびデータをグループ化を制御します。 このチュートリアルを表示し、関連するデータを更新します。

講習においてインストラクターの一覧を表示するインストラクター ページを作成します。 あるインストラクターを選択するとそのインストラクターでコースの一覧を参照してください。 コースを選択すると、コースを受講者コースに登録済みの一覧の詳細を参照してください。 教師名、入社日とオフィス割り当てを編集することができます。 Office の割り当ては、ナビゲーション プロパティを介してアクセスする別のエンティティ セットです。

マスター データは、マークアップやコードの詳細データにリンクできます。 このチュートリアルでは、両方のメソッドを使用します。

[![Image01](the-entity-framework-and-aspnet-getting-started-part-4/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image1.png)

## <a name="displaying-and-updating-related-entities-in-a-gridview-control"></a>表示および GridView コントロール内の関連エンティティを更新します。

という名前の新しい web ページを作成する*Instructors.aspx*を使用して、 *Site.Master*マスター ページ、および次のマークアップを追加、`Content`という名前のコントロール`Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample1.aspx)]

このマークアップを作成、`EntityDataSource`講習においてインストラクターを選択し、更新を有効にするコントロール。 `div`要素は後で、右側の列を追加できるように、左側に表示するマークアップを構成します。

間、`EntityDataSource`マークアップと終了`</div>`タグは、次のマークアップを作成するを追加、`GridView`コントロールと`Label`エラー メッセージに使用するコントロール。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample2.aspx)]

これは、`GridView`コントロールにより、行の選択、明るい灰色の背景色で選択した行を強調表示およびのハンドラーを作成した後で) を指定します、`SelectedIndexChanged`と`Updating`イベント。 指定`PersonID`の`DataKeyNames`プロパティ、選択した行のキーの値は、後で追加する別のコントロールに渡すことができるようにします。

最後の列にはナビゲーション プロパティに格納されている、インストラクター オフィス割り当てが含まれています、`Person`エンティティに関連付けられているエンティティのものであるためです。 注意して、`EditItemTemplate`要素を指定`Eval`の代わりに`Bind`であるため、`GridView`それらを更新するために、コントロールはナビゲーション プロパティにバインド直接ことはできません。 コードでの office 割り当てを更新します。 実行するへの参照を必要があります、`TextBox`制御、およびするを取得し、保存するには`TextBox`コントロールの`Init`イベント。

次の`GridView`コントロールは、`Label`エラー メッセージを使用するコントロール。 コントロールの`Visible`プロパティは`false`、のみコードが発行した場合、エラーへの応答に表示されるラベルが表示されるようにビュー ステートは、無効になります。

開く、 *Instructors.aspx.cs*ファイルし、次の追加`using`ステートメント。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample3.cs)]

Office の割り当てのテキスト ボックスへの参照を保持するために、部分クラス名宣言の直後後には、プライベート クラス フィールドを追加します。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample4.cs)]

スタブを追加、`SelectedIndexChanged`イベント ハンドラーの後でコードを追加します。 また、office の割り当てのハンドラーを追加`TextBox`コントロールの`Init`イベントへの参照を保存すること、`TextBox`コントロール。 ユーザーがナビゲーション プロパティに関連付けられているエンティティを更新するために入力した値を取得するのににはこの参照を使用します。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample5.cs)]

使用して、`GridView`コントロールの`Updating`を更新するイベント、 `Location` 、関連するプロパティ`OfficeAssignment`エンティティです。 次のハンドラーを追加、`Updating`イベント。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample6.cs)]

ユーザーがクリックしたときにこのコードが実行**更新**で、`GridView`行です。 コードでは、LINQ to Entities を使用して、取得、`OfficeAssignment`に現在関連付けられているエンティティ`Person`、エンティティを使用して、`PersonID`イベント引数から、選択した行のです。

コードはの値によって次の操作のいずれか、`InstructorOfficeTextBox`コントロール。

- テキスト ボックスにある値とがあるかどうかはない`OfficeAssignment`を更新するエンティティが 1 つ作成されます。
- テキスト ボックスにある値とがあるかどうか、 `OfficeAssignment` 、エンティティを更新、`Location`プロパティの値。
- テキスト ボックスが空の場合、`OfficeAssignment`エンティティが存在すると、エンティティを削除します。

その後、変更をデータベースに保存します。 例外が発生する場合は、エラー メッセージが表示されます。

ページを実行します。

[![Image02](the-entity-framework-and-aspnet-getting-started-part-4/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image3.png)

をクリックして**編集**し、すべてのフィールドがテキスト ボックスに変更します。

[![Image03](the-entity-framework-and-aspnet-getting-started-part-4/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image5.png)

変更を含め、これらの値のいずれかの**オフィス割り当て**です。 をクリックして**更新**と一覧に反映された変更が表示されます。

## <a name="displaying-related-entities-in-a-separate-control"></a>個別のコントロールに関連するエンティティを表示します。

追加するため、各インストラクターは、1 つまたは複数のコースを教えることができます、`EntityDataSource`コントロールと`GridView`、インストラクターにどの講師が選択されている関連付けのコースを一覧表示するコントロール`GridView`コントロール。 見出しを作成して、`EntityDataSource`コース エンティティの制御、エラー メッセージの間で、次のマークアップを追加`Label`コントロールと、終了`</div>`タグ。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample7.aspx)]

`Where`パラメーターには値が含まれています、`PersonID`内の行が選択されている講師、`InstructorsGridView`コントロール。 `Where`プロパティには、サブセレクト取得するコマンドすべての関連にはが含まれています`Person`からエンティティ、`Course`エンティティの`People`ナビゲーション プロパティを選択、`Course`エンティティ場合にのみ関連付けられているのいずれかの`Person`。エンティティが含まれていますが、選択した`PersonID`値。

作成する、`GridView`コントロール。 引き続いてすぐに次のマークアップを追加、`CoursesEntityDataSource`コントロール (終了する前に`</div>`タグ)。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample8.aspx)]

講師が選択されていない場合のコースは表示されませんので、`EmptyDataTemplate`要素が含まれています。

ページを実行します。

[![Image04](the-entity-framework-and-aspnet-getting-started-part-4/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image7.png)

割り当てられると、1 つまたは複数のコースを持っているインストラクターを選択し、コースまたはコースが一覧に表示します。 (注: データベース スキーマには、複数のコースが使用できますが、データベースに付属しているテスト データでインストラクター実際にはない複数のコースです。 自分自身を追加できますコースにデータベースを使用して、**サーバー エクスプ ローラー**ウィンドウまたは*CoursesAdd.aspx*ページで、後のチュートリアルで追加します)。

[![Image05](the-entity-framework-and-aspnet-getting-started-part-4/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image9.png)

`CoursesGridView`コントロールがいくつかのコース フィールドだけを表示します。 コースのすべての詳細を表示するには、使用する、`DetailsView`ユーザーが選択したコースのコントロールです。 *Instructors.aspx*、終了後、次のマークアップを追加`</div>`タグ (このマークアップを配置するかどうかを確認**後**終了 div タグを前にない)。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample9.aspx)]

このマークアップを作成、`EntityDataSource`コントロールにバインドされている、`Courses`エンティティ セット。 `Where`コースを使用して、プロパティの選択、`CourseID`コースで選択した行の値`GridView`コントロール。 マークアップのハンドラーを指定する、`Selected`別のレベルが階層内の下位であるイベント、学生の成績を表示するため後で使用されます。

*Instructors.aspx.cs*、次のスタブを作成、`CourseDetailsEntityDataSource_Selected`メソッドです。 (を記入するこのスタブ、チュートリアルの後半で、ここでは、必要なページがコンパイルされ実行できるようにします)。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample10.cs)]

ページを実行します。

[![Image06](the-entity-framework-and-aspnet-getting-started-part-4/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image11.png)

最初にはありません。 コースの詳細のためコースが選択されていません。 割り当てられている、コースを持っているインストラクターを選択し、詳細を表示するコースを選択します。

[![Image07](the-entity-framework-and-aspnet-getting-started-part-4/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image13.png)

## <a name="using-the-entitydatasource-selected-event-to-display-related-data"></a>EntityDataSource を使用して「選択」関連データを表示するイベント

最後に、すべての登録済みの受講者と選択したコースの成績を表示します。 これを行うには、使用する、`Selected`のイベント、`EntityDataSource`コースにコントロールがバインドされている`DetailsView`です。

*Instructors.aspx*、後に、次のマークアップを追加、`DetailsView`コントロール。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample11.aspx)]

このマークアップを作成、`ListView`受講者と選択したコースの成績の一覧を表示するコントロール。 データ ソースが指定されていないため、データ バインド コード内でコントロールがあります。 `EmptyDataTemplate`要素はコースが選択されていないときに表示されるメッセージを表示、その場合は、表示する、受講者はありません。 `LayoutTemplate`要素の一覧を表示する HTML テーブルが作成され、`ItemTemplate`を表示する列を指定します。 学生 ID と学生の成績、`StudentGrade`からは、エンティティ、および、受講者名、 `Person` Entity Framework での使用可能なエンティティ、`Person`のナビゲーション プロパティ、`StudentGrade`エンティティです。

*Instructors.aspx.cs*、スタブを置換`CourseDetailsEntityDataSource_Selected`メソッドを次のコード。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample12.cs)]

このイベントのイベント引数は 0 個の項目が選択されていない場合、またはを持つ 1 つの項目コレクションの形式で選択したデータを提供する場合、`Course`エンティティが選択されています。 場合、`Course`エンティティが選択されている場合、コードを使用して、`First`コレクションを 1 つのオブジェクトに変換します。 取得し、`StudentGrade`ナビゲーション プロパティからのエンティティが、コレクションに変換し、バインド、`GradesListView`コレクションを制御します。

これを表示するグレードが初めて、ページが表示されますが、空のデータ テンプレート内のメッセージに表示されることを確認するための十分なは、コースが選択されていないときにします。 実行するには、2 つの場所から呼び出すことになります、次の方法を作成します。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample13.cs)]

この新しいメソッドを呼び出して、`Page_Load`ページが表示される空のデータ テンプレートの最初の時刻を表示するメソッド。 それを呼び出すと、`InstructorsGridView_SelectedIndexChanged`メソッドそのイベントが発生した場合に、インストラクターを選択すると、つまり、新しいコースに読み込まれるコース`GridView`コントロールおよび none がまだ選択されていません。 2 つの呼び出しを次に示します。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample14.cs)]

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample15.cs)]

ページを実行します。

[![Image08](the-entity-framework-and-aspnet-getting-started-part-4/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image15.png)

割り当てられている、コースのあるインストラクターを選択し、コースを選択します。

[![Image09](the-entity-framework-and-aspnet-getting-started-part-4/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image17.png)

関連するデータを操作する方法はいくつか見てきました。 次のチュートリアルでは、既存のエンティティ間のリレーションシップを追加する方法が学習を既存のエンティティへのリレーションシップを持つ新しいエンティティを追加する方法と、リレーションシップを削除する方法です。

> [!div class="step-by-step"]
> [前へ](the-entity-framework-and-aspnet-getting-started-part-3.md)
> [次へ](the-entity-framework-and-aspnet-getting-started-part-5.md)
