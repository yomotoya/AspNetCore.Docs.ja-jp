---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
title: "SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションを配置します SQL Server - 12 の 10 への移行 |。Microsoft ドキュメント"
author: tdykstra
description: "この一連のチュートリアルは、展開する方法を示します (発行)、ASP.NET web アプリケーション プロジェクトを Visual Stu を使用して、SQL Server Compact データベースが含まれています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a89d6f32-b71b-4036-8ff7-5f8ac2a6eca8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
msc.type: authoredcontent
ms.openlocfilehash: b97834e3e287645151bf927996fde63d93ae8356
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-migrating-to-sql-server---10-of-12"></a>SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションを配置します SQL Server - 12 の 10 への移行。
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> この一連のチュートリアルは、展開する方法を示します (発行)、ASP.NET web アプリケーション プロジェクトを Visual Studio 2012 RC または Visual Studio Express 2012 RC for Web を使用して SQL Server Compact データベースが含まれます。 Web 公開の更新プログラムをインストールする場合は、また Visual Studio 2010 を使用することができます。 系列の概要については、次を参照してください。[系列内の最初のチュートリアル](deployment-to-a-hosting-provider-introduction-1-of-12.md)です。
> 
> チュートリアルについては、RC のリリースの Visual Studio 2012 以降の展開機能を示しています、SQL Server Compact 以外の SQL Server のエディションを展開する方法を示していますし、Azure App Service Web アプリを展開する方法を示しています、次を参照してください[ASP.NET Web 配置。Visual Studio を使用して](../../deployment/visual-studio-web-deployment/introduction.md)です。


## <a name="overview"></a>概要

このチュートリアルでは、SQL Server Compact から SQL Server に移行する方法を示します。 SQL Server Compact をサポートしていないストアド プロシージャ、トリガー、ビュー、またはレプリケーションなど、SQL Server の機能を活用するためには、理由の 1 つを実行することができます。 SQL Server Compact および SQL Server の違いの詳細については、次を参照してください。、[を展開する SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)チュートリアルです。

### <a name="sql-server-express-versus-full-sql-server-for-development"></a>開発の完全な SQL Server と SQL Server Express

SQL Server にアップグレードすることが決まっているとは、開発およびテスト環境で SQL Server または SQL Server Express を使用することができます。 ツールのサポートとデータベース エンジン機能の違い、に加えて SQL Server Compact とその他のバージョンの SQL Server プロバイダーの実装で違いがあります。 これらの相違点には、同じコードを異なる結果を生成する可能性があります。 そのため、SQL Server Compact、開発用データベースとして保持する場合は、必要があります十分にテストするサイトで SQL Server または SQL Server Express 実稼働環境にそれぞれ配置する前に、テスト環境でします。

異なり、SQL Server Compact は SQL Server Express を同じデータベース エンジンでは基本的には、および完全な SQL Server と同じ .NET プロバイダーを使用します。 SQL Server Express を使ってテストするときに、SQL Server では、同じ結果を取得することを確信できます。 SQL Server Express の SQL Server で使用できると同じデータベース ツールのほとんどを行うこともできます (されている主な例外[SQL Server Profiler](https://msdn.microsoft.com/library/ms181091.aspx))、ストアド プロシージャ、ビュー、トリガーなどのように SQL Server の他の機能がサポートされますおよびレプリケーション。 (通常ただし、実稼働 web サイトで完全な SQL Server を使用する必要があります。 共有ホスティング環境で SQL Server Express を実行できますが、向けに設計されていないと多くのホスティング プロバイダーではサポートされません。)

Visual Studio 2012 を使用している場合は、通常開発環境に合わせて SQL Server Express LocalDB を選択する Visual Studio で既定でインストールされるものであるため。 ただし、LocalDB が動作しない IIS では、ので、テスト環境用には、SQL Server または SQL Server Express のいずれかを使用する必要があるあります。

### <a name="combining-databases-versus-keeping-them-separate"></a>データベースを個別に維持すると結合

Contoso 大学アプリケーションが 2 つの SQL Server Compact データベース: メンバーシップ データベース (*aspnet.sdf*) と、アプリケーション データベース (*School.sdf*)。 移行する場合、2 つの異なるデータベースまたは 1 つのデータベースにこれらのデータベースを移行できます。 アプリケーションのデータベースと、メンバーシップ データベース間の結合がデータベースを容易にするために結合する可能性があります。 ホスティング プランには、それらを結合する理由を指定も可能性があります。 ホスティング プロバイダーの複数のデータベースの課金する場合がありますなど、複数のデータベースをも許可しない可能性があります。 これは、ような Cytanium Lite でこのチュートリアルでは、により、1 つの SQL Server データベースだけに使用されるアカウントをホストするいるとします。

このチュートリアルではこのように、2 つのデータベースを移行します。

- 開発環境で 2 つの LocalDB データベースを移行します。
- テスト環境で 2 つの SQL Server Express データベースを移行します。
- 1 つ結合された SQL Server データベースの完全実稼働環境で移行します。

アラーム: エラー メッセージを取得する、または、チュートリアルを通過するとおりに機能しない、確認して、[トラブルシューティングのページ](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)です。

## <a name="installing-sql-server-express"></a>SQL Server Express をインストールします。

既定では、Visual Studio 2010 が自動的にインストールされている SQL Server Express しますが、既定でインストールされていない Visual Studio 2012 で。 SQL Server 2012 Express をインストールするには、次のリンクをクリックします。

- [SQL Server Express 2012](https://www.microsoft.com/download/details.aspx?id=29062)

選択*日本語/x64/SQLEXPR\_x64\_ENU.exe*または*日本語/x86/SQLEXPR\_x86\_ENU.exe*、インストール ウィザードで、既定設定。 インストール オプションの詳細については、次を参照してください。[インストール ウィザード (セットアップ) からの SQL Server 2012 のインストール](https://msdn.microsoft.com/library/ms143219.aspx)です。

## <a name="creating-sql-server-express-databases-for-the-test-environment"></a>テスト環境の SQL Server Express データベースを作成します。

次の手順では、ASP.NET メンバーシップおよび School データベースを作成します。

**ビュー**メニュー選択**サーバー エクスプ ローラー** (**データベース エクスプ ローラー** Visual Web Developer で)、し、右クリックし、**データ接続**選択**新しい SQL Server データベースの作成**です。

![Selecting_Create_New_SQL_Server_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image1.png)

**新しい SQL Server データベースの作成**] ダイアログ ボックスで、入力". \SQLExpress"で、**サーバー名**ボックスと"aspnet Test"で、**新しいデータベース名**のボックスで、[**OK**です。

![Create_New_SQL_Server_Database_aspnet](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image2.png)

"学校 Test"という新しい SQL Server Express の School データベースを作成する同じ手順に従います。

(追加する"Test"これらのデータベース名を後で、開発環境の各データベースの追加インスタンスを作成し、データベースの 2 つのセットを区別する必要があるためです。)

**サーバー エクスプ ローラー** 2 つの新しいデータベースが表示されます。

![New_databases_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image3.png)

## <a name="creating-a-grant-script-for-the-new-databases"></a>新しいデータベースの Grant スクリプトを作成します。

アプリケーションを開発用コンピューターに IIS で実行すると、アプリケーションは既定のアプリケーション プールの資格情報を使用して、データベースにアクセスします。 ただし、既定では、アプリケーション プール id は、データベースを開くアクセス許可がありません。 ように権限を付与するスクリプトを実行する必要があります。 このセクションでは、後で、アプリケーションも、IIS での実行時に、データベースを開くことができるかどうかを確認を実行しますスクリプトを作成します。

ソリューションの*SolutionFiles*で作成したフォルダー、[実稼働環境に展開する](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)チュートリアルでは、という名前の新しい SQL ファイルを作成する*Grant.sql*です。 ファイルに次の SQL コマンドをコピーおよび保存し、ファイルを閉じます。

[!code-sql[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample1.sql)]

> [!NOTE]
> このスクリプトは、このチュートリアルでは指定されているように SQL Server 2008 と Windows 7 で IIS 設定を操作する設計されています。 または、Windows の SQL Server の別のバージョンを使用している場合、または IIS を設定するコンピューターに異なる場合は、次のスクリプトへの変更が必要があります。 SQL Server スクリプトの詳細については、次を参照してください。 [SQL Server オンライン ブック](https://go.microsoft.com/fwlink/?LinkId=132511)です。


> [!NOTE] 
> 
> **セキュリティに関する注意**このスクリプトは、db\_、実稼働環境で必要がありますが、実行時に、データベースにアクセスするユーザーを所有者のアクセス許可。 一部のシナリオでのみ展開については、アクセス許可を更新し、実行時のデータを読み書きするのみのアクセス許可を持つ別のユーザーを指定します。 完全なデータベース スキーマを持つユーザーを指定することができます。 詳細については、次を参照してください。 **Code First Migrations を自動の Web.config の変更を確認**で[テスト環境として IIS に展開する](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)です。


## <a name="configuring-database-deployment-for-the-test-environment"></a>データベースの配置、テスト環境用の構成

次に、データベースごとに、次のタスクを実行できるように、Visual Studio を構成します。

- 転送先データベースに、ソース データベースの構造 (テーブル、列、制約など) を作成する SQL スクリプトを生成します。
- 転送先データベース内のテーブルに、ソース データベースのデータを挿入する SQL スクリプトを生成します。
- 生成されたスクリプト、および転送先データベースで、作成した許可スクリプトを実行します。

開く、**プロジェクト プロパティ**ウィンドウを選択、**パッケージ化/発行 SQL**タブです。

確認して**アクティブ (リリース)**または**リリース**でが選択されている、**構成**ドロップダウン リスト。

をクリックして**このページを有効にする**です。

![Package_Publish_SQL_tab_Enable_This_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image4.png)

**パッケージ化/発行 SQL**従来の配置方法を指定するために通常タブは無効になります。 ほとんどのシナリオでデータベースの配置を構成する必要があります、 **Web の発行**ウィザード。 SQL Server Compact から SQL Server または SQL Server Express への移行は、このメソッドは、適切な選択特殊なケースです。

をクリックして**web.config ファイルからインポート**です。

![Selecting_Import_from_Web.config](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image5.png)

Visual Studio が内の接続文字列を探して、 *Web.config*ファイル、メンバーシップ データベースに 1 つ、School データベースの 1 つを検索および内の各接続文字列に対応する行を追加、**データベース エントリ**テーブル。 接続文字列が見つかったが、既存の SQL Server Compact データベースに対して、次の手順は、方法と場所を構成するこれらのデータベースを展開します。

データベースのデプロイ設定を入力する、**データベース エントリの詳細**以下のセクション、**データベース エントリ**テーブル。 表示される設定、**データベース エントリの詳細**セクションの行のどちらかに関係するもの、**データベース エントリ**テーブルが選択されている、次の図に示すようにします。

![Database_Entry_Details_section_of_Package_Publish_SQL_tab](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image6.png)

### <a name="configuring-deployment-settings-for-the-membership-database"></a>メンバーシップ データベースのデプロイ設定を構成します。

選択、 **DefaultConnection 展開**行が、**データベース エントリ**メンバーシップ データベースに適用される設定を構成するためにテーブルです。

**転送先データベースへの接続文字列**、新しい SQL Server Express メンバーシップ データベースを指す接続文字列を入力します。 必要な接続文字列を取得できます**サーバー エクスプ ローラー**です。 **サーバー エクスプ ローラー**、展開**データ接続**を選択し、 **aspnetTest**データベース、してから、**プロパティ**ウィンドウのコピー、**接続文字列**値。

![aspnet_connection_string_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image7.png)

同じ接続文字列はここで再作成します。

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample2.cmd)]

コピーして貼り付けるには、この接続文字列**転送先データベースへの接続文字列**で、**パッケージ化/発行 SQL**タブです。

確認して**または既存のデータベースからスキーマのデータをプル**が選択されています。 これは、何が原因で自動的に生成され、転送先データベースで実行する SQL スクリプトです。

**ソース データベースの接続文字列**から値を抽出、 *Web.config*ファイルと、開発用の SQL Server Compact データベースへのポインター。 これは、ソース データベースをコピー先データベースに後で実行されるスクリプトの生成に使用されます。 データベースの運用バージョンを展開するには、以降は、"aspnet Dev.sdf"を"aspnet Prod.sdf"に変更します。

変更**データベース スクリプト オプション**から**スキーマのみ**に**スキーマとデータ**だけでなく、データベースの構造 (ユーザー アカウントとロール) のデータをコピーするため、します。

先ほど作成した許可スクリプトを実行する配置を構成するには、それらを追加する必要が、**データベース スクリプト**セクションです。 をクリックして**スクリプトの追加**、し、[、 **SQL スクリプトの追加**] ダイアログ ボックスで、許可スクリプトを保存したフォルダーに移動し (これは、ソリューション ファイルが含まれているフォルダーです)。 という名前のファイルを選択して*Grant.sql*、 をクリック**開く**です。

[![Select_File_dialog_box_grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image9.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image8.png)

設定、 **DefaultConnection 展開**で行**データベース エントリ**次の図のようになります。

![Database_Entry_Details_for_DefaultConnection_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image10.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>School データベースのデプロイ設定を構成します。

次に、選択、 **SchoolContext 展開**行が、**データベース エントリ**School データベースのデプロイ設定を構成するためにテーブルです。

新しい SQL Server Express データベースの接続文字列を取得する前に使用した同じメソッドを使用することができます。 この接続文字列にコピー**転送先データベースへの接続文字列**で、**パッケージ化/発行 SQL**タブです。

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample3.cmd)]

確認して**または既存のデータベースからスキーマのデータをプル**が選択されています。

**ソース データベースの接続文字列**から値を抽出、 *Web.config*ファイルと、開発用の SQL Server Compact データベースへのポインター。 "学校 Prod.sdf"データベースの運用バージョンを展開するには、"学校 Dev.sdf"を変更します。 (アプリの学校 Prod.sdf ファイルを作成することはありません\_データ フォルダー、アプリをテスト環境からそのファイルをコピーしますので\_ContosoUniversity プロジェクト フォルダーを後で、データ フォルダーです)。

変更**データベース スクリプト オプション**に**スキーマとデータ**です。

読み取りを許可し、アプリケーション プール id にこのデータベースの権限を書き込み、ため、追加するスクリプトを実行する、 *Grant.sql*ファイル、メンバーシップ データベースの場合と同様のスクリプトを作成します。

終了すると、設定**SchoolContext 展開**で行**データベース エントリ**次の図のようになります。

![Database_Entry_Details_for_SchoolContext_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image11.png)

変更を保存、**パッケージ化/発行 SQL**タブです。

コピー、*学校 Prod.sdf*ファイルから、 *c:\inetpub\wwwroot\ContosoUniversity\App\_データ*フォルダーを*アプリ\_データ*フォルダーContosoUniversity のプロジェクトです。

### <a name="specifying-transacted-mode-for-the-grant-script"></a>Grant スクリプトのトランザクション モードを指定します。

展開プロセスでは、データベース スキーマとデータを配置するスクリプトを生成します。 既定では、これらのスクリプトは、トランザクションで実行します。 ただし、既定で (grant スクリプト) のようなカスタム スクリプトは、トランザクションでは実行されません。 展開プロセスでは、トランザクション モードを混在、展開時に、スクリプトの実行時にタイムアウト エラーが発生した可能性があります。 このセクションでは、トランザクションで実行するカスタム スクリプトを構成するためにプロジェクト ファイルを編集します。

**ソリューション エクスプ ローラー**を右クリックし、 **ContosoUniversity**プロジェクトし、選択**プロジェクトのアンロード**です。

![Unload_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image12.png)

プロジェクトをもう一度右クリックし、選択**編集 ContosoUniversity.csproj**です。

![Edit_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image13.png)

Visual Studio エディターでは、プロジェクト ファイルの XML コンテンツを示します。 含まれているいくつか`PropertyGroup`要素。 (図では、内容、`PropertyGroup`要素が省略されています)。

![プロジェクト ファイルのエディター ウィンドウ](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image15.png)

No を持つ最初の 1 つ`Condition`属性、ビルド構成の設定に関係なく適用されるは、します。 1 つ`PropertyGroup`要素は、デバッグ ビルド構成にのみ適用されます (注、`Condition`属性) 1 つが、リリース ビルド構成にのみ適用されます、およびテストのビルド構成にのみ適用されます。 内で、 `PropertyGroup` 、リリース ビルド構成の要素が表示されます、`PublishDatabaseSettings`で入力した設定を含む要素、**パッケージ化/発行 SQL**タブです。`Object` Grant スクリプトのそれぞれに対応する要素 ("Grant.sql"のインスタンスを 2 つに注意してください) を指定します。 既定では、`Transacted`の属性、 `Source` grant スクリプトごとに要素が`False`です。

![Transacted_false](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image16.png)

値を変更、`Transacted`の属性、`Source`要素を`True`です。

![Transacted_true](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image17.png)

保存して、プロジェクト ファイルを閉じでプロジェクトを右クリックし、**ソリューション エクスプ ローラー**選択**プロジェクトの再読み込み**です。

![Reload_project](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image18.png)

## <a name="setting-up-webconfig-transformations-for-the-connection-strings"></a>接続文字列の Web.Config 変換の設定

接続文字列入力した新しい SQL Express データベースの**パッケージ化/発行 SQL**  タブは、配置時に転送先データベースを更新時にのみ Web Deploy によって使用されます。 設定する必要がある*Web.config*変換できるように、接続文字列、デプロイされている*Web.config*ファイルの新しい SQL Server Express データベースをポイントします。 (使用すると、**パッケージ化/発行 SQL**  タブで、発行プロファイルで接続文字列を構成することはできません)。

開いている*Web.Test.config*と置換、`connectionStrings`を持つ要素、`connectionStrings`例を次の要素。 (周囲コードではなくコンテキストを提供するのには、ここに表示される、connectionStrings 要素のみをコピーすることを確認してください。)

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample4.xml?highlight=2-11)]

このコードにより、`connectionString`と`providerName`の各属性`add`置き換えられる要素で、デプロイされている*Web.config*ファイル。 これらの接続文字列で入力したものとは異なるが、**パッケージ化/発行 SQL**タブです。設定"MultipleActiveResultSets = True"、Entity Framework とユニバーサル プロバイダーのために必要なためそれらに追加されました。

## <a name="installing-sql-server-compact"></a>SQL Server Compact のインストール

SqlServerCompact NuGet パッケージは、SQL Server Compact データベース エンジン アプリケーションのアセンブリを Contoso 大学を提供します。 ここでは、アプリケーションが Web デプロイ、SQL Server データベースで実行するスクリプトを作成するために、SQL Server Compact データベースを読み取りできる必要があります。 Web デプロイを SQL Server Compact データベースの読み取りを有効にする SQL Server Compact 開発用コンピューターに、次のリンクを使用してインストールします。 [Microsoft SQL Server Compact 4.0](https://www.microsoft.com/downloads/details.aspx?FamilyID=15F7C9B3-A150-4AD2-823E-E4E0DCF85DF6)です。

## <a name="deploying-to-the-test-environment"></a>テスト環境に展開します。

使用するように構成する発行プロファイルを作成する必要がテスト環境に発行するために、**パッケージ化/発行 SQL**データベース発行プロファイルのデータベース設定ではなく公開のタブです。

最初に、既存のテストのプロファイルを削除します。

**ソリューション エクスプ ローラー**ContosoUniversity プロジェクトを右クリックし、クリックして、**発行**です。

選択、**プロファイル**タブです。

をクリックして**プロファイルを管理する**です。

選択**テスト**、 をクリックして**削除**、クリックして**閉じる**です。

閉じる、 **Web の発行**ウィザードにこの変更を保存します。

次に、新しいテスト プロファイルを作成して、プロジェクトを発行します。

**ソリューション エクスプ ローラー**ContosoUniversity プロジェクトを右クリックし、クリックして、**発行**です。

選択、**プロファイル**タブです。

選択**&lt;新規作成しています.&gt;** ドロップダウン リスト ボックスの一覧し、プロファイル名として"Test"を入力します。

**サービス URL**ボックスに、入力*localhost*です。

**サイト/アプリケーション**ボックスに、入力*既定の Web サイト/ContosoUniversity*です。

**送信先 URL**ボックスに、入力`http://localhost/ContosoUniversity/`です。

**[次へ]**をクリックします。

**設定** タブに警告すること、**パッケージ化/発行 SQL**  タブが構成済みであり、有効にする をクリックして、新しいデータベース発行機能強化をオーバーライドする機会を提供します。 この展開のたくを上書きする、**パッケージ化/発行 SQL**タブの設定をクリックするだけのため**次**です。

![Publish_Web_wizard_Settings_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image19.png)

メッセージ、**プレビュー**  タブには、ことを示します**パブリッシュするデータベースが選択されていない**、のみつまり、データベースの発行が、発行プロファイルで構成されていないことができます。

**[発行]**をクリックします。

![Publish_Web_wizard_Preview_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image20.png)

Visual Studio では、アプリケーションを展開し、テスト環境で、サイトのホーム ページをブラウザーが開きます。 先ほど見たを同じデータが表示されるを表示するインストラクター page を実行します。 実行、**受講者を追加** ページで新しい学生を追加して、新しい学生を表示、**受講者**ページ。 これは、データベースを更新することを確認します。 選択、**更新クレジット**(にログインする必要があります) ページ メンバーシップ データベースが配置されたされへのアクセス権があることを確認します。

## <a name="creating-a-sql-server-database-for-the-production-environment"></a>運用環境の SQL Server データベースを作成します。

をテスト環境にデプロイした実稼働環境にデプロイをセットアップする準備が整いました。 同様、テスト環境用に展開するデータベースを作成することで開始します。 概要から学習したように、Cytanium Lite のホスティング プランのみで、単一の SQL Server データベースためはより多くのデータベースを 1 つしかない 2 つを設定にできます。 すべてのテーブルと、メンバーシップおよび学校 SQL Server Compact データベースからのデータは、実稼働環境での 1 つの SQL Server データベースに展開されます。

Cytanium コントロール パネルに移動して[http://panel.cytanium.com](http://panel.cytanium.com)です。マウスで**データベース** をクリックし、 **SQL Server 2008**です。

[![Selecting_Databases_in_Control_Panel](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image22.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image21.png)

**SQL Server 2008** ] ページで [ **Create Database**です。

[![Selecting_Create_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image24.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image23.png)

データベース「学校」の名前をクリックして**保存**です。 (ページに自動的にプレフィックスを追加"contosou"、有効な名前は"contosouSchool"になります。)

[![Naming_the_database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image26.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image25.png)

同じページで、をクリックして**Create User**です。 Cytanium のサーバーではなく、Windows 統合セキュリティを使用して、および、データベースを開くアプリケーション プール id をできるようにすることで、データベースを開く権限が付与されているユーザーを作成します。 ユーザーの資格情報にも、実稼働環境での接続文字列を追加する*Web.config*ファイル。 この手順では、これらの資格情報を作成します。

[![Creating_a_database_user](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image28.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image27.png)

必要なフィールドに入力、 **SQL ユーザー プロパティ**ページ。

- 名前として"ContosoUniversityUser"を入力します。
- パスワードを入力します。
- 選択**contosouSchool**既定のデータベースとして。
- 選択、 **contosouSchool**チェック ボックスをオンします。

[![SQL_User_Properties_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image30.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image29.png)

## <a name="configuring-database-deployment-for-the-production-environment"></a>運用環境のデータベースの配置を構成します。

これで、データベースでのデプロイ設定を設定する準備ができたら、**パッケージ化/発行 SQL**  タブで、テスト環境の前に行ったようにします。

開く、**プロジェクトのプロパティ**ウィンドウで、**パッケージ化/発行 SQL**  タブを確認して**アクティブ (リリース)**または**リリース**は選択された状態で、**構成**ドロップダウン リスト。

各データベースのデプロイ設定を構成するときに運用環境とテスト環境の場合の重要な違いが、接続文字列を構成する方法です。 テスト環境の別のインストール先データベースの接続文字列を入力したが、運用環境のコピー先の接続文字列は、両方のデータベースの同じです。 実稼働環境で 1 つのデータベースを両方のデータベースを展開しているためにです。

### <a name="configuring-deployment-settings-for-the-membership-database"></a>メンバーシップ データベースのデプロイ設定を構成します。

メンバーシップ データベースに適用される設定を構成するには、選択、 **DefaultConnection 展開**行が、**データベース エントリ**テーブル。

**転送先データベースへの接続文字列**、先ほど作成した新しい運用 SQL Server データベースを指す接続文字列を入力します。 ようこそ電子メールから、接続文字列を取得できます。 電子メールの関連する部分には、次のサンプルの接続文字列が含まれています。

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample5.cmd)]

3 つの変数を交換した後、必要な接続文字列は、この例のようになります。

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample6.cmd)]

コピーして貼り付けるには、この接続文字列**転送先データベースへの接続文字列**で、**パッケージ化/発行 SQL**タブです。

確認して**または既存のデータベースからスキーマのデータをプル**がオンのままとする**データベース スクリプト オプション**まだ**スキーマとデータ**です。

**データベース スクリプト**ボックスで、Grant.sql スクリプトの横にあるチェック ボックスをオフにします。

![Disable_Grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image31.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>School データベースのデプロイ設定を構成します。

次に、選択、 **SchoolContext 展開**行が、**データベース エントリ**School データベースの設定を構成するためにテーブルです。

同じ接続文字列をコピー**転送先データベースへの接続文字列**をメンバーシップ データベースにそのフィールドにコピーします。

確認して**または既存のデータベースからスキーマのデータをプル**がオンのままとする**データベース スクリプト オプション**まだ**スキーマとデータ**です。

**データベース スクリプト**ボックスで、Grant.sql スクリプトの横にあるチェック ボックスをオフにします。

変更を保存、**パッケージ化/発行 SQL**タブです。

## <a name="setting-up-webconfig-transforms-for-the-connection-strings-to-production-databases"></a>Web.Config の設定を実稼働データベースへの接続文字列の変換します。

次を設定します*Web.config*変換できるように、接続文字列、デプロイされている*Web.config*新しい運用データベースを指すファイル。 接続文字列で入力した、**パッケージ化/発行 SQL** Web を使用する展開のタブは、同じの追加を除く、使用するアプリケーションに必要な 1 つとして、`MultipleResultSets`オプション。

開いている*Web.Production.config*と置換、`connectionStrings`を持つ要素を`connectionStrings`次の例のような要素です。 (コピーのみの`connectionStrings`要素では、周囲タグをコンテキストを示すために用意されていません)。

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample7.xml?highlight=2-11)]

場合があります内の接続文字列は常に暗号化するよう指示するためのアドバイスを参照してください、 *Web.config*ファイル。 適切なは、会社のネットワーク上のサーバーに配置する場合があります。 共有ホスティング環境に展開する場合は、ただし、ホスティング プロバイダーの場合のセキュリティ プラクティスを信頼しているし、必要なまたは接続文字列を暗号化することは実用的ではありません。

## <a name="deploying-to-the-production-environment"></a>実稼働環境に展開します。

これで、実稼働環境に展開する準備ができました。 Web Deploy は、プロジェクトの SQL Server Compact データベースを読み取り*アプリ\_データ*フォルダーとすべてのテーブルと、実稼働 SQL Server データベース内のデータを再作成します。 使用して発行するために、**パッケージ化/発行 Web**  タブの設定、実稼働環境用の新しい発行プロファイルを作成する必要があります。

最初に、テスト プロファイルを前述した方法では、既存の実稼働プロファイルを削除します。

**ソリューション エクスプ ローラー**ContosoUniversity プロジェクトを右クリックし、クリックして、**発行**です。

選択、**プロファイル**タブです。

をクリックして**プロファイルを管理する**です。

選択**運用**、 をクリックして**削除**、順にクリック**閉じる**です。

閉じる、 **Web の発行**ウィザードにこの変更を保存します。

次に、実稼働環境に、新しいプロファイルを作成して、プロジェクトを発行します。

**ソリューション エクスプ ローラー**ContosoUniversity プロジェクトを右クリックし、クリックして、**発行**です。

選択、**プロファイル**タブです。

をクリックして**インポート**、以前にダウンロードした .publishsettings ファイルを選択します。

**接続** タブで、変更、**送信先 URL**正しいの一時的な url では、この例では http://contosouniversity.com.vserver01.cytanium.com します。

実稼働環境に、プロファイルの名前を変更します。 (選択、**プロファイル** タブでをクリックし、**プロファイルの管理**を行うには)。

閉じる、 **Web の発行**変更を保存するウィザード。

実稼働環境でこれで、データベースが更新されている実際のアプリケーションでは手順を実行する 2 つ追加今すぐ発行する前に。

1. アップロード*アプリ\_offline.htm*のように、[実稼働環境に展開する](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)チュートリアルです。
2. 使用して、**ファイル マネージャー**をコピーする Cytanium コントロール パネル の機能、 *aspnet Prod.sdf*と*学校 Prod.sdf*ファイルを実稼働サイトから、 *アプリ\_データ*ContosoUniversity プロジェクトのフォルダーです。 これにより、新しい SQL Server データベースに配置するデータには、実稼働 web サイトによって行われた最新の更新プログラムが含まれます。

**Web 1 つをクリックして 発行**ツールバー、ことを確認して、**運用**プロファイルを選択して、をクリックして**発行**です。

アップロードした場合*アプリ\_offline.htm* 、パブリッシュする前に、使用する必要が、**ファイル マネージャー**を削除する Cytanium コントロール パネルの ユーティリティ*アプリ\_オフライン*。htm ファイルをテストする前にします。 削除することも、同時に、 *.sdf*ファイルから、*アプリ\_データ*フォルダーです。

ブラウザーを開き、アプリケーションをテストするには、テスト環境に配置した後で実行したのと同様に、パブリックのサイトの URL に移動できるようになりました。

## <a name="switching-to-sql-server-express-localdb-in-development"></a>開発中の SQL Server Express LocalDB への切り替え

概要」に説明したとおりには、開発、テストや実稼働環境を使用することで、同じデータベース エンジンを使用する通常お勧めします。 (開発における SQL Server Express を使用する利点は、データベースが動作する、同じ開発、テスト、実稼働環境では注意してください)。このセクションでは、Visual Studio からアプリケーションを実行するときに、SQL Server Express LocalDB を使用する ContosoUniversity プロジェクトを設定します。

この移行を実行する最も簡単な方法として、Code First を使用でき、メンバーシップ システムの両方の新しい開発データベースを作成するという方法があります。 このメソッドを使用して移行するには、3 つの手順が必要です。

1. 新しい SQL Express LocalDB データベースを指定する接続文字列を変更します。
2. 管理者ユーザーを作成する Web サイト管理ツールを実行します。 これには、メンバーシップ データベースが作成されます。
3. 作成し、アプリケーション データベースのシードには、Code First Migrations データベースの更新コマンドを使用します。

### <a name="updating-connection-strings-in-the-webconfig-file"></a>Web.config ファイル内の接続文字列を更新

開く、 *Web.config*ファイルし、置換、`connectionStrings`要素を次のコード。

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample8.xml)]

### <a name="creating-the-membership-database"></a>メンバーシップ データベースを作成します。

**ソリューション エクスプ ローラー**ContosoUniversity プロジェクトを選択し、クリックして**ASP.NET 構成**で、**プロジェクト**メニュー。

[セキュリティ] タブを選択します。

をクリックして**作成またはロールの管理**、し、作成、**管理者**ロール。

[セキュリティ] タブに戻ります。

をクリックして**Create user**、クリックして、**管理者**チェック ボックスをオンし、管理者をという名前のユーザーを作成

閉じる、 **Web サイト管理ツール**です。

### <a name="creating-the-school-database"></a>School データベースを作成します。

パッケージ マネージャー コンソール ウィンドウを開きます。

**既定のプロジェクト**ドロップダウン リストで、ContosoUniversity.DAL プロジェクトを選択します。

次のコマンドを入力します。

[!code-powershell[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample9.ps1)]

Code First Migrations がデータベースを作成し、AddBirthDate 移行を適用する初期の移行を適用し、シード メソッドを実行します。

コントロール f5 キーを押して、サイトを実行します。 テストおよび運用環境の場合と、実行、**受講者を追加** ページで新しい学生を追加して、新しい学生を表示、**受講者**ページ。 これは、School データベースが作成され、初期化することと読み取り書き込みアクセス権を確認します。

選択、**更新クレジット** ページで、メンバーシップ データベースが展開されたことへのアクセス権があることを確認します。 管理者アカウントを作成し、[ユーザー アカウントを移行しなかった場合、**更新クレジット**] ページの動作を確認するためです。

## <a name="cleaning-up-sql-server-compact-files"></a>SQL Server Compact ファイルのクリーンアップ

ファイルと SQL Server Compact をサポートするために含まれている NuGet パッケージが不要になった。 場合 (この手順は不要)、不要なファイルとの参照をクリーンアップすることができます。

**ソリューション エクスプ ローラー**、削除、 *.sdf*ファイルから、*アプリ\_データ*フォルダーおよび*amd64*と*x86*フォルダーから、 *bin*フォルダーです。

**ソリューション エクスプ ローラー**(いない 1 つのプロジェクトの)、ソリューションを右クリックし、クリックして**Manage NuGet Packages for Solution**です。

左側のウィンドウで、 **NuGet パッケージの管理**ダイアログ ボックスで、**インストールされているパッケージ**です。

選択、 **EntityFramework.SqlServerCompact**パッケージ化し、をクリックして**管理**です。

**プロジェクトの選択**ダイアログ ボックスで、両方のプロジェクトを選択します。 両方のプロジェクトでパッケージをアンインストールするには、両方のチェック ボックスをオフにし、クリックして**OK**です。

依存パッケージをアンインストールするかどうかたずねるダイアログ ボックス、いいえ これらの 1 つは、保持する必要のある Entity Framework パッケージです。

アンインストールする同じ手順に従って、 **SqlServerCompact**パッケージです。 (この順序で、パッケージをアンインストールする必要があります、 **EntityFramework.SqlServerCompact**パッケージによって異なります、 **SqlServerCompact**パッケージです)。

SQL Server Express と完全な SQL Server に正常に移行したようになりました。 すれば、別のデータベースの変更、およびするチュートリアルでは、次に、テストおよび実稼働データベースを SQL Server Express と完全な SQL Server を使用すると、データベースに対する変更を配置する方法が表示されます。

>[!div class="step-by-step"]
[前へ](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
[次へ](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
