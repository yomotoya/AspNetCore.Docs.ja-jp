---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
title: 'チュートリアル: ASP.NET MVC アプリのより複雑なデータ モデルを作成します。'
author: tdykstra
description: このチュートリアルでは、複数のエンティティとリレーションシップを追加し、書式設定、検証、およびデータベース マッピングの規則を指定することで、データ モデルをカスタマイズします。
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: 46f7f3c9-274f-4649-811d-92222a9b27e2
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 5deab7da776c3c43e3e2cdf42b04922678f956c7
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889991"
---
# <a name="tutorial-create-a-more-complex-data-model-for-an-aspnet-mvc-app"></a>チュートリアル: ASP.NET MVC アプリのより複雑なデータ モデルを作成します。

前のチュートリアルでは、3 つのエンティティで構成されている単純なデータ モデルと連携してください。 エンティティとリレーションシップをさらに追加するこのチュートリアルでは、書式設定、検証、およびデータベース マッピングの規則を指定して、データ モデルをカスタマイズする. この記事では、データ モデルをカスタマイズする 2 つの方法: エンティティ クラス、およびデータベース コンテキスト クラスにコードを追加することで、属性を追加しています。

完了すると、エンティティ クラスは、以下の図のように完成したデータ モデルを構成します。

![School_class_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

このチュートリアルでは、次の作業を行いました。

> [!div class="checklist"]
> * データ モデルをカスタマイズします。
> * Student エンティティを更新します。
> * Instructor エンティティを作成します。
> * OfficeAssignment エンティティを作成します。
> * Course エンティティを変更します。
> * Department エンティティを作成する
> * Enrollment エンティティを変更する
> * データベース コンテキストにコードを追加します。
> * シード データベースにテスト データ
> * 移行を追加する
> * データベースを更新する

## <a name="prerequisites"></a>必須コンポーネント

* [コードの最初の移行と展開](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="customize-the-data-model"></a>データ モデルをカスタマイズします。

このセクションでは、書式設定、検証、データベース マッピング規則を指定する属性を使用して、データ モデルをカスタマイズする方法を示します。 次のセクションでは、いくつかを作成し、完全な`School`を追加してデータ モデルのクラスに属性を既にモデル内の残りのエンティティ型の新しいクラスを作成し、作成します。

### <a name="the-datatype-attribute"></a>DataType 属性

学生の登録日について、すべての Web ページでは現在、日付と共に時刻が表示されていますが、このフィールドでは日付が重要になります。 データ注釈属性を使用すれば、1 つのコードを変更するだけで、データを表示するすべてのビューの表示形式を修正できます。 その方法例を表示するには、`Student` クラスの `EnrollmentDate` プロパティに属性を追加します。

*Models\Student.cs*、追加、`using`のステートメント、`System.ComponentModel.DataAnnotations`名前空間を追加および`DataType`と`DisplayFormat`属性を`EnrollmentDate`プロパティは、次の例に示すように。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,12-13)]

[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性を使用して、データベースの組み込み型よりも具体的であるデータ型を指定します。 この例では、追跡する必要があるのは、日付と時刻ではなく、日付のみです。 [DataType 列挙](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)など多くのデータ型を提供します*日付、時刻、PhoneNumber、Currency、EmailAddress*など。 また、`DataType` 属性を使用して、アプリケーションで型固有の機能を自動的に提供することもできます。 など、`mailto:`に対してリンクを作成できる[DataType.EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)、日付セレクターを指定することができますと[DataType.Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)をサポートするブラウザーで[HTML5](http://html5.org/). [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性は、HTML 5 を出力[データ -](http://ejohn.org/blog/html-5-data-attributes/) ("と発音*データ ダッシュ*) HTML 5 ブラウザーが認識できる属性。 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性は、いずれかの検証を行いません。

`DataType.Date` は、表示される日付の書式を指定しません。 既定では、データ フィールドはサーバーの既定の書式に従って表示[CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx)します。

`DisplayFormat` 属性は、日付の書式を明示的に指定するために使用されます。


[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample2.cs)]


`ApplyFormatInEditMode`設定では、指定した書式設定も適用されることを編集するためのテキスト ボックスが表示されたら、値を指定します。 (たくないをいくつかのフィールドをたとえば、通貨の値のたくない、テキスト ボックスに通貨記号を編集するためです)。

使用することができます、 [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)自体が、属性が使用することをお勧めでは通常、 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性もします。 `DataType`属性の伝達、*セマンティクス*のデータとだけで画面上でのレンダリング方法ではなくられると、次の利点を提供します`DisplayFormat`:

- ブラウザーは HTML5 機能を有効にすることができます (たとえば、カレンダー コントロール、ロケールに適した通貨記号、メール リンク、一部のクライアント側の入力検証を表示するときなど)。
- 既定では、ブラウザーがに基づいて正しい書式を使用してデータを表示、[ロケール](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx)します。
- [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性には、データを表示するために正しいフィールド テンプレートの選択を MVC が有効にすることができます (、 [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)文字列テンプレートを使用)。 詳細については、Brad Wilson を参照してください。 [ASP.NET MVC 2 テンプレート](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html)します。 (MVC 2 用に記述されたもこの記事でまだに適用 ASP.NET MVC の現在のバージョン)。

使用する場合、`DataType`属性指定する必要が、日付フィールドを持つ、 `DisplayFormat` Chrome ブラウザーで、フィールドが正常に表示されることを確認するためにも属性。 詳細については、次を参照してください。[この StackOverflow スレッド](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie)します。

MVC では、その他の日付形式を処理する方法の詳細についてを参照してください[MVC 5 の概要。メソッドの編集とビューの編集](../introduction/examining-the-edit-methods-and-edit-view.md)と検索のページで&quot;国際化&quot;します。

Student インデックス ページを再度実行し、登録日の時刻が表示されていないことに注意してください。 True を使用する任意のビューを同じになります、`Student`モデル。

![Students_index_page_with_formatted_date](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image2.png)

### <a name="the-stringlengthattribute"></a>StringLengthAttribute

属性を使用して、データ検証規則と検証エラー メッセージを指定することもできます。 [StringLength 属性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)データベースの最大長を設定し、クライアント側とサーバー側の ASP.NET MVC の検証。 この属性で最小長を指定することもできますが、最小値はデータベース スキーマに影響しません。

たとえば、ユーザーが 50 文字を超える名前を入力しないようにする必要があるとします。 この制限を追加するには、追加[StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)属性を`LastName`と`FirstMidName`プロパティは、次の例に示すようにします。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=10,12)]

[StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)属性は、名前に空白文字を入力するからユーザーを妨げることはありません。 使用することができます、 [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx)入力に制限を適用する属性。 たとえば、次のコードは、最初の文字を大文字にして、残りの文字はアルファベットにする必要があります。

`[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]`

[MaxLength](https://msdn.microsoft.com/library/System.ComponentModel.DataAnnotations.MaxLengthAttribute.aspx)属性と同様の機能を提供する、 [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)属性が、クライアント側を提供していない検証します。

アプリケーションを実行し、をクリックして、**学生**タブ。次のエラーが発生しました。

*データベースが作成されたために、'[schoolcontext]' のコンテキストのバックアップ モデルが変更されました。データベースを更新する Code First Migrations を使用する ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269))。*

データベース モデルが、データベース スキーマの変更が必要な方法で変更され、Entity Framework では、ことが検出されました。 移行は、UI を使用して、データベースに追加したすべてのデータを失うことがなく、スキーマの更新を使用します。 によって作成されたデータを変更した場合、`Seed`メソッドは、その元の状態のために変更される、 [AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx)メソッドで使用している、`Seed`メソッド。 ([AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx)はデータベース用語から、"upsert"操作に相当します)。

パッケージ マネージャー コンソール (PMC) で、次のコマンドを入力します。

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample4.cmd)]

`add-migration`コマンドでは、という名前のファイルを作成します。 *&lt;タイムスタンプ&gt;\_MaxLengthOnNames.cs*します。 このファイルには、現在のデータ モデルと一致するようにデータベースを更新する `Up` メソッドのコードが含まれます。 `update-database` コマンドでそのコードが実行されました。

移行ファイル名に付加タイムスタンプは、移行を注文する Entity Framework によって使用されます。 実行する前に複数の移行を作成することができます、`update-database`コマンド、およびすべての移行後は、作成された順序で適用されます。

実行、**作成**ページ、および 50 文字より長くのいずれかの名前を入力します。 クリックすると**作成**クライアント側の検証エラー メッセージを示しています。*[姓] フィールドは、50 の最大長の文字列である必要があります。*

### <a name="the-column-attribute"></a>列の属性

属性を使用して、データベースへのクラスとプロパティのマッピング方法を制御することもできます。 フィールドにミドル ネームも含まれている場合があるため、名フィールドに対して `FirstMidName` という名前を使用したとします。 ただし、データベース列は `FirstName` という名前にする必要があります。これは、データベースに対するアドホック クエリを記述するユーザーがその名前に慣れているためです。 このマッピングを作成する場合、`Column` 属性を使用できます。

`Column` 属性は、データベースの作成時に、`FirstMidName` プロパティにマップする `Student` テーブルの列が `FirstName` という名前になるように指定します。 つまり、コードが `Student.FirstMidName` を参照したときに、データが `Student` テーブルの `FirstName` 列から取り込まれるか、更新されます。 列名を指定しない場合は、プロパティ名と同じ名前が表示されます。

*Student.cs*ファイルを追加、`using`ステートメント[System.ComponentModel.DataAnnotations.Schema](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.aspx)列名の属性を追加し、`FirstMidName`プロパティのように、次の強調表示されたコード。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample5.cs?highlight=4,14)]

追加、[列属性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx)ので、データベースと一致しません、SchoolContext のバックアップ モデルを変更します。 別の移行を作成する PMC で、次のコマンドを入力します。

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample6.cmd)]

**サーバー エクスプ ローラー**、オープン、*学生*テーブル デザイナーをダブルクリックして、*学生*テーブル。

次の図は、最初の 2 つの移行を適用する前に、元の列名を示します。 列の名前を変更するだけでなく`FirstMidName`に`FirstName`から 2 つの名前の列が変更された`MAX`50 文字の長さ。

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image5.png)

行うことができますもデータベースを使用して、マッピングの変更、 [Fluent API](https://msdn.microsoft.com/data/jj591617)、このチュートリアルの後半で表示されます。

> [!NOTE]
> 次のセクションですべてのエンティティ クラスの作成を完了する前にコンパイルしようとすると、コンパイラ エラーが発生する可能性があります。

## <a name="update-student-entity"></a>Student エンティティを更新します。

*Models\Student.cs*、追加したコードを前の手順を次のコードに置き換えます。 変更が強調表示されます。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=11,13,15,18,22,25-32)]

### <a name="the-required-attribute"></a>必須の属性

[必須属性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)名前プロパティの必須フィールドを使用します。 `Required attribute`浮き出して double、DateTime、int などの値型に必要ありません。 本質的に必須フィールドとして扱われるようににより、値の型に null 値を割り当てることができません。 削除する可能性があります、[必須属性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)の最小長パラメーターに置き換えると、`StringLength`属性。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample8.cs?highlight=2)]

### <a name="the-display-attribute"></a>表示属性

`Display` 属性は、テキスト ボックスのキャプションを、各インスタンスのプロパティ名 (単語を区切るスペースがない) の代わりに、"First Name"、"Last Name"、"Full Name"、"Enrollment Date" にするよう指定します。

### <a name="the-fullname-calculated-property"></a>FullName 集計プロパティ

`FullName` は集計プロパティであり、2 つの別のプロパティを連結して作成される値を返します。 したがってがのみ、`get`アクセサー、および no`FullName`データベース内の列が生成されます。

## <a name="create-instructor-entity"></a>Instructor エンティティを作成します。

作成*Models\Instructor.cs*、テンプレート コードを次のコードに置き換えます。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample9.cs)]

`Student` エンティティと `Instructor` エンティティのいくつかのプロパティが同じであることに注目してください。 このシリーズの後半の[継承の実装](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)に関するチュートリアルでは、冗長さをなくすため、このコードをリファクタリングします。

次のように、instructor クラスを記述することもできます。 1 つの行に複数の属性を配置できます。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

### <a name="the-courses-and-officeassignment-navigation-properties"></a>コースと OfficeAssignment ナビゲーション プロパティ

`Courses` と `OfficeAssignment` プロパティはナビゲーション プロパティです。 として定義は通常、以前に説明したよう[仮想](https://msdn.microsoft.com/library/9fkccyh4(v=vs.110).aspx)と呼ばれる Entity Framework 機能の利用できるように[遅延読み込み](https://msdn.microsoft.com/magazine/hh205756.aspx)します。 さらに、ナビゲーション プロパティが複数のエンティティを保持できる場合、型する必要がありますを実装、 [ICollection&lt;T&gt; ](https://msdn.microsoft.com/library/92t2ye13.aspx)インターフェイス。 たとえば[IList&lt;T&gt; ](https://msdn.microsoft.com/library/5y536ey6.aspx)はなく修飾[IEnumerable&lt;T&gt; ](https://msdn.microsoft.com/library/9eekhta0.aspx)ため`IEnumerable<T>`実装していません[追加](https://msdn.microsoft.com/library/63ywd54z.aspx)。

インストラクターが任意の数のコースを教えることができますので、`Courses`のコレクションとして定義されて`Course`エンティティ。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

インストラクターのみが 1 つのオフィスので、ビジネス ルールの状態`OfficeAssignment`は、1 つとして定義`OfficeAssignment`エンティティ (なる可能性がある`null`オフィスが割り当てられていない場合)。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

## <a name="create-officeassignment-entity"></a>OfficeAssignment エンティティを作成します。

作成*Models\OfficeAssignment.cs*を次のコード。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

変更を保存し、すべてのコピーを作成していないことを確認すると、プロジェクトをビルドし、貼り付けエラー コンパイラで検出できます。

### <a name="the-key-attribute"></a>キー属性

0 または 1 に 1 つの関係がある、 `Instructor` 、`OfficeAssignment`エンティティ。 オフィスの割り当てに、割り当てられている講師についてのみ存在であるため、主キーもに対する外部キー、`Instructor`エンティティ。 Entity Framework を自動的に認識できませんが、`InstructorID`プライマリとしてその名前に従っていないため、このエンティティのキー、`ID`または*classname* `ID`名前付け規則。 したがって、`Key` 属性はキーとして識別するために使用されます。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample14.cs)]

使用することも、`Key`属性が、エンティティが自身のプライマリ キーを持って、異なる点が、プロパティの名前を付ける場合`classnameID`または`ID`します。 既定では EF では、列は依存リレーションシップに対するものであるために、キーを非データベース生成として扱います。

### <a name="the-foreignkey-attribute"></a>不変属性

0 または 1 に 1 つのリレーションシップまたは 2 つのエンティティ間の一対一のリレーションシップがある場合 (との間でこのような`OfficeAssignment`と`Instructor`)、EF のリレーションシップが終了するは、プリンシパルとエンドが依存する動作できません。 一対一のリレーションシップでは、他のクラスには、各クラスの参照ナビゲーション プロパティがあります。 [不変属性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx)の関係を確立するために依存するクラスに適用することができます。 省略した場合、[不変属性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx)移行を作成するときに、次のエラーを取得します。

*型 'ContosoUniversity.Models.OfficeAssignment' および 'ContosoUniversity.Models.Instructor' 間のアソシエーションのプリンシパル end を特定できません。このアソシエーションのプリンシパル end は、データ注釈または fluent API のリレーションシップを使用して明示的に構成する必要があります。*

チュートリアルの後半では、fluent API を使用したこの関係を構成する方法を確認します。

### <a name="the-instructor-navigation-property"></a>Instructor ナビゲーション プロパティ

`Instructor`エンティティが null 許容`OfficeAssignment`ナビゲーション プロパティ (講師にオフィスが割り当てをいない可能性があります) ため、および`OfficeAssignment`エンティティには、null 非許容`Instructor`ナビゲーション プロパティ (オフィスの割り当てができないため、--講師なしで存在`InstructorID`は null 非許容の)。 ときに、`Instructor`エンティティには、関連する`OfficeAssignment`エンティティ、各エンティティはそのナビゲーション プロパティで、もう 1 つへの参照になります。

配置すること、`[Required]`関連のインストラクターでは、必要がありますが、必要はありません (これもこのテーブルにキーには) InstructorID 外部キーが null 非許容のために指定する Instructor ナビゲーション プロパティの属性。

## <a name="modify-the-course-entity"></a>Course エンティティを変更します。

*Models\Course.cs*、追加したコードを前の手順を次のコードに置き換えます。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

Course エンティティが外部キー プロパティ`DepartmentID`どの地点を関連する`Department`エンティティとそれが、`Department`ナビゲーション プロパティ。 Entity Framework では、関連エンティティのナビゲーション プロパティがある場合、ユーザーがデータ モデルに外部キー プロパティを追加する必要はありません。 EF は、必要な場合に自動的に外部キーと、データベースに作成します。 ただし、データ モデルに外部キーがある場合は、更新をより簡単かつ効率的に行うことができます。 を編集する course エンティティをフェッチするときなど、`Department`エンティティは null に読み込まない場合ので、course エンティティを更新するときにしなければならないという最初のフェッチ、`Department`エンティティ。 ときに、外部キー プロパティ`DepartmentID`が含まれる、データ モデルをフェッチする必要はありません、`Department`エンティティを更新する前にします。

### <a name="the-databasegenerated-attribute"></a>DatabaseGenerated 属性

[DatabaseGenerated 属性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute.aspx)で、 [None](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.110).aspx)上のパラメーター、`CourseID`を主キーの値は、ユーザーによって提供されるではなく、データベースによって生成された、プロパティを指定します。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

既定では、Entity Framework は主キー値がデータベースによって生成されることを想定しています。 これはほとんどのシナリオに該当します。 ただし、`Course`エンティティ、1 つの部門別の学科に 2000年シリーズに 1000 シリーズなどのユーザー指定のコース番号を使用し、具合です。

### <a name="foreign-key-and-navigation-properties"></a>Foreign Key 制約とナビゲーション プロパティ

外部キー プロパティとナビゲーション プロパティに、`Course`エンティティには、次のリレーションシップが反映されます。

- コースは 1 つの学科に割り当てられます。したがって、前述の理由により、`DepartmentID` 外部キーと `Department` ナビゲーション プロパティが存在します。

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]
- コースには任意の数の学生が登録できるため、`Enrollments` ナビゲーション プロパティはコレクションとなります。

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample18.cs)]
- コースは複数の講師が担当する場合があるため、`Instructors` ナビゲーション プロパティはコレクションとなります。

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample19.cs)]

## <a name="create-the-department-entity"></a>Department エンティティを作成する

作成*Models\Department.cs*を次のコード。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample20.cs)]

### <a name="the-column-attribute"></a>列の属性

以前を使用して、[列属性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx)列名のマッピングを変更します。 コードで、 `Department` 、エンティティ、`Column`属性は、SQL データ型マッピングの SQL Server を使用して列を定義するように変更に使用されている[money](https://msdn.microsoft.com/library/ms179882.aspx)データベース内の型。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample21.cs)]

列マッピングは一般に必要な場合は、Entity Framework は通常、プロパティの定義された CLR 型に基づいて適切な SQL Server データ型が選択されるためです。 CLR `decimal` 型は SQL Server の `decimal` 型にマップされます。 通貨額、列が保持されるわかってここでは、 [money](https://msdn.microsoft.com/library/ms179882.aspx)データ型がより適しています。 CLR データ型と SQL Server データ型に一致する方法の詳細については、次を参照してください。[エンティティ FrameworkTypes 用 SqlClient](https://msdn.microsoft.com/library/bb896344.aspx)します。

### <a name="foreign-key-and-navigation-properties"></a>Foreign Key 制約とナビゲーション プロパティ

外部キーおよびナビゲーション プロパティには、次のリレーションシップが反映されます。

- 学科には管理者が存在する場合とそうでない場合があり、管理者は常に講師となります。 そのため、`InstructorID`プロパティがへの外部キーとして含まれる、`Instructor`後に、エンティティと疑問符が追加、`int`タイプのプロパティを null 許容型としてマークを指定します。ナビゲーション プロパティの名前が`Administrator`を保持するが、`Instructor`エンティティ。

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample22.cs)]
- 部門は多くのコースを必要がありますが、`Courses`ナビゲーション プロパティ。

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample23.cs)]

  > [!NOTE]
  > 規則により、Entity Framework では null 非許容の外部キーと多対多リレーションシップに対して連鎖削除が有効になります。 これにより、循環連鎖削除規則が適用される可能性があり、移行を追加しようとすると例外が発生します。 たとえば、定義していない場合、`Department.InstructorID`プロパティを null 許容型として、次の例外メッセージを取得は。「参照関係になります、循環参照は許可されていません。」 ビジネス ルールが必要な場合`InstructorID`プロパティが null 許容、以下の fluent API ステートメントを使用して、リレーションシップで連鎖削除を無効にする必要があります。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample24.cs)]

## <a name="modify-the-enrollment-entity"></a>Enrollment エンティティを変更する

 *Models\Enrollment.cs*、追加したコードを前の手順を次のコードに置き換えます

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample25.cs?highlight=1,15)]

### <a name="foreign-key-and-navigation-properties"></a>Foreign Key 制約とナビゲーション プロパティ

外部キー プロパティとナビゲーション プロパティには、次のリレーションシップが反映されます。

- 登録レコードは単一のコースに対するものであるため、`CourseID` 外部キー プロパティと `Course` ナビゲーション プロパティがあります。

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample26.cs)]
- 登録レコードは 1 人の学生に対するものであるため、`StudentID` 外部キー プロパティと `Student` ナビゲーション プロパティがあります。

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample27.cs)]

### <a name="many-to-many-relationships"></a>多対多リレーションシップ

多対多リレーションシップがあります、`Student`と`Course`エンティティ、および`Enrollment`エンティティは多対多の結合テーブルとして機能*ペイロードを使用した*データベース内。 つまり、`Enrollment`テーブルには、結合されたテーブルの外部キー以外に追加データが含まれています (この場合は、主キーと`Grade`プロパティ)。

次の図は、エンティティ図でこれらのリレーションシップがどのようになるかを示しています  (この図を使用して生成された、 [Entity Framework Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d); チュートリアルの一部ではない図の作成、使用するだけの図は、ここでします)。

![学生 many_relationship Course_many](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image12.png)

各リレーションシップの線が 1 つの end とアスタリスクの 1 (\*)、他方で一対多のリレーションシップを示します。

場合、`Enrollment`テーブルに成績情報が含まれていなかった、2 つの外部キーを含めることはのみ`CourseID`と`StudentID`します。 その場合は、多対多結合テーブルに対応して、*ペイロードがない*(または*純粋結合テーブル*)、データベース内とそのすべてのモデル クラスを作成する必要はありません。 `Instructor`と`Course`エンティティがその種の多対多のリレーションシップがあるし、それらの間でエンティティ クラスはありません、ご覧のとおり。

![Instructor many_relationship Course_many](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image13.png)

ただし結合テーブルがデータベースに必要なデータベースの次の図のように。

![Instructor many_relationship_tables Course_many](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image14.png)

Entity Framework を自動的に作成、`CourseInstructor`して、テーブルの読み取りし、更新しない、直接の読み取りと更新によって、`Instructor.Courses`と`Course.Instructors`ナビゲーション プロパティ。

## <a name="entity-relationship-diagram"></a>エンティティ関係図

次の図では、完成した School モデルに対して Entity Framework Power Tools で作成される図を示します。

![School_data_model_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

多対多リレーションシップの線だけでなく (\*に\*) と一対多リレーションシップの線 (1 ~ \*)、ご覧のとおり、0 または 1 に 1 つのリレーションシップの線 (1 対 0..1) の間、`Instructor`と`OfficeAssignment`エンティティと 0-または-1-対多リレーションシップの線 (0..1 対\*) Instructor および Department エンティティの間。

## <a name="add-code-to-database-context"></a>データベース コンテキストにコードを追加します。

次に、新しいエンティティを追加します、`SchoolContext`クラスし、マッピングを使用して、一部をカスタマイズ[fluent API](https://msdn.microsoft.com/data/jj591617)呼び出し。 多くの場合、一連の次の例のように、1 つのステートメントにまとめてメソッドの呼び出しで使用されているために、API は"fluent"。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample28.cs)]

このチュートリアルでは、属性は実行できないデータベース マッピングに対してのみ、fluent API を使用します。 ただし、fluent API を使用して、属性で実行できる書式設定、検証、およびマッピング規則のほとんどを指定することもできます。 `MinimumLength` などの一部の属性は fluent API で適用できません。 前述の`MinimumLength`スキーマを変更せず、クライアントとサーバー側の検証規則のみ適用されます

一部の開発者は fluent API のみを使用することを選ぶため、エンティティ クラスを "クリーン" な状態に保つことができます。 必要に応じて、属性と fluent API を組み合わせて使用することができます。fluent API のみを使用して実行できるカスタマイズがいくつかありますが、一般的は 2 つの方法のいずれかを選択して、できるだけ一貫性を保つためにそれを使用することをお勧めします。

モデルのデータに新しいエンティティおよび属性を使用して行われなかったデータベース マッピングの実行を追加するには、コードを置き換えます*DAL\SchoolContext.cs*を次のコード。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample29.cs)]

新しいステートメント、 [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx)メソッドは、多対多の結合テーブルを構成します。

- 多対多のリレーションシップ、`Instructor`と`Course`エンティティ、コードは、結合テーブルのテーブルと列名を指定します。 コード最初を構成できます多対多のリレーションシップがこのコードがないがそれを呼び出さない場合、既定の名前をなど取得は`InstructorInstructorID`の`InstructorID`列。

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample30.cs)]

次のコードの方法を使用しても fluent API 属性ではなく間のリレーションシップを指定する例を示します、`Instructor`と`OfficeAssignment`エンティティ。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample31.cs)]

バック グラウンドで実行は"fluent API"ステートメントについては、次を参照してください。、 [Fluent API](https://blogs.msdn.com/b/aspnetue/archive/2011/05/04/entity-framework-code-first-tutorial-supplement-what-is-going-on-in-a-fluent-api-call.aspx)ブログの投稿。

## <a name="seed-database-with-test-data"></a>シード データベースにテスト データ

コードに置き換えます、 *migrations \configuration.cs*を作成した新しいエンティティのシード データを提供するために次のコード ファイル。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample32.cs)]

最初のチュートリアルで学習したようこのコードのほとんどだけを更新または新しいエンティティ オブジェクトを作成し、テストに必要なプロパティにサンプル データを読み込みます。 ただし、方法、 `Course` 、多対多リレーションシップを持つエンティティで、`Instructor`エンティティの処理します。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample33.cs)]

作成するときに、`Course`オブジェクトを初期化する、`Instructors`ナビゲーション プロパティをコードを使用して空のコレクションとして`Instructors = new List<Instructor>()`します。 追加が可能になります。`Instructor`これに関連するエンティティ`Course`を使用して、`Instructors.Add`メソッド。 空のリストを作成していない場合はできるこれらの関係を追加するため、`Instructors`プロパティは null にして、必要はありません、`Add`メソッド。 コンス トラクターに、リストの初期化を追加することもできます。

## <a name="add-a-migration"></a>移行を追加する

PMC から入力、`add-migration`コマンド (しないで、`update-database`コマンドはまだ)。

`add-Migration ComplexDataModel`

この時点で `update-database` コマンドを実行しようとした場合は (まだ実行しないでください)、以下のようなエラーが表示されます。

*ALTER TABLE ステートメントと競合して外部キー制約"FK\_dbo します。コース\_dbo します。部門\_DepartmentID"。競合が発生したデータベース"ContosoUniversity"テーブル"dbo します。部署"、列 'DepartmentID' です。*

既存のデータの移行を実行するとき、外部キー制約を満たすために、データベースにスタブ データを挿入する必要があるとはここで行う必要があります。 ComplexDataModel で生成されたコード`Up`メソッドを追加、null 非許容`DepartmentID`への外部キー、`Course`テーブル。 内の行が既に存在しているため、`Course`表に、コードを実行すると、 `AddColumn` SQL Server では、null にできない列に格納される値が認識しないため、操作は失敗します。 そのため、既定値を新しい列を付けるし、既定の学科として機能するには、"Temp"という名前のスタブ学科を作成するコードを変更する必要です。 その結果、既存の`Course`行すべてに関連付けられます後に"Temp"学科、`Up`メソッドを実行します。 適切な部門に関連付けることができます、`Seed`メソッド。

編集、 &lt;*タイムスタンプ&gt;\_ComplexDataModel.cs*ファイルを Course テーブルに、[DepartmentID] 列を追加するコードの行をコメント アウトし、次の強調表示されたコードが (コメントを追加行も強調表示されます)。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample34.cs?highlight=14-18)]

ときに、`Seed`メソッドを実行内の行が挿入されますが、`Department`テーブルの既存の関連`Course`これら新しい行`Department`行。 UI でどのコースも受講を追加していない場合が不要になった"Temp"学科または既定値で、`Course.DepartmentID`列。 を、だれかが追加可能性がコース、アプリケーションを使用して可能性を許可するのにはも更新する、`Seed`ことを確認するメソッドのコードすべて`Course`行 (の以前の実行によって挿入されたものだけでなく、`Seed`メソッド) があります。有効な`DepartmentID`値、既定値を削除する前に、列の値し、"Temp"学科を削除します。

## <a name="update-the-database"></a>データベースを更新する

編集が完了した後、 &lt;*タイムスタンプ&gt;\_ComplexDataModel.cs*ファイルで、入力、`update-database`移行を実行する PMC コマンド。

[!code-powershell[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample35.ps1)]

> [!NOTE]
> データとスキーマの変更を移行するときにその他のエラーが発生することになります。 解決できない移行エラーが発生した場合は、接続文字列のデータベース名を変更するか、データベースを削除できます。 データベースの名前を変更する最も簡単な方法は、 *Web.config*ファイル。 次の例では CU へ変更名\_テスト。
>
> [!code-xml[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample36.xml?highlight=1)]
>
> 新しいデータベースを移行するデータがないと、`update-database`コマンドがエラーなしで完了する可能性が高くなります。 データベースを削除する方法の詳細については、次を参照してください。 [Visual Studio 2012 からデータベースを削除する方法](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/)します。
>
> 失敗した場合、PMC で、次のコマンドを入力して、データベースを再初期化が、もう 1 つを試みることができます。
>
> `update-database -TargetMigration:0`

データベースを開く**サーバー エクスプ ローラー**以前を展開すると、**テーブル**ノードをすべてのテーブルが作成されたことを確認します。 (がある場合**サーバー エクスプ ローラー**以前の状態からを開き、、**更新**ボタンをクリックします)。

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image16.png)

モデル クラスを作成していない、`CourseInstructor`テーブル。 これは、多対多のリレーションシップの結合テーブルで前に説明したように、`Instructor`と`Course`エンティティ。

右クリックし、`CourseInstructor`テーブルを選択**テーブル データの表示**がの結果としてデータがあることを確認する、`Instructor`に追加したエンティティ、`Course.Instructors`ナビゲーション プロパティ。

![Table_data_in_CourseInstructor_table](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image17.png)

## <a name="get-the-code"></a>コードを取得する

[完成したプロジェクトのダウンロード](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>その他の技術情報

その他の Entity Framework リソースへのリンクが記載されて、 [ASP.NET データ アクセス - 推奨リソース](../../../../whitepapers/aspnet-data-access-content-map.md)します。

## <a name="next-steps"></a>次の手順

このチュートリアルでは、次の作業を行いました。

> [!div class="checklist"]
> * データ モデルのカスタマイズ
> * 更新された Student エンティティ
> * Instructor エンティティの作成
> * OfficeAssignment エンティティの作成
> * Course エンティティの変更
> * Department エンティティを作成します。
> * Enrollment エンティティの変更
> * データベース コンテキストにコードを追加しました
> * シード データベースにテスト データ
> * 移行を追加
> * データベースの更新

読み取りおよびに Entity Framework がナビゲーション プロパティを読み込む関連のデータを表示する方法については、次の記事に進んでください。

> [!div class="nextstepaction"]
> [関連データの読み取り](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
