---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
title: "Entity Framework 4.0 ObjectDataSource コントロールを使用して、パート 1: 作業の開始 |Microsoft ドキュメント"
author: tdykstra
description: "この一連のチュートリアルについては、Entity Framework チュートリアル シリーズの概要を作成した Contoso 大学 web アプリケーションに基づいています。 場合 yo しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 244278c1-fec8-4255-8a8a-13bde491c4f5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
msc.type: authoredcontent
ms.openlocfilehash: 6f93d6033b68773507d624125936f0a69777e2b7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-1-getting-started"></a>Entity Framework 4.0 ObjectDataSource コントロールを使用して、パート 1: 作業の開始
====================
によって[Tom Dykstra](https://github.com/tdykstra)

> このチュートリアル シリーズのビルドによって作成される Contoso 大学 web アプリケーションで、 [Entity Framework 4.0 の概要](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md)一連のチュートリアルです。 前のチュートリアルを完了していない場合このチュートリアルの開始点とすることができます[アプリケーションをダウンロードして](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a)作成したとします。 こともできます[アプリケーションをダウンロードして](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa)一連の完全なチュートリアルで作成します。
> 
> Contoso 大学でサンプル web アプリケーションでは、Entity Framework 4.0 および Visual Studio 2010 を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。 サンプル アプリケーションは、架空の Contoso 大学の web サイトです。 学生受付、コースの作成、およびインストラクター割り当てなどの機能が含まれています。
> 
> チュートリアルでは、c# の例を示します。 [ダウンロード可能なサンプル](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa)c# および Visual Basic の両方でコードが含まれています。
> 
> ## <a name="database-first"></a>データベースの最初
> 
> Entity Framework でデータを操作できる 3 つの方法があります: *Database First*、 *Model First*、および*Code First*です。 このチュートリアルでは、データベースの最初のです。 シナリオに合った最適なものを選択する方法でこれらのワークフローとガイダンスの違いの詳細については、次を参照してください。 [Entity Framework 開発ワークフロー](https://msdn.microsoft.com/en-us/library/ms178359.aspx#dbfmfcf)です。
> 
> ## <a name="web-forms"></a>Web フォーム
> 
> 同様に、作業の開始シリーズ、このチュートリアルのシリーズ ASP.NET Web フォーム モデルを使用して Visual Studio での ASP.NET Web フォームを操作する方法を把握するという前提で。 表示されない場合、 [ASP.NET 4.5 Web フォームの概要](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md)です。 ASP.NET MVC フレームワークを使用する場合を参照してください。 [ASP.NET MVC を使用して Entity Framework の概要](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)です。
> 
> ## <a name="software-versions"></a>ソフトウェアのバージョン
> 
> | **このチュートリアルで示す** | **でも動作します。** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Visual Studio 2010 Express for Web です。 このチュートリアルは以降のバージョンの Visual Studio でテストされていません。 メニューのダイアログ ボックス、およびテンプレートの多くの違いがあります。 |
> | .NET 4 | .NET 4.5 は .NET 4 との下位互換性ですが、このチュートリアルは .NET 4.5 でテストされていません。 |
> | Entity Framework 4 | このチュートリアルは Entity Framework のバージョンでテストされていません。 EF は Entity Framework 5 以降では、既定では使用して、 `DbContext API` EF 4.1 で導入されたことです。 EntityDataSource コントロールを使用するように設計されました、 `ObjectContext` API です。 EntityDataSource を使用する方法については、コントロールを`DbContext`API を参照してください[このブログの投稿](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx)です。 |
> 
> ## <a name="questions"></a>質問
> 
> チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET Entity Framework フォーラム](https://forums.asp.net/1227.aspx)、 [Entity Framework でも LINQ to Entities フォーラム](https://social.msdn.microsoft.com/forums/en-US/adodotnetentityframework/threads/)、または[StackOverflow.com](http://stackoverflow.com/)です。


`EntityDataSource`コントロールでは、非常に短時間は、アプリケーションを作成することができますが、通常必要な膨大量のビジネス ロジックとデータ アクセス ロジックを保持すること、 *.aspx*ページ。 作成するために開発時間が増加が前もって投資できます複雑化し、継続的なメンテナンスを必要とするアプリケーションの場合、 *n 層*または*層*アプリケーション構造保守が簡単です。 このアーキテクチャを実装するのには、ビジネス ロジック層 (BLL) およびデータ アクセス層 (DAL) からプレゼンテーション層を分離します。 この構造体を実装する方法の 1 つを使用して、`ObjectDataSource`制御の代わりに、`EntityDataSource`コントロール。 使用すると、`ObjectDataSource`コントロール、データ アクセス コードを実装して、呼び出すことで*.aspx*ページが多くの同じコントロールを使用して他のデータ ソース コントロールの機能です。 これにより、n 層のアプローチの利点をデータにアクセスする Web フォーム コントロールを使用する利点と組み合わせるできます。

`ObjectDataSource`制御することをより柔軟での他の方法でも同様です。 独自のデータ アクセス コードを記述するためが容易になりますは単読み取り、挿入、更新、または特定のエンティティの種類を削除、タスクは、これを`EntityDataSource`コントロールを実行するように設計されています。 たとえば、エンティティが更新されるたびにログ記録を実行、エンティティを削除すると、または自動的に確認および更新に関連するデータ外部キーの値を持つ行を挿入するときに必要なときにデータをアーカイブできます。

## <a name="business-logic-and-repository-classes"></a>ビジネス ロジック、およびリポジトリ クラス

`ObjectDataSource`を作成するクラスを呼び出すことによって動作を制御します。 クラスには、取得、データを更新するメソッドが含まれます。 および、これらのメソッドの名前を指定する、`ObjectDataSource`コントロールをマークアップします。 レンダリングまたはポストバックの処理中に、`ObjectDataSource`指定したメソッドを呼び出します。

基本的な CRUD 操作で使用するために作成するクラスだけでなく、`ObjectDataSource`コントロールは、ビジネス ロジックを実行する必要があるときに、`ObjectDataSource`の読み取りやデータを更新します。 たとえば、部門を更新するときに、他の部署があるない同じ管理者 1 人のユーザーが 1 つ以上の部門の管理者にすることはできませんのでを検証する必要があります。

一部の`ObjectDataSource`ドキュメントについてなど、 [ObjectDataSource クラスの概要](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.aspx)と呼ばれるクラスを呼び出し、コントロール、*ビジネス オブジェクト*ビジネス ロジックとデータ アクセス ロジックの両方が含まれています. このチュートリアルでは、ビジネス ロジックとデータ アクセス ロジックの別々 のクラスを作成します。 データ アクセス ロジックをカプセル化するクラスが呼び出されます、*リポジトリ*です。 ビジネス ロジックのクラスには、ビジネス ロジックのメソッドとデータ アクセス メソッドの両方が含まれていますが、データ アクセス メソッドを呼び出すデータ アクセス タスクを実行するリポジトリです。

BLL と容易に自動化された単体 DAL の間で抽象化レイヤーを作成することも、BLL をテストします。 この抽象化レイヤーは、インターフェイスを作成して、ビジネス ロジックのクラスでリポジトリをインスタンス化するときに、インターフェイスを使用して実装されます。 これによりリポジトリ インターフェイスを実装する任意のオブジェクトへの参照でビジネス ロジックのクラスを提供しています。 通常の操作に対しては、Entity Framework で動作するリポジトリ オブジェクトを提供します。 テストでは、に対しては、簡単に操作できる、コレクションとして定義されているクラス変数などの方法で格納されているデータで動作するリポジトリ オブジェクトを提供します。

次の図は、リポジトリなしのデータ アクセス ロジックを含んでいるビジネス ロジック クラスとリポジトリを使用する 1 つの違いを示します。

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image1.png)

Web ページを作成するのには、まず、`ObjectDataSource`基本的なデータ アクセス タスクのみが実行するために、リポジトリに直接コントロールがバインドされています。 次のチュートリアルでは、検証ロジックとビジネス ロジックのクラスを作成し、バインド、`ObjectDataSource`リポジトリ クラスの代わりにそのクラスへのコントロールです。 検証ロジックの単体テストを作成します。 このシリーズの第 3 のチュートリアルでは、並べ替えおよびフィルター処理をアプリケーションに機能を追加します。

このチュートリアルで作成するページを使用、`Departments`で作成したデータ モデルのエンティティ セット、[チュートリアル シリーズの作業の開始](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md)です。

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image3.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image5.png)

## <a name="updating-the-database-and-the-data-model"></a>データベースとデータ モデルの更新

まずこのチュートリアルで作成したデータ モデルに対応する変更を必要とする、データベースに 2 つの変更を加える、 [Entity Framework と Web フォームの概要](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md)チュートリアルです。 これらのチュートリアルの 1 つは、データベースの変更後に、データベースとデータ モデルを同期するために手動で、デザイナーで変更を加えた。 このチュートリアルでは、デザイナーを使用して**モデルをデータベースから更新**データ モデルを自動的に更新するツールです。

### <a name="adding-a-relationship-to-the-database"></a>データベースへのリレーションシップの追加

Visual studio で作成した Contoso 大学 web アプリケーションを開きます、 [Entity Framework と Web フォームの概要](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md)チュートリアル シリーズを開き、`SchoolDiagram`データベース ダイアグラム。

確認する場合、`Department`テーブル データベース ダイアグラムで表示されますがある、`Administrator`列です。 この列は外部キーを`Person`テーブルがない外部キー リレーションシップがデータベースで定義されています。 リレーションシップを作成して、Entity Framework はこのリレーションシップを自動的に処理できるように、データ モデルを更新する必要があります。

データベース ダイアグラムを右クリックし、 `Department` 、テーブルを選択して**リレーションシップ**です。

[![Image80](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image7.png)

**Foreign Key Relationships**ボックス**追加**、... ボタンをクリックして**テーブルと列の指定**です。

[![Image81](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image9.png)

**テーブルと列** ダイアログ ボックスで、主キー テーブルを設定し、フィールドを`Person`と`PersonID`、および外部キー テーブルを設定し、フィールドを`Department`と`Administrator`です。 (これを行うと、リレーションシップの名前がから変更`FK_Department_Department`に`FK_Department_Person`)。

[![Image82](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image11.png)

をクリックして**OK**で、**テーブルと列**ボックスで、をクリックして**閉じる**で、 **Foreign Key Relationships**ボックス、および変更を保存します。 保存するかどうかを要求している場合、`Person`と`Department`テーブルをクリックして**はい**です。

> [!NOTE]
> 削除した場合は`Person`は既にデータに対応する行、`Administrator`列で、この変更を保存できません。 その場合は、エディターを使用して、テーブルで**サーバー エクスプ ローラー**ことを確認する、`Administrator`内の値すべて`Department`行にはで実際に存在するレコードの ID が含まれています、`Person`テーブル。
> 
> 行を削除できませんする変更を保存した後、`Person`表にその人が部門の管理者である場合。 データベースの制約により、削除、またはカスケード delete を指定する場合、実稼働アプリケーションで特定のエラー メッセージが提供します。 連鎖削除を指定する方法の例は、次を参照してください。 [Entity Framework と ASP.NET – の取得開始第 2 部](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2.md)です。


### <a name="adding-a-view-to-the-database"></a>データベースにビューの追加

新しい*Departments.aspx*部門の管理者をユーザーが選択できるように、「姓、名」の形式で名前を持つ講習においてインストラクターのドロップダウン リストを指定する場合を作成する ページで、します。 やすくを行うには、データベースのビューが作成されます。 ビューは、ドロップダウン リストで必要なデータだけで構成されます。 (適切な形式)、完全な名前と、レコードのキー。

**サーバー エクスプ ローラー**、展開*School.mdf*を右クリックし、**ビュー**フォルダー、および選択**新しいビューの追加**です。

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image13.png)

をクリックして**閉じる**ときに、**テーブルの追加** ダイアログ ボックスが表示され、SQL ペインに次の SQL ステートメントを貼り付けます。

[!code-sql[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample1.sql)]

してビューを保存`vInstructorName`です。

### <a name="updating-the-data-model"></a>データ モデルを更新します。

*DAL*フォルダーを開き、 *SchoolModel.edmx*ファイル、デザイン サーフェイスを右クリックし **データベースからモデルを更新**です。

[![Image07](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image15.png)

**データベース オブジェクトの選択**ダイアログ ボックスで、**追加** タブを作成したビューを選択します。

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image17.png)

**[完了]**をクリックします。

ツールが作成されたことが表示されて、デザイナーで、`vInstructorName`エンティティとの関連付けを新しく、`Department`と`Person`エンティティです。

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image19.png)

> [!NOTE]
> **出力**と**エラー一覧**ツールが、プライマリを自動的に作成されたことを通知する警告メッセージが表示される windows の新しいキーの`vInstructorName`ビュー。 これは通常の動作です。


新しいを参照するとき`vInstructorName`コードのエンティティ、たくないに小文字"v"を付けることのデータベースの規則を使用します。 そのため、エンティティと、モデル内のエンティティ セットを変更するされます。

開く、**モデル ブラウザー**です。 表示`vInstructorName`エンティティ型と、ビューとして一覧表示します。

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image21.png)

下にある**SchoolModel** (いない**SchoolModel.Store**) を右クリックして**vInstructorName**選択と**プロパティ**です。 **プロパティ**ウィンドウで、変更、**名前**"InstructorName"および変更するプロパティ、**エンティティ セット名**プロパティを"InstructorNames"です。

[![Image15](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image24.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image23.png)

保存と、データ モデルを終了し、プロジェクトをリビルドします。

## <a name="using-a-repository-class-and-an-objectdatasource-control"></a>リポジトリのクラスと、ObjectDataSource コントロールを使用してください。

新しいクラス ファイルを作成、 *DAL*フォルダーで、名前を付けます*SchoolRepository.cs*、し、既存のコードを次のコードに置き換えます。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample2.cs)]

このコードは、1 つ`GetDepartments`内のエンティティのすべてを返すメソッド、`Departments`エンティティ セット。 アクセスすることがわかっているため、`Person`行ごとにナビゲーション プロパティが返されますを指定する一括を使用してそのプロパティの読み込み、`Include`メソッドです。 クラスを実装しても、`IDisposable`インターフェイスをオブジェクトが破棄されるときに、データベース接続が解放されることを確認します。

> [!NOTE]
> 一般的では、各エンティティ タイプのリポジトリ クラスを作成します。 このチュートリアルでは、複数のエンティティ型の 1 つのリポジトリ クラスを使用します。 リポジトリ パターンの詳細については、内の投稿を参照してください。 [Entity Framework チームのブログ](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)と[Julie Lerman ブログ](http://thedatafarm.com/blog/data-access/agile-ef4-repository-part-3-fine-tuning-the-repository/)です。


`GetDepartments`メソッドを返します、`IEnumerable`オブジェクトではなく、`IQueryable`リポジトリ オブジェクト自体が破棄された後でも、返されるコレクションが使用可能なことを確認するためにオブジェクト。 `IQueryable`オブジェクトが、アクセスされるときに、データ バインディング コントロールのデータをレンダリングしようとしました。 時間によって、リポジトリ オブジェクトを破棄する場合がありますにデータベースへのアクセス可能性があります。 などの他のコレクション型を返す可能性があります、`IList`オブジェクトの代わりに、`IEnumerable`オブジェクト。 ただしを返す、`IEnumerable`オブジェクトにより、処理タスクの標準的な読み取り専用のリストをなど、実行できる`foreach`ループと LINQ クエリことはできません追加または削除するアイテム、コレクションは、このような変更であることを示唆内データベースに保存されます。

作成、 *Departments.aspx*を使用するページ、 *Site.Master*マスター ページとでは、次のマークアップを追加、`Content`という名前のコントロール`Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample3.aspx)]

このマークアップを作成、 `ObjectDataSource` 、作成したリポジトリ クラスを使用するコントロールと`GridView`コントロールにデータを表示します。 `GridView`コントロールを指定**編集**と**削除**コマンドですが、まだそれらをサポートするためのコードが追加されていません。

複数の列を使用して`DynamicField`自動のデータの書式設定と検証の機能の利用を制御します。 呼び出す必要があるこれらの場合、`EnableDynamicData`メソッドで、`Page_Init`イベント ハンドラー。 (`DynamicControl`でコントロールが使用されていない、`Administrator`ナビゲーションのプロパティを使用しないためにのフィールドです)。

`Vertical-Align="Top"`属性重要になる後では、入れ子になった列を追加するときに`GridView`コントロールはグリッドにします。

開く、 *Departments.aspx.cs*ファイルし、次の追加`using`ステートメント。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample4.cs)]

次のハンドラーを追加、ページの`Init`イベント。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample5.cs)]

*DAL*フォルダー、という名前の新しいクラス ファイルを作成する*Department.cs*し、既存のコードを次のコードに置き換えます。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample6.cs)]

このコードは、データ モデルにメタデータを追加します。 指定する、`Budget`のプロパティ、`Department`エンティティは、そのデータ型が実際には通貨を表す`Decimal`であり、値は 0 ~ $1,000,000.00 間ありますを指定します。 指定する、`StartDate`プロパティ形式年/月/日の日付として書式設定する必要があります。

実行、 *Departments.aspx*ページ。

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image26.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image25.png)

書式指定文字列を指定しなかったことに注意して、 *Departments.aspx*ページ マークアップを**予算**または**Start Date**列、既定の通貨、日付書式設定が適用されたことによって、`DynamicField`をコントロールに提供されているメタデータを使用して、 *Department.cs*ファイル。

## <a name="adding-insert-and-delete-functionality"></a>追加の Insert と Delete の機能

開いている*SchoolRepository.cs*、作成するために次のコードを追加、`Insert`メソッドおよび`Delete`メソッドです。 コードは、という名前のメソッドも含まれています。`GenerateDepartmentID`で使用するための次の使用可能なレコード キー値を計算する、`Insert`メソッドです。 これは、これを自動的に計算する、データベースが構成されていないために必要な`Department`テーブル。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample7.cs)]

### <a name="the-attach-method"></a>アタッチする方法

`DeleteDepartment`メソッドの呼び出し、`Attach`メソッド、オブジェクト コンテキストのオブジェクト状態マネージャー メモリ内のエンティティとデータベース間で保持されているリンクを再確立するために、行を表します。 これは、メソッドの呼び出しの前に発生する必要があります、`SaveChanges`メソッドです。

用語*オブジェクト コンテキスト*から派生した Entity Framework クラスを参照、`ObjectContext`エンティティ セットおよびエンティティへのアクセスに使用するクラスです。 クラスの名前は、このプロジェクトのコードで`SchoolEntities`でそのインスタンスの名前が常に`context`です。 オブジェクト コンテキストの*オブジェクト状態マネージャー*から派生したクラスには、`ObjectStateManager`クラスです。 オブジェクトの連絡先は、エンティティ オブジェクトを格納して、それぞれが、対応するテーブルの行またはデータベース内の行と同期するかどうかを追跡するオブジェクト状態マネージャーを使用します。

エンティティを読み取るときにオブジェクト コンテキストはオブジェクト状態マネージャーに格納し、そのオブジェクトの表現は、データベースと同期されているかどうかの追跡します。 たとえば、プロパティ値を変更する場合はフラグがプロパティを変更するが、データベースとの同期が不要になったことを示す設定します。 その後呼び出す、`SaveChanges`メソッド、オブジェクト コンテキストでは、オブジェクトの状態マネージャーは、エンティティの現在の状態と、データベースの状態の間で相違点は正確に認識しているため、データベースを認識しています。

ただし、このプロセス通常機能しません、web アプリケーションで、ページが表示されたら、と共にそのオブジェクトの状態マネージャー内のすべてのエンティティの読み取りオブジェクト コンテキストのインスタンスが破棄されるためです。 変更を適用する必要がありますオブジェクト コンテキストのインスタンスは、新しいポストバック処理のためにインスタンス化されます。 場合、 `DeleteDepartment` 、メソッド、`ObjectDataSource`コントロール再する自動的に作成されたエンティティの元のバージョンをビュー ステートの値からが再作成これ`Department`オブジェクト状態マネージャーにエンティティが存在しません。 呼び出した場合、`DeleteObject`再作成このエンティティに対するメソッドの呼び出しは失敗オブジェクト コンテキストは、エンティティがデータベースと同期するかどうかを把握していないためです。 ただし、呼び出し、`Attach`メソッドでは、データベース エンティティがオブジェクト コンテキストの以前のインスタンスで読み取られたときに自動的に行われた最初に、再作成されたエンティティと値の間で同じトラッキングを再確立します。

オブジェクトの状態マネージャー内のエンティティを追跡するためにオブジェクト コンテキストをしたくないし、これを行うようにするためのフラグを設定することができます。 この例は、このシリーズの後のチュートリアルに表示されます。

### <a name="the-savechanges-method"></a>SaveChanges メソッド

この単純なリポジトリ クラスは、CRUD 操作を実行する方法の基本原則を示しています。 この例では、`SaveChanges`メソッドは、各更新プログラムの直後に呼び出されます。 実稼働アプリケーションでを呼び出したい場合があります、`SaveChanges`データベースが更新されたときより詳細に制御することを別個のメソッドからのメソッドです。 (次のチュートリアルの最後に関連する更新プログラムを調整する方法の 1 つである作業パターンの単位に記載されているホワイト ペーパーへのリンクが検索されます)。注意しても、例では、`DeleteDepartment`メソッドに同時実行の競合を処理するためのコードが含まれていない; このシリーズの後のチュートリアルを行うにはコードは追加されます。

### <a name="retrieving-instructor-names-to-select-when-inserting"></a>挿入するときに選択するインストラクターの名前を取得します。

ユーザーは、新しい部門を作成するときに、講習においてインストラクターにドロップダウン リストの一覧から管理者を選択できる必要があります。 したがって、次のコードを追加*SchoolRepository.cs*講習においてインストラクター先ほど作成したビューを使用しての一覧を取得するメソッドを作成します。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample8.cs)]

### <a name="creating-a-page-for-inserting-departments"></a>部門を挿入するためページを作成します。

作成、 *DepartmentsAdd.aspx*を使用するページ、 *Site.Master*ページとでは、次のマークアップを追加、`Content`という名前のコントロール`Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample9.aspx)]

このマークアップでは、2 つ作成されます`ObjectDataSource`には、新しいを挿入するための 1`Department`エンティティとインストラクター名を取得する 1 つずつ、`DropDownList`部門の管理者を選択するために使用されるコントロールです。 マークアップは、作成、`DetailsView`コントロールのハンドラーを指定する新しい部門を入力用の変更`ItemInserting`イベントを設定できるように、`Administrator`外部キーの値。 最後には、`ValidationSummary`エラー メッセージを表示するコントロール。

開いている*DepartmentsAdd.aspx.cs*し、以下の追加`using`ステートメント。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample10.cs)]

次のクラスの変数とメソッドを追加します。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample11.cs)]

`Page_Init`メソッド Dynamic Data 機能を有効にします。 ハンドラーを`DropDownList`コントロールの`Init`イベントのハンドラーは、コントロールへの参照を保存する、`DetailsView`コントロールの`Inserting`イベントでは、その参照を使用して、取得、`PersonID`選択した講師と更新プログラムの値`Administrator`の外部キーのプロパティ、`Department`エンティティです。

ページの実行、新しい部門の情報を追加し、クリックして、**挿入**リンクします。

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image28.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image27.png)

別の新しい部門の値を入力します。 1,000,000.00 より大きい数値を入力、**予算**フィールドとタブでは、次のフィールドをします。 フィールドに、アスタリスクが表示され、上にマウス ポインターを保持する場合は、そのフィールドのメタデータで指定したエラー メッセージを表示できます。

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image30.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image29.png)

をクリックして**挿入**、によって表示されるエラー メッセージを表示して、`ValidationSummary`ページの下部にあるコントロールです。

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image32.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image31.png)

次に、ブラウザーを閉じてから開き、 *Departments.aspx*ページ。 削除機能を追加、 *Departments.aspx*ページを追加することによって、`DeleteMethod`属性を`ObjectDataSource`コントロール、および`DataKeyNames`属性を`GridView`コントロール。 これらのコントロールの開始タグは、次の例のようになります。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample12.aspx)]

ページを実行します。

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image34.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image33.png)

削除を実行したときに追加した、部門、 *DepartmentsAdd.aspx*ページ。

## <a name="adding-update-functionality"></a>更新機能を追加します。

開いている*SchoolRepository.cs*し、以下の追加`Update`メソッド。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample13.cs)]

クリックすると**更新**で、 *Departments.aspx*  ページで、`ObjectDataSource`コントロールでは、2 つ作成されます`Department`エンティティに渡す、`UpdateDepartment`メソッドです。 ビュー ステートに格納されている元の値を含む 1 つと、入力した新しい値を含む、他の`GridView`コントロール。 内のコード、`UpdateDepartment`メソッドは、`Department`に元の値を持つエンティティ、`Attach`エンティティと、データベースの間の追跡を確立するためのメソッドです。 コードに渡す、`Department`に新しい値を持つエンティティ、`ApplyCurrentValues`メソッドです。 オブジェクト コンテキストでは、新旧の値を比較します。 新しい値が古い値と異なる場合は、オブジェクト コンテキストは、プロパティ値を変更します。 `SaveChanges`メソッドは、データベースで変更された列のみを更新します。 (ただし、このエンティティの update 関数は、ストアド プロシージャにマップされた場合、行全体が更新されますに関係なく列が変更された。)

開く、 *Departments.aspx*ファイルし、次の属性を追加、`DepartmentsObjectDataSource`コントロール。

- `UpdateMethod="UpdateDepartment"`
- `ConflictDetection="CompareAllValues"`   
 この原因が古い値を格納されているビュー内の状態の新しい値を比較できるように、`Update`メソッドです。
- `OldValuesParameterFormatString="orig{0}"`   
 これを元の値パラメーターの名前はコントロールに知らせます`origDepartment`です。

開始タグのマークアップ、`ObjectDataSource`コントロール次の例のようになります。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample14.aspx)]

追加、`OnRowUpdating="DepartmentsGridView_RowUpdating"`属性を`GridView`コントロール。 この設定に使用する、`Administrator`プロパティの値は、ユーザーがドロップダウン リストで選択行に基づきます。 `GridView`開始タグを今すぐには、次の例と似ています。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample15.aspx)]

追加、`EditItemTemplate`の制御、`Administrator`列を`GridView`制御直後、`ItemTemplate`その列の制御。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample16.aspx)]

これは、`EditItemTemplate`コントロールに似ていますが、`InsertItemTemplate`内の制御、 *DepartmentsAdd.aspx*ページ。 違いを使用して、コントロールの初期値を設定すること、`SelectedValue`属性。

前に、`GridView`コントロールを追加、`ValidationSummary`制御と同じにする、 *DepartmentsAdd.aspx*ページ。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample17.aspx)]

開いている*Departments.aspx.cs*部分クラス宣言の直後後を参照するプライベート フィールドを作成する次のコードを追加し、`DropDownList`コントロール。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample18.cs)]

ハンドラーを追加、`DropDownList`コントロールの`Init`イベントおよび`GridView`コントロールの`RowUpdating`イベント。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample19.cs)]

ハンドラーを`Init`イベントへの参照を保存する、`DropDownList`クラス フィールド内のコントロールです。 ハンドラーを`RowUpdating`イベントでは、参照を使用して、ユーザーが入力した値を取得し、それを適用を`Administrator`のプロパティ、`Department`エンティティです。

使用して、 *DepartmentsAdd.aspx*新しい部門を追加するページし、実行、 *Departments.aspx*ページし、をクリックして**編集**追加した行にします。

> [!NOTE]
> 追加していない行を編集することはできません (つまり、データベース内に既に)、データベースに無効なデータによりデータベースで作成された行の管理者は、受講者です。 これらのいずれかを編集しようとする場合のようなエラーが報告されるエラー ページが表示されます。`'InstructorsDropDownList' has a SelectedValue which is invalid because it does not exist in the list of items.`


[![Image10](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image36.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image35.png)

場合は、無効な入力**予算**amount し、をクリックして**更新**、同じアスタリスクとで示したエラー メッセージを参照してください、 *Departments.aspx*ページ。

フィールド値を変更または別の管理者を選択し、クリックして**更新**です。 変更が表示されます。

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image38.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image37.png)

これで完了使用の概要、`ObjectDataSource`コントロールの基本的な CRUD (作成、読み取り、更新、削除)、Entity Framework での操作です。 簡単な n 層アプリケーションを構築しましたが、ビジネス ロジック層は引き続き緊密に自動化された単体テストを複雑になるデータ アクセス層にします。 次のチュートリアルでは、単体テストを容易にするために、リポジトリ パターンを実装する方法が表示されます。

>[!div class="step-by-step"]
[次へ](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
