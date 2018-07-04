---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
title: Entity Framework 4.0 Database でまず getting Started と ASP.NET 4 Web フォーム - パート 4 |Microsoft Docs
author: tdykstra
description: Contoso University のサンプルの web アプリケーションでは、Entity Framework を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。 サンプル アプリケーションは、.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: ceb9e60f-957c-4d25-9331-cc527de96a33
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
msc.type: authoredcontent
ms.openlocfilehash: 9783e30fdabe17958690d178ef95616cb72a8164
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380329"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-4"></a>Entity Framework 4.0 Database でまず getting Started と ASP.NET 4 Web フォーム - パート 4
====================
によって[Tom Dykstra](https://github.com/tdykstra)

> Contoso University のサンプルの web アプリケーションでは、Entity Framework 4.0 と Visual Studio 2010 を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。 チュートリアル シリーズについては、次を参照してください[シリーズの最初のチュートリアル。](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="working-with-related-data"></a>関連データの使用

前のチュートリアルでは使用して、`EntityDataSource`フィルター、並べ替え、およびデータをグループ化を制御します。 このチュートリアルでは、表示し、関連データを更新します。

インストラクターのリストを示す Instructors ページを作成します。 インストラクターを選択するとそのインストラクターが指導するコースのリストを参照してください。 コースを選択すると、コースと、コースに登録されている学生の一覧の詳細を参照してください。 インストラクターの名前、入社日、およびオフィスの割り当てを編集することができます。 オフィスの割り当ては、ナビゲーション プロパティを介してアクセスする別のエンティティ セットです。

マスター データは、マークアップまたはコードの詳細データにリンクできます。 このチュートリアルでは、両方のメソッドを使用します。

[![Image01](the-entity-framework-and-aspnet-getting-started-part-4/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image1.png)

## <a name="displaying-and-updating-related-entities-in-a-gridview-control"></a>表示および GridView コントロール内の関連エンティティを更新します。

という名前の新しい web ページを作成する*Instructors.aspx*を使用して、 *Site.Master*マスター ページ、および次のマークアップを追加、`Content`という名前のコントロール`Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample1.aspx)]

このマークアップを作成、`EntityDataSource`インストラクターを選択し、更新を有効にするコントロール。 `div`要素は、後で、右側の列を追加できるように、左側に表示するマークアップを構成します。

間、`EntityDataSource`マークアップと、終了`</div>`タグを作成する次のマークアップを追加、`GridView`コントロールと`Label`エラー メッセージに使用するコントロール。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample2.aspx)]

これは、`GridView`コントロールの行を選択できるように、淡いグレーの背景の色で選択されている行を強調表示、およびのハンドラー (これは後で作成します) を指定します、`SelectedIndexChanged`と`Updating`イベント。 また、指定`PersonID`の`DataKeyNames`プロパティ、いるため、選択した行のキーの値を後で追加する別のコントロールに渡すことができます。

最後の列にはナビゲーション プロパティに格納されている場合は、インストラクターのオフィスの割り当てが含まれています、`Person`エンティティ関連付けられたエンティティからを備えているためです。 注意、`EditItemTemplate`要素を指定します`Eval`の代わりに`Bind`ため、`GridView`それらを更新するには、コントロールはナビゲーション プロパティにバインド直接ことはできません。 コードでオフィスの割り当てを更新します。 そのためにはへの参照が必要、`TextBox`して、コントロールを取得しでこれを保存、`TextBox`コントロールの`Init`イベント。

次の`GridView`コントロールが、`Label`エラー メッセージを使用するコントロール。 コントロールの`Visible`プロパティは`false`、のみとコード、可視化でエラー発生時に、ラベルが表示されるようビュー ステートがオフになっています。

開く、 *Instructors.aspx.cs*ファイルを開き、次の追加`using`ステートメント。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample3.cs)]

オフィスの割り当てのテキスト ボックスへの参照を保持するために部分クラス名の宣言の直後後には、プライベート クラス フィールドを追加します。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample4.cs)]

追加のスタブ、`SelectedIndexChanged`イベント ハンドラーが、後でコードを追加します。 また、オフィスの割り当てのハンドラーを追加`TextBox`コントロールの`Init`イベントへの参照を保存すること、`TextBox`コントロール。 ユーザーがナビゲーション プロパティに関連付けられたエンティティを更新するために入力した値を取得するのに、この参照を使用します。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample5.cs)]

使用して、`GridView`コントロールの`Updating`を更新するイベント、`Location`プロパティ、関連付けられている`OfficeAssignment`エンティティ。 次のハンドラーを追加、`Updating`イベント。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample6.cs)]

ユーザーがクリックしたときにこのコードが実行**Update**で、`GridView`行。 コードでは、LINQ to Entities を使用して、取得、`OfficeAssignment`に現在関連付けられているエンティティ`Person`、エンティティを使用して、`PersonID`イベント引数から選択した行の。

コードはの値に応じて、次の操作の 1 つ、`InstructorOfficeTextBox`コントロール。

- かどうか、テキスト ボックスの値を持つし、はない`OfficeAssignment`を更新するエンティティが 1 つ作成されます。
- テキスト ボックスの値を持つしがあるかどうか、 `OfficeAssignment` 、エンティティを更新、`Location`プロパティの値。
- テキスト ボックスが空の場合、`OfficeAssignment`エンティティが存在する場合、エンティティを削除します。

その後、変更をデータベースに保存します。 例外が発生する場合は、エラー メッセージが表示されます。

ページを実行します。

[![Image02](the-entity-framework-and-aspnet-getting-started-part-4/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image3.png)

をクリックして**編集**し、すべてのフィールドがテキスト ボックスに変更します。

[![Image03](the-entity-framework-and-aspnet-getting-started-part-4/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image5.png)

これらの値のいずれかを変更する**オフィスの割り当て**します。 クリックして**Update**し、リストに反映される変更が表示されます。

## <a name="displaying-related-entities-in-a-separate-control"></a>個別のコントロールに関連するエンティティを表示します。

各講師は 1 つまたは複数のコースを追加しますので、`EntityDataSource`コントロールと`GridView`どのインストラクターが選択されて、instructors に関連付けられているコースの一覧を表示するコントロール`GridView`コントロール。 見出しを作成して、`EntityDataSource`コース エンティティの管理、エラー メッセージの間で、次のマークアップを追加`Label`コントロールおよび決算`</div>`タグ。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample7.aspx)]

`Where`パラメーターには値が含まれています、`PersonID`の講師である行が選択されて、`InstructorsGridView`コントロール。 `Where`プロパティに関連付けられているすべてを取得するサブセレクト コマンドが含まれています`Person`からエンティティを`Course`エンティティの`People`ナビゲーション プロパティを選択します、`Course`エンティティ場合にのみ関連付けられているのいずれかの`Person`エンティティが含まれていますが、選択した`PersonID`値。

作成する、`GridView`コントロール。 直後に続く次のマークアップを追加、`CoursesEntityDataSource`コントロール (終了する前に`</div>`タグ)。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample8.aspx)]

インストラクターが選択されていない場合のコースは表示されませんので、`EmptyDataTemplate`要素が含まれています。

ページを実行します。

[![Image04](the-entity-framework-and-aspnet-getting-started-part-4/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image7.png)

割り当てられている、1 つまたは複数のコースを持つインストラクターを選択し、コースまたはコースが一覧に表示します。 (注: データベース スキーマには、複数のコースができますが、データベースに付属しているテスト データでインストラクター実際にはない 1 つ以上のコースです。 自分自身を追加できますコースにデータベースを使用して、**サーバー エクスプ ローラー**ウィンドウまたは*CoursesAdd.aspx*ページで、後のチュートリアルで追加します)。

[![Image05](the-entity-framework-and-aspnet-getting-started-part-4/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image9.png)

`CoursesGridView`コントロールには、いくつかのコース フィールドのみが表示されます。 コースのすべての詳細を表示するには、使用する、`DetailsView`ユーザーが選択したコースのコントロール。 *Instructors.aspx*、終了後に次のマークアップを追加`</div>`タグ (このマークアップを配置するかどうかを確認**後**終了 div タグを前にない)。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample9.aspx)]

このマークアップを作成、`EntityDataSource`コントロールにバインドされている、`Courses`エンティティ セット。 `Where`プロパティの選択を使用して、コース、`CourseID`コースで選択した行の値`GridView`コントロール。 マークアップのハンドラーを指定する、`Selected`は別のレベルが階層内の下位イベント、学生の成績を表示するため後で使用します。

*Instructors.aspx.cs*、次のスタブを作成、`CourseDetailsEntityDataSource_Selected`メソッド。 (チュートリアルの後半でこのスタブを入力します。 ここでは、必要なページがコンパイルされ実行されるようにします)。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample10.cs)]

ページを実行します。

[![Image06](the-entity-framework-and-aspnet-getting-started-part-4/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image11.png)

最初にないコースの詳細コースが選択されていないためです。 、割り当てられているコースを持つインストラクターを選択し、詳細を表示するコースを選択します。

[![Image07](the-entity-framework-and-aspnet-getting-started-part-4/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image13.png)

## <a name="using-the-entitydatasource-selected-event-to-display-related-data"></a>EntityDataSource を使用して「選択」イベント関連データを表示するには

最後に、すべての登録済みの受講者とその成績、選択したコースを表示します。 これを行うには、使用する、`Selected`のイベント、`EntityDataSource`コースにコントロールがバインドされている`DetailsView`します。

*Instructors.aspx*、後に次のマークアップを追加、`DetailsView`コントロール。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample11.aspx)]

このマークアップを作成、`ListView`学生と、選択したコースの成績のリストを表示するコントロール。 データ バインド コード内でコントロールがわかるため、データ ソースを指定しません。 `EmptyDataTemplate`要素は、コースが選択されていないときに表示するメッセージを提供します。-その場合は、表示する学生はありません。 `LayoutTemplate`要素の一覧を表示する HTML テーブルを作成して、`ItemTemplate`を表示する列を指定します。 学生 ID と生徒の成績が、`StudentGrade`エンティティと受講者名が、 `Person` Entity Framework で利用するエンティティ、`Person`のナビゲーション プロパティ、`StudentGrade`エンティティ。

*Instructors.aspx.cs*、スタブとして作成された出力を置き換える`CourseDetailsEntityDataSource_Selected`メソッドを次のコード。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample12.cs)]

このイベントのイベント引数がない場合、項目数は 0 または 1 つの項目がコレクションの形式で選択したデータを提供する場合、`Course`エンティティを選択します。 場合、`Course`エンティティが選択されている場合、コードを使用して、`First`コレクションを 1 つのオブジェクトに変換します。 取得し、 `StudentGrade` 、ナビゲーション プロパティからエンティティが、コレクションに変換し、バインド、`GradesListView`コレクションを制御します。

これは、最初に、ページが表示されますが、空のデータ テンプレート内のメッセージに表示されることを確認するには成績を表示するための十分なとコースが選択されていないときにします。 そのためには、次のメソッドは、2 つの場所から呼び出すことがありますを作成します。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample13.cs)]

この新しいメソッドを呼び出し、`Page_Load`ページが表示される空のデータ テンプレートの最初の時刻を表示するメソッド。 それを呼び出すと、`InstructorsGridView_SelectedIndexChanged`メソッド新しいコースの意味がコースに読み込まれるため、インストラクターが選択されたときに、そのイベントを発生は、`GridView`コントロールと none がまだ選択されています。 2 つの呼び出しを次に示します。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample14.cs)]

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample15.cs)]

ページを実行します。

[![Image08](the-entity-framework-and-aspnet-getting-started-part-4/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image15.png)

割り当てられているコースがインストラクターを選択し、コースを選択します。

[![Image09](the-entity-framework-and-aspnet-getting-started-part-4/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image17.png)

関連データを操作するいくつかの方法を確認できました。 次のチュートリアルでは、既存のエンティティ間のリレーションシップを追加する方法が学習、リレーションシップを削除する方法と、既存のエンティティの関係がある新しいエンティティを追加する方法。

> [!div class="step-by-step"]
> [前へ](the-entity-framework-and-aspnet-getting-started-part-3.md)
> [次へ](the-entity-framework-and-aspnet-getting-started-part-5.md)
