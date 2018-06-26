---
title: ASP.NET Core での Entity Framework Core を使用した Razor ページ - チュートリアル 1/8
author: rick-anderson
description: Entity Framework Core を使用して Razor ページ アプリを作成する方法について説明します
ms.author: riande
ms.date: 11/15/2017
uid: data/ef-rp/intro
ms.openlocfilehash: cadf36f4e1ff3776ad4139e1d7c4e9b73687bc5c
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279231"
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a>ASP.NET Core での Entity Framework Core を使用した Razor ページ - チュートリアル 1/8

作成者: [Tom Dykstra](https://github.com/tdykstra)、[Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso University のサンプル Web アプリでは、Entity Framework (EF) Core 2.0 と Visual Studio 2017 を使用して ASP.NET Core 2.0 MVC Web アプリケーションを作成する方法を示します。

このサンプル アプリは架空の Contoso University の Web サイトです。 学生の受け付け、講座の作成、講師の割り当てなどの機能が含まれています。 このページは、Contoso University のサンプル アプリを作成する方法を説明するチュートリアル シリーズの 1 回目です。

[完成したアプリをダウンロードまたは表示します。](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [ダウンロードの方法はこちらをご覧ください。](xref:tutorials/index#how-to-download-a-sample)

## <a name="prerequisites"></a>必須コンポーネント

[!INCLUDE [](~/includes/net-core-prereqs.md)]

[Razor ページ](xref:razor-pages/index)に関する知識。 Razor ページのプログラミングが初めての場合、このシリーズを始める前に[こちらの入門編](xref:tutorials/razor-pages/razor-pages-start)を完了してください。

## <a name="troubleshooting"></a>トラブルシューティング

解決できない問題に遭遇した場合、通常、[完成したステージ](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots)と自分のコードを比較することで解決策がわかります。 一般的なエラーとその解決方法の一覧については、[このシリーズの最後のチュートリアルにあるトラブルシューティングのセクション](xref:data/ef-mvc/advanced#common-errors)をご覧ください。 そこで必要な答えが見つからない場合、[StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) で [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) または [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core) に関する質問を投稿できます。

> [!TIP]
> このチュートリアル シリーズは、前のチュートリアルで行った内容を基に構築されています。 チュートリアルが完了したら、毎回、プロジェクトのコピーを保存するようお勧めします。 問題に遭遇したとき、前のチュートリアルから始めることができます。最初まで戻る必要がありません。 あるいは[完了したステージ](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots)をダウンロードし、それを利用してやり直すこともできます。

## <a name="the-contoso-university-web-app"></a>Contoso University Web アプリ

このチュートリアル シリーズで作成するアプリは、大学向けの基本的な Web サイトです。

ユーザーは学生、講座、講師の情報を見たり、更新したりできます。 チュートリアルで作成する画面は次のようになります。

![Students インデックス ページ](intro/_static/students-index.png)

![Students 編集ページ](intro/_static/student-edit.png)

このサイトの UI スタイルは、組み込みのテンプレートによって生成されるものに類似しています。 このチュートリアルでは、UI ではなく、EF Core と Razor ページに重点を置きます。

## <a name="create-a-razor-pages-web-app"></a>Razor ページ Web アプリを作成する

* Visual Studio の **[ファイル]** メニューから、**[新規作成]**、**[プロジェクト]** の順に選択します。
* 新しい ASP.NET Core Web アプリケーションを作成します。 プロジェクトに **ContosoUniversity** という名前を付けます。 このプロジェクトに *ContosoUniversity* という名前を付けることは重要です。そうすることでコードをコピーしたり貼り付けるときに名前空間が一致します。
 ![新しい ASP.NET Core Web アプリケーション](intro/_static/np.png)
* ドロップダウン リストで **[ASP.NET Core 2.0]** を選択してから、**[Web アプリケーション]** を選択します。
 ![Web アプリケーション (Razor ページ)](../../razor-pages/index/_static/np2.png)

**F5** キーを押して、デバッグ モードでアプリを実行するか、**Ctrl + F5** キーを押して、デバッガーをアタッチせずに実行します。

## <a name="set-up-the-site-style"></a>サイトのスタイルを設定する

変更をいくつか行い、サイトのメニュー、レイアウト、ホーム ページを決めます。

*Pages/_Layout.cshtml* を開き、次のように変更します。

* "ContosoUniversity" をすべて "Contoso University" に変更します。 これは 3 回出てきます。

* メニュー エントリとして「**Students**」、「**Courses**」、「**Instructors**」、「**Departments**」を追加し、「**Contact**」を削除します。

変更が強調表示されます。 (マークアップは一部表示されて*いません*。)

[!code-html[](intro/samples/cu/Pages/_Layout.cshtml?highlight=6,29,35-38,47&range=1-50)]

*Pages/Index.cshtml* で、ファイルの中身を次のコードに変更し、ASP.NET と MVC に関するテキストをこのアプリに関するテキストに変更します。

[!code-html[](intro/samples/cu/Pages/Index.cshtml)]

Ctrl キーを押しながら F5 キーを押してプロジェクトを実行します。 ホーム ページには、次のチュートリアルで作成されるタブが表示されます。

![Contoso University のホーム ページ](intro/_static/home-page.png)

## <a name="create-the-data-model"></a>データ モデルを作成する

Contoso University アプリのエンティティ クラスを作成します。 次の 3 つのエンティティから始めます。

![Course、Enrollment、Student からなるデータ モデルの図](intro/_static/data-model-diagram.png)

`Student` エンティティと `Enrollment` エンティティの間には一対多の関係があります。 `Course` エンティティと `Enrollment` エンティティの間には一対多の関係があります。 1 人の学生をさまざまな講座に登録できます。 1 つの講座に任意の数の学生を登録できます。

次のセクションでは、エンティティごとにクラスを作成します。

### <a name="the-student-entity"></a>Student エンティティ

![Student エンティティの図](intro/_static/student-entity.png)

*Models* フォルダーを作成します。 以下のコードを使用して、*Models* フォルダーで、*Student.cs* という名前のクラス ファイルを作成します。

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

`ID` プロパティは、このクラスに相当するデータベース (DB) テーブルの主キー列になります。 既定では、EF Core は、`ID` または `classnameID` という名前のプロパティを主キーとして解釈します。 `classnameID` で、`classname` は、前述の `Student` の例のとおりクラス名です。

`Enrollments` プロパティはナビゲーション プロパティです。 ナビゲーション プロパティは、このエンティティに関連する他のエンティティにリンクします。 この例では、`Student entity` の `Enrollments` プロパティによって、その `Student` に関連するすべての `Enrollment` エンティティが保持されます。 たとえば、DB 内のある Student 行に 2 つ関連する Enrollment 行がある場合、`Enrollments` ナビゲーション プロパティにその 2 つの `Enrollment` エンティティが含まれます。 関連する `Enrollment` 行とは、その学生の主キー値を `StudentID` 列に含む行です。 たとえば、ID=1 の学生には、`Enrollment` テーブルに行が 2 つあるとします。 その `Enrollment` テーブルには、`StudentID` = 1 の行が 2 つあります。 `StudentID` は `Enrollment` テーブルの外部キーであり、`Student` テーブルでその学生を指定します。

あるナビゲーション プロパティが複数のエンティティを保持できる場合、そのナビゲーション プロパティは `ICollection<T>` のようなリスト型にする必要があります。 `ICollection<T>` を指定するか、`List<T>` や `HashSet<T>` のような型を指定できます。 `ICollection<T>` を使用する場合、EF Core で `HashSet<T>` コレクションが既定で作成されます。 複数のエンティティを保持するナビゲーション プロパティは、多対多または一対多の関係から発生します。

### <a name="the-enrollment-entity"></a>Enrollment エンティティ

![Enrollment エンティティの図](intro/_static/enrollment-entity.png)

以下のコードを使用して、*Models* フォルダーで、*Enrollment.cs* を作成します。

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

`EnrollmentID` プロパティは主キーです。 このエンティティでは、`Student` エンティティのような `ID` ではなく、`classnameID` パターンを使用します。 一般的に、開発者は 1 つのパターンを選択し、データ モデル全体でそれを使用します。 後のチュートリアルでは、クラス名なしの ID を利用し、データ モデルに継承を簡単に実装します。

`Grade` プロパティは `enum` です。 `Grade` の型宣言の後の疑問符は、`Grade` プロパティが null 許容であることを示します。 null という成績は、0 の成績とは異なります。null は成績がわからないことか、まだ評価されていないことを意味します。

`StudentID` プロパティは外部キーです。それに対応するナビゲーション プロパティは `Student` です。 `Enrollment` エンティティは 1 つの `Student`エンティティに関連付けられますので、このプロパティには `Student` エンティティが 1 つ含まれます。 `Student` エンティティは、複数の `Enrollment` エンティティが含まれる `Student.Enrollments` ナビゲーション プロパティとは異なります。

`CourseID` プロパティは外部キーです。それに対応するナビゲーション プロパティは `Course` です。 `Enrollment` エンティティは 1 つの `Course` エンティティに関連付けられます。

EF Core は、プロパティの名前が `<navigation property name><primary key property name>` であれば、それを外部キーとして解釈します。 たとえば、`Student` ナビゲーション プロパティの `StudentID` です。`Student` エンティティの主キーは `ID` であるからです。 外部キーのプロパティには `<primary key property name>` という名前を付けることもできます。 たとえば、`CourseID` にします。`Course` エンティティの主キーが `CourseID` であるからです。

### <a name="the-course-entity"></a>Course エンティティ

![Course エンティティの図](intro/_static/course-entity.png)

以下のコードを使用して、*Models* フォルダーで、*Course.cs* を作成します。

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

`Enrollments` プロパティはナビゲーション プロパティです。 1 つの `Course` エンティティにたくさんの `Enrollment` エンティティを関連付けることができます。

`DatabaseGenerated` 属性によって、アプリで主キーを指定できます。DB に生成させるのではありません。

## <a name="create-the-schoolcontext-db-context"></a>SchoolContext DB コンテキストを作成する

所与のデータ モデルの EF Core 機能を調整するメイン クラスは、DB コンテキスト クラスです。 データ コンテキストは `Microsoft.EntityFrameworkCore.DbContext` から派生します。 データ コンテキストによって、データ モデルに含めるエンティティが指定されます。 このプロジェクトでは、クラスに `SchoolContext` という名前が付けられています。

プロジェクト フォルダーで、*Data* という名前のフォルダーを作成します。

*Data* フォルダーで、次のコードで *SchoolContext.cs* を作成します。

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

このコードによって、エンティティ セットごとに `DbSet` プロパティが作成されます。 EF Core 用語で:

* エンティティ セットは通常、DB テーブルに対応します。
* エンティティはテーブル内の行に対応します。

`DbSet<Enrollment>` と `DbSet<Course>` は省略できます。 EF Core にはそれらが暗黙的に含まれます。`Student` エンティティが `Enrollment` エンティティを参照し、`Enrollment` エンティティが `Course` エンティティを参照するためです。 このチュートリアルでは、`SchoolContext` に `DbSet<Enrollment>` と `DbSet<Course>` を残します。

DB が作成されると、EF Core によって、`DbSet` プロパティと同じ名前を持つテーブルが作成されます。 コレクションのプロパティ名は通常、複数形になります (Student ではなく Students)。 テーブル名を複数形にするかどうかについては、開発者の間で意見が分かれます。 このチュートリアル シリーズでは、DbContext に単数形のテーブル名を指定して既定の動作をオーバーライドします。 単数形のテーブル名を指定するには、次の強調表示されているコードを追加します。

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a>依存関係挿入にコンテキストを登録する

ASP.NET Core には、[依存関係挿入](xref:fundamentals/dependency-injection)が含まれています。 サービス (EF Core DB コンテキストなど) は、アプリケーションの起動時に依存関係挿入に登録されます。 これらのサービスを必要とするコンポーネント (Razor ページなど) には、コンストラクターのパラメーターを介してこれらのサービスが指定されます。 db コンテキスト インスタンスを取得するコンストラクター コードは、チュートリアルの後半で示します。

`SchoolContext` をサービスとして登録するには、*Startup.cs* を開き、強調表示されている行を `ConfigureServices` メソッドに追加します。

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

`DbContextOptionsBuilder` オブジェクトでメソッドが呼び出され、接続文字列の名前がコンテキストに渡されます。 ローカル開発の場合、[ASP.NET Core 構成システム](xref:fundamentals/configuration/index)が *appsettings.json* ファイルから接続文字列を読み取ります。

`using` ステートメントを名前空間 `ContosoUniversity.Data` と `Microsoft.EntityFrameworkCore` に追加します。 プロジェクトをビルドします。

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_Usings)]

*appsettings.json* ファイルを開き、次のコードのように接続文字列を追加します。

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

先の接続文字列では、[SQLClient](/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) が停止しないように `ConnectRetryCount=0` を使用します。

### <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

この接続文字列によって SQL Server LocalDB DB が指定されます。 LocalDB は SQL Server Express データベース エンジンの軽量版であり、実稼働ではなく、アプリの開発を意図して設計されています。 LocalDB は要求時に開始され、ユーザー モードで実行されるため、複雑な構成はありません。 既定では、LocalDB は `C:/Users/<user>` ディレクトリに *.mdf* DB ファイルを作成します。

## <a name="add-code-to-initialize-the-db-with-test-data"></a>テスト データで DB を初期化するコードを追加する

EF Core によって空の DB が作成されます。 このセクションでは、それにテスト データを入力するために *Seed* メソッドを記述します。

*Data* フォルダーで、*DbInitializer.cs* という名前の新しいクラス ファイルを作成し、次のコードを追加します。

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

このコードは、DB に学生が存在するかどうかを確認します。 DB に学生が存在しない場合、DB にテスト データが入力されます。 `List<T>` コレクションではなく配列にテスト データを読み込み、パフォーマンスを最適化します。

`EnsureCreated` メソッドは、DB のコンテキストに合わせて DB を自動的に作成します。 DB が存在する場合、`EnsureCreated` は DB を変更することなく戻ってきます。

*Program.cs* で、次を実行するように `Main` メソッドを変更します。

* 依存関係挿入コンテナーから DB コンテキスト インスタンスを取得します。
* seed メソッドを呼び出し、コンテキストを渡します。
* seed メソッドが完了したら、コンテキストを破棄します。

次は、更新された *Program.cs* ファイルのコードです。

[!code-csharp[](intro/samples/cu/ProgramOriginal.cs?name=snippet)]

初めてアプリを実行すると、DB が作成され、テスト データが入力されます。 データ モデルが更新されたら、次の操作を行います。
* DB を削除します。
* seed メソッドを更新します。
* アプリを実行します。データが入力された新しい DB が作成されます。 

後のチュートリアルでは、データ モデルが変わったとき、DB を削除して作り直すのではなく、DB を更新します。

<a name="pmc"></a>
## <a name="add-scaffold-tooling"></a>スキャフォールディング ツールを追加する

このセクションでは、パッケージ マネージャー コンソール (PMC) を使用し、Visual Studio Web コード生成パッケージを追加します。 スキャフォールディング エンジンを実行するには、このパッケージが必要です。

**[ツール]** メニューで、**[NuGet パッケージ マネージャー]**、**[パッケージ マネージャー コンソール]** の順に選択します。

パッケージ マネージャー コンソール (PMC) で、次のコマンドを入力します。

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Utils
```

先のコマンドでは、*.csproj ファイルに NuGet パッケージが追加されます。

[!code-xml[](intro/samples/cu/ContosoUniversity1_csproj.txt?highlight=7-8)]

<a name="scaffold"></a>
## <a name="scaffold-the-model"></a>モデルのスキャフォールディング

* プロジェクト ディレクトリ (*Program.cs*、*Startup.cs*、および *.csproj* ファイルを含むディレクトリ) でコマンド ウィンドウを開きます。
* 次のコマンドを実行します。


 ```console
dotnet restore
dotnet tool install --global dotnet-aspnet-codegenerator --version 2.1.0
dotnet aspnet-codegenerator razorpage -m Student -dc SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
 ```

エラーが発生した場合は、次のようにします。
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

プロジェクト ディレクトリ (*Program.cs*、*Startup.cs*、および *.csproj* ファイルを含むディレクトリ) でコマンド ウィンドウを開きます。


プロジェクトをビルドします。 ビルドにより、次のようなエラーが生成されます。

 `1>Pages\Students\Index.cshtml.cs(26,38,26,45): error CS1061: 'SchoolContext' does not contain a definition for 'Student'`

 `_context.Student` を `_context.Students` にグローバルに変更します (つまり、"s" を `Student` に追加します)。 7 回の出現が見つかり、更新されます。 次のリリースで[このバグ](https://github.com/aspnet/Scaffolding/issues/633)を解決する予定です。

[!INCLUDE [model4tbl](../../includes/RP/model4tbl.md)]

 <a name="test"></a>
### <a name="test-the-app"></a>アプリのテスト

アプリを実行し、**[Students]\(学生\)** リンクを選択します。 ブラウザーの幅にもよりますが、**[Students]\(学生\)** リンクはページの一番上に表示されます。 **[Students]\(学生\)** リンクが表示されない場合、右上隅にあるナビゲーション アイコンをクリックしてください。

![Contoso University のホーム ページ (ウィンドウ幅が狭いとき)](intro/_static/home-page-narrow.png)

**[Create]\(作成\)**、**[Edit]\(編集\)**、**[Details]\(詳細\)** リンクをテストします。

## <a name="view-the-db"></a>DB を表示する

アプリが起動すると、`DbInitializer.Initialize` は `EnsureCreated` を呼び出します。 `EnsureCreated` は DB の存在を検出し、必要に応じて DB を作成します。 DB に学生が入っていない場合、`Initialize` メソッドによって学生が追加されます。

Visual Studio の **[表示]** メニューから **SQL Server オブジェクト エクスプローラー** (SSOX) を開きます。
SSOX で、**(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1** をクリックします。

**[Tables]\(テーブル\)** ノードを展開します。

**[Student]\(学生\)** テーブルを右クリックし、**[View Data]\(データの表示\)** をクリックすると、作成された列とテーブルに挿入された行が表示されます。

<em>.mdf</em> ファイルと <em>.ldf</em> DB ファイルは <em>C:\Users\\<yourusername></em> フォルダーにあります。

アプリの起動時に `EnsureCreated` が呼び出され、次のワークフローが可能になります。

* DB を削除します。
* DB スキーマを変更します (たとえば、`EmailAddress` フィールドを追加します)。
* アプリを実行します。

`EnsureCreated` によって、`EmailAddress` 列を含む DB が作成されます。

## <a name="conventions"></a>規約

規約を利用することや EF Core で想定が行われることで、EF Core が完全な DB を作成するために記述しなければならないコードの量は最小限に抑えられます。

* `DbSet` プロパティの名前がテーブル名として使用されます。 `DbSet` プロパティによって参照されないエンティティについては、エンティティ クラス名がテーブル名として使用されます。

* 列名には、エンティティ プロパティ名が使用されます。

* ID または classnameID という名前が付けられているエンティティ プロパティは主キーのプロパティとして認識されます。

* *<navigation property name><primary key property name>* という名前が付いている場合、プロパティは外部キー プロパティとして解釈されます。たとえば、`Student` ナビゲーション プロパティの `StudentID` です。`Student` エンティティの主キーが `ID` であるためです。 外部キーには「*<primary key property name>*」という名前を付けることができます。たとえば、`EnrollmentID` です。`Enrollment` エンティティの主キーが `EnrollmentID` であるためです。

規約の動作はオーバーライドできます。 たとえば、このチュートリアルで先に示したように、テーブル名を明示的に指定できます。 列名を明示的に設定できます。 主キーと外部キーを明示的に設定できます。

## <a name="asynchronous-code"></a>非同期コード

ASP.NET Core と EF Core では、非同期プログラミングが既定のモードです。

Web サーバーでは、利用できるスレッド数に限りがあります。負荷が高い状況では、利用できるスレッドが全部使われる可能性があります。 その場合、スレッドが解放されるまでサーバーは新しい要求を処理できません。 同期コードの場合、たくさんのスレッドが関連付けられていても、I/O の完了を待っているため、実際には何の作業も行っていないということがあります。 非同期コードの場合、あるプロセスが I/O の完了を待っているとき、そのスレッドは解放され、サーバーによって他の要求の処理に利用できます。 結果として、非同期コードの場合、サーバー リソースをより効率的に利用できます。サーバーは、より多くのトラフィックを遅延なく処理できます。

非同期コードが実行時に発生させるオーバーヘッドは少量です。 トラフィックが少ない場合、パフォーマンスに与える影響は無視して構わない程度です。トラフィックが多い場合、相当なパフォーマンス改善が見込まれます。

次のコードでは、キーワード `async`、戻り値 `Task<T>`、キーワード `await`、メソッド `ToListAsync` によりコードの実行が非同期になります。

[!code-csharp[](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* キーワード `async` は次のことをコンパイラに指示します。

  * メソッド本文の一部にコールバックを生成する。
  * 返された [Task](/dotnet/api/system.threading.tasks.task?view=netframework-4.7) オブジェクトを自動作成する。 詳細については、[Task の戻り値の型](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType)に関する記事を参照してください。

* 暗黙の戻り値の型 `Task` は進行中の作業を表します。

* キーワード `await` により、コンパイラはメソッドを 2 つに分割します。 最初の部分は、非同期で開始される操作で終わります。 2 つ目の部分は、操作の完了時に呼び出されるコールバック メソッドに入ります。

* `ToListAsync` は、`ToList` 拡張メソッドの非同期バージョンです。

EF Core を利用する非同期コードの記述で注意すべき点:

* クエリやコマンドを DB に送信するステートメントのみが非同期で実行されます。 これには、`ToListAsync`、`SingleOrDefaultAsync`、`FirstOrDefaultAsync`、`SaveChangesAsync` が含まれます。 `var students = context.Students.Where(s => s.LastName == "Davolio")` など、`IQueryable` を変更するだけのステートメントは含まれません。

* EF Core コンテキストはスレッド セーフではありません。複数の操作を並列実行しないでください。 

* 非同期コードのパフォーマンス上の利点を最大限に活用するには、クエリをデータベースに送信させる EF Core メソッドを (ページングなどのための) ライブラリ パッケージで呼び出す場合、そのライブラリ パッケージで非同期が利用されていることを確認します。

.NET の非同期プログラミングについては、「[非同期の概要](/dotnet/articles/standard/async)」を参照してください。

次のチュートリアルでは、基本的な CRUD (作成、読み取り、更新、削除) の操作について説明します。

> [!div class="step-by-step"]
> [次へ](xref:data/ef-rp/crud)
