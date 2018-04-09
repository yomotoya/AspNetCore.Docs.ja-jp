---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
title: データベースの概要 Entity Framework 4.0 最初および ASP.NET 4 Web フォーム - パート 2 |Microsoft ドキュメント
author: tdykstra
description: Contoso 大学でサンプル web アプリケーションでは、Entity Framework を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。 サンプル アプリケーションは、.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: fb63a326-a4ae-4b0c-a4f5-412327197216
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
msc.type: authoredcontent
ms.openlocfilehash: a6c95a92aa77e2bb73aa513a207e0469d1aedbd2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-2"></a>データベースの概要 Entity Framework 4.0 最初および ASP.NET 4 Web フォーム - パート 2
====================
によって[Tom Dykstra](https://github.com/tdykstra)

> Contoso 大学でサンプル web アプリケーションでは、Entity Framework 4.0 および Visual Studio 2010 を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。 一連のチュートリアルについては、次を参照してください[系列内の最初のチュートリアル。](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="the-entitydatasource-control"></a>EntityDataSource コントロール

前のチュートリアルでは、web サイト、データベース、およびデータ モデルを作成します。 このチュートリアルで使用する、`EntityDataSource`コントロールを Entity Framework データ モデルで作業しやすくために ASP.NET が用意されています。 作成、`GridView`を表示および編集学生データ コントロール、`DetailsView`新しい受講者を追加するためのコントロールと`DropDownList`部署 (関連付けられているコースを表示するため後で使用されます) を選択するためのコントロールです。

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image1.png)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image3.png)

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image5.png)

このアプリケーションではありませんに追加する入力の検証ページ、データベースを更新するように、実稼働アプリケーションで必要になるように堅牢なエラー処理の一部はできないことに注意してください。 Entity Framework に重点を置いて、チュートリアルを保持して作業が長すぎます。 これらの機能をアプリケーションに追加する方法の詳細については、「 [ASP.NET Web Pages でのユーザー入力の検証](https://msdn.microsoft.com/library/7kh55542.aspx)と[Error Handling in ASP.NET ページおよびアプリケーション](https://msdn.microsoft.com/library/w16865z6.aspx)です。

## <a name="adding-and-configuring-the-entitydatasource-control"></a>EntityDataSource コントロールの構成の追加と

構成から始めます、`EntityDataSource`を読み取るコントロール`Person`からエンティティ、`People`エンティティ セット。

パート 1 で作成したプロジェクトを使用していることを開いて Visual Studio 有しを確認してください。 データ モデルを作成するか、または最後の変更後に加えたからプロジェクトをビルドしていない場合は、今すぐプロジェクトをビルドします。 データ モデルへの変更は入手されませんをデザイナーに、プロジェクトがビルドされるまでです。

新しい web ページを使用して、作成、**マスター ページを使用して Web フォーム**テンプレート、名前を付けます*Students.aspx*です。

[![Image23](the-entity-framework-and-aspnet-getting-started-part-2/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image7.png)

指定*Site.Master*マスター ページとします。 このマスター ページを使用してのこれらのチュートリアルを作成するページのすべてがします。

[![Image24](the-entity-framework-and-aspnet-getting-started-part-2/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image9.png)

**ソース**ビューで、追加、`h2`に見出し、`Content`という名前のコントロール`Content2`次の例のように。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample1.aspx)]

**データ**のタブ、**ツールボックス**、ドラッグ、`EntityDataSource`コントロールをページ、見出しの下にドロップする ID を変更`StudentsEntityDataSource`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample2.aspx)]

切り替える**デザイン**、データ ソース コントロールのスマート タグをクリックし、をクリックして**データ ソースの構成**を起動する、**データ ソースの構成**ウィザード。

[![Image01](the-entity-framework-and-aspnet-getting-started-part-2/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image11.png)

**構成 ObjectContext**ウィザード ステップで、 **SchoolEntities**の値として**という名前の接続**を選択して**SchoolEntities**として、 **DefaultContainerName**値。 その後、 **[次へ]**をクリックします。

[![Image02](the-entity-framework-and-aspnet-getting-started-part-2/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image13.png)

注: この時点で、次のダイアログ ボックスが表示される場合がある続行する前に、プロジェクトをビルドします。

[![Image25](the-entity-framework-and-aspnet-getting-started-part-2/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image15.png)

**データの選択を構成**手順で、**人**の値として**EntitySetName**です。 **選択**、ことを確認、 ** を選択**ll チェック ボックスをオンします。 更新プログラムを有効化および削除するオプションを選択します。 完了したら、をクリックして**完了**です。

[![Image03](the-entity-framework-and-aspnet-getting-started-part-2/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image17.png)

## <a name="configuring-database-rules-to-allow-deletion"></a>削除を許可するデータベースのルールを構成します。

受講者を削除できるようにページを作成する、`Person`を他のテーブルと 3 つのリレーションシップを持つテーブル (`Course`、 `StudentGrade`、および`OfficeAssignment`)。 既定では、データベースは削除できないの行から`Person`他のテーブルの 1 つの関連する行がある場合。 関連する行を最初に、手動で削除できますか、削除するときに自動的に削除するデータベースを構成することができます、`Person`行です。 学生のレコードがこのチュートリアルでは、関連するデータを自動的に削除するデータベースを構成します。 受講者に行を関連付けることがあるためにのみ、`StudentGrade`テーブル、3 つのリレーションシップの 1 つだけを構成する必要があります。

使用する場合、 *School.mdf*このチュートリアルでは、プロジェクトからダウンロードしたファイル、これらの構成変更が既に実行されているために、このセクションをスキップすることができます。 スクリプトを実行してデータベースを作成した場合は、次の手順を実行することによって、データベースを構成します。

**サーバー エクスプ ローラー**パート 1 で作成したデータベース ダイアグラムを開きます。 間のリレーションシップを右クリックして`Person`と`StudentGrade`(テーブルの間で行) を選択し、**プロパティ**です。

[![Image04](the-entity-framework-and-aspnet-getting-started-part-2/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image19.png)

**プロパティ**ウィンドウで、展開**INSERT および UPDATE の指定**設定と、 **DeleteRule**プロパティを**Cascade**です。

[![Image05](the-entity-framework-and-aspnet-getting-started-part-2/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image21.png)

保存して図を閉じます。 クリックしてデータベースを更新するかどうかをたずねられたら場合**はい**です。

モデルがデータベースを置いて動作との同期のメモリ内にあるエンティティを保持することを確認するには、データ モデルで対応するルールを設定する必要があります。 開いている*SchoolModel.edmx*との間の関連行を右クリックして`Person`と`StudentGrade`、し、**プロパティ**です。

[![Image21](the-entity-framework-and-aspnet-getting-started-part-2/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image23.png)

**プロパティ**ウィンドウで、設定**End1 OnDelete**に**Cascade**です。

[![Image22](the-entity-framework-and-aspnet-getting-started-part-2/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image25.png)

保存して閉じます、 *SchoolModel.edmx*ファイル、およびプロジェクトをリビルドします。

一般に、データベースが変更されたときに、モデルを同期する方法のいくつかの選択肢があります。

- 特定の (の追加やテーブル、ビュー、またはストアド プロシージャを更新する)、変更の種類をクリックし、デザイナー内を右クリックして**データベースからモデルを更新**にデザイナーの作成、変更に自動的にします。
- データ モデルを再生成します。
- 次のような手動の更新を行います。

フィールド名の変更を再度作成する必要があるが、ここでは、モデルを再生成でしたまたはリレーションシップの変更によって影響を受けるテーブルを更新する (から`FirstName`に`FirstMidName`)。

## <a name="using-a-gridview-control-to-read-and-update-entities"></a>使用した GridView コントロールの読み取りし、エンティティを更新します。

このセクションでを使用して、`GridView`受講者を表示、更新、削除を制御します。

開く、またはに切り替える*Students.aspx*に切り替えると**デザイン**ビュー。 **データ**のタブ、**ツールボックス**、ドラッグ、`GridView`コントロールの右側に、`EntityDataSource`制御、名前を付けます`StudentsGridView`、スマート タグをクリックし、 **StudentsEntityDataSource**データ ソースとして。

[![Image06](the-entity-framework-and-aspnet-getting-started-part-2/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image27.png)

をクリックして**スキーマの更新**(をクリックして**はい**確認を求められた場合)、をクリックして**ページングを有効にする**、**並べ替えを有効にする**、 **編集を有効にする**、および**の削除を有効にする**です。

をクリックして**列の編集**です。

[![Image10](the-entity-framework-and-aspnet-getting-started-part-2/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image29.png)

**選択されたフィールド**ボックスで、削除**PersonID**、 **LastName**、および**HireDate**です。 通常しないレコード キー ユーザーに表示する、受講者、雇用された日付は無効とだけで済みます名前フィールドのいずれかのように、1 つのフィールド名の両方の部分を配置します。)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-2/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image31.png)

選択、 **FirstMidName**フィールドし、をクリックして**このフィールドを TemplateField に変換**です。

同じ操作を行います**EnrollmentDate**です。

[![Image13](the-entity-framework-and-aspnet-getting-started-part-2/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image33.png)

をクリックして**OK**に切り替えて**ソース**ビュー。 残りの変更は、マークアップ内で直接実行しやすくなります。 `GridView`マークアップ今のような次の例を制御します。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample3.aspx)]

コマンド フィールドは、テンプレート フィールドが現在後の最初の列には、最初の名前が表示されます。 次の例のようにこのテンプレート フィールドのマークアップを変更します。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample4.aspx)]

表示モードで 2 つ`Label`コントロールは、最初と最後の名前を表示します。 編集モードで 2 つのテキスト ボックスは、最初と最後の名前を変更できるように提供されます。 同様、`Label`内のコントロールの表示モードを使用する`Bind`と`Eval`したとおりに式が ASP.NET のデータ ソース コントロールのデータベースに直接接続するとします。 唯一の違いは、データベース列ではなくエンティティのプロパティを指定していることです。

最後の列は、登録の日付を表示するテンプレートのフィールドです。 次の例のようにこのフィールドのマークアップを変更します。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample5.aspx)]

両方の表示し、編集モード、書式文字列「{0, d}」、"short 日付"形式で表示される日付。 (お使いのコンピューターは、このチュートリアルで示した画面イメージを異なる形式の表示を構成する場合があります)。

これらの各テンプレート フィールド、デザイナーは使用に注意してください、`Bind`既定では、式を変更する、`Eval`内の式、`ItemTemplate`要素。 `Bind`式利用データで`GridView`コード内のデータにアクセスする必要がある場合に、プロパティを制御します。 このページで使用できるように、コードでは、このデータにアクセスする必要はありません`Eval`、これはより効率的です。 詳細については、次を参照してください。[データ コントロールからデータを取得](https://weblogs.asp.net/davidfowler/archive/2008/12/12/getting-your-data-out-of-the-data-controls.aspx)です。

## <a name="revising-entitydatasource-control-markup-to-improve-performance"></a>パフォーマンスを向上させる EntityDataSource コントロールのマークアップの改訂

マークアップで、`EntityDataSource`コントロールを削除、`ConnectionString`と`DefaultContainerName`属性およびに置き換え、`ContextTypeName="ContosoUniversity.DAL.SchoolEntities"`属性。 これは、変更を作成するたびにする必要があります、`EntityDataSource`オブジェクト コンテキスト クラス内でハードコーディングされているとは異なる接続を使用する必要がない場合を制御します。 使用して、`ContextTypeName`属性は、次の利点を提供します。

- パフォーマンスが向上します。 ときに、`EntityDataSource`データ モデルを使用して、このコントロールの初期化、`ConnectionString`と`DefaultContainerName`属性、追加の作業要求のたびにメタデータの読み込みを実行します。 これは必要ありませんを指定する場合、`ContextTypeName`属性。
- 遅延読み込みが生成されたオブジェクト コンテキスト クラス内の既定で有効に (など`SchoolEntities`このチュートリアルでは) 4.0 では Entity Framework。 つまり、ナビゲーション プロパティが読み込まれる関連データに自動的に必要なときに右。 遅延読み込みは、このチュートリアルで後でさらに詳しく説明します。
- オブジェクト コンテキスト クラスを適用したすべてのカスタマイズ (ここで、`SchoolEntities`クラス) を使用するコントロールに表示される、`EntityDataSource`コントロール。 オブジェクト コンテキスト クラスのカスタマイズは、このチュートリアルの系列には対応できない高度なトピックです。 詳細については、次を参照してください。[エンティティ フレームワーク生成の種類の拡張](https://msdn.microsoft.com/library/dd456844.aspx)です。

マークアップは (プロパティの順序が異なる可能性があります)、次の例のようになります。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample6.aspx)]

`EnableFlattening`属性は外部キー列がエンティティのプロパティとして公開されていないため、Entity Framework の以前のバージョンで必要だった機能を参照します。 現在のバージョンを使用すること*外部キーの関連付け*、つまり、外部キー プロパティですが多対多のアソシエーションの公開されます。 外部キーのプロパティと いいえ、エンティティがある[複合型](https://msdn.microsoft.com/library/bb738472.aspx)、この属性に設定しておくことができます`False`です。 既定値があるため、マークアップから属性を削除しない`True`です。 詳細については、次を参照してください。[フラット化オブジェクト (EntityDataSource)](https://msdn.microsoft.com/library/ee404746.aspx)です。

ページを実行し、受講者と (をフィルター処理するだけの受講者の次のチュートリアルで) 従業員の一覧を参照してください。 最初の名前と姓が一緒に表示されます。

[![Image07](the-entity-framework-and-aspnet-getting-started-part-2/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image35.png)

並べ替えるには、表示を列名をクリックします。

をクリックして**編集**任意の行にします。 最初と最後の名前を変更するテキスト ボックスが表示されます。

[![Image08](the-entity-framework-and-aspnet-getting-started-part-2/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image37.png)

**削除**も動作のボタンをクリックします。 登録日のある行の削除 をクリックし、行は表示されなくなります。 (行登録日せず講習においてインストラクターを表し、参照整合性エラーが発生した可能性があります。 次のチュートリアルでをフィルター処理するだけで受講者を含めるには、このリスト。)

## <a name="displaying-data-from-a-navigation-property"></a>ナビゲーション プロパティからデータを表示します。

コースを受講する数を知りたいと各学生に登録されました。 Entity Framework では、その情報を提供する、`StudentGrades`のナビゲーション プロパティ、`Person`エンティティです。 データベースのデザインが割り当てられているグレードをしなくても、コースに登録できる受講者を許可していないのでこのチュートリアルと仮定できますが導入された行、`StudentGrade`コースに関連付けられているテーブルの行は、コースに登録されていると同じです。 (、`Courses`講習においてインストラクターだけのナビゲーション プロパティがあります)。

使用すると、`ContextTypeName`の属性、`EntityDataSource`コントロール、Entity Framework を自動的に取得ナビゲーション プロパティの情報をそのプロパティにアクセスするとします。 これと呼ばれる*遅延読み込み*です。 ただし、このことができます、効率的なためにできないを個別に呼び出して、データベースの各時間の追加情報が必要になります。 によって返されるすべてのエンティティのナビゲーション プロパティからのデータが必要があります、`EntityDataSource`コントロール、データベースに 1 回の呼び出しでは、エンティティ自体と共に、関連するデータを取得する方が効率的です。 これと呼ばれます*一括読み込み*、ナビゲーション プロパティの一括読み込みを設定して指定して、`Include`のプロパティ、`EntityDataSource`コントロール。

*Students.aspx*、全学生のコースの数を表示する場合、最適です。 そのための一括読み込みします。 すべての受講者を表示するが、コースの数を示す場合のみが必要とするマークアップに加えてコードの記述)、それらのいくつかの遅延読み込み場合があることをお勧めします。

開く、またはに切り替える*Students.aspx*に切り替える**デザイン**ビューで、 `StudentsEntityDataSource`、し、、**プロパティ**ウィンドウのセット、 **Include**プロパティを**StudentGrades**です。 (複数のナビゲーション プロパティを取得する場合は、コンマで区切られた名前を指定できます: たとえば、 **StudentGrades、コース**)。

[![Image19](the-entity-framework-and-aspnet-getting-started-part-2/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image39.png)

切り替える**ソース**ビュー。 `StudentsGridView` 、最後の後に、コントロール`asp:TemplateField`要素では、次の新しいテンプレートのフィールドを追加。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample7.aspx)]

`Eval`式では、ナビゲーション プロパティを参照する`StudentGrades`です。 このプロパティ コレクションが含まれているため、`Count`コースを受講者が登録されている数を表示する使用できるプロパティです。 以降のチュートリアルでは、コレクションではなく 1 つのエンティティを含むナビゲーション プロパティからデータを表示する方法が表示されます。 (使用することはできません注`BoundField`ナビゲーション プロパティからデータを表示する要素です)。

ページを実行し、どのくらいコースに受講者が登録されているが表示されます。

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image41.png)

## <a name="using-a-detailsview-control-to-insert-entities"></a>DetailsView コントロールを使用してエンティティを挿入するには

次の手順を持つページを作成するには、`DetailsView`新しい受講者を追加するために使用するコントロール。 ブラウザーを閉じて、新しい web ページを使用して、作成、 *Site.Master*マスター ページ。 ページの名前を付けます*StudentsAdd.aspx*、しに切り替えると**ソース**ビュー。

既存のマークアップを置き換えるには、次のマークアップを追加、`Content`という名前のコントロール`Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample8.aspx)]

このマークアップを作成、`EntityDataSource`と似ていますで作成したコントロール*Students.aspx*これにより、カーソルを除き、します。 同様、`GridView`のバインドされたフィールドを制御する、`DetailsView`コントロールは、コード化されたエンティティのプロパティを参照する点を除いて、データベースに直接接続されたデータ コントロールの場合と同じに正確にします。 ここで、`DetailsView`コントロールのみ使用される、行を挿入するため、既定のモードを設定する`Insert`です。

ページを実行し、新しい学生を追加します。

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image43.png)

新しい学生を挿入した後を実行する場合、何も起こりません*Students.aspx*、新しい学生情報が表示されます。

## <a name="displaying-data-in-a-drop-down-list"></a>ドロップダウン リストでデータを表示します。

次の手順で学習 databind、`DropDownList`コントロールを使用して設定エンティティを`EntityDataSource`コントロール。 このチュートリアルでは、このリストに多くの操作も行いません。 以降の部分では、使用するリスト部門に関連付けられているコースを表示するのに部門を選択できるようにします。

という名前の新しい web ページを作成する*Courses.aspx*です。 **ソース**に見出しを追加、表示、`Content`というコントロール`Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample9.aspx)]

**デザイン**ビューで、追加、`EntityDataSource`ページにコントロールする前に、実行した方法でこの時間を除くという名前を付けます`DepartmentsEntityDataSource`です。 選択**部門**として、 **EntitySetName**値に設定して、のみを選択、 **DepartmentID**と**名前**プロパティです。

[![Image15](the-entity-framework-and-aspnet-getting-started-part-2/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image45.png)

**標準**のタブ、**ツールボックス**、ドラッグ、`DropDownList`コントロールをページに、名前、 `DepartmentsDropDownList`、スマート タグをクリックし、選択**データ ソースの選択**に開始、**データ ソース構成ウィザード**です。

[![Image16](the-entity-framework-and-aspnet-getting-started-part-2/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image47.png)

**データ ソースを選択**手順で、 **DepartmentsEntityDataSource**をクリックして、データ ソースとして**スキーマの更新**、し、**名前**として表示するデータ フィールドと**DepartmentID**値のデータ フィールドとします。 **[OK]**をクリックします。

[![Image17](the-entity-framework-and-aspnet-getting-started-part-2/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image49.png)

データ バインドする Entity Framework を使用して、コントロールを使用する方法は、他の ASP.NET データ エンティティとエンティティのプロパティ、を除き、ソース コントロールを指定するいると同じです。

切り替える**ソース**表示し、追加"部門を選択:"する直前に、`DropDownList`コントロール。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample10.aspx)]

なおにのマークアップを変更する、`EntityDataSource`に置き換えることで、コントロールをこの時点で、`ConnectionString`と`DefaultContainerName`属性を`ContextTypeName="ContosoUniversity.DAL.SchoolEntities"`属性。 変更する前に、データ ソース コントロールにリンクされているデータ バインド コントロールを作成した後にするまで待機することをおは多くの場合、`EntityDataSource`ため、変更を加えた後、デザイナーが用意されていませんが、マークアップを制御、**更新スキーマ**データ バインド コントロールでオプションです。

ページを実行し、部門は、ドロップダウン リストから選択できます。

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image51.png)

これで完了使用の概要、`EntityDataSource`コントロール。 このコントロールの操作は、エンティティとテーブルと列ではなくプロパティを参照できるようにする点を除いて一般的に使用可能なその他の ASP.NET データ ソース コントロールから変わりませんいます。 唯一の例外は、ナビゲーション プロパティにアクセスする場合です。 次のチュートリアルが表示されます、構文を使用することで`EntityDataSource`フィルター、グループ、およびデータの並べ替え時にコントロールが他のデータ ソース コントロールから異なるも場合があります。

> [!div class="step-by-step"]
> [前へ](the-entity-framework-and-aspnet-getting-started-part-1.md)
> [次へ](the-entity-framework-and-aspnet-getting-started-part-3.md)
