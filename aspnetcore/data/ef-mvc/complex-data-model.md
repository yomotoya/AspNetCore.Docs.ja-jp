---
title: "ASP.NET MVC を持つコアを EF Core - データ モデルの 10 の 5"
author: tdykstra
description: "このチュートリアルでは、複数のエンティティとリレーションシップを追加し、書式設定、検証、およびデータベース マッピング規則を指定することによって、データ モデルをカスタマイズします。"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/complex-data-model
ms.openlocfilehash: d844e2a69e4bbfdf3942f2666ead0047bdf83b7a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
# <a name="creating-a-complex-data-model---ef-core-with-aspnet-core-mvc-tutorial-5-of-10"></a>EF コア ASP.NET Core MVC のチュートリアル (10 の 5) に、複雑なデータ モデルを作成します。

によって[Tom Dykstra](https://github.com/tdykstra)と[Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso 大学でサンプル web アプリケーションでは、Entity Framework のコアと Visual Studio を使用して ASP.NET Core MVC web アプリケーションを作成する方法を示します。 一連のチュートリアルについては、次を参照してください。[系列内の最初のチュートリアル](intro.md)です。

前のチュートリアルでは、3 つのエンティティで構成されている単純なデータ モデルを使用していた。 このチュートリアルでは複数のエンティティとリレーションシップを追加し、書式設定、検証、およびデータベース マッピング規則を指定することによって、データ モデルをカスタマイズします。

完了したら、エンティティ クラスは、次の図に示す完成したデータ モデルを構成します。

![エンティティの図](complex-data-model/_static/diagram.png)

## <a name="customize-the-data-model-by-using-attributes"></a>属性を使用して、データ モデルをカスタマイズします。

このセクションでは、書式設定、検証、およびデータベース マッピング規則を指定する属性を使用して、データ モデルをカスタマイズする方法が表示されます。 作成する次のセクションでのいくつか追加することによって完全な学校のデータ モデル属性をクラスは、既に作成して、モデル内の残りのエンティティ型の新しいクラスを作成します。

### <a name="the-datatype-attribute"></a>データ型の属性

学生の登録日のすべての web ページ現在表示、日付と時刻が、このフィールドの関心のあるは、日付。 データの注釈属性を使用することができますいずれかのコード変更データを表示するすべてのビューの表示形式を修正します。 属性を追加することを実行する方法の例を参照する、`EnrollmentDate`プロパティに、`Student`クラスです。

*Models/Student.cs*、追加、`using`のステートメント、`System.ComponentModel.DataAnnotations`名前空間を追加および`DataType`と`DisplayFormat`属性を`EnrollmentDate`プロパティ、次の例で示すように。

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

`DataType`属性を使用してデータベースの組み込み型よりも特定のデータ型を指定します。 ここでのみが必要を追跡する、日付、日付と時刻がありません。 `DataType`日付、時刻、PhoneNumber、通貨、EmailAddress などの多くのデータ型の列挙体を提供します。 また、`DataType` 属性を使用して、アプリケーションで型固有の機能を自動的に提供することもできます。 たとえば、`mailto:` リンクを `DataType.EmailAddress` に作成したり、HTML5 をサポートするブラウザーで `DataType.Date` に日付セレクターを提供したりできます。 `DataType`属性は、HTML 5 を出力`data-`HTML 5 ブラウザーを理解することができます (データと読みます dash) の属性です。 `DataType`属性は、いずれかの検証を提供しません。

`DataType.Date`表示される日付の形式で指定されていません。 既定では、サーバーの CultureInfo に基づく既定の形式に従ってデータ フィールドが表示されます。

`DisplayFormat` 属性は、日付の書式を明示的に指定するために使用されます。

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

`ApplyFormatInEditMode` の設定では、編集用にテキスト ボックスに値を表示するときにも適用する必要がある書式設定を指定します  (たくない一部のフィールドなどの通貨値の可能性がありますしないこと、テキスト ボックス内の通貨記号を編集するためです。)

使用することができます、`DisplayFormat`自体が、属性が使用することをお勧めでは一般に、`DataType`も属性します。 `DataType`属性を画面に表示する方法ではなく、データのセマンティクスを伝達し、次の利点を得られないことを示します`DisplayFormat`:

* ブラウザーが HTML5 機能を有効にすることができます (たとえば、予定表コントロールでは、ロケールに応じた通貨記号、電子メールへのリンクを表示する、いくつかのクライアント側入力検証などです。)。

* ブラウザーの既定では、ロケールに基づいて正しい書式を使ってデータがレンダリングされます。

詳細については、次を参照してください。、 [\<入力 > タグ ヘルパーのドキュメント](../../mvc/views/working-with-forms.md#the-input-tag-helper)です。

アプリを実行する、受講者インデックス ページに移動し、登録の日付の時刻が表示されていないことに注意してください。 学生のモデルを使用する任意のビューの場合は true。 同じになります。

![受講者時刻なしの日付が表示されたページをインデックスします。](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a>StringLength 属性

データ検証ルールおよび属性を使用して検証エラー メッセージを指定することもできます。 `StringLength`属性は、データベースの最大長を設定し、クライアント側とサーバー側の提供 ASP.NET MVC の検証。 この属性には、文字列の長さを指定することも、最小値データベース スキーマに影響がありません。

ユーザーが名前の 50 個を超える文字を入力しないことを確認するとします。 この制限を追加する追加`StringLength`属性を`LastName`と`FirstMidName`プロパティ、次の例で示すようにします。

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

`StringLength`属性は、名前に空白文字を入力するからユーザーを妨げることはありません。 使用することができます、`RegularExpression`入力に制限を適用する属性。 たとえば、次のコードには、最初の文字が大文字で指定し、残りの文字は英文字でが必要です。

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

`MaxLength`属性と同様の機能を提供する、`StringLength`属性が、クライアント側では提供しません検証します。

データベース モデルがデータベース スキーマの変更を必要とするように変更されました。 追加したデータベースにアプリケーション UI を使用してデータを失うことがなく、スキーマを更新するのにには移行を使用します。

変更を保存し、プロジェクトをビルドします。 プロジェクト フォルダー内のコマンド ウィンドウを開くし、次のコマンドを入力します。

```console
dotnet ef migrations add MaxLengthOnNames
```

```console
dotnet ef database update
```

`migrations add`コマンドをデータ損失が発生する可能性があります、変更は、最大の長さの 2 つの列より短いために警告します。  移行という名前のファイルを作成する*\<タイムスタンプ > _MaxLengthOnNames.cs*です。 このファイルには内のコードが含まれています、`Up`メソッドを現在のデータ モデルと一致するデータベースを更新します。 `database update`コマンドは、そのコードを実行します。

移行ファイル名にプレフィックスのタイムスタンプは、Entity Framework によって、移行を並べ替えに使用されます。 データベースの更新コマンドを実行する前に複数の移行を作成することができ、移行のすべてが作成された順序で適用されます、します。

アプリを実行する、選択、**受講者** タブで、をクリックして**新規作成**、50 文字以下のいずれかの名前を入力します。 クリックすると、**作成**クライアント側の検証エラー メッセージが表示されます。

![学生のインデックス文字列の長さエラーを示すページ](complex-data-model/_static/string-length-errors.png)

### <a name="the-column-attribute"></a>列の属性

クラスとプロパティをデータベースにマップする方法を制御するのに属性を使用することもできます。 名前を使用するいたと仮定します`FirstMidName`最初の-名前のフィールドのフィールドには、ミドル ネームが含まれる場合もあるためです。 選択した名前を指定するデータベース列が、`FirstName`データベースに対してアドホック クエリを記述するユーザーがその名前に慣れているため、します。 このマッピングを作成するには、使用することができます、`Column`属性。

`Column`属性を指定するデータベースが作成されるとき、列の`Student`をマップするテーブル、`FirstMidName`プロパティの名前は`FirstName`します。 つまり、ときに、コードを指す`Student.FirstMidName`、データから得られますまたはで更新される、`FirstName`の列、`Student`テーブル。 列名を指定しない場合、プロパティ名と同じ名前で与えられます。

*Student.cs*ファイルに追加し、`using`の声明`System.ComponentModel.DataAnnotations.Schema`に列名の属性を追加し、`FirstMidName`プロパティ、次の強調表示されたコードに示すように。

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

追加、`Column`属性がサポートするモデルを変更、 `SchoolContext`、ので、データベースと一致しません。

変更を保存し、プロジェクトをビルドします。 プロジェクト フォルダー内のコマンド ウィンドウを開くし、別の移行を作成する次のコマンドを入力します。

```console
dotnet ef migrations add ColumnFirstName
```

```console
dotnet ef database update
```

**SQL Server オブジェクト エクスプ ローラー**をダブルクリックして、Student テーブル デザイナーを開き、**学生**テーブル。

![移行した後で SSOX students テーブル](complex-data-model/_static/ssox-after-migration.png)

最初の 2 つの移行を適用する前に、名前の列は、nvarchar (max) 型のでした。 これらは、これで、nvarchar (50) と列の名前が変更された FirstMidName から firstname です。

> [!Note]
> 次のセクションでのすべてのエンティティ クラスの作成を完了する前にコンパイルしようとすると、コンパイラ エラーが発生する可能性があります。

## <a name="final-changes-to-the-student-entity"></a>学生エンティティの最終変更

![学生エンティティ](complex-data-model/_static/student-entity.png)

*Models/Student.cs*、次のコードで以前に追加したコードを置き換えます。 変更が強調表示されます。

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a>必須の属性

`Required`属性は、名前のプロパティの必須フィールドです。 `Required`値の型などの非 null 許容の型の属性は必要ありません (DateTime、int、double、float などです。)。 Null にすることはできません型は、必要なフィールドとして自動的に扱われます。

削除することで、`Required`属性し、の最小の長さのパラメーターに置き換えて、`StringLength`属性。

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a>表示属性

`Display`属性、テキスト ボックスのキャプションにするように指定「姓」、"Last Name"、「完全名」、およびプロパティ名の代わりに「登録日」(を単語に分割する領域を持つなし) 各インスタンスにします。

### <a name="the-fullname-calculated-property"></a>計算 FullName プロパティ

`FullName`その他の 2 つのプロパティを連結することによって作成される値を返す計算されるプロパティです。 存在し、get アクセサーだけ、したがって`FullName`データベース内の列が生成されます。

## <a name="create-the-instructor-entity"></a>Instructor エンティティを作成します。

![Instructor エンティティ](complex-data-model/_static/instructor-entity.png)

作成*Models/Instructor.cs*、テンプレート コードを次のコードに置き換えます。

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

いくつかのプロパティは、Student とインストラクター エンティティで同じことに注意してください。 [実装の継承](inheritance.md)このシリーズの後でチュートリアルでは、冗長性を回避するのには、このコードをリファクターします。

作成することも、1 行に複数の属性を配置することができます、`HireDate`次のように属性します。

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a>CourseAssignments と OfficeAssignment ナビゲーションのプロパティ

`CourseAssignments`と`OfficeAssignment`プロパティは、ナビゲーション プロパティ。

インストラクターは任意の数のコースを教えることができますので、`CourseAssignments`コレクションとして定義されます。

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

ナビゲーション プロパティは、複数のエンティティを保持できる、その型が一覧にエントリを追加、削除すると、更新できるにあります。  指定できます`ICollection<T>`またはなど型`List<T>`または`HashSet<T>`です。 指定した場合`ICollection<T>`、EF の作成、`HashSet<T>`既定のコレクション。

これらは、なぜ理由`CourseAssignment`エンティティが、多対多リレーションシップに関するセクションで後述します。

Contoso 大学のビジネス ルールの状態、インストラクターが最大で 1 つのオフィスを持つことができますのみため、`OfficeAssignment`プロパティは 1 つの OfficeAssignment エンティティ (を office が割り当てられていない場合は null にすることがあります) を保持します。

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a>OfficeAssignment エンティティを作成します。

![OfficeAssignment エンティティ](complex-data-model/_static/officeassignment-entity.png)

作成*Models/OfficeAssignment.cs*を次のコード。

[!code-csharp[Main](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a>キー属性

リレーションシップがある 0 または 1 を 1 つ、インストラクターと OfficeAssignment エンティティの間です。 オフィス割り当てのみに、割り当てられているインストラクターに関連してあり、したがって、主キーも Instructor エンティティへの外部のキー。 Entity Framework は、自動的に、その名前が ID または classnameID 名前付け規則に従っていないためこのエンティティの主キーとして InstructorID を認識できません。 したがって、`Key`属性は、キーとして識別するために使用します。

```csharp
[Key]
public int InstructorID { get; set; }
```

使用することも、`Key`エンティティが、独自のプライマリ キーを持っていて classnameID または ID 以外何か、プロパティの名前を指定の属性

既定では、リレーションシップの列があるために、EF はデータベースによって生成された非としてキーを扱います。

### <a name="the-instructor-navigation-property"></a>講師ナビゲーション プロパティ

Instructor エンティティに null 許容`OfficeAssignment`ナビゲーション プロパティ (インストラクター オフィス割り当てがあるない) ため、OfficeAssignment エンティティが、null 非許容と`Instructor`ナビゲーション プロパティ (ため、オフィス割り当てできません講師--がないと存在`InstructorID`が null 非許容の)。 Instructor エンティティに関連する OfficeAssignment エンティティがある場合、各エンティティは、ナビゲーション プロパティで、もう 1 つへの参照があります。

配置すること、 `[Required]` 、インストラクター ナビゲーション プロパティに関連のインストラクターである必要がありますがこれを行うためがないことを指定する属性、 `InstructorID` (これは、このテーブルへのキーではも) 外部キーが null 非許容されます。

## <a name="modify-the-course-entity"></a>Course エンティティを変更します。

![Course エンティティ](complex-data-model/_static/course-entity.png)

*Models/Course.cs*、次のコードで以前に追加したコードを置き換えます。 変更が強調表示されます。

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

Course エンティティは、外部キーのプロパティを持つ`DepartmentID`が、関連する部門エンティティとそれを`Department`ナビゲーション プロパティ。

Entity Framework は、関連エンティティのナビゲーション プロパティがある場合、データ モデルに外部キーのプロパティを追加する必要ありません。  EF は自動的にしている必要な場所がデータベースに外部キーを作成し、作成[プロパティをシャドウ](https://docs.microsoft.com/ef/core/modeling/shadow-properties)にします。 簡素化されより効率的な更新プログラムを必ずデータ モデルの外部キーを持つことができます。 たとえば、コースを編集するエンティティをフェッチするときに、Department エンティティが null、読み込まない場合ので、course エンティティを更新するときに必要があります、Department エンティティを最初に取得します。 ときに外部キー プロパティ`DepartmentID`が含まれるデータ モデルで更新する前に、Department エンティティをフェッチする必要はありません。

### <a name="the-databasegenerated-attribute"></a>DatabaseGenerated 属性

`DatabaseGenerated`属性が、`None`上のパラメーター、`CourseID`プロパティは、主キーの値は、ユーザーによって提供されるではなくデータベースによって生成されたを指定します。

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

既定では、Entity Framework は、主キーの値がデータベースによって生成されると仮定します。 ほとんどのシナリオに必要な要素です。 ただし、コースのエンティティを 1 つの部門別の部門の 2000年シリーズの一連の 1000 ようコースのユーザーが指定した番号を使用し、します。

`DatabaseGenerated`属性を使用して日付を記録するために使用するデータベース列の場合、行の作成または更新されるように、既定値を生成することもできます。  詳細については、次を参照してください。[生成プロパティ](https://docs.microsoft.com/ef/core/modeling/generated-properties)です。

### <a name="foreign-key-and-navigation-properties"></a>外部キー プロパティとナビゲーション プロパティ

外部キーのプロパティと、Course エンティティでのナビゲーション プロパティは、次のリレーションシップを反映します。

コースを 1 つの部門に割り当てられるがあるため、`DepartmentID`外部キーと`Department`上記の理由によりナビゲーション プロパティ。

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

コースに受講者に、登録済みの任意の数を持つことができますので、`Enrollments`ナビゲーション プロパティがコレクション。

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

コースに複数のインストラクターを教育する可能性がありますので、`CourseAssignments`ナビゲーション プロパティがコレクション (型`CourseAssignment`については説明[後](#many-to-many-relationships))。

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

## <a name="create-the-department-entity"></a>部門 エンティティを作成します。

![部門 エンティティ](complex-data-model/_static/department-entity.png)


作成*Models/Department.cs*を次のコード。

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a>列の属性

以前を使用して、`Column`列名のマッピングを変更する属性。 部門 エンティティのコードで、 `Column` SQL データ型マッピングの列を定義するときは、データベースで SQL Server の money 型の使用ができるように変更する属性が使用されています。

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

列マッピングは一般的に必要ですが、Entity Framework のプロパティを定義する CLR 型に基づく適切な SQL Server データ型の選択されるためです。 CLR `decimal` SQL Server へのマップ」と入力`decimal`型です。 ここでは、列に通貨の値が保持していると、money データ型は方が適していることを知る。

### <a name="foreign-key-and-navigation-properties"></a>外部キー プロパティとナビゲーション プロパティ

外部キー プロパティとナビゲーション プロパティは、次のリレーションシップを反映します。

部門、管理者がない可能性があり、管理者は、常に、インストラクター。 したがって、`InstructorID`プロパティが Instructor エンティティへの外部キーとして含まれると、後に疑問符 () が追加、`int`タイプの null 値許容とプロパティを指定します。 ナビゲーション プロパティの名前が`Administrator`Instructor エンティティを保持しているが、します。

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

部門は、コースのナビゲーション プロパティがあるために、複数のコースにすることがあります。

```csharp
public ICollection<Course> Courses { get; set; }
```

> [!NOTE]
> 慣例により、Entity Framework null 非許容の外部キーおよび多対多リレーションシップの連鎖削除を有効にします。 これは、結果、循環 cascade delete ルールは、移行を追加しようとすると、例外が発生します。 たとえば、Department.InstructorID プロパティは、null 値許容と定義されていないのに、EF よう、部門はこれが発生するを削除すると、インストラクターを削除する連鎖削除規則に構成します。 ビジネス ルールが必要な場合、`InstructorID`プロパティを null 非許容、ステートメントを使用して、次 fluent API をリレーションシップで連鎖削除を無効にする必要があります。
> ```csharp
> modelBuilder.Entity<Department>()
>    .HasOne(d => d.Administrator)
>    .WithMany()
>    .OnDelete(DeleteBehavior.Restrict)
> ```

## <a name="modify-the-enrollment-entity"></a>登録のエンティティを変更します。

![登録エンティティ](complex-data-model/_static/enrollment-entity.png)

*Models/Enrollment.cs*、次のコードで以前に追加したコードを置き換えます。

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a>外部キー プロパティとナビゲーション プロパティ

外部キーのプロパティとナビゲーション プロパティは、次のリレーションシップを反映します。

登録レコードが、1 つのコースがあるため、`CourseID`外部キーのプロパティと`Course`ナビゲーション プロパティ。

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

登録レコードが 1 つの受講者には、`StudentID`外部キーのプロパティと`Student`ナビゲーション プロパティ。

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a>多対多リレーションシップ

Student とコースのエンティティ間に多対多のリレーションシップがあるし、登録エンティティが、多対多の結合テーブルとして機能*ペイロードを持つ*データベースにします。 "ペイロード"で、Enrollment テーブルに (この場合は、主キーおよびグレード プロパティ) 内の結合テーブルの外部キーだけでなく、追加のデータが含まれているを意味します。

次の図は、エンティティの図でこれらのリレーションシップがどのようにを示します。 (この図は、EF の Entity Framework のパワー ツールを使用して生成された 6.x 以外の場合は、チュートリアルの一部ではない、ダイアグラムを作成、使用されているだけを例としては、ここです)。

![受講者コース多対多の関係](complex-data-model/_static/student-course.png)

各リレーションシップの線は、もう一方の端、一対多のリレーションシップを示す一方の端と、アスタリスク (*) で 1 がします。

場合は、Enrollment テーブルには、評価情報が含まれていなかった、CourseID と学生 Id の 2 つの外部キーを保持することはのみです。 その場合は、なりますペイロードせず、多対多の結合テーブル (または純粋な結合テーブル)、データベースにします。 インストラクターとコース エンティティはそのような多対多リレーションシップを持ち、ペイロードしないテーブルの結合として機能するエンティティ クラスを作成するのには、次の手順。

(EF 6.x サポートしていますが、多対多のリレーションシップ、EF コアの暗黙の結合テーブルがありません。 詳細については、次を参照してください、 [EF コア GitHub リポジトリにディスカッション](https://github.com/aspnet/EntityFramework/issues/1368)。)。 

## <a name="the-courseassignment-entity"></a>CourseAssignment エンティティ

![CourseAssignment エンティティ](complex-data-model/_static/courseassignment-entity.png)

作成*Models/CourseAssignment.cs*を次のコード。

[!code-csharp[Main](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="join-entity-names"></a>エンティティ名を参加させる

講師-コース多対多リレーションシップのデータベースに必要な結合テーブルを保持しているエンティティ セットで表されます。 結合のエンティティの名前を が一般的`EntityName1EntityName2`、ここではある`CourseInstructor`です。 ただし、リレーションシップを説明する名前を選択することをお勧めします。 データ モデルでは、簡単な起動し、拡大、いいえペイロード結合を頻繁にペイロードを後で取得します。 エンティティのわかりやすい名前で開始する場合は、後で、名前を変更する必要はありません。 理想的には、結合エンティティは、ビジネス ドメイン内、自身の自然な (場合によって 1 つの単語) 名があります。 たとえば、ブックと顧客は、評価を通じてリンクでした。 このリレーションシップの`CourseAssignment`よりの方が適しています`CourseInstructor`です。

### <a name="composite-key"></a>複合キー

外部キーがない null 許容、まとめて一意にテーブルの各行を識別する、独立したプライマリ キーの必要はありません。 *InstructorID*と*CourseID*プロパティは複合主キーとして機能する必要があります。 EF に複合主キーを識別する唯一の方法を使用して、 *fluent API* (属性を使用して、そのことはできません)。 次のセクションで、複合主キーを構成する方法が表示されます。

複合キーにより、複数の行、1 つのコースおよび講師が 1 つに対して複数の行を保持できますが、することは いる同じインストラクターとコースに複数の行。 `Enrollment`結合エンティティは、この種の重複も有効であるために自身のプライマリ キーを定義します。 このような重複を防ぐためには、外部のキー フィールドで一意のインデックスを追加または構成することが`Enrollment`に複合主キーのような`CourseAssignment`します。 詳細については、次を参照してください。[インデックス](https://docs.microsoft.com/ef/core/modeling/indexes)です。

## <a name="update-the-database-context"></a>データベース コンテキストを更新します。

次の強調表示されたコードを追加、 *Data/SchoolContext.cs*ファイル。

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

このコードは、新しいエンティティを追加し、CourseAssignment エンティティの複合主キーを構成します。

## <a name="fluent-api-alternative-to-attributes"></a>属性に fluent API の代替手段

内のコード、`OnModelCreating`のメソッド、`DbContext`クラスの使用、 *fluent API* EF の動作を構成します。 API が呼び出されます"fluent"からこの例のように、1 つのステートメントには一連のメソッド呼び出しをまとめてを組み合わせるで多くの場合、使用されているため、 [EF の主要なマニュアル](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

このチュートリアルで使用している fluent API のみデータベース マッピング属性を使用して行うことはできません。 ただし、ほとんどの書式、検証、および属性を使用して行うことができますマッピング規則を指定するのに fluent API を使用することもできます。 などのいくつかの属性`MinimumLength`fluent API では適用できません。 前に述べたよう`MinimumLength`スキーマを変更しないクライアントとサーバー側の検証規則のみ適用されます。

"クリーンな状態です"そのエンティティ クラスを維持できるように排他的 fluent API の使用を好む開発者 をしたいが存在している fluent API を使用してのみ実行できるいくつかのカスタマイズ属性および fluent API が混在することができますが、一般に推奨これら 2 つの方法のいずれかを選択し、使用可能な一貫している限りです。 両方を使用して行う場合は、競合がある場所に Fluent API が属性をオーバーライドする注意してください。

Fluent API と属性の詳細については、次を参照してください。[構成の方法](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration)です。

## <a name="entity-diagram-showing-relationships"></a>エンティティの図の関係を示す

次の図は、Entity Framework のパワー ツールが、完成した School モデルを作成するダイアグラムを示します。

![エンティティの図](complex-data-model/_static/diagram.png)

一対多リレーションシップの線以外 (1 ~ \*)、インストラクターと OfficeAssignment エンティティと 0-または-1-対多リレーションシップの線の間の 0 または 1 を 1 つのリレーションシップの線 (1 対 0..1) をここで確認できます (0..1 対 *) の間で、インストラクターと Department エンティティ。

## <a name="seed-the-database-with-test-data"></a>データベースにテスト データをシードします。

コードで置き換え、 *Data/DbInitializer.cs*を作成した新しいエンティティのシードにデータを提供するために次のコード ファイル。

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

最初のチュートリアルで説明したとおり、このコードのほとんどは単に新しいエンティティ オブジェクトを作成し、プロパティをテストするために必要に応じてにサンプル データを読み込みます。 多対多のリレーションシップの処理方法に注意してください: コード内のエンティティを作成することでリレーションシップを作成する、`Enrollments`と`CourseAssignment`エンティティ セットに参加します。

## <a name="add-a-migration"></a>移行を追加します。

変更を保存し、プロジェクトをビルドします。 プロジェクト フォルダー内のコマンド ウィンドウを開き、入力、`migrations add`コマンド (しない update database コマンドが、まだ)。

```console
dotnet ef migrations add ComplexDataModel
```

データ損失の可能性に関する警告が表示されます。

```text
An operation was scaffolded that may result in the loss of data. Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

実行しようとする場合、`database update`コマンドの時点で (これを行わないまだ) 次のエラーが発生します。

> "FK_dbo FOREIGN KEY 制約と競合して ALTER TABLE ステートメント。Course_dbo です。Department_DepartmentID"です。 競合が発生したデータベース"ContosoUniversity"テーブル"dbo します。部門"、列 'DepartmentID' です。

既存のデータと移行を実行するときに、外部キー制約を満たすためにデータベースにスタブ データを挿入する必要があります。 生成されたコードに、`Up`メソッドは、Course テーブルに null 非許容 DepartmentID 外部キーを追加します。 既にある場合の行 Course テーブルに、コードを実行すると、 `AddColumn` SQL Server が null にすることはできません、列内の値を認識していないために、操作が失敗します。 このチュートリアルでは、新しいデータベースでして移行を実行しますが、実稼働アプリケーションでは、既存のデータを処理する次の手順を実行する方法の例を表示するため、移行を行う必要があります。

この移行は、新しい列に、既定値を提供するコードを変更する必要がある既存のデータを操作および明細部分を作成するには、既定の部門として機能するには、"Temp"という名前です。 その結果、既存のコース行はすべてに関連する後に"Temp"部門、`Up`メソッドが実行されます。

* 開く、 *{timestamp}_ComplexDataModel.cs*ファイル。 

* Course テーブルに、[DepartmentID] 列を追加するコードの行をコメントにします。

  [!code-csharp[Main](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

* Department テーブルを作成するコードの後に、次の強調表示されたコードを追加します。

  [!code-csharp[Main](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

実稼働アプリケーションでは、コードまたは部門の行を追加して、コースの行を新しい部門行に関連するスクリプトを記述するとします。 不要になった"Temp"部門または Course.DepartmentID 列での既定値です。

変更を保存し、プロジェクトをビルドします。

## <a name="change-the-connection-string-and-update-the-database"></a>接続文字列を変更し、データベースを更新

今すぐに新しいコードがある、`DbInitializer`シード エンティティのデータを新しいを空のデータベースに追加するクラスです。 EF 新しい空のデータベースを作成するために、内の接続文字列でデータベースの名前を変更する*される appsettings.json* ContosoUniversity3 を使用しているコンピューターで使用していない別の名前。

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
  },
```

変更を保存*される appsettings.json*です。

> [!NOTE]
> データベース名を変更する代わりに、データベースを削除することができます。 使用して**SQL Server オブジェクト エクスプ ローラー** (SSOX) または`database drop`CLI コマンド。
> ```console
> dotnet ef database drop
> ```

データベース名を変更または、データベースを削除した後、実行、`database update`コマンドをコマンド ウィンドウで、移行を実行します。

```console
dotnet ef database update
```

アプリケーションを実行、`DbInitializer.Initialize`メソッドを実行し、新しいデータベースを設定します。

以前に行ったよう、SSOX でデータベースを開き、展開、**テーブル**ノードをすべてのテーブルが作成されたことを確認します。 (以前の状態から開く SSOX まだの場合にをクリックして、**更新**ボタンをクリックします)。

![SSOX 内のテーブル](complex-data-model/_static/ssox-tables.png)

データベースのシードを設定する初期化子のコードをトリガーするアプリを実行します。

右クリックし、 **CourseAssignment**テーブルを選択して**ビュー データ**にデータが入っていることを確認します。

![SSOX で CourseAssignment データ](complex-data-model/_static/ssox-ci-data.png)

## <a name="summary"></a>まとめ

複雑なデータ モデルと対応するデータベースがあるようになりました。 次のチュートリアルでは、学びます関連のデータにアクセスする方法の詳細。

>[!div class="step-by-step"]
[前へ](migrations.md)
[次へ](read-related-data.md)  
