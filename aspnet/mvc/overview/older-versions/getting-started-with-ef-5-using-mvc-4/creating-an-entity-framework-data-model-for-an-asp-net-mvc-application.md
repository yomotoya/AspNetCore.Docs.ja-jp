---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: ASP.NET MVC アプリケーション (1 の 10) の Entity Framework データ モデルを作成する |Microsoft Docs
author: tdykstra
description: このチュートリアル シリーズの新しいバージョンが利用可能な Visual Studio 2013、Entity Framework 6、および MVC 5 です。 Contoso University のサンプル web アプリケーション de をしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 4ba029b6-ee7c-4e45-a0e7-b703c37e5d9a
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 68b74d1e1d13aa4ee080913bfd8219d1a9189bb7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377221"
---
<a name="creating-an-entity-framework-data-model-for-an-aspnet-mvc-application-1-of-10"></a>ASP.NET MVC アプリケーション (1 の 10) の Entity Framework データ モデルを作成します。
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[完成したプロジェクトのダウンロード](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> > [!NOTE] 
> > 
> > A[このチュートリアル シリーズの新しいバージョン](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)が Visual Studio 2013、Entity Framework 6、および MVC 5 を使用します。
> 
> 
> Contoso University のサンプルの web アプリケーションでは、Entity Framework 5 と Visual Studio 2012 を使用して ASP.NET MVC 4 アプリケーションを作成する方法を示します。 サンプル アプリケーションは架空の Contoso University の Web サイトです。 学生の受け付け、講座の作成、講師の割り当てなどの機能が含まれています。 このチュートリアル シリーズでは、Contoso University のサンプル アプリケーションを構築する方法について説明します。 できます[、完成したアプリケーションをダウンロード](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)します。
> 
> ## <a name="code-first"></a>Code First
> 
> Entity Framework 内のデータを使用する 3 つの方法があります: *Database First*、 *Model First*、および*Code First*します。 このチュートリアルでは、Code First の。 シナリオに最適なものを選択する方法に関するこれらのワークフローとガイダンスの違いについては、次を参照してください。 [Entity Framework 開発ワークフロー](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf)します。
> 
> ## <a name="mvc"></a>MVC
> 
> サンプル アプリケーションが上に構築された[ASP.NET MVC](../../../index.md)します。 ASP.NET Web フォーム モデルを使用する場合を参照してください、[モデル バインドと Web フォーム](../../../../web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data.md)チュートリアル シリーズと[ASP.NET データ アクセス コンテンツ マップ](../../../../whitepapers/aspnet-data-access-content-map.md)します。
> 
> ## <a name="software-versions"></a>ソフトウェアのバージョン
> 
> | **このチュートリアルで示すように** | **でも使用できます。** |
> | --- | --- |
> | Windows 8 | Windows 7 |
> | Visual Studio 2012 | Visual Studio 2012 Express for Web です。 VS 2012 または VS 2012 Express for Web がいない場合、Windows Azure SDK によって自動的にインストールされます。 Visual Studio 2013 が動作する必要がありますが、チュートリアルは、テストされていないと一部のメニューやダイアログ ボックスが異なる。 [VS 2013 バージョンの Windows Azure SDK](https://go.microsoft.com/fwlink/p/?linkid=323510) Windows Azure のデプロイが必要です。 |
> | .NET 4.5 | 表示されている機能のほとんどは、.NET 4 の機能しますが、いくつかはありません。 たとえば、EF で列挙型のサポートには、.NET 4.5 が必要です。 |
> | Entity Framework 5 |  |
> | [Windows Azure SDK 2.1](https://go.microsoft.com/fwlink/p/?linkid=323511) | Windows Azure 展開の手順をスキップする場合、SDK は不要です。 新しいバージョンの SDK がリリースされると、リンクは、新しいバージョンをインストールします。 その場合は、新しい UI と機能する手順の一部を調整する必要があります。 |
> 
> ## <a name="questions"></a>質問
> 
> チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET Entity Framework フォーラム](https://forums.asp.net/1227.aspx)、 [Entity Framework と LINQ to エンティティ フォーラム](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/)、または[StackOverflow.com](http://stackoverflow.com/)します。
> 
> ## <a name="acknowledgments"></a>謝辞
> 
> シリーズの最終チュートリアルを参照してください。 [VB に関する注意事項と受信確認](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#acknowledgments)します。
> 
> ## <a name="original-version-of-the-tutorial"></a>このチュートリアルの元のバージョン
> 
> このチュートリアルの元のバージョンが表示されます、 [EF 4.1/MVC 3 の電子書籍](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC)します。


## <a name="the-contoso-university-web-application"></a>Contoso University の Web アプリケーション

一連のチュートリアルで作成するアプリケーションは、簡単な大学向け Web サイトです。

ユーザーは学生、講座、講師の情報を見たり、更新したりできます。 次のような画面をこれから作成します。

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

このサイトの UI スタイルは、組み込みテンプレートで生成されるスタイルに近いものになっています。それにより、このチュートリアルでは主に、Entity Framework の使い方を取り上げることができます。

## <a name="prerequisites"></a>必須コンポーネント

道案内やこのチュートリアルのスクリーン ショットは、使用していることを想定しています[Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads)または[Visual Studio 2012 の Express for Web](https://go.microsoft.com/fwlink/?LinkID=275131)の最新の更新プログラムおよび Azure SDK for .NET は、7 月時点でインストールされています。2013。 次のリンクを使用して、このすべてを取得できます。

[Azure SDK for .NET (Visual Studio 2012)](https://go.microsoft.com/fwlink/?LinkId=254364)

Visual Studio をインストールした場合、上記のリンクは不足しているコンポーネントをインストールします。 Visual Studio を持っていない場合、Visual Studio 2012 の Express for Web のリンクがインストールされます。 Visual Studio 2013 を使用できますが、によって異なる場合は、必要な手順と画面の一部です。

## <a name="create-an-mvc-web-application"></a>MVC Web アプリケーションを作成します。

Visual Studio を開き、新しいプロジェクトを作成 (C#) という名前の"ContosoUniversity"を使用して、 **ASP.NET MVC 4 Web アプリケーション**テンプレート。 対象にするかどうかを確認 **.NET Framework 4.5** (使用する[`enum`プロパティ](https://msdn.microsoft.com/data/hh859576.aspx)、.NET 4.5 が必要になります)。

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

**新しい ASP.NET MVC 4 プロジェクト**ダイアログ ボックスの 、**インターネット アプリケーション**テンプレート。

ままに、 **Razor**選択すると、エンジンを表示して、**単体テスト プロジェクトを作成**チェック ボックスをオフします。

**[OK]** をクリックします。

![Project_template_options](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

## <a name="set-up-the-site-style"></a>サイトのスタイルを設定します。

簡単な変更をいくつか行い、サイトのメニュー、レイアウト、ホーム ページを決めます。

開いている*views \shared\\_Layout.cshtml*ファイルの内容を次のコードに置き換えます。 変更が強調表示されます。

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=5,15,25-28,43)]

このコードにより、次の変更が行われます。

- "My ASP.NET MVC Application"および"your logo here"のテンプレートのインスタンスを"Contoso University"に置き換えます。
- チュートリアルの後半で使用されるいくつかのアクション リンクを追加します。

*Views\Home\Index.cshtml*ファイルの内容を ASP.NET と MVC テンプレート段落を排除するには、次のコードに置き換えます。

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

*Controllers \homecontroller.cs*の値を変更`ViewBag.Message`で、 `Index` 「ようこそ Contoso 大学に!」、次の例に示すようにアクション メソッド。

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=3)]

サイトを実行するには、CTRL + F5 キーを押します。 メイン メニューで、ホーム ページを参照してください。

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

## <a name="create-the-data-model"></a>データ モデルを作成します。

次に、Contoso University アプリケーションのエンティティ クラスを作成します。 次の 3 つのエンティティをから始めます。

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

`Student` エンティティと `Enrollment` エンティティの間に一対多の関係があり、`Course` エンティティと `Enrollment` エンティティの間に一対多の関係があります。 言い換えると、1 人の学生をさまざまな講座に登録し、1 つの講座にたくさんの学生を登録できます。

次のセクションでは、エンティティごとにクラスを作成します。

> [!NOTE]
> これらのエンティティ クラスのすべての作成を完了する前に、プロジェクトをコンパイルしようとすると、コンパイラ エラーが表示されます。


### <a name="the-student-entity"></a>Student エンティティ

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

*モデル*フォルダー作成*Student.cs*既存のコードを次のコードに置き換えます。

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

`StudentID` プロパティは、このクラスに相当するデータベース テーブルの主キー列になります。 既定では、Entity Framework がという名前のプロパティを解釈`ID`または*classname* `ID`主キーとして。

`Enrollments`プロパティは、*ナビゲーション プロパティ*します。 ナビゲーション プロパティには、このエンティティに関連する他のエンティティが含まれます。 ここで、`Enrollments`のプロパティを`Student`すべてのエンティティを保持、`Enrollment`に関連付けられているエンティティ`Student`エンティティ。 つまり場合、指定された`Student`データベース内の行が 2 つの関連`Enrollment`行 (その学生の主キーを含む行の値で、`StudentID`外部キー列)、その`Student`エンティティの`Enrollments`ナビゲーション プロパティこれら 2 つが含まれます`Enrollment`エンティティ。

ナビゲーション プロパティは、通常、として定義`virtual`などの特定の Entity Framework の機能を利用を行うことができるように*遅延読み込み*します。 (遅延読み込みについては説明後の「、[関連データの読み取り](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)このシリーズの後半のチュートリアル。

ナビゲーション プロパティに複数のエンティティが含まれる場合 (多対多または一対多の関係で)、その型はリストにする必要があります。`ICollection` のように、エンティティを追加、削除、更新できるリストです。

### <a name="the-enrollment-entity"></a>Enrollment エンティティ

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

*[Models]* フォルダーで、*Enrollment.cs* を作成し、既存のコードを次のコードに変更します。

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

レベル プロパティは、 [enum](https://msdn.microsoft.com/data/hh859576.aspx)します。 後の疑問符、`Grade`型宣言では、ことを示します、`Grade`プロパティは[null 許容](https://msdn.microsoft.com/library/2cf62fcy.aspx)します。 Null であるグレードが 0 の点から異なる-null は、評価が不明なまたはまだ割り当てられていないことを意味します。

`StudentID` プロパティは外部キーです。それに対応するナビゲーション プロパティは `Student` です。 `Enrollment` エンティティは 1 つの `Student` エンティティに関連付けられており、1 つの `Student` エンティティだけを保持できます (先に見た、複数の `Enrollment` エンティティを保持できる `Student.Enrollments` ナビゲーション プロパティとは異なります)。

`CourseID` プロパティは外部キーです。それに対応するナビゲーション プロパティは `Course` です。 `Enrollment` エンティティは 1 つの `Course` エンティティに関連付けられます。

### <a name="the-course-entity"></a>Course エンティティ

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

*モデル*フォルダー作成*Course.cs*、既存のコードを次のコードに置き換えます。

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

`Enrollments` プロパティはナビゲーション プロパティです。 1 つの `Course` エンティティにたくさんの `Enrollment` エンティティを関連付けることができます。

詳しくは、[[DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx)([DatabaseGeneratedOption](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.95).aspx)します。次のチュートリアルではなし)] 属性です。 基本的に、この属性によって、講座の主キーをデータベースに生成させず、自分で入力できるようになります。

## <a name="create-the-database-context"></a>データベース コンテキストの作成

指定されたデータ モデルの Entity Framework の機能を調整するメイン クラスは、*データベース コンテキスト*クラス。 派生することによってこのクラスを作成する、 [System.Data.Entity.DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx)クラス。 自分のコードでは、データ モデルに含めるエンティティを自分で指定します。 Entity Framework の特定の動作をカスタマイズすることもできます。 このプロジェクトでは、クラスに `SchoolContext` という名前が付けられています。

という名前のフォルダーを作成する*DAL* (のデータ アクセス層)。 そのフォルダー内には、という名前の新しいクラス ファイルを作成*SchoolContext.cs*、既存のコードを次のコードに置き換えます。

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs)]

このコードを作成、 [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=VS.103).aspx)各エンティティ セットのプロパティ。 Entity Framework の用語で、*エンティティ セット*通常、データベース テーブルに対応し、*エンティティ*テーブルの行に対応しています。

`modelBuilder.Conventions.Remove`内のステートメント、 [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx)メソッドは、複数の中からテーブル名を防ぎます。 これをしなかった場合、生成されたテーブルの名前は`Students`、 `Courses`、および`Enrollments`します。 代わりに、テーブル名になります`Student`、 `Course`、および`Enrollment`します。 テーブル名を複数形にするかどうかについては、開発者の間で意見が分かれるでしょう。 このチュートリアルは、単数形を使用しますが、重要な点が含むかのコード行を省略すると、希望するどのフォームを選択することができます。

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx)は、SQL Server Express データベース エンジンの要求時に開始し、ユーザー モードで実行される軽量バージョンです。 LocalDB は SQL Server Express データベースとして使用することができますの特殊な実行モードで実行 *.mdf*ファイル。 LocalDB のデータベース ファイルを保持する、通常、*アプリ\_データ*web プロジェクトのフォルダー。 SQL Server express ユーザー インスタンス機能では使用することもできます *.mdf*ユーザー インスタンスの機能は、ファイルが非推奨とされます。 そのため、LocalDB を使用するため推奨 *.mdf*ファイル。

通常、実稼働 web アプリケーションの SQL Server Express は使用されません。 LocalDB 具体的には使用しないで web アプリケーションを運用環境で使用ため、IIS を使用するものではありません。

Visual Studio 2012 およびそれ以降のバージョンでは、LocalDB は、Visual Studio では既定でインストールされます。 Visual Studio 2010 および以前のバージョンでは、Visual Studio; に既定で (LocalDB) なしの SQL Server Express はインストールします。Visual Studio 2010 を使用している場合は手動でインストールがあります。

このチュートリアルで使用する LocalDB データベースに格納できるように、*アプリ\_データ*とフォルダー、 *.mdf*ファイル。 ルートを開く*Web.config*ファイルし、新しい接続文字列を追加、`connectionStrings`コレクションは、次の例に示すようにします。 (必ず更新して、 *Web.config*ルート プロジェクト フォルダー内のファイル。 *Web.config*ファイルは、*ビュー*サブフォルダーを更新する必要はありません)。

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.xml)]

既定と同じ名前の接続文字列の次の Entity Framework、`DbContext`クラス (`SchoolContext`このプロジェクトの)。 追加した接続文字列をという名前の LocalDB データベースを指定します*ContosoUniversity.mdf*内にある、*アプリ\_データ*フォルダー。 詳細については、次を参照してください。 [ASP.NET Web アプリケーションの SQL Server 接続文字列](https://msdn.microsoft.com/library/jj653752.aspx)します。

実際には、接続文字列を指定する必要はありません。 接続文字列を指定しない場合は、Entity Framework により作成されます。ただし、データベースでできる可能性がありますいない、*アプリ\_データ*アプリのフォルダー。 データベースを作成する方法の詳細については、次を参照してください。[新しいデータベースを Code First](https://msdn.microsoft.com/data/jj193542)します。

`connectionStrings`コレクションもという名前の接続文字列を持つ`DefaultConnection`メンバーシップ データベースに使用されます。 このチュートリアルでは、メンバーシップ データベースは使用しません。 2 つの接続文字列の唯一の違いは、データベース名および名前属性の値は。

## <a name="set-up-and-execute-a-code-first-migration"></a>設定して、コードの最初の移行を実行

アプリケーションの開発を初めて起動すると、データ モデルの変更、頻繁にし、毎回、モデルの変更はデータベースと同期を取得します。 自動的に削除し、データ モデルを変更するたびにデータベースを再作成するには、Entity Framework を構成することができます。 これは、問題には開発の早い段階でテスト データが簡単に再作成されたが、通常は実稼働環境にデプロイした後、データベースを削除せずにデータベース スキーマを更新するためです。 移行の機能によって、Code First を削除し、再作成することがなく、データベースを更新します。 新しいプロジェクトの開発サイクルの早い段階で使用する可能性があります[DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=vs.103).aspx)を削除して再作成し、データベースを再シードするたびに、モデルの変更。 いずれかを取得するアプリケーションをデプロイする準備ができて、移行のアプローチに変換することができます。 このチュートリアルでは移行のみを使用します。 詳細については、次を参照してください。 [Code First Migrations](https://msdn.microsoft.com/data/jj591621)と[移行スクリーン キャスト シリーズ](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx)します。

### <a name="enable-code-first-migrations"></a>Code First Migrations を有効にします。

1. **ツール** メニューのをクリックして**ライブラリ パッケージ マネージャー**し**パッケージ マネージャー コンソール**します。

    ![Selecting_Package_Manager_Console](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)
2. `PM>`プロンプトは、次のコマンドを入力します。

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.ps1)]

    ![enable-migrations コマンド](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

    このコマンドは、作成、*移行*ContosoUniversity プロジェクト、およびそのフォルダーはそのフォルダーに格納、 *Configuration.cs*移行を構成する編集可能なファイル。

    ![Migrations フォルダー](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)

    `Configuration`クラスが含まれています、`Seed`データベースを作成し、データ モデルを変更した後が更新されるたびに呼び出されるメソッドです。

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

    この目的`Seed`メソッドは、Code First によって作成または更新した後、テスト データをデータベースに挿入できるようにします。

### <a name="set-up-the-seed-method"></a>Seed メソッドを設定します。

[シード](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx)メソッドは、データベースの最新の移行を更新するたびに、Code First Migrations は、データベースを作成するときに、実行します。 Seed メソッドの目的は、アプリケーションの前に、テーブルにデータを挿入するため、最初にデータベースにアクセスします。

以前のバージョンの Code First では、移行がリリースされる前に、が一般的でした`Seed`開発中にモデル変更のたびに、データベースは完全に削除され、最初から再作成する必要があるため、テスト データを挿入するメソッド。 テスト データを保持するデータベースの変更後の Code First Migrations でにテスト データを含むため、[シード](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx)メソッドは通常必要ありません。 実際には、したくない、`Seed`場合、使用する移行、データベースを運用環境にデプロイするために、テスト データを挿入するメソッドを`Seed`メソッドは、実稼働環境で実行されます。 場合、`Seed`実稼働環境で挿入するデータのみをデータベースに挿入するメソッド。 などの実際の部署名を含めるデータベースをする可能性があります、`Department`テーブルの場合、アプリケーションが運用環境で使用可能になります。

このチュートリアルで使用する移行展開についてが、`Seed`メソッドが大量のデータを手動で挿入することがなくアプリケーションの機能の動作を確認しやすくためにテスト データをも挿入されます。

1. 内容を置き換える、 *Configuration.cs*ファイルを次のコードは、新しいデータベースにテスト データを読み込まれます。 

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

    [シード](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx)メソッドは、入力パラメーターとして、データベース コンテキストのオブジェクトとメソッドのコードがそのオブジェクトを使用して、データベースに新しいエンティティを追加します。 エンティティ型ごとに、コードは、新しいエンティティのコレクションを作成、適切なに追加します[DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx)プロパティ、および、データベースへの変更を保存します。 呼び出す必要はありません、 [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx)メソッドのエンティティの各グループの後に、ここでは、として行われますが、これを行うコードは、データベースに書き込んでいるときに例外が発生した場合は、問題の原因を見つけることができます。

    一部のデータを挿入するステートメントを使用して、 [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) "upsert"操作を実行するメソッド。 `Seed`メソッドは、すべての移行を実行、データベースを作成する最初の移行後に追加しようとしている行があるなる既にためだけデータを挿入できません。 "Upsert"操作が既に存在する、行を挿入しようとする場合に発生するとエラーを防ぐこと***オーバーライド***アプリケーションのテスト中に行ったデータに変更します。 いくつかのテーブルでのテスト データたくないことが起こる: 場合によってはテスト中にデータを変更すると、変更するデータベースの更新後に残します。 条件付きの insert 操作を実行する場合: 存在しない場合にのみ行を挿入します。 Seed メソッドでは、両方のアプローチを使用します。

    渡される最初のパラメーター、 [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx)メソッドを使用して、行が既に存在するかを確認するプロパティを指定します。 テスト受講者データを提供するには`LastName`各姓の一覧では一意であるために、この目的のプロパティを使用できます。

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

    このコードでは、姓と名が一意であることを前提としています。 重複する最後の名前を持つ学生を手動で追加する場合、次回の移行を実行する次の例外が表示されます。

    シーケンスには、1 つ以上の要素が含まれています。

    詳細については、`AddOrUpdate`メソッドを参照してください[EF 4.3 AddOrUpdate メソッドを使用して対処](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)Julie Lerman のブログ。

    追加するコード`Enrollment`エンティティを使用しない、`AddOrUpdate`メソッド。 エンティティが既に存在し、存在しない場合は、エンティティを挿入かどうかを確認します。 このアプローチには、移行の実行時に、登録レベルに加えた変更が保存されます。 各メンバーがループ処理、 `Enrollment`[一覧](https://msdn.microsoft.com/library/6sh2ey19.aspx)とデータベースの登録が見つからない場合、データベースに登録を追加します。 最初に、データベースを更新するデータベースは空になりますので、各登録が追加されます。

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

    デバッグする方法については、`Seed`メソッドと"Alexander Carson"という名前の 2 つの学生などの冗長なデータを処理する方法を参照してください。[シード処理中、およびデバッグ Entity Framework (EF) Db](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) Rick Anderson のブログ。
2. プロジェクトをビルドします。

### <a name="create-and-execute-the-first-migration"></a>作成し、最初の移行を実行

1. パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。 

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample14.ps1)]

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

    `add-migration` Migrations フォルダーを追加するコマンド、 *[日付]\_InitialCreate.cs*データベースを作成するコードを含むファイル。 最初のパラメーター (`InitialCreate)`ファイルの使用は、単語または語句の移行に実行する事柄をまとめたものを選択する通常; 名前を指定し、必要な値を指定できます。 たとえば、後で移行を名前可能性があります&quot;AddDepartmentTable&quot;します。

    ![最初の移行を migrations フォルダー](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

    `Up`のメソッド、`InitialCreate`クラスは、データ モデルのエンティティ セットに対応するデータベース テーブルを作成し、`Down`メソッドがそれらを削除します。 移行は、`Up` メソッドを呼び出して、移行のためのデータ モデルの変更を実装します。 更新をロールバックするためのコマンドを入力すると、移行が `Down` メソッドを呼び出します。 次のコードの内容を示しています、`InitialCreate`ファイル。

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

    `update-database`コマンドを実行、`Up`データベースし、作成するメソッドの実行、`Seed`メソッドは、データベースを設定します。

データ モデルの SQL Server データベースが作成されました。 データベースの名前は*ContosoUniversity*、および *.mdf*ファイルは、プロジェクトの*アプリ\_データ*フォルダーで指定したものであるためです、接続文字列。

いずれかを使用する**サーバー エクスプ ローラー**または**SQL Server オブジェクト エクスプ ローラー** Visual Studio でデータベースを表示するには、(SSOX)。 このチュートリアルで使用する**サーバー エクスプ ローラー**します。 Visual Studio Express 2012 for Web で**サーバー エクスプ ローラー**呼びます**データベース エクスプ ローラー**します。

1. **ビュー**  メニューのをクリックして**サーバー エクスプ ローラー**します。
2. をクリックして、**接続の追加**アイコン。

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)
3. メッセージが表示されたら、**データ ソースの選択**ダイアログ ボックスで、をクリックして**Microsoft SQL Server**、順にクリックします**続行**します。  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
4. **接続の追加** ダイアログ ボックスに、入力 **(localdb) \v11.0**の**サーバー名**します。 **を選択するか、データベース名を入力**、 **ContosoUniversity します。**  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
5. **[OK]** をクリックします。
6. 展開**SchoolContext**順に展開**テーブル**します。  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image18.png)
7. 右クリックし、**学生**テーブルし、クリックして**テーブル データの表示**に作成された列とテーブルに挿入された行を参照してください。

    ![Student テーブル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image19.png)

## <a name="creating-a-student-controller-and-views"></a>学生向けのコント ローラーとビューの作成

次の手順では、これらのテーブルのいずれかで動作するアプリケーションで、ASP.NET MVC コント ローラーとビューを作成します。

1. 作成する、`Student`コント ローラーを右クリックし、**コント ローラー**フォルダー**ソリューション エクスプ ローラー**を選択します**追加**、 をクリックし、**コント ローラー**. **コント ローラーの追加** ダイアログ ボックス、次の項目を選択してクリックして**追加**: 

   - コント ローラー名: **StudentController**します。
   - テンプレート:**読み取り/書き込みアクションと Entity Framework を使用して、ビューがある MVC コント ローラー**します。
   - モデル クラス:**学生 (ContosoUniversity.Models)** します。 (プロジェクトのビルド ドロップダウン リストでは、このオプションが表示されない場合ともう一度やり直してください。)
   - データ コンテキスト クラス: **SchoolContext (ContosoUniversity.Models)** します。
   - ビュー: **Razor (CSHTML)** します。 (既定値です。)

     ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image20.png)
2. Visual Studio を開き、 *Controllers\StudentController.cs*ファイル。 データベースのコンテキスト オブジェクトをインスタンス化するクラスの変数が作成されたを参照してください。

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

     `Index`アクション メソッドから受講者の一覧を取得する、*学生*エンティティ セットを読み取ることによって、`Students`データベース コンテキスト インスタンスのプロパティ。

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]

     *Student\Index.cshtml*ビューは、テーブルのこの一覧を表示します。

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample18.cshtml)]
3. Ctrl キーを押しながら F5 キーを押してプロジェクトを実行します。

     をクリックして、**学生**テスト データを表示するタブを`Seed`メソッドを挿入します。

     ![受講者の Index ページ](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image21.png)

## <a name="conventions"></a>規約

使用するための完全なデータベースを作成できるように Entity Framework の順序で記述する必要があるコードの量が最小限に抑える*規則*、または Entity Framework は、想定します。 一部の既に説明しました。

- エンティティ クラス名の複数形のフォームは、テーブル名として使用されます。
- 列名には、エンティティ プロパティ名が使用されます。
- という名前のエンティティ プロパティ`ID`または*classname* `ID`主キー プロパティとして認識されます。

規則をオーバーライドできることを説明しました (たとえば、指定したテーブル名を複数化しないこと)、規則とでこれらをオーバーライドする方法の詳細を学習し、[詳細の複雑なデータ モデルを作成する](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)チュートリアルこのシリーズの後半で。 詳細については、次を参照してください。[コードの最初の規則](https://msdn.microsoft.com/data/jj679962)します。

## <a name="summary"></a>まとめ

Entity Framework と SQL Server Express を使用して、保存してデータを表示する単純なアプリケーションが作成されました。 次のチュートリアルでが、基本的な CRUD を実行する方法について説明します (作成、読み取り、更新、削除) 操作。 このページの下部でフィードバックを送信しておくことができます。 チュートリアルのこの部分が立った方法と、それを改善する方法を通知してください。

その他の Entity Framework リソースへのリンクが記載されて、 [ASP.NET データ アクセス コンテンツ マップ](../../../../whitepapers/aspnet-data-access-content-map.md)します。

> [!div class="step-by-step"]
> [次へ](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
