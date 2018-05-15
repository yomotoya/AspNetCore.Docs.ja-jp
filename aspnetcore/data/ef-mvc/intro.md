---
title: ASP.NET Core MVC と Entity Framework Core - チュートリアル 1 (全 10 回)
author: tdykstra
description: ''
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/intro
ms.openlocfilehash: eaa3070e182b161087185bbb9007e8067052d95c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-core-mvc-with-entity-framework-core---tutorial-1-of-10"></a>ASP.NET Core MVC と Entity Framework Core - チュートリアル 1 (全 10 回)

作成者: [Tom Dykstra](https://github.com/tdykstra)、[Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [RP better than MVC](../../includes/RP-EF/rp-over-mvc.md)]

Contoso University のサンプル Web アプリケーションでは、Entity Framework (EF) Core 2.0 と Visual Studio 2017 を使用して ASP.NET Core 2.0 MVC Web アプリケーションを作成する方法を示します。

サンプル アプリケーションは架空の Contoso University の Web サイトです。 学生の受け付け、講座の作成、講師の割り当てなどの機能が含まれています。 これは、Contoso University のサンプル アプリケーションを一から作成する方法を説明するチュートリアル シリーズの 1 回目です。

[完成したアプリケーションをダウンロードまたは表示する。](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

EF Core 2.0 は EF の最新版ですが、EF 6.x の一部の機能にまだ対応していません。 EF 6.x と EF Core のどちらを選択するかについては、「[EF Core と EF 6.x を比較する](https://docs.microsoft.com/ef/efcore-and-ef6/)」を参照してください。 EF 6.x を選択する場合、[このチュートリアル シリーズの以前のバージョン](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application)を参照してください。

> [!NOTE]
> * このチュートリアルの ASP.NET Core 1.1 バージョンについては、[このチュートリアルの VS 2017 Update 2 バージョン (PDF 形式)](https://github.com/aspnet/Docs/blob/master/aspnetcore/data/ef-mvc/intro/_static/efmvc1.1.pdf) を参照してください。
> * このチュートリアルの Visual Studio 2015 バージョンについては、[VS 2015 バージョンの ASP.NET Core ドキュメント (PDF 形式)](https://github.com/aspnet/Docs/blob/master/aspnetcore/common/_static/aspnet-core-project-json.pdf) を参照してください。

## <a name="prerequisites"></a>必須コンポーネント

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="troubleshooting"></a>トラブルシューティング

解決できない問題に遭遇した場合、通常、[完成済みのプロジェクト](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)と自分のコードを比較することで解決策がわかります。 一般的なエラーとその解決方法の一覧については、[チュートリアル シリーズの後半に登場するトラブルシューティング セクション](advanced.md#common-errors)をご覧ください。 そこで必要な答えが見つからない場合、StackOverflow.com で [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) または [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core) に関する質問を投稿できます。

> [!TIP] 
> これは 10 回のチュートリアルからなるシリーズであり、いずれの回も前のチュートリアルを基盤にしています。 チュートリアルが完了したら、毎回、プロジェクトのコピーを保存するようお勧めします。 問題に遭遇したとき、前のチュートリアルから始めることができます。シリーズ全体の始めまで戻る必要がありません。

## <a name="the-contoso-university-web-application"></a>Contoso University Web アプリケーション

一連のチュートリアルで作成するアプリケーションは、簡単な大学向け Web サイトです。

ユーザーは学生、講座、講師の情報を見たり、更新したりできます。 次のような画面をこれから作成します。

![Students インデックス ページ](intro/_static/students-index.png)

![Students 編集ページ](intro/_static/student-edit.png)

このサイトの UI スタイルは、組み込みテンプレートで生成されるスタイルに近いものになっています。それにより、このチュートリアルでは主に、Entity Framework の使い方を取り上げることができます。

## <a name="create-an-aspnet-core-mvc-web-application"></a>ASP.NET Core MVC Web アプリケーションを作成する

Visual Studio を開き、新しい ASP.NET Core C# Web プロジェクトを作成します。プロジェクトの名前は "ContosoUniversity" です。

* **[ファイル]** メニューで **[新規作成]、[プロジェクト]** の順に選択します。

* 左側のウィンドウで、**[インストール済み]、[Visual C#]、[Web]** の順に選択します。

* **[ASP.NET Core Web アプリケーション]** プロジェクト テンプレートを選択します。

* 名前に「**ContosoUniversity**」と入力し、**[OK]** をクリックします。

  ![[新しいプロジェクト] ダイアログ](intro/_static/new-project.png)

* **[新しい ASP.NET Core Web アプリケーション (.NET Core)]** ダイアログが表示されるのを待ちます

* **[ASP.NET Core 2.0]** と **[Web アプリケーション (モデル ビュー コントローラー)]** テンプレートを選択します。

  **注:** このチュートリアルには、2.0 以降の ASP.NET Core と EF Core が必要です。**ASP.NET Core 1.1** が選択されていないことを確認してください。

* **[認証]** に **[認証なし]** が設定されていることを確認してください。

* **[OK]** をクリックします。

  ![[新しい ASP.NET プロジェクト] ダイアログ](intro/_static/new-aspnet.png)

## <a name="set-up-the-site-style"></a>サイトのスタイルを設定する

簡単な変更をいくつか行い、サイトのメニュー、レイアウト、ホーム ページを決めます。

*Views/Shared/_Layout.cshtml* を開き、次のように変更します。

* "ContosoUniversity" をすべて "Contoso University" に変更します。 これは 3 回出てきます。

* メニュー エントリとして「**Students**」、「**Courses**」、「**Instructors**」、「**Departments**」を追加し、「**Contact**」を削除します。

変更が強調表示されます。

[!code-cshtml[](intro/samples/cu/Views/Shared/_Layout.cshtml?highlight=6,30,36-39,48)]

*Views/Home/Index.cshtml* で、ファイルの中身を次のコードに変更し、ASP.NET と MVC に関するテキストをこのアプリケーションに関するテキストに変更します。

[!code-cshtml[](intro/samples/cu/Views/Home/Index.cshtml)]

CTRL を押しながら F5 を押してプロジェクトを実行するか、メニューで **[デバッグ]、[デバッグなしで開始]** の順に選択します。 一連のチュートリアルで作成するページのホーム ページとタブが表示されます。

![Contoso University のホーム ページ](intro/_static/home-page.png)

## <a name="entity-framework-core-nuget-packages"></a>Entity Framework Core NuGet パッケージ

プロジェクトに EF Core サポートを追加するには、対象とするデータベース プロバイダーをインストールします。 このチュートリアルでは SQL Server を使用します。プロバイダー パッケージは [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/) です。 このパッケージは [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) メタパッケージに含まれています。インストールする必要はありません。

このパッケージとその依存関係 (`Microsoft.EntityFrameworkCore` と `Microsoft.EntityFrameworkCore.Relational`) により、EF のランタイム サポートが与えられます。 後の[移行](migrations.md)チュートリアルでツール パッケージを追加します。 

Entity Framework Core で利用できるその他のデータベース プロバイダーに関しては、「[データベース プロバイダー](https://docs.microsoft.com/ef/core/providers/)」を参照してください。

## <a name="create-the-data-model"></a>データ モデルの作成

次に、Contoso University アプリケーションのエンティティ クラスを作成します。 次の 3 つのエンティティから始めます。

![講座、登録、学生からなるデータ モデルの図](intro/_static/data-model-diagram.png)

`Student` エンティティと `Enrollment` エンティティの間に一対多の関係があり、`Course` エンティティと `Enrollment` エンティティの間に一対多の関係があります。 言い換えると、1 人の学生をさまざまな講座に登録し、1 つの講座にたくさんの学生を登録できます。

次のセクションでは、エンティティごとにクラスを作成します。

### <a name="the-student-entity"></a>Student エンティティ

![Student エンティティの図](intro/_static/student-entity.png)

*[Models]* フォルダーで、*Student.cs* という名前のクラス ファイルを作成し、テンプレート コードを次のコードに変更します。

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

`ID` プロパティは、このクラスに相当するデータベース テーブルの主キー列になります。 既定では、Entity Framework は、`ID` または `classnameID` という名前のプロパティを主キーとして解釈します。

`Enrollments` プロパティはナビゲーション プロパティです。 ナビゲーション プロパティには、このエンティティに関連する他のエンティティが含まれます。 この例では、`Student entity` の `Enrollments` プロパティで、その `Student` エンティティに関連するすべての `Enrollment` エンティティが保持されます。 つまり、データベースの Student 行にある 2 つの Enrollment 行が関連している場合 (外部キー列 StudentID にその学生の主キー値が含まれる行)、その `Student` エンティティの `Enrollments` ナビゲーション プロパティにその 2 つの `Enrollment` エンティティが含まれます。

ナビゲーション プロパティに複数のエンティティが含まれる場合 (多対多または一対多の関係で)、その型はリストにする必要があります。`ICollection<T>` のように、エンティティを追加、削除、更新できるリストです。 `ICollection<T>`、または `List<T>` や `HashSet<T>` などの型を指定することができます。 `ICollection<T>` を指定した場合、EF では既定で `HashSet<T>` コレクションが作成されます。

### <a name="the-enrollment-entity"></a>Enrollment エンティティ

![Enrollment エンティティの図](intro/_static/enrollment-entity.png)

*[Models]* フォルダーで、*Enrollment.cs* を作成し、既存のコードを次のコードに変更します。

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

`EnrollmentID` プロパティは主キーになります。このエンティティは、`Student` エンティティと同様に、`ID` ではなく `classnameID` パターンを使用します。 通常、パターンを 1 つ選択し、データ モデル全体でそれを使用します。 ここのバリエーションから、いずれのパターンも利用できることがわかります。 [後のチュートリアル](inheritance.md)では、クラス名なしの ID を利用し、データ モデルに継承を簡単に実装する方法を学習します。

`Grade` プロパティは `enum` です。 型宣言 `Grade` の後の疑問符は、`Grade` プロパティが null 許容であることを示します。 null という成績は 0 点とは異なります。null は点数がわからないことか、まだ採点されていないことを意味します。

`StudentID` プロパティは外部キーです。それに対応するナビゲーション プロパティは `Student` です。 `Enrollment` エンティティは 1 つの `Student` エンティティに関連付けられており、1 つの `Student` エンティティだけを保持できます (先に見た、複数の `Enrollment` エンティティを保持できる `Student.Enrollments` ナビゲーション プロパティとは異なります)。

`CourseID` プロパティは外部キーです。それに対応するナビゲーション プロパティは `Course` です。 `Enrollment` エンティティは 1 つの `Course` エンティティに関連付けられます。

Entity Framework は `<navigation property name><primary key property name>` という名前が付いている場合、プロパティを外部キー プロパティとして解釈します。たとえば、`Student` ナビゲーション プロパティの `StudentID` です。`Student` エンティティの主キーが `ID` であるためです。 外部キーにも `<primary key property name>` という単純な名前を付けることができます。たとえば、`CourseID` です。`Course` エンティティの主キーが `CourseID` であるためです。

### <a name="the-course-entity"></a>Course エンティティ

![Course エンティティの図](intro/_static/course-entity.png)

*[Models]* フォルダーで、*Course.cs* を作成し、既存のコードを次のコードに変更します。

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

`Enrollments` プロパティはナビゲーション プロパティです。 1 つの `Course` エンティティにたくさんの `Enrollment` エンティティを関連付けることができます。

`DatabaseGenerated` 属性については、このシリーズの[後のチュートリアル](complex-data-model.md)で詳しく学習します。 基本的に、この属性によって、講座の主キーをデータベースに生成させず、自分で入力できるようになります。

## <a name="create-the-database-context"></a>データベース コンテキストの作成

所与のデータ モデルの Entity Framework 機能を調整するメイン クラスは、データベース コンテキスト クラスです。 このクラスは、`Microsoft.EntityFrameworkCore.DbContext` クラスから派生させて作成します。 自分のコードでは、データ モデルに含めるエンティティを自分で指定します。 Entity Framework の特定の動作をカスタマイズすることもできます。 このプロジェクトでは、クラスに `SchoolContext` という名前が付けられています。

プロジェクト フォルダーで、*Data* という名前のフォルダーを作成します。

*[Data]* フォルダーで、*SchoolContext.cs* という名前の新しいクラス ファイルを作成し、テンプレート コードを次のコードに変更します。

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

このコードによって、エンティティ セットごとに `DbSet` プロパティが作成されます。 Entity Framework の用語では、エンティティ セットは通常はデータベース テーブルに対応し、エンティティはテーブルの行に対応します。

`DbSet<Enrollment>` ステートメントと `DbSet<Course>` ステートメントは省略しても同じ動作をします。 Entity Framework にはそれらが暗黙的に含まれることがあります。`Student` エンティティが `Enrollment` エンティティを参照し、`Enrollment` エンティティが `Course` エンティティを参照するためです。

データベースが作成されると、EF によって、`DbSet` プロパティと同じ名前を持つテーブルが作成されます。 一般的にコレクションのプロパティ名は複数形 (Student ではなく、Students) ですが、テーブル名を複数にするかどうかについては、開発者の間で意見が分かれています。 このチュートリアル シリーズでは、DbContext に単数のテーブル名を指定して既定の動作をオーバーライドします。 そのために、最後の DbSet プロパティの後に、次の強調表示されているコードを追加します。

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a>依存関係の挿入にコンテキストを登録する

ASP.NET Core は既定で[依存関係の挿入](../../fundamentals/dependency-injection.md)を実装します。 サービス (EF データベース コンテキストなど) は、アプリケーションの起動時に依存関係の挿入に登録されます。 これらのサービス (MVC コント ローラーなど) を必要とするコンポーネントには、コンストラクターのパラメーターを介してこれらのサービスが指定されます。 このチュートリアルの後半で、コンテキスト インスタンスを取得するコントローラー コンストラクター コードが登場します。

`SchoolContext` をサービスとして登録するには、*Startup.cs* を開き、強調表示されている行を `ConfigureServices` メソッドに追加します。

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

`DbContextOptionsBuilder` オブジェクトでメソッドが呼び出され、接続文字列の名前がコンテキストに渡されます。 ローカル開発の場合、[ASP.NET Core 構成システム](xref:fundamentals/configuration/index)が *appsettings.json* ファイルから接続文字列を読み取ります。

名前空間の `ContosoUniversity.Data` と `Microsoft.EntityFrameworkCore` に対して `using` ステートメントを追加し、プロジェクトをビルドします。

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_Usings)]

*appsettings.json* ファイルを開き、次のサンプルのように接続文字列を追加します。

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

### <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

この接続文字列によって SQL Server LocalDB データベースが指定されます。 LocalDB は SQL Server Express データベース エンジンの軽量版であり、実稼働ではなく、アプリケーションの開発を意図して設計されています。 LocalDB は要求時に開始され、ユーザー モードで実行されるため、複雑な構成はありません。 既定では、LocalDB は `C:/Users/<user>` ディレクトリに *.mdf* データベース ファイルを作成します。

## <a name="add-code-to-initialize-the-database-with-test-data"></a>テスト データでデータベースを初期化するコードを追加する

Entity Framework によって空のデータベースが自動的に作成されます。 このセクションでは、テスト データを入力する目的で、データベースの作成後に呼び出されるメソッドを記述します。

ここでは、データベースを自動的に作成する `EnsureCreated` メソッドを使用します。 [後のチュートリアル](migrations.md)では、モデル変更の処理方法について学習します。データベースを削除し、再作成するのではなく、Code First Migrations を利用してデータベース スキーマを変更します。

*[データ]* フォルダーで *DbInitializer.cs* という名前の新しいクラス ファイルを作成し、テンプレート コードを次のコードに変更します。このコードにより、必要なときにデータベースが作成され、新しいデータベースにテスト データが読み込まれます。

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

このコードはデータベースに学生が存在するかどうかを確認し、存在しない場合、そのデータベースは新しく、テスト データを入力する必要があると見なします。 `List<T>` コレクションではなく配列にテスト データを読み込み、パフォーマンスを最適化します。

*Program.cs* で、アプリケーションの起動時に次を実行するように `Main` メソッドを変更します。

* 依存関係挿入コンテナーからデータベース コンテキスト インスタンスを取得します。
* seed メソッドを呼び出し、コンテキストを渡します。
* seed メソッドが完了したら、コンテキストを破棄します。

[!code-csharp[](intro/samples/cu/Program.cs?name=snippet_Seed&highlight=3-20)]

`using` ステートメントを追加します。

[!code-csharp[](intro/samples/cu/Program.cs?name=snippet_Usings)]

以前のチュートリアルでは、*Startup.cs* で `Configure` メソッドと同様のコードを確認できるかもしれません。 要求パイプラインを設定する目的でのみ `Configure` メソッドを利用することをお勧めします。 アプリケーションの起動コードは、`Main` メソッドに属します。

これからアプリケーションを初めて実行します。データベースが作成され、テスト データが入力されます。 データ モデルを変更するたびに、データベースを削除し、seed メソッドを更新し、新しいデータベースで同様にやり直すことができます。 後のチュートリアルでは、データ モデルが変わったとき、データベースを削除して作り直すのではなく、修正する方法について学習します。

## <a name="create-a-controller-and-views"></a>コントローラーとビューの作成

次に、Visual Studio のスキャフォールディング エンジンを利用し、MVC のコントローラーとビューを追加します。このコントローラーとビューでは、EF を利用してクエリを実行し、データを保存します。

CRUD アクションのメソッドとビューの自動作成は、スキャフォールディングと言います。 一般的には生成されたコードは修正しないのに対し、スキャフォールディングされたコードを開始点として独自の要件に合うように変更できるという点で、スキャフォールディングはコード生成と異なります。 生成されたコードをカスタマイズする必要があるとき、部分クラスを利用するか、状況が変わったときにコードを再生成します。

* **ソリューション エクスプローラー**の **Controllers** フォルダーを右クリックし、**[追加]、[スキャフォールディングされた新しい項目]** の順に選択します。

**[MVC 依存関係の追加]** ダイアログ ボックスが表示された場合は、次のようにします。

* [Visual Studio を最新バージョンに更新します](https://www.visualstudio.com/downloads/)。 15.5 より前のバージョンの Visual Studio の場合はこのダイアログが表示されます。
* 更新できない場合は、**[追加]** を選択してから、もう一度コントローラーの追加手順に従ってください。

* **[スキャフォールディングを追加]** ダイアログ ボックスで:

  * **[Entity Framework を使用したビューがある MVC コントローラー]** を選択します。

  * **[追加]** をクリックします。

* **[コントローラーの追加]** ダイアログ ボックスで:

  * **[モデル クラス]** で **[Student]** を選択します。

  * **[データ コンテキスト クラス]** で **[SchoolContext]** を選択します。

  * 名前は **StudentsController** をそのまま選択します。

  * **[追加]** をクリックします。

  ![Student のスキャフォールディング](intro/_static/scaffold-student.png)

  **[追加]** をクリックすると、Visual Studio スキャフォールディング エンジンは *StudentsController.cs* ファイルと、コントローラーと連動する一連のビュー (*.cshtml* ファイル) を作成します。

(スキャフォールディング エンジンは、このチュートリアルで先に行ったように手動で最初に作成しない場合、データベース コンテキストを自動作成することもできます。 **[コントローラーの追加]** ボックスで新しいコンテキスト クラスを指定できます。**[データ コンテキスト クラス]** の右にあるプラス記号をクリックします。  Visual Studio はコントローラーやビューと共に `DbContext` クラスを作成します。)

コントローラーがコンストラクター パラメーターとして `SchoolContext` を受け取ることがわかります。

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Context&highlight=5,7,9)]

ASP.NET 依存関係挿入では、`SchoolContext` のインスタンスがコントローラーに渡されます。 それは先に *Startup.cs* ファイルで構成しました。

コントローラーには `Index` アクション メソッドが含まれます。これはデータベースにあるすべての学生を表示します。 このメソッドはデータベース コンテキスト インスタンスの `Students` プロパティを読み取り、Students エンティティ セットから学生の一覧を取得します。

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex&highlight=3)]

チュートリアルの後半で、このコードの非同期プログラミング要素について学習します。

*Views/Students/Index.cshtml* ビューには、この一覧で表形式で表示されます。

[!code-cshtml[](intro/samples/cu/Views/Students/Index1.cshtml)]

CTRL を押しながら F5 を押してプロジェクトを実行するか、メニューで **[デバッグ]、[デバッグなしで開始]** の順に選択します。

[Students] タブをクリックすると、`DbInitializer.Initialize` メソッドによって挿入されたテスト データが表示されます。 ブラウザーのウィンドウ幅によって決まることですが、`Student` タブ リンクはページの一番上に表示されるか、右上隅のナビゲーション アイコンをクリックしないと表示されません。

![Contoso University のホーム ページ (ウィンドウ幅が狭いとき)](intro/_static/home-page-narrow.png)

![Students インデックス ページ](intro/_static/students-index.png)

## <a name="view-the-database"></a>データベースを表示する

アプリケーションを起動する葉、`DbInitializer.Initialize` メソッドが `EnsureCreated` を呼び出します。 EF はデータベースがないことを認識し、作成します。`Initialize` メソッド コードの残りの部分により、データベースにデータが入力されます。 **SQL Server Object Explorer** (SSOX) を利用し、Visual Studio でデータベースを表示できます。

ブラウザーを閉じます。

SSOX ウィンドウがまだ開いていない場合、Visual Studio の **[表示]** メニューから選択します。

SSOX で **(localdb)\MSSQLLocalDB > Databases** をクリックし、*appsettings.json* ファイルの接続文字列にあるデータベース名のエントリをクリックします。

**[テーブル]** ノードを展開し、データベースのテーブルを表示します。

![SSOX のテーブル](intro/_static/ssox-tables.png)

**[Student]** テーブルを右クリックし、**[データの表示]** をクリックすると、作成された列とテーブルに挿入された行が表示されます。

![SSOX の Student テーブル](intro/_static/ssox-student-table.png)

<em>.mdf</em> データベース ファイルと <em>.ldf</em> データベース ファイルは <em>C:\Users\\<yourusername></em> フォルダーにあります。

アプリの起動時に実行される初期化子メソッドで `EnsureCreated` を呼び出すため、`Student` クラスを変更し、データベースを削除し、アプリケーションを再実行できます。変更に合わせてデータベースが自動的に再作成されます。 たとえば、`Student` クラスに `EmailAddress` プロパティを追加する場合、再作成されたテーブルに新しい `EmailAddress` 列が表示されます。

## <a name="conventions"></a>規約

規約を利用することや Entity Framework が想定を行うことにより、Entity Framework が完全なデータベースを自動作成するために記述しなければならないコードの量が最小限に抑えられます。

* `DbSet` プロパティの名前がテーブル名として使用されます。 `DbSet` プロパティによって参照されないエンティティについては、エンティティ クラス名がテーブル名として使用されます。

* 列名には、エンティティ プロパティ名が使用されます。

* ID または classnameID という名前が付けられているエンティティ プロパティは主キーのプロパティとして認識されます。

* *<navigation property name><primary key property name>* という名前が付いている場合、プロパティは外部キー プロパティとして解釈されます。たとえば、`Student` ナビゲーション プロパティの `StudentID` です。`Student` エンティティの主キーが `ID` であるためです。 外部キーにも *<primary key property name>* という単純な名前を付けることができます。たとえば、`EnrollmentID` です。`Enrollment` エンティティの主キーが `EnrollmentID` であるためです。

従来の動作をオーバーライドできます。 たとえば、このチュートリアルで先に見たように、テーブル名を明示的に指定できます。 また、列名を設定し、任意のプロパティを主キーまたは外部キーとして設定できます。これについては、このシリーズの[後のチュートリアル](complex-data-model.md)で学習します。

## <a name="asynchronous-code"></a>非同期コード

ASP.NET Core と EF Core では、非同期プログラミングが既定のモードです。

Web サーバーでは、利用できるスレッド数に限りがあります。負荷が高い状況では、利用できるスレッドが全部使われる可能性があります。 その場合、スレッドが解放されるまでサーバーは新しい要求を処理できません。 同期コードの場合、たくさんのスレッドが関連付けられていても、I/O の完了を待っているため、実際には何の作業も行っていないということがあります。 非同期コードの場合、あるプロセスが I/O の完了を待っているとき、多の要求の処理にサーバーが利用できるようにそのスレッドが解放されます。 結果として、非同期コードの場合、サーバー リソースをより効率的に利用できます。サーバーは、より多くのトラフィックを遅延なく処理できます。

非同期コードは実行時に少量のオーバーヘッドを発生させるが、トラフィックが少ない場合、パフォーマンスに与える影響は無視して構わない程度です。トラフィックが多い場合、相当なパフォーマンス改善が見込まれます。

次のコードでは、キーワード `async`、戻り値 `Task<T>`、キーワード `await`、メソッド `ToListAsync` によりコードの実行が非同期になります。

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex)]

* キーワード `async` は、メソッド本文の一部にコールバックを生成し、返された `Task<IActionResult>` オブジェクトを自動作成するようにコンパイラに伝えます。

* 戻り値の型 `Task<IActionResult>` は、進行中の作業と型 `IActionResult` の結果を表します。

* キーワード `await` により、コンパイラはメソッドを 2 つに分割します。 最初の部分は、非同期で開始される操作で終わります。 2 つ目の部分は、操作の完了時に呼び出されるコールバック メソッドに入ります。

* `ToListAsync` は、`ToList` 拡張メソッドの非同期バージョンです。

Entity Framework を利用する非同期コードの記述で注意すべき点:

* クエリやコマンドをデータベースに送信するステートメントのみが非同期で実行されます。 たとえば、`ToListAsync`、`SingleOrDefaultAsync`、`SaveChangesAsync` などです。 `var students = context.Students.Where(s => s.LastName == "Davolio")` など、`IQueryable` を変更するだけのステートメントは含まれません。

* EF コンテキストはスレッド セーフではありません。複数の操作を並列実行しないでください。 非同期 EF メソッドを呼び出すとき、`await` キーワードを常に使用します。

* 非同期コードのパフォーマンス上の利点を最大限に活用する場合、(ページングなどのために) ライブラリ パッケージを利用しているのであれば、それがクエリをデータベースに送信させる Entity Framework メソッドを呼び出す場合、非同期を利用する必要があります。

.NET の非同期プログラミングについては、「[非同期の概要](https://docs.microsoft.com/dotnet/articles/standard/async)」を参照してください。

## <a name="summary"></a>まとめ

これで Entity Framework Core と SQL Server Express LocalDB を利用してデータを保存し、表示する単純なアプリケーションが作成されました。 次のチュートリアルでは、基本的な CRUD (作成、読み取り、更新、削除) 操作を実行する方法について学習します。

> [!div class="step-by-step"]
> [次へ](crud.md)
