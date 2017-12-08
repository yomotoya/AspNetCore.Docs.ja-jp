---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: "Entity Framework 6 の Code First MVC 5 を使用すると作業の開始 |Microsoft ドキュメント"
author: tdykstra
description: "このチュートリアル シリーズの新しいバージョンを使用できます。 ASP.NET Core と Visual Studio 2015 を使用して Entity Framework のコアで作業を開始します。 Contoso Universi しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/22/2015
ms.topic: article
ms.assetid: 00bc8b51-32ed-4fd3-9745-be4c2a9c1eaf
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 84ca4bbaebe401d14233131bcaa027debf7ea0f9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="getting-started-with-entity-framework-6-code-first-using-mvc-5"></a>MVC 5 を使用する Entity Framework 6 Code First の概要
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[完成したプロジェクトをダウンロード](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)または[PDF のダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> > [!NOTE] 
> > 
> > このチュートリアル シリーズの新しいバージョンが利用可能な: [ASP.NET Core と Visual Studio 2015 を使用して Entity Framework Core 使い方](https://docs.asp.net/en/latest/data/ef-mvc/intro.html)です。
> 
> 
> Contoso 大学でサンプル web アプリケーションでは、Entity Framework 6 および Visual Studio 2013 を使用して ASP.NET MVC 5 アプリケーションを作成する方法を示します。 このチュートリアルでは、コードの最初のワークフローを使用します。 Code First、Database First または Model First を選択する方法については、次を参照してください。 [Entity Framework 開発ワークフロー](https://msdn.microsoft.com/en-us/library/ms178359.aspx#dbfmfcf)です。
> 
> サンプル アプリケーションは、架空の Contoso 大学の web サイトです。 学生受付、コースの作成、およびインストラクター割り当てなどの機能が含まれています。 このチュートリアルの系列では、Contoso 大学サンプル アプリケーションをビルドする方法について説明します。 実行できます[完成したアプリケーションをダウンロード](https://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)です。
> 
> Mike Brind によって変換 Visual Basic バージョンを利用できます: [Visual Basic での EF 6 を使用して MVC 5](http://www.mikesdotnetting.com/Article/241/MVC-5-with-EF-6-in-Visual-Basic-Creating-an-Entity-Framework-Data-Model) Mikesdotnetting サイトです。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - Entity Framework 6 (EntityFramework 6.1.0 NuGet パッケージ)
> - [Windows Azure SDK 2.2](https://go.microsoft.com/fwlink/p/?linkid=323510) (省略可能)
>   
> 
> このチュートリアルを使用する必要がありますも[Visual Studio 2013 の Express for Web](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express)または Visual Studio 2012。 [VS 2012 バージョンの Windows Azure SDK](https://go.microsoft.com/fwlink/p/?linkid=323511)の Visual Studio 2012 の Windows Azure 展開が必要です。
> 
> 
> ## <a name="tutorial-versions"></a>チュートリアルのバージョン
> 
> 以前のバージョンのこのチュートリアルでは、次を参照してください。 [EF 4.1/MVC 3 の電子書籍](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC)と[MVC 4 を使用する EF 5 の概要](../../older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)です。
> 
> ## <a name="questions-and-comments"></a>質問やコメント
> 
> このチュートリアルをリンクする方法と、ページの下部にあるコメントで改善新機能にフィードバックを送信してください。 チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET Entity Framework フォーラム](https://forums.asp.net/1227.aspx)、 [Entity Framework でも LINQ to Entities フォーラム](https://social.msdn.microsoft.com/forums/en-US/adodotnetentityframework/threads/)、または[StackOverflow.com](http://stackoverflow.com/)です。
> 
> 問題を解決できない場合に発生した場合は、ダウンロードできる完成したプロジェクトにコードを比較することによって一般的に、問題の解決策を検索できます。 一般的なエラーとそれらを解決する方法は、次を参照してください。[の一般的なエラーと解決方法またはそれらの回避策です。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


## <a name="the-contoso-university-web-application"></a>Contoso 大学 Web アプリケーション

これらのチュートリアルで構築するアプリケーションは、単純な大学 web サイトです。

ユーザーでは、表示でき、学生、コース、インストラクターの情報を更新することができます。 ここでは、いくつかの画面を作成します。

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![学生を編集します。](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

このサイトの UI スタイルを維持して組み込みのテンプレートによって生成されたものに近いチュートリアルは、Entity Framework を使用する方法の主に集中することです。

## <a name="prerequisites"></a>必須コンポーネント

参照してください**ソフトウェア バージョン**ページの上部にあります。 Entity Framework 6 は、このチュートリアルの一部として、EF NuGet パッケージをインストールするための前提条件ではありません。

## <a name="create-an-mvc-web-application"></a>MVC Web アプリケーションを作成します。

Visual Studio を開き、"ContosoUniversity"をという名前の新しい c# Web プロジェクトを作成します。

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

**新しい ASP.NET プロジェクト**ダイアログ ボックスの 、 **MVC**テンプレート。

場合、**クラウド内のホスト** チェック ボックス、 **Microsoft Azure**セクションが選択されている、オフにします。

をクリックして**認証変更**です。

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

**認証の変更**ダイアログ ボックスで、**認証なし**、順にクリック**OK**です。 このチュートリアルではありませんするユーザーのログオンを必要とするかログインしているユーザーに基づいてアクセスを制限することです。

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

新しい ASP.NET プロジェクト ダイアログ ボックスで、戻る をクリックして**OK**プロジェクトを作成します。

## <a name="set-up-the-site-style"></a>サイトのスタイルを設定します。

いくつかの簡単な変更は、[サイト] メニューのレイアウト、およびホーム ページを設定します。

開いている*\shared\\_Layout.cshtml*、次の変更を行います。

- 「Contoso 大学」に"My ASP.NET Application"と「アプリケーション名」の各出現する位置を変更します。
- 学生、コース、インストラクター、部門、およびメニュー エントリを追加し、連絡先のエントリを削除します。

変更が強調表示されます。

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=6,19,24-27,38)]

*Views\Home\Index.cshtml*ファイルの内容をこのアプリケーションについてのテキストで ASP.NET と MVC に関するテキストを置き換える次のコードに置き換えます。

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

サイトを実行するには、CTRL + F5 キーを押します。 メイン メニューで、ホーム ページを参照してください。

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

## <a name="install-entity-framework-6"></a>Entity Framework 6 をインストールします。

**ツール**ボタンをクリックし**NuGet Package Manager**  をクリックし、 **Package Manager Console**です。

**Package Manager Console**ウィンドウには、次のコマンドを入力してください。

`Install-Package EntityFramework`

![インストールされている EF](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

NuGet はインストール (プレリリース版を除く)、Entity Framework の最新バージョンのチュートリアルへの最新の更新プログラムの時点では 6.1.1 がイメージがインストールされている 6.0.0 が表示されます。

この手順は、このチュートリアルでは、手動で行うを実現できます自動的に ASP.NET MVC のスキャフォールディング機能によって、いくつかの手順のいずれかです。 Entity Framework を使用するための手順を認識できるようにそれらを手動で実行しています。 MVC コント ローラーとビューを作成するのに、スキャフォールディングを後で使用します。 代わりにはスキャフォールディングを自動的に EF NuGet パッケージのインストールし、データベース コンテキスト クラスを作成し、接続文字列を作成します。 方法で実行する準備ができたら、行う必要があるはこれらの手順をスキップし、エンティティ クラスを作成した後に、MVC コント ローラーをスキャフォールディングです。

## <a name="create-the-data-model"></a>データ モデルを作成します。

次に、Contoso 大学アプリケーション用にエンティティ クラスを作成します。 次の 3 つのエンティティを開始します。

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

一対多の関係がある`Student`と`Enrollment`エンティティ、一対多の関係があると`Course`と`Enrollment`エンティティです。 つまり、任意の数に、コースの受講者を登録して、コースに受講者に、登録の任意の数を持つことができます。

次のセクションでは、これらのエンティティのいずれかのクラスを作成します。

> [!NOTE]
> すべてのエンティティ クラスを作成する前に、プロジェクトをコンパイルしようとすると、コンパイラ エラーが表示されます。


### <a name="the-student-entity"></a>学生エンティティ

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

*モデル*フォルダー、という名前のクラス ファイルを作成する*Student.cs*テンプレート コードを次のコードに置き換えます。

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs)]

`ID`プロパティは、このクラスに対応するデータベース テーブルの主キー列になります。 既定では、Entity Framework では、解釈というプロパティ`ID`または*classname* `ID`主キーとして。

`Enrollments`プロパティは、*ナビゲーション プロパティ*です。 ナビゲーション プロパティは、このエンティティに関連するその他のエンティティを保持します。 ここで、`Enrollments`のプロパティ、`Student`のすべてのエンティティを保持する、`Enrollment`に関連付けられているエンティティ`Student`エンティティです。 つまり場合、指定された`Student`データベース内の行が 2 つの関連`Enrollment`行 (そのスチューデントの主キーが含まれている行の値で、`StudentID`外部キー列)、その`Student`エンティティの`Enrollments`ナビゲーション プロパティこれら 2 つが含まれます`Enrollment`エンティティです。

ナビゲーション プロパティは、通常、として定義`virtual`などの特定の Entity Framework 機能を利用を行うことができるように*遅延読み込み*です。 (遅延読み込みについては説明後の「、[関連のデータの読み取り](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)このシリーズ後でチュートリアルです。)

その型が一覧にエントリを追加、削除すると、更新できるなどをする必要があります、ナビゲーション プロパティ (多対多または一対多のリレーションシップ) のように複数のエンティティに保持できる場合`ICollection`です。

### <a name="the-enrollment-entity"></a>登録エンティティ

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)

*モデル*フォルダー作成*Enrollment.cs*し、既存のコードを次のコードに置き換えます。

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

`EnrollmentID`プロパティは、主キーになります。 このエンティティを使用して、 *classname* `ID`パターンの代わりに`ID`で学習したとしてそれ自体で、`Student`エンティティです。 通常 1 つパターンを選択し、データ モデル全体で使用するとします。 ここでは、いずれかのパターンを使用することができます、バリエーションを示しています。 後のチュートリアルでわかる方法を使用して`ID`せず`classname`データ モデルでの継承を実装するが容易です。

`Grade`プロパティは、 [enum](https://msdn.microsoft.com/en-us/data/hh859576.aspx)です。 後に疑問符 ()、`Grade`型宣言では、ことを示します、`Grade`プロパティは[null 許容](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx)です。 Null である評価とは異なる、ゼロ グレード: null は、評価が不明またはまだ割り当てられていないことを意味します。

`StudentID`プロパティは、foreign key、および対応するナビゲーション プロパティは`Student`します。 `Enrollment`エンティティが 1 つに関連付けられた`Student`エンティティ、プロパティは、1 つのみを保持できるように`Student`エンティティ (とは異なり、`Student.Enrollments`ナビゲーション プロパティ先ほど見た、複数を格納する`Enrollment`エンティティ)。

`CourseID`プロパティは、foreign key、および対応するナビゲーション プロパティは`Course`します。 `Enrollment`エンティティが 1 つに関連付けられた`Course`エンティティです。

Entity Framework がという名前が場合に、外部キーのプロパティとしてプロパティを解釈*&lt;ナビゲーション プロパティ名&gt;&lt;主キーのプロパティ名&gt;* (たとえば、 `StudentID`の`Student`以降のナビゲーション プロパティ、`Student`エンティティの主キーが`ID`)。 外部キー プロパティできますも同じ名前にするだけで*&lt;主キーのプロパティ名&gt;* (たとえば、`CourseID`ので、`Course`エンティティの主キーが`CourseID`)。

### <a name="the-course-entity"></a>Course エンティティ

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

*モデル*フォルダー作成*Course.cs*、テンプレート コードを次のコードに置き換えます。

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

`Enrollments`プロパティは、ナビゲーション プロパティ。 A`Course`エンティティを任意の数に関連付けることができます`Enrollment`エンティティです。

ここでの詳細について、 [DatabaseGenerated](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx)このシリーズの後のチュートリアルでの属性です。 基本的には、この属性には、それを生成、データベースのではなく、コースのプライマリ キーを入力することができます。

## <a name="create-the-database-context"></a>データベース コンテキストを作成します。

指定されたデータ モデルの Entity Framework 機能を調整するメイン クラスは、*データベース コンテキスト*クラスです。 派生することによってこのクラスを作成する、 [System.Data.Entity.DbContext](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext(v=VS.103).aspx)クラスです。 コードでは、データ モデルのエンティティが含まれているを指定します。 特定の Entity Framework の動作をカスタマイズすることもできます。 クラスの名前は、このプロジェクトで`SchoolContext`です。

ContosoUniversity プロジェクトにフォルダーを作成するでプロジェクトを右クリックして**ソリューション エクスプ ローラー**  をクリック**追加**、クリックして**新しいフォルダー**です。 新しいフォルダーの名前を付けます*DAL* (データ アクセス層) 用です。 そのフォルダーの作成という新しいクラス ファイル*SchoolContext.cs*、テンプレート コードを次のコードに置き換えます。

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

### <a name="specifying-entity-sets"></a>エンティティ セットを指定します。

このコードを作成、 [DbSet](https://msdn.microsoft.com/en-us/library/system.data.entity.dbset(v=VS.103).aspx)各エンティティ セットのプロパティです。 Entity Framework の用語で、*エンティティ セット*通常、データベース テーブルに対応し、*エンティティ*テーブル内の行に対応しています。

> [!NOTE] 
> 
> 省略した可能性がありますが、`DbSet<Enrollment>`と`DbSet<Course>`ステートメントと同じように動作します。 Entity Framework はそれらを含める暗黙的に、`Student`エンティティ参照、`Enrollment`エンティティと`Enrollment`エンティティ参照、`Course`エンティティです。


### <a name="specifying-the-connection-string"></a>接続文字列の指定

(これは、後で、Web.config ファイルに追加されます) 接続文字列の名前は、コンス トラクターに渡されます。

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=1)]

Web.config ファイルに格納されているいずれかの名前ではなく、接続文字列自体に渡すこともできます。 使用するデータベースを指定するためのオプションの詳細については、次を参照してください。 [Entity Framework の接続とモデル](https://msdn.microsoft.com/en-us/data/jj592674)です。

場合は、接続文字列か 1 つの名前を明示的に指定しなければ、Entity Framework は、接続文字列名がクラス名と同じであると仮定します。 この例では既定の接続文字列名になりますし`SchoolContext`、明示的に指定しているものと同じです。

### <a name="specifying-singular-table-names"></a>単数形のテーブル名を指定します。

`modelBuilder.Conventions.Remove`内のステートメント、 [OnModelCreating](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx)メソッドは、複数化されてからテーブル名を防止します。 これをしなかった場合、データベースで生成されたテーブルの名前は`Students`、 `Courses`、および`Enrollments`です。 代わりに、テーブル名になります`Student`、 `Course`、および`Enrollment`です。 テーブル名を複数形にするかどうかについては、開発者の間で意見が分かれるでしょう。 このチュートリアルは単数形のフォームを使用しますが、重要な点は、することを含むか、次のコード行を省略することによって、どのフォームを選択することができます。

## <a name="set-up-ef-to-initialize-the-database-with-test-data"></a>データベースにテスト データを初期化するために、EF を設定します。

Entity Framework できます自動的に作成 (または削除して再作成) するアプリケーションの実行時のデータベースです。 これをするアプリケーションを実行するたびに、または、モデルは、既存のデータベースとの同期時にのみを指定することができます。 作成することも、 `Seed` Entity Framework が、テスト データを設定するために、データベースの作成後に自動的に呼び出すメソッド。

既定の動作では、のみそれが存在しないとき (モデルが変更されており、データベースが既に存在する場合、例外がスロー) データベースを作成します。 このセクションでは、データベースを削除して、モデルが変更されるたびを再作成することを指定します。 データベースを削除するには、すべてのデータの損失と、します。 これは一般的に [ok]、開発中にあるため、`Seed`メソッドは、データベースが再作成され、テスト データを再作成時に実行されます。 実稼働環境で通常たくないすべてのデータが失われるたびに、データベース スキーマを変更する必要があります。 後で削除して、データベースを再作成ではなく、データベース スキーマを変更する Code First Migrations を使用して、モデルの変更を処理する方法が表示されます。

DAL フォルダーでという新しいクラス ファイルを作成*SchoolInitializer.cs*にテンプレート コードを置き換えると、  
次のコードは、これにより、ときに作成するデータベースで必要し、新しいデータベースにテスト データを読み込みます。

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

`Seed`メソッドを受け取り、入力パラメーターとして、データベース コンテキストのオブジェクト、メソッドのコードを使用  
そのオブジェクトをデータベースに新しいエンティティを追加します。 各エンティティ型に対して、コードのコレクションを作成新規  
 エンティティ、適切なにそれらを追加`DbSet`プロパティ、および、データベースへの変更を保存します。 そうじゃないです  
呼び出すために必要な`SaveChanges`によりこれが、ここでは、行われるように、エンティティの各グループの後にメソッド  
コードは、データベースに書き込んでいるときに例外が発生した場合は、問題の原因を見つけます。

初期化子クラスを使用する Entity Framework を確認するには、要素を追加、`entityFramework`アプリケーション内の要素*Web.config*ファイル (ファイル、ルート プロジェクト フォルダー内)、次の例で示すようにします。

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.xml?highlight=2-6)]

`context type`コンテキストの完全修飾クラス名とには、アセンブリを指定し、`databaseinitializer type`初期化子のクラスと内にあるアセンブリの完全修飾名を指定します。 (で属性を設定するには、初期化子を使用する EF を含めない場合は、ときに、`context`要素: `disableDatabaseInitialization="true"`)。詳細については、次を参照してください。 [Entity Framework の構成ファイル設定](https://msdn.microsoft.com/en-us/data/jj556606)です。

初期化子を設定する代わりに、 *Web.config*ファイルを追加することによって実行するコードで、`Database.SetInitializer`ステートメントを`Application_Start`メソッドで、 *Global.asax.cs*ファイル。 詳細については、次を参照してください。[についてデータベース初期化子の Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm)です。

アプリケーションは現在設定されている最大の実行で最初にデータベースをアクセスすること、  
Entity Framework がモデルにデータベースを比較するアプリケーション、(、`SchoolContext`およびエンティティ クラス)。 違いがある場合、アプリケーションは削除して、データベースを再作成します。

> [!NOTE]
> アプリケーションを実稼働 web サーバーを展開するときに、削除するか、削除して、データベースを再作成するコードを無効にする必要があります。 このシリーズの後のチュートリアルを実行してみましょう。


## <a name="set-up-ef-to-use-a-sql-server-express-localdb-database"></a>SQL Server Express LocalDB データベースを使用する EF を設定します。

[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx)は、SQL Server Express データベース エンジンの簡易バージョンです。 簡単にインストールして構成は、オンデマンドで起動し、ユーザー モードで実行します。 LocalDB は、SQL Server Express データベースを対象として使用することができますの特殊な実行モードで実行*.mdf*ファイル。 LocalDB のデータベース ファイルに格納して、*アプリ\_データ*プロジェクトにデータベースをコピーできるようにする場合は、web プロジェクトのフォルダーです。 SQL Server express ユーザー インスタンスの機能でを操作することもできます*.mdf*は、ファイル、ユーザー インスタンス機能は推奨されません。 そのため、LocalDB を使用するため推奨*.mdf*ファイル。 Visual Studio 2012 およびそれ以降のバージョンで LocalDB は既定では Visual Studio と共にインストールされます。

通常実稼働 web アプリケーションの SQL Server Express は使用されません。 LocalDB 具体的には、ために推奨されませんを実稼働用 web アプリケーションと IIS を使用するものはありません。

このチュートリアルでは、LocalDB を使用します。 アプリケーションを開く*Web.config*ファイルを追加、`connectionStrings`要素の前、`appSettings`要素は、次の例で示すようにします。 (更新するかどうかを確認、 *Web.config*ルート プロジェクト フォルダー内のファイルです。 *Web.config*ファイルは、*ビュー*サブフォルダーを更新する必要はありません)。

Visual Studio 2015 を使用している場合は、接続文字列で"v11.0"を既定の SQL Server インスタンス名が変更されると"MSSQLLocalDB"で置き換えます。

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.xml?highlight=1-3)]

追加した接続文字列は、Entity Framework がという名前の LocalDB データベースを使用することを指定します*ContosoUniversity1.mdf*です。 (データベースはまだ存在しません。EF が作成されます。)データベースを作成する場合、*アプリ\_データ*追加することも、フォルダー`AttachDBFilename=|DataDirectory|\ContosoUniversity1.mdf`接続文字列にします。 接続文字列の詳細については、次を参照してください。 [ASP.NET Web アプリケーション用の SQL Server 接続文字列](https://msdn.microsoft.com/en-us/library/jj653752.aspx)です。

実際に接続文字列を用意する必要はありません、 *Web.config*ファイル。 接続文字列を指定しない場合、Entity Framework は、コンテキスト クラスに基づくいずれかの既定値を使用します。 詳細については、次を参照してください。 [Code First の新しいデータベースに](https://msdn.microsoft.com/en-us/data/jj193542)です。

## <a name="creating-a-student-controller-and-views"></a>学生のコント ローラーとビューの作成

今すぐデータを表示する web ページを作成し、データを要求の処理が自動的にトリガー  
データベースの作成。 新しいコント ローラーの作成から始めます。 前にプロジェクトをビルドして、モデルおよびコンテキスト クラスを MVC コント ローラーのスキャフォールディングを使用できるようにします。

1. 右クリックし、**コント ローラー**フォルダーに**ソリューション エクスプ ローラー****追加**、順にクリック**スキャフォールディングされた新しい項目**です。
- **追加 Scaffold**ダイアログ ボックスで、 **MVC 5 コント ローラー、ビューがある Entity Framework を使用して**です。

    ![スキャフォールディングを追加します。](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)
- コント ローラーの追加 ダイアログ ボックスで、以下の選択を行い をクリックして**追加**:

    - モデル クラス:**学生 (ContosoUniversity.Models)**です。 (プロジェクトのビルド ドロップダウン リストでは、このオプションが表示されない場合、およびもう一度やり直してください。)
    - データ コンテキスト クラス: **SchoolContext (ContosoUniversity.DAL)**です。
    - コント ローラー名: **StudentController** (StudentsController されません)。
    - その他のフィールドの既定値のままにします。

    ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

    クリックすると、**追加**、scaffolder StudentController.cs ファイルと、コント ローラーを使用するビュー (.cshtml ファイル) のセットを作成します。 Entity Framework を使用するプロジェクトを作成するときに、将来を活用することも、scaffolder の追加機能の一部: をだけで、最初のモデル クラスを作成する接続文字列を作成しませんし、**コント ローラーの追加**ボックスは、新しいコンテキスト クラスを指定します。 Scaffolder を作成、`DbContext`コント ローラーとビューだけでなくクラスとの接続文字列します。
- Visual Studio を開き、 *Controllers\StudentController.cs*ファイル。 データベース コンテキストのオブジェクトをインスタンス化するクラスの変数が作成されてが表示されます。

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

    `Index`アクション メソッドから受講者のリストを取得、*受講者*エンティティ セットを読み取ることによって、`Students`データベース コンテキストのインスタンスのプロパティ。

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

    *Student\Index.cshtml*ビューは、テーブルのこの一覧を表示します。

    [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cshtml)]
- Ctrl キーを押しながら F5 キーを押してプロジェクトを実行します。 (「シャドウ コピーを作成できません」エラーが発生した場合、ブラウザーを閉じ、もう一度やり直してください。)

    をクリックして、**受講者**テスト データを表示 タブを`Seed`メソッドを挿入します。 ナロー方法に応じて、ブラウザーまたはウィンドウは、最上位のアドレス バーに受講者 タブのリンクが表示されますのリンクを表示する右上隅をクリックする必要があります。

    ![メニュー ボタン](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

    ![学生のインデックス ページ](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)

## <a name="view-the-database"></a>データベースを表示します。

受講者 ページを実行すると、アプリケーションがデータベースにアクセスしようとした場合、データベースがありませんでした。 そのため、1 つを作成、し、実行されたデータベースにデータを設定するシード メソッド EF を確認しました。

いずれかを使用する**サーバー エクスプ ローラー**または**SQL Server オブジェクト エクスプ ローラー** Visual Studio でデータベースを表示するには、(SSOX)。 このチュートリアルで使用する**サーバー エクスプ ローラー**です。 (エディションの Visual Studio Express 2013 年より前で**サーバー エクスプ ローラー**が呼び出された**データベース エクスプ ローラー**)。

1. ブラウザーを閉じます。
2. **サーバー エクスプ ローラー**、展開**データ接続**、展開**学校コンテキスト (ContosoUniversity)**、順に展開**テーブル**を表示するには新しいデータベースのテーブルです。

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
3. 右クリックし、**学生**テーブルし、をクリックして**テーブル データの表示**に作成された列とテーブルに挿入された行を参照してください。

    ![Student テーブル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
4. 閉じる、**サーバー エクスプ ローラー**接続します。

*ContosoUniversity1.mdf*と*.ldf*データベース ファイルは、`C:\Users\<yourusername>`フォルダーです。

使用しているため、`DropCreateDatabaseIfModelChanges`初期化子、でした今すぐ変更を加えるには`Student`クラス、もう一度、アプリケーションを実行し、するとデータベースに自動的に変更内容を一致するように再作成します。 たとえば、追加する場合、`EmailAddress`プロパティを`Student`クラス、受講者 ページを再度実行し、表示、テーブルをもう一度、表示して、新しい`EmailAddress`列です。

## <a name="conventions"></a>規則

使用するための完全なデータベースを作成できるように、Entity Framework の順序で記述したコードの量は最小限に抑える*規則*、または Entity Framework で想定します。 既に説明した一部の場合、またはされず、気付かないうち使用されていたは。

- エンティティのクラス名の複数形のフォームは、テーブル名として使用されます。
- エンティティのプロパティ名は、列名に使用されます。
- 名前付きエンティティのプロパティ`ID`または*classname* `ID`主キー プロパティとして認識されます。
- という名前が場合、プロパティが外部キーのプロパティとして解釈されます*&lt;ナビゲーション プロパティ名&gt;&lt;主キーのプロパティ名&gt;* (たとえば、 `StudentID` の`Student`以降のナビゲーション プロパティ、`Student`エンティティの主キーが`ID`)。 外部キー プロパティできますも同じ名前にするだけで&lt;主キーのプロパティ名&gt;(たとえば、`EnrollmentID`ので、`Enrollment`エンティティの主キーが`EnrollmentID`)。

規則がオーバーライドできることを見てきました。 たとえば、テーブル名を複数化しないでください、および、後述することを指定を外部キーのプロパティとしてプロパティを明示的にマークする方法です。 規則およびそれらをオーバーライドする方法の詳細を学習、[詳細複雑なデータ モデルを作成する](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)このシリーズの後でチュートリアルです。 規則の詳細については、次を参照してください。[コード First 規約に従って](https://msdn.microsoft.com/en-us/data/jj679962)です。

## <a name="summary"></a>概要

保存し、データを表示する Entity Framework および SQL Server Express LocalDB を使用する簡単なアプリケーションが作成されました。 次のチュートリアルでは基本的な CRUD を実行する方法を学習 (作成、読み取り、更新、削除) 操作です。

このチュートリアルをリンクする方法と、何を改善にフィードバックを送信してください。 新しいトピックを要求することもできます。 [Me 方法でコードの表示](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)です。

その他の Entity Framework リソースへのリンクは含まれて[ASP.NET データ アクセス - リソースのことをお勧め](../../../../whitepapers/aspnet-data-access-content-map.md)です。

>[!div class="step-by-step"]
[次へ](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
