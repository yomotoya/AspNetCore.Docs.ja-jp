---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: Entity Framework 6 Code First MVC 5 の使用の概要 |Microsoft Docs
author: tdykstra
ms.author: riande
ms.date: 10/04/2018
ms.assetid: 00bc8b51-32ed-4fd3-9745-be4c2a9c1eaf
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 5dd9a5a24d8c67c5190c98e3240d132eaa27661c
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912411"
---
# <a name="get-started-with-entity-framework-6-code-first-using-mvc-5"></a>Entity Framework 6 Code First MVC 5 の使用の概要します。

によって[Tom Dykstra](https://github.com/tdykstra)

[完成したプロジェクトのダウンロード](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

> [!NOTE]
> A[このチュートリアル シリーズの新しいバージョン](/aspnet/core/tutorials/razor-pages/razor-pages-start?view=aspnetcore-2.1)が使用する使用可能な[Razor ページ](/aspnet/core/razor-pages)します。 Razor ページはページ ベースのプログラミング モデルであり、Web UI の開発を容易にし、生産性を高めます。 Razor ページのチュートリアルは、MVC のバージョンをお勧めします。 この Razor ページのチュートリアルの特徴は次のとおりです。
>
> * 理解しやすい。
> * より多くの EF Core のベスト プラクティスが提供されている。
> * より効率的なクエリを使用している。
> * より最新の API を使用している。
> * 多くの機能をカバーしている。
> * 新しいアプリケーションの開発で推奨されるアプローチである。

> この記事では、Entity Framework 6 と Visual Studio を使用して ASP.NET MVC 5 アプリケーションを作成する方法を示します。 このチュートリアルでは、Code First ワークフローを使用します。 Code First、Database First または Model First を選択する方法については、次を参照してください。[モデルを作成する](/ef/ef6/modeling/)します。
>
> サンプル アプリケーションは、Contoso University という名前の架空の大学の web サイトです。 学生の受け付け、講座の作成、講師の割り当てなどの機能が含まれています。 このチュートリアル シリーズでは、Contoso University のサンプル アプリケーションを構築する方法について説明します。 できます[、完成したアプリケーションをダウンロード](https://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)します。
>
> Mike Brind によって変換された Visual Basic バージョンを利用できます: [Visual Basic での EF 6 と MVC 5](http://www.mikesdotnetting.com/Article/241/MVC-5-with-EF-6-in-Visual-Basic-Creating-an-Entity-Framework-Data-Model) Mikesdotnetting サイト。
>
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - [Entity Framework 6](https://www.nuget.org/packages/EntityFramework)
> - [Windows Azure SDK 2.2](https://go.microsoft.com/fwlink/p/?linkid=323510) (省略可能)
>
> ## <a name="tutorial-versions"></a>チュートリアルのバージョン
>
> このチュートリアルの以前のバージョンを参照してください。 [EF 4.1/MVC 3 の電子書籍](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC)と[MVC 4 を使用して EF 5 の概要](../../older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)します。
>
> ## <a name="questions-and-comments"></a>意見やご質問
>
> このチュートリアルと何が立った方法でフィードバックを送信してください、ページの下部にコメントを使用して改善できました。 チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET Entity Framework フォーラム](https://forums.asp.net/1227.aspx)または[StackOverflow.com](http://stackoverflow.com/)します。
>
> 解決できない問題が発生した場合は、ダウンロードできる完成したプロジェクトにコードを比較することで、問題を解決する一般的に検索できます。 一般的なエラーとその解決方法は、次を参照してください。[の一般的なエラーと解決方法または回避策](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)します。

## <a name="the-contoso-university-web-app"></a>Contoso University Web アプリ

これらのチュートリアルを組み込むアプリケーションは、簡単な大学向け web サイトです。 ユーザーは学生、講座、講師の情報を見たり、更新したりできます。 ここでは、いくつかの画面を作成します。

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![学生を編集します。](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

このチュートリアルは Entity Framework を使用する方法に重点を置いて、ように組み込みテンプレートによって生成されたものから多くの web サイトのユーザー インターフェイスは変更されません。

## <a name="prerequisites"></a>必須コンポーネント

参照してください**ソフトウェア バージョン**ページの上部にあります。 Entity Framework 6 は、このチュートリアルの一部として、EF の NuGet パッケージをインストールするための前提条件ではありません。

## <a name="create-an-mvc-web-app"></a>MVC web アプリを作成します。

1. Visual Studio を開き、新しい c# web プロジェクトを作成、 **ASP.NET Web アプリケーション (.NET Framework)** テンプレート。 "ContosoUniversity"プロジェクトの名前を付けます。

   ![Visual Studio で新しいプロジェクト ダイアログ ボックス](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/new-project-dialog.png)

2. 新しい ASP.NET プロジェクト ダイアログ ボックスで、選択、 **MVC**テンプレート。

   ![Visual Studio での新しい web アプリ ダイアログ ボックス](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/new-web-app-dialog.png)

3. 場合**認証**に設定されていない**認証なし**、 をクリックして変更する**認証の変更**します。

   **認証の変更**ダイアログ ボックスで、**認証なし**、選び、 **OK**します。 このチュートリアルでは、web アプリは、サインインするユーザーを必要としないもサインインしているユーザーに基づくアクセスは制限します。

   ![Visual Studio での認証ダイアログ ボックスを変更します。](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/change-authentication.png)

4. 新しい ASP.NET プロジェクト ダイアログ ボックスで、戻る をクリックして**OK**プロジェクトを作成します。

## <a name="set-up-the-site-style"></a>サイトのスタイルを設定する

簡単な変更をいくつか行い、サイトのメニュー、レイアウト、ホーム ページを決めます。

1. 開いている*views \shared\\_Layout.cshtml*、次の変更を行います。

   - 各出現する"My ASP.NET Application"、"Application name"を"Contoso University"に変更します。
   - 学生、コース、教員、および部門、メニュー エントリを追加し、連絡先エントリを削除します。

   次のコード スニペットでは、変更が強調表示されます。

   [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=6,19,24-27,38)]

2. *Views\Home\Index.cshtml*ファイルの内容を ASP.NET と MVC に関するテキストをこのアプリケーションに関するテキストに置き換えるには、次のコードに置き換えます。

   [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

3. キーを押して**Ctrl**+**F5** web サイトを実行します。 メイン メニューで、ホーム ページを参照してください。

   ![Contoso University のホーム ページ](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

## <a name="install-entity-framework-6"></a>Entity Framework 6 をインストールします。

1. **ツール**] メニューの [選択**NuGet パッケージ マネージャー**を選び、**パッケージ マネージャー コンソール**します。

2. **パッケージ マネージャー コンソール**ウィンドウで、次のコマンドを入力します。

   ```text
   Install-Package EntityFramework
   ```

   ![EF のインストール](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

   イメージがインストールされている 6.0.0 を示していますが、NuGet は (プレリリース版を除く)、Entity Framework の最新バージョンをインストールは、チュートリアルの最新の更新の時点では 6.2.0。

この手順は、このチュートリアルでは、手動で行うことがいる可能性がありますが完了に自動的に ASP.NET MVC のスキャフォールディング機能によって、いくつかの手順のいずれかです。 Entity Framework (EF) を使用するために必要な手順が表示されるため、それらを手動で行ってしています。 MVC コント ローラーとビューを作成して後でスキャフォールディングを使用します。 別の方法はスキャフォールディングを自動的に EF の NuGet パッケージをインストール、データベース コンテキスト クラスを作成および接続文字列を作成します。 方法で実行する準備ができたら、行う必要があるすべてがこれらの手順をスキップし、エンティティ クラスを作成した後に、MVC コント ローラーをスキャフォールディングします。

## <a name="create-the-data-model"></a>データ モデルを作成する

次に、Contoso University アプリケーションのエンティティ クラスを作成します。 次の 3 つのエンティティをから始めます。

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

`Student` エンティティと `Enrollment` エンティティの間に一対多の関係があり、`Course` エンティティと `Enrollment` エンティティの間に一対多の関係があります。 言い換えると、1 人の学生をさまざまな講座に登録し、1 つの講座にたくさんの学生を登録できます。

次のセクションでは、これらのエンティティのそれぞれのクラスを作成します。

> [!NOTE]
> これらのエンティティ クラスのすべての作成を完了する前に、プロジェクトをコンパイルしようとすると、コンパイラ エラーが表示されます。

### <a name="the-student-entity"></a>Student エンティティ

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

- *モデル*フォルダー、という名前のクラス ファイルを作成する*Student.cs*でフォルダーを右クリックして**ソリューション エクスプ ローラー**を選択して**追加**  > **クラス**します。 テンプレート コードを次のコードに置き換えます。

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs)]

`ID` プロパティは、このクラスに相当するデータベース テーブルの主キー列になります。 既定では、Entity Framework がという名前のプロパティを解釈`ID`または*classname* `ID`主キーとして。

`Enrollments` プロパティは*ナビゲーション プロパティ*です。 ナビゲーション プロパティには、このエンティティに関連する他のエンティティが含まれます。 ここで、`Enrollments`のプロパティを`Student`すべてのエンティティを保持、`Enrollment`に関連付けられているエンティティ`Student`エンティティ。 つまり場合、指定された`Student`データベース内の行が 2 つの関連`Enrollment`行 (その学生の主キーを含む行の値で、`StudentID`外部キー列)、その`Student`エンティティの`Enrollments`ナビゲーション プロパティこれら 2 つが含まれます`Enrollment`エンティティ。

ナビゲーション プロパティは、通常、として定義`virtual`などの特定の Entity Framework の機能を利用を行うことができるように*遅延読み込み*します。 (遅延読み込みについては説明後の「、[関連データの読み取り](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)このシリーズの後半のチュートリアルです)。

ナビゲーション プロパティに複数のエンティティが含まれる場合 (多対多または一対多の関係で)、その型はリストにする必要があります。`ICollection` のように、エンティティを追加、削除、更新できるリストです。

### <a name="the-enrollment-entity"></a>Enrollment エンティティ

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)

- *[Models]* フォルダーで、*Enrollment.cs* を作成し、既存のコードを次のコードに変更します。

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

`EnrollmentID`プロパティは、主キーになります。 このエンティティを使用して、 *classname* `ID`の代わりにパターン`ID`を単独で使用した、`Student`エンティティ。 通常、パターンを 1 つ選択し、データ モデル全体でそれを使用します。 ここのバリエーションから、いずれのパターンも利用できることがわかります。 後のチュートリアルで紹介を使用する方法`ID`せず`classname`データ モデルで継承を実装しやすきます。

`Grade`プロパティは、 [enum](/ef/ef6/modeling/code-first/data-types/enums)します。 後の疑問符、`Grade`型宣言では、ことを示します、`Grade`プロパティは[null 許容](/dotnet/csharp/programming-guide/nullable-types/using-nullable-types)します。 Null であるグレードが 0 の点から異なる-null は、評価が不明なまたはまだ割り当てられていないことを意味します。

`StudentID` プロパティは外部キーです。それに対応するナビゲーション プロパティは `Student` です。 `Enrollment` エンティティは 1 つの `Student` エンティティに関連付けられており、1 つの `Student` エンティティだけを保持できます (先に見た、複数の `Enrollment` エンティティを保持できる `Student.Enrollments` ナビゲーション プロパティとは異なります)。

`CourseID` プロパティは外部キーです。それに対応するナビゲーション プロパティは `Course` です。 `Enrollment` エンティティは 1 つの `Course` エンティティに関連付けられます。

Entity Framework がという名前が場合に、外部キー プロパティとしてプロパティを解釈*&lt;ナビゲーション プロパティの名前&gt;&lt;主キー プロパティ名&gt;* (たとえば、 `StudentID`の`Student`からナビゲーション プロパティ、`Student`エンティティの主キーが`ID`)。 外部キー プロパティできますも同じ名前に単に*&lt;主キー プロパティ名&gt;* (たとえば、`CourseID`ので、`Course`エンティティの主キーが`CourseID`)。

### <a name="the-course-entity"></a>Course エンティティ

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

- *モデル*フォルダー作成*Course.cs*、テンプレート コードを次のコードに置き換えます。

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

`Enrollments` プロパティはナビゲーション プロパティです。 1 つの `Course` エンティティにたくさんの `Enrollment` エンティティを関連付けることができます。

ここでの詳細について、<xref:System.ComponentModel.DataAnnotations.Schema.DatabaseGeneratedAttribute>このシリーズの以降のチュートリアルでは属性。 基本的に、この属性によって、講座の主キーをデータベースに生成させず、自分で入力できるようになります。

## <a name="create-the-database-context"></a>データベース コンテキストの作成

指定されたデータ モデルの Entity Framework の機能を調整するメイン クラスは、*データベース コンテキスト*クラス。 派生することによってこのクラスを作成する、 [System.Data.Entity.DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.113).aspx)クラス。 コードでは、データ モデルのエンティティが含まれているを指定します。 Entity Framework の特定の動作をカスタマイズすることもできます。 このプロジェクトでは、クラスに `SchoolContext` という名前が付けられています。

- ContosoUniversity プロジェクトのフォルダーを作成するでプロジェクトを右クリックして**ソリューション エクスプ ローラー**クリック**追加**、順にクリックします**新しいフォルダー**します。 新しいフォルダーの名前*DAL* (のデータ アクセス層)。 という名前の新しいクラス ファイルを作成すると、そのフォルダー内*SchoolContext.cs*、テンプレート コードを次のコードに置き換えます。

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

### <a name="specify-entity-sets"></a>エンティティ セットを指定します。

このコードを作成、 [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.113).aspx)各エンティティ セットのプロパティ。 Entity Framework の用語で、*エンティティ セット*通常、データベース テーブルに対応し、*エンティティ*テーブルの行に対応しています。

> [!NOTE]
>
> 省略することができます、`DbSet<Enrollment>`と`DbSet<Course>`ステートメントと同じように動作します。 Entity Framework はそれらを含める暗黙的に、`Student`エンティティ参照、`Enrollment`エンティティと`Enrollment`エンティティ参照、`Course`エンティティ。

### <a name="specify-the-connection-string"></a>接続文字列を指定します。

(これは後で、Web.config ファイルに追加されます) 接続文字列の名前で、コンス トラクターに渡されます。

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=1)]

現在、Web.config ファイルに格納されているいずれかの名前ではなく、接続文字列自体に渡すこともできます。 使用するデータベースを指定するためのオプションの詳細については、次を参照してください。[接続文字列とモデル](/ef/ef6/fundamentals/configuring/connection-strings)します。

接続文字列またはいずれかの名前を明示的に指定しない場合、Entity Framework では、接続文字列名は、クラス名と同じ前提としています。 この例では既定の接続文字列名になりますし`SchoolContext`、明示的に指定しているものと同じです。

### <a name="specify-singular-table-names"></a>単数のテーブル名を指定します。

`modelBuilder.Conventions.Remove`内のステートメント、 [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx)メソッドは、複数の中からテーブル名を防ぎます。 このしなかった場合、データベースで生成されたテーブルの名前は`Students`、 `Courses`、および`Enrollments`します。 代わりに、テーブル名になります`Student`、 `Course`、および`Enrollment`します。 テーブル名を複数形にするかどうかについては、開発者の間で意見が分かれるでしょう。 このチュートリアルは、単数形を使用しますが、重要な点が含むかのコード行を省略すると、希望するどのフォームを選択することができます。

## <a name="set-up-ef-to-initialize-the-database-with-test-data"></a>データベースにテスト データを初期化するために EF を設定します。

Entity Framework できます自動的に作成 (または削除して再作成) すると、アプリケーションの実行時のデータベース。 アプリケーションを実行するたびに、または既存のデータベースと同期されていません。 モデルの場合にのみこの行わ 必要がありますを指定できます。 記述することも、`Seed`その Entity Framework メソッドのテスト データを格納するために、データベースを作成した後。

既定の動作では、それが存在しない (とモデルが変更され、データベースが既に存在する場合に例外をスロー) 場合にのみデータベースを作成します。 このセクションでは、データベースを削除し、モデルが変更されるたびを再作成するを指定します。 データベースを削除すると、すべてのデータの損失とします。 これは、一般に開発中なので問題は、`Seed`メソッドは、データベースの再作成、テスト データが再作成時に実行されます。 運用環境で一般的にしたくない、データベース スキーマを変更する必要があるたびに、すべてのデータが失われます。 後で削除して、データベースを再作成ではなく、データベース スキーマを変更する Code First Migrations を使用してモデルの変更を処理する方法を確認します。

1. という名前の新しいクラス ファイルを作成、DAL フォルダーで*SchoolInitializer.cs*負荷テスト、新しいデータベースにデータと、テンプレート コードをデータベースに必要な時に作成されると、次のコードに置き換えます。

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

   `Seed`メソッドは、入力パラメーターとして、データベース コンテキストのオブジェクトとメソッドのコードがそのオブジェクトを使用して、データベースに新しいエンティティを追加します。 エンティティ型ごとに、コードは、新しいエンティティのコレクションを作成、適切なに追加します`DbSet`プロパティ、および、データベースへの変更を保存します。 呼び出す必要はありません、`SaveChanges`エンティティの各グループの後にメソッドは、ここでは、として行われますが、これを行うコードは、データベースに書き込んでいるときに例外が発生した場合は、問題の原因を見つけることができます。

2. 初期化子クラスを使用する Entity Framework を指示するには、要素を追加、`entityFramework`アプリケーション内の要素*Web.config*ファイル (ファイル、ルート プロジェクト フォルダー内)、次の例に示すようにします。

   [!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.xml?highlight=2-6)]

   `context type`コンテキストの完全修飾クラス名とには、アセンブリを指定します、`databaseinitializer type`と内にあるアセンブリの初期化子クラスの完全修飾名を指定します。 (で属性を設定するには、初期化子を使用する EF をしたくない場合、`context`要素: `disableDatabaseInitialization="true"`)。詳細については、次を参照してください。[構成ファイルの設定](/ef/ef6/fundamentals/configuring/config-file)します。

   初期化子を設定する代わりに、 *Web.config*ファイルを追加してコードで行うが、`Database.SetInitializer`ステートメントを`Application_Start`メソッドで、 *Global.asax.cs*ファイル。 詳細については、次を参照してください。 [Entity Framework Code First でデータベース初期化子を理解する](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm)します。

アプリケーションは Entity Framework がモデルにデータベースを比較し、アプリケーションの特定の実行で初めてデータベースにアクセスするときにできるように、今すぐを設定 (、`SchoolContext`とエンティティ クラス)。 違いがある場合、アプリケーションは削除し、データベースを再作成します。

> [!NOTE]
> 実稼働 web サーバーにアプリケーションを展開するときに、削除するか、削除し、データベースを再作成するコードを無効にする必要があります。 このシリーズの以降のチュートリアルを実行してみましょう。

## <a name="set-up-ef-to-use-a-sql-server-express-localdb-database"></a>SQL Server Express LocalDB データベースを使用するよう EF に設定します。

[LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb?view=sql-server-2017)は、SQL Server Express データベース エンジンの軽量バージョンです。 簡単にインストールして構成するには、オンデマンドで開始およびユーザー モードで実行します。 LocalDB は SQL Server Express データベースとして使用することができますの特殊な実行モードで実行 *.mdf*ファイル。 LocalDB のデータベース ファイルを配置することができます、*アプリ\_データ*プロジェクトにデータベースをコピーする場合は、web プロジェクトのフォルダー。 SQL Server express ユーザー インスタンス機能では使用することもできます *.mdf*ユーザー インスタンスの機能は、ファイルが非推奨とされます。 そのため、LocalDB を使用するため推奨 *.mdf*ファイル。 LocalDB は、Visual Studio では、既定でインストールされます。

通常、実稼働 web アプリケーションの SQL Server Express は使用されません。 LocalDB 具体的には使用しないで web アプリケーションを運用環境で使用ため、これは、IIS を使用する意図していません。

- このチュートリアルでは、LocalDB を使用します。 アプリケーションを開く*Web.config*追加ファイルを開き、`connectionStrings`前の要素、`appSettings`要素は、次の例に示すようにします。 (必ず更新して、 *Web.config*ルート プロジェクト フォルダー内のファイル。 *Web.config*ファイル、*ビュー*サブフォルダーを更新する必要はありません)。

   [!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.xml?highlight=1-3)]

追加した接続文字列は、Entity Framework がという名前の LocalDB データベースを使用するを指定します*ContosoUniversity1.mdf*します。 (データベースがまだ存在しないが EF によって作成されます)。データベースを作成する場合、*アプリ\_データ*でしたを追加するフォルダー、`AttachDBFilename=|DataDirectory|\ContosoUniversity1.mdf`接続文字列にします。 接続文字列の詳細については、次を参照してください。 [ASP.NET Web アプリケーションの SQL Server 接続文字列](/previous-versions/aspnet/jj653752(v=vs.110))します。

内の接続文字列を実際には必要はありません、 *Web.config*ファイル。 接続文字列を指定しない場合、Entity Framework には、コンテキスト クラスに基づいた既定の接続文字列が使用されます。 詳細については、次を参照してください。[新しいデータベースを Code First](/ef/ef6/modeling/code-first/workflows/new-database)します。

## <a name="create-a-student-controller-and-views"></a>学生向けのコント ローラーとビューを作成します。

これでデータを表示する web ページを作成します。 データを自動的に要求するプロセスは、データベースの作成をトリガーします。 新しいコント ローラーの作成から始めます。 その前に、モデルおよびコンテキスト クラスを MVC コント ローラーのスキャフォールディングを使用できるようにプロジェクトを作成します。

1. 右クリックし、**コント ローラー**フォルダー**ソリューション エクスプ ローラー**を選択します**追加**、 をクリックし、**スキャフォールディングされた新しい項目**します。
2. **スキャフォールディングの追加**ダイアログ ボックスで、 **MVC 5 コント ローラーとビュー、Entity Framework を使用して**を選び、**追加**します。

     ![Visual Studio でスキャフォールディング ダイアログ ボックスを追加します。](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/add-scaffold.png)

3. **コント ローラーの追加**ダイアログ ボックスで、次の項目を選択し、**追加**:

   - モデル クラス:**学生 (ContosoUniversity.Models)** します。 (プロジェクトのビルド ドロップダウン リストでは、このオプションが表示されない場合ともう一度やり直してください。)
   - データ コンテキスト クラス: **SchoolContext (ContosoUniversity.DAL)** します。
   - コント ローラー名: **StudentController** (StudentsController されません)。
   - その他のフィールドの既定値のままにします。

     ![Visual Studio でのコント ローラーのダイアログを追加します。](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/add-controller.png)

     クリックすると**追加**をスキャフォールダーの作成、 *StudentController.cs*ファイルと、一連のビュー (*.cshtml*ファイル)、コント ローラーと連動します。 今後の Entity Framework を使用するプロジェクトを作成する場合は、ことができますも活用する、スキャフォールダーの追加機能の一部: 最初のモデル クラスを作成、接続文字列を作成しませんし、 **追加コントローラー**ボックス指定**新しいデータ コンテキスト**を選択して、 **+** 横に**データ コンテキスト クラス**します。 スキャフォールダーの作成、`DbContext`だけでなく、コント ローラーおよびビューのクラスとの接続文字列します。
4. Visual Studio を開き、 *Controllers\StudentController.cs*ファイル。 クラスの変数が作成されているデータベースのコンテキスト オブジェクトをインスタンス化するを参照してください。

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

     `Index`アクション メソッドから受講者の一覧を取得する、*学生*エンティティ セットを読み取ることによって、`Students`データベース コンテキスト インスタンスのプロパティ。

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

     *Student\Index.cshtml*ビューは、テーブルのこの一覧を表示します。

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cshtml)]
5. キーを押して**Ctrl**+**F5**プロジェクトを実行します。 (「シャドウ コピーを作成できません」エラーが発生する場合、ブラウザーを閉じてもう一度お試し。)

     をクリックして、**学生**テスト データを表示するタブを`Seed`メソッドを挿入します。 どの幅の狭いブラウザー ウィンドウが、最上位のアドレス バーに受講者 タブのリンクが表示されますかに応じてへのリンクを参照してください。 右上隅をクリックする必要があります。

     ![メニュー ボタン](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

     ![受講者の Index ページ](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)

## <a name="view-the-database"></a>データベースを表示します。

Students のページを実行すると、アプリケーションが、データベースにアクセスしようとしています。、EF はデータベースがないと、1 つずつ作成があることを検出します。 EF は、データベースにデータを設定するシード メソッドを実行します。

いずれかを使用する**サーバー エクスプ ローラー**または**SQL Server オブジェクト エクスプ ローラー** Visual Studio でデータベースを表示するには、(SSOX)。 このチュートリアルで使用する**サーバー エクスプ ローラー**します。

1. ブラウザーを閉じます。
2. **サーバー エクスプ ローラー**、展開**データ接続**(最初に更新 ボタンを選択する必要があります、) 展開**学校のコンテキスト (ContosoUniversity)** 順に展開**テーブル**に新しいデータベースのテーブルを参照してください。

    ![サーバー エクスプ ローラーでデータベース テーブル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)

3. 右クリックし、**学生**テーブルし、クリックして**テーブル データの表示**に作成された列とテーブルに挿入された行を参照してください。

    ![Student テーブル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/table-data.png)
4. 閉じる、**サーバー エクスプ ローラー**接続します。

*ContosoUniversity1.mdf*と *.ldf*データベース ファイルは、 *%userprofile%* フォルダー。

使用しているため、`DropCreateDatabaseIfModelChanges`初期化子、行うことができますようになりましたへの変更、`Student`クラス、アプリケーションを再実行、およびデータベースに再作成された、変更に合わせて自動的になります。 追加する場合など、`EmailAddress`プロパティを`Student`クラス、学生のページを再度実行して、参照した後の表に、もう一度、表示、新しい`EmailAddress`列。

## <a name="conventions"></a>規約

完全なデータベースを作成できるように Entity Framework の順序で記述する必要があるコードの量は最小限のため*規則*、または Entity Framework は、想定します。 既に説明した一部の場合、または自分がそれらを認識せずに使用されていたは。

- エンティティ クラス名の複数形のフォームは、テーブル名として使用されます。
- 列名には、エンティティ プロパティ名が使用されます。
- という名前のエンティティ プロパティ`ID`または*classname* `ID`主キー プロパティとして認識されます。
- という名前が場合、外部キー プロパティとして、プロパティの解釈*&lt;ナビゲーション プロパティの名前&gt;&lt;主キー プロパティ名&gt;* (たとえば、 `StudentID` の`Student`からナビゲーション プロパティ、`Student`エンティティの主キーが`ID`)。 外部キー プロパティできますも同じ名前に単に&lt;主キー プロパティ名&gt;(たとえば、`EnrollmentID`ので、`Enrollment`エンティティの主キーが`EnrollmentID`)。

規則をオーバーライドできることを見てきました。 たとえば、テーブル名を複数化しないでくださいと、後述することを指定を外部キー プロパティとしてプロパティを明示的にマークする方法。 規則とでこれらをオーバーライドする方法の詳細を学習、[詳細の複雑なデータ モデルを作成する](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)このシリーズの後半のチュートリアル。 規則の詳細については、次を参照してください。[コードの最初の規則](/ef/ef6/modeling/code-first/conventions/built-in)します。

## <a name="summary"></a>まとめ

Entity Framework と SQL Server Express LocalDB を使用して格納し、データを表示する単純なアプリケーションを作成しました。 次のチュートリアルの基本的なを実行する方法について説明します作成、読み取り、更新、および削除 (CRUD) 操作。

このチュートリアルの立った方法で改善できましたフィードバックを送信してください。

その他の Entity Framework リソースへのリンクが記載[ASP.NET データ アクセス - 推奨リソース](../../../../whitepapers/aspnet-data-access-content-map.md)します。

> [!div class="step-by-step"]
> [次へ](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
