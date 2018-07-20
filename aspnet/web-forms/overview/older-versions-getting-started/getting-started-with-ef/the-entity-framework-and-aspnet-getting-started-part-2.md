---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
title: Entity Framework 4.0 Database でまず getting Started と ASP.NET 4 Web フォームの第 2 部 |Microsoft Docs
author: tdykstra
description: Contoso University のサンプルの web アプリケーションでは、Entity Framework を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。 サンプル アプリケーションは、.
ms.author: aspnetcontent
ms.date: 12/03/2010
ms.assetid: fb63a326-a4ae-4b0c-a4f5-412327197216
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
msc.type: authoredcontent
ms.openlocfilehash: c43e7b9d090b0e25fe1db1ce6a944afea4b081d9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802638"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-2"></a>Entity Framework 4.0 Database でまず getting Started と ASP.NET 4 Web フォームの第 2 部
====================
によって[Tom Dykstra](https://github.com/tdykstra)

> Contoso University のサンプルの web アプリケーションでは、Entity Framework 4.0 と Visual Studio 2010 を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。 チュートリアル シリーズについては、次を参照してください[シリーズの最初のチュートリアル。](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="the-entitydatasource-control"></a>EntityDataSource コントロール

前のチュートリアルでは、web サイト、データベース、およびデータ モデルを作成します。 このチュートリアルで使用する、 `EntityDataSource` Entity Framework データ モデルを使用して作業をしやすくために ASP.NET が提供するコントロール。 作成、`GridView`を表示すると、学生データの編集コントロール、`DetailsView`新しいの受講者を追加するためのコントロールと`DropDownList`(これは、関連付けられているコースを表示するため後で使用します) 部門の選択コントロール。

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image1.png)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image3.png)

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image5.png)

このアプリケーションではありませんに追加する入力の検証、データベースを更新するページ、実稼働アプリケーションで処理するために必要な堅牢いくつかのエラー処理ができないことに注意してください。 Entity Framework に重点を置いて、このチュートリアルを保持して長くなりすぎます。 これらの機能をアプリケーションに追加する方法の詳細については、次を参照してください。 [ASP.NET Web Pages でのユーザー入力の検証](https://msdn.microsoft.com/library/7kh55542.aspx)と[ASP.NET ページとアプリケーションのエラー処理](https://msdn.microsoft.com/library/w16865z6.aspx)します。

## <a name="adding-and-configuring-the-entitydatasource-control"></a>追加と EntityDataSource コントロールの構成

構成から始めます、`EntityDataSource`を読み取るコントロール`Person`からエンティティ、`People`エンティティ セット。

Visual Studio を開いていることを確認し、パート 1 で作成したプロジェクトを使用していること。 データ モデルを作成するか、または最後の変更以降に加えたからプロジェクトをビルドしていない場合は、ここで、プロジェクトをビルドします。 データ モデルへの変更が使用はできませんをデザイナーに、プロジェクトがビルドされるまでです。

使用して新しい web ページを作成、**マスター ページを使用して Web フォーム**テンプレート、名前を付けます*Students.aspx*します。

[![Image23](the-entity-framework-and-aspnet-getting-started-part-2/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image7.png)

指定*Site.Master*のマスター ページとして。 これらのチュートリアルを作成するページのすべては、このマスター ページで使用されます。

[![Image24](the-entity-framework-and-aspnet-getting-started-part-2/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image9.png)

**ソース**ビューで、追加、`h2`に見出し、`Content`という名前のコントロール`Content2`次の例のように。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample1.aspx)]

**データ**のタブ、**ツールボックス**、ドラッグ、`EntityDataSource`コントロールをページ、見出しの下にドロップする ID を変更`StudentsEntityDataSource`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample2.aspx)]

切り替える**デザイン**表示、データ ソース コントロールのスマート タグをクリックして**データ ソースの構成**を起動する、**データ ソースの構成**ウィザード。

[![Image01](the-entity-framework-and-aspnet-getting-started-part-2/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image11.png)

**ObjectContext の構成**ウィザード手順**SchoolEntities**の値として**という名前の接続**、選び**SchoolEntities**として、 **DefaultContainerName**値。 その後、 **[次へ]** をクリックします。

[![Image02](the-entity-framework-and-aspnet-getting-started-part-2/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image13.png)

注: この時点で、次のダイアログ ボックスが表示される場合がある、続行する前にプロジェクトをビルドします。

[![Image25](the-entity-framework-and-aspnet-getting-started-part-2/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image15.png)

**構成データの選択**手順で、**人**の値として**EntitySetName**します。 **選択**、ことを確認、 ** を選択**ll チェック ボックスをオンします。 更新プログラムを有効化および削除するオプションを選択します。 完了したら、クリックして**完了**します。

[![Image03](the-entity-framework-and-aspnet-getting-started-part-2/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image17.png)

## <a name="configuring-database-rules-to-allow-deletion"></a>削除を許可するデータベースの規則を構成します。

ユーザーから受講者を削除できるようにするページを作成すること、`Person`と他のテーブルの 3 つのリレーションシップのあるテーブル (`Course`、 `StudentGrade`、および`OfficeAssignment`)。 既定では、データベースは削除できない場合内の行`Person`他のテーブルのいずれかで関連する行がある場合。 最初に、手動で、関連する行を削除できますか削除するときに自動的に削除するデータベースを構成することができます、`Person`行。 このチュートリアルでは、学生のレコードの関連データを自動的に削除するデータベースを構成します。 受講者に行を関連付けることがあるためにのみ、`StudentGrade`テーブル、3 つのリレーションシップのいずれかのみを構成する必要があります。

使用している場合、 *School.mdf*ファイルこのチュートリアルではプロジェクトからをダウンロードして、これらの構成変更が既に実行されているため、このセクションをスキップすることができます。 スクリプトを実行して、データベースを作成した場合は、次の手順を実行することによって、データベースを構成します。

**サーバー エクスプ ローラー**パート 1 で作成したデータベース ダイアグラムを開きます。 間のリレーションシップを右クリックして`Person`と`StudentGrade`(テーブルの行) を選び**プロパティ**します。

[![Image04](the-entity-framework-and-aspnet-getting-started-part-2/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image19.png)

**プロパティ**ウィンドウで、展開**INSERT および UPDATE の指定**設定と、 **DeleteRule**プロパティを**Cascade**します。

[![Image05](the-entity-framework-and-aspnet-getting-started-part-2/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image21.png)

保存して、図を閉じます。 データベースを更新するかどうかをクリックするかを尋ねられたら場合**はい**します。

モデルが、データベースが行っている内容との同期のメモリ内のエンティティを保持することを確認するには、データ モデルに対応するルールを設定する必要があります。 開いている*SchoolModel.edmx*、間の関連行を右クリックして`Person`と`StudentGrade`、し、**プロパティ**します。

[![Image21](the-entity-framework-and-aspnet-getting-started-part-2/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image23.png)

**プロパティ**ウィンドウで、設定**End1 OnDelete**に**Cascade**します。

[![Image22](the-entity-framework-and-aspnet-getting-started-part-2/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image25.png)

保存して閉じます、 *SchoolModel.edmx*ファイルを開き、プロジェクトをリビルドします。

一般に、データベースが変更されたときに、モデルを同期する方法のいくつかの選択肢があります。

- 特定の種類の変更 (追加または更新のテーブル、ビュー、またはストアド プロシージャ) などのクリックし、デザイナーで右クリックし**データベースからモデルを更新**デザイナーの作成、変更を自動的が。
- データ モデルを再生成します。
- 次のような手動の更新プログラムを作成します。

フィールド名の変更をもう一度作成する必要しますが、ここでは、モデルを再生成でしたまたはリレーションシップの変更によって影響を受けるテーブルを更新する (から`FirstName`に`FirstMidName`)。

## <a name="using-a-gridview-control-to-read-and-update-entities"></a>GridView コントロールを使用して、読み取りし、エンティティを更新するには

このセクションで使用する、`GridView`コントロールを表示し、更新、または、受講者を削除します。

開く、またはに切り替える*Students.aspx*に切り替えると**デザイン**ビュー。 **データ**のタブ、**ツールボックス**、ドラッグ、`GridView`コントロールの右側に、`EntityDataSource`制御、名前を付けます`StudentsGridView`、スマート タグをクリックし、 **StudentsEntityDataSource**データ ソースとして。

[![Image06](the-entity-framework-and-aspnet-getting-started-part-2/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image27.png)

をクリックして**スキーマの更新**(をクリックして**はい**ことを確認するよう求める場合)、順にクリックします**ページングを有効にする**、**並べ替えを有効にする**、 **編集を有効にする**、および**の削除を有効にする**します。

クリックして**列の編集**します。

[![Image10](the-entity-framework-and-aspnet-getting-started-part-2/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image29.png)

**選択されたフィールド**ボックスで、削除**PersonID**、 **LastName**、および**HireDate**します。 通常しないレコードのキーをユーザーに表示する、雇用された日付に関連する学生、およびため必要名フィールドの 1 つのみに、1 つのフィールドを名前の両方の部分を配置します。)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-2/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image31.png)

選択、 **FirstMidName**フィールドをクリックして**このフィールドを TemplateField に変換**します。

同じ**EnrollmentDate**します。

[![Image13](the-entity-framework-and-aspnet-getting-started-part-2/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image33.png)

クリックして**OK**に切り替えると**ソース**ビュー。 残りの変更は、マークアップで直接実行しやすくなります。 `GridView`今が次の例のようにマークアップを制御します。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample3.aspx)]

[コマンド] フィールドが現在テンプレート フィールドは、後の最初の列には、最初の名前が表示されます。 次の例のようにこのテンプレート フィールドのマークアップを変更します。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample4.aspx)]

表示モードでは、2 つ`Label`コントロールは、最初と最後の名前を表示します。 編集モードで、2 つのテキスト ボックスは、最初と最後の名前を変更できるように提供されます。 同様、`Label`内のコントロールの表示モードを使用する`Bind`と`Eval`データベースに直接接続する ASP.NET データ ソース コントロールの式と同じ状態の場合します。 唯一の違いは、データベース列ではなく、エンティティのプロパティを指定することです。

最後の列は、登録日付を表示するテンプレートのフィールドです。 次の例のようにこのフィールドのマークアップを変更します。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample5.aspx)]

両方の表示し、編集モードと、"short 日付"形式で表示される日付の書式指定文字列「{0, d}」。 (コンピューターはこのチュートリアルで示すように画面の画像から異なる方法では、この形式を表示する構成があります)。

これらのテンプレート フィールドごとに、デザイナーは使用に注意してください、`Bind`が既定では、式を変更する、`Eval`内の式、`ItemTemplate`要素。 `Bind`データで使用できる式`GridView`コード内のデータにアクセスする必要がある場合は、プロパティを制御します。 このページを使用できるようにコードでは、このデータにアクセスする必要はありません`Eval`、これが効率的です。 詳細については、次を参照してください。[データ コントロールからデータを取得](https://weblogs.asp.net/davidfowler/archive/2008/12/12/getting-your-data-out-of-the-data-controls.aspx)します。

## <a name="revising-entitydatasource-control-markup-to-improve-performance"></a>パフォーマンスを向上させるために改訂 EntityDataSource コントロール マークアップ

マークアップで、`EntityDataSource`コントロールを削除、`ConnectionString`と`DefaultContainerName`属性し、置き換え、`ContextTypeName="ContosoUniversity.DAL.SchoolEntities"`属性。 これは、変更を作成するたびにする必要があります、`EntityDataSource`制御、オブジェクト コンテキスト クラス内でハードコーディングされているとは異なる接続を使用する必要がある場合を除き、します。 使用して、`ContextTypeName`属性は、次の利点を提供します。

- パフォーマンスが向上します。 ときに、`EntityDataSource`コントロールを使用してデータ モデルを初期化します、`ConnectionString`と`DefaultContainerName`属性、追加の作業要求のたびにメタデータの読み込みを実行します。 これは指定した場合は必要ありません、`ContextTypeName`属性。
- 遅延読み込みが生成されたオブジェクト コンテキスト クラスに既定でオン (など`SchoolEntities`このチュートリアルでは) では、Entity Framework 4.0。 これはナビゲーション プロパティが読み込まれる関連データを自動的に必要なときすぐを意味します。 遅延読み込みは、このチュートリアルの後半で詳しく説明します。
- オブジェクト コンテキスト クラスに適用したカスタマイズ (ここで、`SchoolEntities`クラス) を使用するコントロールで使用できる、`EntityDataSource`コントロール。 このチュートリアル シリーズでは説明しません高度なトピックは、オブジェクト コンテキスト クラスをカスタマイズします。 詳細については、次を参照してください。 [Entity Framework 生成型の拡張](https://msdn.microsoft.com/library/dd456844.aspx)します。

マークアップは次の例 (プロパティの順序が異なる可能性があります) のようになります。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample6.aspx)]

`EnableFlattening`属性が、外部キー列は、エンティティのプロパティとして公開されていなかったため、Entity Framework の以前のバージョンで必要な機能を参照します。 現在のバージョンを使用して*外部キー アソシエーション*、つまり、外部キー プロパティは、多対多のアソシエーションの公開します。 外部キー プロパティとなしに、エンティティがある[複合型](https://msdn.microsoft.com/library/bb738472.aspx)、この属性に設定しておくことができます`False`します。 既定値であるために、マークアップから属性を削除しない`True`します。 詳細については、次を参照してください。[フラット化オブジェクト (EntityDataSource)](https://msdn.microsoft.com/library/ee404746.aspx)します。

ページを実行し、受講者と従業員 (しますをフィルター処理するだけの学生で、次のチュートリアル) の一覧を参照してください。 名と姓がまとめて表示されます。

[![Image07](the-entity-framework-and-aspnet-getting-started-part-2/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image35.png)

表示を並べ替えるには、列名をクリックします。

クリックして**編集**いずれかの行。 テキスト ボックスを表示するには、最初と最後の名前を変更することができます。

[![Image08](the-entity-framework-and-aspnet-getting-started-part-2/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image37.png)

**削除**ボタンもは機能します。 加入契約の日を含む行の削除 をクリックし、行は表示されなくなります。 (登録日を設定せずに行がインストラクターを表す、参照整合性エラーが発生する可能性があります。 次のチュートリアルをフィルター処理するだけで生徒を追加するには、この一覧。)

## <a name="displaying-data-from-a-navigation-property"></a>ナビゲーション プロパティからデータを表示します。

今すぐコースの数を知りたいと各受講者に登録されます。 Entity Framework では、その情報を提供する、`StudentGrades`のナビゲーション プロパティ、`Person`エンティティ。 データベースの設計が、学生に割り当てられているグレードをしなくてもコースの登録を許可しないため、このチュートリアルと想定できます内を持つ行、`StudentGrade`コースに関連付けられているテーブルの行は、コースに登録されていると同じです。 (、`Courses`ナビゲーション プロパティはインストラクターのみ)。

使用すると、`ContextTypeName`の属性、`EntityDataSource`コントロール、Entity Framework に自動的に情報を取得しますナビゲーション プロパティのプロパティにアクセスするとします。 これは呼び出されます*遅延読み込み*します。 ただし、このことができます、効率的なためにできないを個別に呼び出して、データベースの各時間の追加情報が必要になります。 によって返されるすべてのエンティティのナビゲーション プロパティからのデータが必要があります、`EntityDataSource`コントロールは、データベースに 1 回の呼び出しでは、エンティティ自体と関連データを取得する方が効率的です。 これは呼び出されます*一括読み込み*、ナビゲーション プロパティの一括読み込みを設定して指定して、`Include`のプロパティ、`EntityDataSource`コントロール。

*Students.aspx*は、各受講者のコースの数を表示するための一括読み込みは、最適な選択肢です。 すべての学生を表示するが、コースの数を示す場合 (これは、マークアップに加えてコードを記述する必要は) その一部に対してのみ遅延読み込みがあります適しています。

開くかに切り替えます*Students.aspx*、切り替える**デザイン**ビューで、 `StudentsEntityDataSource`、し、**プロパティ**ウィンドウのセット、 **Include**プロパティを**StudentGrades**します。 (複数のナビゲーション プロパティを取得する場合は、コンマで区切られた名前を指定できます: たとえば、 **StudentGrades、コース**)。

[![Image19](the-entity-framework-and-aspnet-getting-started-part-2/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image39.png)

切り替える**ソース**ビュー。 `StudentsGridView` 、最後の後に、コントロール`asp:TemplateField`要素では、次の新しいテンプレート フィールドの追加。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample7.aspx)]

`Eval`式、ナビゲーション プロパティを参照する`StudentGrades`します。 このプロパティには、コレクションが含まれている、ため、`Count`コースの受講者が登録されている数を表示するのに使用できるプロパティです。 後のチュートリアルでは、データ コレクションではなく単一のエンティティを含むナビゲーション プロパティを表示する方法を確認します。 (使用することはできません注`BoundField`ナビゲーション プロパティからデータを表示する要素)。

ページを実行し、数のコースの受講者が登録されている表示されます。

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image41.png)

## <a name="using-a-detailsview-control-to-insert-entities"></a>DetailsView コントロールを使用してエンティティを挿入するには

次の手順を持つページを作成するには、`DetailsView`新しい生徒を追加できるようにするコントロール。 ブラウザーを閉じて、使用して新しい web ページを作成し、 *Site.Master*マスター ページ。 ページの名前を付けます*StudentsAdd.aspx*、しに切り替える**ソース**ビュー。

既存のマークアップを置き換えるには、次のマークアップを追加、`Content`という名前のコントロール`Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample8.aspx)]

このマークアップを作成、`EntityDataSource`で作成したようなコントロール*Students.aspx*を除き、これにより、挿入します。 同様、`GridView`コントロールのバインドされたフィールド、`DetailsView`コントロールにエンティティのプロパティを参照する点を除いて、データベースに直接接続するデータ コントロールの場合と同じに正確にコーディングされています。 ここで、`DetailsView`コントロールは、行を挿入するため、既定のモードを設定するには使用`Insert`します。

ページを実行し、新しい生徒を追加します。

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image43.png)

今すぐ実行する場合は、新しい生徒を挿入した後、何も起こりません*Students.aspx*、新しい生徒の情報が表示されます。

## <a name="displaying-data-in-a-drop-down-list"></a>ドロップダウン リストでデータを表示します。

データ バインドが、次の手順で、`DropDownList`コントロールを使用して設定エンティティを`EntityDataSource`コントロール。 このチュートリアルでは、この一覧では多くの操作も行いません。 以降の部分がリストを使用する、ユーザーの部門に関連付けられているコースを表示するのに部署を選択できるようにします。

という名前の新しい web ページを作成する*Courses.aspx*します。 **ソース**、見出しを追加、表示、`Content`というコントロール`Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample9.aspx)]

**デザイン**ビューで、追加、`EntityDataSource`コントロールをページと同様、前に、この時間を除くという名前を付けます`DepartmentsEntityDataSource`します。 選択**部門**として、 **EntitySetName**値に設定して、のみを選択、 **DepartmentID**と**名前**プロパティ。

[![Image15](the-entity-framework-and-aspnet-getting-started-part-2/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image45.png)

**標準**のタブ、**ツールボックス**、ドラッグ、`DropDownList`コントロールをページで、名前を付けます`DepartmentsDropDownList`、スマート タグをクリックし、選択**データ ソースの選択**に開始、**データ ソース構成ウィザード**します。

[![Image16](the-entity-framework-and-aspnet-getting-started-part-2/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image47.png)

**データ ソースの選択**手順で、 **DepartmentsEntityDataSource**データ ソースとして次のようにクリックします**スキーマの更新**、し、 **名。** として表示するデータ フィールドと**DepartmentID**値のデータ フィールドとして。 **[OK]** をクリックします。

[![Image17](the-entity-framework-and-aspnet-getting-started-part-2/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image49.png)

データをバインドする Entity Framework を使用してコントロールを使用するメソッドでは、エンティティとエンティティのプロパティを他の ASP.NET データを除き、ソース コントロールを指定と同じです。

切り替える**ソース**を表示および追加"部署を選択します:"の直前に、`DropDownList`コントロール。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample10.aspx)]

マークアップを変更するアラームとして、`EntityDataSource`コントロールを置き換えることで、この時点で、`ConnectionString`と`DefaultContainerName`属性を`ContextTypeName="ContosoUniversity.DAL.SchoolEntities"`属性。 変更する前に、データ ソース コントロールにリンクされているデータ バインド コントロールを作成した後にまで待機する最適な多くの場合は、`EntityDataSource`変更を行った後、デザイナーは提供しないために、マークアップを制御、**更新スキーマ**データ バインド コントロールでオプションです。

ページを実行し、ドロップダウン リストから、部門を選択することができます。

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image51.png)

これで完了の使用の概要、`EntityDataSource`コントロール。 このコントロールの操作ですが一般的に使用可能なその他の ASP.NET データ ソース コントロールから同じエンティティとテーブルと列の代わりにプロパティを参照します。 唯一の例外は、ナビゲーション プロパティにアクセスする場合です。 次のチュートリアルで使用する構文が表示されます`EntityDataSource`フィルター処理してグループ化、データの並べ替えを実行するときに、コントロールは他のデータ ソース コントロールから異なるも場合があります。

> [!div class="step-by-step"]
> [前へ](the-entity-framework-and-aspnet-getting-started-part-1.md)
> [次へ](the-entity-framework-and-aspnet-getting-started-part-3.md)
