---
title: "EF Core - データ モデル - 8 の 5 と razor ページ"
author: rick-anderson
description: "このチュートリアルでは、複数のエンティティとリレーションシップを追加し、書式設定、検証、およびデータベース マッピング規則を指定することによって、データ モデルをカスタマイズします。"
ms.author: riande
manager: wpickett
ms.date: 10/25/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: c375fe6ea98c621012eb55589c8b174c2a95b697
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="creating-a-complex-data-model---ef-core-with-razor-pages-tutorial-5-of-8"></a>EF コア Razor ページのチュートリアル (5/8) に、複雑なデータ モデルを作成します。

によって[Tom Dykstra](https://github.com/tdykstra)と[Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

前のチュートリアルは、3 つのエンティティで構成されている基本的なデータ モデルと連携しています。 このチュートリアルでは。

* 複数のエンティティとリレーションシップが追加されます。
* データ モデルをカスタマイズするには、書式設定、検証、およびデータベース マッピング規則を指定します。

完成したデータ モデルのエンティティ クラスは、次の図に表示されます。

![エンティティの図](complex-data-model/_static/diagram.png)

問題を解決できない場合に実行する場合は、ダウンロード、[この段階でのアプリを完成](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part5-complex)です。

## <a name="customize-the-data-model-with-attributes"></a>データ モデルとその属性をカスタマイズします。

このセクションで、データ モデルは、属性の使用をカスタマイズします。

### <a name="the-datatype-attribute"></a>データ型の属性

現在、学生のページには、登録日の時刻が表示されます。 通常、のみ日付と時刻ではなく、日付フィールドが表示されます。

更新*Models/Student.cs*次のようにコードを強調表示されます。

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

[DataType](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1)属性は、データベースの組み込み型よりも特定するデータ型を指定します。 ここでは、日付のみを表示するか、日付と時刻がありません。 [DataType 列挙](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1)日付、時刻、PhoneNumber、通貨、EmailAddress など、多くのデータ型を提供します。`DataType`属性も自動的に機能を提供する型固有のアプリを有効にすることができます。 例:

* `mailto:`リンクが自動的に作成`DataType.EmailAddress`です。
* 日付の選択が提供される`DataType.Date`ほとんどのブラウザーでします。

`DataType`属性は、HTML 5 を出力`data-`HTML 5 ブラウザーを使用する (と読みますデータ dash) の属性です。 `DataType`属性は、検証を渡さないようにします。

`DataType.Date` は、表示される日付の書式を指定しません。 既定では、基に、サーバーの既定の形式に従って日付フィールドを表示[CultureInfo](https://docs.microsoft.com/aspnet/core/fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support)です。

`DisplayFormat` 属性は、日付の書式を明示的に指定するために使用されます。

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

`ApplyFormatInEditMode`を書式設定する必要がありますにも適用編集 UI 設定を指定します。 一部のフィールドを使用しないでください`ApplyFormatInEditMode`です。 たとえば、通貨記号必要があります通常に含めません編集ボックス。

`DisplayFormat`属性は、単独で使用することができます。 一般に、使用することをお勧め、`DataType`属性が、`DisplayFormat`属性。 `DataType`属性は、画面に表示する方法ではなく、データのセマンティクスが示されます。 `DataType`属性では使用できない、次の利点を提供する`DisplayFormat`:

* ブラウザーには、HTML5 機能が有効にすることができます。 たとえば、予定表コントロール、ロケールに応じた通貨記号、電子メールのリンク、クライアント側の入力の検証などを表示します。
* 既定では、ブラウザーは、ロケールに基づく正しい形式を使用してデータを表示します。

詳細については、次を参照してください。、 [\<入力 > タグ ヘルパーのドキュメント](xref:mvc/views/working-with-forms#the-input-tag-helper)です。

アプリを実行します。 受講者インデックス ページに移動します。 時刻は表示されません。 使用するすべてのビュー、`Student`モデルには、時刻のない日付が表示されます。

![受講者時刻なしの日付が表示されたページをインデックスします。](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a>StringLength 属性

データの検証規則と検証エラー メッセージは、属性で指定できます。 [StringLength](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1)属性は、データ フィールドで許可される文字の最小値と最大の長さを指定します。 `StringLength`属性には、クライアント側およびサーバー側の検証も用意されています。 最小値は、データベース スキーマには影響を与えません。

更新プログラム、`Student`を次のコード モデル。

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

上記のコードでは、50 個の文字に名前を制限します。 `StringLength`属性を防ぐユーザー名を空白文字を入力します。 [正規表現](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1)属性は、制限を適用する入力に使用します。 たとえば、次のコードには、最初の文字が大文字で指定し、残りの文字は英文字でが必要です。

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

アプリを実行します。

* 学生のページに移動します。
* 選択**新規作成**、50 文字より長い名前を入力します。
* 選択**作成**クライアント側の検証エラー メッセージが表示されます。

![学生のインデックス文字列の長さエラーを示すページ](complex-data-model/_static/string-length-errors.png)

**SQL Server オブジェクト エクスプ ローラー** (SSOX) をダブルクリックして、Student テーブル デザイナーを開き、**学生**テーブル。

![移行する前に SSOX で students テーブル](complex-data-model/_static/ssox-before-migration.png)

上記の図に、スキーマを`Student`テーブル。 型名フィールドである`nvarchar(MAX)`DB での移行が実行されていないためです。 実行すると移行は、このチュートリアルの後半で、名前フィールドは次のようになります。`nvarchar(50)`です。

### <a name="the-column-attribute"></a>列の属性

属性は、クラスとプロパティをデータベースにマップする方法を制御できます。 このセクションで、`Column`の名前にマップするための属性、 `FirstMidName` db では、"firstname"のプロパティです。

列名のモデルのプロパティ名が使用されるデータベースの作成時に (する場合を除く、`Column`属性を使用)。

`Student`モデルの使用`FirstMidName`最初の-名前のフィールドのフィールドには、ミドル ネームが含まれる場合もあるためです。

更新プログラム、 *Student.cs*を次の強調表示されたコード ファイル。

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

前の変更に`Student.FirstMidName`アプリをマップ、`FirstName`の列、`Student`テーブル。

追加、`Column`属性がサポートするモデルを変更、`SchoolContext`です。 モデルのバックアップ、`SchoolContext`データベースと一致しません。 移行を適用する前に、アプリを実行した場合は、次の例外が生成されます。

```SQL
SqlException: Invalid column name 'FirstName'.
```
DB の更新。

* プロジェクトをビルドします。
* プロジェクト フォルダーで、コマンド ウィンドウを開きます。 新しい移行を作成し、データベースを更新するには、次のコマンドを入力します。

    ```console
    dotnet ef migrations add ColumnFirstName
    dotnet ef database update
    ```

`dotnet ef migrations add ColumnFirstName`コマンドには、次の警告メッセージが生成されます。

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

名前フィールドは、ようになりましたので、警告が生成 50 文字に制限されます。 Db 名には、50 個を超える文字が含まれていた、最後の文字を 51 が失われます。

* アプリをテストします。

SSOX では、Student テーブルを開きます。

![移行した後で SSOX students テーブル](complex-data-model/_static/ssox-after-migration.png)

型の名前の列がされた移行が適用される前に、 [nvarchar (max)](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql)です。 名前の列は、今すぐ`nvarchar(50)`です。 列名がから変更`FirstMidName`に`FirstName`です。

> [!Note]
> 次のセクションでアプリケーションをビルドするいくつかの段階でコンパイラ エラーが生成されます。 手順では、アプリをビルドするときに指定します。

## <a name="student-entity-update"></a>学生のエンティティの更新

![学生エンティティ](complex-data-model/_static/student-entity.png)

更新*Models/Student.cs*を次のコード。

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a>必須の属性

`Required`属性は、名前のプロパティの必須フィールドです。 `Required`値の型などの非 null 許容の型の属性は必要ありません (`DateTime`、 `int`、 `double`, などです。)。 Null にすることはできません型は、必要なフィールドとして自動的に扱われます。

`Required`に最小の長さのパラメーターに属性を置き換えることが、`StringLength`属性。

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a>表示属性

`Display`属性は、テキスト ボックスのキャプションにする必要があります「姓」、"Last Name"、「完全名」および「登録日」を指定します 既定のキャプションには、たとえば"Lastname と"単語を分割空白がないです。

### <a name="the-fullname-calculated-property"></a>計算 FullName プロパティ

`FullName`その他の 2 つのプロパティを連結することによって作成される値を返す計算されるプロパティです。 `FullName`設定された場合、get アクセサーだけを持つことはできません。 いいえ`FullName`列が、データベース内に作成します。

## <a name="create-the-instructor-entity"></a>Instructor エンティティを作成します。

![Instructor エンティティ](complex-data-model/_static/instructor-entity.png)

作成*Models/Instructor.cs*を次のコード。

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

いくつかのプロパティは、同じことに注意してください、`Student`と`Instructor`エンティティです。 チュートリアルでは、継承を実装するこのシリーズの後で、このコードは、重複を排除するリファクターされています。

複数の属性は、1 つの行に配置できます。 `HireDate`属性は次のように記述された可能性があります。

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a>CourseAssignments と OfficeAssignment ナビゲーションのプロパティ

`CourseAssignments`と`OfficeAssignment`プロパティは、ナビゲーション プロパティ。

インストラクターは任意の数のコースを教えることができますので、`CourseAssignments`コレクションとして定義されます。

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

場合は、ナビゲーション プロパティは、複数のエンティティを保持します。

* 場所エントリを追加、削除、更新できるリストの種類があります。

ナビゲーション プロパティの型は次のとおりです。

* `ICollection<T>`
*  `List<T>`
*  `HashSet<T>`

場合`ICollection<T>`を指定すると、EF コアを作成、`HashSet<T>`既定のコレクション。

`CourseAssignment`エンティティが、多対多リレーションシップのセクションで説明します。

Contoso 大学のビジネス ルールをインストラクターが最大で 1 つのオフィスを持つことができる状態です。 `OfficeAssignment`プロパティは、1 つを保持`OfficeAssignment`エンティティです。 `OfficeAssignment`office が割り当てられていない場合は null です。

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a>OfficeAssignment エンティティを作成します。

![OfficeAssignment エンティティ](complex-data-model/_static/officeassignment-entity.png)

作成*Models/OfficeAssignment.cs*を次のコード。

[!code-csharp[Main](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a>キー属性

`[Key]`属性の識別に使用、プロパティ、PK (主キー) としてプロパティ名が何か classnameID または ID 以外

0 または 1 を 1 つの関係がある、`Instructor`と`OfficeAssignment`エンティティです。 Office の代入はのみに割り当てられているインストラクターに関連して存在します。 `OfficeAssignment` PK も、外部キー (FK) に、`Instructor`エンティティです。 EF コアを自動的に認識できない`InstructorID`の主キーとして`OfficeAssignment`のため。

* `InstructorID`ID または classnameID 名前付け規則に従っていません。

したがって、`Key`属性を使用して識別`InstructorID`PK として。

```csharp
[Key]
public int InstructorID { get; set; }
```

既定では、EF Core では、列はリレーションシップ用のため、キーをデータベースによって生成された非として扱います。

### <a name="the-instructor-navigation-property"></a>講師ナビゲーション プロパティ

`OfficeAssignment`のナビゲーション プロパティ、`Instructor`エンティティが null 許容ため。

* 参照型 (などのクラスは、null を許容) にします。
* 講師がオフィス割り当てをいない可能性があります。


`OfficeAssignment`エンティティには、null 非許容`Instructor`ナビゲーション プロパティのため。

* `InstructorID`null 非許容です。
* オフィス割り当ては、あるインストラクターなしに存在できません。

ときに、`Instructor`エンティティに関連する`OfficeAssignment`エンティティ、各エンティティのナビゲーション プロパティで、もう 1 つを参照しています。

`[Required]`に対して属性を適用できませんでした、`Instructor`ナビゲーション プロパティ。

```csharp
[Required]
public Instructor Instructor { get; set; }
```

上記のコードでは、関連するインストラクターが存在する必要がありますを指定します。 上記のコードは必要ありませんので、 `InstructorID` (これは主キーではも) 外部キーが null 非許容されます。

## <a name="modify-the-course-entity"></a>Course エンティティを変更します。

![Course エンティティ](complex-data-model/_static/course-entity.png)

更新*Models/Course.cs*を次のコード。

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

`Course`エンティティ プロパティが含まれる外部キー (FK)`DepartmentID`です。 `DepartmentID`関連するを指す`Department`エンティティです。 `Course`エンティティには、`Department`ナビゲーション プロパティ。

EF コアは、モデルに関連するエンティティのナビゲーション プロパティがある場合、データ モデルの外部キー プロパティを必要はありません。

EF コアを使用、データベースには、必要に応じての FKs が自動的に作成します。 EF コア作成[プロパティをシャドウ](https://docs.microsoft.com/ef/core/modeling/shadow-properties)自動的に作成された FKs 用です。 データ モデル内に、外部キーを含めると、簡単かつ効率的に更新を行うことができます。 たとえば、モデル、FK プロパティ`DepartmentID`は*いない*含まれています。 ときにコース エンティティは編集にフェッチされます。

* `Department`エンティティが明示的に読み込まれている場合は null です。
* Course エンティティを更新する、`Department`エンティティをフェッチ最初必要があります。

ときに、外部キー プロパティ`DepartmentID`が含まれるデータ モデルでフェッチする必要はありません、`Department`更新の前にエンティティです。

### <a name="the-databasegenerated-attribute"></a>DatabaseGenerated 属性

`[DatabaseGenerated(DatabaseGeneratedOption.None)]`を主キーは、アプリケーションによって提供されるではなくデータベースによって生成される、属性を指定します。

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

既定では、EF コアは、DB で主キー値が生成されると仮定します。 DB には主キーが生成される値は、最善の方法では、通常、します。 `Course` PK がユーザーのエンティティを指定します たとえば、数値演算する部署の 1000 シリーズ、英語版の部門の 2000年シリーズなどのコース数。

`DatabaseGenerated`属性を使用して既定値を生成することもできます。 たとえば、DB では、行が作成または更新された日時を記録する日付フィールドを自動的に生成できます。 詳細については、次を参照してください。[生成プロパティ](https://docs.microsoft.com/ef/core/modeling/generated-properties)です。

### <a name="foreign-key-and-navigation-properties"></a>外部キー プロパティとナビゲーション プロパティ

外部キー (FK) プロパティとナビゲーション プロパティで、`Course`エンティティは、次のリレーションシップを反映します。

コースを 1 つの部門に割り当てられるがあるため、 `DepartmentID` FK と`Department`ナビゲーション プロパティ。

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

コースに受講者に、登録済みの任意の数を持つことができますので、`Enrollments`ナビゲーション プロパティがコレクション。

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

複数のインストラクターを習得するは、コースのため、`CourseAssignments`ナビゲーション プロパティがコレクション。

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

`CourseAssignment`説明は[後](#many-to-many-relationships)です。

## <a name="create-the-department-entity"></a>部門 エンティティを作成します。

![部門 エンティティ](complex-data-model/_static/department-entity.png)

作成*Models/Department.cs*を次のコード。

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a>列の属性

以前、`Column`列名のマッピングを変更する属性が使用されています。 コードで、 `Department` 、エンティティ、 `Column` SQL データ型マッピングを変更する属性を使用します。 `Budget` Db では、SQL Server の money 型を使用して列を定義します。

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

列マッピングは通常必要ありません。 EF コアは、一般に、プロパティの CLR 型に基づく適切な SQL Server データ型を選択します。 CLR `decimal` SQL Server へのマップ」と入力`decimal`型です。 `Budget`通貨の場合は、money データ型は通貨に適しているとします。

### <a name="foreign-key-and-navigation-properties"></a>外部キー プロパティとナビゲーション プロパティ

FK およびナビゲーションのプロパティは、次のリレーションシップを反映します。

* 部門では、可能性があります。 または管理者がない可能性があります。
* 管理者は、常に、インストラクターです。 したがって、`InstructorID`プロパティに外部キーとして含まれる、`Instructor`エンティティです。

ナビゲーション プロパティの名前が`Administrator`を保持するが、`Instructor`エンティティ。

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

上記のコードでは疑問符 (?) では、プロパティが null 許容型を指定します。

部門は、コースのナビゲーション プロパティがあるために、複数のコースにすることがあります。

```csharp
public ICollection<Course> Courses { get; set; }
```

注: 規則では、EF コアに、null 非許容 FKs および多対多リレーションシップの連鎖削除ができます。 連鎖削除は、循環 cascade delete ルールになります。 循環 cascade は、移行が追加されたときに、規則の例外を削除します。

たとえば場合、`Department.InstructorID`プロパティが null 許容型として定義されていません。

* EF コアは、部門が削除されたときに、インストラクターを削除する連鎖削除規則を構成します。
* 部門が削除されたときに、インストラクターを削除することが意図した動作ではないです。

ビジネス ルールが必要な場合、`InstructorID`プロパティが非 null 許容にする次 fluent API ステートメントを使用します。

 ```csharp
 modelBuilder.Entity<Department>()
    .HasOne(d => d.Administrator)
    .WithMany()
    .OnDelete(DeleteBehavior.Restrict)
 ```

上記のコードには、部門インストラクター リレーションシップで連鎖削除が無効にします。

## <a name="update-the-enrollment-entity"></a>登録のエンティティを更新します。

登録レコードは、1 つのコースを受講者によって実行されるです。

![登録エンティティ](complex-data-model/_static/enrollment-entity.png)

更新*Models/Enrollment.cs*を次のコード。

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a>外部キー プロパティとナビゲーション プロパティ

外部キー プロパティとナビゲーション プロパティは、次のリレーションシップを反映します。

登録レコードが 1 つのコースには、 `CourseID` FK プロパティおよび`Course`ナビゲーション プロパティ。

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

登録レコードが 1 つの受講者には、 `StudentID` FK プロパティおよび`Student`ナビゲーション プロパティ。

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a>多対多リレーションシップ

多対多の関係がある、`Student`と`Course`エンティティです。 `Enrollment`エンティティが、多対多の結合テーブルとして機能*ペイロードを持つ*データベースにします。 "ペイロード"を持つことを意味、`Enrollment`テーブルには FKs だけでなく、結合されたテーブルの追加のデータが含まれています (この場合、主キーと`Grade`)。

次の図は、エンティティの図でこれらのリレーションシップがどのようにを示します。 (この図は、EF の EF パワー ツールを使用して生成された 6.x です。 図の作成は、チュートリアルの一部です。)

![受講者コース多対多の関係](complex-data-model/_static/student-course.png)

各リレーションシップの線は、もう一方の端、一対多のリレーションシップを示す一方の端と、アスタリスク (*) で 1 がします。

場合、`Enrollment`テーブルに評価情報が含まれていなかった、2 つの FKs を含めることはのみ (`CourseID`と`StudentID`)。 ペイロードせず、多対多の結合テーブルは、純粋な結合テーブル (PJT) と呼ばれます。

`Instructor`と`Course`エンティティが純粋な結合テーブルを使用して多対多リレーションシップを設定します。

注: EF 6.x をサポートする多対多のリレーションシップが EF コアの暗黙の結合テーブルはありません。 詳細については、次を参照してください。[多対多リレーションシップ EF コア 2.0 で](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/)です。

## <a name="the-courseassignment-entity"></a>CourseAssignment エンティティ

![CourseAssignment エンティティ](complex-data-model/_static/courseassignment-entity.png)

作成*Models/CourseAssignment.cs*を次のコード。

[!code-csharp[Main](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a>講師-コース

![講師-コース m:M](complex-data-model/_static/courseassignment.png)

講師-コース多対多リレーションシップ:

* エンティティ セットで表される必要がある結合テーブルが必要です。
* 純粋な結合テーブル (テーブル ペイロードなし) です。

一般に結合エンティティの名前を付けます`EntityName1EntityName2`です。 たとえば、このパターンを使用して、インストラクター-コース結合テーブルは`CourseInstructor`します。 ただし、リレーションシップを説明する名前の使用をお勧めします。

データ モデルでは、簡単なを起動し、拡張します。 いいえペイロード結合 (PJTs) は、頻繁に進化ペイロードが含まれます。 使用して開始エンティティのわかりやすい名前、名前は、結合テーブルが変更されたときに変更する必要はありません。 理想的には、結合エンティティは、ビジネス ドメイン内、自身の自然な (場合によって 1 つの単語) 名があります。 たとえば、ブックと顧客は、評価と呼ばれる結合のエンティティにリンクでした。 講師-コース多対多リレーションシップの`CourseAssignment`が優先`CourseInstructor`です。

### <a name="composite-key"></a>複合キー

FKs は null を許容できません。 2 つの FKs `CourseAssignment` (`InstructorID`と`CourseID`) まとめての各行を一意に識別、`CourseAssignment`テーブル。 `CourseAssignment`専用 PK. を必要としません。 `InstructorID`と`CourseID`プロパティが複合 PK. として機能 EF コアにも、複合主キーを指定する唯一の方法は、 *fluent API*です。 次のセクションは、複合 PK を構成する方法を示しています。

複合キーことを確認します。

* 複数の行は、1 つのコースの許可されます。
* 複数の行は、1 つのインストラクターが許可されます。
* 同じインストラクターとコースに複数の行を指定することはできません。

`Enrollment`結合エンティティでは、この種の重複も有効であるため、独自の主キーを定義します。 このような重複を防ぐためには。

* 外部キーの各フィールドに一意のインデックスを追加または
* 構成`Enrollment`に複合主キーのような`CourseAssignment`します。 詳細については、次を参照してください。[インデックス](https://docs.microsoft.com/ef/core/modeling/indexes)です。

## <a name="update-the-db-context"></a>DB コンテキストを更新します。

次の強調表示されたコードを追加*Data/SchoolContext.cs*:

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

上記のコードは、新しいエンティティを追加し、構成、`CourseAssignment`エンティティの複合 PK.

## <a name="fluent-api-alternative-to-attributes"></a>属性に fluent API の代替手段

`OnModelCreating` 、上記のメソッドのコードでは、 *fluent API* EF の主な動作を構成します。 API は一連のメソッド呼び出しに 1 つのステートメントを組み合わせるで多くの場合、使用されているために、"fluent"と呼ばれます。 [次のコード](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration)fluent API の例に示します。

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

このチュートリアルでは、属性を持つことはできません DB の割り当てにのみ、fluent API を使用します。 ただし、fluent API では、ほとんどの書式、検証、および属性を持つを実行するマッピング規則を指定できます。

などのいくつかの属性`MinimumLength`fluent API では適用できません。 `MinimumLength`スキーマを変更しない、最小の長さの検証規則のみ適用されます。

"クリーンな状態です"そのエンティティ クラスを維持できるように排他的 fluent API の使用を好む開発者 属性および fluent API を混在させることができます。 構成できるのみ、fluent API で (複合主キーの指定) があります。 属性によってのみ実行できるいくつかの構成 (`MinimumLength`)。 Fluent API または属性を使用する方法を推奨します。

* これら 2 つの方法のいずれかを選択します。
* 選択した方法を可能な限り一貫して使用します。

こので使用される属性のいくつかのチュートリアルが使用されます。

* のみの検証 (たとえば、 `MinimumLength`)。
* EF のコア構成のみ (たとえば、 `HasKey`)。
* 検証と EF コア構成 (たとえば、 `[StringLength(50)]`)。

Fluent API と属性の詳細については、次を参照してください。[構成の方法](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration)です。

## <a name="entity-diagram-showing-relationships"></a>エンティティの図の関係を示す

次の図では、EF パワー ツールが、完成した School モデルを作成するダイアグラムを表示します。

![エンティティの図](complex-data-model/_static/diagram.png)

前の図を示しています。

* いくつかの一対多リレーションシップの線 (1 ~ \*)。
* 間での 0 または 1 を 1 つのリレーションシップの線 (1 対 0..1)、`Instructor`と`OfficeAssignment`エンティティです。
* 0-または-1-対多リレーションシップの線 (0..1 対 *) の間、`Instructor`と`Department`エンティティです。

## <a name="seed-the-db-with-test-data"></a>テスト データを使用して DB をシードします。

コードを更新*Data/DbInitializer.cs*:

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

上記のコードは、新しいエンティティのシードのデータを提供します。 このコードのほとんどは、新しいエンティティ オブジェクトを作成し、サンプル データを読み込みます。 サンプル データは、テストに使用されます。 上記のコードでは、次の多対多リレーションシップを作成します。

* `Enrollments`
* `CourseAssignment`

注: [EF コア 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap)をサポートする[データ シード](https://github.com/aspnet/EntityFrameworkCore/issues/629)です。

## <a name="add-a-migration"></a>移行を追加します。

プロジェクトをビルドします。 プロジェクト フォルダーに、コマンド ウィンドウを開き、次のコマンドを入力します。

```console
dotnet ef migrations add ComplexDataModel
```

上記のコマンドは、データ損失の可能性に関する警告を表示します。

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

場合、`database update`コマンドを実行する、次のエラーが発生します。

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

移行実行すると、既存のデータと外部キー制約が既存のデータで満たされていない可能性があります。 このチュートリアルでは、外部キー制約違反がないため、新しいデータベースが作成されます。 参照してください[従来のデータと外部キー制約を修正して](#fk)の現在のデータベースで外部キー違反を修正する方法についてはします。

## <a name="change-the-connection-string-and-update-the-db"></a>接続文字列を変更し、DB の更新

更新されたコード`DbInitializer`シード データの新しいエンティティを追加します。 強制的に新しい空のデータベースを作成する EF コア。

* DB の接続文字列名を変更*される appsettings.json* ContosoUniversity3 にします。 新しい名前は、コンピューターで使用されていない名前にする必要があります。

    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
      },
    ```

* または、DB を使用して、削除します。

    * **SQL Server オブジェクト エクスプ ローラー** (SSOX)。
    * `database drop` CLI コマンド。

   ```console
   dotnet ef database drop
   ```

実行`database update`コマンド ウィンドウで。

```console
dotnet ef database update
```

上記のコマンドは、すべての移行を実行します。

アプリを実行します。 アプリの実行を実行している、`DbInitializer.Initialize`メソッドです。 `DbInitializer.Initialize`新しいデータベースを設定します。

SSOX でデータベースを開きます。

* 展開して、**テーブル**ノード。 作成されたテーブルが表示されます。
* SSOX が以前に開かれた場合にクリックして、**更新**ボタンをクリックします。

![SSOX 内のテーブル](complex-data-model/_static/ssox-tables.png)

確認、 **CourseAssignment**テーブル。

* 右クリックし、 **CourseAssignment**テーブルを選択して**ビュー データ**です。
* 確認、 **CourseAssignment**テーブルにデータが含まれています。

![SSOX で CourseAssignment データ](complex-data-model/_static/ssox-ci-data.png)

<a name="fk"></a>

## <a name="fixing-foreign-key-constraints-with-legacy-data"></a>従来のデータと外部キー制約を修正します。

このセクションではオプションです。

移行実行すると、既存のデータと外部キー制約が既存のデータで満たされていない可能性があります。 運用データには、既存のデータを移行する手順を実行する必要があります。 このセクションでは、外部キー制約違反の修正の例を示します。 バックアップがない場合のこれらのコード変更を行わない。 前のセクションを完了し、データベースを更新する場合、これらのコード変更を行わない。

*{Timestamp}_ComplexDataModel.cs*ファイルには、次のコードが含まれています。

[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

上記のコードを追加、null 非許容`DepartmentID`に FK、`Course`テーブル。 前のチュートリアル DB には内の行が含まれています`Course`ので、移行によってそのテーブルを更新できません。

させる、`ComplexDataModel`と既存のデータの移行作業します。

* 新しい列を提供するコードを変更 (`DepartmentID`)、既定値です。
* 既定の部門として機能するには、"Temp"という名前の偽部門を作成します。

### <a name="fix-the-foreign-key-constraints"></a>外部キー制約を修正します。

更新プログラム、`ComplexDataModel`クラス`Up`メソッド。

* 開く、 *{timestamp}_ComplexDataModel.cs*ファイル。
* 追加するコードの行をコメント、`DepartmentID`列を`Course`テーブル。

[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

次の強調表示されたコードを追加します。 新しいコードに記述した後、`.CreateTable( name: "Department"`ブロック。[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

既存の前の変更と`Course`後に"Temp"部門に関連する行、 `ComplexDataModel` `Up`メソッドが実行されます。

運用アプリは。

* コードまたは追加するためのスクリプトを含める`Department`行および関連`Course`を新しい行`Department`行です。
* "Temp"部門やの既定値は使用しない`Course.DepartmentID `です。

次のチュートリアルでは、関連するデータについて説明します。

>[!div class="step-by-step"]
[前へ](xref:data/ef-rp/migrations)
[次へ](xref:data/ef-rp/read-related-data)
