---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
title: "ASP.NET MVC アプリケーション (10 の 4) のより複雑なデータ モデルの作成 |Microsoft ドキュメント"
author: tdykstra
description: "Contoso 大学でサンプル web アプリケーションでは、Entity Framework 5 Code First と Visual Studio を使用して ASP.NET MVC 4 アプリケーションを作成する方法について説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: f81f3d80-3674-4d8e-a9b1-87feed1a93c9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: accb5ddab8df67dfa29038541dc0cd72eaac173c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="creating-a-more-complex-data-model-for-an-aspnet-mvc-application-4-of-10"></a>ASP.NET MVC アプリケーション (10 の 4) のより複雑なデータ モデルの作成
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[完成したプロジェクトをダウンロードします。](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso 大学でサンプル web アプリケーションでは、Entity Framework 5 Code First と Visual Studio 2012 を使用して ASP.NET MVC 4 アプリケーションを作成する方法を示します。 一連のチュートリアルについては、次を参照してください。[系列内の最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)です。 一連のチュートリアルを開始するには、最初からまたは[この章のスタート プロジェクトをダウンロード](building-the-ef5-mvc4-chapter-downloads.md)し、ここから開始します。
> 
> > [!NOTE] 
> > 
> > 解決できない場合、問題が発生した場合[ダウンロード完了章](building-the-ef5-mvc4-chapter-downloads.md)の問題を再現しようとします。 一般に、コードを完成したコードを比較することによって、問題の解決策を検索できます。 一般的なエラーとそれらを解決する方法は、次を参照してください。[エラーと回避策です。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


前のチュートリアルでは、3 つのエンティティで構成されている単純なデータ モデルで機能します。 このチュートリアルでは、複数のエンティティとリレーションシップを追加し、書式設定、検証、およびデータベース マッピング規則を指定することによって、データ モデルをカスタマイズします。 データ モデルをカスタマイズする 2 つの方法が表示されます: とデータベースのコンテキスト クラスにコードを追加することでエンティティ クラスには、属性を追加しています。

完了したら、エンティティ クラスは、次の図に示す完成したデータ モデルを構成します。

![School_class_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

## <a name="customize-the-data-model-by-using-attributes"></a>属性を使用して、データ モデルをカスタマイズします。

このセクションでは、書式設定、検証、およびデータベース マッピング規則を指定する属性を使用して、データ モデルをカスタマイズする方法が表示されます。 次のセクションでは、のいくつかありますを作成してから、完全な`School`追加することでデータ モデルの属性クラスに既にモデル内の残りのエンティティ型の新しいクラスを作成し、作成します。

### <a name="the-datatype-attribute"></a>データ型の属性

学生の登録日のすべての web ページ現在表示、日付と時刻が、このフィールドの関心のあるは、日付。 データの注釈属性を使用することができますいずれかのコード変更データを表示するすべてのビューの表示形式を修正します。 属性を追加することを実行する方法の例を参照する、`EnrollmentDate`プロパティに、`Student`クラスです。

*Models\Student.cs*、追加、`using`のステートメント、`System.ComponentModel.DataAnnotations`名前空間を追加および`DataType`と`DisplayFormat`属性を`EnrollmentDate`プロパティ、次の例で示すように。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,13-14)]

[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性を使用してデータベースの組み込み型よりも特定のデータ型を指定します。 ここでのみが必要を追跡する、日付、日付と時刻がありません。 [DataType 列挙](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)などの多くのデータ型の提供*日付、時刻、PhoneNumber、通貨、EmailAddress*などです。 また、`DataType` 属性を使用して、アプリケーションで型固有の機能を自動的に提供することもできます。 たとえば、`mailto:`に対してリンクを作成できる[DataType.EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)、日付選択を指定することができます、 [DataType.Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)をサポートするブラウザーで[HTML5](http://html5.org/). [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性は、HTML 5 を出力[データ -](http://ejohn.org/blog/html-5-data-attributes/) (発音*データ ダッシュ*) HTML 5 ブラウザーで認識できる属性です。 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性は、いずれかの検証を渡さないようにします。

`DataType.Date` は、表示される日付の書式を指定しません。 既定では、データ フィールドを基に、サーバーの既定の形式に従って表示[CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx)です。

`DisplayFormat` 属性は、日付の書式を明示的に指定するために使用されます。


[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample2.cs)]


`ApplyFormatInEditMode`設定では、指定した書式設定も適用されることを編集するためのテキスト ボックスが表示されたら、値を指定します。 (したくないをいくつかのフィールドの — たとえば、通貨の値のたくない、テキスト ボックス内の通貨記号を編集するためです)。

使用することができます、 [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)自体が、属性が使用することをお勧めでは一般に、 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)も属性します。 `DataType`属性を伝達、*セマンティクス*データとして、画面に表示する方法ではなくおよびのでは得られない、次の利点を提供`DisplayFormat`:

- ブラウザーには、HTML5 機能 (たとえば、予定表コントロール、ロケールに応じた通貨記号、電子メールへのリンクなどを表示します。) が有効にすることができます。
- 既定では、ブラウザーがに基づいて、正しい形式を使用してデータを表示、[ロケール](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx)です。
- [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)属性には、データを表示するために右フィールド テンプレートを選択する MVC が有効にすることができます (、 [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)文字列テンプレートを使用して自体によって使用されている場合)。 詳細については、Brad Wilson を参照してください。 [ASP.NET MVC 2 テンプレート](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html)です。 (MVC 2 用に記述されたがこの記事の内容も適用 ASP.NET MVC の現在のバージョンに)。

使用する場合、`DataType`属性指定する必要が、日付フィールドに関連付け、 `DisplayFormat` Chrome ブラウザーで、フィールドが正常に表示されることを確認するためにも属性。 詳細については、次を参照してください。[この StackOverflow スレッド](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie)です。

学生のインデックス ページを再度実行し、登録の日付の時刻が表示されていないことに注意してください。 いずれかのビューを使用する場合は true。 同じになります、`Student`モデル。

![Students_index_page_with_formatted_date](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image2.png)

### <a name="the-stringlengthattribute"></a>StringLengthAttribute

データ検証ルールおよび属性を使用してメッセージを指定することもできます。 ユーザーが名前の 50 個を超える文字を入力しないことを確認するとします。 この制限を追加する追加[StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)属性を`LastName`と`FirstMidName`プロパティ、次の例で示すようにします。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=10,12)]

[StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)属性は、名前に空白文字を入力するからユーザーを妨げることはありません。 使用することができます、[正規表現](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx)入力に制限を適用する属性。 たとえば、次のコードには、最初の文字が大文字で指定し、残りの文字は英文字でが必要です。

`[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]`

[MaxLength](https://msdn.microsoft.com/library/System.ComponentModel.DataAnnotations.MaxLengthAttribute.aspx)属性と同様の機能を提供する、 [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)属性が、クライアント側では提供しません検証します。

アプリケーションを実行し、をクリックして、**受講者**タブです。次のエラーが発生します。

*データベースが作成されたために、'SchoolContext' コンテキストのバックアップ モデルが変更されました。Code First Migrations を使用して、データベースを更新するを検討してください ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269))。*

データベース スキーマの変更を必要とするように、データベース モデルが変更され、Entity Framework では、ことが検出されました。 UI を使用して、データベースに追加したすべてのデータを失うことがなく、スキーマを更新するのにには移行を使用します。 によって作成されたデータを変更した場合、`Seed`メソッドは、その元の状態のために変更される、 [AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx)メソッドで使用している、`Seed`メソッドです。 ([AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx)はデータベースの用語を"upsert"操作に相当します)。

パッケージ マネージャー コンソール (PMC) では、次のコマンドを入力します。

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample4.cmd)]

`add-migration MaxLengthOnNames`コマンドという名前のファイルを作成する*&lt;タイムスタンプ&gt;\_MaxLengthOnNames.cs*です。 このファイルには、現在のデータ モデルと一致するデータベースを更新するコードが含まれています。 移行ファイル名に付加されるタイムスタンプは、Entity Framework によって、移行を並べ替えに使用されます。 データベースを削除する場合は、複数の移行を作成した後、または移行を使用して、プロジェクトを展開する場合は、作成された順序ですべての移行の適用されます。

実行、**作成**ページ、および、50 文字以下のいずれかの名前を入力します。 50 文字を超えると、すぐに、クライアント側の検証はすぐに、エラー メッセージを示します。

![クライアント側の val エラー](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image3.png)

### <a name="the-column-attribute"></a>列の属性

クラスとプロパティをデータベースにマップする方法を制御するのに属性を使用することもできます。 名前を使用するいたと仮定します`FirstMidName`最初の-名前のフィールドのフィールドには、ミドル ネームが含まれる場合もあるためです。 選択した名前を指定するデータベース列が、`FirstName`データベースに対してアドホック クエリを記述するユーザーがその名前に慣れているため、します。 このマッピングを作成するには、使用することができます、`Column`属性。

`Column`属性を指定するデータベースが作成されるとき、列の`Student`をマップするテーブル、`FirstMidName`プロパティの名前は`FirstName`します。 つまり、ときに、コードを指す`Student.FirstMidName`、データから得られますまたはで更新される、`FirstName`の列、`Student`テーブル。 列名を指定しない場合、プロパティ名と同じ名前が付与されます。

使用して、追加のステートメント[System.ComponentModel.DataAnnotations.Schema](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.aspx)と列の名前属性を`FirstMidName`プロパティ、次の強調表示されたコードに示すようにします。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample5.cs?highlight=4,14)]

追加、[列属性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx)SchoolContext をバックアップするので、データベースと一致しない、モデルを変更します。 PMC 別の移行を作成するには、次のコマンドを入力します。

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample6.cmd)]

**サーバー エクスプ ローラー** (**データベース エクスプ ローラー** for Web Express を使用している場合) をダブルクリックして、*学生*テーブル。

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image4.png)

次の図は、最初の 2 つの移行を適用する前に、元の列名を示します。 変更する列名だけでなく`FirstMidName`に`FirstName`から次の 2 つの名前の列が変更された`MAX`50 文字の長さ。

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image5.png)

変更することもデータベース マッピングを使用して、 [Fluent API](https://msdn.microsoft.com/data/jj591617)ように、このチュートリアルの後半で表示されます。

> [!NOTE]
> すべてのエンティティ クラスの作成を完了する前に、コンパイルしようとすると場合、コンパイラ エラーが発生する可能性があります。


## <a name="create-the-instructor-entity"></a>Instructor エンティティを作成します。

![Instructor_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image6.png)

作成*Models\Instructor.cs*、テンプレート コードを次のコードに置き換えます。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample7.cs)]

いくつかのプロパティは、同じことに注意してください、`Student`と`Instructor`エンティティです。 [実装の継承](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)このシリーズの後でチュートリアルでは、この重複を排除する継承を使用してリファクターします。

### <a name="the-required-and-display-attributes"></a>必要な属性を表示および

上の属性、`LastName`プロパティでは、必須のフィールド、テキスト ボックスのキャプションにする必要があります"Last Name"ではなく、プロパティ名がスペースを入れずに"LastName"になります)、および値は 50 文字より長くすることはできませんがあることを指定します。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

[StringLength 属性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)データベースの最大長を設定し、クライアント側とサーバー側は、ASP.NET MVC の検証。 この属性には、文字列の長さを指定することも、最小値データベース スキーマに影響がありません。 [必須の属性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)は必要ありません値の型など、DateTime、int、double、および float です。 本質的に必須では、により、値の型に null の値を割り当てることはできません。 削除することで、[必須の属性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)の最小の長さのパラメーターに置き換えると、`StringLength`属性。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample9.cs?highlight=2)]

次のように、インストラクター クラスを作成することも、1 行に複数の属性を配置できます。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

### <a name="the-fullname-calculated-property"></a>計算プロパティでは、FullName

`FullName`その他の 2 つのプロパティを連結することによって作成される値を返す計算されるプロパティです。 したがってがのみ、`get`アクセサー、およびなし`FullName`データベース内の列が生成されます。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

### <a name="the-courses-and-officeassignment-navigation-properties"></a>コースと OfficeAssignment ナビゲーション プロパティ

`Courses`と`OfficeAssignment`プロパティは、ナビゲーション プロパティ。 前に説明したとおり、通常は定義として[仮想](https://msdn.microsoft.com/library/9fkccyh4(v=vs.110).aspx)と呼ばれる Entity Framework 機能の活用できるように[遅延読み込み](https://msdn.microsoft.com/magazine/hh205756.aspx)です。 さらに、ナビゲーション プロパティが複数のエンティティを保持できる場合、型実装する必要があります、 [ICollection&lt;T&gt; ](https://msdn.microsoft.com/library/92t2ye13.aspx)インターフェイスです。 (たとえば[IList&lt;T&gt; ](https://msdn.microsoft.com/library/5y536ey6.aspx)はなく修飾[IEnumerable&lt;T&gt; ](https://msdn.microsoft.com/library/9eekhta0.aspx)ため`IEnumerable<T>`実装していません[追加。](https://msdn.microsoft.com/library/63ywd54z.aspx).

インストラクターは任意の数のコースを教えることができますので、`Courses`のコレクションとして定義された`Course`エンティティです。 当社のビジネス ルールの状態、インストラクターだけが最大で 1 つのオフィスのため`OfficeAssignment`は、1 つとして定義されて`OfficeAssignment`エンティティ (可能性もあります`null`office が割り当てられていない場合)。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

## <a name="create-the-officeassignment-entity"></a>OfficeAssignment エンティティを作成します。

![OfficeAssignment_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image7.png)

作成*Models\OfficeAssignment.cs*を次のコード。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

変更を保存してすべてのコピーを作成していないことを確認すると、プロジェクトをビルドし、コンパイラにキャッチできるエラーを貼り付けます。

### <a name="the-key-attribute"></a>キー属性

0 または 1 を 1 つの関係がある、`Instructor`と`OfficeAssignment`エンティティです。 オフィス割り当てのみに、割り当てられているインストラクターに関連してあり、したがって、主キーも、外部キーを`Instructor`エンティティです。 Entity Framework を自動的に認識できないが、`InstructorID`プライマリとしてその名前に従っていないため、このエンティティのキー、`ID`または*classname* `ID`名前付け規則です。 したがって、`Key`属性は、キーとして識別するために使用します。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample14.cs)]

使用することも、`Key`属性のエンティティが、独自のプライマリ キーを持っていて、プロパティの名前とは異なるものにする`classnameID`または`ID`です。 既定では、列はリレーションシップ用のため、EF はデータベースによって生成された非としてキーを扱います。

### <a name="the-foreignkey-attribute"></a>ForeignKey 属性

0 または 1 を 1 つのリレーションシップまたは 2 つのエンティティ間の一対一のリレーションシップがある場合 (との間でこのような`OfficeAssignment`と`Instructor`)、EF はリレーションシップのエンドは、プリンシパルとする end が依存する動作できません。 一対一リレーションシップでは、その他のクラスには、各クラスに参照ナビゲーション プロパティがあります。 [ForeignKey 属性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx)リレーションシップを確立するために依存するクラスに適用できます。 省略した場合、 [ForeignKey 属性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx)移行を作成するときに、次のエラーを取得します。

型 'ContosoUniversity.Models.OfficeAssignment' および 'ContosoUniversity.Models.Instructor' 間のアソシエーションのプリンシパル end を特定できません。 このアソシエーションのプリンシパル end は、リレーションシップ fluent API またはデータ注釈を使用して明示的に構成する必要があります。

このチュートリアルで後ほどは、この関係 fluent API を構成する方法を紹介します。

### <a name="the-instructor-navigation-property"></a>講師ナビゲーション プロパティ

`Instructor`エンティティに null 許容`OfficeAssignment`ナビゲーション プロパティ (インストラクター オフィス割り当てがあるない) ため、および`OfficeAssignment`エンティティには、null 非許容`Instructor`ナビゲーション プロパティ (ため、オフィス割り当てできません講師--がないと存在`InstructorID`が null 非許容の)。 ときに、`Instructor`エンティティに関連する`OfficeAssignment`エンティティ、各エンティティのナビゲーション プロパティで、もう 1 つへの参照になります。

置いたり、`[Required]`インストラクター ナビゲーション プロパティに関連のインストラクターである必要がありますが、これを行う (これは、このテーブルへのキーではも)、InstructorID 外部キーが null 非許容のためがないを指定する属性。

## <a name="modify-the-course-entity"></a>Course エンティティを変更します。

![Course_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image8.png)

*Models\Course.cs*、次のコードで以前に追加したコードを置き換えます。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

Course エンティティは、外部キーのプロパティを持つ`DepartmentID`、関連するどの地点`Department`エンティティとそれが、`Department`ナビゲーション プロパティ。 Entity Framework は、関連エンティティのナビゲーション プロパティがある場合、データ モデルに外部キーのプロパティを追加する必要ありません。 EF を使用、データベースには、必要に応じての外部キーが自動的に作成します。 簡素化されより効率的な更新プログラムを必ずデータ モデルの外部キーを持つことができます。 たとえばを編集するには、コース エンティティをフェッチする、`Department`エンティティが null、読み込まない場合ので、course エンティティを更新するときにする必要が最初のフェッチ、`Department`エンティティです。 ときに外部キー プロパティ`DepartmentID`が含まれるデータ モデルでフェッチする必要はありません、`Department`エンティティを更新する前にします。

### <a name="the-databasegenerated-attribute"></a>DatabaseGenerated 属性

[DatabaseGenerated 属性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute.aspx)で、 [None](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.110).aspx)上のパラメーター、`CourseID`プロパティは、主キーの値は、ユーザーによって提供されるではなくデータベースによって生成されたを指定します。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

既定では、Entity Framework は、主キーの値がデータベースによって生成されると仮定します。 ほとんどのシナリオに必要な要素です。 ただし、`Course`エンティティを 1 つの部門別の部門の 2000年シリーズの一連の 1000 ようコースのユーザーが指定した番号を使用して、します。

### <a name="foreign-key-and-navigation-properties"></a>Foreign Key 制約とナビゲーション プロパティ

外部キーのプロパティとナビゲーション プロパティで、`Course`エンティティは、次のリレーションシップを反映します。

- コースを 1 つの部門に割り当てられるがあるため、`DepartmentID`外部キーと`Department`上記の理由によりナビゲーション プロパティ。 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]
- コースに受講者に、登録済みの任意の数を持つことができますので、`Enrollments`ナビゲーション プロパティがコレクション。 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample18.cs)]
- 複数のインストラクターを習得するは、コースのため、`Instructors`ナビゲーション プロパティがコレクション。 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample19.cs)]

## <a name="creating-the-department-entity"></a>Department エンティティを作成します。

![Department_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image9.png)

作成*Models\Department.cs*を次のコード。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample20.cs)]

### <a name="the-column-attribute"></a>列の属性

以前を使用して、[列属性](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx)列名のマッピングを変更します。 コードで、 `Department` 、エンティティ、 `Column` SQL データ型マッピングの SQL Server を使用して列を定義するように変更する属性が使用されている[money](https://msdn.microsoft.com/library/ms179882.aspx)データベース内の型。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample21.cs)]

列マッピングは一般的に必要ですが、Entity Framework は通常、プロパティを定義する CLR 型に基づく適切な SQL Server データ型が選択されるためです。 CLR `decimal` SQL Server へのマップ」と入力`decimal`型です。 ここでは、通貨、列が保持するを特定し、 [money](https://msdn.microsoft.com/library/ms179882.aspx)データ型がより適しています。

### <a name="foreign-key-and-navigation-properties"></a>Foreign Key 制約とナビゲーション プロパティ

外部キー プロパティとナビゲーション プロパティは、次のリレーションシップを反映します。

- 部門、管理者がない可能性があり、管理者は、常に、インストラクター。 したがって、`InstructorID`プロパティが外部キーとして含まれて、`Instructor`エンティティ、および疑問符 () は、後に追加された、`int`タイプの null 値許容とプロパティを指定します。ナビゲーション プロパティの名前が`Administrator`を保持するが、`Instructor`エンティティ。 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample22.cs)]
- 部門があります複数のコースがあるため、`Courses`ナビゲーション プロパティ。 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample23.cs)]

 > [!NOTE]
 > 慣例により、Entity Framework null 非許容の外部キーおよび多対多リレーションシップの連鎖削除を有効にします。 これは、結果、循環 cascade delete ルールは、初期化コードを実行すると、例外が発生します。 たとえば、定義されていないのに、`Department.InstructorID`プロパティを null 許容型として、初期化子の実行時に次の例外メッセージが表示される:「参照関係になります循環参照には許可されていません」。 ビジネス ルールが必要な場合`InstructorID`として null 非許容のプロパティ、リレーションシップで連鎖削除を無効にする次の fluent API を使用する必要があります。 

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample24.cs)]


## <a name="modifying-the-student-entity"></a>学生のエンティティを変更します。

![Student_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image10.png)

*Models\Student.cs*、次のコードで以前に追加したコードを置き換えます。 変更が強調表示されます。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample25.cs?highlight=12,15,24-27)]

## <a name="the-enrollment-entity"></a>登録エンティティ

 *Models\Enrollment.cs*、次のコードで以前に追加したコードを置き換えます

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample26.cs?highlight=16)]

### <a name="foreign-key-and-navigation-properties"></a>Foreign Key 制約とナビゲーション プロパティ

外部キーのプロパティとナビゲーション プロパティは、次のリレーションシップを反映します。

- 登録レコードが、1 つのコースがあるため、`CourseID`外部キーのプロパティと`Course`ナビゲーション プロパティ。 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample27.cs)]
- 登録レコードが 1 つの受講者には、`StudentID`外部キーのプロパティと`Student`ナビゲーション プロパティ。 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample28.cs)]

### <a name="many-to-many-relationships"></a>多対多リレーションシップ

間の多対多リレーションシップが、`Student`と`Course`エンティティ、および`Enrollment`エンティティが、多対多の結合テーブルとして機能*ペイロードを持つ*データベースにします。 つまり、`Enrollment`テーブルにはだけでなく、結合されたテーブルの外部キーの追加のデータが含まれています (この場合は、主キーと`Grade`プロパティ)。

次の図は、エンティティの図でこれらのリレーションシップがどのようにを示します。 (この図を使用して生成された、 [Entity Framework Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d)以外の場合は、チュートリアルの一部ではない、ダイアグラムを作成、使用されているだけを例としては、ここです)。

![学生 many_relationship Course_many](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image11.png)

各リレーションシップの線が 1 つの end とアスタリスクを 1 (\*)、他方の一対多のリレーションシップを示すです。

場合、`Enrollment`テーブルに評価情報が含まれていなかった、2 つの外部キーを含めることはのみ`CourseID`と`StudentID`です。 はその場合は、多対多の結合テーブルに、対応*ペイロードせず*(または*純粋な結合テーブル*)、データベースのモデル クラスをまったく作成する必要はありません。 `Instructor`と`Course`エンティティがそのような多対多のリレーションシップがあるし、それらの間でエンティティ クラスが存在しないことがわかります。

![講師 many_relationship Course_many](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image12.png)

結合テーブルがデータベースで必須で、ただし、次のデータベース ダイアグラムに示すようにします。

![講師 many_relationship_tables Course_many](https://asp.net/media/2577802/Windows-Live-Writer_Creating-a.NET-MVC-Application-4-of-10h1_B662_Instructor-Course_many-to-many_relationship_tables_03e042cf-db89-4b4c-985a-e458351ada76.png)

Entity Framework を自動的に作成、`CourseInstructor`して、テーブルの読み取りし、更新されません、直接の読み取りと更新によって、`Instructor.Courses`と`Course.Instructors`ナビゲーション プロパティ。

## <a name="entity-diagram-showing-relationships"></a>エンティティの図の関係を示す

次の図は、Entity Framework のパワー ツールが、完成した School モデルを作成するダイアグラムを示します。

![School_data_model_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image13.png)

多対多リレーションシップの線以外 (\*に\*) と一対多リレーションシップの線 (1 ~ \*)、ことがわかります (1 対 0.1) の 0 または 1 を 1 つのリレーションシップの線の間、`Instructor`と`OfficeAssignment`エンティティと 0-または-1-対多リレーションシップの線 (0.1 対\*)、インストラクターと Department エンティティ間です。

## <a name="customize-the-data-model-by-adding-code-to-the-database-context"></a>データベース コンテキストにコードを追加することで、データ モデルをカスタマイズします。

次に新しいエンティティを追加、`SchoolContext`クラスし、を使用して、マッピングの一部はカスタマイズ[fluent API](https://msdn.microsoft.com/data/jj591617)呼び出しです。 (この API は、使用しているため、多くの場合、一連のメソッド呼び出しに 1 つのステートメントを組み合わせるに"fluent")。

このチュートリアルでは、属性を使用して行うことはできませんデータベース マッピングに対してのみ fluent API を使用します。 ただし、ほとんどの書式、検証、および属性を使用して行うことができますマッピング規則を指定するのに fluent API を使用することもできます。 などのいくつかの属性`MinimumLength`fluent API では適用できません。 前に述べたよう`MinimumLength`スキーマを変更しないクライアントとサーバー側の検証規則のみ適用されます

"クリーンな状態です"そのエンティティ クラスを維持できるように排他的 fluent API の使用を好む開発者 をしたいが存在している fluent API を使用してのみ実行できるいくつかのカスタマイズ属性および fluent API が混在することができますが、一般に推奨これら 2 つの方法のいずれかを選択し、使用可能な一貫している限りです。

モデルのデータに新しいエンティティおよび属性を使用して実行していないデータベース マッピングの実行を追加するコードで置き換え*DAL\SchoolContext.cs*を次のコード。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample29.cs)]

新しいステートメント、 [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx)メソッドは、多対多の結合テーブルを構成します。

- 多対多のリレーションシップ、`Instructor`と`Course`エンティティ、コードが、結合テーブルのテーブルと列の名前を指定します。 コード最初に構成できる多対多リレーションシップをこのコードを使用せずがしないで呼び出す場合、表示される既定の名前など`InstructorInstructorID`の`InstructorID`列です。

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample30.cs)]

次のコードを例示する可能性がありますが使用する方法の fluent API 属性ではなく間のリレーションシップを指定する、`Instructor`と`OfficeAssignment`エンティティ。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample31.cs)]

詳細についてはどのような"fluent API"ステートメントはバック グラウンドで、次を参照してください。、 [Fluent API](https://blogs.msdn.com/b/aspnetue/archive/2011/05/04/entity-framework-code-first-tutorial-supplement-what-is-going-on-in-a-fluent-api-call.aspx)ブログの投稿。

## <a name="seed-the-database-with-test-data"></a>データベースにテスト データをシードします。

コードで置き換え、 *Migrations\Configuration.cs*を作成した新しいエンティティのシードにデータを提供するために次のコード ファイル。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample32.cs)]

最初のチュートリアルで説明したとおりこのコードの大部分を単に更新または新しいエンティティ オブジェクトを作成し、プロパティをテストするために必要に応じてにサンプル データを読み込みます。 ただし、方法、`Course`を多対多のリレーションシップを持つエンティティで、`Instructor`エンティティの処理します。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample33.cs)]

作成するときに、`Course`オブジェクトを初期化する、`Instructors`ナビゲーション プロパティをコードを使用して空のコレクションとして`Instructors = new List<Instructor>()`です。 これにより、追加すること`Instructor`これに関連付けられているエンティティ`Course`を使用して、`Instructors.Add`メソッドです。 空のリストを作成していない場合はありませんできるこれらのリレーションシップを追加するため、`Instructors`プロパティが null では必要はありません、`Add`メソッドです。 コンス トラクターにリストの初期化を追加することもできます。

## <a name="add-a-migration-and-update-the-database"></a>移行を追加し、データベースの更新

PMC から次のように入力します。、`add-migration`コマンド。

`PM> add-Migration Chap4`

この時点で、データベースを更新しようとする場合、次のエラーが表示されます。

*ALTER TABLE ステートメントは、FOREIGN KEY 制約と競合して"FK\_dbo します。コース\_dbo します。部門\_DepartmentID"です。競合が発生したデータベース"ContosoUniversity"テーブル"dbo します。部門"、列 'DepartmentID' です。*

編集、 &lt;*タイムスタンプ&gt;\_Chap4.cs*ファイルを開き、次のコードを変更 (SQL ステートメントを追加および変更するが、`AddColumn`ステートメント)。

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample34.cs?highlight=14-18)]

(コメント アウトまたは、既存を削除することを確認してください`AddColumn`線の場合、新しいものを追加するか、または入力するときにエラーが表示されます、`update-database`コマンドです)。

既存のデータと移行を実行するときにスタブ データを外部キー制約を満たすためにデータベースに挿入する必要があります、何をしているようになりました。 生成されたコードを追加、null 非許容`DepartmentID`への外部キー、`Course`テーブル。 既に内の行がある場合、`Course`表に、コードを実行すると、 `AddColumn` SQL Server が null にすることはできません、列内の値を認識していないため操作は失敗します。 したがって、新しい列に、既定値を提供するコードを変更したし、既定の部門として機能するには、"Temp"という名前の明細部分を作成しました。 存在している場合、結果として`Course`行ときにこのコードが実行され、それらはすべてに関連する、"Temp"部門。

ときに、`Seed`メソッドを実行する内の行を挿入は、`Department`テーブルの既存の関連`Course`これら新しい行`Department`行です。 UI では、任意のコースを追加していない、し、不要になったが必要になります"Temp"部門または既定値で、`Course.DepartmentID`列です。 を、ユーザーが追加可能性がありますコース、アプリケーションを使用して発生する可能性を許可するのにはも更新する、`Seed`ことを確認するメソッドのコードすべて`Course`行 (の以前の実行によって挿入されたものだけでなく、`Seed`メソッド) が有効な`DepartmentID`値、既定値を削除する前に、列の値し、"Temp"部門を削除します。

編集が完了した後、 &lt;*タイムスタンプ&gt;\_Chap4.cs*ファイル、入力、 `update-database` PMC 移行を実行するコマンド。

> [!NOTE]
> データとスキーマの変更を移行するときにその他のエラーが発生することができます。 内の接続文字列を変更できますか、解決できない場合は移行エラーが発生した場合、 *Web.config*ファイルまたはデータベースを削除します。 データベースの名前を変更する最も簡単な方法は、 *Web.config*ファイル。 たとえば、CU にデータベース名を変更\_次に示すようにテストします。
> 
> [!code-xml[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample35.xml?highlight=1-2)]
> 
>  新しいデータベースを移行するデータがないと、`update-database`コマンドがエラーなしで完了する可能性が高くなります。 データベースを削除する方法については、次を参照してください。[を Visual Studio 2012 からデータベースを削除する方法](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/)です。


データベースを開く**サーバー エクスプ ローラー**前、展開すると、**テーブル**ノードをすべてのテーブルが作成されたことを確認します。 (がある場合は**サーバー エクスプ ローラー**以前の状態からを開き、、**更新**ボタンをクリックします)。

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image14.png)

モデル クラスを作成していない、`CourseInstructor`テーブル。 これには、多対多リレーションシップの間の結合テーブルを前述のように、`Instructor`と`Course`エンティティです。

右クリックし、`CourseInstructor`テーブルを選択して**テーブル データの表示**がの結果としてデータが入っていることを確認する、`Instructor`エンティティに追加した、`Course.Instructors`ナビゲーション プロパティ。

![Table_data_in_CourseInstructor_table](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image15.png)

## <a name="summary"></a>まとめ

複雑なデータ モデルと対応するデータベースがあるようになりました。 次のチュートリアルで学習関連のデータにアクセスするさまざまな方法の詳細。

その他の Entity Framework リソースへのリンクは含まれて、 [ASP.NET データ アクセス コンテンツ マップ](../../../../whitepapers/aspnet-data-access-content-map.md)です。

>[!div class="step-by-step"]
[前へ](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[次へ](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
