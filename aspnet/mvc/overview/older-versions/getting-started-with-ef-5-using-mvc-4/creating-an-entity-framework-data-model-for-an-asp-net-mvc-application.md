---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: ASP.NET MVC アプリケーション (10 の 1) 用の Entity Framework データ モデルの作成 |Microsoft ドキュメント
author: tdykstra
description: このチュートリアル シリーズの新しいバージョンを使用できる、Visual Studio 2013、Entity Framework 6、および MVC 5 です。 Contoso 大学サンプル web アプリケーション de しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 4ba029b6-ee7c-4e45-a0e7-b703c37e5d9a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: a963f26b408f2a54bd9cd3e852bc1e368f86c41f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877932"
---
<a name="creating-an-entity-framework-data-model-for-an-aspnet-mvc-application-1-of-10"></a>ASP.NET MVC アプリケーション (10 の 1) 用の Entity Framework データ モデルの作成
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[完成したプロジェクトのダウンロード](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> > [!NOTE] 
> > 
> > A[このチュートリアル シリーズの新しいバージョン](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)が Visual Studio 2013、Entity Framework 6、および MVC 5 使用します。
> 
> 
> Contoso 大学でサンプル web アプリケーションでは、Entity Framework 5 と Visual Studio 2012 を使用して ASP.NET MVC 4 アプリケーションを作成する方法を示します。 サンプル アプリケーションは架空の Contoso University の Web サイトです。 学生の受け付け、講座の作成、講師の割り当てなどの機能が含まれています。 このチュートリアルの系列では、Contoso 大学サンプル アプリケーションをビルドする方法について説明します。 実行できます[完成したアプリケーションをダウンロード](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)です。
> 
> ## <a name="code-first"></a>Code First
> 
> Entity Framework でデータを操作できる 3 つの方法があります: *Database First*、 *Model First*、および*Code First*です。 このチュートリアルでは、Code First のです。 シナリオに合った最適なものを選択する方法でこれらのワークフローとガイダンスの違いの詳細については、次を参照してください。 [Entity Framework 開発ワークフロー](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf)です。
> 
> ## <a name="mvc"></a>MVC
> 
> サンプル アプリケーションを作成した[ASP.NET MVC](../../../index.md)です。 ASP.NET Web フォーム モデルを使用する場合を参照してください、[モデル バインドと Web フォーム](../../../../web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data.md)一連のチュートリアルと[ASP.NET データ アクセス コンテンツ マップ](../../../../whitepapers/aspnet-data-access-content-map.md)です。
> 
> ## <a name="software-versions"></a>ソフトウェアのバージョン
> 
> | **このチュートリアルで示す** | **でも動作します。** |
> | --- | --- |
> | Windows 8 | Windows 7 |
> | Visual Studio 2012 | Visual Studio 2012 Express for Web です。 VS 2012 または VS 2012 Express for Web をまだ持っていない場合、Windows Azure SDK によって自動的にインストールされます。 Visual Studio 2013 の作業する必要がありますが、チュートリアルはこれを使ってテストされていませんおよび一部のメニューやダイアログ ボックスが異なります。 [VS 2013 バージョンの Windows Azure SDK](https://go.microsoft.com/fwlink/p/?linkid=323510)の Windows Azure 展開が必要です。 |
> | .NET 4.5 | .NET 4 で示されている機能のほとんどが機能しますが、一部はありません。 たとえば、EF で列挙型のサポートには、.NET 4.5 が必要です。 |
> | Entity Framework 5 |  |
> | [Windows Azure SDK 2.1](https://go.microsoft.com/fwlink/p/?linkid=323511) | Windows Azure 展開の手順をスキップする場合は、SDK が不要です。 新しいバージョンの SDK がリリースされたときに、リンクは、新しいバージョンをインストールします。 その場合は、新しい UI と機能の命令の一部を調整する必要があります。 |
> 
> ## <a name="questions"></a>質問
> 
> チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET Entity Framework フォーラム](https://forums.asp.net/1227.aspx)、 [Entity Framework でも LINQ to Entities フォーラム](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/)、または[StackOverflow.com](http://stackoverflow.com/)です。
> 
> ## <a name="acknowledgments"></a>謝辞
> 
> 一連の前回のチュートリアルを参照してください[受信確認、および VB に関する注記](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#acknowledgments)です。
> 
> ## <a name="original-version-of-the-tutorial"></a>このチュートリアルの元のバージョン
> 
> このチュートリアルの元のバージョンがで使用できる、 [EF 4.1/MVC 3 の電子書籍](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC)です。


## <a name="the-contoso-university-web-application"></a>Contoso 大学 Web アプリケーション

一連のチュートリアルで作成するアプリケーションは、簡単な大学向け Web サイトです。

ユーザーは学生、講座、講師の情報を見たり、更新したりできます。 次のような画面をこれから作成します。

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

このサイトの UI スタイルは、組み込みテンプレートで生成されるスタイルに近いものになっています。それにより、このチュートリアルでは主に、Entity Framework の使い方を取り上げることができます。

## <a name="prerequisites"></a>必須コンポーネント

道案内やこのチュートリアルのスクリーン ショットは、使用するいると仮定[Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads)または[Visual Studio 2012 の Express for Web](https://go.microsoft.com/fwlink/?LinkID=275131)の最新の更新プログラムおよび Azure SDK for .NET は、年 7 月の時点でインストールされています。2013。 次のリンクを使用して、このすべてを取得できます。

[Azure SDK for .NET (Visual Studio 2012)](https://go.microsoft.com/fwlink/?LinkId=254364)

Visual Studio をインストールした場合、上記のリンクは不足しているコンポーネントをインストールします。 Visual Studio を持っていない場合、リンクは Visual Studio 2012 の Express for Web をインストールします。 Visual Studio 2013 を使用できますが、必要な手順と画面の一部が異なります。

## <a name="create-an-mvc-web-application"></a>MVC Web アプリケーションを作成します。

Visual Studio を開き、プロジェクトを作成、新しい c#"ContosoUniversity"を使用して名前付き、 **ASP.NET MVC 4 Web Application**テンプレート。 対象にするかどうかを確認 **.NET Framework 4.5** (使用する[`enum`プロパティ](https://msdn.microsoft.com/data/hh859576.aspx)、.NET 4.5 を必要として)。

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

**新しい ASP.NET MVC 4 プロジェクト**ダイアログ ボックスの 、**インターネット アプリケーション**テンプレート。

ままにして、 **Razor**を選択すると、エンジンを表示およびのままにして、**単体テスト プロジェクトを作成**チェック ボックスをオフします。

**[OK]** をクリックします。

![Project_template_options](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

## <a name="set-up-the-site-style"></a>サイトのスタイルを設定します。

簡単な変更をいくつか行い、サイトのメニュー、レイアウト、ホーム ページを決めます。

開いている*\shared\\_Layout.cshtml*ファイルの内容を次のコードに置き換えます。 変更が強調表示されています。

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=5,15,25-28,43)]

このコードにより、次の変更が行われます。

- "My ASP.NET MVC Application"、「ここにロゴ」のテンプレートのインスタンスを「Contoso 大学」に置き換えます。
- このチュートリアルで後ほど使用されるいくつかのアクション リンクを追加します。

*Views\Home\Index.cshtml*、ASP.NET および MVC テンプレート段落を回避するのには、次のコードで、ファイルの内容を置き換えます。

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

*Controllers \homecontroller.cs*の値を変更`ViewBag.Message`で、 `Index` 「へようこそ Contoso 大学!」を次の例に示すようにアクション メソッド。

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=3)]

サイトを実行するには、CTRL + F5 キーを押します。 メイン メニューで、ホーム ページを参照してください。

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

## <a name="create-the-data-model"></a>データ モデルを作成します。

次に、Contoso University アプリケーションのエンティティ クラスを作成します。 次の 3 つのエンティティを開始します。

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

`Student` エンティティと `Enrollment` エンティティの間に一対多の関係があり、`Course` エンティティと `Enrollment` エンティティの間に一対多の関係があります。 言い換えると、1 人の学生をさまざまな講座に登録し、1 つの講座にたくさんの学生を登録できます。

次のセクションでは、エンティティごとにクラスを作成します。

> [!NOTE]
> すべてのエンティティ クラスを作成する前に、プロジェクトをコンパイルしようとすると、コンパイラ エラーが表示されます。


### <a name="the-student-entity"></a>学生エンティティ

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

*モデル*フォルダー作成*Student.cs*し、既存のコードを次のコードに置き換えます。

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

`StudentID` プロパティは、このクラスに相当するデータベース テーブルの主キー列になります。 既定では、Entity Framework では、解釈というプロパティ`ID`または*classname* `ID`主キーとして。

`Enrollments`プロパティは、*ナビゲーション プロパティ*です。 ナビゲーション プロパティには、このエンティティに関連する他のエンティティが含まれます。 ここで、`Enrollments`のプロパティ、`Student`のすべてのエンティティを保持する、`Enrollment`に関連付けられているエンティティ`Student`エンティティです。 つまり場合、指定された`Student`データベース内の行が 2 つの関連`Enrollment`行 (そのスチューデントの主キーが含まれている行の値で、`StudentID`外部キー列)、その`Student`エンティティの`Enrollments`ナビゲーション プロパティこれら 2 つが含まれます`Enrollment`エンティティです。

ナビゲーション プロパティは、通常、として定義`virtual`などの特定の Entity Framework 機能を利用を行うことができるように*遅延読み込み*です。 (遅延読み込みについては説明後の「、[関連のデータの読み取り](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)このシリーズ後でチュートリアルです。

ナビゲーション プロパティに複数のエンティティが含まれる場合 (多対多または一対多の関係で)、その型はリストにする必要があります。`ICollection` のように、エンティティを追加、削除、更新できるリストです。

### <a name="the-enrollment-entity"></a>登録エンティティ

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

*[Models]* フォルダーで、*Enrollment.cs* を作成し、既存のコードを次のコードに変更します。

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

グレード プロパティは、 [enum](https://msdn.microsoft.com/data/hh859576.aspx)です。 後に疑問符 ()、`Grade`型宣言では、ことを示します、`Grade`プロパティは[null 許容](https://msdn.microsoft.com/library/2cf62fcy.aspx)です。 Null である評価とは異なる、ゼロ グレード: null は、評価が不明またはまだ割り当てられていないことを意味します。

`StudentID` プロパティは外部キーです。それに対応するナビゲーション プロパティは `Student` です。 `Enrollment` エンティティは 1 つの `Student` エンティティに関連付けられており、1 つの `Student` エンティティだけを保持できます (先に見た、複数の `Enrollment` エンティティを保持できる `Student.Enrollments` ナビゲーション プロパティとは異なります)。

`CourseID` プロパティは外部キーです。それに対応するナビゲーション プロパティは `Course` です。 `Enrollment` エンティティは 1 つの `Course` エンティティに関連付けられます。

### <a name="the-course-entity"></a>Course エンティティ

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

*モデル*フォルダー作成*Course.cs*、既存のコードを次のコードに置き換えます。

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

`Enrollments` プロパティはナビゲーション プロパティです。 1 つの `Course` エンティティにたくさんの `Enrollment` エンティティを関連付けることができます。

ここでの詳細について、[[DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx)([DatabaseGeneratedOption](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.95).aspx)です。次のチュートリアルではなし)] 属性します。 基本的に、この属性によって、講座の主キーをデータベースに生成させず、自分で入力できるようになります。

## <a name="create-the-database-context"></a>データベース コンテキストの作成

指定されたデータ モデルの Entity Framework 機能を調整するメイン クラスは、*データベース コンテキスト*クラスです。 派生することによってこのクラスを作成する、 [System.Data.Entity.DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx)クラスです。 自分のコードでは、データ モデルに含めるエンティティを自分で指定します。 Entity Framework の特定の動作をカスタマイズすることもできます。 このプロジェクトでは、クラスに `SchoolContext` という名前が付けられています。

という名前のフォルダーを作成する*DAL* (データ アクセス層) 用です。 そのフォルダーの作成という新しいクラス ファイル*SchoolContext.cs*、し、既存のコードを次のコードに置き換えます。

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs)]

このコードを作成、 [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=VS.103).aspx)各エンティティ セットのプロパティです。 Entity Framework の用語で、*エンティティ セット*通常、データベース テーブルに対応し、*エンティティ*テーブル内の行に対応しています。

`modelBuilder.Conventions.Remove`内のステートメント、 [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx)メソッドは、複数化されてからテーブル名を防止します。 これをしなかった場合、生成されたテーブルの名前は`Students`、 `Courses`、および`Enrollments`です。 代わりに、テーブル名になります`Student`、 `Course`、および`Enrollment`です。 テーブル名を複数形にするかどうかについては、開発者の間で意見が分かれるでしょう。 このチュートリアルは単数形のフォームを使用しますが、重要な点は、することを含むか、次のコード行を省略することによって、どのフォームを選択することができます。

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx) SQL Server Express データベース エンジンの要求時に開始され、ユーザー モードで実行される軽量バージョンです。 LocalDB は、SQL Server Express データベースを対象として使用することができますの特殊な実行モードで実行 *.mdf*ファイル。 LocalDB のデータベース ファイルを保持する、通常、*アプリ\_データ*web プロジェクトのフォルダーです。 SQL Server express ユーザー インスタンスの機能でを操作することもできます *.mdf*は、ファイル、ユーザー インスタンス機能は推奨されません。 そのため、LocalDB を使用するため推奨 *.mdf*ファイル。

通常実稼働 web アプリケーションの SQL Server Express は使用されません。 LocalDB 具体的には、ために推奨されませんを実稼働用 web アプリケーションと IIS を使用するものはありません。

Visual Studio 2012 およびそれ以降のバージョンで LocalDB は既定では Visual Studio と共にインストールされます。 Visual Studio 2010 以前のバージョンで (LocalDB) なしの SQL Server Express がインストールされている既定では、Visual Studio です。Visual Studio 2010 を使用している場合は、手動でインストールする必要があります。

このチュートリアルで扱う LocalDB データベースに格納できるように、*アプリ\_データ*とフォルダー、 *.mdf*ファイル。 ルートを開く*Web.config*ファイルし、する新しい接続文字列を追加、`connectionStrings`コレクション、次の例で示すようにします。 (更新するかどうかを確認、 *Web.config*ルート プロジェクト フォルダー内のファイルです。 *Web.config*ファイルは、*ビュー*サブフォルダーを更新する必要はありません)。

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.xml)]

既定では、Entity Framework と同じ名前の接続文字列の検索、`DbContext`クラス (`SchoolContext`このプロジェクトの)。 という名前の LocalDB データベースを指定する接続文字列を追加した*ContosoUniversity.mdf*にある、*アプリ\_データ*フォルダーです。 詳細については、次を参照してください。 [ASP.NET Web アプリケーション用の SQL Server 接続文字列](https://msdn.microsoft.com/library/jj653752.aspx)です。

実際には、接続文字列を指定する必要はありません。 接続文字列を指定しないで、Entity Framework が作成されます。ただし、データベースでないあります、*アプリ\_データ*アプリのフォルダーです。 データベースを作成する方法の詳細については、次を参照してください。 [Code First の新しいデータベースに](https://msdn.microsoft.com/data/jj193542)です。

`connectionStrings`コレクションも名前付き接続文字列には`DefaultConnection`メンバーシップ データベースに使用されます。 このチュートリアルでは、メンバーシップ データベースを使用しません。 2 つの接続文字列の唯一の違いは、データベース名および名前属性の値です。

## <a name="set-up-and-execute-a-code-first-migration"></a>設定、コードの最初の移行を実行

を初めて起動するアプリケーションを開発するときに、データ モデルの変更するたびにし、多くの場合、モデルの変更と同期して、データベースを取得します。 自動的に削除し、データ モデルを変更するたびにデータベースを再作成するには、Entity Framework を構成することができます。 テスト データを簡単に再作成されますが、通常実稼働環境に配置した後をデータベースを削除しない限り、データベース スキーマを更新するためがない開発の早い段階で問題になります。 移行の機能によって、Code First を削除し、再作成することがなく、データベースを更新します。 新しいプロジェクトの開発サイクルの早い段階でが使用する[DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=vs.103).aspx)を削除して再作成し、データベースの再シードをモデル変更されるたびにします。 いずれかを取得、アプリケーションを展開する準備が、移行アプローチに変換することができます。 このチュートリアルでは、移行のみを使用します。 詳細については、次を参照してください。 [Code First Migrations](https://msdn.microsoft.com/data/jj591621)と[移行スクリーン キャスト系列](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx)です。

### <a name="enable-code-first-migrations"></a>Code First Migrations を有効にします。

1. **ツール** メニューのをクリックして**ライブラリ パッケージ マネージャー**し**Package Manager Console**です。

    ![Selecting_Package_Manager_Console](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)
2. `PM>`プロンプトに次のコマンドを入力してください。

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.ps1)]

    ![enable-migrations コマンド](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

    このコマンドを作成、*移行*ContosoUniversity プロジェクト内のフォルダーはそのフォルダーに格納、*される Configuration.cs*ファイルの移行を構成することができます。

    ![Migrations フォルダー](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)

    `Configuration`クラスが含まれています、`Seed`データベースが作成されると、データ モデルを変更した後が更新されるたびに呼び出されるメソッド。

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

    この目的`Seed`メソッドは、Code First の作成または更新した後、テスト データをデータベースに挿入するためにします。

### <a name="set-up-the-seed-method"></a>Seed メソッドを設定します。

[シード](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx)を最新の移行、データベースを更新するたびに、Code First Migrations がデータベースを作成するときに、メソッドが実行されます。 Seed メソッドの目的は、アプリケーションの前に、テーブルにデータを挿入するために最初にデータベースにアクセスです。

以前のバージョンの Code First では、移行がリリースされる前に、が一般的でした`Seed`開発時にモデル変更のたびに、データベースは完全に削除され、最初から再作成する必要があるため、テスト データを挿入するメソッド。 テスト データを保持するデータベースの変更後に、Code First Migrations にテスト データを含むため、[シード](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx)メソッド通常必要はありません。 実際には、したくない、`Seed`場合、使用する移行を実稼働でデータベースを配置するためにテスト データを挿入するメソッド、`Seed`メソッドは、実稼働環境で実行されます。 場合は、`Seed`実稼働環境で挿入するデータのみをデータベースに挿入するメソッド。 たとえば、ことができます、データベース内の実際の部門名を含める、`Department`表に、アプリケーションは実稼働環境で使用可能です。

このチュートリアルで使用する移行展開についても`Seed`メソッドが大量のデータを手動で挿入することがなくアプリケーションの機能のしくみを確認しやすくためにテスト データをこのまま挿入されます。

1. 内容を置き換える、*される Configuration.cs*ファイルを次のコードは、新しいデータベースにテスト データが読み込まれます。 

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

    [シード](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx)メソッドを受け取り、入力パラメーターとして、データベース コンテキストのオブジェクト、メソッドのコードはそのオブジェクトを使用して、データベースに新しいエンティティを追加します。 エンティティ型ごとに、コードは新しいエンティティのコレクションを作成、適切なにそれらを追加[DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx)プロパティ、および、データベースへの変更を保存します。 呼び出す必要はありません、 [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx)メソッドのエンティティの各グループの後に、ここでは、として行われますが、これを行うコードは、データベースに書き込んでいるときに例外が発生した場合、問題の原因を特定できます。

    一部のデータを挿入するステートメントを使用して、 [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) "upsert"操作を実行するメソッド。 `Seed`メソッドは、すべての移行が実行され、追加しようとしている行が既にありますデータベースを作成する最初の移行後にあるため、データを挿入することはできませんだけです。 "Upsert"操作が既に存在する行を挿入しようとする場合に発生するとエラーを防ぐこと***オーバーライド***アプリケーションのテスト中に対して行ったデータに対する変更。 いくつかのテーブルでのテスト データを使用しない場合発生すること。 場合によってはテスト中にデータを変更すると、変更するデータベースの更新後に残します。 条件付きの挿入操作を実行する場合: 存在しない場合にのみ行を挿入します。 Seed メソッドは、両方のアプローチを使用します。

    渡される最初のパラメーター、 [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx)メソッドが使用して、行が既に存在するかどうかを確認するプロパティを指定します。 次の情報を提供して、テスト学生データの`LastName`最後の各リストの名前は一意なので、この目的のプロパティを使用できます。

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

    このコードでは、最後に名前が一意であることを前提としています。 重複する姓を持つ、受講者を手動で追加する場合は、次回の移行を実行する次の例外が表示されます。

    シーケンスには、複数の要素が含まれています。

    詳細については、`AddOrUpdate`メソッドを参照してください[EF 4.3 AddOrUpdate メソッドを使用して注意](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)Julie Lerman ブログ。

    追加するコード`Enrollment`エンティティを使用していない、`AddOrUpdate`メソッドです。 これは、エンティティが既に存在しが存在しない場合は、エンティティを挿入します。 かどうかを確認します。 このアプローチには、移行を実行するときに、登録グレードに対して行った変更が保存されます。 各メンバーをループ処理、コード、 `Enrollment`[リスト](https://msdn.microsoft.com/library/6sh2ey19.aspx)し、データベースの登録が見つからない場合、データベースに登録を追加します。 初めて、データベースを更新するデータベースは空になりますので、各登録を追加します。

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

    デバッグする方法については、`Seed`メソッドと"Alexander Carson"という 2 つの受講者などの冗長なデータを処理する方法を参照してください[シード処理、およびデバッグ Entity Framework (EF) Db](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) Rick Anderson のブログです。
2. プロジェクトをビルドします。

### <a name="create-and-execute-the-first-migration"></a>作成して最初の移行を実行します。

1. パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。 

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample14.ps1)]

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

    `add-migration`コマンドは、Migrations フォルダーを追加、 *[日付スタンプ]\_InitialCreate.cs*データベースを作成するコードを含むファイルです。 最初のパラメーター (`InitialCreate)`ファイルで使用される単語または語句、移行で行われている新機能をまとめたものを選択する通常以外の名前を指定し、希望することができます。 たとえば、後で移行を名前可能性があります&quot;AddDepartmentTable&quot;です。

    ![最初の移行と移行フォルダー](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

    `Up`のメソッド、`InitialCreate`クラスは、データ モデルのエンティティ セットに対応するデータベース テーブルを作成し、`Down`メソッドでは、それらを削除します。 移行は、`Up` メソッドを呼び出して、移行のためのデータ モデルの変更を実装します。 更新をロールバックするためのコマンドを入力すると、移行が `Down` メソッドを呼び出します。 次のコードの内容を示しています、`InitialCreate`ファイル。

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

    `update-database`コマンドを実行する、`Up`データベースし、それを作成するメソッドの実行、`Seed`メソッドは、データベースを設定します。

SQL Server データベース、データ モデルに作成されました。 データベースの名前は*ContosoUniversity*、および *.mdf*ファイルは、プロジェクトの*アプリ\_データ*フォルダーで指定したものであるため、接続文字列です。

いずれかを使用する**サーバー エクスプ ローラー**または**SQL Server オブジェクト エクスプ ローラー** Visual Studio でデータベースを表示するには、(SSOX)。 このチュートリアルで使用する**サーバー エクスプ ローラー**です。 Visual Studio Express 2012 for Web で**サーバー エクスプ ローラー**が呼び出された**データベース エクスプ ローラー**です。

1. **ビュー**  メニューをクリックして**サーバー エクスプ ローラー**です。
2. クリックして、**接続の追加**アイコン。

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)
3. メッセージが表示されたら、**データ ソースの選択**ダイアログ ボックスで、をクリックして**Microsoft SQL Server**、順にクリック**続行**です。  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
4. **接続の追加** ダイアログ ボックスで、入力 **(localdb) \v11.0**の**サーバー名**です。 **を選択するか、データベースの名前を入力** **ContosoUniversity です。**  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
5. **[OK]** をクリックします。
6. 展開**SchoolContext**順に展開**テーブル**です。  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image18.png)
7. 右クリックし、**学生**テーブルし、をクリックして**テーブル データの表示**に作成された列とテーブルに挿入された行を参照してください。

    ![Student テーブル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image19.png)

## <a name="creating-a-student-controller-and-views"></a>学生のコント ローラーとビューの作成

次の手順では、これらのテーブルのいずれかで使用できるように、アプリケーションで ASP.NET MVC コント ローラーとビューを作成します。

1. 作成する、`Student`コント ローラーを右クリックし、**コント ローラー**フォルダー**ソリューション エクスプ ローラー****追加**、順にクリック**コント ローラー**. **コント ローラーの追加** ダイアログ ボックス、以下の選択を行い、をクリックして**追加**: 

   - コント ローラー名: **StudentController**です。
   - テンプレート:**読み取り/書き込みアクションと Entity Framework を使用して、ビューの MVC コント ローラー**です。
   - モデル クラス:**学生 (ContosoUniversity.Models)** です。 (プロジェクトのビルド ドロップダウン リストでは、このオプションが表示されない場合、およびもう一度やり直してください。)
   - データ コンテキスト クラス: **SchoolContext (ContosoUniversity.Models)** です。
   - ビュー: **Razor (CSHTML)** です。 (既定値です。)

     ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image20.png)
2. Visual Studio を開き、 *Controllers\StudentController.cs*ファイル。 データベース コンテキストのオブジェクトをインスタンス化するクラスの変数が作成されてが表示されます。

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

     `Index`アクション メソッドから受講者のリストを取得、*受講者*エンティティ セットを読み取ることによって、`Students`データベース コンテキストのインスタンスのプロパティ。

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]

     *Student\Index.cshtml*ビューは、テーブルのこの一覧を表示します。

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample18.cshtml)]
3. Ctrl キーを押しながら F5 キーを押してプロジェクトを実行します。

     をクリックして、**受講者**テスト データを表示 タブを`Seed`メソッドを挿入します。

     ![学生のインデックス ページ](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image21.png)

## <a name="conventions"></a>規約

使用するための完全なデータベースを作成できるように、Entity Framework の順序で記述したコードの量は最小限に抑える*規則*、または Entity Framework で想定します。 一部の既に説明した場合。

- エンティティのクラス名の複数形のフォームは、テーブル名として使用されます。
- 列名には、エンティティ プロパティ名が使用されます。
- 名前付きエンティティのプロパティ`ID`または*classname* `ID`主キー プロパティとして認識されます。

規則がオーバーライドすることを確認した (たとえば、指定したテーブル名を複数化しないようにする)、規則およびそれらをオーバーライドする方法の詳細を学習し、[詳細複雑なデータ モデルを作成する](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)チュートリアルこの系列に後で。 詳細については、次を参照してください。[コード First 規約に従って](https://msdn.microsoft.com/data/jj679962)です。

## <a name="summary"></a>まとめ

保存し、データを表示する Entity Framework および SQL Server Express を使用する簡単なアプリケーションが作成されました。 次のチュートリアルでは基本的な CRUD を実行する方法を学習 (作成、読み取り、更新、削除) 操作です。 このページの下部にあるフィードバックを送信することができます。 お知らせ、チュートリアルのこの部分をリンクする方法と、それを改善する方法です。

その他の Entity Framework リソースへのリンクは含まれて、 [ASP.NET データ アクセス コンテンツ マップ](../../../../whitepapers/aspnet-data-access-content-map.md)です。

> [!div class="step-by-step"]
> [次へ](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
