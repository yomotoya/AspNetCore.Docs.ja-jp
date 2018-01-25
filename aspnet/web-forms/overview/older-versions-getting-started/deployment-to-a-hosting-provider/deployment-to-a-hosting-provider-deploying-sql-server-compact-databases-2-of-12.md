---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
title: "SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: を展開する SQL Server Compact データベース - 12 の 2 |Microsoft ドキュメント"
author: tdykstra
description: "この一連のチュートリアルは、展開する方法を示します (発行)、ASP.NET web アプリケーション プロジェクトを Visual Stu を使用して、SQL Server Compact データベースが含まれています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: c3c76516-4c48-4153-bd03-d70e3a3edbb0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
msc.type: authoredcontent
ms.openlocfilehash: 5296bc1ca3fd0b24123bd79a550a7e2cffc34a44
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-sql-server-compact-databases---2-of-12"></a>SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: を展開する SQL Server Compact データベース - 12 の 2
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> この一連のチュートリアルは、展開する方法を示します (発行)、ASP.NET web アプリケーション プロジェクトを Visual Studio 2012 RC または Visual Studio Express 2012 RC for Web を使用して SQL Server Compact データベースが含まれます。 Web 公開の更新プログラムをインストールする場合は、また Visual Studio 2010 を使用することができます。 系列の概要については、次を参照してください。[系列内の最初のチュートリアル](deployment-to-a-hosting-provider-introduction-1-of-12.md)です。
> 
> チュートリアルについては、RC のリリースの Visual Studio 2012 以降の展開機能を示しています、SQL Server Compact 以外の SQL Server のエディションを展開する方法を示していますし、Azure App Service Web アプリを展開する方法を示しています、次を参照してください[ASP.NET Web 配置。Visual Studio を使用して](../../deployment/visual-studio-web-deployment/introduction.md)です。


## <a name="overview"></a>概要

このチュートリアルでは、2 つの SQL Server Compact データベースし、データベース エンジンの展開をセットアップする方法を示します。

データベースへのアクセスには、Contoso 大学アプリケーションには、.NET Framework に含まれていないために、アプリケーションと共に配置する必要があります、次のソフトウェアが必要です。

- [SQL Server Compact](https://www.microsoft.com/sqlserver/en/us/editions/compact.aspx) (データベース エンジン)。
- [ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (これを有効にする SQL Server Compact を使用する ASP.NET メンバーシップ システム)
- [Entity Framework 5.0](https://msdn.microsoft.com/library/gg696172(d=lightweight,v=vs.103).aspx)(移行の最初のコード)。

データベースの構造と一部 (すべて)、アプリケーションの 2 つのデータのデータベースも展開する必要があります。 通常、アプリケーションを開発するには、実際のサイトに展開したくない、データベースにテスト データを入力します。 ただし、展開する一部の実稼働データを入力することも可能性があります。 このチュートリアルでは必要なソフトウェアと正しいデータが含まように展開するときに、Contoso 大学プロジェクトを構成します。

アラーム: エラー メッセージを取得する、または、チュートリアルを通過するとおりに機能しない、確認して、[トラブルシューティングのページ](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)です。

## <a name="sql-server-compact-versus-sql-server-express"></a>SQL Server Express と SQL Server Compact

サンプル アプリケーションでは、SQL Server Compact 4.0 を使用します。 このデータベース エンジンは、比較的新しいオプションを web サイトです。以前のバージョンの SQL Server Compact は、web ホスティング環境では機能しません。 SQL Server Compact は、SQL Server Express を使用した開発と完全な SQL Server への展開の一般的なシナリオと比較して、いくつかの利点を提供します。 によっては、選択したホスティング プロバイダー、SQL Server Compact 可能性がありますより低いコストで展開するには、プロバイダーによって、完全な SQL Server データベースをサポートするために追加の課金。 Web アプリケーションの一部としてデータベース エンジン自体を配置することができますので、SQL Server Compact に対する追加料金はありません。

ただし、制限の対応する必要があります。 SQL Server Compact できませんストアド プロシージャ、トリガー、ビュー、またはレプリケーション。 (SQL Server Compact でサポートされていない SQL Server 機能の一覧については、次を参照してください[の相違点の間で SQL Server Compact および SQL Server](https://msdn.microsoft.com/library/bb896140.aspx)。)。また、SQL Server Compact でスキーマと SQL Server Express と SQL Server データベース内のデータを操作に使用できる、ツールの一部が行えません。 たとえば、SQL Server Compact データベースに Visual Studio で SQL Server Management Studio または SQL Server Data Tools を使用できません。 SQL Server Compact データベースの使用に関するその他のオプションを使用する必要は。

- サーバー エクスプ ローラーを使用して、Visual Studio で、SQL Server Compact の限られたデータベース操作の機能を提供することができます。
- データベース操作機能を使用することができます[WebMatrix](https://www.microsoft.com/web/webmatrix/)、サーバー エクスプ ローラーより多くの機能を持ちます。
- サードパーティの比較的フル機能を使用したりなどのオープン ソース ツール、 [SQL Server Compact ツールボックス](https://github.com/ErikEJ/SqlCeToolbox)と[SQL Compact データとスキーマ スクリプト ユーティリティ](https://github.com/ErikEJ/SqlCeToolbox)です。
- 作成し、データベース スキーマを操作する独自の DDL (データ定義言語) スクリプトを実行できます。

SQL Server Compact で開始し、ニーズに応じて後で、アップグレードしたことができます。 このシリーズの後のチュートリアルでは、SQL Server Compact から SQL Server express と SQL server を移行する方法を示します。 ただしをする場合、新しいアプリケーションを作成している SQL Server を近い将来に必要だと思わ、SQL Server または SQL Server Express で始めることをおはします。

## <a name="configuring-the-sql-server-compact-database-engine-for-deployment"></a>展開用の SQL Server Compact データベース エンジンの構成

Contoso 大学アプリケーションでのデータ アクセスに必要なソフトウェアは、次の NuGet パッケージをインストールすることによって追加されました。

- [SqlServerCompact](http://nuget.org/List/Packages/SqlServerCompact)
- [System.Web.Providers](http://nuget.org/List/Packages/System.Web.Providers) (ASP.NET ユニバーサル プロバイダー)
- [EntityFramework](http://nuget.org/List/Packages/EntityFramework)
- [EntityFramework.SqlServerCompact](http://nuget.org/List/Packages/EntityFramework.sqlservercompact)

リンクは、このチュートリアル用にダウンロードしたスタート プロジェクトにインストールされている内容よりも新しい可能性のあるこれらのパッケージの現在のバージョンをポイントします。 展開については、ホスティング プロバイダーに、Entity Framework 5.0 以降を使用することを確認します。 以前のバージョンの Code First Migrations が完全信頼を必要とし、多くのホスティング プロバイダーに、アプリケーションが中程度の信頼で実行されます。 中程度の信頼の詳細については、次を参照してください。、[テスト環境として IIS に展開する](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)チュートリアルです。

一般に NuGet パッケージのインストールは、アプリケーションにこのソフトウェアを展開するために必要なすべての処理されます。 場合によっては、これには、Web.config ファイルを変更して、ソリューションをビルドするたびに実行する PowerShell スクリプトを追加するなどのタスクが含まれます。 **NuGet を使用せずにこれらの機能 (SQL Server Compact および Entity Framework) などのサポートを追加する場合は、NuGet パッケージのインストールの動作できるように、同じ作業を手動で行うことができますがわかっていることを確認してください。**

NuGet は展開を成功させることを確認するために実行する必要があるすべての注意を受け取らないを 1 つの例外があります。 SqlServerCompact NuGet パッケージのビルド後のスクリプトをプロジェクトに追加するネイティブ アセンブリをコピーする*x86*と*amd64*プロジェクトの下のサブフォルダー *bin*フォルダーが、スクリプトは、プロジェクトでそれらのフォルダーを含みません。 結果として、Web Deploy は場合はコピーしないに web サイトに手動で、プロジェクト内に含めます。 (この動作は、既定の配置構成の結果。 この動作を制御する設定を変更するのには、これらのチュートリアルで使用しない、別のオプションです。 変更できる設定は**アプリケーションの実行に必要なファイルのみ****を展開する項目**上、**パッケージ化/発行 Web**のタブ、**プロジェクトプロパティ**ウィンドウです。 この設定を変更するは、通常ために推奨されませんがあるために必要なよりも、運用環境にファイル数より多くの展開があります。 その他の方法の詳細については、次を参照してください、[プロジェクト プロパティを設定する](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)チュートリアルです。)。

プロジェクトをビルドし、**[ソリューション エクスプ ローラー]** をクリックして**ファイルをすべて表示**まだ行っていない場合。 をクリックする必要がありますも**更新**です。

![Solution_Explorer_Show_All_Files](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image1.png)

展開、 **bin**を表示するフォルダー、 **amd64**と**x86**フォルダー、それらのフォルダーを右クリックしをクリックして**をプロジェクトに含める**.

![amd64_and_x86_in_Solution_Explorer.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image2.png)

フォルダーのアイコンは、表示フォルダーをプロジェクトに含まれていることを変更します。

![Solution_Explorer_amd64_included.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image3.png)

## <a name="configuring-code-first-migrations-for-application-database-deployment"></a>アプリケーション データベースの配置の Code First Migrations を構成します。

アプリケーション データベースを展開するときに通常しないだけで、開発用データベースにすべてのデータのサーバーに配置する、テスト目的でのみで、データの多くはおそらく存在するためです。 たとえば、テスト データベースの受講者名は、架空は。 その一方で、多くの場合、配置できませんにそのデータのないデータベース構造だけですべての。 テスト データベース内のデータの一部は実際のデータがありますであり、ユーザーがアプリケーションの使用を開始時に存在する必要があります。 たとえば、データベースには、有効グレードの値または実際の部署名を含むテーブルがあります。

この一般的なシナリオをシミュレートするには実稼働環境で存在する場合、データのみをデータベースに挿入するコードの最初の移行シード メソッドを構成します。 このシード メソッドは、Code First 実稼働環境で、データベースが作成された後に運用環境で実行するため、テスト データを挿入されません。

以前のバージョンの Code First Migrations がリリースされる前に、が一般的でしたシード メソッドも、テスト データを挿入する開発中にモデル変更のたびに、データベースは完全に削除され、最初から再作成する必要があるためです。 Code First Migrations を使用ためシード メソッドにテスト データを含める必要はありません、データベースの変更後にテスト データが保持されます。 ダウンロードしたプロジェクトには、すべてのデータを含む、シード クラスのメソッドで、初期化子の前の移行メソッドが使用されます。 このチュートリアルでを初期化子のクラスを無効にし、移行を有効にします。 更新する必要を移行構成クラスで Seed メソッドできるように、実稼働環境で挿入するデータのみが挿入されます。

次の図は、アプリケーション データベースのスキーマを示しています。

[![School_database_diagram](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image4.png)

これらのチュートリアルについては、していることを前提、`Student`と`Enrollment`サイトが最初に展開されたときに、テーブルが空にする必要があります。 その他のテーブルには、アプリケーションがライブをプリロードする必要があるデータが含まれます。

不要になったを使用する必要がある Code First Migrations を使用するため、 **DropCreateDatabaseIfModelChanges** Code First の初期化子。 この初期化子のコードは、SchoolInitializer.cs プロジェクト ファイルで、ContosoUniversity.DAL です。 設定、 **appSettings** Web.config ファイルの要素により、アプリケーションが最初にデータベースにアクセスしようとするたびに実行するため、この初期化子。

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample1.xml?highlight=3)]

アプリケーションの Web.config ファイルを開き、appSettings 要素から Code First の初期化子クラスを指定する要素を削除します。 AppSettings 要素は、次のようになります。

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample2.xml)]

> [!NOTE]
> 初期化子クラスを指定する別の方法は、呼び出すことによって行います`Database.SetInitializer`で、`Application_Start`メソッドで、 *Global.asax*ファイル。 そのメソッドを使用して、初期化子を指定するプロジェクトでの移行を有効にする場合は、そのコード行を削除します。


次に、Code First Migrations を有効にします。

最初の手順では、ContosoUniversity プロジェクトがスタートアップ プロジェクトとして設定されていることを確認してください。 **ソリューション エクスプ ローラー**ContosoUniversity プロジェクトを右クリックし、選択、**スタートアップ プロジェクトとして設定**です。 Code First Migrations は、データベース接続文字列を検索するスタートアップ プロジェクトになります。

**ツール** メニューのをクリックして**ライブラリ パッケージ マネージャー**し**Package Manager Console**です。

![Selecting_Package_Manager_Console](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image6.png)

上部にある、**パッケージ マネージャー コンソール**ウィンドウ êaƒ ContosoUniversity.DAL、既定のプロジェクトとし、at、`PM>`プロンプトは「有効な移行」を入力します。

![enable-migrations_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image7.png)

このコマンドを作成、*される Configuration.cs*を新しいファイル*移行*ContosoUniversity.DAL プロジェクト フォルダーにします。

![Migrations_folder_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image8.png)

Code First コンテキスト クラスを含むプロジェクトで"enable-migrations"コマンドを実行する必要があるために、DAL プロジェクトを選択します。 そのクラスは、クラス ライブラリ プロジェクトでは、Code First Migrations はソリューションのスタートアップ プロジェクトでデータベース接続文字列が検索されます。 ContosoUniversity ソリューションでは、web プロジェクトがスタートアップ プロジェクトとして設定されてです。 (Visual Studio のスタートアップ プロジェクトとして接続文字列を含むプロジェクトを指定しなかった場合を指定できます、スタートアップ プロジェクトの PowerShell コマンドでします。 Enable-migrations コマンドのコマンド構文を表示するには、入力、コマンド「get-help で有効な移行」します。)

される Configuration.cs ファイルを開き、内のコメントを置き換える、`Seed`メソッドを次のコード。

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample3.cs)]

参照`List`その下に赤い波線がある必要がないため、`using`まだその名前空間のステートメント。 インスタンスの 1 つを右クリックして`List` をクリック**解決**、順にクリック**using System.Collections.Generic**です。

![ステートメントを使用すると解決するには](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image9.png)

このメニュー項目が次のコードを追加、`using`ステートメント ファイルの上部にあります。

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample4.cs)]

> [!NOTE]
> コードを追加する、`Seed`メソッドは、固定データがデータベースに挿入できるさまざまな方法の 1 つです。 代わりに、コードを追加し、`Up`と`Down`各移行クラスのメソッドです。 `Up`と`Down`メソッドには、データベースの変更を実装するコードが含まれています。 それらの例が表示されます、[データベース更新の展開](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)チュートリアルです。
> 
> 使用して SQL ステートメントを実行するコードを記述することも、`Sql`メソッドです。 たとえば、Department テーブルを予算列を追加して、移行の一環として $1,000.00 をすべての部門予算を初期化するために必要な場合は、次のコード行を追加でした、`Up`その移行方法。
> 
> `Sql("UPDATE Department SET Budget = 1000");`
> 
> この例のこのチュートリアルでは、`AddOrUpdate`メソッドで、`Seed`の Code First Migrations メソッド`Configuration`クラスです。 Code First Migrations 呼び出し、`Seed`メソッドすべての移行した後に、このメソッドは、既に挿入されている、またはまだ存在しない場合は、それらを挿入する行を更新します。 `AddOrUpdate`メソッドをシナリオに最適な選択肢できない可能性があります。 詳細については、次を参照してください。 [EF 4.3 AddOrUpdate メソッドを使用して注意](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)Julie Lerman ブログ。


プロジェクトをビルドするには CTRL-shift キーを押し-B を押します。

次の手順が作成するには、`DbMigration`初期の移行のためのクラスです。 既に存在するデータベースを削除する必要があるため、新しいデータベースを作成するには、この移行ができます。 SQL Server Compact データベースが含まれている*.sdf*内のファイル、*アプリ\_データ*フォルダーです。 **ソリューション エクスプ ローラー**、展開*アプリ\_データ*ContosoUniversity プロジェクトで 2 つの SQL Server Compact データベースを表示する、これがによって表されます*.sdf*。ファイル。

右クリックし、 *School.sdf*ファイルし、クリックして**削除**です。

![sdf_files_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image10.png)

**Package Manager Console**ウィンドウで、「追加移行初期」コマンドを入力して最初の移行を作成し、「初期」という名前を付けます。

![add-migration_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image11.png)

Code First Migrations で別のクラス ファイルの作成、*移行*フォルダー、およびこのクラスは、データベース スキーマを作成するコードが含まれています。

**Package Manager Console**、コマンド"更新プログラム-データベースを入力します"、データベースを作成および実行する、**シード**メソッドです。

![update-database_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image12.png)

(テーブルが既に存在し、作成することはできませんを示すエラーが発生すると考えられますが、データベースを削除した後、および実行する前に、アプリケーションが実行された`update-database`です。 その場合、削除、 *School.sdf*ファイルをもう一度やり直して、`update-database`コマンドです)。

アプリケーションを実行します。 今すぐ受講者ページは空ですが、インストラクター ページには、インストラクターが含まれています。 これは、やりたいこと実稼働環境でアプリケーションを配置した後です。

![Empty_Students_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image13.png)

![Instructors_page_after_initial_migration](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image14.png)

プロジェクトは、展開する準備ができました、*学校*データベース。

## <a name="creating-a-membership-database-for-deployment"></a>メンバーシップ データベースの展開の作成

Contoso 大学アプリケーションでは、ASP.NET メンバーシップ システムとフォーム認証を使用して、ユーザー認証および承認します。 そのページの 1 つは、管理者のみにアクセスできます。 このページを表示するには、アプリケーションの実行を選択して**更新クレジット**下にあるサブメニューから**コース**です。 アプリケーションが表示されます、**ログで**ページで、管理者のみが使用する権限があるため、**更新クレジット**ページ。

[![Log_in_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image15.png)

"Admin"パスワード"Pa$ $w0rd"を使用してとしてログイン ("$w0rd"で文字"o"の代わりに数字のゼロに注意してください)。 では、ログインした後、**更新クレジット**ページが表示されます。

[![Update_Credits_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image17.png)

最初にサイトを展開するときは、ほとんどまたはすべてのテストを作成するユーザー アカウントを除外する一般的なです。 この場合、管理者アカウントとユーザー アカウントがありませんを展開します。 テスト アカウントを手動で削除するではなく、実稼働環境で必要がある 1 人の管理者ユーザー アカウントのみを持つ新しいメンバーシップ データベースを作成します。

> [!NOTE]
> メンバーシップ データベースには、アカウントのパスワードのハッシュが格納されます。 1 台のコンピューターから別のアカウントをデプロイするのには、ソース コンピューターの方がよりに、ハッシュのルーチンが、移行先サーバー上の別のハッシュを生成しないことを確認しての操作を行う必要があります。 生成されます、同じハッシュ ASP.NET ユニバーサル プロバイダーを使用すると、既定のアルゴリズムを変更しない限り、します。 既定のアルゴリズムはで指定された、HMACSHA256、**検証**の属性、  **[machineKey](https://msdn.microsoft.com/library/w8h3skw9.aspx)**  Web.config ファイル内の要素。


Code First Migrations、メンバーシップ データベースが管理されませんし、(School データベースの存在で) テスト アカウントを持つデータベースのシードを設定する自動初期化子はありません。 そのため、使用可能なテスト データを保持する、新しいものを作成する前にテスト データベースのコピーを作成します。

**ソリューション エクスプ ローラー**、名前を変更、 *aspnet.sdf*ファイルで、*アプリ\_データ*フォルダー *aspnet Dev.sdf*です。 (コピーを作成しないで、同じ名前を変更して、すぐに新しいデータベースを作成します)。

**ソリューション エクスプ ローラー**、web プロジェクト (ContosoUniversity、ContosoUniversity.DAL されません) が選択されていることを確認します。 次に、**プロジェクト**メニューの  **ASP.NET 構成**を実行する、 **Web サイト管理ツール**(WAT)。

選択、**セキュリティ**タブです。

[![WAT_Security_tab](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image19.png)

をクリックして**作成またはロールの管理**を追加し、**管理者**ロール。

[![WAT_Create_New_Role](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image21.png)

戻る、**セキュリティ** タブで、をクリックして**Create User**、し、管理者としてユーザー"admin"を追加します。 をクリックする前に、 **Create User**ボタンをクリックして、 **Create User**  ページを選択することを確認してください、**管理者**チェック ボックスをオンします。 このチュートリアルで使用されるパスワードは"Pa$ $w0rd"と、任意の電子メール アドレスを入力することができます。

[![WAT_Create_User](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image23.png)

ブラウザーを閉じます。 **ソリューション エクスプ ローラー**、新しい参照の更新ボタンをクリックして*aspnet.sdf*ファイル。

![New_aspnet.sdf_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image25.png)

右クリック**aspnet.sdf**選択**プロジェクトに含める**です。

## <a name="distinguishing-development-from-production-databases"></a>実稼働データベースから開発の違い

このセクションの名前に変更しますデータベース開発バージョンは、学校 Dev.sdf および学校 Prod.sdf と aspnet Prod.sdf には aspnet Dev.sdf、実稼働バージョンようにします。 必要に応じて、これはありませんが、そのために役立つデータベースのバージョンをテストおよび運用の混乱を取得するを防ぐ。

**ソリューション エクスプ ローラー**をクリックして**更新**アプリを展開し、\_先ほど作成した School データベースを参照してください以外の場合は右クリックしを選択するデータ フォルダー**プロジェクトに含める**.

![Including_School.sdf_in_project](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image26.png)

名前を変更*aspnet.sdf*に*aspnet Prod.sdf*です。

名前を変更*School.sdf*に*学校 Dev.sdf*です。

使用する Visual Studio でアプリケーションを実行すると、 *-Prod*を使用するデータベース ファイルのバージョン、 *- Dev*バージョン。 指すように、Web.config ファイル内の接続文字列を変更する必要があるため、 *- Dev*データベースのバージョン。 (学校 Prod.sdf ファイルを作成していないが Code First が作成されるため、データベースがあるアプリを実行する最初の実稼働時間には問題ありません)。

アプリケーションの Web.config ファイルを開き、接続文字列を検索します。

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample5.xml)]

"Aspnet-Dev.sdf"を"aspnet.sdf"に変更し、"学校 Dev.sdf"を"School.sdf"を変更します。

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample6.xml?highlight=4-5)]

SQL Server Compact データベース エンジンと両方のデータベースには、展開する準備ができました。 自動を設定する次のチュートリアルでは*Web.config*ファイルの開発、テスト、および実稼働環境で別にする必要がある設定の変換。 (接続文字列には、設定を変更する必要がありますが、発行プロファイルを作成するときに後でこれらの変更を設定します)。

## <a name="more-information"></a>説明

NuGet の詳細については、次を参照してください。 [NuGet のプロジェクトのライブラリを管理](https://msdn.microsoft.com/magazine/hh547106.aspx)と[NuGet のドキュメント](http://docs.nuget.org/docs/start-here/overview)です。 NuGet を使用しない場合は、インストールされている場合に、新機能を決定する NuGet パッケージを分析する方法を学習する必要があります。 (たとえば、構成が*Web.config*変換がビルド時などに実行する PowerShell スクリプトを構成します)。NuGet の動作方法についての詳細についてを参照してください特に[を作成して、パッケージを発行する](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package)と[構成ファイルとソース コードの変換](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations)です。

>[!div class="step-by-step"]
[前へ](deployment-to-a-hosting-provider-introduction-1-of-12.md)
[次へ](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
