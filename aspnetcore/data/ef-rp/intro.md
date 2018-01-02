---
title: "Entity Framework Core - 8 のチュートリアル 1 で razor ページ"
author: rick-anderson
description: "Entity Framework のコアを使用して、Razor ページのアプリを作成する方法を示します"
keywords: "ASP.NET Core、Entity Framework Core、チュートリアル"
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/intro
ms.openlocfilehash: 571d683636244565b184cfec49061ec656377f11
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/14/2017
---
# <a name="getting-started-with-razor-pages-and-entity-framework-core-using-visual-studio-1-of-8"></a>Razor ページと Visual Studio (1/8) を使用して Entity Framework Core の概要

によって[Tom Dykstra](https://github.com/tdykstra)と[Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso 大学サンプル web アプリケーションでは、Entity Framework (EF) コア 2.0 と Visual Studio 2017 を使用して ASP.NET Core 2.0 MVC web アプリケーションを作成する方法を示します。

サンプル アプリは、架空の Contoso 大学の web サイトです。 学生受付、コースの作成、およびインストラクター割り当てなどの機能が含まれています。 このページは、最初の一連の Contoso 大学サンプル アプリをビルドする方法を説明するチュートリアルです。

[ダウンロードまたは完成したアプリを表示します。](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [ダウンロード命令の](xref:tutorials/index#how-to-download-a-sample)します。

## <a name="prerequisites"></a>必須コンポーネント

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="troubleshooting"></a>トラブルシューティング

問題を解決できない場合に発生した場合、コードを比較することでのソリューションを見つけることは通常、[ステージを完了した](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots)または[完成したプロジェクト](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu-final)です。 一般的なエラーとそれらを解決する方法の一覧は、次を参照してください。[系列の最後のチュートリアルの「トラブルシューティング](xref:data/ef-mvc/advanced#common-errors)です。 必要なものがない場合は、StackOverflow.com に質問を投稿することができます[ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core)または[EF コア](https://stackoverflow.com/questions/tagged/entity-framework-core)です。

> [!TIP]
> この一連のチュートリアルは、前のチュートリアルでの処理に基づいています。 各チュートリアルが正常に完了した後、プロジェクトのコピーを保存することを検討してください。 問題に遭遇した場合は、前のチュートリアルの先頭に戻るとではなく、経由で開始できます。 代わりに、ダウンロードすることができます、[ステージを完了](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots)し、完了した段階を使用して経由で開始します。

## <a name="the-contoso-university-web-app"></a>Contoso 大学 web アプリ

これらのチュートリアルでビルドされたアプリは、基本的な大学 web サイトです。

ユーザーでは、表示でき、学生、コース、インストラクターの情報を更新することができます。 このチュートリアルで作成した画面の一部を次に示します。

![インデックス ページの受講者](intro/_static/students-index.png)

![受講者の編集 ページ](intro/_static/student-edit.png)

このサイトの UI スタイルは、組み込みのテンプレートによって生成されたものに近いこと。 チュートリアルの焦点は Razor ページ、UI ではない、EF コアです。

## <a name="create-a-razor-pages-web-app"></a>Razor ページの web アプリを作成します。

* Visual Studio の **[ファイル]** メニューから、**[新規作成]**、**[プロジェクト]** の順に選択します。
* 新しい ASP.NET Core Web アプリケーションを作成します。 プロジェクトに名前を**ContosoUniversity**です。 プロジェクトに名前をすることが重要*ContosoUniversity*コードは、コピー/貼り付けときに、名前空間に一致するようにします。
 ![新しい ASP.NET Core Web アプリケーション](intro/_static/np.png)
* ドロップダウン リストで **[ASP.NET Core 2.0]** を選択してから、**[Web アプリケーション]** を選択します。
 ![Web アプリケーション (Razor ページ)](../../mvc/razor-pages/index/_static/np2.png)

**F5** キーを押して、デバッグ モードでアプリを実行するか、**Ctrl + F5** キーを押して、デバッガーをアタッチせずに実行します。

## <a name="set-up-the-site-style"></a>サイトのスタイルを設定します。

いくつかの変更は、[サイト] メニューのレイアウト、およびホーム ページを設定します。

開いている*Pages/_Layout.cshtml*次の変更を加えます。

* 「Contoso 大学」を"ContosoUniversity"の各出現する位置を変更します。 次の 3 つの出現があります。

* メニュー エントリを追加**受講者**、**コース**、**講習においてインストラクター**、および**部門**、および削除、 **にお問い合わせください**メニュー エントリです。

変更が強調表示されます。

[!code-html[](intro/samples/cu/Pages/_Layout.cshtml?highlight=6,29,35-38,47&range=1-50)]

*Pages/Index.cshtml*ファイルの内容をこのアプリに関するテキストで ASP.NET と MVC に関するテキストを置き換える次のコードに置き換えます。

[!code-html[](intro/samples/cu/Pages/Index.cshtml)]

Ctrl キーを押しながら F5 キーを押してプロジェクトを実行します。 ホーム ページには、次のチュートリアルで作成されたタブが表示されます。

![Contoso 大学のホーム ページ](intro/_static/home-page.png)

## <a name="create-the-data-model"></a>データ モデルを作成します。

Contoso 大学アプリ用にエンティティ クラスを作成します。 次の 3 つのエンティティで開始します。

![受講者コース-登録データ モデルのダイアグラム](intro/_static/data-model-diagram.png)

一対多の関係がある`Student`と`Enrollment`エンティティです。 一対多の関係がある`Course`と`Enrollment`エンティティです。 任意の数のコースに受講者を登録できます。 コースには、任意の数の受講者がそれに登録されていることができます。

次のセクションでは、これらのエンティティのいずれかのクラスが作成されます。

### <a name="the-student-entity"></a>学生エンティティ

![学生のエンティティの図](intro/_static/student-entity.png)

作成、*モデル*フォルダーです。 *モデル*フォルダー、という名前のクラス ファイルを作成する*Student.cs*を次のコード。

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

`ID`プロパティがこのクラスに対応するデータベース (DB) テーブルの主キー列になります。 既定では、EF Core では、解釈というプロパティ`ID`または`classnameID`主キーとして。

`Enrollments`プロパティは、ナビゲーション プロパティ。 ナビゲーション プロパティは、このエンティティに関連する他のエンティティにリンクします。 ここで、`Enrollments`のプロパティ、`Student entity`すべて保持、`Enrollment`に関連付けられているエンティティ`Student`です。 たとえば場合は、db 学生行が関連する 2 つ登録行、`Enrollments`ナビゲーション プロパティには、これら 2 つが含まれています。`Enrollment`エンティティです。 関連する`Enrollment`そのスチューデントの主キーの値を含む行が、`StudentID`列です。 たとえば、学生 ID を = 1 2 つの行には、`Enrollment`テーブル。 `Enrollment`テーブルを持つ 2 つの行には`StudentID`= 1 です。 `StudentID`外部キーには、`Enrollment`の受講者を指定するテーブル、`Student`テーブル。

ナビゲーション プロパティがリストの種類をなどでなければなりません場合は、ナビゲーション プロパティは、複数のエンティティを保持できる、`ICollection<T>`です。 `ICollection<T>`指定できますが、またはなど型`List<T>`または`HashSet<T>`です。 ときに`ICollection<T>`は EF コアを作成、使用、`HashSet<T>`既定のコレクション。 複数のエンティティを保持するナビゲーション プロパティは、多対多および一対多のリレーションシップから取得されます。

### <a name="the-enrollment-entity"></a>登録エンティティ

![エンティティの登録の図](intro/_static/enrollment-entity.png)

*モデル*フォルダー作成*Enrollment.cs*を次のコード。

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

`EnrollmentID`プロパティは、主キー。 このエンティティを使用して、`classnameID`パターンの代わりに`ID`と同様に、`Student`エンティティです。 通常の開発者は、1 つのパターンを選択し、データ モデル全体で使用します。 チュートリアルでは、後で、classname せず ID を使用するが、データ モデルで継承の実装を容易にできるように表示されます。

`Grade`プロパティは、`enum`です。 後に疑問符 ()、`Grade`型宣言では、ことを示します、`Grade`プロパティが null 値を許容します。 Null である評価とは異なる、ゼロ グレード--null またはため、評価はまだ割り当てられていません。

`StudentID`プロパティは、foreign key、および対応するナビゲーション プロパティは`Student`します。 `Enrollment`エンティティが 1 つに関連付けられた`Student`エンティティ、プロパティには、1 つが含まれるように`Student`エンティティです。 `Student`エンティティとは異なります、`Student.Enrollments`ナビゲーション プロパティは、複数含まれている`Enrollment`エンティティです。

`CourseID`プロパティは、foreign key、および対応するナビゲーション プロパティは`Course`します。 `Enrollment`エンティティが 1 つに関連付けられた`Course`エンティティです。

という名前が場合、外部キーとして解釈されるプロパティを EF コア`<navigation property name><primary key property name>`です。 たとえば、`StudentID`の`Student`ナビゲーション プロパティから、`Student`エンティティの主キーが`ID`です。 外部キーのプロパティとも呼ばれますできます`<primary key property name>`です。 たとえば、`CourseID`ので、`Course`エンティティの主キーが`CourseID`です。

### <a name="the-course-entity"></a>Course エンティティ

![Course エンティティの図](intro/_static/course-entity.png)

*モデル*フォルダー作成*Course.cs*を次のコード。

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

`Enrollments`プロパティは、ナビゲーション プロパティ。 A`Course`エンティティを任意の数に関連付けることができます`Enrollment`エンティティです。

`DatabaseGenerated` DB 生成ことのではなく、属性を使用して、アプリケーションで主キーを指定します。

## <a name="create-the-schoolcontext-db-context"></a>SchoolContext DB コンテキストを作成します。

指定されたデータ モデルの EF のコア機能を調整するのメイン クラスは、DB コンテキスト クラスです。 データ コンテキストがから派生した`Microsoft.EntityFrameworkCore.DbContext`です。 データ コンテキストでは、データ モデルのエンティティが含まれているを指定します。 クラスの名前は、このプロジェクトで`SchoolContext`です。

プロジェクト フォルダー内には、という名前のフォルダーを作成*データ*です。

*データ*フォルダー作成*SchoolContext.cs*を次のコード。

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

このコードを作成、`DbSet`各エンティティ セットのプロパティです。 EF コア用語では。

* エンティティ セットは、通常は、DB テーブルに対応します。
* エンティティは、テーブル内の行に対応します。

`DbSet<Enrollment>`および`DbSet<Course>`を省略できます。 EF コアそれらを含む暗黙的にあるため、`Student`エンティティ参照、`Enrollment`エンティティ、および`Enrollment`エンティティ参照、`Course`エンティティです。 このチュートリアルでは、このまま`DbSet<Enrollment>`と`DbSet<Course>`で、`SchoolContext`です。

データベースが作成される、EF コア作成と同じ名前を持つテーブルを`DbSet`プロパティの名前。 コレクションのプロパティ名は通常複数形 (受講者ではなく学生)。 テーブル名が複数あるかどうかは開発者が一致しません。 これらのチュートリアルについては、既定の動作は、DbContext で単数形のテーブル名を指定することによってオーバーライドされます。 単数形のテーブル名を指定するには、次の強調表示されたコードを追加します。

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a>依存関係の挿入をコンテキストに登録します。

ASP.NET Core を含む[依存性の注入](xref:fundamentals/dependency-injection)です。 EF コア DB コンテキスト) などのサービスは、アプリケーションの起動時に依存関係の挿入に登録されます。 これらのサービス (Razor ページなど) を必要とするコンポーネントには、コンス トラクターのパラメーターを使用してこれらのサービスが用意されています。 このチュートリアルで後ほど db コンテキストのインスタンスを取得するコンス トラクター コードが表示されます。

登録する`SchoolContext`をサービスとして開く*Startup.cs*を強調表示された行を追加し、`ConfigureServices`メソッドです。

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

接続文字列の名前によって渡されるコンテキストでメソッドを呼び出す、`DbContextOptionsBuilder`オブジェクト。 ローカルの開発、 [ASP.NET Core 構成システム](xref:fundamentals/configuration/index)から接続文字列を読み取り、*される appsettings.json*ファイル。

追加`using`ステートメント`ContosoUniversity.Data`と`Microsoft.EntityFrameworkCore`名前空間。 プロジェクトをビルドします。

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Usings)]

開く、*される appsettings.json*ファイルし、次のコードに示すように、接続文字列を追加します。

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

上記の接続文字列を使用して`ConnectRetryCount=0`を防ぐために[SQLClient](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework)ハングアップします。

### <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

接続文字列では、SQL Server LocalDB データベースを指定します。 LocalDB は、SQL Server Express データベース エンジンの簡易バージョンがあり、アプリの開発では、実稼働環境を使用しないものです。 LocalDB は要求時に開始され、ユーザー モードで実行されるため、複雑な構成はありません。 既定では、LocalDB が作成されます*.mdf* DB ファイル、`C:/Users/<user>`ディレクトリ。

## <a name="add-code-to-initialize-the-db-with-test-data"></a>テスト データを使用して DB に初期化するコードを追加します。

EF Core では、空のデータベースを作成します。 このセクションで、*シード*メソッドは、テスト データを設定に書き込まれます。

*データ*フォルダー、という名前の新しいクラス ファイルを作成*DbInitializer.cs*し、次のコードを追加します。

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

コードでは、db では、任意の受講者があるかどうかを確認します。 Db に受講者がしない場合にテスト データ、DB がシード処理します。 テスト データを配列に読み込むのではなく`List<T>`パフォーマンスを最適化するコレクション。

`EnsureCreated`メソッドは、DB の DB コンテキストを自動的に作成されます。 DB が存在する場合、 `EnsureCreated` DB を変更することがなくを返します。

*Program.cs*、変更、`Main`メソッドを次の操作します。

* 依存関係性の注入コンテナーからの DB コンテキストのインスタンスを取得します。
* コンテキストを引き渡しシード メソッドを呼び出します。
* Seed メソッドの完了時に、コンテキストを破棄します。

次のコードは、更新された*Program.cs*ファイル。

[!code-csharp[Main](intro/samples/cu/ProgramOriginal.cs?name=snippet)]

初めてのアプリを実行すると、DB が作成され、テスト データのシード処理します。 データ モデルが更新されたとき。
* データベースを削除します。
* Seed メソッドを更新します。
* アプリを実行して、新しいシード DB を作成します。 

以降のチュートリアルでは、データ モデルの変更、削除してデータベースを再作成するときに、DB が更新されます。

<a name="pmc"></a>
## <a name="add-scaffold-tooling"></a>Scaffold ツールを追加します。

このセクションでは、パッケージ マネージャー コンソール (PMC) を使用して、Visual Studio web コード生成のパッケージを追加します。 スキャフォールディング エンジンを実行するには、このパッケージが必要です。

**[ツール]** メニューで、**[NuGet パッケージ マネージャー]**、**[パッケージ マネージャー コンソール]** の順に選択します。

パッケージ マネージャー コンソール (PMC) では、次のコマンドを入力します。

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Utils
```

前のコマンドは、*.csproj ファイルには NuGet パッケージを追加します。

[!code-csharp[Main](intro/samples/cu/ContosoUniversity1_csproj.txt?highlight=7-8)]

<a name="scaffold"></a>
## <a name="scaffold-the-model"></a>Scaffold モデル

* プロジェクト ディレクトリ (*Program.cs*、*Startup.cs*、および *.csproj* ファイルを含むディレクトリ) でコマンド ウィンドウを開きます。
* 次のコマンドを実行します。


 ```console
dotnet restore
dotnet aspnet-codegenerator razorpage -m Student -dc SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
 ```
 
場合は、次のエラーが生成されます。

```text
Unhandled Exception: System.IO.FileNotFoundException: 
Could not load file or assembly 
'Microsoft.VisualStudio.Web.CodeGeneration.Utils, 
Version=2.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60'.
The system cannot find the file specified.
```

コマンドを再実行し、ページの下部にあるコメントのままにします。

エラーが発生した場合は、次のようにします。
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

プロジェクト ディレクトリ (*Program.cs*、*Startup.cs*、および *.csproj* ファイルを含むディレクトリ) でコマンド ウィンドウを開きます。


プロジェクトをビルドします。 ビルドには、次のようなエラーが生成されます。

 `1>Pages\Students\Index.cshtml.cs(26,38,26,45): error CS1061: 'SchoolContext' does not contain a definition for 'Student'`

 グローバルに変更する`_context.Student`に`_context.Students`(つまり、"s"を追加する`Student`)。 7 回の出現が検出され、更新します。 修正される予定[この不具合](https://github.com/aspnet/Scaffolding/issues/633)次のリリースでします。

[!INCLUDE[model4tbl](../../includes/RP/model4tbl.md)]

 <a name="test"></a>
### <a name="test-the-app"></a>アプリのテスト

アプリの実行を選択して、**受講者**リンクします。 ブラウザーの幅に応じて、**受講者**リンクは、ページの上部に表示されます。 場合、**受講者**リンクが表示されない、右上隅で、ナビゲーション アイコンをクリックします。

![Contoso 大学のホーム ページの幅の狭い](intro/_static/home-page-narrow.png)

テスト、**作成**、**編集**、および**詳細**リンクします。

## <a name="view-the-db"></a>DB を表示します。

アプリが開始されると、`DbInitializer.Initialize`呼び出し`EnsureCreated`です。 `EnsureCreated`かどうか DB が存在し、必要な場合は作成を検出します。 DB に受講者が存在しない場合、`Initialize`メソッドは、受講者を追加します。

開いている**SQL Server オブジェクト エクスプ ローラー** (SSOX) から、**ビュー** Visual Studio のメニュー。
SSOX、クリックして**(localdb) \MSSQLLocalDB > データベース > ContosoUniversity1**です。

展開して、**テーブル**ノード。

右クリックし、**学生**テーブルし、をクリックして**データの表示**に作成される列とテーブルに挿入された行を参照してください。

*.Mdf*と*.ldf* DB のファイルは、 *C:\Users\\ <yourusername>* フォルダーです。

`EnsureCreated`アプリの起動は、により、次の作業フローに呼び出されます。

* データベースを削除します。
* DB のスキーマを変更 (たとえば、追加、`EmailAddress`フィールド)。
* アプリを実行します。

`EnsureCreated`使用して DB を作成、`EmailAddress`列です。

## <a name="conventions"></a>規約

EF のコアが完全なデータベースを作成するために記述されたコードの量は、規則、または EF コアは、前提を使用するためは最小限です。

* 名前`DbSet`プロパティ テーブル名として使用されます。 参照されていないエンティティに対して、`DbSet`プロパティ、エンティティ クラスの名前テーブル名として使用されます。

* エンティティのプロパティ名は、列名に使用されます。

* ID または classnameID という名前はエンティティのプロパティは、主キー プロパティとして認識されます。

* という名前が場合、プロパティが外部キーのプロパティとして解釈されます *<navigation property name> <primary key property name>*  (たとえば、`StudentID`の`Student`以降のナビゲーション プロパティ、`Student`エンティティの主キーとは`ID`). 外部キーのプロパティの名前を指定できます *<primary key property name>*  (たとえば、`EnrollmentID`ので、`Enrollment`エンティティの主キーが`EnrollmentID`)。

従来の動作をオーバーライドできます。 たとえば、テーブル名できます明示的に指定する、このチュートリアルで前述したとおりです。 列名を明示的に設定することができます。 主キーと外部キーは、明示的に設定することができます。

## <a name="asynchronous-code"></a>非同期コード

非同期プログラミングは、ASP.NET Core と EF コアの既定モードです。

Web サーバーは、使用可能なスレッド数を限定を持ち、負荷が高い状況でのすべての利用可能なスレッドがありますで使用します。 そのような場合は、サーバーは、スレッドが解放されるまで新しい要求を処理できません。 同期コードはの I/O 完了を待機しているため、実際には、作業の実行されない中に多数のスレッド関連付ける可能性があります。 非同期コードは、プロセスが完了するには I/O の待機している場合、他の要求を処理するために使用するサーバー用に、スレッドが解放されます。 その結果、非同期コード サーバー リソースをより効率的に使用でき、遅延なしのより多くのトラフィックを処理するサーバーが有効になっています。

非同期コードには、実行時に少量のオーバーヘッドがについて説明します。 トラフィックが少ない場合は、パフォーマンスの低下はごくわずかであり、中に大量のトラフィックの場合、潜在的なパフォーマンスの向上は大きなです。

次のコードで、`async`キーワード、`Task<T>`値を返す`await`キーワード、および`ToListAsync`メソッドが非同期的に実行するコードを作成します。

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* `async`キーワードようコンパイラに指示します。

  * メソッドの本体の各部分のコールバックを生成します。
  * 自動的に作成、[タスク](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?view=netframework-4.7)返されるオブジェクト。 詳細については、次を参照してください。[タスクの戻り値の型](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType)です。

* 暗黙の型を返す`Task`進行中の作業を表します。

* `await`キーワードによって、コンパイラにメソッドを 2 つの部分に分割します。 最初の部分は、非同期的に開始される操作を終了します。 2 番目の部分は、操作が完了したときに呼び出されるコールバック メソッドに配置されます。

* `ToListAsync`非同期バージョンの`ToList`拡張メソッド。

EF コアを使用する非同期コードを記述する場合の注意すべき点がいくつか:

* クエリまたはコマンドのデータベースに送信されるステートメントだけが非同期的に実行されます。 含む`ToListAsync`、 `SingleOrDefaultAsync`、 `FirstOrDefaultAsync`、および`SaveChangesAsync`です。 だけを変更するステートメントを含まない、`IQueryable`など`var students = context.Students.Where(s => s.LastName == "Davolio")`です。

* EF コア コンテキストを安全なスレッド化されていない: を並列で複数の操作を行うにはしないでください。 

* 非同期コードのパフォーマンスの利点を利用するには、DB にクエリを送信する EF コア メソッドを呼び出す場合に、ライブラリ パッケージ (ページングなど) で非同期を使用することを確認します。

.NET における非同期プログラミングの詳細については、次を参照してください。 [Async 概要](https://docs.microsoft.com/dotnet/articles/standard/async)です。

チュートリアルでは、[次へ]、基本的な CRUD (作成、読み取り、更新、削除) の操作について説明します。

>[!div class="step-by-step"]
[次へ](xref:data/ef-rp/crud)
