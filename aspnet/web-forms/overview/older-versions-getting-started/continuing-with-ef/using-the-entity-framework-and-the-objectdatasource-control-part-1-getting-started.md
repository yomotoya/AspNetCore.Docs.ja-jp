---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
title: 'Entity Framework 4.0 と ObjectDataSource コントロールを使用して、パート 1: 作業の開始 |Microsoft Docs'
author: tdykstra
description: このチュートリアル シリーズでは、Entity Framework のチュートリアル シリーズの概要を作成した Contoso University web アプリケーションに基づいています。 Yo 場合.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 244278c1-fec8-4255-8a8a-13bde491c4f5
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
msc.type: authoredcontent
ms.openlocfilehash: ba3b65dededeca3534b9273bfd3ae48429711fce
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369759"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-1-getting-started"></a>Entity Framework 4.0 と ObjectDataSource コントロールを使用して、パート 1: 作業の開始
====================
によって[Tom Dykstra](https://github.com/tdykstra)

> このチュートリアル シリーズは、Contoso University web アプリケーションによって作成される、 [、Entity Framework 4.0 の概要](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md)チュートリアル シリーズです。 前のチュートリアルを完了していない場合は、このチュートリアルの開始点としてできます[アプリケーションをダウンロードする](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a)に、作成します。 できます[アプリケーションをダウンロードする](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa)完全なチュートリアル シリーズで作成します。
> 
> Contoso University のサンプルの web アプリケーションでは、Entity Framework 4.0 と Visual Studio 2010 を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。 サンプル アプリケーションは、架空の Contoso University の web サイトです。 学生の受け付け、講座の作成、講師の割り当てなどの機能が含まれています。
> 
> このチュートリアルでは、c# で例を示します。 [ダウンロード可能なサンプル](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa)c# および Visual Basic の両方でコードが含まれています。
> 
> ## <a name="database-first"></a>最初のデータベースします。
> 
> Entity Framework 内のデータを使用する 3 つの方法があります: *Database First*、 *Model First*、および*Code First*します。 このチュートリアルでは、データベースの最初の。 シナリオに最適なものを選択する方法に関するこれらのワークフローとガイダンスの違いについては、次を参照してください。 [Entity Framework 開発ワークフロー](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf)します。
> 
> ## <a name="web-forms"></a>Web フォーム
> 
> Getting Started シリーズなどは、このチュートリアル シリーズは、ASP.NET Web フォーム モデルを使用して、Visual Studio で ASP.NET Web フォームを操作する方法を把握するという前提で。 表示されない場合、 [ASP.NET 4.5 Web フォームの概要](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md)します。 ASP.NET MVC フレームワークで作業する場合を参照してください。 [ASP.NET MVC を使用して Entity Framework の概要](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)します。
> 
> ## <a name="software-versions"></a>ソフトウェアのバージョン
> 
> | **このチュートリアルで示すように** | **でも使用できます。** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Visual Studio 2010 Express for Web です。 このチュートリアルは以降のバージョンの Visual Studio でテストされていません。 メニューのダイアログ ボックス、およびテンプレートで多くの違いがあります。 |
> | .NET 4 | .NET 4.5 は .NET 4 では、旧バージョンと互換性のあるが、このチュートリアルは .NET 4.5 でテストされていません。 |
> | Entity Framework 4 | このチュートリアルは Entity Framework の以降のバージョンでテストされていません。 既定では、Entity Framework 5 以降では、EF で使用、`DbContext API`は EF 4.1 で導入されました。 EntityDataSource コントロールが使用するように設計、 `ObjectContext` API。 コントロールを EntityDataSource を使用する方法については、 `DbContext` API を参照してください[このブログの投稿](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx)します。 |
> 
> ## <a name="questions"></a>質問
> 
> チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET Entity Framework フォーラム](https://forums.asp.net/1227.aspx)、 [Entity Framework と LINQ to エンティティ フォーラム](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/)、または[StackOverflow.com](http://stackoverflow.com/)します。


`EntityDataSource`コントロールでは、非常に高速にアプリケーションを作成することができますが、通常必要膨大なビジネス ロジックとデータ アクセス ロジックを保持すること、 *.aspx*ページ。 作成するには多くの開発時間が事前投資できます、アプリケーションで複雑化し、継続的なメンテナンスを必要とする場合は、 *n 層*または*層*アプリケーション構造これは、保守しやすくなります。 このアーキテクチャを実装するには、ビジネス ロジック層 (BLL) とデータ アクセス層 (DAL) からプレゼンテーション層を分離します。 この構造体を実装する方法の 1 つが使用するには、`ObjectDataSource`コントロールの代わりに、`EntityDataSource`コントロール。 使用すると、`ObjectDataSource`コントロール、データ アクセス コードを実装しでを呼び出す *.aspx*ページが多くの同じコントロールを使用するその他のデータ ソース コントロールの機能です。 これにより、n 層のアプローチの利点とデータ アクセスの Web フォーム コントロールを使用する利点を組み合わせることができます。

`ObjectDataSource`コントロールより柔軟で他の方法でも同様です。 独自のデータ アクセス コードを記述するためがしやすくは以上の読み取り、挿入、更新、または特定のエンティティの種類を削除は、タスクを`EntityDataSource`コントロールを実行するように設計します。 たとえば、エンティティが更新されるたびにログ記録を実行、エンティティを削除すると、または自動的に確認および更新を関連するデータの外部キー値を持つ行を挿入するときに、必要に応じてたびにデータをアーカイブできます。

## <a name="business-logic-and-repository-classes"></a>ビジネス ロジック、およびリポジトリ クラス

`ObjectDataSource`作成するクラスを呼び出すことによって動作を制御します。 クラスには取得し、データを更新するメソッドが含まれています、これらのメソッドの名前を指定して、`ObjectDataSource`マークアップ内のコントロール。 レンダリングまたはポストバックの処理中に、`ObjectDataSource`指定したメソッドを呼び出します。

基本的な CRUD 操作、クラスで使用するために作成するだけでなく、`ObjectDataSource`コントロールは、ビジネス ロジックを実行する必要があるときに、`ObjectDataSource`の読み取りやデータを更新します。 たとえば、部署を更新するときに、1 人のユーザーが 1 つ以上の部門の管理者にすることはできませんので、他の部門が、同じ管理者ありませんを検証する必要があります。

一部の`ObjectDataSource`ドキュメントについてなど、 [ObjectDataSource クラスの概要](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.aspx)、コントロールと呼ばれるクラスを呼び出し、*ビジネス オブジェクト*ビジネス ロジックとデータ アクセス ロジックの両方を含む. このチュートリアルでは、ビジネス ロジックとデータ アクセス ロジックの別のクラスを作成します。 データ アクセス ロジックをカプセル化するクラスと呼ばれる、*リポジトリ*します。 ビジネス ロジック クラスには、ビジネス ロジックのメソッドとデータ アクセス メソッドの両方が含まれていますが、データ アクセス メソッドは、データ アクセス タスクを実行するリポジトリを呼び出します。

BLL と自動化された単体を容易にする DAL の抽象化レイヤーを作成することも、BLL のテストします。 この抽象化レイヤーを実装するには、インターフェイスを作成し、ビジネス ロジック クラスでリポジトリをインスタンス化するときに、インターフェイスを使用します。 これにより、リポジトリ インターフェイスを実装する任意のオブジェクトへの参照にビジネス ロジック クラスを提供することが可能にします。 通常の操作の場合は、Entity Framework で動作するリポジトリ オブジェクトを提供します。 テストするには、コレクションとして定義されているクラス変数など、操作に簡単にする方法で格納されているデータで動作するリポジトリ オブジェクトを提供します。

次の図は、リポジトリなしのデータ アクセス ロジックを含むビジネス ロジック クラスと、リポジトリを使用する 1 つの違いを示します。

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image1.png)

Web ページの作成は、まず、`ObjectDataSource`基本的なデータ アクセス タスクのみを実行するために、リポジトリに直接コントロールがバインドされています。 次のチュートリアルでは、検証ロジックとビジネス ロジック クラスを作成し、バインド、`ObjectDataSource`リポジトリ クラスの代わりにそのクラスへのコントロール。 検証ロジックの単体テストも作成されます。 このシリーズの 3 番目のチュートリアルでは、並べ替えおよびフィルター処理、アプリケーションに機能を追加します。

このチュートリアルで作成したページを使用、`Departments`で作成したデータ モデルのエンティティ セット、[入門チュートリアル シリーズ](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md)します。

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image3.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image5.png)

## <a name="updating-the-database-and-the-data-model"></a>データベースとデータ モデルを更新しています

作成したデータ モデルに対応する変更を必要とする、データベースに 2 つの変更を加えて、このチュートリアルを開始するが、 [Entity Framework と Web フォーム概要](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md)チュートリアル。 これらのチュートリアルの 1 つは、データベースを変更した後、データベースとデータ モデルを同期するには、手動でのデザイナーで変更を加えた。 このチュートリアルでは、デザイナーを使用して**データベースからモデルを更新**データ モデルを自動的に更新するためのツール。

### <a name="adding-a-relationship-to-the-database"></a>データベースへのリレーションシップの追加

Visual studio で作成した Contoso University web アプリケーションを開き、 [Entity Framework と Web フォーム概要](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md)チュートリアル シリーズでし、開きます、`SchoolDiagram`データベース ダイアグラム。

確認する場合、`Department`テーブル データベース ダイアグラムで表示されますが、`Administrator`列。 この列への外部キーには、`Person`テーブルがない外部キー リレーションシップが、データベースで定義されています。 リレーションシップを作成し、Entity Framework がこの関係を自動的に処理できるように、データ モデルを更新する必要があります。

データベース ダイアグラムを右クリックし、`Department`テーブル、および選択**リレーションシップ**します。

[![Image80](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image7.png)

**Foreign Key Relationships**ボックス**追加**の省略記号をクリックし、**テーブルと列の指定**します。

[![Image81](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image9.png)

**テーブルと列** ダイアログ ボックスで、主キー テーブルを設定し、フィールドを`Person`と`PersonID`、および外部キー テーブルを設定し、フィールドを`Department`と`Administrator`します。 (これを行うと、リレーションシップの名前がから変更`FK_Department_Department`に`FK_Department_Person`)。

[![Image82](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image11.png)

をクリックして**ok**で、**テーブルと列**ボックスで、**閉じる**で、 **Foreign Key Relationships**ボックス、および変更を保存します。 保存するかどうかを要求している場合、`Person`と`Department`テーブル、 をクリックして**はい**。

> [!NOTE]
> 削除してしまった場合`Person`は既にデータに対応する行、`Administrator`列で、この変更を保存できません。 その場合は、エディターでは、テーブルを使用して、**サーバー エクスプ ローラー**ことを確認する、`Administrator`値すべて`Department`行に実際に存在するレコードの ID が含まれています、`Person`テーブル。
> 
> 行を削除できませんが、変更を保存した後、`Person`テーブルの人物が部門の管理者である場合。 実稼働アプリケーションでは、データベースの制約により、削除、または連鎖削除を指定するときに特定のエラー メッセージを指定するは。 連鎖削除を指定する方法の例は、次を参照してください。 [。 Entity Framework と ASP.NET – 開始パート 2 を取得する](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2.md)します。


### <a name="adding-a-view-to-the-database"></a>データベースにビューを追加します。

新しい*Departments.aspx*部門の管理者をユーザーが選択できるように、「姓, 名」の形式で名前を持つ、インストラクターのドロップダウン リストを提供したいページを作成することで、します。 そのために容易にできるように、データベースでビューが作成されます。 ビューは、ドロップダウン リストで必要なデータだけで構成されます: (適切に書式設定) 完全な名前とレコードのキー。

**サーバー エクスプ ローラー**、展開*School.mdf*を右クリックし、**ビュー**フォルダー、および選択**新しいビューの追加**します。

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image13.png)

をクリックして**閉じる**ときに、**テーブルの追加** ダイアログ ボックスが表示され、次の SQL ステートメントを SQL ペインに貼り付けます。

[!code-sql[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample1.sql)]

ビューとして保存`vInstructorName`します。

### <a name="updating-the-data-model"></a>データ モデルを更新します。

*DAL*フォルダーを開き、 *SchoolModel.edmx*ファイルで、デザイン サーフェイスを右クリックし、選択**データベースからモデルを更新**します。

[![Image07](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image15.png)

**データベース オブジェクトの選択**ダイアログ ボックスで、**追加**タブし、作成したビューを選択します。

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image17.png)

**[完了]** をクリックします。

デザイナーで、表示、ツールが作成されたことを`vInstructorName`エンティティとの関連付けを新しく、`Department`と`Person`エンティティ。

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image19.png)

> [!NOTE]
> **出力**と**エラー一覧**ツールがプライマリに自動的に作成されたことを通知する警告メッセージが表示される windows の新しいキーの`vInstructorName`ビュー。 これは通常の動作です。


新しいを参照するとき`vInstructorName`コード内のエンティティ、たくに小文字"v"を付けることのデータベースの規則を使用します。 そのため、エンティティとモデルのエンティティ セットを変更します。

開く、**モデル ブラウザー**します。 表示`vInstructorName`としてエンティティ型とビューを一覧表示します。

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image21.png)

**SchoolModel** (いない**SchoolModel.Store**)、右クリック**vInstructorName**を選択し、**プロパティ**。 **プロパティ**ウィンドウで、変更、**名前**プロパティを"InstructorName"に変更、**エンティティ セット名**プロパティを"InstructorNames"。

[![Image15](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image24.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image23.png)

保存して、データ モデルを終了し、プロジェクトをリビルドします。

## <a name="using-a-repository-class-and-an-objectdatasource-control"></a>リポジトリのクラスと ObjectDataSource コントロールを使用してください。

新しいクラス ファイルを作成、 *DAL*フォルダー、名前を付けます*SchoolRepository.cs*、既存のコードを次のコードに置き換えます。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample2.cs)]

このコードにより、1 つ`GetDepartments`内のエンティティのすべてを返すメソッド、`Departments`エンティティ セット。 アクセスすることがわかっているため、`Person`ナビゲーション プロパティのすべての行に返されたを指定する一括を使用してそのプロパティの読み込み、`Include`メソッド。 クラスを実装しても、`IDisposable`オブジェクトが破棄されたときに、データベース接続が解放されるようにするインターフェイス。

> [!NOTE]
> 一般的な方法では、各エンティティ型のリポジトリ クラスを作成します。 このチュートリアルでは、複数のエンティティ型の 1 つのリポジトリ クラスが使用されます。 リポジトリ パターンの詳細については、内の投稿を参照してください。 [、Entity Framework チームのブログ](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)と[Julie Lerman のブログ](http://thedatafarm.com/blog/data-access/agile-ef4-repository-part-3-fine-tuning-the-repository/)します。


`GetDepartments`メソッドを返します、`IEnumerable`オブジェクトではなく、`IQueryable`リポジトリ オブジェクト自体が破棄された後でも、返されたコレクションが使用可能なことを確認するにはオブジェクトです。 `IQueryable`オブジェクトによってデータベース アクセス、アクセスはなく、リポジトリ オブジェクトは、データ バインド コントロールが、データをレンダリングしようとしています。 時間によって破棄される可能性があります。 などの別の種類のコレクションを返すことができます、`IList`オブジェクトの代わりに、`IEnumerable`オブジェクト。 ただし、返す、`IEnumerable`オブジェクトは、一般的な読み取り専用リスト処理タスクをなど、実行できることにより、`foreach`ループと、LINQ クエリことはできませんを追加またはそのような変更であることを示唆する、コレクション内の項目を削除データベースに保存します。

作成、 *Departments.aspx*を使用するページ、 *Site.Master*マスター ページ、およびでは、次のマークアップを追加、`Content`という名前のコントロール`Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample3.aspx)]

このマークアップを作成、`ObjectDataSource`コントロールを作成したリポジトリ クラスを使用して、`GridView`データを表示するコントロール。 `GridView`コントロールを指定します**編集**と**削除**コマンドがまだそれらをサポートするコードを追加していません。

複数の列を使用して、`DynamicField`そのデータの自動書式設定と検証機能の利用を制御します。 呼び出す必要があるこれらの場合、`EnableDynamicData`メソッドで、`Page_Init`イベント ハンドラー。 (`DynamicControl`でコントロールが使用されていない、`Administrator`フィールドため、ナビゲーション プロパティでは動作しません)。

`Vertical-Align="Top"`属性重要になる後では、入れ子になった列を追加するときに`GridView`グリッド コントロール。

開く、 *Departments.aspx.cs*ファイルを開き、次の追加`using`ステートメント。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample4.cs)]

次のページのハンドラーを追加し、`Init`イベント。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample5.cs)]

*DAL*フォルダーという新しいクラス ファイルを作成*Department.cs*既存のコードを次のコードに置き換えます。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample6.cs)]

このコードは、データ モデルにメタデータを追加します。 指定します、`Budget`のプロパティ、`Department`エンティティは、そのデータ型が実際には通貨を表します`Decimal`、し、値が 0 ~ $1,000,000.00 の範囲でなければならないことを指定します。 指定する、`StartDate`プロパティは、形式年/月/日の日付として書式設定する必要があります。

実行、 *Departments.aspx*ページ。

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image26.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image25.png)

書式設定文字列で指定しなかったことに注意して、 *Departments.aspx*ページのマークアップを**予算**または**Start Date**列、既定の通貨と日付これらの書式設定が適用されて、`DynamicField`コントロールに提供されているメタデータを使用して、 *Department.cs*ファイル。

## <a name="adding-insert-and-delete-functionality"></a>追加の挿入および削除の機能

開いている*SchoolRepository.cs*、作成するには、次のコードを追加、`Insert`メソッドと`Delete`メソッド。 コードは、という名前のメソッドも含まれています。`GenerateDepartmentID`で使用するための次の使用可能なレコード キー値を計算する、`Insert`メソッド。 これは、これを自動的に計算する、データベースが構成されていないために必要な`Department`テーブル。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample7.cs)]

### <a name="the-attach-method"></a>Attach メソッド

`DeleteDepartment`メソッドの呼び出し、`Attach`メソッドがメモリ内のエンティティとデータベースの間、オブジェクト コンテキストのオブジェクト状態マネージャーに保持されているリンクを再確立するには、行を表します。 これは、メソッド呼び出しの前に発生する必要があります、`SaveChanges`メソッド。

用語*オブジェクト コンテキスト*から派生した Entity Framework クラスを参照、`ObjectContext`クラス、エンティティ セットとエンティティにアクセスするために使用します。 このプロジェクトのコードでクラスの名前は`SchoolEntities`、そのインスタンスの名前は常に`context`します。 オブジェクト コンテキストの*オブジェクト状態マネージャー*から派生したクラスには、`ObjectStateManager`クラス。 オブジェクトの連絡先は、エンティティ オブジェクトを格納して、それぞれに対応するテーブルの行またはデータベース内の行と同期されているかどうかを追跡するオブジェクト状態マネージャーを使用します。

エンティティを読み取るときに、オブジェクト コンテキストはオブジェクト状態マネージャーに格納しのオブジェクトの表現は、データベースと同期されているかどうかを追跡します。 たとえば、プロパティ値を変更する場合は、フラグがプロパティを変更するにはデータベースと同期が不要になったことを示す設定されます。 その後呼び出す、`SaveChanges`メソッド、オブジェクト コンテキストでは、オブジェクト状態マネージャーは、エンティティの現在の状態と、データベースの状態の相違点だけを知っているため、データベースで実行することがわかっています。

ただし、このプロセス通常機能しません、web アプリケーションで、ページが表示されたら、と共に、オブジェクトの状態マネージャー内のすべてのエンティティを読み取るオブジェクト コンテキスト インスタンスが破棄されるためです。 オブジェクト コンテキスト インスタンスに変更を適用する必要がありますが、ポストバック処理がインスタンス化するか新しいします。 場合、`DeleteDepartment`メソッド、`ObjectDataSource`コントロール再作成しますエンティティの元のバージョンをビュー ステートの値からが再作成この`Department`オブジェクト状態マネージャーにエンティティが存在しません。 呼び出した場合、`DeleteObject`この再作成されたエンティティのメソッドは、オブジェクト コンテキストにエンティティがデータベースと同期するかどうかがわからないため、呼び出しが失敗します。 ただし、呼び出し、`Attach`メソッドがオブジェクト コンテキストの以前のインスタンスで、エンティティが読み取られたときに自動的に実行された最初のデータベースで再作成されたエンティティと値の間の追跡を再確立します。

ありますが、オブジェクト状態マネージャー内のエンティティを追跡するためにオブジェクト コンテキストをしたくないし、実行を防ぐためにフラグを設定することができます。 この例は、このシリーズの以降のチュートリアルに表示されます。

### <a name="the-savechanges-method"></a>SaveChanges メソッド

この単純なリポジトリ クラスは、CRUD 操作を実行する方法の基本原則を示しています。 この例で、`SaveChanges`各更新後すぐにメソッドが呼び出されます。 実稼働アプリケーションで呼び出すする可能性があります、`SaveChanges`データベースが更新されたときより詳細に制御を提供する別のメソッドからのメソッド。 (次のチュートリアルの最後の unit of work パターンは関連する更新プログラムを調整する 1 つの方法を説明するホワイト ペーパーへのリンクが検索されます)。また、例では、`DeleteDepartment`メソッドでは、同時実行の競合を処理するためのコードは含まれません。 そのためにコードをこのシリーズの後のチュートリアルで追加します。

### <a name="retrieving-instructor-names-to-select-when-inserting"></a>インストラクターの名前を挿入するときに選択を取得します。

ユーザーは、新しい部門を作成するときに、ドロップダウン リストでインストラクターの一覧から管理者を選択できる必要があります。 そのため、次のコードを追加*SchoolRepository.cs*先ほど作成したビューを使用したインストラクターのリストを取得するメソッドを作成します。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample8.cs)]

### <a name="creating-a-page-for-inserting-departments"></a>部門を挿入するためのページを作成します。

作成、 *DepartmentsAdd.aspx*を使用するページ、 *Site.Master*ページとでは、次のマークアップを追加、`Content`という名前のコントロール`Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample9.aspx)]

このマークアップでは、2 つ作成されます`ObjectDataSource`には、新規に挿入するため 1`Department`エンティティと instructor 名を取得する 1 つずつ、`DropDownList`部門の管理者を選択するために使用されるコントロール。 マークアップは、作成、`DetailsView`コントロールのハンドラーを指定しますの新しい部門を入力制御`ItemInserting`イベント設定できるように、`Administrator`外部キーの値。 最後には、`ValidationSummary`エラー メッセージを表示するコントロール。

開いている*DepartmentsAdd.aspx.cs*し、以下の追加`using`ステートメント。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample10.cs)]

次のクラスの変数とメソッドを追加します。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample11.cs)]

`Page_Init`メソッド Dynamic Data 機能を有効にします。 ハンドラーは、`DropDownList`コントロールの`Init`イベント ハンドラーをコントロールへの参照を保存します、`DetailsView`コントロールの`Inserting`イベントでは、その参照を使用して、取得、`PersonID`の値が選択したインストラクターと更新プログラム`Administrator`の外部キー プロパティ、`Department`エンティティ。

ページの実行、新しい部署の情報を追加し、クリックして、**挿入**リンク。

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image28.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image27.png)

もう 1 つの新しい部署の値を入力します。 1,000,000.00 よりも大きい数値を入力、**予算**フィールドと次のフィールド タブ。 フィールドに、アスタリスクが表示され、上にマウス ポインターを保持する場合は、そのフィールドのメタデータで指定したエラー メッセージを表示できます。

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image30.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image29.png)

をクリックして**挿入**、によって表示されるエラー メッセージが表示して、`ValidationSummary`ページの下部にあるコントロール。

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image32.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image31.png)

次に、ブラウザーを終了し、開く、 *Departments.aspx*ページ。 削除機能を追加、 *Departments.aspx*ページを追加することで、`DeleteMethod`属性を`ObjectDataSource`コントロール、および`DataKeyNames`属性を`GridView`コントロール。 これらのコントロールの開始タグは次の例のようになります。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample12.aspx)]

ページを実行します。

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image34.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image33.png)

削除を実行したときに追加した部門、 *DepartmentsAdd.aspx*ページ。

## <a name="adding-update-functionality"></a>更新プログラムの機能を追加します。

開いている*SchoolRepository.cs*し、以下の追加`Update`メソッド。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample13.cs)]

クリックすると**Update**で、 *Departments.aspx*  ページで、`ObjectDataSource`コントロールでは、2 つ作成されます`Department`に渡すエンティティ、`UpdateDepartment`メソッド。 ビューステートに格納されている元の値が含まれていて、もう一方に入力された新しい値を格納、`GridView`コントロール。 内のコード、`UpdateDepartment`メソッドが渡す、`Department`元の値を持つエンティティ、`Attach`エンティティとデータベースの間の追跡を確立するためのメソッド。 このコードではその後、`Department`エンティティに新しい値を持つ、`ApplyCurrentValues`メソッド。 オブジェクト コンテキストでは、新旧の値を比較します。 新しい値が古い値と異なる場合は、オブジェクト コンテキストは、プロパティの値を変更します。 `SaveChanges`メソッドは、データベースで変更された列のみを更新します。 (ただし場合、このエンティティの update 関数は、ストアド プロシージャにマップされて、行全体が更新されますに関係なく、列が変更されました。)

開く、 *Departments.aspx*ファイルを開き、次の属性を追加、`DepartmentsObjectDataSource`コントロール。

- `UpdateMethod="UpdateDepartment"`
- `ConflictDetection="CompareAllValues"`   
 このエラーの原因の古い値をビューステートに格納されたで新しい値を持つとを比較できるように、`Update`メソッド。
- `OldValuesParameterFormatString="orig{0}"`   
 これを元の値パラメーターの名前は、コントロールに知らせます`origDepartment`します。

開始タグのマークアップ、`ObjectDataSource`コントロール次の例のようになります。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample14.aspx)]

追加、`OnRowUpdating="DepartmentsGridView_RowUpdating"`属性を`GridView`コントロール。 これを使用して、設定するのには、`Administrator`プロパティの値、ユーザーがドロップダウン リストで選択行に基づいています。 `GridView`次の例では、開始タグを今すぐに似ています。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample15.aspx)]

追加、`EditItemTemplate`の制御、`Administrator`列を`GridView`直後、制御、`ItemTemplate`その列の制御。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample16.aspx)]

これは、`EditItemTemplate`コントロールに似ていますが、`InsertItemTemplate`を制御、 *DepartmentsAdd.aspx*ページ。 違いを使用して、コントロールの初期値を設定すること、`SelectedValue`属性。

前に、`GridView`コントロールを追加、`ValidationSummary`で行ったように制御、 *DepartmentsAdd.aspx*ページ。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample17.aspx)]

開いている*Departments.aspx.cs* 、部分クラス宣言の直後後を参照するプライベート フィールドを作成する次のコードを追加し、`DropDownList`コントロール。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample18.cs)]

ハンドラーを追加し、`DropDownList`コントロールの`Init`イベントと`GridView`コントロールの`RowUpdating`イベント。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample19.cs)]

ハンドラーは、`Init`イベントへの参照を保存する、`DropDownList`クラス フィールド内のコントロール。 ハンドラーは、`RowUpdating`イベントでは、参照を使用して、ユーザーが入力した値を取得し、それを適用、`Administrator`のプロパティ、`Department`エンティティ。

使用して、 *DepartmentsAdd.aspx*ページに、新しい部門を追加し、実行、 *Departments.aspx*ページし、をクリックして**編集**追加した行にします。

> [!NOTE]
> 追加しなかった行を編集することはできません (つまり、データベース内に既に)、データベースでは無効なデータのためデータベースで作成された行の管理者とは、受講者です。 これらのいずれかを編集しようとする場合のようなエラーを報告するエラー ページが表示されます。 `'InstructorsDropDownList' has a SelectedValue which is invalid because it does not exist in the list of items.`


[![Image10](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image36.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image35.png)

無効な入力すると**予算**金額し、順にクリックします**Update**、同じアスタリスクとに示したエラー メッセージを参照してください、 *Departments.aspx*ページ。

フィールドの値を変更または別の管理者を選択し、クリックして**Update**します。 変更が表示されます。

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image38.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image37.png)

これで完了の使用の概要、`ObjectDataSource`コントロールの基本的な CRUD (作成、読み取り、更新、削除)、Entity Framework での操作。 単純な n 層アプリケーションを構築したがビジネス ロジック層は、データ アクセス層は、自動化された単体テストが複雑になっても密に結合されています。 次のチュートリアルでは、単体テストを容易にするリポジトリ パターンを実装する方法を確認します。

> [!div class="step-by-step"]
> [次へ](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
