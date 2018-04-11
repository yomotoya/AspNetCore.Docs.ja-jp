---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
title: ASP.NET MVC アプリケーションのより複雑なデータ モデルの作成 |Microsoft ドキュメント
author: tdykstra
description: Contoso 大学でサンプル web アプリケーションでは、Entity Framework 6 の Code First と Visual Studio を使用して ASP.NET MVC 5 アプリケーションを作成する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 46f7f3c9-274f-4649-811d-92222a9b27e2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: fd8bf6502b0dd261505a86a2ed86d4c3f42e8755
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-more-complex-data-model-for-an-aspnet-mvc-application"></a>ASP.NET MVC アプリケーションのより複雑なデータ モデルの作成
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[完成したプロジェクトをダウンロード](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)または[PDF のダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso 大学でサンプル web アプリケーションでは、Entity Framework 6 の Code First と Visual Studio 2013 を使用して ASP.NET MVC 5 アプリケーションを作成する方法を示します。 チュートリアル シリーズについては、[シリーズの最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)をご覧ください。


前のチュートリアルでは、3 つのエンティティで構成されている単純なデータ モデルで機能します。 このチュートリアルでは、複数のエンティティとリレーションシップを追加し、書式設定、検証、およびデータベース マッピング規則を指定することによって、データ モデルをカスタマイズします。 データ モデルをカスタマイズする 2 つの方法が表示されます: とデータベースのコンテキスト クラスにコードを追加することでエンティティ クラスには、属性を追加しています。

完了すると、エンティティ クラスは、以下の図のように完成したデータ モデルを構成します。

![School_class_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

## <a name="customize-the-data-model-by-using-attributes"></a>属性を使用してデータ モデルをカスタマイズする

このセクションでは、書式設定、検証、データベース マッピング規則を指定する属性を使用して、データ モデルをカスタマイズする方法を示します。 次のセクションでは、のいくつかありますを作成してから、完全な`School`追加することでデータ モデルの属性クラスに既にモデル内の残りのエンティティ型の新しいクラスを作成し、作成します。

### <a name="the-datatype-attribute"></a>データ型の属性

学生の登録日について、すべての Web ページでは現在、日付と共に時刻が表示されていますが、このフィールドでは日付が重要になります。 データ注釈属性を使用すれば、1 つのコードを変更するだけで、データを表示するすべてのビューの表示形式を修正できます。 その方法例を表示するには、`Student` クラスの `EnrollmentDate` プロパティに属性を追加します。

*Models\Student.cs*、追加、`using`のステートメント、`System.ComponentModel.DataAnnotations`名前空間を追加および`DataType`と`DisplayFormat`属性を`EnrollmentDate`プロパティ、次の例で示すように。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,12-13)]

[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性を使用してデータベースの組み込み型よりも特定のデータ型を指定します。 この例では、追跡する必要があるのは、日付と時刻ではなく、日付のみです。 [DataType 列挙](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)などの多くのデータ型の提供*日付、時刻、PhoneNumber、通貨、EmailAddress*などです。 また、`DataType` 属性を使用して、アプリケーションで型固有の機能を自動的に提供することもできます。 たとえば、`mailto:`に対してリンクを作成できる[DataType.EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)、日付選択を指定することができます、 [DataType.Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)をサポートするブラウザーで[HTML5](http://html5.org/). [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性は、HTML 5 を出力[データ -](http://ejohn.org/blog/html-5-data-attributes/) (発音*データ ダッシュ*) HTML 5 ブラウザーで認識できる属性です。 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性は、いずれかの検証を渡さないようにします。

`DataType.Date` は、表示される日付の書式を指定しません。 既定では、データ フィールドを基に、サーバーの既定の形式に従って表示[CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx)です。

`DisplayFormat` 属性は、日付の書式を明示的に指定するために使用されます。


[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample2.cs)]


`ApplyFormatInEditMode`設定では、指定した書式設定も適用されることを編集するためのテキスト ボックスが表示されたら、値を指定します。 (したくないをいくつかのフィールドの — たとえば、通貨の値のたくない、テキスト ボックス内の通貨記号を編集するためです)。

使用することができます、 [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)自体が、属性が使用することをお勧めでは一般に、 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)も属性します。 `DataType`属性を伝達、*セマンティクス*データとして、画面に表示する方法ではなくおよびのでは得られない、次の利点を提供`DisplayFormat`:

- ブラウザーは HTML5 機能を有効にすることができます (たとえば、カレンダー コントロール、ロケールに適した通貨記号、メール リンク、一部のクライアント側の入力検証を表示するときなど)。
- 既定では、ブラウザーがに基づいて、正しい形式を使用してデータを表示、[ロケール](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx)です。
- [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性には、データを表示するために右フィールド テンプレートを選択する MVC が有効にすることができます (、 [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)文字列テンプレートを使用)。 詳細については、Brad Wilson を参照してください。 [ASP.NET MVC 2 テンプレート](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html)です。 (MVC 2 用に記述されたがこの記事の内容も適用 ASP.NET MVC の現在のバージョンに)。

使用する場合、`DataType`属性指定する必要が、日付フィールドに関連付け、 `DisplayFormat` Chrome ブラウザーで、フィールドが正常に表示されることを確認するためにも属性。 詳細については、次を参照してください。[この StackOverflow スレッド](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie)です。

MVC での他の日付形式を処理する方法の詳細についてを参照してください[MVC 5 の概要: メソッドの編集とビューの編集を調べて](../introduction/examining-the-edit-methods-and-edit-view.md)と検索のページに&quot;国際化&quot;です。

学生のインデックス ページを再度実行し、登録の日付の時刻が表示されていないことに注意してください。 いずれかのビューを使用する場合は true。 同じになります、`Student`モデル。

![Students_index_page_with_formatted_date](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image2.png)

### <a name="the-stringlengthattribute"></a>StringLengthAttribute

属性を使用して、データ検証規則と検証エラー メッセージを指定することもできます。 [StringLength 属性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)データベースの最大長を設定し、クライアント側とサーバー側は、ASP.NET MVC の検証。 この属性で最小長を指定することもできますが、最小値はデータベース スキーマに影響しません。

たとえば、ユーザーが 50 文字を超える名前を入力しないようにする必要があるとします。 この制限を追加する追加[StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)属性を`LastName`と`FirstMidName`プロパティ、次の例で示すようにします。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=10,12)]

[StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)属性は、名前に空白文字を入力するからユーザーを妨げることはありません。 使用することができます、[正規表現](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx)入力に制限を適用する属性。 たとえば、次のコードには、最初の文字が大文字で指定し、残りの文字は英文字でが必要です。

`[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]`

[MaxLength](https://msdn.microsoft.com/library/System.ComponentModel.DataAnnotations.MaxLengthAttribute.aspx)属性と同様の機能を提供する、 [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)属性が、クライアント側では提供しません検証します。

アプリケーションを実行し、をクリックして、**受講者**タブです。次のエラーが発生します。

*データベースが作成されたために、'SchoolContext' コンテキストのバックアップ モデルが変更されました。Code First Migrations を使用して、データベースを更新するを検討してください ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269))。*

データベース スキーマの変更を必要とするように、データベース モデルが変更され、Entity Framework では、ことが検出されました。 UI を使用して、データベースに追加したすべてのデータを失うことがなく、スキーマを更新するのにには移行を使用します。 によって作成されたデータを変更した場合、`Seed`メソッドは、その元の状態のために変更される、 [AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx)メソッドで使用している、`Seed`メソッドです。 ([AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx)はデータベースの用語を"upsert"操作に相当します)。

パッケージ マネージャー コンソール (PMC) で、次のコマンドを入力します。

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample4.cmd)]

`add-migration`コマンドという名前のファイルを作成する*&lt;タイムスタンプ&gt;\_MaxLengthOnNames.cs*です。 このファイルには、現在のデータ モデルと一致するようにデータベースを更新する `Up` メソッドのコードが含まれます。 `update-database` コマンドでそのコードが実行されました。

移行ファイル名に付加されるタイムスタンプは、Entity Framework によって、移行を並べ替えに使用されます。 実行する前に複数の移行を作成することができます、`update-database`コマンド、し、すべての移行は、作成された順序で適用されます。

実行、**作成**ページ、および、50 文字以下のいずれかの名前を入力します。 **[作成]** をクリックすると、クライアント側の検証でエラー メッセージが表示されます。

![クライアント側の val エラー](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image3.png)

### <a name="the-column-attribute"></a>列の属性

属性を使用して、データベースへのクラスとプロパティのマッピング方法を制御することもできます。 フィールドにミドル ネームも含まれている場合があるため、名フィールドに対して `FirstMidName` という名前を使用したとします。 ただし、データベース列は `FirstName` という名前にする必要があります。これは、データベースに対するアドホック クエリを記述するユーザーがその名前に慣れているためです。 このマッピングを作成する場合、`Column` 属性を使用できます。

`Column` 属性は、データベースの作成時に、`FirstMidName` プロパティにマップする `Student` テーブルの列が `FirstName` という名前になるように指定します。 つまり、コードが `Student.FirstMidName` を参照したときに、データが `Student` テーブルの `FirstName` 列から取り込まれるか、更新されます。 列名を指定しない場合、プロパティ名と同じ名前が付与されます。

*Student.cs*ファイルに追加し、`using`の声明[System.ComponentModel.DataAnnotations.Schema](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.aspx)に列名の属性を追加し、`FirstMidName`プロパティのように、次の強調表示されたコード。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample5.cs?highlight=4,14)]

追加、[列属性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx)SchoolContext をバックアップするので、データベースと一致しない、モデルを変更します。 PMC 別の移行を作成するには、次のコマンドを入力します。

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample6.cmd)]

**サーバー エクスプ ローラー**を開き、*学生*テーブル デザイナーをダブルクリックして、*学生*テーブル。

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image4.png)

次の図は、最初の 2 つの移行を適用する前に、元の列名を示します。 変更する列名だけでなく`FirstMidName`に`FirstName`から次の 2 つの名前の列が変更された`MAX`50 文字の長さ。

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image5.png)

変更することもデータベース マッピングを使用して、 [Fluent API](https://msdn.microsoft.com/data/jj591617)ように、このチュートリアルの後半で表示されます。

> [!NOTE]
> 次のセクションですべてのエンティティ クラスの作成を完了する前にコンパイルしようとすると、コンパイラ エラーが発生する可能性があります。


## <a name="complete-changes-to-the-student-entity"></a>学生のエンティティへの変更します。

![Student_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image6.png)

*Models\Student.cs*、次のコードで以前に追加したコードを置き換えます。 変更が強調表示されています。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=11,13,15,18,22,25-32)]

### <a name="the-required-attribute"></a>必須の属性

[必須の属性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)名プロパティの必須フィールドを使用します。 `Required attribute`は必要ありません値の型など、DateTime、int、double、および float です。 必須フィールドとして扱われる本質的にようににより、値の型に null の値を割り当てることはできません。 削除することで、[必須の属性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)の最小の長さのパラメーターに置き換えると、`StringLength`属性。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample8.cs?highlight=2)]

### <a name="the-display-attribute"></a>表示属性

`Display` 属性は、テキスト ボックスのキャプションを、各インスタンスのプロパティ名 (単語を区切るスペースがない) の代わりに、"First Name"、"Last Name"、"Full Name"、"Enrollment Date" にするよう指定します。

### <a name="the-fullname-calculated-property"></a>計算プロパティでは、FullName

`FullName` は集計プロパティであり、2 つの別のプロパティを連結して作成される値を返します。 したがってがのみ、`get`アクセサー、およびなし`FullName`データベース内の列が生成されます。

## <a name="create-the-instructor-entity"></a>Instructor エンティティを作成する

![Instructor_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image7.png)

作成*Models\Instructor.cs*、テンプレート コードを次のコードに置き換えます。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample9.cs)]

`Student` エンティティと `Instructor` エンティティのいくつかのプロパティが同じであることに注目してください。 このシリーズの後半の[継承の実装](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)に関するチュートリアルでは、冗長さをなくすため、このコードをリファクタリングします。

次のように、インストラクター クラスを作成することも、1 行に複数の属性を配置できます。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

### <a name="the-courses-and-officeassignment-navigation-properties"></a>コースと OfficeAssignment ナビゲーション プロパティ

`Courses` と `OfficeAssignment` プロパティはナビゲーション プロパティです。 前に説明したとおり、通常は定義として[仮想](https://msdn.microsoft.com/library/9fkccyh4(v=vs.110).aspx)と呼ばれる Entity Framework 機能の活用できるように[遅延読み込み](https://msdn.microsoft.com/magazine/hh205756.aspx)です。 さらに、ナビゲーション プロパティが複数のエンティティを保持できる場合、型実装する必要があります、 [ICollection&lt;T&gt; ](https://msdn.microsoft.com/library/92t2ye13.aspx)インターフェイスです。 たとえば[IList&lt;T&gt; ](https://msdn.microsoft.com/library/5y536ey6.aspx)はなく修飾[IEnumerable&lt;T&gt; ](https://msdn.microsoft.com/library/9eekhta0.aspx)ため`IEnumerable<T>`実装していません[追加](https://msdn.microsoft.com/library/63ywd54z.aspx)。

インストラクターは任意の数のコースを教えることができますので、`Courses`のコレクションとして定義された`Course`エンティティです。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

当社のビジネス ルールの状態、インストラクターだけが最大で 1 つのオフィスのため`OfficeAssignment`は、1 つとして定義されて`OfficeAssignment`エンティティ (可能性もあります`null`office が割り当てられていない場合)。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

## <a name="create-the-officeassignment-entity"></a>OfficeAssignment エンティティを作成します。

![OfficeAssignment_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image8.png)

作成*Models\OfficeAssignment.cs*を次のコード。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

変更を保存してすべてのコピーを作成していないことを確認すると、プロジェクトをビルドし、コンパイラにキャッチできるエラーを貼り付けます。

### <a name="the-key-attribute"></a>キー属性

0 または 1 を 1 つの関係がある、`Instructor`と`OfficeAssignment`エンティティです。 オフィス割り当てのみに、割り当てられているインストラクターに関連してあり、したがって、主キーも、外部キーを`Instructor`エンティティです。 Entity Framework を自動的に認識できないが、`InstructorID`プライマリとしてその名前に従っていないため、このエンティティのキー、`ID`または*classname* `ID`名前付け規則です。 したがって、`Key` 属性はキーとして識別するために使用されます。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample14.cs)]

使用することも、`Key`属性のエンティティが、独自のプライマリ キーを持っていて、プロパティの名前とは異なるものにする`classnameID`または`ID`です。 既定では、列はリレーションシップ用のため、EF はデータベースによって生成された非としてキーを扱います。

### <a name="the-foreignkey-attribute"></a>ForeignKey 属性

0 または 1 を 1 つのリレーションシップまたは 2 つのエンティティ間の一対一のリレーションシップがある場合 (との間でこのような`OfficeAssignment`と`Instructor`)、EF はリレーションシップのエンドは、プリンシパルとする end が依存する動作できません。 一対一リレーションシップでは、その他のクラスには、各クラスに参照ナビゲーション プロパティがあります。 [ForeignKey 属性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx)リレーションシップを確立するために依存するクラスに適用できます。 省略した場合、 [ForeignKey 属性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx)移行を作成するときに、次のエラーを取得します。

*型 'ContosoUniversity.Models.OfficeAssignment' および 'ContosoUniversity.Models.Instructor' 間のアソシエーションのプリンシパル end を特定できません。このアソシエーションのプリンシパル end は、リレーションシップ fluent API またはデータ注釈を使用して明示的に構成する必要があります。*

このチュートリアルで後ほど、この関係 fluent API を構成する方法が表示されます。

### <a name="the-instructor-navigation-property"></a>講師ナビゲーション プロパティ

`Instructor`エンティティに null 許容`OfficeAssignment`ナビゲーション プロパティ (インストラクター オフィス割り当てがあるない) ため、および`OfficeAssignment`エンティティには、null 非許容`Instructor`ナビゲーション プロパティ (ため、オフィス割り当てできません講師--がないと存在`InstructorID`が null 非許容の)。 ときに、`Instructor`エンティティに関連する`OfficeAssignment`エンティティ、各エンティティのナビゲーション プロパティで、もう 1 つへの参照になります。

置いたり、`[Required]`インストラクター ナビゲーション プロパティに関連のインストラクターである必要がありますが、これを行う (これは、このテーブルへのキーではも)、InstructorID 外部キーが null 非許容のためがないを指定する属性。

## <a name="modify-the-course-entity"></a>Course エンティティを変更する

![Course_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image9.png)

*Models\Course.cs*、次のコードで以前に追加したコードを置き換えます。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

Course エンティティは、外部キーのプロパティを持つ`DepartmentID`、関連するどの地点`Department`エンティティとそれが、`Department`ナビゲーション プロパティ。 Entity Framework では、関連エンティティのナビゲーション プロパティがある場合、ユーザーがデータ モデルに外部キー プロパティを追加する必要はありません。 EF を使用、データベースには、必要に応じての外部キーが自動的に作成します。 ただし、データ モデルに外部キーがある場合は、更新をより簡単かつ効率的に行うことができます。 たとえばを編集するには、コース エンティティをフェッチする、`Department`エンティティが null、読み込まない場合ので、course エンティティを更新するときにする必要が最初のフェッチ、`Department`エンティティです。 ときに外部キー プロパティ`DepartmentID`が含まれるデータ モデルでフェッチする必要はありません、`Department`エンティティを更新する前にします。

### <a name="the-databasegenerated-attribute"></a>DatabaseGenerated 属性

[DatabaseGenerated 属性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute.aspx)で、 [None](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.110).aspx)上のパラメーター、`CourseID`プロパティは、主キーの値は、ユーザーによって提供されるではなくデータベースによって生成されたを指定します。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

既定では、Entity Framework は、主キーの値がデータベースによって生成されると仮定します。 これはほとんどのシナリオに該当します。 ただし、`Course`エンティティを 1 つの部門別の部門の 2000年シリーズの一連の 1000 ようコースのユーザーが指定した番号を使用して、します。

### <a name="foreign-key-and-navigation-properties"></a>Foreign Key 制約とナビゲーション プロパティ

外部キーのプロパティとナビゲーション プロパティで、`Course`エンティティは、次のリレーションシップを反映します。

- コースは 1 つの学科に割り当てられます。したがって、前述の理由により、`DepartmentID` 外部キーと `Department` ナビゲーション プロパティが存在します。 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]
- コースには任意の数の学生が登録できるため、`Enrollments` ナビゲーション プロパティはコレクションとなります。 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample18.cs)]
- コースは複数の講師が担当する場合があるため、`Instructors` ナビゲーション プロパティはコレクションとなります。 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample19.cs)]

## <a name="create-the-department-entity"></a>部門 エンティティを作成します。

![Department_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image10.png)

作成*Models\Department.cs*を次のコード。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample20.cs)]

### <a name="the-column-attribute"></a>列の属性

以前を使用して、[列属性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx)列名のマッピングを変更します。 コードで、 `Department` 、エンティティ、 `Column` SQL データ型マッピングの SQL Server を使用して列を定義するように変更する属性が使用されている[money](https://msdn.microsoft.com/library/ms179882.aspx)データベース内の型。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample21.cs)]

列マッピングは一般的に必要ですが、Entity Framework は通常、プロパティを定義する CLR 型に基づく適切な SQL Server データ型が選択されるためです。 CLR `decimal` 型は SQL Server の `decimal` 型にマップされます。 ここでは、通貨、列が保持するを特定し、 [money](https://msdn.microsoft.com/library/ms179882.aspx)データ型がより適しています。 CLR データ型と SQL Server データ型に一致する方法の詳細については、次を参照してください。 [Entity Framework 用 SqlClient](https://msdn.microsoft.com/library/bb896344.aspx)です。

### <a name="foreign-key-and-navigation-properties"></a>Foreign Key 制約とナビゲーション プロパティ

外部キーおよびナビゲーション プロパティには、次のリレーションシップが反映されます。

- 学科には管理者が存在する場合とそうでない場合があり、管理者は常に講師となります。 したがって、`InstructorID`プロパティが外部キーとして含まれて、`Instructor`エンティティ、および疑問符 () は、後に追加された、`int`タイプの null 値許容とプロパティを指定します。ナビゲーション プロパティの名前が`Administrator`を保持するが、`Instructor`エンティティ。 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample22.cs)]
- 部門があります複数のコースがあるため、`Courses`ナビゲーション プロパティ。 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample23.cs)]

  > [!NOTE]
  > 規則により、Entity Framework では null 非許容の外部キーと多対多リレーションシップに対して連鎖削除が有効になります。 これにより、循環連鎖削除規則が適用される可能性があり、移行を追加しようとすると例外が発生します。 たとえば、定義されていないのに、`Department.InstructorID`プロパティを null 許容型として、次の例外メッセージが表示される:「参照関係になります循環参照には許可されていません」。 ビジネス ルールが必要な場合`InstructorID`プロパティを null 非許容、ステートメントを使用して、次 fluent API をリレーションシップで連鎖削除を無効にする必要があります。 

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample24.cs)]


## <a name="modify-the-enrollment-entity"></a>登録のエンティティを変更します。

![Enrollment_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image11.png)

 *Models\Enrollment.cs*、次のコードで以前に追加したコードを置き換えます

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample25.cs?highlight=1,15)]

### <a name="foreign-key-and-navigation-properties"></a>Foreign Key 制約とナビゲーション プロパティ

外部キー プロパティとナビゲーション プロパティには、次のリレーションシップが反映されます。

- 登録レコードは単一のコースに対するものであるため、`CourseID` 外部キー プロパティと `Course` ナビゲーション プロパティがあります。 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample26.cs)]
- 登録レコードは 1 人の学生に対するものであるため、`StudentID` 外部キー プロパティと `Student` ナビゲーション プロパティがあります。 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample27.cs)]

### <a name="many-to-many-relationships"></a>多対多リレーションシップ

間の多対多リレーションシップが、`Student`と`Course`エンティティ、および`Enrollment`エンティティが、多対多の結合テーブルとして機能*ペイロードを持つ*データベースにします。 つまり、`Enrollment`テーブルにはだけでなく、結合されたテーブルの外部キーの追加のデータが含まれています (この場合は、主キーと`Grade`プロパティ)。

次の図は、エンティティ図でこれらのリレーションシップがどのようになるかを示しています  (この図を使用して生成された、 [Entity Framework Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d)以外の場合は、チュートリアルの一部ではない、ダイアグラムを作成、使用されているだけを例としては、ここです)。

![学生 many_relationship Course_many](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image12.png)

各リレーションシップの線が 1 つの end とアスタリスクを 1 (\*)、他方の一対多のリレーションシップを示すです。

場合、`Enrollment`テーブルに評価情報が含まれていなかった、2 つの外部キーを含めることはのみ`CourseID`と`StudentID`です。 はその場合は、多対多の結合テーブルに、対応*ペイロードせず*(または*純粋な結合テーブル*)、データベースのモデル クラスをまったく作成する必要はありません。 `Instructor`と`Course`エンティティがそのような多対多のリレーションシップがあるし、それらの間でエンティティ クラスが存在しないことがわかります。

![講師 many_relationship Course_many](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image13.png)

結合テーブルがデータベースで必須で、ただし、次のデータベース ダイアグラムに示すようにします。

![講師 many_relationship_tables Course_many](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image14.png)

Entity Framework を自動的に作成、`CourseInstructor`して、テーブルの読み取りし、更新されません、直接の読み取りと更新によって、`Instructor.Courses`と`Course.Instructors`ナビゲーション プロパティ。

## <a name="entity-diagram-showing-relationships"></a>リレーションシップを示すエンティティ図

次の図では、完成した School モデルに対して Entity Framework Power Tools で作成される図を示します。

![School_data_model_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image15.png)

多対多リレーションシップの線以外 (\*に\*) と一対多リレーションシップの線 (1 ~ \*)、ことがわかります (1 対 0..1) の 0 または 1 を 1 つのリレーションシップの線の間、`Instructor`と`OfficeAssignment`エンティティと 0-または-1-対多リレーションシップの線 (0..1 対\*)、インストラクターと Department エンティティ間です。

## <a name="customize-the-data-model-by-adding-code-to-the-database-context"></a>データベース コンテキストにコードを追加することで、データ モデルをカスタマイズします。

次に新しいエンティティを追加、`SchoolContext`クラスし、を使用して、マッピングの一部はカスタマイズ[fluent API](https://msdn.microsoft.com/data/jj591617)呼び出しです。 API は、使用しているため、多くの場合、一連のメソッド呼び出しに次の例のように、1 つのステートメントを組み合わせるに"fluent"。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample28.cs)]

このチュートリアルでは、属性を使用して行うことはできませんデータベース マッピングに対してのみ fluent API を使用します。 ただし、fluent API を使用して、属性で実行できる書式設定、検証、およびマッピング規則のほとんどを指定することもできます。 `MinimumLength` などの一部の属性は fluent API で適用できません。 前に述べたよう`MinimumLength`スキーマを変更しないクライアントとサーバー側の検証規則のみ適用されます

一部の開発者は fluent API のみを使用することを選ぶため、エンティティ クラスを "クリーン" な状態に保つことができます。 必要に応じて、属性と fluent API を組み合わせて使用することができます。fluent API のみを使用して実行できるカスタマイズがいくつかありますが、一般的は 2 つの方法のいずれかを選択して、できるだけ一貫性を保つためにそれを使用することをお勧めします。

モデルのデータに新しいエンティティおよび属性を使用して実行していないデータベース マッピングの実行を追加するコードで置き換え*DAL\SchoolContext.cs*を次のコード。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample29.cs)]

新しいステートメント、 [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx)メソッドは、多対多の結合テーブルを構成します。

- 多対多のリレーションシップ、`Instructor`と`Course`エンティティ、コードが、結合テーブルのテーブルと列の名前を指定します。 コード最初に構成できる多対多リレーションシップをこのコードを使用せずがしないで呼び出す場合、表示される既定の名前など`InstructorInstructorID`の`InstructorID`列です。

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample30.cs)]

次のコードを例示する可能性がありますが使用する方法の fluent API 属性ではなく間のリレーションシップを指定する、`Instructor`と`OfficeAssignment`エンティティ。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample31.cs)]

詳細についてはどのような"fluent API"ステートメントはバック グラウンドで、次を参照してください。、 [Fluent API](https://blogs.msdn.com/b/aspnetue/archive/2011/05/04/entity-framework-code-first-tutorial-supplement-what-is-going-on-in-a-fluent-api-call.aspx)ブログの投稿。

## <a name="seed-the-database-with-test-data"></a>テスト データでデータベースをシードする

コードで置き換え、 *Migrations\Configuration.cs*を作成した新しいエンティティのシードにデータを提供するために次のコード ファイル。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample32.cs)]

最初のチュートリアルで説明したとおりこのコードの大部分を単に更新または新しいエンティティ オブジェクトを作成し、プロパティをテストするために必要に応じてにサンプル データを読み込みます。 ただし、方法、`Course`を多対多のリレーションシップを持つエンティティで、`Instructor`エンティティの処理します。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample33.cs)]

作成するときに、`Course`オブジェクトを初期化する、`Instructors`ナビゲーション プロパティをコードを使用して空のコレクションとして`Instructors = new List<Instructor>()`です。 これにより、追加すること`Instructor`これに関連付けられているエンティティ`Course`を使用して、`Instructors.Add`メソッドです。 空のリストを作成していない場合はありませんできるこれらのリレーションシップを追加するため、`Instructors`プロパティが null では必要はありません、`Add`メソッドです。 コンス トラクターにリストの初期化を追加することもできます。

## <a name="add-a-migration-and-update-the-database"></a>移行を追加し、データベースの更新

PMC から次のように入力します。、`add-migration`コマンド (を行わないと、`update-database`まだコマンド)。

`add-Migration ComplexDataModel`

この時点で `update-database` コマンドを実行しようとした場合は (まだ実行しないでください)、以下のようなエラーが表示されます。

*ALTER TABLE ステートメントは、FOREIGN KEY 制約と競合して"FK\_dbo します。コース\_dbo します。部門\_DepartmentID"です。競合が発生したデータベース"ContosoUniversity"テーブル"dbo します。部門"、列 'DepartmentID' です。*

既存のデータと移行を実行するときにスタブ データを外部キー制約を満たすためにデータベースに挿入する必要がありますを今すぐ行う必要があります。 生成されたコードに、ComplexDataModel`Up`メソッドは、null 非許容の追加`DepartmentID`への外部キー、`Course`テーブル。 行が既にあるため、`Course`表に、コードを実行すると、 `AddColumn` SQL Server が null にすることはできません、列内の値を認識していないため操作は失敗します。 既定値では、新しい列を付与し、既定の部門として機能するには、"Temp"という名前の明細部分を作成するコードを変更しなければならないためです。 その結果、既存の`Course`行はすべてに関連している後に"Temp"部門、`Up`メソッドが実行されます。 内の正しい部門に関連付けることができます、`Seed`メソッドです。

編集、 &lt;*タイムスタンプ&gt;\_ComplexDataModel.cs*ファイル、Course テーブルに、[DepartmentID] 列を追加するコードの行をコメントと (、コメントが付けられた次の強調表示されたコードを追加行も強調表示されます)。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample34.cs?highlight=14-18)]

ときに、`Seed`メソッドを実行する内の行を挿入は、`Department`テーブルの既存の関連`Course`これら新しい行`Department`行です。 UI では、任意のコースを追加していない、し、不要になったが必要になります"Temp"部門または既定値で、`Course.DepartmentID`列です。 を、ユーザーが追加可能性がありますコース、アプリケーションを使用して発生する可能性を許可するのにはも更新する、`Seed`ことを確認するメソッドのコードすべて`Course`行 (の以前の実行によって挿入されたものだけでなく、`Seed`メソッド) が有効な`DepartmentID`値、既定値を削除する前に、列の値し、"Temp"部門を削除します。

編集が完了した後、 &lt;*タイムスタンプ&gt;\_ComplexDataModel.cs*ファイル、入力、 `update-database` PMC 移行を実行するコマンド。

[!code-powershell[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample35.ps1)]

> [!NOTE]
> データとスキーマの変更を移行するときにその他のエラーが発生することができます。 解決できない移行エラーが発生した場合は、接続文字列のデータベース名を変更するか、データベースを削除できます。 データベースの名前を変更する最も簡単な方法は、 *Web.config*ファイル。 次の例は、名前が CU に変更された\_テスト。
> 
> [!code-xml[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample36.xml?highlight=1)]
> 
> 新しいデータベースを移行するデータがないと、`update-database`コマンドがエラーなしで完了する可能性が高くなります。 データベースを削除する方法については、次を参照してください。[を Visual Studio 2012 からデータベースを削除する方法](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/)です。
> 
> 失敗した場合、もう 1 つを試みることができますが、PMC で、次のコマンドを入力して、データベースを再初期化を示します。
> 
> `update-database -TargetMigration:0`


データベースを開く**サーバー エクスプ ローラー**前、展開すると、**テーブル**ノードをすべてのテーブルが作成されたことを確認します。 (がある場合は**サーバー エクスプ ローラー**以前の状態からを開き、、**更新**ボタンをクリックします)。

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image16.png)

モデル クラスを作成していない、`CourseInstructor`テーブル。 これには、多対多リレーションシップの間の結合テーブルを前述のように、`Instructor`と`Course`エンティティです。

右クリックし、`CourseInstructor`テーブルを選択して**テーブル データの表示**がの結果としてデータが入っていることを確認する、`Instructor`エンティティに追加した、`Course.Instructors`ナビゲーション プロパティ。

![Table_data_in_CourseInstructor_table](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image17.png)

## <a name="summary"></a>まとめ

これで、より複雑なデータ モデルと対応するデータベースの準備ができました。 次のチュートリアルで学習関連のデータにアクセスするさまざまな方法の詳細。

このチュートリアルをリンクする方法と、何を改善にフィードバックを送信してください。 新しいトピックを要求することもできます。 [Me 方法でコードの表示](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)です。

その他の Entity Framework リソースへのリンクは含まれて、 [ASP.NET データ アクセス - リソースのことをお勧め](../../../../whitepapers/aspnet-data-access-content-map.md)です。

> [!div class="step-by-step"]
> [前へ](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [次へ](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
