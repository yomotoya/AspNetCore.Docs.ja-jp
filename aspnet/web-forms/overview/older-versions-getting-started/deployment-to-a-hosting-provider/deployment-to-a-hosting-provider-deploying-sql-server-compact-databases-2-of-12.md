---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
title: 'SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: を展開する SQL Server Compact データベースに 12 の 2 |Microsoft Docs'
author: tdykstra
description: このチュートリアル シリーズには、展開する方法を示します (発行) ASP.NET web アプリケーション プロジェクトを Visual Stu を使用して、SQL Server Compact データベースが含まれています.
ms.author: riande
ms.date: 11/17/2011
ms.assetid: c3c76516-4c48-4153-bd03-d70e3a3edbb0
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
msc.type: authoredcontent
ms.openlocfilehash: 378bcc038335ee852cd1a6c6e545eb72c6e0c78b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835796"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-sql-server-compact-databases---2-of-12"></a>SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: を展開する SQL Server Compact データベースに 12 の 2
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> この一連のチュートリアルは、展開する方法を示します (発行) ASP.NET web アプリケーション プロジェクトを Visual Studio 2012 RC または Visual Studio Express 2012 RC を for Web を使用して、SQL Server Compact データベースが含まれています。 Web の発行の更新をインストールする場合は、Visual Studio 2010 を使用することもできます。 シリーズの概要については、次を参照してください。[シリーズの最初のチュートリアル](deployment-to-a-hosting-provider-introduction-1-of-12.md)します。
> 
> Visual Studio 2012 RC のリリース後に導入された展開機能を示しています、SQL Server Compact 以外の SQL Server のエディションをデプロイする方法を示しています、および Azure App Service Web Apps にデプロイする方法を示していますチュートリアルでは、次を参照してください。 [ASP.NET Web 配置。Visual Studio を使用して](../../deployment/visual-studio-web-deployment/introduction.md)します。


## <a name="overview"></a>概要

このチュートリアルでは、2 つの SQL Server Compact データベースとデータベース エンジンの展開を設定する方法を示します。

データベースへのアクセス、Contoso University アプリケーションは、.NET Framework で指定していないために、アプリケーションと共に配置する必要があります、次のソフトウェアが必要です。

- [SQL Server Compact](https://www.microsoft.com/sqlserver/en/us/editions/compact.aspx) (データベース エンジン)。
- [ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (これを有効にすると、SQL Server Compact を使用して、ASP.NET メンバーシップ システム)
- [Entity Framework 5.0](https://msdn.microsoft.com/library/gg696172(d=lightweight,v=vs.103).aspx)(移行の最初のコード)。

データベースの構造と一部 (すべて)、アプリケーションの 2 つのデータのデータベースをデプロイすることも必要があります。 通常、アプリケーションを開発するときは、ライブ サイトに配置するデータベースにテスト データを入力します。 ただし、デプロイするいくつかの運用データを入力することも可能性があります。 このチュートリアルでは、必要なソフトウェアと正しいデータが含まように展開するときに、Contoso University のプロジェクトを構成します。

リマインダー: エラー メッセージを取得する、または、チュートリアルを進めるときに機能しないを必ず確認、[トラブルシューティング ページ](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)します。

## <a name="sql-server-compact-versus-sql-server-express"></a>SQL Server Express と SQL Server Compact

サンプル アプリケーションでは、SQL Server Compact 4.0 を使用します。 このデータベース エンジンは、web サイトの比較的新しいオプションです。以前のバージョンの SQL Server Compact は、web ホスティング環境では機能しません。 SQL Server Compact は、SQL Server Express を使用した開発と完全な SQL Server への展開の一般的なシナリオと比較して、いくつかの利点を提供しています。 選択したホスティング プロバイダーによって SQL Server Compact 可能性がありますより低いコストで展開するには、一部のプロバイダーには、完全な SQL Server データベースをサポートするために余分なが課金されます。 データベース エンジン自体は、web アプリケーションの一部として展開できるため、SQL Server Compact の追加料金はありません。

ただし、その制限事項に注意してくださかった。 必要があります。 SQL Server Compact はサポートしていませんストアド プロシージャ、トリガー、ビュー、またはレプリケーション。 (SQL Server Compact でサポートされていない SQL Server 機能の完全な一覧を参照してください[の相違点の間で SQL Server Compact および SQL Server](https://msdn.microsoft.com/library/bb896140.aspx)。)。また、SQL Server compact における SQL Server Express と SQL Server データベース スキーマとデータを操作に使用できるツールの一部機能できません。 たとえば、SQL Server Compact データベースに Visual Studio で SQL Server Management Studio または SQL Server Data Tools を使用できません。 SQL Server Compact データベースを操作するためには、その他のオプションには。

- SQL Server Compact の限られたデータベース操作の機能を提供する Visual Studio でサーバー エクスプ ローラーを使用することができます。
- データベース操作の機能を使用する[WebMatrix](https://www.microsoft.com/web/webmatrix/)、サーバー エクスプ ローラーより多くの機能を持ちます。
- 比較的フル装備のサード パーティ製を使用したりなどのオープン ソース ツールでは、 [SQL Server Compact Toolbox](https://github.com/ErikEJ/SqlCeToolbox)と[SQL Compact データとスキーマ スクリプト ユーティリティ](https://github.com/ErikEJ/SqlCeToolbox)します。
- 作成して、データベース スキーマを操作する独自の DDL (データ定義言語) スクリプトを実行できます。

SQL Server Compact で開始し、ニーズに応じて後でアップグレードできます。 このシリーズの以降のチュートリアルでは、SQL Server Compact から SQL Server express と SQL Server を移行する方法を説明します。 ただし、新しいアプリケーションを作成して、近い将来に SQL Server を必要だと思わ、SQL Server または SQL Server Express を起動することをおです。

## <a name="configuring-the-sql-server-compact-database-engine-for-deployment"></a>展開用の SQL Server Compact データベース エンジンの構成

Contoso University アプリケーションでデータ アクセスに必要なソフトウェアは、次の NuGet パッケージをインストールすることによって追加されました。

- [SqlServerCompact](http://nuget.org/List/Packages/SqlServerCompact)
- [System.Web.Providers](http://nuget.org/List/Packages/System.Web.Providers) (ASP.NET ユニバーサル プロバイダー)
- [EntityFramework](http://nuget.org/List/Packages/EntityFramework)
- [EntityFramework.SqlServerCompact](http://nuget.org/List/Packages/EntityFramework.sqlservercompact)

リンクは、このチュートリアルでダウンロードしたスタート プロジェクトにインストールされている内容よりも新しい可能性のあるこれらのパッケージの現在のバージョンをポイントします。 ホスティング プロバイダーにデプロイする場合に、Entity Framework 5.0 以降を使用することを確認します。 Code First Migrations の以前のバージョンが、完全な信頼を必要とし、多くのホスティング プロバイダーに、アプリケーションが中程度の信頼で実行されます。 中程度の信頼の詳細については、次を参照してください。、[テスト環境として IIS への配置](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)チュートリアル。

一般に、NuGet パッケージのインストールは、アプリケーションにこのソフトウェアを展開するために必要なすべての処理されます。 場合によっては、これには、Web.config ファイルを変更して、ソリューションをビルドするたびに実行する PowerShell スクリプトを追加するなどのタスクが含まれます。 **NuGet を使用せずにこれらの機能 (SQL Server Compact や Entity Framework) のいずれかのサポートを追加する場合は、NuGet パッケージのインストールが、同じ作業を手動で実行できるようににがわかっていることを確認します。**

NuGet が処理するには、展開の成功を保証するために行う必要があるすべてのものを実行しない 1 つの例外があります。 SqlServerCompact NuGet パッケージをプロジェクトにネイティブ アセンブリをコピーするビルド後のスクリプトを追加します*x86*と*amd64*プロジェクトの下のサブフォルダー *bin*フォルダーでは、スクリプトは、プロジェクトでそれらのフォルダーを含みません。 その結果、Web Deploy はコピーしませんそれらのターゲットの web サイト プロジェクトに手動で含める場合を除き。 (この動作は、既定の配置構成の結果は、これらのチュートリアルでは使用しません、別のオプションは、この動作を制御する設定を変更します。 変更できる設定は**アプリケーションの実行に必要なファイルのみ** **配置する項目**で、**パッケージ/web の発行**のタブ、**プロジェクトプロパティ**ウィンドウ。 この設定を変更するは一般的に使用しないで運用環境が必要以上に多くの詳細ファイルの展開でしまうためです。 その他の方法の詳細については、次を参照してください、[プロジェクト プロパティを設定する](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)チュートリアルです。)。

プロジェクトをビルドし、**ソリューション エクスプ ローラー**クリックして**ファイルをすべて表示**まだ行っていない場合。 をクリックする必要がありますも**更新**します。

![Solution_Explorer_Show_All_Files](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image1.png)

展開、 **bin**フォルダーを参照してください、 **amd64**と**x86**フォルダー、それらのフォルダーを右クリックしと選択をクリックして**をプロジェクトに含める**.

![amd64_and_x86_in_Solution_Explorer.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image2.png)

フォルダーのアイコンに変更、フォルダーをプロジェクトに含まれていることを表示します。

![Solution_Explorer_amd64_included.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image3.png)

## <a name="configuring-code-first-migrations-for-application-database-deployment"></a>アプリケーション データベースの配置の Code First Migrations を構成します。

アプリケーション データベースを展開するときに通常しないデプロイするだけですべてのデータのデータベースを開発、運用環境にテスト目的でのみ内のデータの多くはおそらくありますので。 たとえば、テスト データベース内の学生の名前は、架空です。 その一方で、多くの場合、配置できませんデータことのないデータベース構造だけですべての。 テスト データベースでデータの一部は、実際のデータがあり、ユーザーがアプリケーションの使用を開始する場合がある必要があります。 たとえば、データベースには、グレードが有効な値または実際の部署名を含むテーブルがあります。

この一般的なシナリオをシミュレートするには、実稼働環境であるデータのみをデータベースに挿入するコードの最初の移行の Seed メソッドを構成します。 このシード メソッドは、Code First に実稼働環境でデータベースを作成した後、運用環境で実行されますので、テスト データを挿入しません。

以前のバージョンの Code First Migrations がリリースされる前に、が一般的でした Seed メソッドも、テスト データを挿入する開発中にモデル変更のたびに、データベースは完全に削除され、最初から再作成する必要があるためです。 Code First Migrations で Seed メソッドのテスト データを含む必要はありませんので、データベースの変更後にテスト データが保持されます。 ダウンロードしたプロジェクトでは、初期化子クラスの Seed メソッドのすべてのデータを含む前の移行メソッドを使用します。 このチュートリアルでは、初期化子クラスを無効にし、移行を有効にします。 運用環境で挿入するデータのみを挿入するための移行の構成クラスの Seed メソッドを更新します。

次の図は、アプリケーション データベースのスキーマを示しています。

[![School_database_diagram](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image4.png)

これらのチュートリアルについてを想定します、`Student`と`Enrollment`サイトが最初に展開された場合に、テーブルが空にする必要があります。 その他のテーブルには、アプリケーションの運用時にプリロードする必要があるデータが含まれます。

不要になったを使用する必要がある Code First Migrations を使用するため、 **DropCreateDatabaseIfModelChanges** Code First 初期化子。 この初期化子のコードは SchoolInitializer.cs ファイル ContosoUniversity.DAL プロジェクト内です。 設定、 **appSettings** Web.config ファイルの要素により、アプリケーションを初めてデータベースにアクセスしようとするたびに実行するこの初期化子。

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample1.xml?highlight=3)]

アプリケーションの Web.config ファイルを開き、appSettings 要素から Code First 初期化子クラスを指定する要素を削除します。 AppSettings 要素は、次のようになります。

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample2.xml)]

> [!NOTE]
> 初期化子クラスを指定することもできますが、それを呼び出すことによって行います`Database.SetInitializer`で、`Application_Start`メソッドで、 *Global.asax*ファイル。 そのメソッドを使用して、初期化子を指定するプロジェクトでの移行を有効にする場合は、そのコード行を削除します。


次に、Code First Migrations を有効にします。

最初の手順では、ContosoUniversity プロジェクトがスタートアップ プロジェクトとして設定されていることを確認します。 **ソリューション エクスプ ローラー**ContosoUniversity プロジェクトを右クリックし、選択、**スタートアップ プロジェクトとして設定**します。 Code First Migrations は、データベース接続文字列を検索するスタートアップ プロジェクトになります。

**ツール** メニューのをクリックして**ライブラリ パッケージ マネージャー**し**パッケージ マネージャー コンソール**します。

![Selecting_Package_Manager_Console](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image6.png)

上部にある、**パッケージ マネージャー コンソール**ウィンドウとして、既定のプロジェクトとし、at ContosoUniversity.DAL を選択して、`PM>`プロンプトは「有効な移行」を入力します。

![enable-migrations_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image7.png)

このコマンドは、作成、 *Configuration.cs*で新しいファイル*移行*ContosoUniversity.DAL プロジェクトのフォルダー。

![Migrations_folder_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image8.png)

Code First のコンテキスト クラスを含むプロジェクトで"enable-migrations"コマンドを実行する必要があるために、DAL プロジェクトを選択します。 そのクラスは、クラス ライブラリ プロジェクトでは、Code First Migrations は、ソリューションのスタートアップ プロジェクト内のデータベース接続文字列が検索されます。 ContosoUniversity ソリューションで web プロジェクトがスタートアップ プロジェクトとして設定されています。 (Visual Studio のスタートアップ プロジェクトとして、接続文字列を含むプロジェクトを指定しなかった場合は、指定できますスタートアップ プロジェクトでの PowerShell コマンド。 Enable-migrations コマンドのコマンド構文を表示するには、ことができますを入力する、コマンド「get-help 有効にする-移行」です。)

Configuration.cs ファイルを開き、内のコメントを置き換える、`Seed`メソッドを次のコード。

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample3.cs)]

参照`List`必要ないため、その下に赤い波線がある、`using`まだその名前空間のステートメント。 インスタンスの 1 つを右クリックして`List` をクリック**解決**、順にクリックします**System.Collections.Generic を使用して**します。

![ステートメントを使用すると解決します。](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image9.png)

このメニュー項目が次のコードを追加します、`using`ファイルの上部にステートメント。

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample4.cs)]

> [!NOTE]
> コードを追加、`Seed`メソッドは、固定データがデータベースに挿入できるさまざまな方法の 1 つです。 代わりに、コードを追加し、`Up`と`Down`各移行クラスのメソッド。 `Up`と`Down`メソッドには、データベースの変更を実装するコードが含まれています。 これらの例が表示されます、[データベース更新の展開](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)チュートリアル。
> 
> 使用して SQL ステートメントを実行するコードを記述することも、`Sql`メソッド。 たとえば、Department テーブルを予算の列を追加し、移行の一環として、すべての部門予算を $1,000.00 を初期化するために必要な場合は、コードの次の行を追加でした、`Up`その移行のためのメソッド。
> 
> `Sql("UPDATE Department SET Budget = 1000");`
> 
> この例を示すに対する、このチュートリアルでは、`AddOrUpdate`メソッドで、 `Seed` Code First Migrations のメソッド`Configuration`クラス。 Code First Migrations を呼び出し、`Seed`メソッドすべての移行後に、このメソッドは、既に挿入されている、またはまだ存在していない場合は、それらを挿入する行を更新します。 `AddOrUpdate`メソッドには、シナリオに最適な選択肢をできない可能性があります。 詳細については、次を参照してください。 [EF 4.3 AddOrUpdate メソッドを使用して対処](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)Julie Lerman のブログ。


CTRL + SHIFT+B プロジェクトをビルドするキーを押します。

次の手順が作成するには、`DbMigration`初回移行のためのクラス。 既に存在するデータベースを削除する必要があるため、新しいデータベースを作成するには、この移行します。 SQL Server Compact データベースに含まれる *.sdf*内のファイル、*アプリ\_データ*フォルダー。 **ソリューション エクスプ ローラー**、展開*アプリ\_データ*2 つの SQL Server Compact データベースを表示する ContosoUniversity プロジェクトでこれがによって表されます *.sdf*。ファイル。

右クリックし、 *School.sdf*ファイルし、クリックして**削除**します。

![sdf_files_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image10.png)

**パッケージ マネージャー コンソール**ウィンドウで、"add-migration Initial"コマンドを入力して、初期移行を作成し、「初期」という名前を付けます。

![add-migration_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image11.png)

Code First Migrations は、別のクラス ファイルを作成、*移行*フォルダー、およびこのクラスは、データベース スキーマを作成するコードが含まれています。

**パッケージ マネージャー コンソール**、コマンド「更新プログラム-データベース」、データベースを作成し、実行の入力、**シード**メソッド。

![update-database_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image12.png)

(テーブルが既に存在し、作成することはできませんを示すエラーが発生する場合は可能性があります、データベースを削除した後、および実行する前にアプリケーションを実行するため、`update-database`します。 見つからず、削除、 *School.sdf*ファイルをもう一度やり直して、`update-database`コマンド)。

アプリケーションを実行します。 今すぐ Students のページは空ですが、Instructors ページには、インストラクターが含まれています。 これは、利用できるもの実稼働環境でアプリケーションを展開した後です。

![Empty_Students_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image13.png)

![Instructors_page_after_initial_migration](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image14.png)

プロジェクトは、デプロイする準備ができました、*学校*データベース。

## <a name="creating-a-membership-database-for-deployment"></a>展開のメンバーシップ データベースを作成します。

Contoso University アプリケーションでは、ASP.NET メンバーシップ システムとフォーム認証を使用して、ユーザー認証および承認します。 そのページの 1 つは管理者のみがアクセスできます。 このページを表示するアプリケーションを実行し、選択**更新クレジット**フライアウト メニューから**コース**します。 アプリケーションが表示されます、**ログで**ページで、管理者のみが使用する権限があるため、**更新クレジット**ページ。

[![Log_in_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image15.png)

「管理者」は"Pa$ $w0rd"パスワードを使用してログイン ("$w0rd"で文字"o"の代わりに、数値の 0 に注意してください)。 では、ログインした後、**更新クレジット**ページが表示されます。

[![Update_Credits_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image17.png)

最初に、サイトを展開するときに、ほとんどまたはすべてのテストを作成するユーザー アカウントを除外する一般的なは。 この場合、管理者アカウントとユーザー アカウントをデプロイします。 テスト アカウントを手動で削除するではなく、運用環境で必要のある 1 人の管理者ユーザー アカウントのみを含む新しいメンバーシップ データベースを作成します。

> [!NOTE]
> メンバーシップ データベースには、アカウントのパスワードのハッシュが格納されます。 1 台のコンピューターから別のアカウントをデプロイするには、ハッシュのルーチンも、これは、移行元コンピューターには、移行先サーバーで異なるハッシュを生成しないことを確認してください。 既定のアルゴリズムを変更しない限り、ASP.NET ユニバーサル プロバイダーを使用する場合に、同じハッシュを生成、されます。 既定のアルゴリズムはで指定された、HMACSHA256、**検証**の属性、 **[machineKey](https://msdn.microsoft.com/library/w8h3skw9.aspx)** Web.config ファイル内の要素。


Code First Migrations では、メンバーシップ データベースが管理されませんし、ようになります (School データベースの) テスト アカウントを使用してデータベースをシードする自動初期化子はありません。 そのため、使用可能なテスト データを保持する、新しいものを作成する前に、テスト データベースのコピーを作成します。

**ソリューション エクスプ ローラー**、名前を変更、 *aspnet.sdf*ファイル、*アプリ\_データ*フォルダー *aspnet Dev.sdf*します。 (コピーを作成するはその名前を変更しません-すぐに新しいデータベースを作成します。)。

**ソリューション エクスプ ローラー**、web プロジェクト (ContosoUniversity、ContosoUniversity.DAL されません) が選択されていることを確認します。 次に、**プロジェクト**メニューの  **ASP.NET 構成**を実行する、 **Web サイト管理ツール**(WAT)。

選択、**セキュリティ**タブ。

[![WAT_Security_tab](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image19.png)

をクリックして**管理ロールの作成または**を追加し、**管理者**ロール。

[![WAT_Create_New_Role](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image21.png)

戻り、**セキュリティ**] タブで [**ユーザーの作成**、"admin"ユーザーを管理者として追加するとします。 クリックする前に、 **Create User**ボタンを**Create User**  ページを選択するかどうかを確認、**管理者**チェック ボックスをオンします。 このチュートリアルで使用されるパスワードは"Pa$ $w0rd"と、任意の電子メール アドレスを入力することができます。

[![WAT_Create_User](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image23.png)

ブラウザーを閉じます。 **ソリューション エクスプ ローラー**、新しいに更新 ボタンをクリックします。 *aspnet.sdf*ファイル。

![New_aspnet.sdf_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image25.png)

右クリックして**aspnet.sdf**選択**プロジェクトに含める**します。

## <a name="distinguishing-development-from-production-databases"></a>実稼働データベースからの特徴的な開発

このセクションで開発バージョンは、学校 Dev.sdf と aspnet Dev.sdf と実稼働バージョンは学校 Prod.sdf と aspnet Prod.sdf ようにデータベースを変更します。 これは、必要に応じてが行うのために役立つ混乱テストと運用環境のデータベースのバージョンを取得することを防止します。

**ソリューション エクスプ ローラー**、 をクリックして**更新**アプリを展開および\_先ほど作成した School データベースを参照してください右クリックし、選択するデータ フォルダー**プロジェクトに含める**。

![Including_School.sdf_in_project](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image26.png)

名前を変更*aspnet.sdf*に*aspnet Prod.sdf*します。

名前を変更*School.sdf*に*の学校 Dev.sdf*します。

使用する Visual Studio でアプリケーションを実行すると、 *-Prod*を使用するデータベース ファイルのバージョン、 *Dev*バージョン。 ポイントするように、Web.config ファイル内の接続文字列を変更する必要があるため、 *Dev*データベースのバージョン。 (学校 Prod.sdf ファイルを作成していないが Code First はデータベースを作成するがアプリを実行する最初の実稼働環境のために [ok] です)。

アプリケーションの Web.config ファイルを開き、接続文字列を検索します。

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample5.xml)]

"Aspnet Dev.sdf"、"aspnet.sdf"に変更し、"学校 Dev.sdf"を"School.sdf"を変更します。

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample6.xml?highlight=4-5)]

SQL Server Compact のデータベース エンジンと両方のデータベースには、展開する準備ができました。 自動を設定する次のチュートリアルでは*Web.config*開発、テスト、および実稼働環境で別にする必要がある設定のファイルの変換。 (接続文字列には、設定を変更する必要がありますが、発行プロファイルを作成するときに後でこれらの変更を設定します)。

## <a name="more-information"></a>説明

NuGet の詳細については、次を参照してください。 [nuget プロジェクトのライブラリを管理](https://msdn.microsoft.com/magazine/hh547106.aspx)と[NuGet のドキュメント](http://docs.nuget.org/docs/start-here/overview)します。 NuGet を使用しない場合がインストールされているときの動作を決定する NuGet パッケージを分析する方法を説明する必要があります。 (たとえば、構成が*Web.config*変換、ビルド時などに実行する PowerShell スクリプトを構成します)。NuGet の動作方法について詳しくは、特にを参照してください[を作成して、パッケージを公開する](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package)と[構成ファイルとソース コード変換](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations)します。

> [!div class="step-by-step"]
> [前へ](deployment-to-a-hosting-provider-introduction-1-of-12.md)
> [次へ](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
