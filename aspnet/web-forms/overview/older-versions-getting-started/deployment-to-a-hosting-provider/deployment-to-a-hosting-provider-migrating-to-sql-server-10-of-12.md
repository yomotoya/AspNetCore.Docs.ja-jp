---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
title: 'SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: SQL Server/10/12 への移行 |Microsoft Docs'
author: tdykstra
description: このチュートリアル シリーズには、展開する方法を示します (発行) ASP.NET web アプリケーション プロジェクトを Visual Stu を使用して、SQL Server Compact データベースが含まれています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a89d6f32-b71b-4036-8ff7-5f8ac2a6eca8
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
msc.type: authoredcontent
ms.openlocfilehash: 5c26baddfabeea8aee12619019c76c36b15a504e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37392913"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-migrating-to-sql-server---10-of-12"></a>SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: SQL Server/10/12 への移行
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> この一連のチュートリアルは、展開する方法を示します (発行) ASP.NET web アプリケーション プロジェクトを Visual Studio 2012 RC または Visual Studio Express 2012 RC を for Web を使用して、SQL Server Compact データベースが含まれています。 Web の発行の更新をインストールする場合は、Visual Studio 2010 を使用することもできます。 シリーズの概要については、次を参照してください。[シリーズの最初のチュートリアル](deployment-to-a-hosting-provider-introduction-1-of-12.md)します。
> 
> Visual Studio 2012 RC のリリース後に導入された展開機能を示しています、SQL Server Compact 以外の SQL Server のエディションをデプロイする方法を示しています、および Azure App Service Web Apps にデプロイする方法を示していますチュートリアルでは、次を参照してください。 [ASP.NET Web 配置。Visual Studio を使用して](../../deployment/visual-studio-web-deployment/introduction.md)します。


## <a name="overview"></a>概要

このチュートリアルでは、SQL Server Compact から SQL Server に移行する方法を示します。 そう理由の 1 つは、SQL Server Compact をサポートしていないストアド プロシージャ、トリガー、ビュー、またはレプリケーションなどの SQL Server 機能の活用することです。 SQL Server Compact および SQL Server 間の違いの詳細については、次を参照してください。、[を展開する SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)チュートリアル。

### <a name="sql-server-express-versus-full-sql-server-for-development"></a>開発用の完全な SQL Server と SQL Server Express

SQL Server にアップグレードする決めたら、開発およびテスト環境内で SQL Server または SQL Server Express を使用する可能性があります。 データベース エンジンの機能とツールのサポートでの違い、SQL Server Compact とその他のバージョンの SQL Server プロバイダーの実装で違いがあります。 これらの違いには、同じコードを異なる結果を生成する可能性があります。 そのため、SQL Server Compact、開発用データベースとして保持する場合は、必ず十分なテストは、サイトで SQL Server または SQL Server Express を運用環境には、各デプロイの前に、テスト環境でします。

異なり、SQL Server Compact は SQL Server Express を同じデータベース エンジンでは基本的には、および完全な SQL Server と同じ .NET プロバイダーを使用します。 SQL Server Express を使ってテストするときに、SQL Server では、同じ結果を取得することを確信できます。 SQL Server express と SQL Server に使用できるほとんどのデータベースと同じツールを使用することができます (注目すべき例外される[SQL Server Profiler](https://msdn.microsoft.com/library/ms181091.aspx))、SQL Server のストアド プロシージャ、ビュー、トリガーなどの他の機能をサポートしていますおよびレプリケーション。 (通常、ただし、運用環境の web サイトで完全な SQL Server を使用する必要があります。 SQL Server Express は、共有ホスティング環境で実行できますが、向けに設計されていないと多くのホスティング プロバイダーはサポートされません。)

Visual Studio 2012 を使用している場合は、通常、開発環境の SQL Server Express LocalDB を選択する Visual Studio では既定でインストールされている内容であるためです。 ただし、LocalDB では機能しません、IIS、テスト環境は、SQL Server または SQL Server Express のいずれかを使用する必要があるためです。

### <a name="combining-databases-versus-keeping-them-separate"></a>データベースを個別に維持すると結合

Contoso University アプリケーションが 2 つの SQL Server Compact データベース: メンバーシップ データベース (*aspnet.sdf*) とアプリケーション データベース (*School.sdf*)。 移行する場合、2 つの異なるデータベースまたは単一のデータベースにこれらのデータベースを移行できます。 アプリケーションのデータベースと、メンバーシップ データベース間のデータベースの結合を容易にするため、それらを結合する可能性があります。 ホスティング プランには、それらを結合するための提供も可能性があります。 など、ホスティング プロバイダーは、複数のデータベースの詳細は課金可能性があります。 または 1 つ以上のデータベースをも許可しない場合があります。 Cytanium Lite でこのチュートリアルでは、により、SQL Server データベースの 1 つでのみ使用されるアカウントをホストしている場合です。

このチュートリアルでこのように、2 つのデータベースを移行します。

- 開発環境で 2 つの LocalDB データベースに移行します。
- テスト環境で 2 つの SQL Server Express データベースに移行します。
- 1 つ結合された完全な SQL Server データベースの運用環境に移行します。

リマインダー: エラー メッセージを取得する、または、チュートリアルを進めるときに機能しないを必ず確認、[トラブルシューティング ページ](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)します。

## <a name="installing-sql-server-express"></a>SQL Server Express をインストールします。

既定では、Visual Studio 2010 が自動的にインストールされている SQL Server Express しますが、既定でインストールされていない Visual Studio 2012 を使用します。 SQL Server 2012 Express をインストールするには、次のリンクをクリックします。

- [SQL Server Express 2012](https://www.microsoft.com/download/details.aspx?id=29062)

選択*日本語/x64/SQLEXPR\_x64\_ENU.exe*または*日本語/x86/SQLEXPR\_x86\_ENU.exe*、し、インストール ウィザードで、既定を受け入れます設定。 インストール オプションの詳細については、次を参照してください。[インストール ウィザード (セットアップ) から SQL Server 2012 のインストール](https://msdn.microsoft.com/library/ms143219.aspx)します。

## <a name="creating-sql-server-express-databases-for-the-test-environment"></a>テスト環境用の SQL Server Express データベースを作成します。

次の手順では、ASP.NET メンバーシップと School データベースを作成します。

**ビュー**メニューの **サーバー エクスプ ローラー** (**データベース エクスプ ローラー** Visual Web Developer で)、右クリックし、**データ接続**選択**新しい SQL Server データベースの作成**です。

![Selecting_Create_New_SQL_Server_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image1.png)

**新しい SQL Server データベースの作成** ダイアログ ボックスに、入力". \SQLExpress"で、**サーバー名**ボックスと"aspnet Test"で、**新しいデータベース名** をクリックし、**OK**します。

![Create_New_SQL_Server_Database_aspnet](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image2.png)

"学校 Test"という名前の新しい SQL Server Express の School データベースを作成する同じ手順に従います。

(追加する"Test"これらのデータベース名を後で開発環境では、各データベースの追加のインスタンスを作成し、データベースの 2 つのセットを区別するためにできるようにする必要があるためです。)

**サーバー エクスプ ローラー** 2 つの新しいデータベースが表示されます。

![New_databases_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image3.png)

## <a name="creating-a-grant-script-for-the-new-databases"></a>新しいデータベースの許可スクリプトを作成します。

アプリケーションを開発用コンピューターに IIS で実行すると、アプリケーションは既定のアプリケーション プールの資格情報を使用して、データベースにアクセスします。 ただし、既定では、アプリケーション プール id は、データベースを開くアクセス許可がありません。 アクセス許可を付与するスクリプトを実行する必要がようにします。 このセクションでは、IIS での実行時に、アプリケーションが、データベースを開くことができるかどうかを確認する後で実行しますスクリプトを作成します。

ソリューションの*SolutionFiles*で作成したフォルダー、[実稼働環境に展開する](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)チュートリアルでは、という名前の新しい SQL ファイルを作成する*Grant.sql*します。 ファイルに次の SQL コマンドをコピーおよび保存して、ファイルを閉じる。

[!code-sql[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample1.sql)]

> [!NOTE]
> このスクリプトは、このチュートリアルで指定されている SQL Server 2008 と Windows 7 で IIS 設定の動作設計されています。 または、Windows の SQL Server の別のバージョンを使用している場合、または IIS を設定するコンピューターに異なる場合は、このスクリプトへの変更が必要な場合があります。 SQL Server スクリプトの詳細については、次を参照してください。 [SQL Server オンライン ブックの「](https://go.microsoft.com/fwlink/?LinkId=132511)します。


> [!NOTE] 
> 
> **セキュリティに関する注意**このスクリプトは、db\_、運用環境でがありますが、実行時にデータベースにアクセスするユーザーに対する所有者アクセス許可。 一部のシナリオで、展開にのみアクセス許可を更新し、実行時のデータを読み書きするのみのアクセス許可を持つ別のユーザーを指定します。 データベースの完全スキーマを持つユーザーを指定する場合があります。 詳細については、次を参照してください。 **Code First Migrations に対する自動の Web.config の変更をレビュー**で[テスト環境として IIS への配置](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)します。


## <a name="configuring-database-deployment-for-the-test-environment"></a>テスト環境用データベース デプロイの構成

次に、データベースごとに、次のタスクを行うことができるように、Visual Studio を構成します。

- 転送先データベースで、ソース データベースの構造 (テーブル、列、制約など) を作成する SQL スクリプトを生成します。
- 転送先データベース内のテーブルにソース データベースのデータを挿入する SQL スクリプトを生成します。
- 生成されたスクリプト、および転送先データベースで、作成した許可スクリプトを実行します。

開く、**プロジェクト プロパティ**ウィンドウと選択、**パッケージ化/発行 SQL**タブ。

確認します**アクティブ (リリース)** または**リリース**でが選択されている、**構成**ドロップダウン リスト。

クリックして**このページを有効にする**します。

![Package_Publish_SQL_tab_Enable_This_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image4.png)

**パッケージ化/発行 SQL**  タブが従来のデプロイ方法を指定するために通常無効になります。 ほとんどのシナリオでデータベースの配置を構成する必要があります、 **Web の発行**ウィザード。 SQL Server Compact から SQL Server または SQL Server Express への移行は、このメソッドは、適切な選択特殊なケースです。

クリックして**web.config ファイルからインポート**します。

![Selecting_Import_from_Web.config](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image5.png)

Visual Studio での接続文字列では、 *Web.config*ファイル、メンバーシップ データベースに 1 つと、School データベースに 1 つを検索および内の各接続文字列に対応する行を追加します、**データベース エントリ**テーブル。 既存の SQL Server Compact データベースの接続文字列が見つかったがあり、次の手順は、方法と場所を構成するこれらのデータベースを展開します。

データベースの配置設定を入力する、**データベース エントリの詳細**以下のセクション、**データベース エントリ**テーブル。 表示される設定、**データベース エントリの詳細**セクションに関連する行の方、**データベース エントリ**テーブルが選択されている、次の図に示すようにします。

![Database_Entry_Details_section_of_Package_Publish_SQL_tab](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image6.png)

### <a name="configuring-deployment-settings-for-the-membership-database"></a>メンバーシップ データベースの展開設定を構成します。

選択、 **DefaultConnection 展開**行、**データベース エントリ**メンバーシップ データベースに適用される設定を構成するにはテーブルです。

**転送先データベースへの接続文字列**、新しい SQL Server Express のメンバーシップ データベースを指す接続文字列を入力します。 必要な接続文字列を取得できます**サーバー エクスプ ローラー**します。 **サーバー エクスプ ローラー**、展開**データ接続**を選択し、 **aspnetTest**データベースから、**プロパティ**ウィンドウのコピー、**接続文字列**値。

![aspnet_connection_string_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image7.png)

同じ接続文字列はここに再掲します。

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample2.cmd)]

コピーして貼り付けるには、この接続文字列**転送先データベースへの接続文字列**で、**パッケージ化/発行 SQL**タブ。

確認します**データや既存のデータベースからスキーマをプル**が選択されています。 これは、自動的に生成され、転送先データベースで実行する SQL スクリプトの原因は何です。

**ソース データベースの接続文字列**から値を抽出、 *Web.config*ファイルと開発用の SQL Server Compact データベースへのポインター。 これは、転送先データベースで後で実行されるスクリプトを生成するために使用するソース データベースです。 データベースの運用バージョンをデプロイするため、"aspnet Dev.sdf"を"aspnet Prod.sdf"に変更します。

変更**データベース スクリプト オプション**から**スキーマのみ**に**スキーマとデータ**データベース構造と (ユーザー アカウントとロール)、データをコピーするため、します。

先ほど作成した許可スクリプトを実行する展開を構成するに追加する必要が、**データベース スクリプト**セクション。 をクリックして**スクリプトの追加**、し、 **SQL スクリプトの追加** ダイアログ ボックスで、許可スクリプトを保存したフォルダーに移動します (これは、ソリューション ファイルを含むフォルダーです)。 という名前のファイルを選択します。 *Grant.sql*、 をクリック**オープン**します。

[![Select_File_dialog_box_grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image9.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image8.png)

設定、 **DefaultConnection 展開**行**データベース エントリ**次の図のようになります。

![Database_Entry_Details_for_DefaultConnection_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image10.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>School データベースの展開設定を構成します。

次に、選択、 **SchoolContext 展開**行、**データベース エントリ**School データベースの展開設定を構成するためにテーブルです。

新しい SQL Server Express データベースの接続文字列を取得する前に使用した同じメソッドを使用することができます。 この接続文字列をコピー**転送先データベースへの接続文字列**で、**パッケージ化/発行 SQL**タブ。

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample3.cmd)]

確認します**データや既存のデータベースからスキーマをプル**が選択されています。

**ソース データベースの接続文字列**から値を抽出、 *Web.config*ファイルと開発用の SQL Server Compact データベースへのポインター。 "学校 Prod.sdf"実稼働バージョンのデータベースを展開するには、"学校 Dev.sdf"を変更します。 (アプリに学校 Prod.sdf ファイルを作成することはありません\_データ フォルダー、アプリに、テスト環境からそのファイルをコピーします\_ContosoUniversity プロジェクト フォルダーを後で、データ フォルダーです)。

変更**データベース スクリプト オプション**に**スキーマとデータ**します。

読み取りを許可し、アプリケーション プール id にこのデータベースに対する権限を書き込む追加して、スクリプトを実行する、 *Grant.sql*メンバーシップ データベースの場合と同様のスクリプト ファイル。

完了したら、設定**SchoolContext 展開**行**データベース エントリ**次の図のようになります。

![Database_Entry_Details_for_SchoolContext_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image11.png)

変更を保存、**パッケージ化/発行 SQL**タブ。

コピー、*の学校 Prod.sdf*ファイルから、 *c:\inetpub\wwwroot\ContosoUniversity\App\_データ*フォルダーを*アプリ\_データ*フォルダーContosoUniversity プロジェクトです。

### <a name="specifying-transacted-mode-for-the-grant-script"></a>許可スクリプトのトランザクション モードを指定します。

展開プロセスでは、データベース スキーマとデータ デプロイ スクリプトを生成します。 既定では、これらのスクリプトは、トランザクションで実行します。 ただし、既定で (grant スクリプト) のようなカスタム スクリプトは、トランザクションでは実行されません。 展開プロセスでは、トランザクション モードを混在、デプロイ時に、スクリプトが実行される場合にタイムアウト エラーが発生する可能性があります。 このセクションでは、トランザクションで実行するカスタム スクリプトを構成するには、プロジェクト ファイルを編集します。

**ソリューション エクスプ ローラー**を右クリックし、 **ContosoUniversity**順に選択して**プロジェクトのアンロード**します。

![Unload_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image12.png)

プロジェクトをもう一度右クリックし、選択**contosouniversity.csproj の編集**します。

![Edit_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image13.png)

Visual Studio エディターでは、プロジェクト ファイルの XML コンテンツを示します。 いくつに注意してください。`PropertyGroup`要素。 (図の内容で、`PropertyGroup`要素が省略されています)。

![プロジェクト ファイルのエディター ウィンドウ](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image15.png)

ない最初の 1 つは、`Condition`属性、構成のビルドに関係なく適用される設定。 1 つ`PropertyGroup`要素が、デバッグ ビルド構成にのみ適用されます (注、`Condition`属性)、リリース ビルド構成の場合にのみ適用し、テストのビルド構成にのみ適用されます。 内で、`PropertyGroup`リリース ビルド構成の要素が表示されます、`PublishDatabaseSettings`で入力した設定を含む要素、**パッケージ化/発行 SQL**タブ。`Object` Grant スクリプトのそれぞれに対応する要素 ("Grant.sql"のインスタンスを 2 つに注目してください) を指定します。 既定で、`Transacted`の属性、 `Source` grant スクリプトごとに要素が`False`します。

![Transacted_false](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image16.png)

値を変更、`Transacted`の属性、`Source`要素`True`します。

![Transacted_true](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image17.png)

保存して、プロジェクト ファイルを閉じてでプロジェクトを右クリックし、**ソリューション エクスプ ローラー**選択**プロジェクトの再読み込み**します。

![Reload_project](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image18.png)

## <a name="setting-up-webconfig-transformations-for-the-connection-strings"></a>接続文字列の Web.Config 変換の設定

接続文字列入力した新しい SQL Express データベースの**パッケージ化/発行 SQL**  タブは、デプロイ中に転送先データベースを更新するためだけ Web Deploy によって使用されます。 設定する必要がある*Web.config*変換、接続文字列ので、デプロイされているように*Web.config*ファイルの新しい SQL Server Express データベースをポイントします。 (使用すると、**パッケージ化/発行 SQL**  タブで、発行プロファイルの接続文字列を構成することはできません)。

開いている*Web.Test.config*と置換、`connectionStrings`を持つ要素、`connectionStrings`次の例では、内の要素。 (コンテキストを提供するは、ここに表示される周囲のコードされません、connectionStrings 要素のみをコピーすることを確認してください。)

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample4.xml?highlight=2-11)]

このコードにより、`connectionString`と`providerName`の各属性`add`置き換えられる要素で、デプロイされている*Web.config*ファイル。 これらの接続文字列で入力したものとは異なるが、**パッケージ化/発行 SQL**タブ。設定"MultipleActiveResultSets = True"の Entity Framework およびユニバーサル プロバイダーことが必要なため、それらに追加されました。

## <a name="installing-sql-server-compact"></a>SQL Server Compact のインストール

SqlServerCompact NuGet パッケージは、Contoso University アプリケーションのアセンブリをエンジン、SQL Server Compact データベースを提供します。 ここではありませんが、アプリケーション、Web Deploy、SQL Server データベースで実行するスクリプトを作成するには、SQL Server Compact のデータベースを読めるようにする必要があります。 Web 配置で SQL Server Compact データベースの読み取りを有効にする SQL Server Compact 開発用コンピューターで、次のリンクを使用してインストール: [Microsoft SQL Server Compact 4.0](https://www.microsoft.com/downloads/details.aspx?FamilyID=15F7C9B3-A150-4AD2-823E-E4E0DCF85DF6)します。

## <a name="deploying-to-the-test-environment"></a>テスト環境に展開します。

テスト環境に発行するためには、発行プロファイルを使用するように構成を作成する必要が、**パッケージ化/発行 SQL**データベース発行プロファイルのデータベース設定ではなく公開のタブ。

最初に、既存のテストのプロファイルを削除します。

**ソリューション エクスプ ローラー**ContosoUniversity プロジェクトを右クリックし、クリックして、**発行**します。

選択、**プロファイル**タブ。

クリックして**プロファイルを管理する**します。

選択**テスト**、 をクリックして**削除**、 をクリックし、**閉じる**します。

閉じる、 **Web の発行**ウィザードでこの変更を保存します。

次に、新しいテスト プロファイルを作成し、それを使用してプロジェクトを発行します。

**ソリューション エクスプ ローラー**ContosoUniversity プロジェクトを右クリックし、クリックして、**発行**します。

選択、**プロファイル**タブ。

選択**&lt;新しい.&gt;** ドロップダウン リストから、プロファイル名として"Test"を入力します。

**サービス URL**ボックスに、入力*localhost*します。

**サイト/アプリケーション**ボックスに、入力*既定の Web サイト/ContosoUniversity*します。

**送信先 URL**ボックスに、入力`http://localhost/ContosoUniversity/`します。

**[次へ]** をクリックします。

**設定**警告を表示するタブを**パッケージ化/発行 SQL**タブが構成されている、および有効にする をクリックして新しいデータベース発行の機能強化をオーバーライドする機会を提供します。 この展開をオーバーライドする必要はありません、**パッケージ化/発行 SQL**設定 タブをクリックしますので、**次**します。

![Publish_Web_wizard_Settings_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image19.png)

メッセージを**プレビュー**  タブには、ことを示します**パブリッシュするデータベースが選択されていない**が単データベースの発行が、発行プロファイルで構成されていないことを意味します。

**[発行]** をクリックします。

![Publish_Web_wizard_Preview_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image20.png)

Visual Studio では、アプリケーションを展開し、テスト環境で、サイトのホーム ページをブラウザーが開きます。 先ほど見たを同じデータを表示するかを確認する、Instructors ページを実行します。 実行、**受講者の追加** ページで、新しい生徒を追加して、新しい学生を表示し、**学生**ページ。 これは、データベースを更新することができますを確認します。 選択、**更新クレジット**ページ (にログインする必要があります) をメンバーシップ データベースが配置され、アクセス権があることを確認します。

## <a name="creating-a-sql-server-database-for-the-production-environment"></a>運用環境の SQL Server データベースを作成します。

テスト環境にデプロイしたは、これでは、運用環境にデプロイをセットアップする準備ができました。 場合と同様、テスト環境にデプロイするデータベースを作成して開始します。 概要から学習したように、Cytanium Lite のホスティング プランはない 2 つの 1 つだけデータベースをセットアップするため、単一の SQL Server データベースを許可するだけです。 すべてのテーブルとメンバーシップおよび学校 SQL Server Compact のデータベースからのデータは、実稼働環境で 1 つの SQL Server データベースにデプロイされます。

Cytanium コントロール パネルに移動して[ http://panel.cytanium.com](http://panel.cytanium.com)します。 マウスで**データベース** をクリックし、 **SQL Server 2008**です。

[![Selecting_Databases_in_Control_Panel](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image22.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image21.png)

**SQL Server 2008** ] ページで [ **Create Database**します。

[![Selecting_Create_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image24.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image23.png)

データベースの名前を「学校」にして**保存**します。 (ページに自動的にプレフィックスを追加"contosou"ため、有効な名前が"contosouSchool"になります。)

[![Naming_the_database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image26.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image25.png)

同じ ページで、 **Create User**します。 Cytanium のサーバーではなく、統合された Windows セキュリティを使用して、およびアプリケーション プール id が、データベースを開くことができますを開き、データベースに権限を持つユーザーを作成します。 運用環境に追加する接続文字列をユーザーの資格情報を追加します*Web.config*ファイル。 この手順では、これらの資格情報を作成します。

[![Creating_a_database_user](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image28.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image27.png)

必要なフィールドに入力、 **SQL ユーザー プロパティ**ページ。

- 名前として"ContosoUniversityUser"を入力します。
- パスワードを入力します。
- 選択**contosouSchool**既定のデータベースとして。
- 選択、 **contosouSchool**チェック ボックスをオンします。

[![SQL_User_Properties_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image30.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image29.png)

## <a name="configuring-database-deployment-for-the-production-environment"></a>運用環境用データベース デプロイの構成

データベースの配置設定を設定する準備ができたので、**パッケージ化/発行 SQL**タブの前、テスト環境用と同じです。

開く、**プロジェクト プロパティ**ウィンドウで、**パッケージ化/発行 SQL**  タブを確認して**アクティブ (リリース)** または**リリース**は選択された状態で、**構成**ドロップダウン リスト。

各データベースの展開設定を構成するときに何が運用環境とテスト環境用の主な違いが、接続文字列を構成する方法。 テスト環境の別の変換先データベースの接続文字列を入力したが、運用環境のターゲットの接続文字列は同じになります、両方のデータベース。 両方のデータベースを運用環境で 1 つのデータベースにデプロイするためです。

### <a name="configuring-deployment-settings-for-the-membership-database"></a>メンバーシップ データベースの展開設定を構成します。

メンバーシップ データベースに適用される設定を構成するには、選択、 **DefaultConnection 展開**行、**データベース エントリ**テーブル。

**転送先データベースへの接続文字列**、先ほど作成した新しい実稼働 SQL Server データベースを指す接続文字列を入力します。 接続文字列は、ウェルカム メールから取得できます。 電子メールの関連部分には、次のサンプルの接続文字列が含まれています。

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample5.cmd)]

3 つの変数を置換した後、必要な接続文字列は、この例のようになります。

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample6.cmd)]

コピーして貼り付けるには、この接続文字列**転送先データベースへの接続文字列**で、**パッケージ化/発行 SQL**タブ。

確認します**データや既存のデータベースからスキーマをプル**がオンのままとする**データベース スクリプト オプション**が**スキーマとデータ**します。

**データベース スクリプト**ボックスに、Grant.sql スクリプトの横にあるチェック ボックスをオフにします。

![Disable_Grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image31.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>School データベースの展開設定を構成します。

次に、選択、 **SchoolContext 展開**行、**データベース エントリ**School データベースの設定を構成するためにテーブルです。

同じ接続文字列をコピー**転送先データベースへの接続文字列**メンバーシップ データベースには、そのフィールドにコピーします。

確認します**データや既存のデータベースからスキーマをプル**がオンのままとする**データベース スクリプト オプション**が**スキーマとデータ**します。

**データベース スクリプト**ボックスに、Grant.sql スクリプトの横にあるチェック ボックスをオフにします。

変更を保存、**パッケージ化/発行 SQL**タブ。

## <a name="setting-up-webconfig-transforms-for-the-connection-strings-to-production-databases"></a>Web.Config の設定は、実稼働データベースへの接続文字列の変換します。

次を設定します*Web.config*変換、接続文字列ので、デプロイされているように*Web.config*新しい実稼働データベースを指すファイル。 接続文字列に入力した、**パッケージ化/発行 SQL** Web 配置で使用してのタブは、同じの追加を除き、使用するアプリケーションに必要な 1 つとして、`MultipleResultSets`オプション。

開いている*Web.Production.config*と置換、`connectionStrings`を持つ要素を`connectionStrings`次の例のような要素です。 (コピーのみの`connectionStrings`要素では、周囲タグをコンテキストを示すために用意されていません)。

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample7.xml?highlight=2-11)]

場合があります内の接続文字列は常に暗号化することを促すアドバイスを参照してください、 *Web.config*ファイル。 適切なは、会社のネットワーク上のサーバーにデプロイする場合があります。 共有ホスティング環境に展開する場合に、ホスティング プロバイダーのセキュリティ プラクティスを信頼しているし、必要なまたは接続文字列を暗号化するは実用的ではありません。

## <a name="deploying-to-the-production-environment"></a>実稼働環境に展開します。

これで、運用環境にデプロイする準備ができました。 Web Deploy は、プロジェクトの SQL Server Compact データベースを読み取り*アプリ\_データ*フォルダーとすべてのテーブルと実稼働 SQL Server データベース内のデータを再作成します。 使用して発行するために、**パッケージ化/発行 Web**  タブの設定、運用環境の新しい発行プロファイルを作成する必要があります。

最初に、前と同じテスト プロファイルは、既存の実稼働プロファイルを削除します。

**ソリューション エクスプ ローラー**ContosoUniversity プロジェクトを右クリックし、クリックして、**発行**します。

選択、**プロファイル**タブ。

クリックして**プロファイルを管理する**します。

選択**運用**、 をクリックして**削除**、順にクリックします**閉じる**します。

閉じる、 **Web の発行**ウィザードでこの変更を保存します。

次に、新しい実稼働プロファイルを作成し、それを使用してプロジェクトを発行します。

**ソリューション エクスプ ローラー**ContosoUniversity プロジェクトを右クリックし、クリックして、**発行**します。

選択、**プロファイル**タブ。

クリックして**インポート**、先ほどダウンロードした .publishsettings ファイルを選択します。

**接続** タブで、変更、**送信先 URL**この例では、一時的な URL が正しいにhttp://contosouniversity.com.vserver01.cytanium.comします。

運用環境に、プロファイルの名前を変更します。 (選択、**プロファイル** タブでをクリックし、**プロファイルの管理**そのために)。

閉じる、 **Web の発行**ウィザード、変更を保存します。

実際のアプリケーションを実稼働環境で、データベースの更新中に行う 2 つの追加手順今すぐ発行する前に。

1. アップロード*アプリ\_offline.htm*ように、[実稼働環境に展開する](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)チュートリアル。
2. 使用して、**ファイル マネージャー**をコピーする Cytanium コントロール パネルの機能、 *aspnet Prod.sdf*と*の学校 Prod.sdf* に運用サイトからファイル*アプリ\_データ*ContosoUniversity プロジェクトのフォルダー。 これにより、新しい SQL Server データベースにデプロイするデータには、実稼働 web サイトによって行われた最新の更新プログラムが含まれます。

**Web の 1 クリックして発行**ツールバーで、ことを確認します、**運用**プロファイルが選択されているし をクリックし、**発行**します。

アップロードした場合<em>アプリ\_offline.htm</em>使用しなければ、パブリッシュする前に、<strong>ファイル マネージャー</strong> Cytanium のコントロール パネルを削除するユーティリティ<em>アプリ\_オフライン</em>。htm テストする前にします。 削除することも同時に、 <em>.sdf</em>ファイルから、<em>アプリ\_データ</em>フォルダー。

ブラウザーを開き、アプリケーションをテストするには、テスト環境に配置した後と同様に、公開サイトの URL に移動できますようになりました。

## <a name="switching-to-sql-server-express-localdb-in-development"></a>開発での SQL Server Express LocalDB への切り替え

この概要で説明した、ようには、テストと運用環境で使用する開発で同じデータベース エンジンを使用する通常お勧めします。 (開発で SQL Server Express を使用する利点は、データベースは、開発、テスト、および実稼働環境で同じに機能に注意してください)。このセクションでは、Visual Studio からアプリケーションを実行するときに、SQL Server Express LocalDB を使用する ContosoUniversity プロジェクトを設定します。

この移行を実行する最も簡単な方法は、Code First を使用して、メンバーシップ システムでは、両方の新しい開発データベースを作成できます。 このメソッドを使用して移行するには、3 つの手順が必要です。

1. 新しい SQL Express LocalDB データベースを指定する接続文字列を変更します。
2. 管理者ユーザーを作成する Web サイト管理ツールを実行します。 これは、メンバーシップ データベースを作成します。
3. 作成し、アプリケーション データベースのシードには、Code First Migrations の update-database コマンドを使用します。

### <a name="updating-connection-strings-in-the-webconfig-file"></a>Web.config ファイル内の接続文字列を更新しています

開く、 *Web.config*ファイルを開き、`connectionStrings`要素を次のコード。

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample8.xml)]

### <a name="creating-the-membership-database"></a>メンバーシップ データベースを作成します。

**ソリューション エクスプ ローラー**ContosoUniversity プロジェクトを選択し、クリックして**ASP.NET 構成**で、**プロジェクト**メニュー。

[セキュリティ] タブを選択します。

クリックして**管理ロールの作成または**、し、作成、**管理者**ロール。

[セキュリティ] タブに戻ります。

をクリックして**ユーザーの作成**を選び、**管理者**チェック ボックスをオンし、管理者をという名前のユーザーを作成

閉じる、 **Web サイト管理ツール**します。

### <a name="creating-the-school-database"></a>School データベースの作成

パッケージ マネージャー コンソール ウィンドウを開きます。

**既定のプロジェクト**ドロップダウン リストで、ContosoUniversity.DAL プロジェクトを選択します。

次のコマンドを入力します。

[!code-powershell[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample9.ps1)]

Code First Migrations は、データベースを作成し、AddBirthDate の移行を適用する初期移行を適用し、Seed メソッドを実行します。

コントロール f5 キーを押してサイトを実行します。 テストおよび運用環境の場合と、実行、**受講者の追加** ページで、新しい生徒を追加して、新しい学生を表示し、**学生**ページ。 これは、School データベースが作成され初期化された、およびその読み取りが書き込みアクセス権を確認します。

選択、**更新クレジット**ページし、メンバーシップ データベースがデプロイされたことと、アクセス権があることを確認するにログインします。 管理者アカウントを作成し、ユーザー アカウントを移行しなかった場合、**更新クレジット**ページの動作を確認します。

## <a name="cleaning-up-sql-server-compact-files"></a>SQL Server Compact ファイルのクリーンアップ

ファイルと SQL Server Compact をサポートするために含まれていた NuGet パッケージが不要になった。 場合 (この手順が必要ない) 不要なファイルと参照をクリーンアップすることができます。

**ソリューション エクスプ ローラー**、削除、 *.sdf*ファイルから、*アプリ\_データ*フォルダーと*amd64*と*x86*フォルダーから、 *bin*フォルダー。

**ソリューション エクスプ ローラー**(いないプロジェクトの 1 つ、)、ソリューションを右クリックし、クリックして**ソリューションの NuGet パッケージの管理**します。

左側のウィンドウで、 **NuGet パッケージの管理**ダイアログ ボックスで、**パッケージがインストールされている**します。

選択、 **EntityFramework.SqlServerCompact**パッケージ化し、をクリックして**管理**します。

**プロジェクトの選択**ダイアログ ボックスで、両方のプロジェクトを選択します。 両方のプロジェクトでパッケージをアンインストールするには、両方のチェック ボックスをオフにし、クリックして**OK**します。

場合も、依存パッケージをアンインストールするかを確認するダイアログ ボックスで、次のようにクリックします。 いいえ。 1 つは、Entity Framework パッケージを保持する必要があります。

アンインストールする同じ手順に従って、 **SqlServerCompact**パッケージ。 (この順序で、パッケージをアンインストールする必要があります、 **EntityFramework.SqlServerCompact**パッケージによって異なります、 **SqlServerCompact**パッケージです)。

SQL Server Express と完全な SQL Server に正常に移行したようになりました。 次のチュートリアルが別のデータベースの変更とすることになりますが、テストおよび運用データベースを SQL Server Express と完全な SQL Server を使用する場合は、データベースの変更をデプロイする方法表示されます。

> [!div class="step-by-step"]
> [前へ](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
> [次へ](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
