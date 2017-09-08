---
title: "ASP.NET MVC を持つコアを Entity Framework Core - 10 のチュートリアル 1"
author: tdykstra
description: 
keywords: "ASP.NET Core、Entity Framework Core、チュートリアル"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: b67c3d4a-f2bf-4132-a48b-4b0d599d7981
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/intro
ms.openlocfilehash: b8ef101458e0a6e6284624693689181646ced051
ms.sourcegitcommit: 5355c96a1768e5a1d5698a98c190e7addcc4ded5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/05/2017
---
# <a name="getting-started-with-aspnet-core-mvc-and-entity-framework-core-using-visual-studio-1-of-10"></a>ASP.NET Core MVC と Visual Studio (10 の 1) を使用して Entity Framework Core の概要

によって[Tom Dykstra](https://github.com/tdykstra)と[Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso 大学でサンプル web アプリケーションでは、Entity Framework (EF) コア 2.0 と Visual Studio 2017 を使用して ASP.NET Core 2.0 MVC web アプリケーションを作成する方法を示します。

サンプル アプリケーションは、架空の Contoso 大学の web サイトです。 学生受付、コースの作成、およびインストラクター割り当てなどの機能が含まれています。 これは、最初の一連の最初から Contoso 大学サンプル アプリケーションをビルドする方法を説明するチュートリアルです。

[ダウンロードまたは完成したアプリケーションを表示します。](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

EF の最新バージョンであるが EF のすべての機能がない、EF コア 2.0 6.x です。 EF との間を選択する方法については 6.x と EF コアを参照してください。 [EF コア vs です。EF6.x](https://docs.microsoft.com/ef/efcore-and-ef6/)です。 EF を選択した場合、6.x を参照してください[このチュートリアル シリーズの以前のバージョン](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application)します。

> [!NOTE]
> * このチュートリアルの ASP.NET Core の 1.1 バージョンを参照してください、 [VS 2017 Update 2 のバージョンを PDF 形式では、このチュートリアルの](https://github.com/aspnet/Docs/blob/master/aspnetcore/data/efmvc/intro/_static/efmvc1.1.pdf)します。
> * このチュートリアルの Visual Studio 2015 バージョンを参照してください、 [VS 2015 バージョンの PDF 形式での ASP.NET Core ドキュメント](https://github.com/aspnet/Docs/blob/master/aspnetcore/common/_static/aspnet-core-project-json.pdf)です。

## <a name="prerequisites"></a>必須コンポーネント

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="troubleshooting"></a>トラブルシューティング

問題を解決できない場合に発生した場合、コードを比較することでのソリューションを見つけることは通常、[完成したプロジェクト](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)です。 一般的なエラーとそれらを解決する方法の一覧は、次を参照してください。[系列の最後のチュートリアルの「トラブルシューティング](advanced.md#common-errors)です。 必要なものがない場合は、StackOverflow.com に質問を投稿することができます[ASP.NET Core](http://stackoverflow.com/questions/tagged/asp.net-core)または[EF コア](http://stackoverflow.com/questions/tagged/entity-framework-core)です。

> [!TIP] 
> これは、一連の 10 のチュートリアルでは、それぞれは、前のチュートリアルの処理に基づいています。  各チュートリアルが正常に完了した後、プロジェクトのコピーを保存することを検討してください。  問題に遭遇した場合は、前のチュートリアルの一連の先頭に戻るとではなく、経由で開始できます。

## <a name="the-contoso-university-web-application"></a>Contoso 大学 web アプリケーション

これらのチュートリアルで構築するアプリケーションは、単純な大学 web サイトです。

ユーザーでは、表示でき、学生、コース、インストラクターの情報を更新することができます。 ここでは、いくつかの画面を作成します。

![インデックス ページの受講者](intro/_static/students-index.png)

![受講者の編集 ページ](intro/_static/student-edit.png)

このサイトの UI スタイルを維持して組み込みのテンプレートによって生成されたものに近いチュートリアルは、Entity Framework を使用する方法の主に集中することです。

## <a name="create-an-aspnet-core-mvc-web-application"></a>ASP.NET Core MVC web アプリケーションを作成します。

Visual Studio を開き、新しい ASP.NET Core c# web という名前のプロジェクト"ContosoUniversity"を作成します。

* **ファイル**メニューの **新規 > プロジェクト**です。

* 左側のウィンドウから次のように選択します。**インストール > Visual c# > Web**です。

* 選択、 **ASP.NET Core Web アプリケーション**プロジェクト テンプレート。

* 入力**ContosoUniversity**クリックと名前として**OK**です。

  ![[新しいプロジェクト] ダイアログ](intro/_static/new-project.png)

* 待って、**新しい ASP.NET Core Web アプリケーション (.NET Core)**ダイアログを表示するには

* 選択**ASP.NET Core 2.0**と**Web アプリケーション (モデル-ビュー-コント ローラー)**テンプレート。

  **注:**このチュートリアルでは、ASP.NET Core 2.0 と EF コア 2.0 以降--、以下のことを確認が必要です**ASP.NET Core 1.1**が選択されていません。

* 確認**認証**に設定されている**認証なし**です。

* [OK] をクリックします。 ****

  ![新しい ASP.NET プロジェクト ダイアログ ボックス](intro/_static/new-aspnet.png)

## <a name="set-up-the-site-style"></a>サイトのスタイルを設定します。

いくつかの簡単な変更は、[サイト] メニューのレイアウト、およびホーム ページを設定します。

開いている*Views/Shared/_Layout.cshtml*次の変更を加えます。

* 「Contoso 大学」を"ContosoUniversity"の各出現する位置を変更します。 次の 3 つの出現があります。

* メニュー エントリを追加**受講者**、**コース**、**講習においてインストラクター**、および**部門**、および削除、 **にお問い合わせください**メニュー エントリです。

変更が強調表示されます。

[!code-html[](intro/samples/cu/Views/Shared/_Layout.cshtml?highlight=6,30,36-39,48)]

*Views/Home/Index.cshtml*ファイルの内容をこのアプリケーションについてのテキストで ASP.NET と MVC に関するテキストを置き換える次のコードに置き換えます。

[!code-html[](intro/samples/cu/Views/Home/Index.cshtml)]

CTRL + f5 キーを押してプロジェクトを実行または選択**デバッグ > デバッグなしで開始** メニューからです。 これらのチュートリアルで作成された、ページのタブで、ホーム ページを参照してください。

![Contoso 大学のホーム ページ](intro/_static/home-page.png)

## <a name="entity-framework-core-nuget-packages"></a>Entity Framework Core NuGet パッケージの管理

プロジェクトに EF Core のサポートを追加するには、対象となるデータベース プロバイダーをインストールします。 このチュートリアルは、SQL Server を使用し、プロバイダー パッケージ[Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/)です。 このパッケージに含まれる、 [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage、ためをインストールする必要はありません。

このパッケージとその依存関係 (`Microsoft.EntityFrameworkCore`と`Microsoft.EntityFrameworkCore.Relational`) EF のランタイム サポートを提供します。 ツール パッケージを追加後の「、[移行](migrations.md)チュートリアルです。 

Entity Framework のコアで利用可能なその他のデータベース プロバイダーについては、次を参照してください。[データベース プロバイダー](https://docs.microsoft.com/ef/core/providers/)です。

## <a name="create-the-data-model"></a>データ モデルを作成します。

次に、Contoso 大学アプリケーション用にエンティティ クラスを作成します。 次の 3 つのエンティティを開始します。

![受講者コース-登録データ モデルのダイアグラム](intro/_static/data-model-diagram.png)

一対多の関係がある`Student`と`Enrollment`エンティティ、一対多の関係があると`Course`と`Enrollment`エンティティです。 つまり、任意の数に、コースの受講者を登録して、コースに受講者に、登録の任意の数を持つことができます。

次のセクションでは、これらのエンティティのいずれかのクラスを作成します。

### <a name="the-student-entity"></a>学生エンティティ

![学生のエンティティの図](intro/_static/student-entity.png)

*モデル*フォルダー、という名前のクラス ファイルを作成する*Student.cs*テンプレート コードを次のコードに置き換えます。

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

`ID`プロパティは、このクラスに対応するデータベース テーブルの主キー列になります。 既定では、Entity Framework では、解釈というプロパティ`ID`または`classnameID`主キーとして。

`Enrollments`プロパティは、ナビゲーション プロパティ。 ナビゲーション プロパティは、このエンティティに関連するその他のエンティティを保持します。 ここで、`Enrollments`のプロパティ、`Student entity`のすべてを保持する、`Enrollment`に関連付けられているエンティティ`Student`エンティティです。 つまり場合、データベース内の特定の学生行が関連する 2 つ登録行 (その StudentID 外部キー列に、そのスチューデントの主キーの値が含まれている行) を`Student`エンティティの`Enrollments`ナビゲーション プロパティで、それらが格納されます2 つ`Enrollment`エンティティです。

その型が一覧にエントリを追加、削除すると、更新できるなどをする必要があります、ナビゲーション プロパティ (多対多または一対多のリレーションシップ) のように複数のエンティティに保持できる場合`ICollection<T>`です。  指定できます`ICollection<T>`またはなど型`List<T>`または`HashSet<T>`です。 指定した場合`ICollection<T>`、EF の作成、`HashSet<T>`既定のコレクション。

### <a name="the-enrollment-entity"></a>登録エンティティ

![エンティティの登録の図](intro/_static/enrollment-entity.png)

*モデル*フォルダー作成*Enrollment.cs*し、既存のコードを次のコードに置き換えます。

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

`EnrollmentID`プロパティは、主キーになります。 このエンティティを使用して、`classnameID`パターンの代わりに`ID`で学習したとしてそれ自体で、`Student`エンティティです。 通常 1 つパターンを選択し、データ モデル全体で使用するとします。 ここでは、いずれかのパターンを使用することができます、バリエーションを示しています。 [後のチュートリアル](inheritance.md)、classname せず ID を使用して簡単方法、データ モデルで継承の実装が表示されます。

`Grade`プロパティは、`enum`です。 後に疑問符 ()、`Grade`型宣言では、ことを示します、`Grade`プロパティが null 値を許容します。 Null である評価とは異なる、ゼロ グレード--null またはため、評価はまだ割り当てられていません。

`StudentID`プロパティは、foreign key、および対応するナビゲーション プロパティは`Student`します。 `Enrollment`エンティティが 1 つに関連付けられた`Student`エンティティ、プロパティは、1 つのみを保持できるように`Student`エンティティ (とは異なり、`Student.Enrollments`ナビゲーション プロパティ先ほど見た、複数を格納する`Enrollment`エンティティ)。

`CourseID`プロパティは、foreign key、および対応するナビゲーション プロパティは`Course`します。 `Enrollment`エンティティが 1 つに関連付けられた`Course`エンティティです。

Entity Framework がという名前が場合に、外部キーのプロパティとしてプロパティを解釈`<navigation property name><primary key property name>`(たとえば、`StudentID`の`Student`以降のナビゲーション プロパティ、`Student`エンティティの主キーが`ID`)。 外部キー プロパティは単にも呼ばれます`<primary key property name>`(たとえば、`CourseID`ので、`Course`エンティティの主キーが`CourseID`)。

### <a name="the-course-entity"></a>Course エンティティ

![Course エンティティの図](intro/_static/course-entity.png)

*モデル*フォルダー作成*Course.cs*し、既存のコードを次のコードに置き換えます。

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

`Enrollments`プロパティは、ナビゲーション プロパティ。 A`Course`エンティティを任意の数に関連付けることができます`Enrollment`エンティティです。

ここでの詳細について、`DatabaseGenerated`属性、[後のチュートリアル](complex-data-model.md)この系列にします。 基本的には、この属性には、それを生成、データベースのではなく、コースのプライマリ キーを入力することができます。

## <a name="create-the-database-context"></a>データベース コンテキストを作成します。

指定されたデータ モデルの Entity Framework 機能を調整するのメイン クラスは、データベース コンテキスト クラスです。 派生することによってこのクラスを作成する、`Microsoft.EntityFrameworkCore.DbContext`クラスです。 コードでは、データ モデルのエンティティが含まれているを指定します。 特定の Entity Framework の動作をカスタマイズすることもできます。 クラスの名前は、このプロジェクトで`SchoolContext`です。

プロジェクト フォルダー内には、という名前のフォルダーを作成*データ*です。

*データ*という新しいクラス ファイルを作成するフォルダー *SchoolContext.cs*、テンプレート コードを次のコードに置き換えます。

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

このコードを作成、`DbSet`各エンティティ セットのプロパティです。 Entity Framework の用語で、エンティティ セットは、通常は、データベース テーブルに対応しています、エンティティをテーブル内の行に対応しています。

省略した可能性がありますが、`DbSet<Enrollment>`と`DbSet<Course>`ステートメントと同じように動作します。 Entity Framework はそれらを含める暗黙的に、`Student`エンティティ参照、`Enrollment`エンティティと`Enrollment`エンティティ参照、`Course`エンティティです。

データベースが作成される、EF 作成と同じ名前を持つテーブルを`DbSet`プロパティの名前。 コレクションのプロパティ名は、通常 (受講者ではなく学生)、複数形が開発者と異なるかどうかテーブル名をする複数化か。 これらのチュートリアル DbContext で単数形のテーブル名を指定して既定の動作をオーバーライドします。 実行するには、DbSet の最後のプロパティの後に、次の強調表示されたコードを追加します。

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a>依存関係の挿入をコンテキストに登録します。

ASP.NET Core を実装する[依存性の注入](../../fundamentals/dependency-injection.md)既定です。 EF データベース コンテキストで) などのサービスは、アプリケーションの起動時に依存関係の挿入に登録されます。 これらのサービス (など、MVC コント ローラー) を必要とするコンポーネントは、コンス トラクターのパラメーターを使用してこれらのサービスに提供されます。 このチュートリアルで後ほどコンテキストのインスタンスを取得するコント ローラーのコンス トラクター コードが表示されます。

登録する`SchoolContext`をサービスとして開く*Startup.cs*を強調表示された行を追加し、`ConfigureServices`メソッドです。

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

接続文字列の名前によって渡されるコンテキストでメソッドを呼び出す、`DbContextOptionsBuilder`オブジェクト。 ローカルの開発、 [ASP.NET Core 構成システム](../../fundamentals/configuration.md)から接続文字列を読み取り、*される appsettings.json*ファイル。

追加`using`に対してステートメントを`ContosoUniversity.Data`と`Microsoft.EntityFrameworkCore`名前空間、し、プロジェクトを作成します。

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Usings)]

開く、*される appsettings.json*ファイルし、次の例に示すように、接続文字列を追加します。

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

### <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

接続文字列では、SQL Server LocalDB データベースを指定します。 LocalDB は、SQL Server Express データベース エンジンの簡易バージョンがあり、アプリケーションの開発では、実稼働環境を使用しないものです。 LocalDB は、要求時に開始され、ユーザー モードで実行されるため、複雑な構成はありません。 既定では、LocalDB が作成されます*.mdf*データベース内のファイル、`C:/Users/<user>`ディレクトリ。

## <a name="add-code-to-initialize-the-database-with-test-data"></a>データベースにテスト データを初期化するコードを追加します。

Entity Framework を空のデータベースに作成されます。  このセクションで、テスト データを設定するために、データベースが作成された後に呼び出されるメソッドを記述します。

ここで使用する、`EnsureCreated`メソッドを自動的にデータベースを作成します。 [後のチュートリアル](migrations.md)を削除して、データベースを再作成ではなく、データベース スキーマを変更する Code First Migrations を使用してモデルの変更を処理する方法について説明します。

*データ*フォルダー、という名前の新しいクラス ファイルを作成する*DbInitializer.cs*により必要な時に作成されるデータベースの次のコードで、テンプレート コードを置き換えるし、ロード テストを新しいデータデータベースです。

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

コードは、場合は、データベースが新しいと、テスト データをシード処理する必要がありますいない場合は、想定していますが、データベースの任意の受講者を確認します。  テスト データを配列に読み込むのではなく`List<T>`パフォーマンスを最適化するコレクション。

*Program.cs*、変更、`Main`メソッドをアプリケーションの起動時に次の操作します。

* 依存関係性の注入コンテナーからのデータベース コンテキストのインスタンスを取得します。
* コンテキストを引き渡しシード メソッドを呼び出します。
* Seed メソッドを実行する場合は、コンテキストを破棄します。

[!code-csharp[Main](intro/samples/cu/Program.cs?name=snippet_Seed&highlight=3-20)]

追加`using`ステートメント。

[!code-csharp[Main](intro/samples/cu/Program.cs?name=snippet_Usings)]

古いチュートリアルは、同様のコードを生じる可能性があります、`Configure`メソッド*Startup.cs*です。 使用することをお勧め、`Configure`メソッド、要求パイプラインを設定するだけです。 アプリケーションのスタートアップ コードが属している、`Main`メソッドです。

今すぐ初めてアプリケーションを実行するデータベースが作成され、テスト データのシード処理します。 データ モデルを変更するたびには、データベースを削除して、そのシード メソッドを更新して開始した後もう一度新しいデータベースと同じ方法ことができます。 以降のチュートリアルでは、データ モデルの変更、削除して再作成するときに、データベースを変更する方法が表示されます。

## <a name="create-a-controller-and-views"></a>コント ローラーとビューを作成します。

次に、ある MVC コント ローラーとクエリおよびデータを保存するのに EF を使用するビューを追加するのに Visual Studio でスキャフォールディング エンジンを使用します。

CRUD アクション メソッドとビューの自動作成は、スキャフォールディングと呼ばれます。 スキャフォールディングは、スキャフォールディング コードが通常生成されたコードを変更しない一方、独自の要件に合わせて変更できる開始点に、コードの生成とは異なります。 生成されたコードをカスタマイズする必要がある場合は、部分クラスを使用するまたは変更発生時に、コードが再生成します。

* 右クリックし、**コント ローラー**フォルダー**ソリューション エクスプ ローラー**選択**追加 > スキャフォールディングされた新しい項目**です。

* **MVC 依存関係の追加**ダイアログで、**最小の依存関係**を選択して**追加**です。

  ![依存関係を追加します。](intro/_static/add-depend.png)

  Visual Studio では、コント ローラーをスキャフォールディングするために必要な依存関係を追加します。 プロジェクト ファイル内の唯一の違いは、の追加、`Microsoft.VisualStudio.Web.CodeGeneration.Design`パッケージです。

  A *ScaffoldingReadMe.txt*ファイルが作成、削除可能です。

* もう一度右クリックし、**コント ローラー**フォルダー**ソリューション エクスプ ローラー**選択**追加 > スキャフォールディングされた新しい項目**です。

* **追加 Scaffold**  ダイアログ ボックス。

  * 選択**Entity Framework を使用して、ビューがある MVC コント ローラー**です。

  * **[追加]**をクリックします。

* **コント ローラーの追加** ダイアログ ボックス。

  * **モデル クラス**選択**学生**です。

  * **データ コンテキスト クラス**選択**SchoolContext**です。

  * 既定値を受け入れる**StudentsController**名として。

  * **[追加]**をクリックします。

  ![Scaffold 受講者](intro/_static/scaffold-student.png)

  クリックすると、**追加**、Visual Studio のスキャフォールディング エンジンを作成、 *StudentsController.cs*ファイルと、一連のビュー (*.cshtml*ファイル)、コント ローラーで動作します。

(スキャフォールディング エンジンも、データベース コンテキストを作成手動で作成しない場合はこのチュートリアルの前に行ったようにまずします。 新しいコンテキスト クラスを指定することができます、**コント ローラーの追加**の右側にあるプラス記号をクリックしてボックス**データ コンテキスト クラス**です。  Visual Studio は作成し、`DbContext`コント ローラーとビューだけでなくクラスです)。

コント ローラーを取ることがわかります、`SchoolContext`コンス トラクターのパラメーターとして。

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Context&highlight=5,7,9)]

ASP.NET の依存関係の挿入のインスタンスを渡す場合の注意`SchoolContext`コント ローラーにします。 構成されている、 *Startup.cs*前ファイルします。

コント ローラーが含まれています、`Index`アクション メソッドは、データベース内のすべての受講者を表示します。 メソッドは、受講者のエンティティを読み取ってセットから受講者の一覧を取得、`Students`データベース コンテキストのインスタンスのプロパティ。

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex&highlight=3)]

このコードで非同期のプログラミング要素は、このチュートリアルで後ほどについて説明します。

*Views/Students/Index.cshtml*ビューは、テーブルのこの一覧を表示します。

[!code-html[](intro/samples/cu/Views/Students/Index1.cshtml)]

CTRL + f5 キーを押してプロジェクトを実行または選択**デバッグ > デバッグなしで開始** メニューからです。

テスト データを表示する、受講者タブをクリックする、`DbInitializer.Initialize`メソッドを挿入します。 どの幅の狭い、ブラウザー ウィンドウがによってが表示されます、`Student`ページの上部にあるタブ リンクは、リンクを表示する右上隅で、ナビゲーション アイコンをクリックする必要があります。

![Contoso 大学のホーム ページの幅の狭い](intro/_static/home-page-narrow.png)

![インデックス ページの受講者](intro/_static/students-index.png)

## <a name="view-the-database"></a>データベースを表示します。

アプリケーションを起動したときに、`DbInitializer.Initialize`メソッド呼び出し`EnsureCreated`です。 EF データベースがありませんでした。 そのために作成され、1 つの残りの部分の説明、`Initialize`メソッドのコードには、データベースにデータが設定されます。 使用することができます**SQL Server オブジェクト エクスプ ローラー** Visual Studio でデータベースを表示するには、(SSOX)。

ブラウザーを閉じます。

SSOX ウィンドウが開いていない場合を選択してから、**ビュー** Visual Studio のメニュー。

SSOX、クリックして**(localdb) \MSSQLLocalDB > データベース**、内の接続文字列に含まれるデータベース名のエントリをクリックして、*される appsettings.json*ファイル。

展開して、**テーブル**ノードをデータベース内のテーブルを参照してください。

![SSOX 内のテーブル](intro/_static/ssox-tables.png)

右クリックし、**学生**テーブルし、をクリックして**ビュー データ**に作成された列とテーブルに挿入された行を参照してください。

![SSOX で student テーブル](intro/_static/ssox-student-table.png)

*.Mdf*と*.ldf*データベース ファイルは、 *C:\Users\<yourusername >*フォルダーです。

呼び出しているため`EnsureCreated`アプリの起動で実行されている初期化メソッドででした今すぐ変更を行うには`Student`クラス、データベースを削除して、もう一度、アプリケーションを実行し、するとデータベースに自動的に変更内容を一致するように再作成します。 たとえば、追加する場合、`EmailAddress`プロパティを`Student`クラスが表示されます、新しい`EmailAddress`再作成されたテーブル内の列です。

## <a name="conventions"></a>規則

完全なデータベースを作成できるように、Entity Framework の順序で記述したコードの量は、規則、または Entity Framework は、前提を使用するためは最小限です。

* 名前`DbSet`プロパティ テーブル名として使用されます。 参照されていないエンティティに対して、`DbSet`プロパティ、エンティティ クラスの名前テーブル名として使用されます。

* エンティティのプロパティ名は、列名に使用されます。

* ID または classnameID という名前はエンティティのプロパティは、主キー プロパティとして認識されます。

* という名前が場合、プロパティが外部キーのプロパティとして解釈されます *<navigation property name> <primary key property name>*  (たとえば、`StudentID`の`Student`以降のナビゲーション プロパティ、`Student`エンティティの主キーとは`ID`). 外部キー プロパティは単にも呼ばれます *<primary key property name>*  (たとえば、`EnrollmentID`ので、`Enrollment`エンティティの主キーが`EnrollmentID`)。

従来の動作をオーバーライドできます。 たとえば、このチュートリアルで既に説明したとおり、テーブル名を明示的に指定することができます。 列名を設定してでわかる foreign key、または主キーとして任意のプロパティを設定し、[後のチュートリアル](complex-data-model.md)このシリーズのです。

## <a name="asynchronous-code"></a>非同期コード

非同期プログラミングは、ASP.NET Core と EF コアの既定モードです。

Web サーバーは、使用可能なスレッド数を限定を持ち、負荷が高い状況でのすべての利用可能なスレッドがありますで使用します。 そのような場合は、サーバーは、スレッドが解放されるまで新しい要求を処理できません。 同期コードはの I/O 完了を待機しているため、実際には、作業の実行されない中に多数のスレッド関連付ける可能性があります。 非同期コードは、プロセスが完了するには I/O の待機している場合、他の要求を処理するために使用するサーバー用に、スレッドが解放されます。 その結果、非同期コード サーバー リソースをより効率的に使用でき、遅延なしのより多くのトラフィックを処理するサーバーが有効になっています。

非同期のコードは、実行時に少量のオーバーヘッドを導入がトラフィックの少ない状況がパフォーマンスの低下はごくわずかであり、中に大量のトラフィックの場合、潜在的なパフォーマンスの向上は大きくします。

次のコードで、`async`キーワード、`Task<T>`値を返す`await`キーワード、および`ToListAsync`メソッドが非同期的に実行するコードを作成します。

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex)]

* `async`キーワード、コンパイラはメソッド本体の各部分のコールバックを生成して自動的に作成する、`Task<IActionResult>`返されるオブジェクト。

* 戻り値の型`Task<IActionResult>`型の結果で進行中の作業を表す`IActionResult`です。

* `await`キーワードによって、コンパイラにメソッドを 2 つの部分に分割します。 最初の部分は、非同期的に開始される操作を終了します。 2 番目の部分は、操作が完了したときに呼び出されるコールバック メソッドに配置されます。

* `ToListAsync`非同期バージョンの`ToList`拡張メソッド。

Entity Framework を使用する非同期コードを作成する場合の注意すべき点がいくつか:

* クエリまたはコマンドのデータベースに送信されるステートメントだけが非同期的に実行されます。 たとえばを含む`ToListAsync`、 `SingleOrDefaultAsync`、および`SaveChangesAsync`です。  含まれません、たとえば、だけを変更するステートメント、`IQueryable`など`var students = context.Students.Where(s => s.LastName == "Davolio")`です。

* EF コンテキストはスレッド セーフではありません: を並列で複数の操作を行うにはしないでください。 ある非同期 EF メソッドを呼び出すときに、常に使用して、`await`キーワード。

* 非同期コードのパフォーマンスの利点を活用、任意のライブラリのパッケージにあるかどうかを確認する場合、(ページングなど) を使用している、データベースに送信されるクエリを Entity Framework メソッドを呼び出す場合にも非同期を使用します。

.NET における非同期プログラミングの詳細については、次を参照してください。 [Async 概要](https://docs.microsoft.com/dotnet/articles/standard/async)です。

## <a name="summary"></a>概要

保存し、データを表示する、エンティティ フレームワークのコアと SQL Server Express LocalDB を使用する単純なアプリケーションが作成されました。 次のチュートリアルでは基本的な CRUD を実行する方法を学習 (作成、読み取り、更新、削除) 操作です。

>[!div class="step-by-step"]
[次へ](crud.md)  
